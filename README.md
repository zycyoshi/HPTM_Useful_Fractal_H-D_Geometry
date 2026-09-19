# HYPER_TERM · Software Overview

## One-line Summary

**HYPER_TERM is an interactive fractal and higher-dimensional geometry explorer covering 2D to 8D — 56 shapes in a single window, with real-time rotation, live parameter editing, and state saving.**

---

## Full Description

### Title

**HYPER_TERM**
*Fractal & Higher-Dimensional Geometry Explorer*

### Body

**HYPER_TERM** is a lightweight, interactive visualization tool for fractals and higher-dimensional geometry, running natively on Windows. It brings together **56 shapes** — from **classic 2D fractals** to the **8D hypercube** — into a single interface, letting you explore some of the most abstract structures in mathematics with nothing more than a mouse and keyboard.

**It has zero external dependencies.** Everything is drawn with Windows GDI. A single `.exe`, portable, no installation required.

---

## Core Features

### 1. 56 Shapes, Spanning 2D to 8D

| Category | Count | Examples |
|----------|-------|----------|
| Classic 2D fractals | 21 | Sierpinski Triangle / Carpet, Koch Snowflake, Dragon Curve, Mandelbrot, Julia, Newton Fractal, Barnsley Fern, Hilbert Curve, Peano Curve, Lévy C Curve, Apollonian Gasket, Flame Fractal |
| 3D geometry | 2 | 3D Menger Sponge, 3D Rotating Cube |
| 4D polytopes | 15 | Tesseract (4D Hypercube), 16-Cell, 24-Cell, 120-Cell, 600-Cell, 5-Cell, 8-Cell, Klein Bottle, Clifford Torus, Hopf Fibration, Hypersphere S³ |
| 5D – 8D | 12 | 5D/6D/7D/8D Hypercubes, 6D Cross-Polytope, 5D Simplex |
| Special geometry | 6 | Calabi–Yau Slice, 4D Hyperhelix, 4D Double Torus, 4D Dupin Torus |

Every shape uses a **standard vertex-generation algorithm** — for example, the 120-Cell uses golden-ratio vertices with even permutations and even sign combinations. Nothing is approximated.

### 2. Real-Time Interaction

- **Drag** to pan
- **Scroll wheel** to zoom (centered on cursor)
- **Auto-rotation** — every higher-dimensional object rotates continuously in 4D/5D/6D/7D/8D space at 20 FPS
- **Spacebar** to pause and inspect
- **Left panel list** to switch shapes with one click

### 3. Live Parameter Editing

The right panel lets you adjust in real time:

- **Depth** — recursion depth (Sierpinski, Menger Sponge, etc.)
- **Iterations** — complex-plane iteration count (Mandelbrot, Julia, Newton)
- **Julia parameters** — real part `a` and imaginary part `b`
- **Show Faces** — toggle between wireframe and solid faces
- **`.frac` Editor** — press `Ctrl+E` to open a dialog where you can edit every parameter directly, applied live

### 4. State Saving

- **`.frac` files** — save every parameter (shape, depth, rotation angle, view, Julia parameters) as a **plain-text file**
- **`Ctrl+S`** to save, **`Ctrl+O`** to load
- Format is `key=value`, openable and editable in Notepad
- GeoGebra-style archiving — ideal for recording research states

### 5. Image Export

- **`Ctrl+B`** or click **Save BMP** to export the current canvas as a 24-bit BMP
- Resolution follows the window size

---

## Technical Highlights

### 1. Pure Windows API, Zero Dependencies

- Uses only `windows.h`, `gdi32`, and `comdlg32`
- Compiles into a single `.exe` — **no DLLs, no runtime required**
- ~3,500 lines of C++

### 2. Multithreaded Rendering

- Complex-plane fractals (Mandelbrot, Julia, Newton) are split row-wise across threads using `CreateThread`
- Auto-detects CPU core count, up to 16 threads
- 900×720 resolution, 200 iterations — completed in tens of milliseconds

### 3. Standard Higher-Dimensional Geometry

- **120-Cell, 600-Cell** — standard vertex generation (golden ratio + even permutations + even signs); edge lengths compared with **relative error**
- **24-Cell** — 8 vertices `(±1,0,0,0)` + 16 vertices `(±1/2,±1/2,±1/2,±1/2)`, edge length² = 2
- **16-Cell** — 8 vertices `(±1,0,0,0)`, 16 triangular faces
- **Hypercubes** — N-dimensional hypercube with `2^N` vertices, edges connecting vertices that differ in exactly one binary digit

### 4. Smart Rendering

- **Double buffering** + `WS_CLIPCHILDREN` — no flicker
- **Depth sorting** — correct occlusion of near/far faces
- **Backface culling** — automatically removes faces pointing away from the camera
- **Depth shading** — distant faces darker, near faces brighter, giving a sense of depth
- **Progress bar** — appears only after 1 second, avoiding flashing on fast renders

### 5. Six-Direction Coloring

Each cube's 6 faces use 6 fixed colors (red, blue, green, yellow, purple, orange). Faces facing the same direction share the same color, so the result is visually coherent. Depth shading adds brightness variation.

---

## Interface Layout

```
┌─────────────────────────────────────────────────────────────┐
│ [✓] Show Faces          [Current: 120-Cell  depth 5]        │
├──────────────┬──────────────────────────┬───────────────────┤
│              │                          │                   │
│  Fractals    │                          │  Save BMP         │
│  ▸ Sierpinski│                          │  Reset View       │
│  ▸ Carpet    │                          │  Pause Anim       │
│  ▸ Koch      │      Canvas              │  Timeout: ON      │
│  ▸ Dragon    │                          │  Save .frac       │
│  ▸ Mandelbrot│      (real-time render)  │  Load .frac       │
│  ▸ Julia     │                          │  Edit .frac       │
│  ▸ Newton    │                          │                   │
│  ...         │                          │  Depth: [5]       │
│              │                          │  Iter:  [50]      │
│  High-D      │                          │  [Apply]          │
│  ▸ 4D Cube   │                          │                   │
│  ▸ 120-Cell  │                          │  Julia Params     │
│  ▸ 600-Cell  │                          │  Real: [-0.7]     │
│  ...         │                          │  Imag: [0.27]     │
│              │                          │  [Set Julia]      │
├──────────────┴──────────────────────────┴───────────────────┤
│ N/M next  +/- precision  R reset  / face  Ctrl+S/O/E/B      │
└─────────────────────────────────────────────────────────────┘
```

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `N` / `M` | Next / previous shape |
| `1`–`9`, `0`, `Q W E T Y A S D F G H J K Z X C V B` | Jump directly to a shape |
| `+` / `-` | Increase / decrease precision |
| `R` | Reset view |
| `/` | Toggle faces |
| `Space` | Pause / resume rotation |
| `Ctrl+S` | Save `.frac` |
| `Ctrl+O` | Load `.frac` |
| `Ctrl+E` | Edit `.frac` |
| `Ctrl+B` | Save BMP |
| Mouse wheel | Zoom |
| Left drag | Pan |

---

## Use Cases

### 1. Mathematics Education

- Demonstrate self-similarity in fractals (Sierpinski, Koch, Dragon Curve)
- Visualize complex-plane iteration (Mandelbrot, Julia)
- Introduce higher-dimensional geometry (4D hypercube, 120-Cell, 600-Cell)

### 2. Fractal Research

- Explore the Mandelbrot boundary; adjust iteration count to reveal detail
- Study morphological changes in Julia sets across parameter `c`
- Save key parameters to `.frac` files for reproducibility

### 3. Higher-Dimensional Visualization

- Observe projections of 4D polytopes (16-, 24-, 120-, 600-Cell)
- Watch 5D, 6D, 7D, 8D hypercubes rotate
- Understand the symmetry of higher-dimensional objects

### 4. Generative Art

- Produce unique fractal images by combining zoom, rotation, and parameter settings
- Export BMP for wallpapers, posters, cover art
- Share `.frac` files to reproduce and remix parameter sets

---

## System Requirements

| Item | Requirement |
|------|-------------|
| OS | Windows 7 / 8 / 10 / 11 |
| Architecture | x86 / x64 |
| RAM | 64 MB |
| Disk | 5 MB |
| GPU | Any (pure CPU rendering) |
| Runtime | None (statically compiled) |

---

## Build

Using **Dev-C++** or **MinGW-w64**:

```
g++ main.cpp -o hyperterm.exe -lgdi32 -lcomdlg32 -mwindows -O2
```

- `-lgdi32` — Windows GDI
- `-lcomdlg32` — file dialogs
- `-mwindows` — GUI subsystem (uses `WinMain`)
- `-O2` — optimization for faster rendering

---

## Design Philosophy

HYPER_TERM aims to be:

1. **Light** — one `.exe`, no dependencies, portable
2. **Complete** — 2D to 8D, 56 shapes in one place
3. **Fast** — multithreaded + in-memory bitmap, real-time interaction
4. **Exact** — standard vertex-generation algorithms, no approximations
5. **Easy** — live parameter editing, saveable state, GeoGebra-like workflow

---

## The Name

**HYPER_TERM** = **HYPER** (higher-dimensional) + **TERM** (terminal)

- **HYPER** — the software's focus is higher-dimensional geometry (4D–8D)
- **TERM** — a terminal-style interface, with neon cyan and magenta accents, evoking cyberpunk aesthetics

The name suggests: **a terminal for exploring higher-dimensional space.**

---

## Version

**HYPER_TERM v1.0**

- 56 shapes
- `.frac` file save / load / edit
- BMP export
- Multithreaded rendering
- Bilingual (Chinese / English) interface

---

## Roadmap

- [ ] Support 9D and 10D hypercubes
- [ ] Add 3D stereoscopic rendering (requires OpenGL)
- [ ] PNG / JPG export
- [ ] Parameter animation (animate a parameter over time)
- [ ] Batch rendering (generate many images with different parameters)
- [ ] Scripting interface (control rendering with a simple script)

---

**HYPER_TERM** — 2D to 8D. All in one window.
