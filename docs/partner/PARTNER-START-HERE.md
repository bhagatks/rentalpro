# RentalPro.ai — Partner Start Guide

**Audience:** Non-technical partner  
**Last updated:** 2026-07-05 (Cursor + Claude Code)  
**You do not need to know how to code.**  
**You do not need GitHub.**  
**You do not need git, Terminal, or pull requests.**

This guide walks you from zero to contributing the same way the founder has been working in BMad — through conversation, decisions, and written requirements.

---

## What you are helping with (30-second version)

RentalPro.ai is an AI property management platform. Before anyone builds software, we must **lock what the product does** — in plain language, with testable outcomes.

Your job is to:

1. **Read** what exists
2. **Ask** good product questions
3. **Decide** what is in MVP vs not
4. **Write down** decisions so engineers and AI agents do not guess later

That is BMad. You are the requirements half. The founder + AI tools (Cursor or Claude Code) handle the file updates.

---

## Part 0 — Before anything else (no GitHub needed)

You can start **today** without GitHub, without cloning, and without knowing what a repo is.

### What the founder sends you (pick one)

| What you get | How you use it | When to move on |
|--------------|----------------|-----------------|
| **Notion link** | Read + comment in Notion | When ready for AI tool on laptop |
| **Google Drive folder** | Read docs, reply in a shared Google Doc | Same |
| **Zip file of the project** | Unzip to Desktop → open in Cursor or Claude Code | Best path to full BMad workflow |
| **30-min screenshare** | Founder walks you through reading list | Anytime |

**Ask the founder for:** a Notion link or zip file. That is enough for week one.

GitHub is optional and can come later. The founder will merge your decisions into the real project files.

---

## Part 1 — Get set up (do this first)

### Step 1: Three phases (start where you are)

```
Phase A — Read only     →  Notion or Google Drive (no install)
Phase B — AI on laptop  →  Zip folder + Cursor or Claude Code
Phase C — Later         →  GitHub access (optional, founder adds when ready)
```

**Start at Phase A.** Do not wait for GitHub.

---

### Step 2: Phase A — Read only (Day 1, zero install)

The founder shares these files (Notion, Drive, or PDF):

| Read first | Why |
|------------|-----|
| This guide (`PARTNER-START-HERE.md`) | How to work |
| `docs/HANDOFF-TO-CURSOR.md` | Vision + locked decisions |
| `docs/AI-MVP-DECISIONS.md` | What AI will / will not do in MVP |
| `docs/capabilities/CAP-2-autonomous-leasing.md` | Example feature spec |
| `docs/capabilities/CAP-4-autonomous-accounting.md` | Second example |

**Your output:** Write answers in a Google Doc or Notion comments. Send the founder a short summary. That counts as contributing.

---

### Step 3: Phase B — Get the project folder (no GitHub)

Ask the founder to **zip the project** and send it (email, Drive, Dropbox, AirDrop).

**On your computer:**

1. Download the zip file
2. Double-click to unzip
3. You should see a folder (often named `rentalpro`) containing `docs/` and `_bmad-output/`
4. Remember where it lives (e.g. Desktop → `rentalpro`)

**You still do not need GitHub.** When you make decisions with Cursor or Claude Code, tell the founder: “I updated the docs locally — please sync.” The founder copies changes into git.

---

### Step 4: Pick your AI tool

The founder uses **Cursor** and/or **Claude Code**. Both read the same project files and follow the same rules (`AGENTS.md`, `docs/rules/`). Pick one — you do not need both.

| Tool | Best for you if… | Looks like… |
|------|------------------|-------------|
| **Cursor** (recommended for non-technical) | You want a normal app with folders on the left and chat on the right | VS Code + AI chat panel |
| **Claude Code Desktop** | You already pay for Claude (Pro/Max/Team) and want a standalone app | Claude app → **Code** tab |
| **Claude Code Terminal** | You are comfortable in Terminal / command line | Text window where you type to Claude |
| **Claude in Cursor** (extension) | You use Cursor but want Claude as the model | Cursor + “Claude Code” extension |

**If unsure:** use **Cursor**. It is the most visual and closest to “chat with your project folder.”

---

## Part 1A — Cursor setup (step by step)

Cursor is a code editor with AI built in. You will use it like a **smart assistant that also updates documents**.

### A1. Install Cursor

1. Go to [https://cursor.com](https://cursor.com)
2. Download for Mac or Windows
3. Create an account and sign in
4. Choose a plan (Free works to start; Pro gives more AI usage)

### A2. Open the project folder

1. Launch Cursor
2. **File → Open Folder…**
3. Select the unzipped `rentalpro` folder (e.g. on your Desktop)
4. In the left sidebar you should see folders like `docs`, `_bmad-output`

**Tour of the screen (you only need 2 areas):**

| Area | Where | What you use it for |
|------|-------|---------------------|
| File tree | Left sidebar | Open markdown docs to read |
| Agent / Chat | Right panel | Talk to AI — **this is your main workspace** |

You can ignore the center “code” area unless you want to read a file there.

### A3. Open your first chat

**Option 1 — Agent mode (best for updating docs):**

1. Press `Cmd+I` (Mac) or `Ctrl+I` (Windows) — or click the Agent icon
2. Confirm mode is **Agent** (not Ask / Plan)
3. Paste your first message (see below)

**Option 2 — Chat mode (best for questions only, no file edits):**

1. Press `Cmd+L` (Mac) or `Ctrl+L` (Windows)
2. Use **Ask** mode if you only want answers, not file changes

**First message to paste:**

```
I am a non-technical partner on RentalPro.ai.
Read docs/partner/PARTNER-START-HERE.md and docs/HANDOFF-TO-CURSOR.md.
In plain English: where are we in the project, and what should I do first?
Do not suggest code — only product requirements.
```

### A4. Cursor shortcuts to remember

| Action | Mac | Windows |
|--------|-----|---------|
| Open Agent (edits files) | `Cmd+I` | `Ctrl+I` |
| Open Chat | `Cmd+L` | `Ctrl+L` |
| Reference a file in chat | Type `@` then filename | Same |
| Accept a change | Click **Accept** on the diff | Same |
| Reject a change | Click **Reject** | Same |

**Tip:** When the agent proposes edits, **read the summary first**, then Accept. You are approving product decisions, not code.

### A5. Cursor modes (simple guide)

| Mode | Use when… | Edits files? |
|------|-----------|--------------|
| **Agent** | “Log this decision and update the CAP doc” | Yes |
| **Ask** | “Explain CAP-4 to me in plain English” | No |
| **Plan** | Big multi-file change — review plan first | After you approve |

For BMad spec work, use **Agent** most of the time.

---

## Part 1B — Claude Code setup (step by step)

Claude Code is Anthropic’s AI agent. It reads your project folder, edits markdown files, and asks before changing anything. Same job as Cursor Agent — different app.

Official docs: [code.claude.com/docs](https://code.claude.com/docs/en/overview)

### B1. Pick how you will run Claude Code

| Surface | Non-technical friendly? | Needs GitHub? |
|---------|---------------------------|---------------|
| **Desktop app** | ⭐ Best | No — open local folder |
| **Terminal CLI** | Medium | No — open local folder |
| **Extension inside Cursor** | Good | No |
| **Web** ([claude.ai/code](https://claude.ai/code)) | Good | Yes — connects to GitHub repos |

**Without GitHub:** use **Desktop app** or **Terminal** with the unzipped folder.

### B2. Install — Desktop app (recommended for you)

1. Download Claude desktop: [claude.ai/download](https://claude.ai/download)
2. Install and sign in (Claude Pro, Max, Team, or Enterprise)
3. Open the app → click the **Code** tab
4. **Open folder** → select the unzipped `rentalpro` folder
5. Start a session — Claude Code can now see your docs

Paid Claude subscription required for Code features.

### B3. Install — Terminal (optional)

**Mac / Linux / WSL:**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows PowerShell:**

```powershell
irm https://claude.ai/install.ps1 | iex
```

**Then start working:**

```bash
cd ~/Desktop/rentalpro
claude
```

(Change the path to wherever you unzipped the folder.)

First run opens a browser to log in. After that, type plain English at the prompt.

**Useful commands:**

| Command | What it does |
|---------|--------------|
| `claude` | Start a session in current folder |
| `claude -c` | Continue your last conversation |
| `/help` | List commands (type inside Claude Code) |
| `/clear` | Start fresh conversation |
| `/exit` | Quit |

**Never used Terminal before?** Use the **Desktop app** instead, or ask the founder for a 10-minute screenshare.

### B4. Install — Claude Code inside Cursor (optional)

If you already use Cursor but want Claude Code specifically:

1. Open Cursor
2. Extensions (`Cmd+Shift+X` / `Ctrl+Shift+X`)
3. Search **Claude Code** → Install
4. Command Palette (`Cmd+Shift+P` / `Ctrl+Shift+P`) → **Claude Code: Open in New Tab**

Same project folder, same prompts as the rest of this guide.

### B5. Open your first session in Claude Code

Paste this as your first message (Desktop, Terminal, or extension):

```
I am a non-technical partner on RentalPro.ai.
Read docs/partner/PARTNER-START-HERE.md and docs/HANDOFF-TO-CURSOR.md.
In plain English: where are we in the project, and what should I do first?
Do not suggest code — only product requirements.
```

### B6. Approving changes in Claude Code

Claude Code **always asks before editing files**. That is good for you.

1. Claude shows what it wants to change
2. Read the summary in plain English
3. Approve or reject
4. For spec work, approve updates to `docs/`, `.memlog.md`, and CAP files

You do not need to understand the diff line-by-line — ask: “Summarize what you changed in plain English.”

### B7. After you edit files locally (no GitHub)

Tell the founder one of these:

- “I updated CAP-2 and memlog on my machine — can you pull/sync?”
- Send the changed files back (zip or Drive)
- Screenshare and walk through what you decided

The founder handles git. You focus on decisions.

---

## Part 1C — Cursor vs Claude Code (same task, both tools)

The **prompts are identical**. Only the button / window differs.

| Task | Cursor | Claude Code |
|------|--------|-------------|
| Open project | File → Open Folder | Desktop: Code tab → Open folder · Terminal: `cd rentalpro && claude` |
| Ask a question (read-only) | `Cmd+L` → **Ask** mode | Type question at prompt |
| Update a doc | `Cmd+I` → **Agent** mode | Type request; approve edit |
| Point at one file | `@filename` in chat | “Read docs/capabilities/CAP-2…” or `@` if supported |
| Start over | New chat / `Cmd+N` | `/clear` |
| Continue yesterday | Chat history in sidebar | `claude -c` or Desktop session history |

**Both tools read the same rules:** `AGENTS.md` and files in `docs/rules/`. You do not configure anything extra.

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
1. You and founder (or your AI tool) discuss a decision
2. Decision gets appended to .memlog.md  ← source of truth
3. Relevant CAP doc gets updated
4. SPEC.md gets re-derived from memlog
```

**File:** `_bmad-output/specs/spec-rentalpro/.memlog.md`  
This is the **decision diary**. Every lock gets a dated line.

**Say this in Cursor (Agent) or Claude Code:**

```
We decided: [your decision in plain English].
Append it to .memlog.md, update the relevant CAP doc, and re-derive SPEC.md.
```

You never need to edit those files by hand — that is what Cursor Agent or Claude Code is for.

**No GitHub?** Still log decisions the same way in your local folder. Tell the founder to sync when done.

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
3. Add comments (Notion/Drive) or tell your AI tool your answers
4. One decision logged = a good day

### Weekly sync with founder (30–45 min)

Agenda template:

1. Which CAPs did we review?
2. What got locked this week?
3. What is still TBD?
4. Any new market-gap items from `MARKET-GAP-CHECKLIST.md`?
5. Are we ready to start Constraints / Non-goals / Success signal?

---

## Part 8 — Copy-paste prompts (Cursor + Claude Code)

Use these in **Cursor Agent** (`Cmd+I` / `Ctrl+I`) or **Claude Code** (Desktop, Terminal, or extension). Replace the `[brackets]`.

### Orientation

```
I am a non-technical partner on RentalPro.ai.
Read docs/partner/PARTNER-START-HERE.md and .memlog.md.
What is the single highest-value thing I should do in the next 30 minutes?
Do not suggest code — only product requirements.
```

### Review one capability

```
Review docs/capabilities/CAP-[N]-*.md for a non-technical partner.
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

### Task B — Answer (Notion, Google Doc, or AI chat)

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

When you pick an answer, log it via Cursor or Claude Code (memlog → CAP → SPEC).

---

## Part 12 — What NOT to do

| Do not… | Because… |
|---------|----------|
| Wait for GitHub to start reading | Notion/Drive/zip is enough for Day 1 |
| Reopen **Why** / business model | Already locked; wastes time |
| Write requirements without **Success** criteria | Engineers cannot test vibes |
| Edit `SPEC.md` directly without memlog | Gets overwritten; decision is lost |
| Jump to “let’s build X” | Spec phase is not finished |
| Worry about database or APIs | Architecture comes after PRD |
| Treat Notion/Drive as source of truth | Git + memlog win; mirrors are for reading |

---

## Part 13 — Glossary

| Term | Meaning |
|------|---------|
| **Cursor** | AI code editor — visual app with chat/Agent panel |
| **Claude Code** | Anthropic’s AI agent — Desktop app, Terminal, or extension |
| **Agent mode** | AI can edit project files (use this for BMad updates) |
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
| Stuck on terminology | Ask your AI: “Explain [term] like I am new to proptech” |
| Not sure if in scope | Ask: “Is [X] a Constraint, Non-goal, or Capability?” |
| Founder unavailable | Leave questions in CAP **Open questions** via Agent / Claude Code |
| No GitHub yet | Read in Notion/Drive; use zip + Cursor/Claude Code for edits |
| Cursor vs Claude Code | Either works — same prompts, same files |
| Want the full BMad map | Read `docs/BMAD-REFERENCE.md` (optional, denser) |

---

## Quick reference card (pin this)

```
PHASE A → Read in Notion/Drive (no install)
PHASE B → Unzip folder → Cursor Agent or Claude Code Desktop
READ    → docs/HANDOFF-TO-CURSOR.md + one CAP doc
THINK   → Intent clear? Success testable? Open questions?
DECIDE  → Talk to founder or AI
LOG     → “Append to .memlog.md, update CAP, re-derive SPEC.md”
SYNC    → Tell founder to merge your local changes (no GitHub required)
```

**Tool pick:** Cursor = visual · Claude Code Desktop = if you already use Claude · Same prompts either way.

**You are not behind. You are exactly who this phase needs.**

---

*Questions about this guide? Tell the founder or paste into Cursor / Claude Code: “Update docs/partner/PARTNER-START-HERE.md based on my question: …”*
