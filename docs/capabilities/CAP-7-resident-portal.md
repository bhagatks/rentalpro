# CAP-7: Resident Portal & Unified Comms Hub

**Status:** draft  
**SPEC reference:** CAP-7 + **M7** expand  
**MVP phase:** 2 (rent + maintenance MVP)  
**Depends on:** CAP-11, CAP-1

## Intent & success (from SPEC)

- **Intent:** Residents self-serve via portal/messaging for rent payment, maintenance requests, lease documents, status 24/7. **M7:** PM admin unified inbox for resident, owner, vendor, and lead threads across chat/SMS/email.
- **Success:** Resident submits maintenance request and pays rent without staff contact; both reflect in ledger and WO system within 5 minutes. PM admin sees all party threads in one inbox; agents reply autonomously on Pro within policy.

## User stories

| Actor | Story |
|-------|-------|
| Resident | I pay rent on `{pmco}.rentalpro.ai` branded portal. |
| Resident | I submit maintenance request with photo/video via chat. |
| Resident | I view my lease and payment history. |
| Resident | I receive status updates via chat/SMS/email (no voice). |
| PM admin | I monitor all lead, resident, owner, and vendor threads in one inbox (M7). |
| PM admin | I take over any agent thread or approve drafts on Basic plan (M7). |

## Happy path

1. Resident invited after lease import or signing (CAP-2).
2. Resident logs in via Clerk (resident role) on org subdomain.
3. **Rent:** Stripe payment → CAP-4 ledger entry within 5 min.
4. **Maintenance:** Chat + media upload → work order created → CAP-3 agent triggered.
5. Push/status via chat thread; portal shows WO timeline.
6. **M7:** Same thread backend powers SMS/email; PM inbox shows unified history.

## Escalation path

| Trigger | Action |
|---------|--------|
| Payment fails | Retry + notify resident |
| Emergency maintenance keywords | CAP-3 emergency path |
| Agent draft on Basic | CAP-5 PM approval before send (M7) |
| Legal notice category | Template-only send — no freeform AI (M2/M7) |

## Integrations

| Service | Use |
|---------|-----|
| Stripe | Rent collection |
| Supabase Storage | Maintenance photos/video |
| Twilio | SMS inbound/outbound (M7) |
| Email provider | Inbound parse + outbound (M7) |
| CAP-3 | Maintenance agent trigger |
| CAP-11 | Branded subdomain |
| M7 | Unified inbox — see [`UNIFIED-COMMS-HUB-MVP-REQ.md`](../UNIFIED-COMMS-HUB-MVP-REQ.md) |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `Resident` | organizationId, userId, leaseId, contactPrefs |
| `MaintenanceRequest` | organizationId, unitId, residentId, description, mediaUrls[], status |
| `ConversationThread` | organizationId, partyType, partyId, entityRef (M7) |
| `Message` | threadId, channel, direction, body, senderType (M7) |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/resident/payments/rent` | Pay rent |
| POST | `/api/resident/maintenance` | Submit request |
| GET | `/api/resident/lease` | Lease docs |
| GET | `/api/resident/maintenance/:id` | WO status |
| GET | `/api/orgs/current/inbox/threads` | PM unified inbox (M7) |

## Acceptance tests

- [ ] Rent payment reflects in ledger within 5 minutes
- [ ] Maintenance request creates WO and triggers agent
- [ ] Portal shows org branding (CAP-11)
- [ ] Resident cannot access other units/orgs
- [ ] PM inbox shows resident thread with portal + SMS messages (M7)

## Open questions

- [ ] PWA install prompt — Phase 2?
- [ ] Twilio per-org vs shared number pool?

## Market parity sub-features

See `docs/MARKET-GAP-CHECKLIST.md`.

- [ ] Autopay / recurring rent
- [x] Delinquency notices & payment plans (M2) — [`DELINQUENCY-RULES-ENGINE.md`](../DELINQUENCY-RULES-ENGINE.md)
- [x] Bulk resident messaging (emergencies) — [`UNIFIED-COMMS-HUB-MVP-REQ.md`](../UNIFIED-COMMS-HUB-MVP-REQ.md) (M7)
- [x] Unified comms hub (M7) — [`UNIFIED-COMMS-HUB-MVP-REQ.md`](../UNIFIED-COMMS-HUB-MVP-REQ.md)
- [ ] Announcements / community feed — Phase 2 non-goal

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Chat/SMS/email only; no voice |
| 2026-07-05 | M7 locked Full MVP — expand CAP-7 with unified inbox; no new CAP |
