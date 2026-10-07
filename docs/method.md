# DSM derivation method

How a digital surface model (DSM) is derived from a 3D Gaussian splat tileset. This is the project's primary research contribution and its main technical risk.

## Pipeline

1. **Load the tileset.** Stream the 3D Tiles tileset (SPZ-compressed GLB content, `KHR_gaussian_splatting`) through the 3D-Tiles-RendererJS-3DGS-Plugin. Each tile exposes its Gaussian primitives: position, covariance (scale and rotation), opacity, and color.
2. **Sample the splat geometry.** For each Gaussian, evaluate its contribution across a regular grid in the horizontal plane. The grid resolution is chosen relative to the tileset's geometric error and the capture's ground sample distance — typically 0.1 to 0.5 meters for town-scale captures.
3. **Take the highest surface.** At each grid cell, the DSM elevation is the maximum height of any Gaussian whose horizontal footprint covers that cell, weighted by opacity. This produces a surface model rather than a terrain model: buildings, canopy, and vehicles are included.
4. **Rasterize to GeoTIFF.** Write the grid as a Cloud-Optimized GeoTIFF with the tileset's CRS (WGS84 / EPSG:4326 or a local projected CRS). Embed georeferencing metadata so the DSM drops into QGIS, ArcGIS, or Cesium ion without reprojection.
5. **Document and validate.** Score the DSM against a handful of surveyed ground control points where available. Report vertical RMSE and note where thin structures (wires, vegetation edges, poles) produce artifacts.

## Known limits

- **Splat density varies.** Sparse regions (shadows, reflective surfaces, sky) produce holes or noisy elevations. The method fills gaps by interpolation but flags cells below a confidence threshold.
- **Thin structures.** Wires, fence lines, and canopy edges can produce spikes or dropouts. The documentation states these limits explicitly rather than overselling accuracy.
- **No LiDAR prior.** A height prior (as in ARS Gaussian) improves results, but a municipality will not have one on every job. The method works from splats alone.
- **Not a satellite method.** GU-GS and EOGS are accuracy references for comparison, not the capture source. This DSM comes from the same splat capture the viewer renders.

## Reproducibility

All parameters (grid resolution, opacity threshold, interpolation method) are recorded in the output GeoTIFF metadata and in a companion JSON sidecar, so any reader can reproduce or improve the derivation.

## References

- GU-GS: Gaussian splatting DSM extraction from satellite imagery (accuracy reference)
- EOGS: Earth-observation Gaussian splatting height estimation (accuracy reference)
- ARS Gaussian: Gaussian splatting with a height prior
- OSU 3dgs_measurement_tool: multi-ray triangulation for point measurement in 3DGS (measurement reference)
