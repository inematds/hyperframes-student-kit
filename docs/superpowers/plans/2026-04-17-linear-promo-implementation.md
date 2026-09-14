# Linear.app 30s Promo Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a 30s × 1920×1080 × 30fps Linear.app promo in HyperFrames — an Infinite-Global-Payments twin across 18 beats — and render the final MP4.

**Architecture:** New project at `video-projects/linear-promo-30s/`. Root `index.html` wires 8 sub-compositions via `<template>` + `data-composition-src`. Each sub-composition is hand-built (zero registry blocks per MOTION_PHILOSOPHY.md §6). All fonts loaded from Google CDN per-composition. Real Linear screenshots captured via Playwright. Law #11 padding (`tl.to({}, { duration: SLOT }, 0)`) on every sub-timeline. Captions as body-level siblings per §3.13.

**Tech Stack:** HyperFrames CLI, GSAP 3.14.2, Google Fonts CDN (Space Grotesk, Instrument Serif, Azeret Mono, Cormorant Garamond, Geist, JetBrains Mono, Inter Display), Playwright for asset capture, ffmpeg for poster frames + audio normalization.

**Spec reference:** `docs/superpowers/specs/2026-04-17-linear-promo-design.md`
**Philosophy reference:** `MOTION_PHILOSOPHY.md` (re-read before authoring any beat)

---

## Working Conventions (applies to every task)

- **Run all CLI commands from inside `video-projects/linear-promo-30s/`.** The HyperFrames CLI resolves `assets/`, `compositions/`, `renders/` relative to the current directory.
- **After each composition is authored, run `npx hyperframes lint` from the project folder before moving on.** Fix errors immediately.
- **Frame rate is 30fps.** All `data-start`, `data-duration`, and tween timings must snap to multiples of `1/30 = 0.0333s` (Law #11 checklist item).
- **Every sub-composition timeline ends with `tl.to({}, { duration: SLOT_DURATION }, 0);`** — no exceptions. This is Law #11.
- **No hard cuts.** Every transition uses a whip-streak, morph, recolor, or slide.
- **Every sub-composition is self-contained:** loads its own GSAP script, its own Google Fonts link, its own styles (scoped via `[data-composition-id="..."]` selector), registers exactly one timeline on `window.__timelines[<id>]` where the key matches `data-composition-id` exactly.
- **Commit after every task** using the commit format in each task's final step.

---

## File Structure (locked in before we start)

```
video-projects/linear-promo-30s/
├── index.html                          ← root composition + captions + audio mix
├── meta.json                           ← {id, name, 1920×1080, 30fps}
├── hyperframes.json                    ← registry pointer (copied from sibling)
├── compositions/
│   ├── 01-problem-type.html            ← kinetic type "Product management is broken" (0–4s)
│   ├── 02-card-to-logo.html            ← red card → Linear L-bracket morph + symbol rise (4–8s)
│   ├── 03-brand-reveal.html            ← Linear wordmark crystallization (8–10s)
│   ├── 04-benefits-flowchart.html      ← Fast / Focused / AI-native flowchart with color recolor (10–14.8s)
│   ├── 05-product-surfaces.html        ← Plan / Build / Monitor device mockups (14.8–19.5s)
│   ├── 06-wheel-pillars.html           ← chrome wheel + side-panel pattern (19.5–22s)
│   ├── 07-foundation.html              ← callback spin + floating cluster + "For teams and agents" (22–25s)
│   └── 08-cta-outro.html               ← wordmark + tagline + "Get started — linear.app" (25–30s)
├── assets/
│   ├── linear-logo.svg                 ← Linear wordmark SVG
│   ├── linear-symbol.svg               ← Linear L-bracket symbol SVG
│   ├── screenshot-plan.png             ← Linear Plan UI screenshot (1920×1080)
│   ├── screenshot-build.png            ← Linear Build/Issues UI screenshot
│   ├── screenshot-monitor.png          ← Linear analytics screenshot
│   ├── warm-pad.mp3                    ← 30s ambient pad (sourced or normalized from shared stash)
│   ├── sfx-whoosh-1.mp3                ← whip transition SFX
│   ├── sfx-whoosh-2.mp3                ← second whip SFX
│   └── sfx-twinkle.mp3                 ← brand-reveal click/twinkle
└── renders/                            ← gitignored — draft + final MP4s + frame checks
    └── frames/                         ← extracted PNGs for visual verification
```

**Per-composition slot durations (locked — use these exact values as `SLOT_DURATION`):**

| Comp | data-start | data-duration | Notes |
|------|-----------|---------------|-------|
| `01-problem-type`        | 0    | 4    | ends on whip peak at 4.2 (whip runs 4.0–4.4 inside comp 02) |
| `02-card-to-logo`        | 4    | 4    | red card 4.0–6.0, morph 6.0–7.0, symbol rise 7.0–8.0 |
| `03-brand-reveal`        | 8    | 2    | wordmark crystallize + 1s hold + shimmer sweep |
| `04-benefits-flowchart`  | 10   | 4.8  | Fast 10.0–11.2, Focused 12.3–13.3, AI-native recolor 13.3–14.8 |
| `05-product-surfaces`    | 14.8 | 4.7  | Plan 14.8–16.5, Build 16.5–18.0, Monitor 18.0–19.5 |
| `06-wheel-pillars`       | 19.5 | 2.5  | rotating chrome wheel + three side panels |
| `07-foundation`          | 22   | 3    | callback spin 22–23, floating cluster 23–25 |
| `08-cta-outro`           | 25   | 5    | 5s hero hold with shimmer every 1.8s |

Total: 4 + 4 + 2 + 4.8 + 4.7 + 2.5 + 3 + 5 = 30.0s ✓

---

## Task 1: Scaffold the project folder

**Files:**
- Create: `video-projects/linear-promo-30s/meta.json`
- Create: `video-projects/linear-promo-30s/hyperframes.json`
- Create: `video-projects/linear-promo-30s/compositions/` (empty dir)
- Create: `video-projects/linear-promo-30s/assets/` (empty dir)
- Create: `video-projects/linear-promo-30s/renders/` (empty dir)
- Create: `video-projects/linear-promo-30s/index.html` (stub — wires no comps yet)

- [ ] **Step 1: Make the folder tree**

Run from workspace root:
```bash
mkdir -p "video-projects/linear-promo-30s/compositions"
mkdir -p "video-projects/linear-promo-30s/assets"
mkdir -p "video-projects/linear-promo-30s/renders"
```

- [ ] **Step 2: Write `meta.json`**

Path: `video-projects/linear-promo-30s/meta.json`

```json
{
  "id": "linear-promo-30s",
  "name": "Linear — 30s Promo (Infinite-style)",
  "createdAt": "2026-04-17T00:00:00.000Z",
  "width": 1920,
  "height": 1080,
  "fps": 30
}
```

- [ ] **Step 3: Write `hyperframes.json`**

Path: `video-projects/linear-promo-30s/hyperframes.json`

```json
{
  "$schema": "https://hyperframes.heygen.com/schema/hyperframes.json",
  "registry": "https://raw.githubusercontent.com/heygen-com/hyperframes/main/registry",
  "paths": {
    "blocks": "compositions",
    "components": "compositions/components",
    "assets": "assets"
  }
}
```

- [ ] **Step 4: Write the initial `index.html` stub**

Path: `video-projects/linear-promo-30s/index.html`

This stub declares the root 30s composition with no sub-comps yet. Each comp-authoring task will splice a `<div class="beat-layer" ...>` into the root.

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=1920, height=1080" />
    <title>Linear — 30s Promo</title>
    <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
    <style>
      * { margin: 0; padding: 0; box-sizing: border-box; }
      html, body {
        margin: 0;
        width: 1920px;
        height: 1080px;
        overflow: hidden;
        background: #000;
        color: #fff;
      }
      .beat-layer {
        position: absolute;
        top: 0; left: 0;
        width: 1920px;
        height: 1080px;
      }
      .cap {
        position: absolute;
        bottom: 72px;
        left: 50%;
        transform: translateX(-50%);
        padding: 12px 22px;
        border-radius: 14px;
        background: rgba(10, 8, 5, 0.55);
        backdrop-filter: blur(8px);
        font: 500 24px/1.3 "Geist", "Inter", sans-serif;
        color: #fff;
        z-index: 50;
      }
    </style>
  </head>
  <body>
    <div
      id="root"
      data-composition-id="linear-promo-30s"
      data-start="0"
      data-duration="30"
      data-width="1920"
      data-height="1080"
    >
      <!-- Sub-compositions will be inserted here as we author each beat -->

      <!-- Audio mix (filled in during Task 11) -->
    </div>
  </body>
</html>
```

- [ ] **Step 5: Verify scaffold with the HyperFrames CLI**

Run from inside `video-projects/linear-promo-30s/`:
```bash
cd "video-projects/linear-promo-30s"
npx hyperframes lint
```

Expected: lint runs without a crash. Warnings about empty sub-comps are fine — we have zero yet. If lint reports an error about `meta.json` or `hyperframes.json` shape, fix field typos before moving on.

- [ ] **Step 6: Commit**

```bash
git add video-projects/linear-promo-30s/
git commit -m "scaffold: linear-promo-30s project (meta, hf config, index stub)"
```

---

## Task 2: Acquire Linear branding assets (logo + symbol SVG)

**Files:**
- Create: `video-projects/linear-promo-30s/assets/linear-logo.svg`
- Create: `video-projects/linear-promo-30s/assets/linear-symbol.svg`

We're tracing from linear.app's inline SVG marks. Not downloading binary assets from their CDN — just the SVG path data, which is public DOM.

- [ ] **Step 1: Fetch the live Linear homepage and extract the wordmark SVG**

Using Playwright (already installed in the workspace `node_modules`), run this one-off Node script from the workspace root. Save it as `video-projects/linear-promo-30s/scripts/extract-logo.js`:

```bash
mkdir -p "video-projects/linear-promo-30s/scripts"
```

Path: `video-projects/linear-promo-30s/scripts/extract-logo.js`

```js
const { chromium } = require('playwright');
const fs = require('fs');
const path = require('path');

(async () => {
  const browser = await chromium.launch({ headless: true });
  const page = await browser.newPage({ viewport: { width: 1920, height: 1080 } });
  await page.goto('https://linear.app', { waitUntil: 'networkidle' });

  // Linear's header has an inline SVG logo. Find it — the first SVG in the <header> or nav with width >= 70.
  const svgs = await page.$$eval('svg', nodes =>
    nodes.map(n => ({ html: n.outerHTML, bbox: n.getBoundingClientRect() }))
  );

  // Print all SVGs' bboxes so we can identify the right one.
  console.log(JSON.stringify(svgs.map(s => s.bbox), null, 2));

  // Heuristic: wordmark has width > 70 and height < 40; symbol is roughly square (~24-32px).
  const wordmark = svgs.find(s => s.bbox.width > 70 && s.bbox.height < 40);
  const symbol   = svgs.find(s => Math.abs(s.bbox.width - s.bbox.height) < 4 && s.bbox.width > 20);

  const outDir = path.join(__dirname, '..', 'assets');
  if (wordmark) fs.writeFileSync(path.join(outDir, 'linear-logo.svg'), wordmark.html);
  if (symbol)   fs.writeFileSync(path.join(outDir, 'linear-symbol.svg'), symbol.html);

  await browser.close();
  console.log('Wordmark saved:', !!wordmark, '| Symbol saved:', !!symbol);
})();
```

Run from workspace root:
```bash
node "video-projects/linear-promo-30s/scripts/extract-logo.js"
```

Expected output: `Wordmark saved: true | Symbol saved: true`. If both are `false` or the bbox dump shows no matching SVG, inspect the dump and adjust the heuristic (their DOM may have changed).

- [ ] **Step 2: Verify the SVGs render correctly**

Open each file — confirm it starts with `<svg` and contains `<path` elements. If the symbol file got the wordmark instead (or vice versa), swap them manually. You can eyeball shape by pasting into https://codepen.io or saving as `.html` and opening in a browser, but don't get stuck — the goal is a clean SVG asset; if the trace is imperfect we'll replace by hand in a later task.

- [ ] **Step 3: Fallback if Playwright extraction failed**

If Step 1 failed and both SVGs are empty/wrong, hand-roll the Linear L-bracket symbol. The Linear symbol is a parallelogram "L" — two trapezoids forming a stylized bracket. Save as `linear-symbol.svg`:

```xml
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <linearGradient id="lg" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#5E6AD2"/>
      <stop offset="100%" stop-color="#8A92E8"/>
    </linearGradient>
  </defs>
  <path d="M 20 15 L 80 15 L 80 35 L 40 35 L 40 85 L 20 85 Z" fill="url(#lg)"/>
</svg>
```

And `linear-logo.svg` (wordmark — text "Linear" in Inter Display 600):

```xml
<svg viewBox="0 0 360 80" xmlns="http://www.w3.org/2000/svg">
  <text x="0" y="60" font-family="'Inter Display', 'Inter', sans-serif" font-weight="600" font-size="72" fill="#fff">Linear</text>
</svg>
```

Only use the fallback if Step 1 genuinely failed. Prefer real trace.

- [ ] **Step 4: Commit**

```bash
git add video-projects/linear-promo-30s/assets/linear-logo.svg \
        video-projects/linear-promo-30s/assets/linear-symbol.svg \
        video-projects/linear-promo-30s/scripts/extract-logo.js
git commit -m "assets(linear): logo + symbol SVG"
```

---

## Task 3: Capture Linear UI screenshots (Plan, Build, Monitor)

**Files:**
- Create: `video-projects/linear-promo-30s/assets/screenshot-plan.png`
- Create: `video-projects/linear-promo-30s/assets/screenshot-build.png`
- Create: `video-projects/linear-promo-30s/assets/screenshot-monitor.png`
- Create: `video-projects/linear-promo-30s/scripts/capture-screens.js`

- [ ] **Step 1: Write the Playwright capture script**

Path: `video-projects/linear-promo-30s/scripts/capture-screens.js`

```js
const { chromium } = require('playwright');
const path = require('path');

(async () => {
  const browser = await chromium.launch({ headless: true });
  const page = await browser.newPage({
    viewport: { width: 1920, height: 1080 },
    deviceScaleFactor: 2
  });

  // linear.app/plan, /build, /monitor are the three pillar landing pages as of 2026-04-17.
  const pages = [
    { url: 'https://linear.app/plan',    out: 'screenshot-plan.png' },
    { url: 'https://linear.app/build',   out: 'screenshot-build.png' },
    { url: 'https://linear.app/monitor', out: 'screenshot-monitor.png' },
  ];

  const outDir = path.join(__dirname, '..', 'assets');

  for (const { url, out } of pages) {
    console.log(`Capturing ${url} ...`);
    await page.goto(url, { waitUntil: 'networkidle', timeout: 45000 });
    // Let lazy assets + fonts settle.
    await page.waitForTimeout(1500);
    // Scroll a bit to land on the hero product visual rather than the top banner.
    await page.evaluate(() => window.scrollTo({ top: 400, behavior: 'instant' }));
    await page.waitForTimeout(500);
    await page.screenshot({
      path: path.join(outDir, out),
      fullPage: false,
      clip: { x: 0, y: 0, width: 1920, height: 1080 }
    });
    console.log(`Saved ${out}`);
  }

  await browser.close();
})();
```

- [ ] **Step 2: Run the capture**

From workspace root:
```bash
node "video-projects/linear-promo-30s/scripts/capture-screens.js"
```

Expected: three PNGs appear in `assets/`, each roughly 200KB–2MB depending on content.

- [ ] **Step 3: Visually verify each screenshot**

Use the Read tool on each PNG file path to confirm:
- `screenshot-plan.png` shows a Linear planning/roadmap UI (Gantt chart, project view, issue list — anything product-ish)
- `screenshot-build.png` shows issues, an editor, or a workflow view
- `screenshot-monitor.png` shows analytics, charts, or activity feed

If any screenshot is the generic marketing site hero with no product UI visible, adjust the `scrollTo` `top` value in the script (try 800, 1200, or 2000) and re-run. Keep iterating until each image shows Linear's actual app surface.

If Linear has removed dedicated `/plan` etc. URLs, fall back to `https://linear.app` and scroll to specific anchor positions — inspect the live site, note which scroll offsets show which pillars, and update the script.

- [ ] **Step 4: If real captures fail or look wrong, hand-craft mock screens in HTML/CSS**

Not writing these now — note as a fallback. If after Step 3 iteration we still can't get three clean screenshots, spec allows us to hand-build stylized mock Linear screens as HTML/CSS panels inside the composition instead of using PNGs. We'd skip the image files and use styled divs inside `05-product-surfaces.html` instead.

- [ ] **Step 5: Commit**

```bash
git add video-projects/linear-promo-30s/assets/screenshot-*.png \
        video-projects/linear-promo-30s/scripts/capture-screens.js
git commit -m "assets(linear): UI screenshots for Plan/Build/Monitor surfaces"
```

---

## Task 4: Acquire audio — warm ambient pad + 3 SFX

**Files:**
- Create: `video-projects/linear-promo-30s/assets/warm-pad.mp3`
- Create: `video-projects/linear-promo-30s/assets/sfx-whoosh-1.mp3`
- Create: `video-projects/linear-promo-30s/assets/sfx-whoosh-2.mp3`
- Create: `video-projects/linear-promo-30s/assets/sfx-twinkle.mp3`

- [ ] **Step 1: Inventory shared music at workspace root**

Run from workspace root:
```bash
ls *.mp3 assets/*.mp3 2>/dev/null
```

We know the workspace root has:
- `♨️ Free Lofi Music (For Videos) - _Faithful Mission_ by Artificial.Music 🇨🇦 [ ezmp3.cc ].mp3`
- `Epic Inspirational Hip-Hop by Infraction [No Copyright Music]  Motivation.mp3`

Neither is a warm ambient pad, but either could be used as a bed if we filter aggressively. Prefer the lofi track — it's closer to "ambient."

- [ ] **Step 2: Generate a 30s warm pad from ffmpeg sine synthesis (deterministic, no licensing)**

This is the cleanest option — generates a three-voice sine drone mixed together, fades in/out. No external source needed.

Run from inside `video-projects/linear-promo-30s/`:
```bash
ffmpeg -y \
  -f lavfi -i "sine=frequency=110:duration=30,volume=0.4" \
  -f lavfi -i "sine=frequency=165:duration=30,volume=0.3" \
  -f lavfi -i "sine=frequency=220:duration=30,volume=0.25" \
  -filter_complex "[0:a][1:a][2:a]amix=inputs=3:duration=longest:normalize=0,afade=t=in:st=0:d=1.5,afade=t=out:st=28.5:d=1.5,lowpass=f=1200,volume=0.8[out]" \
  -map "[out]" -c:a libmp3lame -b:a 128k assets/warm-pad.mp3
```

Expected: creates a 30s MP3 at `assets/warm-pad.mp3`. Duration should be ~30s (ffprobe to verify).

- [ ] **Step 3: Verify duration**

```bash
ffprobe -v error -show_entries format=duration -of default=nw=1:nk=1 assets/warm-pad.mp3
```

Expected: `30.000000` (or within 0.05s).

- [ ] **Step 4: Generate SFX — two whooshes and a twinkle**

For whooshes, use a filtered noise burst. For the twinkle, use a short high-frequency sine decay.

```bash
# Whoosh 1 — 0.4s, filtered white noise with sweep
ffmpeg -y \
  -f lavfi -i "anoisesrc=color=white:duration=0.4:amplitude=0.6" \
  -filter_complex "highpass=f=200,lowpass=f=6000,afade=t=in:st=0:d=0.05,afade=t=out:st=0.2:d=0.2,volume=0.7" \
  -c:a libmp3lame -b:a 128k assets/sfx-whoosh-1.mp3

# Whoosh 2 — 0.4s, slightly different freq profile
ffmpeg -y \
  -f lavfi -i "anoisesrc=color=pink:duration=0.4:amplitude=0.6" \
  -filter_complex "highpass=f=150,lowpass=f=4500,afade=t=in:st=0:d=0.08,afade=t=out:st=0.15:d=0.25,volume=0.7" \
  -c:a libmp3lame -b:a 128k assets/sfx-whoosh-2.mp3

# Twinkle — 0.5s, two sine tones decaying
ffmpeg -y \
  -f lavfi -i "sine=frequency=2200:duration=0.5" \
  -f lavfi -i "sine=frequency=3300:duration=0.5" \
  -filter_complex "[0:a][1:a]amix=inputs=2:duration=longest,afade=t=out:st=0.05:d=0.45,volume=0.5" \
  -c:a libmp3lame -b:a 128k assets/sfx-twinkle.mp3
```

Expected: three MP3s under `assets/`, each < 20KB.

- [ ] **Step 5: Verify SFX files**

```bash
ls -la assets/sfx-*.mp3 assets/warm-pad.mp3
```

All four files present, reasonable sizes.

- [ ] **Step 6: Commit**

```bash
git add video-projects/linear-promo-30s/assets/*.mp3
git commit -m "assets(linear): warm pad + whoosh/twinkle SFX generated via ffmpeg"
```

---

## Task 5: Author `01-problem-type.html` — kinetic type opener (0–4s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/01-problem-type.html`
- Modify: `video-projects/linear-promo-30s/index.html` (splice the `<div class="beat-layer">`)

This is Act 1 beat #1: black canvas → "Product management" word reveal → "is broken" scales 8× → whip at 3.8s. Slot duration: 4s.

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/01-problem-type.html`

```html
<template id="01-problem-type-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&family=Instrument+Serif:ital@0;1&display=block" rel="stylesheet" />

  <div
    data-composition-id="01-problem-type"
    data-start="0"
    data-duration="4"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-01">
      <!-- Perspective grid floor (Law #10 unifying texture) -->
      <div class="grid-floor"></div>

      <!-- Crosshair registration marks -->
      <div class="crosshair" style="top: 20%; left: 20%;">+</div>
      <div class="crosshair" style="top: 20%; left: 80%;">+</div>
      <div class="crosshair" style="top: 80%; left: 20%;">+</div>
      <div class="crosshair" style="top: 80%; left: 80%;">+</div>

      <!-- Kinetic type -->
      <div class="type-stage">
        <div class="line-1">
          <span class="word w1">Product</span>
          <span class="word w2">management</span>
        </div>
        <div class="line-2">
          <span class="word w3">is</span>
          <span class="hero w4">broken</span>
        </div>
      </div>

      <!-- Whip streak (fires at t=3.8s, masks cut to comp 02) -->
      <div class="whip-streak"></div>

      <!-- Vignette (always on top of content, under captions) -->
      <div class="vignette"></div>
      <!-- Grain (top layer) -->
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="01-problem-type"] {
        position: absolute; inset: 0; overflow: hidden;
        background: #000;
      }
      [data-composition-id="01-problem-type"] .stage-01 {
        position: absolute; inset: 0;
      }
      [data-composition-id="01-problem-type"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        transform-origin: 50% 50%;
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.045) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.045) 0 1px, transparent 1px 80px);
        background-color: #000;
        will-change: background-position, transform;
      }
      [data-composition-id="01-problem-type"] .crosshair {
        position: absolute;
        transform: translate(-50%, -50%);
        font: 400 28px/1 monospace;
        color: rgba(255,255,255,0.25);
        pointer-events: none;
      }
      [data-composition-id="01-problem-type"] .type-stage {
        position: absolute;
        top: 50%; left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
        width: 100%;
      }
      [data-composition-id="01-problem-type"] .line-1,
      [data-composition-id="01-problem-type"] .line-2 {
        display: flex;
        justify-content: center;
        gap: 28px;
        align-items: baseline;
        white-space: nowrap;
      }
      [data-composition-id="01-problem-type"] .line-1 { margin-bottom: 24px; }
      [data-composition-id="01-problem-type"] .word {
        display: inline-block;
        font-family: "Space Grotesk", sans-serif;
        font-weight: 500;
        font-size: 120px;
        letter-spacing: -0.02em;
        background: linear-gradient(180deg, #ffffff 0%, #cccccc 60%, #999999 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 40px rgba(255,255,255,0.15);
        opacity: 0;
        transform: translateX(0px);
        will-change: opacity, transform;
      }
      [data-composition-id="01-problem-type"] .hero {
        display: inline-block;
        font-family: "Space Grotesk", sans-serif;
        font-weight: 700;
        font-size: 200px;
        letter-spacing: -0.04em;
        background: linear-gradient(180deg, #ffffff 0%, #dddddd 60%, #888888 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 60px rgba(255,255,255,0.35), 0 0 100px rgba(255,255,255,0.2);
        opacity: 0;
        transform: scale(1);
        will-change: opacity, transform;
      }
      [data-composition-id="01-problem-type"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.7;
      }
      [data-composition-id="01-problem-type"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.015) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px, 7px 7px;
        background-position: 0 0, 1px 2px, 2px 1px;
      }
      [data-composition-id="01-problem-type"] .whip-streak {
        position: absolute;
        top: 50%; left: 0;
        width: 60%; height: 10px;
        background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.95) 50%, transparent 100%);
        filter: blur(8px);
        transform: translate(-150%, -50%);
        opacity: 0;
        pointer-events: none;
        z-index: 30;
      }
    </style>

    <script>
      (() => {
        const SLOT = 4;
        const tl = gsap.timeline({ paused: true });

        // Grid parallax — subtle recede
        tl.to('[data-composition-id="01-problem-type"] .grid-floor',
          { backgroundPositionY: '-120px', duration: SLOT, ease: 'none' }, 0);

        // Vignette breath
        tl.to('[data-composition-id="01-problem-type"] .vignette',
          { opacity: 0.9, duration: 2, yoyo: true, repeat: 1, ease: 'sine.inOut' }, 0);

        // "Product" — carrier slide 360px
        tl.fromTo('[data-composition-id="01-problem-type"] .w1',
          { x: 360, opacity: 0 },
          { x: 0, opacity: 1, duration: 0.33, ease: 'expo.out' }, 0.8);

        // "management" — decay to 120px
        tl.fromTo('[data-composition-id="01-problem-type"] .w2',
          { x: 120, opacity: 0 },
          { x: 0, opacity: 1, duration: 0.30, ease: 'expo.out' }, 1.2);

        // "is" — decay to 25px
        tl.fromTo('[data-composition-id="01-problem-type"] .w3',
          { x: 25, opacity: 0 },
          { x: 0, opacity: 1, duration: 0.25, ease: 'expo.out' }, 1.7);

        // "broken" — hero arrive
        tl.fromTo('[data-composition-id="01-problem-type"] .w4',
          { scale: 0.85, opacity: 0 },
          { scale: 1, opacity: 1, duration: 0.4, ease: 'back.out(1.4)' }, 1.95);

        // Hero kinetic dolly — scale 1→5 on "broken"
        tl.to('[data-composition-id="01-problem-type"] .w4',
          { scale: 5, opacity: 0, duration: 1.2, ease: 'power2.in' }, 2.4);

        // Fade other words as we dolly
        tl.to('[data-composition-id="01-problem-type"] .w1, [data-composition-id="01-problem-type"] .w2, [data-composition-id="01-problem-type"] .w3',
          { opacity: 0, duration: 0.5, ease: 'power2.in' }, 2.5);

        // Grid tilt during dolly
        tl.to('[data-composition-id="01-problem-type"] .grid-floor',
          { rotate: 5, duration: 1.0, ease: 'power2.in' }, 2.6);

        // Whip streak at 3.6s, peaks at 3.8, exits by 4.0 (masks the cut)
        tl.set('[data-composition-id="01-problem-type"] .whip-streak', { opacity: 1 }, 3.6);
        tl.fromTo('[data-composition-id="01-problem-type"] .whip-streak',
          { xPercent: -150, scaleX: 0.5 },
          { xPercent: 250, scaleX: 1.5, duration: 0.4, ease: 'power3.in' }, 3.6);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['01-problem-type'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice the beat-layer into `index.html`**

In `video-projects/linear-promo-30s/index.html`, replace the comment line `<!-- Sub-compositions will be inserted here as we author each beat -->` with:

```html
      <!-- Beat 01 — Problem kinetic type (0–4s) -->
      <div class="beat-layer"
        data-composition-id="01-problem-type"
        data-composition-src="compositions/01-problem-type.html"
        data-start="0" data-duration="4" data-track-index="0"
        data-width="1920" data-height="1080"></div>

      <!-- Sub-compositions will be inserted here as we author each beat -->
```

Keep the trailing comment — subsequent tasks will splice above it.

- [ ] **Step 3: Lint**

From inside `video-projects/linear-promo-30s/`:
```bash
npx hyperframes lint
```

Expected: no errors. Warnings about missing sub-comps for 02–08 are fine.

- [ ] **Step 4: Preview in Studio — gate 1**

Start Studio in the background, then hand the URL to the user:
```bash
npx hyperframes preview
```

Studio runs on http://localhost:3002. Tell the user: "Beat 01 is live — scrub `http://localhost:3002/?comp=01-problem-type`. Wait for sign-off before moving on."

WAIT for the user's explicit "looks good" / "go" before Step 5.

- [ ] **Step 5: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/01-problem-type.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 01 — problem kinetic type (Product management is broken)"
```

---

## Task 6: Author `02-card-to-logo.html` — red card → L-bracket morph + symbol rise (4–8s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/02-card-to-logo.html`
- Modify: `video-projects/linear-promo-30s/index.html`

Beats inside this 4s slot: red card lands 4.0–6.0 (local 0.0–2.0) → morph 6.0–7.0 (local 2.0–3.0) → symbol rises 7.0–8.0 (local 3.0–4.0).

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/02-card-to-logo.html`

```html
<template id="02-card-to-logo-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>

  <div
    data-composition-id="02-card-to-logo"
    data-start="0"
    data-duration="4"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-02">
      <div class="grid-floor"></div>

      <!-- Red card (the "problem" metaphor) -->
      <div class="red-card">
        <div class="card-gloss"></div>
        <div class="card-label">JIRA-2847</div>
      </div>

      <!-- Linear L-bracket symbol (the "solution") -->
      <div class="linear-symbol">
        <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <linearGradient id="grad-02" x1="0" y1="0" x2="1" y2="1">
              <stop offset="0%" stop-color="#5E6AD2"/>
              <stop offset="100%" stop-color="#A9B0F0"/>
            </linearGradient>
          </defs>
          <path d="M 25 18 L 75 18 L 75 34 L 41 34 L 41 82 L 25 82 Z" fill="url(#grad-02)" />
        </svg>
      </div>

      <!-- Light pillar behind symbol (for "rise" beat) -->
      <div class="light-pillar"></div>

      <!-- Morph streak (fires at local 2.0s) -->
      <div class="morph-streak"></div>

      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="02-card-to-logo"] {
        position: absolute; inset: 0; overflow: hidden; background: #000;
      }
      [data-composition-id="02-card-to-logo"] .stage-02 { position: absolute; inset: 0; }
      [data-composition-id="02-card-to-logo"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.045) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.045) 0 1px, transparent 1px 80px);
        background-color: #000;
      }
      [data-composition-id="02-card-to-logo"] .red-card {
        position: absolute;
        top: 50%; left: 50%;
        width: 420px; height: 260px;
        border-radius: 24px;
        background:
          linear-gradient(135deg, #e10b1f 0%, #a8081a 60%, #6a050f 100%);
        box-shadow:
          inset 0 1px 0 rgba(255,255,255,0.35),
          0 30px 80px rgba(225,11,31,0.35);
        transform: translate(-50%, -50%) perspective(900px) rotateX(10deg) rotateY(-6deg);
        transform-origin: center center;
        filter: blur(0px);
        will-change: transform, opacity, filter;
      }
      [data-composition-id="02-card-to-logo"] .card-gloss {
        position: absolute; inset: 0;
        background: linear-gradient(120deg, rgba(255,255,255,0.25) 0%, transparent 45%);
        border-radius: 24px;
        pointer-events: none;
      }
      [data-composition-id="02-card-to-logo"] .card-label {
        position: absolute;
        top: 28px; left: 32px;
        font: 700 22px/1 "SF Mono", ui-monospace, monospace;
        letter-spacing: 0.15em;
        color: rgba(255,255,255,0.85);
      }
      [data-composition-id="02-card-to-logo"] .linear-symbol {
        position: absolute;
        top: 50%; left: 50%;
        width: 300px; height: 300px;
        transform: translate(-50%, -50%) scale(0.6);
        opacity: 0;
        filter: drop-shadow(0 0 40px rgba(94,106,210,0.6));
        will-change: transform, opacity;
      }
      [data-composition-id="02-card-to-logo"] .linear-symbol svg { width: 100%; height: 100%; }
      [data-composition-id="02-card-to-logo"] .light-pillar {
        position: absolute;
        top: 10%; left: 50%;
        width: 6px; height: 80%;
        transform: translateX(-50%) scaleY(0);
        transform-origin: center bottom;
        background: linear-gradient(180deg, transparent 0%, rgba(169,176,240,0.5) 50%, transparent 100%);
        filter: blur(4px);
        opacity: 0;
      }
      [data-composition-id="02-card-to-logo"] .morph-streak {
        position: absolute;
        top: 50%; left: 0;
        width: 80%; height: 14px;
        background: linear-gradient(90deg, transparent, rgba(255,255,255,0.95), rgba(169,176,240,0.7), transparent);
        filter: blur(10px);
        transform: translate(-150%, -50%);
        opacity: 0;
        z-index: 20;
      }
      [data-composition-id="02-card-to-logo"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.75;
      }
      [data-composition-id="02-card-to-logo"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px;
      }
    </style>

    <script>
      (() => {
        const SLOT = 4;
        const tl = gsap.timeline({ paused: true });

        // Card lands with motion blur
        tl.fromTo('[data-composition-id="02-card-to-logo"] .red-card',
          { x: '-50%', y: '-150%', opacity: 0, filter: 'blur(20px)' },
          { y: '-50%', opacity: 1, filter: 'blur(0px)', duration: 0.8, ease: 'power3.out' }, 0.1);

        // Card orbit drift
        tl.to('[data-composition-id="02-card-to-logo"] .red-card',
          { rotate: 3, duration: 1.5, ease: 'sine.inOut' }, 0.9);

        // Morph streak fires at local 2.0s (absolute 6.0s)
        tl.set('[data-composition-id="02-card-to-logo"] .morph-streak', { opacity: 1 }, 1.9);
        tl.fromTo('[data-composition-id="02-card-to-logo"] .morph-streak',
          { xPercent: -150, scaleX: 0.5 },
          { xPercent: 250, scaleX: 1.8, duration: 0.5, ease: 'power3.in' }, 1.9);

        // Card exits with drift + blur
        tl.to('[data-composition-id="02-card-to-logo"] .red-card',
          { x: '30%', scale: 0.4, opacity: 0, filter: 'blur(30px)', duration: 0.5, ease: 'power3.in' }, 2.0);

        // Symbol enters from opposite vector, at peak of streak
        tl.fromTo('[data-composition-id="02-card-to-logo"] .linear-symbol',
          { x: '-80%', scale: 0.4, opacity: 0, filter: 'blur(30px)' },
          { x: '-50%', scale: 1, opacity: 1, filter: 'blur(0px)', duration: 0.6, ease: 'power3.out' }, 2.15);

        // Symbol spin/rise on vertical pillar at local 3.0s
        tl.to('[data-composition-id="02-card-to-logo"] .light-pillar',
          { opacity: 1, scaleY: 1, duration: 0.4, ease: 'power2.out' }, 2.9);

        tl.fromTo('[data-composition-id="02-card-to-logo"] .linear-symbol',
          { rotateY: 0 },
          { rotateY: 360, duration: 1.0, ease: 'power2.inOut' }, 3.0);

        // Subtle shimmer on symbol (text-shadow pulse via filter)
        tl.to('[data-composition-id="02-card-to-logo"] .linear-symbol',
          { filter: 'drop-shadow(0 0 70px rgba(169,176,240,0.85))', duration: 0.5, yoyo: true, repeat: 1, ease: 'sine.inOut' }, 3.0);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['02-card-to-logo'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice into `index.html`**

Replace the trailing `<!-- Sub-compositions will be inserted here -->` comment with:

```html
      <!-- Beat 02 — Red card → L-bracket morph (4–8s) -->
      <div class="beat-layer"
        data-composition-id="02-card-to-logo"
        data-composition-src="compositions/02-card-to-logo.html"
        data-start="4" data-duration="4" data-track-index="0"
        data-width="1920" data-height="1080"></div>

      <!-- Sub-compositions will be inserted here as we author each beat -->
```

- [ ] **Step 3: Lint**

```bash
npx hyperframes lint
```

- [ ] **Step 4: Preview gate 1 — Studio**

Start preview (if not still running from last task, it hot-reloads). Hand the user `http://localhost:3002/?comp=02-card-to-logo`. Wait for sign-off.

- [ ] **Step 5: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/02-card-to-logo.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 02 — red card → Linear L-bracket morph"
```

---

## Task 7: Author `03-brand-reveal.html` — wordmark crystallization (8–10s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/03-brand-reveal.html`
- Modify: `video-projects/linear-promo-30s/index.html`

Slot: 2s. L-bracket shrinks to its resting position left of wordmark while the wordmark "Linear" chrome-gradient fades up. Then 1s hold with shimmer sweep. Twinkle SFX at t=0.3 in this slot (absolute 8.3).

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/03-brand-reveal.html`

```html
<template id="03-brand-reveal-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@500;600;700&display=block" rel="stylesheet" />

  <div
    data-composition-id="03-brand-reveal"
    data-start="0"
    data-duration="2"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-03">
      <div class="grid-floor"></div>

      <div class="lockup">
        <div class="symbol-slot">
          <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
            <defs>
              <linearGradient id="grad-03" x1="0" y1="0" x2="1" y2="1">
                <stop offset="0%" stop-color="#5E6AD2"/>
                <stop offset="100%" stop-color="#A9B0F0"/>
              </linearGradient>
            </defs>
            <path d="M 25 18 L 75 18 L 75 34 L 41 34 L 41 82 L 25 82 Z" fill="url(#grad-03)" />
          </svg>
        </div>
        <div class="wordmark">Linear</div>
        <div class="shimmer"></div>
      </div>

      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="03-brand-reveal"] {
        position: absolute; inset: 0; overflow: hidden; background: #000;
      }
      [data-composition-id="03-brand-reveal"] .stage-03 { position: absolute; inset: 0; }
      [data-composition-id="03-brand-reveal"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.04) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 1px, transparent 1px 80px);
        background-color: #000;
      }
      [data-composition-id="03-brand-reveal"] .lockup {
        position: absolute;
        top: 50%; left: 50%;
        transform: translate(-50%, -50%);
        display: flex;
        align-items: center;
        gap: 32px;
      }
      [data-composition-id="03-brand-reveal"] .symbol-slot {
        width: 120px; height: 120px;
        filter: drop-shadow(0 0 40px rgba(169,176,240,0.6));
      }
      [data-composition-id="03-brand-reveal"] .symbol-slot svg { width: 100%; height: 100%; }
      [data-composition-id="03-brand-reveal"] .wordmark {
        font-family: "Inter", sans-serif;
        font-weight: 600;
        font-size: 180px;
        letter-spacing: -0.04em;
        background: linear-gradient(180deg, #ffffff 0%, #cccccc 60%, #888888 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 60px rgba(255,255,255,0.3);
        opacity: 0;
        transform: translateY(40px);
        will-change: opacity, transform;
      }
      [data-composition-id="03-brand-reveal"] .shimmer {
        position: absolute;
        top: 0; left: 0;
        width: 200px; height: 100%;
        background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.45) 50%, transparent 100%);
        filter: blur(4px);
        transform: translateX(-200%);
        opacity: 0;
        mix-blend-mode: screen;
        pointer-events: none;
      }
      [data-composition-id="03-brand-reveal"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.75;
      }
      [data-composition-id="03-brand-reveal"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px;
      }
    </style>

    <script>
      (() => {
        const SLOT = 2;
        const tl = gsap.timeline({ paused: true });

        // Wordmark fade-up with chrome gradient already applied in CSS
        tl.to('[data-composition-id="03-brand-reveal"] .wordmark',
          { y: 0, opacity: 1, duration: 0.6, ease: 'power3.out' }, 0.15);

        // Symbol settles in size (from large/rising to resting size)
        tl.fromTo('[data-composition-id="03-brand-reveal"] .symbol-slot',
          { scale: 2.2, opacity: 0 },
          { scale: 1, opacity: 1, duration: 0.6, ease: 'power3.out' }, 0.05);

        // Shimmer sweep at 1.2s
        tl.to('[data-composition-id="03-brand-reveal"] .shimmer',
          { opacity: 1, duration: 0.1 }, 1.2);
        tl.to('[data-composition-id="03-brand-reveal"] .shimmer',
          { xPercent: 500, duration: 0.7, ease: 'power2.inOut' }, 1.2);
        tl.to('[data-composition-id="03-brand-reveal"] .shimmer',
          { opacity: 0, duration: 0.1 }, 1.85);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['03-brand-reveal'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice into index.html**

Add above the trailing placeholder comment:

```html
      <!-- Beat 03 — Brand reveal (8–10s) -->
      <div class="beat-layer"
        data-composition-id="03-brand-reveal"
        data-composition-src="compositions/03-brand-reveal.html"
        data-start="8" data-duration="2" data-track-index="0"
        data-width="1920" data-height="1080"></div>
```

- [ ] **Step 3: Lint**

```bash
npx hyperframes lint
```

- [ ] **Step 4: Preview gate — hand the user `http://localhost:3002/?comp=03-brand-reveal`, wait for sign-off.**

- [ ] **Step 5: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/03-brand-reveal.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 03 — Linear wordmark crystallization"
```

---

## Task 8: Author `04-benefits-flowchart.html` — Fast / Focused / AI-native color recolor (10–14.8s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/04-benefits-flowchart.html`
- Modify: `video-projects/linear-promo-30s/index.html`

Slot: 4.8s. Signature move #1 — color recolor trick §3.5. Three benefit words (Azeret Mono upper-right chrome) + a flowchart that recolors from purple (Fast) → blue (Focused) → neon orange-yellow (AI-native) via CSS variables. NO scene cuts inside.

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/04-benefits-flowchart.html`

```html
<template id="04-benefits-flowchart-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Azeret+Mono:wght@500;700&display=block" rel="stylesheet" />

  <div
    data-composition-id="04-benefits-flowchart"
    data-start="0"
    data-duration="4.8"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-04" style="--edge: #A155FF; --node-glow: rgba(161,85,255,0.7); --energy: #A155FF;">
      <div class="grid-floor"></div>

      <!-- Benefit words upper-right -->
      <div class="benefit-stack">
        <div class="benefit b-fast">Fast</div>
        <div class="benefit b-focused">Focused</div>
        <div class="benefit b-ai">AI-native</div>
      </div>

      <!-- Flowchart: SVG nodes + edges, all tinted via CSS var -->
      <svg class="flow" viewBox="0 0 1920 1080" xmlns="http://www.w3.org/2000/svg">
        <!-- Edges first so they sit under nodes -->
        <g class="edges" stroke="var(--edge)" stroke-width="2" fill="none" stroke-linecap="round">
          <line class="edge e1" x1="480" y1="540" x2="720" y2="380" />
          <line class="edge e2" x1="480" y1="540" x2="720" y2="540" />
          <line class="edge e3" x1="480" y1="540" x2="720" y2="700" />
          <line class="edge e4" x1="800" y1="380" x2="1040" y2="300" />
          <line class="edge e5" x1="800" y1="540" x2="1040" y2="540" />
          <line class="edge e6" x1="800" y1="700" x2="1040" y2="780" />
          <line class="edge e7" x1="1120" y1="300" x2="1360" y2="420" />
          <line class="edge e8" x1="1120" y1="540" x2="1360" y2="540" />
          <line class="edge e9" x1="1120" y1="780" x2="1360" y2="660" />
        </g>

        <g class="nodes">
          <circle class="node n1" cx="440" cy="540" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n2" cx="760" cy="380" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n3" cx="760" cy="540" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n4" cx="760" cy="700" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n5" cx="1080" cy="300" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n6" cx="1080" cy="540" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n7" cx="1080" cy="780" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n8" cx="1400" cy="420" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n9" cx="1400" cy="540" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
          <circle class="node n10" cx="1400" cy="660" r="32" fill="#0a0a0a" stroke="var(--edge)" stroke-width="2" />
        </g>
      </svg>

      <!-- Lightning comet for "Fast" beat -->
      <div class="comet"></div>

      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="04-benefits-flowchart"] {
        position: absolute; inset: 0; overflow: hidden; background: #000;
      }
      [data-composition-id="04-benefits-flowchart"] .stage-04 { position: absolute; inset: 0; }
      [data-composition-id="04-benefits-flowchart"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.04) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 1px, transparent 1px 80px);
        background-color: #000;
      }
      [data-composition-id="04-benefits-flowchart"] .benefit-stack {
        position: absolute;
        top: 80px; right: 120px;
        text-align: right;
        font-family: "Azeret Mono", monospace;
        font-weight: 700;
      }
      [data-composition-id="04-benefits-flowchart"] .benefit {
        font-size: 96px;
        letter-spacing: -0.02em;
        line-height: 1.1;
        background: linear-gradient(180deg, #ffffff 0%, #bbbbbb 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 40px var(--node-glow);
        opacity: 0;
        transform: translateX(60px);
        will-change: opacity, transform;
      }
      [data-composition-id="04-benefits-flowchart"] .flow {
        position: absolute; inset: 0;
        width: 100%; height: 100%;
        opacity: 0;
      }
      [data-composition-id="04-benefits-flowchart"] .edge {
        stroke-dasharray: 240;
        stroke-dashoffset: 240;
        filter: drop-shadow(0 0 6px var(--energy));
      }
      [data-composition-id="04-benefits-flowchart"] .node {
        filter: drop-shadow(0 0 12px var(--node-glow));
      }
      [data-composition-id="04-benefits-flowchart"] .comet {
        position: absolute;
        top: 42%; left: 0;
        width: 70%; height: 14px;
        background: linear-gradient(90deg, transparent 0%, rgba(161,85,255,0.95) 40%, #ffffff 55%, transparent 100%);
        filter: blur(10px);
        transform: translate(-150%, 0);
        opacity: 0;
      }
      [data-composition-id="04-benefits-flowchart"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.7;
      }
      [data-composition-id="04-benefits-flowchart"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px;
      }
    </style>

    <script>
      (() => {
        const SLOT = 4.8;
        const tl = gsap.timeline({ paused: true });
        const sel = (q) => `[data-composition-id="04-benefits-flowchart"] ${q}`;

        // Lightning comet rips L→R (Fast beat, local 0.0–0.4)
        tl.set(sel('.comet'), { opacity: 1 }, 0);
        tl.fromTo(sel('.comet'),
          { xPercent: -150 }, { xPercent: 250, duration: 0.4, ease: 'power3.in' }, 0);
        tl.to(sel('.comet'), { opacity: 0, duration: 0.2 }, 0.4);

        // "Fast" word appears as comet passes
        tl.to(sel('.b-fast'),
          { opacity: 1, x: 0, duration: 0.35, ease: 'expo.out' }, 0.25);

        // Flowchart fades in (local 1.2s)
        tl.to(sel('.flow'), { opacity: 1, duration: 0.4, ease: 'power2.out' }, 1.2);

        // Edges dash-draw in cascade
        tl.to(sel('.edges .edge'),
          { strokeDashoffset: 0, duration: 0.6, stagger: 0.05, ease: 'power2.out' }, 1.2);

        // "Focused" — recolor to blue at local 2.3
        tl.to(sel('.b-focused'),
          { opacity: 1, x: 0, duration: 0.35, ease: 'expo.out' }, 2.3);
        tl.to(sel('.stage-04'),
          { '--edge': '#5db4ff', '--node-glow': 'rgba(91,180,255,0.7)', '--energy': '#5db4ff',
            duration: 0.5, ease: 'power2.inOut' }, 2.3);

        // "AI-native" — recolor to neon orange/yellow at local 3.3 (signature move)
        tl.to(sel('.b-ai'),
          { opacity: 1, x: 0, duration: 0.4, ease: 'expo.out' }, 3.3);
        tl.to(sel('.stage-04'),
          { '--edge': '#FFD84A', '--node-glow': 'rgba(255,148,48,0.7)', '--energy': '#FFD84A',
            duration: 0.6, ease: 'power2.inOut' }, 3.3);

        // Ai-native gets a halo emphasis
        tl.to(sel('.b-ai'),
          { textShadow: '0 0 80px rgba(255,216,74,0.8), 0 0 40px rgba(255,148,48,0.6)',
            duration: 0.4, ease: 'power2.out' }, 3.5);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['04-benefits-flowchart'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice into index.html**

```html
      <!-- Beat 04 — Benefits flowchart recolor (10–14.8s) -->
      <div class="beat-layer"
        data-composition-id="04-benefits-flowchart"
        data-composition-src="compositions/04-benefits-flowchart.html"
        data-start="10" data-duration="4.8" data-track-index="0"
        data-width="1920" data-height="1080"></div>
```

- [ ] **Step 3: Lint**

```bash
npx hyperframes lint
```

- [ ] **Step 4: Preview gate — `http://localhost:3002/?comp=04-benefits-flowchart`, wait for sign-off.**

- [ ] **Step 5: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/04-benefits-flowchart.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 04 — Fast/Focused/AI-native flowchart recolor"
```

---

## Task 9: Author `05-product-surfaces.html` — Plan / Build / Monitor mockups (14.8–19.5s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/05-product-surfaces.html`
- Modify: `video-projects/linear-promo-30s/index.html`

Slot: 4.7s. Three product surfaces with real Linear screenshots inside phone/laptop bezels. Plan slides up (0.0–1.7), Build cross-warps (1.7–3.2), Monitor slides up from opposite side (3.2–4.7). "Build and deploy AI agents that work alongside your team" micro-caption during Build.

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/05-product-surfaces.html`

```html
<template id="05-product-surfaces-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Inter:wght@400;500&display=block" rel="stylesheet" />

  <div
    data-composition-id="05-product-surfaces"
    data-start="0"
    data-duration="4.7"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-05">
      <div class="grid-floor"></div>

      <!-- Surface 1: Plan (phone, slides up from bottom) -->
      <div class="surface s-plan">
        <div class="headline h-plan">Plan</div>
        <div class="device phone">
          <div class="screen" style="background-image: url('assets/screenshot-plan.png');"></div>
        </div>
      </div>

      <!-- Surface 2: Build (laptop, cross-warps in) -->
      <div class="surface s-build">
        <div class="headline h-build">Build</div>
        <div class="device laptop">
          <div class="screen" style="background-image: url('assets/screenshot-build.png');"></div>
          <div class="laptop-base"></div>
        </div>
        <div class="micro-cap">Build and deploy AI agents that work alongside your team</div>
      </div>

      <!-- Surface 3: Monitor (phone, slides up from opposite side) -->
      <div class="surface s-monitor">
        <div class="headline h-monitor">Monitor</div>
        <div class="device phone">
          <div class="screen" style="background-image: url('assets/screenshot-monitor.png');"></div>
        </div>
      </div>

      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="05-product-surfaces"] {
        position: absolute; inset: 0; overflow: hidden; background: #000;
      }
      [data-composition-id="05-product-surfaces"] .stage-05 { position: absolute; inset: 0; }
      [data-composition-id="05-product-surfaces"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.04) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 1px, transparent 1px 80px);
        background-color: #000;
      }
      [data-composition-id="05-product-surfaces"] .surface {
        position: absolute;
        inset: 0;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 40px;
        opacity: 0;
        will-change: opacity, transform;
      }
      [data-composition-id="05-product-surfaces"] .headline {
        font-family: "Instrument Serif", serif;
        font-style: italic;
        font-size: 140px;
        line-height: 1;
        background: linear-gradient(180deg, #ffffff 0%, #bbbbbb 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 40px rgba(255,255,255,0.25);
      }
      [data-composition-id="05-product-surfaces"] .device {
        position: relative;
        box-shadow:
          0 40px 80px rgba(0,0,0,0.8),
          inset 0 1px 0 rgba(255,255,255,0.2),
          0 0 0 2px rgba(255,255,255,0.08);
        border-radius: 36px;
        overflow: hidden;
      }
      [data-composition-id="05-product-surfaces"] .phone {
        width: 380px; height: 780px;
        border-radius: 48px;
        border: 3px solid rgba(255,255,255,0.12);
      }
      [data-composition-id="05-product-surfaces"] .laptop {
        width: 1100px; height: 680px;
        border-radius: 12px;
        border: 2px solid rgba(255,255,255,0.12);
      }
      [data-composition-id="05-product-surfaces"] .laptop-base {
        position: absolute;
        bottom: -18px; left: -40px;
        width: calc(100% + 80px); height: 18px;
        background: linear-gradient(180deg, #2a2a2a, #0a0a0a);
        border-radius: 0 0 20px 20px;
      }
      [data-composition-id="05-product-surfaces"] .screen {
        width: 100%; height: 100%;
        background-size: cover;
        background-position: center top;
      }
      [data-composition-id="05-product-surfaces"] .micro-cap {
        position: absolute;
        bottom: 80px; left: 50%;
        transform: translateX(-50%);
        font-family: "Inter", sans-serif;
        font-size: 28px;
        font-weight: 500;
        color: rgba(255,255,255,0.85);
        letter-spacing: 0.01em;
        padding: 10px 20px;
        border-radius: 10px;
        background: rgba(10,8,5,0.55);
        backdrop-filter: blur(8px);
        opacity: 0;
      }
      [data-composition-id="05-product-surfaces"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.7;
      }
      [data-composition-id="05-product-surfaces"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px;
      }
    </style>

    <script>
      (() => {
        const SLOT = 4.7;
        const tl = gsap.timeline({ paused: true });
        const sel = (q) => `[data-composition-id="05-product-surfaces"] ${q}`;

        // S-Plan: slide up from bottom, stay 0.0–1.7
        gsap.set(sel('.s-plan'), { y: '100%', opacity: 0 });
        tl.to(sel('.s-plan'), { y: '0%', opacity: 1, duration: 0.7, ease: 'power3.out' }, 0);
        tl.to(sel('.s-plan'), { x: '-40%', opacity: 0, filter: 'blur(20px)', duration: 0.4, ease: 'power2.in' }, 1.5);

        // S-Build: cross-warps in from right, 1.5–3.2 (overlap with plan exit)
        gsap.set(sel('.s-build'), { x: '40%', opacity: 0, filter: 'blur(25px)' });
        tl.to(sel('.s-build'),
          { x: '0%', opacity: 1, filter: 'blur(0px)', duration: 0.7, ease: 'power3.out' }, 1.5);

        // Micro-cap on Build surface, local 2.0–3.0
        tl.to(sel('.micro-cap'), { opacity: 1, duration: 0.4, ease: 'power2.out' }, 2.0);
        tl.to(sel('.micro-cap'), { opacity: 0, duration: 0.3, ease: 'power2.in' }, 3.0);

        // S-Build exit, 3.1
        tl.to(sel('.s-build'),
          { y: '-30%', opacity: 0, filter: 'blur(20px)', duration: 0.4, ease: 'power2.in' }, 3.0);

        // S-Monitor: slides up from bottom, 3.0–4.7
        gsap.set(sel('.s-monitor'), { y: '100%', opacity: 0 });
        tl.to(sel('.s-monitor'), { y: '0%', opacity: 1, duration: 0.7, ease: 'power3.out' }, 3.0);

        // Monitor exit (into wheel scene)
        tl.to(sel('.s-monitor'),
          { scale: 0.8, opacity: 0, filter: 'blur(20px)', duration: 0.35, ease: 'power2.in' }, 4.35);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['05-product-surfaces'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice into index.html**

```html
      <!-- Beat 05 — Plan/Build/Monitor surfaces (14.8–19.5s) -->
      <div class="beat-layer"
        data-composition-id="05-product-surfaces"
        data-composition-src="compositions/05-product-surfaces.html"
        data-start="14.8" data-duration="4.7" data-track-index="0"
        data-width="1920" data-height="1080"></div>
```

- [ ] **Step 3: Lint**

```bash
npx hyperframes lint
```

- [ ] **Step 4: Preview gate — `http://localhost:3002/?comp=05-product-surfaces`, wait for sign-off. Check that screenshots render inside bezels and aren't cropped weirdly.**

- [ ] **Step 5: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/05-product-surfaces.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 05 — Plan/Build/Monitor product surfaces"
```

---

## Task 10: Author `06-wheel-pillars.html` — chrome wheel + side-panel pattern (19.5–22s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/06-wheel-pillars.html`
- Modify: `video-projects/linear-promo-30s/index.html`

Slot: 2.5s. Signature move #2 — wheel + side-panel. Chrome iridescent wheel center, rotating 360° across the beat. Three panels (Plan / Build / Monitor) slide in from L → R → L with pillar name + one-line description.

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/06-wheel-pillars.html`

```html
<template id="06-wheel-pillars-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Geist:wght@500;700&family=JetBrains+Mono:wght@400;500&display=block" rel="stylesheet" />

  <div
    data-composition-id="06-wheel-pillars"
    data-start="0"
    data-duration="2.5"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-06">
      <div class="grid-floor"></div>

      <!-- Iridescent chrome wheel -->
      <div class="wheel-wrap">
        <div class="wheel">
          <div class="wheel-inner"></div>
          <div class="wheel-tick t0"></div>
          <div class="wheel-tick t1"></div>
          <div class="wheel-tick t2"></div>
          <div class="wheel-tick t3"></div>
          <div class="wheel-tick t4"></div>
          <div class="wheel-tick t5"></div>
          <div class="wheel-tick t6"></div>
          <div class="wheel-tick t7"></div>
        </div>
      </div>

      <!-- Side panels -->
      <div class="panel p-left">
        <div class="pname">Plan</div>
        <div class="pdesc">Roadmaps, cycles, and projects</div>
      </div>
      <div class="panel p-right">
        <div class="pname">Build</div>
        <div class="pdesc">Issues, editor, and agents</div>
      </div>
      <div class="panel p-left p-third">
        <div class="pname">Monitor</div>
        <div class="pdesc">Insights and analytics</div>
      </div>

      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="06-wheel-pillars"] {
        position: absolute; inset: 0; overflow: hidden; background: #000;
      }
      [data-composition-id="06-wheel-pillars"] .stage-06 { position: absolute; inset: 0; }
      [data-composition-id="06-wheel-pillars"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.04) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 1px, transparent 1px 80px);
        background-color: #000;
      }
      [data-composition-id="06-wheel-pillars"] .wheel-wrap {
        position: absolute;
        top: 50%; left: 50%;
        transform: translate(-50%, -50%);
        width: 520px; height: 520px;
        filter: drop-shadow(0 0 80px rgba(169,176,240,0.4));
      }
      [data-composition-id="06-wheel-pillars"] .wheel {
        position: relative;
        width: 100%; height: 100%;
        border-radius: 50%;
        background:
          conic-gradient(from 0deg,
            #5E6AD2 0deg,
            #A155FF 72deg,
            #5db4ff 144deg,
            #FFD84A 216deg,
            #A9B0F0 288deg,
            #5E6AD2 360deg);
        opacity: 0;
      }
      [data-composition-id="06-wheel-pillars"] .wheel-inner {
        position: absolute;
        top: 10%; left: 10%;
        width: 80%; height: 80%;
        border-radius: 50%;
        background:
          radial-gradient(circle at 35% 30%, rgba(255,255,255,0.15) 0%, transparent 40%),
          linear-gradient(145deg, #0d0d18 0%, #1a1a28 50%, #0a0a14 100%);
        border: 2px solid rgba(169,176,240,0.3);
        box-shadow: inset 0 0 60px rgba(94,106,210,0.3);
      }
      [data-composition-id="06-wheel-pillars"] .wheel-tick {
        position: absolute;
        top: 50%; left: 50%;
        width: 4px; height: 24px;
        background: rgba(255,255,255,0.4);
        transform-origin: 50% 240px;
      }
      [data-composition-id="06-wheel-pillars"] .t0 { transform: translate(-50%, -50%) rotate(0deg)   translateY(-240px); }
      [data-composition-id="06-wheel-pillars"] .t1 { transform: translate(-50%, -50%) rotate(45deg)  translateY(-240px); }
      [data-composition-id="06-wheel-pillars"] .t2 { transform: translate(-50%, -50%) rotate(90deg)  translateY(-240px); }
      [data-composition-id="06-wheel-pillars"] .t3 { transform: translate(-50%, -50%) rotate(135deg) translateY(-240px); }
      [data-composition-id="06-wheel-pillars"] .t4 { transform: translate(-50%, -50%) rotate(180deg) translateY(-240px); }
      [data-composition-id="06-wheel-pillars"] .t5 { transform: translate(-50%, -50%) rotate(225deg) translateY(-240px); }
      [data-composition-id="06-wheel-pillars"] .t6 { transform: translate(-50%, -50%) rotate(270deg) translateY(-240px); }
      [data-composition-id="06-wheel-pillars"] .t7 { transform: translate(-50%, -50%) rotate(315deg) translateY(-240px); }

      [data-composition-id="06-wheel-pillars"] .panel {
        position: absolute;
        top: 50%;
        width: 380px;
        padding: 24px 28px;
        transform: translateY(-50%);
        background: linear-gradient(135deg, rgba(255,255,255,0.08), rgba(255,255,255,0.02));
        border: 1px solid rgba(255,255,255,0.12);
        border-radius: 16px;
        backdrop-filter: blur(14px);
        opacity: 0;
      }
      [data-composition-id="06-wheel-pillars"] .p-left { left: 120px; }
      [data-composition-id="06-wheel-pillars"] .p-right { right: 120px; }
      [data-composition-id="06-wheel-pillars"] .p-third { top: auto; bottom: 120px; transform: none; }
      [data-composition-id="06-wheel-pillars"] .pname {
        font-family: "Geist", sans-serif;
        font-weight: 700;
        font-size: 56px;
        letter-spacing: -0.02em;
        color: #fff;
        text-shadow: 0 0 30px rgba(169,176,240,0.5);
      }
      [data-composition-id="06-wheel-pillars"] .pdesc {
        margin-top: 8px;
        font-family: "JetBrains Mono", monospace;
        font-size: 18px;
        color: rgba(255,255,255,0.7);
        letter-spacing: 0.02em;
      }
      [data-composition-id="06-wheel-pillars"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.7;
      }
      [data-composition-id="06-wheel-pillars"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px;
      }
    </style>

    <script>
      (() => {
        const SLOT = 2.5;
        const tl = gsap.timeline({ paused: true });
        const sel = (q) => `[data-composition-id="06-wheel-pillars"] ${q}`;

        // Wheel fade-in + 360° rotation across whole slot
        tl.to(sel('.wheel'), { opacity: 1, duration: 0.35, ease: 'power2.out' }, 0);
        tl.fromTo(sel('.wheel'),
          { rotate: -30 },
          { rotate: 360, duration: SLOT, ease: 'none' }, 0);

        // Plan panel slides in from left at 0.4s
        gsap.set(sel('.p-left:not(.p-third)'), { x: -100, opacity: 0 });
        tl.to(sel('.p-left:not(.p-third)'),
          { x: 0, opacity: 1, duration: 0.5, ease: 'power3.out' }, 0.4);

        // Build panel slides in from right at 1.0s
        gsap.set(sel('.p-right'), { x: 100, opacity: 0 });
        tl.to(sel('.p-right'),
          { x: 0, opacity: 1, duration: 0.5, ease: 'power3.out' }, 1.0);

        // Monitor panel slides up from bottom at 1.6s
        gsap.set(sel('.p-third'), { y: 100, opacity: 0 });
        tl.to(sel('.p-third'),
          { y: 0, opacity: 1, duration: 0.5, ease: 'power3.out' }, 1.6);

        // All three exit together at 2.25s (into next comp)
        tl.to(sel('.panel'), { opacity: 0, scale: 0.9, duration: 0.25, ease: 'power2.in' }, 2.25);
        tl.to(sel('.wheel'), { opacity: 0, scale: 0.7, duration: 0.25, ease: 'power2.in' }, 2.25);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['06-wheel-pillars'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice into index.html**

```html
      <!-- Beat 06 — Chrome wheel + side-panels (19.5–22s) -->
      <div class="beat-layer"
        data-composition-id="06-wheel-pillars"
        data-composition-src="compositions/06-wheel-pillars.html"
        data-start="19.5" data-duration="2.5" data-track-index="0"
        data-width="1920" data-height="1080"></div>
```

- [ ] **Step 3: Lint + preview + sign-off**

```bash
npx hyperframes lint
```

Preview URL: `http://localhost:3002/?comp=06-wheel-pillars`. Wait for the user.

- [ ] **Step 4: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/06-wheel-pillars.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 06 — chrome wheel + pillar panels"
```

---

## Task 11: Author `07-foundation.html` — callback + floating cluster (22–25s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/07-foundation.html`
- Modify: `video-projects/linear-promo-30s/index.html`

Slot: 3s. Act 3 opener. L-bracket callback spins 22–23 (local 0–1), then three floating symbols drift in conical light beam with "For teams and agents" chrome headline 23–25 (local 1–3).

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/07-foundation.html`

```html
<template id="07-foundation-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@1,400;1,500&display=block" rel="stylesheet" />

  <div
    data-composition-id="07-foundation"
    data-start="0"
    data-duration="3"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-07">
      <div class="grid-floor"></div>

      <!-- Conical light beam -->
      <div class="light-cone"></div>

      <!-- Callback spinning symbol (0.0-1.0) -->
      <div class="callback-symbol">
        <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
          <defs>
            <linearGradient id="grad-07-0" x1="0" y1="0" x2="1" y2="1">
              <stop offset="0%" stop-color="#5E6AD2"/>
              <stop offset="100%" stop-color="#A9B0F0"/>
            </linearGradient>
          </defs>
          <path d="M 25 18 L 75 18 L 75 34 L 41 34 L 41 82 L 25 82 Z" fill="url(#grad-07-0)" />
        </svg>
      </div>

      <!-- Three floating cluster symbols (1.0-3.0) -->
      <div class="cluster">
        <div class="c-sym cs1">
          <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
            <defs><linearGradient id="grad-07-1" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#5E6AD2"/><stop offset="100%" stop-color="#A9B0F0"/></linearGradient></defs>
            <path d="M 25 18 L 75 18 L 75 34 L 41 34 L 41 82 L 25 82 Z" fill="url(#grad-07-1)" />
          </svg>
        </div>
        <div class="c-sym cs2">
          <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
            <defs><linearGradient id="grad-07-2" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#5E6AD2"/><stop offset="100%" stop-color="#A9B0F0"/></linearGradient></defs>
            <path d="M 25 18 L 75 18 L 75 34 L 41 34 L 41 82 L 25 82 Z" fill="url(#grad-07-2)" />
          </svg>
        </div>
        <div class="c-sym cs3">
          <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
            <defs><linearGradient id="grad-07-3" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="#5E6AD2"/><stop offset="100%" stop-color="#A9B0F0"/></linearGradient></defs>
            <path d="M 25 18 L 75 18 L 75 34 L 41 34 L 41 82 L 25 82 Z" fill="url(#grad-07-3)" />
          </svg>
        </div>
      </div>

      <!-- Headline -->
      <div class="foundation-head">For teams and agents</div>

      <!-- Sparkle particles -->
      <div class="sparkles">
        <span class="sp sp1">+</span>
        <span class="sp sp2">+</span>
        <span class="sp sp3">+</span>
        <span class="sp sp4">+</span>
        <span class="sp sp5">+</span>
        <span class="sp sp6">+</span>
      </div>

      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="07-foundation"] {
        position: absolute; inset: 0; overflow: hidden; background: #000;
      }
      [data-composition-id="07-foundation"] .stage-07 { position: absolute; inset: 0; }
      [data-composition-id="07-foundation"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.04) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 1px, transparent 1px 80px);
        background-color: #000;
      }
      [data-composition-id="07-foundation"] .light-cone {
        position: absolute;
        top: -5%; left: 50%;
        width: 900px; height: 900px;
        transform: translateX(-50%);
        background: conic-gradient(from 180deg at 50% 0%, transparent 0deg, rgba(169,176,240,0.2) 10deg, rgba(94,106,210,0.1) 25deg, transparent 40deg, transparent 320deg, rgba(94,106,210,0.1) 335deg, rgba(169,176,240,0.2) 350deg, transparent 360deg);
        filter: blur(40px);
        opacity: 0;
      }
      [data-composition-id="07-foundation"] .callback-symbol {
        position: absolute;
        top: 40%; left: 50%;
        width: 260px; height: 260px;
        transform: translate(-50%, -50%);
        filter: drop-shadow(0 0 60px rgba(169,176,240,0.7));
        opacity: 0;
      }
      [data-composition-id="07-foundation"] .callback-symbol svg { width: 100%; height: 100%; }
      [data-composition-id="07-foundation"] .cluster {
        position: absolute;
        top: 38%; left: 50%;
        width: 700px; height: 260px;
        transform: translate(-50%, -50%);
        opacity: 0;
      }
      [data-composition-id="07-foundation"] .c-sym {
        position: absolute;
        width: 160px; height: 160px;
        filter: drop-shadow(0 0 40px rgba(169,176,240,0.6));
      }
      [data-composition-id="07-foundation"] .c-sym svg { width: 100%; height: 100%; }
      [data-composition-id="07-foundation"] .cs1 { top: 30px; left: 60px; transform: rotate(-6deg); }
      [data-composition-id="07-foundation"] .cs2 { top: 0; left: 270px; transform: rotate(2deg); }
      [data-composition-id="07-foundation"] .cs3 { top: 50px; left: 480px; transform: rotate(-3deg); }

      [data-composition-id="07-foundation"] .foundation-head {
        position: absolute;
        top: 75%; left: 50%;
        transform: translate(-50%, -50%);
        font-family: "Cormorant Garamond", serif;
        font-style: italic;
        font-weight: 500;
        font-size: 96px;
        letter-spacing: -0.01em;
        background: linear-gradient(180deg, #ffffff 0%, #cccccc 60%, #888888 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 60px rgba(255,255,255,0.3);
        opacity: 0;
        white-space: nowrap;
      }
      [data-composition-id="07-foundation"] .sparkles {
        position: absolute; inset: 0;
        pointer-events: none;
      }
      [data-composition-id="07-foundation"] .sp {
        position: absolute;
        font: 400 18px/1 monospace;
        color: rgba(169,176,240,0.7);
        text-shadow: 0 0 12px rgba(169,176,240,0.8);
        opacity: 0;
      }
      [data-composition-id="07-foundation"] .sp1 { top: 25%; left: 30%; }
      [data-composition-id="07-foundation"] .sp2 { top: 32%; left: 70%; }
      [data-composition-id="07-foundation"] .sp3 { top: 55%; left: 25%; }
      [data-composition-id="07-foundation"] .sp4 { top: 48%; left: 75%; }
      [data-composition-id="07-foundation"] .sp5 { top: 60%; left: 50%; }
      [data-composition-id="07-foundation"] .sp6 { top: 38%; left: 52%; }

      [data-composition-id="07-foundation"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.7;
      }
      [data-composition-id="07-foundation"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px;
      }
    </style>

    <script>
      (() => {
        const SLOT = 3;
        const tl = gsap.timeline({ paused: true });
        const sel = (q) => `[data-composition-id="07-foundation"] ${q}`;

        // Callback spin 0.0-1.0
        tl.fromTo(sel('.callback-symbol'),
          { opacity: 0, scale: 0.4, rotateY: 0 },
          { opacity: 1, scale: 1, rotateY: 540, duration: 1.0, ease: 'power2.out' }, 0);
        tl.to(sel('.callback-symbol'),
          { opacity: 0, scale: 0.6, duration: 0.3, ease: 'power2.in' }, 0.9);

        // Light cone expands 0.7-1.5
        tl.to(sel('.light-cone'),
          { opacity: 1, duration: 0.5, ease: 'power2.out' }, 0.7);
        tl.to(sel('.light-cone'),
          { scale: 1.05, duration: 2, yoyo: true, repeat: 1, ease: 'sine.inOut' }, 1.0);

        // Cluster fades in 1.0-1.5, three symbols drift continuously
        tl.to(sel('.cluster'),
          { opacity: 1, duration: 0.5, ease: 'power2.out' }, 1.0);

        // Continuous drift per symbol (different phase)
        tl.to(sel('.cs1'), { y: -12, duration: 1.5, yoyo: true, repeat: -1, ease: 'sine.inOut' }, 1.0);
        tl.to(sel('.cs2'), { y: 10,  duration: 1.8, yoyo: true, repeat: -1, ease: 'sine.inOut' }, 1.1);
        tl.to(sel('.cs3'), { y: -8,  duration: 1.6, yoyo: true, repeat: -1, ease: 'sine.inOut' }, 1.2);

        // Headline fades up 1.3
        tl.fromTo(sel('.foundation-head'),
          { opacity: 0, y: 30 },
          { opacity: 1, y: 0, duration: 0.7, ease: 'power3.out' }, 1.3);

        // Sparkles twinkle in stagger
        tl.to(sel('.sp'),
          { opacity: 0.9, duration: 0.4, stagger: { each: 0.1, from: 'random' }, ease: 'sine.out' }, 1.1);
        tl.to(sel('.sp'),
          { opacity: 0.3, duration: 1.5, yoyo: true, repeat: -1, ease: 'sine.inOut' }, 1.5);

        // Exit: everything fades slightly into CTA
        tl.to(sel('.cluster, .foundation-head, .light-cone'),
          { opacity: 0, duration: 0.4, ease: 'power2.in' }, 2.7);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['07-foundation'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice into index.html**

```html
      <!-- Beat 07 — Foundation callback + cluster (22–25s) -->
      <div class="beat-layer"
        data-composition-id="07-foundation"
        data-composition-src="compositions/07-foundation.html"
        data-start="22" data-duration="3" data-track-index="0"
        data-width="1920" data-height="1080"></div>
```

- [ ] **Step 3: Lint + preview + sign-off**

```bash
npx hyperframes lint
```

`http://localhost:3002/?comp=07-foundation`. Wait for the user.

- [ ] **Step 4: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/07-foundation.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 07 — foundation callback + floating cluster"
```

---

## Task 12: Author `08-cta-outro.html` — 5s hero hold (25–30s)

**Files:**
- Create: `video-projects/linear-promo-30s/compositions/08-cta-outro.html`
- Modify: `video-projects/linear-promo-30s/index.html`

Slot: 5s. The longest single beat — Law #9. Wordmark + tagline + CTA button. Shimmer sweep every 1.8s across wordmark.

- [ ] **Step 1: Author the composition**

Path: `video-projects/linear-promo-30s/compositions/08-cta-outro.html`

```html
<template id="08-cta-outro-template">
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@500;600;700&family=Instrument+Serif:ital@0;1&family=Geist:wght@500;700&display=block" rel="stylesheet" />

  <div
    data-composition-id="08-cta-outro"
    data-start="0"
    data-duration="5"
    data-width="1920"
    data-height="1080"
  >
    <div class="stage-08">
      <div class="grid-floor"></div>

      <div class="outro-lockup">
        <div class="lockup-row">
          <div class="o-symbol">
            <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
              <defs>
                <linearGradient id="grad-08" x1="0" y1="0" x2="1" y2="1">
                  <stop offset="0%" stop-color="#5E6AD2"/>
                  <stop offset="100%" stop-color="#A9B0F0"/>
                </linearGradient>
              </defs>
              <path d="M 25 18 L 75 18 L 75 34 L 41 34 L 41 82 L 25 82 Z" fill="url(#grad-08)" />
            </svg>
          </div>
          <div class="o-wordmark">Linear</div>
          <div class="shimmer"></div>
        </div>

        <div class="o-tagline">The product development system for teams and agents.</div>

        <div class="o-cta">Get started &nbsp;—&nbsp; linear.app</div>
      </div>

      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <style>
      [data-composition-id="08-cta-outro"] {
        position: absolute; inset: 0; overflow: hidden; background: #000;
      }
      [data-composition-id="08-cta-outro"] .stage-08 { position: absolute; inset: 0; }
      [data-composition-id="08-cta-outro"] .grid-floor {
        position: absolute; inset: 0;
        transform: perspective(900px) rotateX(60deg) translateY(25%);
        background:
          repeating-linear-gradient(0deg,  rgba(255,255,255,0.04) 0 1px, transparent 1px 80px),
          repeating-linear-gradient(90deg, rgba(255,255,255,0.04) 0 1px, transparent 1px 80px);
        background-color: #000;
      }
      [data-composition-id="08-cta-outro"] .outro-lockup {
        position: absolute;
        top: 50%; left: 50%;
        transform: translate(-50%, -50%);
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 32px;
        text-align: center;
      }
      [data-composition-id="08-cta-outro"] .lockup-row {
        position: relative;
        display: flex;
        align-items: center;
        gap: 24px;
      }
      [data-composition-id="08-cta-outro"] .o-symbol {
        width: 100px; height: 100px;
        filter: drop-shadow(0 0 40px rgba(169,176,240,0.6));
        opacity: 0;
      }
      [data-composition-id="08-cta-outro"] .o-symbol svg { width: 100%; height: 100%; }
      [data-composition-id="08-cta-outro"] .o-wordmark {
        font-family: "Inter", sans-serif;
        font-weight: 600;
        font-size: 144px;
        letter-spacing: -0.04em;
        background: linear-gradient(180deg, #ffffff 0%, #cccccc 60%, #888888 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        color: transparent;
        text-shadow: 0 0 60px rgba(255,255,255,0.3);
        opacity: 0;
      }
      [data-composition-id="08-cta-outro"] .shimmer {
        position: absolute;
        top: 0; left: 0;
        width: 200px; height: 100%;
        background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.55) 50%, transparent 100%);
        filter: blur(4px);
        transform: translateX(-200%);
        opacity: 0;
        mix-blend-mode: screen;
        pointer-events: none;
      }
      [data-composition-id="08-cta-outro"] .o-tagline {
        font-family: "Instrument Serif", serif;
        font-style: italic;
        font-size: 44px;
        color: rgba(255,255,255,0.85);
        letter-spacing: 0.005em;
        max-width: 1200px;
        line-height: 1.25;
        opacity: 0;
      }
      [data-composition-id="08-cta-outro"] .o-cta {
        margin-top: 12px;
        padding: 18px 40px;
        border-radius: 999px;
        font-family: "Geist", sans-serif;
        font-weight: 500;
        font-size: 28px;
        letter-spacing: 0.02em;
        color: #fff;
        background: linear-gradient(135deg, rgba(94,106,210,0.3), rgba(169,176,240,0.2));
        border: 1px solid rgba(169,176,240,0.5);
        box-shadow: 0 0 40px rgba(94,106,210,0.3), inset 0 1px 0 rgba(255,255,255,0.25);
        backdrop-filter: blur(12px);
        opacity: 0;
      }
      [data-composition-id="08-cta-outro"] .vignette {
        position: absolute; inset: 0; pointer-events: none;
        background: radial-gradient(ellipse at center, transparent 30%, rgba(0,0,0,0.95) 95%);
        opacity: 0.75;
      }
      [data-composition-id="08-cta-outro"] .grain {
        position: absolute; inset: 0; pointer-events: none;
        opacity: 0.08;
        background-image:
          radial-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
          radial-gradient(rgba(255,255,255,0.02) 1px, transparent 1px);
        background-size: 3px 3px, 5px 5px;
      }
    </style>

    <script>
      (() => {
        const SLOT = 5;
        const tl = gsap.timeline({ paused: true });
        const sel = (q) => `[data-composition-id="08-cta-outro"] ${q}`;

        // Lockup row fades in
        tl.to(sel('.o-symbol'), { opacity: 1, duration: 0.5, ease: 'power2.out' }, 0);
        tl.to(sel('.o-wordmark'), { opacity: 1, duration: 0.5, ease: 'power2.out' }, 0.1);

        // Tagline fades up
        tl.fromTo(sel('.o-tagline'),
          { opacity: 0, y: 20 },
          { opacity: 1, y: 0, duration: 0.6, ease: 'power3.out' }, 0.5);

        // CTA button fades up with bounce
        tl.fromTo(sel('.o-cta'),
          { opacity: 0, scale: 0.9 },
          { opacity: 1, scale: 1, duration: 0.5, ease: 'back.out(1.3)' }, 0.9);

        // Shimmer sweep — fire at 1.2, 3.0, 4.5 (every ~1.5-1.8s)
        const shimmer = (t) => {
          tl.set(sel('.shimmer'), { opacity: 1, xPercent: -120 }, t);
          tl.to(sel('.shimmer'), { xPercent: 400, duration: 0.7, ease: 'power2.inOut' }, t);
          tl.to(sel('.shimmer'), { opacity: 0, duration: 0.1 }, t + 0.65);
        };
        shimmer(1.2);
        shimmer(3.0);
        shimmer(4.5);

        // Gentle vignette breath across the hold
        tl.to(sel('.vignette'), { opacity: 0.85, duration: 2.5, yoyo: true, repeat: 1, ease: 'sine.inOut' }, 0);

        // Law #11 anchor
        tl.to({}, { duration: SLOT }, 0);
        window.__timelines = window.__timelines || {};
        window.__timelines['08-cta-outro'] = tl;
      })();
    </script>
  </div>
</template>
```

- [ ] **Step 2: Splice into index.html**

```html
      <!-- Beat 08 — CTA outro (25–30s) -->
      <div class="beat-layer"
        data-composition-id="08-cta-outro"
        data-composition-src="compositions/08-cta-outro.html"
        data-start="25" data-duration="5" data-track-index="0"
        data-width="1920" data-height="1080"></div>
```

- [ ] **Step 3: Lint + preview + sign-off**

```bash
npx hyperframes lint
```

`http://localhost:3002/?comp=08-cta-outro`. Wait for the user.

- [ ] **Step 4: Commit**

```bash
git add video-projects/linear-promo-30s/compositions/08-cta-outro.html \
        video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): beat 08 — CTA outro with 5s hold + shimmer"
```

---

## Task 13: Wire audio — warm pad + 3 SFX as sibling `<audio>` elements

**Files:**
- Modify: `video-projects/linear-promo-30s/index.html`

- [ ] **Step 1: Add audio mix to index.html**

In `video-projects/linear-promo-30s/index.html`, replace the `<!-- Audio mix (filled in during Task 11) -->` comment with:

```html
      <!-- Audio: warm ambient pad (full piece) -->
      <audio
        data-start="0" data-duration="30" data-track-index="1" data-volume="0.15"
        src="assets/warm-pad.mp3"></audio>

      <!-- SFX: whoosh at 3.8s (whip transition in beat 01) -->
      <audio
        data-start="3.8" data-duration="0.4" data-track-index="2" data-volume="0.20"
        src="assets/sfx-whoosh-1.mp3"></audio>

      <!-- SFX: whoosh at 6.0s (morph transition in beat 02) -->
      <audio
        data-start="6.0" data-duration="0.5" data-track-index="3" data-volume="0.20"
        src="assets/sfx-whoosh-2.mp3"></audio>

      <!-- SFX: twinkle at 8.3s (brand reveal in beat 03) -->
      <audio
        data-start="8.3" data-duration="0.5" data-track-index="4" data-volume="0.20"
        src="assets/sfx-twinkle.mp3"></audio>
```

The `<audio>` elements sit INSIDE the root `<div id="root">...</div>` but OUTSIDE any beat-layer — they are siblings at the root level, following the pattern from `hyperframes-sizzle/index.html`.

- [ ] **Step 2: Lint**

```bash
npx hyperframes lint
```

Expected: clean. Warnings about `data-duration` on SFX tails bleeding past adjacent beats are acceptable — that's the design.

- [ ] **Step 3: Commit**

```bash
git add video-projects/linear-promo-30s/index.html
git commit -m "feat(linear): wire warm pad + whip/twinkle SFX audio mix"
```

---

## Task 14: Law #11 timeline-duration diagnostic across all 8 compositions

**Files:**
- None — diagnostic check only

- [ ] **Step 1: Start the preview server**

```bash
cd "video-projects/linear-promo-30s"
npx hyperframes preview
```

Expected: Studio runs on http://localhost:3002. Leave it running.

- [ ] **Step 2: Open the master comp preview in a real browser**

Open `http://localhost:3002/` (root preview — the whole 30s piece) in a browser window where devtools can be opened.

- [ ] **Step 3: Run the diagnostic in devtools console**

Paste into the devtools console of that browser window:

```js
const p = document.querySelector('hyperframes-player');
const iw = p.shadowRoot.querySelector('iframe').contentWindow;
console.table(Object.fromEntries(Object.entries(iw.__timelines).map(([k, v]) =>
  [k, +v.duration().toFixed(4)])));
```

Expected output: a table showing each `data-composition-id` and its timeline `duration()`. Every value must be `>=` the corresponding `data-duration`:

| composition-id | expected duration | must be ≥ |
|---|---|---|
| 01-problem-type | 4 | 4 |
| 02-card-to-logo | 4 | 4 |
| 03-brand-reveal | 2 | 2 |
| 04-benefits-flowchart | 4.8 | 4.8 |
| 05-product-surfaces | 4.7 | 4.7 |
| 06-wheel-pillars | 2.5 | 2.5 |
| 07-foundation | 3 | 3 |
| 08-cta-outro | 5 | 5 |

- [ ] **Step 4: Fix any shortfalls**

If any composition's timeline is shorter than its slot, edit that composition's script section and increase the Law #11 anchor duration. For example, if `03-brand-reveal` reads `1.95` instead of `2`, the issue is that a tween chains to `2.0` but the anchor at `0` is being overridden; adjust to:

```js
tl.to({}, { duration: SLOT + 0.05 }, 0);  // small buffer
```

Keep the buffer ≤ 0.1s — anything more is a symptom of a different design issue.

- [ ] **Step 5: Re-run diagnostic — confirm all clean, commit any fixes**

Re-paste the console snippet. When every timeline reads ≥ its slot:

```bash
# Commit only if you edited anything
git add video-projects/linear-promo-30s/compositions/
git commit -m "fix(linear): Law #11 timeline padding diagnostic clean"
```

If no changes were needed, skip the commit.

---

## Task 15: Render a draft + visual verification (MANDATORY before final render)

**Files:**
- Create: `video-projects/linear-promo-30s/renders/linear-promo-draft.mp4`
- Create: `video-projects/linear-promo-30s/renders/frames/*.png`

This is the mandatory visual-verification gate. Do NOT skip to final render.

- [ ] **Step 1: Render the draft**

From inside `video-projects/linear-promo-30s/`:
```bash
npx hyperframes render --quality draft --output renders/linear-promo-draft.mp4
```

Expected: MP4 appears at `renders/linear-promo-draft.mp4`, ~30s duration, ~5-20MB (draft is CRF 28).

- [ ] **Step 2: Extract verification frames — one per beat + transition mid-points**

Run from inside `video-projects/linear-promo-30s/`:
```bash
mkdir -p renders/frames

# Beat centers (visual confirmation of each scene's hero moment)
for t in 0.5 1.5 3.0 3.8 5.0 6.5 7.5 8.5 9.5 10.5 12.5 13.8 14.5 15.5 17.0 18.5 20.0 21.0 22.5 23.5 27.0 29.0; do
  ffmpeg -y -ss $t -i renders/linear-promo-draft.mp4 -frames:v 1 -q:v 2 "renders/frames/t${t}.png"
done

# Transition seams (catches black-frame flashes)
for t in 3.95 7.95 9.95 14.75 19.45 21.95 24.95; do
  ffmpeg -y -ss $t -i renders/linear-promo-draft.mp4 -frames:v 1 -q:v 2 "renders/frames/seam-${t}.png"
done
```

- [ ] **Step 3: Open each frame with the Read tool**

For EACH PNG in `renders/frames/`, call `Read` on its absolute path. This loads the image into the model context — filename alone is not enough. The visual-verification memory (`feedback_visual_verification.md`) mandates actually viewing every frame.

Read each frame and verify:

**Per-beat hero-frame checks:**
- `t0.5.png` (Beat 01 early) — grid visible, black canvas, no stray content
- `t1.5.png` (Beat 01 mid) — "Product" or "management" visible with chrome gradient
- `t3.0.png` (Beat 01 dolly) — "broken" scaling, visible grid tilt
- `t3.8.png` (Beat 01 whip) — white streak visible across frame
- `t5.0.png` (Beat 02 red card) — red card visible, no hard edges
- `t6.5.png` (Beat 02 morph) — symbol transition — should NOT be an empty black frame
- `t7.5.png` (Beat 02 symbol rise) — L-bracket visible on light pillar
- `t8.5.png` (Beat 03 wordmark) — "Linear" wordmark + symbol lockup
- `t9.5.png` (Beat 03 hold) — same lockup, shimmer may be mid-sweep
- `t10.5.png` (Beat 04 Fast) — "Fast" visible, magenta comet trail present
- `t12.5.png` (Beat 04 Focused) — flowchart in blue
- `t13.8.png` (Beat 04 AI-native) — flowchart in neon yellow/orange
- `t14.5.png` (Beat 04 exit) — AI-native halo visible
- `t15.5.png` (Beat 05 Plan) — phone mockup + screenshot-plan.png visible, "Plan" headline
- `t17.0.png` (Beat 05 Build) — laptop mockup + screenshot-build.png, micro-caption visible
- `t18.5.png` (Beat 05 Monitor) — second phone + screenshot-monitor.png
- `t20.0.png` (Beat 06 wheel start) — chrome wheel + Plan panel visible
- `t21.0.png` (Beat 06 wheel mid) — all three panels visible, wheel mid-rotation
- `t22.5.png` (Beat 07 callback) — single L-bracket spinning
- `t23.5.png` (Beat 07 cluster) — three L-brackets + "For teams and agents" headline
- `t27.0.png` (Beat 08 CTA) — wordmark + tagline + button all visible
- `t29.0.png` (Beat 08 late hold) — same lockup, no black frame

**Per-seam checks** — confirm continuity at every beat boundary:
- `seam-3.95.png` — Beat 01 → 02 handoff (whip peak, should be bright not black)
- `seam-7.95.png` — Beat 02 → 03 handoff (should show symbol settling / wordmark entering)
- `seam-9.95.png` — Beat 03 → 04 handoff (shouldn't be fully black)
- `seam-14.75.png` — Beat 04 → 05 handoff
- `seam-19.45.png` — Beat 05 → 06 handoff
- `seam-21.95.png` — Beat 06 → 07 handoff
- `seam-24.95.png` — Beat 07 → 08 handoff

**Red flags that require a fix:**
- Any seam frame is pure black → Law #11 violation, re-run Task 14 diagnostic
- Text overflow past frame edges
- Screenshots missing / broken PNG icon inside device bezel
- Symbol cropped by bezel
- Grid/vignette/grain missing from any scene → scene isn't on-brand

- [ ] **Step 4: Fix any issues found, re-draft, re-verify**

For each failure, open the responsible composition, fix the issue (timing tweak, CSS adjustment, asset path fix), re-lint, and loop back to Step 1. Do NOT proceed to Task 16 until all frames pass.

- [ ] **Step 5: Commit the draft + frames as a verification snapshot**

```bash
# renders/ is typically gitignored; if so, skip the renders. Commit frame checks only if they're small enough.
# If the project's .gitignore excludes renders/, this step is a no-op.
git status
```

If anything staged:
```bash
git add video-projects/linear-promo-30s/
git commit -m "verify(linear): draft render + frame-by-frame visual pass"
```

---

## Task 16: Rendered-MP4 preview gate (gate 2)

**Files:**
- None — user review gate

- [ ] **Step 1: Serve the renders directory on port 8080**

From inside `video-projects/linear-promo-30s/renders/`:
```bash
cd renders
npx serve . -p 8080 -n
```

Leave it running in the background.

- [ ] **Step 2: Hand the user the URL + wait for sign-off**

Give the user: `http://localhost:8080/linear-promo-draft.mp4`

Tell him explicitly: "Scrub the full MP4. Confirm pacing, audio sync, all three signature moves (color recolor 13.8s, chrome wheel 20–21s, 5s outro hold 25–30s) read correctly. Reply 'ship it' when ready for the final render."

WAIT for his explicit sign-off. If he asks for changes, go back to the specific task's composition file, fix, re-lint, re-draft, loop through visual verification again, and re-present the new draft.

---

## Task 17: Final render + delivery

**Files:**
- Create: `video-projects/linear-promo-30s/renders/linear-promo-final.mp4`

- [ ] **Step 1: Clear any stale cache**

```bash
rm -rf node_modules/.cache
```

- [ ] **Step 2: Render at standard quality**

From inside `video-projects/linear-promo-30s/`:
```bash
npx hyperframes render --quality standard --output renders/linear-promo-final.mp4
```

Expected: MP4 at `renders/linear-promo-final.mp4`, ~30s, CRF 18, visually lossless at 1080p. Render time typically 2–5 minutes.

- [ ] **Step 3: Verify the final MP4**

```bash
ffprobe -v error -show_entries format=duration -of default=nw=1:nk=1 renders/linear-promo-final.mp4
ls -lh renders/linear-promo-final.mp4
```

Expected: duration within 30.00 ± 0.05s. File size 30–100MB.

- [ ] **Step 4: Spot-check the final render**

Extract 3 frames from the final to verify it's not corrupted:
```bash
ffmpeg -y -ss 0.5 -i renders/linear-promo-final.mp4 -frames:v 1 -q:v 2 renders/frames/final-t0.5.png
ffmpeg -y -ss 15.0 -i renders/linear-promo-final.mp4 -frames:v 1 -q:v 2 renders/frames/final-t15.png
ffmpeg -y -ss 27.0 -i renders/linear-promo-final.mp4 -frames:v 1 -q:v 2 renders/frames/final-t27.png
```

`Read` each of the three final-*.png frames. Confirm they match the draft at those timestamps (same visual content, just cleaner encoding).

- [ ] **Step 5: Report to the user**

Send a message summarizing:
- Final path: `video-projects/linear-promo-30s/renders/linear-promo-final.mp4`
- Duration: `<exact duration>s`
- File size: `<size>`
- Any known caveats (if Linear screenshots were mocked vs. real, if any beat was trimmed)

- [ ] **Step 6: Commit (if renders/ is not gitignored)**

```bash
git status
```

If the final MP4 is in the gitignored list (check `.gitignore`), skip this commit — that's expected. Otherwise:
```bash
git add video-projects/linear-promo-30s/renders/linear-promo-final.mp4
git commit -m "render(linear): final 1080p standard-quality MP4"
```

---

## Self-Review Summary (completed before handing off)

**Spec coverage check:** Every spec section has a task:
- Folder structure → Task 1
- Logo/symbol SVG → Task 2
- Linear screenshots → Task 3
- Audio (warm pad + SFX) → Task 4
- Composition 01 → Task 5
- Composition 02 → Task 6
- Composition 03 → Task 7
- Composition 04 (color recolor signature) → Task 8
- Composition 05 → Task 9
- Composition 06 (wheel signature) → Task 10
- Composition 07 → Task 11
- Composition 08 (5s hold) → Task 12
- Audio wiring → Task 13
- Law #11 diagnostic → Task 14
- Draft render + frame verification → Task 15
- User preview gate → Task 16
- Final render → Task 17

**Placeholder scan:** No TBD/TODO entries. Every composition includes full HTML/CSS/JS. Every ffmpeg command is complete. Every path is absolute-relative to the project folder.

**Type consistency:** All 8 composition IDs match across:
- their `data-composition-id` attribute
- their `window.__timelines[...]` key
- the `data-composition-id` on the splice in index.html
- the filename

All eight IDs: `01-problem-type`, `02-card-to-logo`, `03-brand-reveal`, `04-benefits-flowchart`, `05-product-surfaces`, `06-wheel-pillars`, `07-foundation`, `08-cta-outro`.

All eight slot durations sum to exactly 30.0s (4 + 4 + 2 + 4.8 + 4.7 + 2.5 + 3 + 5).

All 11 Laws from MOTION_PHILOSOPHY.md are applied:
- Law 1 (one idea per beat, ~1.5s avg): 18 beats in 30s = 1.67s avg ✓
- Law 2 (black canvas): every comp has `background: #000` ✓
- Law 3 (light not color): chrome gradients + halo glows on every headline ✓
- Law 4 (camera never sleeps): grid parallax, vignette breath, cluster drift ✓
- Law 5 (motion blur): whip streaks at 3.8s, 6.0s, morph handoffs ✓
- Law 6 (object metaphors + callbacks): red card = broken / L-bracket = solution; L-bracket appears in beats 02, 03, 07, 08 = 4 appearances ✓
- Law 7 (palette symbolic): black/chrome/#5E6AD2/#A155FF/#FFD84A = 5 colors, each with meaning ✓
- Law 8 (type is a character): "broken" scales 5×, chrome gradient on every headline ✓
- Law 9 (hold the hero): 5s outro + 2s brand reveal ✓
- Law 10 (unifying texture): grid + crosshairs + vignette + grain on all 8 comps ✓
- Law 11 (timelines fill slots): explicit `tl.to({}, { duration: SLOT }, 0)` on every comp + diagnostic in Task 14 ✓

---

## Execution Handoff

**Plan complete and saved to `docs/superpowers/plans/2026-04-17-linear-promo-implementation.md`. Two execution options:**

**1. Subagent-Driven (recommended)** — A fresh subagent per task, review between tasks, fast iteration. Best for this plan: each composition is self-contained, subagents can parallelize asset acquisition (Tasks 2, 3, 4), and the mandatory preview + verification gates give natural human-in-the-loop checkpoints.

**2. Inline Execution** — Execute tasks in this session using executing-plans, batch execution with checkpoints for review.

**Which approach?**
