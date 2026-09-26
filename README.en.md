# HyperFrames Student Kit

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

A reusable video editing kit for **Codex and Claude Code**.
Bring your own recording. Cut the dead air, review the mistakes, plan the story and
build motion graphics with HyperFrames and GSAP.

![It's over. It's the end of an era: complex editing, hours of work and traditional software give way to the agent](docs/images/capa-acabou.jpg)

## 📖 User guide

Full guide (landing + step by step): **https://inematds.github.io/hyperframes-student-kit/guia/en/**

**Just getting started?** Read the [quick guide](docs/GUIA-RAPIDO.md) (in Portuguese): installation, opening the
project in Codex or Claude Code, the ElevenLabs key, the prompt for your first video,
the feedback loop and how to turn the result into a skill. The
[video tutorial transcript](docs/TRANSCRICAO-TUTORIAL.md) (in Portuguese) is translated.

**The method:** the five steps (transcribe, cut, plan the beats, use and create skills,
verify in a loop), the speech-anchored prompt, one recording in three styles, folder
sizzle, product video and the reference → skill loop are in [docs/METODO.md](docs/METODO.md) (in Portuguese)
and the ready-made recipes in [docs/PROMPTS.md](docs/PROMPTS.md) (in Portuguese).

## Short-form video examples

The frames below are simulated presenter photos, used as 9:16 framing references.

### Curiosity reel: unblock your project

![Curiosity reel: unblock your project](docs/images/exemplo-1.jpg)

### Curiosity reel: build a better AI system

![Curiosity reel: build a better AI system](docs/images/exemplo-2.jpg)

### Live announcement

![Live announcement](docs/images/exemplo-3.jpg)

## What's included

- **15 skills**, mirrored for both assistants, with their helper scripts and references.
- **406 draft motion graphics cards** in two styles, with manifests, CSS tokens and editable slots.
- **Two scene templates:** dark graph paper and a glass popout on the left.
- Tools for transcription, silence cutting, mistake detection, rendering reviewed cuts,
  transcript retiming, EDL review, beat-sync validation and preflight.
- **Short-form video editing:** reels, YouTube Shorts, hook and payoff planning, precise captions, moving B-roll and audio review.
- **12 existing teaching projects**, preserved from the original kit (they stay only in the local copy; `video-projects/` is not versioned in this repository).
- A synthetic starter composition and an editing fixture that need neither a recording nor an API key.

## Optional tools and services

**By default the kit uses ElevenLabs Scribe for transcription and Kie.ai for generating videos and
images.** Bring your own API keys and credits when using these services.
You can ask the assistant to use OpenAI Whisper or local Whisper, or
provide an existing transcript with word-level timing. Generated assets are optional.

The local starter needs no paid transcription or generation API. See the
[tools, accounts and API keys guide](docs/TOOLS-AND-API-KEYS.md) (in Portuguese) for the required tools,
optional services, setup details and prompts you can copy.

## Installation

Install Node.js **22 or later**, Git, FFmpeg (including ffprobe) and Chrome or
Chromium. Make `node`, `ffmpeg` and `ffprobe` available in your terminal.
Then run these commands in PowerShell, the macOS Terminal or a Linux shell:

```sh
git clone https://github.com/inematds/hyperframes-student-kit.git
cd hyperframes-student-kit
npm ci
npm run setup
npm test
```

Setup checks the tools and creates the `.env` only if it doesn't exist. Add only the keys
for the services you choose. The included transcription helper uses ElevenLabs;
the Whisper and Kie.ai integrations need the configuration described in the
[tools guide](docs/TOOLS-AND-API-KEYS.md) (in Portuguese). [Setup and troubleshooting](docs/SETUP.md) (in Portuguese).

## Render your first example

```sh
npm run demo
cd video-projects/demo
npx hyperframes lint
npx hyperframes preview
```

Scrub through the eight-second animation in the Studio. After reviewing it, stop the preview
with Ctrl+C and render:

```sh
npx hyperframes render --quality draft --output renders/demo.mp4
```

The demo uses local GSAP. HyperFrames may download and cache its font substitutions on the first render. It has no narration. The separate
[synthetic transcript](examples/editing/source.json) exercises the cutting tools;
it is fictional test data, not a transcript of the title animation.

## Edit your recording

Open this repository's folder in Codex or Claude Code and say:

> Use edit-video to edit my recording at [local path]. Keep my examples and the
> core lessons. Tighten the dead air, show me the proposed mistake cuts and use the
> dark graph-paper style with occasional glass cards. Produce a reviewed draft.

Codex: `$edit-video`. Claude Code: `/edit-video`. For a single operation use
`cut-silences`, `cut-mistakes`, `video-storytelling` or `style-library`.
See the [step-by-step workflow](docs/WORKFLOW.md) (in Portuguese), the [prompt recipes](docs/PROMPTS.md) (in Portuguese)
and the [storytelling workbook](docs/STORYTELLING-WORKBOOK.md) (in Portuguese).

## Create a reel or YouTube Short

> Use short-form-edit to turn my recording at [local path] into a 9:16 reel.
> Build a genuine hook and payoff, preserve my meaning, tighten the mistakes and
> add precise captions, purposeful moving images and sound design. Prepare
> a draft for review. Use my existing recording before proposing generated assets.

Codex: `$short-form-edit`. Claude Code: `/short-form-edit`.
The skill includes planning references and validators for caption timing, source
mapping, scene coverage and image reuse. It is an agent-guided workflow;
review the actual motion and audio before publishing.
[Short-form walkthrough and validation commands](docs/SHORT-FORM.md) (in Portuguese).

## Create a motion design showreel

> Use motion-showreel to create a 15-second showreel for [brand]. Study my reference
> reel at [local path], if I provide one. Choose a motif that transforms through
> every chapter, cut on the music's timing grid and end on the logo. Show me the
> storyboard and the timing sheet before building, and confirm before any paid generation.

Codex: `$motion-showreel`. Claude Code: `/motion-showreel`.
The skill includes the measured analysis of the reel it came from, a per-chapter
technique library, a HUD template and Node tools to analyze a reference video,
measure a song's timing grid, splice it onto the cut grid and pre-mix sound
effects. Music and SFX can be free; for the hero object, prefer your
Kling AI subscription (`kling` CLI). Kie.ai and ElevenLabs remain optional paid alternatives.

## Existing examples and migration

This is the main student kit repository. The newer video pipeline kit was
merged here with both Git histories preserved. The 12 original projects remain
in `video-projects/`, along with the original shared brand examples and the
`make-a-video`, `short-form-video` and `website-to-hyperframes` skills.
Use `short-form-edit` for new reels; `short-form-video` documents the older compositions
from the May Shorts. [Migration and compatibility notes](docs/MIGRATION.md) (in Portuguese).

## Explore and customize

| Resource | Start here |
| --- | --- |
| Step-by-step quick guide | [docs/GUIA-RAPIDO.md](docs/GUIA-RAPIDO.md) (in Portuguese) |
| Video tutorial transcript | [docs/TRANSCRICAO-TUTORIAL.md](docs/TRANSCRICAO-TUTORIAL.md) (in Portuguese) |
| Shared agent instructions | [AGENTS.md](AGENTS.md) |
| Motion and transition vocabulary | [MOTION_PHILOSOPHY.md](MOTION_PHILOSOPHY.md) |
| Card library and design tokens | [Library guide](style-library/GUIDE.md) |
| Searchable card metadata | [registry.json](style-library/registry.json) |
| Full-scene templates | [Templates guide](style-templates/README.md) |
| Codex setup and mirroring | [.codex/README.md](.codex/README.md) |
| Release checks and limits | [Verification](docs/VERIFICATION.md) (in Portuguese) |
| Third-party resources | [Notices](THIRD_PARTY_NOTICES.md) |

The cards are reusable **draft assets**. Test the cards you choose with your own text
and your own recording. The library templates may load GSAP and Google Fonts from their
public CDNs; localize those dependencies when assembling a final project.

The example images are simulated photos; no private recording, transcript, credential
or personal configuration has been included. Examples that were already public remain in the
repository. New folders in `video-projects/` and `raw-media/` are ignored automatically;
files already tracked by Git remain tracked. Create a new project for your own recording.
