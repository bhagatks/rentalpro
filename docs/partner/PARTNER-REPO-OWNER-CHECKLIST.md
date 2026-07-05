# Repo owner checklist — add co-founder as Collaborator

**For:** Partner who owns the GitHub repo (`bhagatks`)  
**Co-founder guide:** [PARTNER-START-HERE.md](./PARTNER-START-HERE.md)

You and your co-founder are equal partners. On GitHub, the **repo owner** handles invites and merges until you agree on a shared workflow.

---

## 1. Add Collaborator (Write access)

1. Open https://github.com/bhagatks/rentalpro
2. **Settings** → **Collaborators**
3. **Add people** → enter co-founder’s GitHub username
4. Role: **Write** → confirm invite
5. Ask co-founder to accept email invite and confirm they see the repo

---

## 2. Who owns what (quick split)

| Co-founder (Collaborator) edits | Repo owner edits / merges |
|---------------------------------|---------------------------|
| `docs/capabilities/CAP-*.md` | `docs/HANDOFF-TO-CURSOR.md` (locked vision) |
| `.memlog.md` (append decisions) | `AGENTS.md`, `docs/rules/` |
| `docs/MARKET-GAP-CHECKLIST.md` | Merge PRs to `main` |
| `docs/AI-MVP-DECISIONS.md` (with alignment) | Code, architecture (later) |
| Draft Constraints / Non-goals in `SPEC.md` | CAP-11 / infra locks |

Full table: Part 2 in [PARTNER-START-HERE.md](./PARTNER-START-HERE.md).

---

## 3. How co-founder reaches you

Co-founder should **not** push to `main` directly until you agree otherwise. Expected flow:

```
Co-founder branch → PR → GitHub Issue or @mention on PR → repo owner reviews & merge
```

**Issue titles to watch for:**

- `Partner sync: …` — needs a joint product decision
- `Review: CAP-N …` — doc ready to lock
- `Blocked: …` — waiting on you
- `Repo owner: …` — your file, merge, or admin task

You get email notifications from GitHub when Issues/PRs are opened.

---

## 4. When co-founder sends username

Reply with:

> Invite sent — check Gmail (and spam) for GitHub invite.  
> After accepting, clone with GitHub Desktop and start at `docs/partner/PARTNER-START-HERE.md`.

---

## 5. First merge

After co-founder’s first PR:

- [ ] Review their files (CAP docs, memlog, checklist — not HANDOFF/rules unless agreed)
- [ ] Merge PR
- [ ] Tell co-founder to **Fetch → Pull** in GitHub Desktop
