# Multi-tenant SaaS rules

Non-negotiable constraints for RentalPro architecture and implementation. Derived from locked spec decisions and CAP-11.

## Core principle

Every PM company operates in an **isolated tenant workspace**. Data, branding, governance rails, and audit trails must never leak across organizations.

## Rules (enforce from Day 1)

### Data isolation

- All tenant-scoped tables include `organizationId` (or equivalent tenant FK)
- Row-level security or application-level tenant filter on **every** query — no exceptions
- Never return cross-tenant data from API routes, server actions, or background jobs
- Shared reference data (jurisdictions, tax codes) is read-only and not tenant-specific

### Branding

- PM companies get white-label surfaces: logo, colors, domain/subdomain
- Default RentalPro branding only for internal lab / direct-to-landlord mode

### Governance per tenant

- Approval thresholds, spend limits, and escalation triggers are **per organization**
- Optionally overridable per property within an organization
- Basic vs Professional plan limits enforced at tenant level

### Module autonomy toggles (CAP-6)

- Each tenant configures AI-driven vs human-in-the-loop **per module** (leasing, maintenance, accounting)
- Toggle state stored per organization; never global defaults that override tenant choice

### Audit trail (CAP-10)

- Every autonomous action logs: tenant, actor (AI agent or human), timestamp, inputs, outcome
- Audit records are tenant-scoped and immutable
- Retention meets tax/legal requirements per tenant jurisdiction (TBD in Constraints phase)

## Schema conventions (Drizzle)

> ORM decision superseded 2026-07-05 by ARCHITECTURE-SPINE AD-2/AD-11 (`_bmad-output/planning-artifacts/architecture/architecture-rentalpro-2026-07-05/`): Drizzle over Prisma — first-class Postgres RLS support.

When tables are created:

- IDs are UUIDv7, database-generated
- Include `created_at` and `updated_at` on all tables
- Add an index on `organization_id` and frequently queried fields
- Tenant-scoped uniqueness is composite (e.g. unique on `(organization_id, slug)`)
- Every tenant-scoped table carries a `FORCE ROW LEVEL SECURITY` policy on `organization_id = current_setting('app.current_org_id')::uuid`
- App connects as a non-superuser role; the Supabase service-role key never appears in application code paths

## What NOT to do

- ❌ Global feature flags that bypass tenant isolation
- ❌ Hard-coded single-tenant assumptions in business logic
- ❌ Shared mutable state between tenants in cache without tenant key
- ❌ Admin "super user" views without explicit audit logging
- ❌ Deferring multi-tenancy to "Phase 2"

## Related capabilities

- **CAP-1:** Portfolio onboarding into isolated workspace
- **CAP-5:** Governance rails per organization
- **CAP-6:** Per-module autonomy toggles
- **CAP-11:** Multi-tenant SaaS platform

See `docs/capabilities/CAP-11-multi-tenant-saas.md` for the capability deep-dive.
