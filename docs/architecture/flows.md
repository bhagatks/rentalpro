# Key Autonomous Flows — Sequence Diagrams

**Source of truth:** [`ARCHITECTURE-SPINE.md`](../../_bmad-output/planning-artifacts/architecture/architecture-rentalpro-2026-07-05/ARCHITECTURE-SPINE.md) (AD-1 … AD-17). This doc expands the spine's invariants into concrete step-by-step flows for the five autonomous processes that define the product. Every flow follows the same skeleton: **rules → governance → trace → side effect**, per AD-5/AD-6/AD-8.

Each diagram cites the AD(s) enforced at each step so a builder can verify compliance by inspection.

---

## 1. Lead → Lease → Key (CAP-2 + CAP-12, 48-hour SLA)

Owning modules: `core/leasing`, `agents/leasing`. Workflow: `leasing.lead_to_lease` (Inngest, keyed by `organizationId` + `leadId`).

```mermaid
sequenceDiagram
    participant P as Prospect
    participant Web as apps/web (publicProcedure)
    participant Wf as agents/leasing workflow
    participant Scr as core/screening
    participant Gov as core/governance
    participant Trc as core/trace
    participant Ext as Stripe Identity / Screening API
    participant Ldg as core/ledger
    participant Seam as integrations/seam
    participant Comms as core/comms

    P->>Web: Submit inquiry (subdomain-resolved org, AD-3 publicProcedure)
    Web->>Wf: emit leasing/lead.received (AD-14 envelope)
    Wf->>Scr: pre-qualify (allowlist inputs only — AD-10, strips protected attrs)
    Scr->>Ext: request screening report (dual source per AI-MVP-DECISIONS)
    Ext-->>Scr: report
    Scr->>Trc: intent+result trace (AD-6)
    Scr-->>Wf: PASS | FAIL (with specific reason for adverse action)
    Wf->>Ext: Stripe Identity verification
    Ext-->>Wf: verified
    Wf->>Web: AI drafts lease from PM template + Texas starter (AD-10 — proposal only)
    Wf->>Gov: evaluate(sendLeaseForSignature, context) — AD-5
    alt Basic plan or draft doesn't match template exactly
        Gov-->>Wf: ESCALATE — create ApprovalRequest, waitForEvent (AD-13)
        Note over Wf: workflow sleeps — PM admin reviews/edits in portal
        Gov-->>Wf: governance/approval.resolved (workflow re-reads row, AD-13)
    else Pro plan, exact template match
        Gov-->>Wf: ALLOW
    end
    Wf->>P: Lease sent for e-sign (integrations/esign port, AD-9)
    P-->>Wf: e-sign webhook → leasing/lease.signed (AD-14)
    Wf->>Ldg: post first-month payment (source_ref idempotent, AD-7)
    Wf->>Trc: decision trace: lease signed, policy version, traceId (AD-6)
    Wf->>Seam: issue digital key (access/key.issuance_requested → access/key.issued, AD-9)
    Seam-->>P: unlock code delivered
    Wf->>Comms: notify resident + PM (core/comms only, AD-15)
```

**Governance checkpoints:** lease-send (AD-5/AD-13), first-payment confirmation implicitly gated by AD-7's idempotent post.
**Compliance ordering (AD-8):** §92.3515 criteria notice must precede the application fee charge; standalone FCRA consent must precede the credit pull — both enforced as rules-engine preconditions inside `core/screening`, not left to UI sequencing.

---

## 2. Maintenance Dispatch (CAP-3 + CAP-9)

Owning modules: `core/maintenance`, `agents/maintenance`. Workflow: `maintenance.work_order_lifecycle` (Inngest, keyed by `organizationId` + `workOrderId` per AD-4's per-aggregate concurrency rule).

```mermaid
sequenceDiagram
    participant R as Resident
    participant Api as packages/api (tRPC)
    participant Wf as agents/maintenance workflow
    participant Rules as core/maintenance (triage)
    participant Gov as core/governance
    participant Vnd as Vendor (via integrations/*)
    participant Ldg as core/ledger
    participant Trc as core/trace
    participant Comms as core/comms

    R->>Api: maintenance.create (video/photo upload → AD-16 signed URL, file row)
    Api->>Wf: emit maintenance/work_order.created (AD-14)
    Wf->>Rules: AI diagnosis (LLM gateway, AD-10) — classify severity
    alt Emergency (fixed list: gas leak, flooding, no heat <40°F, etc.)
        Rules-->>Wf: emergency — bypass spend approval INSIDE governance.evaluate() (AD-5)
        Wf->>Vnd: auto-dispatch immediately
    else Routine/cosmetic
        Wf->>Vnd: request quote via port (AD-9)
        Vnd-->>Wf: quote
        Wf->>Gov: evaluate(dispatchWithSpend, {amount}) — AD-5
        alt Under $500 (Pro, configurable) or Basic always
            Gov-->>Wf: ALLOW (Pro under threshold) or ESCALATE (Basic)
        end
        opt ESCALATE
            Gov->>Wf: create ApprovalRequest, waitForEvent (AD-13)
            Note over Wf: PM approves/denies in portal — tRPC records verdict ONLY
            Gov-->>Wf: governance/approval.resolved
        end
        Wf->>Vnd: confirm dispatch (workflow is sole executor — AD-13)
    end
    Wf->>Trc: decision trace: diagnosis, governance verdict, policy version (AD-6)
    Vnd-->>Wf: maintenance/work_order.completed + photo verification
    Wf->>Ldg: post vendor payout — source_ref = {stripe, transferId} (AD-7, single posting owner)
    Note over Ldg: Stripe webhook RECONCILES this transaction by source_ref — never creates a second posting
    Wf->>Comms: notify resident (core/comms only, thread-persisted, AD-15)
    Comms-->>R: "Repair complete — confirm resolution?"
```

**Governance checkpoint:** spend-threshold dispatch (AD-5/AD-13). **Idempotency:** vendor payout has exactly one posting owner — the workflow at transfer initiation (AD-7); the Stripe webhook reconciles, it does not re-post. **Toggle semantics (CAP-6/AD-5):** if maintenance `autonomyMode = human_approve`, `governance.evaluate()` returns ESCALATE for every non-emergency dispatch — the workflow still runs and triages, it never silently drops the request.

---

## 3. Delinquency → Late Fee (M2, three-layer rules engine)

Owning modules: `core/rules`, `core/ledger`. Workflow: `accounting.delinquency_daily` (Inngest, daily cron, keyed by `organizationId` + `leaseId` per AD-4).

```mermaid
sequenceDiagram
    participant Cron as Daily trigger
    participant Wf as agents/accounting (delinquency)
    participant Time as core/time (AD-11 legal calendar)
    participant Rules as core/rules (3-layer engine, AD-8)
    participant Ldg as core/ledger
    participant Trc as core/trace
    participant Comms as core/comms
    participant PM as PM admin

    Cron->>Wf: for each active lease, balance > 0
    Wf->>Rules: getEffectiveTerms(leaseId, today) — AD-12, reads lease_terms via core/leasing's API
    Rules->>Time: daysPastDue(dueDate, propertyTz) — calendar-day semantics pinned per rule (AD-11)
    alt Within grace (max of lease/org/state grace)
        Wf->>Comms: send friendly reminder (core/comms, AD-15)
    else Past grace, no fee posted yet
        Rules->>Rules: LeaseTerms → OrgPolicy → StateRulePack-TX evaluation (AD-8)
        alt Fee exceeds cap (TX-LF-003/004) or missing lease clause (TX-LF-001)
            Rules-->>Wf: BLOCK + reason code
            Wf->>Trc: intent+result trace: BLOCKED, rulePackVersion (AD-6)
            Wf->>Comms: alert PM admin — no ledger post
        else Fee legal
            Wf->>Ldg: post late fee — source_ref = {delinquency, leaseId, cycleId} (AD-7 idempotent)
            Note over Ldg,Wf: check-and-act in ONE transaction with row lock on lease balance (AD-4) — closes the resident-pays-before-fee-posts race
            Wf->>Trc: intent+result trace: FEE_POSTED, rulePackVersion, policyVersion (AD-6)
            Wf->>Comms: notify resident — attorney-approved template only (AD-15, M2 track P3 gate)
        end
    end
    opt Payment plan requested
        Wf->>Wf: propose plan (max months per OrgPolicy)
        Wf->>PM: ApprovalRequest (AD-13) — workflow creates schedule only after PM approves AND resident accepts
    end
```

**Compliance gate:** `StateRulePack-TX` requires an attorney sign-off record before this workflow runs in production (AD-8). **Race closed:** the balance check and fee post execute inside one scoped transaction with a lease-level lock — a resident payment landing via the tRPC path between "check" and "post" is impossible by construction (AD-4). **Idempotency:** a crashed/re-run daily job cannot double-post because `source_ref` is unique per lease per cycle (AD-7).

---

## 4. Move-In / Move-Out Inspection (M4)

Owning modules: `core/inspections`. Workflow: `inspections.compare` (Inngest, triggered on move-out inspection submission).

```mermaid
sequenceDiagram
    participant PM as PM admin / Resident
    participant Api as packages/api (tRPC, signed upload URLs)
    participant Store as Supabase Storage (AD-16)
    participant Wf as agents/inspections
    participant LLM as LLM gateway (AD-10)
    participant Gov as core/governance
    participant Ldg as core/ledger (M5 deposit sub-ledger)
    participant Trc as core/trace

    PM->>Api: request signed upload URL (org-scoped path, AD-16)
    Api-->>PM: signed URL
    PM->>Store: upload move-in photos → file row created (org/{orgId}/inspections/{id}/{fileId})
    Note over Store: move-out repeats the same flow months later
    PM->>Wf: submit move-out inspection
    Wf->>LLM: compare move-in vs move-out photos (AD-10 — proposal only)
    LLM-->>Wf: suggested deposit deductions + line-item reasoning
    Wf->>Gov: evaluate(deductFromDeposit, {amount}) — AD-5
    Gov-->>Wf: ESCALATE — deposit deductions always require PM approval (AD-13)
    Note over Wf: PM reviews AI suggestion in portal — approve/adjust/deny
    Gov-->>Wf: governance/approval.resolved
    Wf->>Ldg: post approved deduction against trust sub-ledger (AD-7 — trust class, TX 30-day clock via core/time AD-11)
    Wf->>Trc: decision trace: comparison inputs (file IDs), LLM version, PM override if any (AD-6)
```

**Storage tenancy:** every photo is a `file` row under the org-prefixed path with RLS-mirrored bucket policy — cross-tenant leakage via a guessable URL is structurally prevented (AD-16). **Trust accounting:** deductions move funds from the trust class to operating only through this governance-gated path, never a direct ledger write (AD-7/M5). **Retention:** files referenced by the resulting `DecisionTrace` inherit 7-year retention even if the lease is later deleted from active view (AD-6/AD-16).

---

## 5. Monthly Accounting Close (CAP-4, "one sign-off" success criterion)

Owning modules: `core/ledger`, `core/rules`. Workflow: `accounting.monthly_close` (Inngest, monthly cron per org).

```mermaid
sequenceDiagram
    participant Cron as Monthly trigger
    participant Wf as agents/accounting (close)
    participant Feed as Bank feed webhook (Plaid/Stripe, AD-9)
    participant LLM as LLM gateway (AD-10)
    participant Ldg as core/ledger
    participant Trc as core/trace
    participant Acct as Accountant (PM org)
    participant Gov as core/governance

    Feed->>Wf: bank_feed/transaction.synced events (throughout the month)
    Wf->>Ldg: match by source_ref — reconcile if platform-initiated, else new posting
    Wf->>LLM: categorize unmatched externally-originated transactions (AD-10)
    LLM-->>Ldg: categorized entries (100% auto-categorized, per CAP-4 success criterion)
    Wf->>Trc: trace each categorization decision (AD-6)
    Cron->>Wf: period close trigger
    Wf->>Ldg: verify balanced double-entry across all accounts (AD-7)
    Wf->>Acct: present closed-period report — flagged "Pending Review"
    Acct->>Gov: sign off (single approval — AD-13, one transition pending→approved)
    Gov-->>Wf: governance/approval.resolved
    Wf->>Ldg: calculate owner distributions (read model, AD-7)
    Wf->>Trc: trace: month closed, signed by, distribution amounts (AD-6)
```

**Zero manual journal entries:** every entry traces to either a platform-initiated posting (workflow flows 1–4 above) or an LLM-categorized bank-feed item — there is no code path for a human to hand-enter a journal line (AD-7). **One sign-off:** the accountant's single approval transitions the entire period at once via the same `ApprovalRequest` machinery as every other approval in the system (AD-13) — no per-transaction approval UI exists.

---

## Cross-flow invariant recap

Every flow above shares the same four-beat rhythm — this is not incidental, it is AD-1/AD-5/AD-6/AD-13 forcing one shape onto all autonomous work:

1. **Evaluate** — `core/rules` and/or `core/governance` decide, never the workflow or the UI.
2. **Trace** — `core/trace` records intent before (or atomically with) the side effect.
3. **Act** — the workflow is the *only* executor of a gated side effect, whether resuming from an approval or proceeding directly.
4. **Notify** — `core/comms` is the only path to a human, keeping the M7 unified inbox complete by construction.

A builder implementing a *new* autonomous flow (Phase 2) should be able to redraw this same four-beat diagram before writing code — if a step doesn't fit one of the four beats, it's a sign the flow is routing around a choke point the spine intentionally created.
