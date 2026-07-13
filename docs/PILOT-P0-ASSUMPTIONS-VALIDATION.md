# P0: Pilot Plan — Assumptions & Validation Strategy

**Status:** Locked (2026-07-06)  
**Duration:** 6 weeks  
**Goal:** Validate largest product assumptions early, low cost, before full MVP build  
**Partner Target:** Texas PM company, 10–50 units, willing to run 1-week autonomous maintenance pilot

---

## Top 5 Assumptions (Ranked by Risk)

### 1️⃣ **PM companies will actually use autonomous workflows**
- **Risk:** If false, entire product is non-viable
- **Impact:** Adoption blockers = no revenue, no product-market fit signal
- **Validation in Pilot:** Week 5 — run 10–15 real maintenance requests; measure PM approval rate (target: ≥70% auto-approved)
- **Success Signal:** PM accepts AI dispatch without reverting to manual review workflow
- **Go/No-Go:** ≥70% approval rate → proceed to CAP-2 (leasing autonomy); <50% → pivot to human-in-the-loop by default

---

### 2️⃣ **Fair-housing/FCRA compliance can be baked into AI agents**
- **Risk:** If false, legal/regulatory blockers prevent launch; potential liability
- **Impact:** Fair-housing violation = FTC enforcement risk + PR disaster
- **Validation in Pilot:** Week 3 — screen 1 applicant through AI agent; week 6 audit: does agent follow Texas §92.3515 (criteria notice) + FCRA (standalone consent)?
- **Success Signal:** Zero fair-housing violations; audit trail shows compliant decision path
- **Go/No-Go:** Clean audit → proceed to full compliance framework; violation found → legal review + agent redesign (2–3 week delay)

---

### 3️⃣ **The 15 hours/week time savings is real, not aspirational**
- **Risk:** If false, business model breaks (value prop collapses)
- **Impact:** Can't justify $99/mo price to PM companies without 15+ hours freed
- **Validation in Pilot:** Week 1–6 — track PM staff time on CAP-1 import (week 1), 1 week of CAP-3 maintenance dispatch (week 5), basic CAP-4 accounting (week 6)
- **Success Signal:** PM self-reports ≥8 hours/week freed during pilot week; extrapolate to full portfolio
- **Go/No-Go:** ≥8 hrs freed → time-savings bet validated; <4 hrs → value prop needs rethinking

---

### 4️⃣ **Multi-tenant architecture holds at scale (RLS + PostgreSQL)**
- **Risk:** If false, data isolation fails; cross-org data leakage = company-ending bug
- **Impact:** Regulatory + compliance nightmare; loss of customer trust
- **Validation in Pilot:** Week 1 — deploy 2 test orgs (org A, org B); verify org A cannot query org B's data via RLS; run penetration test post-pilot
- **Success Signal:** Zero data leakage between test orgs; RLS policies audit-clean
- **Go/No-Go:** RLS audit passes → scale to 100+ orgs; fails → architecture rework (major delay)

---

### 5️⃣ **Residents will adopt self-serve portal (CAP-7)**
- **Risk:** If false, CAP-7 doesn't drive autonomy; cascades fail (CAP-2, CAP-3 rely on resident engagement)
- **Impact:** Resident adoption <30% = portal is unused feature; autonomy workflows starve for data
- **Validation in Pilot:** Week 2–6 — launch resident portal (manual rent payment + maintenance request submission) for 10 units; track weekly adoption
- **Success Signal:** ≥50% of residents use portal for rent payment within 1 week; ≥30% submit maintenance via portal
- **Go/No-Go:** ≥50% adoption → proceed to CAP-2/3; <30% → UX rework + 1-week extension

---

## 6-Week Pilot Scope

### **Capabilities In Scope**
- **CAP-1:** Portfolio onboarding (CSV import, 10-unit pilot)
- **CAP-3:** Maintenance autonomy (AI dispatch, human approval)
- **CAP-4:** Basic double-entry accounting (auto-categorize rent + maintenance costs)
- **CAP-7:** Resident portal (rent payment + maintenance request submission)
- **CAP-11:** Multi-tenant isolation (RLS validation)

### **Capabilities Out of Scope (Phase 2)**
- **CAP-2:** Leasing autonomy (depends on CAP-7 adoption + CAP-3 success)
- **CAP-5/6:** Governance rails (lock after CAP-3 validation)
- **CAP-8:** Owner reporting (depends on CAP-4 accuracy)
- **CAP-9:** Vendor ecosystem (depends on CAP-3 working at scale)
- **CAP-10:** Audit trail (pervasive; build as CAP-3 scales)
- **CAP-12:** Smart keys (depends on CAP-2 working)

---

## Timeline & Milestones

| Week | Capability | Deliverable | Success Gate | Assumption Tested |
|------|------------|-------------|--------------|-------------------|
| **1** | CAP-1 + CAP-11 | CSV import (10 units); org isolation verified | All 10 units visible; zero RLS leakage to test org | #4 (multi-tenant architecture holds) |
| **2** | CAP-7 | Resident portal live (rent payment only) | ≥50% of 10 residents pay via portal | #5 (resident adoption) |
| **3** | CAP-3 + CAP-7 | AI maintenance dispatch; PM human-in-the-loop | 1 test maintenance request approved by PM; audit trail clean | #2 (compliance baked in) |
| **4** | CAP-4 | Basic double-entry ledger; accountant sign-off | Month 1 close: zero manual journal entries; distributions accurate | #3 (time savings on accounting) |
| **5** | CAP-3 Scale | 10–15 real maintenance requests over 1 week | ≥70% auto-approved (within $500 threshold); resident confirms resolution | #1 (PM adoption of autonomy) + #3 (time savings real) |
| **6** | Retrospective | Pilot readout; go/no-go decision | All 5 assumptions validated or pivot plan locked | All 5 |

---

## Resource Footprint

| Work | Effort | Owner | Notes |
|------|--------|-------|-------|
| CAP-1 (Portfolio onboarding) | 1 week | Backend | CSV parse + validation; multi-tenant scoping |
| CAP-7 (Resident portal) | 1 week | Frontend | Basic form + Stripe payment integration |
| CAP-3 core (Maintenance dispatch) | 2 weeks | Backend + AI | Prompt engineering + approval workflow (human-in-the-loop) |
| CAP-4 micro (Basic ledger) | 1 week | Backend | Ingest CAP-3 + CAP-7 transactions; simple GL; no tax complexity yet |
| QA + pilot ops | 1 week | QA + PM | Monitoring, data validation, surveys, retrospective |
| Contingency | 1 week | All | Compliance review, regulatory feedback, UX tweaks |
| **Total** | **~6 developer-weeks** | 1 BE + 1 FE + 0.5 compliance | Run in parallel; 1 product manager coordinating with partner |

---

## Pilot Partner Profile

**Ideal Partner:**
- **Size:** 10–50 residential units (Texas-based)
- **Pain Point:** Maintenance coordination taking 3–5 hours/week (validates CAP-3 value)
- **Openness:** Willing to give honest feedback (even if pilot fails); open to 1-week autonomous trial
- **Leverage:** Offer free pilot + potential early-bird pricing if they stay through MVP launch

**Recruiting Angle:**
> "We're validating autonomous maintenance dispatch. 6-week pilot, 10–15 units, all data stays yours. If it works, you get the product at $29/mo (vs. $99/mo) for the first year."

**Timeline to Sign:**
- Week 0: Recruit partner (1 week lead time)
- Week 1–6: Pilot
- Week 7: Go/no-go decision

---

## Success Criteria & Metrics

### **Go to Full MVP (≥4 of 5 assumptions validated)**

✅ **CAP-1 + CAP-11:** Portfolio import works; zero RLS violations  
✅ **CAP-7:** ≥50% resident portal adoption (rent payment)  
✅ **CAP-3:** ≥70% AI dispatch approval rate; PM trusts autonomy  
✅ **CAP-4:** Ledger is tax-ready; zero manual fixes post-month-close  
✅ **Time Savings:** PM reports ≥8 hours/week freed (partial validation of 15-hour bet)  
✅ **Compliance:** Zero fair-housing violations; audit trail clean  

**Decision:** Full MVP build approved; lock CAP-1–12 in Phase 2 plan

---

### **Pivot (3–4 assumptions validated, 1–2 need rework)**

⚠️ **CAP-7 adoption 30–50%:** Portal UX needs refinement; add 1-week sprint  
⚠️ **CAP-3 approval 50–70%:** AI threshold needs tuning; adjust cost-estimate algorithm  
⚠️ **Time savings 4–8 hrs:** Value prop still exists but weaker; adjust pricing or scope  

**Decision:** Lock findings; extend pilot 1–2 weeks for rework; re-validate before MVP decision

---

### **Kill/Restart (≤2 assumptions validated)**

❌ **CAP-7 adoption <30%:** Product-market fit signal is weak; pause leasing autonomy (CAP-2), focus on internal PM tools first  
❌ **Fair-housing violation found:** Legal/compliance blocker; regulatory review required (2–3 week delay)  
❌ **Multi-tenant RLS fails:** Architecture risk; consider alternative tech (separate DBs per org, or managed PostgreSQL with stronger isolation)  
❌ **Time savings <4 hrs:** Value prop collapses; pivot to internal-PM-only use case or different market segment  

**Decision:** Retrospective + strategic pivot meeting required before restart

---

## Validation Matrix (Go/No-Go by Assumption)

| Assumption | Test | Target | Measurement | Owner | Go/No-Go Threshold |
|-----------|------|--------|-------------|-------|-------------------|
| #1: PM adoption | Week 5: AI dispatch approval rate | ≥70% | Count approved vs. rejected dispatches | PM partner | ≥70% → Go; 50–70% → Pivot; <50% → Kill |
| #2: Compliance | Week 3–6: Fair-housing audit | Zero violations | Audit trail review + legal sign-off | Legal + PM | Zero violations → Go; violation → Delay + review |
| #3: Time savings | Week 1–6: PM time tracking | ≥8 hrs/week freed | Timesheet + survey day 42 | PM partner | ≥8 hrs → Go; 4–8 hrs → Pivot; <4 hrs → Kill |
| #4: Multi-tenant | Week 1: RLS penetration test | Zero leakage | Query org B data from org A; verify failure | Backend | Zero leaks → Go; leaks found → Restart |
| #5: Resident adoption | Week 2–6: Portal adoption tracking | ≥50% rent payment, ≥30% maintenance | Daily dashboard of portal usage | Frontend + PM | ≥50% → Go; 30–50% → Pivot; <30% → Kill |

---

## Contingency Plans

### **If CAP-1 import fails (Week 1)**
- Cause: CSV parsing or multi-tenant validation broken
- Mitigation: 2-day debug; revert to manual data entry for pilot (slower but proceeding)
- Decision: Skip to CAP-7 (week 2) with manually-created test data

### **If CAP-7 adoption is low (Week 2–3)**
- Cause: Portal UX is confusing or residents don't trust online payment
- Mitigation: Simplify UX in 1-day sprint; add SMS reminder; offer incentive (first payment $5 discount)
- Decision: Extend week 2 by 1 week to re-measure adoption

### **If CAP-3 approval rate is 50–70% (Week 5)**
- Cause: AI cost estimates are too aggressive or PM is risk-averse
- Mitigation: Lower default threshold from $500 to $250; re-run 5 test dispatches
- Decision: Measure new approval rate; if ≥70%, proceed; if <70%, lock human-review-by-default

### **If compliance audit finds 1–2 violations (Week 6)**
- Cause: AI screening prompt didn't enforce fair-housing rules
- Mitigation: Engage legal review; redesign prompt; add compliance validator layer
- Decision: Schedule 1–2 week legal review before MVP approval; don't launch without legal sign-off

---

## Post-Pilot: Decision Framework

### **Scenario A: All 5 Assumptions Validated (Confidence: 95%)**
→ **Approved for MVP.** Proceed to Phase 2: Full 12-CAP build.  
Timeline: 12–14 weeks to launch (Q3 2026).

### **Scenario B: 4 Assumptions Validated, 1 Requires Rework (Confidence: 70%)**
→ **Conditional Approved.** Extend pilot 1–2 weeks; lock rework, re-validate, then MVP.  
Timeline: 14–16 weeks to launch (late Q3 2026).

### **Scenario C: 3 Assumptions Validated, 2 Require Rework (Confidence: 40%)**
→ **Strategic Review.** Consider pivoting to narrower MVP (e.g., internal PM tools first; B2B later).  
Timeline: 2–4 week decision + reset.

### **Scenario D: ≤2 Assumptions Validated (Confidence: <20%)**
→ **Pause MVP.** Shift to Phase 2 research (deeper market validation, competitive analysis, different PM segment).  
Timeline: Restart after 4–6 week research sprint.

---

## Decisions Log

| Date | Decision |
|------|----------|
| 2026-07-06 | P0 Pilot locked: 6-week timeline, 5 assumptions, CAP-1/3/4/7/11 in scope. Target: Texas PM partner with 10–50 units. Go/no-go decision week 6. |
