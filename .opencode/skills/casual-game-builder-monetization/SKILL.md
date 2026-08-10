---
name: casual-game-builder-monetization
description: Loaded by casual-game-builder at the monetization phase. Integrate the Playgama Bridge SDK v2 (the ONLY SDK) into a casual game: verified API facts (initialize, game_ready, pause/audio events, interstitials, rewarded), required SDK steps and the src/sdk.js wrapper template. The game must run WITH and WITHOUT the SDK.
---

# Casual Game Builder - Monetization (Playgama Bridge SDK v2)

This sub-skill wires the Playgama Bridge SDK v2 into the game. It is loaded by
the orchestrator skill at phase 5. Golden rule 7 (never hallucinate an API)
applies hard here: the facts below were VERIFIED against the official docs
(https://wiki.playgama.com/playgama/bridge-sdk - setup, api, platform,
advertisement pages). Re-read them before coding - never trust a remembered
API.

---

## Verified API facts (Playgama Bridge v2)

- **Script tag** (setup.md): `<script src="https://bridge.playgama.com/v2/stable/playgama-bridge.js"></script>`.
- **Initialize first** (api.md, required step 1): `await bridge.initialize()`
  before ANY `bridge.*` call. In unsupported/dev environments Bridge loads a
  mock platform and returns safe defaults instead of throwing - so the game
  never crashes when the SDK is absent.
- **Required steps** (api.md - a game skipping them can be rejected):
  1. `await bridge.initialize()` first.
  2. Localize from `bridge.platform.language`.
  3. Persist progress through `bridge.storage.get/set/delete`, never
     `localStorage` directly.
  4. Subscribe ONCE to `bridge.platform.on(bridge.EVENT_NAME.PAUSE_STATE_CHANGED)`
     and `bridge.platform.on(bridge.EVENT_NAME.AUDIO_STATE_CHANGED)` - the host
     fires them on tab switch, ad opening, system pause. Mute + pause the game
     in a single universal handler. Also apply `bridge.platform.isAudioEnabled`
     at start (the event alone is not enough).
  5. Send `bridge.platform.sendMessage("game_ready")` when the first playable
     frame is ready.
  6. Show interstitials at natural pauses (level transition, game over) when
     `bridge.advertisement.isInterstitialSupported` is true, via
     `bridge.advertisement.showInterstitial(placement?)` - required to qualify
     for revenue share.
- **Rewarded ads** (rewarded.md): optional, only on victory and game over,
  always skippable - never blocking.
  - Request: `bridge.advertisement.showRewarded(placement?)` after the player
    opts in.
  - GRANT THE REWARD ONLY when the state is `rewarded`. Track it via
    `bridge.advertisement.on(bridge.EVENT_NAME.REWARDED_STATE_CHANGED, state =>
    ...)`; states are `loading`, `opened`, `closed`, `rewarded`, `failed`.
    NEVER grant on `closed` - the player may have skipped the video.
  - Hide the BONUS button when `bridge.advertisement.isRewardedSupported` is
    false.
- **Lifecycle messages** (platform.md): `bridge.platform.sendMessage(...)` with
  `"level_started"` / `"level_paused"` / `"level_resumed"` / `"level_completed"`
  / `"level_failed"`. (There is no `gameplayStart()`/`gameplayStop()` in v2 -
  verified in the migration docs.)
- Never show interstitials during active gameplay.
- The REPLAY button must always be present and visible (publisher criteria).

---

## Ad placement policy (MANDATORY - where each ad type goes)

These are the exact placement rules. Follow them in every game - the gameplay
design decides the details, the rules below decide the ad flow:

### 1. Game over -> REVIVE (rewarded ad, "continue where you left off")

- The game over screen shows a **REVIVE / CONTINUE button** (always visible,
  placed next to the REPLAY button, styled as an asset button like every
  button).
- Clicking it opens a **rewarded video** (`showRewardedAd()`). If the player
  watches it to the `rewarded` state, the run resumes EXACTLY where it ended:
  same score, same progression, same level state - only the run is restored.
- This is the player's "save the run" moment - it is a survival hook, NOT a
  coin reward.
- Revive is available **once per run** (after revive, the REVIVE button
  disappears for the rest of that run; the REPLAY button stays).
- If rewarded ads are not supported, hide the REVIVE button (never break
  replay).

### 2. Victory -> BONUS (rewarded ad, "double your reward")

- The victory screen shows a **BONUS / DOUBLE REWARD button** (asset button,
  next to the "Continue" button).
- Clicking it opens a **rewarded video**. If watched to the `rewarded` state,
  the victory reward (coins / stars bonus / multiplier) is **doubled**.
- This is the "profit from success" moment - a happy, skippable extra.
- If the reward is not granted (closed/failed), the base reward is kept - never
  removed.
- If rewarded ads are not supported, hide the BONUS button.

### 3. Interstitial cadence -> after 2 consecutive wins OR 2 consecutive losses

- Interstitials appear **only after 2 consecutive finished runs of the same
  outcome**: 2 wins in a row, or 2 losses in a row.
- After a win streak or loss streak reaches 2, show the interstitial at the
  next natural pause (game over screen or victory screen transition) - never
  mid-gameplay.
- Every finished run (win or loss) increments a streak counter; a run of the
  opposite outcome resets it. Example: win, win -> interstitial. loss -> reset.
  win -> counter 1. win -> counter 2 -> interstitial.
- This cadence caps the ad frequency (max ~1 interstitial per 2 runs) while
  keeping revenue steady - it rewards streaks with a break, not with a punishment.
- When `isInterstitialSupported` is false, skip interstitials entirely - never
  fake them.

### 4. Frequency cap (never abuse the player)

- Max 1 interstitial for every 2 finished runs (the streak rule enforces it).
- Max 1 rewarded ad per finished run (revive OR bonus, not both) - a player
  who already revived on game over does not get a bonus video on victory in
  the same run.
- Never show an interstitial right after a rewarded ad (the player just
  watched a video - no double ads back to back).
- The REPLAY button is always immediately reachable: no ad ever blocks replay.

---

## The src/sdk.js wrapper (single module - the code never touches SDK directly)

```js
const bridgePromise = window.bridge?.initialize
  ? window.bridge.initialize().then(() => window.bridge)
  : Promise.resolve(null);

export async function isSDKAvailable() {
  try { return (await bridgePromise) != null; } catch { return false; }
}
export async function gameReady() {
  try { (await bridgePromise)?.platform.sendMessage("game_ready"); } catch {}
}
export async function setLoadingProgress(p) {
  try { (await bridgePromise)?.setGameLoadingProgress(p); } catch {}
}
export async function sendLevelMessage(name) {   // level_started / level_paused / ...
  try { (await bridgePromise)?.platform.sendMessage(name); } catch {}
}
export async function showRewardedAd() {          // resolves true ONLY on state 'rewarded'
  const b = await bridgePromise;
  if (!b || !b.advertisement?.isRewardedSupported) return false;
  return new Promise((resolve) => {
    let settled = false;
    const done = (ok) => { if (!settled) { settled = true; resolve(ok); } };
    const onState = (state) => {
      if (state === "rewarded") done(true);
      else if (state === "closed" || state === "failed") done(false);
    };
    b.advertisement.on(b.EVENT_NAME.REWARDED_STATE_CHANGED, onState);
    try { b.advertisement.showRewarded(); } catch { done(false); }
  });
}
export function onPlatformPause(cb) {             // pause gameplay when host asks
  bridgePromise.then((b) => {
    try { b?.platform.on(b.EVENT_NAME.PAUSE_STATE_CHANGED, cb); } catch {}
  });
}
export function onAudioChanged(cb) {              // mute game when audio is off
  bridgePromise.then((b) => {
    try { b?.platform.on(b.EVENT_NAME.AUDIO_STATE_CHANGED, cb); } catch {}
  });
}
```

Rules:
- Subscribe ONCE to pause + audio events and pause/mute the game in one handler
  (host fires them for tab switches, ads, system pauses); send `level_paused`
  before opening full-screen UI and `level_resumed` after.
- BONUS button hidden when `isSDKAvailable()` is false or rewarded is not
  supported; the reward is granted ONLY if `showRewardedAd()` returned true.
- Every bridge call wrapped in try/catch.
- Re-verify the method names against the docs of the exact version you ship
  (rule 7) before use.

---

## SDK wiring checklist

- [ ] `<script src="https://bridge.playgama.com/v2/stable/playgama-bridge.js">` in index.html
- [ ] `await bridge.initialize()` on boot, before any bridge call
- [ ] `setLoadingProgress` drives the loading screen progress
- [ ] `game_ready` sent when the first playable frame is ready
- [ ] Pause + audio events subscribed ONCE; game pauses/mutes in one handler;
      `bridge.platform.isAudioEnabled` applied at start
- [ ] Progress persisted via `bridge.storage`, never `localStorage` directly
- [ ] Interstitials only after 2 consecutive wins OR 2 consecutive losses
      (streak counter; opposite outcome resets) - at natural pauses, never
      mid-gameplay, never right after a rewarded ad
- [ ] Game over REVIVE button (rewarded): resumes the run EXACTLY where it
      ended, once per run, hidden when rewarded unsupported, REPLAY always
      reachable
- [ ] Victory BONUS button (rewarded): doubles the victory reward ONLY when
      state is `rewarded`; base reward kept on close/fail; hidden when
      unsupported
- [ ] Max 1 rewarded ad per finished run (revive OR bonus, not both)
- [ ] Rewarded: BONUS button hidden when unsupported; reward granted ONLY on
      state `rewarded` (never on `closed`)
- [ ] REPLAY always present and visible
- [ ] Game runs WITH the SDK AND WITHOUT it (mock platform) - tested
- [ ] Zero console errors with the SDK loaded

Exit condition: SDK wired and the game runs correctly both with and without
the SDK.
