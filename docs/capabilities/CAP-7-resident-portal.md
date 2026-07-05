# CAP-7: Resident Portal

**Status:** draft  
**SPEC reference:** CAP-7  
**MVP phase:** 2 (rent + maintenance MVP)  
**Depends on:** CAP-11, CAP-1

## Intent & success (from SPEC)

- **Intent:** Residents self-serve via portal/messaging for rent payment, maintenance requests, lease documents, status 24/7.
- **Success:** Resident submits maintenance request and pays rent without staff contact; both reflect in ledger and WO system within 5 minutes.

## User stories

| Actor | Story |
|-------|-------|
| Resident | I pay rent on `{pmco}.rentalpro.ai` branded portal. |
| Resident | I submit maintenance request with photo/video via chat. |
| Resident | I view my lease and payment history. |
| Resident | I receive status updates via chat/SMS/email (no voice). |

## Happy path

1. Resident invited after lease import or signing (CAP-2).
2. Resident logs in via Clerk (resident role) on org subdomain.
3. **Rent:** Stripe payment → CAP-4 ledger entry within 5 min.
4. **Maintenance:** Chat + media upload → work order created → CAP-3 agent triggered.
5. Push/status via chat thread; portal shows WO timeline.

## Escalation path

| Trigger | Action |
|---------|--------|
| Payment fails | Retry + notify resident |
| Emergency maintenance keywords | CAP-3 emergency path |

## Integrations

| Service | Use |
|---------|-----|
| Stripe | Rent collection |
| Supabase Storage | Maintenance photos/video |
| CAP-3 | Maintenance agent trigger |
| CAP-11 | Branded subdomain |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `Resident` | organizationId, userId, leaseId, contactPrefs |
| `MaintenanceRequest` | organizationId, unitId, residentId, description, mediaUrls[], status |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/resident/payments/rent` | Pay rent |
| POST | `/api/resident/maintenance` | Submit request |
| GET | `/api/resident/lease` | Lease docs |
| GET | `/api/resident/maintenance/:id` | WO status |

## Acceptance tests

- [ ] Rent payment reflects in ledger within 5 minutes
- [ ] Maintenance request creates WO and triggers agent
- [ ] Portal shows org branding (CAP-11)
- [ ] Resident cannot access other units/orgs

## Open questions

- [ ] PWA install prompt — Phase 2?
- [ ] SMS thread provider (Twilio)?

## Market parity sub-features (TBD)

See `docs/MARKET-GAP-CHECKLIST.md`.

- [ ] Autopay / recurring rent
- [x] Delinquency notices & payment plans (M2) — [`DELINQUENCY-RULES-ENGINE.md`](../DELINQUENCY-RULES-ENGINE.md)
- [ ] Bulk resident messaging (emergencies)
- [ ] Announcements / community feed

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Chat/SMS/email only; no voice |
