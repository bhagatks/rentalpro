# Mobile App — Phase 2 Gap Discussion

**Status:** discussion draft (not locked)  
**Audience:** founders / partners  
**Created:** 2026-07-24  
**Purpose:** One place to review what is already decided about mobile, what MVP web design implies, and **which gaps need a guided discussion** before Phase 2 (or before reopening the Non-goal).

> **Locked posture today:** Native iOS/Android apps are a **SPEC Non-goal for MVP** — responsive web only. Architecture is already **API-first multi-client** so mobile can plug in later without a rewrite.

---

## 1. What already exists (design / architecture)

### Spec & product

| Source | Decision |
|--------|----------|
| `SPEC.md` §Non-goals | Native iOS/Android → **Phase 2**; MVP = responsive web |
| Product brief / HANDOFF | Same — native apps out of MVP scope |
| CAP-7 open Q | "PWA install prompt — Phase 2?" still unchecked |
| P0 pilot | Validates **resident web portal** adoption — not a native app |

### Architecture (already designed for a later mobile client)

| Source | Decision |
|--------|----------|
| Architecture spine AD-1 / AD-3 | Business logic in `packages/core`; **one tRPC API** for web *and* future mobile |
| Structural seed | `apps/mobile` (Expo) scaffold planned; ships Phase 2 |
| Stack pin | Expo SDK 56, Clerk Expo, NativeWind (v5 caveat), EAS Build/Submit + OTA |
| Deferred table | Mobile UI build-out, EAS detail, **push notifications** deferred |
| Arch memlog | "Mobile is Phase 2 UI but architecture is API-first multi-client from Day 1" |
| `ARCHITECTURE-OVERVIEW.md` | Explains why web is a thin shell so Expo can share the same contract |

### Surfaces designed for MVP (web only)

| Surface | Actor | MVP home | Notes |
|---------|-------|----------|-------|
| PM admin / staff dashboard | PM | `apps/web` | Approvals, inbox (M7), portfolio, governance |
| Resident portal | Resident | `apps/web` on `{slug}.rentalpro.ai` | Rent pay, maintenance + media, lease docs (CAP-7) |
| Owner portal | Owner | `apps/web` on org subdomain | Statements (CAP-8); M8 UX still **open** |
| Vendor | Vendor | SMS/email MVP | Vendor **portal** still TBD (not mobile either) |
| Prospect / leasing | Prospect | Public web flows | Inquiry / application (CAP-2) |

There is **no** Figma/UX scenario pack, screen inventory, or `bmad-ux` deliverable for mobile in this repo yet. "Existing design" = **architecture + CAP web portals**, not a visual mobile design system.

---

## 2. Competitive signal (why partners will ask)

From `competitive-matrix.md` (not a mobile deep-dive):

- **AppFolio** called out as mobile-first in mid-market.
- **RentRedi** positioned as mobile-first landlords.
- **Rent Manager** notes a mobile tech (vendor/tech) app.
- Landlord-tier tools often win on phone UX for rent + maintenance.

Implication: residents and field vendors expect phone-native patterns (camera, push, home-screen). MVP bets that **responsive web + SMS/email** is enough to prove autonomy; Phase 2 native is the catch-up surface.

---

## 3. Gap inventory — discuss these

Each row is a decision partners should lock (or park) before investing in mobile. **None of these reopen MVP Non-goals unless you explicitly choose Option C below.**

### A. Product scope & timing

| # | Gap | Options to discuss | Current default |
|---|-----|--------------------|-----------------|
| A1 | When does native ship relative to full MVP? | After P0 only · After full 12-CAP MVP · Never (PWA forever) | After MVP (Phase 2) |
| A2 | Reopen Non-goal for a thin "MVP+" resident app? | Keep web-only MVP · Add Expo resident MVP in parallel · PWA-first bridge | Keep web-only |
| A3 | Who gets a native app first? | Resident · Owner · PM admin · Vendor/tech · All | **Not decided** (biggest gap) |
| A4 | One app vs three? | Single multi-role app · Separate resident / owner / PM apps · White-label per PM | **Not decided** |
| A5 | White-label / store listing | RentalPro-branded shell · Per-PM custom apps (expensive) · WebView wrapper | Subdomain branding on web; store strategy **blank** |

### B. MVP web → mobile feature parity

| # | Gap | Why it matters | Discuss |
|---|-----|----------------|---------|
| B1 | Resident: rent pay + maintenance media | P0 success depends on portal adoption; phone camera UX on mobile web is weaker than native | Is responsive web enough for P0, or do we need a PWA / early Expo for media? |
| B2 | Push notifications | Spine defers push; CAP-7 "status within 5 min" today = chat/SMS/email + portal poll | Push as Phase 2 requirement vs SMS-only forever for MVP loops? |
| B3 | Offline / flaky network | Field vendors + residents in dead zones | Out of scope forever, or Phase 2 offline queue for maintenance photos? |
| B4 | Smart lock / digital key (CAP-12) | Seam deep links / wallet keys often mobile-native | Web "open lock" vs Wallet / Seam mobile SDK? |
| B5 | Owner approvals (M3 / M8) | Owners approve rent increases on phone | Web-first enough, or owner app is Phase 2 priority #1? |
| B6 | PM admin on mobile | Approvals on Basic plan are latency-sensitive | Mobile web for approvals vs native PM app? |
| B7 | Vendor / tech app | Competitors ship tech apps; our MVP is SMS/email | Phase 2 vendor app vs keep messaging-only? |

### C. Platform & engineering gaps

| # | Gap | What exists | What's missing |
|---|-----|-------------|----------------|
| C1 | Monorepo scaffold | Spine says `apps/mobile` "scaffold exists" | Confirm whether repo actually has `apps/mobile` yet (code not started → scaffold may be **aspirational**) |
| C2 | Shared UI tokens | `packages/ui` shared tokens planned | No token package / NativeWind decision locked for production (v5 pre-release caveat) |
| C3 | Auth on device | Clerk Expo in stack table | Org/subdomain tenancy on native (no subdomain bar) — deep-link org resolution **undesigned** |
| C4 | Branding on native | CAP-11 subdomain + portal CSS | App icon, splash, in-app theme per org — store vs runtime branding **undesigned** |
| C5 | Realtime | AD-3: polling MVP; Realtime only behind API | Mobile freshness vs web — avoid side-channel drift (adversarial review F-11) |
| C6 | File upload | AD-16 signed URLs via tRPC | Camera roll / large video upload UX + background upload on RN **undesigned** |
| C7 | EAS / store ops | Deferred | Apple/Google accounts, privacy nutrition labels, TX PM data disclosures |
| C8 | Test strategy | Playwright for web | Detox/Maestro plan for mobile — none |

### D. UX / design gaps (no mobile design yet)

| # | Gap | Notes |
|---|-----|-------|
| D1 | No mobile screen inventory | No resident/owner/PM IA for native |
| D2 | No `bmad-ux` / WDS scenario pack for mobile | Web portals themselves are CAP-level, not high-fidelity UX |
| D3 | Accessibility & platform patterns | iOS HIG / Material — not started |
| D4 | PWA vs native trade study | CAP-7 asks; no decision matrix written until this doc |
| D5 | M8 owner portal UX open | Even **web** owner UX is TBD — mobile owner app would amplify that gap |

### E. Business / GTM gaps

| # | Gap | Discuss |
|---|-----|---------|
| E1 | Does Basic vs Pro change mobile access? | Feature-gate native? |
| E2 | App Store fees / who pays | RentalPro vs pass-through |
| E3 | Pilot narrative | If P0 residents won't use mobile web, do we fail the assumption or accelerate native? |
| E4 | Competitive parity claim | "Mobile-first" in marketing vs Phase 2 reality — messaging risk |

---

## 4. Recommended discussion agenda (partner session)

Use this order — 45–60 minutes:

1. **Keep Non-goal?** Confirm native stays Phase 2 (default) vs reopen for resident-only thin app.
2. **Priority persona** — pick A3 (resident vs owner vs PM). Recommendation to pressure-test: **resident first** (feeds CAP-2/3/7 autonomy), then owner approvals, PM last (desktop-heavy).
3. **PWA bridge** — yes/no for MVP as installable home-screen + limited offline, without App Store.
4. **Push** — SMS remains primary for MVP; push is Phase 2 hard requirement or nice-to-have?
5. **Tenancy on native** — how org is selected without subdomain (invite deep link vs PM picker vs separate apps).
6. **Vendor app** — park as Phase 2.5 unless a pilot PM demands it.
7. **Success signal for Phase 2 mobile** — e.g. "≥X% of pilot residents complete rent + maintenance from native within 14 days."

### Suggested default (for debate, not locked)

| Topic | Straw proposal |
|-------|----------------|
| Timing | Native after P0 go + CAP-7 web proven; not before |
| First app | Resident (Expo), RentalPro-branded with runtime PM theme |
| Bridge | Optional PWA install prompt on CAP-7 web (low cost) |
| Push | Phase 2 with Expo notifications; MVP keeps SMS/email |
| Owner/PM native | Phase 2.1 after resident retention metrics |
| Do not build | Per-PM App Store listings in v1 |

---

## 5. What MVP must still get right (so mobile is cheap later)

These are **not** mobile scope — they are web/MVP guardrails so Phase 2 is extraction, not rewrite:

- [ ] Keep all client data access on `packages/api` (AD-3) — no Supabase client queries from UI
- [ ] Shared design tokens in `packages/ui` from first web components
- [ ] Maintenance media upload only via AD-16 signed URL flow
- [ ] Comms status updates work on SMS/email so mobile push is additive, not required for autonomy
- [ ] Document org-resolution approach for non-subdomain clients in CAP-11 when mobile kicks off

---

## 6. Outcomes to lock after discussion

Record answers here, then append `.memlog.md` and update SPEC Non-goals only if reopened.

| Decision | Owner pick | Date |
|----------|------------|------|
| Keep native as Phase 2 Non-goal? | _TBD_ | |
| First native persona | _TBD_ | |
| PWA in MVP? | _TBD_ | |
| Push notifications Phase 2 priority | _TBD_ | |
| Org resolution on native | _TBD_ | |
| Phase 2 success signal | _TBD_ | |

---

## 7. Related docs

| Doc | Relevance |
|-----|-----------|
| [`SPEC.md`](../_bmad-output/specs/spec-rentalpro/SPEC.md) §Non-goals | Locked "no native apps in MVP" |
| [`ARCHITECTURE-OVERVIEW.md`](./ARCHITECTURE-OVERVIEW.md) | Why multi-client from Day 1 |
| [`ARCHITECTURE-SPINE.md`](../_bmad-output/planning-artifacts/architecture/architecture-rentalpro-2026-07-05/ARCHITECTURE-SPINE.md) | Expo stack, deferred mobile UI/push |
| [`CAP-7-resident-portal.md`](./capabilities/CAP-7-resident-portal.md) | Resident web MVP + PWA open Q |
| [`CAP-8-owner-reporting.md`](./capabilities/CAP-8-owner-reporting.md) | Owner web; M8 open |
| [`CAP-11-multi-tenant-saas.md`](./capabilities/CAP-11-multi-tenant-saas.md) | Subdomain branding (web-shaped) |
| [`PILOT-P0-ASSUMPTIONS-VALIDATION.md`](./PILOT-P0-ASSUMPTIONS-VALIDATION.md) | Resident portal adoption gate |
| [`UNIFIED-COMMS-HUB-MVP-REQ.md`](./UNIFIED-COMMS-HUB-MVP-REQ.md) | SMS/email as MVP notification spine |
| [`MARKET-GAP-CHECKLIST.md`](./MARKET-GAP-CHECKLIST.md) | M8 owner portal UX still open |

---

## Decisions log

| Date | Note |
|------|------|
| 2026-07-24 | Discussion doc created from existing SPEC Non-goal + architecture spine/overview + CAP-7/8/11; no Non-goal change |
