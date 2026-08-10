---
name: casual-game-builder-design
description: Loaded by casual-game-builder at the design phase. Design the COMPLETE gameplay of an original hybrid-casual game. The agent invents the idea and the gameplay itself (never copied from an existing game), then iterates with a hit-potential score until the design reaches the target. Produces the full GAMEDESIGN.md: scene-by-scene breakdown, exact rules and numbers, complete asset list (sprites, sounds, music, FX), level design, interaction specification. Use on its own whenever the user asks to design a casual game's gameplay.
---

# Casual Game Builder - Game Design

This sub-skill produces the COMPLETE gameplay design (`GAMEDESIGN.md`). It is
loaded by the orchestrator skill at phase 1. **Nothing - no asset download, no
engine choice, no code - happens before Gate A passes.**

The deliverable is a COMPLETE, original, deep, hybrid-casual game design - the
kind of document a fresh developer could rebuild the whole game from with ZERO
questions. It is written in ITERATIONS with a hit-potential score, exactly like
an experienced game designer would work, never in a single draft.

---

## 0. Fixed requirements (built into the skill - the user never has to repeat them)

The following are NON-NEGOTIABLE design requirements. They are baked into this
skill and every design must satisfy all of them:

1. **THE IDEA COMES FROM THE AGENT.** No user-supplied starting idea. The agent
   invents the theme, the mechanic, the characters, the world - everything -
   by itself. Never ask the user.
2. **ORIGINALITY IS ABSOLUTE (anti-clone).** The surface mechanic must NOT
   resemble an existing game. Platforms like Playgama reject games that look
   like ones already on the platform - they are actively looking for NEW
   gameplay. The agent must consciously AVOID the known archetypes (match-3,
   merge, runner, flappy-tap, tower defense, idle clicker...) unless the core
   action is genuinely reinvented. If the mechanic could be described as
   "it's like [existing game]", it fails this requirement.
3. **PROVEN PSYCHOLOGY UNDER AN ORIGINAL SURFACE.** Originality applies to the
   visible mechanic. The psychology underneath - reward loops, retention,
   tension, progression - uses the keys that made hit games hits (section 3).
   Original surface + proven psychology = a real hit potential. Original
   surface + nothing = a gamble.
4. **HYBRID CASUAL IS MANDATORY.** The design MUST combine: (a) a simple,
   instantly-learned casual core loop, (b) real meta depth (currency, shop,
   upgrades, skins, progression) so the player keeps coming back, and (c) a
   monetization-friendly structure (session length, reward cadence, hooks).
   A bare-mechanic game is a FAILED design.
5. **DEPTH IS MANDATORY.** At least 5-8 depth features (combo/multiplier, 2+
   power-ups, escalating chapters, currency + meta, levels/unlocks, variety
   events, near-miss, milestones, best-score chase, juice) - each with exact
   rules and numbers. A feature in the design but not built is a blocker.
6. **GAMEPLAY IS ULTRA-DETAILED PER SCENE.** Every scene is broken down with
   exact layout, behaviors, timings and its complete asset list (sprites,
   sounds, music, FX, UI, buttons, particles) - see section 5.
7. **ITERATION WITH A HIT-POTENTIAL SCORE.** The design is written, scored
   (section 7), critiqued, rewritten and re-scored in a loop until it reaches
   the target score. A single draft is never accepted.
8. **SIMULATIONS ARE MANDATORY.** The agent mentally simulates play sessions
   minute by minute to verify fun, reward cadence, tension and retention
   (section 6) - and updates the design from what the simulation reveals.
9. **REASON LIKE A HUMAN EXPERT: CATEGORY FIRST.** The agent thinks and works
   the way a senior web-game designer does: it FIRST chooses the category (or
   1-2 hybrid categories) the game belongs to, then reads that category's
   constraints (verbs, loops, depth, pitfalls) and designs WITHIN them (section
   1). Never invent in the void - choose a category, apply its rules, then
   create.
10. **REAL-WORLD ANCHOR + ONE TWIST (anti-fantasy).** The idea must start from
    a relatable real-world context (kitchen, delivery, barber, cafe, garden,
    workshop, post office, bakery - or a real everyday situation) that everyone
    understands in 2 seconds and that is warm and visually rich. Originality
    comes from ONE single mechanical twist on top of that real context
    ("you deliver parcels... but the parcels are alive and escape"). One twist
    = original. Several twists = fantasy mess. If the idea has no real-world
    anchor or needs more than one twist to be interesting, it fails.
11. **NOT FANTASY, NOT UGLY.** The theme must be grounded (no generic space/
    dragon/wizard/fairy fantasy unless the mechanic genuinely requires it) and
    must have a clearly describable BEAUTIFUL art direction (a named style + a
    concrete palette). If you cannot describe a gorgeous look in one sentence,
    the idea is ugly - redo it.

---

## 1. Invent the idea (origin is free, quality is not)

The idea must be generated by the agent. Follow this process - it is the
reasoning of an experienced web-game designer, not a random brainstorm:

### 1.0 CHOOSE THE CATEGORY FIRST (never invent in the void)

Before any idea, the agent picks the category (or 1-2 hybrid categories) the
game belongs to. This is the "expert web-game coder" way of thinking: the
category tells you what works, what is overdone, what verbs to use, what depth
to add. Then the agent creates WITHIN the chosen category - never against it.

Category library - pick ONE primary category, and (for the mandatory hybrid)
optionally ONE secondary category whose depth features you graft onto the
primary. For the chosen category(ies), read and APPLY the constraints:

**CATEGORY 1 - Puzzle & Logic**
- Description: sorting, matching, linking, arranging, planning moves.
- Core satisfaction: "everything fits together" - the clean solve.
- Verbs: sort, match, pair, link, arrange, rotate, group, merge logically.
- Constraints: single clear objective per level; rules that escalate as levels
  advance; the solution must be reachable (never a luck-based win); star rating
  on the solve.
- What makes a hit here: instant clarity of the goal, satisfying "click" when
  pieces fall into place, level variety without new mechanics.
- Overdone/pitfalls to AVOID: match-3 swap clones (Candy Crush), square-match
  clones - if your idea is "match-3 with a new skin", fail it.

**CATEGORY 2 - Action & Reflex**
- Description: timing, dodging, aiming, quick decisions under pressure.
- Core satisfaction: "perfect timing" - the clutch move.
- Verbs: tap, time, dodge, aim, shoot, avoid, steer.
- Constraints: one dominant input first; speed ramp is the difficulty (never
  random spawns); near-miss risk-reward built in; runs are short (45-90s).
- What makes a hit here: the "one more round" reflex, near-misses, escalating
  speed + variety.
- Overdone/pitfalls to AVOID: endless-runner clones (Subway Surfers), flappy
  clones, one-tap-instant-death clones - if the mechanic can be described as
  "it's like [runner/flappy]", fail it.

**CATEGORY 3 - Time Management & Service (the "real job" category)**
- Description: run a real-world service (cafe, delivery, barber, kitchen,
  post office, workshop) with queues and priorities.
- Core satisfaction: "flow state" - everything served smoothly, no angry
  customer.
- Verbs: serve, queue, deliver, prep, chop, pour, assemble, satisfy.
- Constraints: the setting is a REAL everyday job + ONE twist; customers must
  wait (patience meter) but never be blocked unfairly; upgrades = speed and
  capacity; combo = chained perfect serves.
- What makes a hit here: warm universal theme, visible progress (shop grows),
  the "just one more customer" hook, extremely rich and cute asset potential.
- Overdone/pitfalls to AVOID: Diner-Dash / Papa's-clone layouts, generic
  restaurant sim clones - the twist must change HOW you play, not the decor.

**CATEGORY 4 - Physics & Skill**
- Description: aiming, launching, bouncing, balancing, sliding - mastery of
  physics.
- Core satisfaction: "physical mastery" - I made the perfect shot.
- Verbs: launch, bounce, balance, stack, slide, tilt, pour, knock.
- Constraints: believable (fun, not realistic) physics; one tool, many uses;
  trajectories shown subtly; skill = the only difficulty (never randomness).
- What makes a hit here: expressive physics feedback, slow-mo/replay of the
  perfect shot, near-miss.
- Overdone/pitfalls to AVOID: Angry-Birds clones, Stack clones, physics-knock
  clones - combine TWO verbs (e.g. launch + balance) to be new.

**CATEGORY 5 - Strategy-lite & Defense**
- Description: place units, build small, survive waves - light strategy.
- Core satisfaction: "my plan worked" - I outsmarted the wave.
- Verbs: place, build, upgrade, position, deploy, defend.
- Constraints: few unit types (max 4-5); waves escalate on a readable pattern;
  no complex tech trees; a run is 3-5 minutes max; near-miss = "the wave almost
  broke through".
- What makes a hit here: visible growing power, clean wave rhythm, the
  "one more wave" hook.
- Overdone/pitfalls to AVOID: classic tower-defense clones (Kingdom Rush),
  bloated strategy UI - keep it casual.

**CATEGORY 6 - Idle & Incremental**
- Description: tap, earn, buy upgrades, watch numbers grow.
- Core satisfaction: "my numbers grow" - the climb.
- Verbs: tap, earn, buy, upgrade, hire, prestige.
- Constraints: numbers are the fun - show them popping everywhere; upgrades
  must be visible (the machine/shop visibly changes); a prestige loop; short
  active bursts with a satisfying return.
- What makes a hit here: big-number dopamine, visible growth, unlock cadence.
- Overdone/pitfalls to AVOID: cookie-clicker clones, generic tap-to-earn - add
  a real decision layer so it is not a mindless counter.

**CATEGORY 7 - Merge & Collection**
- Description: drag identical items together to evolve them; collect a full
  set.
- Core satisfaction: "my collection grows and evolves".
- Verbs: merge, drag, evolve, collect, combine, feed.
- Constraints: the merge chain must be readable (levels of evolution);
  collection goals drive long-term play; rare drops for excitement; a NEW merge
  interaction twist is required (never plain drag-drop).
- What makes a hit here: collection completion, evolving visuals, daily goals.
- Overdone/pitfalls to AVOID: Merge-Mansion clones, merge-palace clones - the
  twist must be mechanical, not thematic.

**CATEGORY 8 - Simulation & Lifestyle**
- Description: decorate, build, care for a small warm world.
- Core satisfaction: "my world" - it is mine and it is beautiful.
- Verbs: decorate, build, plant, care, style, organize.
- Constraints: warm, cozy, beautiful art is the product (ugly = fail); the
  player's choices visibly change the world; daily habits (visit, water, feed)
  create the return loop; never punitive (a neglected world is sad, not dead).
- What makes a hit here: emotional attachment, personalization, gentle daily
  hooks.
- Overdone/pitfalls to AVOID: FarmVille/Township layout clones, energy-wall
  gating that blocks play.

**CATEGORY 9 - Arcade & Hyper-casual**
- Description: minimal, instantly understood, score-chase action.
- Core satisfaction: "instant fun, high score".
- Verbs: any ONE verb done perfectly - tap, swipe, steer, balance, aim.
- Constraints: ONE button max; understood in <3 seconds; run 30-90s; the
  best-score chase is the ONLY long-term loop unless you add meta depth (which
  upgrades it to hybrid-casual - the goal).
- What makes a hit here: purity of the hook, one-more-round, score sharing.
- Overdone/pitfalls to AVOID: every hyper-casual clone ever - if it feels like
  something on a 2018 ad banner, fail it. A hyper-casual idea MUST be upgraded
  with a meta layer to reach hybrid-casual depth.

HYBRID rule: because hybrid-casual is mandatory (requirement 4), pick the
primary category for the CORE action and a secondary category for the DEPTH
(the meta loop comes from a different category, e.g. an Action & Reflex core
with a Simulation & Lifestyle shop/world, or a Time Management core with a
Merge & Collection collection). Record: "Primary category: X. Secondary
category: Y. Depth from Y: [which features]."

### 1.1 Generate a broad idea pool WITHIN the category

- Generate 5-8 DIFFERENT original game ideas (one line each). Each must obey
  the chosen category's constraints AND the real-world + one twist rule (10).
  For each idea: a different mechanic (never a variation of the same) and a
  different real-world setting.
- Combine TWO verbs from the category to force innovation (e.g. serve + balance,
  launch + pair, sort + time). Innovation comes from the VERB MASHUP and the
  ONE twist - never from a weird theme.
- For each idea, answer in one line: the SINGLE main action, WHY it is
  different from existing games, and the ONE twist.
- Rank the pool. Pick the strongest - the one that is BOTH original AND has the
  best hook potential. Never pick the safest.

### 1.2 Kill-check the chosen idea (the anti-clone test)

For the chosen idea, write honestly: "This is NOT like [existing game] because
[reason]." Be brutal. If you cannot name a real difference in the CORE action
- not the theme, the CORE ACTION - the idea fails. Go back to the pool and
pick or generate another. Repeat until the core action is genuinely original.
Also apply the ANTI-FANTASY test (real-world anchor + exactly ONE twist) and
the BEAUTY test (a gorgeous look describable in one sentence, requirement 11).

### 1.3 The hook-line test (would a stranger want to play it?)

Write the concept as ONE catchy line ("You run a cat barbershop where every
cat wants a different crazy style - before the next cat arrives"). Then answer
honestly: would someone reading this line feel like playing? If the line is
flat, generic or unexciting, the IDEA is flat - go back to the pool. The hook
line must be so inviting that a player clicks the thumbnail just to try it.
Also describe the SINGLE MOST SELLING screenshot (the store thumbnail): what is
on screen, what is the player doing, why is it beautiful and intriguing? If you
cannot picture a beautiful, attractive thumbnail, the idea cannot look good on
the platform - fail it.

### 1.4 Design the core loop FIRST (before anything else)

Define in ONE sentence the core loop: action -> feedback -> reward -> repeat.
Then write the exact details:

- The single dominant player action (tap, drag, swipe, tilt, shoot, aim,
  rotate, time, stack, draw, serve...) - ONE action, instantly understood.
- The goal of a run/level, the win condition, the lose condition.
- Why it is fun in the first 10 seconds (first reward almost immediately).
- Why it stays fun for hours (depth features, section 4).

---

## 2. Set the frame (theme, art direction, orientation, session)

- **THEME**: derived from the mechanic and its real-world anchor - whatever
  makes the original mechanic feel alive and readable. Never random.
- **ART DIRECTION (DA)**: deduced from the gameplay and theme, and stated as
  ONE precise sentence with a NAMED style reference and a concrete palette
  (e.g. "rounded cartoon in the style of Fruit Ninja, warm 4-color palette,
  cute smiling characters"). The BEAUTY TEST: if the DA sentence is vague or
  you cannot picture it, the look is not defined - rewrite it until a gorgeous,
  coherent look is unmissable (requirement 11). A calm puzzle -> soft pastel
  cartoon. An action game -> bold saturated colors. A warm service game -> cozy
  warm tones. Never random, never generic.
- **ORIENTATION**: portrait or landscape, with the reason tied to the
  mechanic (one-handed thumb play -> portrait; wide aiming/horizon -> landscape).
- **SESSION LENGTH**: tuned to the genre - short enough that "one more round"
  always feels cheap. Written as an exact target (e.g. 45-90 seconds per run).

---

## 3. Proven hit psychology - the keys that made hits hit

These are the psychological drivers behind the biggest casual hits. Apply ALL
of them to the design and write into GAMEDESIGN.md WHERE each one is used:

- **First-session hook**: a real win + a short-term goal + an unlock within
  the very first session. Never a bare empty board with no target.
- **Action -> feedback -> reward loop**: the core action repeats every few
  seconds and ALWAYS answers with sound + visual + points. A hit never lets
  10+ seconds pass without a reward moment.
- **Immediate gratification**: visual + sound feedback on EVERY action.
- **Reward cadence**: a steady rhythm of reward moments through the whole run.
  Long dead moments with no feedback = boring = dead game.
- **Escalating rewards**: increasing numbers, unlocks, combos. The best moment
  of the run is the last moment.
- **One-more-round ending**: the run ends at peak tension, not at a quiet
  moment - the instant retry reflex.
- **Meta loop**: coins earned on every run (even failed ones), spent on
  upgrades/skins/perks. A loss still leaves the player richer -> retention.
- **Near-miss / risk-reward**: brushing danger pays a bonus - skillful risk is
  the fastest path to a high score.
- **Milestone popups**: named celebrations with sound at thresholds ("First
  combo!", "10,000!", "Level up!").
- **Best-score chase**: the end screen shows the record beaten or the
  near-miss gap to it - the #1 "one more round" trigger.
- **Unlock cadence**: the player regularly sees something NEW (new color, new
  obstacle, new shop item, new skin) so the game never feels the same twice.
- **Variety without complexity**: small variations (new obstacle, new pattern,
  faster pace) every few rounds, WITHOUT ever adding a second mechanic.
- **Non-punitive**: no harsh game over, recoverable mistakes, smooth difficulty
  curve, no unavoidable deaths. Difficulty comes from mastery, not unfairness.
- **Gacha-lite reward moments**: chests, daily reward, spin wheels, "claim"
  buttons - predictable small dopamine hits on a timer.

For every feature designed, ask: "which proven psychology does this satisfy?"
If a feature satisfies none, it is probably not what players expect - question
or remove it.

---

## 4. The DEPTH PACKAGE (mandatory - 5-8 features, each fully designed)

A game with only its bare mechanic is a bad game. Design for YOUR game at least
5-8 of the features below and write EACH into GAMEDESIGN.md with EXACT rules
and numbers - then build ALL of them:

- **Combo / streak / multiplier**: consecutive good actions build a multiplier
  (x2, x3, x5...), shown on screen, pitch-up sound. Exact window, exact decay,
  exact display.
- **Power-ups / special moves**: 2+ themed power-ups (magnet, slow-mo, extra
  life, double points, shield, bomb...), each with a duration and a visible
  effect. Where they drop, how rare, what they do.
- **Escalating difficulty events**: 3-4 named "chapters" in a run/level where
  speed and variety rise together, each ending in a milestone moment.
- **Currency + meta progression**: coins earned every run (even a failed one),
  spent in a shop on skins/upgrades/perks. THE retention driver.
- **Level/unlock progression**: levels with stars, unlock gates, a level bar
  that visibly fills toward the next thing.
- **Variety events**: a new obstacle, pattern, colorway or enemy every few
  runs.
- **Near-miss / risk-reward**: brushing danger pays a bonus ("Close call! +50").
- **Milestones & achievements**: named popups with sound at thresholds.
- **Best-score chase**: the record beaten or the near-miss gap shown at the end.
- **Juice**: every action pops (score popup, shake, flash, sound) - tuned so
  the BEST action feels the BEST.

---

## 5. Ultra-detailed scene-by-scene breakdown (the "many info" requirement)

For EVERY scene of the game, write a dedicated subsection in GAMEDESIGN.md
containing ALL of the following (this is the requirement for lots of detail -
never a short paragraph, never vague):

1. **Scene purpose**: what it does for the player and for retention.
2. **Exact layout**: every element's position, size, z-order, alignment.
   Described precisely enough to reproduce without the screenshot.
3. **Behaviors**: every object's behavior, animations, timings, transitions
   into and out of the scene.
4. **States**: every UI state (normal, hover, pressed, disabled, active).
5. **Complete asset list** for the scene - write it as a table with columns:
   Asset | Type (sprite/button/icon/panel/particle/audio/music/FX) | Where used | Size/format. Cover EVERYTHING:
   - background (full scene, with parallax layers if applicable)
   - logo, title treatment
   - every button in its 3 states
   - every icon (score digits, hearts, coins, timer, currencies)
   - panel frames, popups ("+10", "Perfect!", "Combo!", "New record!")
   - particles / confetti / screen FX
   - sounds (click, collect, milestone, victory, defeat, ambient)
   - music (per scene)
   - fonts, favicon, loading assets
6. **Edge cases** for that scene: rapid taps, double-click, out-of-bounds,
   repeated triggers, timing out.

The SIX mandatory scenes (from the engine sub-skill): loading / menu /
gameplay / pause / victory / game over (+ optional: shop, settings, level
select). The gameplay scene MUST also contain the player sprite and EVERY
animation frame set, every obstacle/enemy type, every pick-up, every
power-up, every platform/tile.

Every asset must answer a gameplay need - if an asset does not serve the
gameplay, it does not belong. The asset lists from ALL scenes are merged into
the game-wide asset manifest (assets sub-skill, ASSETS.md).

---

## 6. Mandatory simulations (play the game in your head before it exists)

Simulation is how an experienced designer verifies a game before coding it.
Run these for EVERY design iteration and write the results into GAMEDESIGN.md:

1. **First-contact simulation**: simulate the first 60 seconds of a brand-new
   player. Tick by tick: what does the player see, feel, do? When is the first
   reward moment? Is the game understood in <30 seconds with no tutorial?
2. **Full-run simulation**: simulate a complete run minute by minute (or second
   by second for short runs). Write it as a timeline: 0-10s / 10-30s / 30s-1min
   / ... What rewards happen? When does tension rise? Does the run end at peak
   tension?
3. **Retention simulation**: simulate 5 sessions over 5 days. Session 1: hook
   and first unlock. Sessions 2-5: what brings the player back? Meta goals,
   daily reward, new unlock, new obstacle? Is every session rewarding?
4. **Failure simulation**: simulate a failed run. Is it still rewarding
   (consolation coins, progress)? Is the death/failure moment at a peak of
   tension (one-more-round reflex) and never unfair?
5. **Monetization simulation**: simulate the ad/reward moments - are they at
   natural pauses, never during active gameplay, always skippable?

After each simulation, UPDATE the design with what the simulation revealed.
If a simulation shows boredom, a dead moment, an unfair death or a missing
hook - fix the design, not the description.

---

## 7. The hit-potential score - iterate until the design IS a hit design

Writing the design is done in LOOPS, exactly like an experienced developer
works: write -> score -> critique -> rewrite -> re-score - until the score
reaches the target. A single draft is NEVER accepted.

### 7.1 The scoring grid (score each dimension 0-10)

| Dimension | What it measures |
|-----------|------------------|
| Originality | Core action genuinely new, not a clone (0 = "it's like [game]", 10 = nobody has played this) |
| Fun in 10s | First reward within seconds, instantly satisfying core loop |
| Reward cadence | Steady rewards, no dead moments, best moment at the end |
| Retention depth | Meta loop, unlocks, one-more-round hook, non-punitive failures |
| Hybrid-casual fit | Casual core + real meta depth + monetization-ready structure |
| Beauty & appeal | Gorgeous, describable art direction; warm real-world anchor; one twist; a thumbnail you would click (10 = beautiful, intriguing, not fantasy-cliche, not ugly) |
| Difficulty curve | Numeric ramp, gentle start, peaks at level ends, no unfairness |
| Completeness | Every scene ultra-detailed, every asset listed, no "planned" left |
| Simulated playtest | All simulations passed with no boring/unfair/empty findings |

### 7.2 Iteration loop (mandatory, never skipped)

1. **Write the full first draft** of GAMEDESIGN.md (sections 1-6).
2. **Score it**: apply the grid, get a total (max 90). Write the score into
   the document.
3. **HOSTILE REVIEW**: read the draft as a hostile designer. Attack every
   dimension: is the core action REALLY original? Is the look REALLY beautiful?
   Is any objective below target? Any vague line ("fun", "nice", "more",
   "eventually")? Would this survive 2 hours of play? A fresh developer with
   zero questions? Mark every weakness.
4. **REWRITE, do not patch**: fix every marked weakness by rewriting the
   affected sections - not by adding a line of justification. Vague difficulty
   -> exact numbers. Missing power-up -> fully designed. Weak meta -> real shop.
   Ugly or generic theme -> re-anchor in the real world with a stronger twist.
5. **RE-SCORE** and record. Target: **total >= 78/90 with NO single dimension
   below 7** (Beauty & appeal included - a design that is not beautiful fails).
   Below target = rewrite again and repeat. Record every score in the document
   so the improvement is visible.
6. **CONVERGENCE**: a full loop finds NOTHING below 7 and total >= 78 ->
   the design passes (Gate A) and coding can start.

Every iteration must leave the design strictly better. If two consecutive
iterations do not raise the score, change the approach (new mechanic, new
hook) instead of polishing a weak idea - an experienced designer kills a bad
idea early rather than polishing it.

---

## 8. The COMPLETE GAMEDESIGN.md - mandatory contents checklist

The final document MUST contain every section below (this is the "lots of
information" requirement - a short document fails):

- [ ] The core loop in ONE sentence (action -> feedback -> reward -> repeat)
- [ ] Main action, goal, win condition, lose condition - unambiguous
- [ ] Category chosen (primary + optional secondary hybrid) with the chosen
      category's constraints applied (section 1.0)
- [ ] Real-world anchor + ONE twist, anti-fantasy test passed
- [ ] Hook line (one catchy sentence) + the most selling screenshot described
- [ ] Theme + art direction + orientation (with reasons); DA in ONE precise
      sentence with a named style reference and a concrete palette (beauty test)
- [ ] Session length + "one more round" hook (exact numbers)
- [ ] COMPLETE rules: every object x every interaction x every edge case
- [ ] Full state machine (menu -> gameplay -> pause -> victory -> game over ->
      replay) with exact reset rules
- [ ] Scoring: every point source, every combo and multiplier, exact numbers
- [ ] Difficulty curve, number by number (speed +X% per chapter, spawn
      density +Y%, new obstacle every N rounds)
- [ ] PRECISE OBJECTIVES set (own tuned numbers): early first reward, steady
      reward cadence, session length, peak-tension ending, unlock cadence,
      score milestones, 100% completeness
- [ ] DEPTH PACKAGE: 5-8 features, each with exact rules and numbers
- [ ] ALL levels designed (a complete progression) with star criteria, layout
      and difficulty parameters - or full run pacing for endless games
- [ ] Scene-by-scene breakdown for EVERY scene (section 5): layout, behaviors,
      states, edge cases + COMPLETE asset table (sprites, buttons, icons,
      panels, popups, particles, sounds, music, fonts, favicon)
- [ ] Interaction specification table: every game object -> every action ->
      exact result (position, score delta, sound, effect, next state)
- [ ] Simulation results recorded (first-contact, full-run, retention,
      failure, monetization)
- [ ] Hit-potential score history (each iteration's score, all dimensions)
- [ ] Which hit psychologies are used and WHERE
- [ ] A fresh developer could rebuild the whole game from this document with
      ZERO questions

---

## Gate A - Game design done (GAMEDESIGN.md complete)

- [ ] Idea generated by the agent (no user input), with the anti-clone
      kill-check passed: core action is genuinely original
- [ ] Category chosen (primary + optional secondary hybrid) and its constraints
      applied (section 1.0)
- [ ] Real-world anchor + ONE twist; not fantasy-cliche; hook line + selling
      screenshot described
- [ ] Beauty test passed: DA in ONE precise sentence (named style + concrete
      palette), a gorgeous coherent look, nothing ugly or generic
- [ ] Proven hit psychology applied and located (section 3)
- [ ] Hybrid casual: casual core + real meta depth + monetization-ready
- [ ] DEPTH PACKAGE: 5-8 features designed with exact rules
- [ ] Ultra-detailed scene-by-scene breakdown with COMPLETE asset tables
      (sprites, sounds, music, FX, UI) for EVERY scene
- [ ] Simulations (first-contact, full-run, retention, failure, monetization)
      run and recorded - all pass with no boring/unfair/empty findings
- [ ] Hit-potential score history recorded; final score >= 78/90 with NO
      dimension below 7 (Beauty & appeal included)
- [ ] Complete rules, state machine, scoring, difficulty curve - all numeric
- [ ] ALL levels designed with stars, layout, difficulty params
- [ ] Asset list complete and sized with DENSITY rules
- [ ] English texts only; no vague lines; no "planned/future/stub"
- [ ] A fresh developer could rebuild the whole game from this document with
      ZERO questions

One unchecked box means the design is NOT done - rewrite, do not skip. Re-run
the gate whenever anything in the design changes.
