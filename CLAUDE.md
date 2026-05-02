# Pop-In — Development Guide

## What This App Is

Pop-In is a real-time, location-based iOS/Android app that lets people signal they're open to a spontaneous, platonic conversation in public spaces — coffee shops, coworking spaces, campus dining halls, etc. The core mechanic: set your status to "open to chat," others nearby see a count of people open, tap in to see limited identity info, and send a Match request. The person who is open accepts or quietly ignores it. No rejection, no awkwardness, no dating.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Mobile Framework | React Native + Expo (managed workflow) |
| Navigation | Expo Router |
| Backend & Database | Supabase (Postgres + Realtime + Auth + Storage) |
| Authentication | Supabase Auth (SMS OTP via phone number) |
| ID Verification | Stripe Identity |
| Location | Expo Location + Google Places API |
| Push Notifications | Expo Notifications |
| State Management | Zustand |
| Data Fetching | TanStack Query |
| Content Moderation | OpenAI Moderation API (custom vibe tags) |
| Language | TypeScript (strict mode) |

---

## Folder Structure

```
pop-in/
├── app/                    # Expo Router screens
│   ├── (auth)/             # Onboarding, verification
│   ├── (tabs)/             # Main app tabs
│   └── _layout.tsx
├── components/             # Reusable UI components
├── hooks/                  # Custom hooks (useLocation, useSession, useMatch, etc.)
├── services/               # External service integrations
│   ├── supabase.ts
│   ├── stripeIdentity.ts
│   ├── googlePlaces.ts
│   └── notifications.ts
├── stores/                 # Zustand stores
├── types/                  # TypeScript types and interfaces
├── constants/              # App-wide constants (vibe tag presets, timer options, etc.)
└── utils/                  # Helper functions
```

---

## Key Conventions

- **Absolute imports only** — configured via `tsconfig.json` (`"baseUrl": "."`) to avoid Metro bundler issues
- **TypeScript strict mode** — `strict: true` in tsconfig, no `any` types
- **Feature-based hooks** — each major feature has its own hook (e.g. `useOpenSession`, `useMatchRequest`, `useVenueDetection`)
- **Mock services in dev** — Stripe Identity and push notifications use dev mocks; swap via environment variable (`EXPO_PUBLIC_ENV=development`)
- **Environment variables** — all secrets in `.env.local`, never committed

---

## Supabase Schema (Core Tables)

```
users
  - id, phone, first_name, last_initial, gender, age_range (optional), verified_at, created_at

sessions
  - id, user_id, venue_id, status (open/matched/closed), timer_duration, started_at, ends_at

venues
  - id, google_place_id, name, lat, lng

matches
  - id, session_id, requester_id, status (pending/accepted/ignored), created_at

messages
  - id, match_id, sender_id, content, created_at (auto-deleted on session end)

met_users
  - id, user_id, met_user_id, session_id, created_at

filters
  - id, user_id, gender_filter, min_age, max_age

vibe_tags
  - id, session_id, tag, is_custom, approved
```

---

## Core Feature Flows

### Setting Open Status
1. User opens app → location detected via Expo Location
2. Google Places API resolves venue → user confirms or selects manually
3. User sets timer (30/60/90/120 min) and vibe tag
4. Session row created in Supabase with status `open`
5. Supabase Realtime broadcasts session to other users at same venue
6. Session timer runs in background; Expo Notifications alerts on match
7. Auto-close: if location leaves venue geofence, session status → `closed`

### Discovering Open People
1. User at venue → Supabase query returns count of open sessions at venue (filtered by user's filter settings)
2. User taps in → returns first name, last initial, gender, optional age range, vibe tag for each open session
3. Open sessions that don't pass the viewer's filters OR the open person's filters are excluded from both sides — mutual invisibility
4. User can silently dismiss an open person — no notification sent

### Sending a Match
1. User taps "Say hi" on an open person
2. Match row created in Supabase with status `pending`
3. If session already has an accepted match → return "chat is filled" message to requester
4. Open person receives quiet push notification
5. Open person accepts or ignores — if ignored, requester not notified
6. On accept → match status → `accepted`, Supabase Realtime opens chat channel between both users

### Post-Match Chat
1. Lightweight Supabase Realtime channel opens on match acceptance
2. Both users can optionally share description of where they are / what they look like
3. Chat messages stored temporarily in `messages` table
4. On session end (timer, manual close, or venue departure) → messages deleted, channel closed

### Passive Nudge
- **Trigger 1:** Geofence around user's frequent/saved venues — if someone is open at that venue, quiet background notification fires (opt-in only, requested after first successful match)
- **Trigger 2:** If user is already open at a venue and a new person sets themselves as open and passes mutual filters → quiet nudge

### Group Expansion
- Sessions are 1:1 only — first come first served
- If a second person sends a Match while session is filled → they receive "chat is filled"
- Existing matched pair can receive a nudge that another person at the venue is also open
- Pair can choose to invite the lone person, expanding to a group of three
- Lone person cannot see the existing matched pair to avoid FOMO

---

## Identity & Privacy Rules

| Moment | What's visible |
|---|---|
| Before tapping in | Count only ("2 people open here") |
| After tapping in | First name, last initial, gender, vibe tag, optional age range |
| After sending a Match | Requester's name, last initial, gender, optional photo |
| Person A's photo | Never shown to Person B under any circumstances |
| Age | Optional display, shown as range only (e.g. "mid 20s") never exact |

---

## Filtering Logic

Filters: gender, age range. Vibe tags are informational only — not used for filtering.

Filtering is **mutual**: if either person's filter would block the connection, neither person sees the other. This is enforced at the Supabase query level, not in the UI.

A user who doesn't pass another's filter simply doesn't appear in their count or list — no notification, no indication they exist at that venue.

---

## Age & Verification Rules

- Minimum age: 18 (hard requirement)
- Signup flow: phone number (SMS OTP via Supabase Auth) → ID check (Stripe Identity) → selfie verification
- Users cannot go active until `verified_at` is set on their user row
- Mock verification in development via `EXPO_PUBLIC_ENV=development` flag

---

## Vibe Tag Presets

```
"open to chat"
"just chilling"
"studying but open"
"working remotely"
"killing time"
"new in town"
"up for a chat"
"just eating"
```

Custom tags are sent to OpenAI Moderation API before being saved. Flagged tags are rejected and user is prompted to choose a preset or rewrite.

---

## Recommended Build Order

1. **Auth + verification** — phone OTP, Stripe Identity, user row creation
2. **Location + venue detection** — Expo Location, Google Places API, manual venue selection
3. **Open status + session timer** — session creation, Supabase Realtime broadcast, background timer, auto-close on venue departure
4. **Match mechanic** — Match request, accept/ignore, first-come-first-served logic, quiet notifications
5. **Filtering** — mutual filter logic enforced in Supabase queries
6. **Post-match chat** — Supabase Realtime channel, auto-delete on session end
7. **Passive nudge** — geofencing, opt-in permission flow, both nudge triggers
8. **People You've Met** — met_users table, lightweight follow-up messaging, quiet remove

---

## Development Notes

- Test core location and session features using **Expo Go** on a physical device
- Stripe Identity and push notifications require an **EAS build** or local native build — mock these in development
- Session notes for each build sprint live in `notes/session-notes.md`
- Monitor bundle size periodically with `npx expo export --source-map`
- Never test with real user data in development — use seeded test accounts
