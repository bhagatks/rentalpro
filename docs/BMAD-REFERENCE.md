# BMad Method: Quick Reference Guide

## Core Concepts

| Term | Definition | When You Use It |
|------|-----------|-----------------|
| **BMad Method** | A structured workflow framework for building products from idea → spec → PRD → architecture → stories → code | Overall methodology guide |
| **Spec (SPEC.md)** | Compact contract: Why, Capabilities, Constraints, Non-goals, Success signal. The kernel. | Lock down WHAT before diving into HOW |
| **PRD** | Product Requirements Document. Expanded version of spec with detailed user stories, acceptance criteria, edge cases. | After spec is locked; more narrative than spec |
| **Architecture** | System design: tech decisions, data models, API contracts, integrations, schema. Derived from PRD. | After PRD; before coding |
| **Epic** | Large feature or initiative that delivers business value. Contains multiple stories. | Break down architecture into implementable chunks |
| **Story** | Single, implementable unit of work (one dev task). Has acceptance criteria. Belongs to an epic. | Actual dev work |
| **Memlog (.memlog.md)** | Append-only audit trail. Records every decision, assumption, question, change. Source of truth for the artifact. | Internal; read-only to verify decisions |
| **Companion** | Detail file alongside SPEC.md or PRD (glossary, matrices, diagrams). Holds content that doesn't fit the kernel. | Store reference material & expansions |
| **Capability (CAP-N)** | Something the system can *do*. Format: intent (WHAT) + success criterion (how to test it). | Define what the product delivers |
| **Constraint** | Non-negotiable that bends design decisions. Must rule something out; else it's decoration. | Lock down hard requirements |
| **Non-goal** | Explicit out-of-scope item. Stops downstream from filling the vacuum. | Prevent scope creep |

---

## BMad Skills (Workflows)

| Skill Name | Short Name | Purpose | Input | Output | Typical Order |
|------------|-----------|---------|-------|--------|---|
| **bmad-spec** | Spec | Distill intent into compact WHAT contract. Five-field kernel. | Brain dump, brief, PRD, RFC, email, mockups | SPEC.md + companions | 1️⃣ Early (after ideation) |
| **bmad-prd** | PRD | Expand spec into detailed, narrative requirements. User stories, acceptance criteria. | SPEC.md or new intent | PRD.md + companions | 2️⃣ After spec locked |
| **bmad-architecture** | Architecture | Design HOW: system, data model, integrations, tech stack. | PRD.md | Architecture spec, diagrams | 3️⃣ After PRD locked |
| **bmad-create-epics-and-stories** | Epics/Stories | Break architecture into implementable chunks. | Architecture spec | Epic list + story breakdowns | 4️⃣ Before dev |
| **bmad-dev-story** | Dev | Execute a single story: code, test, review. | Story spec | Working code + tests | 5️⃣ Actual development |
| **bmad-code-review** | Code Review | Review code for bugs, efficiency, best practices. | PR or branch | Review findings + fixes | During/after Dev |
| **bmad-product-brief** | Brief | Coach-facilitated discovery. Lock down concept before spec. | Idea or concept | brief.md + addendum | 0️⃣ Before spec (optional) |
| **bmad-prfaq** | PRFAQ | Stress-test concept via "Working Backwards" PR/FAQ. Kill weak assumptions. | Concept or idea | PRFAQ.md | 0️⃣ Before spec (optional) |
| **bmad-quick-dev** | Quick Dev | Fast intent → code workflow. Unified, no intermediate docs. | Tactical feature idea | Code + spec + tests | Anytime (tactical features) |
| **bmad-checkpoint-preview** | Checkpoint | Human-in-the-loop review of a change. Walkthrough commits, PRs, branches. | Commit, PR, branch | Review notes | Anytime (review phase) |
| **bmad-qa-generate-e2e-tests** | QA Tests | Auto-generate API & E2E tests from spec/PRD. | Story spec or PRD | Test suite | During Dev |
| **bmad-agent-tech-writer** | Tech Writer | Write docs, create diagrams (Mermaid), validate documents. Actions: `write`, `mermaid`, `validate`. | Topic or path | Document + diagrams | Anytime (docs) |
| **bmad-party-mode** | Party Mode | Multi-agent discussion. Get perspectives from analyst, designer, architect, dev, writer. | Current work + question | Diverse viewpoints | Anytime (get second opinions) |
| **bmad-advanced-elicitation** | Advanced Elicitation | Deeper critique via Socratic method, first-principles, pre-mortem, red team. | Current spec/PRD/work | Refined output | Anytime (pressure-test) |
| **bmad-correct-course** | Correct Course | Navigate significant changes mid-sprint. Propose PRD updates, architecture changes, re-plan. | Current sprint state + change signal | Change proposal | Anytime (mid-course corrections) |
| **bmad-help** | Help | Understand where you are in BMad workflow, what to do next. | Current state | Workflow guidance | Anytime (orientation) |

---

## Artifact Locations

| Artifact | Location | Format |
|----------|----------|--------|
| **Specs** | `_bmad-output/specs/spec-{slug}/` | SPEC.md + companions + .memlog.md |
| **PRDs** | `_bmad-output/planning-artifacts/prd-{slug}/` | PRD.md + companions + .memlog.md |
| **Architecture** | `_bmad-output/planning-artifacts/architecture-{slug}/` | Architecture.md + diagrams |
| **Epics & Stories** | `_bmad-output/planning-artifacts/epics-{slug}/` | Spreadsheet or markdown matrix |
| **Implementation** | `_bmad-output/implementation-artifacts/` | Code, tests, PRs, reviews |
| **Project Knowledge** | `docs/` (your project repo) | project-context.md, guides, etc. |

---

## Typical Workflow: Idea → Ship

```
1. Ideation
   ↓
2. bmad-product-brief OR bmad-prfaq (optional; stress-test concept)
   ↓
3. bmad-spec (lock WHAT in kernel form) ← YOU ARE HERE
   ↓
4. bmad-prd (expand into detailed requirements)
   ↓
5. bmad-architecture (design HOW)
   ↓
6. bmad-create-epics-and-stories (break into stories)
   ↓
7. bmad-dev-story (dev each story)
   ↓
8. bmad-code-review (review)
   ↓
9. Ship
```

---

## Key Principles

- **Spec before PRD before Architecture before Code.** Don't jump steps; each feeds the next.
- **Five-field kernel (Spec):** Why, Capabilities, Constraints, Non-goals, Success signal. Compact. Lean prose.
- **Memlog is canonical.** SPEC.md, PRD.md, etc. are *derived* from the memlog. Hand-edits are overwritten.
- **Capabilities need both intent (WHAT) and success (how to test).** Missing either = incomplete.
- **Companions hold detail that doesn't fit the kernel.** Glossaries, matrices, diagrams, reference material.
- **Constraints must bend design.** If it doesn't rule anything out, it's not a constraint.
- **Non-goals prevent scope creep.** Explicit out-of-scope items.

---

## Quick Commands

| Action | Skill | Notes |
|--------|-------|-------|
| "Where am I?" | `bmad-help` | Orient yourself in the workflow |
| "Start over" | `bmad-correct-course` | Pivot or reset mid-sprint |
| "Get fresh eyes" | `bmad-party-mode` | Multi-agent perspectives |
| "Pressure-test this" | `bmad-advanced-elicitation` | Deeper critique |
| "Write documentation" | `bmad-agent-tech-writer` | Docs, diagrams, validation |
| "Make a diagram" | `bmad-agent-tech-writer` (action: `mermaid`) | Mermaid diagram generation |

---

**Save this file locally. Reference it whenever you need to orient yourself in the BMad ecosystem.**
