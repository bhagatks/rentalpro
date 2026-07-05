# M3: Lease Renewals — Full Requirements

**Status:** locked  
**Market gap:** M3  
**CAPs:** CAP-2 (primary), CAP-5, CAP-7, CAP-8, CAP-4, CAP-10; M1 on decline  
**Locked:** 2026-07-05  
**MVP state:** Texas (`TX`)

> **Companion req doc for M3.** All 9 sections filled — use when writing renewal stories/PRD. Chat is not the system of record.

---

## 1. Problem & rationale

Leases expire on a fixed schedule. PM staff today track expirations in spreadsheets, manually send renewal offers, negotiate rent increases with owners, chase resident signatures, and update ledger terms — repeating much of the initial lease workflow. AppFolio Flows and Buildium automate renewal reminders and offers; without M3, RentalPro's "autonomous leasing" story stops at move-in.

**Why it matters:** Renewal is the second half of the leasing lifecycle. APM must cover **90-day notice → offer → owner governance (rent increases) → resident accept/decline → e-sign → ledger update**, with decline feeding vacancy/listing (M1).

**Terminology:** Legal **landlord** = platform **`owner`** role (CAP-8). PM company acts as **agent** on managed properties (Texas §92.201). No separate `landlord` RBAC role.

---

## 2. Product vision

```
90 days before lease end
  → Renewal Agent builds offer (rent, term, from org policy + lease + TX rules)
  → Rent increase > threshold? → Owner approves (CAP-8 / CAP-5)
  → PM review (Basic) or auto-send (Pro within policy)
  → Resident accept/decline (CAP-7)
  → Accept → renewal e-sign (CAP-2 template flow) → CAP-4 ledger update
  → Decline → move-out path → M1 Listing Package when vacant
```

**Differentiator vs market:**

| Market | RentalPro |
|--------|-----------|
| PM tracks expirations manually | Agent triggers at 90/60/30 day milestones |
| Owner approval for increases ad hoc | Configurable threshold; CAP-5 queue + owner portal |
| Separate renewal tool | Same template + e-sign stack as new lease (CAP-2) |
| Decline → manual re-list | Auto-handoff to M1 syndication when unit vacant |

---

## 3. User stories

| Actor | Story |
|-------|-------|
| PM admin | I configure org renewal policy: default term, max auto-increase %, owner approval threshold. |
| PM admin | I review renewal offers before send on Basic plan; I see all pending renewals on a dashboard. |
| **Owner (landlord)** | I approve rent increases above my threshold before the offer goes to the resident. |
| **Owner** | I receive notification when renewal offer is sent, accepted, or declined. |
| **Owner** | I view renewal status for my properties in the owner portal (CAP-8). |
| Resident | I receive renewal offer in portal/email; I accept or decline; I e-sign renewal lease. |
| Renewal Agent | I trigger at 90 days before lease end; build offer; route approvals; send to resident; update ledger on sign. |
| Renewal Agent | I do not re-run screening on same-tenant renewal unless org policy requires it. |
| Leasing Agent | I reuse PM template + Texas addenda for renewal document (same as CAP-2). |
| Accountant | Updated rent flows to CAP-4 ledger after renewal effective date. |

---

## 4. End-to-end flows

### Happy path — renewal accepted (Pro, increase within PM auto band)

1. **T-90 days:** Renewal Agent creates `RenewalCase` for lease approaching end.
2. Agent calculates offer: new rent (CPI/org % cap), term (e.g. 12 mo), from `OrgRenewalPolicy`.
3. Rent increase ≤ `renewalAutoApproveIncreasePercent` → no owner approval required.
4. Pro + offer matches policy → auto-send to resident (CAP-7).
5. Resident accepts → renewal lease generated from template (CAP-2) → e-sign.
6. On sign: extend `Lease.endDate`, update `rentAmount`, post CAP-4 schedule change.
7. Notify Owner (info only) + PM admin; CAP-10 trace complete.

### Happy path — owner approval required (rent increase above threshold)

1. Agent calculates offer with 8% increase; org threshold for owner approval = 5%.
2. **CAP-5 ApprovalRequest** → **Owner** queue in owner portal (CAP-8).
3. Owner approves → offer proceeds to resident (after PM review on Basic).
4. Continue accept → e-sign → ledger as above.

### Basic plan

- PM admin approves every renewal offer before send to resident (even if owner already approved increase).
- No auto-send to resident.

### Decline / non-renewal path

1. Resident declines or PM/Owner elects non-renewal.
2. `RenewalCase.status = declined`; move-out date from lease end.
3. Notify Owner + PM; schedule move-out checklist (Phase 2 M4).
4. At vacancy → trigger M1 Listing Agent (Listing Package).

### Edge paths

| Trigger | Behavior |
|---------|----------|
| Rent decrease or flat renewal | Owner notification only; no owner approval |
| Owner rejects increase | PM revises offer or start non-renewal |
| PM rejects on Basic | Offer returned to agent/PM edit |
| Month-to-month conversion | Separate org policy branch; TX notice timing per lease type |
| Lease lacks renewal clause | PM manual path; agent suggests PM review |
| Same tenant, org requires re-screening | Route to CAP-2 screening (optional org toggle) |
| Multiple owners on one property | Approval from designated primary owner or all (org config TBD — MVP: primary) |
| Resident no response by T-30 | Escalating reminders; PM alert at T-14 |

---

## 5. Functional requirements

| ID | Requirement | MVP |
|----|-------------|-----|
| FR-M3-01 | `RenewalCase` entity linked to lease; statuses: draft, pending_owner, pending_pm, sent, accepted, declined, signed, expired | ✅ |
| FR-M3-02 | Agent triggers at configurable milestones (default 90, 60, 30 days before end) | ✅ |
| FR-M3-03 | `OrgRenewalPolicy`: default term, max increase %, owner approval threshold %, screening re-run toggle | ✅ |
| FR-M3-04 | Owner approval via CAP-5 when increase > threshold | ✅ |
| FR-M3-05 | PM approval on Basic before resident send | ✅ |
| FR-M3-06 | Pro auto-send when within policy and owner approved (if needed) | ✅ |
| FR-M3-07 | Resident accept/decline in CAP-7 portal | ✅ |
| FR-M3-08 | Renewal lease via CAP-2 template + e-sign (not greenfield doc) | ✅ |
| FR-M3-09 | Owner/agent identity on renewal lease (§92.201) | ✅ |
| FR-M3-10 | CAP-4 rent + end date update on signed renewal | ✅ |
| FR-M3-11 | Owner notifications: offer sent, accepted, declined | ✅ |
| FR-M3-12 | Decline → handoff to M1 when unit vacant | ✅ |
| FR-M3-13 | CAP-10 trace for every offer, approval, sign, decline | ✅ |
| FR-M3-14 | Renewal dashboard for PM admin (pipeline by status) | ✅ |
| FR-M3-15 | Owner portal view: pending approvals + renewal status per property | ✅ |

---

## 6. Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-M3-01 | All renewal data scoped by `organizationId` |
| NFR-M3-02 | Owner sees only their properties (CAP-11) |
| NFR-M3-03 | Renewal offer immutable after sent to resident until withdrawn |
| NFR-M3-04 | Idempotent milestone jobs — no duplicate offers for same lease/period |

---

## 7. Data model & integrations

| Entity | Key fields |
|--------|------------|
| `RenewalCase` | id, organizationId, leaseId, unitId, propertyId, ownerId, status, currentRent, offeredRent, offeredTermMonths, increasePercent, milestones JSON, traceId |
| `OrgRenewalPolicy` | organizationId, defaultTermMonths, maxIncreasePercent, ownerApprovalThresholdPercent, rescreenOnRenewal, milestoneDays[] |
| `RenewalOffer` | renewalCaseId, sentAt, expiresAt, residentResponse, signedLeaseId |

**Integrations:**

| CAP / Service | Use |
|---------------|-----|
| CAP-2 | Template merge, e-sign, Texas addenda |
| CAP-5 | Owner + PM approval queues |
| CAP-7 | Resident portal accept/decline |
| CAP-8 | Owner portal approvals + notifications |
| CAP-4 | Rent amount and lease term in ledger |
| CAP-10 | Decision traces |
| M1 | Listing Package on decline → vacancy |

**API (draft):**

| Method | Endpoint | Actor |
|--------|----------|-------|
| GET | `/api/orgs/current/renewals` | PM admin |
| GET | `/api/owner/renewals/pending-approval` | Owner |
| POST | `/api/owner/renewals/:id/approve` | Owner |
| POST | `/api/resident/renewals/:id/respond` | Resident |
| POST | `/api/orgs/current/renewals/:id/send` | PM admin |

---

## 8. Acceptance criteria

- [ ] Lease 90 days from end → RenewalCase created automatically
- [ ] 5% increase with 5% owner threshold → no owner approval; 8% increase → owner queue
- [ ] Owner approves increase → offer visible to resident after PM step (Basic) or auto (Pro)
- [ ] Owner rejects → PM notified; offer not sent to resident
- [ ] Resident accepts → renewal e-sign completes; lease end date extended
- [ ] CAP-4 reflects new rent from renewal effective date
- [ ] Resident declines → Owner + PM notified; M1 triggered at vacancy
- [ ] Renewal lease includes Owner name + PM agent (§92.201)
- [ ] Owner portal shows only their property renewals
- [ ] Full CAP-10 trace for offer → approvals → sign/decline
- [ ] Basic: no resident send without PM approval

---

## 9. Runbook & prerequisites

| ID | Task | Blocker? | Status |
|----|------|----------|--------|
| P1 | Texas attorney review: renewal notice timing, rent increase notice requirements | Prod copy | ⬜ |
| P2 | Reuse CAP-2 e-sign provider decision | MVP | ⬜ |
| P3 | Owner portal approval UI (depends on CAP-8 login — M8 partial) | MVP | ⬜ |
| B1 | RenewalCase + OrgRenewalPolicy schema | — | ⬜ |
| B2 | Renewal Agent milestone scheduler | — | ⬜ |
| B3 | Owner approval integration CAP-5/CAP-8 | — | ⬜ |
| B4 | Resident accept/decline CAP-7 | — | ⬜ |
| B5 | Decline → M1 vacancy hook | — | ⬜ |

**Note:** Owner portal login is shared with **M8**; M3 requires owner **approval queue** minimum for renewals even if M8 broader UX is still open.

---

## 10. Locked decisions (2026-07-05)

| ID | Decision | Value |
|----|----------|-------|
| M3 scope | **A — Full MVP** | CAP-2 sub-feature + Owner approval |
| Landlord role | **`owner`** RBAC | Legal lessor; PM is agent |
| Owner approval | Required when increase > org threshold | Notify-only for flat/decrease |
| Screening on renewal | Optional org toggle | Default: no re-screen same tenant |
| Decline path | M1 Listing when vacant | — |

---

## Related artifacts

| Doc | Link |
|-----|------|
| CAP-2 | [`capabilities/CAP-2-autonomous-leasing.md`](./capabilities/CAP-2-autonomous-leasing.md) |
| CAP-8 | [`capabilities/CAP-8-owner-reporting.md`](./capabilities/CAP-8-owner-reporting.md) |
| M1 decline path | [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) |
| Traceability | [`REQ-TRACEABILITY.md`](./REQ-TRACEABILITY.md) |
