# Rubric Review — ARCHITECTURE-SPINE.md (rentalpro)

**Reviewed:** 2026-07-05
**Artifact:** `_bmad-output/planning-artifacts/architecture/architecture-rentalpro-2026-07-05/ARCHITECTURE-SPINE.md`
**Driving spec:** `_bmad-output/specs/spec-rentalpro/SPEC.md`
**Supporting docs consulted:** `docs/DELINQUENCY-RULES-ENGINE.md`, `docs/capabilities/CAP-11-multi-tenant-saas.md`, `docs/AI-MVP-DECISIONS.md`, `docs/UNIFIED-COMMS-HUB-MVP-REQ.md` (M7), `docs/MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md` (M4)

**Verdict: APPROVE WITH REVISIONS.** This is a genuinely strong spine — the 11 ADs are mostly single-choke-point rules with real enforcement teeth (DB grants, RLS, one-writer ledger, one governance evaluate(), one LLM gateway), the Deferred table is disciplined (ports defined before vendors deferred), and spec traceability is nearly complete. The failures are concentrated in two places: (a) file/object storage and outbound comms are un-governed divergence points that the spec's own locked requirements (M4, M7, Constraint 1) make dangerous, and (b) the operational envelope is half-silent — monitoring/alerting and data lifecycle/backup are whole dimensions this altitude owns that the spine never decides, defers, or flags.

---

## Checklist Item 1 — Fixes the real divergence points; misses none

### What it gets right

The spine correctly identifies and pins the divergences that would actually bite when 12 CAPs + 6 market-gap features are built as independent epics:

- **AD-1 + AD-3** kill the classic web-first trap (logic in Next.js server components / server actions that Expo can't reach). Given "multi-client from day 1," these two are the load-bearing decisions and they are stated crisply.
- **AD-2** pins tenancy at the DB layer, not convention — exactly right for Constraint 1 ("cross-org access is a security incident").
- **AD-5** (single `governance.evaluate()`, emergency bypass inside the function, never at call sites) directly prevents the most likely epic-level divergence: each agent hard-coding its own threshold logic.
- **AD-7** (one ledger writer) prevents CAP-4 / M2 / M5 / CAP-9 from posting entries independently — four separate epics touch money, so this is a real, well-chosen choke point.
- **AD-8** centralizes the 3-layer rules engine that four features (M2, M5, CAP-2 notices, M3) would otherwise each reimplement.
- **AD-11** pins money/time/identity representations — the silent-divergence killers.

### Findings — missed divergence points

**F1 (HIGH) — File/object storage is a divergence point with no AD and no convention.**
Supabase Storage is in the stack and in the deployment diagram, and at least four independently-built units write files: M4 inspection photos ("baseline stored on lease/unit (M10 vault)" per the M4 req doc), CAP-2 lease documents, CAP-3 diagnosis videos, M7 message attachments. The spine says nothing about: bucket strategy (per-org bucket vs path prefix), tenant-scoped path convention, storage-level access control (Supabase Storage RLS on `storage.objects`), or signed vs public URLs. AD-2's rule is explicitly about *rows* ("Every DB access goes through the org-scoped Drizzle client… tenant tables carry FORCE RLS") — object storage is outside it. Two epics can and will diverge incompatibly (one ships public bucket URLs, another per-org signed URLs), and a guessable public storage URL is precisely the cross-tenant leak Constraint 1 calls a security incident and the CAP-11 pen-test success criterion would fail on. This needs either an AD-2 extension ("tenant files live under `org/{organizationId}/…` in private buckets; access only via short-lived signed URLs issued by `packages/api`") or a new AD.

**F2 (MEDIUM-HIGH) — No choke point for outbound communications.**
M7 (locked) requires *every* agent message — Leasing, Maintenance, Delinquency, Comms — to land as a `Message` on a `ConversationThread`, visible and takeover-able in the PM unified inbox, with Basic-plan draft-approval and (per M2 track P3) only attorney-approved templates auto-sending in prod. The spine has `core/comms`, Twilio/Resend adapters, and a `CONVERSATION` entity in the ERD, but no rule that outbound messages must flow through `core/comms` and persist to the thread. As written, the M2 delinquency epic can lawfully (per the spine) call the Twilio adapter directly from an Inngest step — messages then never appear in the unified inbox, breaking M7's core promise ("agents send messages that PM staff cannot see or override" is the exact failure mode M7's own doc names). AD-5 covers the *approval* of resident-facing sends, but not thread persistence, channel fan-out, or the approved-template gate. Recommend a rule: "All outbound resident/owner/vendor/lead messages go through `core/comms` (which persists to the conversation thread and resolves channel + template version); adapters are never called directly by feature code."

**F3 (MEDIUM) — AD-3's exception list is contradicted by the spine's own capability map.**
AD-3: "The only non-tRPC HTTP surfaces are provider webhooks and the Inngest serve route." The M1 row of the Capability Map: "`core/listings`, **MITS feed route**, `agents/listings`" — a syndication feed is a third non-tRPC HTTP surface, unlisted. Related ambiguity: the root router applies "auth + org" middleware, but CAP-2/M1 require unauthenticated surfaces (prospect inquiry, application submission, criteria-notice acknowledgment) resolved to an org by subdomain only. Without a stated public-procedure pattern, each epic will invent its own way to punch through the auth middleware — a divergence AD-3 exists to prevent. Fix: enumerate the exceptions exactly (webhooks, Inngest serve, MITS feed) and define a `publicProcedure` pattern (org from subdomain, no user session, rate-limited).

**F4 (LOW) — Scheduled-job idempotency.** AD-9 makes webhooks idempotent; AD-4 makes steps durable; but nothing states the idempotency rule for *scheduled* runs (e.g., daily delinquency job re-running after partial failure must not double-post fees). AD-7's balanced-entry rule doesn't prevent a duplicate balanced post. The delinquency doc's flow has "no late fee posted" checks, but that is epic-level; a one-line convention (natural idempotency keys on ledger posts / step-level dedupe) would close it.

---

## Checklist Item 2 — Every AD's Rule is enforceable and prevents its divergence

Per-AD assessment:

| AD | Enforceable? | Prevents stated divergence? | Notes |
|----|--------------|------------------------------|-------|
| AD-1 | Mostly | Yes | The dependency rule ("core imports nothing from…") is lint-enforceable; say so. The decide-vs-orchestrate line is judgment, acceptable at this altitude. |
| AD-2 | Yes (DB-enforced) | Yes — for rows | See F1 (storage) and F6 below. |
| AD-3 | Yes | Yes | See F3 — exception list incomplete. |
| AD-4 | Yes | Yes | `waitForEvent` for approvals, per-org concurrency keys — concrete and checkable. |
| AD-5 | Yes | Yes | Emergency bypass *inside* `evaluate()` is the right call — prevents call-site drift. What counts as "financial, legal, or resident-facing" is judgment; a short illustrative list would help reviewers. |
| AD-6 | Yes (DB grants) | Yes, with one wrinkle — F5 | |
| AD-7 | Yes | Yes | Reversing-entries-never-mutations + trust class separation is exactly M5's requirement. |
| AD-8 | Yes | Yes | Write-time policy validation + version pinning + attorney sign-off gate covers FR-M2-02/03, NFR-M2-01/02, and D5/P1 from the delinquency doc. Strong. |
| AD-9 | Yes | Yes | Signature → dedupe → typed event → 200 is a complete, checkable recipe. |
| AD-10 | Yes | Yes | "Allowlist, not the entity object" is the right *by-construction* enforcement for Constraint 5's input side. |
| AD-11 | Yes | Yes | Property-timezone day-boundary math in `core` covers the TX 30-day deposit clock and grace-day traps precisely. |

**F5 (LOW) — AD-6 has an internal tension for external side effects.** The trace row (which includes `outcome`) must be "committed before or atomically with the side effect" — but for external effects (Stripe payout, Twilio send, Seam key issuance) the outcome does not exist until the provider responds, and the external call cannot be in the same DB transaction. The rule should distinguish an intent record (pre-effect, atomic) from an outcome update — except AD-6 also bans UPDATE on trace tables. Suggest: trace tables are append-only *event* logs (intent event + result event sharing `traceId`), which preserves immutability and resolves the ordering.

**F6 (LOW) — AD-2 omits the RLS-bypass footgun.** CAP-11 (locked) specifies "non-superuser app role; FORCE ROW LEVEL SECURITY." The spine carries FORCE RLS but never states that the app connects as a non-owner/non-superuser role and that the Supabase service-role key (which bypasses RLS) is prohibited in application code paths. One sentence, but it is the single most common way RLS architectures silently fail.

---

## Checklist Item 3 — Nothing under Deferred could let two units diverge incompatibly

Item-by-item:

| Deferred item | Safe? | Reasoning |
|---|---|---|
| E-sign provider | ✅ | Port defined (esign/ in integrations); AD-9 isolates. |
| Screening vendor + native thesis | ✅ | Port defined; AD-10's allowlist governs inputs regardless of vendor. |
| Plaid vs Stripe-only feeds | ⚠️ see F7 | Port isolates the code; the tension is with the spec, not between units. |
| Public REST API (M9) | ✅ | Explicitly a facade over `core`; tRPC stays internal. |
| Custom domains / white-label | ✅ | Constraint 9 alignment. |
| Mobile build-out | ✅ | AD-1 + AD-3 make this genuinely safe to defer — the whole point of the spine. |
| AWS/GCP migration | ✅ | Concrete revisit trigger (NFR-M2-04, >~10k leases/org). Postgres/Drizzle/Inngest are portable; Clerk/Storage lock-in is a cost decision, not a divergence risk. |
| Per-CAP schema detail | ✅ mostly | Safe *because* AD-2 (every tenant table: organizationId + RLS) + naming conventions + the ERD naming shared entities (CONVERSATION, WORK_ORDER, VENDOR) constrain what epics can invent. Shared-entity ownership (who owns CONVERSATION's schema when four epics write to it?) is the residual risk — F2's comms choke point would resolve it. |
| Search/reporting infra | ✅ | Read-model note in CAP-8 map row keeps OLTP/report separation possible later. |
| Voice channels | ✅ | Correctly a non-goal (Constraint 7), though listing a non-goal under "Deferred" slightly muddies the table's semantics. |

**F7 (LOW) — Plaid deferral is in tension with SPEC Constraint 8.** Constraint 8 lists Plaid as a *required* integration partner ("MVP workflows assume these APIs"; the spec's assumptions say "architecture phase finalizes"). The spine defers the decision past the architecture phase to the CAP-4 epic. The constraint does allow "architecture phase may add alternates," so this is defensible — but the Deferred row should explicitly acknowledge it is relaxing a spec constraint, or the spec should be amended. Silent constraint-softening is how contract drift starts.

**Conclusion for item 3:** No deferred item allows *incompatible* divergence between units — ports and the core-ownership rules see to that. Pass, with F7 noted.

---

## Checklist Item 4 — Covers the driving spec's capabilities

### 12 CAPs

All 12 CAPs appear in `binds:` and in the Capability → Architecture Map with concrete homes and governing ADs. Spot-checks:

- CAP-2's 48-hour lead→lease→key chain: AD-4 (durable workflow) + AD-5 + AD-10 + CAP-12 map row (Seam step) — covered.
- CAP-4's "zero manual journal entries / one monthly sign-off": AD-7 + AD-4 (monthly close as workflow) + AD-5 (sign-off as approval) — covered.
- CAP-10's 60-second traceability: AD-6 (traceId, versions pinned, append-only via grants, 7-year retention — which also closes the spec's open retention question) — covered.
- CAP-11: AD-2 mirrors the locked micro-spec's defense-in-depth stack faithfully (subdomain → session claim → scoped client → SET LOCAL → FORCE RLS), minus the non-superuser role note (F6).

### 9 Constraints

| # | Constraint | Where governed | Status |
|---|-----------|----------------|--------|
| 1 | Multi-tenancy day 1 | AD-2 | ✅ (rows); ⚠️ storage — F1 |
| 2 | Texas-only | Scope + AD-8 (StateRulePack-TX, versioned, attorney-gated) | ✅ |
| 3 | Tax-ready double-entry | AD-7 + AD-11 (integer cents) | ✅ |
| 4 | Immutable audit | AD-6 | ✅ |
| 5 | FHA/FCRA by design | AD-10 (input side) | ✅ input side; ⚠️ F8 process side |
| 6 | Non-bypassable governance | AD-5 (bypass inside evaluate) | ✅ |
| 7 | Chat/SMS/email only | Deferred table (voice = non-goal) | ✅ |
| 8 | Seam/Stripe/Plaid required | Stack + AD-9 | ✅ with F7 caveat |
| 9 | Subdomain-only branding | Deployment section + Deferred | ✅ |

**F8 (LOW-MEDIUM) — Constraint 5's *sequencing* invariants are unplaced.** AD-10 covers the input side (protected attributes never reach models) by construction — good. But §92.3515 criteria notice *before* application fee, standalone FCRA consent *before* credit pull, and adverse-action-with-specific-reason are cross-feature legal ordering invariants (they bind CAP-2, screening, and payments simultaneously). They are arguably epic-level workflow detail, but because they span epics and carry statutory penalties, a one-line placement (e.g., under AD-8: "statutory notice/consent ordering is enforced as rules-engine preconditions, not UI flow") would prevent the leasing and payments epics from each assuming the other handles it. Currently silent.

### M1–M7 locked market-gap decisions

- **M1** — bound, mapped (`core/listings`, MITS feed, `agents/listings`) ✅ (but see F3: the MITS route contradicts AD-3's exception list).
- **M2** — bound; AD-8 is essentially M2's locked decisions (D1–D6) promoted to an AD, including the attorney prod gate ✅.
- **M3** — bound, mapped with CAP-2 ✅.
- **M4** — bound, mapped (`core/inspections`, Storage, comparison agent) ✅ (but see F1: the storage it depends on is un-governed).
- **M5** — bound; AD-7's trust sub-ledger class + governance-gated transfers is exactly the locked native-CAP-4 decision ✅.
- **M6** — correctly *absent* from binds (non-goal); its one MVP remnant (Notice to Pay template, TX-EV-001) is inside AD-8's delinquency binding ✅.
- **M7** — bound, mapped ✅ structurally, but see F2: the invariant that makes M7 work is missing.

**F9 (MEDIUM) — M8, M9, M10 and the spine's missing Open Questions section.** The spec leaves M8 (owner portal beyond statements), M9 (open API), M10 (document vault) as TBD founder picks. M9 is handled (Deferred, Phase 2 facade). M8 and M10 appear **nowhere** — not bound, not deferred, not an open question — and the spine has no Open Questions section at all. M10 matters architecturally *now* because M4 (locked, in-scope) references "M10 vault" as the storage home for inspection baselines, and CAP-2 produces lease documents that need a home; whether documents are a shared entity or per-feature tables is a real divergence point. The same applies to the spec's parked screening thesis (the spine defers the *vendor* but not the pending 1/2/3/1+3 thesis decision, which could reshape `core/screening`). Rubric item 5 says every dimension must be decided, deferred, or an open question — these are none of the three. Fix: add an Open Questions section carrying M8, M10, and the screening thesis with their revisit triggers.

---

## Checklist Item 5 — Every dimension this altitude owns: decided, deferred, or open

| Dimension | Status | Assessment |
|---|---|---|
| Application structure / boundaries | **Decided** | Paradigm + structural seed + dependency rule — strong. |
| API & client strategy | **Decided** | AD-3 (with F3 nit). |
| Data architecture | **Decided** (core) | ERD ownership + AD-2/7/11; per-CAP detail correctly deferred. |
| Async/workflow | **Decided** | AD-4. |
| AuthN/Z | **Decided** | Clerk roles convention + AD-2. |
| Deployment & environments | **Decided** | Three envs, separate Supabase/Clerk/Inngest per env, preview deploys, wildcard subdomains, EAS Phase 2 — well covered. |
| Infra/provider strategy | **Decided** | Vercel/Supabase/Clerk/Inngest with an explicit, quantified migration trigger. Good. |
| **Operations / monitoring / alerting** | **SILENT — F10 (HIGH)** | See below. |
| **Data lifecycle / backup / DR** | **SILENT — F11 (HIGH)** | See below. |
| Security envelope | **Partial — F12 (MEDIUM)** | See below. |
| Testing/CI | Partial (LOW) | Vitest/Playwright in stack; AD-1 mandates core unit tests; no CI gate stated (e.g., isolation tests as merge gate given CAP-11's pen-test success criterion). Minor at this altitude. |

**F10 (HIGH) — Operations/monitoring is a whole dimension left silent.** The spine has a *logging* convention (structured JSON, traceId, referencing `docs/rules/observability-logging.md` — which exists) but logging is not operations. Undecided, undeferred, unflagged: error tracking/APM (Sentry or equivalent — nothing in stack); who is alerted when a daily delinquency run fails at 3am (an *autonomous* platform whose failure mode is silent inaction — a missed TX 30-day deposit deadline is statutory liability, NFR-M2-04 has a 15-min SLA nobody watches); CAP-11's escalation table requires "alert platform ops" on cross-tenant attempts and RLS violations — no alerting channel exists in the architecture; LLM cost is logged per AD-10 but no budget/monitoring decision; no uptime/SLO posture. For a platform whose product *is* unattended background execution, this is the most consequential silence in the document. Minimum fix: one AD or convention block naming the error-tracking tool, the alert rule for failed/stalled Inngest runs and security events, and the ops destination.

**F11 (HIGH) — Data lifecycle/backup is a whole dimension left silent.** Nothing on: backup/PITR posture and restore testing (Supabase defaults exist but the spine neither adopts nor defers them); DR/RPO-RTO even at the "accept provider defaults, revisit at X" level; tenant offboarding — CAP-11 defines org *suspension* but data export/deletion on churn appears nowhere (B2B PM firms will contractually require export; deletion collides with AD-6's 7-year append-only retention and needs a decided answer, even if the answer is "traces survive offboarding, tenant PII is redacted"); FCRA screening-report retention/disposal obligations; storage-object lifecycle (inspection videos accumulate indefinitely). At minimum this belongs in Deferred with triggers; as written it is invisible.

**F12 (MEDIUM) — Security envelope is partial.** Covered well: tenancy (AD-2), append-only via DB grants (AD-6), no-PII-in-logs, FHA input stripping (AD-10), webhook signatures (AD-9), no-enumeration error convention. Not covered: secrets management beyond "typed env.ts" (who holds provider keys, per-env separation of Stripe/Clerk secrets); rate limiting/abuse protection on the public surfaces (prospect inquiry, application, resident portal auth — F3's public procedures make this concrete); PII/data-classification for screening data (SSNs, credit data — where stored, encrypted how, or explicitly "never stored, vendor-side only" which would be the best answer); session/token policy. Two or three sentences would cover the MVP posture.

---

## Checklist Item 6 — Diagrams are valid mermaid and convey structure

Validated by syntax inspection (no mermaid CLI in this environment):

1. **System graph (`graph TD`)** — valid. Node labels with `·` and `/` inside `[...]` are legal; `API -.emits.-> EVT` is correct dotted-link-with-label syntax. Conveys the dependency rule well, and the prose "arrows above are the only allowed directions" turns the picture into a checkable rule — good practice. Nit: `WEB --> API` renders as web depending on the api *package*, while AD-3 means the tRPC *client*; the prose dependency rule disambiguates.
2. **Deployment graph (`graph LR`)** — valid. Subgraphs, `;`-separated statements, and edges declared outside subgraphs all parse. Conveys the provider topology and the Vercel-as-single-compute-surface decision clearly. Nit: `I --> W` (Inngest invoking the serve route) is the one bidirectional pair and could use an edge label; EAS/mobile floats disconnected from the API surface it will call.
3. **ER diagram (`erDiagram`)** — valid. All cardinality tokens legal (including `}o--o|`), multi-word labels correctly quoted (`"leased via"`, `"audit (append-only)"`). Scoped correctly to names + ownership, resisting the temptation to design schemas at this altitude. Nits: `PROPERTY }o--|| OWNER` asserts exactly-one owner per property (co-ownership excluded — fine if intentional); `VENDOR`, `OWNER`, `RESIDENT` are not shown hanging off `ORGANIZATION`, leaving their tenant-scoping to AD-2's blanket rule (acceptable).

**Pass.**

---

## Consolidated findings

| # | Severity | Finding | Fix sketch |
|---|----------|---------|-----------|
| F1 | **HIGH** | File/object storage (Supabase Storage) has no tenancy, access-control, or convention rule; M4/CAP-2/CAP-3/M7 all write files; cross-tenant leak via storage URL defeats Constraint 1 and the CAP-11 pen test | Extend AD-2 or add AD-12: private buckets, `org/{orgId}/…` paths, storage RLS, signed URLs issued only via `packages/api` |
| F10 | **HIGH** | Monitoring/alerting dimension silent: no error tracking, no alerting for failed autonomous runs or CAP-11 security events, no LLM budget watch | Add ops convention/AD: error-tracker in stack, alert rules for stalled Inngest runs + security events, ops destination |
| F11 | **HIGH** | Data lifecycle/backup dimension silent: backup/PITR/DR posture, tenant offboarding export/deletion (vs 7-yr append-only traces), FCRA data disposal | Decide MVP posture (provider defaults + restore test) and add offboarding + retention rows to Deferred/Open Questions |
| F2 | **MED-HIGH** | No outbound-comms choke point; agents can bypass `core/comms`, breaking M7's unified inbox, Basic-plan approval, and the P3 approved-template gate | Add rule: all outbound messages via `core/comms` → thread persistence + template version; adapters never called by feature code |
| F3 | **MEDIUM** | AD-3's exception list contradicted by M1's MITS feed route; no public/unauthenticated procedure pattern for prospect flows | Enumerate exceptions exactly; define `publicProcedure` (subdomain-org, no session, rate-limited) |
| F9 | **MEDIUM** | M8/M10 (and parked screening thesis) neither decided, deferred, nor open; spine has no Open Questions section; M4 already references "M10 vault" | Add Open Questions section carrying M8, M10, screening thesis with revisit triggers |
| F12 | **MEDIUM** | Security envelope partial: secrets mgmt, rate limiting on public surfaces, screening-PII (SSN/credit) storage posture | 2–3 sentence MVP security posture block |
| F8 | LOW-MED | Constraint 5's statutory *ordering* invariants (§92.3515 notice before fee, FCRA consent before pull) unplaced across epics | One line under AD-8/AD-10 assigning them to rules-engine preconditions |
| F5 | LOW | AD-6 outcome-in-trace vs commit-before-side-effect tension for external calls, given no-UPDATE rule | Model traces as intent event + result event sharing traceId |
| F6 | LOW | AD-2 omits non-superuser app role / service-role-key prohibition (present in locked CAP-11 doc) | One sentence in AD-2 |
| F7 | LOW | "Plaid vs Stripe-only" deferral silently relaxes SPEC Constraint 8 / the "architecture phase finalizes" assumption | Acknowledge in the Deferred row or amend spec |
| F4 | LOW | Scheduled-job idempotency (daily delinquency re-run double-posting) unstated | Convention: idempotency keys on ledger posts / step dedupe |

## Rubric scorecard

| Item | Result |
|---|---|
| 1. Fixes real divergence points, misses none | **Partial** — 11 strong ADs; misses storage (F1) and comms (F2) |
| 2. Rules enforceable, prevent divergence | **Pass** — minor wrinkles F5/F6 |
| 3. Deferred cannot cause incompatible divergence | **Pass** — F7 noted |
| 4. Covers driving spec (12 CAPs, 9 constraints, M1–M7) | **Pass with gaps** — F3, F8, F9 |
| 5. Every owned dimension decided/deferred/open | **Fail on two dimensions** — F10, F11; partial F12 |
| 6. Diagrams valid and structural | **Pass** |

**Overall: approve with revisions.** The core architectural spine (boundaries, tenancy, governance, ledger, audit, rules engine) is decided correctly and enforceably; fix F1/F2/F10/F11 before epics start building against it, since all four are cheapest to fix while nothing exists yet.
