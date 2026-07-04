# 03 — Making consistent animations

## The technique: N artboards = N frames

An "animation" in a Beamer talk is just a sequence of slides that reveal progressively more of a figure. You build it exactly like a static figure, except each frame is its **own artboard**, all the same size, all in the same `.ai` file, in the order they should appear. Keep everything that doesn't change between frames **identical** — copy the artwork from one artboard to the next rather than redrawing it (Illustrator's `Edit > Paste in Place`, `Cmd+Shift+V`), so static elements don't jitter by a fraction of a point when the slide advances.

## Export as one multi-page PDF

Select the frame-artboards and export them together as a **single multi-page PDF**, one page per artboard, in artboard order: `File > Export > Export As... > Adobe PDF`, "Save each artboard as a separate file" **unchecked**. The reference talk ([Example — CLEOus](Example-CLEOus.md)) does this throughout — `WaferScale.pdf` is 3 pages, `ExperimentalDemonstration.pdf` is 5, `CombMetrology.pdf` is 9.

## Reveal it, frame by frame: plain Beamer first

The mechanism underneath all of this is standard Beamer, nothing specific to this group's template: `\includegraphics[page=N]{file.pdf}` pulls one page out of a multi-page PDF, and wrapping it in `\only<M>{...}` shows that page only on overlay `M`, hiding it the rest of the time. Beamer's overlay specification (`<M>`) supports single overlays, ranges, and open-ended ranges natively, so the same three shapes as [01 — Artboards](01-Artboards.md#never-rescale-a-figure-once-its-placed) always yields — no `page=` mismatch to debug, no `\includegraphics[width=...]` sneaking back in.

```latex
\only<1>{\includegraphics[page=1]{./Figures/WaferScale.pdf}}
\only<2>{\includegraphics[page=2]{./Figures/WaferScale.pdf}}
\only<3->{\includegraphics[page=3]{./Figures/WaferScale.pdf}}
```

This works in any Beamer document — no theme, no package, no macro required. Its downside is purely readability: for every frame you repeat the full `\includegraphics{...}` command, and a mapping with several pages and overlay ranges turns into a wall of near-identical `\only<...>{...}` lines that's easy to miscount while editing.

## The shortcut: `\imageseq` (this template only)

`JqiNanoBeamerTemplate` ships `\imageseq[opts]{file.pdf}{mapping}` to collapse exactly that repetition into one line — it is **not** a Beamer built-in, it only exists if your talk is built on this group's template, but if it is, it's the version to reach for: one filename, one line, and the `page:overlay` pairs read like a table instead of a stack of duplicated commands.

```latex
\imageseq{./Figures/WaferScale.pdf}{1:1,2:2,3:3}
```

is the same three-step reveal as the `\only` block above, in a fifth of the source. The mapping grammar: `page:overlay`, where `overlay` is a single number, a range (`3-5`), or open-ended (`6-`) — the exact same overlay shapes `\only<...>` accepts, just written once per page instead of once per line.

No `[width=...]` option in any of the calls below, on purpose — see [01 — Artboards, "Never rescale a figure once it's placed"](01-Artboards.md#never-rescale-a-figure-once-its-placed). Every real call leaves it out: the frame set was already exported at the exact size its slot needs, so it places 1:1. If you find yourself reaching for `[width=...]` to make an `\imageseq` (or a raw `\includegraphics[page=...]`) fit, that's a sign to resize the source artboards instead.

Real examples, straight from the reference talk's `.tex` — all bare, no scaling option:

```latex
% WaferScale.pdf — a plain 3-step build, one page per overlay
\imageseq{./Figures/WaferScale.pdf}{1:1,2:2,3:3}

% ExperimentalDemonstration.pdf — page 1 is a title/setup frame shown before
% the overlay sequence starts, so the mapping begins at page 2. Written as
% plain \only blocks this would be four repeated \includegraphics lines;
% \imageseq keeps it to one.
\imageseq{./Figures/ExperimentalDemonstration.pdf}{2:1,3:2,4:3,5:4-}

% Conclusion.pdf — the last page stays on screen for the remainder of the frame
\imageseq{./Figures/Conclusion.pdf}{1:1,2:2,3:3-}
```

## One artboard set, several frames

You don't need a separate multi-page PDF per slide. `CombMetrology.pdf` (9 artboards, see [01 — Artboards](01-Artboards.md)) is built once and then pulled from **three separate times** on the same slide, each `\twocolfig` bullet showing a different slice of it:

```latex
\item Precise laser frequency: optical frequency synthesis
  \twocol[c]{0.3}{0.3}{ ... }{ \imageseq{./Figures/CombMetrology.pdf}{4:-2,8:3-} }
\item Low-noise microwave generation: optical frequency division (OFD)
  \twocol[b]{0.3}{0.3}{ ... }{ \imageseq{./Figures/CombMetrology.pdf}{3:-1,6:2,7:3-} }
\item Clock stability transfer: optical clockwork
  \twocol[b]{0.3}{0.3}{ ... }{ \imageseq{./Figures/CombMetrology.pdf}{5:-2,9:3-} }
```

Each `\imageseq` call is independent — same source PDF, different page selection, different overlay timing — so one Illustrator file backs the whole slide instead of three near-duplicate ones. If you find yourself reusing the same visual idea more than once in a talk, this is the pattern: build the full frame set once, then call `\imageseq` again wherever a subset of it is needed.

Full macro syntax and options: [`MACRO_MANUAL.md#image-sequences`](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate/blob/main/MACRO_MANUAL.md#image-sequences) in the template repo.

## Why not GIFs or embedded video

They work in Beamer, but they don't survive the trip: a `.gif` or `.mp4` embedded in a PDF depends on the PDF viewer's media support, which is inconsistent across conference-room laptops — exactly the portability problem Beamer/PDF was supposed to solve (see the main [README](../README.md) forewords). The multi-page-PDF + `\imageseq` approach stays inside plain PDF overlays, so it renders identically everywhere.

Next: [04 — Using Beamer](04-UsingBeamer.md)
