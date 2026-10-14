# How to Analyze Proteomics Data Part 1: The Proteomics Landscape & Environment Setup

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Mass Spectrometry, Olink, SomaScan

---

## Table of Contents

- [Introduction](#introduction)
- [Why Study Proteomics When We Have RNA-seq?](#why-study-proteomics-when-we-have-rna-seq)
- [The Three Major Platforms of Modern Proteomics](#the-three-major-platforms-of-modern-proteomics)
  - [1. Discovery Mass Spectrometry (LC-MS/MS)](#1-discovery-mass-spectrometry-lc-msms)
  - [2. Proximity Extension Assay (Olink PEA)](#2-proximity-extension-assay-olink-pea)
  - [3. Slow Off-rate Modified Aptamers (SomaScan)](#3-slow-off-rate-modified-aptamers-somascan)
- [Platform Comparison Matrix](#platform-comparison-matrix)
- [Setting Up Your Analysis Environment](#setting-up-your-analysis-environment)
  - [Step 1: Setting Up the Linux/Conda Environment](#step-1-setting-up-the-linuxconda-environment)
  - [Step 2: Setting Up the R & Bioconductor Environment](#step-2-setting-up-the-r--bioconductor-environment)
- [Project Directory Architecture](#project-directory-architecture)
- [The Dual-Entry Tutorial Philosophy](#the-dual-entry-tutorial-philosophy)
- [Glossary of Key Terms](#glossary-of-key-terms)
- [Conclusion & What's Next](#conclusion--whats-next)

---

## Introduction

In our [Bulk RNA-seq Tutorial Series](../BulkRNAseq/01_part-1-setting-up-the-environment.md), we explored how to quantify the entire transcriptome—from raw sequencing reads to differentially expressed genes and pathway mechanisms. RNA sequencing provides an invaluable genome-wide readout of transcriptional activity. 

However, **RNA is merely the message; proteins are the machinery of life.**

Proteins catalyze biochemical reactions, form structural scaffolds, mediate intracellular signaling cascades, and serve as targets for >95% of FDA-approved therapeutics. Historically, high-throughput proteomics lagged behind genomics due to the vast dynamic range of the proteome and the absence of an enzymatic amplification mechanism analogous to PCR. Today, rapid technological advancements in **Data-Independent Acquisition (DIA) Mass Spectrometry** and **high-throughput affinity assays (Olink and SomaScan)** have brought proteomics into the mainstream omics toolkit.

This tutorial series guides you through modern proteomics data analysis, covering raw spectra processing, rigorous quality control, missing value management, differential abundance testing, and multi-omics integration.

---

## Why Study Proteomics When We Have RNA-seq?

A frequent question from beginners is: *"Can’t I just use mRNA expression as a proxy for protein abundance?"*

Across multiple benchmark studies (e.g., Gygi Lab, CPTAC consortia), the genome-wide correlation between mRNA expression and protein abundance in human cells ranges between **$r \approx 0.40$ and $0.60$**. In complex biofluids like plasma or serum, this correlation drops even further.

```
       DNA
        │  Transcription
        ▼
      mRNA   ───► [Degradation / miRNA targeting / Splicing]
        │  Translation efficiency (Ribosome occupancy)
        ▼
     Protein  ───► [Post-Translational Modifications: Phosphorylation, Glycosylation]
        │
        ▼
  Active State ───► [Complex formation / Ubiquitination / Proteasomal turnover]
```

Several biological processes explain this divergence:
1. **Translational Regulation**: Secondary mRNA structures, uORFs, and microRNA inhibition affect translation efficiency independently of mRNA levels.
2. **Differential Half-Lives**: Protein half-lives span from minutes (e.g., transcription factors like c-Myc) to days (e.g., structural collagens and histones), whereas mRNA half-lives are typically hours.
3. **Post-Translational Modifications (PTMs)**: Phosphorylation, ubiquitination, and acetylation regulate protein enzymatic activity and stability without altering transcript abundance.
4. **Post-Transcriptional Buffering**: Excess subunits of stoichiometric protein complexes (e.g., ribosomes, proteasomes) are rapidly degraded if their binding partners are absent, regardless of how much mRNA is transcribed.

Measuring proteins directly is essential for understanding actual cellular phenotypes and disease states.

---

## The Three Major Platforms of Modern Proteomics

Today's proteomics landscape is divided into two distinct technological philosophies: **Discovery Mass Spectrometry** (unbiased, bottom-up) and **Targeted Affinity-based Proteomics** (high-throughput, biofluid-optimized).

```
                            MODERN PROTEOMICS
                                    │
       ┌────────────────────────────┴────────────────────────────┐
       ▼                                                         ▼
[ DISCOVERY UNBIASED ]                                [ TARGETED AFFINITY ]
• Bottom-up LC-MS/MS                                  • Biofluids (Plasma, Serum, CSF)
• DDA (MaxQuant, FragPipe)                            • Olink PEA (Dual Antibodies + DNA tag)
• DIA (DIA-NN, Spectronaut)                           • SomaScan (Modified DNA Aptamers)
• Measures peptide mass-to-charge                     • Measures intact tertiary epitopes
```

### 1. Discovery Mass Spectrometry (LC-MS/MS)
In "bottom-up" liquid chromatography-tandem mass spectrometry:
1. Intact proteins are enzymatically digested into smaller peptides (typically using trypsin).
2. Peptides are separated by hydrophobicity via high-performance liquid chromatography (HPLC).
3. Eluting peptides are ionized (via Electrospray Ionization, ESI) and enter the mass spectrometer.
4. The instrument measures the mass-to-charge ratio ($m/z$) and intensity of intact precursor ions (**MS1**), fragments them (via Collision-Induced Dissociation or HCD), and records the fragment ion spectra (**MS2**).

Within LC-MS/MS, two acquisition modes dominate:
* **DDA (Data-Dependent Acquisition)**: The mass spectrometer selects the Top-N most intense MS1 peaks and isolates them individually for MS2 fragmentation. *Limitation*: Stochastic sampling leads to significant missing values across runs.
* **DIA (Data-Independent Acquisition)**: The instrument systematically steps through broad precursor isolation windows (e.g., 32 sequential windows of $25\text{ }m/z$, covering $400\text{--}1200\text{ }m/z$) and fragments *all* co-eluting ions together. Combined with modern deep-learning deconvolution tools like **DIA-NN**, DIA provides superior reproducibility, deeper proteome coverage, and lower missingness.

### 2. Proximity Extension Assay (Olink PEA)
Developed by Olink Proteomics, PEA combines dual-antibody specificity with the quantitative sensitivity of next-generation sequencing:
1. A pair of antibodies specific to the same protein target binds simultaneously.
2. Each antibody is covalently conjugated to a unique single-stranded DNA oligonucleotide.
3. When both antibodies bind their target in close spatial proximity, the DNA tags hybridize.
4. DNA polymerase extends the hybridized oligos, creating a synthetic DNA amplicon.
5. Amplicons are quantified via microfluidic qPCR (Olink Target 96) or next-generation sequencing (Olink Explore 3072 / HT).
* **Metric**: **NPX (Normalized Protein eXpression)**, an arbitrary $\log_2$ relative scale.

### 3. Slow Off-rate Modified Aptamers (SomaScan)
Developed by SomaLogic, SomaScan uses synthetic, chemically modified single-stranded DNA aptamers called **SOMAmers**:
1. SOMAmers incorporate modified nucleotides (mimicking amino acid side chains like benzyl, tryptophan, or naphthyl groups) that fold into defined 3D structures.
2. SOMAmers bind to unique conformational tertiary epitopes on intact proteins with high affinity and slow dissociation rates.
3. Protein-bound SOMAmers are captured on streptavidin beads, washed, UV-cleaved, and quantified via DNA hybridization microarrays or NGS.
* **Metric**: **RFU (Relative Fluorescence Units)**, typically analyzed in $\log_2$ space.

---

## Platform Comparison Matrix

| Feature | Discovery LC-MS/MS (DIA) | Olink Explore (PEA) | SomaScan v4.1 / v5 |
| :--- | :--- | :--- | :--- |
| **Core Principle** | Physical mass/charge of tryptic peptides | Dual antibody binding + DNA hybridization | Modified DNA aptamer tertiary binding |
| **Proteome Breadth** | Unbiased discovery (6,000–12,000 proteins) | Targeted panel (~3,000–5,400 proteins) | Targeted panel (~7,000–11,000 SOMAmers) |
| **Primary Sample Types** | Cell lines, tissue lysates, organoids | Plasma, serum, CSF, saliva, interstitial fluid | Plasma, serum, urine, vitreous humor |
| **Volume Required** | High ($10\text{--}50\,\mu\text{g}$ protein) | Extremely low ($1\text{--}5\,\mu\text{L}$ plasma) | Low ($55\,\mu\text{L}$ plasma) |
| **Data Units** | Raw continuous ion intensity / MaxLFQ | **NPX** ($\log_2$ relative scale) | **RFU** (Relative Fluorescence Units) |
| **Missing Data Rate** | 15%–40% (Limit-of-Detection dropouts) | <5% (Flags values below LOD) | Dense matrix (~0% dropouts) |
| **Epitope Sensitivity** | Immune to conformational/missense artifacts | Susceptible to missense SNPs disrupting binding | Susceptible to off-target & epitope-loss artifacts |
| **Computational Entry** | 10–50 GB `.raw` / `.mzML` files | Flat `.csv` / `.parquet` table | Structured `.adat` file |

---

## Setting Up Your Analysis Environment

To analyze both mass spectrometry and affinity datasets, we configure a modern computational environment using **Conda/Mamba** for upstream command-line tools and **R/Bioconductor** for downstream statistical modeling.

### Step 1: Setting Up the Linux/Conda Environment

Open your terminal and create a dedicated Conda environment named `proteomics_env`:

```bash
# 1. Create the environment with Python and core utilities
mamba create -n proteomics_env python=3.10 -y
conda activate proteomics_env

# 2. Install ThermoRawFileParser (for converting Thermo .raw to .mzML on Linux/Mac)
mamba install -c bioconda thermorawfileparser -y

# 3. Install DIA-NN (available via bioconda or native binary)
mamba install -c bioconda dia-nn -y

# 4. Verify installation
thermorawfileparser --version
diann --help
```

> [!NOTE]
> If you are on an Apple Silicon Mac (M1/M2/M3/M4), DIA-NN is best executed using its official Linux container (Docker/Singularity) or run directly on your institution's Linux HPC cluster, as the native DIA-NN binary utilizes AVX-512/x86 vector instructions.

---

### Step 2: Setting Up the R & Bioconductor Environment

Downstream statistical analysis, quality control, and biological visualization are conducted in R (version $\ge 4.3$).

Launch R or RStudio and install the core toolchains:

```r
# 1. Install BiocManager if not already installed
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# 2. Core Bioconductor Proteomics Infrastructure
BiocManager::install(c(
  "QFeatures",        # Modern quantitative proteomics multi-assay container
  "MSstats",          # Statistical linear mixed-effects modeling for MS
  "DEP",              # Differential Enrichment of Proteomics (user-friendly LFQ pipeline)
  "limma",            # Empirical Bayes moderated t-testing
  "ComplexHeatmap",   # High-resolution publication heatmaps
  "clusterProfiler",  # Gene Ontology & KEGG pathway enrichment
  "org.Hs.eg.db",     # Human genome annotation database
  "STRINGdb"          # Protein-protein interaction networks
))

# 3. CRAN & Specialized Packages
install.packages(c(
  "tidyverse",        # ggplot2, dplyr, tidyr, readr
  "patchwork",        # Multi-panel figure layouts
  "OlinkAnalyze"      # Official Olink normalization, QC, and stats
))

# 4. Install SomaDataIO for SomaScan .adat parsing (GitHub / Bioconductor)
if (!requireNamespace("remotes", quietly = TRUE)) install.packages("remotes")
remotes::install_github("SomaLogic/SomaDataIO")
```

Verify that all packages load properly:

```r
library(QFeatures)
library(MSstats)
library(OlinkAnalyze)
library(SomaDataIO)
library(limma)
cat("Proteomics analysis environment successfully initialized!\n")
```

---

## Project Directory Architecture

Following bioinformatics best practices, organize your proteomics workspace logically:

```bash
mkdir -p ~/Projects/learning_omics/Proteomics_Workspace/{data/raw,data/processed,databases,metadata,results/figures,scripts}
```

```text
Proteomics_Workspace/
├── data/
│   ├── raw/             # .raw files, .mzML files, raw .adat files
│   └── processed/       # DIA-NN report.tsv, normalized matrices, NPX files
├── databases/           # UniProt human FASTA, cRAP contaminant FASTA
├── metadata/            # Sample annotations, experimental design, batch IDs
├── results/
│   ├── figures/         # PCA, Volcano, Heatmaps, Boxplots
│   └── tables/          # Differentially abundant protein tables (DAP.csv)
└── scripts/             # Reproducible R and Shell scripts
```

---

## The Dual-Entry Tutorial Philosophy

Processing 20 to 50 GB of raw mass spectrometry files requires substantial compute and bandwidth. We recognize that learners work across different computing environments:

```
              ┌───────────────────────────────────────────────┐
              │           CHOOSE YOUR ENTRY POINT             │
              └───────────────────────────────────────────────┘
                                      │
         ┌────────────────────────────┴────────────────────────────┐
         ▼                                                         ▼
[ HIGH-PERFORMANCE ENTRY ]                                [ LAPTOP ENTRY ]
• Have access to Linux Server / HPC                      • Working locally on personal laptop
• Want to learn DIA-NN command line                      • Want to focus on QC & R statistics
• Download PRIDE raw spectra files                       • Download precomputed report.tsv (15 MB)
• Start with Part 2a                                     • Start with Part 2b
```

1. **High-Performance Entry**: Complete instructions, curl commands, and scripts to process raw `.raw` spectra with DIA-NN.
2. **Laptop Entry**: Direct download links for precomputed, tidy quantification tables (`report.tsv` / `matrix.tsv`) so you can jump straight into exploratory data analysis, imputation, and differential testing in R.

---

## Glossary of Key Terms

- **Bottom-Up Proteomics**: An analytical workflow where proteins are proteolytically digested into peptides prior to mass spectrometric analysis.
- **cRAP**: The *Common Repository of Adventitious Proteins*. A reference database containing common laboratory contaminants (e.g., human keratins, trypsin autolysis products, bovine serum albumin) that must be identified and removed from analysis.
- **DDA**: *Data-Dependent Acquisition*. The mass spectrometer measures MS1 precursor scans and dynamically selects the top N highest-intensity peaks for MS2 fragmentation.
- **DIA**: *Data-Independent Acquisition*. The instrument fragments all precursor ions within sequential wide $m/z$ isolation windows, providing reproducible quantification across runs.
- **LOD**: *Limit of Detection*. The lowest analyte concentration that can be reliably distinguished from analytical background noise.
- **MaxLFQ**: A label-free quantification algorithm that extracts accurate protein ratios by preserving pairwise peptide ratio consistency across samples.
- **NPX**: *Normalized Protein eXpression*. Olink's arbitrary $\log_2$ relative quantification unit. A difference of $1.0\text{ NPX}$ corresponds to a two-fold difference in protein abundance.
- **PSM**: *Peptide-Spectrum Match*. An identification assigned by matching an observed experimental MS2 fragmentation spectrum against a theoretical in silico peptide database.
- **RFU**: *Relative Fluorescence Units*. The intensity metric generated by SomaScan microarray scanning.
- **SOMAmer**: *Slow Off-rate Modified Aptamer*. A chemically modified single-stranded DNA oligonucleotide engineered to bind specific protein epitopes with high affinity.

---

## Conclusion & What's Next

You now understand the structural differences between discovery mass spectrometry and targeted affinity assays, and your analysis environment is configured.

In **[Part 2a: From Raw Spectra to Protein Quantification Matrix with DIA-NN](02_part-2a-ms-from-raw-spectra-to-counts-diann-best-practices.md)**, we dive into Track A: downloading public mass spectrometry runs from PRIDE, generating in silico spectral libraries from UniProt, controlling false discovery rates (FDR), and generating our quantitative protein matrix.

