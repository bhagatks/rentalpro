---
title: Product Brief — RentalPro.ai
status: draft
created: 2026-07-04
updated: 2026-07-24
sources:
  - ../../../specs/spec-rentalpro/SPEC.md
  - ../../../../docs/HANDOFF-TO-CURSOR.md
  - ../../../../docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md
---

# Product Brief: RentalPro.ai

*North Star for partners and agents. Distilled from the locked SPEC Why / Success signal and the P0 pilot gate. Spec kernel remains canonical for build contract: `_bmad-output/specs/spec-rentalpro/SPEC.md`.*

## Executive Summary

RentalPro.ai is an AI-native **Autonomous Property Management (APM)** platform sold as multi-tenant B2B SaaS to existing property management companies. Legacy PMS tools (AppFolio, Buildium, Yardi) help human managers; RentalPro **replaces** the human coordinator for routine leasing, maintenance, and accounting work.

PM companies today spend roughly **10% of rent** on administrative overhead. The product thesis is to collapse that Operating Expense Ratio toward **~2%** by closing the loop with LLMs, Seam smart locks, and Stripe/Plaid money rails—under tiered governance the PM company controls.

**Near-term north star:** prove the five biggest assumptions in a **6-week Texas pilot** (`docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md`) before committing to a full 12-CAP MVP build.

## The Problem

Independent PM companies hit a scalability ceiling: office staff, VAs, and property managers coordinate leasing, maintenance, and accounting by hand. Tenants expect 24/7 digital response; owners expect accurate distributions; regulators expect FHA/FCRA-safe screening and Texas Property Code compliance.

Coping today means more headcount or more VA spend as unit counts grow. That administrative tax is the pain. The opportunity is that agent workflows + hardware/payment APIs can now execute closed loops that used to require a human in the middle of every thread.

## The Solution

A white-label, multi-tenant APM OS for PM firms:

- **Autonomous leasing** — inquiry → pre-qualify → identity → lease → first payment → digital key
- **Autonomous maintenance** — resident media → diagnose → vendor quote → schedule → verify → pay (within spend rails)
- **Autonomous accounting** — API-fed double-entry ledger; AI categorizes 100%; human monthly sign-off before distributions
- **Governance** — Basic ($29/mo) human-approves significant actions; Professional ($99/mo) high autonomy with configurable rails (default $500 maintenance auto-approve)
- **Module toggles** — AI-driven vs human-in-the-loop per module (leasing / maintenance / accounting), never a global override of tenant config

Direct-to-landlord operation is an **internal lab**, not initial GTM.

## What Makes This Different

| Axis | Legacy PMS | RentalPro |
|------|------------|-----------|
| Buyer job | Tools for human managers | Replace routine human coordination |
| Autonomy | Features + dashboards | Closed-loop agents with audit + rails |
| Tenancy | Often single-org or bolted-on | Multi-tenant + RLS from Day 1 |
| Segment | Enterprise multifamily (e.g. Entrata APM™) | Independent PM companies (~10–500 units) |
| Compliance | Manual process | FHA/FCRA + Texas-first rule packs by design |

Honest moat today: **execution + governance + Texas compliance depth**, not a patented model. Competitive research: `_bmad-output/specs/spec-rentalpro/competitive-matrix.md`.

## Who This Serves

| Persona | Need | Success looks like |
|---------|------|--------------------|
| **PM company admin** (primary buyer) | Cut admin OER without losing control of money/compliance | Fewer staff hours; approvals only by exception |
| **Resident** | Fast digital self-serve | Pay rent / file maintenance without calling the office |
| **Owner** | Accurate statements + distributions | Monthly report matches ledger; distributions after sign-off |
| **Vendor** | Clear jobs + timely pay | Quote → schedule → completion → Stripe Connect payout |

## Success Criteria

### Locked product success signal (full APM — SPEC)

Texas PM company, **50–100 units**, within **14 days** of go-live:

- ≥80% routine interactions without staff intervention
- Zero manual journal entries; one accountant monthly sign-off
- ≥15 hours/week coordination time saved (self-report)
- Qualified prospect → signed lease → working key in ≤48 hours on Pro
- Zero cross-tenant leakage in penetration test

### P0 pilot gate (before full MVP build)

Texas partner, **10–50 units**, **6 weeks** — validate ≥4 of 5 assumptions (see pilot doc):

1. PMs adopt autonomous maintenance (≥70% AI dispatch approval)
2. Fair-housing/FCRA path clean on audit
3. Time savings real (≥8 hrs/week freed in pilot as proxy for 15-hr bet)
4. Multi-tenant RLS holds (zero leakage)
5. Residents adopt portal (≥50% rent via portal)

## Scope

### In for MVP (kernel + locked market gaps)

- 12 CAP intents in SPEC (CAP-11 micro-spec locked; others draft)
- Texas jurisdiction only; chat/SMS/email (no voice)
- Required partners: Seam, Stripe, Plaid
- Subdomain branding `{slug}.rentalpro.ai`
- Locked market gaps: **M1–M5, M7** MVP; **M6** non-goal (eviction outside platform)
- CAP-1: owner **management fee locked at 7%** (no per-property override)

### Explicitly out (Non-goals)

Native mobile apps, custom domain white-label, enterprise SAML, live legacy PMS sync, non-Texas states, full eviction workflow, recurring/preventive maintenance, AP outside work orders, voice/IVR, community feed, self-showing tours, 1099 tax reporting — see SPEC §Non-goals.

### Open before CAP lock

M8 (owner portal UX), M9 (open API/webhooks), M10 (document vault), screening criteria defaults (parked).

## Vision

If the pilot and MVP succeed: RentalPro becomes the default operating system for independent PM companies that want **management on autopilot with governance by exception**—expanding state-by-state after Texas proof, then deepening white-label and enterprise auth as deals require.

## Pricing (locked direction)

| Plan | Price | Autonomy |
|------|-------|----------|
| Basic | $29/mo | AI interpreter; human approves significant actions / spend |
| Professional | $99/mo | High autonomy; configurable rails (e.g. auto-approve repairs ≤ $500) |

## Canonical artifacts

| Role | Path |
|------|------|
| Spec kernel (build contract) | `_bmad-output/specs/spec-rentalpro/SPEC.md` |
| Decision log | `_bmad-output/specs/spec-rentalpro/.memlog.md` |
| P0 pilot plan | `docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md` |
| Partner handoff | `docs/HANDOFF-TO-CURSOR.md` |
| CAP micro-specs | `docs/capabilities/` |
