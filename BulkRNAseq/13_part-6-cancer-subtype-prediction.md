# How to Analyze RNAseq Data for Absolute Beginners Part 6: A Comprehensive Guide for Cancer Subtype Prediction

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/)

---

**Meta Description:** Learn how to predict cancer subtypes using RNA-seq data through practical implementations of PAM50, genefu, and GSVA methods. Perfect for bioinformaticians and computational biologists working with gene expression data.

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/)

- [Introduction](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Introduction)
- [Understanding Molecular Subtypes](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Understanding_Molecular_Subtypes)

  - [Breast Cancer Subtypes Overview](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Breast_Cancer_Subtypes_Overview)

- [Setup and Prerequisites](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Setup_and_Prerequisites)

  - [Required R Packages](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Required_R_Packages)
  - [Loading Libraries](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Loading_Libraries)

- [Data Preparation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Data_Preparation)

  - [Processing Gene Expression Data](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Processing_Gene_Expression_Data)
  - [Preparing Gene Annotations](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Preparing_Gene_Annotations)

- [Subtype Prediction Methods](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Subtype_Prediction_Methods)

  - [Method 1: PAM50 Classification](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Method_1_PAM50_Classification)
  - [Method 2: genefu Package](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Method_2_genefu_Package)
  - [Method 3: GSVA Approach](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Method_3_GSVA_Approach)

- [Results Interpretation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Results_Interpretation)
- [Best Practices](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Best_Practices)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/#Like_this)

## Introduction

Cancer subtype prediction from RNA-seq data is crucial for personalized medicine and treatment optimization. This tutorial, part 6 in our RNA-seq analysis series, demonstrates how to implement various subtype prediction methods using R. We’ll use breast cancer as our model system, but the principles can be applied to other cancer types.

> 

**Note:** This tutorial builds on concepts covered in [Part 3: Differential Expression Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/). If you’re new to RNA-seq analysis, we recommend starting with [Part 1: Introduction to RNA-seq](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/).

## Understanding Molecular Subtypes

### Breast Cancer Subtypes Overview

Breast cancer serves as an excellent model for molecular subtyping, featuring four well-characterized intrinsic subtypes. Each subtype has distinct characteristics affecting treatment decisions and prognosis:

| Subtype | Key Characteristics | Treatment Response | Prognosis |
| --- | --- | --- | --- |
| Luminal A | ER+/PR+, HER2-, Low Ki-67 | Excellent hormone therapy response | Best |
| Luminal B | ER+/PR+, HER2±, High Ki-67 | Combined therapy needed | Intermediate |
| HER2-enriched | ER-/PR-, HER2+ | Good trastuzumab response | Intermediate |
| Basal-like/Triple-negative | ER-/PR-, HER2- | Limited targeted options | Poor |

## Setup and Prerequisites

### Required R Packages

First, let’s install all necessary packages. We’ll use BiocManager for Bioconductor packages and standard installation for CRAN packages.

```r
# Install BiocManager if not already installed
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# Install Bioconductor packages
BiocManager::install(c(
    "GEOquery",
    "edgeR",
    "limma",
    "EnsDb.Hsapiens.v86",
    "genefu",
    "GSVA"
))

# Install CRAN packages
install.packages(c(
    "data.table",
    "xtable",
    "rmeta",
    "caret",
    "pheatmap",
    "devtools"
))

# Install GitHub packages
devtools::install_github("ccchang0111/PAM50")
```

### Loading Libraries

```r
# Load required libraries
library(GEOquery)
library(data.table)
library(edgeR)
library(limma)
library(xtable)
library(rmeta)
library(caret)
library(genefu)
library(AnnotationDbi)
library(EnsDb.Hsapiens.v86)
library(PAM50)
library(GSVA)
library(pheatmap)
```

## Data Preparation

### Processing Gene Expression Data

We’ll use the [GSE209998](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE209998) dataset as our example. This section shows how to process raw count data into a format suitable for subtype prediction.

```r
# Fetch data from GEO
gse_meta <- getGEO("GSE209998")
gse_meta <- pData(gse_meta[[1]])
rownames(gse_meta) <- gse_meta$description.1

# Read count data
gse_count <- fread("GSE209998_AUR_129_raw_counts.txt", data.table = F)

# Verify sample matching
if(!identical(rownames(gse_meta), colnames(gse_count)[-1])) {
    stop("Sample names don't match between metadata and count data")
}

# Process count table
gse_count <- gse_count[!duplicated(gse_count[[1]]), ]
rownames(gse_count) <- gse_count[[1]]
gse_count <- gse_count[, -1]

# Filter low-expressed genes
perc_keep <- 0.8  # Keep genes expressed in 80% of samples
gene_keep <- rowSums(gse_count > 0) >= ceiling(perc_keep * ncol(gse_count))
count_tbl_low_rm <- gse_count[gene_keep, ]

# Create DGE list and normalize
dge_gse <- DGEList(counts=count_tbl_low_rm, samples = gse_meta)
dge_gse <- dge_gse[, dge_gse$samples$tissue.ch1 == "Breast"]
dge_gse <- calcNormFactors(dge_gse, method = "TMM")
dge_gse_v <- voom(dge_gse, plot=TRUE)
```

### Preparing Gene Annotations

The prediction methods require ENTREZ IDs. Here’s how to map your genes to the correct identifiers:

```r
# Get ENTREZ IDs
entrezid <- AnnotationDbi::select(
    EnsDb.Hsapiens.v86,
    keys = rownames(dge_gse_v),
    columns = c("ENTREZID", "SYMBOL"),
    keytype = "SYMBOL"
)

# Clean up annotations
entrezid <- na.omit(entrezid)
entrezid <- entrezid[!duplicated(entrezid[[1]]) & !duplicated(entrezid[[2]]), ]
rownames(entrezid) <- entrezid[[2]]

# Match genes
com_gene <- intersect(entrezid[[2]], rownames(dge_gse_v))
entrezid <- entrezid[com_gene, c(2, 2, 1)]
colnames(entrezid) <- c("probe", "Gene.Symbol", "EntrezGene.ID")
rownames(entrezid) <- entrezid[[1]]

# Update expression data
dge_gse_v_entrez <- dge_gse_v[rownames(entrezid), ]
dge_gse_v_entrez$genes <- entrezid

# Create subset for demonstration
set.seed(123)
dge_gse_v_entrez_sub <- dge_gse_v_entrez[, sample(colnames(dge_gse_v_entrez), 30)]
```

The resulting gene annotation table “entrezid” has the following format.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-03-at-5.02.27%E2%80%AFPM.png?resize=822%2C604&ssl=1)

## Subtype Prediction Methods

### Method 1: PAM50 Classification

The PAM50 classifier from the PAM50 package uses a 50-gene signature to predict breast cancer subtypes.

```r
# Format expression matrix
expr_dge_gse_v_entrez_sub <- as.data.frame(dge_gse_v_entrez_sub$E)
rownames(expr_dge_gse_v_entrez_sub) <- entrezid$EntrezGene.ID

# Predict subtypes
df.pred <- PAM50(expr_dge_gse_v_entrez_sub, cutoff = 0)

# Save results
fwrite(as.data.table(df.pred), "subtype_pred_pam50.csv")

# Visualize results
pdf("heatmap_subtype_pred_pam50_expression.pdf", width = 7, height = 9)
plotPAM50(as.data.frame(dge_gse_v_entrez_sub$E), df.pred$PAM50)
dev.off()
```

The resulting df.pred object contains a probability matrix where each row represents a sample and each column shows the likelihood of that sample belonging to a specific subtype.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-03-at-4.10.54%E2%80%AFPM.png?resize=1024%2C550&ssl=1)

Visualize the expression levels of PAM50’s diagnostic gene set using a heatmap, which highlights the distinct expression patterns that characterize each molecular subtype.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-03-at-3.45.07%E2%80%AFPM.png?resize=799%2C1024&ssl=1)

### Method 2: genefu Package

The genefu package supports several breast cancer classification models:

- scmgene

- scmod1/scmod2

- pam50

- ssp2006/ssp2003

- intClust

- AIMS

- claudinLow

To use a different model, simply change the `sbt.model` parameter in the `molecular.subtyping()` function.

> 

**Note**: Each model requires its own reference dataset. If you encounter a ‘missing data’ error, load the corresponding reference data similar to how we loaded `pam50.robust`.

```r
# Transpose expression matrix
expr_dge_gse_v_entrez_sub_t <- t(dge_gse_v_entrez_sub$E)

# Load PAM50 data
data(pam50.robust)

# Predict subtypes
subtype_pred_pam50 <- molecular.subtyping(
    sbt.model = "pam50",
    data = expr_dge_gse_v_entrez_sub_t,
    annot = entrezid,
    do.mapping = TRUE
)

# Process results
subtype_pred_pam50_df <- as.data.frame(subtype_pred_pam50$subtype.proba)
subtype_pred_pam50_df$Subtype <- as.character(subtype_pred_pam50$subtype)

# Save results
fwrite(subtype_pred_pam50_df, "subtype_pred_pam50_df.csv")

# Visualize results
pdf("heatmap_subtype_pred_pam50.pdf", width = 7, height = 9)
pheatmap(
    subtype_pred_pam50$subtype.proba,
    scale = "none",
    cluster_rows = TRUE,
    cluster_cols = FALSE,
    show_rownames = TRUE
)
dev.off()
```

The resulting subtype_pred_pam50_df object contains a probability matrix where each row represents a sample and each column shows the likelihood of that sample belonging to a specific subtype.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-03-at-5.03.53%E2%80%AFPM.png?resize=1024%2C509&ssl=1)

Visualize the classification probabilities in a heatmap format, where warm colors represent higher confidence in subtype assignments for each sample.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-03-at-3.49.54%E2%80%AFPM.png?resize=801%2C1024&ssl=1)

### Method 3: GSVA Approach

If you are working on subtyping other cancers and have access to the relevant subtype gene signatures, the GSVA package is an excellent choice. GSVA offers flexibility for custom gene signatures:

```r
# Define custom gene signatures
gene_sig_ls <- list(
    gene_basal1 = c("MRPS14", "DIP2B", /* ... other genes ... */),
    gene_basal2 = c("AKR1C1", "AMIGO2", /* ... other genes ... */),
    gene_luminal1 = c("ABCA3", "ABCG1", /* ... other genes ... */),
    gene_luminal2 = c("ADAMTS5", "ALCAM", /* ... other genes ... */)
)

# Run ssGSEA for single sample profiling
ssgseapar <- ssgseaParam(dge_gse_v_entrez_sub$E, gene_sig_ls)
gsva_pred <- gsva(ssgseapar)

# Use GSVA method for comparing samples in a larger cohort
# gsvapar <- gsvaParam(dge_gse_v_entrez_sub$E, gene_sig_ls)
# gsva_pred <- gsva(gsvapar)

# Process and save results
gsva_pred_t <- as.data.frame(t(gsva_pred))
fwrite(gsva_pred_t, "gsva_pred_t.csv", row.names = T)

# Visualize results
pdf("heatmap_subtype_pred_ssgsea.pdf", width = 7, height = 9)
pheatmap(
    gsva_pred_t,
    scale = "row",
    cluster_rows = TRUE,
    cluster_cols = FALSE,
    show_rownames = TRUE
)
dev.off()
```

The gsva_pred_t object stores normalized enrichment scores (NES) for each sample-subtype combination, where higher scores indicate stronger association with that particular molecular subtype.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-03-at-4.12.24%E2%80%AFPM.png?resize=1024%2C464&ssl=1)

Visualize the subtype enrichment patterns across all samples using a heatmap, where color intensity represents the strength of each sample’s association with different molecular subtypes.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-03-at-3.54.05%E2%80%AFPM.png?resize=798%2C1024&ssl=1)

## Results Interpretation

When interpreting subtype predictions, consider:

1. **Confidence Scores**: Review prediction probabilities for each subtype

2. **Gene Signature Quality**: Assess the relevance of gene signatures

3. **Sample Quality**: Evaluate RNA-seq quality metrics

4. **Clinical Correlation**: Compare with known clinical parameters

## Best Practices

1. **Gene Signature Selection**

- Use validated gene signatures

- Consider tissue-specific signatures

- Update signatures based on current literature

1. **Validation**

- Cross-validate predictions

- Compare results across methods

- Correlate with clinical data

## References

1. Parker JS, et al. (2009). Supervised risk predictor of breast cancer based on intrinsic subtypes. J Clin Oncol.

2. Hänzelmann S, et al. (2013). GSVA: gene set variation analysis for microarray and RNA-seq data. BMC Bioinformatics.

3. Gendoo DM, et al. (2016). Genefu: an R/Bioconductor package for computation of gene expression-based signatures in breast cancer. Bioinformatics.
