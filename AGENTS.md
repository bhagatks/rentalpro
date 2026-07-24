# RentalPro.ai — Agent Rules

## Shared Rules

The following files in `docs/rules/` are the single source of truth — both Claude Code and Cursor read from them. Do not duplicate their content here.

| Rule | File |
|---|---|
| BMad spec workflow | `docs/rules/bmad-spec.md` |
| Documentation maintenance | `docs/rules/documentation.md` |
| Multi-tenant SaaS | `docs/rules/multi-tenant.md` |
| Observability & logging | `docs/rules/observability-logging.md` |
| Testing & coverage gate | `docs/rules/testing.md` |

**Reference:** EasySubmit rules archive at `docs/EASYSUBMIT-RULES-CAPTURE.md` (pattern source, not active rules).

---

## Project Identity

- **Brand:** RentalPro.ai
- **Mission:** Autonomous Property Management (APM) — AI-native platform that replaces the human property manager.
- **Product surfaces:** B2B SaaS multi-tenant dashboard + resident/owner portals + autonomous agent workflows.
- **Phase:** bmad-spec guided mode — kernel complete except CAP micro-spec locks; P0 pilot gate before full MVP; next: finish M8–M10 / CAP locks → pilot → bmad-prd
- **North star brief:** `_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md`

---

## Tech Stack (planned — not locked)

| Layer | Direction |
|---|---|
| Framework | Next.js 14+ (App Router) |
| Language | TypeScript — strict, no `any` without justification |
| Database | PostgreSQL (Prisma) with multi-tenant row-level security |
| Auth | TBD (multi-tenant org + role model) |
| AI | Claude 3.5 Sonnet / GPT-4o for agent workflows |
| Integrations | Seam (smart locks), Stripe (payments/ID), Plaid (bank feeds) |

---

## Directory Map

```
docs/                           Human wiki — system of record for partners
docs/capabilities/              CAP deep-dives (micro-specs)
docs/rules/                     Shared agent rules (canonical)
_bmad-output/specs/spec-rentalpro/   Spec kernel + memlog + companions
_bmad-output/planning-artifacts/     Briefs, PRDs, architecture (future)
.cursor/rules/                  Cursor rule files (@include docs/rules/)
```

---

## Core Business Rules (locked)

### Tiered autonomy

- **Basic ($29/mo):** AI is interpreter — human approves significant actions and financial spend
- **Professional ($99/mo):** High autonomy with governance rails (e.g., auto-approve repairs up to $500)

### Multi-tenancy (non-negotiable)

- Full rules: `docs/rules/multi-tenant.md`
- Every PM company in isolated workspace from Day 1
- Governance rails, module toggles, and audit trails are tenant-scoped

### Module autonomy (CAP-6)

- PM admin toggles AI-driven vs human-in-the-loop **per module** (leasing, maintenance, accounting)
- Never global defaults that override tenant configuration

### Accounting (MVP)

- AI categorizes 100% of transactions autonomously
- Flagged as "Pending Review" — owner/accountant signs off monthly before distributions
- Double-entry ledger; every transaction via API (Plaid/Stripe)
- Management fee: **7%** (CAP-1 locked on Owner; CAP-4 posts from that rate)

### Compliance

- FHA compliance: AI screening logic must not discriminate
- Audit trail for all autonomous actions (CAP-10)

---

## BMad Workflow Rules

Full rules: `docs/rules/bmad-spec.md`

- **Spec before PRD before Architecture before Code** — do not skip steps
- Five-field kernel: Why, Capabilities, Constraints, Non-goals, Success signal
- **Memlog is canonical** — append to `.memlog.md`, then re-derive `SPEC.md`
- Capabilities need intent (WHAT) + success criterion (how to test)

Current status:

| Field | Status |
|---|---|
| Why | ✅ Locked |
| Capabilities | 🔄 Draft (12 micro-specs; CAP-11 locked) |
| Constraints | ✅ Locked |
| Non-goals | ✅ Locked |
| Success signal | ✅ Locked |

---

## Documentation Rules

Full rules: `docs/rules/documentation.md`

The `/docs` folder is the **system of record** for human readers. `_bmad-output/` is the machine contract.

After any meaningful change:

| Change type | Update |
|---|---|
| Spec decision | `.memlog.md` + `SPEC.md` |
| Capability locked | `docs/capabilities/CAP-N-*.md` |
| MVP scope | `docs/MVP-SPRINT-PLAN.md` |
| Architecture (when exists) | `docs/ARCHITECTURE.md` changelog row |

---

## Completion Gate (when code exists)

1. **Tests** — run test suite; add tests for new shared logic in `lib/`
2. **Types** — `tsc --noEmit`, zero new errors in changed files
3. **Docs** — update relevant doc files per mapping above
4. **Logging** — structured logs per `docs/rules/observability-logging.md`
5. **Build** — confirm build passes before claiming a feature is done
6. **Tenant isolation** — verify multi-tenant rules per `docs/rules/multi-tenant.md`

---

## Coding Conventions (when implementation begins)

- Functional components only — no class components
- No `any` — use `unknown` + type guards if shape is truly unknown
- No default exports from lib files — named exports only
- Server actions separated from client components
- `"use client"` only when the component genuinely needs browser APIs
- Prefer editing existing files over creating new ones
- No comments explaining *what* code does — only *why* when non-obvious
- No half-finished implementations — if a feature isn't ready, don't wire it up
- Do not add error handling for impossible cases — trust internal contracts
- Prisma: both sides of relations, `createdAt`/`updatedAt`, indexes on tenant FKs

---

## Key Files Quick Reference

| What | Where |
|---|---|
| North star (product brief) | `_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md` |
| Vision + locked decisions | `docs/HANDOFF-TO-CURSOR.md` |
| P0 pilot gate | `docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md` |
| BMad workflow guide | `docs/BMAD-REFERENCE.md` |
| Spec kernel | `_bmad-output/specs/spec-rentalpro/SPEC.md` |
| Decision log | `_bmad-output/specs/spec-rentalpro/.memlog.md` |
| Competitive matrix | `_bmad-output/specs/spec-rentalpro/competitive-matrix.md` |
| MVP sprint plan | `docs/MVP-SPRINT-PLAN.md` |
| Capability template | `docs/capabilities/_TEMPLATE.md` |
| EasySubmit rules reference | `docs/EASYSUBMIT-RULES-CAPTURE.md` |

---

## Session Discipline

- Read `docs/HANDOFF-TO-CURSOR.md` and `.memlog.md` before proposing spec changes
- Reference specific files and line numbers in prompts
- Do not start coding MVP until spec is complete
- Prefer targeted reads over full codebase exploration during spec phase
- If a task is done, stop — do not propose follow-up refactors unless asked
