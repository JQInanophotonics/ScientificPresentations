# Scientific Presentations

## Forewords

I won't judge if you use PowerPoint, but know that you'll have a non-negligible chance your slide deck won't display properly at a conference — mismatched fonts, resized figures, colors gone wrong. Beamer (LaTeX for presentations) outputs a PDF that looks identical on every machine, since PDF is a standard, not a moving target. It also means the talk itself is a plain-text `.tex` file rather than a binary blob, so it version-controls properly — real diffs, real history, mergeable changes — the way a `.pptx` never quite does; see [04 — Using Beamer, "Why this is a git repo, not a single file"](Presentations/04-UsingBeamer.md#why-this-is-a-git-repo-not-a-single-file). If git itself is new to you, [QuickStartGit](https://github.com/JQInanophotonics/QuickStartGit) is the group's prerequisite primer — read that first.

This repo is the group's guide to making a good presentation — the narrative you tell, the figures that carry it, and the details that make both land. That covers structuring a talk, building **consistent, reusable, animatable** figures for it, and the practice of assembling them into a deck. Almost all of it — how to structure the talk, how to size and export figures, how to animate them — holds regardless of which software assembles the final deck. Only the last mile (actually placing figures and compiling) is tool-specific, and there the group's recommended path is Beamer via the [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate). Read the pages in order the first time; use them as a checklist afterwards — same spirit as [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement).

## Structuring a talk

Before anything gets built in Illustrator or Beamer, decide what the talk is actually going to say — full discussion in [00 — Structuring a talk](Presentations/00-TalkStructure.md), summary here.

A presentation, like a paper, has to be appealing — more so, since there's no abstract to skim first. Two things to settle before drafting a single slide: **tell the audience why they're here** — not just why the topic matters in the abstract, but why *this* audience should want to listen for the next several minutes — and **respect how much they can absorb**: a talk is heard once, linearly, at your pace, with no rereading; a 12-minute talk has room for two or three ideas, not a paper's worth of them. Keep only what the story needs; the rest is backup slides (see below).

Reading the reference talk ([Example — CLEOus](Presentations/Example-CLEOus.md)) section by section, that plays out as a specific, reusable shape — not a universal law, just what this real, well-received talk actually does:

1. **Hook** — CLEOus opens on optical frequency combs in general and their three textbook applications, not the authors' own result — context the room already cares about, before narrowing to this work.
2. **Tension** — states plainly what the field is stuck on: shrinking a comb on-chip makes CEO detection very hard, ending the slide on `\bad{CEO detection VERY hard with on-chip OFC}`.
3. **The idea** — pivots explicitly as the answer to that exact tension: *"Rethinking microcomb beyond shrinking... CEO encoded rather than post-generation detected."*
4. **Evidence, including the false start** — a whole slide is spent on the first demonstration not being good enough (`\badbox{Not readily useful: ...}`) before the next slide shows the fix.
5. **Payoff, bookended to the hook** — the "Metrological use" slide revisits the *same three applications* from the opening and shows the new architecture satisfying each.
6. **Conclusion** — restates the bottleneck and the architecture, briefly.

See [00 — Structuring a talk](Presentations/00-TalkStructure.md) for the full discussion, how the `\good`/`\warn`/`\bad` macros ([04 — Using Beamer](Presentations/04-UsingBeamer.md)) mark these beats on-slide, and a checklist to self-review a draft against.

## Basic presentation design

Once the narrative is decided, a few things about how a slide *looks*, whichever tool you use:

- **One idea per slide.** If you're tempted to add a second point, it's a second slide.
- **Typography and color should match across the whole talk** — same font, same palette, same meaning for a given color from the first slide to the last. See [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign) for the group's palettes and type choices.
- **Design for colorblindness** — don't rely on red/green alone to distinguish curves; use Illustrator's proof-colors view (Protanopia/Deuteranopia) before you call a figure done.
- **Don't overfill a slide.** If your audience is reading dense text, they've stopped listening to you.

Further reading: *Presentation Zen* by Garr Reynolds, *The Visual Display of Quantitative Information* by Edward Tufte (also recommended in ScientificGraphicDesign).

## The rules, in one screen

These first three apply no matter which software assembles the final deck — they're about how the figure is built, not how it's placed:

1. **One Illustrator artboard per figure, sized to the exact slot it fills** — the column width it sits in, not the whole slide (see [01 — Artboards](Presentations/01-Artboards.md)).
2. **Never rescale a figure once it's placed** — no `width=`/`scale=` in Beamer, no drag-resizing in PowerPoint. That's the whole point of rule 1: place it at native size and any text set inside it in Illustrator matches your slide's type. If it doesn't fit, resize the *artboard* and re-export.
3. **For animated reveals**, build each frame as its own same-size artboard and export them together — see [03 — Animations](Presentations/03-Animations.md) for the Beamer path (`\imageseq` off one multi-page PDF) and the PowerPoint path (one PNG per frame, revealed with `Appear`/`Disappear` animations).

The rest is specific to whichever tool you use:

4. **Export vector (PDF)** for Beamer or anything staying editable; **export raster (PNG)** for PowerPoint or photographic content.
5. **If you're using Beamer** (recommended, see forewords above) — **start every talk from [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate)**, clone it fresh per talk, don't hand-roll a preamble. Its macro reference lives in [`MACRO_MANUAL.md`](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate/blob/main/MACRO_MANUAL.md) — this wiki won't duplicate it.
6. **If you're using PowerPoint** — see the PowerPoint notes in [02 — Exporting figures](Presentations/02-ExportingFigures.md); the artboard/no-rescale rules above are the ones PowerPoint makes easiest to break by accident.

## Pages

| Page | What it covers |
|------|-----------------|
| [00 — Structuring a talk](Presentations/00-TalkStructure.md) | The narrative arc, pacing for short talks, what to cut vs. push to backup |
| [01 — Artboards](Presentations/01-Artboards.md) | Sizing and organizing Illustrator artboards for slide figures |
| [02 — Exporting figures](Presentations/02-ExportingFigures.md) | Vector vs. raster export, and what PowerPoint needs that Beamer doesn't |
| [03 — Animations](Presentations/03-Animations.md) | Multi-artboard → multi-page PDF → `\imageseq` |
| [04 — Using Beamer](Presentations/04-UsingBeamer.md) | Starting a talk from the template, folder layout, compiling, house style |
| [Example — CLEOus](Presentations/Example-CLEOus.md) | Annotated walkthrough of a real talk, start to finish |

## What's in this repo

```
ScientificPresentations/
├── README.md
└── Presentations/
    ├── 00-TalkStructure.md
    ├── 01-Artboards.md
    ├── 02-ExportingFigures.md
    ├── 03-Animations.md
    ├── 04-UsingBeamer.md
    └── Example-CLEOus.md
```

No binary assets live here. Illustrator templates and palettes are in [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign); the Beamer starting point is [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate).

## See also

Prerequisite: [QuickStartGit](https://github.com/JQInanophotonics/QuickStartGit) — the group's git primer, if `clone`/`commit`/`push`/`pull`/branches/conflicts aren't already familiar; also covers syncing a repo through Overleaf's Git integration. Sibling wikis in the org: [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign), [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement), [ScientificWriting](https://github.com/JQInanophotonics/ScientificWriting). Beamer macros: [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate).
