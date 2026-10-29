# AI Horror Video Director

**Turn any scary idea into a faceless horror YouTube video with a locked animation style, a written story, sound design and an automated build.**

Give an AI assistant a topic (or nothing at all). It develops the story, locks the look, writes the storyboard and prompts, and either hands you scene-by-scene prompts or builds the finished video file for you.

Inspired by the two-mode structure of [kaomei/stickman-video-director](https://github.com/kaomei/stickman-video-director). This repository is independent and contains original horror-specific styles, story material, prompts and tooling.

## What is in the box

| Folder | What it holds |
|---|---|
| [`skills/directing-horror-videos/`](skills/directing-horror-videos/SKILL.md) | The director skill. Point your AI assistant here and it runs the whole process. |
| [`styles/`](styles/README.md) | Six locked animation styles H1-H6 (`styles.json` is what the tools read). |
| [`story/`](story/) | Horror craft rules, beat sheets by video length, 30+ original story ideas. |
| [`prompts/`](prompts/) | Copy-paste prompts for story, script, characters, storyboard, images, clips, titles, Shorts. |
| [`sound/`](sound/sound-design.md) | Sound design guide and mix targets. |
| [`scripts/hvd.py`](scripts/hvd.py) | CLI: scaffold, validate, generate prompts, captions, placeholders, ffmpeg build. |
| [`templates/`](templates/storyboard.json) | Blank storyboard. |
| [`examples/the-last-bus/`](examples/the-last-bus/storyboard.json) | A complete 12-scene, ~1.5 minute storyboard. |
| [`docs/`](docs/) | Pipeline walkthrough and YouTube publishing notes. |
| [`tests/`](tests/test_hvd.py) | Tests for the tooling. |

## Two ways to make your video

| | A - Director Mode | B - Autopilot Mode |
|---|---|---|
| Approach | AI plans every shot; you generate clips and edit | AI plans, you approve, a script builds the video |
| Length | 30 s - 3 min | 3 - 15 min narrated |
| Visuals | generated animated clips | one image per scene with slow camera moves |
| Needs | any AI chat assistant, a video generator, an editor | an AI agent with a shell, ffmpeg, an image generator, a voice (TTS or your own) |
| Output | treatment, storyboard, per-scene prompts, sound plan | a finished `.mp4` plus `captions.srt` |

Image, video and voice services are not included and may charge by their own plans.

## The six styles

| ID | Name | Look |
|---|---|---|
| H1 | Void Stickman | white stick figures on pure black, one blood-red accent |
| H2 | Found Footage Glitch | VHS camcorder, scanlines, timestamp, pale figure in the back |
| H3 | Gothic Ink Storybook | sepia parchment, crosshatch ink, woodcut shadows |
| H4 | Red Room Creepypasta | teal and crimson, fog, a tall faceless silhouette |
| H5 | Cursed Crayon | child's crayon drawing that is slightly wrong |
| H6 | Liminal Rooms | endless empty corridors, one tiny figure, fluorescent hum |

See [`styles/README.md`](styles/README.md) for when to use which.

## Quick start

### 1. Tell your AI assistant

```
Read this repository: <your repo URL>
Start with skills/directing-horror-videos/SKILL.md and follow it.
Make me a [2-minute] horror video in style H1 about [your topic, or "surprise me"].
Show me the treatment first. After I approve it, write the script and the storyboard.
```

### 2. Or run it by hand

```bash
# create a project
python scripts/hvd.py init my-first-horror --style H1 --aspect 16:9

# fill projects/my-first-horror/storyboard.json (use the prompts in prompts/)
python scripts/hvd.py validate projects/my-first-horror/storyboard.json
python scripts/hvd.py prompts  projects/my-first-horror/storyboard.json   # -> prompts.md

# generate images + narration, then
python scripts/hvd.py build projects/my-first-horror/storyboard.json --music bed.mp3 --burn-captions
```

### 3. Try the example without spending anything

```bash
python scripts/hvd.py validate examples/the-last-bus/storyboard.json
python scripts/hvd.py prompts  examples/the-last-bus/storyboard.json
pip install pillow
python scripts/hvd.py placeholders examples/the-last-bus/storyboard.json
python scripts/hvd.py build examples/the-last-bus/storyboard.json
```

`placeholders` draws dark text-card frames so you can test timing, motion, captions and audio before paying for any images.

## Workflow in one picture

```
idea -> loglines -> script -> characters -> storyboard.json
     -> validate -> prompts -> images/clips + narration + sfx
     -> build (ffmpeg) -> mp4 + srt -> title, thumbnail, Shorts
```

Full steps: [`docs/pipeline.md`](docs/pipeline.md).

## What `hvd.py` does

| Command | Purpose |
|---|---|
| `init` | Create a project folder from the template with a chosen style and aspect |
| `validate` | Catch missing fields, unknown styles or motions, undefined characters, narration that is too dense for its scene, missing files (`--check-files`) |
| `prompts` | Build image and video prompts per scene: style prefix + scene + locked character designs + negative prompt |
| `srt` | Generate captions timed from the storyboard (or from the real narration lengths) |
| `placeholders` | Draw test frames (needs Pillow) |
| `build` | Render each scene with a slow camera move and vignette (optional `--grain`), mix narration and sfx, add a music bed, optionally burn captions |

Requirements: Python 3.9+, ffmpeg. `pillow` only for `placeholders`. Burned-in captions need an ffmpeg build with libass; without it, upload `captions.srt` to YouTube instead.

## Principles baked in

- **No gore.** Dread comes from implication, a single wrong detail per scene, and silence.
- **Locked style.** Every prompt starts with the same style prefix; characters are defined once and inserted automatically.
- **Cheap iteration.** Validate before generating, preview three scenes before forty, replace one scene at a time.
- **Original stories.** The repo includes seeds and structures, not copied stories. Remix, do not reuse.
- **Honest publishing.** AI-assistance disclosure, music licensing and content notes are part of the checklist ([`docs/youtube-policy.md`](docs/youtube-policy.md)).

## Contributing

Add a style: copy an entry in `styles/styles.json`, give it a new ID, run `python -m unittest discover tests`. Add a story seed to `story/story-ideas.md`. Add an example storyboard under `examples/`.

## License

MIT, see [LICENSE](LICENSE). Third-party images, music, sound effects and voice services stay subject to their own terms. YouTube monetization is subject to YouTube's own review and policies.
