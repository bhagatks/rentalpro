# CAP-11: Multi-Tenant B2B SaaS

**Status:** draft — **start here** (foundation for all other CAPs)  
**SPEC reference:** CAP-11  
**MVP phase:** 0

## Intent & success (from SPEC)

- **Intent:** Platform operates as multi-tenant B2B SaaS with per-PM-company data isolation, white-label branding, and independent configuration.
- **Success:** Two PM companies cannot access each other's data; each presents distinct branding in a penetration test.

## User stories

| Actor | Story |
|-------|-------|
| PM admin | As a PM company admin, I want my own branded workspace so my owners/residents see my brand, not RentalPro. |
| Platform ops | As RentalPro ops, I want tenant isolation so one PM company's data never leaks to another. |
| PM admin | As a PM admin, I want to configure governance defaults for my org without affecting other PM companies on the platform. |

## Happy path

1. PM company signs up → `Organization` created with `slug`, branding (logo, colors, domain).
2. All data scoped by `organizationId` (row-level security).
3. Residents/owners access `pm-company.rentalpro.ai` or custom domain.
4. Each org has independent: governance rails, module toggles, vendor DB, chart of accounts template.

## Escalation path

N/A — infrastructure CAP. Security incidents escalate to platform ops.

## Integrations

| Service | Use |
|---------|-----|
| Auth (TBD) | Org-scoped users, roles: platform-admin, pm-admin, pm-staff, owner, resident |
| Stripe Connect | Per-org payment accounts (CAP-4) |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| Organization | id, name, slug, plan (basic/pro), branding, settings JSON |
| User | id, organizationId, role, email |
| OrganizationSettings | governanceDefaults, moduleToggles, whiteLabel |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/orgs` | Create org (platform admin) |
| GET | `/api/orgs/:id/settings` | Org config |
| PATCH | `/api/orgs/:id/branding` | White-label |

## Acceptance tests

- [ ] User A (org 1) cannot read org 2 properties via API or UI
- [ ] Org 1 and org 2 show different logos on resident portal
- [ ] SQL injection / IDOR attempts on `organizationId` fail

## Competitive notes

**No competitor in matrix offers white-label multi-tenant APM for PM companies.** AppFolio/Buildium/DoorLoop are single-tenant per customer account, not a platform sold to many PM firms.

## Open questions

- [ ] Custom domain per PM company in MVP or subdomain only?
- [ ] Auth provider: Clerk, Auth0, or custom?
- [ ] Single DB with RLS vs schema-per-tenant?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-04 | Multi-tenancy from Day 1 (HANDOFF locked) |
