# FanPollPlay — Product Brief
**Sports Prediction Pool Manager for Indian Social Groups**
*Draft v2.0 | June 2026 — Post PM Review*

> **Reframe:** "Run your office World Cup pool without becoming the pool manager."
> Every CTA, every headline, every onboarding prompt is written for the person about to spend 3 hours managing a WhatsApp thread this week. They want permission to not do that. You're selling that permission for ₹199.

---

## The Problem

Every major sporting event — World Cup, IPL, Champions League — triggers the same behaviour in Indian offices, colleges, and friend groups. Someone volunteers to run a prediction pool. They collect picks over WhatsApp, maintain a messy Excel sheet, manually update scores after every match, and post leaderboard screenshots to the group.

It works. But it is painful, error-prone, and entirely dependent on one person's time.

**The real competitor isn't Superbru — it's WhatsApp Polls + Google Sheets.** An organiser who's been running pools on Excel for 3 years doesn't experience their system as broken. They experience it as effortful. The switch cost from "effortful but free and familiar" → "₹199 on a new platform" must be anchored on time saved, not features listed.

---

## The Opportunity

- 500M+ WhatsApp users in India
- Corporate India runs informal prediction pools at every major tournament
- World Cup 2026 starts June 11 — 104 matches over 39 days
- IPL, Champions League, Euros, Asia Cup follow — the event calendar is year-round
- Global competitors (Superbru, Prodefy, ESPN Predictor) are all free, English-only, not WhatsApp-native, and not priced in rupees
- No Indian product owns this space

---

## The Product

**FanPollPlay** is a web-based group prediction pool manager. An admin creates an event, shares one WhatsApp link, and the platform handles everything else — pick collection, auto-scoring, and live leaderboards.

No app download. No account creation for participants. Works entirely in the browser via a shared link.

---

## Core User Personas

### The Organiser (pays, sets up)
- HR manager, office friend, college group admin
- Goal: run a clean pool without spending time on it
- Pain: Excel chaos, manual score updates, WhatsApp pinging
- Success: sends one message, leaderboard runs itself

### The Participant (free, just plays)
- Casual to moderate football fan
- Goal: submit picks fast, check if winning, brag
- Attention span: 90 seconds per interaction
- Constraint: must work on mobile, no friction

### The Lurker (passive viewer)
- Just checks the leaderboard
- Doesn't predict but shares the link — organic distribution engine

---

## Freemium Model

| Feature | Free Tier | Paid Tier |
|---|---|---|
| Participants | Up to 15 | Unlimited |
| Active competitions | 2 | Unlimited |
| Leaderboard | Basic (name + score) | Full stats, streaks, head-to-head |
| Pool branding | Platform branded | Custom name + logo |
| SMS reminders | None | Automated |
| Result auto-scoring | Manual trigger | Fully automatic |
| Access control | Link + passcode | Phone whitelist (v2) |
| Export | None | CSV leaderboard |

**Pricing:** ₹199 one-time per event (not subscription). Admin pays. Participants always free.

**Participant auth model:** Name + passcode on first join. No phone OTP for participants. Server issues a session token stored in localStorage — returning visits auto-identified. Admin is the only user with a phone-verified account (needed for payment and admin SMS alerts).

**Upgrade trigger:** Admin hits 15-person limit mid-event. Platform prompts: *"X more people want to join — unlock unlimited for ₹199."* Maximum motivation at the moment of friction.

**Critical UX note:** The 15-participant cap must be front-and-center during pool setup — not buried in a pricing table. Organiser who shares a link with 40 colleagues and hits the wall on day 2 will either pay or tell everyone "the free tool broke." This is a copy and UX problem that must be solved before build.

---

## Scoring System (decided)

Simple. Fixed. No exceptions in v1.

| Prediction | Points |
|---|---|
| Match Result (W/D/L) correct | 1 pt |
| Exact Score correct | 3 pt |

No upset bonuses, streak multipliers, or partial credit. Complexity drives engagement but kills adoption. Revisit for v2 once users are active.

---

## v1 Scope — What Ships by June 11

### Must-have (build this, nothing else)

1. Admin creates pool → gets a shareable link (2-min flow)
2. Participant opens link → name + phone OTP → submits Match Result pick
3. Admin dashboard → who has/hasn't submitted
4. Post-match: admin triggers scoring manually (insurance against cron failure)
5. Leaderboard visible at the same URL to everyone including lurkers

### Explicitly NOT in v1

| Cut Item | Reason |
|---|---|
| Exact Score predictions | Add at match 5+ once flow is stable |
| Knockout Bracket | A product in itself — tiebreakers, bracket resets, partial credit, team withdrawals = support tickets |
| Golden Boot / Tournament Winner | v2 |
| WhatsApp Business API | Approval takes weeks; not a v1 feature |
| Access code / phone whitelist | Link-only is fine; complexity kills onboarding speed |
| Razorpay integration | Take ₹199 via UPI QR manually for the first 20 pools. Validate price before building payment infra. |
| AI match previews (Claude Haiku) | Zero users have asked for it. Zero signal. Add when 1,000 active users exist. |

---

## Notifications — v1 Decision

**SMS via MSG91** is the v1 notification layer — live in 30 minutes. WhatsApp Business API is v2 after a business track record is established. The brief's WhatsApp API plan is struck.

---

## Technical Architecture

| Layer | Choice | Reason |
|---|---|---|
| Frontend | Next.js | SSR, fast, SEO |
| Data | football-data.org API | Free tier covers all World Cup fixtures, standings, results |
| Payments | UPI QR (manual) for first 20 pools, then Razorpay | Validate intent before building infra |
| Hosting | Vercel | Instant deploy |
| Auth | Phone OTP | No password friction for participants |
| Notifications | MSG91 SMS | Live in 30 minutes; WhatsApp API is v2 |

**Critical architecture note:** football-data.org free tier = 10 req/min. 500 pools pulling results independently will hit rate limits within days. One shared result cache layer across all pools is required before launch. Miss this and you're doing a war room on live traffic.

**Key automation:** Cron job post-match pulls results from football-data.org → cached → scores all picks in each pool automatically → updates leaderboard. Admin can also trigger manually as a fallback.

---

## Open Questions — Closed

| Question | Decision |
|---|---|
| Freemium line: 10, 15, or 20? | 15. Validate with real organisers post-launch. |
| Notification channel for v1? | SMS (MSG91). WhatsApp API is v2. |
| Scoring: simple vs complex? | Simple. 1pt result, 3pt exact. No upsets/streaks until v2. |
| Sport-agnostic from day 1? | Football-only for v1. Ship in days, not weeks. |
| Corporate sales motion? | Not in v1. One-on-one outreach post-launch when there's a product to show. |
| Post-tournament retention? | One-click pool reactivation for IPL. Not a product feature — it's the organiser relationship. |

---

## Revenue Projections (recalibrated)

No SEO, no budget, no brand, 2 days to launch. Plan for this:

| Scenario | Pool creators | Revenue |
|---|---|---|
| Realistic | 50–100 | ₹10k–₹20k |
| Strong execution | 300–500 | ₹60k–₹1L |
| Brief's base case | 1,500 | Not achievable in Window 1 without distribution |

50–100 paying organisers in Window 1 is still a real product with real paying users. Build for that. The brief's 1,500-pool base case requires distribution that doesn't exist yet.

Affiliate revenue (Dream11, streaming) adds 20–40% on top at scale — valid long-term, not a launch metric.

---

## Cross-sell Architecture (the long game — unchanged)

FanPollPlay's real asset is not the product — it is the audience it builds.

Every pool participant is:
- A verified Indian phone number
- A sports fan with measurable prediction behaviour
- Inside a mapped social graph (office, college, friend group)
- Active for the duration of a tournament

**Monetisation layers beyond pool creation fees:**

| Layer | Mechanic | Timeline |
|---|---|---|
| Dream11 affiliate | "Suggested Dream11 XI based on your picks" + referral link | Tournament Day 1 |
| Streaming affiliate | JioCinema / SonyLIV contextual links in result notifications | Tournament Day 1 |
| Merchandise affiliate | Fancode jersey links contextualised to pool winner picks | Mid-tournament |
| Corporate white-label | Branded pools for HR teams with participation reporting | Post-WC |
| B2B reactivation | One-click pool reactivation for IPL, Euros, Champions League | Next event |

The white-label corporate play remains the highest-value path. Post-WC only.

---

## Competitive Landscape

| Product | Users | Paid? | India-native? | WhatsApp-native? |
|---|---|---|---|---|
| Superbru | 2.6M | Free | No | No |
| ESPN Predictor | Massive | Free | No | No |
| wcpredictor.app | Unknown | Free | No | No |
| Prodefy | Unknown | Free | No | No |
| **FanPollPlay** | 0 | ₹199/event | **Yes** | **Yes** |

---

## Content & Engagement Layer (Retention Play)

The leaderboard URL is shared with everyone in the pool — organisers, participants, and lurkers. Right now it's a utility visit (check score, leave). The goal is to make it a destination.

**The insight:** If someone opens the leaderboard link and finds match context — form, stats, preview, post-match breakdown — they stay longer, share it more, and the URL becomes the group's default football hub for the tournament.

### Pre-Match (visible until kickoff)
- Team form (last 5 results)
- Head-to-head record
- Key player matchups (top scorer vs opposing defence)
- Group standings context
- One-line AI-generated match preview (Claude Haiku — cheap, fast, good enough)
- Pick distribution within the pool: "62% of your group picked France to win"

### Post-Match (visible after result)
- Final score + scorers
- Key stats: possession, shots on target, xG
- AI-generated 3-line match summary
- Pool impact: who moved up/down on the leaderboard, who got the exact score
- "Best call of the match" — highlight the pool member who nailed it

### Why Claude Haiku here (context for earlier cut)
The AI layer was removed from v1 because "match predictions" had no user demand signal. This use case is different: generating 2–3 lines of contextual copy from structured data (team stats, result) has a clear retention hook and near-zero cost (~₹0.002 per match summary). Reinstate as v1.5 — add after the core flow is stable, before match 5.

### Data sources
- football-data.org: fixtures, results, standings (already in stack)
- API-Football (free tier): player stats, xG, possession data
- Claude Haiku: match summary generation from structured inputs

---

## Distribution Strategy — Instagram Content Channel

### The Plan
Automated short-video content channel on Instagram during the World Cup window. Content drives traffic back to FanPollPlay via link-in-bio and story CTAs.

### Content Types
- Match preview cards (graphic + caption)
- Post-match stat breakdowns (carousel)
- Pool leaderboard highlights ("who's winning India's office pools right now")
- Hot takes / prediction polls (mirrors the in-product poll feature)

### ⚠️ Copyright Warning — FIFA YouTube Clips
FIFA and UEFA are among the most aggressive IP enforcers in sports media. Ripping, trimming, or reposting clips from FIFA's YouTube channel — even as 15-second Reels — will trigger:
- Automatic Content ID strikes on Instagram
- Account suspension during the tournament (worst possible timing)
- Potential legal notice if the channel grows

**Do not build a pipeline around third-party match footage.**

### What Actually Works (legally)
| Format | Source | Risk |
|---|---|---|
| Original commentary over public stats/graphics | Your own voice + data viz | None |
| Screen recordings of your own leaderboard | Your product | None |
| Text-based match cards / infographics | Public match data | None |
| Reaction/opinion content | Original footage | None |
| Clips from verified creator partnerships | Licensed | Low |

### Recommended Approach for v1 Distribution
- Build a simple automated pipeline: post-match stats → generate graphic card → post to Instagram with pool CTA
- No video clipping from external sources
- Story format: "Group X pool update — France leads" with swipe-up to FanPollPlay
- This is replicable, scalable, and keeps the account alive

### Channel Strategy
- Handle: @FanPollPlay.in or similar (sport-agnostic for longevity)
- Content cadence: 2 posts per match day, 1 story per result
- CTA on every post: "Running an office pool? Link in bio."

---

## Cross-sell Architecture (updated)

FanPollPlay's real asset is not the product — it is the audience it builds.

Every pool participant is:
- A verified Indian phone number
- A sports fan with measurable prediction behaviour
- Inside a mapped social graph (office, college, friend group)
- Active for the duration of a tournament

**Monetisation layers:**

| Layer | Mechanic | Timeline |
|---|---|---|
| Dream11 affiliate | "Suggested Dream11 XI based on your picks" + referral link | Tournament Day 1 |
| Streaming affiliate | JioCinema / SonyLIV contextual links in result notifications | Tournament Day 1 |
| Jersey affiliate (FanCode) | Jersey links contextualised to pool leaderboard ("Winner picks France? Here's their jersey") | Mid-tournament |
| Jersey direct selling | Curated jersey store, own inventory, higher margin | Post-WC / v2 |
| Corporate white-label | Branded pools for HR teams with participation reporting | Post-WC |
| B2B reactivation | One-click pool reactivation for IPL, Euros, Champions League | Next event |

### Jersey Selling — Affiliate First, Direct Later
Direct jersey sales (own inventory) has logistics overhead — warehousing, sizing, returns, COD complexity. Start with FanCode affiliate links: zero ops, immediate, earns 5–10% commission. If affiliate volume is high, evaluate direct selling for IPL window with a dropshipping or single-SKU approach (one design, pre-order only).

---

## What This Is Not

- Not an AI prediction tool (crowded, free incumbents)
- Not a fantasy sports product (regulatory complexity)
- Not a betting product (out of scope, legally complex)
- Not a content platform — utility comes first. Content is a retention layer that earns its place once the core pool flow is working, not a feature built in parallel.

---

## Immediate Next Steps (ordered by priority)

1. **Today:** Send 10 WhatsApp messages to pool organisers in your network. Ask one question: "Would you pay ₹199 to not manage the pool yourself?" Validate payment intent before writing a single line of code.
2. **Before build:** Fix the 15-person cap UX copy — organiser must see the limit clearly during setup.
3. **Build v1 MVP:** Pool creation → join link → phone OTP → Match Result pick → admin dashboard → manual scoring trigger → leaderboard.
4. **Payments:** UPI QR manually for first 20 pools. No Razorpay until intent is confirmed.
5. **Soft launch before June 11** with personal network as first 50 users.
6. **Post-launch:** Add Exact Score at match 5. Track usage before adding anything else.

---

*Brief v2 compiled post PM review | June 9, 2026*
*Changes from v1: Scope cut hard, revenue recalibrated, WhatsApp API deferred, Razorpay deferred, AI layer removed, result cache requirement added, scoring system decided, open questions closed.*
*Changes from v2 base: Added content & engagement layer (pre/post match insights), Instagram distribution strategy with FIFA copyright warning, reinstated Claude Haiku for match summaries as v1.5, updated cross-sell architecture with jersey affiliate path.*
