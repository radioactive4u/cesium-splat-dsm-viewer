# Method: measure on a lidar DSM, view in splats

This is a plan, not a finished implementation. The measurement surface is a free lidar-derived DSM or DEM, not a surface derived from the splats.

## Pipeline

1. **Get a lidar surface.** Download a free open lidar-derived DSM or DEM at 1 m or better for the area: for example Canada's HRDEM 1 m, LidarBC 1 m (Open Government Licence BC) or USGS 3DEP. Record the source, acquisition year, resolution and vertical datum.
2. **Fix the vertical datum.** Keep every height in one datum (CGVD2013 via the CGG2013a geoid in Canada). Mixing CGVD28 and CGVD2013 can bake in errors of 10 cm or more. CesiumJS works in ellipsoidal heights, so the transform is explicit and documented.
3. **Build the terrain layer.** Convert the lidar tile into Cesium terrain or a 3D Tiles surface.
4. **Align with the splat tileset.** Place the splat tileset (3D Tiles with `KHR_gaussian_splatting`) in the same scene and report the splat-to-lidar offset.
5. **Measure.** Picks are taken on the lidar surface under the cursor, so a pair of points gives the same distance from any camera angle. Tools: point coordinates, distance, height difference, polyline, polygon and area.
6. **Export.** Measured geometry exports to GeoJSON, KML and CSV, with the dataset metadata attached.
7. **Validate.** Compare measured heights and distances against surveyed checkpoints and against the lidar source, and publish the RMSE. State the acquisition year of every tile used.

## Known limits

- Lidar tile vintage varies. Changes since acquisition (new buildings, tree growth, earthworks) are not in the surface, and the viewer shows each tile's year with every measurement.
- A lidar DSM includes buildings and canopy; a DEM does not. The metadata says which one a scene uses.
- Splats can be offset from the lidar surface. The offset is measured and reported, not hidden.

## Prior work

- Deng and Qin (ISPRS Annals 2026), OSU `3dgs_measurement_tool`: measures points on splats in CesiumJS by multi-ray triangulation with uncertainty ellipsoids. It has no DSM or terrain layer and no licence, and leaves distance, area and volume to future work.
