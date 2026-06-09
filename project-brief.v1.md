# FanPollPlay — Product Brief
**Sports Prediction Pool Manager for Indian Social Groups**
*Draft v1.0 | June 2026*

---

## The Problem

Every major sporting event — World Cup, IPL, Champions League — triggers the same behaviour in Indian offices, colleges, and friend groups. Someone volunteers to run a prediction pool. They collect picks over WhatsApp, maintain a messy Excel sheet, manually update scores after every match, and post leaderboard screenshots to the group.

It works. But it is painful, error-prone, and entirely dependent on one person's time.

No product exists that solves this for Indian groups specifically — WhatsApp-native, UPI-ready, and built for the way Indians actually consume sports.

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
| WhatsApp/SMS reminders | None | Automated |
| Result auto-scoring | Manual trigger | Fully automatic |
| Access control | Link only | Link + code, phone whitelist |
| Export | None | CSV leaderboard |

**Pricing:** ₹199 one-time per event (not subscription). Admin pays. Participants always free.

**Upgrade trigger:** Admin hits 15-person limit mid-event. Platform prompts: *"X more people want to join — unlock unlimited for ₹199."* Maximum motivation at the moment of friction.

---

## Access Control

Three modes — admin chooses at setup:

1. **Link + code** — Shareable link requires a 6-digit code to enter. Admin shares both on WhatsApp. Default for friend/college groups.
2. **Phone whitelist** — Admin uploads phone numbers. Only whitelisted numbers can join via OTP. Best for corporates.
3. **Invite-only** — Platform sends individual WhatsApp invites. Each link is unique and non-transferable. Most controlled.

---

## Competition Types (Football — v1 scope)

### Match-level
| Competition | Description | Engagement |
|---|---|---|
| Match Result | Win / Draw / Loss | ★★★ (gateway) |
| Exact Score | Predict scoreline | ★★★★★ |
| First Goalscorer | Who scores first | ★★★★ |
| Both Teams to Score | Yes / No binary | ★★★ |
| MOTM Community Vote | Post-match poll, no scoring | ★★★ |

### Tournament-level
| Competition | Description | Engagement |
|---|---|---|
| Tournament Winner | Pick champion at start | ★★★★ |
| Golden Boot | Top scorer pick | ★★★★ |
| Group Stage Standings | Rank 4 teams per group | ★★★ |
| Knockout Bracket | Full bracket from R32 to Final | ★★★★★ |
| Upset Pick | One underdog call, all-or-nothing | ★★★ |

### Social / engagement (no scoring)
| Competition | Description |
|---|---|
| Weekly Hot Take Poll | Admin writes a question, community votes |
| Team of the Tournament | Running best XI vote, updates every 5 matches |

**v1 recommended scope:** Match Result + Exact Score + Tournament Winner + Golden Boot + Knockout Bracket. Five competitions. Complete product.

---

## The Core Product Insight

> The organiser does not want to manage a product. They want to send one WhatsApp message.

That message is: *"Join our World Cup pool 👉 [link]"*

Everything before and after must be invisible. Pool created in under 2 minutes. Leaderboard updates itself. Organiser touches it again only to brag.

---

## Technical Architecture (suggested)

| Layer | Choice | Reason |
|---|---|---|
| Frontend | Next.js | SSR, fast, SEO |
| Data | football-data.org API | Free tier covers all World Cup fixtures, standings, results |
| AI layer | Claude API (Haiku) | Match previews, predictions — near-zero cost |
| Payments | Razorpay | UPI + cards, 5-min integration |
| Hosting | Vercel | Instant deploy |
| Auth | Phone OTP (magic link) | No password friction for participants |
| Notifications | WhatsApp Business API or MSG91 | Pick reminders, result alerts |

**Key automation:** Cron job post-match pulls results from football-data.org, scores all picks in each pool automatically, updates leaderboard. Zero organiser involvement.

---

## Cross-sell Architecture (the long game)

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

**The white-label corporate play** is the highest-value path. HR teams at mid-size companies will pay ₹5k–₹15k for a branded pool with employee engagement metrics. Prodefy does this globally — no Indian product does.

---

## Revenue Model — World Cup Window (39 days)

| Scenario | Pool creators | Revenue |
|---|---|---|
| Conservative | 500 | ₹99,500 |
| Base | 1,500 | ₹2,98,500 |
| Optimistic | 3,000 | ₹5,97,000 |

Affiliate revenue (Dream11, streaming) adds 20–40% on top at scale.

**Post-tournament:** Reactivation for IPL + Euros + Champions League = same user base, near-zero CAC. Year-round revenue from a once-built product.

---

## Competitive Landscape

| Product | Users | Paid? | India-native? | WhatsApp-native? |
|---|---|---|---|---|
| Superbru | 2.6M | Free | No | No |
| ESPN Predictor | Massive | Free | No | No |
| wcpredictor.app | Unknown | Free | No | No |
| Prodefy | Unknown | Free | No | No |
| **FanPollPlay** | 0 | ₹199/event | **Yes** | **Yes** |

The gap: every global product is free, English-first, and web/app-only. None is WhatsApp-native, rupee-priced, or built for Indian social group dynamics.

---

## What This Is Not

- Not an AI prediction tool (crowded, free incumbents)
- Not a content platform or social media play
- Not a fantasy sports product (regulatory complexity)
- Not a betting product (out of scope, legally complex)

---

## Open Questions for PM Discussion

1. **Freemium line** — Is 15 the right cut-off, or should it be 10 (tighter) or 20 (looser)? Needs validation with actual corporate pool admins.
2. **Notification channel** — WhatsApp Business API has approval friction. Is SMS a sufficient fallback for v1?
3. **Scoring system** — Simple (result = 1pt, exact score = 3pt) vs complex (upset bonus, streak multiplier). Simple wins for adoption but complex drives engagement. Decision needed before build.
4. **Sport-agnostic from day 1 or World Cup only?** — Building sport-agnostic adds 3–4 weeks. Starting football-only ships in 3 days. Risk: brand association with one event.
5. **Corporate sales motion** — Direct outreach to HR teams requires a different funnel than consumer. When to split focus?
6. **Post-tournament retention** — What keeps users on the platform between events? Needs a content or community hook beyond the next tournament.

---

## Immediate Next Steps

1. Talk to 3 corporate pool organisers — validate pain, test ₹199 price point
2. Register domain (sport-agnostic name — not WC-specific)
3. Build v1 MVP: pool creation → join link → pick submission → auto-scoring → leaderboard
4. Soft launch before June 11 with personal network as first 50 users

---

*Brief compiled from product brainstorm session | June 9, 2026*
*Built for: FanPollPlay v1 PM discussion*