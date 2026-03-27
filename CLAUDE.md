# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

"Duck Pond" — a single-file HTML5 interactive visualization (`pond.html`) combining WebGL water shaders with 2D canvas duck animation. No build system, no dependencies. Open directly in a browser.

## Running

```bash
open pond.html
```

No build, lint, or test commands — this is a standalone HTML file.

## Architecture

Two layered fullscreen canvases render independently:

1. **WebGL canvas** (`glCanvas`): Procedural water surface using Perlin noise / fBm in fragment shaders, with grain and vignette post-processing.
2. **2D canvas** (`duckCanvas`): Duck sprites, ripple effects, and shadows rendered via Canvas API on top of the water.

### Duck Simulation

- 3–5 ducks spawn at init, each either white or mallard type
- State machine with three states: `float` (drift), `paddle` (active swim), `washing` (stationary jitter)
- State transitions every 3–8 seconds; ripples and V-wake spawn during paddling
- Movement uses angle lerping, boundary steering, and sine-wave bobbing

### Rendering Pipeline

A single `requestAnimationFrame` loop updates WebGL uniforms (resolution, time), processes ripple/duck state, and draws both layers at 60fps. Window resize is handled automatically.
