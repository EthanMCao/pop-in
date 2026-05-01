# Nearish
### Meet People. Right Now. Right Here.
*App Concept Document — v0.6*

---

## Overview

Nearish is a real-time, location-based app that lets people signal they're open to a conversation in person — without the awkwardness of approaching a stranger cold. The core mechanic is simple: you set your status to "open to chat" at your current venue, and anyone else nearby with the app can see that someone is approachable. No cold approaching, no awkward interruptions, no guessing whether someone wants to be bothered.

Nearish is strictly platonic. It is not a dating app. The goal is low-stakes small talk and human connection — a stranger to grab coffee with, someone to share a table with, a quick conversation that makes the afternoon less solitary.

---

## The Problem

People are increasingly isolated in public spaces. Someone sitting alone at a coffee shop might be completely open to conversation — but there's a massive social barrier to approaching a stranger. The fear of rejection, awkwardness, or misreading signals stops most people before they even try.

Existing solutions either require pre-planning (Timeleft, Meetup) or are built around dating (Tinder, Bumble). There is no app built for spontaneous, platonic, in-person connection right now, right where you are.

---

## The Solution

Nearish works in three steps:

1. You arrive somewhere — a coffee shop, coworking space, campus dining hall, park, or any public venue.
2. You open Nearish, enable location, and set your status to **"Open to chat."** The app detects your venue and adds you to that location's active session. You set how long you want to be open (30, 60, 90, 120 min, etc.) and choose a vibe tag for the session. The app runs quietly in the background.
3. Anyone else at the same venue with the app sees how many people are open there (e.g. "2 people open here"). They can tap in to see a first name, gender, approximate age, and vibe tag for each person, then send a Match — a soft signal of interest. The person who is open gets a quiet notification and can accept or quietly ignore it.

The key insight: you already know someone wants to talk *before* you walk over. That changes everything. The fear of rejection is gone because the person in front of you already said yes.

---

## Core Features

| Feature | Description |
|---|---|
| **Open Status** | Set yourself as "open to chat" at your current venue. Others nearby see a count of how many people are open — not who specifically — until they tap in. |
| **Location Detection** | App uses location services to verify you're physically at the venue and determine which location you're at. If it's uncertain, you can manually select from nearby options. You join that venue's active session and are only visible to others checked into the same place. If the app detects you've left the venue, your session ends automatically. |
| **Session Timer** | You choose how long you want to be open — 30, 60, 90, 120 min, etc. The app runs in the background and alerts you if someone sends you a Match request. You can end your session early at any time. |
| **Vibe Tags** | Set per session, not on your profile. Choose from a preset list or write your own to describe what you're doing and what kind of interaction you're open to — e.g. "open to chat," "just chilling," "studying but open," "killing time," "new in town," "working remotely." Visible to others before they decide to send a Match so they know what they're getting into. Custom tags are subject to moderation. |
| **Match** | When someone taps in and wants to connect, they hit **"Say hi"** to send a Match request. The person who is open gets a quiet notification and can accept or quietly ignore it. If ignored, the other person is not notified. |
| **Filtering** | Filter who can see your open status by gender and age range. Filtering is mutual — if a filter blocks a connection, neither person sees the other as available. No rejection, no notification — they simply don't exist to each other. If a third person fits both filters independently, they can still reach either of them separately. If Person B sees Person A's open status but isn't interested, they can silently dismiss — Person A never knows they were seen or passed over. |
| **Passive Nudge** | Entirely optional and off by default. Two triggers once enabled: (1) If you're near a venue where someone is open to chat, you get a quiet background notification even if you're not actively using the app. Uses geofencing around frequent or saved venues rather than continuous location tracking. The ask is timed after a user's first successful match, when the value is already clear. (2) If you're already open at a venue and someone else there also sets themselves as open and passes your filters, you get a quiet nudge. |
| **Safety Controls** | Block, report, and end session instantly. Status disappears when your timer expires, you manually end the session, or the app detects you've left the venue. |

---

## Identity & Privacy

Nearish intentionally keeps identity minimal. This is not a dating app — you don't need to know much about someone before deciding to have a conversation with them. The tiered reveal below is designed to give just enough context to feel comfortable, nothing more.

- **Before tapping in:** Person B sees only a count — "2 people open here." No names, no identities.
- **After tapping in:** Person B sees each open person's first name, last initial, gender, vibe tag, and optionally an approximate age range (e.g. "mid 20s"). Age display is entirely optional — users choose whether to show it and it's never shown as an exact number.
- **After a Match is sent:** Person A sees Person B's name, last initial, gender, and optional photo. Person B can choose whether to include a photo — it gives Person A more context when deciding to accept.
- **Person A's photo:** Never shown to Person B. Person A is the more exposed party — they've made themselves discoverable — and a photo would make them physically locatable by anyone at the venue. The post-match chat handles finding each other instead.

---

## Connections & Messaging

Nearish is built around real interactions, not digital ones. There are no public profiles, no follower counts, no way to search for or add people you know. You cannot find someone on Nearish unless you've physically met them through the app.

The only persistent social layer is a **"People You've Met"** section — a private list of everyone you've connected with in person through Nearish. From this list you can send a lightweight follow-up message if you forgot to ask something or want to say it was good to meet them. If an interaction went badly, you can quietly remove someone — since there's no search functionality, removing them effectively erases any record of them from your experience entirely.

The long-term vision is for Nearish to evolve into a broader platform centered entirely on real-world interaction — but that is a future consideration, not something being built now.

---

## Target Audience

Nearish is designed for anyone who finds themselves alone in a public space and is open to human connection. Primary early adopters are likely:

- College students eating alone on campus or at nearby spots
- Young professionals working remotely from cafés
- People new to a city trying to build a social circle
- Travelers looking for a local to talk to
- Introverts who want to meet people but struggle with cold approaches

---

## What It Is Not

- **Not a dating app** — no swiping, no romantic framing, no sexual or relationship context whatsoever
- **Not a planning tool** — everything happens in the moment
- **Not a social network** — no feeds, no followers, no searching for people you know
- **Not location tracking** — location is only active during a session; passive nudge uses optional geofencing, not continuous tracking
- **Not a messaging platform** — post-match messaging exists only as a lightweight follow-up, not a chat app

---

## Why This Is Different

| Feature | Nearish | Timeleft | Tinder | Meetup |
|---|:---:|:---:|:---:|:---:|
| No pre-planning needed | ✓ | ✗ | ✓ | ✗ |
| Rejection-free mechanic | ✓ | ✗ | ✓ | ✗ |
| Platonic / non-dating focus | ✓ | ✓ | ✗ | ✓ |
| Purely spontaneous / real-time | ✓ | ✗ | ✓ | ✗ |
| Filter-based inclusivity | ✓ | ✗ | ✗ | ✗ |

---

## Cold Start Strategy (Early Thinking)

The app only works if people are using it at the same venues. The plan is to launch hyper-locally — a single college campus — and seed it manually. The first real interactions get documented and turned into short-form content (TikTok, Reels) to grow organically from there. Concentrated early density beats broad thin coverage every time.

---

## After a Match is Accepted

Once a Match is accepted, the app steps back. A lightweight chat opens between the two people. Each person can optionally share a short description of where they are or what they look like (e.g. "by the window, grey hoodie") — but nothing is required. The chat auto-deletes when the session ends. The goal is to get both people off their phones and into a real conversation as quickly as possible.

---

## Age & Safety Rules

- **Minimum age:** 18. Hard requirement. The app is for adults only.
- **Age verification:** Phone number verification is required on signup. A lightweight ID check (via a third-party service like Stripe Identity or Veriff) confirms users are 18+ before they can go active. Selfie verification matches the user to their ID. These are consistent with the direction dating apps are moving under increasing legal pressure and are worth the onboarding friction to keep bad actors out.
- **Catfishing:** Because Nearish is built around in-person meeting, identity fraud is naturally self-correcting — you can't sustain a fake persona when you're sitting across from someone within minutes. Community reporting handles edge cases.
- **Notifications:** Match alerts use a standard quiet notification — no sounds or interruptions that would draw attention in a public space.

---

## Match Capacity & Group Expansion

Matches are first come first served — only one person can match with Person A at a time. If a second person sends a Match while Person A is already matched, they receive a quiet message that the chat is filled. Person A's session is no longer visible to others at that point.

From there, two things can happen naturally:

- The person who was denied will typically set themselves as open to chat. The existing pair can get a quiet nudge that someone else at the venue is also open, and can choose to invite them into the conversation — expanding to a group of three organically.
- The denied person, now open themselves, remains invisible to the existing pair's session to avoid FOMO. They exist as a separate open status that others can discover independently.

This creates a natural, unforced path toward small group interactions without ever requiring anyone to plan for it. It's a future-facing mechanic that works within the 1:1 launch framework.

---

## Empty State

When someone opens the app at a venue with no one currently open, they see a simple message along the lines of "nobody here yet" alongside a prompt encouraging them to set themselves as open to chat. The count display ("X people open here") only appears when someone is actually active. This is part of why launching in a concentrated area like a single college campus makes sense — density is everything early on, and a thin experience in a small area beats a nonexistent experience spread too wide.

---

## Open Questions / Things to Decide

- **Safety design:** Needs its own dedicated pass — especially around how the filtering system, session controls, and identity reveal work together to protect vulnerable users.

---

## Technology Stack

Nearish is being built solo using a vibe-coding approach. The stack is chosen to be lean, well-documented, and AI-coding-friendly — covering everything the app needs without unnecessary complexity.

**Platform**
- iOS first, with Android supported simultaneously via a shared codebase

**Mobile Framework**
- **React Native + Expo** — one codebase for both iOS and Android. Expo's managed workflow removes most painful native setup and is one of the most vibe-coding-friendly mobile stacks available.

**Backend & Database**
- **Supabase** — covers the majority of backend needs in one managed service: Postgres database, real-time subscriptions for live session and match updates, phone number authentication, and file storage for photos. Minimal infrastructure to set up or maintain.

**Authentication & Verification**
- **Supabase Auth** — phone number sign-in via SMS OTP
- **Stripe Identity** — ID verification to confirm users are 18+ before they can go active

**Location & Venue Detection**
- **Expo Location** — GPS-based location detection and geofencing for the passive nudge feature
- **Google Places API** — venue identification; if the app is uncertain of your location, Places helps resolve it and allows manual selection between nearby options

**Real-Time & Chat**
- **Supabase Realtime** — powers live session updates, match notifications, and the lightweight post-match chat. Chat messages are stored temporarily and deleted when the session ends.

**Push Notifications**
- **Expo Notifications** — quiet background alerts for match requests and passive nudges

**Photo Storage**
- **Supabase Storage** — stores optional photos that Person B can attach when sending a Match request

**Content Moderation**
- **OpenAI Moderation API** — scans custom vibe tags before they go live to catch inappropriate content

---

## Status

This is v0.6 of the concept document. Nothing has been built yet. The purpose of this document is to align on the core idea before any design or development begins.

---

*Nearish — Concept Document v0.7*
