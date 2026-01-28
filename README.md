### Camera Visualizers (GitHub Pages)

This repo includes **three standalone HTML tools** intended to be hosted on **GitHub Pages** (or run locally). They’re all client-side and load `three.js` from a CDN.

- **`index.html`**: launcher page with tabs (Single / Stereo)
- **`single_cam.html`**: single-camera calculator + 3D scene (FOV, resolution, depth-of-field, focus checks)
- **`stereo.html`**: stereo overlap visualizer + 3D scene (FOV, overlap region, image-space overlap)

---

### How to use (GitHub Pages)

- Enable GitHub Pages for the repository (Settings → Pages) and point it at the branch/folder containing these files.
- Open the Pages URL. Start at `index.html` (it embeds the other two tools in iframes).

**Deep links**
- `index.html#single` opens the Single Camera tab
- `index.html#stereo` opens the Stereo tab

---

### How to use (local)

Because these tools use ES modules and clipboard APIs, a local web server is strongly recommended (instead of opening the files via `file://`).

Example (Python):

```bash
python -m http.server 8000
```

Then open:
- `http://localhost:8000/index.html`

---

### `index.html` (Launcher)

- **Tabbed UI** that swaps between:
  - `single_cam.html`
  - `stereo.html`
- Uses iframes so each tool stays self-contained.

---

### `single_cam.html` (Single Camera + DOF)

**Inputs**
- Working distance, focal length, sensor size, pixel resolution
- 3D box size (width/depth/height)
- Depth-of-field controls:
  - f/#
  - Focus distance (or “lock to working distance”)
  - CoC modes:
    - **Auto (industrial)**: sensor diagonal / 1500
    - **2-pixel rule**: CoC = 2 × pixel pitch
    - Manual CoC (µm)

**Outputs (grouped “Results” panel)**
- FOV (physical and angular)
- Resolution in **mm/px** + min detectable feature (3 px)
- DOF: near/far, working range, hyperfocal, CoC used
- Box: fit-in-FOV, effective resolution at top/bottom, box distance span, in-focus YES/NO

**Render visualization**
- Camera frustum + WD plane
- Box in scene
- Focus plane
- DOF “in-focus” volume segment
- **View Panel** quick views (Top/Bottom/Left/Right/Front/Back/Iso)

**Buttons**
- **Copy Results**: copies the entire results section as a clean text block
- **Screenshot** (in View Panel): copies a PNG of the current render to clipboard (with fallback to download if clipboard image write isn’t allowed)

---

### `stereo.html` (Stereo Overlap)

**Inputs**
- Working distance, baseline, convergence
- Focal length, sensor width/height

**Outputs (same grouped “Results” style)**
- H-FOV / V-FOV
- Physical overlap width/height at the working plane
- Image-space overlap width/height (px; currently uses an assumed sensor resolution inside the script)

**Render visualization**
- Two camera bodies + rays to ground plane
- Overlap polygon rendered on the ground plane
- **View Panel** quick views (Top/Bottom/Left/Right/Front/Back/Iso)

**Buttons**
- **Copy Results**
- **Screenshot** (copies PNG to clipboard; falls back to download if required)

---

### Notes / Troubleshooting

- **Clipboard screenshot requires a secure context**:
  - Works on `https://` and usually on `http://localhost`
  - Often does **not** work on `file://` (browser will block clipboard image writes)
  - When blocked, the Screenshot button falls back to downloading a PNG.
- **“Copy Results”** should work broadly; it has a fallback for non-secure contexts.
- The tools load `three.js` from a CDN; offline use requires vendoring those dependencies.

