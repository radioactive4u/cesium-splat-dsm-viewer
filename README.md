# Splat Measure for CesiumJS

An open-source (Apache-2.0) measuring plugin for CesiumJS, so anyone can measure while looking at a Gaussian splat scene. **Status: early placeholder. This repo is being built out; the plan below is what the work will deliver.**

## The gap

Cesium's public viewer can show 3D Gaussian splats (3D Tiles with `KHR_gaussian_splatting`), but it has no tool to measure inside a splat scene. Picking on splats is still an open CesiumJS issue (#13326). Splat viewers render, GIS platforms measure, and nobody joins the two for the public.

## The approach: measure on lidar, view in splats

The measurement surface is **not** derived from the splats. It comes from free, open lidar-derived DSMs and DEMs at 1 m or better, which already cover much of North America (for example Canada's HRDEM 1 m, LidarBC 1 m and USGS 3DEP). The splat sits on top for photoreal context; every number comes from the lidar surface. Clicks are picked on the lidar surface under the cursor, so the same two points give the same number from any camera angle.

## Planned deliverables

1. A CesiumJS measuring plugin that drops into a Cesium Viewer with a splat tileset loaded.
2. A documented lidar terrain step: turn a free lidar DSM or DEM tile into Cesium terrain or a 3D Tiles surface, aligned with the splat tileset, with the splat-to-lidar offset reported.
3. Widgets for point coordinates, distance, height difference, polyline, polygon and area.
4. Honest accuracy: each dataset carries its lidar source, acquisition year, resolution and vertical datum, shown with every measurement. Heights stay in one datum (CGVD2013 in Canada).
5. Export of measured geometry to GeoJSON, KML and CSV.
6. A static-hosting deploy recipe (bucket, no login, no GIS licence; Cesium ion optional).
7. A public accuracy report and two open demo scenes.

## Who it is for

Members of the public and municipal staff. Small and mid-sized towns can self-host it for a few dollars a month instead of paying for an enterprise GIS licence.

## What this is not

- Not a new Gaussian splatting trainer. Splats come from an existing pipeline and are tiled to 3D Tiles.
- Not a way to measure on the splats themselves. Prior work (Deng and Qin, ISPRS 2026, OSU `3dgs_measurement_tool`) measures points on splats by multi-ray triangulation; this project measures against lidar instead.
- Not a satellite DSM method.

## Layout

- `docs/method.md`: how the lidar terrain layer and measurements work
- `docs/application.md`: project description
- `viewer/`: early prototype page (work in progress)
- `samples/`: tileset and DSM samples (not yet)

## License

Apache-2.0. See [LICENSE](LICENSE).
