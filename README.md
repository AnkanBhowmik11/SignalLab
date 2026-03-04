# SignalLab v1.0 — Signal & Systems Plotter

**Author:** Ankan Bhowmik
**Version:** 1.0
**License:** MIT

A browser-based interactive tool for plotting, analyzing, and transforming continuous-time (CTS) and discrete-time (DTS) signals. Built for signals & systems coursework — no installation required, runs entirely in two HTML files with no frameworks or build step.

---

## Getting Started

Download both files and open `index.html` in any modern browser. No server, no dependencies, no install.

```
index.html        ← main plotter
opencanvas.html   ← open-ended drawing & equation canvas
```

---

## Interface Overview

The interface is split into two areas:

- **Left sidebar** — all controls: drawing tools, equation input, transforms, dual operations, view settings, and signal info
- **Right canvas area** — one or more graph panels that render your signals

At the top, the **CTS x(t) / DTS x[n]** toggle switches between continuous and discrete time modes. The **?** button in the top-right opens the help overlay. The **Open Canvas** button links to the standalone drawing page.

---

## Modes

### CTS — Continuous Time x(t)
Signals are plotted as smooth continuous curves. Drawing tools produce line segments and freehand strokes. Equations are evaluated over a real-valued range of t.

### DTS — Discrete Time x[n]
Signals are rendered as stem plots (vertical lines with circles at tips). Clicking on the canvas places individual samples at integer positions n. In the Eqn→Graph tab, DTS mode shows an array pattern input instead of the expression field.

---

## Tabs

### Draw Tab

Draw signals directly on the canvas using the following tools:

| Tool | Behaviour |
|------|-----------|
| **Line Seg** *(default)* | Click and drag to draw a straight line segment between two points |
| **Freehand** | Draw freely; the signal follows your mouse path left-to-right |
| **Ramp** | Click anywhere to instantly place a ramp signal r(t − t₀) starting at that x position |
| **Step** | Click anywhere to instantly place a unit step u(t − t₀) at that x position |
| **Impulse** | Click to place a scaled impulse δ(t − t₀); amplitude is read from where you click on the y-axis |
| **Erase** | Removes drawn content near the cursor |

**Multi-stroke drawing** — draw multiple non-overlapping strokes to build a piecewise signal. Each stroke is shown in a different color. Overlapping x-ranges are automatically rejected with a warning.

**Snap to integer** — when the cursor is near an integer grid position, it snaps and shows a gold crosshair indicator with coordinates.

**Undo** — removes the last placed stroke or DTS point.

**Clear All** — wipes the canvas completely.

**Analyze Signal** — derives the mathematical equation from your drawing. For CTS signals it produces a u(t)/r(t) decomposition (signals & systems standard form) plus a piecewise brace form. For DTS it produces a sum of scaled δ[n] impulses. The result is shown color-coded with a ⎘ copy button for the RHS.

---

### Eqn→Graph Tab

#### CTS Mode
Type any mathematical expression to plot it directly.

**Supported functions:**

| Expression | Meaning |
|------------|---------|
| `u(t)` | Unit step |
| `r(t)` | Unit ramp (t·u(t)) |
| `delta(t)` | Impulse (approximated) |
| `rect(t)` | Rectangle function |
| `tri(t)` | Triangle function |
| `sgn(t)` | Signum function |
| `sinc(t)` | Normalized sinc |
| `sin(x)`, `cos(x)` | Trig functions |
| `exp(x)` | Exponential (e^x) |
| `abs(x)` | Absolute value |
| `PI` | π ≈ 3.14159 |

**Signal chips** — click any chip to append it to the current expression with a `+` sign, building composite signals incrementally.

**t start / t end** — controls the evaluation range.

**Plot Equation** — renders the signal on the main canvas with a formatted equation display and ⎘ copy button.

#### DTS Mode — Array Pattern Input
Instead of a text expression, DTS mode shows a dedicated array input:

- **Values** — enter comma-separated sample values, e.g. `0, 1, 2, 1, 0`
- **Origin index** — specify which index in the array corresponds to n = 0
- **Live preview** — shows the array as a labeled sequence with the origin bolded and marked with ↑, and n-indices shown below each value
- **Plot Array** — renders the sequence as a stem plot and generates the δ[n] impulse sum equation automatically

---

### Transform Tab

Applies the transformation **A · x(at + b)** to the current signal (drawn or equation-plotted).

| Parameter | Effect |
|-----------|--------|
| **a** | Time scaling — compresses (a > 1) or expands (a < 1); negating reverses |
| **b** | Time shift — shifts left (b > 0) or right (b < 0) |
| **A** | Amplitude scaling — scales the output vertically |

**Quick Transform buttons:**

| Button | Transformation |
|--------|---------------|
| x(t+1) | Shift left by 1 |
| x(t−1) | Shift right by 1 |
| x(−t) | Time reversal |
| x(2t) | Time compression by 2 |
| x(t/2) | Time expansion by 2 |
| x(−t+1) | Flip and shift |

**Apply** — renders the transformed signal in a new panel below the main canvas, with the original dimmed for comparison. The transformed equation is displayed with a ⎘ copy button.

**Overlay Both** — shows original and transformed on the same axes.

---

### Dual Ops Tab

Perform operations between two signals. Choose an input mode first:

#### Eqn Form
Type expressions for x₁ and x₂ directly. Supports all CTS functions from the Eqn→Graph tab.

#### Draw Graph
Two separate canvases appear on the right. Use **Draw x₁** / **Draw x₂** to switch which canvas is active, then draw using the line segment tool (default). Snap-to-integer works on both canvases. Use **Undo** and **Clear Both** to manage strokes.

**Operations:**

| Operation | Result |
|-----------|--------|
| **Add** | y(t) = x₁(t) + x₂(t) |
| **Sub** | y(t) = x₁(t) − x₂(t) |
| **Multiply** | y(t) = x₁(t) · x₂(t) |
| **Convolve** | y(t) = x₁(t) ★ x₂(t) |

**Compute** — renders x₁, x₂, and result y in three separate panels with auto-scaled integer y-axes.

---

## Open Canvas

A standalone full-screen drawing environment accessible via the **Open Canvas** button in the header. Includes all drawing tools, an equation plotter supporting multiple named equations, per-equation color assignment, toggle/remove per equation, and the same snap-to-integer system. Use the **← Home** button to return to the main plotter.

---

## View Controls

| Control | Effect |
|---------|--------|
| **Zoom In / Out** | Adjusts x-range by ±2 units |
| **Reset** | Restores default range |
| **Grid** | Toggles background grid |
| **x range / y range** | Set custom axis extents and click Apply Range |

---

## Signal Info Panel

Always visible at the bottom of the sidebar. Shows live stats for the current signal: type (CTS/DTS), equation, max/min amplitude, DC value, energy estimate, and stroke or sample count.

---

## Export & Copy

| Button | Location | Action |
|--------|----------|--------|
| **↓ Export** | Graph header | Downloads canvas as PNG |
| **⎘ Copy Eq** | Graph header | Copies full equation to clipboard |
| **⎘** (inline) | Equation display boxes | Copies RHS only |

---

## Tips

- **Piecewise signals** — draw each piece as a separate stroke; Analyze stitches them into a single u(t)/r(t) expression.
- **Composite equations** — use chips to accumulate terms, then edit coefficients manually.
- **Copying from Draw to Eqn→Graph** — the copied equation is sanitized automatically; unicode minus and implicit multiplication like `2 u(t)` are handled correctly.
- **DTS array origin** — if your sequence is `[1, 2, 3, 2, 1]` and the center value `3` is at index 2, set origin to `2` so n=0 aligns to that sample.
- **Convolution range** — the result x-range is doubled automatically to fit the full convolution length.
- **Snap indicator** — the gold circle with crosshairs confirms the snapped integer coordinate before committing a stroke.

---

## Supported Browsers

Chrome, Firefox, Edge, Safari — any modern browser with HTML5 Canvas support. No internet connection required after fonts load (JetBrains Mono, DM Sans via Google Fonts).

---

## File Structure

```
index.html          ← main Signal & Systems plotter
opencanvas.html     ← standalone open canvas with equation plotter
README.md           ← this file
```

No frameworks. No build step. No backend. Open and use.

---

*SignalLab v1.0 — built by Ankan Bhowmik*
