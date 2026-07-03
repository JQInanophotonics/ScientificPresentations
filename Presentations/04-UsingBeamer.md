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
