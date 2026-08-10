---
name: casual-game-builder
description: Create complete, beautiful casual web games for Playgama from a gameplay description. Use when the user provides a game idea or gameplay text and wants a casual game built, with pre-made assets, audio, monetization, GitHub repo and responsive design. This is the ORCHESTRATOR skill - it loads specialized sub-skills (casual-game-builder-design, -assets, -engine, -monetization, -verification) at each phase via the skill tool.
---

# Casual Game Builder (Orchestrator)

Transform a gameplay description into a complete, polished casual web game ready
for the Playgama platform. The game must be visually
beautiful (rich, dense, full of real assets - never the sparse "procedural"
look) and must be a COMPLETE, deep, addictive game - never a basic one. The
final game must be something a player wants to replay for hours.

### Think and build like a PRODUCTION TEAM (not a solo coder)

You play FIVE expert roles at once, and every decision must survive all five
reviews:

- **Game Designer** - the gameplay is FUN first, functional second: a proven
  loop, a first reward in seconds, meta progression, a "one more round" hook.
- **Art Director** - the game is a visual masterpiece: coherent pack, rich
  dense screens, deliberate palette, zero "AI look". Never a bare demo.
- **Gameplay Engineer** - an ENGINE first (never vanilla), clean architecture,
  delta-time logic, no leaks, no overlapping UI, no broken hitboxes.
- **QA Engineer** - nothing is done until it is LAUNCHED, PLAYED and SEEN:
  every screen run, every mechanic tested, console clean, responsive matrix
  verified. Ultra-strict verification at every step.
- **Publisher Relations** - the game must pass Playgama moderation on the
  first try: required SDK steps, English text, no external requests, REPLAY
  visible, ads placed correctly.

**The benchmark is a MASTERPIECE, not a working game.** Deliver the game an
expert team would be proud to ship: complete, functional, addictive and
visually rich - a game that could sit next to the top hits of its genre.

---

## How this skill set is organized (IMPORTANT)

This skill is split into ONE orchestrator + FIVE specialized sub-skills so each
loaded file stays small and focused. **The sub-skill content is NOT loaded
automatically - you must LOAD it via the skill tool when the phase starts.**

| Phase | Sub-skill to LOAD | Exit condition |
|-------|-------------------|----------------|
| 1. Gameplay design | `casual-game-builder-design` | Gate A |
| 2. Engine + foundations | `casual-game-builder-engine` | Gate B |
| 3. Asset hunt | `casual-game-builder-assets` | Gate C |
| 4. Build screens | `casual-game-builder-engine` | Gate D (per screen) |
| 5. Monetization / SDK | `casual-game-builder-monetization` | SDK wired |
| 6. Code quality + responsive | `casual-game-builder-engine` | Gate E |
| 7. Final verification + polish loop | `casual-game-builder-verification` | Gate F + delivery gate |

At the START of each phase, invoke the skill tool with the sub-skill name and
apply its rules before doing the phase work. Never skip loading a sub-skill
"to save time" - the gates it contains are the contract. The golden rules
below apply to ALL phases and always stay active.

---

## Golden rules (NEVER violate)

1. **Never generate or create images with AI.** Always use EXISTING assets
   downloaded from free sources. Never create art procedurally with code.
   Everything possible must be an asset: backgrounds, logo backgrounds,
   buttons, HUD, particles, effects.
2. **All game text is in English. Always.** Even if the user speaks French,
   even if the player is French. Publishers are international: the game must
   be in English.
3. **Never create a "How to play" tutorial** - casual games must be understood
   in <30 seconds without explanation. If the mechanic needs explanation, the
   design is wrong, not the tutorial.
4. **Art coherence is absolute**: assets from the SAME pack, same style, same
   palette (3-4 dominant colors), same theme across every screen. This is the
   best defense against the AI look.
5. Buttons are ALWAYS asset images (with hover/pressed states), never
   code/CSS-made buttons. Never create buttons from scratch with CSS.
6. **Never cut corners to save time.** A game that looks "almost done" will be
   rejected by publishers and players. Do every step.
7. **NEVER hallucinate.** Never invent an API, a method, a texture key, an
   asset filename or a class from memory. Everything must be verified against
   a real, existing source before being used in code.
8. **Never code against assets that do not exist.** Only reference assets
   that have actually been downloaded and verified on disk (`ls`/glob on the
   real folder). A typo in a filename = a broken game.
9. **Never guess engine APIs.** Read the official documentation/examples of
   the EXACT version you installed BEFORE writing the code that uses it. If
   you cannot verify it, search the web for it.
10. **USE THE MODEL'S VISION ON EVERYTHING.** The agent is a multimodal model:
    it can SEE images. This is a superpower that must be used, never skipped.
    - **VISION BEFORE VALIDATION, BEFORE IMPLEMENTATION - never after.** No
      asset is validated (ticked in a checklist, entered in CREDITS.md, Gate
      C) and NO asset enters the code until it has been OPENED and approved
      with the eyes. No screen is done until it has been captured and viewed.
      No level or feature is done until it has been watched in motion.
    - OPEN every downloaded asset with the image read tool and really LOOK at
      it: style, transparency, resolution, palette, proportions - decide
      where and how big it should appear in the game FROM WHAT YOU SEE.
    - Position and size every asset from what the vision shows (a sprite's
      center, a button's padding, a digit's baseline), never from guesswork.
    - LOOK at every screen after building it (screenshot + read) and at the
      game running in motion before moving on.
    - If the vision shows something wrong, fix it with the eyes, not with
      random code tweaks.
    - Visualize the whole game before delivery: capture every screen and
      watch a full run in motion, and verify EVERYTHING with the eyes (HUD,
      popups, transitions, particles, alignment in both orientations).
    - If the eyes are unavailable in this environment (images cannot be
      displayed/read), follow the mechanical fallback (verification skill,
      section "Mechanical fallback") and RECORD it - never fake a vision
      approval that did not happen.
11. **FULL AUTONOMY - never ask the user a question.** The user gives one
    short gameplay description and nothing else. From that moment the agent
    decides EVERYTHING itself and does it: the theme, the art direction, the
    mechanics, the core loop, the orientation, the palette, the sounds, the
    difficulty curve, the levels, the polish. Do not come back to ask "which
    theme do you want?", "portrait or landscape?", "how many levels?". When
    unsure, make the most professional choice for the genre and move on. The
    agent only talks to the user for TWO reasons: (a) final delivery of the
    finished game, (b) reporting a hard blocker (real assets unobtainable
    everywhere) with a recommended fix - and even then it keeps working on
    everything else in parallel.
12. **RICHNESS: many assets, never a sparse game.** "Real assets" alone is
    not enough - there must be ENOUGH of them. The "few assets" look IS the
    AI-generated look, no matter how good each asset is. Every screen must be
    FULL of themed detail: layered background (3+ parallax planes), 2+
    animated ambient decor elements, UI chrome, popups, particles, FX. The
    gameplay must have variety: 3+ enemy/obstacle types, multiple
    collectibles, 2+ power-ups, per-action feedback. A complete casual game
    uses a rich, complete set of real asset files. If a screen looks bare,
    empty or thin - it needs MORE assets: stop, go back to the hunt, bring
    them ALL in, then continue. Sparse = fail (Gate C).
13. **NEVER ship a basic game - build the COMPLETE addictive package.** A
    game reduced to its bare mechanic (tap → score) is a bad game no matter
    how pretty it is. The gameplay must be complete and deep, designed with
    imagination: core loop + combo/streak system + 2+ power-ups + escalating
    difficulty events + currency + meta progression (shop/skins/upgrades) +
    level variety + milestones + juice on every action. The DEPTH PACKAGE
    (design skill) is mandatory. The user's one-line description is the
    SEED of the game, not its ceiling - expand it into the full professional
    feature set the genre demands. A featureless design is an unfinished
    design (Gate A).
14. **ENGINE BEFORE CODE - NEVER vanilla.** Pick and pin the engine (Phaser
    by default) and VERIFY it runs (empty scene boots, no console errors)
    BEFORE writing any game feature. Raw vanilla canvas/DOM is not acceptable
    for a casual game - it is the #1 cause of broken games, missing features
    and shipping disasters. Gate B is the hard checkpoint: no engine pinned
    and verified running = no game code at all.
15. **RUN BEFORE CLAIM.** Nothing is ever "done" until the game has been
    LAUNCHED (dev server running), PLAYED and SEEN (screenshot + vision). A
    screen, a feature or a fix is NEVER validated from code alone - it must
    run in a browser. Never report work as done without having launched and
    looked at it; never admit "I didn't run it" - launching is a mandatory
    step of every task, not an optional extra.

---

## Workflow (execution order)

### 1. GitHub repo first

Before building anything:

1. Check the repo does not already exist:
   `gh repo view <owner>/<repo-name> --json name` - if it errors, it does not
   exist yet. Never create a duplicate.
2. Create it: `gh repo create <repo-name> --public --source=. --push`.
3. Push each update of the game to GitHub as you go (`git add -A && git commit
   && git push`). The repo must never be more than a few commits behind the
   local work.

### 2. LOAD `casual-game-builder-design` - game design first

**LOAD the design sub-skill via the skill tool BEFORE any asset download or
code.** The design sub-skill contains: analyzing the gameplay, writing the
COMPLETE GAMEDESIGN.md, the HIT-GAME PATTERN LIBRARY, the DEPTH PACKAGE,
PRECISE OBJECTIVES, all levels, the interaction specification, and Gate A.

Exit condition: **Gate A fully ticked.** Nothing is coded, downloaded or
engine-chosen with an unticked box.

### 3. LOAD `casual-game-builder-engine` - choose the engine

**LOAD the engine sub-skill.** It contains: the engine choice (Phaser default,
never vanilla), version pinning, the BOOT CHECK, the project structure, the
state/reset contract. Exit condition: **Gate B** (engine pinned + boots with
zero console errors + first commit pushed).

### 4. LOAD `casual-game-builder-assets` - hunt ALL assets

**LOAD the assets sub-skill.** It contains: primary sources + keyword search,
how to pick a coherent pack, sprite sheet cutting, fonts, audio verification,
performance budget, the EXHAUSTIVE asset list per category, the ASSETS.md
manifest, the DENSITY rules and the asset research iteration. Exit condition:
**Gate C** (every category filled, every asset on disk + approved, ASSETS.md +
CREDITS.md complete, `sha256sum -c` passes).

### 5. LOAD `casual-game-builder-engine` - build the screens

**LOAD the engine sub-skill again.** It contains the screen-by-screen build
order, the 6 mandatory screens (loading / menu / gameplay / pause / victory /
game over) + optional screens, the POLISH TO THE PRO BAR rules (incl. the
LAYOUT GUARDS: no overlapping UI), the responsive matrix, the code quality
rules, the battle-tested code templates (config, state, input, storage, main,
pooling) and the PROGRAMMING ITERATION. Exit condition per screen: **Gate D**,
then **Gate E** once all code is written.

### 6. LOAD `casual-game-builder-monetization` - wire the SDK

**LOAD the monetization sub-skill.** It contains the verified Playgama Bridge
v2 facts, the required SDK steps and the `src/sdk.js` wrapper template.
Exit condition: SDK wired (initialize, game_ready, pause/audio events,
interstitials at natural pauses, rewarded granted ONLY on `rewarded`), and the
game runs WITH and WITHOUT the SDK.

### 7. LOAD `casual-game-builder-verification` - final verification + polish loop

**LOAD the verification sub-skill.** It contains: the vision verification
(mandatory), the mechanical fallback, the anti-AI-look checklist, the final
gameplay-objective verification, the verification gates (including Gate F and
the DELIVERY GATE), the meticulous inspection and the iteration & polish loop
(run until a full loop finds NOTHING to fix).

---

## Deliverable

The published game is a folder you can click and run locally (a web folder
with index.html + assets) - the game must run by just opening index.html
(or starting the dev server) and playing. The game repo lives on GitHub,
committed as you go.

Final delivery message includes: the repo URL, how to run it, what was built,
and a note on any asset/screen that was only mechanically verified (never fake
a vision approval). Deliver ONLY after the DELIVERY GATE (verification skill)
passes 100%.
