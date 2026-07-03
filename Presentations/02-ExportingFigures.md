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
