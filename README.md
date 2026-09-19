# Image Dithering Studio

Turn any photo into a 1-bit, Game Boy, CGA or custom-palette image with real error-diffusion and ordered dithering — eleven kernels, nine palettes, and a PNG straight back out. Runs entirely in your browser, nothing is uploaded.

**Live:** <https://dither-studio.slippylabs.com/>

## What it does

- Error diffusion: Floyd–Steinberg, Atkinson, Jarvis–Judice–Ninke, Stucki, Burkes, Sierra (3 row, 2 row and Lite) and False Floyd–Steinberg, with an optional serpentine scan.
- Ordered dithering: Bayer 2×2, 4×4 and 8×8.
- Palettes: 1-bit, 4/8/16-level grayscale, Game Boy DMG, CGA/EGA 16, Commodore 64 16, 3-bit RGB, or an adaptive median-cut palette of 2–64 colours.
- Diffuse in linear light instead of sRGB, desaturate first, resize, and download the result as a PNG.

## How it works

The working buffer is float64 per channel: the whole point of error diffusion is that the error is a fraction of a level, and rounding back to bytes at each pixel would throw away exactly the quantity the algorithm exists to carry. Strength is capped at 1 — above that the loop feeds on itself and the buffer diverges to 10^12 within a few rows.

## Verification

Every kernel and palette was compared **pixel for pixel** against a reference implementation written from the published weights, across several test images and both scan directions — a wrong tap offset or a serpentine pass that forgot to mirror its kernel shows up as a handful of differing pixels, not as a visibly broken image. On top of that: palette closure and alpha preservation on every output, Bayer matrices checked to be permutations of 0..n²−1, and Pillow used as an outside opinion on both Floyd–Steinberg (block-average agreement) and median-cut palette quality.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/dither-studio.slippylabs.com.git
cd dither-studio.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
