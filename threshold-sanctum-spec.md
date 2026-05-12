# Threshold — Sanctum Specification
*Mixed audio + text, Spaces-inspired but differentiated*

---

## What Is Sanctum?

Sanctum is a host-led, themed conversation space inside Threshold. It combines a **live audio stage** with a **parallel text thread**, so people can participate at whatever level feels right — speaking aloud or contributing quietly in text.

Unlike Twitter/X Spaces (which is freeform and chaotic), every Sanctum session is shaped by guided prompts and closes with a mandatory reflection moment.

---

## Core Components

| Component | Description |
|---|---|
| Live audio stage | The main spoken conversation, hosted and moderated |
| Parallel text thread | A simultaneous chat for quieter participants |
| Guided prompts | Threshold injects structured prompts to pace the session |
| Reflection capture | At session close, all participants are prompted to reflect and save |

---

## Sanctum Roles

| Role | Permissions |
|---|---|
| **Host** | Opens the Sanctum, controls flow, can end the session |
| **Speaker** | On-stage audio participant |
| **Participant** | Listens to audio + contributes in the text thread |
| **Moderator** | Enforces safety — can mute, remove, or terminate the session |

---

## Sanctum User Flow

1. Discover Sanctum (directory or invite link)
2. Emotional check-in gate (required before joining)
3. Join as a listener
4. Request mic to become a Speaker, OR contribute in text thread
5. Threshold injects guided prompt moments throughout the session
6. Host closes the session
7. All participants see the Reflection screen — prompted to write and save
8. Reflection is saved to the user's personal Archive

---

## Wireframe Mockups (Low-Fidelity)

### Sanctum Discovery

```
+--------------------------------------------------+
| Sanctum                                          |
| Live themed conversations, open now              |
|--------------------------------------------------|
| "Grief & Growth" — 14 listening    [Join]        |
| "Creative Blocks" — 6 listening    [Join]        |
| "Quiet Sunday" — 22 listening      [Join]        |
|--------------------------------------------------|
| [ Host a Sanctum ]                               |
+--------------------------------------------------+
```

### Sanctum Check-In Gate

```
+--------------------------------------------------+
| Before You Enter                                 |
| How are you arriving?                            |
|                                                  |
| [ Calm ] [ Heavy ] [ Curious ] [ Numb ]          |
| [ Lonely ] [ Energized ]                         |
|                                                  |
| [ Enter Sanctum ]      [ Back ]                  |
+--------------------------------------------------+
```

### Sanctum Room — Participant View

```
+--------------------------------------------------+
| Sanctum: "Grief & Growth"           [Leave]      |
| Host: @river  |  Speakers: 3  |  Listening: 14  |
|--------------------------------------------------|
| [AUDIO STAGE]                                    |
|  🎙 @river (host)                               |
|  🎙 @maya                                       |
|  🎙 @james                                      |
|                            [ Request Mic ]       |
|--------------------------------------------------|
| TEXT THREAD                                      |
| @sam: this is resonating deeply                  |
| @lena: thank you for naming that                 |
|--------------------------------------------------|
| [Type in the thread............................]  |
| [Send]                                           |
|--------------------------------------------------|
| Prompt: "What are you ready to let go of?"       |
+--------------------------------------------------+
```

### Sanctum Reflection (Session Close)

```
+--------------------------------------------------+
| The Sanctum Has Closed                           |
| Take a moment to reflect before you go.          |
|                                                  |
| Prompt: "What stayed with you from this session?"|
|                                                  |
| [ Reflection text area........................ ] |
| [ .............................................] |
|                                                  |
| [ Save Reflection ]      [ Skip ]                |
+--------------------------------------------------+
```

---

## How Sanctum Differs from Twitter/X Spaces

| Feature | Twitter Spaces | Threshold Sanctum |
|---|---|---|
| Format | Freeform open mic | Prompt-guided, host-paced |
| Entry | Anyone can join | Emotional check-in required |
| Participation | Audio only or listen | Audio stage + parallel text thread |
| Session close | Host ends it, people leave | Mandatory reflection prompt for all |
| Continuity | No follow-up | Solo fallback if user leaves early |
| Purpose | Public discourse | Meaningful, guided exchange |

---

## Degradation Behavior

If a participant's bandwidth drops or audio fails, Sanctum automatically downgrades:

- Audio → Text-only mode (participant stays in text thread)
- No hard drop from the session
- Host is notified if a Speaker loses audio

---

## Safety Controls

### In-Session (All Users)
- Report a participant
- Block a participant
- Exit the session immediately

### Host Controls
- Mute any Speaker
- Remove a Speaker from the stage
- Promote a Participant to Speaker
- End the session for all

### Moderator Controls
- All host controls
- Remove any participant from the room
- Escalate to emergency termination
- Flag session for review

---

## Sanctum Backend Requirements

### Services Needed
- Sanctum creation + lifecycle management
- Participant role management (host / speaker / listener / moderator)
- WebRTC SFU integration for audio stage
- Text thread WebSocket stream (parallel to audio)
- Prompt injection scheduler
- Reflection collection at session end
- Safety/moderation event logging

### Data Entities
- `SanctumRoom` — room metadata, theme, host, status
- `SanctumParticipant` — user, role, join time, audio/text mode
- `ModerationReport` — reporter, target, reason, timestamp
- `AuditEvent` — all moderation actions logged for review

### API Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| POST | /sanctum | Create a new Sanctum room |
| GET | /sanctum | List available Sanctum rooms |
| POST | /sanctum/{id}/join | Join a Sanctum room |
| POST | /sanctum/{id}/request-mic | Request to speak on stage |
| POST | /sanctum/{id}/promote | Host promotes a participant to speaker |
| POST | /sanctum/{id}/mute | Host/mod mutes a speaker |
| POST | /sanctum/{id}/remove | Host/mod removes a participant |
| POST | /sanctum/{id}/end | Host ends the session |
| POST | /moderation/report | Submit a safety report |

---

## Phased Rollout

### Phase 2 — Alpha
- Sanctum creation (host-led)
- Audio stage (WebRTC SFU, managed provider)
- Parallel text thread
- Basic moderation controls (mute/remove)
- Reflection closeout screen

### Phase 3 — Maturity
- Prompt injection system (Threshold-guided pacing)
- Advanced moderation (automated flags, human review queue)
- Sanctum directory (public + invite modes)
- Session recordings or transcripts (optional, opt-in)
- Deeper analytics for hosts

---

## Open Questions

1. Sanctum discoverability: public directory, invite-only, or both?
2. Can Sanctums be recorded? If so, consent flow required.
3. How long can a Sanctum run before it auto-closes?
4. Can the same reflection prompt appear in both 1:1 and Sanctum flows?
5. Should hosts be verified/credentialed in any way before launching a Sanctum?
