---
name: casual-game-builder-engine
description: Loaded by casual-game-builder at the engine, screens and code phases. Choose and boot the engine (never vanilla), build the 6 screens one at a time with POLISH TO THE PRO BAR + LAYOUT GUARDS (no overlapping UI), the responsive test matrix, code quality rules, battle-tested code templates (config, state, input, storage, main, pooling, scene lifecycle), the ZERO-HALLUCINATION code guard (every API verified, every asset path verified, automated audit scripts), automated pre-delivery tests (Playwright smoke suite + performance/leak gates) and the PROGRAMMING ITERATION. Pass Gates B, D and E.
---

# Casual Game Builder - Engine, Screens & Code

This sub-skill covers the engine choice, the screen-by-screen build, the
responsive matrix, and the code quality rules. It is loaded by the orchestrator
skill at phases 2 (engine), 4 (screens) and 6 (code quality). Golden rules 5
(asset buttons), 7 (never hallucinate), 8 (never code against missing assets),
9 (never guess engine APIs), 14 (engine before code), 15 (run before claim)
always apply.

---

## 1. Choose the engine

**ENGINE FIRST - this step happens BEFORE any game code exists.** It is a hard
order, not a preference: pin the engine and verify it RUNS before writing a
single feature (golden rule 14). If you ever catch yourself about to write
game logic in raw vanilla canvas/DOM, STOP - that is how broken games are made
(no scene management, no delta time, no input abstraction, no asset loader, no
state reset, no physics). Choose the engine best adapted to the gameplay:

- **Phaser** - best all-around for 2D casual games (sprites, physics,
  animations, input, audio all built-in). The default choice for 90% of
  casual games.
- **PixiJS** - pure 2D rendering, lighter, when you need maximum performance
  and write your own logic.
- **Three.js** - 3D games only. Most casual games do NOT need 3D.
- **Plain Canvas** - NEVER for a deliverable game. Only acceptable for a
  throwaway 5-minute prototype sketch, and even then Phaser is faster.

### Engine version rules

1. **Use the most STABLE version, never the newest.** Example: if Phaser 3.87
   is the latest but 3.80 is the one with proven ecosystem support, use 3.80.
   Check the official docs for the version you actually install.
2. **Pin the exact version** in package.json (no `^` or `~`): `"phaser":
   "3.80.0"`.
3. **Verify how the target mechanics are integrated** in the chosen engine
   before coding: read the official docs/examples for your mechanic (e.g.
   Phaser arcade physics for jumping, input events for dragging). Do not guess
   the API from memory - look it up, then implement.
4. **BOOT CHECK (mandatory)**: right after scaffolding, launch the game with
   an empty scene and confirm it boots with ZERO console errors BEFORE adding
   any feature. A game that cannot boot at step one is a game that will be
   delivered broken.

### Project structure (standard layout)

```
game/
├── assets/
│   ├── screens/   → per-screen backgrounds (loading, menu, game, end...)
│   ├── ui/        → logo, buttons, HUD icons, loader
│   ├── sprites/   → characters, gameplay elements
│   ├── fonts/     → typographies
│   └── audio/     → music + sounds
├── src/
│   ├── screen-1.js   (loading)
│   ├── screen-2.js   (menu)
│   ├── screen-3.js   (gameplay + pause overlay)
│   ├── screen-4.js   (victory / game over)
│   ├── sdk.js        (Playgama Bridge wrapper - monetization skill)
│   └── main.js       (boot, screen manager)
├── ASSETS.md         (machine-verifiable manifest: sha256, source, approval)
├── CREDITS.md        (every asset + license)
└── index.html
```

### The backgrounds matter the most - hunt them with real effort

The background is the largest and most visible asset on every screen - it is
60-80% of what the player sees. A weak background makes the whole game look
weak. Rules:

1. There is ONE themed background per screen, all from the SAME pack and the
   SAME DA: `screens/loading-bg`, `screens/menu-bg`, `screens/game-bg`,
   `screens/victory-bg`, `screens/gameover-bg`. Continuity between screens is
   what makes a game feel professional.
2. Full-screen resolution: at least the internal resolution (960x540 or
   540x960), ideally 2x for retina. A blurry stretched background is a defect.
3. VISION-approve each one: it must clearly show the theme, be rich enough to
   not look empty/plain, and NOT fight with the UI (busy backgrounds get a
   subtle dark overlay under the HUD).
4. Never reuse a gameplay background for the menu or vice versa - each screen
   gets its own, designed for its mood (menu = inviting, victory =
   celebratory, game over = soft, never dead/dark).

### Study the real hits (web research)

Before designing the screens, SEARCH the web for the actual top hits of the
target genre and study what they DO - not in theory, in practice:

1. `best casual mobile games <genre>` / `most addictive casual games`
   / `game design of <hit title>`. Read how they structure their menu, their
   gameplay screen, their onboarding, their retention hooks.
2. Note down the CONCRETE techniques they use (first-10-seconds onboarding,
   session length, meta loop, difficulty curve, reward cadence) and the visual
   language of their screens (layered UI, big readable numbers, bold colors,
   clear focal point).
3. Apply these techniques to YOUR game - do not invent a new standard, match
   the proven one. The goal is a game that could sit next to these hits on a
   store page and not look out of place.

---

## 2. Build screen by screen, ONE at a time, fully

Work on ONE screen at a time and finish it completely before touching the
next: hunt its assets, build it, verify it, pass its gate. NEVER work on
several screens in parallel, NEVER start a screen with a missing asset "to be
filled in later", NEVER leave a screen half-done. A missing asset is a
blocker, not a TODO.

For EACH screen, in this exact order:

1. **Write the screen's COMPLETE asset needs list** (from the GAMEDESIGN asset
   list): every background, every button state, every icon, every sprite, FX
   and sound that this screen uses. Nothing is added later "as a surprise".
2. **Hunt every asset** (per-screen asset hunt, assets skill): search with the
   method, download, verify on disk, VISION-approve each one. Do NOT start
   building until ALL of them exist and are approved.
3. Sort them into the correct folders.
4. Integrate them in the code.
5. Build the screen.
6. Verify: run it, VISION-check it, check for errors.
7. Pass the screen's Gate D - only then move to the next screen.

Never proceed to the next screen before the current one is done and verified.

### Mandatory screens (6)

**Screen 1 - Loading**

- **TWO-PHASE LOADING - this is its exact function**:
  - **Phase A (fraction of a second)**: load ONLY the loading screen's own
    assets FIRST - its background, its logo and its loader figure. Nothing
    else. This guarantees the screen appears instantly.
  - **Phase B (while visible)**: the loading screen is now on screen with its
    themed loader. From here, preload ALL the remaining assets of the game
    (menu, gameplay, victory, sprites, UI, FX, audio) and fill the progress
    bar as they arrive.
- Background: `assets/screens/loading-bg` (themed, from the game's DA).
- Logo: `assets/ui/logo` (asset or big title using an imported Google Font).
- Loader: `assets/ui/loader` - a THEMED loader figure (e.g. a star that fills,
  a fruit that grows). NEVER a default browser progress bar.
- **The progress bar MUST visibly fill from 0% to 100%** as the Phase B
  assets load - wire it to the engine's `progress` event, do not fake it:
- Code: `src/screen-1.js`.

```js
// Phase A: in preload(), load ONLY this screen's own assets so it appears
// instantly.
preload() {
  this.load.image("loading-bg", "assets/screens/loading-bg.png");
  this.load.image("logo", "assets/ui/logo.png");
  this.load.image("loader-fill", "assets/ui/loader.png");
}

// Phase B: in create(), the screen is visible with its themed loader. Load
// every remaining asset and fill the progress bar as they arrive.
create() {
  this.load.on("progress", (v) => this.loaderFill.setScale(v, 1)); // bar fills 0->100%
  this.load.on("complete", () => this.scene.start("Menu"));
  this.load.image("menu-bg", "assets/screens/menu-bg.png"); // menu assets...
  this.load.image("game-bg", "assets/screens/game-bg.png"); // gameplay...
  this.load.image("player", "assets/sprites/player.png");   // sprites...
  this.load.audio("bgm", "assets/audio/bgm.mp3");           // audio...
  // ... every other asset of the game
  this.load.start();
}
```

- Keep the screen visible at least ~1 second so the player actually SEES the
  branded loading screen (publishers require it); if assets are already
  cached and load instantly, still hold a minimum ~0.5-1s before switching.
  If loading is slower, keep showing the animated themed loader until ready -
  but never block the player for a long time (keep the total load as short as
  possible).
- Role: after Phase B completes, transition to the menu.
- **Audio unlock on this screen**: register one `pointerdown`/`keydown`
  listener that resumes the AudioContext (see Input template, section 4).
  Browsers block autoplay - without this the whole game is mute.

**Screen 2 - Main menu**

- Background: same theme as loading screen (continuity is essential).
- Logo: reuse `assets/ui/logo` from screen 1 (brand coherence).
- Buttons: PLAY only (→ game). The sound toggle is provided by the platform
  SDK button (top corner of the page), so do NOT duplicate it.
- Optional: subtle animated decor (falling leaves, moving clouds...) using
  assets - only if the theme allows it, keep it sober.
- NO "How to play" button.
- Code: `src/screen-2.js`. Buttons use the asset images with hover/pressed
  states (swap image on pointerover/pointerdown).

**Screen 3 - Gameplay + HUD**

- The heart of the game. Use the EXHAUSTIVE asset list (assets skill).
- HUD is MINIMAL: score at top (asset digits), pause button top-right. Only
  add lives/coins if the mechanic needs them.
- Code: `src/screen-3.js` (game loop, physics, spawn, score, mechanics,
  pause overlay).
- The FIRST 10 seconds of gameplay are the most important: the player must
  immediately understand the goal by watching (one clear action, immediate
  feedback, small win quickly).

**Screen 4 - Pause**

- Anti-frustration + required by SDK (auto-pause when the tab loses focus).
- Overlay on top of gameplay (inside screen-3.js), NOT a separate screen.
- Assets: themed pause background (soft blur/overlay), title "Pause", ONE
  button: RESUME.
- Sound on open/close (subtle).
- NO replay button, NO main menu button here (decided: keep it minimal).
- Must also trigger automatically on `blur`/visibility change (SDK
  requirement), and resume on focus with the same flow.

**Screen 5A - Victory / Level complete**

- Victory background asset (celebration, same DA).
- Stars: 1-3 depending on performance, with a "star collect" animation (they
  pop in one by one with a "ting" sound each).
- Score displayed with asset digits; rewards (coins) with an animated counter.
- Confetti asset animation.
- Title: "Level Complete!" / "Victory!" (asset or font).
- Victory fanfare sound.
- Buttons: CONTINUE / NEXT LEVEL (MAIN action, most prominent), BONUS
  (rewarded ad, optional), replay level, main menu.

**Screen 5B - Game Over**

- Game over background (same DA, not dark/dead - keep the game's charm).
- Final score + best score in asset digits.
- Encouraging visual message (e.g. cheerful character), NO punishment.
- Soft, non-frustrating defeat sound.
- Buttons: REPLAY (MAIN action, always present and visible - publisher
  requirement), BONUS (rewarded ad: revive/coins/second chance, optional),
  main menu.

### Optional screens (only if the AI decides the gameplay needs them)

**Shop / Boutique**

- Cosmetic ONLY. Never pay-to-win (forbidden on these platforms).
- Currency earned in-game or via rewarded ad, never real purchases.
- Accessible from the main menu only, never from gameplay.
- Elements: shop background, "Shop" title, item grid (assets), prices (digits
  + currency icon), player balance at top, back button, "Claim" button for a
  free rewarded reward.
- Sounds: purchase confirmation, "not enough coins".

**Settings**

- NOT necessary in general: the sound toggle is SDK-provided and casual games
  don't include a settings screen (less screens = less friction).
- Only add it if the game has a REAL reason: difficulty selection, language,
  vibration toggle...

---

## 3. POLISH TO THE PRO BAR - every screen must look like a real mobile hit

The benchmark is NOT "a working game" - it is a screenshot of a top casual
mobile game (Candy Crush, Subway Surfers, Merge Mansion, Township, Royal
Match). Ask, for EVERY screen: "would a player mistake this for a real
released game, not a web demo?" The gap between a demo and a hit is exactly
this polish. Rules:

1. **Take REAL time per screen - polish is a per-screen step, not a global
   pass at the end.** After a screen works, do a dedicated POLISH pass on it
   BEFORE moving on (Gate D). A screen is not done when it functions - it is
   done when it looks designed.
2. **Every screen must have visual DEPTH, never flat**: layered background
   (3+ planes), soft shadows under every floating UI element and button, a
   subtle vignette or gradient overlay, rounded panel frames with borders,
   depth through size and lighting. Flat flat colors with no shadow = demo.
3. **Everything is EASED and ALIVE**: every menu element enters with an eased
   animation (fade+slide+scale with an ease curve and slight stagger), the
   logo breathes, buttons pulse subtly on hover, the background ambient
   elements drift. Nothing static, nothing linear, nothing robotic.
4. **Typography is designed**: title in the imported display font, hierarchy
   of sizes, letterspaced uppercase micro-labels, text never touching edges,
   drop shadows on text over busy backgrounds.
5. **Transitions between screens are a moment**: a short themed transition
   (fade through the game's palette, the logo scaling in), 200-400ms, eased.
   Screen changes are part of the polish, not a hard cut.
6. **Micro-interactions**: button hover/pressed states, score popups that
   scale+fade, combo meters that glow at max, HUD elements that react to
   events. Every touch of the screen must get a visual answer.
7. **Consistent UI language**: same button style, same panel style, same
   stroke, same corner radius on every screen - a design SYSTEM, not
   per-screen improvisation.
8. **VISION comparison (mandatory)**: capture the screen and LOOK at it as if
   comparing to a real mobile hit. Ask: "is this at the bar of a released
   game?" Any screen that still reads as "web demo" goes back to polish - more
   shadows, more easing, more layering, better spacing - until it passes.
9. **LAYOUT GUARDS - no overlapping UI, ever.** The pause button, HUD bars,
   labels, score and notifications must NEVER overlap. This is a build rule,
   not an afterthought:
   - Reserve dedicated zones per element (top-left score, top-right pause,
     top-center bars, bottom-center actions) and assert no two interactive
     elements intersect.
   - Bounding-box check: for every pair of HUD/interactive elements, verify
     their rectangles do not intersect (unless the overlap is an INTENDED
     overlay like a popup panel).
   - Any element that grows (health bar, exp bar, combo meter) must grow
     INSIDE its own reserved space, never under another element.
   - VISION-check this at every resolution of the test matrix - an overlap
     that fits at 960x540 can appear at 375x667.

---

## 4. Code quality (NON-NEGOTIABLE)

CODE QUALITY IS NON-NEGOTIABLE - it is what separates a professional game
from a prototype. A broken or sloppy game is immediately rejected by
publishers and players. Rules:

- **Architecture**: one file per screen (screen-1.js ... screen-4.js), a
  screen manager (main.js), an SDK wrapper module (sdk.js), a config module
  (config.js). No 2000-line monolith files.
- **Separation of concerns**: display code, game logic and data never mix.
  The game state (score, lives, level) lives in its own state object, not
  scattered across screen code.
- **No dead code, no unused variables, no duplicated logic.** If two places
  do the same thing, extract a function. If a variable is never used, delete
  it.
- **Constants at the top of files**: speeds, sizes, colors, multipliers,
  timings. Never magic numbers buried in the middle of a function.
- **Consistent naming**: camelCase for variables/functions, PascalCase for
  classes, SCREAMING_SNAKE for constants. Descriptive names: `playerSpeed`,
  not `ps`.
- **No global state pollution**: use modules or closures; do not leak
  variables to `window`.
- **Input handling**: a single input manager that handles BOTH mouse and
  touch, so gameplay logic never cares which device the player uses.
- **State reset on restart**: when REPLAY is pressed, every piece of state
  must reset (score = 0, positions, timers, spawners, HUD). A bug where the
  second round starts with the previous score is a fatal quality issue.
- **Memory hygiene**: destroy game objects, remove event listeners and stop
  tweens/timers when leaving a screen, or the game leaks and slows down after
  10 minutes of play.
- **Handle edge cases**: assets failing to load (show fallback, never crash),
  tab losing focus (auto-pause), browser resize (recompute scale), very slow
  devices (lower spawn rate), repeated clicks on the same button (debounce,
  no double-trigger), first-time players (auto-start after the menu).
- **No console spam**: no console.log left in production code, no unhandled
  promise rejections, no unhandled exceptions. Every promise has a .catch.
- **Readability**: someone (a future you) must understand the code without the
  author present. Self-documenting names, short focused functions, comments
  only where the logic is non-obvious.

### Code templates (battle-tested foundations - adapt, do not reinvent)

These are the proven patterns for the risky 20% of the code. Copy them and
adapt to the theme - never rewrite architecture from scratch each game.

`src/config.js` - every magic number lives here, never inside a function:

```js
export const CONFIG = {
  GAME_WIDTH: 960, GAME_HEIGHT: 540,          // matches the DECIDED orientation
  GRAVITY: 1500, JUMP_FORCE: 560, PLAYER_SPEED: 240,
  SPAWN_INTERVAL_MS: 1400, COMBO_WINDOW_MS: 2000,
};
```

`src/state.js` - the single source of truth + reset (the replay-reset bug is
fatal; make it impossible):

```js
export const state = { score: 0, lives: 3, level: 1, combo: 0, best: 0 };
export function resetState() {
  state.score = 0; state.lives = 3; state.level = 1; state.combo = 0;
}
```

On REPLAY the scene must also destroy all groups, stop all timers/tweens/
spawners and rebuild the HUD - all inside ONE function `startRound()`, never
scattered code. Then `resetState()`.

`src/input.js` - one manager for mouse AND touch, and it unlocks audio on the
first gesture:

```js
export class Input {
  constructor(scene) {
    scene.input.on("pointerdown", (p) => {
      this.onDown?.(p);
      scene.sound.resumeAll();               // audio unlock (browsers block autoplay)
    });
    scene.input.on("pointerup", (p) => this.onUp?.(p));
  }
}
```

Tap vs drag: record the down position; on up, movement < 10px and < 250ms =
TAP, otherwise DRAG. Never let both fire. Buttons respond on `pointerdown`
for instant feel, and are debounced (ignore a second press within 400ms).

`src/storage.js` - safe persistence (private mode throws, never crash):

```js
export function load(key, fallback) {
  try { const v = JSON.parse(localStorage.getItem(key)); return v ?? fallback; }
  catch { return fallback; }
}
export function save(key, value) {
  try { localStorage.setItem(key, JSON.stringify(value)); } catch { /* private */ }
}
```

Save the best score the moment it is beaten, not only on the end screen.

`src/sdk.js` - Playgama Bridge v2 wrapper (see the MONETIZATION skill for the
full verified template). Every bridge call wrapped in try/catch.

`src/main.js` - boot + screen manager (no 2000-line monolith):

```js
const config = {
  type: Phaser.AUTO,
  width: CONFIG.GAME_WIDTH, height: CONFIG.GAME_HEIGHT,
  scale: { mode: Phaser.Scale.FIT, autoCenter: Phaser.Scale.CENTER_BOTH,
           orientation: Phaser.Scale.LANDSCAPE },   // per GAMEDESIGN.md
  fps: { target: 60 },
  physics: { default: "arcade", arcade: { gravity: { y: CONFIG.GRAVITY } } },
  scene: [LoadingScene, MenuScene, GameScene, EndScene],
};
new Phaser.Game(config);
```

Screen lifecycle contract (prevents leaks and ghost objects): on `shutdown()`
every scene must pause physics, destroy all groups (`group.clear(true,true)`),
remove all timers (`time.removeAllEvents()`), kill all tweens
(`tweens.killAll()`), and remove every input listener it registered. Shut the
old scene down BEFORE starting the new one.

Object pooling for anything that spawns repeatedly (pickups, bullets,
particles, obstacles) - reusing objects instead of create/destroy per spawn
kills garbage-collection hitches:

```js
class SpawnPool {
  constructor(scene, key, size) {
    this.group = scene.physics.add.group({ maxSize: size });
    for (let i = 0; i < size; i++) {
      const o = this.group.create(0, 0, key);
      o.setActive(false).setVisible(false); o.body.enable = false;
    }
  }
  spawn(x, y) {
    const o = this.group.getFirstDead(false);
    if (!o) return null;                       // pool exhausted - skip, never create
    o.setPosition(x, y).setActive(true).setVisible(true);
    o.body.enable = true;
    return o;
  }
  kill(o) { o.setActive(false).setVisible(false); o.body.enable = false; }
}
```

Game loop: always use delta time (`update(dt)`), multiply every velocity by
`dt`, accumulate spawn timers (`timer += dt`) - never count frames. No
allocations inside `update()` (reuse vectors/arrays). Update text only when
its value changes. Physics tuning starting values: gravity 1400-1800, jump
force 500-650, move speed 200-300 (jump height = `v^2 / (2g)` - verify it
clears the intended obstacle).

### Zero-hallucination code guard (MANDATORY - the game must have NO invented code)

Hallucinated code = code whose API, asset path or mechanic was written from
memory instead of verified. It is the #1 source of broken games. Golden rule 7
applies at the CODE level too, with these hard rules:

1. **Every engine API call is verified BEFORE it is written.** Before using
   any method/event/property, look it up in the official docs/examples of the
   EXACT pinned version. If you cannot verify it, you do not use it. Never
   write `this.physics.add.something()` or `this.scene.something()` from
   memory - confirm it exists first.
2. **Every asset path is verified against the real disk BEFORE the code is
   written.** The path in code must be `ls`-confirmed to exist (golden rule 8).
   A typo in a filename = a broken game. When the code is written, run the
   AUDIT SCRIPT (below) which re-checks every referenced path mechanically.
3. **Never write "experimental" or "placeholder" code.** Every line written is
   intended to ship. If you are unsure how to implement something, STOP and
   research it (web search + docs) before writing - never guess and "see if it
   works".
4. **Every game mechanic must have a verifiable source**: either a tested
   template of this skill, the engine's official example, or a documented
   verified API. Code with no source = suspect code. Mark the source next to
   any non-obvious logic.

### The CODE AUDIT SCRIPT - mechanical, run before every gate (MANDATORY)

Auditing code by eye misses things. Run these mechanical checks every time the
code changes - they are fast and catch the silent killers:

```bash
# 1. Every asset path referenced in src/*.js exists on disk (zero missing):
#    extract all "assets/..." strings, strip quotes, verify with ls. Any
#    missing file = broken game, fix before continuing.
rg -o '"assets/[^"]+"' src/ | tr -d '"' | sort -u | while read f; do
  test -f "$f" || echo "MISSING: $f"; done

# 2. No console.log left in production (only console.error/warn allowed):
rg -n "console\.log\(" src/ && echo "FOUND console.log - REMOVE" || echo "clean"

# 3. No magic numbers: every numeric literal that is a design value (speed,
#    size, duration, multiplier) must live in config.js, not in screen code.
#    Manual review of any literal > 3 that is not a coordinate/offset.
rg -n "[0-9]{3,}" src/screen-*.js   # review each hit

# 4. Every promise has a .catch / is awaited inside try (unhandled
#    rejections crash nothing but log errors the QA tool flags):
rg -n "\.then\(|await " src/sdk.js

# 5. No engine API call is unverified: grep every `this.` method used and
#    confirm each against the pinned version's docs. When in doubt, search:
#    websearch "<engine> <version> <method> example"
rg -o "this\.[a-zA-Z]+\.[a-zA-Z]+" src/ | sort -u   # review each against docs
```

Run these five checks after EVERY feature, before Gate D/E and before the
DELIVERY GATE. Output must be clean (only the `rg -n` review lines you
explicitly reviewed and accepted).

### Automated pre-delivery tests - the game is NOT deliverable without them (MANDATORY)

Manual testing is not enough to guarantee "no errors before delivery". Before
the DELIVERY GATE, run an automated suite that drives the REAL game in a REAL
browser and checks everything mechanically. Use Playwright (or Puppeteer if
Playwright is unavailable) with the dev server running:

```bash
npx playwright install chromium          # once
```

Write a test script `tests/verify.mjs` that, for EVERY resolution of the test
matrix (portrait + landscape + desktop, section 5):

1. Launches the page at that viewport size.
2. Waits for the canvas to be present and the game to boot.
3. Captures a screenshot of EVERY screen (loading, menu, gameplay, pause,
   victory, game over) - drive the game there by simulating taps/clicks
   (Playwright `page.mouse`/`page.touchscreen`) and by clicking the correct
   buttons.
4. Collects ALL console messages; asserts ZERO errors and ZERO warnings.
5. Asserts no page crash / no WebGL context loss.
6. Asserts the canvas fills the viewport correctly (no distortion, no
   overflow, no scrollbars).

Then a PERFORMANCE + LEAK gate (weakest-device benchmark):

```bash
# Measure FPS, frame time and memory during 60s of gameplay at 1280x720:
#   - use Playwright CDP session to read performance metrics
#   - assert average FPS >= 55 on a simulated mid-tier device (throttle CPU 4x)
#   - assert no frame-time spikes above 100ms after the first second
#   - measure heap used before and after 60s of play + replay 10x: assert
#     memory does NOT grow by more than 15% (leak check - a leak = reject)
```

And an ASSET-LOAD gate:

```bash
# Assert the FULL game (all assets) loads in under a target time on a throttled
# 4G connection (Playwright emulateNetworkConditions), and that no asset fails
# to load (collect browser console errors while loading every screen).
```

Every assertion must PASS before delivery. Any failure = fix the code, re-run
the suite, do not deliver. Record the suite's output in the delivery message
(the verification skill's DELIVERY GATE re-checks it).

### Browser/mobile gotchas (real bugs if ignored)

- `index.html` MUST have the viewport meta and `touch-action` on the canvas:
  `<meta name="viewport" content="width=device-width, initial-scale=1,
  maximum-scale=1, user-scalable=no">` and CSS `canvas { touch-action: none;
  -webkit-touch-callout: none; user-select: none; }` (prevents double-tap
  zoom, scroll gestures, long-press context menu).
- **Audio unlock**: browsers block autoplay. Resume the AudioContext on the
  first `pointerdown`/`keydown` (the `Input` template does it). Without it the
  game is mute - a functional bug. Do it on the loading screen.
- **iOS Safari**: never use `height: 100vh` for the container (URL-bar bugs) -
  let the engine FIT manage the canvas.
- **WebGL context loss**: `game.events.on("contextlost", e =>
  e.preventDefault())` so a GPU hiccup on a weak phone never hard-freezes the
  game (Phaser re-creates the context).
- **Auto-pause**: listen to `visibilitychange` + window `blur` → open the
  pause overlay; resume only on focus. This is also an SDK requirement.
- **devicePixelRatio**: let the engine handle DPR (Phaser AUTO does); never
  force a high DPR that melts weak GPUs.
- **No `setInterval`/`setTimeout` for game logic** - use the engine's timer
  system so everything pauses/resumes/cleans with the scene.

---

## 5. Full responsive

- The game must adapt to ANY screen size (320px phones to 4K monitors).
- Must work in LANDSCAPE and PORTRAIT orientation.

### Choose the game orientation (a design decision, not a default)

- The ORIENTATION is decided in GAMEDESIGN.md and driven by the gameplay:
  - PORTRAIT (one hand, thumb) for clickers, match-3, merge, one-touch
    arcade, endless vertical runners.
  - LANDSCAPE (two hands) for platformers, physics puzzles, tower defense,
    horizontal runners, driving/ball games.
- Write the choice with the reason in GAMEDESIGN.md. Then implement for BOTH
  orientations:
  - The game plays fully in its chosen orientation.
  - The other orientation is handled gracefully: the game stays letterboxed
    to its aspect ratio with a themed border (never black bars), and a small
    rotation-hint overlay appears on mobile suggesting the player rotate the
    device.
- Never let a resize or a rotation break the layout, the hitboxes or the HUD.

### Implementation guidance

- Use a fixed internal resolution (e.g. 960x540 landscape / 540x960 portrait)
  and scale the canvas to fit the screen while keeping aspect ratio
  (letterboxing with a themed border if needed).
- OR use the engine's scale manager (Phaser: `scale.mode = FIT`) with the
  chosen orientation enforced.
- Compute positions relative to the screen size, never absolute pixels.
- On orientation change / resize: recompute scale, reposition UI, verify
  hitboxes are still aligned.
- Touch: make tap targets at least 44x44px. Mouse: support hover states.
- Support both mouse AND touch input for every interaction.
- Test matrix (MANDATORY): 320x568, 375x667, 414x896 and 360x640 (portrait
  phones), 844x390, 896x414 (landscape phones), 768x1024 / 1024x768
  (tablets), 1366x768, 1920x1080, 2560x1440 (desktop) AND every one of these
  rotated. No misaligned UI, no cut-off text, no broken hitboxes in any of
  them.
- **SCREENSHOT EVERY RESOLUTION (mandatory)**: for every size in the matrix,
  launch the game, set the viewport to that size, and CAPTURE each screen. A
  resolution without a screenshot is a resolution that was never checked -
  "it scales with CSS" is not a verification. Do the same for every rotated
  size.
- **Overlap scan per resolution (mandatory)**: at each captured resolution,
  VISION-check that no UI element overlaps another (the LAYOUT GUARDS of
  section 3): pause button vs HUD bars, labels vs buttons, popups vs HUD. An
  overlap that fits at the base resolution often appears at 320x568 or
  2560x1440 - catch it by looking, not by assuming.

---

## 6. The PROGRAMMING ITERATION - implement EVERYTHING, then test EVERYTHING

Coding is done in a long loop that implements the WHOLE design and then tests
it hard - never "code a bit, call it done". This is the biggest iteration of
the build. Process:

1. **Implement feature by feature, from the design**: take GAMEDESIGN.md's
   list and implement ONE feature at a time - core mechanic, then scoring,
   then each DEPTH PACKAGE feature (combos, power-ups, currency, shop, levels,
   milestones, near-miss), then ALL screens, then ALL audio (music loop +
   every SFX wired to its action, with the audio unlock on loading).
2. **Run and verify after every feature**: no console errors, the feature
   visibly works, VISION-check it, tick it in GAMEDESIGN.md. Never start the
   next feature with the current one untested.
3. **Implement ALL of the audio** - it is a feature like any other, not an
   afterthought: every action has its sound, the music loop plays, volumes
   balanced, autoplay unlocked. A silent game is a failed game.
4. **FULL TEST CAMPAIGN** (once everything is implemented - many tests, not
   one):
   - Play EVERY level end to end: each must be beatable, its star criteria
     reachable, its difficulty parameters correct.
   - Play every screen flow: loading → menu → gameplay → pause → victory →
     game over → replay, and every button, in both orientations and on every
     resolution of the test matrix.
   - Test every edge case: miss, double tap, out-of-bounds, timeout, spawn
     overlap, rapid button mashing, idle 30s, tab switch mid-play, reload
     mid-game, replay 10x in a row.
   - Check ZERO console errors/warnings, no leaks after 10+ minutes.
   - VISION-check every screen and every moment in motion.
   - **AUTOMATED SUITE (mandatory)**: run the Playwright suite (section 4) at
     every resolution of the test matrix: every screen loads, zero console
     errors/warnings, screenshots captured, no crash. Run the performance/leak
     gate (60s of play, memory stable, FPS >= 55 throttled) and the asset-load
     gate (all assets load, under target time, none fail). Record the results.
     A failing assertion blocks the campaign - fix and re-run, do not skip.
5. **TICK THE COMPLETENESS CHECKLIST** (100% - the loop's exit condition):
   - [ ] EVERY feature of GAMEDESIGN.md is implemented and ticked (nothing
         "planned" or "later")
   - [ ] ALL levels work and are beatable
   - [ ] ALL audio plays (music + every SFX, unlocked on first input)
   - [ ] ZERO console errors/warnings across the full test campaign
   - [ ] Every screen passes its Gate D and the POLISH/PRO bar
   - [ ] Every PRECISE OBJECTIVE verified on real timed runs
6. **LOOP CHECK**: any feature missing, any level broken, any audio silent,
   any error in the console, any screen below the bar? If YES, fix and re-test
   that part, then loop again. If NO, the implementation is complete and the
   polish loop (verification skill) takes over.

The programming iteration ends only when a full campaign finds nothing to fix
- implementing everything and testing everything IS the job.

### Manual final verification (MANDATORY)

Re-read ALL the code line by line and test every screen flow:

1. Loading appears, preloads, transitions to menu.
2. Menu: PLAY starts the game, sound works.
3. Gameplay: mechanics work, score updates, HUD correct, no console errors.
4. Pause: opens manually, opens automatically on tab switch, resumes.
5. Victory: stars, score, rewards, buttons all work.
6. Game over: score, best score, REPLAY restarts cleanly.
7. Replay flow: no accumulated state, score resets to zero.
8. Resize + rotate the browser: nothing breaks, no misaligned UI.
9. Check the console for ZERO errors and ZERO warnings.
10. Verify every asset referenced in code exists in the right folder (run the
    AUDIT SCRIPT from section 4 - mechanically, not by eye).
11. Confirm all text in the game is English.
12. Confirm CREDITS.md lists every asset with its license.
13. Run the AUTOMATED SUITE (Playwright + performance/leak gate + asset-load
    gate, section 4) and keep its recorded output - it is part of the delivery
    evidence.

---

## Gates

### Gate B - Engine + foundations done

- [ ] Engine chosen; exact version pinned in package.json (no `^`/`~`)
- [ ] Official docs/examples of THAT version read for every mechanic used
- [ ] **BOOT CHECK passed: an empty scene runs in the browser with ZERO
      console errors BEFORE any feature is added** (the game proves it can
      boot before it gets complexity)
- [ ] Project structure matches the standard layout (config.js, screen-1..4,
      sdk.js, main.js)
- [ ] State object + reset defined; replay reset contract clear
- [ ] First commit pushed to GitHub

### Gate D - Each screen done (repeat for EVERY screen)

- [ ] COMPLETE asset needs list written for this screen BEFORE building
- [ ] Every asset of this screen hunted, downloaded, on disk, VISION-approved
      (no missing asset, no placeholder, no "added later" surprise)
- [ ] Screen background: full-screen, on-theme, same DA as the other screens,
      VISION-approved
- [ ] Screen built and run: loads, zero console errors
- [ ] VISION: screen captured and inspected (layout, alignment, cut text,
      overlaps, empty zones)
- [ ] LAYOUT GUARDS: no overlapping UI - pause button never under a HUD bar,
      no label under a button, no popup colliding with HUD (checked at base
      resolution AND in the responsive test matrix screenshots)
- [ ] Every button shows its English label + visible hover/pressed states
- [ ] Flow to the next screen works
- [ ] No regression on the previously built screens
- [ ] POLISH: dedicated polish pass done on this screen (depth, shadows,
      layering, eased entrances, typography, transitions, micro-interactions)
- [ ] POLISH: VISION comparison - the screen reads as a released mobile hit,
      NOT a web demo (shadows, easing, spacing, focal point all at the bar)

### Gate E - Code quality done

- [ ] Line-by-line re-read: no dead code, no unused variables, no duplicated
      logic
- [ ] No magic numbers (all constants in config)
- [ ] State reset verified on replay (score = 0, positions, timers, HUD)
- [ ] Memory hygiene: timers/tweens/listeners stopped on scene shutdown
- [ ] Edge cases handled (debounce, auto-pause, resize, slow device)
- [ ] Zero console.log in production; every promise has a .catch
- [ ] CODE AUDIT SCRIPT (section 4) run and clean: every asset path exists on
      disk, no console.log, no unverified engine API, no unhandled promises
- [ ] Every engine API used is verified against the pinned version's official
      docs (zero hallucinated code)
- [ ] AUTOMATED SUITE (section 4) run: every resolution of the test matrix
      passes (zero console errors, screenshots captured, no crash)
- [ ] Performance/leak gate passed: 60s throttled play FPS >= 55, memory
      stable, no leak over 10x replays
- [ ] Asset-load gate passed: all assets load under target time on throttled
      4G, none fail
- [ ] GAMEDESIGN.md completeness checklist 100% ticked; every feature
      implemented and tested

One unchecked box in any gate means that phase is NOT done - fix it, do not
skip. Re-run the gate whenever anything in that phase changes.
