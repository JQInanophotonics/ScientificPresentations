# 01 — Using the artboard

## One artboard, one figure, one size

Every figure that goes into a talk starts life as its own **artboard** in Illustrator, sized to match the usable area of your slide — not the full slide, the area inside the template's margins/title. Build the figure to fill that artboard. When you place the exported PDF into Beamer at `\includegraphics[width=\textwidth]{...}`, it lands at the right proportions without Beamer (or you) having to rescale, crop, or stretch it.

Why this matters: a figure exported at the wrong aspect ratio gets squashed or padded with dead space the moment it's dropped into a slide, and a figure resized by hand in Illustrator after the fact drifts out of sync with every other figure's line weight and font size in the talk.

## Setting artboard size and units

- `File > Document Setup` or the Artboard tool (`Shift+O`) to set width/height in the same units you think in (mm or in — pick one for the whole project).
- Match the artboard's aspect ratio to your slide template's content frame. `JqiNanoBeamerTemplate` uses `aspectratio=169` (16:9); size your artboards accordingly so a full-width figure fills the frame without letterboxing.
- One `.ai` file can hold **many artboards** — one per figure in the talk, or one per frame of an animation (see [03 — Animations](03-Animations.md)). Keep the whole talk's visual assets in one file so they share swatches, character styles, and symbols.

## Naming artboards

When a file holds more than a couple of artboards, name them (double-click the artboard name in the Artboards panel) after what they show, not their order: `Fig1-SetupDiagram`, `Fig2-SpectrumOverlay`. Order still matters for animations (see below) — Illustrator exports multi-page PDFs in artboard order, so keep animation frames adjacent and sequential in the Artboards panel.

## Reuse the group's styles

Don't start from a blank artboard. Pull in colors, character styles, and symbols from [ScientificGraphicDesign/IllustratorsStyles](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/IllustratorsStyles) so a figure built for a talk still matches the group's paper figures.

Next: [02 — Exporting figures](02-ExportingFigures.md)
