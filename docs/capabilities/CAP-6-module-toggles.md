# CAP-6: Per-Module Autonomy Toggles

**Status:** draft  
**SPEC reference:** CAP-6  
**MVP phase:** 5  
**Depends on:** CAP-5, CAP-11

## Intent & success (from SPEC)

- **Intent:** PM admin toggles AI-driven vs human-in-the-loop independently for leasing, maintenance, accounting.
- **Success:** PM sets maintenance autonomous + leasing human-approve; lead queues for human while maintenance under threshold auto-executes.

## User stories

| Actor | Story |
|-------|-------|
| PM admin | I set leasing to human-approve and maintenance to autonomous. |
| PM admin | I change toggles without affecting other orgs. |
| AI agent | I check module toggle before acting; route to human queue if off. |

## Happy path

1. PM admin opens Settings → Module Autonomy.
2. Sets per module: `leasing`, `maintenance`, `accounting` → `autonomous` | `human_approve`.
3. Stored in `OrganizationSettings.moduleToggles`.
4. Agent reads toggle at job start:
   - `human_approve` → agent prepares action + approval queue (CAP-5)
   - `autonomous` → agent proceeds subject to governance rails
5. Plan floor (Basic) may force `human_approve` on financial actions regardless of toggle (TBD).

## Escalation path

| Trigger | Action |
|---------|--------|
| Module set to human_approve | All agent side-effects require approval |
| Toggle changed mid-job | In-flight jobs use toggle at start time |

## Integrations

| Service | Use |
|---------|-----|
| CAP-5 | Approval queue when human_approve |
| CAP-11 | Org-scoped settings |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `OrganizationSettings.moduleToggles` | `{ leasing, maintenance, accounting }` each autonomous \| human_approve |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET/PATCH | `/api/orgs/current/settings/modules` | Read/update toggles |

## Acceptance tests

- [ ] Maintenance autonomous + leasing human: WO auto-dispatches; lead waits in queue
- [ ] Toggle change applies to new jobs only
- [ ] Org B toggles do not affect Org A

## Open questions

- [ ] Basic plan: force human_approve on all modules regardless of toggle?
- [ ] Per-property module overrides?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Draft micro-spec |
