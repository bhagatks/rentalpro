# RentalPro.ai: Handoff to Cursor

**Last updated:** 2026-07-05 (M1–M7 locked; M8–M10 open)  
**Status:** Walking M1–M10 — M1–M5, M7 locked; M6 non-goal  
**Next session:** Continue M8–M10 → lock remaining CAPs → bmad-prd

## Pick up here

1. Read [`docs/README.md`](./README.md) — CAP status table  
2. **M1:** [`docs/SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md)  
3. **M2:** [`docs/DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) — **P1 attorney review = prod blocker**  
4. **M7:** [`docs/UNIFIED-COMMS-HUB-MVP-REQ.md`](./UNIFIED-COMMS-HUB-MVP-REQ.md) — expand CAP-7 unified inbox  
5. Continue **M8–M10** in [`MARKET-GAP-CHECKLIST.md`](./MARKET-GAP-CHECKLIST.md)  
6. Lock remaining CAP micro-specs → **bmad-prd**  
7. **Mobile (Phase 2):** [`MOBILE-APP-PHASE2-GAP-DISCUSSION.md`](./MOBILE-APP-PHASE2-GAP-DISCUSSION.md) — open TODOs (native apps remain Non-goal for MVP)

**Stack direction (not locked in spec):** Vercel + Supabase + Clerk

## Project Overview

### Vision: Autonomous Property Management (APM)
RentalPro.ai is an **AI-native "Self-Driving" Rental Operating System** that automates the entire property management lifecycle—from lead inquiry to maintenance resolution—using autonomous agents and smart-hardware integrations.

**Core Premise:** Unlike legacy systems (AppFolio, Propertyware, Buildium, Yardi) that serve human agents, RentalPro **replaces** the human agent. The AI *is* the manager.

---

## Business Model (Locked)

### Primary: B2B SaaS
- **Who:** Property Management (PM) companies
- **What:** Multi-tenant, white-label APM platform sold to existing PM firms
- **Value:** Eliminate admin staff, run entire portfolio autonomously
- **Go-to-market:** SaaS-ready engine with multi-tenancy from Day 1

### Secondary: Direct-to-Landlord (Internal Lab)
- Used to validate that AI agents actually work
- Not the initial GTM, but proof-of-concept vehicle

---

## The Force Behind This Work (Why Field - LOCKED)

### The Problem (Scalability Ceiling)
- **Pain:** "Administrative Tax" — PM companies spend 10% of rent on office staff, VAs, property managers
- **Target:** PM companies and landlords who want to eliminate this overhead
- **Lever:** Move Operating Expense Ratio (OER) from 10% → 2%

### Why Now
- **LLM Maturity:** GPT-4o, Claude 3.5 Sonnet can handle tenant screening, maintenance diagnostics with high accuracy
- **Hardware APIs:** Seam API controls smart locks (Schlage, Yale, August) through single integration
- **Customer Expectations:** Modern tenants demand 24/7 digital-first experience; refuse to wait 24 hours for replies

### Escalation & Autonomy Model (Tiered)

**Basic Plan ($29/mo):**
- AI is the **Interpreter/Intermediary**
- Handles all communication (tenant/vendor back-and-forth)
- Human owner/admin clicks "Approve" for significant actions or financial spend
- AI does legwork; human makes final call

**Professional Plan ($99/mo):**
- High autonomy with "Governance Rails"
- AI can approve repairs up to $500 without asking
- Escalation thresholds configurable by PM company
- Management on Autopilot with human governance by exception

---

## Core Workflows (What the System Does)

### 1. Autonomous Leasing
```
Lead inquiry → AI pre-qualifies → Stripe Identity verification 
→ Seam digital key issuance → AI analyzes lease signatures 
→ AI confirms first month payment → Lease signed
```

### 2. Autonomous Maintenance
```
Tenant uploads video of issue → AI diagnoses problem 
→ AI checks vendor database → AI calls/texts vendor for quote 
→ AI schedules repair → Photo verification of completion 
→ Stripe Connect payment to vendor
```

### 3. Autonomous Accounting
```
100% automated double-entry ledger
- No manual entries
- Every transaction via API (Plaid/Stripe)
- AI categorizes automatically → Flagged as "Pending Review" 
- Owner/accountant signs off monthly
- Automated owner distributions calculation
```

### 4. Customizable by PM Company
- PM admin can toggle "AI-Driven" vs "Human-in-the-loop" per module
- Examples:
  - Use AI for Maintenance, Human for Leasing
  - Use AI for Accounting, Human for Maintenance
  - Full autonomy across all modules

---

## Accounting Requirements (MVP Focus)

### For Basic Plan (MVP):
- AI categorizes 100% of transactions autonomously
- Flagged as "Pending Review" in ledger
- Owner/accountant must sign off on monthly report before distribution

### For SaaS Customization:
- PM companies decide their own governance model
- Full customization of approval workflows

---

## Technical Stack (To Be Finalized in Architecture Phase)

**Current Direction (Not Locked):**
- Frontend: Next.js 14+ (App Router)
- Language: TypeScript (type-safe accounting)
- Database: PostgreSQL (Prisma) with multi-tenant row-level security
- AI: Claude 3.5 Sonnet / GPT-4o
- Integrations: Seam (hardware), Stripe (payments/ID), Plaid (bank feeds)

---

## Current Spec State

### ✅ COMPLETED
- **Why:** Locked (force, target, lever, escalation model)
- **Competitive research:** 17 platforms in `competitive-matrix.md`
- **Capabilities kernel:** 12 CAPs drafted in `SPEC.md`
- **Constraints:** Locked (10 non-negotiables — see SPEC.md)
- **Non-goals:** Locked (Phase 2 out-of-scope table — see SPEC.md)
- **Success signal:** Locked (14-day APM validation criteria — see SPEC.md)
- **Market gaps:** M1–M10 **open questions**; M11–M12 locked Phase 2 non-goals
- **CAP-11 micro-spec:** Locked (`docs/capabilities/CAP-11-multi-tenant-saas.md`)
- **AI MVP decisions (partial):** Locked + TBD in `docs/AI-MVP-DECISIONS.md` (screening parked)

### 🔄 IN PROGRESS
- **Capabilities:** CAP-1–10, CAP-12 micro-specs draft — need review/lock
- **AI agents:** Texas leasing/maintenance decisions locked; screening TBD

### ⏳ PENDING
- **bmad-prd:** Expand locked spec into detailed requirements (next phase)
- **CAP micro-spec locks:** Review drafts against market gap assignments

### 📁 ARTIFACTS
- **Spec workspace:** `_bmad-output/specs/spec-rentalpro/`
  - `SPEC.md` (kernel — synced with memlog)
  - `competitive-matrix.md`
  - `.memlog.md` (decision log)
- **Partner wiki:** `docs/`
  - `AI-MVP-DECISIONS.md` — AI locked + TBD
  - `capabilities/CAP-11-multi-tenant-saas.md` — locked
- **Brief workspace:** `_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/`
- **Reference:** `docs/BMAD-REFERENCE.md` (BMad keyword guide)

---

## Next Steps (For Cursor)

### 1. Competitive Research
Research these 14 property management platforms:
1. AppFolio
2. OnQ PM
3. Buildium
4. Yardi
5. DoorLoop
6. TenantCloud
7. Rentec Direct
8. RentRedi
9. Avail
10. Innago
11. RealPage
12. Entrata
13. Rent Manager
14. TurboTenant
15. MRI Software
16. Belong Home
17. Latchel

**For Each, Document:**
- Core features (leasing, maintenance, accounting)
- Architecture patterns (how they handle workflows)
- Gaps/weaknesses (what they miss)
- Pricing model
- AI/automation capabilities (or lack thereof)

**Deliverable:** Competitive matrix showing:
- Feature comparison
- Architectural approach
- Gaps RentalPro can exploit
- Pricing/business model breakdown

### 2. Finalize Capabilities
Use competitive findings to define:
- **CAP-1 through CAP-N:** Each capability with intent + success criterion
- Organize by domain: Leasing, Maintenance, Accounting, Portals, Infrastructure

### 3. Lock Constraints
Non-negotiables that bend design:
- Multi-tenancy from Day 1
- Accounting accuracy (tax-ready, audit-ready)
- FHA compliance (AI screening logic must not discriminate)
- Data isolation per organization (SaaS security)
- Scalability (support 100s of PM companies)

### 4. Define Non-Goals
Explicit out-of-scope for MVP:
- Example: Not building a mobile app (Phase 2)
- Example: Not integrating with external PM systems (Phase 2)
- (To be finalized based on scope decisions)

### 5. Success Signal
Concrete, testable signal that APM is working:
- Example: "A PM company with 100 units adds their portfolio. Within 2 weeks, 80% of routine tasks (tenant inquiries, maintenance triage, accounting categorization) are handled autonomously. PM admin reports 15+ fewer hours/week of manual work."

### 6. Complete SPEC.md
Once all five fields are locked, Spec is complete. Ready to move to PRD phase.

---

## Key Decisions Made (Locked)

| Decision | Status | Rationale |
|----------|--------|-----------|
| Business model: B2B SaaS primary, Direct-to-Landlord as lab | ✅ Locked | SaaS is the lever; lab proves feasibility |
| Tiered autonomy: Basic (human-approve) + Professional (AI-decide within rails) | ✅ Locked | Addresses market spectrum; compliance safety |
| Accounting: AI categorizes, human signs off (MVP) | ✅ Locked | Balances autonomy with tax accuracy risk |
| Multi-tenancy from Day 1 | ✅ Locked | Requirement for SaaS reusability |
| No human review for low-value transactions | ✅ Locked | Moves OER from 10% → 2% |

---

## Open Questions (To Resolve in Capabilities Phase)

1. **Leasing scope:** Does AI generate lease documents, or just pre-qualify + issue key?
2. **Maintenance scope:** Full end-to-end vendor management, or triage only?
3. **Escrow/deposits:** Handled by APM or routed to third-party?
4. **Compliance:** Which jurisdictions are MVP (federal only, or state-specific)?
5. **Audit trail:** What level of logging required for tax/legal purposes?

---

## Workflow in BMad

**You are here:**
```
1. Ideation ✅
2. bmad-product-brief (skipped)
3. bmad-spec (GUIDED MODE) ← NEARLY COMPLETE
   - Why: ✅ Locked
   - Capabilities: 🔄 Draft (CAP-11 locked; others need review)
   - Constraints: ✅ Locked
   - Non-goals: ✅ Locked
   - Success signal: ✅ Locked
4. bmad-prd (next)
5. bmad-architecture (then)
6. bmad-create-epics-and-stories (then)
7. bmad-dev-story (then)
8. Ship
```

---

## How to Continue in Cursor

**Prompt for Cursor:**
```
I'm building RentalPro.ai, an Autonomous Property Management platform.

Context files to read:
- /Users/bstar/rentalpro/docs/BMAD-REFERENCE.md (BMad keywords)
- /Users/bstar/rentalpro/_bmad-output/specs/spec-rentalpro/.memlog.md (decision log)
- /Users/bstar/rentalpro/docs/HANDOFF-TO-CURSOR.md (this file)

Current status: Mid-spec in bmad-spec (guided mode)
- Why field: ✅ Locked
- Capabilities: 🔄 In progress

Next step: Conduct deep competitive research on 14 PM platforms, then finalize Capabilities in the spec.

Continue guiding me through bmad-spec in guided mode. After research, we'll lock Constraints, Non-goals, and Success signal.
```

---

## Files to Reference

- **Spec:** `_bmad-output/specs/spec-rentalpro/SPEC.md`
- **Memlog:** `_bmad-output/specs/spec-rentalpro/.memlog.md`
- **BMad Reference:** `docs/BMAD-REFERENCE.md`
- **Project root:** `/Users/bstar/rentalpro`

---

**Good luck in Cursor. Pick this up where you left off, and you'll have the spec locked in one focused session.**
