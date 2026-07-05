# CAP-4: Autonomous Accounting

**Status:** draft  
**SPEC reference:** CAP-4  
**MVP phase:** 2  
**Depends on:** CAP-1, CAP-5, CAP-10

## Intent & success (from SPEC)

- **Intent:** Fully automated double-entry ledger fed by payment/bank APIs; AI categorizes every transaction; monthly period flagged for human sign-off before owner distributions.
- **Success:** 100% of closed-month transactions auto-categorized with zero manual journal entries; accountant approves one monthly report; owner distributions calculate correctly.

## User stories

| Actor | Story |
|-------|-------|
| Accounting agent | I categorize every inbound/outbound transaction to GL accounts. |
| PM admin | I see pending-review items flagged by AI. |
| Accountant | I sign off monthly report before distributions run. |
| Owner | I receive accurate distribution after sign-off (CAP-8). |

## Happy path

1. Stripe/Plaid webhook posts transaction → `LedgerTransaction` created.
2. Accounting agent loads context (lease, WO, vendor, property).
3. Agent assigns debit/credit accounts → double-entry journal auto-posted.
4. Low-confidence categorization → flagged `pending_review`.
5. Month-end: agent closes period → `MonthlyClose` status `awaiting_signoff`.
6. Accountant reviews report → approves → distributions calculated (CAP-8).
7. Every categorization logged in CAP-10.

## Escalation path

| Trigger | Approver |
|---------|----------|
| Monthly close | Accountant sign-off (always) |
| Unmatched transaction | PM admin queue |
| Distribution amount anomaly | PM admin + accountant |

## Integrations

| Service | Use |
|---------|-----|
| Stripe Connect | Rent payments, vendor payouts |
| Plaid | Bank feed (TBD) |
| CAP-5 | Distribution requires post-signoff gate |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `ChartOfAccounts` | organizationId, accountCode, name, type |
| `JournalEntry` | organizationId, date, lines[], sourceRef, categorizedBy (agent\|human) |
| `LedgerTransaction` | organizationId, amount, source, externalId, status |
| `MonthlyClose` | organizationId, period, status, signedOffBy, signedOffAt |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/orgs/current/ledger/transactions` | Transaction list |
| GET | `/api/orgs/current/ledger/monthly/:period` | Monthly report |
| POST | `/api/orgs/current/ledger/monthly/:period/signoff` | Accountant approve |
| PATCH | `/api/orgs/current/ledger/transactions/:id/recategorize` | Human override |

## Acceptance tests

- [ ] Rent payment auto-categorizes to correct GL accounts
- [ ] Vendor payment links to work order
- [ ] Zero manual journal entries required for closed month test portfolio
- [ ] Distribution blocked until sign-off
- [ ] Recategorization creates CAP-10 human.action event

## Open questions

- [ ] Plaid vs Stripe-only for MVP?
- [ ] Texas TREC trust account rules (2-day deposit)?
- [ ] Security deposit sub-ledger (M5)?

## Market parity sub-features (TBD)

See `docs/MARKET-GAP-CHECKLIST.md`.

- [ ] Security deposit lifecycle — collect, trust sub-ledger, itemize, TX 30-day return (M5) 🔴
- [x] Delinquency & late fee auto-assessment (M2) — [`DELINQUENCY-RULES-ENGINE.md`](../DELINQUENCY-RULES-ENGINE.md)
- [ ] Management fee auto-calculation
- [ ] Bank reconciliation UI
- [ ] 1099 generation (M11) — Phase 2 candidate

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Human monthly sign-off before distributions (HANDOFF locked) |
