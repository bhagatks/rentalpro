# M6: Eviction & Legal Notices — Non-goal (Deferred)

**Status:** non-goal for MVP — revisit Phase 2  
**Locked:** 2026-07-05  
**Market gap:** M6  
**MVP coverage without M6:** M2 delinquency + Notice to Pay template (TX 2026 first delinquency) — **not** full eviction workflow

> **Partner + attorney review required before any M6 scope change.** Attorney items (tagged Mandatory / Deferred): [`ATTORNEY-REVIEW-CHECKLIST.md`](./ATTORNEY-REVIEW-CHECKLIST.md) §6 + checklist below when reopening M6.

---

## Decision (locked)

| Item | Value |
|------|-------|
| M6 scope | **D — Non-goal for MVP** |
| PM handles eviction | Outside platform — attorney/court process |
| Platform provides | M2 late fees, reminders, **Notice to Pay template** only |
| Revisit trigger | Post-Texas MVP validation + partner + attorney sign-off |

---

## Partner checklist (before reopening M6)

Discuss with partner and record answers in `.memlog.md`:

- [ ] **GTM impact:** Do target PM customers require in-app eviction tracking to buy, or is manual/attorney workflow acceptable for first 10 customers?
- [ ] **Liability appetite:** Are we willing to productize court/filing workflows, or stay at notice-generation only?
- [ ] **Support burden:** Who handles tenant disputes and wrongful-eviction claims if we ship M6?
- [ ] **Competitive must-have:** Is Buildium/AppFolio eviction module a deal-breaker for our ICP (10–500 unit PM cos)?
- [ ] **Scope preference if reopened:** **B** (notices + deadline tracking only) vs **A** (full filing/court tracker)?
- [ ] **Integration:** Use external eviction attorney API/service vs build native?
- [ ] **Insurance:** E&O / legal review budget for eviction features?
- [ ] **Partner sign-off date:** _______________

---

## Attorney checklist (Texas — before any M6 build)

Engage Texas RE attorney; do **not** ship eviction features without written review.

### Notices already in MVP (M2 — confirm only)

- [ ] **Notice to Pay Rent or Vacate** — 2026 first delinquency in lease term wording approved?
- [ ] Delivery method: email/SMS sufficient or certified mail required?
- [ ] Cure period length in template matches current TX law?
- [ ] Fair Housing: notice content excludes discriminatory language?

### If Phase 2 scope B (notices + tracking only)

- [ ] **3-day notice to vacate** (non-rent violations) — template required?
- [ ] **30-day notice** (month-to-month termination) — template required?
- [ ] Log service date, method, recipient — minimum audit fields?
- [ ] Retention period for notice records?
- [ ] Can PM e-sign notices on behalf of owner, or owner must approve each?

### If Phase 2 scope A (full eviction workflow)

- [ ] Platform **must not** auto-file court documents without attorney review?
- [ ] Required disclaimers: "not legal advice" + PM must use licensed counsel?
- [ ] Writ of possession / lockout — any platform role or PM-only?
- [ ] SB 2349 / local ordinances affecting notice content?
- [ ] Multi-state: defer until TX validated?

### Attorney sign-off

| Item | Version | Approved by | Date |
|------|---------|-------------|------|
| Notice templates (M2) | | | |
| M6 scope B templates | | | |
| M6 scope A workflow | | | |

---

## What competitors do (reference when revisiting)

| Platform | Eviction / legal module |
|----------|-------------------------|
| **Buildium** | Rent reminders → late fees → eviction tracking workflow |
| **AppFolio** | Notices + process tracking; PM still drives legal steps |
| **DoorLoop** | Late rent workflows; less full court integration |

RentalPro MVP parity: **M2 delinquency** covers rent side; **M6 deferred** intentionally.

---

## If M6 reopened later

1. Partner checklist ✅  
2. Attorney checklist ✅  
3. Pick **B** or **A**  
4. Create full 9-section req doc (`docs/templates/MARKET-GAP-REQ-TEMPLATE.md`)  
5. Update SPEC Non-goals → remove or narrow M6 row  
6. Append `.memlog.md`

---

## Related artifacts

| Doc | Link |
|-----|------|
| M2 delinquency + Notice to Pay | [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) |
| M5 deposits (often precedes eviction) | [`SECURITY-DEPOSIT-MVP-REQ.md`](./SECURITY-DEPOSIT-MVP-REQ.md) |
| Traceability | [`REQ-TRACEABILITY.md`](./REQ-TRACEABILITY.md) |
