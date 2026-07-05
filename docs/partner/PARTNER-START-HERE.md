# RentalPro.ai — Partner Start Guide

**Audience:** Non-technical partner  
**Last updated:** 2026-07-05 (GitHub + Cursor + Claude Code)  
**You do not need to know how to code.**  
**You do need a GitHub account** — create one with your **personal Gmail** (Step 1 below).

This guide walks you from zero to contributing the same way the founder has been working in BMad — through conversation, decisions, and written requirements.

---

## What you are helping with (30-second version)

RentalPro.ai is an AI property management platform. Before anyone builds software, we must **lock what the product does** — in plain language, with testable outcomes.

Your job is to:

1. **Read** what exists
2. **Ask** good product questions
3. **Decide** what is in MVP vs not
4. **Write down** decisions so engineers and AI agents do not guess later

That is BMad. You own **requirements**. The founder owns **merge, infrastructure, and final locks**. You both use Cursor or Claude Code to update docs — but different files and different jobs (see Part 2).

---

## Part 0 — Create your GitHub account (do this first)

GitHub is where the project files live. You need your **own account** so the founder can invite you to the repo.

**Use your personal Gmail.** Do not skip account creation — no Notion-only or zip-only workaround for this project.

### Step 0.1 — Sign up

1. Go to [https://github.com/signup](https://github.com/signup)
2. Enter your **personal Gmail address** (the one you check daily)
3. Create a password → verify email → complete signup
4. Pick a **username** (e.g. `johnsmith-pm`) — you will send this to the founder
5. Skip team/enterprise prompts unless asked — free account is fine

### Step 0.2 — Tell the founder

Send a message like:

> GitHub username: `[your-username]`  
> Email: `[your-personal-gmail@gmail.com]`

The founder will invite you to the **rentalpro** private repo.

### Step 0.3 — Accept the invite

1. Check your Gmail inbox (and spam) for an email from GitHub
2. Subject is usually: “You've been invited to collaborate on …”
3. Click **View invitation** → **Accept invitation**
4. You should now see the repo at `github.com/bhagatks/rentalpro` (or similar)

### Step 0.4 — Bookmark these GitHub pages

| Page | URL | What it is |
|------|-----|------------|
| Repo home | `github.com/[org]/rentalpro` | All project files in the browser |
| This guide | `docs/partner/PARTNER-START-HERE.md` | Click through: docs → partner |
| Handoff | `docs/HANDOFF-TO-CURSOR.md` | Vision + locked decisions |

You can **read docs in the browser** on day one even before installing anything else.

---

## Part 0B — Founder: add you as Collaborator

**Partner:** copy this section and send it to the founder with your GitHub username.  
**Founder:** follow these steps once.

### What “Collaborator” means

| | Partner (Collaborator) | Founder (repo owner) |
|--|------------------------|----------------------|
| **Can do** | Read repo, edit docs, push branches, open Issues & PRs | Everything + merge to `main`, repo settings, invites |
| **Cannot do** | Change repo settings, invite others, delete repo | — |
| **Access level needed** | **Write** (not Read-only) | Owner |

### Founder steps (5 minutes)

1. Open the repo in a browser: `https://github.com/bhagatks/rentalpro`
2. Click **Settings** (top tab — only visible to owner)
3. Left sidebar → **Collaborators** (under “Access”)
   - If GitHub asks for your password or 2FA, complete verification
4. Click **Add people**
5. Type the partner’s **GitHub username** (exact spelling)
6. Select role: **Write** → click **Add [username] to this repository**
7. GitHub sends an invite email to the partner’s Gmail

**Done when:** Partner confirms they accepted the invite and can open the repo.

### If the invite does not arrive

| Check | Action |
|-------|--------|
| Wrong username | Partner sends profile URL: `github.com/[username]` |
| Spam folder | Partner searches inbox for “GitHub” |
| Pending invite | Settings → Collaborators → see “Pending invite” → **Resend invitation** |
| Still stuck | 10-min screenshare: partner shares screen on github.com |

---

## Part 2 — Who updates what (you vs founder)

This is the split. Stay in your lane to avoid overwriting each other.

### You (partner) own and edit

| Area | Files / folders | Your job |
|------|-----------------|----------|
| **Capability specs** | `docs/capabilities/CAP-*.md` | Intent, success, user stories, open questions, lock status |
| **Product decisions log** | `_bmad-output/specs/spec-rentalpro/.memlog.md` | Append every decision (via AI tool) |
| **Market gaps** | `docs/MARKET-GAP-CHECKLIST.md` | Mark items MVP / Phase 2 / Never |
| **AI product calls** | `docs/AI-MVP-DECISIONS.md` | Resolve TBD items with founder, then log |
| **Constraints / Non-goals / Success** | `_bmad-output/specs/spec-rentalpro/SPEC.md` | Draft sections (AI re-derives from memlog) |
| **Questions for founder** | GitHub **Issues** you create | See Part 2B below |

### Founder owns (do not edit without asking)

| Area | Files / folders | Founder’s job |
|------|-----------------|-----------------|
| **Vision & business model** | `docs/HANDOFF-TO-CURSOR.md` | Locked — partner builds on, does not rewrite |
| **Agent / engineering rules** | `AGENTS.md`, `docs/rules/`, `.cursor/rules/` | Engineering conventions |
| **Merge to main** | Pull requests | Founder reviews and merges your PRs |
| **Infrastructure locks** | CAP-11, stack choices | Already locked by founder |
| **Code & architecture** | App code, Prisma, APIs | Not started until after spec/PRD |
| **Repo admin** | GitHub Settings, collaborators | Founder only |

### Both of you can read everything

When unsure: **open a GitHub Issue** and ask before editing a file in the founder-only column.

---

## Part 2B — How to reach the founder and tell him what to do

Use **GitHub Issues** as your inbox to the founder. He gets an email notification when you create one.

### When to open an Issue

| Situation | Example title |
|-----------|---------------|
| You need a **founder decision** | `Founder decision: screening income multiplier for CAP-2` |
| You **locked a CAP** and want review | `Review: CAP-4 accounting draft ready to lock` |
| You are **blocked** | `Blocked: need answer on deposits — MVP or Phase 2?` |
| You pushed a **branch/PR** | `PR ready: partner CAP-2 updates for review` |
| Something is **founder-only** | `Founder action: update HANDOFF with new pricing call` |

### How to create an Issue (browser — no coding)

1. Go to `https://github.com/bhagatks/rentalpro`
2. Click **Issues** tab → green **New issue**
3. Use this template:

```markdown
## What I need from the founder
[One sentence — e.g. "Pick income multiplier for screening"]

## Context
- CAP / doc: [e.g. CAP-2, AI-MVP-DECISIONS.md]
- What I already decided: [bullet list or "none yet"]

## Options (if you have a recommendation)
1. Option A — …
2. Option B — …
**My recommendation:** …

## What I changed (if anything)
- Branch / PR: [link or "not pushed yet"]
- Files: [e.g. memlog, CAP-2 doc]

## Urgency
- [ ] Blocking my work
- [ ] Can wait until weekly sync
```

4. Click **Submit new issue**
5. Founder gets notified by email and on GitHub

### How to @mention the founder

In the Issue comment box, type `@bhagatks` (founder’s GitHub username) to ping him directly.

### Your push workflow (keeps `main` clean)

Do **not** push directly to `main` until the founder says you can. Use this flow:

```
1. GitHub Desktop → Current branch → New branch
   Name: partner/cap-2-screening  (use descriptive names)
2. Make doc changes with Cursor / Claude Code
3. GitHub Desktop → Commit → Push origin
4. On GitHub.com → "Compare & pull request" banner → Open PR
5. Open an Issue OR add PR description: "Ready for review — [summary]"
6. Founder merges → you pull latest in GitHub Desktop (Fetch → Pull)
```

**Partner creates branch + PR. Founder merges.** That is the handoff.

---

## Part 1 — Get the project on your laptop

After GitHub access works, get a local copy for Cursor or Claude Code.

### Step 1.1 — Install GitHub Desktop (recommended — no Terminal)

GitHub Desktop is a simple app to download the repo without typing commands.

1. Go to [https://desktop.github.com](https://desktop.github.com)
2. Download and install for Mac or Windows
3. Sign in with the **same GitHub account** (personal Gmail)
4. **File → Clone repository**
5. Select **rentalpro** from the list (or paste the repo URL the founder sends)
6. Choose a location (e.g. `Documents/rentalpro`) → **Clone**
7. You now have a folder on your computer with `docs/` and `_bmad-output/`

**Alternative (browser only, no install):** On the repo page, click green **Code → Download ZIP**, unzip, open in Cursor. Re-download when the founder says there are updates — GitHub Desktop is easier long-term.

### Step 1.2 — Setup checklist

- [ ] GitHub account created with **personal Gmail**
- [ ] Username sent to founder
- [ ] Repo invite **accepted**
- [ ] Project folder on laptop (GitHub Desktop clone or zip)
- [ ] AI tool installed (Cursor or Claude Code — next sections)

You do **not** need to learn git commands, branches, or pull requests yet. Focus on reading docs and logging decisions; the founder helps with git sync early on.

---

### Step 1.3 — Pick your AI tool

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
3. Select your cloned `rentalpro` folder (e.g. `Documents/rentalpro` from GitHub Desktop)
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

| Surface | Non-technical friendly? | Notes |
|---------|---------------------------|-------|
| **Desktop app** | ⭐ Best | Open local clone from GitHub Desktop |
| **Terminal CLI** | Medium | `cd` into cloned folder, type `claude` |
| **Extension inside Cursor** | Good | Same folder as Cursor |
| **Web** ([claude.ai/code](https://claude.ai/code)) | Good | Sign in with GitHub — uses repo directly |

**Recommended:** Clone with GitHub Desktop, then open that folder in Claude Code Desktop or Cursor.

### B2. Install — Desktop app (recommended for you)

1. Download Claude desktop: [claude.ai/download](https://claude.ai/download)
2. Install and sign in (Claude Pro, Max, Team, or Enterprise)
3. Open the app → click the **Code** tab
4. **Open folder** → select your cloned `rentalpro` folder (from GitHub Desktop)
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
cd ~/Documents/rentalpro
claude
```

(Change the path to wherever GitHub Desktop cloned the repo.)

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

### B7. Saving your work (branch + PR — not straight to main)

After you approve doc changes in Cursor or Claude Code:

1. Open **GitHub Desktop**
2. **Current branch → New branch** (e.g. `partner/cap-4-review`)
3. You should see changed files listed on the left
4. Write a short summary (e.g. “CAP-4: monthly sign-off flow clarified”)
5. Click **Commit to partner/cap-4-review** → **Push origin**
6. GitHub.com shows **Compare & pull request** → open PR
7. Open a GitHub **Issue** or comment on the PR: `@bhagatks ready for review`

Founder merges. Then **Fetch origin → Pull** in GitHub Desktop to sync.

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

When ready, commit on a **partner branch** and open a **Pull Request** — do not push to `main` unless the founder told you to. See **Part 2B**.

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
3. Log answers via your AI tool (or GitHub issue comments if reading in browser only)
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

### Task A — Setup + read (45 min)

**Setup (do first):**

- [ ] GitHub account with **personal Gmail**
- [ ] Username sent to founder → invite accepted
- [ ] Repo cloned via GitHub Desktop (or reading in browser)

**Read:**

- [ ] `docs/HANDOFF-TO-CURSOR.md`
- [ ] `docs/AI-MVP-DECISIONS.md`
- [ ] `docs/capabilities/CAP-2-autonomous-leasing.md`
- [ ] `docs/capabilities/CAP-4-autonomous-accounting.md`

### Task B — Answer (AI chat or GitHub comment)

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
| Skip GitHub account setup | Founder cannot invite you without a username + Gmail |
| Use a throwaway email | Use **personal Gmail** — invite goes there |
| Reopen **Why** / business model | Already locked; wastes time |
| Write requirements without **Success** criteria | Engineers cannot test vibes |
| Edit `SPEC.md` directly without memlog | Gets overwritten; decision is lost |
| Jump to “let’s build X” | Spec phase is not finished |
| Worry about database or APIs | Architecture comes after PRD |
| Treat copies outside GitHub as source of truth | Repo + memlog win |

---

## Part 13 — Glossary

| Term | Meaning |
|------|---------|
| **GitHub** | Where project files live; you need an account (personal Gmail) |
| **GitHub Desktop** | Simple app to clone and push — no Terminal commands |
| **Repo** | The project folder on GitHub (rentalpro) |
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
| Need founder to decide something | Open a GitHub **Issue** — see Part 2B template |
| PR ready for review | Comment `@bhagatks` on the PR + short summary |
| Not sure if a file is yours to edit | Check **Part 2** table; if founder-owned, open Issue first |
| GitHub invite not received | Check spam; send founder your exact username + Gmail |
| Founder unavailable | Open Issue (non-urgent) + note in CAP **Open questions** |
| Cursor vs Claude Code | Either works — same prompts, same files |
| Want the full BMad map | Read `docs/BMAD-REFERENCE.md` (optional, denser) |

---

## Quick reference card (pin this)

```
GITHUB  → Personal Gmail → founder adds Collaborator (Write) → accept invite
CLONE   → GitHub Desktop → rentalpro folder
OPEN    → Cursor or Claude Code Desktop
READ    → Part 2: know what YOU edit vs founder
WORK    → CAP docs, memlog, checklist — your files only
HANDOFF → New branch → commit → push → Pull Request → Issue @founder
MERGE   → Founder merges PR → you Pull in GitHub Desktop
```

**Tool pick:** Cursor = visual · Claude Code Desktop = if you already use Claude · Same prompts either way.

**You are not behind. You are exactly who this phase needs.**

---

*Questions about this guide? Tell the founder or paste into Cursor / Claude Code: “Update docs/partner/PARTNER-START-HERE.md based on my question: …”*
