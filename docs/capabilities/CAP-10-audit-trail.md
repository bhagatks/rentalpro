# CAP-10: Audit Trail

**Status:** draft  
**SPEC reference:** CAP-10  
**MVP phase:** 0  
**Depends on:** CAP-11

## Intent & success (from SPEC)

- **Intent:** Every autonomous agent action produces an immutable audit record sufficient for tax, legal, and fair-housing review.
- **Success:** An auditor can trace any autonomous leasing or maintenance decision to inputs, policy applied, actions taken, and human overrides within 60 seconds.

## User stories

| Actor | Story |
|-------|-------|
| PM admin | I can view a full trace of any agent decision in my org by applicant ID, work order ID, or transaction ID. |
| PM admin | I see who overrode an agent and why. |
| Auditor | I export org audit records for a date range as JSON or CSV. |
| Platform ops | I see cross-tenant security events (IDOR, RLS violations). |
| AI agent | I write a decision trace before any side-effecting action. |
| Accountant | I trace any ledger categorization to source transaction and agent reasoning. |

## Happy path

1. Agent receives trigger → creates `DecisionTrace` with `traceId`.
2. Agent logs `trace.start` (trigger, inputs, orgId).
3. Agent evaluates governance (CAP-5) → logs `trace.policy`.
4. Agent logs `trace.reasoning` (model version, summary, promptHash — not full prompt in MVP).
5. Agent executes or escalates → logs `trace.action` or `trace.escalation`.
6. Human approves if escalated → logs `trace.override`.
7. Agent logs `trace.complete`.
8. PM admin searches by entity ID → full chain rendered in one view (<60s).

## Escalation path

| Trigger | Logged as |
|---------|-----------|
| Cross-tenant access attempt | `security.access_denied` + alert platform ops |
| RLS violation | `security.rls_violation` |
| Applicant disputes screening | Export trace bundle for fair-housing review |
| Legal hold | Flag traces; block purge |

## Integrations

| Service | Use |
|---------|-----|
| Supabase PostgreSQL | Append-only `AuditEvent` table; INSERT only for app role |
| CAP-5 | Policy version referenced in traces |
| CAP-11 | All events scoped by `organizationId` |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `DecisionTrace` | id, organizationId, traceId, agentType, triggerRef, status, policyVersion, startedAt, completedAt |
| `AuditEvent` | id, organizationId, traceId, sequence, eventType, actorType, actorId, entityType, entityId, payload JSON, createdAt |

**Immutability:** No UPDATE/DELETE on `AuditEvent` via application role. **Retention (TBD):** recommend 7 years.

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/orgs/current/audit/traces/:traceId` | Full decision trace |
| GET | `/api/orgs/current/audit/events` | Filtered event list |
| GET | `/api/orgs/current/audit/export` | Bulk JSON/CSV export |
| GET | `/api/platform/audit/security` | Cross-org security events |

## Acceptance tests

- [ ] Leasing decision trace retrievable by applicant ID in under 60 seconds
- [ ] Maintenance decision trace retrievable by work order ID in under 60 seconds
- [ ] UPDATE/DELETE on AuditEvent fails for app role
- [ ] Agent side-effect without trace.start is rejected
- [ ] Org A cannot read Org B audit events
- [ ] IDOR attempt creates security.access_denied event
- [ ] Screening denial trace includes criteria version, rationale, adverse action flag

## Open questions

- [ ] Retention: 7 years vs 3 years?
- [ ] PDF formal audit report — Phase 2?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Draft micro-spec; reasoning = summary + modelVersion + promptHash (not full prompt MVP) |
