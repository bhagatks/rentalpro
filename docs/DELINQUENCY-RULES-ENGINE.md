# Delinquency Rules Engine — State Rules + PM Customization

**Created:** 2026-07-05  
**Owner:** Founder + eng lead  
**Status:** active — **M2 locked** MVP (CAP-4 + CAP-7 + Rules Engine)  
**MVP state:** Texas (`TX`)  
**CAPs:** CAP-4 (ledger/fees), CAP-5 (escalation), CAP-7 (resident comms), CAP-10 (audit)

> **One doc for M2 full requirements + time-sensitive tasks.** Architecture, user stories, acceptance criteria, state rules, and runbook — so nothing from spec sessions is lost when writing stories.

---

## Full requirements (read this when building M2)

### 1. Problem & rationale

Delinquency rules vary by **state** (grace periods, fee caps, notice requirements). PM companies need **customization** (fee amounts, reminder cadence, payment plans) on top of legal minimums. Competitors (AppFolio, Buildium) let PMs configure late fees but **do not hard-block illegal configs** — PMs can accidentally violate §92.019 and face $100 + 3× fee penalties. RentalPro's agent must **refuse** illegal assessments and log every decision (CAP-10).

**Founder vision (locked):** Per-state **StateRulePack** (immutable legal bounds) + **OrgDelinquencyPolicy** (PM config within bounds) + **LeaseTerms** (per lease). PM can customize what law allows; platform blocks what law forbids.

### 2. Product vision

Three-layer evaluation on every delinquency action:

```
LeaseTerms → OrgDelinquencyPolicy → StateRulePack → ALLOW | BLOCK | ESCALATE
```

**Differentiator:** Agent hard-blocks illegal late fees with auditable trace. AppFolio/Buildium document "consult legal counsel" — RentalPro enforces at runtime.

### 3. User stories

| Actor | Story |
|-------|-------|
| PM admin | I configure late fee %, grace days, and reminder schedule within Texas legal limits; platform rejects invalid saves. |
| PM admin | I see 🔒 fields I cannot weaken (e.g. 2-day minimum grace in TX). |
| PM admin | I waive a late fee with required reason; action logged in CAP-10. |
| PM admin | I approve payment plan offers from agent (CAP-5 queue). |
| Resident | I receive SMS/email reminders on schedule; I see late fee on ledger when legally assessable. |
| Resident | I accept a payment plan through portal after PM approval. |
| Delinquency Agent | I evaluate lease + org + state rules daily; post fee or block with trace. |
| Delinquency Agent | I never assess fee if lease lacks late fee clause (TX). |
| Accountant | Late fees appear auto-categorized in CAP-4 ledger. |
| Maintenance Agent | I block dispatch if org toggle `blockMaintenanceIfDelinquent` and tenant delinquent (CAP-3). |

### 4. End-to-end flows

**Happy path — rent late, fee legally assessable:**

1. Rent due 1st; unpaid after grace (max of state 2 days, org grace, lease grace).
2. Delinquency Agent runs daily job for lease.
3. Rule engine: lease has late fee clause ✓, fee ≤ TX cap ✓, structureUnitCount determines 10% vs 12%.
4. Post late fee to CAP-4 ledger; notify resident via CAP-7 (template P3-approved in prod).
5. Reminders fire on org schedule (day 1, 3, 7…).
6. CAP-10 DecisionTrace: `{ rulePackVersion, policyVersion, rulesApplied, outcome: FEE_POSTED }`.

**Block path — illegal fee:**

1. PM org policy set to 15% fee; property has ≤4 units (12% cap).
2. Agent computes fee → exceeds TX-LF-003 → **BLOCK**.
3. PM admin alerted; no ledger post; trace logs `BLOCK_REASON: TX-LF-003`.

**Block path — no lease clause:**

1. Lease imported without late fee language.
2. Agent **BLOCK** auto-assess (TX-LF-001); PM may manual assess with liability acknowledgment.

**Payment plan path:**

1. Agent proposes plan (max months from org policy) → CAP-5 ApprovalRequest.
2. PM approves → resident accepts in CAP-7 portal → schedule created in CAP-4.

**Edge paths:**

| Trigger | Behavior |
|---------|----------|
| Partial rent payment | Org policy: apply fee regardless / waive if above threshold / pro-rate |
| Initial + daily fee exceeds combined cap | BLOCK (TX-LF-005) |
| Fee above safe harbor | WARN + block auto; PM manual + legal flag (TX-LF-006) |
| First delinquency in lease term (2026 TX) | Generate Notice to Pay template; no auto-eviction (M6 Phase 2) |
| Resident pays before fee posts | Cancel fee assessment for that cycle |
| PM waives fee | Require reason; CAP-10 override log |
| Delinquent + maintenance request | Escalate to PM if `blockMaintenanceIfDelinquent` (default on) |

### 5. Functional requirements

| ID | Requirement | MVP |
|----|-------------|-----|
| FR-M2-01 | `StateRulePack-TX.json` with rules TX-LF-001 through TX-PAY-001 | ✅ |
| FR-M2-02 | Rule validation engine: hard-block on violation | ✅ |
| FR-M2-03 | OrgDelinquencyPolicy UI with bound validation on save | ✅ |
| FR-M2-04 | Per-property override (optional stricter policy) | ✅ |
| FR-M2-05 | LeaseTerms import: late fee clause, due day, grace | ✅ |
| FR-M2-06 | Daily delinquency agent job | ✅ |
| FR-M2-07 | CAP-4 auto late fee ledger posting | ✅ |
| FR-M2-08 | CAP-7 SMS/email reminder sequences | ✅ |
| FR-M2-09 | Payment plan offer + CAP-5 approval | ✅ |
| FR-M2-10 | PM fee waive with required reason → CAP-10 | ✅ |
| FR-M2-11 | `structureUnitCount` on Property for 10%/12% cap | ✅ |
| FR-M2-12 | Block maintenance if delinquent (org toggle) | ✅ |
| FR-M2-13 | Notice to Pay template generation (2026 TX first delinquency) | ✅ |
| FR-M2-14 | **Prod gate:** no auto-fee in prod until attorney sign-off (P1) | ✅ |

### 6. Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-M2-01 | Every fee assess/block/waive → CAP-10 trace with rulePackVersion |
| NFR-M2-02 | RulePack version pinned on each DelinquencyEvent |
| NFR-M2-03 | Org policy save returns field-level errors citing state rule ID |
| NFR-M2-04 | Daily job completes for 10k leases in <15 min |
| NFR-M2-05 | No delinquency logic runs without resolved `organizationId` |

### 7. Acceptance criteria

- [ ] Org policy 8% fee on ≤4-unit property saves successfully
- [ ] Org policy 15% fee on ≤4-unit property rejected with TX-LF-003 error
- [ ] Org grace 1 day rejected; grace 3 days accepted (TX min 2)
- [ ] Lease without late fee clause → agent blocks auto-fee
- [ ] Initial $50 + daily $10 fees blocked if combined > 12% rent
- [ ] Fee posted → appears in CAP-4 ledger; resident notified
- [ ] PM waive → fee reversed/adjusted; reason in CAP-10
- [ ] Payment plan requires CAP-5 approval before resident sees offer
- [ ] Delinquent tenant maintenance blocked when toggle on
- [ ] **Prod:** StateRulePack-TX has attorney sign-off record (P5)
- [ ] **Prod:** Auto-sent notices use P3-approved templates only

### 8. State expansion (post-Texas)

Same 3-layer model; new `StateRulePack-{ST}.json` per state with attorney review. PM org enables only states they operate in. CA/NY/FL/WA = Phase 2 (complex late fee frameworks).

---

## M2 Runbook — prerequisites & parallel tracks

> **Prod gate:** Do not ship delinquency automation to production until **P1** (Texas attorney review) is ✅. **Master checklist:** [`ATTORNEY-REVIEW-CHECKLIST.md`](./ATTORNEY-REVIEW-CHECKLIST.md) §2.

### Critical path

```
WEEK 1 ──► Eng draft StateRulePack-TX.json from §92.019
           Eng attorney brief (P2) — send RulePack + notice templates

WEEK 2–4 ► Attorney review + revisions (P1) ← BLOCKER for prod
           Build rule validation engine (B1) in parallel

WEEK 3–5 ► OrgDelinquencyPolicy UI + save validation (B2)
           Reminder templates after attorney approves wording (P3)

WEEK 5–6 ► Delinquency agent + ledger posting (B3, B4)
           CAP-10 decision traces for every fee assess/block

WEEK 6+  ► Pilot with lab org; only after P1 ✅
```

### Master task tracker

Status: `⬜ todo` · `🔄 in progress` · `✅ done` · `🅿️ blocked`

#### Track P — Prerequisites (legal — start Week 1)

| ID | Task | Owner | Start by | Est. | Blocker? | Status | Notes |
|----|------|-------|----------|------|----------|--------|-------|
| P1 | **Texas attorney review** of `StateRulePack-TX` + notice templates | Founder → TX RE attorney | **Week 1** | 2–4 wk | **YES — prod gate** | ⬜ | §92.019 caps, grace, 2026 Notice to Pay; fee assessment logic |
| P2 | Prepare attorney brief package | Eng + Founder | Week 1 | 2 days | — | ⬜ | RulePack JSON, 3-layer model doc, sample org policies at boundary |
| P3 | **Attorney-approved notice templates** (reminder, late fee assessed, formal notice) | Attorney → Eng | After P1 | 1 wk | YES for auto-send | ⬜ | SMS/email copy; no auto-send until approved |
| P4 | Confirm 2026 first-delinquency Notice to Pay requirements | Attorney | Week 1 | — | — | ⬜ | Template only; eviction workflow M6 Phase 2 |
| P5 | Document attorney sign-off version | Eng | After P1 | 1 day | — | ⬜ | `StateRulePack-TX` version + `approvedBy` + date in repo |

#### Track B — Build (parallel — sandbox OK before P1)

| ID | Task | Owner | Start by | Depends on | Status | Notes |
|----|------|-------|----------|------------|--------|-------|
| B1 | `StateRulePack-TX.json` + validation engine | Eng | Week 1 | — | ⬜ | Hard-block illegal fee configs |
| B2 | `OrgDelinquencyPolicy` UI with bound validation | Eng | Week 2 | B1 | ⬜ | Show 🔒 fields from state pack |
| B3 | Delinquency agent daily job + rule evaluation | Eng | Week 3 | B1, B2 | ⬜ | Lease → Org → State order |
| B4 | CAP-4 late fee ledger posting + waive flow | Eng | Week 4 | B3 | ⬜ | PM waive requires reason → CAP-10 |
| B5 | CAP-7 reminder SMS/email sequences | Eng | Week 4 | P3 for prod | ⬜ | Dev with placeholder copy until P3 |
| B6 | Payment plan offer + CAP-5 approval queue | Eng | Week 5 | B3 | ⬜ | PM approval required MVP |
| B7 | `structureUnitCount` on Property (≤4 vs >4 cap) | Eng | Week 1 | CAP-1 | ⬜ | Required for TX-LF-003/004 |
| B8 | Block maintenance if delinquent (org toggle) | Eng | Week 3 | CAP-3 | ⬜ | Default on per AI-MVP-DECISIONS |

#### Track D — Locked decisions (M2)

| ID | Decision | Locked value |
|----|----------|--------------|
| D1 | M2 scope | **MVP — CAP-4 + CAP-7** with 3-layer Rules Engine |
| D2 | RulePack authority | **Platform enforces; PM can only be stricter** |
| D3 | Missing lease late-fee clause | **Block auto-fee; PM manual with liability ack** |
| D4 | Payment plans | **MVP with CAP-5 PM approval** |
| D5 | Prod gate | **P1 attorney review required before production** |
| D6 | Differentiator | **Agent hard-blocks illegal fees** (auditable in CAP-10) |

#### Attorney brief checklist (P2)

Send to Texas RE attorney:

- [ ] `StateRulePack-TX` draft (rules TX-LF-001 through TX-PAY-001 in this doc)
- [ ] Three-layer model diagram (State → Org → Lease)
- [ ] Sample org policies: 8% fee (valid), 15% fee (blocked), 1-day grace (blocked)
- [ ] Reminder + formal notice draft templates
- [ ] Question: 2026 Notice to Pay Rent or Vacate — required wording for first delinquency in term
- [ ] Question: NSF/bounced check fee limits for TX residential
- [ ] Request: written sign-off on `StateRulePack-TX` version number for production

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

## Locked decisions (M2 — 2026-07-05)

| ID | Decision | Value |
|----|----------|-------|
| M2-1 | Scope | **CAP-4 + CAP-7** with Rules Engine |
| M2-2 | RulePack authority | Platform enforces; PM can only be stricter |
| M2-3 | Invalid lease (no late fee clause) | Block auto-fee; PM manual with liability ack |
| M2-4 | Payment plans | MVP with CAP-5 PM approval |
| M2-5 | Prod prerequisite | **Texas attorney review (Runbook P1) before production** |

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
