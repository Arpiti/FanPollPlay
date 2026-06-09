# FanPollPlay — PRD v1.5
**Product Requirements Document**
*June 2026 | World Cup 2026 Window*

---

## Overview

FanPollPlay is a web-based group prediction pool manager for Indian social groups. An admin creates a pool, shares one WhatsApp link, and the platform handles pick collection, auto-scoring, and live leaderboards.

**v1.5 scope builds on v1 (core pool flow) by adding:**
- Exact Score predictions (activated from match 5 onwards)
- Pre-match context panel on the leaderboard page
- Post-match stats panel on the leaderboard page
- Automated data pipeline (no Haiku — all templated)

---

## Goals

| Goal | Metric | Target |
|---|---|---|
| Organiser activation | Pools created per day | 10+ by match 5 |
| Participant engagement | Picks submitted per pool | >80% of joined members |
| Retention | Return visits per participant per match day | 2+ (pre + post) |
| Revenue | Paying organisers (₹199) | 50+ by end of group stage |

---

## User Personas

### Organiser
- Sets up the pool, shares link, monitors who's submitted
- Pays ₹199 if group exceeds 15 people
- Wants zero management overhead after setup

### Participant
- Joins via link, submits picks in <90 seconds
- Checks leaderboard post-match to see rank
- Mobile-first, Android, 6.5" screen

### Lurker
- Opens the leaderboard link to see standings
- Does not submit picks
- Distribution agent — shares the page if it's interesting

---

## Feature Scope

### Carried from v1 (must be stable before v1.5 ships)

- [ ] Admin creates pool with event name, description, access type (link-only for v1)
- [ ] Shareable join link generated on creation
- [ ] Participant: name + passcode → joins pool (no phone, no OTP)
- [ ] Match Result pick submission (Win/Draw/Loss per match)
- [ ] Admin dashboard: who has/hasn't submitted, match schedule
- [ ] Manual scoring trigger (admin presses "Score this match")
- [ ] Auto-scoring cron (runs post-match, manual trigger as fallback)
- [ ] Leaderboard: rank, name, points, visible to all including lurkers
- [ ] 15-participant free tier cap with upgrade prompt
- [ ] Payments: UPI QR manually for first 20 pools, Razorpay thereafter
- [ ] SMS notifications via MSG91 — admin only (pool created, upgrade prompt, scoring complete)

### New in v1.5

#### Feature 1 — Exact Score Predictions
- Unlocks for all participants from match 5 onwards (configurable by admin)
- Participant submits: Match Result (required) + Exact Score (optional)
- Scoring: 1pt correct result, 3pt correct exact score (exact score also implies correct result — award 3pt total, not 4)
- UI: Exact Score input appears below the Match Result selector, labelled "Bonus pick — 3 pts"
- If participant skips Exact Score, only Match Result is scored

#### Feature 2 — Pre-Match Context Panel
Visible on the leaderboard page for each upcoming match (from 24h before kickoff until kickoff).

**Content (all templated, no AI):**

```
[MATCH HEADER]
France vs Morocco | Group C | Jun 14 · 3:30pm IST

[TEAM FORM — last 5]
France:   W W W D W
Morocco:  W D W W L

[HEAD-TO-HEAD]
Last 10 meetings: France 4 · Draw 3 · Morocco 3
Last meeting: Qatar 2022 QF — France 2–0 Morocco

[GROUP STANDINGS]
1. France      6pts  +4
2. Morocco     4pts  +1
3. [Team C]    ...

[YOUR POOL]
68% picked France · 20% Morocco · 12% Draw
```

#### Feature 3 — Post-Match Stats Panel
Replaces Pre-Match panel after the final whistle. Visible permanently after match ends.

**Content (all templated):**

```
[RESULT]
France 2 – 1 Morocco
Scorers: Mbappe 34', Griezmann 67' | Ziyech 80'

[KEY STATS]
Possession:    France 58% · Morocco 42%
Shots on tgt:  France 7  · Morocco 3
xG:            France 2.1 · Morocco 0.8

[POOL IMPACT]
✓ 12 members got the result right (+1pt each)
★ 2 members got the exact score (+3pt each)
Biggest mover: Rahul jumped from 4th → 2nd

[LEADERBOARD — updated]
(live table below)
```

---

## Technical Architecture

### Stack

| Layer | Choice | Reason |
|---|---|---|
| Frontend | Next.js 14 (App Router) | SSR, fast, SEO |
| Hosting | GCP Cloud Run | Containerised Next.js, auto-scales to zero |
| Database | Firebase Firestore | Free tier generous, real-time updates, no schema migrations |
| Auth | Firebase Auth (Phone OTP) — admin only | Participants auth via passcode + localStorage token, no SMS |
| Cron jobs | GCP Cloud Scheduler | 3 free jobs/month covers both cron needs |
| Background jobs | GCP Cloud Functions (2nd gen) | Post-match scoring triggered by Scheduler |
| Payments | UPI QR (manual) → Razorpay | Validate intent before building infra |
| Notifications | MSG91 SMS | ₹0.18/SMS, live in 30 min |
| Match data | football-data.org (free) | Fixtures, results, standings, H2H |
| Match stats | API-Football via RapidAPI (free) | xG, possession, shots — 100 req/day free |

### GCP Free Tier Limits (relevant)

| Service | Free Tier | Expected Usage |
|---|---|---|
| Cloud Run | 2M requests/month, 360k vCPU-sec | Well within limits at launch scale |
| Cloud Functions | 2M invocations/month | 4 matches/day × 39 days = 156 invocations |
| Cloud Scheduler | 3 jobs/month free | Need 2 jobs — free |
| Firestore | 50k reads/day, 20k writes/day, 1 GiB storage | Sufficient for <500 pools |
| Firebase Auth | 10k SMS/month free | Admin OTP only — no longer a scaling concern |

### Firestore Collections

```
pools/
  {poolId}/
    name, adminPhone, eventId, passcode, participantCount, isPaid, createdAt
    // passcode: 6-char alphanumeric, generated on pool creation, shown to admin only

participants/
  {poolId}/
    {participantId}/
      name, token, joinedAt
      // token: UUID generated server-side on first join, stored in participant's localStorage
      // no phone number — identified by token on return visits

picks/
  {poolId}/
    {matchId}/
      {participantId}/
        result: 'HOME' | 'DRAW' | 'AWAY'
        exactScore: { home: int, away: int } | null
        submittedAt

scores/
  {poolId}/
    {participantId}/
      totalPoints, breakdown: [{ matchId, points, type }]

match_content/
  {matchId}/
    pre: { teamForm, h2h, standings, fetchedAt }
    post: { result, stats, fetchedAt }
    status: 'PRE' | 'LIVE' | 'POST'
```

---

## Data Pipeline

### Cron 1 — Daily Fixtures Sync
**Schedule:** 6:00am IST (00:30 UTC) daily via Cloud Scheduler
**Trigger:** Cloud Function `dailyFixturesSync`

```
1. Fetch today's fixtures from football-data.org
   GET /competitions/2000/matches?dateFrom={today}&dateTo={today}

2. For each match:
   a. Fetch team form (last 5):
      GET /teams/{homeTeamId}/matches?status=FINISHED&limit=5
      GET /teams/{awayTeamId}/matches?status=FINISHED&limit=5
      → 7-second delay between calls to respect 10 req/min limit

   b. Fetch H2H:
      GET /matches?homeTeam={id1}&awayTeam={id2}&limit=10

   c. Fetch standings:
      GET /competitions/2000/standings
      → Cache for the day — only fetch once regardless of match count

3. Write to Firestore: match_content/{matchId}/pre
4. Set match status = 'PRE'
```

API calls per day: ~1 (fixtures) + 2 (team form × 2) × 4 matches + 4 (H2H) + 1 (standings) = **14 calls/day** — well within 10 req/min limit with 7s delays.

### Cron 2 — Post-Match Results
**Schedule:** Every 15 minutes, 14:30–23:59 UTC (8pm–5:30am IST) via Cloud Scheduler
**Trigger:** Cloud Function `postMatchResultsSync`

```
1. Query Firestore for matches where:
   status = 'PRE' AND kickoffTime < now - 110 minutes

2. For each match:
   a. Fetch result:
      GET /matches/{matchId}
      If status = 'FINISHED':

   b. Fetch stats from API-Football:
      GET /fixtures?id={apiFootballFixtureId}
      Extract: possession, shots_on_target, xG per team, goal scorers

   c. Write to Firestore: match_content/{matchId}/post
   d. Set match status = 'POST'

3. Trigger scoring job for all pools containing this match
```

API-Football calls: 1 per finished match × max 4 matches/day = **4 calls/day** (100/day free limit — very safe).

### Scoring Job (Cloud Function — event-driven)
Triggered by Firestore write to `match_content/{matchId}/post`:

```
1. Fetch all pools containing this matchId
2. For each pool:
   a. Fetch all participant picks for this match
   b. Score each pick:
      - result correct: +1pt
      - exact score correct: +3pt (supersedes result point)
   c. Update scores/{poolId}/{participantId}
3. Update leaderboard rankings
```

---

## User Flows

### Flow 1 — Organiser Creates Pool
```
Landing page → "Create Pool" CTA
→ Enter: Pool name, your name, phone number
→ OTP verification (admin only)
→ Select: Tournament (World Cup 2026 — only option in v1)
→ Select: Competition types (Match Result enabled by default; Exact Score shown as "unlocks at match 5")
→ Set participant limit visibility (shown clearly: "Free plan: up to 15 participants")
→ Pool created
→ Admin sees: shareable link + 6-char passcode
→ Admin shares on WhatsApp: "Join our World Cup pool 👉 [link] | Code: ABC123"
→ Admin dashboard
```

### Flow 2 — Participant Joins and Submits Pick
```
Opens WhatsApp link
→ Pool page: pool name, upcoming match, current leaderboard (if picks exist)
→ "Join & Pick" CTA
→ Enter: your name + passcode (2 fields, no OTP, no SMS)
→ Server validates passcode → creates participant record → returns token → stored in localStorage
→ Pick submission: Match Result selector (3 chunky buttons: Home / Draw / Away)
  [if v1.5 and match >= 5]: Exact Score input below
→ Submit → confirmation screen: "Pick locked in ✓"
→ Returns to leaderboard/pool home

Returning visit:
→ Opens pool link → localStorage token found → auto-identified → straight to picks/leaderboard
→ If token missing (cleared storage): re-enter name + passcode → new token issued
```

### Flow 3 — Viewing Pre-Match Context
```
Participant / lurker opens pool link before kickoff
→ Leaderboard visible
→ Below leaderboard: upcoming match panel
→ Shows: team form, H2H, standings snippet, pool pick distribution
→ No login required to view (lurker-accessible)
```

### Flow 4 — Post-Match Result + Leaderboard Update
```
Cloud Function fires ~110 min after kickoff
→ Scores all picks
→ Leaderboard updated in Firestore (real-time listener updates UI instantly)
→ SMS sent to admin only via MSG91: "France 2–1 Morocco | Scoring complete | 12/15 got result right"
→ Admin posts result update in WhatsApp group (their existing habit — not replaced)
→ Post-match panel visible on pool page with stats + pool impact
```

### Flow 5 — Upgrade Trigger
```
16th person tries to join a free pool
→ Passcode is valid but pool is at capacity
→ Participant sees: "This pool is full — ask [Admin Name] to upgrade"
→ Admin receives SMS: "16 people want to join your pool. Unlock unlimited for ₹199 → [link]"
→ Admin pays (UPI QR initially, Razorpay later)
→ Pool unlocked, 16th participant enters name + passcode and joins
```

---

## Non-Functional Requirements

| Requirement | Target |
|---|---|
| Page load (leaderboard) | <2s on 4G |
| Pick submission flow | <90 seconds end-to-end |
| Leaderboard update after scoring | <30 seconds from cron completion |
| Uptime during match days | 99.5% (Cloud Run auto-scales) |
| Mobile support | Android Chrome, iOS Safari — no native app |
| SMS delivery | >95% delivery rate via MSG91 |

---

## Out of Scope (v1.5)

| Item | Status |
|---|---|
| WhatsApp Business API notifications | v2 |
| Knockout Bracket / Golden Boot | v2 |
| Phone whitelist (participant phone collection) | v2 |
| Claude Haiku match summaries | v2 |
| Dream11 / streaming affiliate links | v2 |
| Corporate white-label | Post-WC |
| Instagram content pipeline | Parallel workstream, not blocking |
| Jersey affiliate links | v2 |
| Cricket / other sports | v3+ |

---

## Open Questions

1. **Passcode collision risk:** 6-char alphanumeric = 2.2 billion combinations. No practical collision risk at launch scale. Passcode is scoped per pool so even an identical code across two pools is not a vulnerability.
2. **API-Football fixture ID mapping:** football-data.org and API-Football use different fixture IDs. Need a one-time mapping table built during daily sync to avoid a separate lookup at post-match time.
3. **Exact Score UX:** Does the Exact Score input appear for all matches from match 5, or does the admin control it per match? Recommendation: admin toggles it globally at setup (simpler), not per match.

---

## Launch Checklist (v1.5 ready)

- [ ] v1 core flow stable and tested with 10+ real users
- [ ] `dailyFixturesSync` Cloud Function deployed and tested
- [ ] `postMatchResultsSync` Cloud Function deployed and tested
- [ ] API-Football to football-data.org fixture ID mapping table built
- [ ] Pre-match panel rendering correctly on mobile
- [ ] Post-match panel rendering correctly on mobile
- [ ] Exact Score input live and scoring correctly (3pt, not 4pt)
- [ ] SMS notification template updated to include points earned
- [ ] Rate limit monitoring on football-data.org in place
- [ ] Firestore security rules reviewed (participants can only write their own picks)

---

*PRD v1.5 | June 9, 2026*
*Stack: Next.js + GCP Cloud Run + Firebase Firestore + Firebase Auth + Cloud Scheduler + Cloud Functions*
*No Haiku. No Vercel. No Razorpay until payment intent validated.*
