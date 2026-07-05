# Texas Attorney Review — Master Checklist

**Last updated:** 2026-07-05  
**Purpose:** Single checklist for all Texas RE attorney reviews before production. Domain detail lives in linked M-docs — **check items here, sign off once per area.**

> **Not legal advice.** Product architecture only. Engage licensed Texas real estate attorney before ship.

---

## Tag legend

| Tag | Meaning |
|-----|---------|
| **Mandatory (prod gate)** | Attorney sign-off **required** before enabling this in production. Engineering may build in sandbox/dev with placeholder copy until approved. |
| **Optional (recommended)** | Attorney review **recommended** for quality and risk reduction; **not** a standalone prod blocker unless noted. |
| **Conditional** | Required **only when** the stated condition applies (e.g. pre-1978 property, screening feature enabled). |
| **Deferred (Phase 2)** | Not MVP scope. Becomes **Mandatory (prod gate)** if/when that feature ships (see M6). |

**Prod gate rule:** Do not enable autonomous production behavior for a feature until all **Mandatory** items for that feature's section are ✅.

---

## How to use

1. Schedule **one attorney engagement** for Texas MVP — bring this doc + linked briefs  
2. Check off each section as attorney approves  
3. Record sign-off in **Sign-off log** at bottom + append `.memlog.md`  
4. Filter by tag: ship blockers = all **Mandatory (prod gate)** items for features going live

---

## Master status

| Area | Doc | Mandatory items | Status |
|------|-----|-----------------|--------|
| **Leasing & screening** | [`AI-MVP-DECISIONS.md`](./AI-MVP-DECISIONS.md) | Yes — lease + screening when shipped | ⬜ |
| **M2 Delinquency** | [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) Runbook P1–P5 | **Yes — prod blocker** | ⬜ |
| **M3 Renewals** | [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) P1 | Yes — prod copy | ⬜ |
| **M4 Inspections** | [`MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md`](./MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md) P1 | Yes — deduction copy | ⬜ |
| **M5 Deposits** | [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) P1 | **Yes — prod blocker** | ⬜ |
| **M6 Eviction** | [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) | Deferred MVP | — |
| **M1 Syndication** | [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) Track C | Mixed — see §7 | ⬜ |
| **Cross-cutting** | SPEC Constraints, CAP-10 | Yes — platform letter + notices | ⬜ |

**Partner review (separate from attorney):** [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) P0 (M5), [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) partner checklist (M6 if reopened)

---

## 1. Leasing & screening (CAP-2)

*Sources: `AI-MVP-DECISIONS.md`, `capabilities/CAP-2-autonomous-leasing.md`, SPEC Constraints*

### Application & screening (Mandatory when screening ships)

- [ ] **[Mandatory (prod gate)]** §92.3515 tenant selection criteria notice **before** application fee + signed acknowledgment
- [ ] **[Mandatory (prod gate)]** FCRA standalone written consent before consumer report (credit/background)
- [ ] **[Mandatory (prod gate)]** Adverse action notice with **specific** denial reasons (not generic)
- [ ] **[Mandatory (prod gate)]** FHA: protected classes excluded from all agent inputs, criteria, and decision logic
- [ ] **[Optional (recommended)]** Criminal history: HUD individualized assessment default (not blanket ban) — confirm approach when screening resumes
- [ ] **[Optional (recommended)]** Screening criteria defaults (income, eviction, credit minimums) — confirm PM-configurable bounds when screening resumes
- [ ] **[Optional (recommended)]** Dual screening source model (integrated API + PM vendor) — liability/disclosure sufficient?

### Lease documents & addenda

- [ ] **[Mandatory (prod gate)]** **Platform Texas starter lease template** — attorney review before ship
- [ ] **[Mandatory (prod gate)]** Owner/agent identity in lease (§92.201)
- [ ] **[Mandatory (prod gate)]** Tenant repair remedies in **bold/underlined** (§92.056)
- [ ] **[Mandatory (prod gate)]** Flood disclosure — **both landlord and tenant sign** (SB 2349, effective 2025)
- [ ] **[Conditional — Mandatory when applicable]** Lead-based paint disclosure if built before 1978
- [ ] **[Optional (recommended)]** PM-uploaded lease template flow — platform disclaimer sufficient? (PM retains responsibility for template)
- [ ] **[Optional (recommended)]** Pro plan auto-send when draft "matches template exactly" — legally equivalent to PM manual send?

### Leasing flow links (M1 → CAP-2)

- [ ] **[Mandatory (prod gate)]** Application path from listing includes §92.3515 criteria notice (not on listing page itself — confirm flow)
- [ ] **[Optional (recommended)]** E-sign provider choice — Texas UETA / ESIGN compliance for residential leases

---

## 2. M2 — Delinquency & late fees

*Source: `DELINQUENCY-RULES-ENGINE.md` — **prod blocker***

### StateRulePack-TX (all rules)

- [ ] **[Mandatory (prod gate)]** Written attorney sign-off on `StateRulePack-TX` version (Runbook P5)
- [ ] **[Mandatory (prod gate)]** `TX-LF-001` — Late fee only if in **written lease**
- [ ] **[Mandatory (prod gate)]** `TX-LF-002` — Minimum **2 full days** after due date before fee
- [ ] **[Mandatory (prod gate)]** `TX-LF-003` — Cap **12%** monthly rent if structure ≤4 units
- [ ] **[Mandatory (prod gate)]** `TX-LF-004` — Cap **10%** monthly rent if structure >4 units
- [ ] **[Mandatory (prod gate)]** `TX-LF-005` — Initial + daily fees **combined** ≤ cap
- [ ] **[Mandatory (prod gate)]** `TX-LF-006` — Fee above safe harbor: block auto-assess; PM manual + legal flag only
- [ ] **[Optional (recommended)]** `TX-LF-007` — Violation penalty disclosure copy ($100 + 3× fee + attorney fees) shown when fee blocked
- [ ] **[Mandatory (prod gate)]** `TX-EV-001` — **Notice to Pay Rent or Vacate** — 2026 first delinquency in lease term (wording + timing)
- [ ] **[Mandatory (prod gate)]** `TX-PAY-001` — Must accept cash if lease doesn't specify payment method (no forced online-only)
- [ ] **[Mandatory (prod gate)]** §92.019 summary: 2-day grace, 10%/12% caps, combined initial+daily fees — matches RulePack

### Notices & templates

- [ ] **[Mandatory (prod gate)]** Reminder templates: friendly, late fee assessed, formal notice — attorney-approved wording (Runbook P3)
- [ ] **[Mandatory (prod gate)]** Auto-send blocked until templates approved (Runbook P3)
- [ ] **[Mandatory (prod gate)]** Confirm 2026 first-delinquency Notice to Pay requirements (Runbook P4)
- [ ] **[Mandatory (prod gate)]** **Delivery method** for delinquency notices: email/SMS legally sufficient or certified mail required?
- [ ] **[Mandatory (prod gate)]** Cure period length in Notice to Pay template matches current TX law
- [ ] **[Mandatory (prod gate)]** Fair Housing: notice content excludes discriminatory language
- [ ] **[Mandatory (prod gate)]** NSF / bounced check fee limits for TX residential (RulePack org policy cap)

### Org policy & agent behavior

- [ ] **[Mandatory (prod gate)]** Payment plan offers must **not waive** tenant legal rights (RulePack constraint)
- [ ] **[Optional (recommended)]** Block maintenance dispatch when delinquent — any legal restriction on PM practice?
- [ ] **[Optional (recommended)]** Waive late fee audit trail — sufficient for disputes?

**Brief to send (P2):** RulePack JSON, 3-layer model, sample policies (8% valid, 15% blocked, 1-day grace blocked), notice drafts, NSF question, 2026 Notice to Pay question

---

## 3. M3 — Lease renewals

*Source: `LEASE-RENEWAL-MVP-REQ.md` P1*

- [ ] **[Mandatory (prod gate)]** Rent increase notice timing — fixed-term vs month-to-month (TX)
- [ ] **[Mandatory (prod gate)]** Non-renewal / move-out notice timing when resident declines or PM elects non-renewal
- [ ] **[Mandatory (prod gate)]** Owner approval on increases above threshold — legally sufficient? (Owner = platform `owner` role; PM = agent)
- [ ] **[Mandatory (prod gate)]** Renewal lease = amendment vs new lease — template approach OK?
- [ ] **[Mandatory (prod gate)]** §92.201 owner/agent identity on renewal document
- [ ] **[Optional (recommended)]** Month-to-month conversion branch — separate notice rules
- [ ] **[Optional (recommended)]** Re-screening on renewal (org toggle) — FCRA/adverse action if denied
- [ ] **[Optional (recommended)]** Multiple owners on one property — primary owner approval sufficient?

---

## 4. M4 — Move-in / move-out inspections

*Source: `MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md` P1 — combine brief with M5*

- [ ] **[Mandatory (prod gate)]** **Normal wear and tear** vs damage — definitions for deduction suggestions
- [ ] **[Mandatory (prod gate)]** Itemized deduction list format meets TX **30-day** return requirements
- [ ] **[Mandatory (prod gate)]** Resident acknowledgment on move-in baseline — sufficient for deposit disputes?
- [ ] **[Mandatory (prod gate)]** Itemized deduction export content (description + amount per line)
- [ ] **[Optional (recommended)]** AI-suggested deductions — PM must always decide, or can agent auto-suggest above threshold only?
- [ ] **[Optional (recommended)]** Photo/video evidence retention period for disputes

---

## 5. M5 — Security deposits

*Source: `SECURITY-DEPOSIT-MVP-REQ.md` — **prod blocker***

- [ ] **[Mandatory (prod gate)]** **30-day** return rule after surrender of premises
- [ ] **[Mandatory (prod gate)]** **Forwarding address** requirement before return / refund initiation
- [ ] **[Mandatory (prod gate)]** Itemized deduction statement content (ties to M4)
- [ ] **[Mandatory (prod gate)]** **TREC / trust account** rules for PM companies
- [ ] **[Mandatory (prod gate)]** Deposit to trust within **2 business days** of receipt (TREC — confirm for TX PM use case)
- [ ] **[Mandatory (prod gate)]** Separate trust bank account vs operating — platform guidance to PM sufficient?
- [ ] **[Mandatory (prod gate)]** Partial month / early termination deposit handling — template language
- [ ] **[Optional (recommended)]** Interest on deposits — required in TX for this residential use case?
- [ ] **[Deferred (Phase 2)]** Deposit alternatives (Rhino, Jetty, etc.) — N/A MVP; confirm if mentioned in UI
- [ ] **[Optional (recommended)]** Chart of accounts template: trust vs operating (TX PM) — guidance only

*Partner P0 (business, not attorney):* confirm native sub-ledger vs escrow bolt-on — [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md)

---

## 6. M6 — Eviction (deferred)

*Full checklist when reopening:* [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md)

**MVP:** Only M2 items in §2 above apply. M6 items below are **Deferred (Phase 2)** until scope change + partner sign-off.

### Notices already in MVP (confirm via M2 §2 — do not duplicate sign-off)

These are **Mandatory (prod gate)** for M2, listed here for traceability only:

- Notice to Pay Rent or Vacate (2026 first delinquency) → §2
- Delivery method, cure period, fair housing notice content → §2

### If Phase 2 scope B (notices + tracking only)

- [ ] **[Deferred (Phase 2)]** 3-day notice to vacate (non-rent violations) — template required?
- [ ] **[Deferred (Phase 2)]** 30-day notice (month-to-month termination) — template required?
- [ ] **[Deferred (Phase 2)]** Log service date, method, recipient — minimum audit fields
- [ ] **[Deferred (Phase 2)]** Retention period for notice records
- [ ] **[Deferred (Phase 2)]** Can PM e-sign notices on behalf of owner, or owner must approve each?

### If Phase 2 scope A (full eviction workflow)

- [ ] **[Deferred (Phase 2)]** Platform **must not** auto-file court documents without attorney review
- [ ] **[Deferred (Phase 2)]** Required disclaimers: "not legal advice" + PM must use licensed counsel
- [ ] **[Deferred (Phase 2)]** Writ of possession / lockout — any platform role or PM-only?
- [ ] **[Deferred (Phase 2)]** SB 2349 / local ordinances affecting notice content
- [ ] **[Deferred (Phase 2)]** Multi-state: defer until TX validated

---

## 7. M1 — Listing / marketing copy

*Source: `SYNDICATION-MVP-RUNBOOK.md` Track C*

- [ ] **[Mandatory (prod gate)]** Rent, deposit, and fees displayed **accurately** on syndicated listings (C4)
- [ ] **[Mandatory (prod gate)]** Application link routes to CAP-2 flow with §92.3515 criteria notice (C5 — not on listing page)
- [ ] **[Optional (recommended)]** Fair Housing Act — AI listing copy review + block discriminatory language before publish (C3)
- [ ] **[Optional (recommended)]** `fairHousingDisclaimer` field in Listing Package schema — required text?
- [ ] **[Optional (recommended)]** MITS 5.0 fee transparency fields — Phase 2; confirm TX disclosure needs for MVP MITS 4.x
- [ ] **[Deferred (Phase 2)]** Oregon third-party syndication restrictions — N/A Texas MVP

---

## 8. Cross-cutting (platform)

*Sources: SPEC Constraints, CAP-10, governance*

- [ ] **[Mandatory (prod gate)]** **One attorney letter** covering RentalPro as B2B SaaS — PM is decision-maker, platform is software not landlord
- [ ] **[Mandatory (prod gate)]** Disclaimer language for autonomous agent actions (CAP-5 governance)
- [ ] **[Mandatory (prod gate)]** Electronic notice delivery (email/SMS) — legally sufficient for TX MVP notices (delinquency, renewal, deposit)
- [ ] **[Optional (recommended)]** Data retention for legal/audit records — align CAP-10 (7 years recommended)
- [ ] **[Optional (recommended)]** CAP-10 audit export format sufficient for fair-housing dispute review
- [ ] **[Optional (recommended)]** Legal hold flag on audit traces — block purge during litigation
- [ ] **[Optional (recommended)]** Texas-only jurisdiction disclaimer in PM onboarding (no other state templates until validated)

---

## Sign-off log

Record attorney approval per section. All **Mandatory** items in a section must be ✅ before prod enable.

| Area | Version / date | Attorney firm | Attorney name | Mandatory ✅ | Signed |
|------|----------------|---------------|---------------|--------------|--------|
| Leasing & screening | | | | ⬜ | ⬜ |
| M2 Delinquency | | | | ⬜ | ⬜ |
| M3 Renewals | | | | ⬜ | ⬜ |
| M4 Inspections | | | | ⬜ | ⬜ |
| M5 Deposits | | | | ⬜ | ⬜ |
| M1 Listings | | | | ⬜ | ⬜ |
| Cross-cutting | | | | ⬜ | ⬜ |
| M6 Eviction | | | | N/A MVP | — |

---

## Related artifacts

| Doc | Role |
|-----|------|
| [`REQ-TRACEABILITY.md`](./REQ-TRACEABILITY.md) | Chat → req docs map |
| [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) | M2 RulePack + Runbook P1–P5 detail |
| [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) | M6 partner + attorney (deferred) |
| [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) | M5 partner P0 + attorney P1 detail |
| [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) | M1 Track C compliance tasks |
| [`AI-MVP-DECISIONS.md`](./AI-MVP-DECISIONS.md) | Leasing/screening locked decisions |

**Maintenance:** When a new M-item or CAP adds a legal requirement, add it here with the correct tag and link from the domain doc.
