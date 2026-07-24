# Requirements Traceability — Chat → Docs

**Last updated:** 2026-07-24  
**Purpose:** Every aligned decision in spec sessions is written to requirement docs — not left in chat. This index shows **where to find what**.

---

## Documentation workflow (BMad rule)

When we align in a guided session:

```
1. Discuss & decide (chat)
2. Append decision     →  _bmad-output/specs/spec-rentalpro/.memlog.md  (machine log)
3. Update kernel       →  _bmad-output/specs/spec-rentalpro/SPEC.md       (if kernel-level)
4. Write requirements  →  docs/ companion (runbook, engine, CAP deep-dive)
5. Cross-link          →  MARKET-GAP-CHECKLIST, HANDOFF, CAP-N, README
6. Commit & push       →  PR branch
```

**Chat is not the system of record.** If it's aligned but not in `docs/` or `_bmad-output/`, it doesn't exist for engineering or PRD.

## Full requirement docs (M1–M10)

Each locked market gap has a **companion req doc** with 9 sections (problem, vision, stories, flows, FRs, NFRs, data/API, acceptance criteria, runbook). Template: [`templates/MARKET-GAP-REQ-TEMPLATE.md`](./templates/MARKET-GAP-REQ-TEMPLATE.md).

| ID | Full requirements doc | Sections complete? |
|----|----------------------|-------------------|
| M1 | [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) | ✅ §1–9 |
| M2 | [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) | ✅ §1–9 |
| M3 | [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) | ✅ §1–9 |
| M4 | [`MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md`](./MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md) | ✅ §1–9 |
| M5 | [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) | ✅ §1–9 |
| M6 | [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) | ✅ non-goal + checklists |
| M7 | [`UNIFIED-COMMS-HUB-MVP-REQ.md`](./UNIFIED-COMMS-HUB-MVP-REQ.md) | ✅ §1–9 |
| M8–M10 | TBD on lock | ⬜ |

---

## Session decisions → canonical docs

### Spec kernel (five fields)

| Topic | Status | Canonical doc |
|-------|--------|---------------|
| Why (force, target, lever, tiered autonomy) | ✅ Locked | `SPEC.md` §Why · `HANDOFF-TO-CURSOR.md` |
| Capabilities (12 CAPs intent + success) | 🔄 Draft | `SPEC.md` §Capabilities · `docs/capabilities/CAP-N-*.md` |
| Constraints (9 non-negotiables) | ✅ Locked | `SPEC.md` §Constraints |
| Non-goals (Phase 2 table) | ✅ Locked | `SPEC.md` §Non-goals |
| Success signal (14-day APM criteria) | ✅ Locked | `SPEC.md` §Success signal |

### Market gaps M1–M10 (guided picks)

| ID | Topic | Status | Requirements doc | CAPs |
|----|-------|--------|------------------|------|
| **M1** | Listing Package + one-submit syndication | ✅ **Locked Full MVP** | [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) | CAP-2 |
| **M2** | Delinquency + state rules + PM customization | ✅ **Locked MVP** | [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) | CAP-4, CAP-7, CAP-5 |
| **M3** | Lease renewals + Owner approval on increases | ✅ **Locked MVP** | [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) | CAP-2, CAP-8, CAP-5 |
| **M4** | Move-in / move-out inspections | ✅ **Locked MVP** | [`MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md`](./MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md) | CAP-2, CAP-7, M5 |
| **M5** | Security deposit native sub-ledger | ✅ **Locked MVP** | [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) | CAP-4, M4 |
| **M6** | Eviction & legal notices | ✅ **Non-goal MVP** | [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) | M2 notices only |
| **M7** | Unified comms hub (inbox + SMS/email/chat) | ✅ **Locked Full MVP** | [`UNIFIED-COMMS-HUB-MVP-REQ.md`](./UNIFIED-COMMS-HUB-MVP-REQ.md) | CAP-7 expand |
| M8 | Owner portal UX | ⬜ Open | `MARKET-GAP-CHECKLIST.md` | CAP-8 (TBD) |
| M9 | Open API & webhooks | ⬜ Open | `MARKET-GAP-CHECKLIST.md` | TBD |
| M10 | Document vault | ⬜ Open | `MARKET-GAP-CHECKLIST.md` | CAP-2 (TBD) |
| M11 | 1099 tax reporting | ✅ Non-goal | `SPEC.md` §Non-goals | — |
| M12 | Tour / self-showing | ✅ Non-goal | `SPEC.md` §Non-goals | — |

### M1 — what's documented (from chat)

| Chat topic | Captured in |
|------------|-------------|
| Listing Package unified form (pre-info, one submit) | `SYNDICATION-MVP-RUNBOOK.md` — Listing Package schema |
| Publish to all ILS (Zillow, Apartments.com, org page) | Runbook Track A (external approvals) + Track B (Syndication Hub) |
| Competitor pattern (Buildium one-click, MITS feeds) | Runbook + competitive-matrix.md |
| Zillow approval 4–6 wk, Apartments.com vendor partnership | Runbook A1–A7 |
| Day 1 emails (Zillow, Apartments.com, RentSync) | Runbook copy-paste templates |
| Org listing page as no-approval fallback | Runbook blockers table |
| MITS as canonical feed format | Runbook B2 |

### M2 — what's documented (from chat)

| Chat topic | Captured in |
|------------|-------------|
| State-by-state rules + PM customization | `DELINQUENCY-RULES-ENGINE.md` — 3-layer model |
| What PM can vs cannot modify | Same — table §What PM can vs cannot modify |
| Texas §92.019 rules (grace, 10%/12% caps) | Same — Texas RulePack v1 |
| Agent hard-blocks illegal fees | Same — agent flow + competitive gap |
| Payment plans with PM approval | Same — Track D D4 |
| **Attorney review before prod** | `DELINQUENCY-RULES-ENGINE.md` Runbook P1 + [`ATTORNEY-REVIEW-CHECKLIST.md`](./ATTORNEY-REVIEW-CHECKLIST.md) §2 (**Mandatory** items) |
| Master attorney checklist (all M-areas) | [`ATTORNEY-REVIEW-CHECKLIST.md`](./ATTORNEY-REVIEW-CHECKLIST.md) — **Mandatory / Optional / Conditional / Deferred** tags |
| Attorney brief checklist | Same — Runbook P2 |

### AI agents (Texas MVP)

| Topic | Doc |
|-------|-----|
| Leasing, maintenance, communication locked | [`AI-MVP-DECISIONS.md`](./AI-MVP-DECISIONS.md) |
| Screening parked | Same §Parked |
| Emergency dispatch list | Same |

### Infrastructure & SaaS

| Topic | Doc |
|-------|-----|
| CAP-11 multi-tenant locked | [`capabilities/CAP-11-multi-tenant-saas.md`](./capabilities/CAP-11-multi-tenant-saas.md) |
| Clerk, RLS, subdomain branding | CAP-11 + SPEC assumptions |
| Stack direction (Vercel, Supabase) | SPEC assumptions · memlog |

---

## Doc types & when to use each

| Type | Path pattern | Use for |
|------|--------------|---------|
| **Spec kernel** | `_bmad-output/specs/spec-rentalpro/SPEC.md` | WHY, CAP summaries, Constraints, Non-goals, Success |
| **Decision log** | `_bmad-output/specs/spec-rentalpro/.memlog.md` | Every locked decision (dated, append-only) |
| **Runbook** | `docs/*-RUNBOOK.md` or runbook section in engine doc | Time-sensitive tasks, approvals, prerequisites |
| **Rules engine / architecture** | `docs/*-RULES-ENGINE.md` | Domain logic, state rules, data models |
| **CAP deep-dive** | `docs/capabilities/CAP-N-*.md` | User stories, flows, APIs, acceptance tests |
| **Checklist** | `docs/MARKET-GAP-CHECKLIST.md` | M1–M10 status + sub-feature assignment |
| **Handoff** | `docs/HANDOFF-TO-CURSOR.md` | Session pick-up, next steps |

---

## Still in chat only (not yet locked → not full req docs)

| Topic | Next action |
|-------|-------------|
| M8–M10 picks | Continue guided walkthrough → doc per M2/M1 pattern |
| Screening criteria + industry thesis | `resume screening` → update `AI-MVP-DECISIONS.md` |
| CAP-1–10, 12 micro-spec locks | `lock CAP-N` → promote draft to locked |
| Native mobile (Phase 2 Non-goal) | Open TODOs → [`MOBILE-APP-PHASE2-GAP-DISCUSSION.md`](./MOBILE-APP-PHASE2-GAP-DISCUSSION.md); do not reopen Non-goal until decided |
| bmad-prd | After M1–M10 + CAP locks |

---

## Maintenance rule for agents

After every aligned decision in chat:

1. Update this file's status table if M1–M10 or kernel field changes  
2. Append `.memlog.md`  
3. Write or extend the companion req doc (never chat-only)  
4. Cross-link from `MARKET-GAP-CHECKLIST.md` and affected CAP docs  
5. Commit before ending session  

**Partner-readable summary:** [`docs/README.md`](./README.md) · **Machine contract:** `_bmad-output/specs/spec-rentalpro/`
