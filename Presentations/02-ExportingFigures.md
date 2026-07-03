# 02 — Exporting figures

## Vector first, and keep the source next to the export

Default to exporting **PDF** from Illustrator: `File > Export > Export As...` or `File > Save As... > Adobe PDF`. Vector figures scale to any size without quality loss, stay editable if you need to tweak them later, and drop straight into Beamer with `\includegraphics{figure.pdf}` — no format conversion needed.

The reference talk ([Example — CLEOus](Example-CLEOus.md)) keeps every `.ai` source sitting right next to its exported `.pdf` in `Figures/`, same basename: `WaferScale.ai` / `WaferScale.pdf`, `PDCS_Concept.ai` / `PDCS_Concept.pdf`, and so on. `.tex` only ever references the `.pdf`; the `.ai` is there so a figure can be reopened and re-exported without hunting for it elsewhere. It's normal for a few `.ai` files to end up without a matching export (`Figure1.ai`, `Figure2.ai`, `CEcomparison.ai` in the reference talk have none) — early drafts or cut ideas. Leave them; don't rename them into the numbered sequence used elsewhere, since that numbering convention is for finished, in-use figures.

Export settings that matter:
- **Artboard only, no bleed** — you want exactly the artboard's content, nothing outside it.
- **Embed fonts, or outline text** before export — a font missing on the compiling/rendering machine otherwise turns into missing glyphs. The reference talk ships its own `fonts/` folder in the talk repo for exactly this reason (matching what Illustrator embeds).
- Export **one artboard per figure** as its own single-page PDF, unless you're exporting an animation sequence (see [03 — Animations](03-Animations.md)), in which case you export all the frame-artboards together into a single multi-page PDF — that's how `CombMetrology.pdf` ends up with 9 pages and `WaferScale.pdf` with 3.

## Keep figures light — your talk repo likely syncs through Overleaf's git bridge

If you compile on Overleaf and sync it with the talk's GitHub repo (see [04 — Using Beamer](04-UsingBeamer.md) for why a talk repo is a real git repo in the first place), two different numbers apply, and they're not the same kind of limit ([plan limits](https://docs.overleaf.com/getting-started/free-and-premium-plans/plan-limits), [project limits](https://docs.overleaf.com/on-premises/support/project-limits)):

- **The hard limit: 50 MB on any individual file.** This is enforced — a figure over that will not upload or sync, full stop. This is the wall; design around never getting near it.
- **A recommendation, not an enforced wall: keep total project size under 100 MB when using Git/GitHub sync** (vs. 500 MB for a project with no git integration). Overleaf doesn't block you at 100 MB, but this is where a git-synced project starts getting slow and flaky — treat it as the real working ceiling for a talk repo, even though nothing errors out at exactly that number.

A dozen or so figures well under a few MB each, per the reference talk's own `Figures/` folder, stays comfortably inside that 100 MB working ceiling; one bloated 80 MB figure could blow through it on its own even though it's still under the 50 MB hard limit. If a figure is unexpectedly large, the bitmap inside it likely isn't rasterized down to what the plot actually needs (see [01 — Artboards](01-Artboards.md) and the note below on linked vs. embedded bitmaps).

- **Link raster into Illustrator, don't embed it.** A linked bitmap keeps the `.ai` file itself small and responsive; Illustrator embeds the linked raster data into the PDF automatically at export time, so you don't lose anything at publication/talk time. (Uncheck "Create PDF Compatible File" in Illustrator's save options, or every save silently embeds a preview copy of every linked image into the `.ai` file too.)
- This is also the same rule as [01 — Artboards, "Never rescale a figure once it's placed"](01-Artboards.md#never-rescale-a-figure-once-its-placed), from the other direction: an oversized source bitmap and a rescaled placement tend to go together — if you're not placing a figure at native size, it's easy to not notice the file is carrying more raster data than the slide will ever show.

## Raster only when you have to

Use **PNG/JPG** when the content itself is raster — a photograph, a microscope image, a complex 3D render that doesn't simplify to vector — or when the destination can't take vector at all (PowerPoint). Keep raster assets out of the main `Figures/` listing so it isn't a mix of vector figures and photos: the reference talk keeps them in `Figures/raster/` (lab photos like `P9290629_whiteBG.jpg`, `IDG_20250910_112247_907.jpg`). A raster file sitting in that subfolder doesn't have to be wired into a frame yet — it's fine to keep candidate photos there even if only some end up used.

- Export at a resolution that holds up on a projector: high enough that text and thin lines don't look soft when the slide fills the screen, but there's no benefit to going far beyond what the display can show.
- Keep the background **transparent** unless the figure is meant to sit on a solid panel. `P9290629_whiteBG.jpg` in the reference talk is a deliberately background-cleaned version of `P9290629.jpg` — kept as a separate file rather than overwritten, so the original photo is still there if the white-background version needs redoing.
- A raster figure does not rescale gracefully — export it once at final size, don't resize the file afterwards.

## If you're using PowerPoint

The forewords on the main [README](../README.md) explain why Beamer is the safer default. But the artboard/no-rescale rules from [01 — Artboards](01-Artboards.md) apply just as much here — PowerPoint just makes it easier to break them by accident:
- Export figures as PNG at roughly **twice** the resolution you'd use for Beamer — PowerPoint decks get projected off more varied hardware, and you don't control the final render path the way a PDF guarantees.
- **Insert at native size, then leave it alone.** `Insert > Pictures` drops the image in at 100%; don't drag a corner handle to "make it fit" the slide — that's the same rescaling mistake as `\includegraphics[width=...]` in Beamer, and it shrinks or grows any text baked into the figure right along with it. If it doesn't fit the slot, go back to Illustrator, resize the artboard, and re-export — check the exact placed size afterwards in `Format Picture > Size` (height/width fields), not by eye.
- Embed fonts in the `.pptx` (`File > Options > Save > Embed fonts`) or stick to fonts you know will be installed on the podium computer — this is the single most common way a PowerPoint deck looks different at the conference than it did on your laptop.
- PowerPoint has no `\imageseq` equivalent for the animation technique in [03 — Animations](03-Animations.md). The same same-size-artboards idea still applies — export each frame as its own PNG at identical pixel dimensions, then either place one per slide (duplicate the slide, swap the picture) or stack all of them on one slide and reveal them one at a time with `Animations > Appear`/`Disappear` in the same order they'd occupy `\imageseq` overlays.

Next: [03 — Animations](03-Animations.md)
