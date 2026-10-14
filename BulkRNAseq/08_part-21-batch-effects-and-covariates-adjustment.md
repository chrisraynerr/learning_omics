# How to Analyze RNAseq Data for Absolute Beginners 21: A Comprehensive Guide to Batch Effects & Covariates Adjustment

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/)

- [Introduction to Batch Effects in RNA-seq Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Introduction_to_Batch_Effects_in_RNA-seq_Analysis)

  - [What Causes Batch Effects?](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#What_Causes_Batch_Effects)
  - [Why Batch Effects Matter in RNA-seq Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Why_Batch_Effects_Matter_in_RNA-seq_Analysis)

- [Understanding Batch Effect Adjustment Strategies](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Understanding_Batch_Effect_Adjustment_Strategies)

  - [1. Correction Methods](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#1_Correction_Methods)
  - [2. Statistical Modeling Approaches](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#2_Statistical_Modeling_Approaches)

- [Practical Implementation: Setting Up Your Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Practical_Implementation_Setting_Up_Your_Environment)
- [Data Preparation and Preprocessing](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Data_Preparation_and_Preprocessing)
- [Visualizing Batch Effects in Your Data](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Visualizing_Batch_Effects_in_Your_Data)
- [Three Methods for Correcting Batch Effects](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Three_Methods_for_Correcting_Batch_Effects)

  - [Method 1: Batch Correction Using ComBat-seq](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Method_1_Batch_Correction_Using_ComBat-seq)
  - [Method 2: Batch Correction Using removeBatchEffect from limma](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Method_2_Batch_Correction_Using_removeBatchEffect_from_limma)
  - [Method 3: Batch Correction Using Mixed Linear Models](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Method_3_Batch_Correction_Using_Mixed_Linear_Models)

- [Integrating Batch Effect Adjustment in Differential Expression Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Integrating_Batch_Effect_Adjustment_in_Differential_Expression_Analysis)

  - [Including Batch in DESeq2 Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Including_Batch_in_DESeq2_Analysis)
  - [Including Batch in edgeR and limma Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Including_Batch_in_edgeR_and_limma_Analysis)

- [Best Practices for Batch Effect Adjustment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Best_Practices_for_Batch_Effect_Adjustment)

  - [When to Adjust for Batch Effects](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#When_to_Adjust_for_Batch_Effects)
  - [Determining Which Variables to Include as Covariates](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Determining_Which_Variables_to_Include_as_Covariates)
  - [Avoiding Common Pitfalls](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Avoiding_Common_Pitfalls)

- [Conclusion and Next Steps](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Conclusion_and_Next_Steps)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/#Like_this)

## Introduction to Batch Effects in RNA-seq Analysis

In high-throughput sequencing experiments, batch effects represent one of the most challenging technical hurdles researchers face. These systematic variations arise not from biological differences between samples but from technical factors in the experimental process. Understanding and properly adjusting for batch effects is essential for generating reliable and reproducible research findings.

### What Causes Batch Effects?

Batch effects can originate from multiple sources throughout the experimental workflow:

- Different sequencing runs or instruments

- Variations in reagent lots or manufacturing batches

- Changes in sample preparation protocols

- Different personnel handling the samples

- Environmental conditions (temperature, humidity)

- Time-related factors when experiments span weeks or months

These seemingly minor technical variations can create significant artifacts in your data that may be mistakenly interpreted as biological signals if not properly addressed.

### Why Batch Effects Matter in RNA-seq Analysis

The impact of batch effects extends to virtually all aspects of RNA-seq data analysis:

- Differential expression analysis may identify genes that differ between batches rather than between biological conditions

- Clustering algorithms might group samples by batch rather than by true biological similarity

- Pathway enrichment analysis could highlight technical artifacts instead of meaningful biological processes

- Meta-analyses combining data from multiple sources become particularly vulnerable

This makes batch effect detection and correction a critical step in your RNA-seq analysis pipeline, especially for large-scale studies where samples are processed in multiple batches over time.

## Understanding Batch Effect Adjustment Strategies

Now that we understand why batch effects are problematic, let’s explore the methodologies available for addressing them. There are two primary approaches to handling batch effects in RNA-seq analysis:

### 1. Correction Methods

These approaches transform your data to remove batch-related variation while preserving the biological signals of interest. Common correction methods include:

- **Empirical Bayes methods**: Particularly useful for small sample sizes, as they borrow information across genes

- **Linear model adjustments**: Remove estimated batch effects using linear regression techniques

- **Mixed linear models**: Account for both fixed and random effects in your experimental design

### 2. Statistical Modeling Approaches

Instead of directly transforming the data, these approaches incorporate batch information into your statistical models:

- **Including batch as a covariate**: Common in differential expression analysis frameworks like DESeq2, edgeR, and limma

- **Surrogate variable analysis**: Useful when batch information is incomplete or unknown

Choosing the appropriate approach depends on your specific experimental design, the nature of the batch effects, and your downstream analysis goals.

Now, let’s move from theory to practice and implement these concepts with real data.

## Practical Implementation: Setting Up Your Environment

Before we dive into batch effect correction, we need to set up our R environment with the necessary packages. The following code installs and loads all the required libraries:

```r
# Install CRAN packages
install.packages(c("data.table", "ggplot2", "ggprism"), dependencies = TRUE)

# Install Bioconductor packages
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install(c("GEOquery", "limma", "edgeR", "sva", "lme4"), dependencies = TRUE)

# Load the libraries
library(data.table)
library(ggplot2)
library(ggprism)
library(limma)
library(edgeR)
library(sva)
library(lme4)
```

> 

**Tip:** If you encounter any package installation issues, try installing them one at a time and check if you have the necessary dependencies for Bioconductor packages.

## Data Preparation and Preprocessing

We’ll use a publicly available dataset from the [ComBat-seq GitHub repository](https://github.com/zhangyuqing/ComBat-seq/blob/master/real_data_application/signature_data.rds) to demonstrate batch effect adjustment techniques. This dataset provides a practical example of sequencing data with batch effects.

```r
# Download and reformat the example dataset
data_exp <- readRDS("signature_data.rds")
exprmx <- data_exp@assays@data$counts
meta <- as.data.frame(data_exp@colData)[1:3]

# Clean up metadata
meta$batch <- as.factor(meta$batch)
meta$treatment <- sample(meta$group, 89)  # Creating a simulated treatment variable

# Filter low-expressed genes
# Note: This step is crucial as low-count genes can introduce noise
perc_keep <- 0.8  # Keep genes expressed in at least 80% of samples
gene_keep <- rowSums(exprmx > 0) >= ceiling(perc_keep * ncol(exprmx))

# Create a filtered count matrix
count_tbl_low_rm <- as.matrix(exprmx[gene_keep, ])
```

## Visualizing Batch Effects in Your Data

Before attempting any correction, it’s important to visualize batch effects to understand their magnitude and pattern. Principal Component Analysis (PCA) provides an excellent tool for this purpose.

```r
# Transpose count matrix for PCA
count_tbl_low_rm_t <- as.data.frame(t(count_tbl_low_rm))

# Perform PCA
pca_prep <- prcomp(count_tbl_low_rm_t, scale. = TRUE)

# Create PCA plot colored by batch
pca_plot_batch <- autoplot(pca_prep, label = F, data = meta, colour = "batch") + 
  theme_prism(base_size = 16) +
  ggtitle("PCA Plot Before Batch Effect Correction") +
  theme(plot.title = element_text(hjust = 0.5))

# Save the plot
ggsave("pca_plot_batch_unadjusted.png", pca_plot_batch, 
       device = "png", units = "cm", height = 12, width = 14)
```

When you examine the resulting PCA plot, look for clustering by batch rather than by biological condition. If samples cluster primarily by batch, this confirms the presence of significant batch effects that require correction.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/02/pca_plot_batch_unadjusted.png?resize=1024%2C1024&ssl=1)

## Three Methods for Correcting Batch Effects

Now we’ll demonstrate three different approaches to batch effect correction, each with its own advantages and appropriate use cases.

### Method 1: Batch Correction Using ComBat-seq

ComBat-seq is specifically designed for RNA-seq count data and uses an empirical Bayes framework to adjust for batch effects while preserving biological signals.

```r
# Adjust for batch effects using ComBat-seq
# Note: This method works directly on count data
count_tbl_low_rm_combat_corrected <- ComBat_seq(count_tbl_low_rm, 
                                               batch = meta$batch, 
                                               group = meta$treatment)

# Visualize the results with PCA
pca_prep_batch_combat <- prcomp(t(count_tbl_low_rm_combat_corrected), scale. = TRUE)

combat_pca_plot <- autoplot(pca_prep_batch_combat, 
                           label = F, 
                           data = meta, 
                           colour = "batch") + 
  theme_prism(base_size = 16) +
  ggtitle("PCA Plot After ComBat-seq Correction") +
  theme(plot.title = element_text(hjust = 0.5))

# Display and save the plot
print(combat_pca_plot)
ggsave("pca_plot_batch_combat_corrected.png", combat_pca_plot, 
       device = "png", units = "cm", height = 12, width = 14)
```

In the corrected PCA plot, you should now observe less distinct clustering by batch, indicating successful batch effect reduction.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/02/pca_plot_batch_combat.png?resize=1024%2C878&ssl=1)

### Method 2: Batch Correction Using removeBatchEffect from limma

The limma package offers the removeBatchEffect function, which works on normalized expression data rather than raw counts. This method is particularly well-integrated with the limma-voom workflow.

```r
# Create a DGEList object
dge <- DGEList(counts=count_tbl_low_rm, samples = meta)

# Calculate normalization factors using Trimmed Mean of M-values (TMM)
dge <- calcNormFactors(dge, method = "TMM")

# Normalize gene expression with the voom method
# voom transforms count data to log-CPM values suitable for linear modeling
dge_v <- voom(dge, plot=TRUE)

# Correct batch effects using removeBatchEffect
batch_corrected_limma <- removeBatchEffect(dge_v$E, batch = dge_v$targets$batch)

# Visualize the results with PCA
pca_prep_batch_limma <- prcomp(t(batch_corrected_limma), scale. = TRUE)

limma_pca_plot <- autoplot(pca_prep_batch_limma, 
                          label = F, 
                          data = dge_v$targets, 
                          colour = "batch") + 
  theme_prism(base_size = 16) +
  ggtitle("PCA Plot After limma Batch Correction") +
  theme(plot.title = element_text(hjust = 0.5))

# Display and save the plot
print(limma_pca_plot)
ggsave("pca_plot_batch_limma_corrected.png", limma_pca_plot, 
       device = "png", units = "cm", height = 12, width = 14)
```

> 

**Important Note:** The removeBatchEffect function should not be used directly for differential expression analysis. Instead, include batch as a covariate in your design matrix as shown in the later sections.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/02/pca_plot_batch_limma.png?resize=1024%2C878&ssl=1)

### Method 3: Batch Correction Using Mixed Linear Models

Mixed linear models (MLM) provide a sophisticated approach that can handle more complex experimental designs, including nested and crossed random effects.

```r
# Create a function for batch effects adjustment using mixed linear model
adjust_batch_mlm <- function(gene_expression, metadata) {
  # Build model with fixed effect for treatment and random effect for batch
  model <- lmer(gene_expression ~ treatment + (1 | batch), data = metadata)

  # Return residuals (batch-effect removed) plus intercept (baseline expression)
  return(residuals(model) + fixef(model)[1])
}

# Apply the function to each gene in the expression matrix
expr_matrix_corrected <- t(apply(dge_v$E, 1, adjust_batch_mlm, dge_v$targets))

# Visualize the results with PCA
pca_prep_batch_mlm <- prcomp(t(expr_matrix_corrected), scale. = TRUE)

mlm_pca_plot <- autoplot(pca_prep_batch_mlm, 
                        label = F, 
                        data = dge_v$targets, 
                        colour = "batch") + 
  theme_prism(base_size = 16) +
  ggtitle("PCA Plot After Mixed Linear Model Correction") +
  theme(plot.title = element_text(hjust = 0.5))

# Display and save the plot
print(mlm_pca_plot)
ggsave("pca_plot_batch_mlm_corrected.png", mlm_pca_plot, 
       device = "png", units = "cm", height = 12, width = 14)
```

This approach is particularly powerful when you have multiple random effects or when batch effects have a hierarchical structure.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/02/pca_plot_batch_mlm.png?resize=1024%2C878&ssl=1)

## Integrating Batch Effect Adjustment in Differential Expression Analysis

Rather than correcting the data before analysis, a more statistically sound approach is to incorporate batch information directly into your differential expression model. This section demonstrates how to adjust for batch effects during differential expression analysis with popular packages.

### Including Batch in DESeq2 Analysis

```r
# Create a DESeq2 object with batch in the design formula
obj_deseq2 <- DESeqDataSetFromMatrix(
    countData = count_tbl_low_rm,
    colData = meta,
    design = ~ batch + treatment  # Note: batch comes first in the formula
)

# Proceed with standard DESeq2 workflow
obj_deseq2 <- DESeq(obj_deseq2)
res_deseq2 <- results(obj_deseq2)
```

> 

**Cross-reference:** For a detailed tutorial on DESeq2, limma, and edgeR, see “[How to Analyze RNAseq Data for Absolute Beginners Part 20: Comparing limma, DESeq2, and edgeR in Differential Expression Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/)“.

### Including Batch in edgeR and limma Analysis

```r
# Create design matrix with batch as a covariate
design <- model.matrix(~ batch + treatment, data = meta)

# For edgeR:
dge <- DGEList(counts = count_tbl_low_rm, samples = meta)
dge <- calcNormFactors(dge)
dge <- estimateDisp(dge, design)
fit <- glmQLFit(dge, design)
qlf <- glmQLFTest(fit, coef = grep("treatment", colnames(design)))
qlf_top <- topTags(qlf, n = Inf, adjust.method = "BH")
qlf_top <- qlf_top$table

# For limma-voom:
v <- voom(dge, design, plot = TRUE)
fit <- lmFit(v, design)
fit <- eBayes(fit)
top_genes <- topTable(fit, coef = grep("treatment", colnames(design)), n = Inf)
```

> 

**Important note:** The order of terms in the design formula matters. Placing batch before the treatment of interest ensures that batch effects are accounted for before estimating treatment effects.

## Best Practices for Batch Effect Adjustment

### When to Adjust for Batch Effects

Batch effect correction should be applied when:

1. Samples were processed in different batches

2. Data visualization (e.g., PCA) reveals clustering by technical factors

3. Combining data from multiple sources or studies

4. Technical replicates do not cluster together

However, correction may not be necessary or even appropriate if:

1. Batch and biological condition are completely confounded

2. Batch effects are minimal compared to biological variation

3. Sample size per batch is very small

### Determining Which Variables to Include as Covariates

When designing your experiment and analysis, consider including these common covariates:

1. **Technical factors:** batch, sequencing facility, technician

2. **Sample characteristics:** tissue type, disease stage, age, sex

3. **Quality metrics:** RNA quality (RIN), sequencing depth, GC content

### Avoiding Common Pitfalls

1. **Complete confounding:** If your condition of interest perfectly aligns with batch (e.g., all controls sequenced in batch 1, all treatments in batch 2), batch correction becomes impossible without additional samples.

2. **Overcorrection:** Aggressive batch correction can remove true biological signals. Always visualize your data before and after correction.

3. **Undercorrection:** Insufficient adjustment can leave batch effects in your data. Verify correction effectiveness through visualization.

4. **Incorrect order in design formula:** Place the batch variable before your variable of interest in the design formula.

5. **Applying batch correction twice:** Don’t correct the data and then include batch in your model. Choose one approach.

## Conclusion and Next Steps

Proper batch effect adjustment is crucial for robust RNA-seq analysis. By implementing the methods described in this tutorial, you can significantly improve the reliability of your findings and avoid drawing incorrect conclusions due to technical artifacts.

After batch correction, you’re ready to proceed with:

1. Differential expression analysis

2. Pathway enrichment

3. Gene set analysis

4. Clustering and classification

Remember that batch effect adjustment is not a one-size-fits-all process. The optimal approach depends on your specific experimental design, the nature of your data, and your research questions.

  Want to run this whole pipeline on your own data? [Join the live workshop →](https://ngs101.com/workshop/rna-seq-workshop/)

## References

- Stephanie C. Hicks, F. William Townes, Mingxiang Teng, Rafael A. Irizarry. bioRxiv 025528; doi: https://doi.org/10.1101/025528

- Johnson, W. E., Li, C., & Rabinovic, A. (2007). Adjusting batch effects in microarray expression data using empirical Bayes methods. Biostatistics, 8(1), 118-127.

- Leek, J. T., Johnson, W. E., Parker, H. S., Jaffe, A. E., & Storey, J. D. (2012). The sva package for removing batch effects and other unwanted variation in high-throughput experiments. Bioinformatics, 28(6), 882-883.

- Zhang, Y., Parmigiani, G., & Johnson, W. E. (2020). ComBat-seq: batch effect adjustment for RNA-seq count data. NAR Genomics and Bioinformatics, 2(3), lqaa078.
