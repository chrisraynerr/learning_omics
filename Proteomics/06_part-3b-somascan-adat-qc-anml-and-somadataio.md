# How to Analyze Proteomics Data Part 3b: SomaScan Aptamer Proteomics: .adat Parsing and ANML Normalization

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, SomaScan, Aptamers, SOMAmers, SomaDataIO, Normalization

---

## Table of Contents

- [Introduction](#introduction)
- [How SomaScan Works: The Biology of SOMAmers](#how-somascan-works-the-biology-of-somamers)
  - [What are SOMAmers?](#what-are-somamers)
  - [The SomaScan Multi-Step Assay Cycle](#the-somascan-multi-step-assay-cycle)
- [The Anatomy of a .adat File](#the-anatomy-of-a-adat-file)
  - [What is an .adat Table?](#what-is-an-adat-table)
  - [Header, Feature Metadata, and Expression Matrix](#header-feature-metadata-and-expression-matrix)
- [Step 1: Setting Up the R Environment with SomaDataIO](#step-1-setting-up-the-r-environment-with-somadataio)
- [Step 2: Loading and Exploring .adat Files](#step-2-loading-and-exploring-adat-files)
  - [Reading .adat Data](#reading-adat-data)
  - [Accessing Sample Annotations and Feature Metadata](#accessing-sample-annotations-and-feature-metadata)
  - [Converting to a Bioconductor SummarizedExperiment](#converting-to-a-bioconductor-summarizedexperiment)
- [Step 3: SomaLogic's Three-Tier Normalization Pipeline](#step-3-somalogics-three-tier-normalization-pipeline)
  - [1. Hybridization Control Normalization](#1-hybridization-control-normalization)
  - [2. Plate Scaling (Calibrator Normalization)](#2-plate-scaling-calibrator-normalization)
  - [3. Adaptive Normalization by Maximum Likelihood (ANML)](#3-adaptive-normalization-by-maximum-likelihood-anml)
- [Step 4: Quality Control & Filtering Non-Human Controls](#step-4-quality-control--filtering-non-human-controls)
  - [Filtering Control SOMAmers](#filtering-control-somamers)
  - [Evaluating Sample Type Hierarchy](#evaluating-sample-type-hierarchy)
  - [Checking ANML Scaling Factors for Outliers](#checking-anml-scaling-factors-for-outliers)
- [Step 5: Log-Transformation & Distribution Checks](#step-5-log-transformation--distribution-checks)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In [Part 3a](05_part-3a-olink-qc-npx-normalization-and-olinkanalyze.md), we explored antibody-based proximity extension assays (Olink). 

Now, we examine the other major titan of high-throughput biofluid proteomics: **SomaLogic’s SomaScan platform**.

Rather than relying on natural antibodies, SomaScan uses **synthetic, chemically modified single-stranded DNA aptamers** called **SOMAmers**. SomaScan assays (such as the v4.1 7K panel and the latest 11K panel) can measure between **7,000 and 11,000 protein targets** simultaneously from just $55\,\mu\text{L}$ of human plasma or serum.

SomaScan data is distributed in a unique proprietary file format known as the **`.adat`** (Annotated Data Table). In this guide, we demystify the `.adat` format, explore SomaLogic's automated **ANML** normalization pipeline, and show you how to parse and quality-control SomaScan data in R using the official **`SomaDataIO`** package.

---

## How SomaScan Works: The Biology of SOMAmers

### What are SOMAmers?

Standard RNA/DNA aptamers typically bind nucleic-acid binding proteins via electrostatic interactions. However, they struggle to bind hydrophobic protein surfaces with high affinity.

SomaLogic overcame this limitation by synthesizing **SOMAmers (Slow Off-rate Modified Aptamers)**:
- They incorporate synthetic, chemically modified nucleotides at the 5-position of uridine (e.g., **benzyl-dU, naphthyl-dU, tryptaminocarbonyl-dU**).
- These chemical appendages mimic the hydrophobic side chains of amino acids (phenylalanine, tryptophan, tyrosine), allowing the DNA aptamer to fold into complex 3D hydrophobic pockets that fit snugly into protein surface clefts.
- They exhibit slow dissociation kinetics ($t_{1/2} > 30\text{ minutes}$), ensuring tight binding.

```
                  MODIFIED NUCLEOTIDE (e.g., Benzyl-dU)
                             O
                           // \
                      HN  C    C - CH2 - [ Benzyl Ring ]  ◄── Hydrophobic moiety
                      │   │    │
                      O = C    CH
                           \  /
                            N
                            │
                     [ Deoxyribose ] ── [ Phosphate Backbone ]
```

---

### The SomaScan Multi-Step Assay Cycle

To achieve high specificity without secondary antibodies, SomaScan uses a dual-bead catch-and-release mechanism:

```
1. BINDING: Biotinylated SOMAmers (with UV-cleavable linkers) bind intact proteins in plasma.
                 │
                 ▼
2. CATCH-1: Complexes captured on Streptavidin beads via biotin. Unbound proteins washed away.
                 │
                 ▼
3. TAGGING: Bound proteins labeled with a chemical NHS-biotin tag.
                 │
                 ▼
4. PHOTO-CLEAVAGE: Ultraviolet (UV) light cleaves the linker, releasing SOMAmer-protein complexes.
                 │
                 ▼
5. CATCH-2: Complexes recaptured on fresh Streptavidin beads (via the protein's new biotin tag).
            Nonspecific aptamers are washed away.
                 │
                 ▼
6. QUANTIFICATION: Pure SOMAmers eluted with high pH, hybridized to custom DNA microarrays (or NGS).
```

Because each SOMAmer has a predefined DNA sequence, measuring fluorescence intensity at a specific microarray spot directly quantifies that protein's abundance in **Relative Fluorescence Units (RFU)**.

---

## The Anatomy of a .adat File

### What is an .adat Table?

When you receive SomaScan data, it arrives as a plain-text file ending in `.adat`. 

An `.adat` file is not a simple CSV; it contains three distinct components packed into one file:
1. **Header Metadata**: Instrument version, date, experiment ID, and normalization parameters.
2. **ColMetaData (Sample Annotations)**: Patient demographics, sample IDs, plate positions, and dilution bins (e.g., 40%, 1%, 0.005%).
3. **RowMetaData (Feature Annotations)**: SOMAmer sequence ID (`SeqId`), Target Name, UniProt ID, Entrez Gene Symbol, and dilution group.
4. **Data Matrix**: Samples $\times$ SOMAmer RFU measurements.

---

## Step 1: Setting Up the R Environment with SomaDataIO

SomaLogic maintains an official Bioconductor/GitHub package called **`SomaDataIO`**:

```r
# Install SomaDataIO if needed
if (!requireNamespace("SomaDataIO", quietly = TRUE)) {
  if (!requireNamespace("remotes", quietly = TRUE)) install.packages("remotes")
  remotes::install_github("SomaLogic/SomaDataIO")
}

library(SomaDataIO)
library(SummarizedExperiment)
library(tidyverse)
library(patchwork)

setwd("~/Projects/learning_omics/Proteomics_Workspace")
```

---

## Step 2: Loading and Exploring .adat Files

### Reading .adat Data

`SomaDataIO` provides the `read_adat()` parser, which handles the complex header and parses both metadata and intensities:

```r
# Load example dataset included with SomaDataIO
# Measures 1,305 SOMAmers across human plasma samples
adat_file <- system.file("extdata", "example_data.adat", package = "SomaDataIO")
adat <- read_adat(adat_file)

# Inspect class and dimensions
class(adat)
# [1] "soma_adat"  "tbl_df"  "tbl"  "data.frame"
dim(adat)
# [1]   20 1324
```
* The table contains 20 samples and 1,324 columns (sample metadata columns + SOMAmer features).

---

### Accessing Sample Annotations and Feature Metadata

`SomaDataIO` includes helper accessors:

```r
# 1. Extract sample metadata (ColMetaData)
meta_samples <- getMeta(adat)
head(meta_samples %>% select(SampleId, SampleType, PlateId, SlideId), 4)

# 2. Extract SOMAmer feature annotations (RowMetaData)
meta_features <- getFeatureData(adat)
head(meta_features %>% select(SeqId, Target, UniProt, EntrezGeneSymbol, Organism), 4)
```

```text
# Sample Annotations:
  SampleId  SampleType PlateId SlideId
1  SMPL_01      Sample Plate_1  Slide_1
2  SMPL_02      Sample Plate_1  Slide_1
3  SMPL_03      Sample Plate_1  Slide_1
4  SMPL_04      Sample Plate_1  Slide_1

# Feature Annotations:
     SeqId      Target UniProt EntrezGeneSymbol Organism
1 10000-28       MAPK1  P28482            MAPK1    Human
2 10001-7       VEGFA  P15692            VEGFA    Human
3 10002-12        IL6  P05231              IL6    Human
4 10003-8     CXCL10  P02778           CXCL10    Human
```

---

### Converting to a Bioconductor SummarizedExperiment

To integrate SomaScan data seamlessly into standard Bioconductor tools (`limma`, `ComplexHeatmap`), convert the `soma_adat` object into a `SummarizedExperiment`:

```r
# Convert to SummarizedExperiment
se_soma <- as.SummarizedExperiment(adat)

# Inspect container
se_soma
```

```text
class: SummarizedExperiment 
dim: 1305 20 
metadata(3): Header Col.Meta Row.Meta
assays(1): exprs
rownames(1305): seq.10000.28 seq.10001.7 ... seq.11304.5
rowData names(14): SeqId Target UniProt EntrezGeneSymbol ...
colnames(20): SMPL_01 SMPL_02 ... SMPL_20
colData names(19): SampleId SampleType PlateId ...
```

---

## Step 3: SomaLogic's Three-Tier Normalization Pipeline

Raw microarray images undergo three automated calibration steps before clinical reporting:

```
  [ Raw Microarray Hybridization ]
                 │
                 ▼
  [ 1. Hybridization Normalization ] ──► Corrects spot-to-spot scanner intensity differences
                 │                       using 12 spiked internal control SOMAmers.
                 ▼
  [ 2. Plate Scaling / Calibration ] ──► Standardizes overall signal across 96-well plates
                 │                       using multi-well Calibrator plasma pools.
                 ▼
  [ 3. ANML Normalization ]          ──► Equalizes total protein content across samples
                                         within predetermined physiological bounds.
```

### 1. Hybridization Control Normalization
Each well is spiked with 12 synthetic control SOMAmers that have no biological target. A single scaling factor per sample is calculated by comparing these controls to a reference standard, eliminating optical scanning artifacts.

### 2. Plate Scaling (Calibrator Normalization)
Every 96-well plate contains 3–5 replicate wells of a reference plasma pool (**Calibrator samples**). Plate-specific scaling factors adjust all assays so that the calibrator median matches the historical platform baseline.

### 3. Adaptive Normalization by Maximum Likelihood (ANML)
Biological samples have different total protein concentrations (e.g., fasting vs. fed plasma, hydration levels). 

Unlike standard total-sum normalization (which can be distorted if a few acute-phase proteins spike), **ANML** finds a scaling factor that aligns the sample's non-responsive proteins to an external reference distribution using maximum likelihood estimation.

---

## Step 4: Quality Control & Filtering Non-Human Controls

### Filtering Control SOMAmers

An `.adat` file contains technical probes alongside human protein targets:
- Hybridization controls (`HybControl`)
- Non-cleavable linker controls (`Non-Cleavable`)
- Spurious or non-human probes (`Mouse`, `Rat`, `Spurious`)

Filter these out to retain only true human targets:

```r
# Check Organism and Type annotations
table(rowData(se_soma)$Organism)

# Filter rowData to keep only Human protein targets
keep_somamers <- rowData(se_soma)$Organism == "Human" & 
                 !grepl("HybControl|Non-Cleavable|Buffer", rowData(se_soma)$Target)

se_human <- se_soma[keep_somamers, ]
cat("Retained", nrow(se_human), "human protein targets.\n")
```

---

### Evaluating Sample Type Hierarchy

SomaScan runs include multiple technical sample types in the `SampleType` metadata column:
- `Sample`: True biological experimental samples.
- `Calibrator`: Plate calibration pools.
- `Buffer`: Negative control buffer wells (measures analytical background).
- `QC`: Quality control reference aliquots used to calculate coefficients of variation (%CV).

For biological differential abundance analysis, **subset only biological samples**:

```r
# Filter colData to retain only true experimental samples
is_biological_sample <- colData(se_human)$SampleType == "Sample"

se_final <- se_human[, is_biological_sample]
cat("Biological samples retained for analysis:", ncol(se_final), "\n")
```

---

### Checking ANML Scaling Factors for Outliers

SomaLogic records the ANML sample scaling factor in the metadata column `NormScale_40` (or `NormScale_all`).

In healthy, high-quality plasma runs, ANML scaling factors typically fall within **$0.7$ and $1.3$**. If a sample has a scaling factor $> 2.0$ or $< 0.4$, it indicates an severely diluted or hemolyzed sample that should be flagged:

```r
# Check ANML scaling factor distribution
anml_scales <- colData(se_final)$NormScale_40

# Summary of scaling factors
summary(anml_scales)
```

---

## Step 5: Log-Transformation & Distribution Checks

SomaScan RFU values are continuous and right-skewed. Just as with mass spectrometry, standard linear models require **$\log_2$ transformation**:

```r
# Extract RFU assay matrix
rfu_matrix <- assay(se_final, "exprs")

# Apply log2 transformation
log2_rfu <- log2(rfu_matrix)

# Attach log2 assay back to SummarizedExperiment
assay(se_final, "log2_exprs") <- log2_rfu
```

Visualize the distribution across samples before and after log-transformation:

```r
# Plot before vs after log2
p_raw <- ggplot(data.frame(RFU = as.vector(rfu_matrix[, 1:4])), aes(x = RFU)) +
  geom_density(fill = "#4E79A7", alpha = 0.5) +
  theme_bw() +
  labs(title = "Raw RFU (Heavily Skewed)", x = "RFU")

p_log <- ggplot(data.frame(Log2RFU = as.vector(log2_rfu[, 1:4])), aes(x = Log2RFU)) +
  geom_density(fill = "#E15759", alpha = 0.5) +
  theme_bw() +
  labs(title = "Log2 RFU (Symmetric & Stabilized)", x = "Log2 RFU")

p_dist <- p_raw + p_log
ggsave("results/figures/somascan_log_transformation.png", p_dist, width = 8, height = 4)
```

---

## Conclusion & What’s Next

In this guide, you learned:
1. The biochemistry of **SOMAmers** and how modified nucleotides enable high-affinity binding.
2. The structure of `.adat` files and how to parse them in R with **`SomaDataIO`**.
3. SomaLogic’s three-stage calibration pipeline: **Hybridization Controls $\to$ Plate Scaling $\to$ ANML**.
4. How to filter out control SOMAmers and non-biological calibrator/buffer wells.
5. How to convert and log-transform RFU into a clean Bioconductor `SummarizedExperiment`.

Now that we have explored both Olink (NPX) and SomaScan (RFU), we are ready to test for differential abundance in targeted affinity cohorts.

In **[Part 3c: Affinity Differential Abundance and Handling Limits of Detection (LOD)](07_part-3c-affinity-differential-abundance-and-lod-handling.md)**, we learn how to handle values below LOD, apply moderated linear models (`limma`), and run ANCOVA to discover plasma disease biomarkers!
