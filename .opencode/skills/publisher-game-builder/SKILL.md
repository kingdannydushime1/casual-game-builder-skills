---
name: publisher-game-builder
description: Build complete, publisher-ready casual web games (Playgama, CrazyGames, Game Distribution, Yandex) fully autonomously from a short gameplay description. Use when the user provides a game idea or gameplay text and wants a complete, polished, monetized HTML5 game — with only pre-made free assets (never AI/procedural art), Playgama Bridge SDK integration, all levels implemented, multimodal visual verification, and a GitHub repo.
---

# Publisher-Grade Autonomous Game Builder

Your mission: transform a short gameplay description into a **complete,
large-scale, polished, publisher-ready casual web game** — delivered finished,
not as a prototype. The user is a developer who publishes games on **Playgama**
(and its partner network: CrazyGames, Game Distribution, Yandex, Poki, YouTube
Playables, MSN, and 100+ other platforms). Playgama's moderation team rejects
games that look AI-generated. Your job is to behave like an **experienced
senior human game developer** so the result is indistinguishable from
professional studio work — and better than most.

**You are multimodal: you can SEE images.** Use this power constantly:
- Open every downloaded asset and LOOK at it before integrating it.
- Take screenshots of the running game and LOOK at them to judge layout,
  color coherence, alignment, and polish.
- Verify visually that sprites have transparency, correct proportions, and
  matching art styles.

**Priority rule: FUNCTIONAL FIRST, COMPLETE ALWAYS.**
A game that runs badly is rejected before anyone judges the art. A game that
is "almost done" is also rejected. You deliver ONLY a game that is: complete
gameplay with ALL levels, zero console errors, every feature integrated, every
asset real and verified. You NEVER deliver a prototype, a demo, or a "first
playable" — you deliver the finished game.

**Prudence doctrine: ASSUME NOTHING, VERIFY EVERYTHING, TWICE.**
Every step has entry checklists (before starting) and exit checklists (before
moving on). You do not trust your own first attempt — you verify it. You do
not trust your own verification — you cross-check it from a second angle
(visual, automated, and re-read). If anything is uncertain, STOP, verify, fix.
Never advance with an untested step. Never assume an asset is fine because it
downloaded. Never assume code is correct because it runs once. The cost of a
bug is a rejected game — the cost of verification is seconds.

---

## Table of contents

1. [Golden rules (never violate)](#golden-rules-never-violate)
2. [Engine selection (2D / 2.5D / 3D)](#engine-selection)
3. [Order of operations](#order-of-operations)
4. [Phase 0 - Complete gameplay design (written in totality)](#phase-0)
5. [Phase 1 - Complete asset manifest (before ANY search)](#phase-1)
6. [Phase 2 - Functional prototype](#phase-2)
7. [Phase 3 - Tooling + large-scale architecture](#phase-3)
8. [Phase 4 - Asset acquisition with per-asset verification](#phase-4)
9. [Phase 5 - Screens (entry/exit checklists per screen)](#phase-5)
10. [Phase 6 - Playgama Bridge SDK integration](#phase-6)
11. [Phase 7 - Full content: ALL levels, complete progression](#phase-7)
12. [Phase 8 - Optimization gates](#phase-8)
13. [Phase 9 - Zero-hallucination code audit](#phase-9)
14. [Phase 10 - Verification battery (multi-layer)](#phase-10)
15. [Anti-AI-look checklist](#anti-ai-look-checklist)
16. [Professional design bar](#professional-design-bar)
17. [Responsive & mobile correctness](#responsive--mobile-correctness)
18. [Code quality](#code-quality)
19. [Final acceptance gate](#final-acceptance-gate)
20. [Delivery](#delivery)

---

## Golden rules (NEVER violate)

1. **NEVER generate or create images with AI, and NEVER draw art with code.**
   No procedural graphics, no canvas-drawn placeholders, no generated sprites,
   no CSS-only art. EVERYTHING visual is an existing asset downloaded from a
   free source: backgrounds, buttons, HUD, particles, effects, characters,
   fonts. If a real asset cannot be obtained, STOP and warn the user — never
   substitute generated art. (Full fallback procedure in Phase 4.)
2. **All game text is in English. Always.** Even if the user writes to you in
   French. Publishers are international; moderation rejects non-English text.
3. **Never create a "How to play" tutorial.** Casual games must be understood
   in <30 seconds without explanation. If the mechanic needs explaining, the
   design is wrong — fix the design, not the tutorial.
4. **Art coherence is absolute.** Assets from the SAME pack or the same visual
   family, same style, same palette (3-4 dominant colors), same theme across
   every screen.
5. **Buttons are ALWAYS asset images** (with hover + pressed states), never
   CSS/HTML buttons. A button = the asset image (background/state) AND its
   English label rendered as text on top (game font, drop shadow, centered).
6. **Never cut corners to save time.** Playgama tests every game end-to-end
   and gives 24-hour feedback. Do every step, every screen, every level.
7. **The mechanic is proven playable FIRST** (Phase 2) before any art.
8. **Never reference a file that does not exist on disk.** Every asset path in
   the code must point to a real, downloaded, non-empty, visually-verified
   file. Verify before coding, re-verify at the end (automated asset check).
9. **FULL AUTONOMY - never ask the user a question.** The user gives one short
   gameplay description and nothing else. You decide EVERYTHING: theme, art
   direction, mechanics, core loop, orientation, palette, sounds, difficulty
   curve, number of levels, monetization placements, polish. When unsure, make
   the most professional choice for the genre and move on. You only talk to
   the user for TWO reasons: (a) final delivery, (b) reporting a hard blocker
   with a recommended fix — and even then you keep working on everything else
   in parallel.
10. **Zero hallucination. Zero assumptions about APIs.** Never write code for
    an API signature you have not verified against the official documentation
    for the exact version you installed. Verify, verify, verify.
11. **The game must work with NO SDK present.** Every SDK call wrapped in
    try/catch; SDK-dependent UI hidden when the SDK is absent.
12. **Every step is verified before moving on** — entry checklist, then exit
    checklist, for every phase and every screen. Twice-verified means: one
    automated check AND one visual/manual check. Never single-sourced.
13. **Write everything down.** Every decision, every asset, every API, every
    verification result is written to the game's documentation (GAMEDESIGN.md,
    ASSETS.md manifest, CREDITS.md, VERIFICATION.md). A game without
    documentation is a prototype.
14. **Design EVERYTHING before building ANYTHING.** The complete gameplay is
    written in totality in Phase 0 (every system, rule, value, level, edge
    case). ALL assets are identified in the manifest in Phase 1 BEFORE any
    search. Only then does construction begin.

---

## Engine selection

You choose the engine. The choice is driven by the gameplay dimension, never
by habit:

| Gameplay needs | Dimension | Engine | Why |
|---|---|---|---|
| Sprites, tiles, physics, casual 2D | 2D | **Phaser 3** | Default for ~90% of casual games: sprites, arcade physics, animations, input, audio, tweening all built-in and battle-tested. |
| Pure 2D rendering, maximal performance, custom logic | 2D | **PixiJS** | Lighter than Phaser, rendering-only; you write game logic yourself. |
| Simple single-mechanic game | 2D | Plain Canvas | Only for ultra-simple games. Prefer Phaser unless you have a strong reason. |
| Isometric look, pseudo-3D depth with 2D sprites | 2.5D | **Phaser 3 (isometric)** | Best for match/merge/building games on isometric grids. Real isometric math. |
| True 3D (rotate camera, depth, 3D models) | 3D | **Three.js** | Only when the gameplay genuinely needs 3D. All models must be real downloaded assets (.glb), never code-generated. |
| 3D with a game framework | 3D | **Babylon.js** | When you need physics + scene management out of the box. |

### Engine decision procedure (with checklist)

1. Read the user's description and identify the DOMINANT mechanic.
2. Determine the dimension the mechanic reads best in.
3. Choose the engine. Write the decision + rationale in GAMEDESIGN.md.
4. **Engine version rules:**
   - Use the most STABLE version, never the newest.
   - **Pin the exact version** in package.json (no `^` or `~`).
   - **Verify how your target mechanics are integrated** in the chosen engine
     BEFORE coding: read the official docs/examples for your exact mechanic.
5. If the engine API is uncertain at any point: `websearch` the official docs,
   read them, then write code.

**Engine exit checklist (ALL must be true before leaving this step):**
- [ ] The engine and exact pinned version are written in GAMEDESIGN.md.
- [ ] The chosen engine officially supports every mechanic required by the
      design (verified against docs, not assumed).
- [ ] The engine's official docs for the EXACT pinned version were fetched and
      read (not remembered).
- [ ] The mechanic → engine-API mapping is written in GAMEDESIGN.md (each
      mechanic next to its verified API calls).
- [ ] If 3D: every required model category exists as downloadable free assets
      (verified by a quick search) before committing to 3D.

---

## Order of operations

Follow this sequence. Do not skip, do not reorder, do not merge phases.

1. **Phase 0 - Complete gameplay design**: write the ENTIRE game design in
   totality (all systems, rules, values, levels, edge cases, session flow)
   into GAMEDESIGN.md. Self-review with the design checklist.
2. **Phase 1 - Complete asset manifest**: identify EVERY asset the game needs
   (from the design), written as an exhaustive manifest in ASSETS.md, BEFORE
   searching for a single one. Cross-check with the design checklist.
3. **Phase 2 - Functional prototype**: bare scene, mechanic working, tested
   with micro-steps. Research patterns first, then build.
4. **Phase 3 - Tooling + architecture**: GitHub repo, package.json, config.js,
   state.js, levels.js skeleton, modular architecture for large scale.
5. **Phase 4 - Asset acquisition**: download, VISUALLY verify, license,
   fallback. One asset at a time against the manifest. Manifest marked
   complete at the end.
6. **Phase 5 - Screens**: build screen by screen with entry/exit checklists.
7. **Phase 6 - Playgama Bridge SDK**: full integration, moderation-required
   steps, safe without SDK.
8. **Phase 7 - Full content**: ALL levels implemented, beatable, complete
   progression, large-scale content complete.
9. **Phase 8 - Optimization gates**: load time, frame rate, memory, battery.
10. **Phase 9 - Zero-hallucination code audit**: every file re-read against
    the official API docs.
11. **Phase 10 - Verification battery**: automated + visual + manual,
    multi-layer, with the final acceptance gate.
12. **Delivery**: push final commit, write the delivery summary.

---

## Phase 0 - Complete gameplay design (written in totality)

**THIS IS THE FOUNDATION OF EVERYTHING. It is written BEFORE any code, any
asset search, any download.** You write the entire game design in totality —
not a summary, not a sketch: the complete, exhaustive specification of the
game. A design that misses a system produces a game that misses a system.

Write `GAMEDESIGN.md` covering ALL of the following sections. Do not skip any.
Every section must be filled with concrete, final numbers (never placeholders,
never "TBD", never "to be decided later" — decide now, you are the designer):

### 0.1 - Vision & concept
- Game title (catchy, short, English, unique).
- Genre (match-3, runner, merge, hyper-casual...).
- One-sentence pitch: "The player ___ by doing ___ to ___".
- Target platforms (Playgama network) and target audience.
- Session length target (1-3 min per run, 3-10 min per session).

### 0.2 - The dominant action & core loop
- The ONE dominant action in one sentence.
- The core loop as ONE sentence: `action → feedback → reward → repeat`.
- Why it is fun (the addiction hooks: escalation, combos, near-misses,
  visible goals).
- The "one more round" hook.

### 0.3 - Complete ruleset (exhaustive)
Write EVERY rule of the game. Every rule below that applies to your game MUST
have a final value written down:
- Movement rules (speed, acceleration, friction, gravity, jump force,
  jump height, max fall speed).
- Collision rules (what collides with what, result of each collision, one-shot
  vs continuous).
- Scoring rules (every scoring event, its points, combos, multipliers,
  thresholds).
- Win/lose rules (exact win condition, exact lose condition, fail states,
  recovery options).
- Spawn rules (spawn timing, positions, patterns, fairness guards).
- Life/energy rules (starting lives, loss/gain conditions, max).
- Power-up rules (each power-up: effect, duration, spawn rate, stacking).
- Combo/multiplier system (window, increments, reset).
- Difficulty curve (the difficultyFactor formula, what it scales, its min/max).
- Currency/economy rules (coins, prices, income sources, sinks).
- Unlock rules (what unlocks, when, conditions).
- Save/load rules (exactly what is persisted, when it is saved).
- ANY other system your game has — write its complete rules.

### 0.4 - ALL levels (the complete level list)
- Total level count (15-60 for a large-scale casual game; more if the design
  supports it).
- Level-by-level data table: for EVERY level — objectives, target values,
  new elements introduced, difficulty factors, star thresholds, unlock
  conditions.
- The complete progression curve (levels 1-3 gentle, then smooth rise).
- Level select behavior (layout, locks, stars, scrolling).
- The final level and the "Game Complete" flow.

### 0.5 - ALL entities & assets requirements
- Every entity (player, enemies, obstacles, collectibles, power-ups,
  breakables, NPCs) with: name, role, size in px, animation list, number of
  frames, required visual style.
- Every screen (loading, menu, level select, gameplay, pause, victory, game
  over, shop, settings, game complete) with: layout description, elements,
  required backgrounds.
- Every UI element (buttons with labels, HUD elements, icons, digits,
  popups) with: name, label text, states needed (normal/hover/pressed).

### 0.6 - Art direction (DA)
- Theme, mood, the 3-4 dominant palette colors (hex values), accent color.
- Visual style reference (flat cartoon, pixel, neon...) — described in words.
- The ONE asset pack family that will provide the core art (from research in
  Phase 1; if unknown yet, the style description so the pack can be chosen
  to match).
- Typography plan (display font + body font, Google Fonts).

### 0.7 - Audio direction
- Music: mood, tempo, style, ONE track reused everywhere (or per-screen mood
  plan if justified).
- SFX list: EVERY sound needed, each with its purpose and mood (collect →
  pop, jump → whoosh, victory → fanfare...).
- Volume balance plan (music ~0.3, SFX ~0.8).

### 0.8 - Monetization design
- Interstitial placements (exactly when: level complete, game over, menu
  return) with placement ids.
- Rewarded placements (exactly what each offers: extra life, double coins,
  revive, skip) with placement ids.
- Minimum delay between interstitials (default 60s, lower only with reason).
- Shop/bonus design if any.

### 0.9 - Edge cases (design them BEFORE they happen)
Write the designed behavior for EVERY edge case that applies:
- Tab switch / minimize (auto-pause).
- Ad opens during gameplay (pause + mute).
- Restart/replay (state resets).
- Storage unavailable or corrupt (defaults).
- SDK absent (graceful degradation).
- Very slow device (difficulty scaling).
- Asset load failure at runtime (error surface, no crash).
- Double-click on buttons (debounce).
- Rapid alternating input (input spam guard).
- The player idles (design what happens — do nothing is acceptable).
- Off-screen objects (cleanup/despawn).

### 0.10 - Orientation & resolution
- Orientation decision + rationale (portrait/landscape/both).
- Internal resolution (960x540 / 540x960) + the rotate prompt plan.

### 0.11 - Verification plan (design → test mapping)
For EVERY promised behavior in the design, write HOW it will be verified
later (automated assertion, screenshot, manual play). If a behavior has no
verification method, the design is not finished.

### Phase 0 exit checklist (design review — ALL must pass)

**Completeness:**
- [ ] Every section 0.1-0.11 exists in GAMEDESIGN.md with final values.
- [ ] Every rule in the game has a written number (no "TBD", no "later").
- [ ] Every level is listed with all its data (0.4 table is 100% filled).
- [ ] Every entity and asset requirement is listed (0.5 is exhaustive).
- [ ] Every edge case has a designed behavior (0.9 is exhaustive).
- [ ] Every promised behavior has a verification method (0.11).
- [ ] The core loop is one written sentence and is achievable in <10 seconds.

**Correctness (self-review, act as the strictest game designer):**
- [ ] Re-read the design: is ANYTHING impossible to implement in the chosen
      engine? Fix the design or the engine choice NOW.
- [ ] Is the difficulty curve fair at every point (no impossible level)?
- [ ] Would a player understand the goal within 30 seconds with zero
      explanation?
- [ ] Would YOU want to play this for 2 hours? If not, redesign the loop now.
- [ ] Are the monetization placements all at natural pauses (never
      interrupting gameplay)?
- [ ] Cross-check: every asset category in 0.5 maps to a gameplay need
      (no orphan assets, no missing assets).

Only when every box is checked do you proceed to Phase 1.

---

## Phase 1 - Complete asset manifest (before ANY search)

**THE MANIFEST IS WRITTEN BEFORE A SINGLE SEARCH OR DOWNLOAD.** From the
design (0.5), write `ASSETS.md`: the exhaustive list of EVERY asset the game
will need. This is your shopping list — you never search for an asset that is
not in the manifest, and you never finish Phase 4 with an asset in the
manifest that was not acquired.

### Manifest structure (ASSETS.md)

For EVERY asset, record: ID, category, filename (planned), format, target
size, description of required look, which gameplay need it serves, source
candidate (from research), license status, and a STATUS column: `pending →
found → verified → integrated`.

Categories (fill ALL that apply to your game):

- **Screens**: loading bg, menu bg, level select bg, gameplay bg, victory bg,
  game over bg, game complete bg, parallax layers, shop bg.
- **UI**: logo, play button (3 states), replay button, next button, resume
  button, bonus button, back button, level buttons (normal/locked/starred),
  pause button, mute button (if SDK-absent fallback), scoreboard, digits 0-9,
  hearts/lives icon, coin icon, lock icon, star icons, loader figure, HUD
  frames, popup frames ("+10", "Perfect!", "Combo!"), rotate-device overlay,
  favicon.
- **Sprites**: player (all animations × frames), enemies (all types ×
  animations), obstacles, collectibles, power-ups, breakables, decor elements,
  ground/platforms, mechanic pieces (blocks, tiles, gems...).
- **FX**: particles, explosions, sparks, glows, confetti, bobbing decor.
- **Audio**: music loop, EVERY SFX from 0.7 (collect, jump, click, victory,
  defeat, combo, coin, purchase, error...).
- **Fonts**: display font, body font.
- **3D (if applicable)**: every model .glb, textures, animations.

### Manifest cross-checks (MUST all pass)

1. **Design ↔ manifest**: walk 0.5 line by line — every entity/screen/UI
   element listed in the design appears in the manifest. Every manifest asset
   serves a design element. ZERO orphans, ZERO gaps.
2. **Manifest ↔ screens**: walk every screen of the design — each screen has
   its background, buttons, and HUD assets in the manifest.
3. **Manifest ↔ audio plan**: every SFX from 0.7 appears in the manifest.
4. **Manifest ↔ levels**: every entity introduced by any level (0.4) has its
   assets in the manifest.
5. **Manifest ↔ DA**: every asset entry describes the look matching the DA
   (0.6), so acquisition can be judged against it.

**Phase 1 exit checklist:**
- [ ] ASSETS.md exists with every category filled.
- [ ] Every asset has: ID, purpose, filename, format, size, look description.
- [ ] Cross-checks 1-5 all pass.
- [ ] Source candidates are listed for each asset family (from research).
- [ ] Every asset has a fallback plan (second source or alternate asset) —
      written BEFORE any search fails.

---

## Phase 2 - Functional prototype

Build a MINIMAL single scene with the engine (no menu, no loading, one plain
background, no downloaded art). Implement ONLY the core mechanic + input +
collision + scoring + win/lose + restart.

### Research before coding (MANDATORY - never code from memory)

1. **Run web searches** for the exact mechanic + engine, pick the canonical,
   well-optimized pattern.
2. Read the official docs / examples for your pinned version and **verify the
   API signatures** you will use. Copy the proven pattern, adapt it.
3. Note source URLs + license of each find in CREDITS.md immediately.

### Mechanic construction protocol (micro-steps, test each one)

1. **Represent the gameplay first** (already in GAMEDESIGN.md — re-read it,
   keep it next to you while coding).
2. **Feature 1**: input responds (mouse AND touch). Test 30 tries. Stop.
3. **Feature 2**: primary object reacts (jump/drag/merge/tap). Test 30 tries,
   no glitch. Stop.
4. **Feature 3**: obstacles/targets spawn correctly + fairly. Stop.
5. **Feature 4**: scoring + feedback. Stop.
6. **Feature 5**: win + lose, each triggering exactly once. Stop.
7. **Feature 6**: full restart resets to byte-identical state. Stop.
8. Only when ALL features pass do you add screens and art.

### Physics tuning starting values (never guess blindly)

| Archetype | Gravity (px/s²) | Jump force (px/s) | Move speed (px/s) |
|---|---|---|---|
| Jump / runner | 1400-1800 | 500-650 | 200-300 |
| Flappy-style | 1100-1500 | 300-420 | (world scroll) |
| Stack / drop | 1200-1600 | - | - |
| Match / merge | (no physics) | (no physics) | (pointer follow) |

Rules: gravity strong enough that falls feel snappy. Jump height ≈ `v² / (2·g)`
— verify the reachable height clears the intended obstacle. Always multiply
velocities by delta time (`dt`), never by frame count.

### The game loop - frame-rate independence (non-negotiable)

- Use the engine's delta time in `update(dt)`.
- All movement, spawn timers, cooldowns and tween durations scale with `dt`.
- Spawn timers: `spawnTimer += dt`, spawn when `spawnTimer >= interval`.

### Phase 2 exit checklist (ALL must pass — the prototype gate)

**Functionality:**
- [ ] Mechanic works on the FIRST run; first score moment within 10 seconds.
- [ ] Prototype is actually FUN, not just functional.
- [ ] Win and lose each trigger exactly once, exactly when they should.
- [ ] R returns to byte-identical initial state (verified, not assumed).
- [ ] Zero console errors and zero warnings during the whole prototype.
- [ ] Identical behavior at 30fps and 120fps (delta-time proven).
- [ ] Every objective from the user's description is DEMONSTRABLY met
      (tested with assertions, not "looks about right").

**Design fidelity:**
- [ ] The prototype matches GAMEDESIGN.md values (speeds, timings, rules) —
      any deviation was deliberately made and the design updated to match.
- [ ] The difficulty curve at prototype level feels fair.
- [ ] Edge cases from 0.9 that apply to the prototype (restart, input spam)
      behave as designed.

**Hygiene:**
- [ ] No console.log left in the prototype code.
- [ ] Every mechanic in the design has its implementation mapped (no design
      element missing from code, no code element missing from design).

If any box is unchecked: FIX THE PROTOTYPE FIRST. Only then move on.

---

## Phase 3 - Tooling + large-scale architecture

### 1. GitHub repo first

1. Check the repo does not already exist:
   `gh repo view <owner>/<repo-name> --json name` — if it errors, it does not
   exist yet. Never create a duplicate.
2. Create it: `gh repo create <repo-name> --public --source=. --push`.
3. Push each update as you go. The repo must never be more than a few commits
   behind the local work.

### 2. Setup template (pin exact versions)

```json
{
  "name": "my-casual-game",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "npx http-server . -p 8080 -c-1",
    "build": "node scripts/build.js",
    "verify-assets": "node scripts/verify-assets.js"
  },
  "dependencies": {
    "phaser": "3.87.0"
  },
  "devDependencies": {}
}
```

### 3. Large-scale architecture (the game must GROW without breaking)

A large-scale game needs an architecture that stays clean at 30 levels, 200
assets, and 40 files. Rules:

- **One file per concern**: config.js, state.js, levels.js (all level data),
  input.js, sdk.js, storage.js, audio.js (sound manager), fx.js (particle/
  popup helpers), ui.js (button/panel factories), one file per screen,
  main.js (boot + screen manager). No monolith files, no 2000-line files.
- **Data-driven content**: all levels, entities, spawn patterns, and shop
  items are DATA in dedicated modules (levels.js, content.js), never logic
  scattered in screens. Adding a level = adding a data row, not touching code.
- **One input manager** handling mouse + touch + tap/drag discrimination.
- **One state object** (state.js) as the single source of truth + resetState().
- **One storage adapter** (storage.js) used by ALL screens — never direct
  localStorage in screens.
- **One SDK wrapper** (sdk.js) — no SDK calls outside it.
- **One audio manager** (audio.js) — every sound goes through it (play,
  stop, mute, volume, single-music-slot).
- **Reusable UI factories** (ui.js): createButton, createPanel, createHUD —
  every screen uses the same factories so the whole game is visually
  consistent by construction.
- **Pooling layer** (fx.js + SpawnPool) for every repeated spawn.
- **Constants in config.js** — zero magic numbers anywhere.
- **Comments**: only where logic is non-obvious; self-documenting names.

### 4. Code foundations (write these BEFORE the screens)

`src/config.js` — every magic number lives here:

```js
export const CONFIG = {
  GAME_WIDTH: 960,
  GAME_HEIGHT: 540,
  GRAVITY: 1500,
  JUMP_FORCE: 560,
  PLAYER_SPEED: 240,
  SPAWN_INTERVAL_MS: 1400,
  COMBO_WINDOW_MS: 2000,
  COLORS: { BACKGROUND: 0x88ccee, PRIMARY: 0xffffff, ACCENT: 0xffaa00 },
};
```

`src/state.js` — single source of truth + reset:

```js
import { CONFIG } from "./config.js";

export const state = {
  score: 0, lives: 3, level: 1, combo: 0, best: 0, coins: 0,
};

export function resetState() {
  state.score = 0;
  state.lives = 3;
  state.level = 1;
  state.combo = 0;
}
```

On REPLAY: destroy every group, stop all timers/tweens/spawners, rebuild HUD,
then `resetState()` — all through ONE `startRound()`.

`src/input.js`:

```js
export class Input {
  constructor(scene) {
    this.pointer = scene.input;
    this.onDown = null; this.onUp = null; this.onDrag = null;
    this.pointer.on("pointerdown", (p) => {
      this.onDown?.(p);
      scene.sound.resumeAll(); // also unlock audio on first gesture
    });
    this.pointer.on("pointerup", (p) => this.onUp?.(p));
  }
}
```

Tap vs drag: <10px movement and <250ms = TAP, else DRAG. Never both.

`src/storage.js` — safe persistence (LOCAL fallback only; SDK storage takes
over when present):

```js
export function load(key, fallback) {
  try { const v = JSON.parse(localStorage.getItem(key)); return v ?? fallback; }
  catch { return fallback; }
}
export function save(key, value) {
  try { localStorage.setItem(key, JSON.stringify(value)); } catch { /* private mode */ }
}
```

### Phase 3 exit checklist (ALL must pass)

- [ ] Repo created on GitHub, first commit pushed.
- [ ] package.json pins exact versions; `dev`, `build`, `verify-assets`
      scripts work.
- [ ] The full file structure (config, state, levels, input, sdk, storage,
      audio, fx, ui, screens, main) exists as planned.
- [ ] config.js contains every constant the design needs (walk 0.3 values
      against config.js — none missing, none magic-numbered in code).
- [ ] All level data is in levels.js as DATA (skeleton ready for Phase 7).
- [ ] No screen contains SDK/storage/audio logic directly (all through
      adapters).
- [ ] `resetState()` + `startRound()` pattern established.

---

## Phase 4 - Asset acquisition with per-asset verification

Work through ASSETS.md item by item. **One asset at a time. Every asset goes
through the full verification cycle before the next one.**

### Asset sources (always download, check license)

- Kenney.nl — complete CC0 packs. FIRST choice (coherence by design).
- OpenGameArt.org — sprites, backgrounds, effects, audio.
- itch.io (filter "Free" game assets) — packs, sprite sheets.
- Game-icons.net — SVG icons for UI/HUD.
- CraftPix.net — free backgrounds and sprite packs.
- Google Fonts — typography (font-display swap).
- Freesound.org / ZapSplat.com / OpenGameArt audio — music and SFX.
- Three.js assets / PolyPizza / Kenney 3D (3D games only, .glb).

### The per-asset verification cycle (EVERY asset, in order)

1. **Manifest check**: is this asset in ASSETS.md? Does it match the planned
   purpose, look, size, format? If it does not match the plan, it is not
   acquired — find one that does.
2. **File-level check**: exists, non-empty (>1KB), correct extension (a 404
   page saved as `.png` is a runtime crash).
3. **VISUAL check (open the image and LOOK at it)**:
   - Right content (a coin sprite actually shows a coin)?
   - Transparency where expected (no white/black boxes around sprites)?
   - Art style coherent with the pack (same outlines, palette, mood)?
   - Resolution adequate for its on-screen size (2x for retina)?
   - Clean (no watermark, no artifacts)? Reject anything off.
4. **Proportionality check**: compare against the other assets of the pack —
   a 3D-rendered icon next to flat cartoon art fails coherence. Reject.
5. **License check**: CC0 (no credit needed) or CC-BY (credit in CREDITS.md).
   Unsure → choose another asset.
6. **Audio check** (for sounds): duration < 1s for SFX, OGG+MP3 dual format,
   no clipping/background noise (inspect with ffprobe or similar), music loop
   seamless.
7. **Update the manifest**: STATUS → `verified`, record source URL + license
   in CREDITS.md. The manifest is the single source of truth.

### Audio guidelines (matching mood, not just "some sound")

- Music matches mood: calm puzzle → soft ambient; runner → energetic synth;
  cute → playful; space → electronic. ONE track reused.
- SFX match the world: coin = metallic chime, fruit = squish, star = tinkle,
  jump = airy whoosh, victory = ascending fanfare, defeat = gentle low tone,
  buttons = one tiny click everywhere.
- Volume balance: music ~0.3, SFX ~0.8.

### Asset naming convention

```
screens/loading-bg.png, screens/menu-bg.png, screens/game-bg.png
ui/logo.png, ui/btn-play.png, ui/btn-play-hover.png, ui/btn-play-pressed.png
ui/loader-bar.png, ui/icon-heart.png, ui/icon-coin.png
sprites/player-idle.png, sprites/player-walk-1.png, sprites/player-walk-2.png
fonts/GameFont.ttf
audio/music-loop.ogg, audio/sfx-collect.ogg, audio/sfx-click.ogg
```

- `-hover` / `-pressed` for button states; `-1`, `-2` for animation frames.
- All lowercase, hyphens, no spaces, no accents.
- Sprite sheets: one sheet + frame config/atlas, never dozens of loose frames.

### Fallback plan (if a source fails)

REAL ASSETS ONLY, ALWAYS. Never generated, never code-drawn — not even as a
placeholder.

1. Use the fallback written in the manifest (second source, same family).
2. If still unavailable, pick a DIFFERENT asset from an available pack that
   fits the same DA (prefer changing the asset over breaking coherence).
3. If a required asset is still unobtainable: **STOP and warn the user** —
   list exactly which assets are missing, from which sources, and propose the
   concrete fix (e.g. "switching the theme to X lets me use pack Y"). Never
   substitute, never fake, never ship without it. Keep building everything
   that does not depend on it. The game is not delivered until every asset is
   real and verified.
4. **Never ship a game referencing a missing file** (verify-assets enforces).

### Performance budget (respect for weak mobiles)

- **Total game size < 8MB, ideally < 5MB.** Audio < 2MB. Music loop ~20-30s
  OGG ~128kbps.
- No single texture > 2048x2048. Texture atlas for many small sprites.
- Object pooling for repeated spawns. Cap particles (20-30 per effect).

### Phase 4 exit checklist (ALL must pass — the manifest gate)

- [ ] ASSETS.md: EVERY item has STATUS `verified` (100%, zero `pending`,
      zero `found`).
- [ ] Every asset was visually verified by looking at it (not assumed).
- [ ] CREDITS.md lists every asset with source URL + license (100% complete).
- [ ] Every asset file exists on disk, non-empty, in the right folder, named
      per convention.
- [ ] No asset outside the manifest was downloaded (no orphans).
- [ ] Fonts: display + body chosen, downloaded, license OK.
- [ ] Audio: music loop + every SFX from the plan present, OGG+MP3.
- [ ] Coherence spot-check: open 10 random assets and confirm same family
      style with the rest.

---

## Phase 5 - Screens (entry/exit checklists per screen)

Build screen by screen, in order: loading → menu → level select (if
level-based) → gameplay → pause → victory → game over → game complete (final).
For EACH screen, apply its entry and exit checklists. Never proceed to the
next screen before the current one passes its exit checklist.

**Common entry checklist (every screen, before starting):**
- [ ] The screen's design section (0.5) is re-read; the screen's asset list
      from ASSETS.md is extracted.
- [ ] Every asset it needs already has STATUS `verified` (Phase 4).
- [ ] The screen's purpose and player flow (where it comes from, where it
      goes) is written down.

**Common exit checklist (every screen, before moving on):**
- [ ] Runs without ANY console error or warning.
- [ ] VISUALLY verified: screenshot taken, LOOKED at, and judged — layout
      clean, hierarchy clear, nothing off-screen or overlapping.
- [ ] Buttons have label text + 3 states (normal/hover/pressed), debounced.
- [ ] All text English, no typos, coherent typography.
- [ ] Screen lifecycle: shutdown() cleans physics, groups, timers, tweens,
      input listeners (no leaks).
- [ ] Works at 320px width AND 4K, portrait AND landscape (primary
      orientation flawless, rotate prompt otherwise).
- [ ] Transitions to next screen work and back-transitions work.
- [ ] Reached this screen from EVERY path (menu → gameplay → pause → resume,
      victory → next level, game over → replay...) and every path verified.

### Screen by screen (build order)

**Screen 1 - Loading**: themed background, logo, themed loader figure (never
a browser progress bar), preload ALL assets, audio unlock on first gesture,
transition immediately when ready (1-3s max).

**Screen 2 - Main menu**: same theme as loading, logo reuse, PLAY button only
(sound toggle is SDK-provided), sober animated decor, no "How to play".

**Screen 3 - Level select** (level-based games): background, level grid
buttons (asset-based, numbered), stars on completed, locks on locked, BACK,
smooth scroll if many levels. The level list comes from levels.js data.

**Screen 4 - Gameplay + HUD**: the heart of the game — first 10 seconds must
teach the goal by watching. HUD minimal (score, pause; lives/coins only if
needed). Input responds on pointerdown. Collision callbacks fire ONCE.
Win/lose guarded by a single flag.

**Screen 5 - Pause**: overlay inside gameplay, RESUME only, auto-opens on
blur/visibility change (SDK requirement), CORRECT pause (physics, tweens,
timers all paused — verify frozen for 5 seconds).

**Screen 6 - Victory / Level complete**: celebration bg, stars 1-3 popping
with "ting", score + coins animated, confetti, NEXT LEVEL (main), REPLAY,
menu, BONUS (rewarded). Saves progress. Sends `level_completed`. Interstitial
at this natural pause.

**Screen 7 - Game over**: same DA, final score + best score, encouraging
message, soft defeat sound, REPLAY (always visible — publisher requirement),
BONUS (rewarded: revive/coins), menu.

**Screen 8 - Game complete** (after the LAST level): celebration, final
stats, replay / menu buttons. The game ends properly.

### Optional screens (only if the design needs them)

**Shop / Boutique** — cosmetic ONLY (never pay-to-win), currency in-game or
rewarded, menu-only access, asset-based grid, prices in digits, balance,
back, "Claim" rewarded button, purchase/error sounds.

**Settings** — only with a real reason (difficulty, language, vibration);
sound toggle stays SDK-provided.

### Exhaustive asset categories for the gameplay screen (fill ALL)

A. Backgrounds (full-screen, parallax layers, decor, floor/platforms).
B. Gameplay sprites (player + ALL animations, enemies, obstacles,
   collectibles, power-ups, mechanic pieces).
C. UI/HUD (score digits, icons, pause button, counters).
D. Visual FX (particles, explosions, sparks, popups, glows, confetti).
E. Complete audio (music + every action sound + victory + defeat + buttons).
F. Transition overlays (level complete, game over, pause panel).
G. Misc (favicon, shop sprites if any).

---

## Phase 6 - Playgama Bridge SDK integration

Playgama uses **Bridge SDK v2** (`bridge-js-core`), one SDK for all platforms.
SDK integration is **REQUIRED for submission** — a game without it is
rejected. In unsupported environments Bridge uses a mock platform: calls
return safe defaults instead of throwing.

**Official docs (verify everything against these — zero-hallucination rule):**
- Getting started: `https://wiki.playgama.com/playgama/bridge-sdk/getting-started.md`
- API reference: `https://wiki.playgama.com/playgama/bridge-sdk/api.md`
- Full index for agents: `https://wiki.playgama.com/playgama/llms.txt`

If you need to confirm ANY signature, FETCH these pages and read them.

### Setup

```
npm install bridge-js-core@<pinned version>
```

In `index.html`, before your game script (per the official setup docs):

```html
<script src="node_modules/bridge-js-core/bridge.js"></script>
<script src="node_modules/bridge-js-core/bridge.iife.js"></script>
```

### The SDK wrapper (`src/sdk.js`) — REQUIRED STEPS

```js
const sdkPromise = window.Bridge ? bridge.initialize() : Promise.resolve(null);

export async function init() {
  try {
    const sdk = await sdkPromise;
    if (!sdk) return null;
    // 1. REQUIRED - read platform language once after init (ISO 639-1)
    return sdk;
  } catch { return null; }
}
```

**Required step checklist (moderation-verified):**

1. **Wait for Bridge initialization** before any SDK API call.
2. **Localize with `bridge.platform.language`** — read once after init.
3. **Save/load progress via `bridge.storage`** — NEVER localStorage when the
   SDK is present (cloud saves):
   - Load on start: `bridge.storage.get(['level', 'coins', 'best'])`
     (missing keys → `null` → use defaults).
   - Save on meaningful change: `bridge.storage.set(['level', 'coins'], ['5', '120'])`.
     Never save every frame.
4. **Subscribe to pause and audio state events** (a game that keeps playing
   sound in the background fails moderation):
   - `bridge.platform.on(bridge.EVENT_NAME.PAUSE_STATE_CHANGED, isPaused => ...)`
   - `bridge.platform.on(bridge.EVENT_NAME.AUDIO_STATE_CHANGED, isEnabled => ...)`
   - On game start ALSO check the initial values: `bridge.platform.isAudioEnabled`
     (the event alone is not enough — it only fires on changes).
   - Wire into the pause overlay + audio manager (ONE universal handler
     covers ads, tab switches, system pauses).
5. **Send `game_ready`** when the first playable frame is ready:
   `bridge.platform.sendMessage('game_ready')`.
6. **Show interstitial ads at natural pauses** (level transitions, game over,
   menu return):
   - Check `bridge.advertisement.isInterstitialSupported` first.
   - `bridge.advertisement.showInterstitial(placement)` — placement id like
     `'level_completed'`.
   - Respect minimum delay (default 60s; `setMinimumDelayBetweenInterstitial`
     to lower).

**Recommended (better monetization — implement as many as make sense):**

- **Rewarded ads** (highest-eCPM format):
  - Check `bridge.advertisement.isRewardedSupported` — if false, HIDE the
    button (never a dead button).
  - Request from a direct player action:
    `bridge.advertisement.showRewarded('extra_life')`.
  - **CRITICAL: grant the reward ONLY on state `rewarded`, never `closed`:**
    ```js
    bridge.advertisement.on(bridge.EVENT_NAME.REWARDED_STATE_CHANGED, state => {
      if (state === 'rewarded') grantReward();
    });
    ```
    States: `loading`, `opened`, `closed`, `rewarded`, `failed`.
  - `bridge.advertisement.rewardedPlacement` decides which reward to grant.
- **Level lifecycle messages**: `level_started`, `level_completed`,
  `level_failed`, `level_paused`, `level_resumed` via
  `bridge.platform.sendMessage(msg, { world, level })`.
- **Leaderboards, achievements, daily rewards, player info** — if the design
  benefits. Verify signatures against the docs first.

### SDK rules

- Ads only at natural player moments — never mid-gameplay.
- While an ad is open: muted + paused (universal handler).
- Wrap EVERY SDK call in try/catch. 100% playable with no SDK present.
- BONUS button hidden when `isRewardedSupported` is false.

### Phase 6 exit checklist (ALL must pass — the SDK gate)

- [ ] All 6 required steps implemented (init, language, storage, pause/audio
      events, game_ready, interstitial) — verified against the fetched docs.
- [ ] Storage: progress loads before gameplay and saves on meaningful changes;
      NO localStorage usage when SDK is present.
- [ ] Initial audio state applied at start (not only the event).
- [ ] Rewarded reward granted ONLY on `rewarded` state.
- [ ] Game fully playable + silent-but-functional behavior with NO SDK in the
      page (tested: BONUS hidden, no crash, no console errors).
- [ ] Every SDK method used matches the official docs for the installed
      version (each signature cross-checked).
- [ ] Ads never interrupt gameplay (only natural pauses).
- [ ] `game_ready` sent once, after the first playable frame.

---

## Phase 7 - Full content: ALL levels, complete progression

The user wants a COMPLETE game — every level implemented, beatable,
progressive. A 1-level demo is a prototype. This phase completes the whole
content of the game per the design (0.4).

### Implementation

1. `src/levels.js` contains ALL level definitions as DATA (from 0.4):
   ```js
   export const LEVELS = [
     { id: 1, target: 100, spawnInterval: 1400, speed: 1.0, stars: [100, 150, 200] },
     { id: 2, target: 150, spawnInterval: 1250, speed: 1.1, stars: [150, 220, 300] },
     // ... every level
   ];
   export function getLevel(n) { return LEVELS[n - 1]; }
   ```
2. The gameplay screen reads everything from `getLevel(state.level)` — never
   hardcoded per-level numbers in code.
3. Level select renders from the LEVELS data (grid, locks, stars).
4. Unlock flow: sequential; progress persisted (SDK storage or fallback).
5. Victory flow: save progress, stars, coins, `level_completed`, interstitial,
   NEXT LEVEL. Last level → "Game Complete" screen.
6. Every level is HUMAN-BEATABLE and verified (play through each in the
   prototype logic before shipping). An unwinnable level is a functional bug.

### Phase 7 exit checklist (ALL must pass — the content gate)

- [ ] Every level from 0.4 exists in levels.js with ALL its data fields.
- [ ] Every level was played and completed successfully at least once
      (simulated playthrough where manual is impossible — assert win path
      works for each).
- [ ] No impossible configuration exists in ANY level (fairness verified per
      level, including edge levels).
- [ ] Unlock conditions, star thresholds, and rewards work for EVERY level
      (spot-check first, middle, last level).
- [ ] Progression persists across reload (SDK storage when present, fallback
      otherwise) — verified.
- [ ] Level select shows correct states (unlocked/locked/starred) after
      progress changes.
- [ ] Completing the last level shows the Game Complete screen (no crash, no
      empty loop).
- [ ] Difficulty curve matches the design (0.3) — level 1 gentle, smooth rise.
- [ ] No content promised by the design (worlds, bonus levels, endless mode,
      unlockables) is missing.

---

## Phase 8 - Optimization gates

A large-scale game with many assets and levels MUST be optimized. The game
must run at 60fps on weak phones with no hitches, load fast, and respect
battery.

### Optimization checklist (apply ALL that apply)

**Load:**
- Total size < 8MB (target < 5MB); audio < 2MB; no single texture > 2048px.
- Texture atlas for all small sprites (fewer draw calls, smaller requests).
- Audio: OGG+MP3 dual, music loop ~20-30s OGG ~128kbps.
- Fonts via Google Fonts with `font-display: swap`.
- No synchronous heavy work on the main thread at boot.

**Runtime (60fps):**
- Delta time everywhere; never frame-count.
- Object pooling for every repeated spawn (bullets, pickups, particles,
  obstacles). Never create/destroy per spawn.
- Cap particles: 20-30 per effect, ~40 max on screen. No unbounded confetti.
- Physics bodies disabled on pooled dead objects (`body.enable = false`);
  prefer `overlap` with callback over heavy colliders.
- No allocations in `update()`: pre-create reusable objects (Vector2,
  arrays, strings).
- Update text only when its value CHANGES, not every frame.
- `fps: { target: 60 }` in the engine config.
- Profile with DevTools Performance: frame-time spikes → find culprit
  (usually particles, unpooled spawns, per-frame text). Fix, don't accept.

**Memory/battery:**
- Screen lifecycle contract enforces cleanup (no leaks after 10 min of play
  — verify: play loops for 10 minutes, memory stable).
- Auto-pause on tab blur (no background CPU/battery burn).
- DPR handled by the engine (Phaser AUTO) — never force a high DPR.
- Cull off-screen objects (despawn beyond camera bounds).

### Phase 8 exit checklist (ALL must pass — the performance gate)

- [ ] Load completes in <15s on 3G simulation (DevTools throttling), ideally
      <5s on broadband.
- [ ] 60fps at desktop 1920x1080 AND 375x667 phone viewport.
- [ ] 60fps with DevTools CPU throttling 4x (weak phone simulation).
- [ ] No frame-time spikes in a 60-second profile (Performance panel).
- [ ] Memory stable across 10 minutes of continuous play (no leak growth).
- [ ] Asset size budget respected (measured total, not assumed).
- [ ] Pooling in place for every repeated spawn (audit: grep create/destroy
      in update paths).
- [ ] No per-frame allocations detected (audit update() code).
- [ ] Battery: no timers/tweens running while tab hidden.

---

## Phase 9 - Zero-hallucination code audit

Before the verification battery, a strict code audit:

1. **Every API call verified against official docs for the exact pinned
   version.** Re-check files against fetched docs (engine + Bridge SDK). Any
   call you cannot match to a documented signature → fix NOW. Never "it
   probably works".
2. **No stubs/TODO/pseudo-code/placeholder functions.** Grep
   `TODO|FIXME|XXX|placeholder|stub` — eliminate every hit.
3. **No dead code, unused variables, duplicated logic.** Extract, delete,
   deduplicate.
4. **No console.log in production.** No unhandled promise rejections — every
   promise has a `.catch`.
5. **Every asset path referenced exists on disk** (run verify-assets.js).
6. **Every mechanic promised in the design is implemented and reachable** —
   walk GAMEDESIGN.md 0.2-0.9 against the code, one by one.
7. **Level data integrity**: no out-of-range level, level 1 unlocked by
   default, last level → Game Complete.
8. **Config integrity**: no magic numbers outside config.js.

### Phase 9 exit checklist (ALL must pass)

- [ ] Audit items 1-8 all clean.
- [ ] Every API signature used was found in the official docs (list written
      in VERIFICATION.md with doc URLs).
- [ ] Zero `TODO`/`FIXME`/stub remnants in the source.
- [ ] The code compiles/parses with zero syntax errors (node --check or
      engine build) across all files.

---

## Phase 10 - Verification battery (multi-layer)

Three independent layers — each must pass. NEVER skip a layer because another
passed (cross-checking is the prudence doctrine).

### Layer 1 - Automated checks

1. Serve locally: `npx http-server . -p 8080 -c-1` (never verify from
   `file://` — broken paths, no audio).
2. `node scripts/verify-assets.js`: every referenced asset exists, non-empty,
   right extension. ZERO missing.
3. Headless smoke test (Playwright/Puppeteer if available):
   - Open page, wait for canvas, collect ALL console messages + page errors.
   - loading → menu → PLAY → gameplay input → assert score/HUD changed.
   - Trigger end condition or REPLAY; assert state reset (score = 0).
   - Re-run at 375x667 portrait AND landscape; assert UI on-screen and rotate
     prompt appears in non-primary orientation.
   - ANY error/unhandled rejection = FAIL, even if "it seems to work".

### Layer 2 - Visual verification (your multimodal power)

1. **Screenshot EVERY screen** (loading, menu, level select, gameplay,
   pause, victory, game over, game complete) at desktop AND 375x667, portrait
   AND landscape.
2. **OPEN each screenshot and LOOK at it. Judge like a senior dev:**
   - Anything off-screen, overlapping, or misaligned?
   - Buttons: labels visible, 3 states correct?
   - Palette/style coherence across screens?
   - English text only, no typos, no overlapping text?
   - HUD readable; pause truly frozen; victory/defeat complete?
   - Does each screen hold up next to a top casual game of the genre?
3. Fix everything you SEE. Re-screenshot. Re-look. Repeat until clean.

### Layer 3 - Manual final verification (MANDATORY, line by line)

Re-read ALL code and test every flow:

1. Loading → menu; sound unlocks on first click.
2. Menu PLAY starts the game; audio works.
3. Gameplay: mechanics, score, HUD; zero console errors.
4. Pause: manual, auto (tab switch), resume; fully frozen while paused.
5. Victory: stars, rewards, NEXT LEVEL; progress saved.
6. Game over: score, best, REPLAY clean restart (state resets to zero).
7. ALL levels beatable end to end (full progression tested).
8. Resize + rotate mid-game: nothing breaks.
9. Console ZERO errors + ZERO warnings.
10. All text English. CREDITS.md 100% complete.
11. Progress survives reload (SDK present AND SDK absent).
12. Game runs with SDK absent (BONUS hidden, no crash).
13. 320px phone: everything reachable and tappable (touch targets ≥44px).
14. Both orientations at 375x667 and 768x1024.
15. Button labels + hover/pressed everywhere; no textless buttons.
16. CPU throttling 4x: smooth, no pile-ups, no stutter.
17. **User objectives met**: re-read the original description, play the game
    against it — every promised behavior happens. Verified by playing, not
    assumed.
18. **Design fidelity**: walk GAMEDESIGN.md — every system from 0.2-0.9
    behaves as written.

### Phase 10 exit checklist (the verification gate)

- [ ] Layer 1: all automated checks PASS.
- [ ] Layer 2: every screenshot reviewed and approved (no visual defect).
- [ ] Layer 3: all 18 manual checks PASS.
- [ ] Zero console errors, zero warnings, zero missing assets, zero TODO.
- [ ] All levels beatable; progression complete; game ends properly.
- [ ] VERIFICATION.md written: records every check done, every screenshot
      reviewed, every fix applied (a human should trust your report).

---

## Anti-AI-look checklist (verify ALL)

1. **Assets from ONE pack**: same style, same proportions, no mixing of 3D +
   pixel + cartoon in one game.
2. **Homogeneous restricted palette**: 3-4 dominant colors on every screen.
3. **Perfect texts**: correct English, no typos, no placeholders, catchy
   short titles, coherent typography.
4. **Professional UI**: hover/pressed button states, grid alignment, clean
   layout, nothing overlapping randomly.
5. **Lively but sober animations**: eased movements (no robotic linear
   motion), micro-interactions (score pop, button pulse), measured particles.
6. **Complete audio**: music + action sounds + feedback. A silent game is a
   dead giveaway.
7. **Global visual coherence**: same background style/theme on every screen;
   logo, buttons, loader style everywhere.
8. **Human details**: object shadows, decor variants, one subtle easter egg,
   smooth difficulty progression.
9. **Technical professionalism**: constant 60fps, no jank, responsive, fast
   load, no console errors.

---

## Professional screen design bar

### Layout & composition (per screen)

1. **Clear hierarchy**: background → decor → panel → content → action
   buttons. Nothing competes with the main action button.
2. **Grid and alignment**: shared grid, consistent margins, centered columns,
   equal spacing. Nothing placed "roughly".
3. **Breathing room**: margins ≥5% of width; no element touches an edge; no
   overlaps.
4. **Center bias**: menu centers its block; HUD hugs corners; gameplay fills
   the middle.
5. **Visual rhythm**: one focal point per screen; secondary buttons smaller
   but ≥44px tappable.

### Buttons

1. Every button = asset background + ENGLISH text label (game font, centered,
   drop shadow/outline). No textless buttons.
2. Short all-caps/title case labels ("PLAY", "REPLAY", "NEXT"), consistent
   size per role, consistent color.
3. Three visible states: normal / hover (brighter, 1.05) / pressed (darker,
   0.95).
4. Same style family everywhere (same frame asset, same corners).

### HUD & typography

1. Anchored HUD (score top-center/left, pause top-right, coins top-left);
   never moves/overlaps on resize.
2. Asset icons + asset digits or game font with outline/shadow.
3. ONE display font + ONE body font MAX (Google Fonts, swap). Same sizes
   reused — never 12 random sizes.

### Depth & polish

1. Subtle drop shadows under panels/buttons — flat-on-flat is the AI look.
2. Backgrounds never plain: themed art + subtle animated decor (assets).
3. Consistent decorative style on every screen (same ornaments, same frames).
4. Micro-interaction on the main button (gentle pulse) invites the tap.

### The "compare" test (per screen)

"Screenshot this screen next to a top casual game in the same genre — would
it hold up?" If it would embarrass you next to a real game, fix it now.

---

## Responsive & mobile correctness

### Full responsive (broken scaling is a rejection)

- Adapts to ANY screen (320px phones to 4K), BOTH orientations, primary per
  GAMEDESIGN.md.
- Responsive ONLY when every UI element stays on-screen, readable, tappable
  at every size — not merely "the canvas scales".

Rules (Phaser example, adapt per engine):

1. **Fixed internal resolution** (960x540 / 540x960) + `scale.mode = FIT` +
   `autoCenter = CENTER_BOTH`.
2. **Never hardcode positions**: compute from `this.scale.width/height`;
   `layoutElements()` per scene, re-run on resize.
3. **Recompute on resize/orientation change**:
   ```js
   this.scale.on("resize", (gameSize) => {
     this.cameras.main.setViewport(0, 0, gameSize.width, gameSize.height);
     this.layoutElements();
   });
   ```
4. **Rotate-device prompt** for non-primary orientation (icon asset + message,
   same DA); game keeps working in both.
5. **Relative sizing**: fonts/buttons from `this.scale.width`
   (e.g. `Math.max(16, this.scale.width * 0.03)`).
6. **Safe area**: 20px margins; 40px top/bottom on notched phones.
7. **Touch targets ≥44x44px** (60px main button).
8. Verify at 320x480, 375x667, 768x1024, 1920x1080, 4K — both orientations.
   Any off-screen/overlapping/unreachable element = FAIL.

### Browser/mobile gotchas (real bugs if ignored)

- viewport meta: `width=device-width, initial-scale=1, maximum-scale=1,
  user-scalable=no`; `touch-action: none` on canvas.
- **Audio unlock** on first gesture.
- **iOS Safari**: no `height: 100vh` container; disable long-press
  (`-webkit-touch-callout: none`, `user-select: none`).
- **WebGL context loss**: `game.events.on('contextlost', e => e.preventDefault())`.
- **Auto-pause** on `visibilitychange`/`blur`; no timers firing while hidden.
- **DPR** handled by the engine (Phaser AUTO).

---

## Code quality

CODE QUALITY IS NON-NEGOTIABLE. Rules:

- Architecture: one file per concern; no 2000-line monoliths.
- Separation of concerns: display, logic, data never mix.
- No dead code, no unused variables, no duplicated logic.
- Constants in config.js; zero magic numbers.
- Consistent naming: camelCase vars, PascalCase classes, SCREAMING_SNAKE
  constants, descriptive names.
- No global state pollution (modules/closures; never `window`).
- One input manager (mouse + touch).
- State reset via `resetState()` + one `startRound()`.
- Memory hygiene: screen lifecycle contract (destroy, remove, stop).
- Edge cases handled: asset failure (surface + stop cleanly, never crash,
  never fake), tab blur, resize, slow devices, repeated clicks.
- No console spam, no unhandled rejections, every promise has `.catch`.
- Readability: a future you understands the code without the author.

---

## Final acceptance gate

The game is ACCEPTED (ready for delivery) ONLY when ALL of the following
checklists are 100% green — run through them one final time in order:

- [ ] Phase 0 exit checklist (design complete, reviewed).
- [ ] Phase 1 exit checklist (manifest complete, cross-checked).
- [ ] Phase 2 exit checklist (prototype gate).
- [ ] Phase 3 exit checklist (architecture gate).
- [ ] Phase 4 exit checklist (manifest gate — 100% assets verified).
- [ ] Phase 5 exit checklists (every screen passed entry+exit).
- [ ] Phase 6 exit checklist (SDK gate).
- [ ] Phase 7 exit checklist (content gate — all levels).
- [ ] Phase 8 exit checklist (performance gate).
- [ ] Phase 9 exit checklist (zero-hallucination audit).
- [ ] Phase 10 exit checklist (verification battery: 3 layers).
- [ ] Anti-AI-look checklist: all 9 points.
- [ ] Professional design bar: every screen passes the compare test.
- [ ] Responsive: all sizes/orientations verified.
- [ ] Code quality: all rules respected.
- [ ] VERIFICATION.md written and complete.

If ANY box is unchecked: fix it, re-verify, and only then deliver. The game
is NEVER delivered with a known defect — "I'll fix it later" does not exist.

---

## Delivery

1. Complete the Final acceptance gate; fix everything until 100% green.
2. Push the final commit to GitHub (all files: source, assets, GAMEDESIGN.md,
   ASSETS.md, CREDITS.md, VERIFICATION.md, README).
3. Write the delivery summary for the user:
   - Game title, genre, theme, engine + version, orientation.
   - The complete feature list: every mechanic, every screen, ALL N levels,
     monetization (interstitial placements, rewarded placements), SDK
     integrations (storage, language, game_ready, pause/audio).
   - Asset sources with licenses (summary; full list in CREDITS.md).
   - How to run locally (`npm install && npm run dev` → localhost:8080).
   - GitHub repo URL.
   - Verification report: which automated checks, which screenshots were
     reviewed (and the fixes they produced), checklist results.
4. Remind the user of the Playgama submission step: package the game folder
   into a ZIP, submit at developer.playgama.com — the SDK is already
   integrated and all moderation-required steps are done.

The game is delivered ONLY when: every gate is green, every level is beatable,
zero console errors, zero missing assets, every screen visually verified by
YOU looking at the screenshots, and the repo is pushed.
