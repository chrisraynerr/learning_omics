# How to Analyze Proteomics Data Part 3c: Affinity Differential Abundance and Handling Limits of Detection (LOD)

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Olink, SomaScan, Differential Abundance, LOD, ANCOVA, limma

---

## Table of Contents

- [Introduction](#introduction)
- [The LOD Dilemma: How to Treat Values Below Detection](#the-lod-dilemma-how-to-treat-values-below-detection)
  - [The Three Strategies for Below-LOD Values](#the-three-strategies-for-below-lod-values)
  - [Why Olink Recommends Keeping Observed Values](#why-olink-recommends-keeping-observed-values)
  - [The Detection Frequency Filter (Best Practices)](#the-detection-frequency-filter-best-practices)
- [Differential Abundance in Olink with OlinkAnalyze](#differential-abundance-in-olink-with-olinkanalyze)
  - [Two-Group Testing (t-tests)](#two-group-testing-t-tests)
  - [Controlling for Confounders with ANCOVA](#controlling-for-confounders-with-ancova)
  - [Longitudinal Modeling with Linear Mixed Models (lmer)](#longitudinal-modeling-with-linear-mixed-models-lmer)
- [Differential Abundance in SomaScan with limma](#differential-abundance-in-somascan-with-limma)
  - [Building the Model with Clinical Covariates](#building-the-model-with-clinical-covariates)
  - [Extracting Significant SOMAmers](#extracting-significant-somamers)
- [Affinity vs. Mass Spec: Why Biomarker Profiles Differ](#affinity-vs-mass-spec-why-biomarker-profiles-differ)
- [Exporting Affinity DAP Tables](#exporting-affinity-dap-tables)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In [Part 3a](05_part-3a-olink-qc-npx-normalization-and-olinkanalyze.md) and [Part 3b](06_part-3b-somascan-adat-qc-anml-and-somadataio.md), we explored the upstream QC and normalization of Olink PEA and SomaScan SOMAmer datasets.

Now we tackle **differential abundance analysis in affinity proteomics**.

Unlike mass spectrometry (where missing values are widespread and require complex imputation), affinity platforms deliver **dense matrices** where almost every sample has a recorded number. 

However, affinity data introduces its own unique challenges:
1. **Values below the Limit of Detection (LOD)**: When a low-abundance cytokine falls below the negative control threshold, how should it be modeled?
2. **Clinical Confounders**: Blood biomarker cohorts are frequently confounded by patient **Age, Sex, Body Mass Index (BMI), and Collection Site**. Failing to adjust for these covariates generates false-positive disease biomarkers.

In this tutorial, we learn how to statistically address LOD thresholds, run covariate-adjusted models (ANCOVA and linear mixed models) using **`OlinkAnalyze`**, and apply **`limma`** to large-scale SomaScan cohorts.

---

## The LOD Dilemma: How to Treat Values Below Detection

When an assay's observed signal falls below the plate's **Limit of Detection (LOD)**, the machine still records a numerical reading, but that reading carries higher technical noise.

### The Three Strategies for Below-LOD Values

Historically, analysts have handled below-LOD values using three different approaches:

```
                  HOW SHOULD YOU TREAT VALUES BELOW LOD?
                                    │
       ┌────────────────────────────┼────────────────────────────┐
       ▼                            ▼                            ▼
[ 1. FIXED REPLACEMENT ]     [ 2. SET TO NA & IMPUTE ]    [ 3. KEEP OBSERVED NPX ]
(e.g., Replace with LOD/2)   (Heuristic imputation)       (Official Recommendation)
• Creates synthetic spike    • Adds artificial variance   • Preserves ranking & distribution
• Artificially deflates p-val • Distorts distribution      • Prevents boundary inflation
```

1. **Fixed Value Substitution (LOD / 2 or LOD / $\sqrt{2}$)**:
   * *The Method*: Every sample below LOD is replaced by a constant number (e.g., $\text{LOD} - 1.0$).
   * *The Problem*: Replaces continuous variance with a synthetic spike at the boundary. If 40% of Control samples are replaced with the exact same constant, the sample variance collapses to 0, producing artificially tiny $p$-values and massive false positives!
2. **Set to NA & Impute**:
   * *The Problem*: Unnecessary for affinity assays because the instrument already collected a real, continuous optical or sequencing readout.
3. **Keep Actual Observed Values (The Consensus Recommendation)**:
   * *The Method*: Even if an observed NPX falls below the nominal LOD, keep the actual measurement.
   * *Why it works*: Biological signals below LOD still follow an approximately normal distribution centered around the background mean. Keeping the observed value preserves rank ordering and linear modeling assumptions.

### Why Olink Recommends Keeping Observed Values

Official Olink white papers and epidemiological consortia (like UK Biobank) strongly recommend:
> **Do not censor, truncate, or replace NPX values below LOD with constants.** Perform statistical testing on the continuous observed NPX values.

---

### The Detection Frequency Filter (Best Practices)

While we keep continuous values, we should **exclude assays that are undetectable in almost all samples**:
- If an assay has $< 25\%$ of samples above LOD across the entire cohort, its values reflect random background noise rather than biological signal.

```r
library(OlinkAnalyze)
library(tidyverse)

# Filter out assays with low detection frequency
data("olink_extended_data", package = "OlinkAnalyze")

# Calculate detection frequency per assay
detectable_assays <- olink_extended_data %>%
  group_by(Assay) %>%
  summarise(Pct_Above_LOD = mean(NPX > LOD, na.rm = TRUE) * 100) %>%
  filter(Pct_Above_LOD >= 25.0) %>%
  pull(Assay)

# Filter dataset to retain only reliably detected assays
df_filtered <- olink_extended_data %>%
  filter(Assay %in% detectable_assays) %>%
  filter(QC_Warning == "Pass") # Also drop failed sample wells

cat("Assays passing detection threshold (>=25% above LOD):", length(detectable_assays), "\n")
```

---

## Differential Abundance in Olink with OlinkAnalyze

### Two-Group Testing (t-tests)

For simple two-condition experiments (e.g., Healthy vs. Disease), `OlinkAnalyze` provides `olink_ttest()`:

```r
# Welch's two-sample t-test across all assays
ttest_results <- olink_ttest(
  df = df_filtered,
  variable = "Treatment",
  alternative = "two.sided"
)

# Inspect top differentially abundant proteins
head(ttest_results %>% select(Assay, OlinkID, estimate, p.value, Adjusted_pval, Threshold), 5)
```

```text
  Assay   OlinkID estimate      p.value Adjusted_pval   Threshold
1   IL6  OID00001 2.841029 1.481029e-12  1.362547e-10 Significant
2 CXCL10 OID00003 3.120491 3.204192e-11  1.473928e-09 Significant
3   TNF  OID00002 1.942104 5.920410e-09  1.815593e-07 Significant
4 VEGFA  OID00004 1.421094 8.104291e-08  1.863987e-06 Significant
5  IL10  OID00008 1.849102 1.140291e-07  2.098135e-06 Significant
```
* `estimate`: The difference in mean NPX between groups ($\Delta\text{NPX} = \log_2\text{FC}$). For IL6, $\Delta\text{NPX} = 2.84$, meaning IL6 is $2^{2.84} \approx 7.16\times$ higher in Treated patients!
* `Adjusted_pval`: False Discovery Rate (Benjamini-Hochberg).

---

### Controlling for Confounders with ANCOVA

In clinical plasma cohorts, inflammatory proteins rise with age and differ between sexes. Failing to adjust for them creates false discoveries:

```r
# Simulate clinical covariates for demonstration
set.seed(42)
sample_ids <- unique(df_filtered$SampleID)
covariates <- data.frame(
  SampleID = sample_ids,
  Age = rnorm(length(sample_ids), mean = 55, sd = 12),
  Sex = sample(c("M", "F"), length(sample_ids), replace = TRUE)
)

df_with_covariates <- left_join(df_filtered, covariates, by = "SampleID")

# Fit ANCOVA adjusting for Treatment, Age, and Sex
ancova_results <- olink_anova(
  df = df_with_covariates,
  variable = "Treatment",
  covariates = c("Age", "Sex")
)

head(ancova_results %>% select(Assay, term, estimate, p.value, Adjusted_pval), 5)
```

By adding `Age` and `Sex` as covariates, we isolate the true disease effect from demographic confounding.

---

### Longitudinal Modeling with Linear Mixed Models (lmer)

Many clinical studies track patients over multiple time points (e.g., Baseline vs. Day 3 vs. Day 7). Because repeated measurements from the same patient are correlated, we use **Linear Mixed-Effects Models** (`olink_lmer()`):

```r
# Linear mixed-effects model with random intercept for each patient
lmer_results <- olink_lmer(
  df = df_with_covariates,
  variable = "Time",
  random = "SubjectID",
  covariates = c("Age", "Sex")
)
```

---

## Differential Abundance in SomaScan with limma

For SomaScan's large 7,000–11,000 SOMAmer panels, **`limma`** is the gold standard engine for fitting linear models:

```r
library(SomaDataIO)
library(SummarizedExperiment)
library(limma)

# Load normalized SummarizedExperiment from Part 3b
# (Containing log2-transformed RFU)
adat_file <- system.file("extdata", "example_data.adat", package = "SomaDataIO")
adat <- read_adat(adat_file)
se_soma <- as.SummarizedExperiment(adat)

# Extract log2 RFU matrix
expr_mat <- log2(assay(se_soma, "exprs"))
col_meta <- as.data.frame(colData(se_soma))
row_meta <- as.data.frame(rowData(se_soma))

# Filter to biological samples only
is_sample <- col_meta$SampleType == "Sample"
expr_mat  <- expr_mat[, is_sample]
col_meta  <- col_meta[is_sample, ]

# 1. Build design matrix with clinical variables
# Example: Testing SampleGroup adjusting for PlateId
design <- model.matrix(~ SampleGroup + PlateId, data = col_meta)

# 2. Fit moderated linear models with empirical Bayes
fit <- lmFit(expr_mat, design)
fit <- eBayes(fit, trend = TRUE, robust = TRUE)

# 3. Extract top differential SOMAmers
dap_soma <- topTable(fit, coef = "SampleGroupDisease", number = Inf, adjust.method = "BH")
dap_soma$SeqId <- rownames(dap_soma)
dap_soma <- left_join(dap_soma, row_meta, by = "SeqId")

head(dap_soma %>% select(Target, UniProt, logFC, P.Value, adj.P.Val), 5)
```

---

## Affinity vs. Mass Spec: Why Biomarker Profiles Differ

When comparing DAP lists between Mass Spectrometry and Affinity platforms on the same disease, researchers frequently notice striking differences:

| Metric | Discovery Mass Spectrometry | Affinity Assays (Olink / SomaScan) |
| :--- | :--- | :--- |
| **Cytokines & Chemokines (IL-6, TNF, IFN-$\gamma$)** | Rarely detected in neat plasma ($< 1\text{ pg/mL}$ detection limit) | Strongly detected and quantified with high sensitivity |
| **High-Abundance Tissue Enzymes (LDHA, GAPDH)** | Readily identified with dozens of peptides | Often absent from affinity panels |
| **Structural Integrity** | Measures denatured tryptic peptides | Measures intact tertiary protein folds |
| **Target Identity** | Verified by physical mass spectrum ($m/z$) | Inferred by binding affinity (subject to cross-reactivity) |

Affinity proteomics excels at **signaling molecules, cytokines, and shed receptors in blood**, while mass spectrometry excels at **cellular pathways, metabolic enzymes, and structural complexes in tissues**.

---

## Exporting Affinity DAP Tables

Save your finalized differential abundance tables:

```r
# Save Olink DAPs
write.csv(ttest_results, "results/tables/DAP_olink_results.csv", row.names = FALSE)

# Save SomaScan DAPs
write.csv(dap_soma, "results/tables/DAP_somascan_results.csv", row.names = FALSE)

cat("Affinity differential abundance analysis complete!\n")
```

---

## Conclusion & What’s Next

In this guide, you learned:
1. The consensus strategy for **Limits of Detection (LOD)**: why you should keep continuous observed NPX values rather than substituting artificial constants.
2. How to run Welch's t-tests, ANCOVA, and linear mixed-effects models in **`OlinkAnalyze`**.
3. How to perform large-scale empirical Bayes linear modeling on SomaScan panels with **`limma`**.
4. Why plasma affinity biomarker signatures differ fundamentally from mass spectrometry proteomes.

Now that we have discovered significant proteins across both Mass Spectrometry and Affinity tracks, **the two tracks converge!**

In **[Part 4: Creating Publication-Ready Figures: Volcano Plots, Heatmaps, and PCA](08_part-4-creating-publication-ready-figures-volcano-heatmaps-pca.md)**, we build beautiful, high-resolution scientific figures using `ggplot2` and `ComplexHeatmap`!
