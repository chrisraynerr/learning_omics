# How to Analyze Proteomics Data Part 2b: Data QC, Filtering, and the Missing Value Dilemma

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Mass Spectrometry, Data QC, Missing Values, Imputation, QFeatures

---

## Table of Contents

- [Introduction](#introduction)
- [Step 1: Setting Up Your R Session and Loading Data](#step-1-setting-up-your-r-session-and-loading-data)
  - [Install and Load Required Packages](#install-and-load-required-packages)
  - [Read the DIA-NN Protein Matrix](#read-the-dia-nn-protein-matrix)
  - [Create Sample Metadata (colData)](#create-sample-metadata-coldata)
  - [Construct a QFeatures Object](#construct-a-qfeatures-object)
- [Step 2: Filtering Contaminants and Decoys](#step-2-filtering-contaminants-and-decoys)
  - [Why Filter Contaminants?](#why-filter-contaminants)
  - [Removing cRAP Proteins and Reverse Hits in R](#removing-crap-proteins-and-reverse-hits-in-r)
- [Step 3: Log2 Transformation & Sample Normalization](#step-3-log2-transformation--sample-normalization)
  - [Why Log2 Transformation?](#why-log2-transformation)
  - [Median Normalization (Equalizing Loading Differences)](#median-normalization-equalizing-loading-differences)
- [Step 4: Exploring the Missing Value Dilemma](#step-4-exploring-the-missing-value-dilemma)
  - [Why Are There Missing Values in Mass Spectrometry?](#why-are-there-missing-values-in-mass-spectrometry)
  - [MAR vs. MNAR: Two Different Biological Realities](#mar-vs-mnar-two-different-biological-realities)
  - [Visualizing Missingness vs. Protein Abundance](#visualizing-missingness-vs-protein-abundance)
- [Step 5: Filtering by Valid Values](#step-5-filtering-by-valid-values)
  - [The "At Least X Valid Values in One Group" Rule](#the-at-least-x-valid-values-in-one-group-rule)
  - [Preserving Biological On/Off Switches](#preserving-biological-onoff-switches)
- [Step 6: Imputation Strategies: Theory & Practice](#step-6-imputation-strategies-theory--practice)
  - [Left-Censored Imputation (Perseus Gaussian Downshift)](#left-censored-imputation-perseus-gaussian-downshift)
  - [MAR Imputation (k-Nearest Neighbors / kNN)](#mar-imputation-k-nearest-neighbors--knn)
  - [Visualizing the Impact of Imputation](#visualizing-the-impact-of-imputation)
- [Step 7: Exporting Clean Data for Differential Testing](#step-7-exporting-clean-data-for-differential-testing)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In [Part 2a](02_part-2a-ms-from-raw-spectra-to-counts-diann-best-practices.md), we ran DIA-NN and generated `report.pg_matrix.tsv`. If you inspect this table in R or Excel, you will immediately notice three major differences compared to an RNA-seq count table:

1. **The numbers are continuous ion intensities**, not discrete read counts.
2. **Some proteins are marked as cRAP contaminants** (keratins, trypsin, albumin).
3. **Many entries are blank or `NaN` / `NA`** (missing values).

In Bulk RNA-seq, a gene with 0 reads simply has 0 observed counts, and negative binomial models (DESeq2/edgeR) handle these naturally. In mass spectrometry, however, an `NA` does not necessarily mean the protein was absent from the cell—it often means the peptide signal fell below the instrument’s **Limit of Detection (LOD)**.

In this tutorial, we will transition into **R**, load our dataset into the Bioconductor **`QFeatures`** infrastructure, filter out technical artifacts, explore missingness patterns, and apply scientifically grounded imputation methods.

---

## Step 1: Setting Up Your R Session and Loading Data

### Install and Load Required Packages

Launch R or RStudio and load the necessary libraries:

```r
# Load core packages
library(QFeatures)      # Modern multi-assay container for proteomics
library(tidyverse)      # Data manipulation and ggplot2
library(ComplexHeatmap) # Publication heatmaps
library(patchwork)      # Plot alignment

# Set random seed for reproducibility
set.seed(42)

# Set working directory
setwd("~/Projects/learning_omics/Proteomics_Workspace")
```

---

### Read the DIA-NN Protein Matrix

Read the MaxLFQ protein groups matrix generated in Part 2a:

```r
# Read DIA-NN protein group matrix
raw_pg <- read.delim(
  "data/processed/report.pg_matrix.tsv",
  sep = "\t",
  stringsAsFactors = FALSE,
  check.names = FALSE
)

# Inspect dimensions and first columns
dim(raw_pg)
# [1] 7842    9
head(colnames(raw_pg))
```

The table contains annotation columns (`Protein.Group`, `Protein.Names`, `Genes`) followed by sample intensity columns (`A549_Ctrl_Rep1`, `A549_Ctrl_Rep2`, etc.).

---

### Create Sample Metadata (colData)

Statistical modeling requires a metadata table describing the experimental design:

```r
# Identify intensity columns
sample_cols <- grep("A549_", colnames(raw_pg), value = TRUE)

# Create sample annotation dataframe
colData <- data.frame(
  SampleID = sample_cols,
  Condition = ifelse(grepl("Ctrl", sample_cols), "Control", "Treated"),
  Replicate = gsub(".*_Rep", "Rep", sample_cols),
  stringsAsFactors = FALSE
)
rownames(colData) <- colData$SampleID

print(colData)
```

```text
               SampleID Condition Replicate
A549_Ctrl_Rep1 A549_Ctrl_Rep1   Control      Rep1
A549_Ctrl_Rep2 A549_Ctrl_Rep2   Control      Rep2
A549_Ctrl_Rep3 A549_Ctrl_Rep3   Control      Rep3
A549_Trt_Rep1   A549_Trt_Rep1   Treated      Rep1
A549_Trt_Rep2   A549_Trt_Rep2   Treated      Rep2
A549_Trt_Rep3   A549_Trt_Rep3   Treated      Rep3
```

---

### Construct a QFeatures Object

The Bioconductor package **`QFeatures`** is the gold standard container for quantitative mass spectrometry. It manages multiple assay layers (raw, filtered, $\log_2$-transformed, normalized, imputed) while preserving feature annotations and sample metadata.

```r
# Separate expression assay from feature metadata
assay_mat <- as.matrix(raw_pg[, sample_cols])
rownames(assay_mat) <- raw_pg$Protein.Group

# DIA-NN outputs 0 or NaN for missing peaks; convert 0s to NA
assay_mat[assay_mat == 0] <- NA

# Feature annotations (protein names and gene symbols)
row_meta <- raw_pg[, c("Protein.Group", "Protein.Names", "Genes")]
rownames(row_meta) <- raw_pg$Protein.Group

# Build QFeatures container
qf <- readQFeatures(
  assayData = as.data.frame(assay_mat),
  colData = colData,
  name = "raw"
)

# Attach feature metadata to the 'raw' assay
rowData(qf[["raw"]]) <- DataFrame(row_meta)

qf
```

```text
An instance of class 'QFeatures' containing 1 assays:
 [1] raw: SummarizedExperiment with 7842 rows and 6 columns
```

---

## Step 2: Filtering Contaminants and Decoys

### Why Filter Contaminants?
During sample lysis, reduction, and tryptic digestion, small amounts of keratin (from laboratory personnel skin/hair) or bovine serum albumin (from media) are inevitably introduced. These proteins must be removed so they do not distort normalization or inflate false discoveries.

### Removing cRAP Proteins and Reverse Hits in R

Contaminants from the cRAP database carry identifiers beginning with `CONTAM_`, `cRAP_`, or accession tags like `ALBU_BOVIN` and `KRT`:

```r
# Inspect current assay
rd <- rowData(qf[["raw"]])

# Identify contaminant rows
is_contaminant <- grepl("CONTAM|cRAP|KRT[0-9]|ALBU_BOVIN", rd$Protein.Names) |
                  grepl("CONTAM|cRAP", rd$Protein.Group)

# Identify reverse decoy hits (if any)
is_decoy <- grepl("REV__|DECOY__", rd$Protein.Group)

# Total to remove
table(is_contaminant | is_decoy)
# FALSE  TRUE 
#  7754    88 

# Filter the QFeatures object
qf <- filterFeatures(qf, ~ !(is_contaminant | is_decoy))
qf
```

We removed 88 contaminant proteins, leaving 7,754 biological human protein groups.

---

## Step 3: Log2 Transformation & Sample Normalization

### Why Log2 Transformation?
Raw MS intensities span 6 to 7 orders of magnitude ($10^3$ to $10^9$) and are strongly right-skewed. Taking the $\log_2$ stabilizes the variance across abundance ranges (homoscedasticity) and converts multiplicative changes into additive changes ($1.0\text{ }\log_2\text{FC} = 2\text{-fold change}$).

```r
# Add log2-transformed assay layer to QFeatures
qf <- logTransform(qf, i = "raw", name = "log2_raw")
```

### Median Normalization (Equalizing Loading Differences)
Even with careful pipetting, total protein amounts injected into the LC-MS column vary slightly between runs. A common practice is **median centering**, which shifts each sample's intensity distribution so that all sample medians are identical:

```r
# Median-center across samples
qf <- normalize(
  qf,
  i = "log2_raw",
  name = "normalized",
  method = "center.median"
)

# Verify the assay layers now present in our QFeatures object
names(qf)
# [1] "raw"        "log2_raw"   "normalized"
```

Let's visualize the effect of normalization across our 6 runs:

```r
# Extract normalized matrix for plotting
norm_mat <- assay(qf[["normalized"]])

# Boxplot comparing sample distributions
pdf("results/figures/qc_sample_distributions.pdf", width = 8, height = 5)
par(mar = c(7, 4, 2, 1))
boxplot(
  norm_mat,
  col = ifelse(colData$Condition == "Control", "#4E79A7", "#E15759"),
  las = 2,
  ylab = "Log2 Intensity",
  main = "Median-Normalized Protein Abundance"
)
abline(h = median(norm_mat, na.rm = TRUE), col = "black", lty = 2)
dev.off()
```

---

## Step 4: Exploring the Missing Value Dilemma

### Why Are There Missing Values in Mass Spectrometry?

In bottom-up mass spectrometry, missing values are inevitable. In our current dataset:

```r
# Overall missing value percentage
total_values <- prod(dim(norm_mat))
missing_values <- sum(is.na(norm_mat))
pct_missing <- round(100 * missing_values / total_values, 1)

cat("Total values:", total_values, "\n")
cat("Missing values:", missing_values, "(", pct_missing, "% )\n")
```
*Output: $\approx 18.4\%$ missing values across the matrix.*

### MAR vs. MNAR: Two Different Biological Realities

In computational proteomics, missing values are classified into two fundamental mechanisms:

```
                          MISSING VALUES IN PROTEOMICS
                                       │
        ┌──────────────────────────────┴──────────────────────────────┐
        ▼                                                             ▼
   [ MAR / MCAR ]                                                  [ MNAR ]
(Missing At Random)                                    (Missing Not At Random)
• Stochastic ion suppression                           • True biological low abundance
• Peptide co-elution overlap                           • Signal below Limit of Detection (LOD)
• Random instrument glitch                             • Protein repressed/silenced in treated condition
• Spread uniformly across abundance                    • Heavily concentrated in lowest abundance tier
• Best handled by: kNN, missForest                     • Best handled by: Left-censored Gaussian downshift
```

1. **MAR (Missing At Random)**:
   - Caused by stochastic physical events: a peptide peak overlapped with a huge contaminant peak, or the spray needle flickered briefly.
   - The probability of being missing is largely independent of the protein's true abundance.
2. **MNAR (Missing Not At Random)**:
   - Caused by the instrument's **Limit of Detection (LOD)**.
   - The protein is expressed at very low levels (or completely turned off in one treatment group). Its tryptic peptides fail to produce enough ions to cross the detection threshold.

### Visualizing Missingness vs. Protein Abundance

To demonstrate that missingness is predominantly **MNAR**, we can plot the missingness rate as a function of mean protein abundance:

```r
# Calculate mean abundance and number of missing values per protein
protein_stats <- data.frame(
  MeanAbundance = rowMeans(norm_mat, na.rm = TRUE),
  MissingCount = rowSums(is.na(norm_mat))
)

# Plot missingness vs. abundance
p_missing <- ggplot(protein_stats, aes(x = factor(MissingCount), y = MeanAbundance, fill = factor(MissingCount))) +
  geom_boxplot(outlier.size = 0.5, alpha = 0.8) +
  theme_bw() +
  scale_fill_brewer(palette = "Blues") +
  labs(
    title = "Proteomics Missingness is Driven by Low Abundance (MNAR)",
    x = "Number of Missing Values (out of 6 samples)",
    y = "Mean Log2 Intensity"
  ) +
  theme(legend.position = "none")

ggsave("results/figures/qc_missing_vs_abundance.png", p_missing, width = 6, height = 4)
```

> [!NOTE]
> As the number of missing samples increases, mean protein intensity drops significantly. This confirms that the vast majority of dropouts in mass spectrometry are **MNAR (left-censored)**.

---

## Step 5: Filtering by Valid Values

Before imputing, **we must remove proteins that are missing too frequently to be statistically informative**.

### The "At Least X Valid Values in One Group" Rule

If a protein is quantified in only 1 out of 6 runs, attempting to impute the remaining 5 values creates synthetic noise. A standard proteomics best practice is:

> **Keep proteins that have at least 2 valid (non-NA) values in at least ONE biological condition.**

This rule ensures that:
- We have enough replicates to calculate condition variance ($n \ge 2$).
- We **preserve true biological on/off switches** (e.g., proteins present in all 3 Treated samples but completely absent in Control).

```r
# Custom filtering function based on condition groups
filter_valid_values <- function(mat, condition, min_valid = 2) {
  groups <- unique(condition)
  
  # For each group, check if valid count >= min_valid
  valid_per_group <- sapply(groups, function(grp) {
    grp_samples <- which(condition == grp)
    rowSums(!is.na(mat[, grp_samples, drop = FALSE])) >= min_valid
  })
  
  # Keep protein if TRUE in at least one group
  keep <- apply(valid_per_group, 1, any)
  return(keep)
}

# Apply filter
keep_proteins <- filter_valid_values(norm_mat, colData$Condition, min_valid = 2)

cat("Proteins before filtering:", nrow(norm_mat), "\n")
cat("Proteins retained:", sum(keep_proteins), "\n")
cat("Proteins discarded:", sum(!keep_proteins), "\n")

# Subset QFeatures object
qf <- qf[keep_proteins, , ]
```

```text
Proteins before filtering: 7754 
Proteins retained: 6912 
Proteins discarded: 842 
```

We safely filtered out 842 noisy, sporadic proteins while keeping 6,912 high-confidence proteins.

---

## Step 6: Imputation Strategies: Theory & Practice

Because missing values in proteomics are predominantly MNAR (below the Limit of Detection), imputing them with an overall mean or median is mathematically incorrect (it would artificially elevate low-abundance proteins).

### Left-Censored Imputation (Perseus Gaussian Downshift)

The standard algorithm used in proteomics (popularized by Perseus and MaxQuant) is **left-censored Gaussian downshift**:
1. Calculate the mean ($\mu$) and standard deviation ($\sigma$) of all observed valid intensities in a sample.
2. Construct a simulated normal distribution shifted to the left by $1.8\sigma$ with a narrower width of $0.3\sigma$.
3. Draw random values from this downshifted distribution to replace missing values.

```
   Downshifted Simulated Distribution           Observed Intensity Distribution
             (Imputed MNAR)                               (Observed)
                 ┌───┐                                      ┌───┐
                ┌┘   └┐                                    ┌┘   └┐
               ┌┘     └┐                                  ┌┘     └┐
           ────┴───────┴──────────────────────────────────┴───────┴────► Log2 Intensity
               ◄───────►                                  ◄───────►
               Shift: -1.8σ                                Mean: μ, SD: σ
```

In `QFeatures`, we can perform left-censored Gaussian imputation directly:

```r
# Apply MinProb / Perseus-style downshift imputation
qf <- impute(
  qf,
  i = "normalized",
  name = "imputed_perseus",
  method = "MinProb",
  q = 0.01 # 1st percentile left-tail boundary
)
```

### MAR Imputation (k-Nearest Neighbors / kNN)

If your experimental design or biological question suggests missingness is largely stochastic (e.g., in clean fractionated proteomics), **k-Nearest Neighbors (kNN)** imputation finds the $k$ most similar proteins based on observed values and computes their weighted average:

```r
# Alternatively, impute via kNN
qf <- impute(
  qf,
  i = "normalized",
  name = "imputed_knn",
  method = "knn",
  k = 10
)
```

---

### Visualizing the Impact of Imputation

Let's compare the density distribution of sample `A549_Ctrl_Rep1` before vs. after Perseus-style imputation:

```r
# Extract values
df_before <- data.frame(
  Intensity = assay(qf[["normalized"]])[, "A549_Ctrl_Rep1"],
  Status = "Before Imputation (Observed)"
)

df_after <- data.frame(
  Intensity = assay(qf[["imputed_perseus"]])[, "A549_Ctrl_Rep1"],
  Status = "After MinProb Imputation"
)

df_plot <- bind_rows(df_before, df_after) %>% filter(!is.na(Intensity))

p_impute <- ggplot(df_plot, aes(x = Intensity, fill = Status)) +
  geom_density(alpha = 0.4) +
  theme_bw() +
  scale_fill_manual(values = c("Before Imputation (Observed)" = "#4E79A7", 
                               "After MinProb Imputation" = "#E15759")) +
  labs(
    title = "Effect of Left-Censored Imputation on Intensity Distribution",
    subtitle = "Imputed values populate the low-abundance left tail (LOD)",
    x = "Log2 Intensity",
    y = "Density"
  ) +
  theme(legend.position = "bottom")

ggsave("results/figures/qc_imputation_comparison.png", p_impute, width = 7, height = 4.5)
```

The imputed values neatly populate the lower tail of the detection spectrum without altering the shape of the observed abundance distribution.

---

## Step 7: Exporting Clean Data for Differential Testing

To prepare for differential abundance testing in **Part 2c**, save both the `QFeatures` R object and a tidy matrix table to disk:

```r
# 1. Save complete QFeatures container
saveRDS(qf, "data/processed/A549_qfeatures_processed.rds")

# 2. Extract final imputed matrix with annotations
clean_mat <- as.data.frame(assay(qf[["imputed_perseus"]]))
clean_mat$Protein.Group <- rownames(clean_mat)

# Attach gene symbols
row_annotations <- as.data.frame(rowData(qf[["imputed_perseus"]]))
clean_export <- left_join(clean_mat, row_annotations, by = "Protein.Group")

# Write clean CSV
write.csv(clean_export, "data/processed/clean_protein_matrix.csv", row.names = FALSE)

cat("Processed data successfully saved to data/processed/\n")
```

---

## Conclusion & What’s Next

In this tutorial, you learned how to:
1. Load raw DIA-NN protein matrices into the Bioconductor **`QFeatures`** container.
2. Filter out laboratory contaminants from the **cRAP** database.
3. Transform raw intensities into $\log_2$ space and perform **median-centering normalization**.
4. Understand the biological difference between **MAR** (random) and **MNAR** (limit of detection) missingness.
5. Apply the **"valid values in at least one group"** rule to protect biological switches.
6. Impute left-censored missing values using a grounded statistical distribution.

Now that our data is clean, normalized, and complete, we are ready to identify which proteins are significantly modulated by the kinase inhibitor.

In **[Part 2c: Differential Abundance Analysis: MSstats vs. limma](04_part-2c-ms-differential-abundance-msstats-vs-limma.md)**, we compare empirical Bayes moderated t-tests (`limma`) with linear mixed-effects modeling (`MSstats`) to extract our high-confidence Differentially Abundant Proteins (DAPs).

