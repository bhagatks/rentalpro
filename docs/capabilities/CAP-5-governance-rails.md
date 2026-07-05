# CAP-5: Governance Rails

**Status:** draft  
**SPEC reference:** CAP-5  
**MVP phase:** 0  
**Depends on:** CAP-11

## Intent & success (from SPEC)

- **Intent:** PM admin configures tiered governance rails—approval thresholds, escalation triggers, spend limits—per organization and optionally per property.
- **Success:** Action exceeding threshold (e.g., $500 repair) is blocked from autonomous execution and routed to human approver with full context; sub-threshold actions proceed without queue.

## User stories

| Actor | Story |
|-------|-------|
| PM admin | I set maintenance auto-approve limit ($500 default on Pro). |
| PM admin | I override governance per property (e.g., stricter on Class A building). |
| PM admin | I receive approval queue items with full agent context. |
| AI agent | I check governance before every side-effecting action. |
| Resident | I am not blocked on emergencies while approval queue runs for routine items. |

## Happy path

1. PM admin sets org defaults: `maintenanceAutoApproveLimit: 500`, `plan: basic|pro`.
2. Optional per-property overrides stored in `PropertyGovernance`.
3. Agent prepares action with estimated cost.
4. Governance engine evaluates: plan + module toggle (CAP-6) + spend limit + emergency list.
5. **Allow** → agent proceeds; **Escalate** → `ApprovalRequest` created; agent pauses.
6. PM admin approves/rejects in dashboard → agent resumes or cancels.
7. Decision logged in CAP-10.

## Escalation path

| Trigger | Approver | Notes |
|---------|----------|-------|
| Maintenance cost > limit | PM admin | Full WO context |
| Leasing draft changed from template (Pro) | PM admin | Basic always reviews |
| Screening REVIEW result | PM admin | When criteria case-by-case (TBD) |
| Emergency maintenance | None (auto-dispatch) | Locked — bypasses spend approval |
| Accounting distribution | Accountant | Monthly sign-off gate (CAP-4) |

## Integrations

| Service | Use |
|---------|-----|
| CAP-6 | Module toggles gate autonomy per domain |
| CAP-10 | Every governance decision logged |
| Inngest (TBD) | `waitForEvent` on approval |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `OrganizationGovernance` | organizationId, maintenanceAutoApproveLimit, emergencyAutoPayLimit (TBD), leasingAutoSendEnabled |
| `PropertyGovernance` | propertyId, organizationId, overrides JSON |
| `ApprovalRequest` | id, organizationId, traceId, type, entityRef, context JSON, status, approverId, decidedAt |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET/PATCH | `/api/orgs/current/governance` | Org defaults |
| GET/PATCH | `/api/properties/:id/governance` | Property overrides |
| GET | `/api/orgs/current/approvals` | Pending queue |
| POST | `/api/orgs/current/approvals/:id/decide` | Approve/reject |

## Acceptance tests

- [ ] $501 repair on Pro with $500 limit creates approval request; agent does not dispatch
- [ ] $400 repair auto-executes on Pro
- [ ] Basic plan: all non-emergency maintenance requires approval
- [ ] Emergency gas leak dispatches without approval on both plans
- [ ] Property override stricter than org default is enforced

## Open questions

- [ ] Emergency auto-pay cap on Pro (suggested $1,000)?
- [ ] Leasing spend thresholds (placement fees)?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Pro maintenance auto-approve default $500 (from AI-MVP-DECISIONS) |
| 2026-07-05 | Emergency list bypasses spend approval |
