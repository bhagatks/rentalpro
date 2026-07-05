# CAP-8: Owner Reporting

**Status:** draft  
**SPEC reference:** CAP-8  
**MVP phase:** 5  
**Depends on:** CAP-4, CAP-11

## Intent & success (from SPEC)

- **Intent:** Owners receive automated financial statements, distribution calculations, portfolio performance on a defined schedule.
- **Success:** Owner views accurate month-to-date statement matching ledger; distribution matches accountant-approved monthly close.

## User stories

| Actor | Story |
|-------|-------|
| Owner | I log into branded portal and see my property financials. |
| Owner | I receive monthly statement after accountant sign-off. |
| PM admin | I configure statement schedule and delivery (email/portal). |
| Accounting agent | I compute distributions only after CAP-4 monthly sign-off. |

## Happy path

1. CAP-4 monthly close signed off by accountant.
2. System generates owner statements per property: income, expenses, management fees, net.
3. Distribution amount calculated per owner agreement (CAP-1).
4. Owner notified via email; views in owner portal on org subdomain.
5. Optional: Stripe Connect payout to owner account (TBD).

## Escalation path

| Trigger | Action |
|---------|--------|
| Ledger mismatch in statement | Block publish; alert PM |
| Missing owner forwarding address | Hold distribution (Texas security deposit parallel) |

## Integrations

| Service | Use |
|---------|-----|
| CAP-4 | Ledger source of truth |
| CAP-11 | Branded owner portal |
| Email (TBD) | Statement delivery |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `OwnerStatement` | organizationId, ownerId, propertyId, period, lines[], netDistribution, status |
| `Distribution` | organizationId, ownerId, amount, period, paidAt, status |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/owner/statements` | Owner statement list |
| GET | `/api/owner/statements/:period` | Statement detail |
| GET | `/api/orgs/current/owners/:id/statements` | PM view |

## Acceptance tests

- [ ] Statement totals match ledger for test period
- [ ] Distribution not visible until accountant sign-off
- [ ] Owner sees only their properties
- [ ] Org branding on owner portal

## Open questions

- [ ] Automated owner payout via Stripe or statement-only MVP?
- [ ] Statement PDF export?

## Market parity sub-features (TBD)

See `docs/MARKET-GAP-CHECKLIST.md`.

- [ ] Dedicated owner portal login (M8)
- [ ] Owner approval for capital repairs
- [ ] ACH distribution execution

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Draft micro-spec; gated on CAP-4 sign-off |
