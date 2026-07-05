# CAP-9: Vendor Management

**Status:** draft  
**SPEC reference:** CAP-9  
**MVP phase:** 3  
**Depends on:** CAP-1, CAP-11

## Intent & success (from SPEC)

- **Intent:** Vendor database; outreach, quote collection, scheduling, payment via integrated rails.
- **Success:** Dispatched WO results in vendor quote, appointment, completion photo, and Stripe Connect payment recorded without duplicate entry.

## User stories

| Actor | Story |
|-------|-------|
| PM admin | I maintain vendor list with trades, service area, COI expiry. |
| Maintenance agent | I match WO to qualified vendor and send outreach. |
| Vendor | I receive WO via SMS/email; respond with quote without logging in (MVP). |
| Vendor | I optionally use vendor portal for quote upload (Phase 2 polish). |

## Happy path

1. PM imports vendors (CAP-1): name, trades[], phone, email, serviceZipCodes[], coiExpiresAt.
2. Agent filters vendors by trade + geography + COI valid.
3. Agent sends SMS/email with WO summary + magic link for quote/schedule.
4. Vendor submits quote via link → stored on WO.
5. Agent accepts quote (or PM approves if over threshold).
6. Completion photo uploaded via same link.
7. Payment via Stripe Connect (CAP-3/4).

## Escalation path

| Trigger | Action |
|---------|--------|
| COI expired | Block dispatch; notify PM |
| No vendor response in 24h | Agent tries next vendor; notify PM |
| Quote over threshold | CAP-5 approval |

## Integrations

| Service | Use |
|---------|-----|
| Stripe Connect | Vendor payouts |
| SMS/email (TBD Twilio) | Outreach — no voice |
| CAP-3 | WO lifecycle |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `Vendor` | organizationId, name, trades[], email, phone, stripeConnectId, coiExpiresAt, serviceArea |
| `VendorQuote` | workOrderId, vendorId, amount, proposedDate, status |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| CRUD | `/api/orgs/current/vendors` | Vendor management |
| GET | `/api/vendor/jobs/:token` | Magic-link WO view |
| POST | `/api/vendor/jobs/:token/quote` | Submit quote |

## Acceptance tests

- [ ] Agent dispatches only to vendors with valid COI
- [ ] Quote appears on WO without duplicate entry
- [ ] Payment ties to vendor + WO in ledger
- [ ] Expired COI blocks dispatch

## Open questions

- [ ] COI upload/verification — manual MVP vs automated?
- [ ] Vendor Stripe Connect onboarding flow?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | SMS/email outreach first; chat only |
