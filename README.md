# Cesium splat DSM viewer

Open CesiumJS viewer that streams a georeferenced Gaussian splat as 3D Tiles and overlays a DSM derived from that splat, with height, distance, and area measurement and GeoJSON / KML / CSV export.

The open layer is the viewer. Capture (drone and street-view) is a separate service and is not in this repository.

The [clickable demo](https://radioactive4u.github.io/cesium-splat-dsm-viewer/viewer/) is the live scene and uses ion asset 4547222 until a project tileset exists. The same scene is at https://radioactive.tor1.digitaloceanspaces.com/cesium-splat-dsm-viewer/viewer/index.html. A raw PLY or SOG will not load in CesiumJS. The real URL is later: convert a trained PLY to 3D Tiles with `KHR_gaussian_splatting_compression_spz_2`, host `tileset.json` on Spaces with CORS open, then swap the asset id for that URL.

## The gap

Splat viewers render. Satellite Gaussian-splatting papers extract DSMs offline. The Ohio State 3DGS measurement tool picks points by multi-view ray triangulation in Cesium. Nobody joins those: a derived DSM as its own layer in a public CesiumJS viewer, measured by someone who is not a photogrammetrist.

This repo is that join, aimed at small and mid-sized communities that cannot carry an Esri subscription.

## What this is not

- Not a new Gaussian splatting trainer. Splats come from an existing pipeline and are tiled to 3D Tiles (`KHR_gaussian_splatting`).
- Not a satellite DSM method. GU-GS and EOGS are the accuracy references, not the capture source.
- Not LiDAR-dependent. ARS Gaussian shows a height prior helps; a town will not have one on every job.
- PlanetScope is a change-detection source, not the height source.

## Milestones

1. Viewer loads one georeferenced splat tileset. One working distance measurement.
2. Nadir elevation render from trained splats, written as a Cloud-Optimized GeoTIFF, draped in the same scene. Scored against a few surveyed points, not a LiDAR campaign.
3. Height, distance, and area read from that DSM. Export GeoJSON, KML, CSV.
4. One town-scale example, deploy notes, accuracy table.

## Layout

- `docs/method.md` — how the DSM will be derived.
- `viewer/` — static CesiumJS page (no build). Live scene: GitHub Pages `/viewer/` and Spaces `cesium-splat-dsm-viewer/viewer/index.html`.
- `samples/` — tileset and DSM samples (not yet).

## License

Apache-2.0. See [LICENSE](LICENSE).
