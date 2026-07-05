# MVP Sprint Plan — 24/7 Build Order

**Goal:** Ship MVP with **most capabilities** — micro-spec each CAP before coding it.

## MVP includes (all 12 CAPs)

Per your direction: MVP is broad. Defer polish, not core loops.

| Phase | CAPs | Why this order |
|-------|------|----------------|
| **0 — Foundation** | CAP-11, CAP-10, CAP-5 | Multi-tenant + audit + governance underpin everything |
| **1 — Data in** | CAP-1 | Can't run agents without portfolio data |
| **2 — Money loop** | CAP-4, CAP-7 (rent only) | Ledger + resident pay = trust accounting core |
| **3 — Maintenance loop** | CAP-3, CAP-9 | Highest autonomy ROI; Latchel proves market |
| **4 — Leasing loop** | CAP-2, CAP-12 | Identity → lease → key = differentiator |
| **5 — Control plane** | CAP-6, CAP-8 | Module toggles + owner statements |

## Micro-level before code (per CAP)

For each capability, complete its `docs/capabilities/CAP-N-*.md` section:

1. **User stories** (PM admin, resident, owner, agent)
2. **Happy path** (step-by-step autonomous flow)
3. **Escalation path** (when human must approve)
4. **Integrations** (Stripe, Seam, Plaid, etc.)
5. **Data model** (entities + key fields)
6. **API surface** (rough endpoints)
7. **Acceptance tests** (from SPEC success criterion)
8. **Open questions** (blockers only)

**Gate:** Do not start coding a CAP until sections 1–3 and 7 are filled.

## Parallel work (you + partner)

| Role | Focus |
|------|-------|
| **You** | CAP deep-dives + agent logic + integrations |
| **Partner** | Review competitive matrix, fill open questions, test acceptance criteria |
| **Cursor** | Derive architecture after CAP-1..4 micro-specs locked |

## After micro-specs → BMad path

```
✅ Why (locked)
🔄 Capabilities → micro-spec each CAP in docs/capabilities/
⏳ Lock Constraints / Non-goals / Success signal (1 session)
→ bmad-prd (expand stories)
→ bmad-architecture (schema, APIs, agents)
→ bmad-dev-story (sprint)
```

## Tonight's 24/7 sequence

1. **Hour 1–2:** CAP-11 + CAP-10 micro-spec (foundation)
2. **Hour 3–4:** CAP-1 micro-spec (onboarding)
3. **Hour 5–8:** CAP-4 + CAP-7 micro-spec (accounting + resident rent)
4. **Next block:** CAP-3 + CAP-9 (maintenance)
5. **Then:** CAP-2 + CAP-12 (leasing + Seam)

Say **"micro CAP-11"** (or any CAP number) to start the guided deep-dive in chat; I'll fill the doc with you.
