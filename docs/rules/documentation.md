# Documentation maintenance

When you change architecture, entry flows, major modules, or user-visible behavior, update docs in the same change — do not defer to chat-only notes.

## Full requirement capture standard

Every locked market gap (M1–M10) and locked CAP must have a **companion req doc** with all sections below filled before bmad-prd or dev stories. Use [`docs/templates/MARKET-GAP-REQ-TEMPLATE.md`](../templates/MARKET-GAP-REQ-TEMPLATE.md).

| # | Section | Purpose |
|---|---------|---------|
| 1 | Problem & rationale | Why — survives months later when building |
| 2 | Product vision | What + differentiator vs competitors |
| 3 | User stories | WHO wants WHAT |
| 4 | End-to-end flows | Happy path + Basic vs Pro + edge cases |
| 5 | Functional requirements | FR-IDs, testable |
| 6 | Non-functional requirements | Performance, tenancy, audit |
| 7 | Data model & integrations | Entities, APIs, third parties |
| 8 | Acceptance criteria | Checkboxes for QA / story done |
| 9 | Runbook | Time-sensitive tasks, prerequisites, blockers |

**Rule:** If it was discussed in chat and affects build behavior, it goes in sections 1–8 — not chat-only.

## Change → doc mapping

| Change type | Update |
|---|---|
| Spec kernel (Why, Capabilities, Constraints) | `_bmad-output/specs/spec-rentalpro/SPEC.md` + `.memlog.md` |
| Market gap decision (M1–M10) | Companion req doc + `MARKET-GAP-CHECKLIST.md` + `REQ-TRACEABILITY.md` + `.memlog.md` |
| Time-sensitive MVP tasks / external approvals | `docs/*-RUNBOOK.md` or runbook section in domain doc |
| Domain rules engine (state compliance, etc.) | `docs/*-RULES-ENGINE.md` + affected CAP docs |
| Competitive research / market positioning | `_bmad-output/specs/spec-rentalpro/competitive-matrix.md` |
| Capability deep-dive locked | Matching `docs/capabilities/CAP-N-*.md` + `.memlog.md` |
| Architecture / major module / entry flow | `docs/ARCHITECTURE.md` (when created) — changelog row with date + one-line summary |
| Product-visible feature (dashboard, portals, onboarding) | `docs/PROJECT_STATE.md` (when created) |
| DB schema / Prisma model | `docs/database-schema.md` (when created) |
| Auth / onboarding gate | `docs/FLOW.md` (when created) |
| MVP scope / sprint order | `docs/MVP-SPRINT-PLAN.md` |
| BMad workflow reference | `docs/BMAD-REFERENCE.md` |
| Follow-up / action items | `docs/ACTION_ITEMS.md` (when created) |
| Finished deliverables | `docs/COMPLETED_ITEMS.md` (when created) |

## Rules

- Only update docs affected by the current change — never rewrite everything
- Never speculate beyond code or locked spec — if unknown write "Not found in current context"
- Skip doc updates for trivial fixes (typos, single-line tweaks) unless they change documented behavior
- Do not expand README.md with feature details — point to `docs/README.md` instead
- **Memlog is canonical** for spec decisions — append to `.memlog.md`, then re-derive `SPEC.md`

## Spec phase (current)

During bmad-spec guided mode:

- Lock fields in order: Why → Capabilities → Constraints → Non-goals → Success signal
- Every locked decision gets a dated entry in `_bmad-output/specs/spec-rentalpro/.memlog.md`
- Do not jump to PRD or architecture until all five spec fields are locked

## Onboarding / web UI changes (when code exists)

- New or updated wizard steps → `docs/FLOW.md` + `docs/PROJECT_STATE.md`
- New Prisma models → `docs/database-schema.md` + migration
