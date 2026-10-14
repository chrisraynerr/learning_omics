# How to Analyze Proteomics Data Part 3a: Olink Proximity Extension Assay: QC, Normalization, and OlinkAnalyze

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Olink, PEA, Plasma Proteomics, OlinkAnalyze, Quality Control

---

## Table of Contents

- [Introduction](#introduction)
- [How Olink Proximity Extension Assay (PEA) Works](#how-olink-proximity-extension-assay-pea-works)
  - [The PEA Mechanism: Dual Antibodies + DNA Tag](#the-pea-mechanism-dual-antibodies--dna-tag)
  - [Olink Panels: Target 96 vs. Explore 3072 / HT](#olink-panels-target-96-vs-explore-3072--ht)
- [Understanding the NPX Scale](#understanding-the-npx-scale)
  - [What is NPX?](#what-is-npx)
  - [Mathematical Interpretation](#mathematical-interpretation)
- [Step 1: Setting Up the R Environment](#step-1-setting-up-the-r-environment)
- [Step 2: Loading Olink Data](#step-2-loading-olink-data)
  - [Long-Format NPX Structure](#long-format-npx-structure)
  - [Inspecting Sample Annotations](#inspecting-sample-annotations)
- [Step 3: Quality Control & Flagging](#step-3-quality-control--flagging)
  - [The Four Internal Controls](#the-four-internal-controls)
  - [Evaluating Sample QC Warnings](#evaluating-sample-qc-warnings)
  - [Evaluating Assay (Protein) QC Warnings](#evaluating-assay-protein-qc-warnings)
  - [QC Diagnostic Plots with OlinkAnalyze](#qc-diagnostic-plots-with-olinkanalyze)
- [Step 4: Handling Limits of Detection (LOD)](#step-4-handling-limits-of-detection-lod)
  - [How LOD is Calculated](#how-lod-is-calculated)
  - [Assessing Assay Detectability Rates](#assessing-assay-detectability-rates)
- [Step 5: Multi-Plate Bridging Normalization](#step-5-multi-plate-bridging-normalization)
  - [Why Plate Normalization is Essential](#why-plate-normalization-is-essential)
  - [Bridging Normalization Across Plates](#bridging-normalization-across-plates)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In **Track A (Parts 2a–2c)**, we explored unbiased discovery mass spectrometry. While LC-MS/MS is unbeatable for deeply characterizing cell lines and tissue lysates, it encounters severe limitations in human blood plasma:
- **Dynamic range**: In plasma, the top 10 most abundant proteins (albumin, immunoglobulins, transferrin) make up >90% of total protein mass, crowding out low-abundance signaling cytokines (like IL-6, TNF, and CXCL10).
- **Throughput**: Analyzing 1,000 plasma samples by high-resolution LC-MS/MS takes weeks of continuous instrument time.

To solve this, **Targeted Affinity Proteomics** was developed. In this chapter, we explore **Olink's Proximity Extension Assay (PEA)**, the technology powering massive epidemiological biobanks such as the **UK Biobank Pharma Proteomics Project (UKB-PPP)**.

Unlike mass spectrometry, Olink delivers an already assembled, high-throughput matrix with near-zero missing values. However, analyzing Olink data requires mastering its unique quality control metrics, plate-bridging algorithms, and the **NPX** relative quantification scale.

---

## How Olink Proximity Extension Assay (PEA) Works

### The PEA Mechanism: Dual Antibodies + DNA Tag

The ingenuity of Olink's PEA is that it solves antibody non-specific cross-reactivity by requiring a **dual-recognition event**:

```
      Antibody A                          Antibody B
   (with DNA Oligo A)                  (with DNA Oligo B)
          \                                  /
           \                                /
            ▼                              ▼
        ┌──────────────────────────────────────┐
        │            TARGET PROTEIN            │
        └──────────────────────────────────────┘
                           │
      Both antibodies bind simultaneously in close proximity
                           │
                           ▼
          Oligo A and Oligo B hybridize!
                           │
                           ▼
              DNA Polymerase Extension
                           │
                           ▼
        Synthetic DNA Amplicon Formed
                           │
                           ▼
       Quantified via microfluidic qPCR or NGS readout
```

1. **Dual Antibody Binding**: Two separate polyclonal antibodies—each conjugated to unique single-stranded oligonucleotides (Oligo A and Oligo B)—bind to adjacent epitopes on the same target protein.
2. **Proximity Hybridization**: Only when both antibodies bind the *same* protein molecule do Oligos A and B come close enough to hybridize. Free antibodies floating in solution never hybridize.
3. **DNA Polymerase Extension**: A DNA polymerase extends the hybridized oligonucleotides, creating a double-stranded DNA reporter amplicon.
4. **Digital Quantification**: The resulting amplicon is amplified and counted either by microfluidic qPCR (for 96-plex panels) or Next-Generation Sequencing (for Explore 1536 / 3072 / HT panels).

### Olink Panels: Target 96 vs. Explore 3072 / HT

- **Olink Target 96 / 48**: Focuses on specific disease pathways (e.g., Inflammation, Cardiovascular, Oncology, Neurology). Read out by microfluidic real-time qPCR (Fluidigm BioMark).
- **Olink Explore 3072 / HT**: High-throughput library targeting >3,000 to 5,400 human proteins across plasma or serum cohorts. Read out by Illumina NovaSeq sequencing, matching sequencing reads directly to protein identity.

---

## Understanding the NPX Scale

### What is NPX?

When you receive data from an Olink service provider or public repository, the values are reported in **NPX (Normalized Protein eXpression)**.

NPX is an arbitrary **$\log_2$ relative quantification unit**:
- Higher NPX values indicate higher protein concentrations.
- It is a **relative** metric, not an absolute concentration (you cannot directly convert NPX to $\text{pg/mL}$ without an external recombinant protein standard curve).

### Mathematical Interpretation

Because NPX is in $\log_2$ space:
$$\Delta\text{NPX} = \text{NPX}_{\text{Sample A}} - \text{NPX}_{\text{Sample B}}$$
$$\text{Fold Change} = 2^{\Delta\text{NPX}}$$

- An increase of **$1.0\text{ NPX}$** corresponds to a **2-fold increase** in protein concentration.
- An increase of **$2.0\text{ NPX}$** corresponds to a **4-fold increase**.
- A difference of **$0.5\text{ NPX}$** corresponds to a **$1.41$-fold change** ($\sqrt{2}$).

---

## Step 1: Setting Up the R Environment

Olink provides an official, highly optimized R package on CRAN called **`OlinkAnalyze`**.

```r
# Install OlinkAnalyze from CRAN if needed
if (!requireNamespace("OlinkAnalyze", quietly = TRUE)) {
  install.packages("OlinkAnalyze")
}

library(OlinkAnalyze)
library(tidyverse)
library(patchwork)

setwd("~/Projects/learning_omics/Proteomics_Workspace")
```

---

## Step 2: Loading Olink Data

### Long-Format NPX Structure

Olink export files (`.csv` or `.parquet`) are typically formatted as a **tidy long table** rather than a wide matrix. Each row represents a single measurement of one protein in one sample.

Let's load the built-in multi-plate cohort dataset included with `OlinkAnalyze`:

```r
# Load standard benchmark dataset from OlinkAnalyze
# Contains 4 plates measuring human plasma proteins
data("olink_extended_data", package = "OlinkAnalyze")

# Assign to our working variable
df_olink <- olink_extended_data

# Inspect dimensions and structure
glimpse(df_olink)
```

```text
Rows: 29,440
Columns: 13
$ SampleID      <chr> "SAMPLE_001", "SAMPLE_001", "SAMPLE_001", ...
$ Index         <int> 1, 2, 3, 4, 5, ...
$ Assay         <chr> "IL6", "TNF", "CXCL10", "VEGFA", ...
$ UniProt       <chr> "P05231", "P01375", "P02778", "P15692", ...
$ OlinkID       <chr> "OID00001", "OID00002", "OID00003", ...
$ Panel         <chr> "Inflammation", "Inflammation", ...
$ PlateID       <chr> "Plate_1", "Plate_1", "Plate_1", ...
$ QC_Warning    <chr> "Pass", "Pass", "Pass", "Warning", ...
$ LOD           <dbl> 0.24, 0.45, 0.12, 0.55, ...
$ NPX           <dbl> 4.82, 3.12, 7.89, 5.41, ...
$ MissingFreq   <dbl> 0.01, 0.00, 0.00, 0.02, ...
$ Treatment     <chr> "Control", "Control", "Control", ...
$ Time          <chr> "Baseline", "Baseline", ...
```

---

## Step 3: Quality Control & Flagging

### The Four Internal Controls

Every well in an Olink 96-well or 384-well plate is spiked with four technical internal controls:

1. **Incubation Control 1 & 2**: Non-human antigen spiked into the reaction with its matching antibody pair. Monitors antibody binding and incubation temperature.
2. **Extension Control**: A synthetic double-stranded DNA template. Monitors DNA polymerase extension efficiency.
3. **Detection Control**: A synthetic DNA template. Monitors qPCR amplification or NGS adapter ligation.

### Evaluating Sample QC Warnings

If any of the internal controls in a well deviate by more than $\pm 0.3\text{ NPX}$ from the median of the plate, that sample receives a **`QC_Warning = "Warning"`**:

```r
# Tabulate sample QC results
sample_qc_summary <- df_olink %>%
  group_by(SampleID, PlateID) %>%
  summarise(Overall_QC = ifelse(any(QC_Warning == "Warning"), "Warning", "Pass"), .groups = "drop")

table(sample_qc_summary$Overall_QC)
```

```text
Pass Warning 
 182      10 
```

Out of 192 samples, 10 samples failed technical quality thresholds (due to pipetting inaccuracies, hemolyzed plasma, or lipid interference). In downstream differential abundance testing, samples marked `"Warning"` are typically excluded.

---

### QC Diagnostic Plots with OlinkAnalyze

`OlinkAnalyze` provides dedicated visualization functions:

```r
# 1. Plot Sample QC across plates
p_qc <- olink_qc_plot(df_olink, color_g = "QC_Warning") +
  theme_bw() +
  labs(title = "Olink Sample QC Status Across Plates")

# 2. PCA Plot to detect sample outliers or batch effects
p_pca <- olink_pca_plot(df_olink, color_g = "PlateID") +
  theme_bw() +
  labs(title = "PCA of Samples Colored by Plate ID")

# Combine plots
p_combined <- p_qc / p_pca
ggsave("results/figures/olink_qc_diagnostics.png", p_combined, width = 8, height = 9)
```

---

## Step 4: Handling Limits of Detection (LOD)

### How LOD is Calculated

On every plate, negative control wells (containing buffer instead of plasma) are included. The **Limit of Detection (LOD)** for each assay is defined as:
$$\text{LOD} = \text{Mean}(\text{Negative Controls}) + 3 \times \text{SD}(\text{Negative Controls})$$

If a sample's observed NPX falls below this threshold, its true concentration cannot be reliably distinguished from technical background noise.

### Assessing Assay Detectability Rates

Let's check which proteins are detectable across our cohort:

```r
# Calculate percentage of samples above LOD for each protein
lod_summary <- df_olink %>%
  mutate(Above_LOD = NPX > LOD) %>%
  group_by(Assay, Panel) %>%
  summarise(
    Detectability_Pct = round(100 * mean(Above_LOD, na.rm = TRUE), 1),
    Mean_NPX = mean(NPX, na.rm = TRUE),
    .groups = "drop"
  ) %>%
  arrange(Detectability_Pct)

head(lod_summary, 8)
```

```text
# A tibble: 8 × 4
  Assay   Panel        Detectability_Pct Mean_NPX
  <chr>   <chr>                    <dbl>    <dbl>
1 IL2     Inflammation              12.4     0.31
2 TSLP    Inflammation              24.1     0.52
3 IL33    Inflammation              48.2     0.94
4 IL10    Inflammation              88.5     2.41
5 CXCL10  Inflammation             100.0     7.82
6 VEGFA   Inflammation             100.0     5.65
7 IL6     Inflammation             100.0     4.21
8 TNF     Inflammation             100.0     3.89
```

> [!NOTE]
> High-abundance circulating cytokines (like VEGFA, CXCL10, IL6) are detected in 100% of samples. Low-abundance tissue-resident cytokines (like IL-2 and TSLP) are detected in only 12–24% of healthy plasma samples.
> In **Part 3c**, we will learn how to statistically handle assays with low detectability without introducing bias!

---

## Step 5: Multi-Plate Bridging Normalization

### Why Plate Normalization is Essential

In large clinical cohorts (e.g., hundreds or thousands of patients), samples cannot fit onto a single 96-well plate. Because different plates are run with different reagent lots or on different days, subtle plate-level baseline shifts occur:

```r
# Boxplot of raw NPX across plates shows plate-to-plate baseline shifts
p_plates <- ggplot(df_olink, aes(x = PlateID, y = NPX, fill = PlateID)) +
  geom_boxplot(alpha = 0.7, outlier.size = 0.5) +
  theme_bw() +
  labs(
    title = "Pre-Normalization: Inter-Plate Baseline Drift",
    x = "Plate ID",
    y = "Raw NPX"
  ) +
  theme(legend.position = "none")

ggsave("results/figures/olink_plate_drift.png", p_plates, width = 6, height = 4)
```

### Bridging Normalization Across Plates

To eliminate plate effects without erasing biological differences, Olink workflows include **Bridge Samples** (identical pooled reference plasma samples run across all plates).

The bridging normalization calculates the median difference for each assay in the bridge samples and subtracts it:

```r
# Perform bridging normalization across plates
# (Assuming bridge samples are annotated in SampleID or PlateID)
df_normalized <- olink_normalization(
  df = df_olink,
  overlapping_samples_df1 = df_olink %>% filter(grepl("BRIDGE", SampleID)),
  df1_project_nr = "Project_Plate1",
  df2_project_nr = "Project_Plate2",
  reference_project = "Project_Plate1"
)

cat("Bridging normalization complete. Plate batch effects removed!\n")
```

---

## Conclusion & What’s Next

In this guide, you learned:
1. The **Proximity Extension Assay (PEA)** mechanism: how dual antibodies and synthetic DNA tags solve cross-reactivity.
2. The mathematical meaning of the **NPX scale** ($\log_2$ relative quantification).
3. How to evaluate **internal controls** and filter out samples flagged with `QC_Warning`.
4. How the **Limit of Detection (LOD)** is calculated and how to assess assay detectability.
5. How **bridge samples** remove plate-to-plate technical variance across large cohorts.

Now let's examine the competing affinity-based platform: **SomaScan modified DNA aptamers**.

In **[Part 3b: SomaScan Aptamer Proteomics: .adat Parsing and ANML Normalization](06_part-3b-somascan-adat-qc-anml-and-somadataio.md)**, we explore how synthetic modified aptamers measure 7,000+ proteins and how to process raw `.adat` files using `SomaDataIO`!
