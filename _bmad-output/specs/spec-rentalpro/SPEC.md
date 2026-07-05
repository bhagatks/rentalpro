---
id: SPEC-rentalpro
companions:
  - competitive-matrix.md
sources:
  - ../../docs/HANDOFF-TO-CURSOR.md
---

> **Canonical contract.** This SPEC and the files in `companions:` are the complete, preservation-validated contract for what to build, test, and validate.

# RentalPro.ai — Autonomous Property Management Platform

## Why

Property management companies hit a scalability ceiling: roughly 10% of rent goes to administrative overhead—office staff, VAs, and human property managers coordinating leasing, maintenance, and accounting. RentalPro.ai exists to collapse that Operating Expense Ratio toward 2% by replacing the human coordinator with an AI-native Autonomous Property Management (APM) platform sold as B2B SaaS to existing PM firms.

The force is a **pain to solve** (administrative tax) combined with an **opportunity to capture** (LLM maturity, Seam smart-lock APIs, and Stripe identity/payments now make closed-loop autonomous workflows reliable). The target is PM companies managing residential portfolios who want to eliminate routine staff work while retaining governance over significant financial decisions.

Tiered autonomy addresses market spectrum and compliance: Basic plan ($29/mo) makes AI the interpreter—handling all communication but requiring human approval for significant actions; Professional plan ($99/mo) grants high autonomy with configurable governance rails (e.g., auto-approve repairs up to $500). Direct-to-landlord operation serves as an internal lab, not initial GTM.

## Capabilities

*Draft — review in guided mode before locking.*

- **CAP-1**
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

- **CAP-11**
  - **intent:** The platform operates as multi-tenant B2B SaaS with per-PM-company data isolation, white-label branding, and independent configuration.
  - **success:** Two PM companies on the platform cannot access each other's data; each presents distinct branding to their owners and residents in a penetration test.

- **CAP-12**
  - **intent:** The system integrates smart-access hardware so that identity-verified applicants receive time-bound digital keys upon lease activation.
  - **success:** A signed lease triggers Seam API key issuance; the tenant unlocks the unit door within 15 minutes of lease activation without manual key handoff.

## Constraints

*Pending — guided session next.*

## Non-goals

*Pending — guided session next.*

## Success signal

*Pending — guided session next.*

## Assumptions

- Competitive set per HANDOFF (17 names) includes two service operators (On Q PM, Belong Home) as benchmarks, not direct software competitors.
- Entrata's "Autonomous Property Management™" targets enterprise multifamily; RentalPro targets independent PM companies (approx. 10–500 units).
- MVP accounting requires human monthly sign-off before distributions (per locked HANDOFF decision).
- Seam, Stripe, and Plaid are the intended integration partners (architecture phase finalizes).

## Open Questions

- Does CAP-2 include AI-generated lease documents, or only pre-qualify + verify + key issuance?
- Does CAP-3 include full vendor contract management, or triage-and-dispatch only for MVP?
- Are security deposits and escrow handled natively or routed to a third party?
- MVP compliance scope: federal FHA screening rules only, or specific state jurisdictions?
- Required audit retention period and export format for tax/legal purposes?
