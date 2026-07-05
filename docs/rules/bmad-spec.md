# BMad spec workflow

RentalPro is in **bmad-spec guided mode**. Follow this workflow before writing application code.

## Where we are

```
1. Ideation ✅
2. bmad-product-brief (skipped)
3. bmad-spec (GUIDED MODE) ← CURRENT
   - Why: ✅ Locked
   - Capabilities: 🔄 Draft (12 CAP micro-specs; CAP-11 locked)
   - Constraints: ✅ Locked (2026-07-05)
   - Non-goals: ✅ Locked (2026-07-05)
   - Success signal: ✅ Locked (2026-07-05)
4. bmad-prd (next)
5. bmad-architecture (then)
6. bmad-create-epics-and-stories (then)
7. bmad-dev-story (then)
8. Ship
```

## Five-field kernel (SPEC.md)

| Field | Status | Rule |
|---|---|---|
| **Why** | Locked | Force, target, lever, escalation model — do not re-litigate |
| **Capabilities** | Draft | CAP-N with intent + success criterion each; CAP-11 locked |
| **Constraints** | Locked | Must bend design; if it doesn't rule anything out, it's decoration |
| **Non-goals** | Locked | Explicit out-of-scope stops scope creep |
| **Success signal** | Locked | Concrete, testable signal APM is working |

## Capability format

Each capability needs **both**:

- **intent** — what the system can do (WHAT, not HOW)
- **success** — how to test it (observable outcome)

Missing either = incomplete. See `docs/capabilities/_TEMPLATE.md`.

## Locked business decisions (do not override)

| Decision | Rationale |
|---|---|
| B2B SaaS primary; Direct-to-Landlord as internal lab | SaaS is the lever; lab proves feasibility |
| Tiered autonomy: Basic (human-approve) + Professional (AI within rails) | Market spectrum + compliance safety |
| Accounting: AI categorizes, human signs off (MVP) | Balances autonomy with tax accuracy |
| Multi-tenancy from Day 1 | Required for SaaS reusability |
| No human review for low-value transactions | Moves OER from 10% → 2% |

## Artifact locations

| Artifact | Path |
|---|---|
| Spec kernel | `_bmad-output/specs/spec-rentalpro/SPEC.md` |
| Decision log | `_bmad-output/specs/spec-rentalpro/.memlog.md` |
| Competitive matrix | `_bmad-output/specs/spec-rentalpro/competitive-matrix.md` |
| Capability deep-dives | `docs/capabilities/CAP-N-*.md` |
| Human wiki index | `docs/README.md` |
| BMad reference | `docs/BMAD-REFERENCE.md` |

## Rules for agents

- Read `docs/HANDOFF-TO-CURSOR.md` and `.memlog.md` before proposing spec changes
- Append decisions to `.memlog.md` first, then update `SPEC.md`
- Do not invent capabilities not grounded in competitive research or locked Why
- Do not start coding MVP until spec is complete and PRD phase begins
- Companions (competitive matrix, capability docs) hold detail that doesn't fit the kernel

## Open questions (resolve in Capabilities phase)

1. Leasing scope: AI generates lease documents, or pre-qualify + key only?
2. Maintenance scope: full vendor management, or triage only?
3. Escrow/deposits: handled by APM or third-party?
4. Compliance: federal only, or state-specific jurisdictions?
5. Audit trail: logging level for tax/legal purposes?
