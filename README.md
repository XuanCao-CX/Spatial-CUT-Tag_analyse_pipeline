# Spatial-CUT-Tag_analyse_pipeline
Integrative analyse of spatial-CUT&Tag datasets

**References:**
Xuan Cao, Terry Ma, Rong Fan, Guo-Cheng Yuan. Broad H3K4me3 Domain Is Associated with Spatial Coherence during Mammalian Embryonic Development. Biorxiv 2023.
(https://www.biorxiv.org/content/10.1101/2023.12.11.570452v1)

![spatial_cuttag_analysis_summary](https://github.com/user-attachments/assets/542be610-d4af-4b43-b1f0-e11feae7ec4d)


## Analyse workflow

### 1.Pre-processing spatial-CUT&Tag data

We used ArchR (v1.0.1) 21 to pre-process the data and perform downstream analyses of the spatial-CUT&Tag data. We converted the fragments file into a tile matrix. Then perfored the latent semantic indexing (LSI), UMAP and clustering.
Here is the [tutorial](https://github.com/XuanCao-CX/Spatial-CUT-Tag_analyse_pipeline/blob/main/1.LSI_UMAP_spatial-CUT%26TAG_in_ArchR.ipynb) for process spatial-CUT&Tag on H3K4me3.

### 2.Spatially coherence genes detection

We applied Giotto (v1.1.2) to evaluate spatially coherent genes. Firstly, we loaded the gene score matrix, together with the spatial coordinates of the spots, to create the giotto object. Secondly, we created the spatial network connecting spots based on their physical distance by applying the createSpatialNetwork function (method = kNN, minimum_k = 0, k=10, maximum_distance_knn = 40). To calculate spatially coherent genes, we used the binSpect (Binary Spatial extract) function and selected top 200 genes ranked by adj.p.value in further analysis.
