---
name: casual-game-builder-design
description: Loaded by casual-game-builder at the design phase. Design the complete gameplay of a casual game: analyze the description, write the full GAMEDESIGN.md, apply HIT-GAME PATTERN LIBRARY + DEPTH PACKAGE + PRECISE OBJECTIVES, design all levels, write the interaction specification, and pass Gate A. Use on its own whenever the user asks to design a casual game's gameplay.
---

# Casual Game Builder - Game Design

This sub-skill produces the COMPLETE gameplay design (`GAMEDESIGN.md`). It is
loaded by the orchestrator skill at phase 1. **Nothing - no asset download, no
engine choice, no code - happens before Gate A passes.**

The deliverable is a COMPLETE, shippable game (all levels, all screens, full
progression, save system) - never a prototype.

---

## 1. Analyze the gameplay

Read the gameplay description carefully, then determine:

- **The THEME** (animals, fruits, space, cooking, magic...). Example: a merge
  game about pets → theme = cute animals.
- **The DIRECTION ARTISTIQUE (DA)** - deduced from the gameplay, never chosen
  randomly. Example: a calm puzzle → soft pastel cartoon. An action runner →
  bold saturated colors, dynamic shapes. A space game → dark background,
  neon accents.
- **The mechanics to implement** (jump, collect, drag, tap, merge, swipe...).
- **The main single action**: a casual game has ONE dominant action. Write it
  down in one sentence and keep it in mind for every screen.

## 2. Define the gameplay FIRST (before any asset download)

The gameplay definition drives EVERYTHING: it determines which assets to
download, which mechanics to code, which screens are needed. Never download
assets before the gameplay is fully defined. Steps:

### 0. SPEND THE TIME - this is the longest phase, not a formality

Writing the complete gameplay design is the most important work in the whole
build: it decides whether the game is good or broken. Take as long as needed
to write EVERYTHING below in precise detail - do not rush it, do not write
one line and move on. A game built on a rushed design document is a broken
or bland game; the hour spent here saves ten hours of bug fixing later. The
GAMEDESIGN.md you write must be the kind of document another developer
could rebuild the entire game from without asking you a single question.

### 1. Write the COMPLETE gameplay design document (`GAMEDESIGN.md`)

This is the blueprint of the WHOLE game, not a sketch. It must contain:

- The main action, the goal, the win condition, the lose condition.
- The COMPLETE rules: every object, every interaction, every edge case
  (what happens on tap/drag/collision/miss/timeout for EVERY element).
- The full game state machine (menu → gameplay → pause → victory → game
  over → replay) and how every piece of state is reset on replay.
- The scoring system: every point source, every combo and multiplier,
  exact numbers.
- The session length (tuned to the genre - short enough that "one more
  round" always feels cheap) and the "one more round" hook.
- The difficulty curve, number by number.
- The chosen orientation (portrait or landscape) with the reason.

It must be detailed enough that a fresh developer could rebuild the whole
game from this document alone. If the document is done, coding is just
execution.

### 2. Draw inspiration from proven successful casual mechanics

Do not invent a new mechanic for the first version. The best casual games are
variations of proven loops:

- Match-3 (Candy Crush) - swap & match tiles
- Merge (Merge Mansion) - drag identical items together to evolve them
- Runner / endless (Subway Surfers) - one-input obstacle avoidance
- Clicker / idle (Cookie Clicker) - tap for incremental gains
- Stack / balance (Stack) - tap to place blocks precisely
- Physics puzzle (Cut the Rope) - one-object interaction
- One-touch arcade (Flappy Bird) - single tap, perfect timing
- Tower defense / defense (Kingdom Rush) - place units, waves
- Memory / concentration - flip and match pairs
- Pull-the-pin, draw-the-path, slice-the-fruit - hyper-casual classics

Choose the closest proven archetype for the requested gameplay and adapt it
with the requested theme. A proven loop + a fresh theme = a strong casual game.

### 2b. Set PRECISE, MEASURABLE gameplay objectives - the "worldwide hit" bar

Do not design "good gameplay" vaguely: write down exact targets - YOUR OWN
numbers, tuned to your genre and your design - into GAMEDESIGN.md, then
verify each one on the running game (verification skill). This skill does NOT
impose fixed numbers: you set them, measure them, and tune them. Targets:

- **First reward moment early**: the player gets its first satisfying
  feedback (first collect / match / combo / point pop) almost immediately
  after touching the screen. No slow buildup - a hit hooks in the first
  seconds.
- **Reward cadence**: a steady rhythm of reward moments through the whole
  run. Long dead moments with no feedback = boring.
- **Session length**: tuned to the genre - short enough that "one more
  round" always feels cheap.
- **Peak-tension ending**: the run/level ends at the moment of highest
  tension, creating the instant "one more round" impulse. Never end on a
  quiet moment.
- **Measurable difficulty ramp**: write the ramp as concrete numbers
  (speed +X% per chapter, spawn density +Y%, a new obstacle every N
  rounds) - never "gets harder over time".
- **Unlock cadence**: the player regularly sees something NEW (new color,
  new obstacle, new shop item, new skin) so the game never feels the same
  twice.
- **Score pacing**: every action adds points (visible popups), milestones
  ("1000!", "Combo x5!"), the numbers visibly grow.
- **100% completeness**: EVERY feature in the design is built, wired and
  playable. Nothing marked "planned", "future", "stub" - a hit is not a
  roadmap.

### 2c. HIT-GAME PATTERN LIBRARY - apply proven patterns, never invent the loop

Before designing, study 3-5 real hits of the target genre (web search: `best
<genre> games`, `game design of <hit>`). Then apply the patterns they actually
use - the ones below are the recurring DNA of the biggest casual hits. Write
into GAMEDESIGN.md WHICH patterns you use and WHERE, then build them all:

- **First-session hook** (Candy Crush, Subway Surfers): the player gets a
  real win + a short-term goal + an unlock within the very first session.
  Never start a session with a bare empty board and no target.
- **Action → feedback → reward loop** (every hit): the core action repeats
  every few seconds and ALWAYS answers with sound + visual + points. A hit
  never lets 10+ seconds pass without a reward moment.
- **Meta loop** (Merge Mansion, Township): coins earned on every run (even
  failed ones), spent on upgrades/skins/perks. A loss still leaves the
  player richer → retention.
- **Gacha-lite reward moments** (Royal Match, Archero): chests, free daily
  reward, spin wheels, "claim" buttons. Predictable small dopamine hits on a
  timer.
- **One-more-round ending** (Flappy Bird, Ballz): the run ends at peak
  tension, not at a quiet moment - the instant retry reflex.
- **Near-miss / risk-reward** (Subway Surfers, Crossy Road): brushing
  danger pays a bonus - skillful risk is the fastest path to a high score.
- **Escalation chapters** (Vampire Survivors, Survivor.io): the run rises
  through named phases where speed + variety + rewards grow together.
- **Milestone popups** (every hit): named celebrations with sound at
  thresholds ("First combo!", "10,000!", "Level up!").
- **Star rating on level complete** (Candy Crush): 1-3 stars with exact
  thresholds, star-pop animation, rewards tied to stars.
- **Combo / multiplier** (Fruit Ninja, Jetpack Joyride): streaks build a
  visible multiplier with a pitch-up sound and a decay timer.
- **Power-up moments** (every runner): timed power-ups that change the feel
  for a few seconds (magnet, shield, slow-mo, double points).
- **Best-score chase** (every arcade hit): the end screen shows the record
  beaten or the near-miss gap to it - the #1 "one more round" trigger.

If a feature you designed matches none of these proven patterns, question it:
it is probably not what players expect from the genre. Proven loop + fresh
theme = strong game; invented loop = gamble.

### 3. Define the game feel (what makes it FUN and ADDICTIVE)

- A satisfying core loop that repeats every few seconds (collect → grow →
  upgrade → collect more)
- A clear short-term goal (beat your score) and a long-term goal (unlock
  everything)
- Escalating rewards: increasing numbers, unlocks, combos
- A "one more round" hook: the round ends just when the player wants one more
  try
- Juice: small visual/audio rewards on every action (this is why we download
  so many FX assets)

**The DEPTH PACKAGE - MANDATORY, this is what separates a real game from a
basic one.** A game with only its bare mechanic is a bad game. Design for YOUR
game at least 5-8 of the depth features below and write each one into
GAMEDESIGN.md with EXACT rules and numbers - then build ALL of them. A feature
in the design but not built is a blocker (Gate A). Features:

- **Combo / streak / multiplier**: consecutive good actions build a
  multiplier (x2, x3, x5...), shown on screen, with pitch-up sound. Exact
  window, exact decay, exact display.
- **Power-ups / special moves**: 2+ themed power-ups (magnet, slow-mo, extra
  life, double points, shield, bomb...), each with a duration and a visible
  effect. Where they drop, how rare, what they do.
- **Escalating difficulty events**: the run/level has 3-4 named "chapters"
  where speed and variety rise together, each ending in a milestone moment
  (mini-boss, wave, frenzy).
- **Currency + meta progression**: coins earned every run (even a failed
  one), spent in a shop on skins/upgrades/perks. THE retention driver - even
  a loss leaves the player richer and coming back.
- **Level/unlock progression**: levels with stars, unlock gates, a level bar
  that visibly fills toward the next thing.
- **Variety every few rounds**: a new obstacle, pattern, colorway or enemy
  every few runs - without ever adding a second mechanic.
- **Near-miss / risk-reward**: brushing danger pays a bonus ("Close call!
  +50"), making skillful risk the fastest path to a high score.
- **Milestones & achievements**: named popups with a sound at thresholds
  ("First combo!", "10,000!", "Level 5!", "5 in a row!").
- **Best-score chase**: the end screen shows the record beaten or the
  near-miss gap to it - the #1 "one more round" trigger.
- **Juice**: every action pops (score popup, shake, flash, sound) - tune so
  the BEST action feels the BEST.

When designing, ask "what would a professional version of this game have?"
and ADD it. The user's description is the seed, never the ceiling.

### 4. Design ALL the levels NOW (before any asset download)

A complete game has a full progression, not one random level:

- Fix the total number of levels - a complete progression, sized by the
  game. A level-based game gets as many levels as its difficulty curve needs;
  quality over count.
- For EACH level: name, objective, the star criteria (1-3 stars with exact
  thresholds), the layout (where entities spawn, what obstacles exist), the
  difficulty parameters (speed, frequency, health), and what unlocks after it.
- Define the difficulty curve across the whole game: gentle start, smooth
  rise, a peak of tension at the end of each level and at the final level.
- For endless/hyper-casual games, define the full run pacing instead: exactly
  when speed and density rise, the 3-4 "chapters" of a run, and the
  end-of-run reward moment.
- Document everything in `GAMEDESIGN.md`.

### 5. Write the full interaction specification

A table of every game object → every possible player action → the exact result
(position, score delta, sound, effect, next state). Cover ALL edge cases (two
simultaneous taps, dragging out of bounds, spawn overlap, timing out, repeated
clicks). This is the document that prevents half-finished mechanics.

### 6. Only now derive the asset list from the defined gameplay

This step is a REAL ANALYSIS, not a quick guess - walk through the ENTIRE game
in your head and enumerate every single asset the screens need:

- Per screen: background (loading, menu, gameplay, victory, game over).
- Per mechanic: the player sprite + EVERY animation frame set (idle, walk,
  run, jump, attack, death...), every obstacle/enemy, every pick-up, every
  power-up, every platform/tile.
- Per screen element: every button in its 3 states, every icon (score digits,
  hearts, coins, timer), the logo, the loader figure, panel frames, popups
  ("+10", "Perfect!", "Combo!"), particles/confetti.
- Per action: the sound (collect, jump, click, match, victory, defeat,
  fanfare, ambient) and the music loop.
- Plus: the font (self-hosted), the favicon.

Every asset must answer a gameplay need - if an asset does not serve the
gameplay, it does not belong. Write this full list in GAMEDESIGN.md and tick
each item as it is downloaded and VISION-approved.

---

## The GAMEPLAY WRITING ITERATION - rewrite until EVERY objective is ticked

Writing the gameplay is done in LOOPS, never in one draft. This is the phase
that decides if the game is a hit or a dud - give it real time and real
rewrites. Process:

1. **Write the full first draft** of GAMEDESIGN.md (steps 1-6 above).
2. **HOSTILE REVIEW**: read your own draft as a hostile designer and attack
   it. Ask, per feature: is this as good as what the real hits do? Is there an
   objective below target? Is any part vague ("fun", "nice", "more")? Would
   this survive 2 hours of play? Mark every weakness.
3. **REWRITE, do not patch**: fix every marked weakness by rewriting the
   affected sections - not by adding a line of justification. A vague
   difficulty curve becomes exact numbers; a missing power-up gets designed
   fully; a weak meta gets a real shop.
4. **TICK THE OBJECTIVE CHECKLIST** (100% before going further - this is the
   loop's exit condition):
   - [ ] Core loop, rules, edge cases, state machine - complete
   - [ ] Scoring exact; difficulty curve numeric; session length set
   - [ ] DEPTH PACKAGE: 5-8 features, each with exact rules
   - [ ] PRECISE OBJECTIVES: early first reward, steady reward cadence,
         session length tuned to the genre, peak-tension ending, unlock
         cadence, score milestones, 100% completeness
   - [ ] Hit-game techniques applied (from the research)
   - [ ] ALL levels designed (a complete progression, not a fixed count) with
         stars, layout, difficulty params
   - [ ] Asset list complete and sized with DENSITY rules (as many real
         assets as the gameplay demands)
   - [ ] A fresh developer could rebuild the whole game from this document
         with ZERO questions
5. **LOOP CHECK**: any box unticked, any vague line, any "I will figure it
   out later"? If YES, go back to step 3 and rewrite. If NO, the design passes
   (Gate A) and coding can start.

The design is the contract - everything that follows is execution. Never start
coding, downloading assets or choosing the engine with an unticked box.

---

## Quality guarantees (inform the design)

The game must also satisfy these casual-game quality guarantees (the
verification skill checks them at the end):

- Comprehensible in <30s, no tutorial, ONE main mechanic.
- Short sessions (tuned to the genre - short enough that "one more round"
  feels cheap), "one more round" loop.
- Immediate gratification: visual + sound feedback on EVERY action.
- Non-punitive: no harsh game over, recoverable mistakes, smooth difficulty
  curve (easy start, gentle rise).
- Light and fast: fast load (ideally near-instant), 60fps, works on weak
  mobile devices and low-end browsers.
- Universal themes: animals, fruits, space, cooking, magic...
- English text only.
- Hidden depth: easy to learn, skill to master, score/combo system that
  rewards mastery.

### The game must be FUN and ADDICTIVE (playable for many hours)

A beautiful game nobody wants to play again is a failure. Before finishing,
ask: "would I want to play this for 2 hours straight?" If not, fix the
gameplay. Key addictiveness principles:

- **The core loop must be satisfying in under 10 seconds**: the player reaches
  the first reward moment (first match, first collect, first combo)
  immediately. Slow games lose players.
- **Progression with every round**: each run leaves the player slightly
  stronger or richer (coins, unlocked skins, higher best score). Even a failed
  run must not feel wasted - small consolation gains keep people coming back.
- **Escalating tension**: speed, difficulty and rewards rise together. The
  game ends at a moment of high tension, creating the "one more round" impulse
  (this is what the difficulty curve is for: die at the peak, not at a random
  point).
- **Combos and near-misses**: reward streaks (x2, x3, x5 multipliers) and let
  players brush against failure - near-misses are proven to make players retry
  immediately.
- **Visible long-term goals**: a shop to unlock, levels to clear, a level bar
  filling - the player always knows "what comes next" beyond the next round.
- **Variety without complexity**: introduce small variations (new obstacle,
  new color, faster pace) every few rounds so the game never feels repetitive,
  without ever adding a second mechanic.
- **Juice everywhere**: every score gain pops, every action has a sound, the
  screen pulses on milestones. This is not decoration - it IS the addiction.
- **Short runs, tuned to the genre**: short enough that "just one more" always
  feels cheap to the player.
- **Never frustrate the player**: no unavoidable deaths, no difficulty spikes,
  no punishing penalties. The difficulty comes from the player's own mastery,
  not from the game being unfair.

---

## Gate A - Game design done (GAMEDESIGN.md complete)

- [ ] Main action, goal, win/lose written and unambiguous
- [ ] Complete rules: every object x every interaction x every edge case
- [ ] Full state machine (menu -> gameplay -> pause -> victory -> game over ->
      replay) with exact reset rules
- [ ] Scoring with exact numbers (every point source, combo, multiplier)
- [ ] Session length + "one more round" hook defined
- [ ] Difficulty curve, number by number
- [ ] Orientation chosen with the reason
- [ ] ALL levels designed (a complete progression) with star criteria, layout
      and difficulty parameters - or full run pacing for endless games
- [ ] Interaction specification table complete (all edge cases)
- [ ] Core loop written in ONE sentence (action -> feedback -> reward -> repeat)
- [ ] PRECISE OBJECTIVES set (your own tuned numbers): early first reward,
      steady reward cadence, session length tuned to the genre, peak-tension
      ending, numeric difficulty ramp, unlock cadence, score milestones,
      100% completeness
- [ ] Hit-game research done: the top hits of the genre studied and their
      concrete techniques applied (menu, onboarding, retention, difficulty)
- [ ] DEPTH PACKAGE designed: at least 5-8 of (combo/multiplier, 2+ power-ups,
      escalating chapters, currency + meta/shop, level/unlock progression,
      variety events, near-miss, milestones, best-score chase, juice) - each
      with exact rules and numbers
- [ ] Asset list derived from the gameplay, complete (sized with DENSITY
      rules, engine skill: 3+ parallax planes, 3+ enemy types, 2+ power-ups,
      per-action FX - sized to the gameplay's real needs)
- [ ] A fresh developer could rebuild the whole game from this document with
      ZERO questions

One unchecked box means the design is NOT done - rewrite, do not skip. Re-run
the gate whenever anything in the design changes.
