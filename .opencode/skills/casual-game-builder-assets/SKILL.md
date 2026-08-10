---
name: casual-game-builder-assets
description: Loaded by casual-game-builder at the asset phase. Hunt, verify and record ALL real assets for a casual game: primary sources + keyword search, coherent packs, sprite sheets, self-hosted fonts, audio verification with ffprobe, performance budget, exhaustive asset lists, the ASSETS.md sha256 manifest, DENSITY rules, the automated code<->assets cross-check (zero missing files, zero hallucinated paths) and the asset research iteration. Pass Gate C.
---

# Casual Game Builder - Assets

This sub-skill hunts, verifies and records EVERY real asset. It is loaded by
the orchestrator skill at phase 3. **Nothing enters the code before it exists
on disk and is approved (vision or recorded mechanical fallback).** Golden
rules 1 (never AI art), 4 (coherence), 8 (never code against non-existent
assets), 10 (vision), 12 (richness) always apply.

---

## 1. Asset sources - primary sources + KEYWORD SEARCH ANYWHERE

The table below lists the RELIABLE primary sources (direct download, no
account, no captcha - they work on any machine). They are the DEFAULT, NOT the
only places allowed. **A keyword web search is often the BETTER way to find an
asset** - especially for a photographic background, a specific theme or a mood
that no pack covers. Never settle for a wrong asset because "it must come from
Kenney". Rules:

1. **Primary sources first** (table below): fastest and safest when they have
   what you need.
2. **If the primary sources don't have it, search the whole web with
   keywords** (subsection below): `free <theme> <type> png`, `royalty free
   <theme> background photo`, `CC0 <theme> sprites pack download`. Any site
   that lets you download with a direct link (no account, no login wall, no
   captcha) is usable.
3. **Never burn time on a blocked source**: if a site refuses downloads
   (login wall, captcha, IP block - e.g. itch.io, Pixabay, Freesound, ZapSplat
   often do on cloud IPs), do NOT fight it - go to the next result. One failed
   site = one line lost, not a dead end.
4. **Check the license on EVERY asset, wherever it comes from** (CC0, CC-BY,
   or commercial-friendly). Unknown/unclear license = choose another asset.
   Never download blindly from a random page without reading its license.
5. Log every asset in ASSETS.md + CREDITS.md with its real source URL, exactly
   like primary-source assets.

| Source | What to find | URL pattern | Account? |
|--------|--------------|-------------|----------|
| **Kenney.nl** | almost EVERYTHING: backgrounds, characters, UI, buttons, particles, AUDIO | https://kenney.nl/assets (catalog, categories on the left) | NO |
| **OpenGameArt** | sprites, backgrounds, effects, music, SFX | https://opengameart.org/art-search-advanced (filter by type + license) | NO |
| **Game-icons.net** | SVG icons for UI/HUD | https://game-icons.net (click any icon → download) | NO |
| **Google Fonts** | typography | https://fonts.google.com → open a family → "Download family" (zip) | NO |
| **CraftPix (Freebies)** | themed backgrounds, sprite packs | https://craftpix.net/freebies/ | NO |
| **Wikimedia Commons** | public-domain backgrounds/patterns | https://commons.wikimedia.org | NO |
| **Any direct-link site found by keyword search** | anything the table misses | search the web (below) | NO only |

### Keyword search - find ANY asset anywhere (use when the packs fall short)

A web search is a FIRST-CLASS asset source, not a last resort. For themes,
photos, moods or styles no pack covers (a realistic jungle photo for a
background, a 1920s poster style, a specific cartoon animal), keyword search
beats fixed sites. Method:

1. Build the NEEDS list first (section 3 - same list, same discipline).
2. Search with targeted keywords, several variants per need:
   - `free <theme> <type> png transparent`, `CC0 <theme> background`,
     `royalty free <theme> game background`, `<theme> <type> asset pack
     download zip`, `open source <theme> sprites`.
   - For photo backgrounds specifically: `free <theme> background photo
     <orientation>` (e.g. `free tropical jungle background photo landscape`),
     `royalty free <theme> landscape`.
3. Filter results to sources with a DIRECT download link: read the page, find
   the file link, verify it downloads with `curl -L` before extracting
   (snippet below). Skip anything behind an account, a captcha, or a login
   wall instantly.
4. Check the license on the page BEFORE downloading. Prefer CC0 (no credit
   needed); CC-BY is fine but MUST be credited in CREDITS.md. Unknown
   license = discard.
5. Judge the found asset with VISION like any other - a keyword find still has
   to match the style, palette and resolution of the game.

Download method (works for all of the above - primary sources AND keyword
finds):

```bash
curl -L -o pack.zip "https://<direct-download-url>"   # follow redirects
unzip -t pack.zip && unzip -o pack.zip -d assets/      # validate then extract
ls -la assets/                                         # confirm real files on disk
```

### How to easily search for what you want (repeatable method)

Follow this exact 4-step method - do not browse randomly:

1. **Write the NEEDS list first** (from GAMEDESIGN.md): one line per asset
   with its type, theme, mood and the on-screen size you need. Example:
   `jungle background 960x540`, `player idle/walk frames 64x64`, `coin icon
   48x48`, `button frame with states`, `score digits`, `collect pop sfx`,
   `calm music loop`. This list IS your search list.
2. **Pick the source per type** (Kenney first, always):
   - Backgrounds → Kenney (Platformer/RPG packs), OpenGameArt, CraftPix free
   - Characters/sprites → Kenney character packs, OpenGameArt
   - UI/buttons/icons → Kenney UI packs, Game-icons.net
   - Fonts → Google Fonts "Download family"
   - Music + SFX → Kenney Audio packs (search `site:kenney.nl/assets category audio`), OpenGameArt
3. **Search the site the right way**:
   - Kenney: https://kenney.nl/assets - the catalog is small and packs are
     self-descriptive; browse the category column, no search needed.
   - OpenGameArt: https://opengameart.org/art-search-advanced → set "Art type"
     (2D Art / Music / Sound Effects / Backgrounds) → add keyword → sort by
     rating. Direct download button on each result (no account).
   - Backup web searches: `site:kenney.nl/assets <theme>`,
     `site:opengameart.org <theme> <type>`.
4. **Download in BULK, stay in ONE pack**: pick the single coherent pack that
   covers the most needs and grab everything from it. Never mix two packs of
   different visual styles. Verify each file on disk, then approve it with
   VISION before it enters the code.

### How to pick an asset pack correctly

1. **Start with Kenney.nl** - its packs are designed to be coherent together.
   Pick ONE pack and stay in it. Do NOT mix two different Kenney packs unless
   they are from the same visual series.
2. **Check the license of every single asset.** CC0 = free to use, no credit
   needed. CC-BY = free to use but you MUST credit the author in CREDITS.md.
   If you are unsure, choose another asset.
3. **Prefer PNG with transparency** for sprites and UI elements. JPG only for
   full backgrounds (it is smaller).
4. **Check the resolution**: a 32x32 sprite stretched to 500x500 will look
   blurry. Prefer assets close to the size you need, or 2x for retina.
5. Track every asset + source + license in a CREDITS.md file in the game repo.

---

## 2. Cut sprite sheets correctly (with VISION - never guess)

1. **Open the sprite sheet image** and really LOOK: count the columns and rows
   of frames, measure the exact frame width/height, and check for padding
   between frames. Write these numbers down FROM WHAT YOU SEE.
2. **Phaser**: load the sheet with the measured frame size, then verify the
   frame count in the console:

```js
this.load.spritesheet("player", "assets/sprites/player.png",
  { frameWidth: 64, frameHeight: 64 });          // values from VISION
// after load: this.textures.get("player").getFrameNames().length
```

3. If the pack ships a TexturePacker JSON atlas, use `this.load.atlas()` and
   reference frames by name - no manual cutting needed.
4. If you hand-cut: make sure the LAST frame is complete (not truncated), the
   frames are uniform, and you create animations with a sensible frame rate
   (idle ~4-6fps, walk ~8-12fps, action ~12-20fps) - tune from VISION.
5. VISION-check the running animation: frames must look smooth, no jumps, no
   clipping. Fix by adjusting frame size or frame rate, never blindly.

## 3. Fonts - self-host ALWAYS, never a CDN link

1. Google Fonts "Download family" → zip → put the `.ttf`/`.woff2` files in
   `assets/fonts/`.
2. Load them LOCALLY (CSS `@font-face` with a relative path, or the engine's
   font loader). The final build must contain ZERO remote URLs - publishers
   block external requests, a CDN font = broken typography.
3. Google Fonts = OFL license, embedding is allowed. Note it in CREDITS.md.

## 4. Audio verification WITHOUT ears (verify with tools, you cannot listen)

You cannot hear the files, so verify them mechanically with `ffprobe`/`ffmpeg`:

```bash
ffprobe -v error -show_entries stream=codec_name,channels,sample_rate,duration \
  -of default=noprint_wrappers=1 sfx.ogg                 # valid + dimensions
ffmpeg -v error -i sfx.ogg -f null -                      # decode errors = empty output
ffmpeg -i music.wav -ar 44100 -ac 2 -b:a 128k music.ogg   # convert to OGG
ffmpeg -i music.wav -ar 44100 -ac 2 -b:a 128k -c:a libmp3lame music.mp3  # + MP3
```

Rules:
1. Ship BOTH `.ogg` AND `.mp3` for every sound (Phaser picks per browser).
2. **Music**: pick tracks explicitly labeled "loop" / "seamless loop" - you
   cannot hear a loop point, so never gamble on an arbitrary track. Loop
   length 20-30s. `ffprobe` must show a valid duration and clean decode.
3. **SFX**: under 1 second, short and punchy, decode cleanly.
4. **Keep it light**: convert audio to OGG+MP3, reuse textures, avoid
   duplicated files - load speed matters. Never force a fixed byte budget.
5. Check the volume balance in code: music quiet (~0.3), SFX louder (~0.8).

## 5. Performance & fluidity budget (assets that keep 60fps)

- Keep single textures within the GPU max texture size of your lowest-end
  target device (commonly 4096x4096; stay safely below it).
- Put many small sprites into ONE texture atlas (fewer draw calls), not 50
  loose images.
- Cap particles per effect (20-30 max) - unlimited confetti kills framerate.
- Everything that spawns repeatedly uses a pool (reuse objects, never
  create/destroy per spawn).
- Multiply every velocity/timer by delta time (`dt`) - never by frame count.
  A game that runs at half speed on a 30fps phone is broken.

## 6. Asset naming convention

Always name files clearly and consistently:

```
screens/loading-bg.png, screens/menu-bg.png, screens/game-bg.png
ui/logo.png, ui/btn-play.png, ui/btn-play-hover.png, ui/btn-play-pressed.png
ui/loader-bar.png, ui/icon-heart.png, ui/icon-coin.png
sprites/player-idle.png, sprites/player-walk-1.png, sprites/player-walk-2.png
fonts/GameFont.ttf
audio/music-loop.ogg, audio/sfx-collect.ogg, audio/sfx-click.ogg
```

- `-hover` and `-pressed` suffixes for button states.
- `-1`, `-2`, `-3` suffixes for animation frames.
- All lowercase, hyphens between words, no spaces, no accents.

---

## 7. Exhaustive asset list for the gameplay screen

Fill EVERY category with real found assets. Never code for art, never
procedural, never AI. This is the screen where everything happens: do not
skip a single category.

**Size the list with the DENSITY rules** (section 8): 3+ parallax planes, 3+
enemy/obstacle types, 2+ power-ups, per-action FX, collectibles, popups,
decor. Size the list to what the gameplay actually needs - as many real asset
files as the game demands. If your complete asset list fits in ~20 lines, it
is FAR too small - walk the whole game again and enumerate more. An asset list
too small = a game that looks procedural.

**Per-screen discipline (applies to EVERY screen, not just gameplay)**: before
building any screen, write its COMPLETE asset needs list from GAMEDESIGN.md.
For the gameplay screen use the exhaustive categories below; for the other
screens use their own short list (loading: bg + logo + loader; menu: bg +
logo + buttons; victory/game over: bg + buttons + decor). Hunt one screen at a
time - the same focus that finds perfect assets for the gameplay screen
applies to finding a perfect background and buttons for EVERY screen.

### Mass download procedure (do this ONCE, from the asset list)

1. Build the full asset list from `GAMEDESIGN.md` FIRST: every sprite,
   button, background, FX and sound the gameplay requires, with the exact
   filename from the naming convention.
2. **Download in bulk**, in as few passes as possible: pick ONE coherent pack
   per need and grab everything the list needs from it (never download asset
   by asset when a pack covers many needs). Reuse the same pack across all
   screens for coherence.
3. **Verify every download on disk**: run `ls` on the actual folder and check
   each expected filename exists at the exact path the code will reference.
   Fix any mismatch immediately - never adjust the code to a guess.
4. **Verify the file is valid**: images open with real dimensions and correct
   format (PNG with alpha for sprites/UI, JPG only for full backgrounds),
   audio decodes without error. Delete corrupt files and re-download.
5. **Check resolution** against the size needed: never stretch a 32x32 sprite
   to 500px; prefer 2x for retina when available.
6. **Record every asset** in CREDITS.md (source + license) as you go, never
   at the end.

### ASSETS.md manifest - the durable, resume-safe record of every download

Every downloaded asset is recorded in `ASSETS.md` at the moment it lands on
disk (never at the end). This is the audit trail that survives a session
restart - the repo IS the memory:

```
| path (in repo)              | source URL               | license | sha256      | resolution | approved         |
|-----------------------------|--------------------------|---------|-------------|------------|------------------|
| assets/ui/btn-play.png      | https://kenney.nl/...    | CC0     | a3f9d2...   | 256x128    | vision 2026-08-09 |
| assets/audio/sfx-click.ogg  | https://opengameart.org/ | CC0     | 9c1e77...   | 0.3s       | mechanical 2026-08-09 |
```

1. **Checksum on download**: run `sha256sum file` right after the download
   and store it. On resume, `sha256sum -c` against the manifest proves the
   file on disk is the exact file that was approved. A file that was never
   approved, was re-downloaded, or is corrupted is caught instantly.
2. **approved column** records HOW and WHEN it was validated: `vision <date>`
   or `mechanical <date>` (verification skill, mechanical fallback). Never
   leave it blank.
3. **RESUME protocol** (when a session restarts): read ASSETS.md + CREDITS.md
   + `git log --oneline` FIRST. Re-verify every manifest entry with
   `sha256sum -c`. Anything missing, corrupt or unapproved is re-downloaded
   and re-approved; everything else is trusted and NOT re-downloaded (no
   wasted work, no re-hunting, no duplicate downloads).
4. **CREDITS.md** stays the human-facing source+license record; ASSETS.md is
   its machine-verifiable twin. Update both as you go.

### The CODE <-> ASSETS cross-check - zero missing assets, zero hallucinated paths (MANDATORY)

The two biggest asset bugs are: code referencing a file that does not exist
(hallucinated path = broken game), and a file on disk that no code uses
(dead weight = slower load). Kill both with a mechanical cross-check run
whenever the code OR the assets change:

```bash
# 1. Every path referenced in src/*.js exists on disk (zero MISSING):
rg -o '"assets/[^"]+"' src/ | tr -d '"' | sort -u | while read f; do
  test -f "$f" || echo "MISSING: $f"; done

# 2. Every approved asset in ASSETS.md is still on disk with the SAME bytes:
sha256sum -c <(grep -oP '^\S+ \*?\K\S+' ASSETS.md | while read f; do
  sha256sum "$f"; done) 2>/dev/null || echo "ASSETS.md <-> disk mismatch"

# 3. Every file in assets/ is actually USED (optional cleanup, keeps load fast):
for f in $(find assets -type f); do
  rg -q "assets/${f#assets/}" src/ || echo "UNUSED: $f"; done
```

Rules:
1. Output must contain ZERO "MISSING" lines before any screen is built and
   before delivery. A missing file is a hard blocker - fix the path or
   download the asset, never ship with a missing file.
2. A path only "exists" if it is both on disk AND in ASSETS.md with a sha256
   that matches. An asset that was never approved does not exist for the game.
3. Review the "UNUSED" lines once per screen: they usually mean an asset was
   replaced - delete the leftover (keeps the build light). Do not delete an
   asset that a screen still needs.
4. This cross-check is the ASSET twin of the CODE AUDIT SCRIPT (engine skill,
   section 4) - run both together before every gate.

### The categories (fill ALL)

**A. Backgrounds**
- Full-screen gameplay background asset (themed).
- Parallax layers (far/mid/near) if the theme allows it.
- Non-interactive themed decor elements.
- Floor/platforms/edges if the mechanic requires them.

**B. Gameplay sprites** (depends on the mechanic)
- Player: main sprite + ALL animations (idle, walk, jump, attack, death...).
- Enemies / obstacles / targets - AT LEAST 3 DISTINCT TYPES, each with its
  own sprite, animation and behavior. One enemy type = sparse game.
- Interactive objects: collectibles (at least 2 visual variants), fusion
  items, breakables, clickables.
- Power-ups / bonuses / maluses - AT LEAST 2 power-up types with distinct
  sprites and effects (magnet, slow-mo, shield, double points, extra life...).
- Mechanic-specific pieces: platforms, blocks, tiles, elements to match.

**C. UI / HUD**
- Score: asset digits images (0-9) or a scoreboard asset.
- Lives / hearts / stamina icons.
- In-game buttons: pause, replay, restart.
- Counters: time, coins, multiplier.
- HUD chrome: panel frames, combo bar, progress bar, level badge - the HUD
  must look designed, not like floating text.

**D. Visual FX - always assets**
- Particles: explosions, sparks, pops.
- Floats / bobbing animations.
- Feedback popups: "+10", "Perfect!", "Combo!" (asset images).
- Glows, halos, light effects.
- Confetti, special effects.
- Ambient decor sprites (at least 3 distinct elements) reused on every screen.

**E. Complete audio** (sound feedback is part of the experience)
- Game music loop.
- Action sounds: collect, jump, click, fusion, match.
- Victory / combo sounds.
- Defeat sound (soft, not frustrating).
- Button sounds (hover, click).
- Themed ambient background.

**F. Transition overlays**
- "Level complete" overlay.
- "Game over" overlay.
- Pause panel (reused from the pause screen).

**G. Misc**
- Favicon / game icon (used by publishers).
- Shop sprites (if the optional shop is included).

---

## 8. Asset DENSITY - no screen may look empty or procedural

The #1 reason games look AI-generated is too FEW assets. "Real assets" alone
is not enough - there must be ENOUGH of them, and they must fill the screen.
Rules:

1. **Every screen is layered and alive**: background (3+ parallax planes or
   depth layers), 2+ animated ambient decor elements (floating clouds,
   falling leaves, bobbing flowers, drifting dust, shimmering stars), UI
   chrome, popups and particles. Zero large empty zones anywhere.
2. **Gameplay richness**: 3+ enemy/obstacle/target types, multiple
   collectibles, 2+ power-ups, per-action FX (collect, hit, dash, combo,
   milestone), score popups, feedback overlays (flash, shake, glow). One
   enemy + one coin is NOT a game.
3. **Target volume**: a complete casual game uses as many real asset files as
   the gameplay demands (screens, sprites, UI, FX, audio). If a screen looks
   sparse, it is - hunt MORE before continuing.
4. **VISION density check (mandatory, every screen)**: capture the screen and
   LOOK at it. Ask "is any zone empty, plain or generic?" - if yes, find a
   themed asset to fill it. Ask "does at least one ambient element move?" -
   if no, add one. Empty zones and dead screens ARE the AI look.
5. **The DENSITY GATE**: if after the polish loop the game still looks bare
   or procedural, the fix is MORE assets, never "good enough". Stop, go back
   to the hunt, bring in everything the screen needs, re-run the gate.

---

## 9. The ASSET RESEARCH ITERATION - hunt until the game is asset-RICH

Asset hunting is done in LOOPS, never in one pass. The visual target is a
successful casual MOBILE game (Candy Crush, Township, Royal Match): bright,
very CARTOON, clean, full. This is where "a game that en jette" is won or
lost - give it real time. Process:

1. **Fill the needs list** from GAMEDESIGN.md (one line per asset: type,
   theme, mood, on-screen size).
2. **First hunt pass**: search each source (section "How to search"),
   download in bulk from ONE coherent pack per need, verify on disk.
3. **VISION + COUNT review**: open every asset and LOOK at it (style,
   cartoon-ness, palette, transparency, resolution). Then count against the
   DENSITY targets (3+ parallax planes, 3+ enemy types, 2+ power-ups, 2+
   ambient decor per screen, per-action FX, complete audio).
4. **HOSTILE STYLE REVIEW**: compare with the eyes to real mobile casual
   screenshots. Ask: "does this look like a released cartoon hit, or a bare
   demo?" Mark every asset that is weak, blurry, off-palette, off-style or
   missing.
5. **RE-HUNT, do not settle**: for every marked item, run NEW searches with
   different keywords/packs until the exact right asset is found. If a pack
   does not have the style, switch packs (same DA). Missing assets are
   blockers - the hunt continues until the list is FULL.
6. **TICK THE ASSET CHECKLIST** (100% - the loop's exit condition):
   - [ ] Every category of section 7 filled with REAL found assets
   - [ ] DENSITY targets met (as many real assets as the gameplay demands)
   - [ ] VISION: every asset approved (style, palette, resolution, alpha)
   - [ ] Very cartoon, bright, clean - matches the mobile-hit bar
   - [ ] Every file verified on disk with `ls` (paths match the code)
   - [ ] Audio complete (music loop + every SFX), decoded cleanly
   - [ ] CREDITS.md filled for every asset
7. **LOOP CHECK**: any box unticked? Any screen that would look bare once
   assembled? Any asset that would embarrass next to a real hit? If YES, loop
   from step 5. If NO, asset research passes (Gate C) and building can start.

The assets ARE the look of the game - a rich, cartoon, professional set of
assets is what makes the game look like months of work by experts.

---

## Gate C - Assets done

- [ ] Bulk download done from the asset list (one coherent pack per need)
- [ ] Every file verified on disk with `ls`: the exact paths match the code
- [ ] Every file valid (images open with real dimensions, audio decodes)
- [ ] Resolution fits the on-screen size (never a blurry stretch)
- [ ] VISION: every asset opened, described, approved and placed FROM the
      image - or mechanical fallback used and RECORDED (verification skill)
- [ ] CREDITS.md filled in as you go (source + license)
- [ ] ASSETS.md manifest complete: sha256, source, license, approval status
      for every file; `sha256sum -c` passes
- [ ] Real assets only: nothing generated, nothing hallucinated
- [ ] CODE <-> ASSETS cross-check (section 7) run and clean: zero MISSING
      paths, ASSETS.md sha256 matches disk, unused files reviewed
- [ ] Zero asset referenced in code that does not exist on disk (both
      directions verified mechanically, not by eye)
- [ ] DENSITY: as many real assets as the gameplay demands; 3+ parallax
      planes; 2+ ambient decor per screen; 3+ enemy/obstacle types; 2+
      power-ups; per-action FX. Sparse = FAIL - go back and hunt more.

One unchecked box means the asset phase is NOT done - re-hunt, do not skip.
Re-run the gate whenever anything in the asset set changes.
