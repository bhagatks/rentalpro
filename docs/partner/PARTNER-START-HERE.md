# RentalPro.ai — Partner Start Guide

**For:** Partners  
**Last updated:** 2026-07-05  
**Focus:** GitHub setup, Cursor + Claude Code, BMad workflow  

Welcome. This guide walks through account setup, repo access, tools, and how we work together on RentalPro. The steps below make the mechanics clear.

**First step:** Create a GitHub account with your **personal Gmail** (Part 0).

**Repository:** [https://github.com/bhagatks/rentalpro](https://github.com/bhagatks/rentalpro) (private — your partner sends the invite)

---

## What you are helping with

RentalPro.ai is an AI property management platform. Before engineering starts, we **lock what the product does** — with clear language and testable outcomes.

Your focus in this phase:

1. **Read** what exists
2. **Ask** strong product questions
3. **Decide** what belongs in MVP vs later
4. **Record** decisions so the team and AI agents have a single source of truth

That is BMad. You both use Cursor or Claude Code to update documentation, align on decisions, and open PRs for each other to review (see Part 2).

---

## BMad intro — what you need on day one

BMad is the **method we use to build RentalPro** — a fixed order of steps so we lock *what* the product does before we design *how* or write code. You do not need the full BMad manual to start; this section covers what matters **right now**.

### Where we are

```
Ideas → SPEC → PRD → Architecture → Stories → Code
          ↑
     YOU ARE HERE (bmad-spec)
```

| Spec box | Status | Your involvement |
|----------|--------|------------------|
| **Why** | ✅ Locked | Read only — do not reopen |
| **Capabilities** (CAP-1 … CAP-12) | 🔄 Draft | **Main focus** — review, sharpen, lock |
| **Constraints** | ⏳ Next | Lead together |
| **Non-goals** | ⏳ Next | Lead together |
| **Success signal** | ⏳ Next | Lead together |

PRD, architecture, and code come **after** all five boxes are filled. Full workflow reference: [`docs/BMAD-REFERENCE.md`](../BMAD-REFERENCE.md) when you want the deep dive.

### Six terms you will see immediately

| Term | One-line meaning |
|------|------------------|
| **SPEC** | One-page product contract (`_bmad-output/specs/spec-rentalpro/SPEC.md`) |
| **Memlog** | Decision diary — append here **first** (`.memlog.md`) |
| **CAP** | One capability — must have **Intent** (what) + **Success** (how to test) |
| **Companion** | Extra detail files (competitive matrix, CAP deep-dives in `docs/capabilities/`) |
| **Lock** | Requirement is agreed — update memlog + mark CAP status |
| **Re-derive** | AI updates SPEC from memlog (do not hand-edit SPEC alone) |

### The one loop you will repeat daily

```
Read CAP doc  →  Discuss / decide  →  Log in memlog  →  Update CAP  →  Re-derive SPEC  →  PR for review
```

That loop **is** BMad spec work. Everything else in BMad builds on it later.

### Commands to use in Cursor or Claude Code (copy-paste)

These are the prompts you need **immediately** — paste into **Cursor Agent** (`Cmd+I` / `Ctrl+I`) or **Claude Code**:

**1. Orient (start of any session)**

```
Read _bmad-output/specs/spec-rentalpro/.memlog.md and SPEC.md.
Where are we in BMad spec phase? What should I focus on today?
Focus on product requirements, not implementation.
```

**2. Log a decision (most important — use after every alignment)**

```
We decided: [state the decision clearly].
Append to _bmad-output/specs/spec-rentalpro/.memlog.md,
update the relevant CAP doc in docs/capabilities/,
and re-derive _bmad-output/specs/spec-rentalpro/SPEC.md.
Summarize what you changed.
```

**3. Review one capability**

```
Review docs/capabilities/CAP-[N]-*.md.
Is Intent clear? Is Success testable? What's missing for MVP?
List open questions to align on with your partner.
```

**4. Pressure-test a requirement**

```
Read CAP-[N] and docs/MARKET-GAP-CHECKLIST.md.
What could go wrong or scope-creep if we ship this as written?
Suggest Constraints or Non-goals we should add.
```

**5. Ask what to do next**

```
Read .memlog.md and docs/partner/PARTNER-START-HERE.md.
What is the highest-value BMad spec task for the next 30 minutes?
```

### BMad skills — useful later (not day one)

In Cursor or Claude Code you can also ask for BMad-style help by name:

| Say this | What it does |
|----------|--------------|
| “Where are we in BMad?” | Orientation — like `bmad-help` |
| “Pressure-test this CAP” | Deeper critique — like `bmad-advanced-elicitation` |
| “Draft Constraints for SPEC” | Workshop the Constraints box |
| “Compare to AppFolio for CAP-N” | Competitive context from `competitive-matrix.md` |

You do not install these separately — they are **ways of asking** the AI, grounded in our repo rules.

### What to read when you have 15 more minutes

| Order | File | Why |
|-------|------|-----|
| 1 | `docs/HANDOFF-TO-CURSOR.md` | Vision + locked business decisions |
| 2 | `_bmad-output/specs/spec-rentalpro/SPEC.md` | The spec kernel at a glance |
| 3 | One CAP, e.g. `docs/capabilities/CAP-2-autonomous-leasing.md` | See Intent + Success in practice |

Part 4 below expands the workflow; Part 9 has copy-paste prompts for ongoing work.

---

## Part 0 — Create your GitHub account (do this first)

GitHub is where the project files live. You need your **own account** so your **other partner** can invite you as a Collaborator.

**Use your personal Gmail.** Do not skip account creation — no Notion-only or zip-only workaround for this project.

### Step 0.1 — Sign up

1. Go to [https://github.com/signup](https://github.com/signup)
2. Enter your **personal Gmail address** (the one you check daily)
3. Create a password → verify email → complete signup
4. Pick a **username** (e.g. `johnsmith-pm`) — send this to your **other partner**
5. Skip team/enterprise prompts unless asked — free account is fine

### Step 0.2 — Tell your other partner

Send a message like:

> GitHub username: `[your-username]`  
> Email: `[your-personal-gmail@gmail.com]`

Your partner will invite you to the **rentalpro** private repo as a **Collaborator**.  
Details: [PARTNER-GITHUB-SETUP.md](./PARTNER-GITHUB-SETUP.md)

### Step 0.3 — Accept the invite

1. Check your Gmail inbox (and spam) for an email from GitHub
2. Subject is usually: “You've been invited to collaborate on …”
3. Click **View invitation** → **Accept invitation**
4. You should now see the **rentalpro** repo on GitHub

### Step 0.4 — Bookmark these GitHub pages

| Page | URL | What it is |
|------|-----|------------|
| Repo home | `github.com/[org]/rentalpro` | All project files in the browser |
| This guide | `docs/partner/PARTNER-START-HERE.md` | Click through: docs → partner |
| Handoff | `docs/HANDOFF-TO-CURSOR.md` | Vision + locked decisions |

You can **read docs in the browser** on day one even before installing anything else.

---

## Part 0B — Adding your partner on GitHub

**Need access?** Send your GitHub username to your **other partner**.  
**Created the repo?** Follow the invite steps below (or see [PARTNER-GITHUB-SETUP.md](./PARTNER-GITHUB-SETUP.md)).

### What “Collaborator” means

Both partners get **Write** access. That means read, edit, branch, PR, and merge — with the habit of reviewing each other’s PRs before merging to `main`.

### Invite steps (whoever created the repository)

1. Open the **rentalpro** repo on GitHub
2. Click **Settings** (top tab)
3. Left sidebar → **Collaborators** (under “Access”)
   - Complete password or 2FA if GitHub asks
4. Click **Add people**
5. Type your partner’s **GitHub username** (exact spelling)
6. Select role: **Write** → click **Add [username] to this repository**
7. GitHub sends an invite email to your partner’s Gmail

**Done when:** Your partner confirms they accepted the invite and can open the repo.

### If the invite does not arrive

| Check | Action |
|-------|--------|
| Wrong username | Partner sends profile URL: `github.com/[username]` |
| Spam folder | Search inbox for “GitHub” |
| Pending invite | Settings → Collaborators → **Resend invitation** |
| Still stuck | Quick screenshare on github.com |

---

## Part 2 — Files we work on (this phase)

Both partners work from the same repo. Align on a change before editing locked baseline files.

| Area | Files / folders | What we do |
|------|-----------------|------------|
| **Capability specs** | `docs/capabilities/CAP-*.md` | Intent, success, user stories, open questions, lock status |
| **Product decisions log** | `_bmad-output/specs/spec-rentalpro/.memlog.md` | Append every decision (via AI tool) |
| **Market gaps** | `docs/MARKET-GAP-CHECKLIST.md` | Mark items MVP / Phase 2 / Never |
| **AI decisions** | `docs/AI-MVP-DECISIONS.md` | Resolve TBD items together, then log |
| **Spec kernel** | `_bmad-output/specs/spec-rentalpro/SPEC.md` | Draft Constraints / Non-goals / Success (AI re-derives from memlog) |
| **Vision & business model** | `docs/HANDOFF-TO-CURSOR.md` | Locked baseline — align before changing |
| **Engineering rules** | `AGENTS.md`, `docs/rules/` | Align before changing |
| **Handoffs** | GitHub **Issues** and **PRs** | See Part 2B |

Code and architecture folders matter **after** spec and PRD — not the focus right now.

When unsure: **open a GitHub Issue** and @mention your partner.

---

## Part 2B — How to reach your other partner

Use **GitHub Issues** as your shared inbox. Both partners get email notifications when Issues are created.

### When to open an Issue

| Situation | Example title |
|-----------|---------------|
| You need a **joint decision** | `Partner sync: screening income multiplier for CAP-2` |
| You **locked a CAP** and want review | `Review: CAP-4 accounting draft ready to lock` |
| You are **blocked** | `Blocked: need answer on deposits — MVP or Phase 2?` |
| You pushed a **branch/PR** | `PR ready: partner/cap-2-updates for review` |
| Something needs **alignment on HANDOFF or engineering files** | `Partner sync: update HANDOFF with new pricing call` |

### How to create an Issue (in the browser)

1. Go to the **rentalpro** repo on GitHub
2. Click **Issues** tab → green **New issue**
3. Use this template:

```markdown
## What I need from my partner
[One sentence — e.g. "Align on income multiplier for screening"]

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
5. Your partner gets notified by email and on GitHub

### How to @mention your partner

In the Issue comment box, type `@` plus your partner’s GitHub username to notify them.

### Your push workflow (keeps `main` clean)

Use a **branch and Pull Request** for doc changes — review before merging to `main`:

```
1. GitHub Desktop → Current branch → New branch
   Name: partner/cap-2-screening  (use descriptive names)
2. Make doc changes with Cursor / Claude Code
3. GitHub Desktop → Commit → Push origin
4. On GitHub.com → "Compare & pull request" → Open PR
5. @mention your partner: "Ready for review — [summary]"
6. Partner merges → Fetch → Pull in GitHub Desktop
```

**Branch → PR → review → merge.** That is the handoff between partners.

---

## Part 1 — Get the project on your laptop

After GitHub access works, get a local copy for Cursor or Claude Code.

### Step 1.1 — Install GitHub Desktop (recommended)

GitHub Desktop lets you clone and sync the repo with a visual interface — no command-line workflow required unless you prefer one.

1. Go to [https://desktop.github.com](https://desktop.github.com)
2. Download and install for Mac or Windows
3. Sign in with the **same GitHub account** (personal Gmail)
4. **File → Clone repository**
5. Select **rentalpro** from the list (or paste the repo URL your partner sends)
6. Choose a location (e.g. `Documents/rentalpro`) → **Clone**
7. Confirm the folder contains `docs/` and `_bmad-output/`

**Alternative:** On the repo page in your browser, click green **Code → Download ZIP**, unzip, and open in Cursor. GitHub Desktop is easier when you sync updates after merges.

### Step 1.2 — Setup checklist

- [ ] GitHub account created with **personal Gmail**
- [ ] Username sent to your other partner
- [ ] Repo invite **accepted**
- [ ] Project folder on laptop (GitHub Desktop clone or zip)
- [ ] AI tool installed (Cursor or Claude Code — next sections)

During the first week, focus on reading docs and logging decisions. Walk through branch/PR workflow together on the first merge — see Part 2B and Part 1B (section B7).

---

### Step 1.3 — Pick your AI tool

Both partners use **Cursor** and/or **Claude Code**. Both tools read the same project files and follow the same rules (`AGENTS.md`, `docs/rules/`). Choose one primary tool — both work with the same prompts in this guide.

| Tool | Good fit when… | What it looks like |
|------|----------------|-------------------|
| **Cursor** | You prefer a visual editor with folders and an AI panel side by side | VS Code + integrated AI chat |
| **Claude Code Desktop** | You already use Claude (Pro/Max/Team) and want a dedicated app | Claude app → **Code** tab |
| **Claude Code Terminal** | You prefer working from the command line | Terminal session with Claude |
| **Claude in Cursor** (extension) | You use Cursor and want Claude Code inside it | Cursor + “Claude Code” extension |

**Default recommendation:** **Cursor** — strong for browsing docs and running Agent mode in one window.

---

## Part 1A — Cursor setup (step by step)

Cursor is an AI-enabled editor. For this project you will use it primarily to **read and update product documentation** with Agent assistance.

### A1. Install Cursor

1. Go to [https://cursor.com](https://cursor.com)
2. Download for Mac or Windows
3. Create an account and sign in
4. Select a plan (Free works to start; Pro offers higher AI usage limits)

### A2. Open the project folder

1. Launch Cursor
2. **File → Open Folder…**
3. Select your cloned `rentalpro` folder (e.g. `Documents/rentalpro` from GitHub Desktop)
4. In the left sidebar you should see folders like `docs`, `_bmad-output`

**Layout — two areas matter most for spec work:**

| Area | Location | Purpose |
|------|----------|---------|
| File tree | Left sidebar | Open and read markdown docs |
| Agent / Chat | Right panel | AI conversation and document updates |

The center editor shows file contents when you select a doc from the tree.

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
I am a partner on RentalPro.ai, focused on requirements and spec.
Read docs/partner/PARTNER-START-HERE.md and docs/HANDOFF-TO-CURSOR.md.
Summarize where we are in the project and recommend my highest-value next step.
Focus on product requirements, not implementation.
```

### A4. Cursor shortcuts to remember

| Action | Mac | Windows |
|--------|-----|---------|
| Open Agent (edits files) | `Cmd+I` | `Ctrl+I` |
| Open Chat | `Cmd+L` | `Ctrl+L` |
| Reference a file in chat | Type `@` then filename | Same |
| Accept a change | Click **Accept** on the diff | Same |
| Reject a change | Click **Reject** | Same |

**Tip:** When the agent proposes edits, read the summary first, then Accept or Reject. You are approving **product decisions** captured in documentation.

### A5. Cursor modes

| Mode | Use when… | Edits files? |
|------|-----------|--------------|
| **Agent** | “Log this decision and update the CAP doc” | Yes |
| **Ask** | “Walk me through CAP-4 accounting requirements” | No |
| **Plan** | Big multi-file change — review plan first | After you approve |

For BMad spec work, use **Agent** most of the time.

---

## Part 1B — Claude Code setup (step by step)

Claude Code is Anthropic’s AI agent. It reads your project folder, edits markdown files, and asks before changing anything. Same job as Cursor Agent — different app.

Official docs: [code.claude.com/docs](https://code.claude.com/docs/en/overview)

### B1. Pick how you will run Claude Code

| Surface | Ease of use | Notes |
|---------|-------------|-------|
| **Desktop app** | ⭐ Recommended | Open local clone from GitHub Desktop |
| **Terminal CLI** | Familiar if you use Terminal | `cd` into cloned folder, type `claude` |
| **Extension inside Cursor** | Good | Same folder as Cursor |
| **Web** ([claude.ai/code](https://claude.ai/code)) | Good | Sign in with GitHub — works directly against the repo |

**Recommended:** Clone with GitHub Desktop, then open that folder in Claude Code Desktop or Cursor.

### B2. Install — Desktop app (recommended starting point)

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

First run opens a browser to log in. After that, type your requests at the prompt.

**Useful commands:**

| Command | What it does |
|---------|--------------|
| `claude` | Start a session in current folder |
| `claude -c` | Continue your last conversation |
| `/help` | List commands (type inside Claude Code) |
| `/clear` | Start fresh conversation |
| `/exit` | Quit |

**Prefer a graphical workflow?** Use the **Desktop app** (section B2). The Terminal path is optional.

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
I am a partner on RentalPro.ai, focused on requirements and spec.
Read docs/partner/PARTNER-START-HERE.md and docs/HANDOFF-TO-CURSOR.md.
Summarize where we are in the project and recommend my highest-value next step.
Focus on product requirements, not implementation.
```

### B6. Approving changes in Claude Code

Claude Code **asks for approval before editing files**.

1. Claude shows the proposed changes
2. Review the summary
3. Approve or reject
4. For spec work, expect updates to `docs/`, `.memlog.md`, and CAP files

If the diff is dense, ask: “Summarize what you changed in a few sentences.”

### B7. Saving your work (branch + PR — not straight to main)

After you approve doc changes in Cursor or Claude Code:

1. Open **GitHub Desktop**
2. **Current branch → New branch** (e.g. `partner/cap-4-review`)
3. You should see changed files listed on the left
4. Write a short summary (e.g. “CAP-4: monthly sign-off flow clarified”)
5. Click **Commit to partner/cap-4-review** → **Push origin**
6. GitHub.com shows **Compare & pull request** → open PR
7. Open a GitHub **Issue** or comment on the PR and @mention your partner: `ready for review`

Your partner merges. Then **Fetch origin → Pull** in GitHub Desktop to sync.

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

## Part 3 — Understand the project (read these in order)

Work through these at your own pace. The Day 1 list below is a suggested starting sequence.

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

### Out of scope for this phase (engineering)

These areas exist in the repo but are not the focus while we finish the spec:

- `AGENTS.md`, `docs/rules/` — engineering agent conventions
- Application source code, Prisma schema, API routes, tests — after spec and PRD

---

## Part 4 — BMad workflow (expanded)

This section builds on the **BMad intro** above. BMad is the step-by-step order we follow before engineering starts.

```
Ideas  →  SPEC  →  PRD  →  Architecture  →  Stories  →  Code
           ↑
      YOU ARE HERE
```

### The SPEC has 5 boxes

| Box | Meaning | Status today |
|-----|---------|--------------|
| **Why** | Why does this product exist? | ✅ Done — do not reopen |
| **Capabilities** | What can the product do? (12 features, CAP-1 to CAP-12) | 🔄 Draft — **your main focus** |
| **Constraints** | Hard rules that limit design | ⏳ Next — **you lead this** |
| **Non-goals** | Explicit “we are NOT building this” | ⏳ Next — **you lead this** |
| **Success signal** | How we know APM is actually working | ⏳ Next — **you lead this** |

**Golden rule:** Spec before PRD before architecture before code.  
If someone says “let’s just build the portal,” the answer is: **not until the five boxes are filled.**

---

## Part 5 — What is a “Capability” (CAP)?

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

## Part 6 — How decisions get saved (important)

There are two layers of documents:

```
docs/                    ← Human-friendly wiki (you read and edit here)
_bmad-output/            ← Machine contract (agents keep in sync)
```

### The decision loop (both partners use this)

```
1. You and your partner (or your AI tool) discuss a decision
2. Decision gets appended to .memlog.md  ← source of truth
3. Relevant CAP doc gets updated
4. SPEC.md gets re-derived from memlog
```

**File:** `_bmad-output/specs/spec-rentalpro/.memlog.md`  
This is the **decision diary**. Every lock gets a dated line.

**Say this in Cursor (Agent) or Claude Code:**

```
We decided: [state your decision clearly].
Append it to .memlog.md, update the relevant CAP doc, and re-derive SPEC.md.
```

The AI tools can apply edits for you — you review and approve. Same workflow both partners use.

When ready, commit on a **partner branch** and open a **Pull Request**. See **Part 2B** for the full handoff flow.

---

## Part 7 — Locked decisions (do not overturn)

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

## Part 8 — Your weekly rhythm

### Daily (15–30 min)

1. Open one CAP doc
2. Read **Intent**, **Success**, and **Open questions**
3. Log answers via your AI tool (or GitHub issue comments if reading in browser only)
4. One decision logged = a good day

### Weekly sync with your partner (30–45 min)

Agenda template:

1. Which CAPs did we review?
2. What got locked this week?
3. What is still TBD?
4. Any new market-gap items from `MARKET-GAP-CHECKLIST.md`?
5. Are we ready to start Constraints / Non-goals / Success signal?

---

## Part 9 — Copy-paste prompts (Cursor + Claude Code)

Use these in **Cursor Agent** (`Cmd+I` / `Ctrl+I`) or **Claude Code** (Desktop, Terminal, or extension). Replace the `[brackets]`.

### Orientation

```
I am a partner on RentalPro.ai, focused on requirements and spec.
Read docs/partner/PARTNER-START-HERE.md and .memlog.md.
What is the single highest-value thing I should do in the next 30 minutes?
Focus on product requirements, not implementation.
```

### Review one capability

```
Review docs/capabilities/CAP-[N]-*.md.
Is Intent clear? Is Success testable?
List open questions to align on with my partner.
Focus on product requirements, not implementation.
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
CAP-[N] requirements look complete after partner review.
Mark the CAP doc status as locked, append to .memlog.md, update SPEC.md.
List any remaining TBD items that do not block lock.
```

### Compare to competitors (product view)

```
Read competitive-matrix.md and CAP-[N] doc.
Summarize what AppFolio/Buildium do here and how our MVP approach differs.
```

---

## Part 10 — Your first assignment (Day 1)

Complete this before your first working session together.

### Task A — Setup + read (45 min)

**Setup (do first):**

- [ ] GitHub account with **personal Gmail**
- [ ] Username sent to your other partner → invite accepted
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

### Task C — Message your partner

A short message:

> I finished Day 1 reading. CAP-2 questions: … CAP-4 questions: …  
> Ready to sync on [date/time].

---

## Part 11 — Your second assignment (Week 1)

| Day | Focus | Output |
|-----|-------|--------|
| Mon | CAP-2 + CAP-3 | List of open questions + proposed decisions |
| Tue | CAP-4 + CAP-5 | Governance thresholds validated ($500 rule, sign-off flow) |
| Wed | CAP-7 + CAP-8 | Resident + owner portal MVP scope |
| Thu | `MARKET-GAP-CHECKLIST.md` | Mark 🔴 items: MVP / Phase 2 / Never |
| Fri | Constraints session together | First draft of Constraints section in SPEC |

---

## Part 12 — Open questions waiting for you

These are explicitly unresolved. Your product judgment is needed.

| Topic | Question | Where to look |
|-------|----------|---------------|
| Screening | Income multiplier? Eviction lookback? | `docs/AI-MVP-DECISIONS.md`, CAP-2 |
| Market gaps | Deposits, delinquency, renewals — MVP or not? | `docs/MARKET-GAP-CHECKLIST.md` |
| Audit depth | How much detail for tax/legal? | CAP-10 |
| Owner experience | Minimum viable owner report for MVP? | CAP-8 |

When you pick an answer, log it via Cursor or Claude Code (memlog → CAP → SPEC).

---

## Part 13 — What NOT to do

| Do not… | Because… |
|---------|----------|
| Skip GitHub account setup | Your partner cannot invite you without username + Gmail |
| Use a throwaway email | Use **personal Gmail** — invite goes there |
| Reopen **Why** / business model | Already locked; wastes time |
| Write requirements without **Success** criteria | Engineers cannot test vibes |
| Edit `SPEC.md` directly without memlog | Gets overwritten; decision is lost |
| Jump to implementation before spec is complete | Spec → PRD → architecture → code is the agreed order |
| Edit engineering-only files without alignment | See Part 2 — open an Issue first |
| Treat copies outside GitHub as source of truth | Repo + memlog win |

---

## Part 14 — Glossary

| Term | Meaning |
|------|---------|
| **GitHub** | Where project files live; you need an account (personal Gmail) |
| **GitHub Desktop** | Desktop app to clone, commit, and push — visual git workflow |
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

## Part 15 — When spec is “done”

You will know spec phase is complete when:

- [ ] All 12 CAPs are **locked** or explicitly deferred with reason
- [ ] **Constraints** section filled in `SPEC.md`
- [ ] **Non-goals** section filled in `SPEC.md`
- [ ] **Success signal** section filled in `SPEC.md`
- [ ] Open questions in `MARKET-GAP-CHECKLIST.md` assigned (MVP / Phase 2 / Never)

Then the project moves to **PRD** (more detailed user stories). You will still lead requirements — the format just gets richer.

---

## Part 16 — Getting help

| Need | Do this |
|------|---------|
| Unfamiliar term | Ask your AI: “Explain [term] in the context of property management SaaS” |
| Not sure if in scope | Ask: “Is [X] a Constraint, Non-goal, or Capability?” |
| Need partner input | Open a GitHub **Issue** — see Part 2B template |
| PR ready for review | @mention your partner on the PR + short summary |
| Not sure about editing a file | Check **Part 2** — open an Issue to align first |
| GitHub invite not received | Check spam; confirm username + Gmail with your partner |
| Partner unavailable | Open Issue (non-urgent) + note in CAP **Open questions** |
| Cursor vs Claude Code | Either works — same prompts, same files |
| Want the full BMad map | Read `docs/BMAD-REFERENCE.md` (optional, denser) |

---

## Quick reference card (pin this)

```
GITHUB  → Personal Gmail → partner sends invite (Write) → accept
CLONE   → GitHub Desktop → rentalpro folder
OPEN    → Cursor or Claude Code Desktop
READ    → Part 2: files we work on this phase
WORK    → CAP docs, memlog, checklist
HANDOFF → New branch → commit → push → Pull Request → @partner
MERGE   → Partner reviews → merge → Fetch → Pull
```

**Tool pick:** Cursor = visual · Claude Code Desktop = if you already use Claude · Same prompts either way.

**You are exactly who this phase needs.**

---

*Questions about this guide? Sync with your partner, or ask Cursor / Claude Code to update `docs/partner/PARTNER-START-HERE.md` based on your feedback.*

**GitHub invite steps:** [PARTNER-GITHUB-SETUP.md](./PARTNER-GITHUB-SETUP.md)
