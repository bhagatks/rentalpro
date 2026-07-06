# M7: Unified Comms Hub — Full Requirements

**Status:** locked  
**Market gap:** M7  
**CAPs:** CAP-7 (primary expand), CAP-2, CAP-3, CAP-5, CAP-8, CAP-9, CAP-10, CAP-11  
**Locked:** 2026-07-05  
**MVP state:** Texas (`TX`) — comms legal applies federally + TX notice delivery (see attorney checklist §9)

> **Attorney review (Mandatory / Optional items):** [`ATTORNEY-REVIEW-CHECKLIST.md`](./ATTORNEY-REVIEW-CHECKLIST.md) §9.

> **Companion req doc for M7.** Expands CAP-7 from resident-only portal into PM **unified inbox** — resident, owner, vendor, and lead threads in one hub with SMS/email/chat aggregation.

---

## 1. Problem & rationale

PM staff today juggle disconnected channels: Zillow lead emails, resident SMS, owner reply-all threads, vendor text chains, and in-app portal messages. AppFolio **Realm-X Messages** and Buildium inbox unify parties in one place. Without M7, RentalPro's agents (Leasing, Maintenance, Delinquency) send messages that PM staff cannot see or override in a single view — breaking the APM governance story.

**Why it matters:** Autonomous agents **must** communicate; PM admins need one inbox to monitor, approve (Basic), and take over any thread. Residents, owners, and vendors should reply on their preferred channel while the PM sees one continuous conversation.

**Scope boundary:** Community feed / announcements remain **Phase 2 non-goal** (SPEC). Voice/IVR out of scope (Constraint #7).

---

## 2. Product vision

```
Inbound (SMS / email / portal chat / syndication lead webhook)
  → Normalize to Message on ConversationThread
  → Route to correct agent (Leasing / Maintenance / Delinquency / Comms)
  → Pro: agent auto-replies within policy
  → Basic: draft in PM inbox → PM approves send
  → PM admin: unified inbox — filter by party, property, unread, assigned agent
  → Outbound: fan-out on same thread via resident/owner/vendor preferred channel
```

**Differentiator vs market:**

| Market (AppFolio Realm-X, Buildium) | RentalPro |
|-------------------------------------|-----------|
| Unified inbox for staff | Same — PM admin hub |
| Manual replies dominate | **AI agents reply first**; PM monitors or approves |
| Threads disconnected from autonomous actions | Every agent message linked to CAP-10 `DecisionTrace` |
| Separate tools for leads vs residents | One thread model; lead → applicant → resident continuity |
| Bulk blast as add-on | Emergency bulk to property/building in MVP |

---

## 3. User stories

| Actor | Story |
|-------|-------|
| PM admin | I open one inbox and see all unread threads across leads, residents, owners, and vendors. |
| PM admin | I filter by property, party type, assigned agent, or SLA breach. |
| PM admin | I take over any thread from an AI agent and reply as myself; agent stops auto-reply until I release. |
| PM admin (Basic) | I approve agent-drafted outbound messages before they send. |
| PM admin (Pro) | I see agent-sent messages in real time; I can override or pause the agent on a thread. |
| PM admin | I send **bulk emergency message** to all residents in a property (e.g. water shutoff). |
| Lead / prospect | I text or email from a listing; I get a coherent reply whether Leasing Agent or PM responds. |
| Resident | I chat in portal, SMS, or email — I see one conversation, not three silos. |
| Resident | I receive delinquency reminders and renewal offers in the same thread history (M2, M3). |
| Owner | I receive renewal approval requests and statement questions in owner-branded email/SMS; replies land in PM inbox linked to my property. |
| Vendor | I receive work-order SMS/email; my quote reply attaches to the WO thread (CAP-3). |
| Leasing Agent | I handle inquiry threads; hand off to PM on governance escalation (CAP-5). |
| Maintenance Agent | I receive resident WO messages in the hub; vendor sub-thread on same WO. |
| Comms Agent | I draft/sent routine acknowledgments; never send legal notices without approved templates (M2 P3). |
| Platform ops | I see org-level SMS/email delivery failures and rate-limit alerts. |

---

## 4. End-to-end flows

### Happy path — inbound lead SMS (Pro)

1. Prospect texts number on Zillow listing → Twilio webhook → `ConversationThread` (partyType: `lead`, source: M1).
2. Leasing Agent loads thread context + Listing Package (M1) + unit availability.
3. Agent replies via SMS within 60s; message stored with `channel: sms`, `senderType: agent`.
4. CAP-10 `DecisionTrace` links to thread + message IDs.
5. PM admin sees thread in inbox (read/unread); no action required if within policy.
6. Lead converts → same thread `partyType` updates to `applicant` → `resident` on lease sign (no history loss).

### Happy path — resident maintenance chat

1. Resident sends photo via portal chat → message on existing `resident` thread (or new if first contact).
2. Maintenance Agent triages → creates WO (CAP-3) → replies with ETA.
3. Agent opens vendor sub-thread on WO; vendor SMS reply visible to PM in linked view.
4. Resident status updates post to same resident thread.

### Basic plan — agent draft approval

1. Leasing Agent generates reply → `Message.status = pending_approval`.
2. PM admin inbox shows **Draft** badge; CAP-5 approval queue.
3. PM edits/approves → outbound on preferred channel.
4. CAP-10 logs approval + final sent body hash.

### PM takeover

1. PM clicks **Take over** on thread → `thread.humanOwnerId = pmUserId`; agents paused.
2. PM sends manual replies; CAP-10 logs `human_override`.
3. PM clicks **Release to agent** → agents resume per org policy.

### Bulk emergency broadcast

1. PM selects property/building → **Emergency broadcast** template.
2. CAP-5: Basic always requires confirm; Pro optional one-click if template pre-approved.
3. Fan-out SMS + email to all active leases on property; each outbound logged (not a group thread — individual threads).
4. CAP-10 campaign record with recipient count + template version.

### Edge paths

| Trigger | Behavior |
|---------|----------|
| Unknown sender (no matching lead/resident) | Create `lead` thread; Leasing Agent qualifies |
| Opt-out STOP on SMS | Mark party `smsOptOut`; email/portal only; log |
| Agent unsure / policy edge | Escalate to PM inbox with draft; do not auto-send |
| Delinquency formal notice (M2) | Use **attorney-approved template only**; Comms Agent sends; no freeform AI for legal notices |
| Cross-org message routing | Reject; security event CAP-10 |
| Email reply to no-reply address | Parse In-Reply-To; attach to thread |
| Owner replies to statement email | Thread linked to owner + property scope |
| Thread idle 30 days | Archive; resurface on new inbound |

---

## 5. Functional requirements

| ID | Requirement | MVP |
|----|-------------|-----|
| FR-M7-01 | `ConversationThread` per org: partyType (`lead`, `applicant`, `resident`, `owner`, `vendor`), partyId, optional entityRef (lease, unit, lead, workOrder) | ✅ |
| FR-M7-02 | `Message` model: channel (`chat`, `sms`, `email`), direction, body, mediaUrls[], senderType (`agent`, `human`, `party`), status (`sent`, `pending_approval`, `failed`) | ✅ |
| FR-M7-03 | PM admin **Unified Inbox** UI: list, filters, search, unread counts | ✅ |
| FR-M7-04 | Thread detail: full history, entity sidebar (unit, lease, WO), agent status | ✅ |
| FR-M7-05 | Inbound webhooks: Twilio SMS, inbound email (parse), portal chat WebSocket | ✅ |
| FR-M7-06 | Outbound fan-out on same thread via party `channelPreference` | ✅ |
| FR-M7-07 | Agent assignment: Leasing / Maintenance / Delinquency / Comms by thread context | ✅ |
| FR-M7-08 | Basic: outbound agent messages require PM approval (CAP-5) | ✅ |
| FR-M7-09 | Pro: auto-send within org `CommsPolicy`; governance exceptions escalate | ✅ |
| FR-M7-10 | PM **Take over** / **Release to agent** on thread | ✅ |
| FR-M7-11 | Lead → applicant → resident thread continuity (partyType migration) | ✅ |
| FR-M7-12 | Vendor messages linked to WO sub-thread (CAP-3) | ✅ |
| FR-M7-13 | Owner messages linked to property/owner record (CAP-8) | ✅ |
| FR-M7-14 | M2 delinquency + M3 renewal messages appear on resident thread | ✅ |
| FR-M7-15 | Bulk emergency broadcast to property/building residents | ✅ |
| FR-M7-16 | SMS opt-out (STOP) handling | ✅ |
| FR-M7-17 | CAP-10 trace on every agent send + PM override | ✅ |
| FR-M7-18 | Org isolation: no cross-org thread access (CAP-11) | ✅ |
| FR-M7-19 | Delivery status: sent, delivered, failed per channel | ✅ |
| FR-M7-20 | Syndication lead webhook (M1) creates thread + Lead (CAP-2) | ✅ |
| FR-M7-21 | Legal/notice templates (M2 P3): agent may only send templateId-approved bodies | ✅ |
| FR-M7-22 | Inbox SLA indicator: threads awaiting PM > configurable hours | ✅ |
| FR-M7-23 | Resident portal chat (existing CAP-7) uses same thread backend | ✅ |

**Phase 2 (not MVP):** Community announcements feed, WhatsApp, Facebook Messenger, read-receipts for owners, translation.

---

## 6. Non-functional requirements

| ID | Requirement |
|----|-------------|
| NFR-M7-01 | All threads/messages scoped by `organizationId`; RLS enforced |
| NFR-M7-02 | Inbound webhook processing < 3s p95 before agent trigger |
| NFR-M7-03 | Message body + media retained per CAP-10 retention policy (7 years recommended) |
| NFR-M7-04 | PII in messages encrypted at rest; media in Supabase Storage with org prefix |
| NFR-M7-05 | Rate limits on bulk broadcast (e.g. max 500 recipients per campaign without ops review) |
| NFR-M7-06 | Agent freeform replies blocked for categories: legal notice, adverse action, fee assessment (use templates) |

---

## 7. Data model & integrations

| Entity / Service | Purpose |
|------------------|---------|
| `ConversationThread` | organizationId, partyType, partyId, entityType, entityId, status, assignedAgent, humanOwnerId, lastMessageAt |
| `Message` | threadId, channel, direction, body, mediaUrls, senderType, senderId, templateId?, status, externalId (Twilio/SG id) |
| `PartyChannelPreference` | partyType, partyId, preferredChannel, smsOptOut, email |
| `BulkMessageCampaign` | organizationId, propertyId, templateId, recipientCount, sentAt, createdBy |
| `OrgCommsPolicy` | autoReplyEnabled, requireApprovalCategories[], bulkRequireConfirm, slaHours |
| **Twilio** | SMS inbound/outbound |
| **Email provider** (Resend/SendGrid) | Inbound parse + outbound |
| **Supabase Realtime** | Portal chat + inbox live updates |
| CAP-2 | Lead/applicant linkage |
| CAP-3 | WO + vendor threads |
| CAP-5 | Approval queues for drafts + bulk |
| CAP-8 | Owner party |
| CAP-10 | DecisionTrace ↔ thread/message |
| M1 | Lead webhook → thread |
| M2 | Template-only delinquency notices |

**API (draft):**

| Method | Endpoint | Actor |
|--------|----------|-------|
| GET | `/api/orgs/current/inbox/threads` | PM admin |
| GET | `/api/orgs/current/inbox/threads/:id` | PM admin |
| POST | `/api/orgs/current/inbox/threads/:id/messages` | PM admin |
| POST | `/api/orgs/current/inbox/threads/:id/takeover` | PM admin |
| POST | `/api/orgs/current/inbox/threads/:id/release` | PM admin |
| POST | `/api/orgs/current/inbox/bulk/emergency` | PM admin |
| POST | `/api/webhooks/twilio/sms` | Platform |
| POST | `/api/webhooks/email/inbound` | Platform |
| GET | `/api/resident/messages` | Resident (portal) |
| POST | `/api/resident/messages` | Resident (portal) |

---

## 8. Acceptance criteria

- [ ] PM admin sees leads, residents, owners, vendors in one inbox with correct filters
- [ ] Inbound SMS from unknown number creates lead thread; Leasing Agent replies on Pro
- [ ] Basic plan: agent reply stays `pending_approval` until PM sends
- [ ] Resident portal chat and SMS appear on same thread chronologically
- [ ] PM takeover pauses agent; manual PM message sends; release resumes agent
- [ ] M2 delinquency reminder uses approved templateId only — freeform agent block tested
- [ ] Bulk emergency sends to all active leases on property; each logged individually
- [ ] STOP SMS → party opted out; no further SMS until re-opt-in
- [ ] Lead thread survives conversion to resident lease without history loss
- [ ] Vendor WO reply visible on WO-linked thread view
- [ ] Org A PM cannot read Org B threads
- [ ] CAP-10 trace retrievable by thread ID within 60 seconds
- [ ] Delivery failure surfaces Error status in inbox

---

## 9. Runbook & prerequisites

| ID | Task | Blocker? | Status |
|----|------|----------|--------|
| P1 | **Attorney review:** TCPA SMS consent, opt-out, bulk messaging, email CAN-SPAM footers | **Prod gate** for SMS/bulk | ⬜ |
| P2 | Twilio account + per-org phone number strategy (shared vs dedicated) | MVP | ⬜ |
| P3 | Inbound email parse domain (`reply.rentalpro.ai` or org subdomain) | MVP | ⬜ |
| P4 | Template registry for M2/M3/M2 legal notices — wire to FR-M7-21 | MVP | ⬜ |
| B1 | ConversationThread + Message schema + RLS | — | ⬜ |
| B2 | Unified Inbox UI (PM admin) | — | ⬜ |
| B3 | Twilio webhook + outbound | — | ⬜ |
| B4 | Portal chat ↔ thread backend unification | — | ⬜ |
| B5 | Agent routing (Leasing / Maintenance / Comms) | — | ⬜ |
| B6 | CAP-5 approval for Basic drafts | — | ⬜ |
| B7 | Bulk emergency broadcast | — | ⬜ |
| B8 | M1 lead webhook → thread integration | — | ⬜ |

**Dependencies:** M1 (lead source), M2 P3 (notice templates), CAP-11 (org isolation). Can build inbox + portal chat before Twilio prod (P1 gate for SMS).

---

## 10. Locked decisions (2026-07-05)

| ID | Decision | Value |
|----|----------|-------|
| M7 scope | **A — Full MVP** | Unified inbox; expand CAP-7, no new CAP |
| Party types | lead, applicant, resident, owner, vendor | All in one hub |
| Channels | chat, SMS, email | Constraint #7; no voice |
| Agent model | Domain agents reply; Comms Agent for generic ack | Template-only for legal notices |
| Basic vs Pro | Basic approves drafts; Pro auto-send within policy | CAP-5 + OrgCommsPolicy |
| Bulk messaging | Emergency property broadcast | MVP yes; not community feed |
| Community feed | Phase 2 | SPEC non-goal |
| New CAP? | **No** — expand CAP-7 | Sub-feature M7 |

---

## Related artifacts

| Doc | Link |
|-----|------|
| CAP-7 | [`capabilities/CAP-7-resident-portal.md`](./capabilities/CAP-7-resident-portal.md) |
| M1 leads | [`SYNDICATION-MVP-RUNBOOK.md`](./SYNDICATION-MVP-RUNBOOK.md) |
| M2 notices | [`DELINQUENCY-RULES-ENGINE.md`](./DELINQUENCY-RULES-ENGINE.md) |
| M3 renewals | [`LEASE-RENEWAL-MVP-REQ.md`](./LEASE-RENEWAL-MVP-REQ.md) |
| Attorney | [`ATTORNEY-REVIEW-CHECKLIST.md`](./ATTORNEY-REVIEW-CHECKLIST.md) §9 |
| Traceability | [`REQ-TRACEABILITY.md`](./REQ-TRACEABILITY.md) |
