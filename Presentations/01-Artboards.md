# 01 — Using the artboard

## One artboard, one figure, one size — but "size" means the slot, not the slide

Every figure that goes into a talk starts life as its own **artboard** in Illustrator, sized to match the *slot it will occupy in the slide layout* — not necessarily the whole slide. Most talk slides aren't a single full-bleed figure; they're `\twocolfig{textW}{figW}{...}{...}` (see [04 — Using Beamer](04-UsingBeamer.md)), text on one side and the figure filling the other column. Size the artboard to that column, not to the full 16:9 canvas — otherwise Beamer letterboxes or overflows it.

A real reference talk ([Example — CLEOus](Example-CLEOus.md)) settles on two recurring artboard sizes because it only ever uses two column splits:

| Layout | Column width | Artboard size used |
|---|---|---|
| `\twocolfig[T]{0.3}{0.7}{...}` (narrow text, wide figure) | ~70% of content width | **297 × 222 pt** — `WaferScale.pdf`, `ExperimentalDemonstration.pdf`, `PDCS_MetrologyResults.pdf` |
| `\twocolfig{0.55}{0.45}{...}` or `{0.5}{0.5}{...}` (roughly even split) | ~45–50% of content width | **200–212 × 222–230 pt** — `CombSWAP_Broadening.pdf` (200×222), `Conclusion.pdf` (212×222), `PDCS_Concept.pdf` (212×230) |

Once you know which column split a slide uses, reuse the matching artboard size for every figure that goes in that slot — don't recompute it per figure. This is also why `CombMetrology.pdf` (used inside a *nested* `\twocol[c]{0.3}{0.3}{...}` for a small equation-side diagram) is smaller still: **220 × 254 pt**. The artboard follows the layout, the layout doesn't follow the artboard.

## Never rescale a figure once it's placed

This is the entire reason to bother sizing the artboard correctly in the first place: **place the figure at its native size, never scaled up or down** — no `width=`/`scale=` in `\includegraphics`/`\imageseq` (Beamer), no drag-resizing on the canvas (PowerPoint). This holds regardless of which tool assembles the final deck.

Why it matters: any text set inside the figure in Illustrator was drawn at a specific point size. If you then place the figure at, say, 80% of its native size to force it into a slot, that text shrinks to 80% along with it — and now it no longer matches your slide's body text, even though you picked the same font and the same nominal size in both places. Scale the figure and you've silently made two different type sizes look the same on screen and two same-declared sizes look different. The only fix that doesn't fight itself: get the artboard's physical size right before export (per the sizes above), so placing it "as-is" is always correct.

If a figure looks wrong once placed — too big, too small, wrong aspect ratio for its slot — that's a signal to go back to Illustrator, resize the *artboard*, and re-export. It is never a signal to reach for a `width=` option.



## Setting artboard size and units

- `File > Document Setup` or the Artboard tool (`Shift+O`) to set width/height. Working in points (`pt`) makes it trivial to match the numbers above directly, since Beamer/TikZ also thinks in points.
- `JqiNanoBeamerTemplate` uses `aspectratio=169` (16:9) for the *overall slide*, but individual figure artboards are sized to their column, as shown above — don't assume every artboard should be 16:9.
- One `.ai` file can hold **many artboards**. In the reference talk, `CombMetrology.ai` alone holds 9 artboards, all 220×254 pt, exported together as one multi-page PDF and reused across *three different bullet points on the same slide* (see [03 — Animations](03-Animations.md) for how `\imageseq` pulls different page ranges from the same file).

## Naming artboards and files

The reference talk names figures after the physics, one `.ai`/`.pdf` pair per concept — the same convention used in [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement)'s `Data/` folders:

```
Figures/
├── PDCS_Concept.ai / .pdf              # one concept per file
├── PDCS_DispersionConditions.ai / .pdf
├── PDCS_FirstResults.ai / .pdf
├── PDCS_MetrologyResults.ai / .pdf
├── CombMetrology.ai / .pdf             # reused across 3 bullets on 1 slide
├── CombSWAP_Broadening.ai / .pdf
├── DifferentSpeedRings.ai / .pdf
├── WaferScale.ai / .pdf
├── ExperimentalDemonstration.ai / .pdf
├── Conclusion.ai / .pdf
├── Acknowledgment.ai / .pdf            # opening "team" slide
├── Acknowledgment_end.pdf              # closing thank-you slide (different crop)
└── raster/                             # photos — see 02-ExportingFigures.md
```

Not `Figure1`, `Figure2`, `Figure_final`. When a name describes the physics, you can find the right artboard six months later without opening every file. Within a multi-artboard file, name each artboard for what it adds (`Frame1-Baseline`, `Frame2-AddSecondPump`, ...) — Illustrator exports multi-page PDFs in artboard order, so keep frames adjacent and sequential in the Artboards panel.

## Reuse the group's styles

Don't start from a blank artboard. Pull in colors, character styles, and symbols from [ScientificGraphicDesign/IllustratorsStyles](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/IllustratorsStyles) so a figure built for a talk still matches the group's paper figures. The reference talk also ships its own `fonts/` folder (Helvetica Neue, IBM Plex Sans Condensed) — match those in Illustrator's type tool so text set in Illustrator doesn't visually clash with text set in Beamer on the same slide.

Next: [02 — Exporting figures](02-ExportingFigures.md)
