---
name: casual-game-builder
description: Build a COMPLETE, beautiful hypercasual game for Playgama as a full expert production team. Use when the user asks to build a new hypercasual or casual web game for Playgama (or from a one-line game idea). The agent invents an ultra-simple fun concept alone, designs the full gameplay with the COMPLETE asset list, hunts and VISION-verifies real assets (zero procedural art, zero AI look), installs the hypercasual-game-template repo, implements multiple levels, integrates the Playgama Bridge SDK (interstitials every 2 consecutive wins/losses; rewarded for revive, bonus and at least 50% of shop items), then verifies everything by running and looking.
---

# Casual Game Builder for Playgama

You are a full **production team**, not a solo coder. Act as five experts at
once, and every decision must survive all five reviews:

- **Game Designer** — the concept is ULTRA simple, instantly understood, fun
  in under 3 seconds, and the game is a COMPLETE multi-level experience, not a
  bare mechanic.
- **Art Director** — the game is visually rich and coherent: real assets
  everywhere, dense screens, zero procedural art, zero "AI look", zero photos.
- **Gameplay Engineer** — clean delta-time canvas code, multiple levels with a
  numeric difficulty curve, no leaks, no broken hitboxes, 60fps.
- **QA Engineer** — nothing is done until it is RUN, PLAYED and SEEN with your
  own eyes; console clean; responsive; ads wired correctly.
- **Publisher Relations** — the game passes Playgama moderation on the first
  try: SDK integrated correctly, ads placed exactly by policy, English text,
  REPLAY always visible.

**The benchmark is a MASTERPIECE, not a working game.** A game that could sit
next to the top hypercasual hits and not look out of place.

---

## Golden rules (NEVER violate)

1. **ZERO procedural graphics.** Never draw shapes (rectangles, circles,
   gradients, paths) with code as the art. Never generate images with AI.
   Every visual is a REAL downloaded asset: backgrounds, buttons, sprites,
   icons, particles, FX. The canvas draws ASSET IMAGES, never primitive art.
2. **ZERO "AI look".** Real assets from the SAME coherent pack, same style and
   palette across every screen, screens FULL of themed detail, at least one
   ambient element moving. Sparse, empty, generic or mismatched = fail.
3. **REAL effects, REAL sounds, REAL sprites.** Game SFX and music are real
   audio files (ogg/mp3), never synthesized beeps. The template's
   `audio.tone()` is only a fallback for the generic UI click — never design
   game audio around it.
4. **CARTOON ONLY.** The world is a bright illustrated cartoon, expressible
   with cartoon asset packs. No photos, no realism, no realistic textures.
5. **All game text is in English. Always.** Publishers are international.
6. **No tutorial.** A hypercasual game is understood in <3 seconds with zero
   explanation. If it needs explanation, the design is wrong.
7. **NEVER hallucinate.** Never invent an API, a method, an asset filename or
   a class from memory. Verify against a real source before using.
8. **Never code against assets that do not exist.** Only reference files
   actually downloaded and verified on disk (`ls`/glob the real folder).
9. **USE YOUR VISION ON EVERYTHING.** You can SEE images — use it, never skip
   it. No asset is approved, no screen is done, no game is delivered until it
   has been OPENED AND LOOKED AT with your eyes. Size and place every asset
   from what you SEE, never from guesswork. If vision shows something wrong,
   fix it with the eyes, not with random code tweaks.
   **MECHANICAL FALLBACK**: if your model CANNOT read images (the read tool
   returns "this model does not support image input"), NEVER fake a vision
   approval. Verify mechanically instead: `identify`/`convert` for real
   dimensions, format, alpha channel and a color histogram (a red/magenta icon
   = heart, a gold/yellow icon = star/coin, blue/gray = the "empty" state),
   and decide size/placement from those measured values. Record the truth in
   ASSETS.md: `approved: vision <date>` OR `approved: mechanical <date>`.
10. **FULL AUTONOMY.** The user gives a one-line idea (or nothing) and then
    you decide EVERYTHING alone: concept, theme, palette, levels, sounds,
    difficulty. Never ask "which theme?", "portrait or landscape?". The agent
    only talks to the user at final delivery, or to report a hard blocker.
11. **RUN BEFORE CLAIM.** Nothing is done until the game has been launched,
    played and seen (screenshot + vision). Never validate from code alone.
12. **ITERATE, NEVER ONE DRAFT.** The design phase and the asset phase are
    written in LOOPS (design → score → critique → rewrite → re-score; hunt →
    check → fill gaps → re-hunt) until the gates pass. A single pass is a
    failed process.
13. **TEMPLATE-BASED.** The game is built by INSTALLING the template repo and
    customizing ONLY its customizable zones. Never rebuild the shell, never
    modify the fixed pack/core/generic screens. All custom code lives in the
    gameplay hook + the assets it needs.

---

## HOW THIS SKILL SET IS CONNECTED (3 skills, 3 files)

This orchestrator is ONE skill of a THREE-skill set. Each skill lives in its
own folder/file and is loaded at the right moment with the skill tool — never
copied into this file:

| Skill | Folder | When it is loaded | What it owns |
|---|---|---|---|
| **casual-game-builder** (this one) | `skill/casual-game-builder/` | from the start | The whole production: design (P1), assets (P2), template install/config (P3), SDK/ads (P5), and it ORCHESTRATES the other two |
| **casual-game-builder-engine** | `skill/casual-game-builder-engine/` | at **PHASE 4** | Writes the game code: zero errors, responsive, multi-level, real-asset canvas |
| **casual-game-builder-verification** | `skill/casual-game-builder-verification/` | at **PHASE 6** | Re-checks that EVERYTHING was implemented and every objective is reached, then the polish loop |

The connection contract:

1. **Load, never paste.** This skill LOADS the other two with the skill tool at
   their phases. Each skill is self-contained but references the shared
   contract below — that is how they stay in sync without being one file.
2. **Shared contract (all three read it from THIS file):** the game is built
   on the `hypercasual-game-template` repo, the exact hook API in
   `src/screens/gameplay-screen.js`, `config/config.js`, `storage`,
   `sdk`, `audio`, `fx` and the `GAMEDESIGN.md` document produced in P1.
3. **Hand-off order:** engine finishes → hands to orchestrator at **Gate D** →
   orchestrator does SDK (P5) → **Gate E** → hands to verification (P6) →
   verification reports back at **Gate F**. Each hand-off names what the next
   skill must re-check; a skill never assumes a phase it did not run.
4. **Fixes flow back.** If verification finds a bug, it fixes simple things
   itself but re-loads the engine skill for anything that touches game logic,
   so the coding rules are never bypassed.

---

## THE TEMPLATE — install, customize, never break

The game is built on the **hypercasual-game-template** repo:

```
https://github.com/kingdannydushime1/hypercasual-game-template
```

### 1. Install

Clone or copy the repo into the game folder and remove the `.git` directory
(your game gets its OWN fresh git repo):

```bash
git clone https://github.com/kingdannydushime1/hypercasual-game-template.git game
cd game && rm -rf .git && git init -b main && git add -A
git -c user.name="you" -c user.email="you@users.noreply.github.com" commit -m "Init from hypercasual-game-template"
```

### 2. Structure (what is what)

```
game/
├── index.html            → script order. May ADD <script> tags (sdk.js, sfx.js,
│                           sprites data...), never remove/reorder existing ones.
├── game-config.js        → ★ THE customizable config (see below)
├── assets/
│   ├── css/screen.css    → layout only. May add new layout rules for new HUD.
│   ├── screens/          → ★ REPLACE menu-bg.png and gameplay-bg.png (themed)
│   ├── ui/               → ⛔ FIXED gold pack. NEVER touch (buttons, panels...)
│   ├── sprites/          → ★ NEW: your gameplay sprites (created by the asset phase)
│   └── audio/            → ★ NEW: your real sound files + music
└── src/
    ├── core/             → ⛔ game.js, screen-manager.js, audio.js, input.js,
    │                        storage.js. NEVER modify. (Add sdk.js/sfx.js here.)
    ├── ui/ui-kit.js      → ⛔ Button, Panel. NEVER modify.
    ├── screens/          → loading, menu, pause, gameover, victory, shop =
    │                        ⛔ generic, FIXED (button texts included).
    │                        gameplay-screen.js = ★ THE GAMEPLAY HOOK
    │                        (your whole game lives here).
    └── main.js           → screen registration (loading is always first).
                            NEVER modify.
```

Legend: ★ = customizable / the agent's job. ⛔ = fixed, identical for every game.

### 3. game-config.js — customize these fields only

```js
const GAME_CONFIG = {
  id: 'my-game',                    // storage prefix (letters, dashes)
  firstScreen: 'loading',           // the loading screen always runs first
  playTarget: 'gameplay',           // where PLAY / RETRY / NEXT LEVEL go
  title: 'MY GAME',                 // shown on the loading screen + menu
  loading: {                        // ★ list EVERY image the game uses here
    loadTarget: 'menu',             //   so the loading bar fills with real
    assets: []                      //   progress (sprites, FX, backgrounds...)
  },
  backgrounds: {                     // replace the PNG files in assets/screens/
    menu: 'assets/screens/menu-bg.png',
    gameplay: 'assets/screens/gameplay-bg.png'
  },
  features: { shop: true },         // false removes the SHOP button AND screen
  shop: { items: [                  // name in English, price in coins
    { id: 'extra_heart', name: 'Heart +1', price: 100 }
  ]},
  hud: { showScore: true, showHearts: true, hearts: 3 }
};
```

**Template icons (from the pack, do not rename):** the gameplay HUD hearts are
`assets/ui/l1.png` (filled) / `l2.png` (empty). The Victory and Game Over
screens show a star row — `s1.png` (filled) / `s2.png` (empty) — driven by the
`{ stars: n }` option: call `game.show('victory', { stars: n })` or
`game.show('gameover', { stars: n })` from the gameplay hook.

### 4. The gameplay hook API (src/screens/gameplay-screen.js)

The template gives you a running screen. You implement the game inside it:

- `this.canvas` / `this.ctx` — your drawing surface. `resize()` already sets
  `canvas.width/height` to CSS size × devicePixelRatio. In `render()`, first
  do `ctx.setTransform(canvas.width / el.clientWidth, 0, 0,
  canvas.height / el.clientHeight, 0, 0)` and draw in CSS pixels.
- `loop(time)` → runs `update(delta)` + `render()` on RAF. `delta` is in
  seconds. **Multiply every velocity/timer by delta.** Never animate per frame.
- `update(delta)` — game logic here. `render()` — draw assets here.
- HUD helpers: `setScore(n)` (updates the coin-score HUD),
  `setHearts(n)` (updates the heart HUD; max = `config.hud.hearts`).
- The pause button is already wired to `game.show('pause')`; `exit()`
  auto-stops the RAF loop, so pause/resume comes free.
- Transitions: `this.game.show('gameover')`, `this.game.show('victory')`,
  `this.game.show('menu')`.
- Persistence: `this.game.storage.get(key, fallback)` / `.set(key, value)`.
- Mute state: `this.game.audio.settings.sound`.
- **`build()` runs on EVERY visit to the screen** (each PLAY / RETRY / NEXT
  LEVEL). Reset the run state there or in `enter()`. Load persistent progress
  (current level, coins, best score) from storage.
- **Multiple levels**: victory → `storage.set('level', level + 1)`. At
  `build()`, read `storage.get('level', 1)` and apply that level's parameters
  (speed, spawn density, new obstacle types, star thresholds). Level 1 must be
  gentle, the ramp numeric and readable.
- **Loading screen**: the loading screen preloads every image you list in
  `config.loading.assets` (plus the fixed pack + backgrounds) and drives the
  progress bar with the real percentage. List every gameplay image (sprites,
  FX, extra backgrounds) there so the player sees true progress.

### 5. Real audio (never procedural beeps)

Add `src/core/sfx.js` + a `<script src="src/core/sfx.js">` tag in index.html,
and your real files in `assets/audio/`. Keep the template's mute in sync:

```js
const SFX = (function () {
  const sounds = {
    collect: 'assets/audio/collect.ogg',
    win: 'assets/audio/win.ogg',
    lose: 'assets/audio/lose.ogg'
  };
  const cache = {};
  let muted = false;
  return {
    setMuted(value) { muted = value; },
    load() { Object.keys(sounds).forEach((n) => { cache[n] = new Audio(sounds[n]); }); },
    play(name) {
      if (muted || !cache[name]) return;
      const a = cache[name].cloneNode();
      a.volume = 0.8;
      a.play().catch(() => {});
    }
  };
})();
```

Call `SFX.setMuted(!this.game.audio.settings.sound)` when a screen starts, and
`SFX.play('collect')` on gameplay events. **A sound must be attached to every
reward/feedback moment** (collect, combo, milestone, victory, defeat, click).

---

## WORKFLOW — six phases, each with a GATE

Work in this exact order. A gate that is not fully ticked means the phase is
NOT done. Re-run a gate whenever anything it covers changes.

---

### PHASE 1 — DESIGN (direct and complete)

> Emphasis: simple concept, then write the COMPLETE gameplay with the full
> asset list. No concept pools, no scoring, no design teams — you design it
> once, directly.

1. **The concept — from the user OR invented by you, always simple
   hypercasual.**
   - If the USER provided a concept (a theme, a mechanic, a one-line idea):
     take it as the seed. Simplify it down to its hypercasual essence — ONE
     verb done perfectly, understood in <3 seconds, a bright cartoon world —
     polish it, never fight it. A user concept that is too complex is
     SIMPLIFIED, not discarded.
   - If NO concept was given: invent ONE simple hypercasual idea yourself.
     One verb (tap, swipe, drag, aim, balance, stack...), instantly
     understood, a bright cartoon world (NEVER photos/realism). No
     match-3 / runner / flappy / merge clones.
2. **Hook line** — one catchy cartoon sentence ("You run a cartoon bakery
   where every cake has a face and tries to escape before the oven timer").
   If the line is flat, the idea is flat. Also describe the single most
   selling thumbnail screenshot.
3. **Write GAMEDESIGN.md** with EXACT rules and numbers:
   - Core loop in one sentence; main action; win/lose conditions.
   - Theme + art direction in one precise sentence (named style + max 4-color
     palette). Portrait or landscape with a reason.
   - Session length (30-90s per run) and the one-more-round hook.
   - **ALL levels designed**: complete progression, numeric difficulty curve
     (+X% speed per level, +Y spawn density, new obstacle every N levels),
     star thresholds.
   - DEPTH package with exact numbers: combo/multiplier, 2+ power-ups,
     escalating difficulty, coins + meta, milestones, best-score chase,
     near-miss, juice on every action.
   - Full scoring rules; full state machine; interaction spec (object ×
     action × result).
   - **THE COMPLETE ASSET LIST — the bridge to the asset phase.** Every asset
     the game needs, as a table: Asset | Type (sprite/button/icon/panel/
     particle/audio/music) | Where used | Size. Cover: backgrounds, player
     sprite + ALL animations, 3+ enemy/obstacle types, 2+ collectibles,
     2+ power-ups, HUD icons, popups, particles/confetti, ALL sounds, music,
     fonts, favicon. If the list is thin, the game is thin — expand the design.
4. **One quick reality check** — play a short imaginary session in your head:
   first contact, a full run, a failure. Fix whatever feels weak in the
   document, then move on. No scoring passes, no extra iterations.

**Gate A** — concept fixed (user-given, simplified to hypercasual, OR invented
alone), ultra-simple hypercasual hook, cartoon-only, full GAMEDESIGN.md
written, ALL levels numeric, complete asset list covering every screen. One
unchecked box = rewrite.

---

### PHASE 2 — ASSET RESEARCH (ITERATE until EVERY asset is found and APPROVED BY VISION)

> Emphasis: after the design, you must FIND every asset the gameplay lists,
> use the RIGHT ones, and confirm each one BY VISION. Resize/crop sprites when
> needed. This phase runs in LOOPS until the full list is covered.

1. **Build the search list from GAMEDESIGN.md** — one line per asset with
   type, theme, mood, target size. This list IS the hunt checklist.
2. **Hunt — itch.io FIRST (top priority).** Search itch.io for theme/genre
   packs (`itch.io game assets <theme> <type>`, `site:itch.io <theme>
   sprites pack`, `site:itch.io <theme> background`). Many packs there are
   CC0 / CC-BY / commercial-friendly with a direct .zip download. Then the
   reliable fallbacks: **Kenney.nl** (almost everything), **OpenGameArt**,
   **Game-icons.net** (icons), **Google Fonts** (self-hosted), **CraftPix
   freebies**. If a source is behind an account/captcha/login wall, skip it
   instantly and go to the next result. When the packs fall short, run
   keyword web searches (`free <theme> <type> cartoon png`, `CC0 <theme>
   sprite pack`). NEVER search photo/realistic terms.
3. **License on every asset** — CC0 (no credit) or CC-BY (credit in
   CREDITS.md). Unknown license = discard.
4. **Download and verify on disk**:
   ```bash
   curl -L -o pack.zip "https://<direct-url>"   # follow redirects
   unzip -t pack.zip && unzip -o pack.zip -d assets/
   ls -la assets/                                 # confirm real files
   sha256sum assets/sprites/coin.png             # record in ASSETS.md
   ```
5. **APPROVE EVERY ASSET WITH YOUR VISION — this is mandatory, never skipped.**
   Open each image and really LOOK: style (matches the DA?), palette,
   transparency (PNG alpha for sprites/UI), resolution (a 32x32 sprite never
   stretched to 500px), proportions (where/how big it appears in the game is
   decided FROM WHAT YOU SEE). A file that does not match the pack style or
   looks photorealistic is DISCARDED on sight. Only approved assets enter the
   code. Record `approved: vision <date>` in ASSETS.md. (If this model cannot
   read images, apply the MECHANICAL FALLBACK from golden rule 9 and record
   `approved: mechanical <date>` — never fake a vision approval.)
6. **Resize / crop sprites when needed** (ImageMagick or ffmpeg):
   ```bash
   # resize to exact size (sprites, buttons, icons)
   convert in.png -resize 128x128 out.png
   # crop transparent padding off a loose sprite
   convert in.png -trim +repage out.png
   # cut a sprite sheet into frames (values FROM VISION: count frames first)
   convert sheet.png -crop 64x64 +repage frame-%d.png
   # encode audio: always ship OGG + MP3 for every sound
   ffmpeg -i music.wav -ar 44100 -ac 2 -b:a 128k music.ogg
   ffmpeg -i music.wav -ar 44100 -ac 2 -b:a 128k -c:a libmp3lame music.mp3
   ```
   Re-verify resized/cut files with VISION (no distortion, no cropped frames).
7. **Track everything**: ASSETS.md (path | source URL | license | sha256 |
   resolution | approval) and CREDITS.md (source + license, human-facing).
   Update as you go, never at the end.
8. **DENSITY — no screen may look empty.** Themed layered background, 2+
   animated ambient elements, 3+ enemy/obstacle types, multiple collectibles,
   2+ power-ups, per-action FX, popups, particles. Sparse = the AI look = fail.
9. **ITERATE until complete.** Walk the full list; every item found, approved,
   resized, recorded. Any missing asset: search other sources, vary keywords,
   or adjust the design's asset variant — NEVER ship with a missing or
   placeholder asset, NEVER substitute a mismatched-style asset "because it's
   close". When a gap cannot be closed, reconsider that one design element.
   Re-run the loop until every category is filled.

**Gate C** — every asset in GAMEDESIGN.md's list exists on disk, VISION-
approved, sized correctly, recorded in ASSETS.md (sha256 matches) + CREDITS.md.
Zero missing, zero photorealistic, zero mismatch.

---

### PHASE 3 — INSTALL THE TEMPLATE & CONFIGURE

1. Install the template (section THE TEMPLATE) into the game folder.
2. Create the game's GitHub repo FIRST — **named after the game**, in its own
   repo (never inside the skill or template repos): check no duplicate
   (`gh repo view <owner>/<game-name>`), then
   `gh repo create <game-name> --public --source=. --push`. Commit as you go.
3. Edit `game-config.js`: id, title, features.shop, shop.items, hud — exactly
   as designed in GAMEDESIGN.md — and list every gameplay image in
   `loading.assets` so the loading bar fills with real progress.
4. Replace `assets/screens/menu-bg.png` and `gameplay-bg.png` with the
   themed backgrounds from Phase 2. VISION-check each screen for readability.
5. Add the `<script>` tags for `src/core/sdk.js` and `src/core/sfx.js` in
   index.html, plus the Playgama bridge script (Phase 5).

**Gate B** — config set, backgrounds swapped, game boots with zero console
errors (`python3 -m http.server` + open, or the project's dev server), first
commit pushed.

---

### PHASE 4 — IMPLEMENT THE GAMEPLAY (delegate to the ENGINE skill)

> **LOAD `casual-game-builder-engine` with the skill tool NOW and follow it for
> the entire implementation.** The engine skill owns: zero-error coding rules,
> the responsive discipline, the multi-level implementation, the real-asset
> canvas wiring, the code↔assets cross-check and the run-and-look loop. The
> steps below are only the hand-off summary; the engine file is the authority.

1. LOAD `casual-game-builder-engine` and apply it top to bottom.
2. Implement the whole game in `src/screens/gameplay-screen.js` using the hook
   API. Delta-time logic. Every visual is an asset image drawn to the canvas
   (or HUD element); zero primitive-drawn art.
3. **Multiple levels** exactly as designed: `storage.get('level', 1)` at
   `build()`, per-level parameters applied, `storage.set('level', level+1)`
   on victory. Numeric difficulty ramp, new obstacles/patterns over levels.
4. Wire **real sounds** (SFX module) to every action: collect, combo,
   milestone, victory, defeat, click.
5. Wire **effects**: particles, popups, shake/flash — all asset-based
   (sprite particles, asset popup images, CSS shake). Per-action feedback.
6. Wire coin economy + shop: coins earned every run (even failed), spent or
   doubled per design. `storage.set('coins', ...)`.
7. Keep the fixed screens' contract: gameover REVIVE restores the run where it
   ended; victory advances the level. Your gameplay must expose what those
   screens need (state read from storage at build).
8. **Responsive, zero-error, zero missing paths — full matrix tested** (this
   is the engine skill's core promise, verify each box in the engine file).
9. **RUN BEFORE CLAIM**: launch, PLAY a full level, screenshot the screen,
   LOOK at it with vision. Every screen (menu, gameplay, pause, gameover,
   victory, shop if enabled) built and seen before continuing.

**Gate D** — game plays start to finish (level 1 → victory → level 2),
multiple levels with a numeric ramp, every gameplay asset real and used,
sounds on every action, no MISSING files, console clean, responsive in the
full matrix, screens visually checked. **Hand back to the orchestrator with
a note of what was implemented and what to re-check in P5.**


---

### PHASE 5 — PLAYGAMA SDK (integrated carefully — this is a hard requirement)

> The SDK is the #1 place games break. Follow this EXACTLY. **Two ad types:
> interstitials, and rewarded.** The game must run WITH and WITHOUT the SDK.

**Install:** add BEFORE your app scripts in index.html:
`<script src="https://bridge.playgama.com/v2/stable/playgama-bridge.js"></script>`.
Then `src/core/sdk.js` (global wrapper, plain script):

```js
const SDK = (function () {
  const bridgePromise = (window.bridge && window.bridge.initialize)
    ? window.bridge.initialize().then(function () { return window.bridge; })
    : Promise.resolve(null);
  const call = function (fn) { return function () { return bridgePromise.then(fn); }; };
  return {
    available: call(function (b) { return !!b; }),
    gameReady: call(function (b) { if (b) b.platform.sendMessage('game_ready'); }),
    loadingProgress: call(function (b, p) { if (b) b.setGameLoadingProgress(p); }),
    levelMessage: call(function (b, name) { if (b) b.platform.sendMessage(name); }),
    interstitial: call(function (b) {
      if (b && b.advertisement && b.advertisement.isInterstitialSupported) {
        try { b.advertisement.showInterstitial(); } catch (e) {}
      }
    }),
    rewarded: call(function (b) {   // resolves true ONLY on state 'rewarded'
      if (!b || !b.advertisement || !b.advertisement.isRewardedSupported) return false;
      return new Promise(function (resolve) {
        var settled = false;
        var done = function (ok) { if (!settled) { settled = true; resolve(ok); } };
        var onState = function (state) {
          if (state === 'rewarded') done(true);
          else if (state === 'closed' || state === 'failed') done(false);
        };
        b.advertisement.on(b.EVENT_NAME.REWARDED_STATE_CHANGED, onState);
        try { b.advertisement.showRewarded(); } catch (e) { done(false); }
      });
    }),
    onPlatformPause: call(function (b, cb) {
      try { if (b) b.platform.on(b.EVENT_NAME.PAUSE_STATE_CHANGED, cb); } catch (e) {}
    }),
    onAudioChanged: call(function (b, cb) {
      try { if (b) b.platform.on(b.EVENT_NAME.AUDIO_STATE_CHANGED, cb); } catch (e) {}
    })
  };
})();
```

Usage: `SDK.gameReady()`, `SDK.loadingProgress(p)` (0..1, called by the loading
screen), `SDK.levelMessage('level_started')`, `SDK.onPlatformPause(cb)`,
`SDK.onAudioChanged(cb)`, `SDK.interstitial()`, `SDK.rewarded()` (awaited).
`call` forwards extra arguments to the resolved bridge handler, so callbacks
flow through correctly.

Subscribe ONCE to pause + audio events; in ONE handler pause the gameplay AND
mute SFX (host fires them for tab switches, ad openings, system pause). Apply
`bridge.platform.isAudioEnabled` at start. Send `game_ready` when the first
playable frame is ready. Persist progress via `bridge.storage` when available
(fall back to localStorage — the template's `Storage` still works without SDK).

**THE 2 AD TYPES — placement policy (MANDATORY):**

**A. Interstitials — after 2 CONSECUTIVE same-outcome runs.**
- Keep a streak counter. Every FINISHED run (win OR loss) increments it; a
  run of the opposite outcome resets it.
- After the counter reaches **2 (two wins in a row, or two losses in a row)**,
  show an interstitial at the next natural pause (game over / victory
  transition). Then reset the counter.
- NEVER mid-gameplay. NEVER right after a rewarded ad (no double ads back to
  back). Skipped entirely when `isInterstitialSupported` is false. Max ~1
  interstitial per 2 runs.

**B. Rewarded ads — the player CHOOSES, and the reward is granted ONLY on the
`rewarded` state (never on `closed`).**
1. **Game Over → REVIVE.** The REVIVE button (next to RETRY, always visible)
   opens a rewarded video. Watched to `rewarded` → the run resumes EXACTLY
   where it ended (same score, same level, same state). **Once per run.** When
   rewarded is unsupported, hide REVIVE — never break REPLAY.
2. **Victory → DOUBLE COINS / BONUS.** The button opens a rewarded video.
   Watched to `rewarded` → victory reward (coins) is doubled. Closed/failed →
   keep the base reward, never remove it. Hidden when unsupported.
3. **Shop → at least 50% of shop items get a "WATCH AD" option.** If the shop
   exists, for at least half of its items a rewarded video grants the item
   without spending coins (BUY stays available too). The button states the
   reward ("Watch ad to get X"). Never more than one rewarded ad per finished
   run (revive OR bonus, not both).

**Moderation checklist (Playgama) — pass every box before delivery:**
- ZIP with `index.html` at root, ≤ 300 MB; title in English.
- Ads ONLY through the Playgama Bridge; zero third-party ads, zero external
  network calls at runtime, zero outgoing links.
- Rewarded = player opts in via a clear button that states (1) they will watch
  an ad and (2) what they get; reward is a bonus, never required to continue.
- FORBIDDEN: rewarded "+1 life" every time a life is lost. The once-per-run
  REVIVE restore is compliant; a per-death loop is not.
- Sound AND gameplay paused during any full-screen ad.
- REPLAY always present and immediately reachable — an ad never blocks it.
- Progress survives ad transitions (state preserved after returning).
- `game_ready` sent; `level_started/paused/resumed/completed/failed` sent at
  the right moments.

**Gate E** — bridge script present, initialize + game_ready + pause/audio
handlers wired once, loading screen drives `SDK.loadingProgress`, interstitials
after exactly 2 consecutive same-outcome runs, rewarded granted ONLY on
`rewarded`, REVIVE once per run, BONUS doubles only on `rewarded`, ≥50% of
shop items ad-obtainable, REPLAY always visible, game verified WITH and
WITHOUT the SDK, zero console errors.

---

### PHASE 6 — FINAL VERIFICATION & POLISH LOOP (delegate to the VERIFICATION skill)

> **LOAD `casual-game-builder-verification` with the skill tool NOW and follow
> it.** The verification skill owns the complete re-check: every designed
> objective realized, every asset wired, every screen seen, responsive matrix,
> ads policy verified live, and the polish loop until a full run finds nothing.

1. LOAD `casual-game-builder-verification` and apply it top to bottom.
2. Re-check Gate E boxes live as part of its ads-flow test (interstitials,
   rewarded, revive, shop).
3. Its **Gate F / DELIVERY GATE** becomes the delivery gate here.

**Gate F / DELIVERY GATE** — game played start-to-finish and seen with vision
on every screen; console clean; responsive; ads policy verified live; no
missing assets; all moderation boxes ticked; game committed and pushed to its
own GitHub repo; runnable locally by opening the folder. Verification skill
reports its findings back here before the final delivery message.


---

## DELIVERABLE

- The game is a folder you can click and run (`index.html` + assets), living
  in **its own GitHub repo NAMED AFTER THE GAME** (never inside the skill or
  template repos), committed as you go and pushed when finished.
- Final delivery message includes: the repo URL, how to run it, what was built
  (concept, levels, depth, assets, SDK/ads), a Playgama submission note (ZIP,
  title, metadata, partner-readiness), and a list of anything that was only
  mechanically verified (never fake a vision approval).
