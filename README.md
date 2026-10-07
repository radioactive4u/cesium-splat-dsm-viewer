# Cesium splat DSM viewer

Open-source viewer for 3D Gaussian splat tilesets with a derived DSM layer and in-browser measurement tools. No public tool combines splat rendering, measurable terrain, and standard exports (GeoJSON, KML).

**Live demo:** [radioactive4u.github.io/cesium-splat-dsm-viewer](https://radioactive4u.github.io/cesium-splat-dsm-viewer/) (enable GitHub Pages in repo settings if the link 404s)

## The gap

Splat viewers render. GIS platforms measure. Nobody joins those: a derived DSM as its own layer in a public viewer, measured by someone who is not a photogrammetrist, with export to the formats practitioners already use.

This repo is that join, aimed at small and mid-sized municipalities that cannot carry an enterprise GIS subscription.

## Motivation and use case

Small and medium-sized municipalities are priced out of professional GIS by enterprise licensing like Esri. This project offers a cloud-native, open-source alternative: a browser viewer serving 3D Gaussian splat imagery with a derived DSM, in-browser measurement, and GeoJSON and KML export. A municipality could capture its own assets, host tilesets on commodity cloud storage, and give staff a measurable 3D map without proprietary licenses.

## What this project adds

The rendering and conversion components build on existing open-source libraries (3d-tiles-renderer, Gaussian Splat Lite, 3DGS-PLY-3DTiles-Converter). This project's original contribution is the integration layer: the DSM derivation method, in-browser measurement tools with uncertainty-aware point placement, and the measurement-to-export pipeline producing GeoJSON and KML directly from the viewer. No existing open project combines all three.

## What this is not

- Not a new Gaussian splatting trainer. Splats come from an existing pipeline and are tiled to 3D Tiles (`KHR_gaussian_splatting`).
- Not a satellite DSM method. GU-GS and EOGS are the accuracy references, not the capture source.
- Not LiDAR-dependent. A height prior helps; a town will not have one on every job.
- PlanetScope is a change-detection source, not the height source.

## Technical approach

1. **Conversion:** PLY-format Gaussian splat captures are converted to hierarchical 3D Tiles tilesets using the open-source 3DGS-PLY-3DTiles-Converter (Apache-2.0). Output tiles use SPZ-compressed GLB content with adaptive k-d tree LOD. WGS84 placement is supported via a coordinate flag.
2. **Rendering:** The 3D-Tiles-RendererJS-3DGS-Plugin streams splat tiles into a Three.js scene via Gaussian Splat Lite, supporting WebGPU and WebGL2, ECEF and GIS coordinates, and tile disposal with memory accounting.
3. **DSM derivation:** The splat point cloud is sampled to a regular grid and rasterized into a GeoTIFF DSM. The method is documented in `docs/method.md`. Main technical risk: splat density varies, and thin structures (wires, vegetation edges) can produce artifacts. Limits are stated explicitly.
4. **Measurement:** In-browser tools for point, distance, height, and area measurement, built on raycasting against the splat depth buffer. The OSU 3dgs_measurement_tool (multi-ray triangulation with uncertainty ellipsoids) is the reference implementation.
5. **Export:** Measured geometry exports to GeoJSON and KML, interoperable with QGIS, ArcGIS, and Cesium ion.

## Milestones

1. Viewer loads one georeferenced splat tileset. One working distance measurement.
2. Nadir elevation render from trained splats, written as a Cloud-Optimized GeoTIFF, draped in the same scene. Scored against a few surveyed points.
3. Height, distance, and area read from that DSM. Export GeoJSON, KML, CSV.
4. One town-scale example, deploy notes, accuracy table.

## Deliverables

- A public, hosted viewer loading at least one real-world splat tileset
- Documented DSM derivation method with sample code and a worked example
- At least two sample datasets (one urban, one natural) with converted tilesets
- An export pipeline producing GeoJSON and KML from in-browser measurements
- A README covering setup, conversion, and usage for non-expert users

## Layout

- `docs/method.md` — how the DSM will be derived
- `viewer/` — static viewer page (no build): streams a splat tileset, click-to-measure distance
- `samples/` — tileset and DSM samples (not yet)

## License

Apache-2.0. See [LICENSE](LICENSE).
