---
name: casual-game-builder-engine
description: The CODING skill of the casual-game-builder skill set. Loaded by the orchestrator at PHASE 4. Builds the game with Phaser 3 (2D; a 3D engine for 3D/2.5D): zero errors, responsive on every screen, rich and juicy (particles, tweens, cameras, real audio), multi-level, no overlapping UI, and the user's concept implemented verbatim. Use when implementing or fixing the game code of a casual-game-builder game.
---

# Casual Game Builder — ENGINE (Phaser 3, zero errors, rich and juicy)

You are loaded by `casual-game-builder` at PHASE 4 and you are the ONLY
authority on how the game is coded. The game is a **Phaser 3** project (see
THE ENGINE section in the orchestrator: scaffold, structure, scene contract).
Your contract:

- **ZERO ERRORS.** Zero console errors, zero uncaught exceptions, zero missing
  assets, at any moment, on any device.
- **RICH, NOT THIN.** Hypercasual = simple hook, NOT a bare prototype. The game
  is a COMPLETE hit: polished controls, depth, meta, juice. Ugly, empty or
  prototype-looking = fail. Build like a human dev: few focused passes, no
  over-engineering.
- **THE USER'S CONCEPT VERBATIM.** The core mechanic is exactly what the user
  (or GAMEDESIGN.md) specified. Never change, swap, or replace it — not even
  to make it "easier hypercasual".
- **RESPONSIVE + NO OVERLAP.** Phaser.Scale.FIT + a layout system = nothing cut
  off, nothing overlapping, HUD intact, at every size.
- **MULTI-LEVEL + JUICE + DOPAMINE** exactly as designed.

You hand off at **Gate D**, then verification takes over at P6.

---

## Before writing a line

1. Check the installed Phaser version and its real API (`Phaser.VERSION`,
   the project's `node_modules/phaser/types`). **NEVER hallucinate a Phaser
   method** — verify before use.
2. Read `GAMEDESIGN.md`. Every code line maps to a design line. The core
   action IS the user's concept — verbatim. Missing design = bug you are about
   to ship.
3. `ls`/glob the real folders under `public/assets/`. Only reference files
   actually on disk (cross-check below is mandatory).
4. Keep every SDK/audio/storage call defensive so the game also runs WITHOUT
   the Playgama bridge (that is checked again in P5).

---

## Zero-error discipline

- **No API from memory.** Verify Phaser calls against the installed version.
- **Guard everything external.** `sdk.*`, audio, storage and image loads are
  wrapped so a failure can never crash the loop.
- **Delta time everywhere.** Use `time`/`delta` in `update()` for motion,
  timers, combos. Never per-frame increments.
- **No leaks.** Scene shutdown destroys tweens/particles/listeners
  (`this.tweens.killAll()`, `this.particles.destroy()`, remove input
  listeners). Pause/resume 10× must not multiply entities or memory.
- **Edge cases as first-class work.** Level 1, last level, 0 coins, exact-coin
  buy, revive with 0 coins, win exactly as a particle spawns, double-tap,
  resize mid-run. Each is a test you run.
- **PAUSE must never break.** After EVERY code change: press P — the game
  freezes instantly, the Pause scene opens, RESUME restores the run, zero
  errors. A dead pause = ship-blocking bug, fix it first.
- **No zombie code.** No unused assets/imports/dead branches, no `console.log`
  left in the shipped build.
- **Layout = one system, no overlap.** All UI positions come from ONE layout
  function computed from the screen size; elements stack relative to each
  other with fixed gaps — never eyeballed coordinates. Audit the high-risk
  spots every time: HUD (score/coins/pause), shop grid, popups, gameover/
  victory buttons. Overlap = fix the layout system, not the pixels.
- **Shop is visual.** Every item = illustration image + label + price + BUY
  (and WATCH AD for ≥50% of items). Clean grid, zero overlap, never text-only.

### Code↔assets cross-check (after EVERY code or asset change)

```bash
rg -o '"public/assets/[^"]+"|"assets/[^"]+"' src/ index.html | tr -d '"' | sort -u \
  | while read f; do test -f "$f" || echo "MISSING: $f"; done
```

Zero MISSING — ever. Then delete asset files no longer referenced (no orphans).

---

## Responsive discipline

- `Phaser.Scale.FIT` + logical size (from THE ENGINE) = the whole layout scales
  and centers on any screen. Never fight the scale manager.
- HUD, buttons and text stay inside the logical bounds; nothing is drawn off
  the visible area at any aspect.
- Re-test the matrix on every UI change — screenshot each cell and LOOK:

| Row | Portrait | Landscape |
|---|---|---|
| 320×480 / 375×667 (phone) | pass | pass |
| 768×1024 (tablet) | pass | pass |
| 1366×768 / 2560×1440 (desktop) | pass | pass |

Failing cell = not done.

---

## Multi-level implementation

- `storage.get('level', 1)` at every run start; per-level parameters (speed,
  spawn, density, score targets) from a numeric curve in `src/config.js`.
- New obstacles/mechanics on later levels per GAMEDESIGN.md, with real assets.
- `storage.set('level', level + 1)` ONLY on real victory; revives don't skip.
- The ramp is felt between level 1 and level 2, and every level after.

---

## Juice + dopamine (Phaser-native — this is what seduces the player)

- **Music** loops from the start (with a working mute toggle) + real SFX on
  every action: collect, combo, milestone, victory, defeat, click.
- **Particles + confetti**: `this.add.particles(...)` — burst on collect /
  level-up, big confetti rain on victory, particles on gameover. Capped for
  60fps on weak phones.
- **Tweens everywhere**: `this.tweens.add(...)` — popups, scaling, entrance
  animations, moving title. **GAMEOVER / VICTORY are ANIMATED**, never static.
- **Camera feedback**: `this.cameras.main.shake(...)` / `.flash(...)` on key
  moments (near-miss, hit, victory).
- **Dopamine hooks, coded to the design**: reward cadence (a sound+particle+
  popup every few seconds), combos with escalating juice (pitch-up, bigger
  bursts), near-miss slow-mo/shake + "so close!" cue, milestone popups,
  one-more-round pull (gameover lands on a juicy animated screen, REPLAY
  visible).
- **DENSE environment**: layered background + parallax, 5+ ambient decor
  elements, an animated ambient element, floor/ground detail. Empty scene =
  fail.
- **Economy**: coins every run (even failed), earn AND spend via `storage`,
  spend/double per design.

---

## RUN BEFORE CLAIM

1. `npm run dev`, play a full level; screenshot and LOOK with vision (or the
   mechanical fallback from the orchestrator's golden rules — never fake a
   vision approval).
2. Visit EVERY scene: Menu, Gameplay, Pause, GameOver, Victory, Shop. Pause
   freezes and resumes. Shop shows images, nothing overlaps.
3. Watch the juice live: collect → particles + sound; win → confetti + animated
   Victory; die → animated GameOver; menu → music + ambient motion. Any static,
   silent or empty screen = NOT done.
4. Stress: win, lose, revive, double coins, resize mid-run, pause/resume 10×.
5. Console zero errors the whole session. Then `npm run build` + `npm run
   preview` — the production build must run clean too.

---

## Hand-off

**Gate D — DONE means:** game plays start to finish (level 1 → victory →
level 2); mechanic is the USER's concept verbatim; multi-level with a numeric
ramp; juice + dopamine present and SEEN (music, particles, confetti, animated
GameOver/Victory, dense environment, reward cadence); zero MISSING files; zero
console errors; PAUSE works everywhere; shop illustrated without overlap; full
responsive matrix green; every scene seen with vision; production build works.
Report to the orchestrator: what was implemented, storage keys in use, and
what P5 (SDK) must re-check. **Do NOT proceed to the SDK — that is PHASE 5.**
