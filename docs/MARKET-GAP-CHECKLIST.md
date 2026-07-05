# Market Gap Checklist — vs mid-market PMS

**Last updated:** 2026-07-05  
**Purpose:** Features competitors ship that are missing or thin in our 12 CAPs.  
**Status:** 🔄 M1–M5 locked; M6 non-goal; M7–M10 open. M11–M12 Phase 2 non-goals.

**Legend:** 🔴 Must clarify for MVP · 🟡 Should add to CAP draft · ⚪ Phase 2 / non-goal candidate

---

## Missing capabilities (no CAP today)

| ID | Feature | Market | Priority | TBD action |
|----|---------|--------|----------|------------|
| M1 | Listings & marketing syndication (Zillow, etc.) | AppFolio, DoorLoop, Entrata | ⚪ | **Full MVP** — [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) |
| M2 | Delinquency & rent collection (late fees, reminders, plans) | All Tier 2 | 🔴 | **MVP locked** — CAP-4 + CAP-7 · [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) |
| M3 | Lease renewals (90-day workflow, renewal e-sign) | AppFolio Flows, Buildium | 🟡 | **MVP locked** — CAP-2 + Owner approval · [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) |
| M4 | Move-in / move-out inspections (photo checklist) | DoorLoop AI inspections | ⚪ | **MVP locked** — full in + out · [`MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md`](./MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md) |
| M5 | Security deposit lifecycle (trust sub-ledger, TX 30-day return) | All trust accounting | 🔴 | **MVP locked** — native CAP-4 · [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) · ⚠️ **check with partner** |
| M6 | Eviction & legal notice tracking | ⚪ | **Non-goal MVP** — partner + attorney checklist · [`EVICTION-NON-GOAL-CHECKLIST.md`](./EVICTION-NON-GOAL-CHECKLIST.md) |
| M7 | Unified comms hub (resident + owner + vendor + leads inbox) | AppFolio Realm-X Messages | 🟡 | CAP-7 expand or new CAP · **TBD** |
| M8 | Owner portal UX (beyond statements — docs, approvals) | All Tier 2 | 🟡 | Expand CAP-8 · **TBD** |
| M9 | Open API & webhooks (all tiers) | Rent Manager, matrix G-gap | 🟡 | Constraint or CAP-14 · **TBD** |
| M10 | Document vault (leases, COIs, notices per unit) | All | 🟡 | CAP-2 partial; general vault · **TBD** |
| M11 | 1099 / vendor & owner tax reporting | Buildium | ⚪ | **Non-goal** — Phase 2 (locked in SPEC) |
| M12 | Tour / self-showing scheduling (pre-lease access) | On Q, AppFolio Performer | ⚪ | **Non-goal** — Phase 2 (locked in SPEC) |

---

## Sub-features missing inside existing CAPs

### CAP-2 Autonomous leasing
- [ ] 🔴 Guest card / CRM pipeline (lead stages, source) · **TBD**
- [ ] 🔴 Application fee + Texas §92.3515 criteria flow · partial in AI-MVP-DECISIONS
- [ ] 🟡 Listing pages & syndication · **Full MVP** — [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) (M1)
- [ ] 🟡 Tour scheduling · **Non-goal** (M12 locked)
- [x] 🟡 Lease renewal workflow · **MVP** — [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) (M3)
- [ ] ⚪ Renters insurance requirement · **TBD**

### CAP-3 / CAP-7 Maintenance & resident
- [ ] 🟡 Bulk resident messaging (emergencies) · **TBD**
- [ ] ⚪ Maintenance chargeback to tenant · **TBD**
- [ ] ⚪ Recurring / preventive maintenance · **TBD**
- [ ] 🟡 Autopay / recurring rent · **TBD**

### CAP-4 Autonomous accounting
- [x] 🔴 Security deposit sub-ledger · **MVP** — [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) (M5)
- [x] 🔴 Late fee auto-assessment · **MVP** — [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) (M2)
- [ ] 🟡 Management fee auto-calculation · **TBD**
- [ ] 🟡 Bank reconciliation UI · **TBD**
- [ ] ⚪ AP / vendor bills (non-WO) · **TBD**
- [ ] ⚪ 1099 generation · **Non-goal** (M11 locked)

### CAP-7 Resident portal
- [ ] 🟡 Autopay · **TBD**
- [ ] ⚪ Announcements / community feed · **TBD**
- [ ] ⚪ Renters insurance upload · **TBD**

### CAP-8 Owner reporting
- [ ] 🟡 Dedicated owner portal login · **TBD** (M8)
- [ ] 🟡 Owner approval for capital repairs · **TBD** (M8)
- [ ] 🟡 ACH distribution execution · **TBD**

### CAP-9 Vendor management
- [ ] 🟡 W-9 / 1099 collection · **TBD**
- [ ] ⚪ Vendor portal (vs SMS/email MVP) · **TBD**

### CAP-11 Multi-tenant
- [ ] 🟡 SaaS billing (charge PM companies Basic/Pro) · **TBD**
- [ ] 🟡 Granular RBAC (accountant, etc.) · **TBD**

---

## Open decisions (resolve before CAP lock)

| Question | Options | Status |
|----------|---------|--------|
| M1–M10 MVP vs Phase 2? | Per-item founder pick | **Open** |
| New CAPs vs sub-features? | CAP-13/14/15 vs expand existing | **TBD** |
| Open API in MVP? | Yes / Phase 2 | **TBD** (M9) |
| Security deposits native? | CAP-4 sub-ledger / third party | **TBD** (M5) |

## Locked (not reopening)

| Item | Decision |
|------|----------|
| M11 (1099) | Phase 2 non-goal — SPEC.md |
| M12 (tours) | Phase 2 non-goal — SPEC.md |

---

## When resuming

1. Founder picks 🔴 and 🟡 items in M1–M10 for MVP vs Phase 2  
2. Update affected CAP micro-specs  
3. Lock remaining CAPs (CAP-1–10, 12)  
4. Say `assign market gaps` in Cursor to map decisions to CAPs  

**Related:** `docs/AI-MVP-DECISIONS.md` · `_bmad-output/specs/spec-rentalpro/SPEC.md` · `.memlog.md`
