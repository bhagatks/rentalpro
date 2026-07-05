# M5: Security Deposit Lifecycle — Full Requirements

**Status:** locked  
**Market gap:** M5  
**CAPs:** CAP-4 (primary), CAP-7, CAP-8, CAP-10; M4 (deductions), CAP-2 (collection at move-in)  
**Locked:** 2026-07-05  
**MVP state:** Texas (`TX`)

> **Partner review:** Confirm native CAP-4 trust sub-ledger approach with partner (P0). **Attorney review:** [`ATTORNEY-REVIEW-CHECKLIST.md`](./ATTORNEY-REVIEW-CHECKLIST.md) §5.

> **Companion req doc for M5.** Native trust sub-ledger in CAP-4 — same pattern as AppFolio, Buildium, DoorLoop. Not third-party escrow for MVP.

---

## 1. Problem & rationale

Security deposits are tenant funds held in trust — not PM or owner income. PM companies must collect, segregate, track per tenant/unit, apply move-out deductions (M4), and return remainder within Texas **30 days** with itemized list. Mishandling triggers legal penalties and license risk.

**Market standard:** AppFolio, Buildium, DoorLoop all ship **native trust accounting** with tenant/property sub-ledgers and three-way reconciliation — not optional for B2B PM SaaS.

**Why native (Option A locked):** M4 move-out inspections produce approved deductions that must post to a deposit sub-ledger. Third-party escrow (Roost/Rentable) is a Phase 2 bolt-on option; MVP matches mid-market PMS with CAP-4 as system of record.

---

## 2. Product vision

```
Move-in → collect deposit (Stripe) → CAP-4 deposit liability sub-ledger
       ↓
Held separate from rent operating ledger (pooled trust bank account)
       ↓
Move-out (M4) → PM approves deductions → reduce liability
       ↓
Deposit Agent tracks 30-day TX deadline → return remainder + itemized statement
       ↓
Three-way reconcile: bank = trust GL = sum of sub-ledgers (monthly, CAP-4)
```

**vs bolt-ons:** Roost/Rentable add compliance automation on top of PMS — RentalPro owns the ledger Day 1; optional partner sync Phase 2.

**Deposit alternatives (Rhino/Jetty/Obligo):** Phase 2 — not M5 MVP scope.

---

## 3. User stories

| Actor | Story |
|-------|-------|
| Resident | I pay security deposit at move-in online; I receive itemized statement at move-out. |
| Resident | I provide forwarding address for deposit return. |
| PM admin | I see deposit balance per tenant/unit; trust vs operating clearly separated. |
| PM admin | I apply M4-approved deductions and initiate refund. |
| **Owner** | I see deposit status for my properties (held amounts, not my cash). |
| Accountant | I run three-way trust reconciliation monthly; sign off before distributions (CAP-4). |
| Deposit Agent | I alert at day 25 if return not initiated; block disposition past 30 days without override + CAP-10. |
| Inspection Agent (M4) | Approved deductions flow to deposit adjustment queue. |

---

## 4. End-to-end flows

### Move-in collection

1. Lease signed (CAP-2); deposit amount from lease terms.
2. Resident pays via CAP-7 (Stripe) → funds to org **trust** Connect account (separate from operating).
3. CAP-4 posts: **Dr Trust Bank / Cr Tenant Deposit Liability** (sub-ledger by leaseId/unitId).
4. Link to M4 move-in inspection (optional: block key until paid — org toggle).

### Hold period

1. Deposit remains on tenant sub-ledger — never recognized as income.
2. Optional interest accrual: **Phase 2** (some states; TX residential often no interest requirement — confirm P1).
3. Accountant monthly: three-way reconcile trust account.

### Move-out return

1. M4 move-out complete; PM approves deduction line items.
2. Resident forwarding address captured (required TX).
3. Deposit Agent calculates: deposit − approved deductions = refund due.
4. PM confirms → Stripe payout to resident (or check workflow TBD).
5. CAP-4: reduce liability; post disposition entries.
6. Generate **itemized deduction statement** (PDF/email) within 30 days of surrender.

### Edge paths

| Trigger | Behavior |
|---------|----------|
| Deductions exceed deposit | Resident owes balance; route to M2 delinquency |
| No forwarding address | Hold refund; agent alerts PM (TX requirement) |
| Disputed deductions | Flag dispute; PM manual; CAP-10 |
| Partial month / early termination | Apply lease + TX rules; attorney template P1 |
| Uncashed refund / escheatment | Track dormancy — Phase 2 |
| Deposit transferred to new tenant (unit re-rent) | PM manual; rare — document only MVP |

---

## 5. Functional requirements

| ID | Requirement | MVP |
|----|-------------|-----|
| FR-M5-01 | Separate **deposit liability** GL accounts vs rent income (CAP-4) | ✅ |
| FR-M5-02 | Per-tenant/unit **deposit sub-ledger** balance | ✅ |
| FR-M5-03 | Collect deposit at move-in via Stripe → trust account | ✅ |
| FR-M5-04 | Accept M4-approved deductions as adjustment entries | ✅ |
| FR-M5-05 | Capture forwarding address before return workflow | ✅ |
| FR-M5-06 | Itemized deduction statement (description + amount per line) | ✅ |
| FR-M5-07 | Deposit Agent: 30-day TX deadline tracking + day-25 alert | ✅ |
| FR-M5-08 | Block return past 30 days without PM override + CAP-10 | ✅ |
| FR-M5-09 | Three-way reconciliation report (bank = GL = sub-ledgers) | ✅ |
| FR-M5-10 | Trust bank account separate from operating (Stripe Connect or Plaid) | ✅ |
| FR-M5-11 | Owner read-only deposit status on their properties (CAP-8) | ✅ |
| FR-M5-12 | CAP-10 trace for collect, deduct, return, override | ✅ |
| FR-M5-13 | Export audit package for trust exam | ✅ |

---

## 6. Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-M5-01 | Deposit funds never commingled with PM operating account in ledger |
| NFR-M5-02 | Sub-ledger sum must equal trust GL deposit liability balance |
| NFR-M5-03 | All deposit transactions immutable after period close |
| NFR-M5-04 | Multi-tenant isolation on all deposit records |

---

## 7. Data model & integrations

| Entity | Key fields |
|--------|------------|
| `DepositAccount` | organizationId, leaseId, unitId, tenantId, originalAmount, balance, status, collectedAt |
| `DepositTransaction` | depositAccountId, type (collect\|deduction\|refund\|adjustment), amount, m4ComparisonId, traceId |
| `DepositReturn` | depositAccountId, forwardingAddress, deductions[], refundAmount, initiatedAt, completedAt, itemizedStatementUrl |
| `TrustBankAccount` | organizationId, stripeAccountId, accountType (trust\|operating), lastReconciledAt |

**GL mapping (double-entry):**

| Event | Debit | Credit |
|-------|-------|--------|
| Collect deposit | Trust bank | Tenant deposit liability |
| Deduction | Tenant deposit liability | Owner income / repair expense (per chart) |
| Refund | Tenant deposit liability | Trust bank |

**Integrations:**

| CAP / M | Use |
|---------|-----|
| CAP-4 | Ledger, reconciliation, monthly sign-off |
| CAP-7 | Resident payment + forwarding address |
| M4 | Approved deductions input |
| CAP-2 | Deposit amount from lease |
| CAP-8 | Owner visibility |
| CAP-10 | Audit |
| Stripe Connect | Trust account payouts |
| Plaid | Trust bank balance for reconcile |

**Phase 2 optional:** Roost/Rentable API sync for multi-state compliance automation.

---

## 8. Acceptance criteria

- [ ] Deposit collection posts to liability sub-ledger, not income
- [ ] Sub-ledger balance matches tenant deposit amount after collection
- [ ] M4-approved deductions reduce balance correctly
- [ ] Itemized statement lists each deduction with amount + description
- [ ] Refund cannot initiate without forwarding address
- [ ] Agent alerts at day 25; override past day 30 logged in CAP-10
- [ ] Three-way reconcile report balances for test month
- [ ] Owner sees deposit status only for their properties
- [ ] Operating and trust Stripe accounts are distinct per org
- [ ] Two orgs' deposit ledgers are isolated

---

## 9. Runbook & prerequisites

| ID | Task | Blocker? | Status | Notes |
|----|------|----------|--------|-------|
| **P0** | **Partner review** — confirm native sub-ledger vs escrow bolt-on, trust bank setup, TX compliance strategy | Before prod | ⬜ | **Check with partner** |
| P1 | **Texas attorney review:** 30-day rule, itemization, TREC trust account, forwarding address | **Prod gate** | ⬜ | Can combine brief with M4 P1 |
| P2 | Chart of accounts template: trust vs operating (TX PM) | MVP | ⬜ |
| P3 | Stripe Connect trust account setup per org | MVP | ⬜ |
| B1 | DepositAccount + sub-ledger schema | — | ⬜ |
| B2 | Collection flow CAP-7 → CAP-4 | — | ⬜ |
| B3 | M4 deduction import + PM approve | — | ⬜ |
| B4 | Return workflow + itemized PDF | — | ⬜ |
| B5 | Deposit Agent deadline job | — | ⬜ |
| B6 | Three-way reconciliation UI | — | ⬜ |

**Dependencies:** M4 locked (deduction source). Align P1 with M4 P1 (can combine attorney brief).

---

## 10. Locked decisions (2026-07-05)

| ID | Decision | Value |
|----|----------|-------|
| M5 scope | **A — Native CAP-4 sub-ledger** | Same as AppFolio/Buildium/DoorLoop |
| Third-party escrow | Phase 2 optional (Roost/Rentable) | Not MVP |
| Deposit alternatives | Phase 2 (Rhino/Jetty) | Not MVP |
| Interest on deposits | Phase 2 / state-specific | TX MVP TBD in P1 |
| M5 trust approach | **Check with partner** before prod | Native CAP-4 locked pending partner confirm |

---

## Related artifacts

| Doc | Link |
|-----|------|
| M4 inspections | [`MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md`](./MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md) |
| CAP-4 | [`capabilities/CAP-4-autonomous-accounting.md`](./capabilities/CAP-4-autonomous-accounting.md) |
| M2 delinquency | [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) (balance owed after deposit exhausted) |
