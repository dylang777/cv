# Threshold MVP Spec
*For the technical cofounder — copy/paste ready*

---

## 1. Product Summary

Threshold is a guided conversation app for people who want one meaningful exchange without swiping, scrolling, or performance pressure.

---

## 2. User Problem

Users are socially fatigued and want a fast, safe path to one real conversation or one meaningful solo reflection.

---

## 3. User Types

- Lonely user seeking connection
- Socially burnt-out user
- Dating-app-fatigued user
- Transitioning user seeking grounded conversation

---

## 4. MVP Loop

Check in → choose conversation type → choose mode (solo/live) → get opening → converse/reflect → save reflection → revisit archive.

---

## 5. MVP Scope (Must-Have)

- Auth (email magic link or OAuth)
- Check-in state capture
- Conversation type selection
- Solo flow
- Live text flow (with fallback to solo)
- Reflection save
- Archive + repeat-use home

---

## 6. Not MVP-Center

- Full ecosystem/IRL layers
- Heavy symbolic worldbuilding UI
- Full audio/video complexity
- Multi-room complexity outside core loop

---

## User Flow Map

**Path A — Solo**
Home → Check In → Conversation Type → Mode: Solo → Opening Prompt → Solo Reflection Room → Save Reflection → Archive

**Path B — Live Text**
Home → Check In → Conversation Type → Mode: Live → Opening → Match/Waiting → Shared Text Room → Save Reflection → Archive

**Path C — No Match Fallback**
Home → Check In → Conversation Type → Mode: Live → Opening → No Match → [Start Solo Now] or [Try Again]

---

## Wireframe Mockups (Low-Fidelity)

### 1. Home

```
+--------------------------------------------------+
| THRESHOLD                                        |
| One meaningful conversation.                     |
|                                                  |
| [ Start ]                                        |
| [ View Archive ]                                 |
|                                                  |
| Why Threshold | Safety | Account                 |
+--------------------------------------------------+
```

### 2. Check In

```
+--------------------------------------------------+
| Check In                                         |
| How are you arriving today?                      |
|                                                  |
| [ Calm ] [ Heavy ] [ Curious ] [ Numb ]          |
| [ Lonely ] [ Energized ]                         |
|                                                  |
| [ Continue ]           [ Back ]                  |
+--------------------------------------------------+
```

### 3. Conversation Type

```
+--------------------------------------------------+
| Choose Conversation Type                         |
|                                                  |
| ( ) Emotional support                            |
| ( ) Deep life reflection                         |
| ( ) Creative/intellectual                        |
| ( ) Faith/spiritual                              |
|                                                  |
| [ Continue ]           [ Back ]                  |
+--------------------------------------------------+
```

### 4. Mode Selection

```
+--------------------------------------------------+
| Choose Mode                                      |
|                                                  |
| [ Solo ]     Guided self-reflection              |
| [ Live ]     Real-time human conversation        |
|                                                  |
| [ Continue ]           [ Back ]                  |
+--------------------------------------------------+
```

### 5. Opening

```
+--------------------------------------------------+
| Your Opening                                     |
| "What is something true you haven't said lately?"|
|                                                  |
| [ Start Conversation ]                           |
| [ Try Different Opening ]                        |
|                                                  |
| Mode: Live/Text | Type: Emotional support        |
+--------------------------------------------------+
```

### 6. Live Waiting / Fallback

```
+--------------------------------------------------+
| Finding a Match...                               |
|                                                  |
| Searching for someone in your chosen mode        |
|                                                  |
| [ Keep Waiting ]                                 |
| [ Switch to Solo Now ]                           |
| [ Try Again ]                                    |
+--------------------------------------------------+
```

### 7. Live Text Room

```
+--------------------------------------------------+
| Live Room #A91                                   |
| Timer: 12:44                                     |
|--------------------------------------------------|
| You: ...                                         |
| Other: ...                                       |
|--------------------------------------------------|
| [Type message.................................]  |
| [Send]   [End Session]                           |
| Safety: [Report] [Block] [Exit]                  |
+--------------------------------------------------+
```

### 8. Solo Reflection Room

```
+--------------------------------------------------+
| Solo Reflection                                  |
| Prompt: "What are you avoiding naming?"          |
|                                                  |
| [ Reflection text area........................ ] |
| [ .............................................] |
|                                                  |
| [ Save Reflection ]      [ New Prompt ]          |
+--------------------------------------------------+
```

### 9. Reflection Save

```
+--------------------------------------------------+
| Save Reflection                                  |
| Title: [ Today I admitted... ]                   |
| Tags: [anxiety] [clarity] [+]                    |
| Privacy: (x) Private  ( ) Share later            |
|                                                  |
| [ Save ]                 [ Discard ]             |
+--------------------------------------------------+
```

### 10. Archive

```
+--------------------------------------------------+
| Archive                                          |
|--------------------------------------------------|
| May 10 - "I felt seen"       [Open]              |
| May 08 - "Hard conversation" [Open]              |
| May 05 - "Solo clarity"      [Open]              |
|--------------------------------------------------|
| [ Start New Conversation ]                       |
+--------------------------------------------------+
```

---

## Product Logic Directives

- If user selects Live and match exists → live room
- If user selects Live and no match after timeout → solo fallback options
- If session ends → reflection save screen
- If reflection saved → archive entry created
- If returning user exists → home shows "Start New Conversation" + recent archive
- Sanctum join requires active account + safety eligibility checks
- Sanctum can degrade from audio to text-only if bandwidth fails

---

## Backend Requirements

### Core Services

- Auth service (session + identity)
- Profile/state service (arrival state, preferences)
- Matching service (live text pairing)
- Realtime service (WebSocket for rooms + Sanctum)
- Session service (conversation lifecycle)
- Reflection/archive service
- Moderation/safety service
- Notification service
- Analytics/event pipeline

### Data Model — Minimum Entities

| Entity | Purpose |
|---|---|
| User | Identity and account |
| UserSession | Active login session |
| CheckIn | Arrival state capture |
| ConversationPreference | Type + mode selection |
| MatchQueue | Live matching queue |
| ConversationSession | Lifecycle of a conversation |
| Room | Text room instance |
| Message | Individual message in a room |
| Reflection | User's written reflection |
| ArchiveItem | Saved past session reference |
| SanctumRoom | Host-led audio/text space |
| SanctumParticipant | Participant role record |
| ModerationReport | In-session safety reports |
| BlockList | User block records |
| AuditEvent | Logged moderation/safety events |

### Realtime Requirements

- Presence (online/in-room)
- Matchmaking queue updates
- Typing/message events
- Session timer/state sync
- Sanctum stage state (host/speaker/listener)
- Text chat stream for Sanctum

### Safety / Moderation Requirements

- Report/block in-session
- Host/moderator remove/mute controls (Sanctum)
- Rate limits + abuse detection
- Content/event logging for moderation review
- Emergency session termination paths

### API Surface (High-Level)

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /auth/* | Auth flows |
| POST | /checkins | Submit check-in state |
| POST | /match/enqueue | Enter match queue |
| POST | /match/dequeue | Leave match queue |
| POST | /sessions | Create session |
| POST | /sessions/{id}/end | End session |
| POST | /messages | Send message |
| POST | /reflections | Save reflection |
| GET | /archive | Retrieve archive |
| POST | /sanctum | Create Sanctum room |
| POST | /sanctum/{id}/join | Join Sanctum |
| POST | /sanctum/{id}/request-mic | Request to speak |
| POST | /moderation/report | Submit safety report |

---

## Technical Architecture Assumptions

| Layer | Technology |
|---|---|
| Frontend | Next.js (existing demo evolves) |
| Backend | Node/TypeScript (REST + WebSocket) |
| Database | Postgres |
| Cache/Queue | Redis (match queue + presence) |
| Realtime | WebSocket gateway or managed provider |
| Media (Sanctum) | WebRTC SFU provider (managed first) |
| Storage | Object storage for artifacts |
| Observability | Logs + metrics + error tracking |

---

## Phased Roadmap

### Phase 1 — Now
- Solidify product definition + wireframes
- Build backend foundation (auth, sessions, reflections, archive)
- Ship solo flow end-to-end
- Ship live text prototype with fallback

### Phase 2 — Next
- Improve matching quality + repeat-use home
- Safety tooling expansion
- Sanctum alpha (mixed audio+text, host-led)

### Phase 3+ — Later
- Sanctum maturity (moderation depth, quality controls)
- Advanced rooms/chambers/portals
- Deeper audio/video support
- IRL ecosystem integrations

---

## Open Questions for Final Build Spec

1. Match timeout threshold before fallback (e.g., 30s / 60s / 90s)?
2. Anonymous by default, or lightweight profiles visible in live mode?
3. Sanctum discoverability: public directory, invite-only, or both?
4. Reflection privacy defaults and export/delete requirements?
5. Moderation policy: automated actions vs. human review SLA?
