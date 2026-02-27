# Mandelbrot Fractal Explorer - Handover

## Current State
Single-file HTML fractal explorer (`mandelbrot.html`) is complete and committed. Opens in any browser.

## What's Working
- **WebGL-powered rendering** - GPU-accelerated, renders in milliseconds
- **8 fractal types**: Mandelbrot, Julia Set, Burning Ship, Tricorn/Mandelbar, Celtic, Buffalo, Perpendicular Mandelbrot, Multibrot (variable power)
- **Rectangle zoom**: Click and drag to draw a box, releases zooms into that area (aspect-ratio constrained)
- **Color cycling**: Toggle with C key or checkbox, 10 palettes with adjustable speed
- **10 color palettes**: Ocean, Fire, Psychedelic, Ice, Gold & Blue, Neon, Grayscale, Emerald, Sunset, Vaporwave
- **Smooth coloring**: No banding, uses normalized iteration count
- **Pan**: Shift+drag to pan the view
- **Scroll wheel zoom**: Zooms toward cursor position
- **Double-click**: Zoom in 3x at point
- **Right-click**: Zoom out 3x
- **Touch support**: Single-finger pan, two-finger pinch zoom
- **10 preset locations**: Seahorse Valley, Elephant Valley, Double Spiral, Mini Mandelbrot, Antenna, Tendrils, Julia Star, Julia Dendrite, Burning Ship detail, Tricorn detail
- **Julia pick mode**: Click any point on Mandelbrot to see its Julia set
- **URL sharing**: Copy URL button encodes current view state
- **Save PNG**: Download current view as image
- **Keyboard shortcuts**: H=help, P=panel, C=cycle, R=reset, B=back, S=save, +/-=iterations, arrows=pan, [/]=palette shift, 1-8=fractal type
- **Auto-iterations**: Automatically increases max iterations as you zoom deeper
- **View history**: Back button with 50-level history stack
- **Info bar**: Shows center coordinates, zoom level, iteration count, render time, cursor position

## Build Status
Single HTML file, no build needed. Just open in browser.

## Key Decisions
- Used WebGL fragment shaders for performance (50-200x faster than Canvas 2D)
- Single-precision float limits zoom to ~1e-13 before pixelation
- Sinusoidal RGB palettes (Inigo Quilez method) - no lookup tables needed
- Bailout radius of 256 for smooth coloring quality

## Next Steps (if requested)
- Orbit trap coloring modes
- Distance estimation for boundary highlighting
- Interior coloring (period detection, BOF60)
- Emulated double precision for deeper zooms
- High-resolution render/export option
