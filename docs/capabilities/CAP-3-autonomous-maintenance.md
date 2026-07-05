# CAP-3: Autonomous Maintenance

**Status:** draft  
**SPEC reference:** CAP-3  
**MVP phase:** 3  
**Depends on:** CAP-5, CAP-7, CAP-9, CAP-10

## Intent & success (from SPEC)

- **Intent:** Autonomous maintenance from resident report through resolution—diagnosis, vendor quote, scheduling, completion verification, vendor payment—within spend governance.
- **Success:** Non-emergency request with video triaged, dispatched, completed, and paid without human action when cost under PM threshold; resident confirms resolution.

## User stories

| Actor | Story |
|-------|-------|
| Resident | I report issue via chat with video; get status updates. |
| Maintenance agent | I diagnose, dispatch vendor, verify completion, trigger payment. |
| PM admin | I approve over-threshold repairs (Basic) or review exceptions. |
| Vendor | I receive SMS/email with WO details; submit quote and completion photo. |

## Happy path

1. Resident submits request (CAP-7) with photo/video.
2. Agent classifies: emergency vs routine vs cosmetic.
3. Rent delinquency check → if delinquent, escalate to PM (TBD recommend yes).
4. Agent diagnoses category + estimates cost.
5. CAP-5 governance: under limit → auto; over → approval queue.
6. Agent selects vendor (CAP-9); outreach via SMS/email (no voice).
7. Vendor quotes + schedules via chat link.
8. Work completed → vendor uploads completion photo.
9. Resident confirms resolution via chat.
10. Stripe Connect pays vendor → CAP-4 ledger entry.
11. Full trace in CAP-10.

## Escalation path

| Trigger | Action |
|---------|--------|
| Emergency list (gas, sewage, flood, wiring, no heat <40°F, no water) | Auto-dispatch — no spend approval |
| Cost > governance limit | PM approval (CAP-5) |
| Resident disputes resolution | PM review queue |
| Tenant delinquent on rent | Escalate to PM (recommended) |

## Integrations

| Service | Use |
|---------|-----|
| CAP-9 | Vendor DB + outreach |
| Stripe Connect | Vendor payment |
| Supabase Storage | Video/photos |
| CAP-5, CAP-10 | Governance + audit |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `WorkOrder` | organizationId, unitId, category, priority, estimatedCost, status, vendorId, traceId |
| `WorkOrderEvent` | workOrderId, type, payload, createdAt |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/orgs/current/work-orders` | List WOs |
| GET | `/api/orgs/current/work-orders/:id` | WO detail + trace link |
| POST | `/api/vendor/work-orders/:id/quote` | Vendor quote |
| POST | `/api/vendor/work-orders/:id/complete` | Completion photo |

## Acceptance tests

- [ ] Under-threshold WO completes without PM touch on Pro
- [ ] Over-threshold WO waits for approval
- [ ] Emergency bypasses approval
- [ ] Payment recorded against WO in ledger
- [ ] Resident confirmation required before pay (TBD)

## Open questions

- [ ] Require resident confirmation before vendor payment?
- [ ] Cosmetic requests — agent auto-close without vendor?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Full lifecycle MVP |
| 2026-07-05 | Emergency auto-dispatch locked (see AI-MVP-DECISIONS) |

**See also:** `docs/AI-MVP-DECISIONS.md`
