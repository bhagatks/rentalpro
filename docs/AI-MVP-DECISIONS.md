# AI MVP Decisions — Texas

**Status:** in progress — **screening parked (TBD)**  
**Last updated:** 2026-07-05  
**Scope:** AI agent behavior only (leasing, maintenance, accounting). Infra/hosting in separate notes.

---

## Locked ✅

| Area | Decision |
|------|----------|
| **Jurisdiction** | Texas only for MVP (federal FHA/FCRA + Texas Property Code); expand state-by-state after Texas validated |
| **Communication** | Chat, SMS, email — **no voice** in MVP |
| **Leasing — document** | **A + B:** PM uploads attorney-reviewed template **and** platform ships Texas starter template |
| **Leasing — flow** | AI fills merge fields + required addenda → PM admin reviews/edits → e-sign (Basic always reviews; Pro auto-send if draft matches template exactly) |
| **Screening — sources** | **A + B:** integrated screening API **and** PM connects own vendor — **plus** build native industry-breaking layer (thesis TBD) |
| **Maintenance scope** | Full lifecycle: report → diagnose → vendor quote → schedule → verify → pay |
| **Maintenance — emergency** | Auto-dispatch on fixed emergency list (bypasses spend approval on both plans) |
| **Governance — Pro plan** | Auto-approve maintenance up to **$500** (PM configurable) |
| **Accounting** | AI auto-categorizes 100%; human monthly sign-off before distributions (from HANDOFF) |

### Emergency list (locked)

Agent auto-dispatches without spend approval when classified as:

- Gas leak
- Sewage backup
- Active flooding
- Exposed live wiring
- No heat when outdoor temp below 40°F
- Total loss of water service

---

## Parked — Screening (decide later) 🅿️

Screening research and industry-breaking thesis **paused**. Revisit before CAP-2 micro-spec.

### Locked within screening (partial)

| Item | Status |
|------|--------|
| Dual source (integrated API + PM vendor) | ✅ Locked |
| Native decision layer (industry-breaking) | ⏳ Thesis TBD |
| Texas §92.3515 criteria notice before application fee | ✅ Required |
| FCRA standalone consent before credit pull | ✅ Required |
| Adverse action with **specific** denial reason | ✅ Required (recommended default) |
| Criminal history: HUD individualized assessment (not blanket ban) | ✅ Recommended default |
| Credit score default | ⏳ TBD (620 vs 650 vs PM-only) |

### TBD — Screening criteria (PM configures; agent applies)

| Criterion | Options | Recommendation (not locked) |
|-----------|---------|-------------------------------|
| **Income** | 1: 3× rent · 2: 2.5× rent · 3: PM sets per property | **3 with platform default 3×** |
| **Eviction** | 1: Deny if within 3 years · 2: Deny if any ever · 3: Case-by-case | **1 on Basic, 3 on Pro** |
| **Credit minimum** | 620 · 650 · none | 620 default |
| **Criminal** | HUD individualized · blanket ban · no check | HUD individualized only |

### TBD — Industry-breaking screening thesis

Pick one primary direction when resuming:

| # | Direction | Summary |
|---|-----------|---------|
| **1** | Transparent Decision Engine | Every approve/deny gets a Decision Card: criteria version, inputs, rule failed, adverse action draft |
| **2** | Portable Applicant Passport | Pay-once screening profile reusable across PM companies on platform (90 days) — Phase 2 complexity |
| **3** | Criteria-First Autonomous Screener | RentalPro owns decision layer; vendors are interchangeable report providers |
| **1+3** | Combined (recommended when resuming) | Autonomous + auditable decision on top of any report source (A or B) |

### TBD — Screening integrations

| Item | Options |
|------|---------|
| Primary integrated API vendor | TransUnion SmartMove · RentPrep · other — not selected |
| PM custom vendor connection model | API key · manual upload · webhook — not designed |
| Screening fee handling | Pass-through to applicant · PM absorbs · platform fee — not decided |

---

## TBD — Other AI topics

### Leasing agent

| Item | Status |
|------|--------|
| E-sign provider | TBD (DocuSign · HelloSign · built-in) |
| Identity verification provider | TBD (Stripe Identity locked in HANDOFF direction) |
| Starter template legal review | TBD — need Texas attorney review before ship |
| Pro plan auto-send conditions | TBD — exact rules when draft "matches template exactly" |

### Maintenance agent

| Item | Status |
|------|--------|
| Emergency spend cap on Pro | TBD (suggested $1,000 for emergency auto-pay) |
| Vendor outreach default | TBD (recommended: SMS/email first, portal for quote upload) |
| Cosmetic vs routine classification | TBD — agent triage rules |
| Rent-delinquency block on dispatch | TBD (recommended: yes, escalate to PM) |

### Accounting agent

| Item | Status |
|------|--------|
| Texas TREC trust account rules in agent | TBD — deposit within 2 business days |
| Security deposit tracking | TBD — separate ledger sub-account |
| Plaid vs Stripe-only feeds | TBD (HANDOFF lists both) |

### Cross-cutting

| Item | Status |
|------|--------|
| CAP-10 audit trail micro-spec | Draft — `docs/capabilities/CAP-10-audit-trail.md` |
| CAP-5 governance rails micro-spec | Draft — `docs/capabilities/CAP-5-governance-rails.md` |
| Basic vs Pro per-module matrix | Partially locked in HANDOFF; detail TBD |
| Agent orchestration (Inngest etc.) | Parked with infra discussion |
| AWS/GCP migration | Parked with infra discussion |

---

## Texas compliance checklist (reference)

Required for leasing agent when Texas MVP ships:

- [ ] §92.3515 tenant selection criteria notice **before** application fee + signed acknowledgment
- [ ] FCRA standalone written consent before consumer report
- [ ] Adverse action notice with specific reasons if denied
- [ ] Flood disclosure — **both landlord and tenant sign** (SB 2349, effective 2025)
- [ ] Lead-based paint disclosure if built before 1978
- [ ] Owner/agent identity in lease (§92.201)
- [ ] Tenant repair remedies in bold/underlined (§92.056)
- [ ] FHA protected classes excluded from all agent inputs and criteria

---

## Session log

| Date | Event |
|------|-------|
| 2026-07-05 | AI MVP Texas research session; screening parked; this doc created |
| 2026-07-05 | Locked: lease A+B, screening A+B, emergency dispatch, $500 Pro limit, chat-only, Texas-only |

---

## When resuming

Say in chat:

- **"resume screening"** — pick criteria defaults + industry-breaking thesis (1, 2, 3, or 1+3)
- **"walk one lead"** — story-form leasing agent walkthrough
- **"walk one work order"** — story-form maintenance agent walkthrough
- **"micro CAP-10"** — audit trail micro-spec

Canonical machine log: `_bmad-output/specs/spec-rentalpro/.memlog.md`
