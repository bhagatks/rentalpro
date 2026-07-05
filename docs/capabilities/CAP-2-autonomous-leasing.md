# CAP-2: Autonomous Leasing

**Status:** draft — **screening criteria parked**  
**SPEC reference:** CAP-2  
**MVP phase:** 4  
**Depends on:** CAP-5, CAP-10, CAP-12, CAP-1

## Intent & success (from SPEC)

- **Intent:** Autonomous lead-to-lease—pre-qualification, identity verification, lease execution, first payment, digital key—without human intervention on Professional plan.
- **Success:** Qualified prospect completes inquiry through signed lease and working digital key within 48 hours with zero PM staff touches on Pro; every step logged.

## User stories

| Actor | Story |
|-------|-------|
| Prospect | I inquire via chat 24/7; complete application on branded portal. |
| Leasing agent | I qualify, screen, draft lease, coordinate sign and payment. |
| PM admin | I review lease draft (Basic always; Pro if template deviates). |
| Applicant | I receive specific adverse action notice if denied. |

## Happy path

1. Prospect chats on org subdomain → agent answers unit questions.
2. Agent sends Texas §92.3515 selection criteria **before** application fee; applicant acknowledges.
3. Application + FCRA standalone consent → screening report (integrated API or PM vendor).
4. **Screening decision (TBD):** RentalPro decision engine applies PM criteria → APPROVE / DENY / REVIEW.
5. If approved: Stripe Identity verification.
6. Agent fills lease from PM template + platform Texas starter + required addenda (flood, lead paint, etc.).
7. PM review (Basic always; Pro if exact template match → auto-send TBD).
8. E-sign → first month payment → CAP-12 Seam key.
9. Full CAP-10 trace including criteria version and denial rationale if applicable.

## Escalation path

| Trigger | Approver |
|---------|----------|
| DENY or REVIEW screening | PM admin; adverse action auto-generated |
| Lease draft edited | PM admin before send |
| Identity verification fail | PM admin |
| Basic plan | All significant steps require approval |

## Integrations

| Service | Use |
|---------|-----|
| Screening API + PM vendor | Report sources (A+B) |
| Stripe Identity | ID verification |
| E-sign (TBD) | Lease execution |
| CAP-12 | Digital key |
| CAP-10 | Decision traces |

## Data model (draft)

| Entity | Key fields |
|--------|------------|
| `Lead` | organizationId, unitId, status, source |
| `Application` | organizationId, leadId, criteriaAckAt, consentAt, screeningReportId, decision, traceId |
| `LeaseTemplate` | organizationId, source (pm\|platform), documentUrl, version |
| `LeaseDraft` | organizationId, applicationId, templateVersion, mergeData JSON, status |

## API surface (draft)

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/public/leads` | Inquiry (org from subdomain) |
| POST | `/api/applicant/apply` | Application + fee |
| GET | `/api/orgs/current/applications/:id/decision-card` | Screening trace |
| POST | `/api/orgs/current/leases/:id/send` | Send for e-sign |

## Acceptance tests

- [ ] Criteria notice before fee (Texas §92.3515)
- [ ] FCRA consent before credit pull
- [ ] Denial includes specific adverse action reason
- [ ] Pro: qualified applicant → key within 48h with zero staff touches
- [ ] Signed lease triggers Seam key (CAP-12)
- [ ] Protected classes never in agent inputs

## Open questions

- [ ] **Parked:** Screening criteria defaults — see `docs/AI-MVP-DECISIONS.md`
- [ ] **Parked:** Industry-breaking screening thesis (1+3 recommended)
- [ ] E-sign provider?
- [ ] Pro auto-send exact-match rules?

## Market parity sub-features (TBD)

See `docs/MARKET-GAP-CHECKLIST.md`.

- [ ] Guest card / CRM pipeline (lead stages, source tracking)
- [ ] Listing pages & syndication (M1)
- [ ] Tour / self-showing scheduling (M12)
- [ ] Lease renewal workflow (M3)
- [ ] Renters insurance requirement

## Decisions log

| Date | Decision |
|------|----------|
| 2026-07-05 | Lease A+B: PM template + platform Texas starter |
| 2026-07-05 | AI fills draft; PM reviews; not greenfield generation |
| 2026-07-05 | Screening A+B sources; decision layer TBD |

**See also:** `docs/AI-MVP-DECISIONS.md`
