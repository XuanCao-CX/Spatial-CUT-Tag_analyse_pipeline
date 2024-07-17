# Spatial-CUT-Tag_analyse_pipeline
Integrative analyse of spatial-CUT&Tag datasets

**References:**
Xuan Cao, Terry Ma, Rong Fan, Guo-Cheng Yuan. Broad H3K4me3 Domain Is Associated with Spatial Coherence during Mammalian Embryonic Development. Biorxiv 2023.
(https://www.biorxiv.org/content/10.1101/2023.12.11.570452v1)

![spatial_cuttag_analysis_summary](https://github.com/user-attachments/assets/542be610-d4af-4b43-b1f0-e11feae7ec4d)

## Overall view

We have adapted recently developed spatial transcriptomics 19,20 and chromatin state analysis tools 21,22 to systematically characterize the tissue-level spatial organization of chromatin state profiles in the mouse embryo 13. To our knowledge, our study is among the first to systematically characterize the spatial pattern of histone modification marks. Through an innovative use of spatial variable gene analysis to spatial-CUT&Tag data, we identified spatially coherent chromatin state patterns that recapitulated the anatomic structure, and found that the associated genes comprised many tissue-specific transcriptional regulators that are important for organ development. The genomic loci of the spatially coherent genes are distinctly marked by broad domains of H3K4me3, which typically has a narrow peak signature. Aiming to understand the spatial relationships between histone modifications H3K4me3 and H3K27me3, we aligned these two modalities and performed integrative analyses, revealing spatial transitions of the epigenomic profiles across tissue boundaries. Additionally, we further identified transcription factors (TFs)  that mediate the broad H3K4me3 domain and explored their spatial transition.


## Analyse workflow

### 1.Pre-processing spatial-CUT&Tag data

We used ArchR (v1.0.1) 21 to pre-process the data and perform downstream analyses of the spatial-CUT&Tag data. We converted the fragments file into a tile matrix. Then perfored the latent semantic indexing (LSI), UMAP and clustering.

Here is the [tutorial](https://github.com/XuanCao-CX/Spatial-CUT-Tag_analyse_pipeline/blob/main/1.LSI_UMAP_spatial-CUT%26TAG_in_ArchR.ipynb) for process spatial-CUT&Tag on H3K4me3.

### 2.Spatially coherence genes detection

We applied Giotto (v1.1.2) to evaluate spatially coherent genes. Firstly, we loaded the gene score matrix, together with the spatial coordinates of the spots, to create the giotto object. Secondly, we created the spatial network connecting spots based on their physical distance by applying the createSpatialNetwork function (method = kNN, minimum_k = 0, k=10, maximum_distance_knn = 40). To calculate spatially coherent genes, we used the binSpect (Binary Spatial extract) function and selected top 200 genes ranked by adj.p.value in further analysis. 

### 3.Spatial co-localization modules

To identify spatial co-localization modules, we applied the binSpect method implemented in Giotto and identified the top 200 spatially coherent genes. We used the function detectSpatialCorGenes (method = network, spatial_network_name = kNN_network) to calculate a gene-to-gene correlation score matrix followed by function clusterSpatialCorGenes (hclust_method = ward.D2, k = 8) to identified co-localization  modules by hierarchical clustering the gene-to-gene correlation score matrix. Finally, we used the function createMetagenes to summarize the overall spatial pattern for each module.

Here is the [tutorial](https://github.com/XuanCao-CX/Spatial-CUT-Tag_analyse_pipeline/blob/main/2.H3K4me3_spatial_gene_and_modules_in_Giotto.ipynb) for identifying H3K4me3 spatial genes and modules.

Here is the [tutorial](https://github.com/XuanCao-CX/Spatial-CUT-Tag_analyse_pipeline/blob/main/5.H3K27me3_spatial_gene_and_modules_in_Giotto.ipynb) for identifying H3K27me3 spatial genes and modules.

### 4.Multi-scale chromatin state annotation by using diHMM
 
We applied the diHMM method to identify both nucleosome- and domain-level chromatin states, using its Python/C++ implementation. The fragments of H3K4me3 across each cluster were divided into 100 bp bins, and the fragment counts were binarized by using n=5 cutoff. Next, we trained the diHMM model by using the following parameter setting: domain_size=5, domain_states=2, bin_states=2, bin_size=100. 

Here is the [tutorial](https://github.com/XuanCao-CX/Spatial-CUT-Tag_analyse_pipeline/blob/main/3.diHMM_domain_calling%20.ipynb) for running diHMM.

### 5.Integration of H3K4me3 and H3K27me3 data 

Here is the [tutorial](https://github.com/XuanCao-CX/Spatial-CUT-Tag_analyse_pipeline/blob/main/4.align_spots.H3K27me3_to_H3K4me3.ipynb) for align two modilities.



