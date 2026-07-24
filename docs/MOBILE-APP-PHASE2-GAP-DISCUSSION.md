# Mobile App — TODOs (Phase 2)

**Status:** open todos (not locked)  
**Updated:** 2026-07-24  

> Native iOS/Android = **MVP Non-goal** (responsive web only). Architecture is already multi-client for a later Expo app. Capture discussion items here; lock later.

## Locked (do not re-litigate unless we reopen Non-goal)

- [x] Native apps out of MVP — SPEC §Non-goals
- [x] API-first so mobile can plug into same tRPC later — architecture spine AD-1 / AD-3
- [x] MVP resident/owner/PM surfaces = web portals (CAP-7 / CAP-8 / CAP-11)

## TODOs — discuss & decide

- [ ] Keep native as Phase 2, or reopen Non-goal for a thin resident app?
- [ ] First native persona: resident / owner / PM admin / vendor?
- [ ] One multi-role app vs separate apps?
- [ ] PWA install prompt on CAP-7 web as a bridge? (open in CAP-7)
- [ ] Push notifications: Phase 2 hard requirement vs SMS/email only?
- [ ] Offline / flaky-network photo upload for maintenance?
- [ ] Org resolution on native without subdomain (invite link / picker / per-PM apps)?
- [ ] Store branding: RentalPro shell + runtime PM theme vs per-PM store listings?
- [ ] Owner native timing vs M8 web owner portal UX (still open)?
- [ ] Vendor/tech app Phase 2 vs stay SMS/email?
- [ ] Phase 2 success signal (e.g. resident rent + maintenance from native)?
- [ ] Confirm `apps/mobile` scaffold exists when code starts (spine assumes it)

## Related

| Doc | Why |
|-----|-----|
| `SPEC.md` §Non-goals | Locked “no native in MVP” |
| `docs/ARCHITECTURE-OVERVIEW.md` | Multi-client from Day 1 |
| `docs/capabilities/CAP-7-resident-portal.md` | PWA open Q |
| `docs/PILOT-P0-ASSUMPTIONS-VALIDATION.md` | Resident **web** portal adoption gate |
