# 02 — Exporting figures

## Vector first, and keep the source next to the export

Default to exporting **PDF** from Illustrator: `File > Export > Export As...` or `File > Save As... > Adobe PDF`. Vector figures scale to any size without quality loss, stay editable if you need to tweak them later, and drop straight into Beamer with `\includegraphics{figure.pdf}` — no format conversion needed.

The reference talk ([Example — CLEOus](Example-CLEOus.md)) keeps every `.ai` source sitting right next to its exported `.pdf` in `Figures/`, same basename: `WaferScale.ai` / `WaferScale.pdf`, `PDCS_Concept.ai` / `PDCS_Concept.pdf`, and so on. `.tex` only ever references the `.pdf`; the `.ai` is there so a figure can be reopened and re-exported without hunting for it elsewhere. It's normal for a few `.ai` files to end up without a matching export (`Figure1.ai`, `Figure2.ai`, `CEcomparison.ai` in the reference talk have none) — early drafts or cut ideas. Leave them; don't rename them into the numbered sequence used elsewhere, since that numbering convention is for finished, in-use figures.

Export settings that matter:
- **Artboard only, no bleed** — you want exactly the artboard's content, nothing outside it.
- **Embed fonts, or outline text** before export — a font missing on the compiling/rendering machine otherwise turns into missing glyphs. The reference talk ships its own `fonts/` folder in the talk repo for exactly this reason (matching what Illustrator embeds).
- Export **one artboard per figure** as its own single-page PDF, unless you're exporting an animation sequence (see [03 — Animations](03-Animations.md)), in which case you export all the frame-artboards together into a single multi-page PDF — that's how `CombMetrology.pdf` ends up with 9 pages and `WaferScale.pdf` with 3.

## Raster only when you have to

Use **PNG/JPG** when the content itself is raster — a photograph, a microscope image, a complex 3D render that doesn't simplify to vector — or when the destination can't take vector at all (PowerPoint). Keep raster assets out of the main `Figures/` listing so it isn't a mix of vector figures and photos: the reference talk keeps them in `Figures/raster/` (lab photos like `P9290629_whiteBG.jpg`, `IDG_20250910_112247_907.jpg`). A raster file sitting in that subfolder doesn't have to be wired into a frame yet — it's fine to keep candidate photos there even if only some end up used.

- Export at a resolution that holds up on a projector: high enough that text and thin lines don't look soft when the slide fills the screen, but there's no benefit to going far beyond what the display can show.
- Keep the background **transparent** unless the figure is meant to sit on a solid panel. `P9290629_whiteBG.jpg` in the reference talk is a deliberately background-cleaned version of `P9290629.jpg` — kept as a separate file rather than overwritten, so the original photo is still there if the white-background version needs redoing.
- A raster figure does not rescale gracefully — export it once at final size, don't resize the file afterwards.

## If you're using PowerPoint

The forewords on the main [README](../README.md) explain why Beamer is the safer default. If you do need PowerPoint slides:
- Export figures as PNG at roughly **twice** the resolution you'd use for Beamer — PowerPoint decks get projected off more varied hardware, and you don't control the final render path the way a PDF guarantees.
- Embed fonts in the `.pptx` (`File > Options > Save > Embed fonts`) or stick to fonts you know will be installed on the podium computer — this is the single most common way a PowerPoint deck looks different at the conference than it did on your laptop.

Next: [03 — Animations](03-Animations.md)
