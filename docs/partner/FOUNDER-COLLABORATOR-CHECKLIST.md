# Founder checklist — add partner as Collaborator

**For:** Repo owner (`bhagatks`)  
**Partner guide:** [PARTNER-START-HERE.md](./PARTNER-START-HERE.md)

---

## 1. Add Collaborator (Write access)

1. Open https://github.com/bhagatks/rentalpro
2. **Settings** → **Collaborators**
3. **Add people** → enter partner’s GitHub username
4. Role: **Write** → confirm invite
5. Ask partner to accept email invite and confirm they see the repo

---

## 2. Who owns what (quick split)

| Partner edits | You edit / merge |
|---------------|------------------|
| `docs/capabilities/CAP-*.md` | `docs/HANDOFF-TO-CURSOR.md` (locked vision) |
| `.memlog.md` (append decisions) | `AGENTS.md`, `docs/rules/` |
| `docs/MARKET-GAP-CHECKLIST.md` | Merge PRs to `main` |
| `docs/AI-MVP-DECISIONS.md` (with your sign-off) | Code, architecture (later) |
| Draft Constraints / Non-goals in `SPEC.md` | CAP-11 / infra locks |

Full table: Part 2 in partner guide.

---

## 3. How partner reaches you

Partner should **not** push to `main` directly. Expected flow:

```
Partner branch → PR → GitHub Issue or @bhagatks on PR → you review & merge
```

**Issue titles to watch for:**

- `Founder decision: …` — needs your product call
- `Review: CAP-N …` — doc ready to lock
- `Blocked: …` — waiting on you
- `Founder action: …` — your file or admin task

You get email notifications from GitHub when Issues/PRs are opened.

---

## 4. When partner sends username

Reply with:

> Invite sent — check Gmail (and spam) for GitHub invite.  
> After accepting, clone with GitHub Desktop and start at `docs/partner/PARTNER-START-HERE.md`.

---

## 5. First merge

After partner’s first PR:

- [ ] Review only their files (CAP docs, memlog, checklist — not HANDOFF/rules)
- [ ] Merge PR
- [ ] Tell partner to **Fetch → Pull** in GitHub Desktop
