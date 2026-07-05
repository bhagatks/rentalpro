# Texas Attorney Review — Master Checklist

**Last updated:** 2026-07-05  
**Purpose:** Single checklist for all Texas RE attorney reviews before production. Domain detail lives in linked M-docs — **check items here, sign off once per area.**

> **Not legal advice.** Product architecture only. Engage licensed Texas real estate attorney before ship.

---

## How to use

1. Schedule **one attorney engagement** for Texas MVP — bring this doc + linked briefs  
2. Check off each section as attorney approves  
3. Record sign-off in **Sign-off log** at bottom + append `.memlog.md`  
4. **Prod gate:** Do not enable autonomous features in production until required sections for that feature are ✅

---

## Master status

| Area | Doc | Prod gate? | Status |
|------|-----|------------|--------|
| **Leasing & screening** | [`AI-MVP-DECISIONS.md`](./AI-MVP-DECISIONS.md) | Yes (lease + screening) | ⬜ |
| **M2 Delinquency** | [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) Runbook P1–P5 | **Yes — blocker** | ⬜ |
| **M3 Renewals** | [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) P1 | Yes | ⬜ |
| **M4 Inspections** | [`MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md`](./MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md) P1 | Yes (deduction copy) | ⬜ |
| **M5 Deposits** | [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) P1 | **Yes — blocker** | ⬜ |
| **M6 Eviction** | [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) | N/A MVP (non-goal) | — deferred |
| **M1 Syndication** | Listing copy / fair housing | Recommended | ⬜ |

**Partner review (separate):** [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) P0 (M5), [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) (M6 if reopened)

---

## 1. Leasing & screening (CAP-2)

*Source: `AI-MVP-DECISIONS.md` Texas compliance checklist*

- [ ] §92.3515 tenant selection criteria notice **before** application fee + signed acknowledgment
- [ ] FCRA standalone written consent before consumer report
- [ ] Adverse action notice with **specific** denial reasons
- [ ] **Platform Texas starter lease template** — attorney review before ship
- [ ] PM-uploaded lease template flow — disclaimer sufficient?
- [ ] Flood disclosure — **both landlord and tenant sign** (SB 2349, effective 2025)
- [ ] Lead-based paint disclosure if built before 1978
- [ ] Owner/agent identity in lease (§92.201)
- [ ] Tenant repair remedies in bold/underlined (§92.056)
- [ ] FHA: protected classes excluded from agent inputs and criteria
- [ ] Screening criteria defaults (when resumed) — income, eviction, credit, criminal HUD assessment

---

## 2. M2 — Delinquency & late fees

*Source: `DELINQUENCY-RULES-ENGINE.md` — **prod blocker***

- [ ] `StateRulePack-TX` rules TX-LF-001 through TX-PAY-001 approved
- [ ] §92.019: 2-day grace, 10%/12% caps, combined initial+daily fees
- [ ] Late fee only if in written lease (TX-LF-001)
- [ ] **Notice to Pay Rent or Vacate** — 2026 first delinquency in lease term (TX-EV-001)
- [ ] Reminder templates: friendly, late fee assessed, formal notice (wording approved)
- [ ] NSF / bounced check fee limits for TX residential
- [ ] Auto-send blocked until templates approved (Runbook P3)
- [ ] Sign-off version recorded (Runbook P5)

**Brief to send:** RulePack JSON, 3-layer model, sample policies (8% valid, 15% blocked, 1-day grace blocked)

---

## 3. M3 — Lease renewals

*Source: `LEASE-RENEWAL-MVP-REQ.md` P1*

- [ ] Rent increase notice timing for fixed-term vs month-to-month
- [ ] Owner approval on increases — legally sufficient?
- [ ] Renewal lease = amendment vs new lease — template approach OK?
- [ ] §92.201 owner/agent on renewal document

---

## 4. M4 — Move-in / move-out inspections

*Source: `MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md` P1*

- [ ] **Normal wear and tear** vs damage — definitions for deduction suggestions
- [ ] Itemized deduction list format meets TX 30-day return requirements
- [ ] Resident acknowledgment on move-in baseline — sufficient for disputes?
- [ ] Can AI-suggested deductions be used or PM must always decide?

*Combine review with M5 deposit brief.*

---

## 5. M5 — Security deposits

*Source: `SECURITY-DEPOSIT-MVP-REQ.md` — **prod blocker***

- [ ] **30-day** return rule after surrender of premises
- [ ] **Forwarding address** requirement before return
- [ ] Itemized deduction statement content
- [ ] **TREC / trust account** rules for PM companies
- [ ] Separate trust bank account vs operating — platform guidance OK?
- [ ] Interest on deposits — required in TX for this use case?
- [ ] Deposit alternatives (Rhino etc.) — N/A MVP but confirm

*Partner P0 (business) separate from this attorney section.*

---

## 6. M6 — Eviction (deferred)

*Full checklist when reopening:* [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) § Attorney checklist

MVP: only M2 Notice to Pay items in §2 above apply.

---

## 7. M1 — Listing / marketing copy

- [ ] Fair Housing Act — listing copy AI review sufficient?
- [ ] Deposit/rent/fees displayed accurately on syndicated listings
- [ ] Application link + §92.3515 criteria notice flow from listing

---

## 8. Cross-cutting

- [ ] **One attorney letter** covering RentalPro as B2B SaaS — PM is decision-maker, platform is software not landlord?
- [ ] Disclaimer language for autonomous agent actions
- [ ] Data retention for legal/audit records (align CAP-10 — 7 years recommended)
- [ ] Electronic notice delivery (email/SMS) — legally sufficient for TX MVP notices?

---

## Sign-off log

| Area | Version / date | Attorney firm | Attorney name | Signed |
|------|----------------|---------------|---------------|--------|
| Leasing & screening | | | | ⬜ |
| M2 Delinquency | | | | ⬜ |
| M3 Renewals | | | | ⬜ |
| M4 Inspections | | | | ⬜ |
| M5 Deposits | | | | ⬜ |
| M1 Listings | | | | ⬜ |
| Cross-cutting | | | | ⬜ |

---

## Related artifacts

| Doc | Role |
|-----|------|
| [`REQ-TRACEABILITY.md`](./REQ-TRACEABILITY.md) | Chat → req docs map |
| [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) | M6 partner + attorney (deferred) |
| [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) | M5 partner P0 + attorney P1 detail |

**Maintenance:** When a new M-item adds attorney P1, add a section here and link from domain doc.
