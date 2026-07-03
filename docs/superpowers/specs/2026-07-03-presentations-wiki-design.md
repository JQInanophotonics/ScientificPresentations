# ScientificPresentations poor-boy wiki — design

## Purpose

Turn this repo into a "poor-boy wiki" — plain Markdown, no build step, no
separate wiki repo — teaching postdocs/students in the group how to:

1. Build consistent scientific-talk figures on Illustrator artboards.
2. Export those figures correctly for vector vs. raster use.
3. Build consistent frame-by-frame animations from same-size artboards.
4. Give talks in Beamer using `JqiNanoBeamerTemplate`.

It follows the same pattern already established by
[`ScientificDataManagement`](https://github.com/JQInanophotonics/ScientificDataManagement):
a `README.md` that is a one-screen index + rules summary, with the actual
content as numbered Markdown pages in a subfolder.

## Non-goals

- No native GitHub Wiki, no static site generator (mkdocs/Docusaurus). Every
  sibling repo in the org uses plain in-repo Markdown; this stays consistent.
- No duplication of `JqiNanoBeamerTemplate/MACRO_MANUAL.md`. This wiki links
  to it as the single source of truth for macro syntax.
- No binary/boilerplate assets committed to this repo (no sample `.ai`
  files, no sample multi-page PDFs). Reusable Illustrator assets stay in
  `ScientificGraphicDesign`; the Beamer starting point stays in
  `JqiNanoBeamerTemplate`. This repo is documentation only.
- No general graphic-design theory beyond a short pointer section — that
  content already lives in `ScientificGraphicDesign`'s README and is not
  re-derived here.

## Repo structure

```
ScientificPresentations/
├── README.md                          # index + one-screen summary
└── Presentations/
    ├── 01-Artboards.md                # Illustrator artboard setup for slide figures
    ├── 02-ExportingFigures.md         # vector vs raster export, PowerPoint vs Beamer sizing
    ├── 03-Animations.md               # multi-artboard → multi-page PDF → \imageseq
    ├── 04-UsingBeamer.md              # start from JqiNanoBeamerTemplate, compile, house style
    └── Example-<TalkName>.md          # annotated walkthrough (placeholder talk)
```

## `README.md` content

- **Forewords** — keep/lightly edit the existing PowerPoint-vs-Beamer
  paragraph; state the repo's purpose (consistent figures, consistent
  animations, Beamer via `JqiNanoBeamerTemplate`).
- **Basic presentation design** *(short)* — a handful of bullets: one idea
  per slide, typography/color consistency (pointer to
  `ScientificGraphicDesign`), don't overfill slides, accessibility
  (colorblind-safe, matching the graphic-design repo's existing note), plus
  2-3 book recommendations (e.g. *Presentation Zen*, Tufte). Not a
  restatement of ScientificGraphicDesign's design-theory section — a short,
  presentation-specific complement to it.
- **The rules, in one screen** — condensed numbered checklist, e.g.:
  1. Build every figure on an Illustrator artboard sized to match your
     slide's usable area — nothing gets rescaled/distorted on paste.
  2. Export **vector (PDF/SVG)** for Illustrator-editable/Beamer-native
     figures; **raster (PNG, correct DPI)** only for PowerPoint or photos.
  3. For animated reveals, build each frame as its own same-size artboard,
     export as one multi-page PDF, drive it with `\imageseq`.
  4. Start every talk from `JqiNanoBeamerTemplate` (as a GitHub template
     repo) — don't hand-roll a `.tex` preamble.
  5. Macro reference lives in the template's `MACRO_MANUAL.md` — this wiki
     doesn't duplicate it.
- **Pages table** — same format as `ScientificDataManagement`'s, one row per
  page in `Presentations/`.
- **What's in this repo** — the tree diagram above.
- **See also** — sibling wikis (`ScientificGraphicDesign`,
  `ScientificDataManagement`, `ScientificWriting`) + `JqiNanoBeamerTemplate`.

## Page content

### `01-Artboards.md` — Using the artboard

- Why one artboard = one figure, sized to the slide's usable area (matching
  `JqiNanoBeamerTemplate`'s content-frame dimensions so nothing gets
  rescaled on paste).
- Setting artboard size/units in Illustrator; naming artboards when a file
  holds many.
- One `.ai` file can hold many artboards (one per figure, or one per
  animation frame) — keeps a talk's visual assets in one versioned file.
- Pointer to `ScientificGraphicDesign/IllustratorsStyles` for color/type
  styles to apply on the artboard.

### `02-ExportingFigures.md` — Exporting figures

- **Vector** (PDF/SVG): default choice — scales infinitely, editable later,
  native to Beamer via `\includegraphics`. Export settings: artboard-only,
  no bleed, embed fonts vs. outline text.
- **Raster** (PNG): only when needed — photos, complex renders, or
  PowerPoint. Correct DPI for projector/print, transparent background.
- **PowerPoint note** (short): export raster at 2x display resolution;
  watch for cross-machine font substitution (ties back to the README
  forewords warning about PowerPoint portability).

### `03-Animations.md` — Making consistent animations

- Core technique: N same-size artboards = N frames.
- Export **Artboards → one multi-page PDF** (all artboards, in order) — the
  deliverable is one PDF, not individual PNGs.
- Wire into Beamer: `\imageseq[opts]{frames.pdf}{1:1, 2:2, 3:3-5, ...}` —
  short example, link to `MACRO_MANUAL.md#image-sequences` for full syntax.
- Tip: keep static background elements identical across artboards
  (copy-paste, don't redraw) so the reveal doesn't jitter.

### `04-UsingBeamer.md` — Using Beamer

- Start a new talk: use `JqiNanoBeamerTemplate` as a GitHub template repo
  (not a fork) — one repo per talk.
- Folder layout inherited from the template: `Figures/`, `Biblio.bib`,
  `main.tex`, compiled via `.latexmkrc`/`latexmk`.
- House-style highlights as teasers only (not full docs): `\good/\warn/\bad`,
  `\goodbox/\warnbox/\badbox`, `\twocol`, `\point`, `\imageseq`, `\citefoot`.
- **Full macro reference → `JqiNanoBeamerTemplate/MACRO_MANUAL.md`**
  (explicit link, not duplicated here).

### `Example-<TalkName>.md` — worked example (placeholder)

- Marked *(to fill in — pick a reference talk)*, same convention as
  `ScientificDataManagement`'s "in progress" tag.
- Structure to fill in once a reference talk is chosen: source `.ai` with
  its artboards → exported PDFs (static + multi-page animation) → where
  they land in the talk repo → relevant `.tex` snippets.

## Related fix (flagged, out of scope for this project)

`ScientificGraphicDesign/README.md` still links to `./SlideDeckFigures/`
and `./SlidesWithBeamer/`, which no longer exist in that repo. Once this
wiki exists, those two lines should point here instead. Not part of this
implementation — call out separately.

## Open item

The `Example-<TalkName>.md` page needs a real reference talk (source files
the user has on hand) before it can be written with real content; until
then it ships as a structured placeholder.
