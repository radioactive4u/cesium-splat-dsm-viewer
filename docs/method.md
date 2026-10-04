# DSM from a splat

Working method. Not done.

## Elevation render

Follow the EOGS elevation composite. Replace color with the altitude of each Gaussian center and alpha-composite in a nadir view:

E(u) = sum_k [altitude(mu_k) * omega_k(u)]

Write the result as a Cloud-Optimized GeoTIFF in the same CRS as the tileset. Drape it in Cesium beside the splat.

## Cleaning

Borrow GU-GS, not the full satellite pipeline. Significance-guided pruning and opacity-entropy regularization to drop floaters and blurry primitives before the elevation render. Do not require LiDAR. A ground mask from the existing terrain is enough as a prior.

## Check, do not pin, on structures

Bridges are checkpoints, not control. Deck elevation deflects, the deck is a thin structure where splats float, and GNSS under a span is obstructed. Control is surveyed points on approaches and pavement. The span is a test if a surveyed deck elevation already exists.

## Measurement

Start from multi-view ray triangulation (Deng and Qin, ISPRS 2026) for points. Add a DSM sample so one click returns a height and a polyline returns distance and area. Export GeoJSON, KML, and CSV.

## References

- Ding et al., GU-GS, TGRS 2026. Satellite splat DSM, pruning and uncertainty masking.
- Aira et al., EOGS. Elevation composite from Gaussian centers.
- Yao et al., ARS Gaussian, ISPRS Journal 2026. Aerial splat geometry. LiDAR prior we will not depend on.
- Deng and Qin, Accurate Point Measurement in 3DGS, ISPRS 2026. Cesium measurement tool.
- CesiumJS 3D Tiles Gaussian splat LOD, 2026.
