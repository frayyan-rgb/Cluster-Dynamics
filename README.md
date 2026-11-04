# Particle Clustering

Notebooks for analyzing experimental video data of active and passive particles. The goal is to understand how each particle moves inside its cluster.

## Files

- `linking.ipynb`: links particle detections across video frames using trackpy.
- `Clustering.ipynb`: groups particles into clusters, labels them active or passive, and adds `N_active` and `N_passive`.
- `Week2_features.ipynb`: adds `active_fraction` and the AA/AP/PP contact counts inside each cluster, finds active-active pairs, and counts how many particles the relabeling rule would change.
- `Week3_radius_of_gyration.ipynb`: adds the cluster center of mass, each particle's position relative to it, and the radius of gyration.
- `Week3_total_contacts.ipynb`: adds contact counts that include neighbors from other clusters, and the total contacts per cluster.

## Pipeline

1. **Linking:** run `linking.ipynb` on the detection data (`Results.csv`).
2. **Clustering:** run `Clustering.ipynb`.
   - Clusters use a distance threshold of 24 px.
   - Only clusters with 3 to 7 particles are kept.
   - Each particle gets a `particle_type` (active or passive) from an area cutoff of about 103, found from the area histogram.
3. **Week 2 columns:** run `Week2_features.ipynb`.
   - `active_fraction` = `N_active` / `cluster_size`.
   - `cluster_num_AA_contacts`, `cluster_num_AP_contacts`, `cluster_num_PP_contacts`: pairs inside the cluster that are within the contact cutoff.
4. **Week 3 columns:** run the two Week 3 notebooks.
   - `cm_x`, `cm_y`: cluster center of mass (average x and y).
   - `x_rel_CM`, `y_rel_CM`: each particle's position relative to the center.
   - `Rg_px`: radius of gyration in pixels, the square root of the average squared distance from the center.
   - `cluster_num_AA/AP/PP_contacts_all` and `cluster_total_contacts_pair_specific`: contacts that include neighbors in other clusters, and their total.

## Contact cutoffs

A pair of particles is a contact if the distance between them is within the cutoff for its type:

- AA (active-active): 48 px
- AP (active-passive): 24 px
- PP (passive-passive): 24 px

## Status

Work in progress. Done so far: linking, clustering, active/passive labels, Week 2 features, center of mass, radius of gyration (in pixels) and total contacts.

Still to do:

- Convert `Rg_px` to micrometers (`Rg_recomputed_um`) once the pixel-to-micrometer scale is known.
- Decide whether to apply the active-active relabeling (the larger-area particle in each AA pair becomes passive). So far it is only counted, not applied.
- Add nearest-neighbor distance and neighbor count columns.
- Add the velocity columns (observed, predicted, residual) for the ML model.

## Notes

- The data files are not included in this repo.
- File paths in the notebooks point to local folders and need to be changed to run on another machine.
