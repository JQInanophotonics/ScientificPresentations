# Example — CLEO US 2026 postdeadline talk

Annotated walkthrough of a real talk repo: *"Self-Alignment of an Integrated Parametrically Driven Cavity Soliton to its Octave-Separated Pumps for Universal Frequency Metrology"*, CLEO US 2026 Postdeadline Session III (SW3A.6), 20 May 2026 — a full slide deck built entirely on the workflow in [01](01-Artboards.md)–[04](04-UsingBeamer.md).

## Repo layout

```
2026-SLIDES-CLEO_PostDeadline-SelfAlignedPDCS/
├── 2026-SLIDES-CLEO_PostDeadline-SelfAlignedPDCS.tex   # 438 lines, the whole talk
├── beamerthemeJQInanophotonics.sty                     # local copy of the theme
├── beamertheme/                                        # logos, title-page art
├── Biblio.bib
├── fonts/
├── .latexmkrc
├── .gitignore
└── Figures/
    ├── PDCS_Concept.ai / .pdf                # 2 pages — the core idea, 2-step build
    ├── PDCS_DispersionConditions.ai / .pdf    # 3 pages
    ├── PDCS_FirstResults.ai / .pdf            # 3 pages
    ├── PDCS_MetrologyResults.ai / .pdf        # 6 pages
    ├── CombMetrology.ai / .pdf                # 9 pages — reused across 3 bullets, 1 slide
    ├── CombSWAP_Broadening.ai / .pdf          # 6 pages
    ├── DifferentSpeedRings.ai / .pdf          # 5 pages
    ├── WaferScale.ai / .pdf                   # 3 pages
    ├── ExperimentalDemonstration.ai / .pdf    # 5 pages
    ├── Conclusion.ai / .pdf                   # 3 pages
    ├── Acknowledgment.ai / .pdf               # 1 page — opening "team" slide
    ├── Acknowledgment_end.pdf                 # different crop — closing thank-you slide
    ├── Figure_SupMat_*.pdf                    # backup/appendix slides, single-page
    ├── Figure1.ai, Figure2.ai, ...            # early drafts, never exported — left as-is
    └── raster/                                # lab photos (.jpg), not all wired into a frame
```

Eleven of the twelve main-text figures are `.ai`/`.pdf` pairs named after the physics they show, exactly the convention from [01 — Artboards](01-Artboards.md). Multi-page ones (2–9 pages) are animation sequences built from same-size artboards, per [03 — Animations](03-Animations.md).

## The preamble

```latex
% !TEX program = lualatex
\documentclass[xcolor={HTML, dvipsnames}, onlytextwidth, 8pt, aspectratio=169]{beamer}
\usepackage{emoji}
\usepackage{hyphenat}
\usetheme{JQInanophotonics}
\addbibresource{Biblio.bib}

\title{Self-Alignment of an Integrated Parametrically Driven Cavity Soliton to its Octave-Separated Pumps for Universal Frequency Metrology}
\author[G.~MOILLE ET AL.]{\textbf{Gr\'egory Moille}, P.~Shandilya, J.~Stone, ...}
\affiliation{Joint Quantum Institute, UMD/NIST}
\conference{CLEO US - Postdeadline Session III - SW3A.6}
\date{20 May 2026}
\logovisual{./beamertheme/Logos.png}
\titlepagevisual{beamertheme/RingGears_V5.png}
```

Matches the minimum-viable skeleton in [04 — Using Beamer](04-UsingBeamer.md) exactly — a real talk doesn't need more than this to get the house title page.

## A content slide, in full

The "First χ⁽³⁾ PDCS demonstration" slide shows the whole pattern together: `\twocolfig` for layout, `\onslide`/overlay-revealed bullets, a `\goodbox`/`\badbox` pair for the takeaway, and `\imageseq` for the figure:

```latex
\begin{frame}{First $\chi^{(3)}$ PDCS demonstration}
  \twocolfig[T]{0.3}{0.7}{
    \onslide<+->{\textbf{Degenerate optical parametric oscillation (OPO) to drive the cavity soliton}}
    \begin{itemize}[<.->]
      \item Energy/momentum conservation from dispersion engineering
      \item Only possible in integrated microrings — fiber dispersion control insufficient
    \end{itemize}
    \onslide<.->{\textbf{Integrated $\chi^{(3)}$ PDCS demonstration}\footcite{MoilleNat.Photon.2024}:}
    \begin{itemize}
      \item 23~{\textmu}m SiN microring made in foundry
      \item Pumped at 1550/950 nm
    \end{itemize}
    \goodbox<+->[\textwidth][l][br]{\bfseries New on-chip soliton class}
    \badbox<.->[\textwidth][l][br]{\bfseries
      Not readily useful:
      \begin{enumerate}
        \item Does not span an octave
        \item Made of 3 microcombs: the 2 pumps are not teeth of the PDCS
      \end{enumerate}
    }
  }
  {
    \centering
    \imageseq{./Figures/PDCS_FirstResults.pdf}{2:1,3:2-}
  }
\end{frame}
```

Note the `0.3`/`0.7` split — this is the layout that fixed `PDCS_FirstResults.ai`'s artboard size to 297×222 pt (see [01 — Artboards](01-Artboards.md)), and `2:1,3:2-` skips page 1 of the artboard set (used as a cold-open image before the frame's overlays start) and maps pages 2–3 onto overlays 1 and 2-onward.

## Bookend figures: the same idea, two crops

`Acknowledgment.pdf` (1 page, 432×200 pt) opens the talk on a plain "Team work" frame:

```latex
\begin{frame}{Team work}
  \centering
  \includegraphics{./Figures/Acknowledgment.pdf}
\end{frame}
```

`Acknowledgment_end.pdf` (432×117 pt — same width, shorter crop) closes it, fed into the house `\thankyouslide` macro instead of a plain frame:

```latex
\thankyouslide{./Figures/Acknowledgment_end.pdf}{gmoille@umd.edu}{%
  \textbf{\large More information in \href{https://arxiv.org/abs/2602.05151}{our preprint: arXiv:2602.05151 (2026)}}\par
  ...two-column reference list...
}
```

Two separate exports from the same source artwork, cropped for two different slots (a full frame vs. `\thankyouslide`'s hero-image region) — not one image awkwardly reused at the wrong aspect ratio in both places.

## Backup slides

Five `Figure_SupMat_*.pdf` files back five appendix frames after `\appendix` and the bibliography, each a plain single-page `\includegraphics`, no `\imageseq` needed since they don't build up:

```latex
\begin{frame}{Complete setup}
  \centering
  \includegraphics[width = \textwidth]{./Figures/Figure_SupMat_FullSetup.pdf}
\end{frame}
```

Backup material doesn't need the same animation treatment as the main narrative — reserve `\imageseq` for slides where the reveal is actually part of telling the story.
