# EasySubmit Rules Capture

**Source:** [bhagatks/EasySubmit](https://github.com/bhagatks/EasySubmit)  
**Captured:** 2026-07-05  
**Purpose:** Reference archive of EasySubmit's agent/engineering rules. RentalPro's active rules live in `AGENTS.md`, `docs/rules/`, and `.cursor/rules/`.

---

## Rule architecture (EasySubmit pattern)

| Layer | Location | Role |
|---|---|---|
| **Canonical shared rules** | `docs/rules/*.md` | Single source of truth for Claude Code + Cursor |
| **Agent entry point** | `AGENTS.md` / `CLAUDE.md` | Project-wide agent rules |
| **Cursor rules** | `.cursor/rules/*.mdc` | `@include` shared rules or add domain-specific globs |
| **Domain specs** | `docs/resume/RULES.md`, `docs/IDENTITY_AND_BOOT_RULES.md`, etc. | Deep specs referenced by agents |
| **Implementation prompts** | `docs/cursor-prompts/*-rules.md` | Completed feature-specific rule sets |

---

## Shared rules (`docs/rules/`)

### Observability & logging

- Every new feature, pipeline step, gate, or server action **must** include structured logs
- Log `start` / `done` / `fail` / `block` / `skip` for each logical step
- Use domain loggers: `logEnhanceDiag()`, `logEnhance()`, `logApiCall()`, etc.
- Severity: `light` | `low` | `high`
- Required fields: `traceId`, `designStep`, `track`, `phase`, `flags`, `params`
- No silent `catch` blocks — every error logs at `high` with `errorCode`
- Exempt: trivial copy/CSS-only changes

### Documentation maintenance

- Update docs in the same change — never defer to chat-only notes
- Change → doc mapping (enhance pipeline → `enhance-pipeline-design.md`, schema → `database-schema.md`, etc.)
- Only update affected docs; never rewrite everything
- Never speculate — write "Not found in current context" if unknown
- Don't expand README with feature details — point to `docs/`

### Env domains

- **Three isolated domains:** `database`, `posthog-admin`, `posthog-runtime`
- PostHog admin scripts must use `buildPostHogAdminEnv()` — **never** load full `.env.local`
- `prisma.config.ts` does not load `.env.local`
- Single source: `lib/env/env-resolution.mjs`

### Sidepanel completion gate

- Applies to user-visible extension/sidepanel changes
- Before done: update docs, run vitest, run `npm run build`
- Note test gaps in `ACTION_ITEMS.md` if React-only and untestable

### Testing

- **`lib/` coverage must stay ≥ 85%** (statements, branches, functions, lines)
- Enforced by pre-push hook + vitest thresholds
- Every new `lib/` file needs a `.test.ts`
- Don't test: React components, hooks, server actions, Prisma/Supabase-dependent code
- Don't mock the database

### Analytics

- **Never call `posthog.capture()` directly** — use `@/src/shared/analytics` helpers
- All event names in `AnalyticsEvents` catalog
- Every event needs `surface` property
- **No PII** (email, name, resume text, API keys)

---

## Main agent rules (`AGENTS.md`)

### Project identity

- **Brand:** EasySubmit.ai
- **Mission:** Best ATS resume quality + end-to-end job application automation
- **Surfaces:** Next.js 14 dashboard + Chrome Extension (MV3) + AI resume engine

### Tech stack

- Next.js 14 App Router, TypeScript strict, Supabase Postgres + Prisma, NextAuth, Zustand, Tailwind OKLCH, BYOK AI + Gemini pool, Vitest

### Design system

- OKLCH tokens: deep navy surface, primary blue glow, mint success, warning red
- 12px radius, Space Grotesk display, DM Sans body
- Tone: "Career Navigator" — professional, never cutesy
- Buttons say **"Continue"** not "Next"
- Extension UI via **Shadow DOM**

### Core business rules

**Features framework:**

- Any feature touching flags/config/quota/subscription **must** register in `lib/features/index.ts` → `resolveFeature()`
- Never call `getFeatureFlags()`, `getAppConfig()`, `isSubscribed()`, `resolveAiRoute()` directly in server actions
- Registered: `enhance`, `subscription`

**AI routing (every AI action):**

1. BYOK vaulted key → use it (unlimited)
2. `aiDailyUnlimited` → unlimited system AI
3. Check `app_config.aiEngine.quotas.system.enable`
4. Check daily quota → decrement or block with `capacity_exhausted`

**Resume format (non-negotiable):**

- Fixed section order: Header → Summary → Skills → Experience → Education → Certifications → Projects → Languages
- Section names match `RESUME_SECTION_TITLES` in `lib/resume/resumeSpec.ts`
- Never: multi-column, layout tables, text boxes, header/footer fields, images, unicode bullets, hidden text, skill ratings
- Always: single column, real bullet numbering, tab stops for dates, Calibri/Arial/Helvetica only

**Chrome extension:**

- MV3 only, service worker background
- Messaging via `chrome.runtime.sendMessage` / `onMessage` / `connect` ports
- Always handle `chrome.runtime.lastError`
- Never edit `dist/`

### Completion gate

1. Run vitest, fix failures, add tests for new shared logic
2. Run `tsc --noEmit`, zero new errors
3. Update relevant docs
4. Structured logging per observability rules
5. `npm run build` passes

### Coding conventions

- Functional components only, no `any`, named exports from lib files
- Server actions in `app/actions/`, no server imports in client components
- `"use client"` only when needed
- Prefer editing existing files
- Comments explain *why*, not *what*
- No half-finished implementations

### Session discipline

- Use `/clear` between unrelated tasks
- Reference specific files and line numbers
- Split explore vs implement into separate sessions
- Targeted test runs over full suite
- Stop when done — no unsolicited follow-up refactors

---

## Cursor-specific rules (`.cursor/rules/`)

| File | Scope | Content |
|---|---|---|
| `easysubmit-basics.mdc` | `alwaysApply: true` | `@CLAUDE.md` |
| `observability-logging.mdc` | always | `@docs/rules/observability-logging.md` |
| `documentation.mdc` | always | `@docs/rules/documentation.md` |
| `testing.mdc` | always | `@docs/rules/testing.md` |
| `analytics.mdc` | always | `@docs/rules/analytics.md` |
| `sidepanel-completion.mdc` | always | `@docs/rules/sidepanel-completion.md` |
| `graphify.mdc` | always | Mandatory graphify-first exploration |
| `easysubmit-resume-format.mdc` | resume globs | Section order + hard never list |
| `chrome-extension.mdc` | extension | MV3 messaging rules |
| `web-ui.mdc` | dashboard/landing | React + brand tokens |
| `resume-engine.mdc` | resume-engine | LaTeX master template pipeline |
| `resume-parser.mdc` | parser | Dual-route parser architecture |
| `firestore-email.mdc` | support/email | Firestore + EmailJS schema |

---

## Domain rule specs

### Resume formatting (`docs/resume/RULES.md`)

- ATS parses linear text top-to-bottom
- US Letter, 0.5–1" margins, single font family, 10–12pt body
- Generate **.docx first**, PDF from .docx
- Fixed section order, standard section names only
- Skills: 6–20 items, ≥70% hard skills, banned soft-skill slot-wasters
- Experience: tab-aligned dates, max 6 bullets/role, recency taper
- Page-length engine: integer pages 1–6 only
- Hard never list: multi-column, tables, text boxes, hidden text, creative section names, etc.

### Identity & boot (`docs/IDENTITY_AND_BOOT_RULES.md`)

- **Two domains never merge:** login profile (`users`) vs resume profile (`profiles`)
- Resume edits never write back to `users`
- Mandatory profile fields before dashboard
- Boot resolver runs on every authenticated request

### Features framework (`docs/features-framework.md`)

- `resolveFeature()` is the only entry point for flag/config/quota/subscription decisions
- Surface→flag mapping lives inside resolver files only
- Resolved objects contain `available` + `reason` — never raw flags

### Enhance pipeline (`docs/north-star.md`)

- 3 phases: Analyze → Baseline Apply → AI Upgrade (conditional)
- AI never sees raw form — only `baselineForm + brief`
- AI failure ≠ enhance failure — returns baseline + warning

---

## Content validation rules

### Summary rules

- Exactly **4 sentences**, **70–80 words**
- Banned words in `lib/resume/summary-rules.ts`

### Skills rules

- Manual: 6 min, 10–15 target, 20 max
- AI/deterministic: 15 min, 15–20 target, 20 max
- Validators in `lib/resume/skills-rules.ts`

### Resume validation framework

- Entry: `validateResume(form, targetRole)` in `lib/resume/validation/index.ts`
- UI must call `validateResume()` — never low-level validators directly

---

## Complete source file inventory

```
EasySubmit/
├── AGENTS.md
├── CLAUDE.md
├── .claude/CLAUDE.md
├── .cursor/rules/          (13 .mdc files)
└── docs/
    ├── rules/              (6 canonical shared rules)
    ├── resume/RULES.md
    ├── IDENTITY_AND_BOOT_RULES.md
    ├── features-framework.md
    ├── north-star.md
    ├── DEVELOPMENT_WORKFLOW.md
    └── cursor-prompts/     (summary, skills, validation rules)
```

---

## What RentalPro adopted

See `AGENTS.md` and `docs/rules/` for RentalPro's adapted version of this pattern — same layered structure, domain rules rewritten for APM / BMad / multi-tenant SaaS.
