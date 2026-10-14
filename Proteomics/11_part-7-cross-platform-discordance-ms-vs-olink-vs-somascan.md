# How to Analyze Proteomics Data Part 7: Cross-Platform Discordance: Why MS, Olink, and SomaScan Disagree

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Benchmarking, Olink, SomaScan, Mass Spectrometry, Cross-Platform Discordance

---

## Table of Contents

- [Introduction](#introduction)
- [The Reality of Cross-Platform Concordance](#the-reality-of-cross-platform-concordance)
  - [The Overlap Landscape: How Well Do They Correlate?](#the-overlap-landscape-how-well-do-they-correlate)
  - [The Distribution of Correlation Coefficients](#the-distribution-of-correlation-coefficients)
- [The 5 Major Drivers of Proteomic Discordance](#the-5-major-drivers-of-proteomic-discordance)
  - [1. Epitope Effects & Genetic Missense Variants (The pQTL Trap)](#1-epitope-effects--genetic-missense-variants-the-pqtl-trap)
  - [2. Post-Translational Modifications (PTMs) & Glycan Masking](#2-post-translational-modifications-ptms--glycan-masking)
  - [3. Protein Complex Formation & Epitope Shielding](#3-protein-complex-formation--epitope-shielding)
  - [4. Alternative Splicing and Isoform Specificity](#4-alternative-splicing-and-isoform-specificity)
  - [5. Off-Target Binding and Cross-Reactivity](#5-off-target-binding-and-cross-reactivity)
- [Step 1: Quantifying Cross-Platform Concordance in R](#step-1-quantifying-cross-platform-concordance-in-r)
  - [Mapping Shared UniProt Identifiers](#mapping-shared-uniprot-identifiers)
  - [Calculating Pairwise Spearman Correlations](#calculating-pairwise-spearman-correlations)
- [Step 2: Classifying Concordant vs. Discordant Protein Targets](#step-2-classifying-concordant-vs-discordant-protein-targets)
  - [Visualizing High-Concordance Targets (e.g., VEGFA, IL6)](#visualizing-high-concordance-targets-eg-vegfa-il6)
  - [Investigating Zero-Concordance Outliers](#investigating-zero-concordance-outliers)
- [The Biomarker Triangulation Checklist](#the-biomarker-triangulation-checklist)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

As high-throughput proteomics expands into clinical biomarker trials, researchers frequently encounter an unsettling observation:

> **When the exact same plasma sample is measured using Mass Spectrometry, Olink PEA, and SomaScan, the quantification for the "same" protein often disagrees—sometimes showing zero or even negative correlation ($r \approx 0$).**

Large-scale comparative studies (e.g., Pietzner et al., *Science* 2021; Katz et al., *Circulation* 2022) have shown that cross-platform correlation across thousands of plasma proteins spans from $r = +0.95$ (near-perfect agreement) down to $r = -0.20$.

Why does this happen? Is one platform "right" and another "wrong"?

In this tutorial, we dissect the biophysical, genetic, and analytical reasons for cross-platform discordance. We will write R scripts to benchmark overlapping targets, identify discordant proteins, and apply a **Biomarker Triangulation Checklist** before taking protein candidates into clinical validation.

---

## The Reality of Cross-Platform Concordance

### The Overlap Landscape: How Well Do They Correlate?

When measuring overlapping proteins in clinical cohorts:
- $\approx 40\%$ of proteins show **high correlation** ($r \ge 0.70$).
- $\approx 35\%$ of proteins show **moderate correlation** ($0.40 \le r < 0.70$).
- $\approx 25\%$ of proteins show **poor or zero correlation** ($r < 0.40$).

```
        Distribution of Cross-Platform Spearman Correlations (Olink vs. SomaScan)
   Frequency
      ▲
      │                              ┌───┐
      │                              │   │
      │                     ┌───┐    │   │    ┌───┐
      │            ┌───┐    │   │    │   │    │   │
      │   ┌───┐    │   │    │   │    │   │    │   │
      └───┴───┴────┴───┴────┴───┴────┴───┴────┴───┴────► Spearman r
         -0.2     0.0      0.3      0.6      0.8     1.0
         ◄─────────────────►        ◄──────────────────►
         Discordant / Traps            High Concordance
```

---

## The 5 Major Drivers of Proteomic Discordance

```
                     WHY DO PROTEOMICS PLATFORMS DISAGREE?
                                       │
      ┌──────────────┬─────────────────┼─────────────────┬──────────────┐
      ▼              ▼                 ▼                 ▼              ▼
 [ EPITOPE      [ GLYCAN / PTM    [ COMPLEX         [ ISOFORM      [ OFF-TARGET
  MUTATIONS ]    MASKING ]         SHIELDING ]       BIAS ]         BINDING ]
Missense SNP   Sugar or phospho  Heterodimer       Antibody binds Non-specific
destroys       physically blocks buries epitope    exon 2; aptamer cross-reaction
binding site   probe docking     in core           binds exon 8   to paralog
```

### 1. Epitope Effects & Genetic Missense Variants (The pQTL Trap)
This is the single most common cause of false-positive genetic findings in affinity proteomics:
- An affinity binder (antibody or SOMAmer) recognizes a specific 3D pocket of 8–15 amino acids on the surface of protein $X$.
- A patient carries a common non-synonymous Single Nucleotide Polymorphism (SNP) that changes an amino acid inside that pocket (e.g., Arg $\to$ Leu).
- The protein is expressed at **identical physiological levels**, but the probe's binding affinity drops 50-fold.
- **The Result**: The affinity assay reports that the patient has "dramatically depleted protein $X$." Mass spectrometry, which measures denatured peptides across the entire length of the protein, shows that protein $X$ is completely normal!

### 2. Post-Translational Modifications (PTMs) & Glycan Masking
Proteins in plasma (e.g., immunoglobulins, complement factors, cell surface receptors) are heavily $N$-glycosylated and $O$-glycosylated. 
- Bulky carbohydrate trees can physically shield the epitope from an antibody or aptamer.
- Changes in patient disease state (e.g., cancer-associated altered glycosylation) can block probe binding without changing actual protein copy number.

### 3. Protein Complex Formation & Epitope Shielding
Mass spectrometry denatures and digests all proteins into linear tryptic peptides. Affinity platforms, however, measure **native, folded proteins in intact plasma**.
- If protein $A$ binds tightly to protein $B$ (forming a heterodimer), the binding epitope may be buried at the interaction interface.
- In this scenario, the affinity assay measures only the **uncomplexed (free) pool**, while mass spectrometry measures the **total pool**.

### 4. Alternative Splicing and Isoform Specificity
A gene may express multiple splice isoforms:
- Olink’s dual antibodies might target Exon 3 (present only in Isoform A).
- SomaScan’s SOMAmer might target Exon 7 (present only in Isoform B).
- Mass spectrometry may identify peptides shared by all isoforms.

### 5. Off-Target Binding and Cross-Reactivity
Proteins often belong to large gene families with homologous structural paralogs (e.g., Matrix Metalloproteinases: MMP-1 through MMP-28). A SOMAmer or antibody intended for MMP-1 may cross-react with MMP-8, inflating the apparent concentration.

---

## Step 1: Quantifying Cross-Platform Concordance in R

Let's write a practical R pipeline to compare quantification across platforms:

```r
library(tidyverse)

setwd("~/Projects/learning_omics/Proteomics_Workspace")

# Load Olink NPX data (simulated overlapping cohort)
olink_data <- read.csv("results/tables/DAP_olink_results.csv", stringsAsFactors = FALSE)

# Load SomaScan RFU data
soma_data  <- read.csv("results/tables/DAP_somascan_results.csv", stringsAsFactors = FALSE)

# Load Mass Spec MaxLFQ data
ms_data    <- read.csv("results/tables/DAP_results_limma.csv", stringsAsFactors = FALSE)
```

### Mapping Shared UniProt Identifiers

To compare platforms, match proteins using standardized **UniProt Accessions**:

```r
# Find overlapping proteins measured across both affinity platforms
overlap_uniprot <- intersect(olink_data$UniProt, soma_data$UniProt)
cat("Overlapping protein targets between Olink and SomaScan:", length(overlap_uniprot), "\n")
```

---

### Calculating Pairwise Spearman Correlations

```r
# Function to calculate Spearman correlation across matching samples
# (Assuming aligned sample expression matrices)
calc_concordance <- function(vec_olink, vec_soma) {
  cor(vec_olink, vec_soma, method = "spearman", use = "pairwise.complete.obs")
}

# Example benchmarking dataframe
concordance_df <- data.frame(
  UniProt = c("P15692", "P05231", "P02778", "P01375", "P04040", "P18146"),
  Gene    = c("VEGFA",  "IL6",    "CXCL10", "TNF",    "CAT",    "EGR1"),
  Spearman_r = c(0.89,    0.84,     0.92,     0.78,     0.12,     -0.08),
  Category   = c("Concordant", "Concordant", "Concordant", "Concordant", "Discordant", "Discordant")
)

print(concordance_df)
```

```text
  UniProt   Gene Spearman_r   Category
1  P15692  VEGFA       0.89 Concordant
2  P05231    IL6       0.84 Concordant
3  P02778 CXCL10       0.92 Concordant
4  P01375    TNF       0.78 Concordant
5  P04040    CAT       0.12 Discordant
6  P18146   EGR1      -0.08 Discordant
```

VEGFA, IL6, and CXCL10 show high concordance ($r > 0.8$), while intracellular enzymes and transcription factors show low agreement in plasma.

---

## Step 2: Classifying Concordant vs. Discordant Protein Targets

### Visualizing High-Concordance Targets (e.g., VEGFA, IL6)

When two platforms agree, a scatter plot shows a linear relationship:

```r
# Simulate concordant scatter plot for VEGFA
set.seed(42)
n_samples <- 100
true_conc <- rnorm(n_samples, mean = 5, sd = 1.5)
olink_vegfa <- true_conc + rnorm(n_samples, sd = 0.3)
soma_vegfa  <- true_conc * 1.2 + rnorm(n_samples, sd = 0.4)

df_vegfa <- data.frame(Olink_NPX = olink_vegfa, SomaScan_log2RFU = soma_vegfa)

p_vegfa <- ggplot(df_vegfa, aes(x = Olink_NPX, y = SomaScan_log2RFU)) +
  geom_point(color = "#2B5C8F", size = 2.5, alpha = 0.8) +
  geom_smooth(method = "lm", color = "black", linetype = "dashed") +
  theme_bw() +
  labs(
    title = "High Concordance: VEGFA (Spearman r = 0.89)",
    subtitle = "Both platforms measure the same physiological signal",
    x = "Olink NPX",
    y = "SomaScan Log2 RFU"
  )

ggsave("results/figures/Fig7a_Concordant_VEGFA.png", p_vegfa, width = 6, height = 4.5)
```

---

### Investigating Zero-Concordance Outliers

When platforms disagree, the points form a random cloud. What should an analyst do?

```
               HOW TO INVESTIGATE A DISCORDANT TARGET:
                                 │
     1. Check for common missense SNPs in the gene locus (cis-pQTL).
     2. Check if the protein has multiple distinct splice isoforms.
     3. Check if the protein binds a high-affinity carrier protein in blood.
     4. Inspect Mass Spectrometry peptide data to confirm intact protein existence.
```

---

## The Biomarker Triangulation Checklist

Before investing thousands of dollars validating a candidate biomarker discovered on any single platform, run this **Biomarker Triangulation Checklist**:

1. [ ] **Cross-Platform Verification**: Does the candidate biomarker validate on an orthogonal technology (e.g., Mass Spec or high-sensitivity ELISA)?
2. [ ] **Variant Screening**: Does the protein locus harbor a common missense mutation that coincides with your phenotype?
3. [ ] **Detection Frequency Check**: Is the protein detected well above the Limit of Detection (LOD)?
4. [ ] **Directional Consistency**: Does the direction of change match between platforms (up-regulated in both)?
5. [ ] **Biological Plausibility**: Is the protein known to be secreted into plasma, or is it an intracellular structural component leaking from dying cells?

---

## Conclusion & What’s Next

In this guide, you learned:
1. The empirical reality of **cross-platform discordance**: why 20–30% of proteins show poor correlation across platforms.
2. The 5 major biophysical drivers: **epitope SNPs, glycan masking, complex shielding, alternative splicing, and off-target binding**.
3. How to compute pairwise **Spearman concordance distributions** in R.
4. How to apply the **Biomarker Triangulation Checklist** to protect against clinical false positives.

Now that we understand how genetic mutations can artificially distort protein binding, we are ready to explore the genetic basis of the proteome itself!

In **[Part 8: Population Plasma pQTL Mapping & Mendelian Randomization](12_part-8-plasma-pqtl-mapping-and-mendelian-randomization.md)**, we integrate genomics with proteomics to map *cis*- and *trans*-pQTLs and perform causal drug target prioritization!
