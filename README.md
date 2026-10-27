# Data-Dash — Cyber Safety Arcade

**Data-Dash** is a Pac-Man–style arcade game that teaches kids cybersecurity and privacy awareness *by playing*. Instead of pellets and power-ups, the maze is full of personal data, phishing links, trackers, and four "ghosts" that each embody a real online threat — and the only way to win is to make safe digital choices.

> *Data is the new cookie crumb. Don't let the ghosts eat it.*

---

## The Problem

Cyber-safety education for children has a delivery gap:

- **Lectures don't stick.** Traditional awareness material (slides, pamphlets, "don't click suspicious links") is passive, forgettable, and rarely reaches kids where they actually spend time — games.
- **Threats are abstract.** Concepts like phishing, homoglyph URLs, EXIF geotagging, data brokers, and shoulder-surfing are hard to grasp without experiencing the consequence, even once, in a safe context.
- **Kids learn by doing.** A child who has *been* lured by a fake "FREE ROBUX — CLICK!" bubble and lost a life remembers the lesson far better than one who read a bullet point about urgency tactics.
- **Existing "edutainment" is either too dry or not really a game.** Quizzes test recall; Data-Dash tests judgement under pressure — in real time, with real stakes inside the game loop.

**Data-Dash closes that gap** by encoding an entire digital-safety curriculum into the mechanics of a familiar, immediately playable arcade game. Every tile, ghost, door, and mini-challenge is a teachable moment with a concrete in-game consequence.

---

## The Concept

Take Pac-Man. Replace the world with a network:

| Arcade element | Data-Dash meaning |
|---|---|
| Pellets | Safe data / safe browsing — collect to score |
| Red PII nodes | **Personal data** you must secure (address, phone, password, school, birthday…) |
| Power pellet | Temporary power-up to turn the tables on threats |
| Ghosts | Four archetypal online threats (see below) |
| Two exit doors | A **real URL vs. a homoglyph phishing URL** — pick right or pay |
| Maze tiles | Privacy filters, VPN tokens, incognito gates, HTTPS gates, bait tiles, a quarantine vault, a merchant, a report zone |
| Extra lives | Your attention budget — plus an **Exposure** meter that rises every time you slip up |

### The four ghosts

| Ghost | Threat it embodies |
|---|---|
| **BLINKY** | Phishing & urgency lures — *"FREE ROBUX — CLICK!"*, *"ACCOUNT LOCKED! URGENT!"* |
| **PINKY** | Stranger danger — ambushes with unverified links, spyware "gifts", *"What's your real name?"* |
| **INKY** | The troll — spams the chat with scripts. Your counter: **Report & Block** |
| **CLYDE** | Trackers & dark patterns — drops cookies and fake *"free skin"* gift boxes |

Each ghost has taunt bubbles *paired with an educational tip*, so the lesson arrives at the exact moment of the threat.

### Three missions (levels)

Progressively larger mazes, progressively faster ghosts:

1. **GAMING LOBBY** (21×17) — *Fake links, stranger gifts, public Wi-Fi — and a merchant with gear.* Doors: `dlsc0rd-security.ru` vs `discord.com/verify`.
2. **SOCIAL STREAM** (25×21) — *A wider feed: geotags, screen-peekers and risky shares.* Doors: `insta-gr0ut.com/free` vs `instagram.com`.
3. **GROUP CHAT** (29×25) — *The full arena: trolls, spam chains and one brave upstander.* Doors: `free-robu4.win` vs `roblox.com/dev`.

Every level-clear screen delivers a concrete safety tip (spot the typo'd domain, use a privacy filter, screenshot–block–report bullying).

### Deeper teaching layers

Beyond the maze, Data-Dash layers in richer judgement checks:

- **URL challenges** — 6 sets of real vs. homoglyph phishing domains (`rob1ox-rewards.win`, `gmaill-secure.win`, `n3tflix.com`…).
- **Password sets** — pick the strong password (`Troll$42`) over the weak (`blue123`, `sunshine`).
- **PII sets** — know what's *private* (home address, phone) vs. *fine to share* (high score, favourite song).
- **Riddles & awareness bank** — 6 riddles (phishing masks, EXIF location, data brokers, fake HTTPS badges) plus a 12-question bank covering blackmail traps, app permissions, 2FA fatigue, cyberbullying upstanding, and more — each with a *"why"* explanation.
- **11 branching scenarios** (English & Greek) — multi-stage choice situations graded `safe / cautious / fail`, from the "free" AI face-filter app harvesting biometrics to the "accidentally" shared live location to the borrowed password.
- **The Five Digital Rules** — shown on the profile screen: never share PII · check for trick typos · unique password + 2FA per account · screenshot-block-report · public Wi-Fi only with a VPN.

### Arcade systems that keep it a *game*

- **Credits & shop** — buy weapons/gear (Neon Sword, Data Bow, Firewall Laser, Shield Spell, Firewall Armor, Privacy Helmet), each with a gameplay perk that also reinforces a security idea (the Privacy Helmet literally stops the Looker from reading you).
- **Ranks 1–5** — gear visually "attaches" to your avatar as you climb; rank 2 unlocks the Shield Charge.
- **Exposure meter** — passive decay, spikes on mistakes; at 100 the screen goes critical. Safe play literally keeps the world visible.
- **Report & Block** — an active ability on cooldown that banishes the troll (INKY) — the mechanic *is* the lesson.
- **Bait & switch tiles** — a "FREE +500" trap that can reward or punish depending on your read.
- **Persistent save** (`localStorage`) — high score + missions unlocked; mute toggle.

---

## Tech Stack

| Layer | Choice |
|---|---|
| Framework | React 19 + TypeScript 5.9 |
| Build | Vite 7 — with `vite-plugin-singlefile` → ships as **one self-contained HTML file** |
| Styling | Tailwind CSS 4 (`@tailwindcss/vite`) + `clsx` / `tailwind-merge` |
| Rendering | Hand-rolled Canvas 2D renderer (`src/game/render.ts`) — no game engine dependency |
| Audio | WebAudio-synthesized SFX (`src/game/audio.ts`) — zero audio assets |
| Fonts | Google Fonts: *Titan One* (display) + *Nunito* (UI) |

The single-file build is deliberate: the game can be dropped on any static host, emailed, or run from a USB stick with no server and no install — ideal for classrooms.

### Project structure

```
data-dash/
├── index.html                 # App shell, PWA/mobile meta, SEO description
├── vite.config.ts             # react + tailwind + singlefile plugins, "@" alias
├── src/
│   ├── App.tsx                # UI shell: menus, HUD, shop, scenarios, profile, save system
│   ├── main.tsx / index.css   # Entry + global styles
│   ├── utils/cn.ts            # clsx + tailwind-merge helper
│   └── game/
│       ├── engine.ts          # Core game loop: movement, ghosts, exposure, fights, economy
│       ├── levels.ts          # 3 maze definitions, PII labels, ghost roster, taunts/tips
│       ├── challenges.ts      # URL / password / PII / riddle / awareness challenge banks
│       ├── scenarios.ts       # 11 branching choice scenarios (EN + GR)
│       ├── items.ts           # Merchant inventory (4 weapons, 2 gear pieces)
│       ├── render.ts          # Canvas renderer (maze, actors, speech bubbles, effects)
│       └── audio.ts           # Synthesized sound effects + mute persistence
└── scripts/
    ├── check.mjs              # Maze validator: BFS reachability, sealed borders, no overlaps
    ├── sim.mjs                # Headless engine smoke test (scripted input, NaN/stuck detection)
    ├── sim2.mjs               # Targeted mechanics tests (entropy key, vault, Looker, bait tunnel)
    └── render-test.mjs        # Headless renderer test (stubbed 2D context, every state)
```

---

## Getting Started

```bash
npm install
npm run dev       # start dev server
npm run build     # production build → single HTML file in dist/
npm run preview   # preview the production build
```

### Validation & tests

Headless Node scripts (bundled on the fly with esbuild — no test framework needed):

```bash
node scripts/check.mjs        # validates all mazes: reachability, borders, tile sanity
node scripts/sim.mjs          # engine smoke test across all 3 levels
node scripts/sim2.mjs         # targeted mechanics regression tests
node scripts/render-test.mjs  # renderer smoke test (stubbed canvas context)
```

`check.mjs` is the design guard-rail for the hand-authored levels: it BFS-verifies that the player can reach **every** pellet and special tile, that ghost spawns/patrols live in the main ghost region, that borders are sealed on the ghost grid, that the report zone is wall-free, and that no two special kinds overlap.

---

## Development

### Architecture decisions

- **Data-driven levels.** Mazes are authored as wall *segments* on a `cols × rows` grid with auto-walled borders (`levels.ts`), which keeps corridors wide and kid-friendly and makes automated validation practical. Grid sizes grow per level (21×17 → 25×21 → 29×25) to ramp difficulty without changing the rule set.
- **Engine / renderer / UI separation.** `engine.ts` is a pure-ish state machine (`createGame` / `updateGame` + explicit action functions like `doReportBlock`, `resolveBait`, `buyItem`), `render.ts` draws from state, and `App.tsx` owns React UI and input. This split is what makes the headless test scripts possible — the whole game logic runs in Node without a DOM.
- **Content as code.** Challenges, scenarios, ghost taunts, PII labels, tips, and the Five Rules are all plain data structures in TS files. Adding a new scenario or URL challenge is a data edit, not a code change — and every taunt is structurally paired with its educational tip so lessons can't ship without one.
- **No game engine, no assets.** Canvas 2D + synthesized audio + procedural particles keep the bundle tiny and the single-file build viable.
- **Difficulty as a dial.** Ghost base speed scales per level (104 → 114 → 126) *and* with player exposure (`v *= 1 + (exposure/100)*0.4`) — sloppy play makes threats physically faster, a mechanical metaphor for rising risk.
- **Economy with meaning.** Points/credits are awarded for *safe* behaviours (securing PII +50, verified door +100, report & block +75) and exposure is charged for mistakes (bait fail, challenge fail) — the score itself teaches the curriculum.

### Quality process

Because levels are hand-authored and the engine is timing-sensitive, validation was built *with* the game rather than after it:

1. `check.mjs` — structural invariants for every maze (BFS connectivity for player and ghost grids separately).
2. `sim.mjs` — runs minutes of simulated play per level with scripted input to catch crashes, NaNs, stuck actors, and broken state transitions.
3. `sim2.mjs` — regression coverage for the trickier mechanics (entropy key, quarantine vault + magnet, shoulder-surfing Looker, bait tunnel).
4. `render-test.mjs` — draws every level in every special state against a stubbed 2D context to catch renderer runtime errors headlessly.

---

## Content Inventory

| Category | Count |
|---|---|
| Missions (maze levels) | 3 |
| Threat ghosts | 4 |
| Branching scenarios (EN/GR) | 11 |
| URL phishing challenge sets | 6 |
| Password / PII challenge sets | 3 + 3 |
| Riddles | 6 |
| Awareness questions (with explanations) | 12 |
| Shop items | 6 |
| Safety tips (loading/profile/rules) | 7 + 5 rules |

---

## Target Audience

Children & young teens (~8–14), parents, and educators. The game is mobile-ready (viewport/PWA meta tags), single-file deployable, and uses bilingual content (English + Greek scenarios) for classroom and home use.

---

## License / Status

Private project. Built with React, Vite, Tailwind CSS, and a lot of hand-authored maze geometry.
