# Sharing Options for Your Partner

You have research, spec, and capability deep-dives. Pick one path based on how fast you need a shareable URL.

## Option comparison

| Option | Setup time | Partner UX | Best for |
|--------|------------|------------|----------|
| **A. GitHub repo** | 0 min (done) | Reads markdown on GitHub | Technical partner, tonight |
| **B. Notion** | 30–60 min | Best non-dev UX, comments | Co-founder who wants a real wiki |
| **C. MkDocs site** | 1–2 hours | Clean docs site + search | Long-term product wiki |
| **D. Vercel + VitePress** | 1–2 hours | Fast, modern, free hosting | If you already use Vercel |
| **E. GitHub Wiki** | 30 min | Wiki tab on repo | Simple, no extra deploy |
| **F. Google Docs export** | 15 min | Familiar, bad for code | One-off investor read |

## Recommended for you (hurry + MVP sprint)

### Tonight: **Option A** — GitHub repo

1. Push `rentalpro` to a private GitHub repo.
2. Share repo access with partner.
3. Point them to `docs/README.md` as the index.

**Pros:** Zero setup, versioned, you and Cursor keep writing markdown.  
**Cons:** Raw markdown on GitHub isn't pretty; no inline comments unless Issues/PRs.

### This weekend: **Option B** — Notion (fastest pretty wiki)

1. Create Notion workspace → Import folder → select `docs/` + `_bmad-output/specs/spec-rentalpro/competitive-matrix.md`.
2. Share link with partner (view or comment).
3. Keep Notion as **read mirror**; source of truth stays in git.

**Pros:** Partner-friendly, comments, mobile.  
**Cons:** Can drift from git unless you re-import or use Notion sync tools.

### Next week: **Option C** — MkDocs Material (permanent wiki site)

```bash
# One-time scaffold (when ready)
pip install mkdocs-material
mkdocs new .
# Point nav at docs/ → deploy to GitHub Pages or Netlify
```

**Pros:** Free URL like `rentalpro.docs.io`, search, sidebar, professional.  
**Cons:** 1–2 hour initial setup.

## What to share with partner (minimum packet)

Send these three links/paths:

1. **Vision & decisions** — `docs/HANDOFF-TO-CURSOR.md`
2. **Competitive research** — `_bmad-output/specs/spec-rentalpro/competitive-matrix.md`
3. **Capability index** — `docs/README.md` → each `docs/capabilities/CAP-N-*.md`

Optional fourth: `SPEC.md` for the formal contract.

## Keeping docs in sync

| Source of truth | Mirror (optional) |
|-----------------|-------------------|
| Git `docs/` + `_bmad-output/` | Notion import weekly |
| `.memlog.md` decisions | Summarize in CAP deep-dives |
| `SPEC.md` | Auto-derived after memlog updates |

**Rule:** Write in git first. Mirror to Notion/wiki for partner reading — never the reverse.

## Decision

| If partner is… | Pick |
|----------------|------|
| Technical, needs tonight | **A** GitHub |
| Non-technical, needs comments | **B** Notion |
| You want a public product wiki later | **C** MkDocs |

Tell me which option you want and I can scaffold it (MkDocs config, Notion import structure, or GitHub Pages).
