# Cesium Ecosystem Grant: project description

Splat Measure for CesiumJS: an open-source plugin for measuring Gaussian splat scenes on free lidar terrain. Applicant: Lucas Crawford, individual, 9 months, USD 15,000 cap.

## The gap

Cesium's public viewer streams 3D Gaussian splats through 3D Tiles (`KHR_gaussian_splatting`), but there is no tool in it to measure inside a splat scene. Picking on splats is still an open CesiumJS issue (#13326). Splat viewers render, GIS platforms measure, and nobody joins the two.

## Approach: measure on lidar, view in splats

The measurement surface is not derived from the splats. It comes from free, open lidar-derived DSMs and DEMs at 1 m or better that already cover much of North America (HRDEM 1 m, LidarBC 1 m, USGS 3DEP). The splat gives photoreal context; every number comes from the lidar surface.

## What will be built (Apache-2.0)

1. A CesiumJS measuring plugin for a Cesium Viewer with a splat tileset loaded.
2. A documented lidar terrain and alignment step, with the splat-to-lidar offset reported.
3. Widgets: point, distance, height difference, polyline, polygon, area.
4. Dataset metadata (source, year, resolution, vertical datum) shown with every measurement; heights kept in CGVD2013 in Canada.
5. Export to GeoJSON, KML and CSV.
6. A static-hosting deploy recipe (bucket, no login, no GIS licence; Cesium ion optional).
7. A public accuracy report against surveyed checkpoints, two open demo scenes, and tutorials.

## Audience

Members of the public and municipal staff. Small and mid-sized towns can self-host it instead of paying for an enterprise GIS licence.

## Milestones (9 months)

- M1, month 2: CesiumJS viewer with a splat tileset and lidar terrain in one scene
- M2, month 3: measurement and alignment method documented
- M3, month 4: measuring widgets and GeoJSON, KML, CSV export
- M4, month 6: accuracy report and first demo scene
- M5, month 8: second demo scene, tutorials, deploy recipe
- M6, month 9: final release and demo at the Cesium Developer Conference 2027

## Status

This repository is an early placeholder that is being built out. Nothing here claims a finished plugin.
