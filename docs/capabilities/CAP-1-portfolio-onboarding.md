# CAP-1: Portfolio Onboarding

**Status:** draft  
**SPEC reference:** CAP-1  
**MVP phase:** 1  
**Depends on:** CAP-11

## Intent & success (from SPEC)

- **Intent:** PM company onboards portfolio (properties, units, owners, tenants, vendors) into isolated tenant workspace with imported historical records.
- **Success:** PM admin uploads 50-unit portfolio; all units, active leases, and vendor records accessible in branded workspace within one business day of import completion.

## User stories

| Actor | Story |
|-------|-------|
| PM admin | I import properties/units via CSV or manual entry. |
| PM admin | I attach owners to properties and set management fee %. |
| PM admin | I import active leases with tenant contact info. |
| PM admin | I import vendor list with trade categories. |
| PM staff | I see only my org's portfolio after import. |

## Happy path

1. PM admin completes org setup (CAP-11).
2. Upload CSV templates: properties → units → owners → leases → vendors (or API bulk import).
3. System validates Texas address fields; maps units to properties.
4. Active leases create resident portal invites (CAP-7).
5. Vendors available to maintenance agent (CAP-9).
6. Chart of accounts template applied (CAP-4).
7. Import job status visible; errors row-level with fix-and-retry.

## Escalation path

| Trigger | Action |
|---------|--------|
| Duplicate unit/property | Block row; PM fixes CSV |
| Missing owner on managed property | Warning; block distribution until resolved |
| Invalid lease dates | Row error; manual fix |

## Integrations

| Service | Use |
|---------|-----|
| Supabase Storage | CSV upload |
| CAP-11 | organizationId on all imported rows |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `Property` | organizationId, name, address, ownerId, builtYear, floodplainFlag |
| `Unit` | organizationId, propertyId, unitNumber, rent, status |
| `Owner` | organizationId, name, email, portalUserId |
| `Lease` | organizationId, unitId, tenantId, startDate, endDate, rent, status |
| `ImportJob` | organizationId, type, status, errorLog JSON |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/orgs/current/import/:type` | Upload CSV |
| GET | `/api/orgs/current/import/:jobId` | Job status |
| POST | `/api/orgs/current/properties` | Manual CRUD |
| POST | `/api/orgs/current/units` | Manual CRUD |

## Acceptance tests

- [ ] 50-unit CSV import completes; all units searchable in org workspace
- [ ] Imported data not visible to other orgs
- [ ] Active lease creates resident record linked to unit
- [ ] Vendor import available in maintenance dispatch

## Open questions

- [ ] Supported import formats beyond CSV (AppFolio export, Buildium)?
- [ ] Historical ledger import scope for MVP?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Draft micro-spec; Texas property fields required (floodplain, builtYear for lease addenda) |
