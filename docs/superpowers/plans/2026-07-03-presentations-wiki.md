# ScientificPresentations Poor-Boy Wiki Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the `ScientificPresentations` poor-boy wiki: a `README.md` index plus five Markdown pages in `Presentations/`, teaching the group how to build consistent slide figures, animate them, and give talks in Beamer via `JqiNanoBeamerTemplate`.

**Architecture:** Plain Markdown, no build step, no separate wiki repo — mirrors `ScientificDataManagement`'s pattern (README as index/rules-summary, numbered content pages in a subfolder). Five content pages are written first (`01`–`04` plus one placeholder `Example` page), then the README is written last so its links target files that already exist, then every internal link in the repo is verified against the filesystem.

**Tech Stack:** Markdown only. No dependencies, no CI, no generated assets.

## Global Constraints

- No binary/boilerplate assets committed to this repo (spec: "Non-goals"). Illustrator assets live in `ScientificGraphicDesign`; the Beamer starting point lives in `JqiNanoBeamerTemplate`.
- No duplication of `JqiNanoBeamerTemplate/MACRO_MANUAL.md` macro syntax — link to it instead (spec: "Non-goals").
- No general graphic-design theory beyond a short pointer section (spec: "Non-goals") — that content lives in `ScientificGraphicDesign`'s README.
- Repo structure is fixed by the spec:
  ```
  ScientificPresentations/
  ├── README.md
  └── Presentations/
      ├── 01-Artboards.md
      ├── 02-ExportingFigures.md
      ├── 03-Animations.md
      ├── 04-UsingBeamer.md
      └── Example-TalkName.md
  ```
- The `Example-TalkName.md` page ships as a structured placeholder (spec: "Open item") — no real talk has been chosen yet.
- Working directory for all tasks: `/Users/greg/Documents/Lib/JQInanophotonics/ScientificPresentations`.

---

## Task 1: `Presentations/01-Artboards.md`

**Files:**
- Create: `Presentations/01-Artboards.md`

**Interfaces:**
- Produces: a page reachable at relative path `Presentations/01-Artboards.md`, linked from `README.md`'s Pages table and from `Presentations/02-ExportingFigures.md`'s trailing "Next" link (Task 2).
- Consumes: nothing (first content page).

- [ ] **Step 1: Create the directory and write the file**

Create `Presentations/01-Artboards.md` with this exact content:

```markdown
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
```

- [ ] **Step 2: Verify the file was written correctly**

Run: `grep -c '^#' Presentations/01-Artboards.md`
Expected: `5` (one `#` title + four `##` sections)

- [ ] **Step 3: Commit**

```bash
git add Presentations/01-Artboards.md
git commit -m "Add artboards page to presentations wiki"
```

---

## Task 2: `Presentations/02-ExportingFigures.md`

**Files:**
- Create: `Presentations/02-ExportingFigures.md`

**Interfaces:**
- Produces: a page reachable at relative path `Presentations/02-ExportingFigures.md`, linked from `README.md`'s Pages table, from Task 1's "Next" link, and from Task 3's "Next" link.
- Consumes: links back to `01-Artboards.md` (Task 1, already exists) and forward to `../README.md` and `03-Animations.md` (Task 3).

- [ ] **Step 1: Write the file**

Create `Presentations/02-ExportingFigures.md` with this exact content:

```markdown
# 02 — Exporting figures

## Vector first

Default to exporting **PDF** (or SVG) from Illustrator: `File > Export > Export As...` or `File > Save As... > Adobe PDF`. Vector figures scale to any size without quality loss, stay editable if you need to tweak them later, and drop straight into Beamer with `\includegraphics{figure.pdf}` — no format conversion needed.

Export settings that matter:
- **Artboard only, no bleed** — you want exactly the artboard's content, nothing outside it.
- **Embed fonts, or outline text** before export — a font missing on the compiling/rendering machine otherwise turns into missing glyphs.
- Export **one artboard per figure** as its own PDF, unless you're exporting an animation sequence (see [03 — Animations](03-Animations.md)), in which case you export all the frame-artboards together into a single multi-page PDF.

## Raster only when you have to

Use **PNG** when the content itself is raster — a photograph, a microscope image, a complex 3D render that doesn't simplify to vector — or when the destination can't take vector at all (PowerPoint).

- Export at a resolution that holds up on a projector: high enough that text and thin lines don't look soft when the slide fills the screen, but there's no benefit to going far beyond what the display can show.
- Keep the background **transparent** unless the figure is meant to sit on a solid panel.
- A raster figure does not rescale gracefully — export it once at final size, don't resize the PNG afterwards.

## If you're using PowerPoint

The forewords on the main [README](../README.md) explain why Beamer is the safer default. If you do need PowerPoint slides:
- Export figures as PNG at roughly **twice** the resolution you'd use for Beamer — PowerPoint decks get projected off more varied hardware, and you don't control the final render path the way a PDF guarantees.
- Embed fonts in the `.pptx` (`File > Options > Save > Embed fonts`) or stick to fonts you know will be installed on the podium computer — this is the single most common way a PowerPoint deck looks different at the conference than it did on your laptop.

Next: [03 — Animations](03-Animations.md)
```

- [ ] **Step 2: Verify the file was written correctly**

Run: `grep -c '^#' Presentations/02-ExportingFigures.md`
Expected: `4` (one `#` title + three `##` sections)

- [ ] **Step 3: Commit**

```bash
git add Presentations/02-ExportingFigures.md
git commit -m "Add exporting-figures page to presentations wiki"
```

---

## Task 3: `Presentations/03-Animations.md`

**Files:**
- Create: `Presentations/03-Animations.md`

**Interfaces:**
- Produces: a page reachable at relative path `Presentations/03-Animations.md`, linked from `README.md`'s Pages table, from Task 1's forward reference, from Task 2's "Next" link, and from Task 4's "Next" link.
- Consumes: links back to `../README.md` and forward to `04-UsingBeamer.md` (Task 4). References the external anchor `MACRO_MANUAL.md#image-sequences` in `JqiNanoBeamerTemplate` (verified by inspection in Task 7, not created by this plan).

- [ ] **Step 1: Write the file**

Create `Presentations/03-Animations.md` with this exact content:

````markdown
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
````

- [ ] **Step 2: Verify the file was written correctly**

Run: `grep -c '^#' Presentations/03-Animations.md`
Expected: `4` (one `#` title + three `##` sections)

- [ ] **Step 3: Commit**

```bash
git add Presentations/03-Animations.md
git commit -m "Add animations page to presentations wiki"
```

---

## Task 4: `Presentations/04-UsingBeamer.md`

**Files:**
- Create: `Presentations/04-UsingBeamer.md`

**Interfaces:**
- Produces: a page reachable at relative path `Presentations/04-UsingBeamer.md`, linked from `README.md`'s Pages table and from Task 3's "Next" link.
- Consumes: links forward to `Example-TalkName.md` (Task 5). References the external file `JqiNanoBeamerTemplate/MACRO_MANUAL.md` (verified by inspection in Task 7).

- [ ] **Step 1: Write the file**

Create `Presentations/04-UsingBeamer.md` with this exact content:

````markdown
# 04 — Using Beamer

## Starting a new talk

Every talk gets its own repo, started from [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate):

```bash
git clone https://github.com/JQInanophotonics/JqiNanoBeamerTemplate.git 2026-Talk-ShortName
cd 2026-Talk-ShortName
rm -rf .git && git init   # detach from the template's history, start fresh
```

(If/when the template repo is switched on as a GitHub "template repository", `Use this template` on GitHub does the same thing without the manual `git init` step.)

## Folder layout

The template gives you:

```
2026-Talk-ShortName/
├── beamerthemeJQInanophotonics.sty   # the group's Beamer theme — don't edit per-talk
├── beamertheme/                      # logos, title-page art
├── Biblio.bib                        # your talk's references
├── Figures/                          # exported PDFs from Illustrator (incl. \imageseq sources)
├── fonts/                            # theme fonts
├── .latexmkrc                        # latexmk config (lualatex, biber)
└── main.tex                          # your talk — start here
```

## Minimum viable `main.tex`

```latex
% !TEX program = lualatex
\documentclass[xcolor={HTML, dvipsnames}, onlytextwidth, 8pt, aspectratio=169]{beamer}
\usetheme{JQInanophotonics}
\addbibresource{Biblio.bib}

\titleboxw{1cm}
\title{Your Talk Title}
\author[LAST NAME ET AL.]{\textbf{Your Name}, Coauthor One, Coauthor Two}
\affiliation{Joint Quantum Institute UMD/NIST}
\conference{Conference Name}
\date{DD Month YYYY}
\logovisual{./beamertheme/Logos.png}
\titlepagevisual{beamertheme/RingGears_V5.png}

\begin{document}

\titlepage

\begin{frame}{First slide}
  Content here.
\end{frame}

\end{document}
```

## Compiling

The template is set up for `lualatex` + `biber` via `latexmk` (see `.latexmkrc`: `$pdf_mode = 4; $bibtex_use = 2;`):

```bash
latexmk main.tex
```

This runs lualatex and biber as many times as needed and produces `main.pdf`. Re-run the same command after edits; `latexmk` only redoes the passes that are actually stale.

## House style, at a glance

These are the macros you'll reach for most often — full parameter lists and every macro in [`MACRO_MANUAL.md`](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate/blob/main/MACRO_MANUAL.md):

- `\good{...}` / `\warn{...}` / `\bad{...}` — semantic inline highlighting (green/purple/orange), colors math too.
- `\goodbox{...}` / `\warnbox{...}` / `\badbox{...}` — the same, as a boxed callout.
- `\twocol[align]{leftW}{rightW}{left}{right}` — two-column slide layout.
- `\point{text}` — bold section header that auto-advances the overlay.
- `\imageseq[opts]{file.pdf}{mapping}` — animated figure reveals, see [03 — Animations](03-Animations.md).
- `\citefoot{key}` — footnote citation from `Biblio.bib`.

## Where the answer to "how do I...?" almost always is

If it's about a macro's syntax, options, or troubleshooting — it's in `JqiNanoBeamerTemplate/MACRO_MANUAL.md`, not here. This page only covers getting a talk repo up and compiling.

Next: [Example — a real talk](Example-TalkName.md)
````

- [ ] **Step 2: Verify the file was written correctly**

Run: `grep -c '^#' Presentations/04-UsingBeamer.md`
Expected: `6` (one `#` title + five `##` sections)

- [ ] **Step 3: Commit**

```bash
git add Presentations/04-UsingBeamer.md
git commit -m "Add using-beamer page to presentations wiki"
```

---

## Task 5: `Presentations/Example-TalkName.md`

**Files:**
- Create: `Presentations/Example-TalkName.md`

**Interfaces:**
- Produces: a page reachable at relative path `Presentations/Example-TalkName.md`, linked from `README.md`'s Pages table and from Task 4's "Next" link. This is an intentional placeholder (spec: "Open item") — no real talk has been picked yet.
- Consumes: links back to `../README.md#pages`.

- [ ] **Step 1: Write the file**

Create `Presentations/Example-TalkName.md` with this exact content:

```markdown
# Example — <TalkName> *(placeholder — to fill in)*

This page is meant to walk through one real talk end-to-end: the Illustrator file with its artboards, the exported figures and animation PDFs, and the `.tex` that assembles them — the same role [`PaperData/Example-OctaveSelfKIS.md`](https://github.com/JQInanophotonics/ScientificDataManagement/blob/main/PaperData/Example-OctaveSelfKIS.md) plays for `ScientificDataManagement`.

It isn't written yet because it needs a real reference talk with its source files at hand. When you pick one, this page should cover:

- **The source `.ai` file** — how many artboards, how they're named/ordered, which ones are static figures vs. animation frames.
- **The exports** — which artboards became single-page PDFs, which became one multi-page PDF for `\imageseq`, and where they landed in `Figures/`.
- **The `.tex`** — the actual frame(s) that use those figures, including the `\imageseq` mapping used for any animation.

Once a talk is chosen, replace this page's filename (`Example-TalkName.md`) with the real short name, and update the link to it in the main [README](../README.md#pages) table.
```

- [ ] **Step 2: Verify the file was written correctly**

Run: `grep -c '^#' Presentations/Example-TalkName.md`
Expected: `1`

- [ ] **Step 3: Commit**

```bash
git add Presentations/Example-TalkName.md
git commit -m "Add placeholder example page to presentations wiki"
```

---

## Task 6: `README.md`

**Files:**
- Modify: `README.md` (currently an untracked skeleton with empty section headers — full replacement)

**Interfaces:**
- Produces: the repo's index page. Links to all five pages created in Tasks 1–5, plus external links to `ScientificGraphicDesign`, `ScientificDataManagement`, `ScientificWriting`, and `JqiNanoBeamerTemplate`.
- Consumes: nothing new — this is the terminal task that ties the already-created pages together.

- [ ] **Step 1: Replace the file's content**

Replace the entire content of `README.md` with this exact content:

```markdown
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
| [Example — TalkName](Presentations/Example-TalkName.md) | Annotated walkthrough of a real talk *(placeholder — to fill in)* |

## What's in this repo

```
ScientificPresentations/
├── README.md
└── Presentations/
    ├── 01-Artboards.md
    ├── 02-ExportingFigures.md
    ├── 03-Animations.md
    ├── 04-UsingBeamer.md
    └── Example-TalkName.md
```

No binary assets live here. Illustrator templates and palettes are in [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign); the Beamer starting point is [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate).

## See also

Sibling wikis in the org: [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign), [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement), [ScientificWriting](https://github.com/JQInanophotonics/ScientificWriting). Beamer macros: [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate).
```

- [ ] **Step 2: Verify the file was written correctly**

Run: `grep -c '^#' README.md`
Expected: `6` (one `#` title + five `##` sections)

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "Write full README index for presentations wiki"
```

---

## Task 7: Verify all internal links resolve

**Files:**
- None created or modified — verification only. If broken links are found, fix them in the relevant file from Tasks 1–6 and amend that task's understanding (re-stage and commit as a fix).

**Interfaces:**
- Consumes: every file created in Tasks 1–6.
- Produces: confirmation that the wiki is internally consistent — the deliverable of this plan.

- [ ] **Step 1: Run a link-resolution check over every Markdown file**

Run this from the repo root:

```bash
python3 - <<'EOF'
import re, pathlib

root = pathlib.Path(".")
files = [root / "README.md"] + sorted((root / "Presentations").glob("*.md"))
link_re = re.compile(r"\[[^\]]*\]\(([^)]+)\)")

broken = []
for f in files:
    text = f.read_text()
    for target in link_re.findall(text):
        if target.startswith(("http://", "https://")):
            continue  # external links, not checked here
        path_part = target.split("#", 1)[0]
        if not path_part:
            continue  # pure in-page anchor
        resolved = (f.parent / path_part).resolve()
        if not resolved.is_file():
            broken.append((str(f), target, str(resolved)))

if broken:
    print("BROKEN LINKS:")
    for f, target, resolved in broken:
        print(f"  {f}: '{target}' -> {resolved} (missing)")
else:
    print(f"All internal links resolve OK across {len(files)} files.")
EOF
```

Expected output: `All internal links resolve OK across 6 files.`

If it reports broken links, open the listed file, fix the link target, save, and re-run this step until it passes.

- [ ] **Step 2: Manually confirm the two external cross-repo references are accurate**

These can't be checked by the script above since they point outside this repo. Confirm by reading the target file directly:

```bash
grep -n '^## Image Sequences' ../JqiNanoBeamerTemplate/MACRO_MANUAL.md
grep -n '^# 01 — Repository setup' ../ScientificDataManagement/PaperData/01-RepoSetup.md
```

Expected: both `grep` commands print a matching line (confirms `MACRO_MANUAL.md#image-sequences` anchor and the `ScientificDataManagement` example page referenced in `Presentations/Example-TalkName.md` both exist as claimed).

- [ ] **Step 3: Review the full page list renders sensibly**

Run: `find . -name '*.md' -not -path './docs/*' | sort`
Expected:
```
./Presentations/01-Artboards.md
./Presentations/02-ExportingFigures.md
./Presentations/03-Animations.md
./Presentations/04-UsingBeamer.md
./Presentations/Example-TalkName.md
./README.md
```

- [ ] **Step 4: Final commit (only if Step 1 required fixes)**

If no fixes were needed, skip this step — Task 6's commit is the final state. If fixes were made in Step 1:

```bash
git add -A
git commit -m "Fix broken internal links in presentations wiki"
```

---

## Self-Review Notes

- **Spec coverage:** Every section of the design doc (`docs/superpowers/specs/2026-07-03-presentations-wiki-design.md`) maps to a task — Forewords/Basic design/Rules/Pages table/Tree/See-also all in Task 6; the four content pages in Tasks 1–4; the placeholder Example page in Task 5; the flagged `ScientificGraphicDesign` link fix is explicitly out of scope per the spec and is not a task here.
- **Placeholder scan:** `Example-TalkName.md` (Task 5) is an intentional, fully-specified placeholder per the spec's "Open item" — it has complete content describing what it will contain, not a bare "TBD".
- **Type/interface consistency:** Link targets are consistent throughout — every page uses the same relative-path convention (`Presentations/...` from `README.md`, bare filename between sibling pages, `../README.md` from within `Presentations/`), verified mechanically in Task 7 rather than by inspection alone.
