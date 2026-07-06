# M{N}: {Title} — Full Requirements

**Status:** draft | locked  
**Market gap:** M{N}  
**CAPs:** CAP-X, CAP-Y  
**Locked:** YYYY-MM-DD (if locked)

> **Use this template for every M1–M10 decision.** A locked market gap is not done until all 9 sections below are filled. Chat context must land here — not only in `.memlog.md`.

---

## 1. Problem & rationale

*Why this exists, what pain it solves, why now, competitive gap.*

---

## 2. Product vision (what we're building)

*Plain-language description. How RentalPro differs from AppFolio/Buildium/etc.*

---

## 3. User stories

| Actor | Story |
|-------|-------|
| PM admin | As a … I want … so that … |
| Resident / Prospect | … |
| AI agent | … |
| Owner | … |

---

## 4. End-to-end flows

### Happy path (autonomous — Pro plan)

1. Step …
2. Step …

### Happy path (Basic plan — human approve)

1. Step …

### Escalation / edge paths

| Trigger | System behavior | Human action |
|---------|-----------------|--------------|
| … | … | … |

---

## 5. Functional requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-1 | … | MVP |
| FR-2 | … | MVP |

---

## 6. Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-1 | Multi-tenant: all data scoped by `organizationId` |
| NFR-2 | CAP-10 audit trace for every autonomous action |
| NFR-3 | … |

---

## 7. Data model & integrations

| Entity / Service | Purpose |
|------------------|---------|
| … | … |

---

## 8. Acceptance criteria (testable)

- [ ] …
- [ ] …

---

## 9. Runbook (time-sensitive / prerequisites)

| ID | Task | Blocker? | Status |
|----|------|----------|--------|
| … | … | … | ⬜ |

---

## 10. Open questions & Phase 2

| Item | Status |
|------|--------|
| … | TBD |

**Related:** `REQ-TRACEABILITY.md` · `.memlog.md` · `MARKET-GAP-CHECKLIST.md`
