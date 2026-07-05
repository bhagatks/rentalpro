# Observability — logging is mandatory

Apply when application code exists. Every new feature, agent workflow step, gate, or server action MUST include structured logs. A PR is not complete without them. Trivial copy-only or pure CSS tweaks are exempt.

## What to log (minimum)

For each logical step in a transaction:

1. **`start`** — entry with `traceId` + key inputs (counts, flags, surface, tenantId)
2. **`done`** — exit with outcome metrics
3. **`fail` / `block` / `skip`** — always include `errorCode` + `errorMessage` when applicable

Never swallow failures silently. If a branch returns early or falls back, log why.

## Required fields

Always include where applicable:

- `traceId` — correlate logs across services
- `organizationId` — tenant scope (never log PII in the same payload)
- `designStep` or domain step id — maps to design doc / runbook
- `track` — e.g. `leasing`, `maintenance`, `accounting`, `gate`, `agent`
- `phase` — `start` | `done` | `skip` | `fail` | `block`
- `flags` — feature flags, autonomy mode, governance inputs
- `params` — counts, durations, safe previews (no secrets, no tenant PII)

## Severity levels

| Level | Use when |
|---|---|
| `light` | Verbose internals — flag values, cache hits, gate passed |
| `low` | Step completed with key params |
| `high` | Transaction boundaries, gate blocks, failures, governance escalations |

## Autonomous agent actions

When AI agents take actions (lease approval, maintenance dispatch, ledger categorization):

- Log the **decision** (proceed / escalate / block) with governance context
- Log **threshold checks** when spend limits apply (CAP-5)
- Log **module toggle state** when autonomy mode affects routing (CAP-6)
- Escalations to human approvers must log full context reference (not raw PII)

## Applies to

- Server actions and API route handlers
- AI agent workflow steps
- Governance gates and conditional branches
- Background jobs and scheduled tasks
- Integration webhooks (Seam, Stripe, Plaid)

## Completion gate

Before marking work done:

- [ ] Every new branch / gate / external call has at least one log line
- [ ] Failures and fallbacks are `high` and include `errorCode`
- [ ] No silent `catch` blocks
- [ ] Tenant scope (`organizationId`) included on all tenant-scoped operations
