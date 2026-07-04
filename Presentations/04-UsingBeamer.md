# 04 — Using Beamer

## Why this is a git repo, not a single file

A `.pptx` is a binary blob: git can tell you the file changed, but not *what* changed inside it, `git diff` shows nothing useful, and two people editing the same deck can't merge their changes — someone redoes work by hand. A `.tex` file is plain text: every edit is a real, readable diff, `git blame` tells you who changed which line and why, and if two people work on different frames of the same talk, git can merge those changes automatically. That compounds with the portability argument in the forewords above — plain text is also what turns a talk's version history into something actually useful, instead of a folder of `talk_v1.pptx`, `talk_v2_final.pptx`, `talk_v2_final_ACTUALLY.pptx`.

This wiki assumes you already know git basics (`clone`, `add`, `commit`, `push`, `pull`, branches, resolving a conflict). If you don't yet, [QuickStartGit](https://github.com/JQInanophotonics/QuickStartGit) is the group's on-ramp — it's written for true beginners and also covers syncing a repo like this one through Overleaf's Git integration, which is how the reference talk below was actually written (see its `Update on Overleaf` commit, below).

## Starting a new talk

Every talk gets its own repo, started from [JqiNanoBeamerTemplate](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate):

```bash
git clone https://github.com/JQInanophotonics/JqiNanoBeamerTemplate.git 2026-Talk-ShortName
cd 2026-Talk-ShortName
rm -rf .git && git init   # detach from the template's history, start fresh
```

(If/when the template repo is switched on as a GitHub "template repository", `Use this template` on GitHub does the same thing without the manual `git init` step.) The reference talk ([Example — CLEOus](Example-CLEOus.md)) follows exactly this pattern: its own git history starts with `First commit` / `Update on Overleaf`, then a long series of small, per-slide commits (`Add wafer-scale fabrication slide and figure assets`, `Update dicsussion`, `Fix typos`) — commit as you build slides, not once at the end, the same habit [ScientificDataManagement](https://github.com/JQInanophotonics/ScientificDataManagement/blob/main/PaperData/01-RepoSetup.md#commits) asks for on paper-data repos.

## Folder layout

```
2026-Talk-ShortName/
├── beamerthemeJQInanophotonics.sty   # the group's Beamer theme — don't edit per-talk
├── beamertheme/                      # logos, title-page art
├── Biblio.bib                        # your talk's references
├── Figures/                          # exported PDFs from Illustrator (incl. \imageseq sources)
│   └── raster/                       # photos, kept separate from vector figures
├── fonts/                            # theme fonts, embedded at compile time
├── .latexmkrc                        # latexmk config (lualatex, biber)
├── .gitignore                        # ignore *.aux, *.nav, *.snm, *.synctex.gz, etc.
└── main.tex                          # your talk — start here
```

`.gitignore` matters here specifically because Beamer/latexmk produces a lot of build byproducts (`.aux`, `.nav`, `.snm`, `.synctex.gz`, `.fdb_latexmk`, ...) alongside the real source — the template's `.gitignore` already excludes them, don't commit them by accident.

## Minimum viable `main.tex`

```latex
% !TEX program = lualatex
\documentclass[xcolor={HTML, dvipsnames}, onlytextwidth, 8pt, aspectratio=169]{beamer}
\usetheme{JQInanophotonics}
\addbibresource{Biblio.bib}

\title{Your Talk Title}
\author[LAST NAME ET AL.]{\textbf{Your Name}, Coauthor One, Coauthor Two}
\affiliation{Joint Quantum Institute, UMD/NIST}
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

This is the same shape as the reference talk's actual preamble — title, author list, affiliation, conference name/session, date, logo, title visual — nothing more is required to get a compiling deck with the house title page.

## Compiling

The template is set up for `lualatex` + `biber` via `latexmk` (see `.latexmkrc`: `$pdf_mode = 4; $bibtex_use = 2;`):

```bash
latexmk main.tex
```

This runs lualatex and biber as many times as needed and produces `main.pdf`. Re-run the same command after edits; `latexmk` only redoes the passes that are actually stale.

## House style, at a glance

- `\good{...}` / `\warn{...}` / `\bad{...}` — semantic inline highlighting (green/purple/orange), colors math too. Used constantly in the reference talk to mark a result as a win (`\good{single frequency grid}`) or a dead end (`\bad{Does not solve our metrology problem (yet)}`).
- `\goodbox{...}` / `\warnbox{...}` / `\badbox{...}` — the same, as a boxed callout, typically overlay-revealed (`\warnbox<+->[\textwidth][c][br]{...}`) to land a key statement.
- `\twocol[align]{leftW}{rightW}{left}{right}` — general two-column layout.
- `\twocolfig[align]{leftW}{rightW}{left}{right}` — the figure-slide variant of `\twocol`: it also raises the right column so a figure's top edge lines up with the frame title, instead of sitting a full title-height lower. This is what almost every content slide in the reference talk uses — text/equations on the left, an `\imageseq` figure on the right.
- `\point{text}` — bold section header that auto-advances the overlay.
- `\imageseq[opts]{file.pdf}{mapping}` — this template's shortcut over plain Beamer's `\only<N>{\includegraphics[page=N]{...}}` pattern, one line instead of one repeated block per frame; see [03 — Animations](03-Animations.md) for both. Leave `[opts]` empty in normal use — no `width=`/`scale=`; the artboard should already be the right size (see [01 — Artboards](01-Artboards.md#never-rescale-a-figure-once-its-placed)).
- `\citefoot{key}` — footnote citation from `Biblio.bib`, e.g. `\good{optical frequency synthesis}\footcite{DiddamsScience2020}`.
- `\thankyouslide{hero-image}{email}{references}` — the closing slide layout: a large hero figure, your contact email, and a two-column reference list, all laid out for you. The reference talk calls it once in `\appendix`, right after the last content frame.

Full parameter lists and every macro: [`MACRO_MANUAL.md`](https://github.com/JQInanophotonics/JqiNanoBeamerTemplate/blob/main/MACRO_MANUAL.md). Note: a talk's local copy of `beamerthemeJQInanophotonics.sty` can be ahead of what's documented there (`\twocolfig` and `\thankyouslide` above are two examples) — if a macro you see in a past talk's `.tex` isn't in the manual, grep the `.sty` directly for `\newcommand{\macroname}` to see its real definition.

## Where the answer to "how do I...?" almost always is

If it's about a macro's syntax, options, or troubleshooting — it's in `JqiNanoBeamerTemplate/MACRO_MANUAL.md` (or the `.sty` itself, per the note above), not here. This page only covers getting a talk repo up and compiling.

Next: [Example — CLEOus](Example-CLEOus.md)
