---
title: "Proteomics Track: Curriculum Overview"
subtitle: "Complete Step-by-Step Guide: Discovery Mass Spectrometry & Targeted Affinity (Olink & SomaScan)"
---

# Proteomics Data Analysis: Complete Tutorial Series

Welcome to the **Proteomics Data Analysis** curriculum! This series provides a practical, step-by-step guide for computational biologists, bioinformaticians, and wet-lab researchers navigating modern high-throughput proteomics.

Just as RNA sequencing revolutionized transcriptomics, modern proteomics now allows us to quantify thousands of proteins across biological conditions. However, analyzing proteomics data presents unique challenges distinct from RNA-seq: **continuous ion intensities instead of discrete counts**, complex **peptide-to-protein aggregation**, **limit-of-detection missing values**, and emerging **affinity-based proteomic technologies (Olink and SomaScan)**.

---

## Curriculum Architecture: "Two Tracks → One Converged Core"

To reflect modern proteomics without confusing fundamentally different data structures, this tutorial series is organized into a dual-track architecture:

```
                                  [ THE PROTEOMICS LANDSCAPE ]
                                             (Part 1)
                                                 │
                   ┌─────────────────────────────┴─────────────────────────────┐
                   ▼                                                           ▼
     [ TRACK A: Discovery LC-MS/MS ]                           [ TRACK B: Targeted Affinity ]
      • Part 2a: Raw Spectra to Matrix (DIA-NN)                 • Part 3a: Olink QC & NPX Analysis
      • Part 2b: QC & Missing Value Imputation                  • Part 3b: SomaScan .adat & ANML
      • Part 2c: DAPs (MSstats vs. limma)                       • Part 3c: Affinity DAPs & LOD
                   │                                                           │
                   └─────────────────────────────┬─────────────────────────────┘
                                                 ▼
                                     [ CONVERGED DOWNSTREAM ]
                               • Part 4: Publication-Ready Figures
                               • Part 5: Pathways & STRING PPI Networks
                               • Part 6: Protein Co-expression (WGCNA)
                                                 │
                                                 ▼
                                     [ ADVANCED TRANSLATIONAL ]
                               • Part 7: Cross-Platform Discordance
                               • Part 8: Plasma pQTLs & Mendelian Randomization
                               • Part 9: Phosphoproteomics & PTMs
                               • Part 10: Multi-Omics: Bulk RNA-seq vs. Proteomics
                               • Part 11: Data Deposition to PRIDE / ProteomeXchange
```

---

## Tutorial Index

### Foundations
- **[01. Part 1: The Proteomics Landscape & Environment Setup](01_part-1-the-proteomics-landscape-ms-vs-olink-vs-somascan.md)**
  - Why protein $\neq$ mRNA (post-transcriptional buffering).
  - Comparing Mass Spectrometry (LC-MS/MS), Olink (PEA), and SomaScan (SOMAmers).
  - Setting up Conda and R/Bioconductor environments (`QFeatures`, `MSstats`, `OlinkAnalyze`, `SomaDataIO`).

### Track A: Discovery Mass Spectrometry (LC-MS/MS)
- **[02. Part 2a: From Raw Spectra to Protein Quantification Matrix with DIA-NN](02_part-2a-ms-from-raw-spectra-to-counts-diann-best-practices.md)**
  - Basics of LC-MS/MS, DDA vs. DIA, and MS1 vs. MS2.
  - Preparing UniProt FASTA + cRAP contaminant databases.
  - Running DIA-NN from the command line: Target-decoy FDR, in silico library generation, MaxLFQ rollup.
- **[03. Part 2b: Data QC, Filtering, and the Missing Value Dilemma](03_part-2b-ms-data-qc-filtering-and-handling-missing-values.md)**
  - Working with Bioconductor's `QFeatures` and `DEP`.
  - Understanding MNAR (Missing Not At Random) vs. MAR (Missing At Random).
  - Imputation benchmarks: Left-censored Gaussian (Perseus/MinProb) vs. kNN vs. imputation-free models (`proDA`).
- **[04. Part 2c: Differential Abundance Analysis: MSstats vs. limma](04_part-2c-ms-differential-abundance-msstats-vs-limma.md)**
  - Linear mixed-effects modeling at the peptide level (`MSstats`) vs. protein-rollup empirical Bayes (`limma`).
  - Handling multi-condition and longitudinal experimental designs.

### Track B: Targeted Affinity Proteomics (Olink & SomaScan)
- **[05. Part 3a: Olink Proximity Extension Assay: QC, Normalization, and OlinkAnalyze](05_part-3a-olink-qc-npx-normalization-and-olinkanalyze.md)**
  - Understanding NPX ($\log_2$ scale), internal controls, and assay warning flags.
  - Plate bridging normalization across cohorts.
- **[Hands-On Workbook: Plasma Proteomics Paired QC & Analysis Notebook](05b_olink_plasma_paired_qc_and_analysis_notebook.md)** (Online Guide, [`.Rmd`](05b_olink_plasma_paired_qc_and_analysis_notebook.Rmd) & [`.ipynb`](05b_olink_plasma_paired_qc_and_analysis_notebook.ipynb))
  - Complete executable walkthrough on a paired pre/post-treatment clinical cohort ($N = 60$ samples across 2 plates with bridge controls).
  - Step-by-step QC diagnostics: Spiked internal controls ($\pm 0.3$ NPX deviation), LOD detectability ($50\%$ threshold), and multi-plate bridging.
  - Statistical testing: Paired Student's t-test and Linear Mixed-Effects Models (`lme4::lmer`), volcano plots, and matched patient trajectory spaghetti plots.
- **[06. Part 3b: SomaScan Aptamer Proteomics: .adat Parsing and ANML Normalization](06_part-3b-somascan-adat-qc-anml-and-somadataio.md)**
  - Structure of `.adat` files, SOMAmer metadata, and hybridization controls.
  - Adaptive Normalization by Maximum Likelihood (ANML) using `SomaDataIO`.
- **[07. Part 3c: Affinity Differential Abundance and Handling Limits of Detection (LOD)](07_part-3c-affinity-differential-abundance-and-lod-handling.md)**
  - Left-censoring vs. observed values below LOD.
  - Linear modeling with `limma` on dense affinity matrices.

### Converged Downstream Analysis & Biological Discovery
- **[08. Part 4: Creating Publication-Ready Figures](08_part-4-creating-publication-ready-figures-volcano-heatmaps-pca.md)**
  - Custom Volcano plots with peptide confidence annotations.
  - Clustered heatmaps using `ComplexHeatmap`.
  - Missingness heatmaps and PCA/UMAP embeddings.
- **[09. Part 5: Functional Pathway Enrichment & STRING Protein Interaction Networks](09_part-5-pathway-enrichment-and-string-ppi-networks.md)**
  - UniProt ID mapping to Entrez/Symbol.
  - GSEA and Over-Representation Analysis using `clusterProfiler`.
  - Extracting and charting physical Protein-Protein Interaction (PPI) subgraphs with `STRINGdb`.
- **[10. Part 6: Protein Co-expression Network Analysis (WGCNA)](10_part-6-protein-co-expression-networks-wgcna.md)**
  - Adapting WGCNA for continuous log-transformed proteomic intensities.
  - Module-trait association and hub protein identification.

### Advanced & Multi-Omics
- **[11. Part 7: Cross-Platform Discordance: Why MS, Olink, and SomaScan Disagree](11_part-7-cross-platform-discordance-ms-vs-olink-vs-somascan.md)**
  - Epitope effects: How missense variants fool affinity binders.
  - Protein complexes, PTMs, and isoform masking.
- **[12. Part 8: Population Plasma pQTL Mapping & Mendelian Randomization](12_part-8-plasma-pqtl-mapping-and-mendelian-randomization.md)**
  - Integrating genetics and plasma proteomics (*cis*-pQTL vs. *trans*-pQTL).
  - Causal inference for drug target validation.
- **[13. Part 9: Phosphoproteomics & Post-Translational Modification (PTM) Analysis](13_part-9-phosphoproteomics-and-ptm-site-analysis.md)**
  - Phosphosite localization probabilities (phosphoRS, A-score).
  - Kinase-Substrate Enrichment Analysis (KSEA).
- **[14. Part 10: Integrated Proteogenomics: Bulk RNA-seq vs. Proteomics](14_part-10-integrated-proteogenomics-bulk-rnaseq-vs-proteomics.md)**
  - Correlating RNA counts and protein abundance.
  - Quadrant scatter plots ($\log_2\text{FC}_{\text{RNA}}$ vs. $\log_2\text{FC}_{\text{Protein}}$).
- **[15. Part 11: Depositing and Reanalyzing Data in PRIDE & ProteomeXchange](15_part-11-submitting-data-to-pride-and-proteomexchange.md)**
  - Submitting datasets via the PX Submission Tool.

---

## Datasets Used in This Tutorial

1. **Mass Spectrometry (DIA)**:
   - *Cohort*: A549 lung cancer cell line treated with targeted inhibitor vs. DMSO vehicle (3 vs. 3 replicates).
   - *Public Accession*: PRIDE **[PXD014777](https://www.ebi.ac.uk/pride/archive/projects/PXD014777)**.
   - *Dual-Entry Option*: Precomputed `report.tsv` and `report.pg_matrix.tsv` tables are provided so learners on laptops can jump straight into R without running 6 hours of raw file extraction.
2. **Olink (PEA)**:
   - *Cohort*: MGH COVID-19 / ARDS Plasma Cohort (GSE157103) + `OlinkAnalyze::olink_extended_data`.
3. **SomaScan (SOMAmers)**:
   - *Cohort*: GEO GSE169680 plasma cohort + `SomaDataIO::example_data`.
4. **Proteogenomics**:
   - *Cohort*: CPTAC Clear Cell Renal Cell Carcinoma / DepMap cell lines with matched RNA-seq and Proteomics.

---

## Software Prerequisites

- **Linux / macOS environment** (or Windows with WSL2).
- **Miniconda / Mamba** for upstream tool management.
- **R (>= 4.3)** and **RStudio**.
- Key R packages:
  ```r
  BiocManager::install(c(
    "QFeatures", "MSstats", "DEP", "limma", 
    "ComplexHeatmap", "clusterProfiler", "STRINGdb"
  ))
  install.packages(c("tidyverse", "OlinkAnalyze"))
  devtools::install_github("SomaLogic/SomaDataIO")
  ```

