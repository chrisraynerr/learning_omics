# Recommended Reading: Single-Cell RNA-seq & Transcriptomics

This reading curriculum accompanies the **Single-Cell RNA-seq (scRNA-seq) Tutorial Series** in this directory. It brings together the foundational textbooks, peer-reviewed methodology papers, and rigorous benchmarking studies that define contemporary best practices in single-cell transcriptomic analysis.

---

## Table of Contents

- [Core Textbooks & Living Books](#core-textbooks--living-books)
- [Foundational Methodology Papers](#foundational-methodology-papers)
  - [1. Quality Control, Ambient RNA & Doublet Detection](#1-quality-control-ambient-rna--doublet-detection)
  - [2. Normalization, Feature Selection & Dimensionality Reduction](#2-normalization-feature-selection--dimensionality-reduction)
  - [3. Batch Integration & Harmonization](#3-batch-integration--harmonization)
  - [4. Cell Clustering & Cell Type Annotation](#4-cell-clustering--cell-type-annotation)
  - [5. Differential Expression, Pseudobulk & Proportion Testing](#5-differential-expression-pseudobulk--proportion-testing)
  - [6. Trajectory Inference & RNA Velocity](#6-trajectory-inference--rna-velocity)
  - [7. Cell-Cell Communication (Intercellular Signaling)](#7-cell-cell-communication-intercellular-signaling)
  - [8. Gene Regulatory Networks & Functional Signatures](#8-gene-regulatory-networks--functional-signatures)
  - [9. Cancer In Silico Karyotyping & Copy Number Profiling](#9-cancer-in-silico-karyotyping--copy-number-profiling)
- [Best-Practice Reviews & Benchmarks](#best-practice-reviews--benchmarks)
- [Tutorial-to-Reading Mapping Matrix](#tutorial-to-reading-mapping-matrix)

---

## Core Textbooks & Living Books

### 1. Orchestrating Single-Cell Analysis with Bioconductor (OSCA)
- **Authors:** Robert Amezquita, Aaron Lun, Stephanie Hicks, Raphael Gottardo
- **Format:** Free Living Online Book & Nature Methods Paper
- **Access:** [bioconductor.org/books/release/OSCA/](https://bioconductor.org/books/release/OSCA/)
- **Citation:** Amezquita, R. A., et al. (2020). Orchestrating single-cell analysis with Bioconductor. *Nature Methods*, 17(2), 137–145.
- **Why It’s Essential:** The authoritative reference for SingleCellExperiment data structures, pooled size-factor normalization, empirical variance decomposition, and graph-based community detection.

### 2. Best Practices for Single-Cell Analysis Across Modalities
- **Authors:** Fabian J. Theis, Malte D. Luecken, Anna Schaar, Lukas Heumos, et al.
- **Format:** Living Book & Nature Reviews Genetics Paper
- **Access:** [sc-best-practices.org](https://www.sc-best-practices.org/)
- **Citation:** Heumos, L., Schaar, A. C., Lance, C., et al. (2023). Best practices for single-cell analysis across modalities. *Nature Reviews Genetics*, 24(9), 550–572.
- **Why It’s Essential:** The gold-standard synthesis of when to apply specific algorithms, how to evaluate batch-correction tradeoffs (bio-conservation vs. batch mixing), and recommendations spanning scRNA-seq, scATAC-seq, and spatial transcriptomics.

---

## Foundational Methodology Papers

### 1. Quality Control, Ambient RNA & Doublet Detection

#### Ambient RNA Contamination & Decontamination
* **SoupX**: Young, M. D., & Behjati, S. (2020). SoupX removes ambient RNA contamination from droplet-based single-cell RNA sequencing data. *GigaScience*, 9(12), giaa151. [DOI: 10.1093/gigascience/giaa151](https://doi.org/10.1093/gigascience/giaa151)
* **DecontX**: Yang, S., et al. (2020). Decontamination of ambient RNA in single-cell RNA-seq with DecontX. *Genome Biology*, 21(1), 57. [DOI: 10.1186/s13059-020-1950-6](https://doi.org/10.1186/s13059-020-1950-6)
* **Takeaway:** Ambient extracellular RNA in cell suspensions causes cross-contamination of lineage-defining transcripts. SoupX and DecontX model the cell-free droplet soup profile to subtract contamination without altering endogenous expression.

#### In Silico Doublet / Multiplet Detection
* **DoubletFinder**: McGinnis, C. S., Murrow, L. M., & Gartner, Z. J. (2019). DoubletFinder: Doublet detection in single-cell RNA sequencing data using artificial nearest neighbors. *Cell Systems*, 8(4), 329–337. [DOI: 10.1016/j.cels.2019.03.003](https://doi.org/10.1016/j.cels.2019.03.003)
* **scDblFinder**: Germain, P. L., Lun, A., Garcia Meixide, C., Macnair, W., & Robinson, M. D. (2021). Doublet identification in single-cell sequencing data using scDblFinder. *F1000Research*, 10, 979. [DOI: 10.12688/f1000research.73607.2](https://doi.org/10.12688/f1000research.73607.2)
* **Takeaway:** Heterotypic doublets simulate intermediate cell states, masquerading as novel cell types. These algorithms generate artificial doublets in silico to establish decision boundaries.

---

### 2. Normalization, Feature Selection & Dimensionality Reduction

#### Regularized Negative Binomial Regression (sctransform)
* **Citation:** Hafemeister, C., & Satija, R. (2019). Normalization and variance stabilization of single-cell RNA-seq data using regularized negative binomial regression. *Genome Biology*, 20(1), 296. [DOI: 10.1186/s13059-019-1874-1](https://doi.org/10.1186/s13059-019-1874-1)
* **Citation (v2):** Choudhary, S., & Satija, R. (2022). Comparison and evaluation of statistical error models for scRNA-seq. *Genome Biology*, 23(1), 27.
* **Takeaway:** Standard $\log(\text{CP10k} + 1)$ introduces spurious correlations driven by sequencing depth. `sctransform` fits a generalized linear model with Poisson or Negative Binomial error distributions and pools parameter estimates via regularized spline shrinkage.

#### UMAP: Uniform Manifold Approximation and Projection
* **Citation:** McInnes, L., Healy, J., & Melville, J. (2018). UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. *arXiv:1802.03426*.
* **Takeaway:** Provides Riemannian geometric foundations for preserving both local neighborhood structures and global continuum topologies compared to t-SNE.

---

### 3. Batch Integration & Harmonization

* **Harmony**: Korsunsky, I., Millard, N., Fan, J., et al. (2019). Fast, sensitive and flexible integration of single-cell data with Harmony. *Nature Methods*, 16(12), 1289–1296. [DOI: 10.1038/s41592-019-0619-0](https://doi.org/10.1038/s41592-019-0619-0)
* **Seurat Anchor Integration (v3/v4/v5)**: Stuart, T., Butler, A., Hoffman, P., et al. (2019). Comprehensive Integration of Single-Cell Data. *Cell*, 177(7), 1888–1902. [DOI: 10.1016/j.cell.2019.05.031](https://doi.org/10.1016/j.cell.2019.05.031)
* **scVI (Probabilistic Deep Generative Modeling)**: Lopez, R., Regier, J., Cole, M. B., Jordan, M. I., & Yosef, N. (2018). Deep generative modeling for single-cell transcriptomics. *Nature Methods*, 15(12), 1053–1058. [DOI: 10.1038/s41592-018-0229-2](https://doi.org/10.1038/s41592-018-0229-2)
* **Benchmarking Reference:** Tran, H. T. N., et al. (2020). A benchmark of batch-effect correction methods for single-cell RNA sequencing data. *Genome Biology*, 21(1), 12.

---

### 4. Cell Clustering & Cell Type Annotation

* **Leiden Community Detection**: Traag, V. A., Waltman, L., & van Eck, N. J. (2019). From Louvain to Leiden: guaranteeing well-connected communities. *Scientific Reports*, 9(1), 5233. [DOI: 10.1038/s41598-019-41695-z](https://doi.org/10.1038/s41598-019-41695-z)
* **SingleR**: Aran, D., Looney, A. P., Liu, L., et al. (2019). Reference-based analysis of lung single-cell sequencing reveals a stable pro-inflammatory subtype of macrophages. *Nature Immunology*, 20(2), 163–172.
* **CellTypist**: Domínguez Conde, C., Xu, C., Jarvis, L. B., et al. (2022). Cross-tissue immune cell analysis reveals tissue-specific features in humans. *Science*, 376(6594), eabl5197. [DOI: 10.1126/science.abl5197](https://doi.org/10.1126/science.abl5197)

---

### 5. Differential Expression, Pseudobulk & Proportion Testing

* **The Pseudobulk Imperative**: Squair, J. W., Gautier, M., Kathe, C., et al. (2021). Confronting false discoveries in single-cell differential expression. *Nature Communications*, 12(1), 5692. [DOI: 10.1038/s41467-021-25960-2](https://doi.org/10.1038/s41467-021-25960-2)
  * *Critical Finding:* Single-cell DE tests treating each cell as an independent observation (e.g., Wilcoxon, naive edgeR/DESeq2 on single cells) suffer from extreme false discovery inflation (FDR > 50%) because they fail to account for biological sample-level replication. Aggregating cells to sample-level **pseudobulks** restores nominal FDR control.
* **Cell Proportion Testing**:
  * **miloR**: Dann, E., Henderson, N. C., Teichmann, S. A., Morgan, M. D., & Marioni, J. C. (2022). Differential abundance testing on single-cell data using k-nearest neighbor graphs. *Nature Biotechnology*, 40(2), 245–253.
  * **propeller / speckle**: Phipson, B., et al. (2022). propeller: testing for differences in cell type proportions in single cell data. *Bioinformatics*, 38(10), 2720–2726.

---

### 6. Trajectory Inference & RNA Velocity

* **Monocle 3**: Cao, J., Spielmann, M., Qiu, X., et al. (2019). The single-cell transcriptional landscape of mammalian organogenesis. *Nature*, 566(7745), 496–502. [DOI: 10.1038/s41586-019-0969-x](https://doi.org/10.1038/s41586-019-0969-x)
* **Slingshot**: Street, K., Risso, D., Fletcher, R. B., et al. (2018). Slingshot: cell lineage and pseudotime inference for single-cell transcriptomics. *BMC Genomics*, 19(1), 477. [DOI: 10.1186/s12864-018-4772-0](https://doi.org/10.1186/s12864-018-4772-0)
* **scVelo (Dynamical RNA Velocity)**: Bergen, V., Lange, M., Peidli, S., Wolf, F. A., & Theis, F. J. (2020). Generalizing RNA velocity to transient cell states through dynamical modeling. *Nature Biotechnology*, 38(12), 1408–1414. [DOI: 10.1038/s41587-020-0591-3](https://doi.org/10.1038/s41587-020-0591-3)
* **CellRank**: Lange, M., Bergen, V., Klein, M., et al. (2022). CellRank for directed single-cell fate mapping. *Nature Methods*, 19(2), 159–170. [DOI: 10.1038/s41592-021-01346-6](https://doi.org/10.1038/s41592-021-01346-6)

---

### 7. Cell-Cell Communication (Intercellular Signaling)

* **CellChat**: Jin, S., Guerrero-Juarez, C. F., Zhang, L., et al. (2021). Inference and analysis of cell-cell communication using CellChat. *Nature Communications*, 12(1), 1088. [DOI: 10.1038/s41467-021-21246-9](https://doi.org/10.1038/s41467-021-21246-9)
* **NicheNet**: Browaeys, R., Saelens, W., & Saeys, Y. (2020). NicheNet: modeling intercellular communication by linking ligands to target genes using prior knowledge. *Nature Methods*, 17(2), 159–162. [DOI: 10.1038/s41592-019-0667-5](https://doi.org/10.1038/s41592-019-0667-5)
* **Benchmark Comparison**: Dimitrov, D., Türei, D., Garrido-Rodriguez, M., et al. (2022). Comparison of methods and resources for cell-cell communication inference from single-cell RNA-seq data. *Nature Communications*, 13(1), 3224.

---

### 8. Gene Regulatory Networks & Functional Signatures

* **SCENIC**: Aibar, S., González-Blas, C. B., Moerman, T., et al. (2017). SCENIC: single-cell regulatory network inference and clustering. *Nature Methods*, 14(11), 1083–1086. [DOI: 10.1038/nmeth.4463](https://doi.org/10.1038/nmeth.4463)
* **decoupleR & CollecTRI**: Badia-i-Mompel, P., Vélez Santiago, J., Braunger, J., et al. (2022). decoupleR: ensemble of methods to infer biological activities from omics data. *Bioinformatics Advances*, 2(1), vbac016. [DOI: 10.1093/bioadv/vbac016](https://doi.org/10.1093/bioadv/vbac016)
* **hdWGCNA**: Morabito, S., Reese, F., Rahimzadeh, N., et al. (2023). hdWGCNA: High-dimensional co-expression network analysis in single-cell data. *Cell Reports Methods*, 3(6), 100498. [DOI: 10.1016/j.crmeth.2023.100498](https://doi.org/10.1016/j.crmeth.2023.100498)

---

### 9. Cancer In Silico Karyotyping & Copy Number Profiling

* **CopyKAT**: Gao, R., Bai, S., Henderson, Y. C., et al. (2021). Delineating copy number and clonal substructure in human tumors by single-cell transcriptomics. *Nature Biotechnology*, 39(5), 599–608. [DOI: 10.1038/s41587-020-00795-2](https://doi.org/10.1038/s41587-020-00795-2)
* **inferCNV**: Patel, A. P., et al. (2014). Single-cell RNA-seq highlights intratumoral heterogeneity in primary glioblastoma. *Science*, 344(6190), 1396–1401.

---

## Tutorial-to-Reading Mapping Matrix

| Tutorial Chapter | Recommended Primary Paper | Method / Package |
|:---|:---|:---|
| **01: Fastq to Counts** | Zheng et al., 2017 (*Nat Commun*) | Cell Ranger / STARsolo |
| **02: Quality Control & Filtering** | Luecken & Theis, 2019 (*Mol Syst Biol*) | Single-cell QC guidelines |
| **03: SoupX vs DecontX & Doublets** | Young & Behjati, 2020 / McGinnis et al., 2019 | SoupX & DoubletFinder |
| **04: Integration & Clustering** | Korsunsky et al., 2019 / Traag et al., 2019 | Harmony & Leiden |
| **05: Cell Type Identification** | Aran et al., 2019 / Domínguez Conde et al., 2022 | SingleR & CellTypist |
| **06: Pseudobulk DE & Proportions** | Squair et al., 2021 / Dann et al., 2022 | DESeq2 Pseudobulk & miloR |
| **07: Seurat vs SCE Objects** | Amezquita et al., 2020 (*Nat Methods*) | Bioconductor vs Seurat |
| **08: Monocle 3 Trajectories** | Cao et al., 2019 (*Nature*) | Monocle 3 (Reversed Graph Embedding) |
| **09: Slingshot Trajectories** | Street et al., 2018 (*BMC Genomics*) | Slingshot (Cluster-based MST) |
| **10: PDX Human/Mouse Deconvolution**| Callari et al., 2018 (*Br J Cancer*) | Xenome / Disambiguate |
| **11: CellChat Signaling** | Jin et al., 2021 (*Nat Commun*) | CellChat |
| **12: NicheNet Target Signaling** | Browaeys et al., 2020 (*Nat Methods*) | NicheNet |
| **13: CopyKAT Tumor CNV** | Gao et al., 2021 (*Nat Biotechnol*) | CopyKAT |
| **14: hdWGCNA Networks** | Morabito et al., 2023 (*Cell Rep Methods*) | hdWGCNA (Metacells) |
| **15: scVelo RNA Velocity** | Bergen et al., 2020 (*Nat Biotechnol*) | scVelo dynamical model |
| **16: CellRank Fate Mapping** | Lange et al., 2022 (*Nat Methods*) | CellRank Markov chains |
| **17: scPlotter Visualizations** | Wickham, 2016 (*ggplot2 book*) | ComplexHeatmap & scPlotter |
| **18: decoupleR & CollecTRI** | Badia-i-Mompel et al., 2022 (*Bioinformatics*) | decoupleR / CollecTRI |
| **19: decoupleR & PROGENy** | Schubert et al., 2018 (*Nat Commun*) | PROGENy pathway footprints |

