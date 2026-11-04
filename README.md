# Particle Clustering

Notebooks for analyzing experimental video data of active and passive particles. The goal is to understand how each particle moves inside its cluster.

## Files

- `linking.ipynb`: links particle detections across video frames using trackpy.
- `Clustering.ipynb`: groups particles into clusters, labels them active or passive, and adds extra columns for analysis.

## Pipeline

1. **Linking:** run `linking.ipynb` on the detection data (`Results.csv`).
2. **Clustering:** run `Clustering.ipynb`.
   - Clusters use a distance threshold of 24 px.
   - Only clusters with 3 to 7 particles are kept.
   - Each particle gets a `particle_type` (active or passive) from an area cutoff of about 103, found from the area histogram.
3. **Added columns:**
   - Per cluster: `N_active`, `N_passive`, `active_fraction`, and AA/AP/PP contact counts.
   - Per particle: position relative to the cluster center of mass.

## Status

Work in progress. Still to do:

- Compute g(r) for AA, AP and PP pairs to replace the 24 px placeholder contact cutoffs.
- Add center of mass, radius of gyration, and nearest-neighbor columns.
- Add the velocity columns (observed, predicted, residual) for the ML model.

## Notes

- The data files are not included in this repo.
- File paths in the notebooks point to local folders and need to be changed to run on another machine.
