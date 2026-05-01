# PopIn — Build Order

Each phase should be fully working and tested on a physical device before moving to the next. Phases 0–5 are the MVP. Everything after is layering on top of a working foundation.

---

## Phase 0 — Project Scaffolding

Do this all in one go. It's just boilerplate.

**What to build:**
- Initialize Expo project with React Native + TypeScript strict mode
- Configure Expo Router for navigation
- Set up absolute imports in `tsconfig.json`
- Create the full folder structure per `CLAUDE.md`
- Set up Supabase client in `services/supabase.ts` using environment variables
- Configure `.env.local` for all secrets

**Test:** Project runs on Expo Go with no errors.

---

## Phase 1 — Auth + Verification

This is the gate to everything else. Nothing works until a verified user exists.

**What to build:**
- Phone number input screen
- SMS OTP flow via Supabase Auth
- Create user row in `users` table on first login
- Stripe Identity ID check (mock in dev via `EXPO_PUBLIC_ENV=development`)
- Selfie verification screen
- Block access to the main app until `verified_at` is set on the user row

**Test:** Create an account on a physical device end to end. Confirm user row appears in Supabase with correct fields.

---

## Phase 2 — Location + Venue Detection

**What to build:**
- Request location permissions via Expo Location
- Detect current venue using GPS + Google Places API
- Venue confirmation screen ("Are you at X?")
- Manual venue selection if the app is uncertain
- Create venue row in Supabase `venues` table if it doesn't already exist

**Test:** Sit somewhere, open the app, confirm it correctly identifies the venue. Test manual selection fallback.

---

## Phase 3 — Open Status + Session Timer

This is the core mechanic of the app.

**What to build:**
- Vibe tag selection screen — presets from `constants/` plus custom input
- Custom tag moderation via OpenAI Moderation API before saving
- Timer selection UI (30 / 60 / 90 / 120 min)
- Create session row in Supabase `sessions` table with status `open`
- Supabase Realtime broadcasts session to others at the same venue
- Background timer that auto-closes session on expiry
- Auto-close session if Expo Location detects user has left the venue geofence
- Manual "End session" button

**Test:** Set open status on a physical device. Confirm session row appears in Supabase with correct venue, timer, and status. Confirm session closes automatically on expiry.

---

## Phase 4 — Discovering Open People

**What to build:**
- Venue screen showing count of open people ("2 people open here")
- Tap-in view showing each open person: first name, last initial, gender, vibe tag, optional age range
- Silent dismiss — Person A is never notified they were seen or dismissed
- Empty state: "nobody here yet — be the first to set your status"

**Test:** Use two phones or two test accounts at the same venue. Confirm one appears in the other's list. Confirm dismiss sends no notification.

---

## Phase 5 — Match Mechanic

**What to build:**
- "Say hi" button on each open person's card
- Create match row in Supabase `matches` table with status `pending`
- First come first served — if session already has an accepted match, return "chat is filled" to the requester
- Push notification to Person A via Expo Notifications (mock in dev)
- Accept / ignore UI for Person A
- On ignore: requester is not notified
- On accept: match status → `accepted`, Supabase Realtime opens connection between both users

**Test:** Two physical devices. Send a match request, accept it, confirm both sides update. Send a second request while matched, confirm "chat is filled" message appears.

---

## Phase 6 — Filtering

**What to build:**
- Filter settings screen (gender filter, age range filter)
- Save filter preferences to `filters` table in Supabase
- Enforce mutual filter logic at the Supabase query level — not in the UI
- If either person's filter blocks the connection, neither sees the other in counts or lists

**Test:** Set a gender filter on one account. Confirm that account disappears from the other's count entirely. Confirm no notification is sent to either party.

---

## Phase 7 — Post-Match Chat

**What to build:**
- Open lightweight Supabase Realtime channel on match acceptance
- Chat UI: text input and message list
- Both sides can optionally share a description of where they are or what they look like
- Auto-delete all messages from `messages` table when session ends (timer, manual close, or venue departure)

**Test:** Accept a match on two devices. Send messages. End the session. Confirm all messages are deleted and the channel is closed.

---

## Phase 8 — Passive Nudge

**What to build:**
- Geofence setup around frequent/saved venues via Expo Location
- Opt-in permission request — ask after first successful match, not on signup
- Trigger 1: quiet background notification when someone is open at a nearby geofenced venue
- Trigger 2: quiet nudge when you're already open and someone else at your venue also goes open and passes your filters

**Test:** Requires a physical device and real background location permissions. Cannot be fully tested in simulator.

---

## Phase 9 — People You've Met

**What to build:**
- Write to `met_users` table when a matched session ends
- "People You've Met" screen — private list of past in-person connections
- Lightweight follow-up messaging from that list
- Quiet remove — no confirmation dialog, no notification to the other person. Since there's no search, removing someone erases them from the experience entirely.

**Test:** Complete a match session. Confirm both users appear in each other's People You've Met list. Test remove and confirm the person is gone with no trace.

---

## Phase 10 — Group Expansion

**What to build:**
- When a second person tries to match a session that's already filled → "chat is filled" message
- Nudge to existing matched pair that another person at the venue is open
- Invite mechanic allowing the pair to bring in the lone person
- Lone person's open status is invisible to the matched pair — they cannot see each other to prevent FOMO

**Test:** Three devices. Match two, have third attempt to match. Confirm "chat is filled." Confirm matched pair gets nudge. Confirm lone person cannot see the pair.

---

## Phase 11 — UI Polish

Only touch this after all above phases are working. Do not polish as you go.

**What to refine:**
- Consistent design language across all screens
- Loading states and error handling on every async action
- Empty states for all lists and venue views
- Smooth screen transitions
- App icon and splash screen
- Bundle size check: `npx expo export --source-map`

---

## Notes

- Never test with real user data in development — use seeded test accounts
- Stripe Identity and Expo Notifications require an EAS build or local native build — mock in development
- Commit to git at the end of every phase so you have clean checkpoints to roll back to
- Keep `notes/session-notes.md` updated with what was built and what's next at the end of each session
