# VERTASCAN VIZION™

**Advanced WebGL architectural visualization and look development, in a single file.**

One HTML file. No build step, no dependencies to install, no asset folder. Open it and
you are inside a working architectural render environment — geographic sun study,
physically sampled depth of field, procedural PBR materials, DaVinci-style grading and
print-resolution stills.

```
vizion.html   ~167 KB   Three.js r170   WebGL2
```

---

## Quick start

1. Download `vizion.html`.
2. Open it in any modern browser — or upload it to GitHub Pages by **drag-and-drop**.
3. Press `H` for the on-screen reference sheet.

The sandbox opens empty on purpose: sky, a measured grid, an invisible plane that
catches shadows, and a survey plate with a true-north arrow. Drop a model on it, or
switch **Ground → Procedural site** for the generated terrain, road, context and scatter.

---

## The sandbox

| Ground mode | What you get |
|---|---|
| **Grid and shadow plane** *(default)* | Sky, a measured grid with north and east axes, and an invisible shadow catcher |
| **Shadow plane only** | Clean studio floor for product-style stills |
| **Procedural site and terrain** | Displaced terrain, site paving, road, context blocks and scatter |
| **Sky only** | Nothing but the atmosphere |

The grid is analytic — line weight widens with distance rather than relying on shader
derivatives, so it stays stable under accumulation jitter. The gold axis runs north, the
blue axis runs east, and both track the True North setting the sun study uses.

Nothing else changes between modes. Sun study, sky, weather, materials, camera, grading,
scopes, shot generation and export all work identically on an imported model in an empty
sandbox as they do on a generated building.

## The six workspaces

| | Workspace | What it is for |
|---|---|---|
| `1` | **LOOK DEV** | Material authoring, surface response, environment lighting |
| `2` | **SCENE** | Typology, site, terrain, scatter, model import |
| `3` | **CAMERA** | Lens, aperture, focus, navigation, composition, shot list |
| `4` | **RENDER** | Quality mode, output resolution, batch render, export |
| `5` | **POST** | Grading, optics, scopes, node chain |
| `6` | **PRESENT** | Boards, contact sheets, microsite, project files |

---

## Rendering

The renderer is built around **progressive accumulation**. Every idle frame adds a
sample, and three things are jittered per sample — which means image quality converges
toward offline-render character while you sit still and look at it.

| Jitter | Converges to |
|---|---|
| Sub-pixel projection offset (Halton 2,3) | Clean analytic anti-aliasing |
| Camera origin across the aperture disc, re-aimed at the focal plane | Physically sampled depth of field with real bokeh |
| Sun direction within its true angular diameter | Genuine soft shadows and penumbra |

The full chain, in linear HDR throughout:

```
scene (half-float + depth)
  → SSAO           depth-only, 12 taps, rotated per sample
  → accumulation   ping-pong progressive average
  → bloom          threshold + 5-level down/upsample
  → composite      chromatic aberration → sharpen → exposure → light rays
                   → bloom + lens dirt → halation → white balance → tone map
                   → lift/gamma/gain → contrast → saturation → curve
                   → vignette → grain → sRGB
```

**Four quality modes** — Realtime, Hybrid, Path-accumulated and Ultra — plus print
scaling up to 6× for stills large enough for board presentation.

---

## Sun study

Real NOAA solar position: equation of time, obliquity of the ecliptic and atmospheric
refraction, computed from latitude, longitude, date, local time and true north. Twelve
city presets, or enter coordinates directly.

Verified against reference values:

| Case | VIZION | Reference |
|---|---|---|
| Toronto, 21 Jun, solar noon | 69.79° altitude / 180.5° azimuth | 69.8° / due south |
| Toronto, 21 Dec, solar noon | 22.94° altitude | ≈ 23.2° |
| Toronto daylight, 21 Jun / 21 Dec | 15.40 h / 8.85 h | 15.4 h / 8.9 h |
| Sydney, 21 Dec, noon | 79.46° altitude / 351° azimuth | high, due north |

The signature element of the interface is a true **polar sun-path diagram** for the
chosen site, with June and December solstice arcs drawn as ghosts and a bead tracking
the current hour. Scrub time and watch shadows fall exactly where they will on the day.

---

## Sky, weather and environment

Preetham analytic scattering — Rayleigh and Mie, turbidity-driven — with sun disc,
night floor, stars, a procedural fbm cloud deck and horizon haze. The dome is captured
into a PMREM environment map, so image-based lighting **always** matches the sky:
golden hour lights the model gold without touching a single slider.

- Seven weather states: clear, partly cloudy, overcast, rain, storm, fog, snow
- Animated precipitation with wet-surface response across every registered material
- Season control driving foliage colour
- Moonlight, interior glow and street lighting that fade up automatically at dusk
- Kelvin control on artificial light
- HDRI import when you want a captured environment instead

---

## Materials

Every surface is synthesised procedurally at load. A height field becomes albedo,
roughness and a Sobel-derived normal map — nothing to download, nothing to go missing.

Eighteen library presets — concrete, board-formed concrete, plaster, travertine,
terrazzo, marble, oak, walnut, brick, steel, aluminium, corten, dark metal, asphalt,
paving, grass, soil, fabric — plus dedicated glass, water and emissive materials.

Full PBR control per material: diffuse, roughness, metalness, clear coat, transmission,
sheen, iridescence, opacity, normal depth, tiling and reflection intensity.

---

## Camera

- Prime lens library from 14 mm to 200 mm, with sensor width control
- Aperture, focus distance and auto-focus that ray-picks what is under the reticle
- Three navigation modes: **orbit**, **fly** and **walk** (walk holds eye height)
- Composition guides: thirds, golden ratio, diagonals, grid, safe frame
- Horizon levelling and auto-framing to the model bounds
- Flythrough along a Catmull-Rom path through your captured shots
- **Automatic shot generator** — ten architectural compositions derived from the model:
  hero three-quarter, entrance, corner, eye level, pedestrian, street, drone, bird's
  eye, detail and elevation

---

## Post

A full grade, not a filter stack.

- Tone mapping: ACES, AgX, filmic, linear
- Exposure, temperature, tint, contrast, saturation
- **Lift / gamma / gain colour wheels** with drag control and double-click reset
- Master curve editor — click to add points, drag to shape, right-click to remove
- Bloom, lens dirt, halation, light rays, chromatic aberration, vignette, grain, sharpen
- False-colour IRE exposure map
- Live RGB histogram and waveform monitor
- **Node chain** across the dock — select any node, double-click to bypass it

Twelve cinematic presets: daylight, golden hour, blue hour, night, overcast, rain,
storm, morning fog, snow, museum, luxury interior and minimal white.

---

## Scatter and site

An instanced scatter engine populates the site with trees, people, vehicles, benches
and bollards — density, slope threshold, keep-out radius, season and seed, all live.
Terrain is displaced and the road network excludes scatter automatically, so cars sit
on asphalt and trees do not grow through the pavement.

Five typologies ship in the box: residential villa, commercial tower, gallery pavilion,
perimeter block and interior.

---

## Presentation and export

- Shot list with thumbnails, recall and reorder
- Board view for client review
- **Contact sheet** export as a single PNG
- **Self-contained interactive microsite** — one HTML file you can send to a client
- Batch render of the entire shot list
- Project save and load as JSON

**Export** GLB for the onward pipeline — Unreal, Unity, Blender, Rhino, Twinmotion, D5.

---

## Importing your own models

Drop files anywhere on the window, or use **Scene → Import**.

| Format | Notes |
|---|---|
| `.glb` / `.gltf` | Including Draco-compressed geometry |
| `.gltf` + `.bin` + textures | Select the whole set together and they resolve as one asset |
| `.obj` (+ `.mtl`) | A matching `.mtl` is picked up automatically |
| `.fbx` | Binary and ASCII |
| `.dae` | Collada |
| `.stl` · `.ply` | Loaded as clay, with normals computed and vertex colours honoured |
| `.hdr` | Becomes the environment lighting |
| `.vizion.json` | Restores a saved project |

Several assets live in the scene at once, each with its own controls:

- **Source units** — auto-fit, or metres, centimetres, millimetres, feet, inches, so a
  model authored in millimetres arrives at true size next to a 3.4 m floor height
- **Transform** — scale, rotation, position on the site, lift, snap to ground, centre
  on site, and a one-click 180° forward flip
- **Material override** — keep the materials the file shipped with, or replace them
  with white clay or any VIZION library material, at which point weather, wetness and
  the environment map drive them like everything else in the scene
- **Shadows** — cast and receive, on by default
- **Show demo building** — switch off the generated typology and keep the site, road,
  context and scatter around your own model

Everything else — sun study, weather, grading, shot list, render — works on your model
exactly as it does on the built-in typologies.

---

## Keyboard

| Key | Action |
|---|---|
| `1`–`6` | Switch workspace |
| `W` `A` `S` `D` | Move (fly / walk) |
| `Q` `E` | Down / up |
| `Shift` | Boost speed |
| `F` | Frame scene · `Shift+F` frame selection |
| `C` | Capture shot |
| `R` | Render still |
| `G` | Cycle composition guides |
| `Space` | Play / stop flythrough |
| `H` | Reference sheet |
| `Esc` | Close panel |

---

## Built for the browser

- WebGL2, ES modules, Three.js r170 loaded via import map
- Metering defaults to automatic, so the frame self-levels on any scene
- Accessible by default: skip link, focus rings, ARIA state, reduced-motion support
- Responsive down to a mobile drawer layout

---

## License

Licensed under the **Apache License, Version 2.0**. See [`LICENSE`](LICENSE) and
[`NOTICE`](NOTICE).

You may use, modify, distribute and build commercial work on this software, including
inside closed-source products. The license carries an express patent grant, so
contributors cannot later assert patents against users of the work. In return, keep the
copyright and license notices, state significant changes you make, and include the
`NOTICE` file in any redistribution.

Renders, boards, project files and microsites you produce with VIZION are entirely
yours — no attribution required.

VERTASCAN™, VERTASCAN VIZION™ and VERTASCAN | CREATE are trademarks of Vertascan and
are **not** licensed by Apache-2.0 (see Section 6). Fork the code freely; give the fork
its own name.

---

Part of the **VERTASCAN | CREATE** suite. Copyright 2026 Vertascan.
