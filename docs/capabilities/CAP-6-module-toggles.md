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

## Architecture

**Owning modules.** CAP-6 has no dedicated core module — it is data, not logic: two settings per module (`enabled`, `autonomyMode`) on `OrganizationSettings`, owned and validated by `core/governance` alongside `OrganizationGovernance`/`PropertyGovernance` (AD-12: one owner per entity). Exposed via the `org` tRPC router (`GET/PATCH /api/orgs/current/settings/modules`) with role-checked middleware (`pm-admin` only). No Inngest workflow — CAP-6 is read-only configuration consumed by other workflows, never itself autonomous.

**Governing decisions**

| AD | What it constrains for CAP-6 |
|----|-------------------------------|
| AD-5 | Defines the exact two-setting split this capability implements: `enabled` (kill switch) vs `autonomyMode: autonomous \| human_approve`. This is the capability's entire contract. |
| AD-4 | "Toggle changed mid-job uses toggle at start time" (CAP-6 escalation-path row) — concurrency/consistency for in-flight Inngest workflows reading settings once at start. |
| AD-2 | `OrganizationSettings` is tenant-scoped and RLS-enforced; org B's toggles can never leak into org A's `evaluate()` call. |
| AD-12 | `core/governance` is the single owner/writer of the toggle fields; no other module reads or writes them directly. |
| AD-3 | The settings read/write path goes only through the `org` tRPC router — no direct DB reads from `apps/web` or `agents/*`. |

**The enabled/autonomyMode decision tree (the crux of this capability).** This is the fix for a real bug the review process found: a naive single kill-switch reading would silently drop a maintenance request instead of escalating it. The two settings are checked at **different layers**, by **different code**, for **different reasons**:

```mermaid
flowchart TD
    A[Agent or tRPC procedure attempts a module action] --> B{enabled?}
    B -- "false (kill switch)\nchecked at agents/* entry\nAND tRPC middleware" --> C[Reject / no-op at the door\nmodule fully off for this org\nNOT a governance decision]
    B -- true --> D[Workflow proceeds to the gated side effect]
    D --> E["core/governance.evaluate(action, context)\n(the ONLY place autonomyMode is read)"]
    E --> F{autonomyMode?}
    F -- human_approve --> G[evaluate() returns ESCALATE\nfor ALL significant actions\nregardless of spend/threshold]
    F -- autonomous --> H{spend limit / emergency list\n/ other governance checks}
    H -- within limits --> I[ALLOW: agent executes autonomously]
    H -- exceeds limits --> G
    G --> J[ApprovalRequest created; workflow pauses\nper AD-13 — human resolves, workflow resumes]

    style C fill:#f8d7da
    style G fill:#fff3cd
    style I fill:#d4edda
```

The bug this prevents: if `enabled=false` were misread as "route to human approval" (conflating it with `autonomyMode`), a disabled module would queue actions indefinitely instead of correctly rejecting/no-op'ing them at entry — e.g. a maintenance request could vanish into a queue nobody is checking rather than bouncing back to the resident or PM immediately. `enabled` must short-circuit before governance is ever invoked; `autonomyMode` must never be checked anywhere except inside `evaluate()`.

**Cross-CAP dependencies.** CAP-6 settings are read (never written) by:
- **CAP-5**'s `core/governance.evaluate()` — the only reader of `autonomyMode`
- **`agents/*` entry points** (CAP-2 leasing, CAP-3 maintenance, CAP-4 accounting, CAP-9 vendor workflows) and the corresponding **tRPC middleware** — the only readers of `enabled`
- **CAP-11** provides the org-scoped settings storage/RLS substrate that makes per-tenant isolation of these toggles possible

No other CAP writes to `OrganizationSettings.moduleToggles` — writes happen only via the CAP-6 `org` router, gated to `pm-admin`.

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Draft micro-spec |
