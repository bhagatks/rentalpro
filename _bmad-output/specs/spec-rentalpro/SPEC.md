---
id: SPEC-rentalpro
companions:
  - competitive-matrix.md
  - ../../../docs/AI-MVP-DECISIONS.md
sources:
  - ../../docs/HANDOFF-TO-CURSOR.md
  - ../../docs/AI-MVP-DECISIONS.md
---

> **Canonical contract.** This SPEC and the files in `companions:` are the complete, preservation-validated contract for what to build, test, and validate.

# RentalPro.ai — Autonomous Property Management Platform

## Why

Property management companies hit a scalability ceiling: roughly 10% of rent goes to administrative overhead—office staff, VAs, and human property managers coordinating leasing, maintenance, and accounting. RentalPro.ai exists to collapse that Operating Expense Ratio toward 2% by replacing the human coordinator with an AI-native Autonomous Property Management (APM) platform sold as B2B SaaS to existing PM firms.

The force is a **pain to solve** (administrative tax) combined with an **opportunity to capture** (LLM maturity, Seam smart-lock APIs, and Stripe identity/payments now make closed-loop autonomous workflows reliable). The target is PM companies managing residential portfolios who want to eliminate routine staff work while retaining governance over significant financial decisions.

Tiered autonomy addresses market spectrum and compliance: Basic plan ($29/mo) makes AI the interpreter—handling all communication but requiring human approval for significant actions; Professional plan ($99/mo) grants high autonomy with configurable governance rails (e.g., auto-approve repairs up to $500). Direct-to-landlord operation serves as an internal lab, not initial GTM.

## Capabilities

*Draft — 12 CAP micro-specs in `docs/capabilities/` (CAP-11 locked; others draft).*

- **CAP-1** *(draft — `docs/capabilities/CAP-1-portfolio-onboarding.md`)*
  - **intent:** A PM company can onboard a new client portfolio (properties, units, owners, tenants, vendors) into an isolated tenant workspace with imported historical records.
  - **success:** A PM admin uploads a 50-unit portfolio and sees all units, active leases, and vendor records accessible in their branded workspace within one business day of import completion.

- **CAP-2**
  - **intent:** The system autonomously executes the lead-to-lease lifecycle—pre-qualification, identity verification, lease execution, first-payment confirmation, and digital key issuance—without human intervention on the Professional plan.
  - **success:** A qualified prospect completes inquiry through signed lease and receives a working digital key within 48 hours with zero PM staff touches; every step is logged.

- **CAP-3**
  - **intent:** The system autonomously executes maintenance from resident report through resolution—video/photo diagnosis, vendor quote, scheduling, completion verification, and vendor payment—within configured spend governance.
  - **success:** A non-emergency maintenance request submitted with video is triaged, dispatched, completed, and paid without human action when estimated cost is under the PM company's configured threshold; resident confirms resolution.

- **CAP-4**
  - **intent:** The system maintains a fully automated double-entry ledger fed by payment and bank APIs, with AI categorizing every transaction and flagging the monthly period for human sign-off before owner distributions.
  - **success:** 100% of transactions in a closed month are auto-categorized with zero manual journal entries; accountant approves one monthly report and owner distributions calculate correctly.

- **CAP-5**
  - **intent:** A PM company admin can configure tiered governance rails—approval thresholds, escalation triggers, and spend limits—per organization and optionally per property.
  - **success:** An action exceeding the configured threshold (e.g., $500 repair) is blocked from autonomous execution and routed to a human approver with full context; sub-threshold actions proceed without queue.

- **CAP-6**
  - **intent:** A PM company admin can toggle AI-driven versus human-in-the-loop mode independently for each operational module (leasing, maintenance, accounting).
  - **success:** PM admin sets maintenance to autonomous and leasing to human-approve; a new lead queues for human review while a maintenance request under threshold auto-executes.

- **CAP-7**
  - **intent:** Residents can self-serve via a portal or messaging channel for rent payment, maintenance requests, lease documents, and status updates 24/7.
  - **success:** A resident submits a maintenance request and pays rent through the portal without staff contact; both actions reflect in the ledger and work-order system within 5 minutes.

- **CAP-8**
  - **intent:** Property owners receive automated financial statements, distribution calculations, and portfolio performance summaries on a defined schedule.
  - **success:** Owner logs into portal and views accurate month-to-date statement matching ledger totals; distribution amount matches accountant-approved monthly close.

- **CAP-9**
  - **intent:** The system manages a vendor database and executes vendor outreach, quote collection, scheduling, and payment via integrated payment rails.
  - **success:** A dispatched work order results in vendor quote, accepted appointment, completion photo, and Stripe Connect payment recorded against the work order without duplicate data entry.

- **CAP-10**
  - **intent:** Every autonomous agent action produces an immutable audit record sufficient for tax, legal, and fair-housing review.
  - **success:** An auditor can trace any autonomous leasing or maintenance decision to inputs, policy applied, actions taken, and human overrides within 60 seconds.

- **CAP-11** *(micro-spec locked — see `docs/capabilities/CAP-11-multi-tenant-saas.md`)*
  - **intent:** The platform operates as multi-tenant B2B SaaS with per-PM-company data isolation, white-label branding, and independent configuration.
  - **success:** Two PM companies on the platform cannot access each other's data; each presents distinct branding to their owners and residents in a penetration test.

- **CAP-12**
  - **intent:** The system integrates smart-access hardware so that identity-verified applicants receive time-bound digital keys upon lease activation.
  - **success:** A signed lease triggers Seam API key issuance; the tenant unlocks the unit door within 15 minutes of lease activation without manual key handoff.

## Constraints

*Locked 2026-07-05 — guided session.*

1. **Multi-tenancy from Day 1.** Every tenant-scoped row carries `organizationId`; PostgreSQL RLS is the backstop. Cross-org data access is a security incident, not a bug. (CAP-11)
2. **Texas jurisdiction only for MVP.** Federal FHA/FCRA plus Texas Property Code (§92.x). No other state lease templates, screening rules, or trust-account logic until Texas is validated.
3. **Tax-ready double-entry accounting.** All transactions flow through a balanced ledger fed by payment/bank APIs. AI auto-categorizes 100%; human accountant signs off monthly before owner distributions. No manual journal entries in steady state. (CAP-4)
4. **Immutable audit trail for autonomous actions.** Every agent decision traceable to inputs, policy version, actions, and human overrides within 60 seconds. Append-only storage; no application-level deletes. (CAP-10)
5. **Fair-housing and FCRA compliance by design.** Screening criteria, agent inputs, and adverse-action notices must not discriminate on protected classes. Texas §92.3515 criteria notice before application fee; standalone FCRA consent before credit pull.
6. **Tiered governance rails are non-bypassable.** Basic plan requires human approval for significant actions. Professional plan auto-approves only within configured limits (default $500 maintenance). Emergency list auto-dispatch bypasses spend approval but still logs. (CAP-5)
7. **Communication channels: chat, SMS, email only.** No voice calls or IVR in MVP.
8. **Required integration partners.** Seam (smart access), Stripe (payments/identity/Connect), Plaid (bank feeds). Architecture phase may add alternates; MVP workflows assume these APIs.
9. **Subdomain branding only in MVP.** `{slug}.rentalpro.ai` per PM company; custom domain and full white-label are Phase 2. (CAP-11)

## Non-goals

*Locked 2026-07-05 — guided session. Explicit out-of-scope for MVP. Market-gap items M1–M10 are **open questions** — not pre-assigned here.*

| Area | Non-goal | Rationale |
|------|----------|-----------|
| **Platform** | Native iOS/Android apps | Responsive web only; mobile apps Phase 2 |
| **Platform** | Custom domain white-label | Subdomain MVP sufficient; CNAME + SSL Phase 2 |
| **Platform** | Enterprise SAML/SSO | Clerk orgs sufficient; Auth0/SAML when enterprise deals require |
| **Platform** | Bidirectional sync with legacy PMS (AppFolio, Yardi) | Import-onboarding only (CAP-1); no live sync |
| **Geography** | States outside Texas | Expand state-by-state after Texas validation |
| **Leasing** | Renters insurance requirement enforcement | Phase 2 |
| **Operations** | Recurring / preventive maintenance schedules | Phase 2 |
| **Operations** | Maintenance chargeback to tenant | Phase 2 |
| **Accounting** | AP / vendor bills outside work orders | Phase 2 |
| **Comms** | Voice calls / IVR | Chat/SMS/email only |
| **Comms** | Community feed / announcements | Phase 2 |
| **Leasing** | Self-showing / tour scheduling with pre-lease access | CAP-12 covers post-lease keys; tours Phase 2 (M12) |
| **Accounting** | 1099 / vendor & owner tax reporting | Phase 2 (M11) |

## Success signal

*Locked 2026-07-05 — guided session.*

**Primary signal (APM working):** A Texas PM company with 50–100 residential units completes portfolio onboarding (CAP-1). Within **14 calendar days** of go-live:

- **≥80%** of routine interactions (tenant inquiries, maintenance triage under spend threshold, transaction categorization) complete **without PM staff intervention**
- **Zero** manual journal entries in the closed month; accountant performs **one** monthly sign-off and owner distributions calculate correctly (CAP-4)
- PM admin self-reports **≥15 hours/week** reduction in coordination work (survey at day 14 and day 30)
- A qualified prospect completes inquiry → signed lease → working digital key in **≤48 hours** with zero staff touches on Professional plan (CAP-2 + CAP-12)
- Penetration test confirms **zero** cross-tenant data leakage between two PM companies (CAP-11)

**Secondary signals (validate before scale):**

- Emergency maintenance on locked list auto-dispatches without human approval; resident confirms resolution
- Auditor traces any autonomous leasing or maintenance decision to full decision trace in **≤60 seconds** (CAP-10)
- Screening adverse-action notice includes specific denial reason when applicant denied (when screening resumes)

## Assumptions

- Competitive set per HANDOFF (17 names) includes two service operators (On Q PM, Belong Home) as benchmarks, not direct software competitors.
- Entrata's "Autonomous Property Management™" targets enterprise multifamily; RentalPro targets independent PM companies (approx. 10–500 units).
- MVP accounting requires human monthly sign-off before distributions (per locked HANDOFF decision).
- Seam, Stripe, and Plaid are the intended integration partners (architecture phase finalizes).
- **MVP jurisdiction:** Texas only (federal FHA/FCRA + Texas Property Code); expand state-by-state after validation. AI agent detail in `docs/AI-MVP-DECISIONS.md`.
- **MVP stack direction:** Vercel (Next.js) + Supabase (PostgreSQL + RLS + Storage) + Clerk (auth); Postgres portable to AWS/GCP at scale.
- **MVP communication:** Chat, SMS, email — no voice calls.
- **CAP-11 locked:** Clerk orgs, single DB + RLS, subdomain branding (`{slug}.rentalpro.ai`); custom domain Phase 2.

## Open Questions

- Screening criteria defaults and industry-breaking screening thesis — **parked** (see `docs/AI-MVP-DECISIONS.md`).
- Required audit retention period and export format for tax/legal purposes — recommend 7 years; pending CAP-10 lock.

### Market parity gaps M1–M10 (founder pick before CAP lock)

| ID | Feature | Priority | TBD action |
|----|---------|----------|------------|
| M1 | Listings & marketing syndication (Zillow, etc.) | ⚪ | **Full MVP** — see `docs/SYNDICATION-MVP-RUNBOOK.md` |
| M2 | Delinquency & rent collection (late fees, reminders, plans) | 🔴 | **MVP locked** — CAP-4 + CAP-7 · `docs/DELINQUENCY-RULES-ENGINE.md` |
| M3 | Lease renewals (90-day workflow, renewal e-sign) | 🟡 | **MVP locked** — CAP-2 + Owner approval · `docs/LEASE-RENEWAL-MVP-REQ.md` |
| M4 | Move-in / move-out inspections (photo checklist) | ⚪ | **MVP locked** — full in + out · `docs/MOVE-IN-OUT-INSPECTIONS-MVP-REQ.md` |
| M5 | Security deposit lifecycle (trust sub-ledger, TX 30-day return) | 🔴 | CAP-4 sub-feature or third party · **TBD** |
| M6 | Eviction & legal notice tracking | ⚪ | Phase 2 CAP or Non-goal · **TBD** |
| M7 | Unified comms hub (resident + owner + vendor + leads inbox) | 🟡 | CAP-7 expand or new CAP · **TBD** |
| M8 | Owner portal UX (beyond statements — docs, approvals) | 🟡 | Expand CAP-8 · **TBD** |
| M9 | Open API & webhooks (all tiers) | 🟡 | Constraint or CAP-14 or Phase 2 · **TBD** |
| M10 | Document vault (leases, COIs, notices per unit) | 🟡 | CAP-2 partial; general vault · **TBD** |

- New CAPs (13–15) vs expand existing CAPs for market gaps? — **TBD**
- Are security deposits handled natively (CAP-4 sub-ledger) or third party? — **TBD** (M5)
- ~~Phase 2 non-goals for M11/M12~~ → **Resolved:** M11 (1099), M12 (tours) locked as Non-goals above.
- ~~Does CAP-2 include AI-generated lease documents?~~ → **Resolved:** AI fills PM template + platform Texas starter; PM reviews before e-sign.
- ~~Does CAP-3 include full vendor management end-to-end?~~ → **Resolved:** Full maintenance lifecycle for MVP.
- ~~MVP compliance scope?~~ → **Resolved:** Texas only for MVP.
