# RentalPro.ai — Partner Knowledge Base

**Last updated:** 2026-07-05  
**Audience:** Founders, engineers, partners  
**Status:** Pre-MVP spec phase

## For partners

| Doc | What it is |
|-----|------------|
| [PARTNER-START-HERE.md](./partner/PARTNER-START-HERE.md) | Partner onboarding — GitHub, BMad install, Cursor + Claude Code |
| [PARTNER-GITHUB-SETUP.md](./partner/PARTNER-GITHUB-SETUP.md) | GitHub invite and clone — both partners |

## Start here

| Doc | What it is |
|-----|------------|
| [ARCHITECTURE-OVERVIEW.md](./ARCHITECTURE-OVERVIEW.md) | **Architecture walkthrough** — stack, boundaries, why (spine: `_bmad-output/planning-artifacts/architecture/`) |
| [architecture/README.md](./architecture/README.md) | **Technical architecture reference** — schema, API, event catalog, flows, integrations |
| [REQ-TRACEABILITY.md](./REQ-TRACEABILITY.md) | **Chat → req docs map** — where every aligned decision lives |
| [templates/MARKET-GAP-REQ-TEMPLATE.md](./templates/MARKET-GAP-REQ-TEMPLATE.md) | **9-section template** for full M1–M10 requirements |
| [HANDOFF-TO-CURSOR.md](./HANDOFF-TO-CURSOR.md) | Vision, business model, locked decisions |
| [BMAD-REFERENCE.md](./BMAD-REFERENCE.md) | How we build (spec → PRD → architecture → code) |
| [MVP-SPRINT-PLAN.md](./MVP-SPRINT-PLAN.md) | Build order, MVP scope, 24/7 sprint map |
| [AI-MVP-DECISIONS.md](./AI-MVP-DECISIONS.md) | AI agent decisions — locked + TBD (Texas MVP) |
| [MARKET-GAP-CHECKLIST.md](./MARKET-GAP-CHECKLIST.md) | M1–M5 + M7 locked; M6 non-goal; M8–M10 open |
| [SYNDICATION-MVP-RUNBOOK.md](./SYNDICATION-MVP-RUNBOOK.md) | **M1 Full MVP** — time-sensitive syndication tasks (single tracker) |
| [ATTORNEY-REVIEW-CHECKLIST.md](./ATTORNEY-REVIEW-CHECKLIST.md) | **Master Texas attorney review** — all M-areas; **Mandatory vs Optional** tags + sign-off log |
| [UNIFIED-COMMS-HUB-MVP-REQ.md](./UNIFIED-COMMS-HUB-MVP-REQ.md) | **M7** — unified inbox, SMS/email/chat, bulk emergency |
| [SHARING-OPTIONS.md](./SHARING-OPTIONS.md) | How to share this with your partner (wiki, site, Notion) |
| [**MOBILE-APP-PHASE2-GAP-DISCUSSION.md**](./MOBILE-APP-PHASE2-GAP-DISCUSSION.md) | **Mobile (Phase 2)** — existing design inventory + gaps to discuss before native apps |

## Spec & research (canonical)

| Artifact | Path |
|----------|------|
| **SPEC kernel** | `_bmad-output/specs/spec-rentalpro/SPEC.md` |
| **Competitive matrix** | `_bmad-output/specs/spec-rentalpro/competitive-matrix.md` |
| **Decision log** | `_bmad-output/specs/spec-rentalpro/.memlog.md` |

## P0: Pilot Plan — Risk Mitigation (LOCKED 2026-07-06)

**Goal:** Validate 5 biggest product assumptions early, low cost, before full MVP build.

| Doc | What it is |
|-----|------------|
| [**PILOT-P0-ASSUMPTIONS-VALIDATION.md**](./PILOT-P0-ASSUMPTIONS-VALIDATION.md) | **6-week pilot plan.** Top 5 assumptions (ranked by risk), validation strategy per assumption, success/go-no-go criteria, resource footprint (~6 dev-weeks), pilot partner profile, contingency plans. Primary gate: **≥4 of 5 assumptions validated → proceed to MVP build.** |

**At a glance:**
- **Assumption #1:** PM companies will adopt autonomous workflows (test: week 5, ≥70% approval rate)
- **Assumption #2:** Fair-housing/FCRA compliance baked into AI (test: week 3–6, zero violations audit)
- **Assumption #3:** 15 hrs/week time savings is real (test: week 1–6, PM self-report ≥8 hrs/week freed)
- **Assumption #4:** Multi-tenant architecture holds (test: week 1, RLS isolation verified)
- **Assumption #5:** Residents adopt self-serve portal (test: week 2–6, ≥50% adoption)

**Scope:** CAP-1 (onboarding) + CAP-3 (maintenance autonomy) + CAP-4 (accounting) + CAP-7 (resident portal) + CAP-11 (multi-tenant isolation)  
**Partner:** Texas PM company, 10–50 units  
**Timeline:** 6 weeks; decision gate week 6

---

## Capability deep-dives (micro-level)

One page per capability. **Gate:** sections 1–3 and 7 filled before coding.

| CAP | Title | Status | Doc |
|-----|-------|--------|-----|
| CAP-1 | Portfolio onboarding | draft | [CAP-1-portfolio-onboarding.md](./capabilities/CAP-1-portfolio-onboarding.md) |
| CAP-2 | Autonomous leasing | draft 🅿️ screening | [CAP-2-autonomous-leasing.md](./capabilities/CAP-2-autonomous-leasing.md) |
| CAP-3 | Autonomous maintenance | draft | [CAP-3-autonomous-maintenance.md](./capabilities/CAP-3-autonomous-maintenance.md) |
| CAP-4 | Autonomous accounting | draft | [CAP-4-autonomous-accounting.md](./capabilities/CAP-4-autonomous-accounting.md) |
| CAP-5 | Governance rails | draft | [CAP-5-governance-rails.md](./capabilities/CAP-5-governance-rails.md) |
| CAP-6 | Per-module autonomy toggles | draft | [CAP-6-module-toggles.md](./capabilities/CAP-6-module-toggles.md) |
| CAP-7 | Resident portal | draft | [CAP-7-resident-portal.md](./capabilities/CAP-7-resident-portal.md) |
| CAP-8 | Owner reporting | draft | [CAP-8-owner-reporting.md](./capabilities/CAP-8-owner-reporting.md) |
| CAP-9 | Vendor management | draft | [CAP-9-vendor-management.md](./capabilities/CAP-9-vendor-management.md) |
| CAP-10 | Audit trail | draft | [CAP-10-audit-trail.md](./capabilities/CAP-10-audit-trail.md) |
| CAP-11 | Multi-tenant SaaS | **locked** | [CAP-11-multi-tenant-saas.md](./capabilities/CAP-11-multi-tenant-saas.md) |
| CAP-12 | Smart access (Seam) | draft | [CAP-12-smart-access.md](./capabilities/CAP-12-smart-access.md) |

## Agent & engineering rules

| Doc | What it is |
|-----|------------|
| [AGENTS.md](../AGENTS.md) | Agent entry point — project identity, locked decisions, conventions |
| [docs/rules/](./rules/) | Shared rules (BMad, docs, multi-tenant, logging, testing) |
| [.cursor/rules/](../.cursor/rules/) | Cursor rule files (`@include` docs/rules/) |
| [EASYSUBMIT-RULES-CAPTURE.md](./EASYSUBMIT-RULES-CAPTURE.md) | Reference archive of EasySubmit's rule pattern (source project) |

## How docs relate to BMad

```
docs/           → Human wiki (you + partner read this)
docs/rules/     → Shared agent rules (canonical for Cursor + Claude Code)
_bmad-output/   → Machine contract (agents + PRD consume this)
```

When a CAP deep-dive is locked, append decisions to `.memlog.md` and re-derive `SPEC.md`.
