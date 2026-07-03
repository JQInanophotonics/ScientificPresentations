# Scientific Presentations

## Forewords

I won't judge if you use PowerPoint, but know that you'll have a non-negligible chance your slide deck won't display properly at a conference — mismatched fonts, resized figures, colors gone wrong. Beamer (LaTeX for presentations) outputs a PDF that looks identical on every machine, since PDF is a standard, not a moving target.

This repo is the group's guide to building **consistent, reusable, animatable** figures for a talk, and to giving that talk in Beamer using the [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate). Read the pages in order the first time; use them as a checklist afterwards — same spirit as [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement).

## Basic presentation design

Before the tooling, a few things worth keeping in mind whichever tool you use:

- **One idea per slide.** If you're tempted to add a second point, it's a second slide.
- **Typography and color should match across the whole talk** — same font, same palette, same meaning for a given color from the first slide to the last. See [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign) for the group's palettes and type choices.
- **Design for colorblindness** — don't rely on red/green alone to distinguish curves; use Illustrator's proof-colors view (Protanopia/Deuteranopia) before you call a figure done.
- **Don't overfill a slide.** If your audience is reading dense text, they've stopped listening to you.

Further reading: *Presentation Zen* by Garr Reynolds, *The Visual Display of Quantitative Information* by Edward Tufte (also recommended in ScientificGraphicDesign).

## The rules, in one screen

1. **One Illustrator artboard per figure**, sized to match the slide's usable area — nothing gets rescaled or distorted when it's placed in Beamer.
2. **Export vector (PDF/SVG)** for anything going into Beamer or staying editable; **export raster (PNG) only** for PowerPoint or photographic content.
3. **For animated reveals**, build each frame as its own same-size artboard, export all of them together as **one multi-page PDF**, and drive it with the template's `\imageseq` macro.
4. **Start every talk from [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate)** — clone it fresh per talk, don't hand-roll a preamble.
5. **The macro reference lives in the template**, at [`MACRO_MANUAL.md`](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate/blob/main/MACRO_MANUAL.md) — this wiki won't duplicate it.

## Pages

| Page | What it covers |
|------|-----------------|
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
    ├── 01-Artboards.md
    ├── 02-ExportingFigures.md
    ├── 03-Animations.md
    ├── 04-UsingBeamer.md
    └── Example-CLEOus.md
```

No binary assets live here. Illustrator templates and palettes are in [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign); the Beamer starting point is [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate).

## See also

Sibling wikis in the org: [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign), [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement), [ScientificWriting](https://github.com/JQInanophotonics/ScientificWriting). Beamer macros: [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate).
