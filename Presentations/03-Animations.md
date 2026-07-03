# 03 — Making consistent animations

## The technique: N artboards = N frames

An "animation" in a Beamer talk is just a sequence of slides that reveal progressively more of a figure. You build it exactly like a static figure, except each frame is its **own artboard**, all the same size, all in the same `.ai` file, in the order they should appear:

- Frame 1: the base figure.
- Frame 2: frame 1 plus whatever appears next.
- Frame 3: frame 2 plus the next addition.
- ...and so on.

Keep everything that doesn't change between frames **identical** — copy the artwork from one artboard to the next rather than redrawing it, so static elements don't jitter by a fraction of a point when the slide advances. Illustrator's `Edit > Paste in Place` (`Cmd+Shift+V`) across artboards keeps positions exact.

## Export as one multi-page PDF

Select all the frame-artboards and export them together as a **single multi-page PDF** — one page per artboard, in artboard order: `File > Export > Export As... > Adobe PDF`, with "All" (or the relevant range) of artboards selected, "Save each artboard as a separate file" **unchecked**. The deliverable is one `.pdf` with N pages, not N separate PDFs.

## Drive it with `\imageseq`

`JqiNanoBeamerTemplate` ships a macro built exactly for this:

```latex
\imageseq[width=0.8\textwidth]{frames.pdf}{
  1:1,      % page 1 on slide-overlay 1
  2:2,      % page 2 on slide-overlay 2
  3:3-5,    % page 3 stays visible on overlays 3 through 5
  4:6-      % page 4 stays visible from overlay 6 onward
}
```

Each `page:overlay` pair maps one PDF page (one artboard) to when it appears. A page can be pinned to a single overlay, a range (`3-5`), or "from here on" (`6-`). See the template's own example in `test-helpers.tex`:

```latex
\imageseq[width=\textwidth]{./Figures/CombMetrology.pdf}{1:1, 2:2, 3:3-}
```

Full macro syntax and options: [`MACRO_MANUAL.md#image-sequences`](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate/blob/main/MACRO_MANUAL.md#image-sequences) in the template repo.

## Why not GIFs or embedded video

They work in Beamer, but they don't survive the trip: a `.gif` or `.mp4` embedded in a PDF depends on the PDF viewer's media support, which is inconsistent across conference-room laptops — exactly the portability problem Beamer/PDF was supposed to solve (see the main [README](../README.md) forewords). The multi-page-PDF + `\imageseq` approach stays inside plain PDF overlays, so it renders identically everywhere.

Next: [04 — Using Beamer](04-UsingBeamer.md)
