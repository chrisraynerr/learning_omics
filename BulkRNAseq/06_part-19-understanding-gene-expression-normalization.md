# How to Analyze RNAseq Data for Absolute Beginners Part 19: Understanding RNA-Seq Gene Expression Normalization

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 25, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/)

- [Introduction: Why Normalization Matters in RNA-Seq Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Introduction_Why_Normalization_Matters_in_RNA-Seq_Analysis)
- [Understanding Expression Metrics: From Raw Counts to Normalized Values](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Understanding_Expression_Metrics_From_Raw_Counts_to_Normalized_Values)

  - [The Challenge of Raw Count Data](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#The_Challenge_of_Raw_Count_Data)
  - [Common Expression Metrics and Their Applications](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Common_Expression_Metrics_and_Their_Applications)
  - [Mathematical Relationships Between Metrics](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Mathematical_Relationships_Between_Metrics)

- [Practical Implementation: Converting Between Expression Metrics](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Practical_Implementation_Converting_Between_Expression_Metrics)

  - [Setting Up Your R Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Setting_Up_Your_R_Environment)
  - [Extracting Gene Lengths from GTF](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Extracting_Gene_Lengths_from_GTF)
  - [Converting to Different Metrics](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Converting_to_Different_Metrics)

- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-19-understanding-rna-seq-gene-expression-normalization/#Like_this)

## Introduction: Why Normalization Matters in RNA-Seq Analysis

RNA sequencing (RNA-seq) has revolutionized our ability to measure gene expression, but the raw data needs careful processing to yield meaningful biological insights. In this comprehensive guide, we’ll explore why normalization is crucial and how to convert between different expression metrics using R.

Building on our previous tutorials in this series, we’ll dive deep into the various ways to represent gene expression data. Whether you’re analyzing differential expression or comparing samples across experiments, understanding these normalization methods is essential for robust analysis.

## Understanding Expression Metrics: From Raw Counts to Normalized Values

### The Challenge of Raw Count Data

Raw sequencing counts pose several challenges:

- Varying sequencing depths between samples

- Gene length biases

- Technical variations that can mask biological signals

These issues make direct comparisons problematic, which is why we need normalization methods.

### Common Expression Metrics and Their Applications

Let’s explore the main metrics used in RNA-seq analysis:

**Count****TPM****CPM****FPKM****Z-score**

| Metric | Depth Normalized? | Length Normalized? | DE Analysis | Statistical Modeling | Within-Sample Comparison | Between-Sample Comparison | Visualization |
| --- | --- | --- | --- | --- | --- | --- | --- |
|  | ❌ | ❌ | ✅ (Best for DESeq2/edgeR) | ✅ (Negative binomial models) | ❌ (Gene length bias) | ❌ (Library size bias) | ❌ (Use log-transformed counts) |
|  | ✅ | ✅ | ❌ (Not recommended) | ❌ (No cross-sample correction) | ✅ (Best for within-sample) | ❌ (Cannot compare between samples) | ✅ (Good for heatmaps, boxplots) |
|  | ✅ | ❌ | ✅ (After TMM normalization in edgeR) | ✅ (Used for filtering genes) | ✅ (Better than counts for visualization) | ❌ (No gene length correction) | ✅ (PCA, boxplots, density plots) |
|  | ✅ | ✅ | ❌ (Obsolete, replaced by TPM) | ❌ (Not robust for DE analysis) | ✅ (Within-sample isoform comparison) | ❌ (Affected by sequencing depth) | ✅ (Isoform expression analysis) |
|  | ✅ (Depends on input) | ✅ (Depends on input) | ❌ (Not used for DE) | ✅ (Useful for clustering, PCA) | ✅ (Good for within-sample standardization) | ✅ (Can be used for cross-sample normalization) | ✅ (Heatmaps, clustering, PCA) |

### Mathematical Relationships Between Metrics

Here’s how these metrics relate to each other:

1. **Counts to CPM**:

  $$CPM = \frac{\text{Raw Counts}}{\text{Total Mapped Reads}} \times 10^6$$

1. **Counts to TPM**:

$$RPK = \frac{\text{Raw Counts}}{\text{Gene Length (kb)}}$$

 $$TPM = \frac{RPK}{\sum RPK} \times 10^6$$

1. **Counts to RPKM/FPKM**:

$$RPKM = \frac{\text{Raw Counts}}{\text{Gene Length (kb)} \times \text{Total Mapped Reads}} \times 10^9$$

1. **Counts to Z-Score:**

1. 

$$ Z = \frac{X – \mu}{\sigma} $$

- X: The expression value of a gene in a sample.

- μ: The mean expression of the gene across all samples.

- σ: The standard deviation of the gene’s expression across all samples.

## Practical Implementation: Converting Between Expression Metrics

### Setting Up Your R Environment

First, let’s prepare our R environment with the necessary packages and data structures:

```r
# Load required libraries
library(data.table)
library(rtracklayer)
library(edgeR)

# Read the count table
# Note: This continues from our previous tutorial on read counting
count_tbl <- fread("counts_raw.tsv", data.table = F)

# Set row names and remove the first column
rownames(count_tbl) <- count_tbl[[1]]
count_tbl <- count_tbl[, -1]
```

### Extracting Gene Lengths from GTF

A crucial step is obtaining accurate gene lengths:

```r
# Import GTF file
gtf <- import("~/Genome_Index/mm10/gencode.vM25.annotation.gtf")

# Create gene symbol mapping
gene_symbols <- unique(data.frame(
  gene_id = gtf$gene_id,
  gene_symbol = gtf$gene_name
))

# Calculate exon lengths
exon_lengths <- width(gtf[gtf$type == "exon"])
gene_ids <- gtf$gene_id[gtf$type == "exon"]

# Sum exon lengths by gene
gene_lengths <- tapply(exon_lengths, gene_ids, sum)

# Match genes with count table
gene_symbols <- gene_symbols[!duplicated(gene_symbols$gene_symbol), ]
rownames(gene_symbols) <- gene_symbols$gene_symbol
gene_symbols <- gene_symbols[rownames(count_tbl), ]

# Verify matching
if(!identical(rownames(count_tbl), rownames(gene_symbols))) {
  stop("Gene names don't match between count table and annotations")
}

gene_lengths <- gene_lengths[gene_symbols$gene_id]
names(gene_lengths) <- gene_symbols$gene_symbol
```

### Converting to Different Metrics

#### CPM Conversion

```r
# Create DGEList object
dge <- DGEList(counts = count_tbl, genes = as.data.frame(gene_lengths))

# Calculate CPM
cpm_values <- as.data.frame(cpm(dge))

# Save results
fwrite(cpm_values, "Expression_CPM.tsv", sep = "\t", row.names = T)
```

#### FPKM Conversion

```r
# Calculate normalization factors
dge <- calcNormFactors(dge)

# Convert to FPKM
rpkm_values <- as.data.frame(rpkm(dge))

# Save results
fwrite(rpkm_values, "Expression_FPKM.tsv", sep = "\t", row.names = T)
```

#### TPM Conversion

If you’ve used Salmon for transcript quantification (covered in [Part 10](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/) of my series), you’ll find that it automatically provides both raw counts and TPM values in its output. This is one of Salmon’s advantages – it handles the normalization process during the quantification step, saving you time and reducing potential conversion errors.

However, if you’re working with count data from other sources like HTSeq or featureCounts, you’ll need to perform the TPM conversion manually. Let’s examine a custom function that handles this conversion accurately:

```r
# Define TPM conversion function
counts_to_tpm <- function(counts, gene_lengths) {
  # Calculate reads per kilobase
  rpk <- counts / (gene_lengths / 1000)

  # Calculate scaling factor
  scaling_factor <- colSums(rpk) / 1e6

  # Convert to TPM
  tpm <- sweep(rpk, 2, scaling_factor, "/")

  return(tpm)
}

# Convert to TPM
tpm_values <- counts_to_tpm(count_tbl, gene_lengths)

# Save results
fwrite(tpm_values, "Expression_TPM.tsv", sep = "\t", row.names = T)
```

#### Z-Score Conversion

```r
# Function to calculate Z-scores with additional options
counts_to_zscore <- function(counts, log_transform = TRUE, pseudo_count = 1) {
    # Step 1: Optional log transformation
    if (log_transform) {
        # Add pseudo-count to avoid log(0)
        expression_matrix <- log2(counts + pseudo_count)
    } else {
        expression_matrix <- counts
    }
    
    # Step 2: Calculate Z-scores
    # scale() centers and scales the data by default
    z_scores <- scale(expression_matrix)
    
    # Convert to data frame and maintain row names
    z_scores_df <- as.data.frame(z_scores)
    rownames(z_scores_df) <- rownames(counts)
    
    return(z_scores_df)
}

# Calculate Z-scores (with log transformation)
zscore_values <- counts_to_zscore(count_tbl)

# Save results
fwrite(zscore_values, "Expression_ZScore.tsv", sep = "\t", row.names = T)
```

## Conclusion

Understanding and properly implementing gene expression normalization is crucial for meaningful RNA-seq analysis. Each metric serves specific purposes, and choosing the right one depends on your analytical goals. Remember to validate your results and maintain consistent methods throughout your analysis pipeline.

  Want to run this whole pipeline on your own data? [Join the live workshop →](https://ngs101.com/workshop/rna-seq-workshop/)

## References

Corchete, L.A., Rojas, E.A., Alonso-López, D. et al. Systematic comparison and assessment of RNA-seq procedures for gene expression quantitative analysis. Sci Rep 10, 19737 (2020). https://doi.org/10.1038/s41598-020-76881-x
