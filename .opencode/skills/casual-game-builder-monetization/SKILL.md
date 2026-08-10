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
- [ ] Interstitials at natural pauses when `isInterstitialSupported` (level
      transition, game over) - never during active gameplay
- [ ] Rewarded: BONUS button hidden when unsupported; reward granted ONLY on
      state `rewarded` (never on `closed`)
- [ ] REPLAY always present and visible
- [ ] Game runs WITH the SDK AND WITHOUT it (mock platform) - tested
- [ ] Zero console errors with the SDK loaded

Exit condition: SDK wired and the game runs correctly both with and without
the SDK.
