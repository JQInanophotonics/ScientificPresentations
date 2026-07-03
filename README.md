# Scientific Presentations

## Forewords

I won't judge if you use PowerPoint, but know that you'll have a non-negligible chance your slide deck won't display properly at a conference — mismatched fonts, resized figures, colors gone wrong. Beamer (LaTeX for presentations) outputs a PDF that looks identical on every machine, since PDF is a standard, not a moving target.

This repo is the group's guide to making a good presentation — the narrative you tell, the figures that carry it, and the details that make both land. That covers structuring a talk, building **consistent, reusable, animatable** figures for it, and the practice of assembling them into a deck. Almost all of it — how to structure the talk, how to size and export figures, how to animate them — holds regardless of which software assembles the final deck. Only the last mile (actually placing figures and compiling) is tool-specific, and there the group's recommended path is Beamer via the [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate). Read the pages in order the first time; use them as a checklist afterwards — same spirit as [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement).

## Basic presentation design

Before the tooling, a few things worth keeping in mind whichever tool you use. This is about how a slide *looks*; for how a whole talk should be *structured and paced* — the narrative arc, what to cut for a short talk, how to hook the audience — see [00 — Structuring a talk](Presentations/00-TalkStructure.md).

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

Sibling wikis in the org: [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign), [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement), [ScientificWriting](https://github.com/JQInanophotonics/ScientificWriting). Beamer macros: [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate).
