# Syndication MVP Runbook — M1 Full MVP

**Created:** 2026-07-05  
**Owner:** Founder + eng lead  
**Status:** active — time-sensitive parallel tracks  
**SPEC:** M1 locked Full MVP · CAP-2 sub-feature · Listing Package + Syndication Hub

> **One doc for everything time-sensitive AND full build requirements.** External partner gates, build tasks, legal checks, user stories, flows, and acceptance criteria — so nothing from spec sessions is lost at story time.

---

## Full requirements (read this when building M1)

### 1. Problem & rationale

PM companies manually re-enter unit data on Zillow, Apartments.com, and dozens of ILS sites. Competitors (Buildium, DoorLoop, TurboTenant) offer "one-click syndication" but still require humans to fill listing forms and click publish. RentalPro's APM thesis requires **vacant → listed → inquiry → lease → key** with minimal staff — syndication is the **front door** to CAP-2 autonomous leasing, not a bolt-on marketing feature.

**Founder vision (locked):** SaaS-native **Listing Package** — one unified form pre-filled from portfolio (CAP-1), all info needed for every ILS, **one Submit publishes everywhere**. Not copying legacy "listing form" UX; pre-populate from data you already have.

### 2. Product vision

```
Portfolio (CAP-1) → Listing Package (auto-built) → One Submit → Syndication Hub → all ILS
                                                              ↓
                                                    Inbound leads → CAP-2 Leasing Agent
```

**Differentiator vs market:**

| Market (Buildium, DoorLoop, MagicDoor) | RentalPro |
|----------------------------------------|-----------|
| PM re-enters unit data into listing form | Pre-filled from CAP-1 portfolio |
| Human writes description | AI generates; PM approves (Basic) or auto if complete (Pro) |
| Human clicks publish per vacancy | Listing Agent triggers on `unit.status → vacant` |
| Leads sit in CRM inbox | Leasing Agent qualifies 24/7 |
| Single-tenant | White-label per PM org (CAP-11) |
| Syndication assists staff | Syndication feeds autonomous leasing loop |

### 3. User stories

| Actor | Story |
|-------|-------|
| PM admin | I review a pre-filled Listing Package (not a blank form), edit gaps, select syndication targets, and publish with one Submit. |
| PM admin | I see per-platform status (Zillow live, Apartments.com pending) and inquiry counts per source. |
| PM admin (Basic) | I approve Listing Package before it publishes externally. |
| PM admin (Pro) | Package auto-publishes when complete (photos + rent + description) without my click. |
| Listing Agent | When a unit goes vacant, I auto-build Listing Package from CAP-1 data and AI copy. |
| Prospect | I find the unit on Zillow or `{pmco}.rentalpro.ai/units/{id}` and chat with Leasing Agent 24/7. |
| Leasing Agent | Every inbound lead (any source) becomes a CAP-2 `Lead` with `source` attribution. |
| Platform ops | I see syndication errors per org and failed feed syncs. |

### 4. End-to-end flows

**Autonomous (Pro — package complete):**

1. Lease ends → `unit.status = vacant` (CAP-1).
2. Listing Agent builds Listing Package from unit/property/photos; AI writes headline + description.
3. Governance (CAP-5): Pro + complete checklist → auto-publish; else approval queue.
4. Syndication Hub fans out: org page (instant), MITS feed URL, Zillow API, Apartments.com (when approved).
5. Prospect inquires on any platform → normalized `Lead` → CAP-2 guest card.
6. Leasing Agent runs inquiry → apply → lease → CAP-12 key.
7. Lease signed → auto-unpublish all feeds (B9).

**Basic plan:** Steps 3–4 require PM admin approval before external publish.

**Edge paths:**

| Trigger | Behavior |
|---------|----------|
| Missing photos | Block external publish; org page optional with placeholder |
| Missing rent amount | Block publish; prompt PM |
| Zillow feed error | Retry; show Error in dashboard; org page still live |
| Apartments.com not approved yet | Publish to Zillow + org page only |
| Oregon property | Zillow third-party syndication blocked — flag PM (N/A Texas MVP) |
| Multifamily 4+ units on Apartments.com | May require PM paid advertising account — surface in UI |
| Lead webhook format mismatch | Log raw payload; alert ops; never lose lead |

### 5. Functional requirements

| ID | Requirement | MVP |
|----|-------------|-----|
| FR-M1-01 | Listing Package entity with schema below; pre-fill ≥80% fields from CAP-1 | ✅ |
| FR-M1-02 | Review UI: PM edits only gaps; not blank form | ✅ |
| FR-M1-03 | One Submit → Syndication Hub publishes to selected targets | ✅ |
| FR-M1-04 | MITS XML feed URL per org (canonical outbound format) | ✅ |
| FR-M1-05 | Org listing page `{slug}.rentalpro.ai/units/{id}` — no external approval | ✅ |
| FR-M1-06 | Zillow Rentals Feed integration (after A1–A5 approval) | ✅ |
| FR-M1-07 | Apartments.com vendor feed (after A6 partnership) | ✅ parallel |
| FR-M1-08 | Lead webhook normalizes all sources → CAP-2 `Lead` | ✅ |
| FR-M1-09 | Listing Agent auto-builds package on vacancy event | ✅ |
| FR-M1-10 | AI listing copy generation with fair housing review (C3) | ✅ |
| FR-M1-11 | Auto-unpublish all targets when unit leased | ✅ |
| FR-M1-12 | Syndication dashboard: status, last sync, inquiries per source | ✅ |
| FR-M1-13 | Basic: CAP-5 approval before publish | ✅ |
| FR-M1-14 | Pro: auto-publish when completeness checklist met | ✅ |
| FR-M1-15 | RentSync/Showdigs interim path if Zillow delayed (A8/A9) | Optional |

### 6. Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-M1-01 | All listing data scoped by `organizationId`; feeds never cross orgs |
| NFR-M1-02 | Every publish/unpublish logged in CAP-10 |
| NFR-M1-03 | Feed generation completes in <30s for 100 units |
| NFR-M1-04 | Lead webhook responds 200 within 5s; async processing |
| NFR-M1-05 | Photos served via Supabase Storage CDN URLs in feed |
| NFR-M1-06 | Idempotent publish — re-submit updates, does not duplicate listings |

### 7. Architecture & integrations

**Syndication Hub (internal):**

```
POST /api/orgs/current/listings/:unitId/publish
  → validate ListingPackage
  → for each target in syndication.targets:
       org_page     → render immediately
       mits_feed    → regenerate org feed XML
       zillow       → push via Rentals Feed API (post-approval)
       apartments_com → push via partner feed (post-approval)
  → update syndication.status per target
```

**Lead inbound:**

```
POST /api/webhooks/leads/zillow | /apartments | /org-form
  → normalize to Lead { source, unitId, name, email, phone, message, listingPackageId }
  → create CAP-2 guest card
  → trigger Leasing Agent
```

**Stack (from SPEC assumptions):**

| Layer | Tool | Role |
|-------|------|------|
| Trigger | Inngest / Supabase webhook | `unit.status → vacant` |
| Content AI | Claude/GPT | Listing copy |
| Photos | Supabase Storage | CDN URLs in MITS/Zillow payload |
| Outbound | MITS XML + Zillow Rentals Feed API | Syndication |
| Inbound | Webhook routes | Lead normalization |

**Competitor syndication reference:**

| Platform | Pattern |
|----------|---------|
| Buildium | One-click to Zillow Group, Apartments.com, Apartment List, Zumper |
| DoorLoop | Listing generator + AI description |
| MagicDoor | Single unit record → MITS → multi-ILS; auto-delist on off-market |
| TurboTenant | 50+ sites via syndication partner |
| Avail | One listing → 19 partner sites |

RentalPro uses **MITS as canonical** + direct Zillow + org page Day 1.

### 8. Acceptance criteria

- [ ] Unit goes vacant → Listing Package auto-created with CAP-1 data pre-filled
- [ ] PM admin sees review screen; one Submit publishes to org page + selected ILS
- [ ] MITS feed URL validates against spec; Zillow sandbox accepts feed (post A4)
- [ ] Inbound Zillow lead creates CAP-2 Lead with `source: zillow` within 5 min
- [ ] Inbound org page chat creates Lead with `source: org_page`
- [ ] Lease signed → all syndication targets show Delisted within next sync cycle
- [ ] Basic plan: publish blocked until PM approves in CAP-5 queue
- [ ] Pro plan: auto-publish when photos + rent + description present
- [ ] Fair housing: discriminatory listing copy blocked before publish
- [ ] Dashboard shows per-platform Live/Pending/Error + inquiry count
- [ ] Two orgs cannot see each other's listings (CAP-11 isolation)

### 9. External approvals (summary — detail in Track A below)

| Partner | Timeline | Blocker for |
|---------|----------|-------------|
| Zillow Rentals Feed | 1–2 wk apply + 4–6 wk test | Zillow/Trulia/HotPads |
| Zillow Lead API | Parallel with feed | Zillow inbound leads |
| Apartments.com vendor | Weeks–months | Apartments.com network |
| Org listing page | None | Day 1 |

---

## Critical path (at a glance)

```
DAY 1 ──► Submit Zillow application + email rentalfeeds@zillow.com
          Email feeds@apartments.com (vendor partnership)
          Contact RentSync/Showdigs (interim syndication)

WEEK 1–2 ► Build Listing Package schema + MITS feed + org listing pages
           Zillow application review (1–2 wk)

WEEK 2–3 ► Interim aggregator connected (optional accelerator)
           Lead webhook endpoint live

WEEK 4–8 ► Zillow sandbox feed testing (4–6 wk — BLOCKER for Zillow prod)

WEEK 8+  ► Zillow production go-live
           Apartments.com partnership (parallel — months)
```

**Day-1 fallback (no external approval needed):** Org listing pages at `{slug}.rentalpro.ai/units/{id}` + inbound → CAP-2 agent.

---

## Master task tracker

Update `Status`: `⬜ todo` · `🔄 in progress` · `✅ done` · `🅿️ blocked` · `❌ cancelled`

### Track A — External approvals (start Day 1)

| ID | Task | Contact / Link | Start by | Est. duration | Depends on | Status | Notes |
|----|------|----------------|----------|---------------|------------|--------|-------|
| A1 | Submit Zillow Rentals Feed partner application | [Zillow intake form](https://www.zillowgroup.com/developers/api/rentals/rentals-feed-integrations/) | **Day 1** | 1–2 wk review | — | ⬜ | B2B multi-tenant PMS; Texas SFR focus |
| A2 | Email Zillow for feed spec + Lead API docs | `rentalfeeds@zillow.com` | **Day 1** | 3–5 days | — | ⬜ | Request Rental Listings Feed Guide + MITS spec |
| A3 | Register Lead API webhook URL with Zillow | `rentalfeeds@zillow.com` | Week 2 | 1 wk | B4 webhook live | ⬜ | Fixed URL-encoded format; no field customization |
| A4 | Zillow sandbox feed testing | Zillow Integrations team | After A1 approved | **4–6 wk** | B2 MITS feed | ⬜ | Production-like data required |
| A5 | Zillow production go-live | Zillow Integrations team | After A4 pass | 1 wk | A4 | ⬜ | Listings → Zillow, Trulia, HotPads |
| A6 | Email Apartments.com vendor partnership | `feeds@apartments.com` + [Become a partner](https://www.apartments.com/grow/integrations) | **Day 1** | Weeks–months | — | ⬜ | No public API; must join partner list |
| A7 | Per-PM Apartments.com property verification | PM admin → `feeds@apartments.com` | After A6 | 24–48 hr/property | A6 approved | ⬜ | Include Property/Integration ID |
| A8 | RentSync interim syndication (optional) | [rentsync.com](https://rentsync.com/integration-partners) | Week 1 | 2–4 wk | B1 data file/API | ⬜ | Accelerator while A1–A5 run |
| A9 | Showdigs API partnership (optional alt) | [showdigs.com/integrations](https://www.showdigs.com/integrations) | Week 1 | 2–4 wk | B1 | ⬜ | Zillow + Apartments.com via API |
| A10 | Facebook Marketplace API (optional) | Meta Business app review | Phase 2 | 2–4 wk | — | ⬜ | Not MVP-critical |

### Track B — Build (parallel — no approval wait)

| ID | Task | Owner | Start by | Est. duration | Depends on | Status | Notes |
|----|------|-------|----------|---------------|------------|--------|-------|
| B1 | **Listing Package** schema (unified pre-info model) | Eng | Week 1 | 1 wk | CAP-1 unit data | ⬜ | See schema below |
| B2 | **MITS XML feed generator** (canonical outbound format) | Eng | Week 1 | 2 wk | B1 | ⬜ | Zillow accepts MITS; unlocks other ILS |
| B3 | **Listing review UI** — pre-filled form, one Submit | Eng | Week 2 | 1 wk | B1 | ⬜ | PM confirms/edits; not blank form |
| B4 | **Lead webhook** `POST /api/webhooks/leads/{source}` | Eng | Week 2 | 1 wk | — | ⬜ | Normalizes Zillow + org page → CAP-2 Lead |
| B5 | **Org listing pages** `{slug}.rentalpro.ai/units/{id}` | Eng | Week 1 | 1 wk | B1 | ⬜ | **No external approval** — ship first |
| B6 | **Syndication Hub** — fan-out + per-platform status | Eng | Week 3 | 2 wk | B2 | ⬜ | Status: Live / Pending / Error / Delisted |
| B7 | **Listing Agent** — auto-build package on vacancy | Eng | Week 3 | 1 wk | B1, CAP-1 event | ⬜ | Trigger: unit.status → vacant |
| B8 | **AI listing copy** — headline + description from unit data | Eng | Week 2 | 3 days | B1 | ⬜ | PM approves before publish (Basic) |
| B9 | **Auto-unpublish** on lease signed / unit leased | Eng | Week 4 | 2 days | B6, CAP-2 | ⬜ | Delist all feeds on next cycle |
| B10 | **Syndication dashboard** — inquiries per source | Eng | Week 4 | 1 wk | B4, B6 | ⬜ | PM admin view |

### Track C — Governance & compliance (internal)

| ID | Task | Owner | Start by | Status | Notes |
|----|------|-------|----------|--------|-------|
| C1 | Basic plan: PM approves Listing Package before publish | Product | Week 2 | ⬜ | CAP-5 approval queue |
| C2 | Pro plan: auto-publish if complete (photos + rent + description) | Product | Week 3 | ⬜ | Define "complete" checklist |
| C3 | Fair housing AI review on listing copy before publish | Eng | Week 3 | ⬜ | Block discriminatory language |
| C4 | Validate rent/deposit/fees accuracy in Listing Package | Eng | Week 2 | ⬜ | Required fields gate |
| C5 | Link §92.3515 criteria notice on application (not listing) | Eng | Week 2 | ⬜ | CAP-2 flow — already in AI-MVP-DECISIONS |

### Track D — Decision gates (founder)

| ID | Decision | Options | Status | Locked value |
|----|----------|---------|--------|--------------|
| D1 | M1 scope | MVP partial / Full MVP / Non-goal | ✅ | **Full MVP** — Listing Package + Syndication Hub |
| D2 | Interim aggregator | RentSync / Showdigs / None (Zillow only) | ⬜ | TBD — recommend RentSync if A1 slow |
| D3 | MITS version | MITS 4.x / MITS 5.0 fee transparency | ⬜ | Start 4.x; 5.0 for fee fields Phase 2 |
| D4 | Pro auto-publish rules | Exact checklist for "complete" package | ⬜ | TBD in CAP-2 lock |

---

## Listing Package schema (B1 reference)

Single source of truth — pre-filled from CAP-1; PM reviews once; one Submit publishes everywhere.

```
ListingPackage
├── unitId, organizationId, status (draft|pending_approval|published|leased)
├── marketing: { headline, description, photos[], tourUrl, virtualTourType }
├── pricing: { rent, deposit, fees[], availableDate, minLeaseTerm, maxOccupants }
├── unit: { beds, baths, sqft, floor, propertyType, address, amenities[] }
├── policies: { pets, smoking, parking, utilitiesIncluded[], hoaRules }
├── compliance: { floodZone, leadPaintPre1978, fairHousingDisclaimer }
├── showing: { contactMethod (agent_chat), applicationUrl, criteriaNoticeUrl }
└── syndication: {
      targets: [org_page, zillow, apartments_com, ...],
      status: { zillow: live|pending|error, ... },
      lastSyncedAt: {},
      feedUrl: string  // MITS XML URL for partner consumption
    }
```

---

## Day 1 action emails (copy-paste)

### A1 + A2 — Zillow

**To:** via intake form + `rentalfeeds@zillow.com`  
**Subject:** PMS Integration Request — RentalPro.ai B2B Multi-Tenant Platform

```
Company: RentalPro.ai
Product: B2B multi-tenant property management SaaS (Autonomous Property Management)
Target market: Independent PM companies, 10–500 units, Texas SFR/residential
Integration type: Rentals Feed (MITS or Zillow XML) + Lead API webhook

We are building a Listing Package → one-submit → multi-platform syndication hub.
Requesting:
1. Rentals Feed Integration partner application
2. Rental Listings Feed Guide + MITS format documentation
3. Lead API webhook registration process

Technical contact: [name, email]
Expected listing volume (Year 1): [X PM companies × Y units]
MVP jurisdiction: Texas (SFR, condos, townhomes, duplexes)

Happy to provide sample feed URL once MITS generator is built (Week 2).
```

### A6 — Apartments.com

**To:** `feeds@apartments.com`  
**Subject:** New PMS Vendor — Integration Partnership Inquiry (RentalPro.ai)

```
RentalPro.ai is a B2B multi-tenant property management platform launching in Texas.
We are implementing MITS-compliant listing feeds and seek vendor partnership for
Apartments.com network syndication (similar to AppFolio, Buildium, Rent Manager).

Product: Listing Package with one-submit multi-ILS publish
Feed format: MITS XML (URL per org)
Lead delivery: Webhook to our CAP-2 leasing CRM

Request: Vendor partnership application process and technical requirements.
Contact: [name, email, company]
```

### A8 — RentSync (interim)

**To:** RentSync via [integration partners page](https://rentsync.com/integration-partners)  
**Subject:** New PMS Integration — Interim Syndication While Zillow Direct Approval Pending

```
Building RentalPro.ai (multi-tenant PMS). Seeking interim listing syndication via
RentSync while we complete direct Zillow feed certification (4–6 wk process).

Can provide: routinely updated MITS/data file or API feed per org.
Target: Zillow network + major ILS until direct integrations live.
Volume: [estimate]
```

---

## Weekly checkpoint

| Week | External (Track A) | Build (Track B) | Gate |
|------|-------------------|-----------------|------|
| **1** | A1, A2, A6, A8 submitted | B1, B2, B5 started | Org listing pages live? |
| **2** | A1 review; A3 prep | B3, B4, B8 | Lead webhook receiving test leads? |
| **3** | A4 sandbox begins (if approved) | B6, B7 | MITS feed URL testable? |
| **4** | A4 testing continues | B9, B10 | End-to-end: vacant → publish → lead → CAP-2? |
| **5–8** | A4 sandbox | Polish + PM pilot | Zillow test pass? |
| **8+** | A5 production | Apartments.com if A6 approved | Full MVP syndication live |

---

## Blockers & fallbacks

| Blocker | Fallback | User impact |
|---------|----------|-------------|
| Zillow approval delayed | RentSync/Showdigs interim (A8) | Syndication works; extra cost |
| Zillow sandbox fails | Fix feed; re-test | 1–2 wk delay |
| Apartments.com partnership slow | Zillow + org page only | PM posts org link manually to other sites |
| No aggregator, no Zillow | Org listing page only (B5) | Leasing agent works; limited reach |
| Oregon properties | Direct Zillow agreement per PM | N/A for Texas MVP |

---

## Related artifacts

| Doc | Purpose |
|-----|---------|
| [`CAP-2-autonomous-leasing.md`](./capabilities/CAP-2-autonomous-leasing.md) | Leasing CAP; M1 sub-feature |
| [`MARKET-GAP-CHECKLIST.md`](./MARKET-GAP-CHECKLIST.md) | M1 assignment |
| [`AI-MVP-DECISIONS.md`](./AI-MVP-DECISIONS.md) | Leasing agent behavior |
| [`SPEC.md`](../_bmad-output/specs/spec-rentalpro/SPEC.md) | M1 locked Full MVP |
| [`.memlog.md`](../_bmad-output/specs/spec-rentalpro/.memlog.md) | Decision log |

---

## How to maintain this doc

1. **Weekly:** Update Status column in master tracker  
2. **On decision:** Append to `.memlog.md`, update Track D  
3. **On CAP-2 lock:** Move completed B-items to CAP-2 acceptance tests  
4. **When Zillow/Apartments.com approve:** Record partner IDs and spec version here  
5. **Do not duplicate** — this is the single time-sensitive task source; link from HANDOFF and README
