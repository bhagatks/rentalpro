# Review: Stack Versions & Technology Currency — ARCHITECTURE-SPINE.md

**Reviewer:** fact-checking pass with live web verification
**Date:** 2026-07-05
**Scope:** `## Stack` table and every named technology in the spine
**Method:** WebSearch + WebFetch against official docs, changelogs, and the npm registry (live `@clerk/expo` package metadata), plus the current Anthropic API model catalog. Findings below distinguish "verified current" from "asserted but stale/risky."

---

## Verdict

**PASS with two corrections.** The Stack table is overwhelmingly accurate for July 2026 — every version pin checked out against live sources except one (NativeWind 5, which is still pre-release), and one claim that was flagged as a possible mismatch (Clerk ↔ Expo SDK 56) turns out to have been **resolved upstream** since the earlier research. No technology in the table is dead, renamed, or misfit for its stated role.

---

## Line-by-line verification results

| Spine claim | Verified? | Reality (July 2026) |
| --- | --- | --- |
| Next.js 16.2.x LTS | ✅ | 16.2.10 LTS is the current supported release (16.2 backport line receiving fixes as of June 2026; Next 15 LTS EOS Oct 21, 2026). Pin is correct. |
| Expo SDK 56 (RN 0.85, React 19.2) | ✅ | SDK 56 released May 21, 2026 with exactly RN 0.85 / React 19.2. Note: SDK 56 bumps TypeScript to 6.0.3, replaces global fetch with `expo/fetch`, deprecates `@expo/vector-icons`. None affect the spine's invariants. |
| tRPC 11.x | ✅ | Current stable is 11.16.0 (released late June 2026). v12 is discussed publicly but not stable — pinning "11.x" is the right call today. |
| Drizzle ORM + RLS policies | ✅ with caveat | Drizzle has first-party RLS support (since 0.36.0): `pgPolicy`, roles, auto-enable RLS on policied tables, and a dedicated `drizzle-orm/supabase` import with Supabase's predefined roles. Caveat below (Finding 3). |
| PostgreSQL 17 (Supabase) | ✅ | Postgres 17 is the Supabase platform default for new projects; self-hosted docker-compose moved 15→17 the week of June 15, 2026. |
| Supabase Storage | ✅ | Exists, fits stated role (files/videos/photos). |
| Clerk `@clerk/nextjs` / `@clerk/expo` Core 3 | ✅ (mismatch resolved) | Core 3 line is real (Clerk changelog 2026-03-09: native RN components, Google Sign-In, Core 3). See Finding 1 for the Expo SDK 56 question. |
| Inngest | ✅ | Confirmed fit (Finding — none). Details below. |
| Vercel AI SDK 6.x | ✅ | AI SDK 6 went stable Dec 22, 2025: production agents, stable MCP (`@ai-sdk/mcp`) with OAuth, human-in-the-loop tool approval, stable `generateImage`. Zod-validated structured outputs (per AD-10) are a core supported pattern. |
| Anthropic `claude-sonnet-5` / `claude-haiku-4-5` | ✅ | Both are current, correct API model IDs (verified against the current Anthropic model catalog — exact strings, no date suffixes). `claude-sonnet-5`: 1M context, $3/$15 per MTok with intro pricing $2/$10 through 2026-08-31 — well-suited to "agent reasoning." `claude-haiku-4-5`: 200K context, $1/$5 — appropriate for classification. See Finding 4 for two budget-relevant notes. |
| Tailwind CSS 4 + shadcn/ui (web) | ✅ | Tailwind 4 current and stable; shadcn/ui actively maintained with Tailwind 4 support. |
| NativeWind v5 (mobile) | ⚠️ | **Still pre-release.** See Finding 2. |
| Zod (latest stable) | ✅ | Zod 4 is current; unpinned "latest stable" is fine. |
| Stripe / Seam / Plaid / Twilio / Resend | ✅ (shallow) | All active vendors with current APIs; behind ports per AD-9 so swap risk is contained. Not deep-verified (no version pins claimed). |
| Vitest / Playwright | ✅ (shallow) | Both actively maintained; "latest stable" unpinned is fine. |
| Turborepo + pnpm workspaces | ✅ (shallow) | Standard, actively maintained. |

---

## Findings

### Finding 1 — Clerk `@clerk/expo` Core 3 ↔ Expo SDK 56: mismatch is RESOLVED, but update the research trail — **LOW (info/correction)**

The concern raised (earlier research said Clerk docs target Expo SDK 53–55, spine pins SDK 56) was checked against the **live npm registry**:

- `@clerk/expo` latest = **3.6.5**, with peer dependency **`expo: ">=53 <57"`** — i.e., **Expo SDK 56 is inside the supported range**.
- `react-native: ">=0.75"` (Expo 56's RN 0.85 ✓) and `react: "^18 || ^19"` (React 19.2 ✓).

The earlier "53–55" finding was accurate for `@clerk/expo` 3.1.x-era docs (Clerk's compat articles cover SDK 54/55 and haven't been updated), but the package itself has moved. **Action:** note in the spine's `.memlog.md` (or research companion) that the Clerk/Expo-56 blocker is cleared as of `@clerk/expo` 3.6.5 — and, since mobile is Phase 2 anyway, re-verify the peer range at Phase 2 kickoff (Expo 57 will likely be out by then and the `<57` ceiling will bite in exactly the same way).

### Finding 2 — NativeWind v5 is still pre-release, "not intended for production use" — **MEDIUM**

The spine's Stack table pins "NativeWind v5 (mobile)" under a header that says "*verified current July 2026*." The **official NativeWind docs today label v5 pre-release and explicitly not intended for production**; v4 is the stable line (but v4 is built on Tailwind 3-era config, which conflicts with the spine's "Tailwind 4" pin). So the row as written contains an internal tension: Tailwind 4 (web, correct) + NativeWind 5 (mobile, pre-release).

**Mitigations already in the spine:** mobile is Phase 2 and only a scaffold exists; the Deferred table pushes "Mobile UI build-out" to Phase 2. So this is not a build blocker.

**Action:** annotate the Stack row — e.g. "NativeWind v5 (pre-release as of 2026-07; re-verify GA at Phase 2 kickoff; fallback = NativeWind v4 + Tailwind 3 preset scoped to `apps/mobile`)". Don't present it as "verified current" without the caveat. The requirement that `packages/ui` share design tokens across web (Tailwind 4) and native (NativeWind) should be designed token-first (plain TS/CSS-var tokens) so it doesn't depend on which NativeWind major ships.

### Finding 3 — Drizzle RLS with Supabase: real, but the convenience layer is Neon-first — **LOW**

Drizzle's RLS support is genuine and current: policies/roles in the schema DSL, auto-enabled RLS, drizzle-kit migration generation, and a `drizzle-orm/supabase` import shipping Supabase's predefined roles (`authenticated`, `serviceRole`, etc.) marked as existing. However, the ergonomic `crudPolicy()` helper lives in the **`/neon`** import; the Supabase import is thinner ("will be extended in a future release" per Drizzle docs).

This barely matters for the spine because **AD-2's design doesn't lean on Supabase-auth-JWT RLS at all** — it uses `SET LOCAL app.current_org_id` + `FORCE ROW LEVEL SECURITY` with an org-scoped client, i.e., plain Postgres policies referencing a session GUC. That pattern is fully expressible in Drizzle's `pgPolicy` (or raw SQL migrations) with no Supabase-specific helper needed. **Action:** none required; just don't expect `crudPolicy`-style sugar for the Supabase path — expect to write the `USING (organization_id = current_setting('app.current_org_id')::uuid)` policies by hand.

### Finding 4 — Anthropic model IDs are correct; two budget/context notes — **LOW**

Both IDs are exact current API identifiers (correct hyphenation, no date suffixes — date-suffixed variants would be wrong):
1. **`claude-sonnet-5` intro pricing ($2/$10 per MTok) expires 2026-08-31**, reverting to $3/$15. Any AI cost modeling done during planning should use the post-intro price.
2. **`claude-haiku-4-5` has a 200K context window** (vs Sonnet 5's 1M). Fine for classification per AD-10, but the LLM gateway should enforce that large-context tasks (e.g., whole-thread comms categorization with history) route to Sonnet, not Haiku.

Also worth noting: current Anthropic API guidance for Claude Sonnet 5 is adaptive thinking (`thinking: {type: "adaptive"}`) with no sampling params (`temperature` etc. rejected) — relevant when the gateway (AD-10) is implemented via AI SDK 6's Anthropic provider; don't copy older `budget_tokens`/temperature patterns from pre-2026 examples.

### Finding 5 — Inngest: confirmed fit for the stated role — **no issue**

Verified against current Inngest docs/site: (a) first-class Vercel integration — functions deploy inside the Next.js app, auto-registration on deploy, no separate workers, matching the spine's "Inngest serve route" in `apps/web`; (b) `step.waitForEvent()` suspends at zero cost with configurable timeouts — exactly what AD-4/AD-5 use for `approval.resolved` human-in-the-loop pauses; (c) concurrency control with a `key` expression (e.g., per-`organizationId`) is a current, documented feature — matching AD-4's "keyed by organizationId for concurrency." Durable execution survives serverless restarts, which is the precise failure mode AD-4 exists to prevent. No changes needed.

### Finding 6 — Minor row hygiene — **INFO**

- The Stack header "*Seed — verified current July 2026*" is now accurate for every row **except** NativeWind (Finding 2); fix that one annotation and the claim holds.
- Rows pinned as "latest stable" (TypeScript, Turborepo/pnpm, Drizzle, Zod, Inngest, Vitest/Playwright) are appropriately unpinned — code lockfiles will own exact versions. No action.
- Expo 56's TypeScript 6.0.3 bump: the monorepo-wide "TypeScript latest stable" pin should be checked for web↔mobile TS version compatibility at Phase 2 (Next.js 16.2 tooling vs Expo 56 tooling) — a Turborepo per-app tsconfig concern, not a spine change.

---

## Sources

- [Next.js EOL/version tracker — 16.2.10 LTS, July 2026](https://eosl.date/eol/product/nextjs/) · [Next.js 16.2 release notes](https://nextjs.org/blog/next-16-2) · [endoflife.date/nextjs](https://endoflife.date/nextjs)
- [Expo SDK 56 changelog](https://expo.dev/changelog/sdk-56) · [Expo SDK 56 page](https://expo.dev/sdk/56)
- [tRPC releases (11.16.0 current)](https://github.com/trpc/trpc/releases) · [@trpc/server versions on npm](https://www.npmjs.com/package/@trpc/server?activeTab=versions)
- [Drizzle ORM RLS docs](https://orm.drizzle.team/docs/rls) · [drizzle-orm 0.36.0 release (RLS)](https://github.com/drizzle-team/drizzle-orm/releases/tag/0.36.0)
- [Supabase self-hosted PG 15→17 changelog (June 2026)](https://supabase.com/changelog/46080-self-hosted-supabase-upgrading-from-pg-15-to-17-breaking-change) · [Supabase PG17 upgrade docs](https://supabase.com/docs/guides/self-hosting/postgres-upgrade-17)
- `@clerk/expo` live npm registry metadata (v3.6.5, `expo: ">=53 <57"`) · [Clerk changelog — Core 3 / native components (2026-03-09)](https://clerk.com/changelog/2026-03-09-expo-native-components) · [Clerk Expo 54/55 compat article](https://clerk.com/articles/clerk-compatibility-in-expo-54-and-55)
- [Inngest — durable workflows](https://www.inngest.com/uses/durable-workflows) · [Inngest GitHub](https://github.com/inngest/inngest)
- [AI SDK 6 announcement (Vercel)](https://vercel.com/blog/ai-sdk-6) · [vercel/ai releases](https://github.com/vercel/ai/releases)
- [NativeWind v5 docs (pre-release notice)](https://www.nativewind.dev/v5) · [NativeWind v5 migration guide](https://www.nativewind.dev/blog/v5-migration-guide)
- Anthropic API model catalog (current model IDs + pricing, cached 2026-06)
