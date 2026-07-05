# Adversarial Architecture Review — ARCHITECTURE-SPINE.md (rentalpro)

**Reviewer stance:** adversary. For each finding I construct two units one level down (feature teams, agents, or clients) that each obey **every AD to the letter** and still build incompatibly. Every pair is a hole; every hole gets a concrete fix.

**Reviewed:** `ARCHITECTURE-SPINE.md` (2026-07-05 draft)
**Context:** `SPEC.md`, `docs/DELINQUENCY-RULES-ENGINE.md`

**Verdict: NOT READY TO BIND.** The spine is strong on tenancy, ledger-writer, governance choke point, and trace immutability — but it is silent or ambiguous exactly where two teams meet: entity ownership, event contracts, approval resolution protocol, outbound comms, storage, and toggle semantics. I found **12 exploitable seams**, 3 of them critical, plus **2 internal contradictions** in the spine's own text/sources that guarantee divergence even by teams acting in good faith.

---

## F-1 — Who owns `Lease` vs `LeaseTerms`? Two compliant teams, two truths about rent — **CRITICAL**

**The pair.** Team Leasing (CAP-2 + M3 renewals, `core/leasing`) vs Team Delinquency (M2, `core/rules` + `agents/accounting`).

- Team Leasing, obeying AD-1, puts rent amount, due day, and grace on the `lease` row it creates at signing, and on renewal (M3) writes a new lease version with the new rent. Fully compliant.
- Team Delinquency, following its own binding source (`DELINQUENCY-RULES-ENGINE.md` FR-M2-05: "LeaseTerms import: late fee clause, due day, grace"; data model: `LeaseTerms { leaseId, rentDueDay, lateFeeClause, graceDays }`), builds and populates a separate `lease_terms` table at CAP-1 import time. Also fully compliant — the spine's ERD even blesses it: `LEASE ||--o{ LEASE_TERMS : carries`.

**The collision.** Renewal raises rent from $1,000 → $1,100 on `lease`; `lease_terms` still says $1,000 and an 8% clause. The daily delinquency job computes the fee off stale terms — either an illegal over-cap fee (TX-LF-003: $100 + 3× fee + attorney-fee penalty) or a silent under-assessment. Worse, the ERD's one-to-many "carries" implies *versioned* terms, but no AD defines which version is effective on a given date — Team Leasing appends a row per renewal; Team Delinquency reads "the" row. Neither team violated anything.

**Same hole, second instance:** `structureUnitCount` (drives the 10%/12% cap) is required by M2 (FR-M2-11) but lives on `Property`, owned by Team Onboarding (CAP-1). Nothing obliges CAP-1's import to populate it or make it non-null; delinquency silently picks the wrong cap.

**Fix — new AD-12: Entity ownership registry.**
> Every core entity has exactly one owning module in `packages/core`; the owner exposes the only write API and any versioned/derived reads. Concretely: `core/leasing` owns `Lease` **and** `LeaseTerms` (terms are a versioned child of the lease, written at signing/renewal/import) and exposes `getEffectiveTerms(leaseId, onDate)`. `core/rules` and `agents/*` are consumers — they never write terms and never read the raw table. The registry (entity → owner → write API) is a table in the spine, updated before any epic that adds an entity. Cross-module required fields (e.g., `property.structure_unit_count`) are declared by the consumer in the registry and enforced `NOT NULL`/validated at the owner's write path.

---

## F-2 — Vendor payout posts to the ledger twice: workflow post vs webhook categorization — **CRITICAL**

**The pair.** Team Maintenance (CAP-3/CAP-9, `agents/maintenance`) vs Team Accounting (CAP-4, `agents/accounting`).

- Team Maintenance's work-order workflow reaches its "pay vendor" `step.run`: governance-checked (AD-5), trace written (AD-6), Stripe Connect transfer via the port (AD-9), then — since AD-7 only says *`core/ledger` is the sole writer*, not *who may call it* — it calls `core/ledger.post()` to record the payout against the work order. Letter-perfect compliance with AD-4/5/6/7/9.
- Team Accounting, building CAP-4's promise ("AI categorizes 100% of transactions fed by payment/bank APIs"), consumes the `transfer.created`/Plaid webhook (AD-9: dedupe on provider event ID, translate to event), categorizes it via the LLM gateway (AD-10), and calls `core/ledger.post()`. Also letter-perfect.

**The collision.** One real-world money movement, two balanced double-entry postings. Books are internally balanced and *wrong by 2×* — the exact class of failure AD-7 exists to prevent. AD-9's webhook dedupe doesn't help: it dedupes *webhook retries*, not *a webhook vs a workflow* posting the same economic event. Symmetric variants: rent payment posted by the resident-portal payment flow **and** by the Stripe `payment_intent.succeeded` categorizer; late fee posted by the delinquency agent **and** re-categorized from the ledger feed.

**Fix — tighten AD-7 (two clauses).**
> (a) **Source-ref idempotency:** every ledger transaction carries a mandatory `source_ref` (`{provider, providerTxnId}` for external money movement, `{domain, entityId, eventType}` for internal assessments like late fees) with a DB unique constraint. `core/ledger.post()` is idempotent on it — a second post with the same ref is a no-op returning the original transaction.
> (b) **Posting catalog:** the spine (or CAP-4 epic) enumerates every money-event type with exactly one designated posting owner (e.g., `vendor.payout` → posted by the maintenance workflow at transfer initiation; provider webhooks *reconcile/settle* the existing transaction — they never create one for a movement the platform itself initiated). AI categorization applies only to *externally originated* transactions (bank feed items with no matching `source_ref`).

---

## F-3 — Approval resolution: `waitForEvent` workflow vs tRPC approval mutation both execute the side effect — **CRITICAL**

**The pair.** Team Agents (owns the paused Inngest workflows, AD-4) vs Team Web/API (owns the approval inbox UI and the `governance` tRPC router).

- Team Agents, per AD-4/AD-5: `ESCALATE` → create `ApprovalRequest` → `waitForEvent('approval.resolved')` → on approval, execute the gated step. Compliant.
- Team Web/API builds `governance.approve` as a tRPC procedure. Per the state-mutation convention ("Request path: tRPC procedure → core service → scoped DB txn") and AD-1 (decisions in core), the natural build is: mark the request approved **and call the core service to execute the approved action right there** — "approve & dispatch" in one click, snappy UX, every AD satisfied. Then emit the event (the diagram even blesses `API -.emits.-> EVT`).

**The collision.** The side effect runs twice — once in the tRPC transaction, once when the woken workflow resumes. Double vendor dispatch, double lease send, double payment-plan creation (the delinquency doc's payment-plan path — "PM approves → resident accepts in portal → schedule created in CAP-4" — has *three* actors who could each create the schedule). Adjacent races nothing in the spine forbids: two approvers approving concurrently (no single-transition rule on `ApprovalRequest`); workflow timeout expiring the request in the same instant a human approves; the event emitted before the row commit (workflow wakes, reads `pending`, re-sleeps forever).

**Fix — new AD-13: Approval resolution protocol.**
> `ApprovalRequest` is a single-transition state machine (`pending → approved | denied | expired`) enforced by a conditional `UPDATE … WHERE status='pending'` inside `core/governance.resolve()`. The tRPC approval procedure records the verdict and **nothing else** — the paused workflow is the *sole executor* of the gated side effect, for every approval type, no exceptions for UX. `governance/approval.resolved` is emitted only after the resolving transaction commits (outbox or post-commit hook). On wake, the workflow re-reads the request row (event is a wake-up signal; the DB is truth) and treats an already-`expired`/`denied` row as final. Timeout expiry uses the same conditional transition, so approve-vs-expire is decided by the database exactly once.

---

## F-4 — The spine contradicts itself on the approval event name: `approval.resolved` vs `domain/noun.verb` — **HIGH** *(internal contradiction)*

**The pair.** Any workflow team implementing AD-4 *verbatim* (`waitForEvent` on **`approval.resolved`**) vs any team implementing the naming convention *verbatim* (events are **`domain/noun.verb`**, so it emits **`governance/approval.resolved`**).

**The collision.** Inngest `waitForEvent` matches on exact event name. The workflow sleeps until its timeout and the approval silently does nothing. Both teams followed the spine to the letter; the spine disagrees with itself. This is the sharpest possible proof that prose event names don't survive two teams — and it generalizes: nothing standardizes payload shape. Team A emits `{ organizationId, leaseId, amountCents }`; Team B emits `{ orgId, lease_id, amount }`. AD-2 *requires* `organizationId` in every background payload but doesn't say **where** in the envelope; the logging convention requires `traceId` propagation but no field is defined to carry it across an event hop.

**Fix — new AD-14: Typed event catalog.**
> One package (`packages/events` or `core/events`) defines every Inngest event: name (fix AD-4's text to `governance/approval.resolved`), Zod payload schema, and a mandatory envelope `{ organizationId, traceId, occurredAt, schemaVersion }`. Inngest clients are constructed with `EventSchemas` from this catalog; `inngest.send()` and `waitForEvent` outside catalog types fail typecheck. Lint/CI rule: no string event names at call sites. An event schema change is a catalog PR reviewed by both producer and consumer owners.

---

## F-5 — Outbound comms: M7 hub team vs each agent's own Twilio port — duplicate, unlogged, legally unvetted messages — **HIGH**

**The pair.** Team Comms (CAP-7 + M7, `core/comms`) vs Team Delinquency/Leasing agents.

- Team Comms builds `core/comms` with the `Conversation` entity and enforces the M2 prod gates there: P3 attorney-approved templates only, opt-outs, quiet hours.
- Team Delinquency's reminder step (FR-M2-08) calls the `integrations/twilio` port directly. AD-9 explicitly *invites* this ("domain code depends on port interfaces") — governance-checked, trace-written, fully compliant with AD-4/5/6/9/10. Team Leasing does the same for showing/lease notifications.

**The collision.** (a) The M7 "unified inbox" — the whole point of the module — is blind to every agent-sent message, so a PM reading a delinquency thread sees the resident's replies but not what the agent said. (b) The attorney-template gate (P3, DELINQUENCY doc: "Auto-sent notices use P3-approved templates only") and TCPA opt-out/quiet-hour handling are enforced only on the hub path; the port path sends whatever it likes. (c) Resident gets a hub reminder *and* an agent reminder for the same fee. (d) Inbound SMS replies ("I'll pay Friday") land in the hub with no route back to the waiting delinquency workflow.

**Fix — new AD-15: Comms is a choke point, like governance.**
> All outbound resident/owner/vendor/lead communication goes through `core/comms.send(templateId, params, recipientRef, traceId)` — Twilio/Resend adapters are callable **only** by `core/comms` (enforce with lint boundary or package export scoping). `core/comms` owns the versioned template registry (with per-template approval status; unapproved templates refuse to send in prod), opt-outs, quiet hours, channel preference, and writes the `Conversation` message row atomically with the send. LLM-drafted copy is a *template parameter set*, not free-form body, for any legally sensitive category. Inbound webhooks translate to `comms/message.received` events that route to the owning workflow via the conversation's `contextRef` (workOrderId / leaseId / delinquencyCaseId).

---

## F-6 — CAP-6 module toggle: "checked in `agents/*` entry" vs "no module implements thresholds locally" — two compliant, opposite behaviors — **HIGH** *(internal contradiction)*

**The pair.** Team Maintenance vs Team Leasing, each implementing the human-in-the-loop toggle.

- Team Maintenance follows the Capability Map row literally ("CAP-6 … checked in `agents/*` entry"): toggle off-autonomy → the Inngest function returns at entry. Result: maintenance requests are *never triaged at all* in human mode — they rot in a queue nobody built.
- Team Leasing follows AD-5 literally ("No module implements thresholds locally" — and an org-level autonomy toggle *is* threshold logic): the workflow always runs, and `governance.evaluate()` returns `ESCALATE` for every significant action when the module is in human mode. Result: leads still get processed, humans approve each step — which is what CAP-6's success criterion actually describes ("a new lead **queues for human review**").

**The collision.** Same toggle, one team built a kill switch, the other built an escalation mode. A PM flipping "maintenance → human mode" on Team Maintenance's build experiences silent outage. The spine's two sentences point in opposite directions.

**Fix — tighten AD-5 + Capability Map row.**
> CAP-6 defines **two distinct org settings** per module: `enabled` (kill switch — workflow doesn't start; checked once at `agents/*` entry and in tRPC middleware so the UI also reflects it) and `autonomyMode: autonomous | human_approve` (checked **only inside** `core/governance.evaluate()`, which returns `ESCALATE` for all significant actions in `human_approve`). No other code reads the autonomy setting. Update the Capability Map row to name both and where each is checked.

---

## F-7 — Files: inspection photos (M4) vs maintenance videos (CAP-3) — no storage AD at all — **HIGH**

**The pair.** Team Inspections (M4) vs Team Maintenance (CAP-3), both told only "Supabase Storage" in the Stack table.

- Team Inspections: bucket `inspections`, path `{inspectionId}/{photo}.jpg`, public-read URLs for the owner report, client uploads via a small REST route (arguing AD-3's tRPC rule can't carry multi-MB photo batches).
- Team Maintenance: bucket `uploads`, path `org/{orgId}/wo/{workOrderId}/video.mp4`, private + signed URLs, direct-to-storage client upload with a Supabase storage token (arguing that's "not a Supabase *query*", so AD-3 doesn't apply).

**The collision.** (a) **Tenancy hole:** AD-2's RLS story covers Postgres only; Team Inspections' guessable, org-less paths are a cross-tenant leakage vector — the exact "security incident, not a bug" of SPEC Constraint 1, and the pen-test success criterion fails on Storage while every DB check passes. (b) AD-3 drift: each team invents its own non-tRPC upload surface, and mobile (Phase 2) inherits two different upload dances. (c) AD-6's `DecisionTrace.inputsRef` pointing at files can't be resolved uniformly by audit tooling across two ad-hoc layouts. (d) Nobody owns retention — traces must live 7 years, but the videos they reference can be deleted freely.

**Fix — new AD-16: Storage convention.**
> One layout: `org/{organizationId}/{domain}/{entityId}/{fileId}` in private buckets; Supabase Storage policies mirror RLS on the org prefix. Every stored object has a `file` row in Postgres (org-scoped, RLS'd — owning entity, content type, retention class) and **that row's ID** is what traces and entities reference. Uploads/downloads use short-lived signed URLs issued **only** by tRPC procedures — name this as the third sanctioned non-tRPC HTTP surface in AD-3. Files referenced by a `DecisionTrace` inherit trace retention (7y, delete-blocked).

---

## F-8 — AD-4's concurrency key is the org, not the aggregate: payment-vs-fee race inside one compliant team boundary — **HIGH**

**The pair.** The resident-payment tRPC path (Team Portal) vs the daily delinquency workflow (Team Delinquency) — both mutating the same lease's balance, both on their sanctioned mutation path (the *only two* paths the conventions allow, and they're concurrent by design).

**The collision.** DELINQUENCY doc edge case: "Resident pays before fee posts → cancel fee assessment." The workflow reads `balance > 0` in one step, posts the fee in a later step; the resident's payment lands between them via tRPC. Fee posted against a settled balance → resident charged illegally late; TX penalty exposure. AD-4 keys concurrency by `organizationId` only — that throttles a tenant's parallelism but serializes *nothing* per lease, and can't serialize against the tRPC path anyway. Same shape: two work-order mutations (vendor accepts quote via portal while the workflow cancels/re-dispatches).

**Fix — tighten AD-4 + state-mutation convention.**
> Check-and-act on an aggregate is never split across `step.run` boundaries: the guard and the mutation execute inside **one core service call in one scoped transaction** with a row lock or optimistic version on the aggregate (`SELECT … FOR UPDATE` on the lease/work order). `core/ledger` assessment APIs take preconditions (`assessLateFee(leaseId, expectedOpenBalanceCents, …)`) and fail typed when stale; the workflow step retries/re-evaluates. Additionally, Inngest functions that mutate a specific aggregate declare a second concurrency key on that aggregate ID, not just the org.

---

## F-9 — DecisionTrace: the M2 source doc writes it *after* the side effect, and nobody owns who writes governance-verdict traces — **MEDIUM**

**The pair.** Team Delinquency implementing its own binding source (`DELINQUENCY-RULES-ENGINE.md` §Agent enforcement flow, step 6: post fee → notify → *then* "Write DecisionTrace") vs Team Maintenance implementing AD-6 ("committed **before or atomically with** the side effect"). One of the spine's own `sources:` contradicts AD-6 — a team building from the M2 doc violates the spine while believing it complies. Second seam: for a governance verdict, does `evaluate()` write its own trace, or the caller? Team A does it inside `evaluate()`, Team B at every call site → duplicated traces in one module, missing traces in another; the "auditor traces any decision in 60 seconds" criterion dies on inconsistency.

**Fix — tighten AD-6 with an owned API + fix the source doc.**
> `core/trace` exposes the only write path (`trace.record(...)` returning a `traceId`); governance-, rules-, and LLM-gateway verdicts are traced **inside** those modules (call sites never re-trace them); side-effect helpers in core *require* a `traceId` argument so an untraced side effect doesn't typecheck. Add an erratum to `DELINQUENCY-RULES-ENGINE.md` step 6: trace commits atomically with the ledger post, not after the notify.

---

## F-10 — "Delinquent" has no single definition: maintenance-block vs fee engine disagree — **MEDIUM**

**The pair.** Team Maintenance implements FR-M2-12 (`blockMaintenanceIfDelinquent`) by reading the ledger: `openBalanceCents > 0` ⇒ delinquent. Team Delinquency defines delinquent as *past the effective grace period* (`max(lease, org, state)` grace). Both are reasonable, both compliant (each computed in `core`, per AD-1).

**The collision.** Resident is 1 day late, inside grace: Team Maintenance blocks their emergency-adjacent repair request; Team Delinquency's engine says they're not delinquent yet and would never have assessed anything. PM sees contradictory statuses in two screens; a fair-housing complaint about inconsistent treatment now has evidence. Same family as F-1 but for *derived* state.

**Fix — extend AD-12 (entity registry) to derived statuses.**
> Cross-module derived statuses (`delinquencyStatus`, `leaseStatus`, `workOrderStage`) each have one computing function in the owning core module (`core/rules.getDelinquencyStatus(leaseId, onDate)`); every consumer — agents, tRPC, UI badges — calls it. No module re-derives another module's status from raw tables.

---

## F-11 — Multi-client drift AD-3 doesn't stop: realtime subscriptions and server-side shortcuts — **MEDIUM**

**The pair.** Team Web vs Team Mobile (Phase 2). CAP-7 requires status updates "within 5 minutes." Team Web wires Supabase Realtime channels straight from the browser — arguing a *subscription* isn't a "direct Supabase client **query**" (AD-3's literal word) — and renders owner reports in RSCs that import a server-side caller. Team Mobile, on tRPC-over-HTTP only, polls. Now: web receives rows RLS lets through the raw channel (bypassing tRPC's role middleware and response shaping); mobile shows different data freshness and different field shapes; the "one API contract" is one contract plus a side channel mobile can't use.

**Fix — tighten AD-3.**
> Rewrite the rule from "no direct Supabase client queries" to "no client-originated data channel of any kind bypasses `packages/api` — queries, mutations, **subscriptions, storage, realtime**." Realtime in MVP = tRPC polling/`useQuery` refetch (or tRPC subscriptions later); if Supabase Realtime is ever adopted it is wrapped behind an API-issued, org-scoped, table-agnostic channel token and listed as a sanctioned surface. RSC/server-caller usage must go through the same root-router middleware stack (already implied — state it).

---

## F-12 — Legal day-boundary math: "2 full days" computed two ways in two core modules — **MEDIUM**

**The pair.** Team Delinquency implements TX-LF-002 ("minimum 2 full days after due date") as *calendar days in property TZ*: due the 1st ⇒ fee assessable 12:00 AM on the 4th. Team Deposits (M5) implements the TX 30-day deposit clock as *72-hour-style elapsed time from move-out timestamp*. Both satisfy AD-11 ("computed in the property's timezone in `core`") — AD-11 fixes the *timezone* but not the *day semantics* (calendar day vs 24h period, inclusive vs exclusive boundary, DST-transition days). Off-by-one on either = statutory violation with the §92.019 / §92.103 penalty attached.

**Fix — tighten AD-11.**
> Add `core/time` as the single legal-calendar utility (`addLegalDays`, `startOfLegalDay(propertyTz)`, `daysPastDue`) with the boundary semantics documented per rule *inside the StateRulePack entry* (each rule carries `daySemantics: calendar_days_exclusive | …`), unit-tested across DST transitions. Domain modules are forbidden from doing date arithmetic with raw Date/Temporal on legal deadlines.

---

## Summary table

| # | Finding | Severity | Fix |
|---|---------|----------|-----|
| F-1 | `Lease` vs `LeaseTerms` dual ownership; stale terms after renewal; `structureUnitCount` orphaned | **CRITICAL** | New **AD-12** entity ownership registry + `getEffectiveTerms()` |
| F-2 | Vendor payout / rent payment posted twice: workflow post + webhook categorization | **CRITICAL** | Tighten **AD-7**: `source_ref` idempotency + posting catalog |
| F-3 | Approval executed in tRPC mutation *and* resumed workflow; approve/expire races | **CRITICAL** | New **AD-13** approval resolution protocol (workflow is sole executor) |
| F-4 | Spine self-contradiction: `approval.resolved` vs `governance/approval.resolved`; no payload standard | **HIGH** | New **AD-14** typed event catalog + envelope; fix AD-4 text |
| F-5 | Agents send Twilio directly, bypassing M7 hub, template approval, opt-outs | **HIGH** | New **AD-15** comms choke point; adapters callable only by `core/comms` |
| F-6 | CAP-6 toggle: kill switch (agents entry) vs escalate-all (governance) — spine says both | **HIGH** | Tighten **AD-5**: split `enabled` vs `autonomyMode`, each checked in one place |
| F-7 | No storage AD: org-less paths, ad-hoc upload routes, cross-tenant leak via Storage | **HIGH** | New **AD-16** storage convention + `file` rows + signed-URL-only, sanctioned in AD-3 |
| F-8 | Org-level concurrency key; payment-vs-fee check/act race across the two sanctioned paths | **HIGH** | Tighten **AD-4**: aggregate-level guards in one txn + per-aggregate concurrency keys |
| F-9 | M2 source doc writes trace *after* side effect (violates AD-6); governance-trace ownership unassigned | **MEDIUM** | Tighten **AD-6**: `core/trace` API, verdicts traced inside owning module; erratum to M2 doc |
| F-10 | "Delinquent" defined differently by maintenance-block vs fee engine | **MEDIUM** | Extend AD-12 to derived statuses: one computing function per status |
| F-11 | Realtime subscriptions / side channels evade AD-3's "no direct queries" wording; web/mobile drift | **MEDIUM** | Tighten **AD-3**: no client data channel of any kind bypasses `packages/api` |
| F-12 | "2 full days" / 30-day clock day-semantics divergence between M2 and M5 | **MEDIUM** | Tighten **AD-11**: `core/time` legal calendar; day semantics pinned in RulePack |

## Priority order to close before the spine binds epics

1. **F-2, F-3, F-1** — money duplication and dual entity ownership poison the ledger and the legal posture; they are unfixable retroactively without re-migration.
2. **F-4, F-6** — the two *internal contradictions*: teams following the document diverge even with perfect discipline. Cheapest fixes in the list (text changes + one package).
3. **F-5, F-7, F-8** — compliance and tenancy exposure (TCPA/attorney templates, Storage leak, illegal fee race).
4. **F-9–F-12** — schedule with the owning epics but write the AD text now.

*End of adversarial review.*
