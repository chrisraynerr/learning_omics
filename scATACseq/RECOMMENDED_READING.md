# Recommended Reading: Single-Cell ATAC-seq & Epigenomics

This reading curriculum accompanies the **Single-Cell ATAC-seq (scATAC-seq) Tutorial Series** in this directory. It outlines the foundational literature, benchmarking studies, and methodological frameworks governing single-cell chromatin accessibility, transcription factor motif analysis, and multi-omic gene regulatory networks.

---

## Table of Contents

- [Core Living Handbooks & Framework Guides](#core-living-handbooks--framework-guides)
- [Foundational Methodology Papers](#foundational-methodology-papers)
  - [1. The Genesis of scATAC-seq & Assay Physics](#1-the-genesis-of-scatac-seq--assay-physics)
  - [2. Quality Control, Fragment Periodicity & TSS Enrichment](#2-quality-control-fragment-periodicity--tss-enrichment)
  - [3. Latent Semantic Indexing (TF-IDF & SVD)](#3-latent-semantic-indexing-tf-idf--svd)
  - [4. Peak Calling & Unified Feature Matrix Construction](#4-peak-calling--unified-feature-matrix-construction)
  - [5. Motif Activity & Transcription Factor Footprinting](#5-motif-activity--transcription-factor-footprinting)
  - [6. Multi-Omic Integration (WNN & Peak-to-Gene Links)](#6-multi-omic-integration-wnn--peak-to-gene-links)
- [Tutorial-to-Reading Mapping Matrix](#tutorial-to-reading-mapping-matrix)

---

## Core Living Handbooks & Framework Guides

### 1. Signac: Comprehensive Single-Cell Epigenomics in R
- **Authors:** Tim Stuart, Avi Srivastava, Sanja Vickovic, Rahul Satija (NYGC)
- **Format:** Peer-Reviewed Software & Documentation
- **Access:** [stuartlab.org/signac/](https://stuartlab.org/signac/)
- **Citation:** Stuart, T., Srivastava, A., Madad, S., Lareau, C. A., & Satija, R. (2021). Single-cell chromatin state analysis with Signac. *Nature Methods*, 18(11), 1333–1341. [DOI: 10.1038/s41592-021-01282-5](https://doi.org/10.1038/s41592-021-01282-5)
- **Why It’s Essential:** Fully integrates with Seurat, providing genomic interval algebra via `GenomicRanges`, TF-IDF transformation, motif enrichment (`chromVAR`), footprinting, and RNA-ATAC co-embedding.

### 2. ArchR: Fast & Scalable Single-Cell Epigenomics
- **Authors:** Jeffrey M. Granja, M. Ryan Corces, Steven E. Pierce, Howard Y. Chang, William J. Greenleaf (Stanford University)
- **Format:** Living Book & Nature Genetics Paper
- **Access:** [archrproject.com](https://www.archrproject.com/)
- **Citation:** Granja, J. M., et al. (2021). ArchR is a scalable software package for integrative single-cell chromatin accessibility analysis. *Nature Genetics*, 53(3), 403–411. [DOI: 10.1038/s41588-021-00790-6](https://doi.org/10.1038/s41588-021-00790-6)
- **Why It’s Essential:** Introduces disk-backed HDF5 architecture for million-cell atlases, genome-wide 500-bp tile matrices, pseudo-bulk peak calling, and peak-to-gene co-accessibility modeling.

---

## Foundational Methodology Papers

### 1. The Genesis of scATAC-seq & Assay Physics

* **Combinatorial Cellular Indexing**: Cusanovich, D. A., Daza, R., Adey, A., et al. (2015). Multiplex single-cell profiling of chromatin accessibility by combinatorial cellular indexing. *Science*, 348(6237), 910–914. [DOI: 10.1126/science.aab1601](https://doi.org/10.1126/science.aab1601)
* **Microfluidic Droplet scATAC**: Buenrostro, J. D., Wu, B., Litzenburger, U. M., et al. (2015). Single-cell chromatin accessibility reveals principles of regulatory variation. *Nature*, 523(7561), 486–490. [DOI: 10.1038/nature14590](https://doi.org/10.1038/nature14590)
* **Key Concept:** Hyperactive Tn5 transposase simultaneously cleaves accessible, nucleosome-depleted DNA and inserts sequencing adapters ("tagmentation"). The binary nature of single-cell accessibility ($0, 1$, or $2$ copies per diploid locus) dictates extreme sparsity.

---

### 2. Quality Control, Fragment Periodicity & TSS Enrichment

* **ENCODE Epigenomic QC Standards**: Landt, S. G., et al. (2012). ChIP-seq guidelines and practices of the ENCODE and modENCODE consortia. *Genome Research*, 22(9), 1813–1831.
* **Key Epigenetic QC Metrics:**
  * **TSS Enrichment Score**: Ratio of fragment insertion density in the $\pm 2$ kb window flanking canonical Transcription Start Sites versus flanking background regions. A score $> 4\text{--}6$ signifies high signal-to-noise.
  * **Nucleosome Banding / Periodicity**: Frequency distribution of paired-end insert lengths showing sharp peaks at $< 100$ bp (nucleosome-free fragments) followed by $\sim 147$ bp periodicities (mononucleosomal, dinucleosomal cuts). Loss of periodicity indicates dead cell debris or apoptotic degradation.
  * **Fraction of Reads in Peaks (FRiP)**: Proportion of sequenced fragments overlapping called open chromatin peaks (ideally $> 20\text{--}30\%$).

---

### 3. Latent Semantic Indexing (TF-IDF & SVD)

* **Citation:** Cusanovich, D. A., et al. (2018). The single-cell transcriptional landscape of mammalian organogenesis. *Cell*, 174(5), 1309–1324.
* **Mathematical Concept:** Term Frequency-Inverse Document Frequency (**TF-IDF**) borrows from computational linguistics to weight binary peak accessibility:
  $$\text{TF}_{i,j} = \frac{c_{i,j}}{\sum_k c_{k,j}}, \quad \text{IDF}_i = \log\left(1 + \frac{N}{\sum_j \mathbb{I}(c_{i,j} > 0)}\right)$$
  $$\text{TF-IDF}_{i,j} = \text{TF}_{i,j} \times \text{IDF}_i$$
  Singular Value Decomposition (**SVD**) then reduces dimensionality. The first singular vector typically correlates strongly with sequencing depth ($r > 0.95$) and must be carefully inspected or excluded.

---

### 4. Peak Calling & Unified Feature Matrix Construction

* **MACS2**: Zhang, Y., et al. (2008). Model-based analysis of ChIP-Seq (MACS). *Genome Biology*, 9(9), R137. [DOI: 10.1186/gb-2008-9-9-r137](https://doi.org/10.1186/gb-2008-9-9-r137)
* **Pseudo-Bulk Peak Calling**: Because individual cells lack sequencing depth to call peaks directly, fragments from homogeneous cell clusters are pooled into pseudo-bulk BAM files before peak calling, preventing dropout-driven omissions.

---

### 5. Motif Activity & Transcription Factor Footprinting

* **chromVAR**: Schep, A. N., Wu, B., Buenrostro, J. D., & Greenleaf, W. J. (2017). chromVAR: inferring transcription-factor-associated accessibility from single-cell epigenomic data. *Nature Methods*, 14(10), 975–978. [DOI: 10.1038/nmeth.4401](https://doi.org/10.1038/nmeth.4401)
  * *Method:* Computes bias-corrected deviation z-scores for TF motifs across cells, controlling for cell-specific GC bias and fragment coverage.
* **Digital Genomic Footprinting (TOBIAS)**: Bentsen, M., et al. (2020). ATAC-seq footprinting unravels kinetics of transcription factor binding during zygotic genome activation. *Nature Communications*, 11(1), 4267. [DOI: 10.1038/s41467-020-18035-1](https://doi.org/10.1038/s41467-020-18035-1)
  * *Method:* Identifies steric hindrance "footprints" where bound TFs protect DNA from Tn5 transposase insertion at nucleotide resolution.

---

### 6. Multi-Omic Integration (WNN & Peak-to-Gene Links)

* **Weighted Nearest Neighbor (WNN) Analysis**: Hao, Y., Hao, S., Andersen-Nissen, E., et al. (2021). Integrated analysis of multimodal single-cell data. *Cell*, 184(13), 3573–3587. [DOI: 10.1016/j.cell.2021.04.048](https://doi.org/10.1016/j.cell.2021.04.048)
  * *Method:* Learns cell-specific modality weights based on information content, constructing a shared multi-omic neighborhood graph.
* **Cicero (Co-Accessibility & Chromatin Conformation)**: Pliner, H. A., et al. (2018). Cicero Predicts cis-Regulatory DNA Interactions from Single-Cell Chromatin Accessibility Data. *Molecular Cell*, 71(5), 858–871. [DOI: 10.1016/j.molcel.2018.06.044](https://doi.org/10.1016/j.molcel.2018.06.044)

---

## Tutorial-to-Reading Mapping Matrix

| Tutorial Chapter | Recommended Primary Paper | Core Epigenomic Tool |
|:---|:---|:---|
| **01: FASTQ to Peaks** | Zhang et al., 2008 / Stuart et al., 2021 | MACS2 & Cell Ranger ATAC |
| **02: Thorough QC with Signac** | Buenrostro et al., 2015 / Landt et al., 2012 | TSS Enrichment & Periodicity |
| **03: Integration & Clustering** | Cusanovich et al., 2018 / Korsunsky et al., 2019 | TF-IDF, SVD & Harmony |
| **04: Cell Type Identification** | Schep et al., 2017 / Stuart et al., 2021 | GeneActivity & chromVAR |
| **05: Differential Accessibility** | Squair et al., 2021 / Granja et al., 2021 | Pseudo-bulk DA & Limma-voom |
