# RentalPro.ai: Handoff to Cursor

**Last updated:** 2026-07-24 (north star brief synced; P0 pilot + M1–M7 reflected)  
**Status:** Kernel locked; P0 pilot gate before full MVP; M1–M5, M7 locked; M6 non-goal; M8–M10 open  
**Next session:** Continue M8–M10 → lock remaining CAP micro-specs → run/complete P0 pilot → bmad-prd  
**North star brief:** [`_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md`](../_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md)

## Pick up here

1. Read [`docs/README.md`](./README.md) — CAP status table  
2. **M1:** [`docs/SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md)  
3. **M2:** [`docs/DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) — **P1 attorney review = prod blocker**  
4. **M7:** [`docs/UNIFIED-COMMS-HUB-MVP-REQ.md`](./UNIFIED-COMMS-HUB-MVP-REQ.md) — expand CAP-7 unified inbox  
5. Continue **M8–M10** in [`MARKET-GAP-CHECKLIST.md`](./MARKET-GAP-CHECKLIST.md)  
6. Lock remaining CAP micro-specs → **bmad-prd**

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
- **Constraints:** Locked (9 non-negotiables — see SPEC.md; Constraint #10 parity floor removed)
- **Non-goals:** Locked (Phase 2 out-of-scope table — see SPEC.md)
- **Success signal:** Locked (14-day APM validation criteria — see SPEC.md)
- **P0 pilot plan:** Locked — `docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md` (validate 5 assumptions before full MVP)
- **Market gaps:** M1–M5, M7 locked MVP; M6 non-goal; M8–M10 open; M11–M12 Phase 2 non-goals
- **CAP-1 fee:** Management fee locked at **7%** (no per-property override)
- **CAP-11 micro-spec:** Locked (`docs/capabilities/CAP-11-multi-tenant-saas.md`)
- **AI MVP decisions (partial):** Locked + TBD in `docs/AI-MVP-DECISIONS.md` (screening parked)
- **Product brief (north star):** Populated draft from locked kernel + P0

### 🔄 IN PROGRESS
- **Capabilities:** CAP-1–10, CAP-12 micro-specs draft — need review/lock
- **AI agents:** Texas leasing/maintenance decisions locked; screening TBD
- **P0 pilot:** Plan locked; partner recruit + execution pending

### ⏳ PENDING
- **M8–M10:** Owner portal UX, open API/webhooks, document vault
- **CAP micro-spec locks:** Review drafts against market gap assignments
- **P0 go/no-go:** ≥4 of 5 assumptions validated → full MVP build
- **bmad-prd:** Expand locked spec into detailed requirements (after CAP locks / pilot gate)

### 📁 ARTIFACTS
- **Spec workspace:** `_bmad-output/specs/spec-rentalpro/`
  - `SPEC.md` (kernel — synced with memlog)
  - `competitive-matrix.md`
  - `.memlog.md` (decision log)
- **Partner wiki:** `docs/`
  - `PILOT-P0-ASSUMPTIONS-VALIDATION.md` — P0 gate
  - `AI-MVP-DECISIONS.md` — AI locked + TBD
  - `capabilities/CAP-11-multi-tenant-saas.md` — locked
- **Brief (north star):** `_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md`
- **Reference:** `docs/BMAD-REFERENCE.md` (BMad keyword guide)

---

## Next Steps (For Cursor)

1. **Pick up here** — see top of this file (M8–M10, CAP micro-spec locks, P0 pilot).
2. **North star** — product brief at `_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md` (draft; partner review welcome).
3. **P0** — recruit Texas 10–50 unit pilot partner; execute `docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md`.
4. **Then** — bmad-prd after CAP locks / pilot go-no-go. Architecture spine already exists under `_bmad-output/planning-artifacts/architecture/`.

*Competitive research, Constraints, Non-goals, and Success signal are done — do not re-open Why.*

---

## Key Decisions Made (Locked)

| Decision | Status | Rationale |
|----------|--------|-----------|
| Business model: B2B SaaS primary, Direct-to-Landlord as lab | ✅ Locked | SaaS is the lever; lab proves feasibility |
| Tiered autonomy: Basic (human-approve) + Professional (AI-decide within rails) | ✅ Locked | Addresses market spectrum; compliance safety |
| Accounting: AI categorizes, human signs off (MVP) | ✅ Locked | Balances autonomy with tax accuracy risk |
| Multi-tenancy from Day 1 | ✅ Locked | Requirement for SaaS reusability |
| No human review for low-value transactions | ✅ Locked | Moves OER from 10% → 2% |
| Management fee | ✅ Locked | **7%** on Owner; no per-property override (CAP-1) |
| P0 before full MVP | ✅ Locked | Validate top 5 assumptions in 6-week Texas pilot |

---

## Open Questions (Still Live)

1. **Screening criteria defaults** — parked (`docs/AI-MVP-DECISIONS.md`)
2. **M8–M10** — owner portal UX, open API/webhooks, document vault
3. **Audit retention** — recommend 7 years; pending CAP-10 lock
4. **P0 partner recruit** — Texas PM, 10–50 units

---

## Workflow in BMad

**You are here:**
```
1. Ideation ✅
2. bmad-product-brief ✅ draft north star (partner review welcome)
3. bmad-spec (GUIDED MODE) ← KERNEL LOCKED; CAP MICRO-SPECS + M8–M10 REMAIN
   - Why / Constraints / Non-goals / Success: ✅ Locked
   - Capabilities: 🔄 Draft (CAP-11 locked; others need review)
   - P0 pilot plan: ✅ Locked (execute before full MVP build)
4. P0 pilot go/no-go
5. bmad-prd
6. bmad-architecture (spine drafted — refine as needed)
7. bmad-create-epics-and-stories
8. bmad-dev-story
9. Ship
```

---

## How to Continue in Cursor

**Prompt for Cursor:**
```
I'm building RentalPro.ai, an Autonomous Property Management platform.

Context files to read:
- docs/HANDOFF-TO-CURSOR.md
- _bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md (north star)
- _bmad-output/specs/spec-rentalpro/SPEC.md + .memlog.md
- docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md

Current status: Spec kernel locked; P0 pilot is the risk gate before full MVP.
Next: Walk M8–M10, lock CAP micro-specs, recruit Texas pilot partner.
```

---

## Files to Reference

- **North star brief:** `_bmad-output/planning-artifacts/briefs/brief-rentalpro-2026-07-04/brief.md`
- **Spec:** `_bmad-output/specs/spec-rentalpro/SPEC.md`
- **Memlog:** `_bmad-output/specs/spec-rentalpro/.memlog.md`
- **P0 pilot:** `docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md`
- **BMad Reference:** `docs/BMAD-REFERENCE.md`
