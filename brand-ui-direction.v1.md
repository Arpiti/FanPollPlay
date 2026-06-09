# FanPollPlay — Brand & UI Direction
*Pixel Retro Soccer Game Aesthetic | v1.0 | June 2026*

---

## 1. Brand Identity

### 1.1 Brand Name Treatment

"FanPollPlay" is rendered as a three-part wordmark in a single continuous line — three words, three visual registers, one name. The split is the core identity logic: each segment carries a distinct personality drawn directly from the brand's pixel-retro soccer game aesthetic. All three parts use Press Start 2P. No space between parts — a 4px gap only, keeping the name compound.

**Fan** — Press Start 2P, Pixel White (`#F0EDE8`), regular weight. The lightest of the three parts visually. This is the "crowd" word — the audience, the group chat, the people watching. No shadow, no overlay. Clean and present, the way a stadium full of people is background and foreground simultaneously.

**Poll** — Press Start 2P, Scoreboard Amber (`#F5A623`), uppercase. Pixel drop shadow offset 2px right and 2px down, rendered in Deep Arcade Navy (`#0D1B2A`). This is the "scoreboard" word — heavy, blocky, stadium-announcement energy. It carries the weight of the prediction mechanic: the thing everyone sees, the number everyone argues about.

**Play** — Press Start 2P, Pitch Green (`#16A83C`). Rendered as if lit from below by pitch-side floodlights — the CRT glow of a match in progress. A 1px scanline overlay runs across this segment only, at 10% opacity, simulating CRT phosphor persistence. This is the "game" word: the action, the result, the moment the prediction resolves.

The three parts sit on one baseline with a 4px gap between each segment — not a space, not a hyphen. The name reads as one product with three personalities running left to right: who is here, what they are doing, why it matters.

The wordmark sits inside an optional lockup: a pixel-art scoreboard frame (two vertical columns of pixels flanking the full name, suggesting stadium floodlight stanchions). This frame is used on app launch screens, OG images, and WhatsApp share cards. It is omitted on compact placements like the mobile header, where the icon mark takes its place.

**Clear space rule**: The height of the letter "P" on all sides. Never place the wordmark on busy photographic backgrounds without a dark overlay.

### 1.2 Logo Concept

**Primary mark**: A pixel football (soccer ball) rendered at 32x32px grid resolution. The hexagonal panels are drawn in 3-4 pixel blocks — not detailed, deliberately blocky. The ball is mid-flight, tilted 15 degrees, with a 4-pixel motion trail in amber extending from its lower-left. This references the ball trail effect in ISS Pro Evolution and FIFA 98 when a player struck a shot.

The ball sits inside a pixel-art stadium arc — a thin arc of pixels suggesting the top of a stadium bowl, like the scoreboard frame seen in early FIFA games. The arc is 2px thick in the pixel style.

**Icon-only mark** (favicon, app icon, small placements): Just the ball with the motion trail. No frame, no text. At 16x16, it reduces cleanly to a white ball on pitch green with the amber trail.

**Do not do**: Illustrated soccer players, grass textures, photographic elements. Pure pixel geometry. No gradients inside the mark — flat pixel fills only.

### 1.3 Tagline Options

1. **"Pick. Score. Brag."** ← Recommended primary
2. **"Your group's prediction game."**
3. **"The gaffer in your group chat."**
4. **"Every match is a bet you don't need money for."**
5. **"16-bit. Full-throttle. Full points."**

**Recommended**: "Pick. Score. Brag." — rhythm, works in English, translates conceptually to Hindi contexts, tells you everything in three words.

### 1.4 Brand Voice

**Five adjectives**: Competitive, Sharp, Nostalgic, Punchy, Never Corporate.

FanPollPlay sounds like the smartest guy in your office cricket WhatsApp group — the one who always remembers the score from 2002 and has an opinion about everything. Copy is short, takes jabs, occasionally calls you out ("Still going with a draw? Brave."). It references the game era without being a museum about it. It never says "leverage," "ecosystem," or "seamlessly." When something good happens, it sounds like a cheer in a stadium corridor, not a notification from a SaaS product.

---

## 2. Color Palette

### 2.1 Primary Palette

| Name | Hex | Usage |
|---|---|---|
| **Pitch Green** | `#16A83C` | Primary CTA buttons, active states, success, "LIVE" pill |
| **Scoreboard Amber** | `#F5A623` | Score highlights, user's rank number, match timer, "YOUR PICK" label |
| **Pixel Red** | `#E81E1E` | Goal alerts, rank drops, wrong predictions, deadline warnings, paywall accent |
| **Deep Arcade Navy** | `#0D1B2A` | Primary dark background, table rows, nav bar, modal overlays |
| **Pixel White** | `#F0EDE8` | Body text on dark, input fields, light card surfaces |

### 2.2 Supporting Colors

| Name | Hex | Usage |
|---|---|---|
| **Penalty Box Gray** | `#3A4A5C` | Secondary surfaces, disabled states, inactive tabs |
| **Half-Time Green** | `#0A5C1E` | Pressed states on Pitch Green buttons |
| **Own Goal Red** | `#7A0000` | Destructive confirmations (delete pool, end league) |

### 2.3 Usage Rules

| Context | Color |
|---|---|
| Primary CTA | Pitch Green `#16A83C` |
| Score / Rank highlight | Scoreboard Amber `#F5A623` |
| Alerts / Deadlines / Goals | Pixel Red `#E81E1E` |
| Page backgrounds (dark) | Deep Arcade Navy `#0D1B2A` |
| Body text on dark | Pixel White `#F0EDE8` |
| Upgrade / paywall accent | Pixel Red border + Amber text |

**Critical**: Never use Scoreboard Amber text on Pitch Green background — yellow-on-green fails contrast and creates visual vibration. Lock `#16A83C` exactly — even a slight yellow-green shift drops below WCAG AA.

---

## 3. Typography

### 3.1 Primary Font: Press Start 2P (Google Fonts)

**Why over alternatives**:
- VT323: Too thin, reads as hacker/terminal. Wrong register.
- Silkscreen: Clean compromise. This brand does not compromise.
- Press Start 2P: The Helvetica of pixel fonts. Nails the 1980s–90s arcade/console bitmap aesthetic without irony.

**Minimum sizes**:

| Size | Use |
|---|---|
| 8px | Never for readable content |
| 10px | Decorative labels only |
| 12px | Nav labels, badges, secondary UI chrome |
| 14px | CTA button labels (recommended minimum) |
| 16px | Body-size pixel text — use sparingly |
| 24px+ | Score displays, rank numbers, headline moments |

### 3.2 Secondary Font: DM Sans (Google Fonts, variable)

Geometric, clean, excellent legibility at 14px on mid-range Android screens. Does not feel imported. Does not fight with Press Start 2P — the contrast is clean, not cluttered.

### 3.3 Font Pairing Rules

| Context | Font | Weight | Size |
|---|---|---|---|
| Screen titles / Section headers | Press Start 2P | Regular | 18–32px |
| Score displays | Press Start 2P | Regular | 32–72px |
| CTA button labels | Press Start 2P | Regular | 12–14px |
| Nav bar labels | Press Start 2P | Regular | 10–12px |
| Rank numbers | Press Start 2P | Regular | 24–48px |
| Body copy / descriptions | DM Sans | 400 | 16px |
| Stats / data labels | DM Sans | 500 | 14px |
| Error / helper text | DM Sans | 400 | 14px |
| Match team names | DM Sans | 600 | 16–18px |
| Small print / legal | DM Sans | 300 | 12–13px |

**Golden rule**: Text that needs to be read carefully → DM Sans. Text that the eye jumps to (score, rank, CTA) → Press Start 2P.

---

## 4. UI Component Direction

### 4.1 Leaderboard Table

**Background**: Deep Arcade Navy. Alternating rows: `#0D1B2A` / `#111F30`. No rounded corners — pixel-grid rectangles. No horizontal dividers.

**Rank number**: Press Start 2P, Scoreboard Amber, 24px desktop / 20px mobile. Ranks 1–3: pixel-art icon (gold star / silver medal / bronze badge at 16x16 grid) instead of number.

**Current user row**: 4px Pitch Green left border, full-opacity Pixel White text, 5% scanline overlay on the row only.

**Points column**: Scoreboard Amber, DM Sans 600, tabular numerals. Points changes tick up in 200ms increments.

**Name column**: DM Sans 500, Pixel White, 16px, truncated at 18 characters.

### 4.2 Pick Submission Card

Full-width mobile, max 480px desktop, centered. Border-radius: 0 (or 4px max). Border: 2px solid Pitch Green. Background: Deep Arcade Navy.

**Match header**: Team name (DM Sans 600, 18px) — "VS" (Press Start 2P, 12px, Amber) — Team name. Match time below in DM Sans 400 14px at 60% opacity.

**Pick buttons**: Three fat rectangles in a row. "HOME" / "DRAW" / "AWAY" in Press Start 2P 12px. 80px wide × 56px tall minimum. Default: Navy fill, 2px Pixel White border. Selected: Pitch Green fill. Tap: 100ms scale to 0.96, snap back.

**Pixel team crests**: 24x24 pixel-block in team's primary color — no photographic logos, avoids licensing. Deliberately low-res, true to aesthetic.

**Confirmation state**: Card stays visible, shows "LOCKED IN" in Press Start 2P 12px Pitch Green + pixel checkmark + countdown to kickoff.

### 4.3 Admin Dashboard

**Overall**: Retro arcade control panel. Sidebar/bottom sheet at `#070E17`. Main area: Deep Arcade Navy.

**Header**: Full-width Pitch Green strip, pool name in Press Start 2P 18px in Deep Arcade Navy. Status pill: Scoreboard Amber text on 4px amber left border.

**Section headers**: Press Start 2P 12px, Scoreboard Amber, 1px dashed border below.

**Top stats**: Three bordered rectangles — number in Press Start 2P 32px amber, label in DM Sans 400 12px. Stats: PARTICIPANTS / PICKS SUBMITTED / MATCHES LIVE.

**Match toggle**: Pixel padlock icon (red = locked) / pixel arrow icon (green = open). 150ms snap — no easing.

### 4.4 Match Stat Card

**Pre-match**: Scoreboard countdown. Team A — blinking ":" — Team B. Colon in Scoreboard Amber, 1s on / 1s off at 50% opacity when off.

**Post-match**: Score in Press Start 2P 48px Scoreboard Amber. Goal log in DM Sans 400 13px. Goalless draw gets a tiny "BORING." in Press Start 2P 8px in Penalty Box Gray — a reward for attention.

**Prediction strip**: A row of colored pixel blocks (one per participant) — green (correct) / amber (partial) / red (wrong). Tap to reveal name.

### 4.5 Mobile Header / Nav (HUD)

**Header (52px)**: Left: ball icon only (no wordmark). Center: Pool name DM Sans 600 16px. Right: rank chip in Scoreboard Amber — Press Start 2P 10px, amber background, navy text.

**Bottom nav (3 items max)**: Home | Picks | Leaderboard. Pixel-art 20px icons + Press Start 2P 8px labels. Active: Pitch Green icon + green pixel underline. No hamburger menu — ever.

### 4.6 Upgrade Prompt (₹199 Paywall)

**Step 1 — The Nudge**: Amber bottom banner (56px). "Pool filling up. Upgrade to unlock unlimited." + "LEVEL UP →" CTA button.

**Step 2 — The Modal**: Full-screen overlay (mobile). `#111F30` surface with **2px Pixel Red border** — the single urgency signal. No countdown timers, no fake scarcity.

Modal content:
- "UNLOCK FULL POOL" — Press Start 2P 16px Pixel White
- Three feature bullets in DM Sans 400 15px (game language: "Unlimited players", "All matches", "Season leaderboard")
- **"₹199"** — Press Start 2P 32px Scoreboard Amber — largest element on the screen
- "One-time per pool. No subscription." — DM Sans 400 12px
- "PAY & PLAY" — full-width Pitch Green button, Press Start 2P 12px
- "Not now" — DM Sans 400 12px at 40% opacity (present, not competing)

### 4.7 Toast Notifications

Appear from bottom. 56px tall, full-width minus 16px margins, 8px border-radius. 3.5 second auto-dismiss, tap to dismiss.

| Type | Treatment |
|---|---|
| Success | Pitch Green background, Deep Arcade Navy text, pixel checkmark |
| Error | Pixel Red background, Pixel White text |
| Info | Deep Arcade Navy + 2px Amber border, Amber text |
| Banter/Social | `#111F30` + `>` cursor prefix before DM Sans text (terminal energy) |

Example banter toast: `> Rahul moved up to 2nd.`

---

## 5. Motion & Interaction Language

### 5.1 Loading State

Horizontal pixel-block progress bar (80% width, centered). Fill: Pitch Green advancing in discrete 8px block increments using CSS `steps()` timing — not smooth, stepping. "LOADING..." in Press Start 2P 10px Scoreboard Amber with blinking cursor block.

### 5.2 Score Reveal Sequence

1. All point values flip to "---" simultaneously
2. 500ms hold
3. Scores tick up digit-by-digit using vertical scroll clip, 80ms per digit
4. Rows move position with 300ms ease-out translateY
5. Rank up: 100ms Pitch Green background flash
6. Rank down: 100ms Pixel Red background flash
7. Banner above table: "UPDATING SCORES..." → "FINAL STANDINGS"

### 5.3 Leaderboard Row Movement

- Moving up: translateY(-8px) over 250ms + Pitch Green left border pulse (2px → 6px → 2px over 400ms)
- Moving down: translateY(+8px) + Pixel Red border pulse
- No change: no animation (stillness = nothing happened)
- Points badge on gain: "+3 POINTS" chip in Press Start 2P 10px, Pitch Green, fade in 100ms / hold 800ms / fade out 200ms

---

## 6. Mobile-First Constraints

**Target device**: ₹15,000 Android (Redmi 12 / Moto G64 range) — 360dp–412dp logical width, Chrome for Android.

**Press Start 2P rendering**: Always use multiples of 8px. Set `image-rendering: pixelated` on any CSS-scaled pixel art SVG/Canvas.

**Touch target minimums**:

| Element | Minimum |
|---|---|
| Pick buttons (HOME/DRAW/AWAY) | 80px × 56px |
| Nav bar icons | 48px × 48px touch area |
| Leaderboard rows | 52px height |
| Upgrade CTA | Full-width minus 32px, min 52px tall |
| Admin toggles | 44px × 44px |

**Contrast checks**:
- Pitch Green + Pixel White: 5.1:1 — WCAG AA pass, do not drift green toward yellow
- Deep Arcade Navy + Pixel White: 12:1 — AAA
- Scoreboard Amber + Deep Arcade Navy: 9:1 — excellent

---

## 7. Competitive Differentiation

**Superbru**: Flat corporate teal, WordPress-template aesthetic. Nothing says "thrill of competition." Designed to not offend enterprise procurement.

**ESPN Predictor**: ESPN red/gray/white editorial system. Desktop product reluctantly made mobile. Hierarchy screams journalism, not interactivity.

**Dream11**: Neon green and black, gamification as manipulation. "Crypto exchange trying to look like a game." High noise, low playfulness. No retro reference.

**What FanPollPlay says that they don't**:

1. **Retro as identity, not decoration**: The pixel language is the design system — in typography, color, button shape, loading bar, score animation. Not a skin over a generic interface.

2. **Scoreboard as theatre, not widget**: Every competitor shows a leaderboard as a data table. FanPollPlay's leaderboard borrows the visual language of the stadium, the typography of the LED display, the dramatic reveal of the real thing.

3. **Designed for sharing, not sessions**: The large amber rank number, the "LOCKED IN" confirmation — optimized to produce a screenshot that immediately communicates pool status without explanation. The product is a group object, not a personal app.

4. **Earned nostalgia for the target cohort**: The 25–40 Indian sports fan went to college between 2000–2015. They played ISS Pro Evolution on a CRT or FIFA 2002 on a PS2 in the hostel. This aesthetic is not "retro" in an ironic sense — it is autobiographical. No product in this space has claimed this memory. FanPollPlay owns it first.

---

## Developer Handoff Notes

- **Design tokens file**: Create `design-tokens.css` with all color + typography variables
- **Key component files**: `components/leaderboard.css`, `components/pick-card.css`, `components/toast.css`
- **Animation implementation**: CSS `steps()` for pixel-block loading bar, `translateY` + opacity for leaderboard movement, CSS custom property transitions for score tick-up
- **Font import**: `@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&family=DM+Sans:wght@300;400;500;600;700&display=swap')`

---

*Brand & UI Direction v1.0 | June 9, 2026*
*Aesthetic: 16-bit pixel retro soccer game (ISS Pro Evolution / FIFA 98 era)*
*Primary font: Press Start 2P | Secondary: DM Sans | Stack: Next.js + Tailwind*
