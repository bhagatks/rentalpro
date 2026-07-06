# Architecture Documentation — RentalPro.ai

**Canonical contract:** [`ARCHITECTURE-SPINE.md`](../../_bmad-output/planning-artifacts/architecture/architecture-rentalpro-2026-07-05/ARCHITECTURE-SPINE.md) (17 binding decisions, AD-1 … AD-17, status: final)
**Partner walkthrough:** [`../ARCHITECTURE-OVERVIEW.md`](../ARCHITECTURE-OVERVIEW.md) — plain-language tour, start here if you're new
**Decision rationale + review history:** [`.memlog.md`](../../_bmad-output/planning-artifacts/architecture/architecture-rentalpro-2026-07-05/.memlog.md) and [`reviews/`](../../_bmad-output/planning-artifacts/architecture/architecture-rentalpro-2026-07-05/reviews/)

This folder holds the **detailed technical companions** to the spine — expanded diagrams and reference tables an engineer needs while building, kept separate from the spine itself so the spine stays terse and binding while these can carry full implementation-level detail. Every doc here cites spine AD IDs rather than re-explaining them; if a rule changes, it changes in the spine first.

## What's here

| Doc | Covers | Read this when |
|---|---|---|
| [`flows.md`](./flows.md) | 5 full sequence diagrams: lead→lease→key, maintenance dispatch, delinquency→fee, move-in/out inspection, monthly close — plus the shared 4-beat rhythm (evaluate → trace → act → notify) every autonomous flow follows | You're implementing or reviewing any autonomous workflow |
| [`database-schema.md`](./database-schema.md) | Full logical schema: every table, column, type, RLS policy, index, and owning module | You're writing a migration or a Drizzle schema file |
| [`api-structure.md`](./api-structure.md) | tRPC router tree, every procedure's input/output shape and role gate, middleware stack, the `publicProcedure` pattern, sanctioned non-tRPC surfaces | You're adding or calling a tRPC procedure |
| [`integrations.md`](./integrations.md) | Per-vendor port interfaces (Stripe, Seam, Plaid/bank-feed, Twilio, Resend, screening, e-sign), webhook translation, idempotency strategy, vendor-selection status | You're wiring up or swapping an external integration |
| [`event-catalog-and-states.md`](./event-catalog-and-states.md) | Every Inngest event (name, envelope, producer/consumer), plus state machines for `ApprovalRequest`, `Lease`, `WorkOrder`, `DelinquencyEvent`, `DecisionTrace` | You're emitting/consuming an event or modeling a status field |

## Per-capability architecture

Each of the 12 CAP docs in [`../capabilities/`](../capabilities/) now carries its own **`## Architecture`** section: owning modules, the specific ADs that govern it, a flow diagram, and its cross-CAP dependencies. Start at the CAP doc for capability-specific detail; come here for cross-cutting reference.

| CAP | Doc |
|---|---|
| CAP-1 Portfolio onboarding | [`CAP-1-portfolio-onboarding.md`](../capabilities/CAP-1-portfolio-onboarding.md) |
| CAP-2 Autonomous leasing | [`CAP-2-autonomous-leasing.md`](../capabilities/CAP-2-autonomous-leasing.md) |
| CAP-3 Autonomous maintenance | [`CAP-3-autonomous-maintenance.md`](../capabilities/CAP-3-autonomous-maintenance.md) |
| CAP-4 Autonomous accounting | [`CAP-4-autonomous-accounting.md`](../capabilities/CAP-4-autonomous-accounting.md) |
| CAP-5 Governance rails | [`CAP-5-governance-rails.md`](../capabilities/CAP-5-governance-rails.md) |
| CAP-6 Module toggles | [`CAP-6-module-toggles.md`](../capabilities/CAP-6-module-toggles.md) |
| CAP-7 Resident portal | [`CAP-7-resident-portal.md`](../capabilities/CAP-7-resident-portal.md) |
| CAP-8 Owner reporting | [`CAP-8-owner-reporting.md`](../capabilities/CAP-8-owner-reporting.md) |
| CAP-9 Vendor management | [`CAP-9-vendor-management.md`](../capabilities/CAP-9-vendor-management.md) |
| CAP-10 Audit trail | [`CAP-10-audit-trail.md`](../capabilities/CAP-10-audit-trail.md) |
| CAP-11 Multi-tenant SaaS (locked) | [`CAP-11-multi-tenant-saas.md`](../capabilities/CAP-11-multi-tenant-saas.md) |
| CAP-12 Smart access | [`CAP-12-smart-access.md`](../capabilities/CAP-12-smart-access.md) |

## How these docs relate

```
_bmad-output/.../ARCHITECTURE-SPINE.md   ← binding contract, terse, AD-1..AD-17
        │
        ├── docs/ARCHITECTURE-OVERVIEW.md        ← plain-language partner tour
        │
        ├── docs/architecture/*.md               ← cross-cutting technical detail (this folder)
        │
        └── docs/capabilities/CAP-N-*.md          ← per-capability detail
                (## Architecture section cites the same AD IDs)
```

If a technical doc here ever conflicts with the spine, **the spine wins** — file it as a spine update (`bmad-architecture`, Update intent), not a silent edit here.

## Status

All docs in this folder and all 12 CAP `## Architecture` sections were generated 2026-07-05 directly from the finalized spine (post-review-gate). Two CAP docs' open questions were partially resolved in the process: CAP-4's "Plaid vs Stripe-only" is now a vendor-agnostic bank-feed port (see `integrations.md`), and CAP-8 was clarified as a pure read layer over CAP-4's ledger rather than an independently-owned entity.
