# Market Gap Checklist — vs mid-market PMS

**Last updated:** 2026-07-05  
**Purpose:** Features competitors ship that are missing or thin in our 12 CAPs.  
**Status:** ✅ Assigned — MVP floor locked in SPEC Constraints #10; Phase 2 items in SPEC Non-goals.

**Legend:** 🔴 MVP (assigned) · 🟡 MVP sub-feature · ⚪ Phase 2 / non-goal

---

## Missing capabilities (no CAP today)

| ID | Feature | Market | Priority | Assignment |
|----|---------|--------|----------|------------|
| M1 | Listings & marketing syndication (Zillow, etc.) | AppFolio, DoorLoop, Entrata | ⚪ | **Non-goal** — Phase 2 |
| M2 | Delinquency & rent collection (late fees, reminders, plans) | All Tier 2 | 🔴 | **CAP-4 + CAP-7** — MVP sub-feature |
| M3 | Lease renewals (90-day workflow, renewal e-sign) | AppFolio Flows, Buildium | 🟡 | **CAP-2** — MVP sub-feature |
| M4 | Move-in / move-out inspections (photo checklist) | DoorLoop AI inspections | ⚪ | **Non-goal** — Phase 2 |
| M5 | Security deposit lifecycle (trust sub-ledger, TX 30-day return) | All trust accounting | 🔴 | **CAP-4** — MVP sub-feature (Constraint #10) |
| M6 | Eviction & legal notice tracking | Buildium, AppFolio | ⚪ | **Non-goal** — Phase 2 |
| M7 | Unified comms hub (resident + owner + vendor + leads inbox) | AppFolio Realm-X Messages | 🟡 | **CAP-7** — MVP partial (threaded inbox per entity) |
| M8 | Owner portal UX (beyond statements — docs, approvals) | All Tier 2 | 🟡 | **CAP-8** — MVP expand (portal login + doc access + capital repair approval) |
| M9 | Open API & webhooks (all tiers) | Rent Manager, matrix G-gap | ⚪ | **Non-goal** — Phase 2 |
| M10 | Document vault (leases, COIs, notices per unit) | All | 🟡 | **CAP-2** — MVP sub-feature (Constraint #10) |
| M11 | 1099 / vendor & owner tax reporting | Buildium | ⚪ | **Non-goal** — Phase 2 |
| M12 | Tour / self-showing scheduling (pre-lease access) | On Q, AppFolio Performer | ⚪ | **Non-goal** — Phase 2 |

---

## Sub-features assigned inside existing CAPs

### CAP-2 Autonomous leasing
- [x] 🔴 Guest card / CRM pipeline (lead stages, source) — **MVP**
- [x] 🔴 Application fee + Texas §92.3515 criteria flow — **MVP** (partial in AI-MVP-DECISIONS)
- [ ] 🟡 Listing pages & syndication — **Phase 2** (M1)
- [ ] ⚪ Tour scheduling — **Phase 2** (M12)
- [x] 🟡 Lease renewal workflow — **MVP** (M3)
- [ ] ⚪ Renters insurance requirement — **Phase 2**

### CAP-3 / CAP-7 Maintenance & resident
- [ ] 🟡 Bulk resident messaging (emergencies) — **MVP partial** (CAP-7)
- [ ] ⚪ Maintenance chargeback to tenant — **Phase 2**
- [ ] ⚪ Recurring / preventive maintenance — **Phase 2**
- [x] 🟡 Autopay / recurring rent — **MVP** (CAP-7, supports M2)

### CAP-4 Autonomous accounting
- [x] 🔴 Security deposit sub-ledger — **MVP** (M5, Constraint #10)
- [x] 🔴 Late fee auto-assessment — **MVP** (M2)
- [ ] 🟡 Management fee auto-calculation — **MVP stretch** (CAP-4)
- [ ] 🟡 Bank reconciliation UI — **MVP stretch** (CAP-4)
- [ ] ⚪ AP / vendor bills (non-WO) — **Phase 2**
- [ ] ⚪ 1099 generation — **Phase 2** (M11)

### CAP-7 Resident portal
- [x] 🟡 Autopay — **MVP**
- [ ] ⚪ Announcements / community feed — **Phase 2**
- [ ] ⚪ Renters insurance upload — **Phase 2**

### CAP-8 Owner reporting
- [x] 🟡 Dedicated owner portal login — **MVP** (M8)
- [x] 🟡 Owner approval for capital repairs — **MVP** (M8)
- [ ] 🟡 ACH distribution execution — **MVP stretch** (CAP-4 dependency)

### CAP-9 Vendor management
- [ ] 🟡 W-9 / 1099 collection — **Phase 2** (1099 gen is non-goal)
- [ ] ⚪ Vendor portal (vs SMS/email MVP) — **Phase 2**

### CAP-11 Multi-tenant
- [ ] 🟡 SaaS billing (charge PM companies Basic/Pro) — **MVP** (platform ops manual billing acceptable for first customers)
- [ ] 🟡 Granular RBAC (accountant, etc.) — **MVP partial** (pm-admin, pm-staff, owner, resident locked; accountant role TBD in PRD)

---

## Decisions locked (2026-07-05)

| Question | Decision |
|----------|----------|
| New CAPs vs sub-features? | **Expand CAP-2, 4, 7, 8** — no CAP-13/14/15 for MVP |
| Phase 2 non-goals | **Locked in SPEC.md Non-goals table** |
| Open API in MVP? | **No** — Phase 2 non-goal |
| MVP market parity floor | **M2, M5, guest-card CRM, document vault** — Constraint #10 |

---

## When resuming

1. Lock remaining CAP micro-specs (CAP-1–10, 12) using assignments above
2. Say **`resume screening`** to pick criteria defaults + industry-breaking thesis
3. Say **`lock CAP-N`** to promote a draft micro-spec to locked

**Related:** `docs/AI-MVP-DECISIONS.md` · `_bmad-output/specs/spec-rentalpro/SPEC.md` · `.memlog.md`
