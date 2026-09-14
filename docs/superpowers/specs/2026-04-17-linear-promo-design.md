# Spec — Linear.app 30s Promo (Infinite-style)

**Date:** 2026-04-17
**Author:** User + Claude
**Project slug:** `linear-promo-30s`

## Summary

A 30-second, 1920×1080 @ 30fps showpiece promo for Linear (linear.app), authored in HyperFrames and styled as a near-twin of the Infinite Global Payments reference spot. The piece is a fan/spec promo — unsolicited — whose purpose is to demonstrate what Claude + HyperFrames + `MOTION_PHILOSOPHY.md` can produce end-to-end for a real business use case, ready to show the user's audience.

## Context

The user wants a demo that proves the motion-philosophy workflow against a real business. Linear was chosen because:
- Dark-native brand aesthetic doesn't fight the Infinite dark-chrome look
- Three clean pillars (Plan / Build / Monitor — pulled live from linear.app 2026-04-17) rhyme with Infinite's Dash / API / SDK structure
- Linear's current hero — *"The product development system for teams and agents"* — maps directly onto the user's AI-automation audience
- Beautiful product = the promo has to earn its keep visually (forces discipline)

Goal: a 30s render that a viewer couldn't tell wasn't commissioned by Linear.

## Success Criteria

1. Final render at 1920×1080 @ 30fps, ~30s (±0.5s), delivered as `renders/linear-promo-final.mp4` via `npx hyperframes render --quality standard`.
2. All 11 Laws from `MOTION_PHILOSOPHY.md` pass the pre-flight checklist.
3. Every timeline ends with `tl.to({}, { duration: SLOT_DURATION }, 0)` — zero black-frame flashes.
4. Every scene cut hides inside a motion-blur whip, morph, or recolor — no hard fades.
5. Visual verification pass: 30+ frames extracted, each opened with `Read`, confirmed on-brand.

## Tech / Platform

- HyperFrames (latest), authored as a new project under `video-projects/linear-promo-30s/`
- GSAP (bundled), Google Fonts CDN (per-beat font discipline)
- No Three.js, no Lottie, no WebGL (except possibly one fallback-gated FBM beat)
- Captions: `<div class="cap">` body-level siblings per §3.13 of philosophy
- Audio: warm ambient pad at `data-volume="0.15"` + 2–3 SFX at `0.2`, no VO, no music track
- Zero registry blocks — hand-built per §6 anti-pattern ("reference-quality work is hand-built")

## Folder Structure

```
video-projects/linear-promo-30s/
├── index.html                          ← root composition + captions
├── compositions/
│   ├── 01-problem-type.html            ← "Project management is..." kinetic type
│   ├── 02-card-to-logo.html            ← red broken card → Linear teal logo morph
│   ├── 03-brand-reveal.html            ← Linear wordmark hold
│   ├── 04-benefits-flowchart.html      ← Fast / Focused / AI-native (color recolor)
│   ├── 05-product-surfaces.html        ← Plan / Build / Monitor phone+laptop mockups
│   ├── 06-wheel-pillars.html           ← rotating chrome wheel with three pillars
│   ├── 07-foundation.html              ← "For teams and agents" floating cluster
│   └── 08-cta-outro.html               ← "Get started — linear.app" 5s hold
├── assets/
│   ├── linear-logo.svg                 ← Linear wordmark (trace from real mark)
│   ├── linear-symbol.svg               ← Linear's L-bracket symbol
│   ├── screenshot-plan.png             ← actual Linear planning UI screenshot
│   ├── screenshot-build.png            ← actual Linear issue/build UI screenshot
│   ├── screenshot-monitor.png          ← actual Linear analytics screenshot
│   ├── phone-bezel.png                 ← generic iPhone frame
│   ├── laptop-bezel.png                ← generic MacBook frame
│   ├── warm-pad.mp3                    ← 30s ambient pad (sourced or generated via CLI tts)
│   ├── sfx-click.mp3                   ← twinkle/click for brand reveal
│   ├── sfx-whoosh-1.mp3                ← whip transition sound
│   └── sfx-whoosh-2.mp3                ← second whip transition sound
├── hyperframes.json
├── meta.json
└── renders/                             ← gitignored
```

## Narrative Timeline (beat-by-beat)

Three-act structure, rule of threes at every layer. Cuts hide in whips; beats follow §4.1 GSAP code dictionary values.

| Time | Beat | On screen | Font | Key techniques |
|------|------|-----------|------|----------------|
| **Act 1 — Problem → Brand Reveal (0–9s)** |
| 0.0–1.0 | Empty stage | Black canvas, perspective grid recedes, 4 `+` crosshair marks, vignette breath begins | Instrument Serif (muted) | Grid background §3.2, vignette breath §2.4 |
| 1.0–2.5 | "Product management" | Chrome-gradient type word reveal, slide-in from x:360 → 120 → 60 → 25 → 12 decay | Space Grotesk | Word-reveal carrier §2.2, chrome sweep §2.2 |
| 2.5–4.0 | **"is broken"** | Hero kinetic moment — type scales 1× → 8× camera dolly-through, grid tilts sideways | Space Grotesk 480px | §2.4 camera dolly through type |
| 4.0–4.4 | **Whip transition** | Glowing white light-streak L→R + whoosh SFX at 4.2 peak | — | §2.5 light-streak whip, SFX at volume 0.2 |
| 4.4–6.0 | Red card lands | 3D credit-card metaphor for "bloated Jira/Asana" — red gradient, motion blur on entry, slight orbit | — | Object metaphor §0 Law 6 |
| 6.0–7.0 | **Morph** | Red card warps into Linear's teal-purple L-bracket symbol mid-flight, cross-warp light streak | — | §2.4 object morph drift |
| 7.0–8.0 | Symbol rises | Linear L-bracket rotates to face camera on vertical light pillar, subtle iridescent shimmer | — | §2.4 coin-spin reveal pattern |
| 8.0–9.0 | Brand reveal | Symbol shrinks + locks into "Linear" wordmark position, wordmark chrome-gradient fade-up | Linear's brand font (Inter Display weight 600 trace) | §2.4 crystallize-to-wordmark, SFX twinkle at 8.3 |
| **Act 2 — Benefits → Surfaces → Pillars (9–22s)** |
| 9.0–10.0 | Logo hold | Pure black canvas, shimmer sweep across wordmark | — | §0 Law 9: hold the hero |
| 10.0–11.2 | **"Fast"** | Magenta-purple lightning comet rips L→R; "Fast" appears upper-right chrome-gradient with halo | Azeret Mono | §2.4 light-streak, chrome-gradient halo §2.2 |
| 11.2–12.3 | Flowchart appears | White-outline node network builds stroke-dash-draw, purple energy pulses along edges | — | §2.4 energy-pulse along path, SVG dash-draw §from repo crawl |
| 12.3–13.3 | "Focused" | Same flowchart, blue energy fills all nodes, "Focused" word lights up | Azeret Mono | Reuse scene (no cut) |
| 13.3–14.8 | **Recolor → "AI-native"** | Same DOM flowchart, CSS variables tween to neon orange/yellow, dashed lines, halo on "AI-native" | Azeret Mono | §3.5 color recolor trick — **one of the signature moves** |
| 14.8–16.5 | "Plan" surface | iPhone mockup slides up from bottom, Linear Plan screenshot inside, headline "Plan" above | Instrument Serif (italic) | §2.4 slide-up phone, §3.12 poster-bracket on screenshot |
| 16.5–18.0 | "Build" surface | Laptop mockup cross-warps in, Linear Build/Issues screenshot, "Build" headline — *this is where the AI-agent messaging lands: "Build and deploy AI agents that work alongside your team"* as micro-caption | Instrument Serif (italic) | §2.4 cross-warp, caption §3.13 |
| 18.0–19.5 | "Monitor" surface | Second phone mockup slides up from opposite side, analytics screenshot, "Monitor" headline | Instrument Serif (italic) | §2.4 slide-up |
| 19.5–22.0 | **Chrome wheel — Plan / Build / Monitor** | Iridescent chrome wheel centered, three panels slide in from L/R/L with each pillar name and one-line description. Wheel rotates 360° across the beat. | Geist + JetBrains Mono | §2.4 wheel + side-panel pattern — the *second* signature move |
| **Act 3 — Foundation → CTA (22–30s)** |
| 22.0–23.0 | Callback | Linear L-bracket symbol returns spinning in cylinder, callback to Act 1 brand reveal | — | §0 Law 6: callback |
| 23.0–25.0 | Foundation | Three floating Linear symbols in conical light beam with sparkle particles, headline "For teams and agents" chrome-gradient | Cormorant Garamond italic | §2.4 floating cluster drift, §2.1 conical light beam |
| 25.0–30.0 | **CTA hold** | Linear wordmark + "The product development system for teams and agents." + button-style "Get started — linear.app" | Instrument Serif + Geist | §0 Law 9: 5s hold. Shimmer sweep every 1.8s over logo. |

**Three-acts check:** 9s + 13s + 8s = 30s. ✓
**Rule of threes:** 3 benefits (Fast/Focused/AI-native), 3 surfaces (Plan/Build/Monitor phone+laptop+phone), 3 pillar names (Plan/Build/Monitor), 3 floating symbols, 3 acts. ✓
**Callbacks:** Linear L-bracket appears at 7s (reveal), 22s (spin callback), 23s (3-symbol cluster), 25s (wordmark outro). 4 reappearances — above the §0 Law 6 minimum. ✓
**Breathing moments:** 9.0s logo hold, 19.5s wheel centerpiece, 25.0s outro. Three breaths in 30s. ✓

## Visual Direction

- **Palette** (≤5 active hues per §4 checklist):
  - Black `#000` — canvas
  - Chrome `#fff → #999` gradient — premium voice (headlines)
  - Linear teal/purple `#5E6AD2` — brand (symbol, "Focused" benefit)
  - Magenta/purple `#A155FF` — speed (lightning comet, "Fast")
  - Neon orange/yellow `#FFD84A` → `#ff9430` — AI/value (recolored flowchart)
- **Type cast (per-beat font discipline §2.2):** Space Grotesk (opener kinetic) · Instrument Serif (surface headlines + outro) · Azeret Mono (benefit words) · Cormorant Garamond italic (foundation) · Geist + JetBrains Mono (CTA + wheel panels) · Inter Display weight 600 (Linear wordmark trace)
- **Textures on every scene:** perspective grid + crosshair markers (faint) + vignette + grain overlay (all pure-CSS, no PNG)
- **Scene average:** 1.67s (30s ÷ 18 beats)
- **No hard cuts.** Every transition uses a whip, morph, slide, recolor, or cross-warp.

## Asset Acquisition Strategy

- **Linear screenshots**: use Playwright (already installed) to capture linear.app at 1920×1080. Target the three pillar sections by scrolling the homepage and screenshotting each pillar block (Plan, Build, Monitor) at its in-view position, OR capture dedicated pages if the homepage scroll doesn't yield clean framings. Export as high-DPI PNG. Fallback: hand-craft mock Linear screens in HTML/CSS if real captures don't composite well against the Infinite aesthetic.
- **Linear logo + symbol**: trace from the live site's SVG (linear.app ships their logo inline). Copy the SVG path data directly — no PNG fallback needed.
- **Warm ambient pad**: source a 30s royalty-free ambient pad from the user's shared music stash (check root `assets/` and project-level); fall back to generating a simple drone via `npx hyperframes tts` or ffmpeg sine synthesis.
- **SFX (whooshes, click twinkle)**: source from free sound library; keep files under 100KB each.
- **Font files**: NONE. All fonts loaded per-composition from Google Fonts CDN.

## Out of Scope (deliberately)

- Vertical 9:16 cutdown — horizontal only, per clarification
- Multiple language variants
- Custom Linear-approved messaging (this is an unsolicited spec promo; we use the public hero copy verbatim)
- VO track — kinetic typography carries the narrative
- WebGL-heavy backgrounds — all shader work is either pre-rendered MP4 or CSS-only

## Risk Register

| Risk | Mitigation |
|------|-----------|
| Linear screenshot quality / freshness | Capture at spec time; if their UI changes, the screenshots date but the promo still plays |
| Fan-promo legal concern (Linear's branding) | Keep internal / demo-only; if the user shares publicly, add "Unofficial fan concept" micro-caption bottom-left for 1s during outro |
| 30s over-stuffing | Track 18 beats at 1.67s avg — already tight. If lint / render reveals overflow, cut the "Monitor" phone surface (18.0–19.5s) first since it's structurally duplicative of "Plan" |
| Black-frame flashes between beats | Law #11 padding + diagnostic snippet run before every render (per-project checklist) |
| Caption overlap with hero type | Track-index 30+ for all captions; visual verification frame-check per §4 |

## Verification Plan

1. `npx hyperframes lint` — zero errors, triage warnings
2. Run Law-#11 diagnostic console snippet in preview — all timelines ≥ slot duration
3. `npx hyperframes render --quality draft --output renders/draft.mp4` (fast iteration render)
4. Extract 30+ frames with ffmpeg (one per beat + transition mid-points) to `renders/frames/`
5. `Read` each frame — confirm: no cropped logos, no text overflow, whip transitions landing on beat boundaries, chrome gradients present on all headline type, grid+vignette+grain on every scene
6. Iterate fix → re-draft → re-verify until every frame passes
7. Final: `npx hyperframes render --quality standard --output renders/linear-promo-final.mp4`
8. Report path + duration back to the user

## Next Step

Invoke `superpowers:writing-plans` to produce the step-by-step implementation plan (project scaffold → asset acquisition → composition authoring order → per-beat timelines → integration → verification).
