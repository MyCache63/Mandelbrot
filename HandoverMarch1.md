# Mandelbrot Fractal Explorer — Handover March 1, 2026

## Project Overview

A single-file WebGL-powered fractal explorer (`mandelbrot.html`, ~1400 lines). No build tools, no dependencies — just open the HTML file in any modern browser. GPU-accelerated rendering means frames render in under 10ms.

**Repo:** https://github.com/MyCache63/Mandelbrot
**Branch:** `main`
**Safety tag:** `before-mandelbrot-feb27`

## Current State: Everything Working

All features below are committed and pushed to GitHub. The app opens and runs in any browser with WebGL support.

### Fractal Types (8 total)
1. **Mandelbrot** — classic z² + c
2. **Julia Set** — fixed c parameter, interactive picking from Mandelbrot view
3. **Burning Ship** — absolute value on both components before squaring
4. **Tricorn / Mandelbar** — complex conjugate before squaring
5. **Celtic** — absolute value on real part of z²
6. **Buffalo** — abs on both components, then abs on real of result
7. **Perpendicular Mandelbrot** — abs on real, negate imaginary
8. **Multibrot** — z^N + c with adjustable power slider (2.0 to 8.0, fractional)

### Navigation & Interaction
- **Rectangle zoom** — click and drag a box, release to zoom into that area (aspect-ratio locked)
- **Pan** — Cmd+drag (Mac), Ctrl+drag, or Shift+drag
- **Scroll wheel** — zoom toward cursor position
- **Double-click** — zoom in 3x at that point
- **Right-click** — zoom out 3x
- **Arrow keys** — pan the view
- **Touch support** — single-finger pan, two-finger pinch zoom
- **View history** — 50-level back stack (B key or Back button)
- **Go To Coordinates** — type exact Re, Im, zoom to jump there

### Color System
- **10 palettes**: Ocean Blues, Fire, Psychedelic, Ice, Gold & Blue, Neon, Grayscale, Emerald, Sunset, Vaporwave
- **Smooth coloring** — normalized iteration count, no banding (bailout radius 256)
- **Palette period** slider — controls how many iterations span one color cycle
- **Palette offset** slider — shifts the palette start point (`[` and `]` keys too)
- **Color cycling animation** — toggle with C key, adjustable speed
- **Interior color** — black, dark blue, or dark red
- All palettes use Inigo Quilez sinusoidal RGB method (no lookup tables)

### SavedBrots System
- **Save Brot** button opens a dialog to name the current view
- Saves to browser **localStorage** (persists between sessions)
- Downloads both a **.png screenshot** and a **.json parameter file**
- **SavedBrots dropdown** in the panel works like built-in presets
  - Select + Go to load, or double-click to load immediately
  - X button to delete with confirmation
  - Shows metadata (fractal type, palette, zoom level, date saved)
- **Export JSON** — download selected view (or current if none selected)
- **Export All** — download entire collection as JSON array
- **Import JSON** — load single or array of views from .json file
- JSON files are portable and contain all parameters (fractal type, coordinates, zoom, palette, iterations, Julia params, power, etc.)

### Built-in Presets (10 locations)
Seahorse Valley, Elephant Valley, Double Spiral, Mini Mandelbrot, Antenna, Tendrils, Julia Star, Julia Dendrite, Burning Ship detail, Tricorn Detail

### Other Features
- **Save PNG** — downloads current canvas as PNG
- **Copy URL** — encodes full view state as URL query params for sharing
- **Auto-iterations** — automatically increases max iterations as you zoom deeper
- **Keyboard shortcuts** — H=help, P=panel, C=cycle, R=reset, B=back, S=save, +/-=iterations, 1-8=fractal type
- **Info bar** — shows center coordinates, zoom level, iteration count, render time, cursor position in real-time

## Commit History
```
e10786e Add Cmd/Ctrl+drag to pan in addition to Shift+drag
976cdd1 Add SavedBrots system: save/load/export/import fractal views
35f15a7 Add handover doc
db5908d Initial Mandelbrot fractal explorer - single file HTML
```

## Technical Notes

- **WebGL fragment shader** — the iteration math runs on the GPU, one thread per pixel. This is 50-200x faster than CPU Canvas 2D rendering.
- **Single-precision float** — WebGL uses 32-bit floats. Zoom is limited to roughly 1e-13 before pixelation appears. This is a hard GPU limit.
- **Sinusoidal palettes** — all colors computed procedurally in the shader using `a + b * cos(2π(c*t + d))`. No texture lookups.
- **preserveDrawingBuffer: true** — required for Save PNG to work (canvas retains pixels after draw).

## Known Limitations
- Zoom depth limited to ~1e-13 (single-precision float). Beyond that, pixels become blocky.
- Color cycling adds palette offset per frame, so very long cycling sessions will accumulate floating-point drift (not practically noticeable).
- localStorage has a ~5MB limit per origin. With JSON saved brots this is effectively unlimited (each is <1KB), but something to know.
- No undo for deleted saved brots (once you confirm delete, it's gone from localStorage).

## Possible Next Steps

### Visual Enhancements
- **Orbit trap coloring** — track min distance to geometric shapes (point, line, circle, cross) during iteration. Produces stunning Pickover-stalk visuals and gives the interior interesting texture instead of solid black.
- **Distance estimation (DEM)** — track the derivative alongside z to estimate distance to the fractal boundary. Highlights ultra-fine filaments that escape-time coloring misses.
- **Interior coloring** — instead of solid black: period detection (color by orbit period length), BOF60 (min |z| across orbit reveals concentric rings), or triangle inequality average (smooth stripe patterns).
- **More palettes** — could add a custom palette editor with color stops, or let users define their own sinusoidal parameters (12 floats = infinite palettes).

### Deep Zoom
- **Emulated double precision** — store each coordinate as two floats ("double-float" technique). Extends usable zoom from 1e-7 to roughly 1e-14. Adds ~4x computation cost.
- **Perturbation theory** — compute one reference orbit at arbitrary precision in JavaScript, then compute all pixels as small deltas from that orbit in the GPU shader. Enables zooms to 1e-300+. Significantly more complex to implement.

### UI/UX Improvements
- **Thumbnail previews** in the SavedBrots dropdown — render a tiny canvas for each saved view.
- **Undo delete** for saved brots (keep a trash array in localStorage).
- **Render progress indicator** — for very high iteration counts, show a progress bar or spinner.
- **High-res export** — render off-screen at 2x or 4x resolution, then download. Good for printing or wallpapers.
- **Fullscreen mode** — hide all UI, just the fractal.
- **Mini-map** — small inset showing where you are in the full Mandelbrot while zoomed deep.

### Animation & Sharing
- **Zoom animation** — instead of instant jump, animate a smooth zoom into the rectangle selection.
- **Record zoom path** — save a sequence of keyframes and export as video or GIF.
- **GitHub Pages** — enable Pages on the repo so the explorer is live at `mycache63.github.io/Mandelbrot`.

### Additional Fractal Types
- **Phoenix fractal** — z(n+1) = z(n)² + Re(c) + Im(c)*z(n-1)
- **Magnet fractals** — based on renormalization group equations from physics
- **Newton fractals** — root-finding iteration, produces different visual style
- **Lyapunov fractals** — not escape-time based, but visually striking
- **3D Mandelbulb** — would require a completely different renderer (ray marching) but could be a separate mode

## Files
```
/Users/michaelashe/Projects/Mandelbrot/
├── mandelbrot.html      # The entire app (1400 lines, single file)
├── handover.md          # Original handover (now superseded by this file)
└── HandoverMarch1.md    # This file
```

## How to Run
Just open `mandelbrot.html` in any browser. That's it. No install, no build, no server needed.
