<div align="center">

  <h1>Fractal Explorer</h1>

  <p>
    <b>An interactive Python/Pygame fractal viewer for Mandelbrot and Julia sets, powered by vectorized NumPy escape-time rendering.</b>
  </p>

  <p>
    <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
    <img alt="Pygame" src="https://img.shields.io/badge/Pygame-0E7A3E?style=for-the-badge&logo=python&logoColor=white" />
    <img alt="NumPy" src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" />
    <img alt="Fractals" src="https://img.shields.io/badge/Fractals-FF006E?style=for-the-badge" />
    <img alt="Escape Time" src="https://img.shields.io/badge/Escape_Time-FFB703?style=for-the-badge" />
  </p>

  <p>
    <a href="#features">
      <img alt="Features" src="https://img.shields.io/badge/Features-00C2A8?style=for-the-badge" />
    </a>
    <a href="#demo-video">
      <img alt="Demo video" src="https://img.shields.io/badge/Demo_Video-FF3864?style=for-the-badge" />
    </a>
    <a href="#how-it-works">
      <img alt="How it works" src="https://img.shields.io/badge/How_It_Works-7C3AED?style=for-the-badge" />
    </a>
    <a href="#controls">
      <img alt="Controls" src="https://img.shields.io/badge/Controls-F59E0B?style=for-the-badge" />
    </a>
  </p>

</div>

---

## Overview

**Fractal Explorer** is a desktop visualizer for exploring the structure of Mandelbrot and Julia sets. It starts with a simple menu, lets you choose the fractal family, then renders a full-window `1280x720` view that you can pan and zoom.

The important part is the renderer: instead of calculating each pixel one-by-one in Python, it builds the whole complex plane with NumPy arrays and runs the escape-time loop across the full viewport.

## Demo Video

>i ll find some time for it one day :p

## Features

<table>
  <tr>
    <td><b>Mandelbrot rendering</b></td>
    <td>Renders the classic Mandelbrot set from a configurable complex-plane viewport.</td>
  </tr>
  <tr>
    <td><b>Julia rendering</b></td>
    <td>Renders Julia sets using a changing complex constant.</td>
  </tr>
  <tr>
    <td><b>Vectorized math</b></td>
    <td>Uses NumPy arrays instead of per-pixel Python function calls.</td>
  </tr>
  <tr>
    <td><b>Interactive movement</b></td>
    <td>Pan the viewport with arrow keys after rendering.</td>
  </tr>
  <tr>
    <td><b>Mouse zoom</b></td>
    <td>Left click zooms in, right click zooms out around the current center.</td>
  </tr>
  <tr>
    <td><b>Color-table rendering</b></td>
    <td>Maps escape counts into a compact RGB palette for visible iteration bands.</td>
  </tr>
  <tr>
    <td><b>Pygame UI</b></td>
    <td>Includes a lightweight menu with buttons for set selection and render start.</td>
  </tr>
</table>

## Tech Stack

<p>
  <img alt="Python" src="https://img.shields.io/badge/Python-1D4ED8?style=flat-square&logo=python&logoColor=white" />
  <img alt="Pygame" src="https://img.shields.io/badge/Pygame-16A34A?style=flat-square&logo=python&logoColor=white" />
  <img alt="NumPy" src="https://img.shields.io/badge/NumPy-2563EB?style=flat-square&logo=numpy&logoColor=white" />
  <img alt="Complex numbers" src="https://img.shields.io/badge/Complex_Numbers-9333EA?style=flat-square" />
  <img alt="Vectorized rendering" src="https://img.shields.io/badge/Vectorized_Rendering-F97316?style=flat-square" />
</p>

| Layer | Technology | Role |
| --- | --- | --- |
| Language | `Python` | Main application and fractal logic. |
| Window/UI | `pygame` | Window, event loop, buttons, surfaces, and display updates. |
| Math/rendering | `numpy` | Complex-plane generation, escape iterations, and RGB array mapping. |
| UI font | `font.ttf` | Optional custom font for button text. |

## Project Structure

```text
.
|-- app.py           # Main Pygame application, menu, controls, viewport state
|-- source.py        # Button class, Mandelbrot/Julia functions, render helpers
|-- requirements.txt # Python dependency list placeholder
|-- font.ttf         # Optional UI font used by the buttons
`-- README.md        # Project documentation
```

## Run Locally

Create and activate a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the packages used by the app:

```bash
pip install pygame numpy
```

Run from the repository root so `font.ttf` can be found:

```bash
python app.py
```

## Controls

| Input | Action |
| --- | --- |
| `mandelbrot` button | Select Mandelbrot mode. |
| `julia` button | Select Julia mode. |
| `start` button | Render the selected fractal. |
| Left arrow | Pan left. |
| Right arrow | Pan right. |
| Up arrow | Pan up. |
| Down arrow | Pan down. |
| Left mouse click | Zoom in. |
| Right mouse click | Zoom out. |
| Window close | Quit. |

## How It Works

### 1. Viewport

The app stores the visible area as four bounds:

| Bound | Meaning |
| --- | --- |
| `left` / `right` | Real-axis range. |
| `down` / `up` | Imaginary-axis range. |

Panning shifts those bounds. Zooming shrinks or expands the bounds around the current center.

### 2. Complex Plane

`source.py` builds a grid of complex numbers for the current viewport:

```python
real = np.linspace(left, right, width)
imag = np.linspace(bottom, top, height)
X, Y = np.meshgrid(real, imag)
C = X + 1j * Y
```

That gives the renderer one complex coordinate per screen pixel.

### 3. Escape-Time Iteration

For Mandelbrot, the renderer starts with `Z = 0` and repeatedly applies:

```python
Z = Z * Z + C
```

For Julia, the renderer starts from the pixel coordinate and uses a shared complex constant:

```python
C = -0.8 + (animation_time * 0.01) + 0.2j
Z = Z * Z + C
```

Pixels that remain within radius `2` keep iterating. The final iteration count becomes the color index.

### 4. Surface Rendering

The iteration result is mapped through a small RGB color table, transposed into Pygame's surface layout, converted with `pygame.surfarray.make_surface()`, and blitted to the screen.

## Why I Built This

math was always and will always be my most adored subject , especially when its abt imaginary numbers , and i ve long heard abt the marlbrot and julia sets and took this chance to get to know them more , fun project ngl(yes the hardware was a bottleneck ...)



## Current Status

| Area | Status |
| --- | --- |
| Mandelbrot rendering | Working |
| Julia rendering | Working |
| Menu selection | Working |
| Panning | Working |
| Mouse zoom | Working |
| Vectorized rendering | Working |
| Screenshot/export | Not implemented yet |

## Known Limitations

| Limitation | Current state |
| --- | --- |
| Rendering flow | Rendering is synchronous, so high iteration counts can pause the UI. |
| Zoom behavior | Zooms around the viewport center, not the exact mouse position. |
| Julia animation | The constant changes only when a render is triggered. |
| UI | Menu and controls are intentionally minimal. |
| Requirements file | `requirements.txt` exists but appears empty in this repo copy. |

## Roadmap Ideas

| Idea | Why it would help |
| --- | --- |
| Mouse-centered zoom | Make exploration more precise. |
| Screenshot export | Save interesting fractal views directly from the app. |
| Iteration slider | Let users trade speed for detail. |
| Palette picker | Add more visual styles without changing the math. |
| Async rendering | Keep the UI responsive during expensive renders. |
| Reset/back button | Return to the menu or reset the viewport without restarting. |

---

<div align="center">
  <sub>Built with Python, Pygame surfaces, NumPy arrays, and complex-plane math.</sub>
</div>
