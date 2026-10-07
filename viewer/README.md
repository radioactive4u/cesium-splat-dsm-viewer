# viewer/

Static, no-build viewer page. Open `index.html` directly in a browser, or host it on any static site (GitHub Pages, a CORS-enabled bucket).

## What it does

- Streams a Gaussian splat tileset as 3D Tiles (`KHR_gaussian_splatting`, SPZ-compressed GLB) through [3d-tiles-renderer](https://github.com/NASA-IMPACT/3d-tiles-renderer-js) with the [Gaussian Splat Lite](https://github.com/NASA-IMPACT/gaussian-splat-lite) plugin.
- Orbit controls for navigation.
- **Click-to-measure distance**: click two points on the splat surface; markers and a line are drawn, and the distance prints in meters. This is the grant demo's one working measurement tool — height, area, and uncertainty-aware placement are milestone 3.

## Setup

1. Convert a PLY capture with [3DGS-PLY-3DTiles-Converter](https://github.com/NASA-IMPACT/3dgs-ply-3dtiles-converter) (or use a sample tileset from the plugin repo).
2. Edit `TILESET_URL` in `index.html` to point at your `tileset.json`.
3. Serve over HTTP with CORS enabled on the tileset bucket (file:// will fail on fetch).
4. Open in a browser with WebGL2 or WebGPU.

## Dependencies (CDN, no build step)

- three (r170)
- 3d-tiles-renderer
- 3d-tiles-rendererjs-3dgs-plugin
- gaussian-splat-lite

All loaded as ES modules from esm.sh.

## Notes

- The current `index.html` uses a placeholder tileset URL. Swap it for real project data before the demo is grant-ready.
- Measurement is Euclidean distance between two raycast hits — fine for a demo, not a survey instrument. The OSU 3dgs_measurement_tool (multi-ray triangulation with uncertainty ellipsoids) is the reference for the production version.
