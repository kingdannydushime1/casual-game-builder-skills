---
name: casual-game-builder-verification
description: The VERIFICATION skill of the casual-game-builder skill set. Loaded by the orchestrator at PHASE 6. Runs the hard pre-delivery checklist: user concept kept verbatim, every designed feature implemented, every screen themed with zero overlap, SDK/ads correct, pause working, zero errors — each box PROVEN with evidence or the game is NOT delivered. Use when auditing a finished casual-game-builder game before delivery.
---

# Casual Game Builder — VERIFICATION (last defense)

You are the **LAST pair of eyes** before delivery. Loaded by
`casual-game-builder` at PHASE 6. **If a bad game reaches the user — a
replaced concept, bad backgrounds, overlapping UI, a broken SDK — YOU failed.**
Nothing ships until every box below is proven.

Go through the checklist IN ORDER. Every box needs EVIDENCE (a screenshot you
looked at, a console capture, a line of code): write PASS + proof, or FAIL +
what is wrong. **Any FAIL = fix it (gameplay fixes go through the engine skill,
never patched around) and re-run the checklist. The loop ends only when a full
run passes every box.**

---

## THE CHECKLIST

### 1. The user's concept is the game
- If the user gave a concept, PLAY the game and confirm the core action is
  EXACTLY that concept — same mechanic, same goal. Replaced, swapped or
  "simplified into an easier hypercasual" = **FAIL, do not audit further**.

### 2. Design vs code (GAMEDESIGN.md)
- Every designed entity, mechanic, level, screen, effect, sound and economy
  rule = one row. Mark IMPLEMENTED or MISSING. **MISSING = FAIL.**
- Re-run the code↔assets cross-check (zero MISSING, zero orphans):
  ```bash
  rg -o '"assets/[^"]+"' src/ | tr -d '"' | sort -u | while read f; do
    test -f "$f" || echo "MISSING: $f"; done
  ```
- Grep for `TODO`, `FIXME`, `console.log`, `debugger`, stubs, commented blocks.
  Any hit = FAIL.

### 3. Every screen, seen and good
Screenshot and LOOK at menu, gameplay, pause, gameover, victory, shop:
- **Backgrounds**: themed, dense, coherent with the palette — bad, empty or
  mismatched = FAIL.
- **No overlapping elements** anywhere (shop included). Overlap = FAIL.
- Dense and aligned (no empty zones), readable HUD, REPLAY visible.
- Shop items show their **illustration image + label** — text-only = FAIL.

### 4. Pause and controls
- Click PAUSE in gameplay: freezes instantly, pause screen opens, resume
  restores the run, no error. Dead pause = FAIL.

### 5. Technical
- Console: zero errors across run, die, revive, win, shop, pause, resize.
- No leaks (pause/resume 10×), stable 60fps in the heaviest scene.
- Responsive matrix: portrait AND landscape × phone/tablet/desktop/big —
  nothing cut off, buttons tappable, HUD intact. Failing cell = FAIL.
- Runs WITH and WITHOUT the SDK.

### 6. SDK + ads, live
- Interstitial after **2 consecutive wins** and after **2 consecutive losses**.
- Rewarded: revive restores the run where it ended; double-coins doubles on
  `rewarded` only; ≥50% of shop items ad-obtainable. Rewards granted ONLY on
  `rewarded`.
- Pause when an ad shows, resume correctly after; `game_ready` sent; runs with
  AND without the SDK; all text English; REPLAY visible; ZIP ready.

---

## Hand-off

**Gate F / DELIVERY GATE — DONE only when every box above is PASS with proof.**
Game played start-to-finish, seen on every screen; concept verbatim; SDK/ads
policy proven live; console clean; responsive; no overlap; no bad backgrounds;
committed and pushed to its own repo; runnable by opening the folder. Report
to the orchestrator: PASS/FAIL per box, what was mechanically verified, and
the final "deliver now" verdict — or the list of FAILs being fixed.
