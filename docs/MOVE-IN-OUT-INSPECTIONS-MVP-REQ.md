# M4: Move-in / Move-out Inspections — Full Requirements

**Status:** locked  
**Market gap:** M4  
**CAPs:** CAP-2, CAP-7, CAP-4 (M5 deposits), CAP-8, CAP-10; M3 decline triggers move-out  
**Locked:** 2026-07-05  
**MVP state:** Texas (`TX`)

> **Companion req doc for M4.** Full move-in + move-out checklists with photos, comparison report, deposit deduction handoff to M5.

---

## 1. Problem & rationale

Move-in and move-out condition documentation prevents deposit disputes and supports Texas trust-account rules (itemized deductions within 30 days of move-out). PM staff today use paper forms, Google Drive photos, or third-party apps (DoorLoop AI inspections) — disconnected from lease, ledger, and deposit sub-ledger.

**Why full move-in + move-out for MVP:** User locked **A — full both phases**. Move-out inspection without move-in baseline is weak; together they enable M5 security deposit lifecycle (comparison → suggested deductions → PM/Owner review → CAP-4).

**Competitor reference:** DoorLoop AI inspections; AppFolio/Buildium photo checklists — parity feature, not core APM differentiator, but **required** for deposit accuracy and resident trust.

---

## 2. Product vision

```
Lease signed (CAP-2) → Move-in inspection assigned
  → Resident and/or PM completes room checklist + photos
  → Baseline stored on lease/unit (M10 vault)

Move-out triggered (M3 decline, non-renewal, or lease end)
  → Move-out inspection assigned
  → Second checklist + photos
  → Inspection Agent compares in vs out
  → Flags damage beyond normal wear → suggested deposit deductions (M5/CAP-4)
  → PM/Owner review → itemized deduction list (Texas)
```

**Differentiator:** Inspection data lives in the same system as lease, deposit ledger, and renewal/vacancy flow — not a bolt-on app.

---

## 3. User stories

| Actor | Story |
|-------|-------|
| PM admin | I configure org inspection template (rooms, items, required photos). |
| PM admin / staff | I complete or co-sign move-in/move-out inspection on site. |
| Resident | I complete move-in checklist with photos on my phone before or at key handoff. |
| Resident | I complete move-out checklist at departure; I see move-in baseline for reference. |
| Inspection Agent | I compare move-in vs move-out photos/checklist; flag discrepancies; suggest deposit line items. |
| PM admin | I review suggested deductions, edit, and approve before deposit return (M5). |
| Owner | I view inspection reports for my property; approve large deductions if org policy requires. |
| Accountant | Approved deductions post to CAP-4 deposit sub-ledger (M5). |

---

## 4. End-to-end flows

### Move-in happy path

1. Lease signed + CAP-12 key issued (or scheduled move-in date).
2. System creates `Inspection` type=`move_in`, status=`pending`, due by move-in date.
3. Resident receives CAP-7 link; completes room-by-room checklist + min photos per org template.
4. Optional: PM staff adds supplemental photos/notes.
5. Both parties acknowledge (e-sign or checkbox) → status=`completed`; baseline locked.
6. Photos/docs stored under unit/lease (M10); CAP-10 log.

### Move-out happy path

1. Trigger: M3 decline, non-renewal, or `lease.endDate` approaching (configurable T-7 days).
2. `Inspection` type=`move_out` created; resident + PM notified.
3. Resident completes move-out checklist + photos (same template structure).
4. Inspection Agent runs comparison: room/item level match move-in vs move-out.
5. Agent outputs `InspectionComparisonReport`: normal wear vs damage flags + **suggested deduction amounts** (org line-item catalog).
6. PM reviews → approves/edits deductions → feeds M5 deposit return workflow.
7. Texas: itemized deduction list + forwarding address captured before deposit disposition.

### Edge paths

| Trigger | Behavior |
|---------|----------|
| Resident skips move-in | PM-only inspection; flag `resident_skipped`; weaker dispute position noted |
| Resident skips move-out | PM schedules move-out inspection; charge PM-configured fee if lease allows |
| Dispute on deductions | Resident dispute flag → PM manual review; CAP-10 trace |
| Normal wear | Agent excludes from deduction suggestions (org policy definitions) |
| Partial completion | Reminders; block deposit return until move-out complete or PM override |
| Month-to-month no formal move-in on platform | PM imports legacy or completes PM-only baseline |

---

## 5. Functional requirements

| ID | Requirement | MVP |
|----|-------------|-----|
| FR-M4-01 | Org-configurable inspection template (rooms, items, min photos) | ✅ |
| FR-M4-02 | Move-in inspection auto-created on lease activation | ✅ |
| FR-M4-03 | Move-out inspection auto-created on M3 decline / lease end | ✅ |
| FR-M4-04 | Resident mobile-friendly checklist + photo upload (CAP-7) | ✅ |
| FR-M4-05 | PM staff can complete or augment inspection | ✅ |
| FR-M4-06 | Move-in baseline locked after completion (immutable) | ✅ |
| FR-M4-07 | Side-by-side or structured comparison move-in vs move-out | ✅ |
| FR-M4-08 | Agent suggests deposit deduction line items (not auto-charge) | ✅ |
| FR-M4-09 | PM approval before deductions applied to M5/CAP-4 | ✅ |
| FR-M4-10 | Store photos in Supabase Storage; link to lease/unit | ✅ |
| FR-M4-11 | CAP-10 trace for completion, comparison, deduction approval | ✅ |
| FR-M4-12 | Export comparison + itemized list for Texas 30-day deposit return | ✅ |
| FR-M4-13 | Reminder sequences for incomplete inspections | ✅ |

---

## 6. Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-M4-01 | Photos scoped by `organizationId`; RLS enforced |
| NFR-M4-02 | Move-in record immutable after lock (append-only notes allowed) |
| NFR-M4-03 | Support 20+ photos per inspection; compress on upload |
| NFR-M4-04 | Comparison job completes in <60s for typical unit |

---

## 7. Data model & integrations

| Entity | Key fields |
|--------|------------|
| `InspectionTemplate` | organizationId, rooms[], items[], minPhotosPerRoom, version |
| `Inspection` | id, organizationId, leaseId, unitId, type (move_in\|move_out), status, completedAt, lockedAt |
| `InspectionItem` | inspectionId, room, item, condition, notes, photoUrls[] |
| `InspectionComparison` | moveInId, moveOutId, flags[], suggestedDeductions[] |
| `DeductionSuggestion` | item, amount, reason, approved, approvedBy |

**Integrations:**

| CAP / M | Use |
|---------|-----|
| CAP-2 | Trigger move-in on lease signed |
| CAP-7 | Resident checklist UI |
| M3 | Trigger move-out on decline |
| M5 | Deposit sub-ledger + 30-day return |
| M10 | Document vault storage |
| CAP-10 | Audit |
| CAP-12 | Optional: move-in before key release (org toggle) |

**Optional org toggle:** Block digital key (CAP-12) until move-in inspection complete.

---

## 8. Acceptance criteria

- [ ] Lease signed → move-in inspection created and resident notified
- [ ] Resident completes move-in with photos → baseline locked
- [ ] M3 decline → move-out inspection created
- [ ] Move-out completion → comparison report generated
- [ ] Agent suggests deductions; PM must approve before CAP-4/M5 posting
- [ ] Move-in photos visible during move-out for resident reference
- [ ] Itemized deduction export includes descriptions + amounts (Texas)
- [ ] Two orgs cannot access each other's inspection photos
- [ ] CAP-10 trace for full lifecycle

---

## 9. Runbook & prerequisites

| ID | Task | Blocker? | Status |
|----|------|----------|--------|
| P1 | Texas attorney review: deposit deduction itemization, normal wear definitions | Prod copy | ⬜ |
| P2 | Align with M5 security deposit sub-ledger schema | MVP | ⬜ |
| B1 | InspectionTemplate + Inspection entities | — | ⬜ |
| B2 | Resident mobile checklist (CAP-7) | — | ⬜ |
| B3 | Comparison agent + deduction suggestions | — | ⬜ |
| B4 | M3 decline → move-out trigger | — | ⬜ |
| B5 | PM review UI + M5 handoff | — | ⬜ |

**Dependency:** M5 (security deposits) should be locked MVP for full value; M4 move-out deductions assume M5 native sub-ledger.

---

## 10. Locked decisions (2026-07-05)

| ID | Decision | Value |
|----|----------|-------|
| M4 scope | **A — Full MVP** | Move-in + move-out |
| Deductions | Suggest only; PM approves; M5/CAP-4 posts | — |
| Key gate | Optional org toggle: key after move-in complete | TBD default off for Pro speed |
| AI comparison | Agent flags + suggests; not autonomous charge | CAP-5 if large deduction |

---

## Related artifacts

| Doc | Link |
|-----|------|
| M3 move-out trigger | [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) |
| M5 deposits | `MARKET-GAP-CHECKLIST.md` (TBD) |
| CAP-7 | [`capabilities/CAP-7-resident-portal.md`](./capabilities/CAP-7-resident-portal.md) |
| CAP-2 | [`capabilities/CAP-2-autonomous-leasing.md`](./capabilities/CAP-2-autonomous-leasing.md) |
