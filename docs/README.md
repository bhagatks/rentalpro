# RentalPro.ai — Partner Knowledge Base

**Last updated:** 2026-07-05  
**Audience:** Founders, engineers, partners  
**Status:** Pre-MVP spec phase

## Start here

| Doc | What it is |
|-----|------------|
| [HANDOFF-TO-CURSOR.md](./HANDOFF-TO-CURSOR.md) | Vision, business model, locked decisions |
| [BMAD-REFERENCE.md](./BMAD-REFERENCE.md) | How we build (spec → PRD → architecture → code) |
| [MVP-SPRINT-PLAN.md](./MVP-SPRINT-PLAN.md) | Build order, MVP scope, 24/7 sprint map |
| [SHARING-OPTIONS.md](./SHARING-OPTIONS.md) | How to share this with your partner (wiki, site, Notion) |

## Spec & research (canonical)

| Artifact | Path |
|----------|------|
| **SPEC kernel** | `_bmad-output/specs/spec-rentalpro/SPEC.md` |
| **Competitive matrix** | `_bmad-output/specs/spec-rentalpro/competitive-matrix.md` |
| **Decision log** | `_bmad-output/specs/spec-rentalpro/.memlog.md` |

## Capability deep-dives (micro-level)

One page per capability. Fill these **before** coding each module.

| CAP | Title | Doc |
|-----|-------|-----|
| CAP-1 | Portfolio onboarding | [capabilities/CAP-1-portfolio-onboarding.md](./capabilities/CAP-1-portfolio-onboarding.md) |
| CAP-2 | Autonomous leasing | [capabilities/CAP-2-autonomous-leasing.md](./capabilities/CAP-2-autonomous-leasing.md) |
| CAP-3 | Autonomous maintenance | [capabilities/CAP-3-autonomous-maintenance.md](./capabilities/CAP-3-autonomous-maintenance.md) |
| CAP-4 | Autonomous accounting | [capabilities/CAP-4-autonomous-accounting.md](./capabilities/CAP-4-autonomous-accounting.md) |
| CAP-5 | Governance rails | [capabilities/CAP-5-governance-rails.md](./capabilities/CAP-5-governance-rails.md) |
| CAP-6 | Per-module autonomy toggles | [capabilities/CAP-6-module-toggles.md](./capabilities/CAP-6-module-toggles.md) |
| CAP-7 | Resident portal | [capabilities/CAP-7-resident-portal.md](./capabilities/CAP-7-resident-portal.md) |
| CAP-8 | Owner reporting | [capabilities/CAP-8-owner-reporting.md](./capabilities/CAP-8-owner-reporting.md) |
| CAP-9 | Vendor management | [capabilities/CAP-9-vendor-management.md](./capabilities/CAP-9-vendor-management.md) |
| CAP-10 | Audit trail | [capabilities/CAP-10-audit-trail.md](./capabilities/CAP-10-audit-trail.md) |
| CAP-11 | Multi-tenant SaaS | [capabilities/CAP-11-multi-tenant-saas.md](./capabilities/CAP-11-multi-tenant-saas.md) |
| CAP-12 | Smart access (Seam) | [capabilities/CAP-12-smart-access.md](./capabilities/CAP-12-smart-access.md) |

## How docs relate to BMad

```
docs/           → Human wiki (you + partner read this)
_bmad-output/   → Machine contract (agents + PRD consume this)
```

When a CAP deep-dive is locked, append decisions to `.memlog.md` and re-derive `SPEC.md`.
