# Market Gap Checklist — vs mid-market PMS

**Last updated:** 2026-07-05  
**Purpose:** Features competitors ship that are missing or thin in our 12 CAPs. Mark **TBD** until assigned to a CAP, sub-feature, or Non-goal.

**Legend:** 🔴 Must clarify for MVP · 🟡 Should add to CAP draft · ⚪ Phase 2 / non-goal candidate

---

## Missing capabilities (no CAP today)

| ID | Feature | Market | Priority | TBD action |
|----|---------|--------|----------|------------|
| M1 | Listings & marketing syndication (Zillow, etc.) | AppFolio, DoorLoop, Entrata | ⚪ | Assign to CAP-2 sub-feature or CAP-14 · **TBD** |
| M2 | Delinquency & rent collection (late fees, reminders, plans) | All Tier 2 | 🔴 | CAP-4 or CAP-7 sub-feature · **TBD** |
| M3 | Lease renewals (90-day workflow, renewal e-sign) | AppFolio Flows, Buildium | 🟡 | CAP-2 sub-feature · **TBD** |
| M4 | Move-in / move-out inspections (photo checklist) | DoorLoop AI inspections | ⚪ | New sub-feature or Phase 2 · **TBD** |
| M5 | Security deposit lifecycle (trust sub-ledger, TX 30-day return) | All trust accounting | 🔴 | CAP-4 sub-feature · **TBD** |
| M6 | Eviction & legal notice tracking | Buildium, AppFolio | ⚪ | Phase 2 CAP or Non-goal · **TBD** |
| M7 | Unified comms hub (resident + owner + vendor + leads inbox) | AppFolio Realm-X Messages | 🟡 | CAP-7 expand or new CAP · **TBD** |
| M8 | Owner portal UX (beyond statements — docs, approvals) | All Tier 2 | 🟡 | Expand CAP-8 · **TBD** |
| M9 | Open API & webhooks (all tiers) | Rent Manager, matrix G-gap | 🟡 | Constraint or CAP-14 · **TBD** |
| M10 | Document vault (leases, COIs, notices per unit) | All | 🟡 | CAP-2 partial; general vault · **TBD** |
| M11 | 1099 / vendor & owner tax reporting | Buildium | ⚪ | CAP-4 sub-feature · **TBD** |
| M12 | Tour / self-showing scheduling (pre-lease access) | On Q, AppFolio Performer | ⚪ | CAP-12 Phase 2 · **TBD** |

---

## Sub-features missing inside existing CAPs

### CAP-2 Autonomous leasing
- [ ] 🔴 Guest card / CRM pipeline (lead stages, source) · **TBD**
- [ ] 🔴 Application fee + Texas §92.3515 criteria flow · partial in AI-MVP-DECISIONS
- [ ] 🟡 Listing pages & syndication · **TBD**
- [ ] 🟡 Tour scheduling · **TBD**
- [ ] 🟡 Lease renewal workflow · **TBD**
- [ ] ⚪ Renters insurance requirement · **TBD**

### CAP-3 / CAP-7 Maintenance & resident
- [ ] 🟡 Bulk resident messaging (emergencies) · **TBD**
- [ ] ⚪ Maintenance chargeback to tenant · **TBD**
- [ ] ⚪ Recurring / preventive maintenance · **TBD**
- [ ] 🟡 Autopay / recurring rent · **TBD**

### CAP-4 Autonomous accounting
- [ ] 🔴 Security deposit sub-ledger · **TBD**
- [ ] 🔴 Late fee auto-assessment · **TBD**
- [ ] 🟡 Management fee auto-calculation · **TBD**
- [ ] 🟡 Bank reconciliation UI · **TBD**
- [ ] ⚪ AP / vendor bills (non-WO) · **TBD**
- [ ] ⚪ 1099 generation · **TBD**

### CAP-7 Resident portal
- [ ] 🟡 Autopay · **TBD**
- [ ] ⚪ Announcements / community feed · **TBD**
- [ ] ⚪ Renters insurance upload · **TBD**

### CAP-8 Owner reporting
- [ ] 🟡 Dedicated owner portal login · **TBD**
- [ ] 🟡 Owner approval for capital repairs · **TBD**
- [ ] 🟡 ACH distribution execution · **TBD**

### CAP-9 Vendor management
- [ ] 🟡 W-9 / 1099 collection · **TBD**
- [ ] ⚪ Vendor portal (vs SMS/email MVP) · **TBD**

### CAP-11 Multi-tenant
- [ ] 🟡 SaaS billing (charge PM companies Basic/Pro) · **TBD**
- [ ] 🟡 Granular RBAC (accountant, etc.) · **TBD**

---

## Recommended MVP decisions (not locked — TBD)

| Question | Options | Recommendation |
|----------|---------|----------------|
| New CAPs vs sub-features? | CAP-13/14/15 vs expand existing | Expand CAP-2, 4, 7, 8 for MVP · **TBD** |
| Phase 2 non-goals | Evictions, syndication, 1099, inspections | Lock in Constraints session · **TBD** |
| Open API in MVP? | Yes / Phase 2 | Document as constraint if yes · **TBD** |

---

## When resuming

1. Founder picks 🔴 items for MVP vs Phase 2  
2. Update affected CAP micro-specs  
3. Lock **Non-goals** in SPEC for ⚪ items  
4. Say `assign market gaps` in Cursor to map M1–M12 to CAPs  

**Related:** `docs/AI-MVP-DECISIONS.md` (AI/screening TBD) · `_bmad-output/specs/spec-rentalpro/competitive-matrix.md`
