
Particle Clustering
Notebooks for analyzing experimental video data of active and passive particles. The goal is to understand how each particle moves inside its cluster.

Files
linking.ipynb: links particle detections across video frames using trackpy.
Clustering.ipynb: groups particles into clusters, labels them active or passive, and adds N_active and N_passive.
Week2_features.ipynb: adds active_fraction and the AA/AP/PP contact counts inside each cluster, finds active-active pairs, and counts how many particles the relabeling rule would change.
radius_of_gyration_calculation.ipynb: adds each cluster's center of mass, each particle's position relative to it, and the radius of gyration.
Week3_total_contacts.ipynb: adds contact counts that include neighbors from other clusters, and the total contacts per cluster.
week4_local_features.ipynb: adds per-particle active-active neighbor features (neighbor count, minimum and mean neighbor distance).
week5_cluster_tracking.ipynb: clusters all particles with no size filter, plots the cluster size distribution, and tracks size-3 clusters across frames until they break.
Pipeline
Linking: run linking.ipynb on the detection data (Results.csv).
Output: linked result.csv (all particles), plus linked result active.csv and linked result passive.csv.
Clustering: run Clustering.ipynb.
Clusters use a distance threshold of 24 px.
Only clusters with 3 to 7 particles are kept.
Each particle gets a particle_type (active or passive) from an area cutoff of about 103, found from the area histogram.
Output: Clustering_Results.csv.
Week 2 columns: run Week2_features.ipynb.
active_fraction = N_active / cluster_size.
cluster_num_AA_contacts, cluster_num_AP_contacts, cluster_num_PP_contacts: pairs inside the cluster that are within the contact cutoff.
Output: Clustering_Results_week2.csv and AA_pairs_24px.csv.
Week 3 columns: run radius_of_gyration_calculation.ipynb, then Week3_total_contacts.ipynb.
cm_x, cm_y: cluster center of mass (average x and y).
x_rel_CM, y_rel_CM: each particle's position relative to the center.
Rg_px: radius of gyration in pixels, the square root of the average squared distance from the center.
cluster_num_AA/AP/PP_contacts_all and cluster_total: contacts that include neighbors in other clusters, and their total.
Output: Clustering_Results_week3_radius_of_gyration.csv, then Clustering_Results_week3_contacts.csv.
Week 4 columns: run week4_local_features.ipynb.
Computed for active particles only, using every active particle in the same frame. Passive particles are left blank (NaN).
num_AA_neighbors_48px: number of other active particles within 48 px.
local_min_AA_distance_px, local_mean_AA_distance_px: minimum and mean distance to those neighbors (blank if there are none).
Plots: week4_AA_distance_distributions.png and week4_Rg_check_cluster.png.
Week 5 cluster tracking: run week5_cluster_tracking.ipynb. It starts again from linked result.csv.
Clusters every particle with the same 24 px threshold and no size filter, then plots how many clusters there are of each size (week5_cluster_size_distribution.png).
Size 3 is the most common real cluster (23,978 clusters across all frames), so size-3 clusters are tracked.
trackpy renumbers clusters in every frame, so each cluster is identified by its set of member particle_ids instead.
track_id: a cluster keeps the same track_id from one frame to the next as long as it has exactly the same members. If any member leaves, joins or changes, the cluster gets a new track_id. A group that breaks and later re-forms also gets a new one.
Result: 4,789 tracked size-3 clusters. The median track lasts 3 frames, and the longest lasts 114 frames.
Plots the center-of-mass displacement from the start position over time, and the center-of-mass path, for the longest track (week5_cluster_CM_displacement.png).
Output: Clustering_Results_week5_tracked_size3.csv.
Contact cutoffs
A pair of particles is a contact if the distance between them is within the cutoff for its type:

AA (active-active): 48 px
AP (active-passive): 24 px
PP (passive-passive): 24 px
Status
Work in progress. Done so far: linking, clustering, active/passive labels, Week 2 features, center of mass, radius of gyration (in pixels), total contacts, active-active neighbor features, and tracking size-3 clusters across frames.

Still to do:

Convert Rg_px to micrometers (Rg_recomputed_um) once the pixel-to-micrometer scale is known.
Decide whether to apply the active-active relabeling (the larger-area particle in each AA pair becomes passive). So far it is only counted, not applied.
Save the Week 4 neighbor columns to a CSV. Add nearest-neighbor features for passive particles and AP pairs if needed.
Check whether the many 1-frame cluster tracks are real or caused by linking losing particles. Linking uses no memory, so a particle that is missed for one frame comes back with a new particle_id.
Extend cluster tracking to sizes 4 and 5.
Add the velocity columns (observed, cluster center-of-mass, predicted, residual) for the ML model, using the tracked clusters.
Notes
The data files are not included in this repo.
File paths in the notebooks point to local folders and need to be changed to run on another machine.
radius_of_gyration_calculation.ipynb saves Clustering_Results_week3.csv. The file used downstream is Clustering_Results_week3_radius_of_gyration.csv, so rename it after running.
The local copy of Week3_total_contacts.ipynb contains only the setup cell. The code that produced Clustering_Results_week3_contacts.csv needs to be added back.
The "fewer than 5 frames" filter in linking.ipynb does not remove anything, because trackpy linking with no memory never leaves gaps in a track.
