# How to Analyze Proteomics Data Part 2c: Differential Abundance Analysis: MSstats vs. limma

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Mass Spectrometry, Differential Abundance, limma, MSstats, Statistical Modeling

---

## Table of Contents

- [Introduction](#introduction)
- [The Two Statistical Philosophies in Proteomics](#the-two-statistical-philosophies-in-proteomics)
  - [Philosophy 1: Protein-Level Modeling (limma)](#philosophy-1-protein-level-modeling-limma)
  - [Philosophy 2: Peptide-Level Mixed Modeling (MSstats)](#philosophy-2-peptide-level-mixed-modeling-msstats)
  - [Quick Summary Comparison](#quick-summary-comparison)
- [Workflow 1: Differential Abundance Testing with limma](#workflow-1-differential-abundance-testing-with-limma)
  - [Step 1: Load Normalized & Imputed Data](#step-1-load-normalized--imputed-data)
  - [Step 2: Build the Experimental Design Matrix](#step-2-build-the-experimental-design-matrix)
  - [Step 3: Define Contrasts](#step-3-define-contrasts)
  - [Step 4: Fit Linear Models & Empirical Bayes Moderation](#step-4-fit-linear-models--empirical-bayes-moderation)
  - [Step 5: Extract Differentially Abundant Proteins (DAPs)](#step-5-extract-differentially-abundant-proteins-daps)
- [Workflow 2: Peptide-Level Modeling with MSstats](#workflow-2-peptide-level-modeling-with-msstats)
  - [Step 1: Convert DIA-NN Output to MSstats Format](#step-1-convert-dia-nn-output-to-msstats-format)
  - [Step 2: Run MSstats Data Processing](#step-2-run-msstats-data-processing)
  - [Step 3: Test Group Comparisons](#step-3-test-group-comparisons)
- [Benchmarking limma vs. MSstats](#benchmarking-limma-vs-msstats)
  - [Comparing Log2 Fold-Changes and P-values](#comparing-log2-fold-changes-and-p-values)
  - [Which Tool Should You Choose?](#which-tool-should-you-choose)
- [Post-Testing Quality Checks](#post-testing-quality-checks)
  - [1. P-value Distribution Histogram](#1-p-value-distribution-histogram)
  - [2. Peptide Count Support (Filtering Single-Hit Wonders)](#2-peptide-count-support-filtering-single-hit-wonders)
- [Saving the Final DAP Results](#saving-the-final-dap-results)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In [Part 2b](03_part-2b-ms-data-qc-filtering-and-handling-missing-values.md), we cleaned our raw mass spectrometry data, removed cRAP contaminants, $\log_2$-transformed and median-normalized intensities, filtered out unreliable proteins, and imputed left-censored missing values.

Now we arrive at the central scientific question of our study:

> **Which proteins are significantly up- or down-regulated between vehicle-treated (Control) and kinase inhibitor-treated (Treated) A549 lung cancer cells?**

In transcriptomics, we call these **Differentially Expressed Genes (DEGs)**. In proteomics, we call them **Differentially Abundant Proteins (DAPs)**.

While standard Student's $t$-tests or ANOVA might seem like obvious choices, they perform poorly in omics datasets with small sample sizes ($n = 3$ per group), where individual protein variance estimates are noisy and prone to extreme false positives.

In this tutorial, we evaluate the two premier statistical frameworks in quantitative proteomics:
1. **`limma`**: Moderated empirical Bayes t-testing on protein-rollup values.
2. **`MSstats`**: Linear mixed-effects modeling operating directly on individual peptides.

---

## The Two Statistical Philosophies in Proteomics

```
                 HOW TO STATISTICALLY MODEL PROTEIN ABUNDANCE?
                                       │
        ┌──────────────────────────────┴──────────────────────────────┐
        ▼                                                             ▼
[ PHILOSOPHY 1: PROTEIN ROLLUP ]                             [ PHILOSOPHY 2: PEPTIDE LEVEL ]
           (limma)                                                      (MSstats)
• Peptides aggregated to 1 number per protein                • Keeps every peptide precursor separate
• Modeled using moderated t-test (eBayes)                   • Modeled using Linear Mixed-Effects Model
• Borrows variance across all 7,000 proteins                 • Incorporates peptide-specific random effects
• Extremely fast, flexible, identical to RNA-seq              • Gold standard for complex multi-run designs
```

### Philosophy 1: Protein-Level Modeling (limma)
Originally built for microarrays and adapted for RNA-seq (`limma-voom`), **`limma`** fits a linear model to each protein's MaxLFQ abundance. 

Its key strength is **empirical Bayes variance shrinkage**: instead of estimating sample variance for a protein using only its 3 replicates, `limma` shrinks each protein's variance toward a global pooled variance across all 7,000 proteins. This prevents proteins with coincidentally tiny sample variances from producing artificially inflated $t$-statistics.

### Philosophy 2: Peptide-Level Mixed Modeling (MSstats)
Developed by the Vitek Lab at Northeastern University, **`MSstats`** avoids rolling up peptides into a single protein number beforehand. 

A protein might be identified by 8 tryptic peptides, each with different ionization efficiencies, charge states, and chromatographic peak shapes. `MSstats` models each peptide as an individual observation using a **Linear Mixed-Effects Model**:
$$\log_2(\text{Intensity}_{ijk}) = \mu + \text{Condition}_i + \text{Peptide}_j + \text{BiologicalReplicate}_k + \epsilon_{ijk}$$
Where:
- $\text{Condition}_i$ is the fixed biological effect of interest.
- $\text{Peptide}_j$ and $\text{BiologicalReplicate}_k$ are random effects capturing technical and sample variability.

### Quick Summary Comparison

| Metric | `limma` | `MSstats` |
| :--- | :--- | :--- |
| **Input Data** | Protein-level matrix (`report.pg_matrix.tsv`) | Long-format precursor table (`report.tsv`) |
| **Speed** | Blazing fast (<2 seconds for 7,000 proteins) | Computationally intensive (3–10 minutes) |
| **Statistical Model** | Moderated linear model with empirical Bayes | Linear Mixed-Effects Model |
| **Peptide Information** | Discarded after MaxLFQ aggregation | Preserved and used to weight confidence |
| **Complex Designs** | Handles factorial, batch, covariates easily | Excels at longitudinal, multi-run, nested designs |

---

## Workflow 1: Differential Abundance Testing with limma

Let's begin with the fast and widely adopted `limma` workflow.

### Step 1: Load Normalized & Imputed Data

Open your R session, set your working directory, and load the cleaned dataset prepared in Part 2b:

```r
library(limma)
library(QFeatures)
library(tidyverse)

setwd("~/Projects/learning_omics/Proteomics_Workspace")

# Load QFeatures object from Part 2b
qf <- readRDS("data/processed/A549_qfeatures_processed.rds")

# Extract the imputed expression matrix
expr_mat <- assay(qf[["imputed_perseus"]])
col_meta  <- as.data.frame(colData(qf))
row_meta  <- as.data.frame(rowData(qf[["imputed_perseus"]]))

# Verify sample alignment
stopifnot(colnames(expr_mat) == col_meta$SampleID)
cat("Proteins to test:", nrow(expr_mat), "| Samples:", ncol(expr_mat), "\n")
```

```text
Proteins to test: 6912 | Samples: 6 
```

---

### Step 2: Build the Experimental Design Matrix

We formulate our design matrix without an intercept (`~ 0 + Condition`) to make defining contrasts straightforward:

```r
# Define group factor
group <- factor(col_meta$Condition, levels = c("Control", "Treated"))

# Create design matrix
design <- model.matrix(~ 0 + group)
colnames(design) <- c("Control", "Treated")
rownames(design) <- col_meta$SampleID

print(design)
```

```text
               Control Treated
A549_Ctrl_Rep1       1       0
A549_Ctrl_Rep2       1       0
A549_Ctrl_Rep3       1       0
A549_Trt_Rep1        0       1
A549_Trt_Rep2        0       1
A549_Trt_Rep3        0       1
```

---

### Step 3: Define Contrasts

We want to measure the fold-change of **Treated versus Control**:
$$\text{Contrast} = \text{Treated} - \text{Control}$$

```r
# Set up contrast matrix
contrast_matrix <- makeContrasts(
  Treated_vs_Control = Treated - Control,
  levels = design
)

print(contrast_matrix)
```

---

### Step 4: Fit Linear Models & Empirical Bayes Moderation

Now we fit the linear model across all 6,912 proteins simultaneously:

```r
# 1. Fit linear model for each protein
fit <- lmFit(expr_mat, design)

# 2. Compute contrasts
fit_contrast <- contrasts.fit(fit, contrast_matrix)

# 3. Apply Empirical Bayes variance shrinkage
fit_bayes <- eBayes(fit_contrast, trend = TRUE, robust = TRUE)
```
* `trend = TRUE`: Allows the prior variance to depend on the overall abundance of the protein (mean-variance trend), a standard recommendation for proteomic intensities.
* `robust = TRUE`: Protects against hyper-variable outlier proteins distorting the prior distribution.

---

### Step 5: Extract Differentially Abundant Proteins (DAPs)

Extract the complete results table using `topTable()`:

```r
# Extract all tested proteins
dap_limma <- topTable(
  fit_bayes,
  coef = "Treated_vs_Control",
  number = Inf,
  adjust.method = "BH", # Benjamini-Hochberg FDR
  sort.by = "P"
)

# Merge with protein and gene annotations
dap_limma$Protein.Group <- rownames(dap_limma)
dap_limma <- left_join(dap_limma, row_meta[, c("Protein.Group", "Protein.Names", "Genes")], by = "Protein.Group")

# Define significance thresholds
# 1. Adjusted P-value (FDR) < 0.05
# 2. Absolute log2 Fold-Change >= 1.0 (2-fold change)
dap_limma <- dap_limma %>%
  mutate(
    Significance = case_when(
      adj.P.Val < 0.05 & logFC >= 1.0  ~ "Up-regulated",
      adj.P.Val < 0.05 & logFC <= -1.0 ~ "Down-regulated",
      TRUE                             ~ "Not Significant"
    )
  )

# Summary of results
table(dap_limma$Significance)
```

```text
Down-regulated  Not Significant     Up-regulated 
           312             6248              352 
```

Out of 6,912 quantified proteins, **352 are significantly up-regulated** and **312 are significantly down-regulated** upon targeted kinase inhibition!

Inspect the top 5 most significant hits:

```r
head(dap_limma %>% select(Genes, Protein.Group, logFC, AveExpr, P.Value, adj.P.Val, Significance), 5)
```

```text
  Genes Protein.Group     logFC  AveExpr      P.Value    adj.P.Val   Significance
1 DUSP4        Q13115  3.421045 22.84102 1.482011e-08 1.024366e-04   Up-regulated
2  EGR1        P18146 -4.102391 21.10429 3.204192e-08 1.107328e-04 Down-regulated
3 FOSL1        P15407 -3.892014 20.45291 5.920410e-08 1.364022e-04 Down-regulated
4 SPRY2        O43597  2.894102 23.10492 8.104291e-08 1.400492e-04   Up-regulated
5 CCND1        P24385 -2.749102 24.89104 1.140291e-07 1.576210e-04 Down-regulated
```

Notice the immediate biological story: **EGR1, FOSL1, and CCND1 (Cyclin D1)** are strongly down-regulated, indicating cell cycle arrest and shutdown of MAPK/ERK transcriptional activity, while feedback negative regulators like **DUSP4 and SPRY2** are induced.

---

## Workflow 2: Peptide-Level Modeling with MSstats

Now let's examine the peptide-level approach using **`MSstats`**.

### Step 1: Convert DIA-NN Output to MSstats Format

`MSstats` requires the detailed precursor table (`report.tsv`) from DIA-NN rather than the protein matrix:

```r
library(MSstats)

# Convert DIA-NN report into MSstats-compatible data frame
# This handles column renaming, charge states, and filtering 1% FDR
msstats_input <- DIANNtoMSstatsFormat(
  input = "data/processed/report.tsv",
  annotation = data.frame(
    Run = c("A549_Ctrl_Rep1", "A549_Ctrl_Rep2", "A549_Ctrl_Rep3",
            "A549_Trt_Rep1", "A549_Trt_Rep2", "A549_Trt_Rep3"),
    Condition = c("Control", "Control", "Control", "Treated", "Treated", "Treated"),
    BioReplicate = c("1", "2", "3", "4", "5", "6")
  ),
  global_qvalue_cutoff = 0.01,
  qvalue_cutoff = 0.01,
  use_unique_peptide = TRUE,
  MBR = TRUE
)

head(msstats_input, 3)
```

---

### Step 2: Run MSstats Data Processing

The `dataProcess()` function performs log-transformation, peptide-level normalization, feature selection (removing shared or low-quality peptides), and summarizes protein abundance using Tukey's median polish:

```r
# Process data through MSstats pipeline
processed_data <- dataProcess(
  msstats_input,
  normalization = "equalizeMedians", # Equivalent to median centering
  summaryMethod = "TMP",              # Tukey's Median Polish
  censoredInt = "NA",
  MBR = TRUE,
  maxQuantileforCensored = 0.999
)
```

---

### Step 3: Test Group Comparisons

Now define the contrast comparison in `MSstats`:

```r
# Create contrast matrix for MSstats
comparison <- matrix(c(-1, 1), nrow = 1)
colnames(comparison) <- c("Control", "Treated")
rownames(comparison) <- "Treated_vs_Control"

# Perform group comparison with linear mixed-effects model
dap_msstats <- groupComparison(
  contrast.matrix = comparison,
  data = processed_data
)

# Extract comparison results table
msstats_res <- dap_msstats$ComparisonResult
head(msstats_res, 3)
```

---

## Benchmarking limma vs. MSstats

### Comparing Log2 Fold-Changes and P-values

Let's evaluate the concordance between `limma` and `MSstats`:

```r
# Merge results on Protein ID
comparison_df <- inner_join(
  dap_limma %>% select(Protein.Group, logFC_limma = logFC, pval_limma = adj.P.Val),
  msstats_res %>% select(Protein.Group = Protein, logFC_msstats = log2FC, pval_msstats = adj.pvalue),
  by = "Protein.Group"
)

# Calculate Pearson correlation of fold changes
r_fc <- cor(comparison_df$logFC_limma, comparison_df$logFC_msstats, use = "complete.obs")
cat("Correlation of Log2 Fold Changes between limma and MSstats: r =", round(r_fc, 3), "\n")
```

```text
Correlation of Log2 Fold Changes between limma and MSstats: r = 0.962 
```

The fold-change correlation is **$r = 0.96$**, demonstrating remarkable agreement between the two methodologies!

```
     log2FC (MSstats)
            ▲                /
            │               /  High Concordance (r = 0.96)
            │              /
            │             /
         0  ┼────────────/────────► log2FC (limma)
            │           /
            │          /
            │         /
```

### Which Tool Should You Choose?

- **Use `limma`** if:
  - You have a standard 2-group or multi-group design with standard replication.
  - You want fast iteration, custom blocking factors, or want to follow the standard `QFeatures` / `DEP` Bioconductor ecosystem.
  - You are integrating proteomics directly with RNA-seq data (where `limma` syntax is identical).
- **Use `MSstats`** if:
  - You have a complex hierarchical experiment (e.g., patient samples measured across multiple technical fractions and LC batches).
  - You want statistical weighting based on individual peptide measurement quality.
  - You are analyzing targeted SRM/PRM/DIA data with extensive run-to-run dropouts.

---

## Post-Testing Quality Checks

Before publishing a DAP list, run these two diagnostic checks to ensure statistical validity.

### 1. P-value Distribution Histogram

In a well-powered experiment, non-differentially expressed proteins should follow a **uniform flat distribution** between 0 and 1, while truly regulated proteins create a **sharp spike near 0**:

```r
p_hist <- ggplot(dap_limma, aes(x = P.Value)) +
  geom_histogram(bins = 40, fill = "#4E79A7", color = "white", boundary = 0) +
  theme_bw() +
  labs(
    title = "P-value Distribution for Treated vs. Control",
    subtitle = "Healthy signature: sharp peak near 0 with flat null baseline",
    x = "Raw P-value",
    y = "Protein Count"
  )

ggsave("results/figures/qc_pvalue_histogram.png", p_hist, width = 6, height = 4)
```

> [!WARNING]
> If your $p$-value histogram shows a hill/bump in the middle or spikes near 1.0, this indicates uncorrected batch effects, failed normalization, or aggressive over-imputation.

---

### 2. Peptide Count Support (Filtering Single-Hit Wonders)

A common pitfall in proteomics is declaring a protein "significantly altered" when it was identified by only **one single peptide**. 

Single-peptide identifications have higher rates of false-positive sequence assignments. To ensure high experimental confidence, best practice is to require at least **2 unique peptides**:

```r
# Check unique peptide count per protein from DIA-NN report
pep_counts <- read.delim("data/processed/report.tsv") %>%
  group_by(Protein.Group) %>%
  summarise(UniquePeptides = n_distinct(Stripped.Sequence))

# Merge with DAP results
dap_limma <- left_join(dap_limma, pep_counts, by = "Protein.Group")

# Flag high-confidence DAPs (FDR < 0.05, |logFC| >= 1.0, UniquePeptides >= 2)
dap_limma <- dap_limma %>%
  mutate(
    HighConfidenceDAP = (Significance != "Not Significant") & (UniquePeptides >= 2)
  )

table(dap_limma$HighConfidenceDAP)
```

```text
FALSE  TRUE 
 6314   598 
```

We obtain **598 high-confidence DAPs** supported by multiple independent peptide fragments!

---

## Saving the Final DAP Results

Save the complete statistical tables to disk:

```r
# Export complete results table
write.csv(
  dap_limma,
  "results/tables/DAP_results_limma.csv",
  row.names = FALSE
)

# Export subset of high-confidence hits only
write.csv(
  dap_limma %>% filter(HighConfidenceDAP),
  "results/tables/DAP_high_confidence_hits.csv",
  row.names = FALSE
)

cat("Differential abundance results saved to results/tables/\n")
```

---

## Conclusion & What’s Next

Congratulations! You have completed the entire discovery mass spectrometry track:
1. Processed raw spectra with **DIA-NN** (Part 2a).
2. Cleaned, normalized, and handled missing values in **`QFeatures`** (Part 2b).
3. Fitted linear empirical Bayes models with **`limma`** and linear mixed models with **`MSstats`** to identify 598 high-confidence DAPs (Part 2c).

With our mass spectrometry pipeline complete, we now turn our attention to the second major technological pillar of modern proteomics: **Targeted Affinity Assays**.

In **[Part 3a: Olink Proximity Extension Assay: QC, Normalization, and OlinkAnalyze](05_part-3a-olink-qc-npx-normalization-and-olinkanalyze.md)**, we explore how antibody-DNA conjugates measure thousands of proteins in plasma with near-zero missing values!
