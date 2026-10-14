# How to Analyze RNAseq Data for Absolute Beginners Part 20: Comparing limma, DESeq2, and edgeR in Differential Expression Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/)

- [Introduction](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Introduction)

  - [Why Multiple Tools Exist for DE Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Why_Multiple_Tools_Exist_for_DE_Analysis)

- [Statistical Foundations: Understanding the Tools](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Statistical_Foundations_Understanding_the_Tools)
- [Practical Implementation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Practical_Implementation)

  - [Setting Up Your R Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Setting_Up_Your_R_Environment)
  - [Data Preparation and Quality Control](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Data_Preparation_and_Quality_Control)
  - [DESeq2 Analysis Pipeline](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#DESeq2_Analysis_Pipeline)
  - [edgeR Analysis Pipeline](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#edgeR_Analysis_Pipeline)

- [Comparing Results Across Methods](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Comparing_Results_Across_Methods)
- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/#Like_this)

## Introduction

Differential expression (DE) analysis represents a fundamental step in understanding how genes respond to different biological conditions. When we perform RNA sequencing, we’re essentially taking a snapshot of all the genes that are active (or expressed) in our samples at a given moment. However, the real biological insights come from understanding how these expression patterns change between different conditions, time points, or disease states.

The power of DE analysis lies in its ability to identify these changes systematically across tens of thousands of genes simultaneously, while accounting for biological variability and technical noise inherent in RNA-seq experiments.

*For a detailed walkthrough of performing DE analysis using limma, please refer to our previous tutorial: [From Count Table to DEGs](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/).*

### Why Multiple Tools Exist for DE Analysis

The field has developed several sophisticated tools for DE analysis, each addressing specific challenges in RNA-seq data:

- Count data overdispersion

- Small sample sizes

- Complex experimental designs

- Varying levels of biological and technical noise

In this tutorial, we’ll explore the three most widely-used tools: limma, DESeq2, and edgeR. Understanding their unique approaches will help you choose the most appropriate tool for your specific research question.

## Statistical Foundations: Understanding the Tools

**Core Statistical Approach****Data Transformation****Variance Handling****Key Components**• Linear modeling• Empirical Bayes moderation• Precision weights• Dispersion estimation• GLM fitting• Hypothesis testing• Dispersion modeling• GLM/QLF testing• Exact testing option**Ideal Sample Size****Best Use Cases**• Multi-factor experiments• Time-series data• Integration with other omics• High biological variability• Subtle expression changes• Strong FDR control• Large datasets• Technical replicates• Flexible modeling needs**Computational Efficiency****Special Features**• Works well with other high-throughput data• Independent filtering• Visualization tools• Quasi-likelihood options• Fast exact tests**Limitations**• Requires careful QC of voom transformation• Conservative fold change estimates• Common dispersion may miss gene-specific patterns

| Aspect | limma | DESeq2 | edgeR |
| --- | --- | --- | --- |
|  | Linear modeling with empirical Bayes moderation | Negative binomial modeling with empirical Bayes shrinkage | Negative binomial modeling with flexible dispersion estimation |
|  | voom transformation converts counts to log-CPM values | Internal normalization based on geometric mean | TMM normalization by default |
|  | Empirical Bayes moderation improves variance estimates for small sample sizes | Adaptive shrinkage for dispersion estimates and fold changes | Flexible options for common, trended, or tagged dispersion |
|  | • voom transformation | • Normalization | • Normalization |
|  | ≥3 replicates per condition | ≥3 replicates, performs well with more | ≥2 replicates, efficient with small samples |
|  | • Small sample sizes | • Moderate to large sample sizes | • Very small sample sizes |
|  | Very efficient, scales well | Can be computationally intensive | Highly efficient, fast processing |
|  | • Handles complex designs elegantly | • Automatic outlier detection | • Multiple testing strategies |
|  | • May not handle extreme overdispersion well | • Computationally intensive for large datasets | • Requires careful parameter tuning |

## Practical Implementation

### Setting Up Your R Environment

First, let’s install and load the required packages:

```r
# Install required packages if not already present
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# Install Bioconductor packages
BiocManager::install(c("DESeq2", "edgeR", "limma"))

# Install CRAN packages
install.packages(c("data.table", "VennDiagram"))

# Load required libraries
library(data.table)
library(limma)
library(edgeR)
library(DESeq2)
library(VennDiagram)
```

### Data Preparation and Quality Control

```r
# Read the count matrix from our previous tutorial
count_tbl <- fread("counts_raw.tsv", data.table = F)

# Set row names and remove the first column
rownames(count_tbl) <- count_tbl[[1]]
count_tbl <- count_tbl[, -1]

# Filter low-expressed genes
# Keep genes expressed in at least 80% of samples
perc_keep <- 0.8
gene_keep <- rowSums(count_tbl > 0) >= ceiling(perc_keep * ncol(count_tbl))
count_tbl_low_rm <- count_tbl[gene_keep, ]

# Create metadata
meta <- data.frame(
    SampleID = colnames(count_tbl),
    Treatment = factor(
        c("KRAS_SPIB", "KRAS_SPIB", "KRAS", "KRAS"), 
        levels = c("KRAS", "KRAS_SPIB")
    )
)
rownames(meta) <- meta$SampleID
```

### DESeq2 Analysis Pipeline

```r
# Create DESeq2 object
obj_deseq2 <- DESeqDataSetFromMatrix(
    countData = count_tbl_low_rm,
    colData = meta,
    design = ~ Treatment
)

# Add feature annotations
featureData <- data.frame(GeneSymbol = rownames(count_tbl_low_rm))
mcols(obj_deseq2) <- DataFrame(mcols(obj_deseq2), featureData)

# Set reference level and perform DE analysis
obj_deseq2$Treatment <- relevel(obj_deseq2$Treatment, ref = "KRAS")
obj_deseq2 <- DESeq(obj_deseq2)

# Get results with desired thresholds
res_deseq2 <- results(
    obj_deseq2, 
    alpha = 0.05,    # FDR threshold
    lfcThreshold = 1 # log2 fold change threshold
)

# Sort and save results
res_deseq2 <- res_deseq2[order(res_deseq2$padj),]
res_deseq2_df <- as.data.frame(res_deseq2)
fwrite(res_deseq2_df, "DEG_DESeq2_p0_fc1.tsv", sep = "\t", row.names = T)
```

### edgeR Analysis Pipeline

```r
# Create DGEList object
dge <- DGEList(
    counts = count_tbl_low_rm, 
    samples = meta
)

# Create design matrix
design <- model.matrix(~ dge$samples$Treatment)

# Normalize library sizes
dge <- normLibSizes(dge)

# Estimate dispersion
dge <- estimateDisp(dge, design)

# Set reference level
dge$samples$Treatment <- relevel(dge$samples$Treatment, ref = "KRAS")

# Perform quasi-likelihood F-tests
fit <- glmQLFit(dge, design)
qlf <- glmQLFTest(fit, coef = 2)
res_qlf <- topTags(qlf, n = Inf, adjust.method = "BH")
res_qlf_df <- res_qlf$table

# Save results
fwrite(res_qlf_df, "DEG_edgeR_qlf_p0_fc1.tsv", sep = "\t", row.names = T)
```

## Comparing Results Across Methods

```r
# Read limma results from previous analysis
res_limma <- fread("DEG_P1_lfc0_limma.tsv")

# Filter significant DEGs for each method (adjusted P < 0.05 & FC > 2)
res_limma_sig <- res_limma[adj.P.Val < 0.05 & abs(logFC) > 1, ]

res_deseq2_sig <- res_deseq2_df[
    res_deseq2_df$padj < 0.05 & 
    abs(res_deseq2_df$log2FoldChange) > 1, ]

res_qlf_df_sig <- res_qlf_df[
    res_qlf_df$FDR < 0.05 & 
    abs(res_qlf_df$logFC) > 1, ]

# Separate up/down-regulated genes
res_sig_up <- list(
    limma_up = res_limma_sig[logFC > 0][[1]],
    deseq2_up = rownames(res_deseq2_sig[res_deseq2_sig$log2FoldChange > 0, ]),
    edger_qlf_up = rownames(res_qlf_df_sig[res_qlf_df_sig$logFC > 0, ])
)

res_sig_dn <- list(
    limma_dn = res_limma_sig[logFC < 0][[1]],
    deseq2_dn = rownames(res_deseq2_sig[res_deseq2_sig$log2FoldChange < 0, ]),
    edger_qlf_dn = rownames(res_qlf_df_sig[res_qlf_df_sig$logFC < 0, ])
)

# Create Venn diagrams
venn.diagram(
    x = res_sig_up, 
    filename = "Venn_DEG_up.png", 
    output = TRUE,
    resolution = 300, 
    imagetype = "png", 
    height = 1500, 
    width = 1500
)

venn.diagram(
    x = res_sig_dn, 
    filename = "Venn_DEG_dn.png", 
    output = TRUE,
    resolution = 300, 
    imagetype = "png", 
    height = 1500, 
    width = 1500
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/02/venn-e1739562639394-1024x352.png?resize=1024%2C352&ssl=1)

Despite the limited sample size of our example dataset, we observed a remarkable level of agreement in the differentially expressed genes (DEGs) identified by limma, edgeR, and DESeq2. This concordance across methods strengthens our confidence in the results, as each tool uses distinct statistical approaches yet arrives at similar biological conclusions. Our example data is small and simple, which may result in significant difference among the three methods.

Extensive benchmark studies have provided valuable insights into the relative strengths of these tools. Limma demonstrates remarkable versatility and robustness across diverse experimental conditions, particularly excelling in handling outliers that might skew results in other methods. Its computational efficiency becomes especially apparent when processing large-scale datasets containing thousands of samples or genes. However, limma’s statistical framework relies on having sufficient data points to estimate variance reliably, which translates to a requirement of at least three biological replicates per experimental condition. This requirement ensures the tool maintains its statistical power to accurately identify true differential expression.

DESeq2 and edgeR share many performance characteristics, which isn’t surprising given their common foundation in negative binomial modeling. Both tools perform admirably in benchmark studies using both real experimental data and simulated datasets where true differential expression is known. However, they do show subtle differences in their sweet spots – edgeR particularly shines when analyzing genes with low expression counts, where its flexible dispersion estimation can better capture the inherent variability in sparse count data.

## Conclusion

Understanding these nuances helps inform tool selection based on your specific experimental context. For instance, if you’re working with a well-replicated experiment and complex design, limma might be your best choice. Conversely, if your dataset includes many low-abundance transcripts of biological interest, edgeR’s strengths in handling low counts could make it the optimal choice. DESeq2 often serves as an excellent middle ground, providing robust results across a wide range of experimental scenarios.

  Want to run this whole pipeline on your own data? [Join the live workshop →](https://ngs101.com/workshop/rna-seq-workshop/)

## References

- Soneson C, Delorenzi M. A comparison of methods for differential expression analysis of RNA-seq data. BMC Bioinformatics. 2013 Mar 9;14:91. doi: 10.1186/1471-2105-14-91. PMID: 23497356; PMCID: PMC3608160.

- Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biol. 2014;15(12):550. doi: 10.1186/s13059-014-0550-8. PMID: 25516281; PMCID: PMC4302049.

- Law CW, Chen Y, Shi W, Smyth GK. Voom: precision weights unlock linear model analysis tools for RNA-seq read counts. Genome Biol. 2014;15:29. doi: 10.1186/gb-2014-15-2-r29.
