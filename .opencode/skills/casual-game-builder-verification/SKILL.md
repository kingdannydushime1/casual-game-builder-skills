---
name: casual-game-builder-verification
description: Loaded by casual-game-builder at the final verification phase. Ultra-strict verification of a casual game: vision-based asset/screen verification + mechanical fallback, the anti-AI-look checklist, the final gameplay-objective verification, the verification gates (Gate F + DELIVERY GATE), the meticulous inspection, the iteration & polish loop (repeat until flawless) and subagent delegation. Run until a full loop finds NOTHING to fix.
---

# Casual Game Builder - Verification

This sub-skill runs the ultra-strict verification of the whole game. It is
loaded by the orchestrator skill at phase 7. Golden rules 10 (vision), 15 (run
before claim) and 6 (never cut corners) always apply. **Nothing is delivered
until the DELIVERY GATE passes 100%.**

---

## 1. Vision-based asset & screen verification (MANDATORY)

The agent MUST use the AI model's image-reading capability to SEE every asset
and every screen. Vision is not optional decoration - it is the tool the agent
uses to know what it is integrating and where to put it. Never trust code
blindly.

### 1.1 Look at every asset BEFORE integrating it

1. **OPEN the image** with the image read tool after every download. Actually
   SEE it and describe it out loud in your reasoning: what is it, what is its
   style, its colors, its transparency, its resolution, its proportions.
2. **Judge with the eyes**: is it coherent with the pack (same style, same
   line width, same palette 3-4 colors)? Is the transparency clean (no white
   or black box around the sprite)? Is the resolution close to the on-screen
   size needed (never a blurry 32x32 stretched to 500px)?
3. **Decide placement FROM THE IMAGE**: the exact position, scale, z-order,
   rotation and layering of each asset comes from what you see (where is the
   sprite's visual center, where is the button's padding, what part overlaps
   the frame edge). Write these numbers down for integration.
4. Reject and replace any asset that clashes, is blurry or has wrong
   proportions - the vision must approve every asset before it enters the
   code.

### 1.2 Look at every screen AFTER building it

5. **Capture each screen** (headless browser screenshot, Playwright screenshot,
   or the engine's snapshot API) and OPEN the capture with the image read
   tool. Verify with the eyes: layout, alignment, text not cut off, buttons
   visible with correct states, palette coherence, no empty or grey zones, no
   random overlaps.
6. **Look at the game in motion**: capture mid-gameplay and verify the
   mechanic visually: sprites collide correctly, the score updates, FX appear,
   nothing clips, the asset sizes look right on screen.
7. **Look at HUD & overlays**: capture pause, victory and game over screens
   and check every element is visible and aligned.
8. Fix anything the vision reveals before moving to the next screen. Vision is
   the last line of defense against the AI look.

### 1.3 If vision is unavailable - mechanical fallback (never fake an approval)

The image read tool may not display images in some environments. In that case
DO NOT claim a vision approval that did not happen - a fake approval is worse
than a missing one. Fall back to mechanical verification and record it in
ASSETS.md:

1. **Declare the limitation**: mark every asset `approved: mechanical` (with
   date) instead of `approved: vision` in ASSETS.md. A human re-check is
   required before submission - say so in the final delivery message.
2. **Verify every asset mechanically** instead of with the eyes:
   - Images: `identify -verbose file.png` (or `file`) - real pixel size,
     correct format, PNG alpha where needed, not truncated.
   - Audio: `ffprobe`/`ffmpeg` decode (assets skill) - valid, clean.
   - Sprite sheets: never guess a frame size. Use the pack's OWN documented
     grid, then verify arithmetically that the sheet's measured width/height
     divides evenly by cols/rows (e.g. `identify` dimensions, `width % cols
     == 0`). Record the source of the numbers.
   - Screens/layout: take headless screenshots and verify mechanically -
     bounding boxes, element overlaps, cut-off text via DOM/canvas
     measurements, console errors. You can check alignment without seeing it.
3. **Conservative placement**: without eyes, use the pack's documented sizes
   and standard centering, with larger safety margins, instead of custom
   per-pixel placement.
4. A human must visually approve the game before submission - list in the
   final delivery which assets/screens were only mechanically verified.

---

## 2. Anti-AI-look checklist (verify ALL)

A game looks AI-generated when it is incoherent, empty, generic, or THIN.
Check each point before finishing:

1. **Assets from ONE pack**: same style, same proportions, no mixing of 3D +
   pixel + cartoon in the same game.
1b. **Asset DENSITY**: the game is rich - dozens of real assets, layered
   backgrounds with parallax, animated decor on every screen, multiple enemy
   and collectible types, per-action FX. A game with a handful of assets looks
   procedural no matter how good they are (assets skill, DENSITY rules).
2. **Homogeneous restricted palette**: 3-4 dominant colors, applied to every
   screen.
3. **Perfect texts**: correct English, no typos, no placeholders ("text",
   "lorem", "???", "TODO"), catchy short titles, coherent typography.
4. **Professional UI**: hover/pressed button states, grid alignment, clean
   layout, nothing overlapping randomly.
5. **Lively but sober animations**: eased movements (no robotic linear
   motion), micro-interactions (score pop, button pulse), measured particles
   (not fireworks everywhere).
6. **Complete audio**: music + action sounds + feedback. A silent game is a
   dead giveaway of AI generation.
7. **Global visual coherence**: the same background style and theme on every
   screen; the logo, buttons and loader style appear everywhere.
8. **Human details**: object shadows, decor variants, one subtle easter egg,
   smooth difficulty progression (no random difficulty spikes).
9. **Technical professionalism**: constant 60fps, no jank, responsive, fast
   load, no console errors.

---

## 3. Final gameplay-objective verification (MANDATORY)

Re-open `GAMEDESIGN.md` and verify that EVERY documented objective is really
implemented and really works. Build the checklist from the document, then test
each line. For every entry tick one of: DONE + tested, or FIXED. Nothing may
stay "planned". Minimum checklist:

1. Every mechanic of the main action works and feels right (tested on the real
   running game, visually verified).
2. Every edge case from the interaction specification is handled (miss, double
   tap, out-of-bounds, timeout, spawn overlap).
3. Every level from the level list exists, is playable, and its star criteria
   and difficulty parameters match the document.
4. The difficulty curve is respected: no spike, no plateau.
5. Score, combos and multipliers count exactly as specified.
6. Win/lose conditions are exact and trigger the correct screens.
7. The "one more round" hook is verified: a failed run ends at a moment of
   high tension and still gives a consolation gain.
8. Progression (coins, unlocks, best score) is saved and kept across rounds.
9. Replay fully resets state: score = 0, positions, timers, HUD.
10. The chosen orientation plays fully; the other orientation letterboxes
    cleanly with the rotation hint.
10b. DEPTH PACKAGE verified: combos/multipliers build and display correctly,
    EVERY power-up works with its exact effect and duration, coins are earned
    every run (even failed ones), the shop/meta/unlocks work and persist,
    variety events appear on schedule, milestones pop with sound, the
    near-miss bonus triggers.
11. Every asset referenced in code exists on disk (verified via `ls`), loads
    without error, and looks visually coherent.
12. Audio: music + every action sound plays and is not cut off.
13. Zero console errors/warnings, no leaks after 10 minutes of play.
14. The game is FUN: play 2 full runs and honestly answer "would I play this
    for 2 hours?". If not, fix the gameplay, not the screens.
15. PRECISE OBJECTIVES verified on the real run (stopwatch + VISION): early
    first reward, steady reward cadence, run/level duration matches the
    design, end at peak tension, difficulty ramps as designed, something new
    shows up regularly, milestones pop, and EVERY designed feature is built
    and working.

---

## 4. Verification gates - checklists at EVERY step (MANDATORY)

Every step of the workflow ends with a GATE: a checklist that must be ticked
100% before moving on. One unchecked box means the step is NOT done - redo it,
do not skip it. Re-run the gate whenever anything in that step changes.
(Gates A-E live in their phase skills: A = design, B = engine, C = assets,
D = screens, E = code.)

### Gate F - Verification done

- [ ] Manual final verification (engine skill) all pass
- [ ] Vision verification (section 1) all pass
- [ ] Gameplay-objective verification (section 3) all pass
- [ ] Test matrix: every resolution x every orientation passes
- [ ] SCREENSHOTS exist for every screen at EVERY resolution x orientation of
      the matrix (a resolution without a screenshot = not verified)
- [ ] Overlap scan done on every screenshot: no two UI elements intersect at
      any resolution (LAYOUT GUARDS, engine skill)

### DELIVERY GATE - HARD REFUSAL LIST (ultra-strict)

Before delivering, re-check ALL of these. If ANY one is "no", delivery is
REFUSED - fix it and re-run the verification loop first:

- [ ] The game was ACTUALLY LAUNCHED and played end to end (dev server
      running, gameplay run, menu → gameplay → victory/game over → replay) -
      no delivery without a real run.
- [ ] Every screen was SCREENSHOTTED from the running game AND viewed
      (VISION) or mechanically verified with the limitation recorded. No "I
      didn't launch it" and no "I didn't screenshot".
- [ ] ZERO console errors on the final run.
- [ ] Every mechanic is FUNCTIONAL and was play-tested in the running game
      (not just coded): core loop, meta loop, pause, replay, bonus, ads flow.
- [ ] Gameplay is FUN, not just working: at least 2 full playtest runs, reward
      cadence present, "one more round" hook present. Boring = refuse.
- [ ] Responsive test matrix: EVERY resolution + both orientations visited
      with screenshots; no overlapping elements (overlap scan passed), no
      clipped text, no broken hitboxes.
- [ ] UI overlap scan passed: no two interactive/HUD elements intersect (pause
      button vs bars, buttons vs popups) at any resolution.
- [ ] Engine used, NOT vanilla (unless the explicit one-screen exception
      applies): architecture in place, scene manager, delta-time logic.
- [ ] ASSETS.md + CREDITS.md complete; `sha256sum -c` passes; every asset
      approved or mechanically recorded.
- [ ] SDK: Playgama Bridge v2 integrated per the required steps; game runs
      with mock and with SDK; rewarded granted ONLY on `rewarded` state.
- [ ] English texts, no placeholders, no console.log left, REPLAY always
      visible, no external runtime requests.
- [ ] Game looks like a MASTERPIECE: coherent pack, dense rich screens,
      palette respected, no AI look - the Art Director says yes.

---

## 5. Meticulous inspection (the "fine-tooth comb" review)

After the build passes all gates, run a slow, hostile review - assume the game
is broken and TRY to break it. Two full passes.

### Code inspection pass (file by file, line by line)

- Re-read every file top to bottom. For each function: does it do exactly what
  its name says? Any unused variable, dead branch, duplicated logic?
- Search for the classic bug patterns: `console.log` left in, magic numbers,
  missing `.catch`, `setInterval` for game logic, objects created per frame,
  state mutated in two places, timers/tweens not stopped on shutdown.
- Verify every asset path is a real file (re-run `ls` on the folders).
- Re-run `sha256sum -c` against ASSETS.md: every approved asset is intact on
  disk (catches re-downloads, corruption, drift between sessions).
- Check every promise/async call is handled - zero unhandled rejections.

### Game inspection pass (play it like a hostile player)

- Play each screen as if you had never seen it. Then play it FAST, then
  SLOWLY, then mash every button rapidly, then idle 30 seconds, then
  rotate/resize mid-play, then switch tabs mid-play.
- Try to break the win condition (reach it, exceed it, trigger it twice).
- Try to break the fail condition (dodge it completely, trigger it twice).
- Let the game run 5+ minutes with objects spawning: no slowdown, no memory
  creep, no object stack-up.
- Test with the SDK present AND absent, reload mid-game, replay 10 times in a
  row and check the state resets perfectly every time.
- LOOK at every screen and every moment with VISION throughout all of this.

Every problem found goes into a FIX LIST. Fix them all, then re-run the gates.

---

## 6. Iteration & polish loop (repeat until flawless)

This loop sits on TOP of the three big per-phase iterations that must ALL be
run to completion before delivery:
- **Gameplay writing iteration** (design skill) - rewrite GAMEDESIGN.md until
  every objective is ticked.
- **Asset research iteration** (assets skill) - hunt until the game is
  asset-RICH and cartoon, at the mobile-hit bar.
- **Programming iteration** (engine skill) - implement EVERYTHING (incl. audio)
  and test EVERYTHING (all levels, all flows, many tests).

A cycle of THIS loop refines the result of all three:

1. **PLAYTEST pass** - play 2-3 full rounds like a real player. Note anything
   that feels unfair, slow, boring, or buggy. This finds what code review
   cannot.
2. **OBJECTIVE pass** - re-open GAMEDESIGN.md and tick every documented
   objective as really working. Anything not demonstrably working is a blocker.
3. **CODE review pass** - run the meticulous inspection (section 5). Fix all.
4. **DESIGN/polish pass** - make what is good into what is great:
   - stronger juice (better sound on the key action, snappier popups)
   - faster first reward (the first score moment within 5-10 seconds)
   - tighter difficulty (tension rises exactly when it should)
   - more satisfying feedback (the best action feels BEST)
   - any screen that still looks "almost pro" - bring it up to the bar
   - **RICHNESS pass (mandatory every cycle)**: VISION-check every screen
     again - any empty/plain/generic zone? any screen without an animated
     ambient element? If the game still looks sparse or procedural, GO HUNT
     MORE ASSETS (assets skill) until every screen is full and rich. Never
     ship a thin-looking game.
   - **DEPTH pass (mandatory every cycle)**: re-read the DEPTH PACKAGE
     (design skill). Is the gameplay still basic (one mechanic, no
     combos/progression/power-ups/meta)? Then ADD the missing depth features
     NOW - combos, power-ups, currency, unlocks, variety, milestones - not
     just polish. A basic game is a failed game.
   - **PRO BAR pass (mandatory every cycle)**: look at each screen as if
     comparing it to a top mobile hit (engine skill, POLISH rules). Any screen
     still reading as a "web demo" gets its dedicated polish pass until it
     passes. Then re-check the PRECISE OBJECTIVES (design skill) on a real
     timed run - first reward, reward cadence, session length, peak-tension
     ending, unlock cadence. Anything below target is re-tuned NOW.
5. **RE-TEST** - re-run ALL gates and verifications from scratch for everything
   that changed.
6. **CONVERGENCE CHECK** - did this cycle find any issue (bug, polish, design,
   RICHNESS/density, DEPTH, spelling, alignment, juice)? If YES, loop again
   from step 1. If NO, the game is complete and can be delivered.

Keep a CHANGELOG.md / commit history showing the iterations - every cycle must
leave the game better than it was. Never deliver after a single pass. A game
is finished when a full loop finds NOTHING to fix.

---

## 7. USE SUBAGENTS - parallelize the work and get second eyes

The build has many independent tasks - delegate them to SUBAGENTS (the Task
tool). Subagents make the build faster AND more thorough: a second model
looking at the assets and the screens catches what the main agent misses.
Launch them in parallel whenever the tasks are independent.

Launch subagents for (parallelize whenever possible):

- **Asset verification & research**: one subagent per pack/source checks the
  downloads on disk (`ls` the real folders), validates licenses, prepares the
  CREDITS.md entries.
- **Vision verification**: subagents OPEN every asset and every screen
  screenshot with the image read tool and report exactly what they SEE -
  style, palette, resolution, transparency, empty zones, misalignment, cut
  text. A vision subagent checking each screen while the main agent builds the
  next one is the best defense against the AI look.
- **Code review**: a subagent re-reads every file line by line hunting dead
  code, magic numbers, missing `.catch`, leaked timers/tweens/listeners, state
  not reset on replay.
- **Testing**: subagents run test campaigns - every level end to end, every
  screen flow, every resolution of the test matrix, replay 10x - and report
  every error, console warning and visual defect found.
- **Playtest / objectives**: a subagent plays 2-3 full runs like a real player
  and reports against the PRECISE OBJECTIVES (first reward time, reward
  cadence, session length, peak-tension ending, unlock cadence).

Rules for subagents:

1. Give each subagent a PRECISE, bounded mission and the exact output to return
   (a list of findings, not opinions). Tell it explicitly whether it must
   WRITE code or ONLY report.
2. Vision subagents MUST open the images/screenshots with the image read tool
   and quote what they see - never summarize from memory.
3. Merge every subagent report into the relevant checklist/gate and FIX
   everything they find before moving on - a subagent finding that goes
   unfixed is a defect shipped.
4. Subagents report and verify; the main agent integrates their findings and
   makes the final decisions. Subagents never leave the build in a partial
   state (no half-applied changes, no untested edits).
5. Never skip subagents "to save time" - the vision and testing subagents in
   particular are mandatory at their gates (assets, screens, final campaign).
