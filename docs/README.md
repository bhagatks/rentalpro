# RentalPro.ai — Partner Knowledge Base

**Last updated:** 2026-07-05  
**Audience:** Founders, engineers, partners  
**Status:** Pre-MVP spec phase

## Start here

| Doc | What it is |
|-----|------------|
| [**REQ-TRACEABILITY.md**](./REQ-TRACEABILITY.md) | **Chat → req docs map** — where every aligned decision lives |
| [HANDOFF-TO-CURSOR.md](./HANDOFF-TO-CURSOR.md) | Vision, business model, locked decisions |
| [BMAD-REFERENCE.md](./BMAD-REFERENCE.md) | How we build (spec → PRD → architecture → code) |
| [MVP-SPRINT-PLAN.md](./MVP-SPRINT-PLAN.md) | Build order, MVP scope, 24/7 sprint map |
| [AI-MVP-DECISIONS.md](./AI-MVP-DECISIONS.md) | AI agent decisions — locked + TBD (Texas MVP) |
| [MARKET-GAP-CHECKLIST.md](./MARKET-GAP-CHECKLIST.md) | Market parity M1–M10 — M1/M2 locked; M3–M10 open |
| [SYNDICATION-MVP-RUNBOOK.md](./SYNDICATION-MVP-RUNBOOK.md) | **M1 Full MVP** — time-sensitive syndication tasks (single tracker) |
| [DELINQUENCY-RULES-ENGINE.md](./DELINQUENCY-RULES-ENGINE.md) | **M2** — state rules + PM customization (3-layer model) |
| [SHARING-OPTIONS.md](./SHARING-OPTIONS.md) | How to share this with your partner (wiki, site, Notion) |

## Spec & research (canonical)

| Artifact | Path |
|----------|------|
| **SPEC kernel** | `_bmad-output/specs/spec-rentalpro/SPEC.md` |
| **Competitive matrix** | `_bmad-output/specs/spec-rentalpro/competitive-matrix.md` |
| **Decision log** | `_bmad-output/specs/spec-rentalpro/.memlog.md` |

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
