# RentalPro.ai — Product Director Start Guide

**Audience:** Non-technical product partner  
**Last updated:** 2026-07-05  
**You do not need to know how to code.**  
**You do not need GitHub on day one.**

This guide walks you from zero to contributing the same way the founder has been working in BMad — through conversation, decisions, and written requirements.

---

## What you are helping with (30-second version)

RentalPro.ai is an AI property management platform. Before anyone builds software, we must **lock what the product does** — in plain language, with testable outcomes.

Your job is to:

1. **Read** what exists
2. **Ask** good product questions
3. **Decide** what is in MVP vs not
4. **Write down** decisions so engineers and AI agents do not guess later

That is BMad. You are the requirements half. The founder + Cursor handle the file updates.

---

## Part 1 — Get set up (do this first)

### Step 1: Get the project files

Pick **one** path with the founder. You only need one.

| Option | Best if… | What the founder does |
|--------|----------|------------------------|
| **A. GitHub access** | You are okay learning “click to read a file” | Invites you to the private repo; you open files in the browser |
| **B. Notion mirror** | You want a clean wiki with comments | Founder imports `docs/` into Notion and shares a link |
| **C. Google Drive / PDF export** | You want zero new tools tonight | Founder exports key docs; you read and reply in a shared doc |
| **D. Cursor on your laptop** | You want the full experience like the founder | Founder helps you install Cursor and open the project folder |

**Recommended for non-technical partners:** Start with **B (Notion)** or **C (Drive)** to read. Move to **D (Cursor)** once you are comfortable.

You do **not** need to understand git, branches, or pull requests to contribute product decisions.

---

### Step 2: Install Cursor (when you are ready)

Cursor is a code editor with an AI chat built in. The founder uses it like a **smart PM assistant that also updates documents**.

1. Go to [https://cursor.com](https://cursor.com)
2. Download Cursor for your computer (Mac or Windows)
3. Sign up / log in
4. Ask the founder to either:
   - **Clone the repo for you** on a call (easiest), or
   - **Send you the project folder** as a zip (works for reading; founder syncs your decisions back to git)

**Open the project in Cursor:**

1. File → Open Folder
2. Select the `rentalpro` (or similarly named) folder
3. You should see folders like `docs/` and `_bmad-output/`

---

### Step 3: Open your first chat in Cursor

1. Press `Cmd+L` (Mac) or `Ctrl+L` (Windows) — or click the chat panel
2. Make sure **Agent** mode is selected (not just Ask)
3. Paste this as your first message:

```
I am the product director partner on RentalPro.ai. I am non-technical.
Read docs/partner/PRODUCT-DIRECTOR-START-HERE.md and docs/HANDOFF-TO-CURSOR.md.
In plain English: where are we in the project, and what should I do first?
```

The AI will summarize the project and give you a concrete first task.

---

## Part 2 — Understand the project (read these in order)

You do not need to read everything at once. Follow this sequence.

### Reading list — Day 1 (about 45–60 minutes)

| Order | File | What it tells you |
|-------|------|-------------------|
| 1 | `docs/HANDOFF-TO-CURSOR.md` | Vision, business model, locked decisions |
| 2 | `docs/AI-MVP-DECISIONS.md` | What the AI agents will / will not do in MVP |
| 3 | `docs/README.md` | Index of all capability docs (CAP-1 through CAP-12) |
| 4 | One CAP doc, e.g. `docs/capabilities/CAP-2-autonomous-leasing.md` | Example of a feature spec |

### Reading list — Day 2–3

| File | What it tells you |
|------|-------------------|
| `docs/MARKET-GAP-CHECKLIST.md` | Gaps vs AppFolio / Buildium — what MVP might still need |
| `docs/MVP-SPRINT-PLAN.md` | Build order (for context; you are not scheduling sprints yet) |
| `_bmad-output/specs/spec-rentalpro/SPEC.md` | The one-page “contract” summary |

### Skip for now (founder / engineering)

- `AGENTS.md`, `docs/rules/`, anything about Prisma, API routes, or tests
- You can ignore code folders entirely during spec phase

---

## Part 3 — BMad explained without jargon

BMad is a **step-by-step order** for building a product. Think of it as a checklist you must complete before coding.

```
Ideas  →  SPEC  →  PRD  →  Architecture  →  Stories  →  Code
           ↑
      YOU ARE HERE
```

### The SPEC has 5 boxes

| Box | Plain English | Status today |
|-----|---------------|--------------|
| **Why** | Why does this product exist? | ✅ Done — do not reopen |
| **Capabilities** | What can the product do? (12 features, CAP-1 to CAP-12) | 🔄 Draft — **your main focus** |
| **Constraints** | Hard rules that limit design | ⏳ Next — **you lead this** |
| **Non-goals** | Explicit “we are NOT building this” | ⏳ Next — **you lead this** |
| **Success signal** | How we know APM is actually working | ⏳ Next — **you lead this** |

**Golden rule:** Spec before PRD before architecture before code.  
If someone says “let’s just build the portal,” the answer is: **not until the five boxes are filled.**

---

## Part 4 — What is a “Capability” (CAP)?

Each capability is **one thing the system can do**.

Every CAP must have **two sentences**:

| Field | Question it answers | Bad example | Good example |
|-------|---------------------|-------------|--------------|
| **Intent** | WHAT does it do? | “Good leasing experience” | “A qualified prospect completes inquiry through signed lease without PM staff on Professional plan” |
| **Success** | HOW would we test it? | “Users are happy” | “Prospect gets working digital key within 48 hours; every step logged” |

If either is vague, your job is to sharpen it.

### The 12 capabilities (quick map)

| CAP | Name | Your likely focus |
|-----|------|-------------------|
| CAP-1 | Portfolio onboarding | Import scope, data quality |
| CAP-2 | Autonomous leasing | Screening rules, lease flow (partially parked) |
| CAP-3 | Autonomous maintenance | Emergency vs non-emergency, spend limits |
| CAP-4 | Autonomous accounting | Monthly sign-off, categorization |
| CAP-5 | Governance rails | Approval thresholds ($500 rule, etc.) |
| CAP-6 | Module toggles | AI vs human per module |
| CAP-7 | Resident portal | Self-service scope |
| CAP-8 | Owner reporting | Statements, distributions |
| CAP-9 | Vendor management | Vendor database, payments |
| CAP-10 | Audit trail | Legal / fair housing traceability |
| CAP-11 | Multi-tenant SaaS | ✅ Locked — read only |
| CAP-12 | Smart access (Seam) | Digital keys at lease activation |

Each has a deep-dive page in `docs/capabilities/CAP-N-*.md`.

---

## Part 5 — How decisions get saved (important)

There are two layers of documents:

```
docs/                    ← Human-friendly wiki (you read and edit here)
_bmad-output/            ← Machine contract (agents keep in sync)
```

### The decision loop (same as the founder uses)

```
1. You and founder (or Cursor) discuss a decision
2. Decision gets appended to .memlog.md  ← source of truth
3. Relevant CAP doc gets updated
4. SPEC.md gets re-derived from memlog
```

**File:** `_bmad-output/specs/spec-rentalpro/.memlog.md`  
This is the **decision diary**. Every lock gets a dated line.

**You can say to Cursor:**

```
We decided: [your decision in plain English].
Append it to .memlog.md, update the relevant CAP doc, and re-derive SPEC.md.
```

You never need to edit those files by hand — that is what Cursor is for.

---

## Part 6 — Locked decisions (do not overturn)

These were already decided. Your job is to **build on** them, not relitigate.

| Topic | Decision |
|-------|----------|
| Business model | B2B SaaS to PM companies (primary) |
| Pricing tiers | Basic $29/mo (human approves big actions) · Professional $99/mo (AI within rails) |
| MVP geography | Texas only (federal + state rules) |
| Communication | Chat / text / email — **no voice calls in MVP** |
| Leasing | AI drafts lease from template; PM admin reviews before e-sign |
| Maintenance | Full lifecycle in MVP (report → pay) |
| Accounting | AI categorizes 100%; human signs off monthly before distributions |
| Multi-tenancy | Required from day one — each PM company isolated |
| CAP-11 | Locked (SaaS infra, auth, branding) |

Full detail: `docs/HANDOFF-TO-CURSOR.md` and `docs/AI-MVP-DECISIONS.md`.

---

## Part 7 — Your weekly rhythm

### Daily (15–30 min)

1. Open one CAP doc
2. Read **Intent**, **Success**, and **Open questions**
3. Add comments (Notion/Drive) or tell Cursor your answers
4. One decision logged = a good day

### Weekly sync with founder (30–45 min)

Agenda template:

1. Which CAPs did we review?
2. What got locked this week?
3. What is still TBD?
4. Any new market-gap items from `MARKET-GAP-CHECKLIST.md`?
5. Are we ready to start Constraints / Non-goals / Success signal?

---

## Part 8 — Copy-paste prompts for Cursor

Use these exactly. Replace the `[brackets]`.

### Orientation

```
I am the product director on RentalPro.ai. Non-technical.
Read docs/partner/PRODUCT-DIRECTOR-START-HERE.md and .memlog.md.
What is the single highest-value thing I should do in the next 30 minutes?
```

### Review one capability

```
Review docs/capabilities/CAP-[N]-*.md for a non-technical product director.
Is Intent clear? Is Success testable?
List open questions I should answer with the founder.
Do not suggest code — only product requirements.
```

### Make a decision

```
Product decision: [write your decision here].
Append to _bmad-output/specs/spec-rentalpro/.memlog.md.
Update the matching CAP doc.
Re-derive _bmad-output/specs/spec-rentalpro/SPEC.md.
Show me a short summary of what you changed.
```

### Constraints workshop

```
Help me draft Constraints for RentalPro MVP spec.
Read HANDOFF-TO-CURSOR.md, AI-MVP-DECISIONS.md, and .memlog.md.
Each constraint must rule something OUT (not vague platitudes).
Propose 8–12 constraints for my review.
```

### Non-goals workshop

```
Help me draft Non-goals for RentalPro MVP.
Compare docs/MARKET-GAP-CHECKLIST.md and all CAP open questions.
Propose explicit out-of-scope items so we stop scope creep.
```

### Success signal workshop

```
Help me draft the Success signal for SPEC.md.
It must be concrete and testable — how do we know APM is working for a PM company?
Propose 3–5 measurable signals.
```

### Lock a capability

```
CAP-[N] requirements look complete after founder review.
Mark the CAP doc status as locked, append to .memlog.md, update SPEC.md.
List any remaining TBD items that do not block lock.
```

### Compare to competitors (product view)

```
Read competitive-matrix.md and CAP-[N] doc.
In plain English: what do AppFolio/Buildium do here, and what are we doing differently for MVP?
```

---

## Part 9 — Your first assignment (Day 1 homework)

Complete this before the first working session with the founder.

### Task A — Read (45 min)

- [ ] `docs/HANDOFF-TO-CURSOR.md`
- [ ] `docs/AI-MVP-DECISIONS.md`
- [ ] `docs/capabilities/CAP-2-autonomous-leasing.md`
- [ ] `docs/capabilities/CAP-4-autonomous-accounting.md`

### Task B — Answer (write in Notion, Google Doc, or Cursor chat)

For **CAP-2** and **CAP-4**, answer:

1. Is the **Intent** clear to a PM company buyer?
2. Is the **Success** line something we could demo or test?
3. What are the top **3 open questions** you still have?
4. One thing that should be a **Non-goal** for MVP
5. One **Constraint** that must never be broken

### Task C — Send the founder

A short message:

> I finished Day 1 reading. CAP-2 questions: … CAP-4 questions: …  
> Ready to sync on [date/time].

---

## Part 10 — Your second assignment (Week 1)

| Day | Focus | Output |
|-----|-------|--------|
| Mon | CAP-2 + CAP-3 | List of open questions + proposed decisions |
| Tue | CAP-4 + CAP-5 | Governance thresholds validated ($500 rule, sign-off flow) |
| Wed | CAP-7 + CAP-8 | Resident + owner portal MVP scope |
| Thu | `MARKET-GAP-CHECKLIST.md` | Mark 🔴 items: MVP / Phase 2 / Never |
| Fri | Constraints session with founder | First draft of Constraints section in SPEC |

---

## Part 11 — Open questions waiting for you

These are explicitly unresolved. Your product judgment is needed.

| Topic | Question | Where to look |
|-------|----------|---------------|
| Screening | Income multiplier? Eviction lookback? | `docs/AI-MVP-DECISIONS.md`, CAP-2 |
| Market gaps | Deposits, delinquency, renewals — MVP or not? | `docs/MARKET-GAP-CHECKLIST.md` |
| Audit depth | How much detail for tax/legal? | CAP-10 |
| Owner experience | Minimum viable owner report for MVP? | CAP-8 |

When you pick an answer, log it via Cursor (memlog → CAP → SPEC).

---

## Part 12 — What NOT to do

| Do not… | Because… |
|---------|----------|
| Reopen **Why** / business model | Already locked; wastes time |
| Write requirements without **Success** criteria | Engineers cannot test vibes |
| Edit `SPEC.md` directly without memlog | Gets overwritten; decision is lost |
| Jump to “let’s build X” | Spec phase is not finished |
| Worry about database or APIs | Architecture comes after PRD |
| Treat Notion/Drive as source of truth | Git + memlog win; mirrors are for reading |

---

## Part 13 — Glossary (10 terms)

| Term | Meaning |
|------|---------|
| **APM** | Autonomous Property Management — AI runs ops, humans govern by exception |
| **BMad** | Our structured workflow: spec → PRD → architecture → code |
| **CAP** | Capability — one thing the product can do (CAP-1 … CAP-12) |
| **SPEC** | One-page product contract (five boxes) |
| **Memlog** | Append-only decision diary |
| **MVP** | Minimum product we ship first (Texas, chat/text/email, etc.) |
| **PM company** | Our B2B customer — property management firm |
| **Governance rails** | Rules for when AI must ask a human (spend limits, etc.) |
| **Intent** | WHAT the feature does |
| **Success** | HOW we test the feature worked |

---

## Part 14 — When spec is “done”

You will know spec phase is complete when:

- [ ] All 12 CAPs are **locked** or explicitly deferred with reason
- [ ] **Constraints** section filled in `SPEC.md`
- [ ] **Non-goals** section filled in `SPEC.md`
- [ ] **Success signal** section filled in `SPEC.md`
- [ ] Open questions in `MARKET-GAP-CHECKLIST.md` assigned (MVP / Phase 2 / Never)

Then the project moves to **PRD** (more detailed user stories). You will still lead requirements — the format just gets richer.

---

## Part 15 — Getting help

| Need | Do this |
|------|---------|
| Stuck on terminology | Ask Cursor: “Explain [term] like I am new to proptech” |
| Not sure if in scope | Ask: “Is [X] a Constraint, Non-goal, or Capability?” |
| Founder unavailable | Leave questions in CAP **Open questions** section via Cursor |
| Want the full BMad map | Read `docs/BMAD-REFERENCE.md` (optional, denser) |

---

## Quick reference card (pin this)

```
READ  →  docs/HANDOFF-TO-CURSOR.md + one CAP doc
THINK →  Intent clear? Success testable? Open questions?
DECIDE→  Talk to founder or Cursor
LOG   →  “Append to .memlog.md, update CAP, re-derive SPEC.md”
REPEAT→  Next CAP
```

**You are not behind. You are exactly who this phase needs.**

---

*Questions about this guide? Tell the founder or paste into Cursor: “Update docs/partner/PRODUCT-DIRECTOR-START-HERE.md based on my question: …”*
