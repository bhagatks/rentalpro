# Syndication MVP Runbook — M1 Full MVP

**Created:** 2026-07-05  
**Owner:** Founder + eng lead  
**Status:** active — time-sensitive parallel tracks  
**SPEC:** M1 locked Full MVP · CAP-2 sub-feature · Listing Package + Syndication Hub

> **One doc for everything time-sensitive.** External partner gates, build tasks, and legal checks run in parallel. Update the Status column as you go; append decisions to `.memlog.md`.

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
