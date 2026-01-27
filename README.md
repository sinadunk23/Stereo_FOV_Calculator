# Camera Visualizer (GitHub Pages)

This repo is meant to be hosted on **GitHub Pages**. The site entrypoint is **`index.html`** (tabbed UI).

## What it does

`index.html` contains two tabs:

- **Single Camera**: Visualizes a pinhole camera’s field-of-view (FOV) and shows how a centered 3D box fits within the FOV at different heights.
- **Stereo**: Visualizes two cameras and computes the overlap region on the ground plane, reporting overlap **width** and **height** (bounding box).

## Core math (pinhole model)

### Angular FOV (from sensor size + focal length)

Horizontal and vertical angular FOV:

$$
\mathrm{HFOV} = 2\arctan\left(\frac{s_w}{2f}\right), \quad
\mathrm{VFOV} = 2\arctan\left(\frac{s_h}{2f}\right)
$$

where:
- $f$ = focal length (mm)
- $s_w, s_h$ = sensor width/height (mm)

### FOV size at distance (working distance)

At a working distance $d$:

$$
\mathrm{FOV}_w(d) = \frac{s_w \cdot d}{f}, \quad
\mathrm{FOV}_h(d) = \frac{s_h \cdot d}{f}
$$

### Effective resolution (px/mm) vs distance

Given pixel resolution \(p_w, p_h\):

$$
\mathrm{px/mm}_w(d) = \frac{p_w}{\mathrm{FOV}_w(d)} = \frac{p_w f}{s_w d}, \quad
\mathrm{px/mm}_h(d) = \frac{p_h}{\mathrm{FOV}_h(d)} = \frac{p_h f}{s_h d}
$$

The **Single Camera** tab applies this at:
- **bottom of box**: $d = \mathrm{WD}$
- **top of box**: $d = \mathrm{WD} - y_{\text{top}}$

## Stereo overlap (ground plane)

Each camera’s frustum is projected to the ground plane as a quadrilateral. The overlap polygon is computed via **polygon clipping** (Sutherland–Hodgman). The UI reports:
- **Overlap width** = $ \max(x) - \min(x) $
- **Overlap height** = $ \max(z) - \min(z) $
