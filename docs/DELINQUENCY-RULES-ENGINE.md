# Delinquency Rules Engine — State Rules + PM Customization

**Created:** 2026-07-05  
**Status:** draft — M2 architecture  
**MVP state:** Texas (`TX`)  
**CAPs:** CAP-4 (ledger/fees), CAP-5 (escalation), CAP-7 (resident comms), CAP-10 (audit)

> **Problem:** Delinquency rules vary by state. PMs need customization (fee amounts, reminder cadence, payment plans). The platform must enforce **what cannot be violated** (law) while allowing **what PMs can tune** (business policy) — and block the agent from illegal actions.

---

## Three-layer model

```
┌─────────────────────────────────────────────────────────┐
│  Layer 1: StateRulePack (platform-owned, immutable)      │
│  Legal ceilings, minimums, required notices, blocked acts │
└──────────────────────────┬──────────────────────────────┘
                           │ constrains ▼
┌─────────────────────────────────────────────────────────┐
│  Layer 2: OrgDelinquencyPolicy (PM admin configurable)   │
│  Fee structure, grace, reminders, payment plans, toggles │
│  — only within Layer 1 bounds                            │
└──────────────────────────┬──────────────────────────────┘
                           │ overridden by ▼
┌─────────────────────────────────────────────────────────┐
│  Layer 3: LeaseTerms (per lease, imported or signed)     │
│  Rent due date, late fee clause, payment method          │
│  — must match or be stricter than Layer 2; never violate L1 │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
              Delinquency Agent evaluates → act or block
                           │
                           ▼
              CAP-10 DecisionTrace (rules version + outcome)
```

**Evaluation order:** `LeaseTerms` → `OrgPolicy` → `StateRulePack` → **hard block if any layer fails**.

---

## What PM can vs cannot modify

| Field | Layer 1 (State — locked) | Layer 2 (Org — PM config) | Layer 3 (Lease — per tenant) |
|-------|--------------------------|---------------------------|------------------------------|
| **Minimum grace before late fee** | Floor (e.g. TX: 2 full days) | Can extend, never shorten below L1 | If lease specifies longer grace, use lease |
| **Late fee cap (% or $)** | Ceiling (e.g. TX: 10% or 12%) | Set fee ≤ cap; flat or % | Must be in written lease (TX required) |
| **Daily late fee allowed?** | Yes, but combined ≤ cap (TX) | PM sets initial + daily split | Lease must disclose both |
| **Late fee in lease required?** | Yes (TX) | — | Imported from lease doc |
| **Reminder cadence (SMS/email)** | Required notice types if any | PM sets days: 1, 3, 7, 14… | — |
| **Payment plan offers** | Must not waive tenant legal rights | PM enables + max duration | Agent proposes within org rules |
| **Eviction trigger / notice** | State notice requirements (TX 2026+) | **Phase 2 (M6)** — PM manual | — |
| **Waive late fee** | Audit required | PM admin only; reason required | — |
| **Block maintenance if delinquent** | — | PM toggle (recommended default: yes) | — |
| **NSF / bounced check fee** | State cap (TX: $30 check fee rules) | PM sets ≤ cap | Lease clause |

**UI rule:** Fields marked 🔒 in org settings are read-only with tooltip: *"Set by Texas Property Code §92.019 — cannot reduce."*

---

## Texas RulePack v1 (MVP)

**Source:** Texas Property Code §92.019 (late fees); 2026 Notice to Pay Rent or Vacate (first delinquency in lease term)

| Rule ID | Rule | Type | Agent behavior |
|---------|------|------|----------------|
| `TX-LF-001` | Late fee only if in **written lease** | 🔒 Block | No fee if lease has no late fee clause |
| `TX-LF-002` | Minimum **2 full days** after due date before fee | 🔒 Floor | Agent waits until day 3 (if due 1st) |
| `TX-LF-003` | Cap **12%** monthly rent if structure ≤4 units | 🔒 Ceiling | Reject org/lease fee > 12% |
| `TX-LF-004` | Cap **10%** monthly rent if structure >4 units | 🔒 Ceiling | Use property unit count |
| `TX-LF-005` | Initial + daily fees **combined** ≤ cap | 🔒 Ceiling | Sum check before post |
| `TX-LF-006` | Fee > safe harbor requires landlord prove damages | ⚠️ Warn | Block auto-assess; PM manual + legal flag |
| `TX-LF-007` | Violation penalty: $100 + 3× fee + attorney fees | 🔒 Info | Shown in PM admin if blocked |
| `TX-EV-001` | First late in lease term: Notice to Pay before eviction (2026) | 🔒 Notice | Generate notice template; no auto-eviction (M6 Phase 2) |
| `TX-PAY-001` | Must accept cash if lease doesn't specify payment method | 🔒 Block | Cannot force online-only if lease silent |

**Property attribute required:** `structureUnitCount` (≤4 vs >4) on `Property` — drives TX-LF-003 vs TX-LF-004.

---

## OrgDelinquencyPolicy (PM customizable — within bounds)

PM admin configures at **org level**; optional **per-property override**:

```yaml
OrgDelinquencyPolicy:
  enabled: true
  graceDays: 3                    # ≥ StateRulePack.minGraceDays (TX: 2)
  lateFee:
    type: percentage | flat       # PM choice
    amount: 8                     # 8% — must be ≤ TX cap (10 or 12)
    dailyAmount: 0                # optional; combined ≤ cap
  reminders:
    - dayOffset: 1                # day after due
      channels: [sms, email]
      template: friendly_reminder
    - dayOffset: 3                # after grace
      channels: [sms, email]
      template: late_fee_assessed
    - dayOffset: 7
      template: formal_notice
  paymentPlans:
    enabled: true
    maxMonths: 3
    requiresPmApproval: true      # Basic always; Pro configurable
  blockMaintenanceIfDelinquent: true
  waiveFeeRequiresReason: true
```

**Validation on save:** Platform runs `validatePolicy(orgPolicy, stateRulePack, property)` → errors if out of bounds.

---

## Agent enforcement flow (CAP-4 + CAP-7)

```
Daily job: for each active lease where rent due and balance > 0
  1. Resolve state from property.address.state → load StateRulePack
  2. Load OrgDelinquencyPolicy (+ property override if any)
  3. Load LeaseTerms.lateFeeClause, dueDay, rentAmount
  4. Compute daysPastDue, structureUnitCount
  5. RULE ENGINE:
       IF daysPastDue < max(lease.grace, org.grace, state.minGrace) → send reminder only
       IF daysPastDue >= grace AND no late fee posted:
            fee = calculate(org.lateFee, lease.clause)
            IF fee > state.cap(structureUnitCount) → BLOCK + alert PM + log CAP-10
            ELSE post late fee to ledger (CAP-4) + notify resident (CAP-7)
       IF daysPastDue IN org.reminders → send template
       IF paymentPlan pending → wait for CAP-5 approval
  6. Write DecisionTrace: { rulePackVersion, policyVersion, leaseId, outcome }
```

---

## State expansion pattern (post-Texas)

| Phase | States | Approach |
|-------|--------|----------|
| **MVP** | Texas only | Hand-authored `StateRulePack-TX.json` + attorney review |
| **Phase 2** | CA, NY, FL, WA | One RulePack per state; same 3-layer model |
| **Scale** | 50 states | RulePack registry `stateCode → version`; PM org locked to states they operate in |

**New state checklist:**
1. Attorney review → `StateRulePack-{ST}.json`
2. Notice templates per state
3. Unit tests: org policies at boundary (cap, grace)
4. Enable state in org settings only after RulePack `status: approved`

**Competitive gap:** AppFolio/Buildium let PMs set fees but **do not hard-block** illegal configs. RentalPro agent **refuses** to assess fee if RulePack fails — auditable differentiator.

---

## Data model (draft)

| Entity | Purpose |
|--------|---------|
| `StateRulePack` | `stateCode`, `version`, `rules JSON`, `effectiveDate`, `status` |
| `OrgDelinquencyPolicy` | `organizationId`, config JSON, `stateCode`, `updatedAt` |
| `PropertyDelinquencyOverride` | Optional per-property overrides |
| `LeaseTerms` | `leaseId`, `rentDueDay`, `lateFeeClause JSON`, `graceDays` |
| `DelinquencyEvent` | `leaseId`, `type`, `amount`, `rulePackVersion`, `traceId` |

---

## Open decisions (founder — M2)

| ID | Question | Options | Recommendation |
|----|----------|---------|----------------|
| M2-1 | Scope | CAP-4 only / CAP-4+7 / Phase 2 | **CAP-4 + CAP-7** with Rules Engine |
| M2-2 | RulePack authority | Platform-only / PM can opt into stricter | **Platform enforces; PM can only be stricter** |
| M2-3 | Invalid lease (no late fee clause) | Block fee / PM override with ack | **Block auto-fee; PM manual with liability ack** |
| M2-4 | Payment plans | MVP yes / Phase 2 | **MVP with PM approval (CAP-5)** |
| M2-5 | Attorney review | Before TX ship | **Required — add to runbook** |

---

## Related artifacts

| Doc | Link |
|-----|------|
| Syndication runbook (M1 pattern) | [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) |
| CAP-4 accounting | [`capabilities/CAP-4-autonomous-accounting.md`](./capabilities/CAP-4-autonomous-accounting.md) |
| CAP-7 resident portal | [`capabilities/CAP-7-resident-portal.md`](./capabilities/CAP-7-resident-portal.md) |
| Texas compliance | [`AI-MVP-DECISIONS.md`](./AI-MVP-DECISIONS.md) |
| M6 evictions | Phase 2 non-goal — notices only in MVP |

---

## Maintenance

1. **Legal change:** New `StateRulePack` version → agent uses new version for future events; log version in CAP-10  
2. **PM saves policy:** Run validation → reject with clear errors  
3. **New state:** Add RulePack → enable in org onboarding for that state  

**Disclaimer:** RulePack content requires **Texas attorney review** before production. This doc is product architecture, not legal advice.
