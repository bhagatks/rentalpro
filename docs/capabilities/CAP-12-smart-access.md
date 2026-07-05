# CAP-12: Smart Access (Seam)

**Status:** draft  
**SPEC reference:** CAP-12  
**MVP phase:** 4  
**Depends on:** CAP-2, CAP-11

## Intent & success (from SPEC)

- **Intent:** Smart-access integration so identity-verified applicants receive time-bound digital keys upon lease activation.
- **Success:** Signed lease triggers Seam key issuance; tenant unlocks unit within 15 minutes of lease activation without manual key handoff.

## User stories

| Actor | Story |
|-------|-------|
| Leasing agent | I issue time-bound digital key after lease signed + payment confirmed. |
| New tenant | I unlock unit door via phone within 15 minutes of lease activation. |
| PM admin | I see lock status and access events per unit. |
| PM admin | I revoke key on lease termination. |

## Happy path

1. Unit configured with Seam device ID during onboarding (CAP-1).
2. Lease signed + first payment confirmed (CAP-2).
3. Agent calls Seam API: create access grant for verified tenant identity.
4. Key valid from lease start date/time; bound to tenant device.
5. Access events logged (CAP-10).
6. Lease ends → agent revokes access grant.

## Escalation path

| Trigger | Action |
|---------|--------|
| Seam API failure | Notify PM; manual key fallback workflow |
| Identity mismatch | Block key issuance; PM review |
| Device offline | Notify PM + tenant; schedule physical key |

## Integrations

| Service | Use |
|---------|-----|
| Seam API | Lock control, access grants |
| Stripe Identity | Verified identity linked to grant |
| CAP-2 | Lease activation trigger |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `SmartLock` | organizationId, unitId, seamDeviceId, status |
| `AccessGrant` | organizationId, leaseId, tenantId, seamAccessCodeId, validFrom, validUntil, status |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/orgs/current/units/:id/locks` | Register Seam device |
| POST | `/api/internal/access/issue` | Agent issues grant (internal) |
| DELETE | `/api/internal/access/:id` | Revoke grant |
| GET | `/api/orgs/current/units/:id/access-events` | Access log |

## Acceptance tests

- [ ] Signed lease + payment → key works within 15 minutes
- [ ] Revoked lease → key stops working
- [ ] Access issue logged in CAP-10
- [ ] Wrong tenant cannot receive key for another unit

## Open questions

- [ ] Supported lock brands for MVP (Schlage, Yale, August via Seam)?
- [ ] Self-showing keys for prospects pre-lease — Phase 2?

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Seam as integration partner (HANDOFF direction) |
