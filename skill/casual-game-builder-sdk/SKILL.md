---
name: casual-game-builder-sdk
description: The SDK skill of the casual-game-builder skill set. Loaded by the orchestrator at PHASE 5. Integrates the Playgama Bridge SDK exactly and safely: bridge script, sdk.js wrapper, game_ready, loading progress, pause/audio handlers, interstitials after 2 consecutive same-outcome runs, rewarded ads for revive / double coins / at least 50% of shop items, moderation checklist, Gate E. The game must run identically WITH and WITHOUT the SDK. Use when integrating or fixing the Playgama SDK and ads in a casual-game-builder game.
---

# Casual Game Builder — SDK (Playgama, done right)

You are loaded by `casual-game-builder` at PHASE 5. The SDK is the #1 place
games break — follow this EXACTLY. **Two ad types: interstitials, and
rewarded.** The game must run WITH and WITHOUT the SDK. You hand off to
verification at **Gate E**.

---

## Install

Add BEFORE your app scripts in index.html:

```html
<script src="https://bridge.playgama.com/v2/stable/playgama-bridge.js"></script>
```

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

---

## Wire it correctly (never more than once)

- Subscribe ONCE to pause + audio events; in ONE handler pause the gameplay
  AND mute SFX (host fires them for tab switches, ad openings, system pause).
- Apply `bridge.platform.isAudioEnabled` at start.
- Send `game_ready` when the first playable frame is ready.
- Persist progress via `bridge.storage` when available (fall back to
  localStorage — the template's `Storage` still works without SDK).
- Every `sdk.*` call is defensive: it can never crash the game if the bridge
  is absent or fails (the wrapper above already guarantees that).

---

## THE 2 AD TYPES — placement policy (MANDATORY)

### A. Interstitials — after 2 CONSECUTIVE same-outcome runs

- Keep a streak counter. Every FINISHED run (win OR loss) increments it; a
  run of the opposite outcome resets it.
- After the counter reaches **2 (two wins in a row, or two losses in a row)**,
  show an interstitial at the next natural pause (game over / victory
  transition). Then reset the counter.
- NEVER mid-gameplay. NEVER right after a rewarded ad (no double ads back to
  back). Skipped entirely when `isInterstitialSupported` is false. Max ~1
  interstitial per 2 runs.

### B. Rewarded ads — the player CHOOSES; reward granted ONLY on `rewarded`

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

---

## Moderation checklist (Playgama) — pass every box before delivery

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

---

## RUN BEFORE CLAIM

Verify every box LIVE in the browser: win twice in a row → interstitial; lose
twice in a row → interstitial; revive restores the run; double-coins doubles
on `rewarded`; shop "watch ad" items grant without coins; pause fires on ad
open and gameplay resumes after; console clean. Then flip the bridge OFF and
confirm the whole game still runs with zero errors.

---

## Hand-off

**Gate E — DONE means:** bridge script present, initialize + game_ready +
pause/audio handlers wired once, loading screen drives `SDK.loadingProgress`,
interstitials after exactly 2 consecutive same-outcome runs, rewarded granted
ONLY on `rewarded`, REVIVE once per run, BONUS doubles only on `rewarded`,
≥50% of shop items ad-obtainable, REPLAY always visible, game verified WITH
and WITHOUT the SDK, zero console errors. Report to the orchestrator: what was
wired and what verification (P6) must re-check. **Do NOT do the final audit —
that is PHASE 6.**
