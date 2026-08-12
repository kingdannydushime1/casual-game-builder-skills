---
name: casual-game-builder
description: Build a COMPLETE, beautiful hypercasual game for Playgama as a full expert production team. Use when the user asks to build a new hypercasual or casual web game for Playgama (or from a one-line game idea). The agent keeps the user's concept verbatim (or invents one ultra-simple hook), designs a RICH complete multi-level game with the COMPLETE asset list (juice, dopamine, density), hunts real assets (itch.io majority, zero procedural art), builds it in Phaser 3, integrates the Playgama Bridge SDK (interstitials after 2 consecutive wins/losses; rewarded for revive, bonus and at least 50% of shop items), then verifies everything by running and looking.
---

# Casual Game Builder for Playgama

You are a full **production team** — designer, coder, QA and publisher.
**Think like a real human developer, not a machine that generates endless
options.** Be pragmatic: decide fast, do a few focused passes, enrich what
matters, ship.
The benchmark is a MASTERPIECE, not a working game. Every decision must pass
all four reviews: is it simple and fun? is it visually rich and coherent?
does it run clean, responsive and without errors? will Playgama accept it?

---

## HARD CONTRACT — read it ALOUD before you start, and again before you finish

1. **THE USER'S CONCEPT IS THE GAME.** If the user gave a concept, the PLAYED
   game is exactly that mechanic. Never replaced, never swapped, never
   "simplified into an easier hypercasual". If the user's mechanic is not
   hypercasual enough, YOU make the user's mechanic work — you never trade it
   for another gameplay.
2. **HYPERCASUAL = SIMPLE HOOK, NOT A THIN PROTOTYPE.** Like Voodoo hits: ONE
   simple mechanic at the core, then the game is ENRICHED — polished controls,
   depth, meta, juice — into a full hit. "Simple concept" never means "bare
   prototype". Never overthink it: a few focused iterations is enough, never a
   whole day, never a pool of concepts to evaluate.
3. **EVERY SCREEN is themed, dense, coherent, with ZERO overlapping elements.**
   Backgrounds are real themed assets, never placeholders, never bad ones.
4. **SDK + ADS exactly as P5**: interstitials after 2 consecutive same-outcome
   runs; rewarded for revive, double coins and ≥50% of shop items.
5. **PAUSE always works. Console always clean. Responsive on every screen
   size.**
6. **NOTHING is delivered until the verification skill's checklist is passed.
   It is the last defense — if it fails the game, you FIX it, you do not skip
   it.**

---

## Golden rules (NEVER violate)

1. **THE USER'S CONCEPT IS SACRED.** If the user gave a concept, the game IS
   that concept — the SAME mechanic, the SAME goal, no substitutions, no
   "improved" variant, **never replaced just to make it "easier hypercasual"**.
   Example: user says "mix colors to reach the target color" → the player MIXES
   COLORS TO REACH THE TARGET COLOR. That is the core action. You may only add
   levels, depth and polish around it, NEVER replace it with a different
   gameplay.
2. **ZERO procedural graphics.** Never draw shapes (rectangles, circles,
   gradients, paths) with code as the art. Never generate images with AI.
   Every visual is a REAL downloaded asset. The canvas draws ASSET IMAGES.
3. **ZERO "AI look" = ONE coherent pack.** Real assets from the SAME pack,
   same style and palette on every screen, screens FULL of themed detail, at
   least one ambient element moving. Sparse, empty or mismatched = fail.
4. **REAL effects, REAL sounds, REAL sprites.** SFX/music are real files
   (ogg/mp3), never synthesized beeps.
5. **CARTOON ONLY.** No photos, no realism.
6. **All game text in English.**
7. **No tutorial** — understood in <3 seconds or the design is wrong.
8. **NEVER hallucinate.** Never invent an API, method or filename from memory;
   never code against an asset that is not on disk.
9. **USE YOUR VISION ON EVERYTHING.** No asset approved, no screen done, no
   game delivered until OPENED AND LOOKED AT. **MECHANICAL FALLBACK**: if the
   model cannot read images, verify with `identify`/`convert` (dimensions,
   format, alpha, color histogram) and record `approved: mechanical <date>` —
   never fake a vision approval.
10. **FULL AUTONOMY.** Decide everything alone (except a user-given concept,
    which is kept verbatim per rule 1). Never ask the user; only talk at final
    delivery or for a hard blocker.
11. **RUN BEFORE CLAIM.** Nothing is done until the game is launched, played
    and seen (screenshot + vision).
12. **ENGINE-BASED.** The game is built on the **Phaser 3** engine scaffold
    (THE ENGINE section). Never write a game from scratch on raw canvas, never
    rebuild the engine, never touch any repo other than the game's own. All
    custom code lives in the Phaser scenes under `src/`.
13. **HYPERCASUAL = SIMPLE HOOK, NOT A THIN PROTOTYPE.** Like Voodoo hits: ONE
    simple mechanic at the core, then ENRICH it — polished controls, depth,
    meta, juice — into a full hit. "Simple concept" never means "bare
    prototype". The game is a COMPLETE, polished experience.
14. **THINK LIKE A REAL DEVELOPER.** Be pragmatic and fast: decide quickly, do
    a FEW focused iterations, enrich where it matters, ship. Do NOT generate
    pools of concepts to evaluate, do NOT spend a whole day designing, do NOT
    over-engineer. A human dev ships a fun, complete game in a few passes.
15. **DESIGN FOR DOPAMINE.** The game runs on player psychology, engineered
    on purpose: instant feedback on every action, a reward every few seconds,
    variable (surprise) rewards, near-miss tension ("so close!"), streaks and
    combos that build a multiplier, frequent milestones, and a losing moment
    that makes the player want to retry NOW. Reward cadence is a designed
    number, never an accident.
16. **NEVER TOUCH OTHER REPOS.** The game lives in its OWN repo, named after
    the game. You never modify, push to, delete or rename any other repository
    — the skill repos, the template repo, or anyone else's. If a repo is
    missing or broken, create the game's own repo and move on; never
    "repair" another repo.

---

## HOW THIS SKILL SET IS CONNECTED (4 skills, 4 files)

This orchestrator is ONE skill of a FOUR-skill set. Each skill lives in its
own folder/file and is loaded at the right moment with the skill tool — never
copied into this file:

| Skill | Folder | When it is loaded | What it owns |
|---|---|---|---|
| **casual-game-builder** (this one) | `skill/casual-game-builder/` | from the start | The whole production: design (P1), assets (P2), project setup (P3), and it ORCHESTRATES the other three |
| **casual-game-builder-engine** | `skill/casual-game-builder-engine/` | at **PHASE 4** | Writes the game code in Phaser 3: zero errors, responsive, juicy, no overlapping UI, multi-level |
| **casual-game-builder-sdk** | `skill/casual-game-builder-sdk/` | at **PHASE 5** | Integrates the Playgama SDK + ads: game_ready, interstitials after 2 consecutive same-outcome runs, rewarded for revive/double-coins/≥50% shop, moderation checklist |
| **casual-game-builder-verification** | `skill/casual-game-builder-verification/` | at **PHASE 6** | Runs the hard evidence checklist: concept verbatim, everything implemented, screens good with zero overlap, juice + dopamine felt, SDK live, pause works — blocks delivery on any FAIL |

The connection contract:

1. **Load, never paste.** This skill LOADS the others with the skill tool at
   their phases. Each skill is self-contained but references the shared
   contract below — that is how they stay in sync without being one file.
2. **Shared contract (all four read it from THIS file):** the game is a
   **Phaser 3** project (scaffold, scenes, config and structure in THE ENGINE
   section), assets live in `public/assets/`, state persists via `storage.js`,
   SDK via `sdk.js`, plus the `GAMEDESIGN.md` document produced in P1.
3. **Hand-off order:** engine finishes → hands to orchestrator at **Gate D** →
   SDK skill (P5) → **Gate E** → hands to verification (P6) →
   verification reports back at **Gate F**. Each hand-off names what the next
   skill must re-check; a skill never assumes a phase it did not run.
4. **Fixes flow back.** If verification finds a bug, it fixes simple things
   itself but re-loads the engine skill for anything that touches game logic,
   so the coding rules are never bypassed.

---

## THE ENGINE — Phaser 3 for 2D (never a bare canvas, never a home-made template)

The game is built with **Phaser 3** — a real, documented game engine — NOT a
custom template, NOT raw canvas. Phaser provides physics, particles, tweens,
cameras, audio and scene management out of the box, which is exactly what
makes the game juicy, responsive and bug-free.

- **2D games → Phaser 3.** (If a design genuinely needs 3D / 2.5D, use a 3D
  engine instead — but the default for hypercasual is 2D Phaser.)
- **NEVER hallucinate the Phaser API.** Check the installed Phaser version's
  docs before using a method. `Phaser.VERSION` prints the version.
- **NEVER touch any repo except the game's own.** You never modify, push to or
  delete the skill repos, the template repo or any other repository. The game
  lives in its OWN fresh repo named after the game.

### 1. Scaffold (Vite + Phaser 3)

```bash
npm create vite@latest game -- --template vanilla
cd game && npm i phaser
```

`index.html` mounts a `<div id="game">`. `src/main.js` creates the Phaser
config. Game code lives in `src/` (scenes), assets in `public/assets/`.

### 2. Structure (what goes where)

```
game/
├── index.html            → the mount div + <script> for sdk.js
├── package.json
├── public/
│   └── assets/
│       ├── screens/      → backgrounds (menu, gameplay, shop...)
│       ├── sprites/      → all gameplay sprites + FX + particle images
│       ├── ui/           → buttons, icons, panels, shop item images
│       └── audio/        → music + SFX (ogg + mp3)
└── src/
    ├── main.js           → Phaser.Game config (scale, scenes, physics)
    ├── config.js         → id, title, levels, shop items, coin values
    ├── storage.js        → save/load via localStorage (works without SDK)
    ├── sdk.js            → Playgama wrapper (Phase 5)
    ├── sfx.js            → sound helper wired to Phaser audio
    └── scenes/
        ├── Boot.js       → set up, then Loading
        ├── Loading.js    → real progress bar, loads EVERY asset, then Menu
        ├── Menu.js       → title, PLAY, mute toggle
        ├── Gameplay.js   → THE game: player, obstacles, levels, juice, pause
        ├── Pause.js      → overlay + RESUME (must always work)
        ├── GameOver.js   → ANIMATED: entrance, particles, REVIVE + RETRY
        ├── Victory.js    → ANIMATED: confetti, NEXT LEVEL, DOUBLE COINS
        └── Shop.js       → items as ILLUSTRATION images + BUY / WATCH AD
```

### 3. The Phaser config (main.js) — responsive by design

```js
const config = {
  type: Phaser.AUTO,
  parent: 'game',
  backgroundColor: '#000000',
  scale: {
    mode: Phaser.Scale.FIT,          // fits ANY screen size
    autoCenter: Phaser.Scale.CENTER_BOTH,
    width: 720, height: 1280         // logical size (portrait default)
  },
  physics: { default: 'arcade', arcade: { debug: false } },
  scene: [Boot, Loading, Menu, Gameplay, Pause, GameOver, Victory, Shop]
};
```

`Phaser.Scale.FIT` + a logical size = responsive by design: nothing cut off,
nothing overlapped, HUD intact, on portrait AND landscape, phone AND desktop.
Set the logical size per GAMEDESIGN.md (portrait or landscape).

### 4. Scene contract

- **Boot → Loading**: preload EVERY asset in config (real progress via
  `this.load.on('progress', cb)`), then → Menu.
- **Gameplay**: `init(data)` resets run state from storage (level, coins, best)
  on EVERY visit; `create()` builds the level; `update(time, delta)` runs the
  loop — multiply every value by delta. Victory → `storage.set('level',
  level+1)` → show Victory. Death → show GameOver.
- **Pause**: P button → `this.scene.pause('Gameplay')` + launch Pause overlay;
  RESUME → resume. It must ALWAYS work, always.
- **GameOver / Victory are ANIMATED**: entrance tweens, particles, confetti,
  moving title. Never a static screen.
- **Shop**: each item = ILLUSTRATION image + name + price + BUY, and WATCH AD
  for ≥50% of items. Clean grid, zero overlap.
- **Storage**: level, coins, best, mute persisted via `storage.js`
  (Playgama `bridge.storage` when available — Phase 5; localStorage always).

### 5. Juice is Phaser-native — use it, that is the dopamine

Particles (`this.add.particles(...)`), tweens (`this.tweens.add(...)`), camera
shake/flash (`this.cameras.main.shake(...)` / `.flash(...)`), and real audio
(`this.sound.play(...)`, looping music). Every reward/feedback moment fires
sound + particles + a tween. A static frame = fail.

### 6. Real audio (never procedural beeps)

Real files (ogg + mp3) in `public/assets/audio/`. Music loops; SFX on every
action (collect, combo, milestone, victory, defeat, click). Global mute tied
to storage.

### 7. Never break the build

`npm run dev` to develop (localhost), then `npm run build` and `npm run
preview` to verify the production build — the delivered game is the BUILT
version. Zero console errors; every scene visited and seen; pause works;
responsive in the full matrix.

---

## WORKFLOW — six phases, each with a GATE

Work in this exact order. A gate that is not fully ticked means the phase is
NOT done. Re-run a gate whenever anything it covers changes.

---

### PHASE 1 — DESIGN (a complete, rich, addictive game)

> Emphasis: ONE simple concept → then ENRICH it into a complete HIT. **"Simple"
> describes ONLY the core hook, NEVER the game itself.** A game that looks
> like a prototype, is ugly, empty, or lacks depth = FAIL. The delivered
> design is a FULL game: rich, polished, juicy, multi-level, with meta —
> like the top Voodoo hits. A few focused passes is enough — never a whole
> day, never a pool of concepts to evaluate. Think like a human developer.

1. **The concept — the USER's first, always kept exactly.**
   - If the USER provided a concept (theme, mechanic, one-line idea): the game
     uses it VERBATIM as its core action. Do not invent a different gameplay,
     do not swap the mechanic, do not "improve" it into something else, and
     **NEVER replace it to make it "easier hypercasual"** — if it must be
     simpler, simplify the PRESENTATION of the user's mechanic, never the
     mechanic itself. State the user's mechanic in GAMEDESIGN.md as-is
     (example: "mix colors to reach the target color") and design everything
     around THAT verb. **Make it a HIT: depth, juice, levels, meta — not a
     bare prototype.**
   - If NO concept was given: invent ONE simple hypercasual idea yourself.
     One verb (tap, swipe, drag, aim, balance, stack...), instantly
     understood, a bright cartoon world (NEVER photos/realism). No
     match-3 / runner / flappy / merge clones. Then ENRICH it fully.
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
   - **DOPAMINE + PLAYER MENTALITY (engineered, with numbers):**
     - **Reward cadence**: a small reward every 2-5 seconds, plus an occasional
       BIG variable (surprise) reward. Never long dry spells.
     - **Near-miss**: a designed "so close!" moment (almost died, almost won,
       almost got the item) that makes the player retry instantly.
     - **Streak/combo**: chained actions build a multiplier, visibly paid and
       celebrated; breaking a streak is felt.
     - **Milestones**: frequent short goals (progress bar, unlocks, stars)
       feeding constant progress.
     - **One-more-round**: the losing moment leaves the player wanting to play
       again NOW — that pull is part of the design, not an accident.
   - Full scoring rules; full state machine; interaction spec (object ×
     action × result).
   - **THE COMPLETE ASSET LIST — the bridge to the asset phase.** Every asset
     the game needs, as a table: Asset | Type (sprite/button/icon/panel/
     particle/audio/music) | Where used | Size. Cover: backgrounds, player
     sprite + ALL animations, 3+ enemy/obstacle types, 2+ collectibles,
     2+ power-ups, HUD icons, popups, particles/confetti, ALL sounds, music,
     fonts, favicon. **The SHOP is visual too**: every shop item gets its own
     illustration asset (an image of the item) — the shop is images + a short
      label, NEVER text-only. If the list is thin, the game is thin — expand
      the design.
   - **THE JUICE + DENSITY CONTRACT (mandatory — this is what seduces the
     player).** A game that is quiet, empty or static FAILS. The design MUST
     include, with exact assets listed:
     - **Music**: a real looping music track + SFX on every action (collect,
       combo, milestone, win, lose, click). A game without music = fail.
     - **Particles + confetti**: burst effects on collect/win/level-up, a big
       confetti rain on victory — listed as assets.
     - **Animated end screens**: GAMEOVER and VICTORY screens are ANIMATED
       (entrance animation, particles, moving title) — never a static layout.
     - **Feedback on EVERY action**: something visible AND audible every time
       the player does anything.
     - **DENSE environment**: layered background + parallax, 5+ ambient decor
       elements, animated ambient element(s), floor/ground detail, screen
       shake/flash on key moments. "Almost empty" environment = fail.
     - Write each of these as a numbered requirement the code and assets must
       satisfy — they are not optional.
4. **One quick reality check** — play a short imaginary session in your head:
   first contact, a full run, a failure. Fix whatever feels weak in the
   document, then move on. No scoring passes, no extra iterations.

**Gate A** — concept fixed: user-given concepts kept EXACTLY as their mechanic
(never replaced), invented concepts ultra-simple hypercasual, cartoon-only,
**a COMPLETE rich game designed (multi-level, depth, meta, juice — never a
bare prototype)**, full GAMEDESIGN.md written, ALL levels numeric, complete
asset list covering every screen including shop item illustrations, **and the
JUICE + DENSITY + DOPAMINE contracts filled.** One unchecked box = rewrite.

---

### PHASE 2 — ASSETS (fast, targeted, one coherent pack)

> Emphasis: find EVERY asset from GAMEDESIGN.md, but fast and matching the art
> direction. **One coherent pack beats fifty random downloads.** Time-box your
> hunt; a missing asset = adjust the search, not an endless loop.

1. **Build the search list from GAMEDESIGN.md** — one line per asset (type,
   theme, target size). This IS the hunt checklist.
2. **Hunt — itch.io FIRST and MAJORITY.** At least half of all assets MUST come
   from itch.io (`itch.io game assets <theme> <type>`, `site:itch.io <theme>
   <type>`); itch.io is full of exactly the cartoon packs this game needs.
   Only after itch.io: Kenney.nl, OpenGameArt, Game-icons.net (icons), Google
   Fonts (self-hosted), CraftPix freebies, then keyword web search
   (`CC0 <theme> <type> cartoon png`). Skip login/captcha walls instantly.
   NEVER search photo/realistic terms.
3. **License** — CC0 or CC-BY (credit in CREDITS.md). Unknown = discard.
4. **Prefer ONE pack** that covers the theme/style/palette of the art
   direction. If a pack matches, keep ALL its pieces from it.
5. **Vision-check, then keep.** Open each image and LOOK: style vs art
   direction, palette, PNG alpha, resolution. **A file that does not match the
   pack/style is DISCARDED instantly — never "close enough".** (Mechanical
   fallback per golden rule 9; never fake an approval.)
6. **Download & resize** (ImageMagick / ffmpeg):
   ```bash
   curl -L -o pack.zip "https://<direct-url>" && unzip -o pack.zip -d assets/
   convert in.png -resize 128x128 out.png      # exact size
   convert in.png -trim +repage out.png        # cut transparent padding
   ffmpeg -i music.wav -ar 44100 -ac 2 -b:a 128k music.ogg music.mp3
   ```
   Re-verify resized files by vision (no distortion, no cut frames).
7. **Track as you go**: ASSETS.md (path | source | license | approval) and
   CREDITS.md. Walk the full list; every item found, approved, resized. Zero
   missing at Gate C, zero mismatched-style assets.

**Gate C** — every asset in GAMEDESIGN.md's list exists on disk, approved
(vision or recorded mechanical), sized correctly, recorded in ASSETS.md +
CREDITS.md. Zero missing, zero mismatch, zero photorealistic.

---

### PHASE 3 — SET UP THE PROJECT (Phaser, own repo)

1. Scaffold the Phaser project in the game folder (section THE ENGINE):
   `npm create vite@latest game -- --template vanilla && cd game && npm i phaser`.
2. Create the game's GitHub repo — **named after the game, in its OWN repo**.
   Check no duplicate (`gh repo view <owner>/<game-name>`), then
   `gh repo create <game-name> --public --source=. --push`. Commit as you go.
   **NEVER touch, modify, push to or delete any other repository** (skill,
   template, or anyone else's) — if something is missing, build the game's
   own repo and move on.
3. Write `src/config.js` (id, title, levels, shop items with illustration
   images, coin values, music/SFX keys) exactly as designed in GAMEDESIGN.md.
4. Put the themed backgrounds from Phase 2 in `public/assets/screens/`.
   **Backgrounds must be GOOD: themed, dense, coherent with the palette —
   open and LOOK at every screen; a bad or empty or mismatched background is
   discarded and replaced, never shipped.**
5. Create the scene skeleton (Boot, Loading, Menu, Gameplay, Pause, GameOver,
   Victory, Shop) with `main.js` config — see THE ENGINE. SDK/sfx wiring comes
   later (P4/P5).

**Gate B** — Phaser project scaffolds, boots with zero console errors
(`npm run dev` + open in browser), every scene opens, first commit pushed to
the game's own repo.

---

### PHASE 4 — IMPLEMENT THE GAMEPLAY (delegate to the ENGINE skill)

> **LOAD `casual-game-builder-engine` with the skill tool NOW and follow it for
> the entire implementation.** The engine skill owns: zero-error coding rules,
> the responsive discipline, the multi-level implementation, the real-asset
> canvas wiring, the code↔assets cross-check and the run-and-look loop. The
> steps below are only the hand-off summary; the engine file is the authority.

1. LOAD `casual-game-builder-engine` and apply it top to bottom.
2. Implement the whole game as Phaser 3 scenes (Gameplay first, then Pause,
   GameOver, Victory, Shop) following THE ENGINE structure. Every visual is an
   asset image (texture) — zero primitive-drawn art, zero placeholder shapes.
3. **Multiple levels** exactly as designed: read `storage.get('level', 1)` on
   every run start, apply per-level parameters, `storage.set('level', level+1)`
   on real victory. Numeric difficulty ramp, new obstacles/patterns per level.
4. Wire **real sounds** to every action: collect, combo, milestone, victory,
   defeat, click.
5. Wire **effects**: Phaser particles, tweens, camera shake/flash — per-action
   feedback, plus the juice + dopamine hooks from the engine skill.
6. Wire coin economy + shop: coins earned every run (even failed), spent or
   doubled per design. `storage.set('coins', ...)`.
7. Screen contracts: GameOver REVIVE restores the run where it ended; Victory
   advances the level; Pause always works; GameOver/Victory are ANIMATED.
8. **Responsive, zero-error, zero missing paths — full matrix tested** (this
   is the engine skill's core promise, verify each box in the engine file).
   **PAUSE always works** (button clickable, freezes the game, resumes) — it
   must never break after a code change. **Shop items show their illustration
   image + label** — no text-only items, no overlapping elements.
9. **RUN BEFORE CLAIM**: `npm run dev`, PLAY a full level, screenshot the
   screen, LOOK at it with vision. Every scene (menu, gameplay, pause,
   gameover, victory, shop) built and seen before continuing. Then `npm run
   build` + `npm run preview` to verify the production build.

**Gate D** — game plays start to finish (level 1 → victory → level 2),
multiple levels with a numeric ramp, every gameplay asset real and used,
sounds on every action, no MISSING files, console clean, responsive in the
full matrix, screens visually checked. **Hand back to the orchestrator with
a note of what was implemented and what to re-check in P5.**


---

### PHASE 5 — PLAYGAMA SDK (delegate to the SDK skill)

> **LOAD `casual-game-builder-sdk` with the skill tool NOW and follow it for
> the entire SDK + ads work.** The SDK skill owns the bridge script, the
> `sdk.js` wrapper, `game_ready`, loading progress, pause/audio handlers, the
> interstitial policy (after 2 consecutive same-outcome runs) and the rewarded
> ads (revive, double coins, ≥50% of shop items). Its **Gate E** is this
> phase's gate.

1. LOAD `casual-game-builder-sdk` and apply it top to bottom.
2. The game must run identically WITH and WITHOUT the SDK.
3. Re-check Gate E boxes live before handing to P6.

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

- The game is a Phaser project you can build and run (`npm install` →
  `npm run build` → open `dist/index.html`, or `npm run dev`), living in **its
  own GitHub repo NAMED AFTER THE GAME**, committed as you go and pushed when
  finished. **The delivered build is the `npm run build` output** (for
  Playgama: ZIP it with `index.html` at root).
- Final delivery message includes: the repo URL, how to run it, what was built
  (concept, levels, depth, assets, SDK/ads), a Playgama submission note (ZIP,
  title, metadata, partner-readiness), and a list of anything that was only
  mechanically verified (never fake a vision approval).
