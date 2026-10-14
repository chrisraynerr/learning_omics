# How to Cluster RNA-seq Data to Uncover Gene Expression Patterns: Hierarchical and K-means Methods for Absolute Beginners

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/)

- [Introduction: Understanding Clustering in RNA-seq Analysis](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Introduction_Understanding_Clustering_in_RNA-seq_Analysis)

  - [What is Clustering in Bulk RNA-seq Analysis?](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#What_is_Clustering_in_Bulk_RNA-seq_Analysis)
  - [What Biological Insights Can We Gain From Clustering Analysis?](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#What_Biological_Insights_Can_We_Gain_From_Clustering_Analysis)
  - [Hierarchical Clustering vs. K-means Clustering: Understanding the Differences](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Hierarchical_Clustering_vs_K-means_Clustering_Understanding_the_Differences)
  - [When to Use Each Clustering Method in RNA-seq Analysis](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#When_to_Use_Each_Clustering_Method_in_RNA-seq_Analysis)
  - [The Workflow Ahead](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#The_Workflow_Ahead)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Setting_Up_Your_Analysis_Environment)

  - [Installing Essential R Packages](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Installing_Essential_R_Packages)

- [Preparing Example Data for Analysis](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Preparing_Example_Data_for_Analysis)

  - [Downloading and Extracting Data from GEO](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Downloading_and_Extracting_Data_from_GEO)
  - [Extracting and Organizing Sample Groups](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Extracting_and_Organizing_Sample_Groups)
  - [Preparing Count Matrix for Analysis](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Preparing_Count_Matrix_for_Analysis)
  - [Normalizing Count Data with DESeq2](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Normalizing_Count_Data_with_DESeq2)
  - [Identifying Differentially Expressed Genes](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Identifying_Differentially_Expressed_Genes)

- [Understanding Scaling in Clustering Analysis](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Understanding_Scaling_in_Clustering_Analysis)

  - [Why Scaling Matters](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Why_Scaling_Matters)
  - [Applying Scaling to Our Data](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Applying_Scaling_to_Our_Data)

- [Hierarchical Clustering Analysis of Differentially Expressed Genes](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Hierarchical_Clustering_Analysis_of_Differentially_Expressed_Genes)

  - [Performing Hierarchical Clustering](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Performing_Hierarchical_Clustering)
  - [Extracting Discrete Gene Clusters](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Extracting_Discrete_Gene_Clusters)
  - [Visualizing Clusters with Separated Gaps](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Visualizing_Clusters_with_Separated_Gaps)

- [K-means Clustering Analysis of Differentially Expressed Genes](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#K-means_Clustering_Analysis_of_Differentially_Expressed_Genes)

  - [Determining Optimal K: The Elbow Method](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Determining_Optimal_K_The_Elbow_Method)
  - [Silhouette Analysis for K Selection](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Silhouette_Analysis_for_K_Selection)
  - [Applying K-means Clustering](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Applying_K-means_Clustering)
  - [Visualizing Cluster Centroids](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Visualizing_Cluster_Centroids)

- [Comparing Hierarchical and K-means Results](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Comparing_Hierarchical_and_K-means_Results)
- [Best Practices for Clustering Analysis](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Best_Practices_for_Clustering_Analysis)

  - [Data Quality and Preparation](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Data_Quality_and_Preparation)
  - [Parameter Selection Guidelines](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Parameter_Selection_Guidelines)
  - [Validation and Quality Control](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Validation_and_Quality_Control)
  - [Visualization Best Practices](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Visualization_Best_Practices)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Common_Pitfalls_to_Avoid)
  - [Downstream Analysis Recommendations](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Downstream_Analysis_Recommendations)

- [Conclusion: From Patterns to Biology](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Conclusion_From_Patterns_to_Biology)
- [References](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#References)

  - [Share this:](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Share_this)
  - [Like this:](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/#Like_this)

## Introduction: Understanding Clustering in RNA-seq Analysis

In the vast landscape of gene expression data, patterns often hide in plain sight. Among thousands of genes measured simultaneously, groups of genes may share similar expression patterns across samples, suggesting coordinated biological functions or responses. Clustering analysis serves as a powerful computational microscope that brings these hidden patterns into focus, transforming complex datasets into interpretable biological insights.

### What is Clustering in Bulk RNA-seq Analysis?

Clustering is a fundamental unsupervised machine learning technique that groups similar entities together based on their characteristics. In the context of bulk RNA-seq analysis, clustering can be applied in two complementary ways:

**Sample Clustering:** Groups samples (e.g., patients, tissues, or experimental conditions) based on their overall gene expression profiles. This approach helps identify subgroups within your dataset, such as distinct disease subtypes, tissue types, or treatment responses. For example, in cancer research, sample clustering might reveal that patients diagnosed with the same cancer type actually fall into distinct molecular subtypes with different prognoses and treatment responses.

**Gene Clustering:** Groups genes based on their expression patterns across samples. Genes that cluster together often share biological functions, belong to the same pathway, or are co-regulated by common transcription factors. For instance, genes involved in cell cycle progression typically show coordinated expression patterns, rising and falling together as cells move through different phases.

> 

**Beginner’s Tip:** Think of clustering like organizing a library. Sample clustering is like grouping books by genre (fiction, non-fiction, science), while gene clustering is like organizing books that share similar themes or topics together, regardless of their genre.

### What Biological Insights Can We Gain From Clustering Analysis?

Clustering analysis unlocks multiple layers of biological understanding:

**Disease Subtype Discovery:** In cancer genomics, clustering has revolutionized our understanding of disease heterogeneity. What was once considered a single disease entity often reveals itself as multiple molecular subtypes with distinct therapeutic vulnerabilities. The landmark discovery of breast cancer subtypes (Luminal A, Luminal B, HER2-enriched, and Basal-like) through gene expression clustering has transformed clinical practice, enabling personalized treatment strategies.

**Pathway-Level Understanding:** When genes cluster together, they often participate in common biological processes. Identifying these co-expression modules helps researchers understand which pathways are active or disrupted in different conditions. For example, if a cluster of genes all involved in inflammation shows coordinated upregulation, it suggests an inflammatory response is occurring.

**Biomarker Identification:** Genes that distinguish between sample clusters can serve as potential biomarkers for disease diagnosis, prognosis, or treatment response prediction. These signature genes often become targets for clinical assay development.

**Temporal Dynamics:** In time-course experiments, clustering reveals genes that follow similar temporal patterns, helping identify early response genes versus late response genes, or genes that show transient versus sustained changes.

### Hierarchical Clustering vs. K-means Clustering: Understanding the Differences

While both methods group similar entities together, they approach the clustering problem from fundamentally different perspectives:

**Hierarchical Clustering** builds a tree-like structure (dendrogram) that shows relationships at multiple scales. Starting with each data point as its own cluster, the algorithm iteratively merges the most similar clusters until all points are connected in a single tree. This approach:

- Reveals hierarchical relationships between clusters

- Doesn’t require you to specify the number of clusters beforehand

- Produces a dendrogram that visualizes cluster relationships

- Can be computationally intensive for very large datasets

- Provides more interpretable results through the tree structure

**K-means Clustering** partitions data into a predetermined number (K) of distinct, non-overlapping clusters. The algorithm iteratively assigns each point to the nearest cluster center and updates cluster centers until convergence. This approach:

- Requires you to specify K (the number of clusters) in advance

- Produces compact, well-separated clusters

- Runs efficiently even on large datasets

- May produce different results across runs due to random initialization

- Works best when clusters are roughly spherical and similar in size

> 

**Key Difference:** Hierarchical clustering asks “How are these data points related?”, revealing nested groupings. K-means asks “How can I partition these data points into K groups?”, producing discrete assignments.

### When to Use Each Clustering Method in RNA-seq Analysis

The choice between hierarchical and K-means clustering depends on your biological question and data characteristics:

**Use Hierarchical Clustering When:**

1. **Exploring Data Without Assumptions:** You don’t know how many distinct groups exist in your data and want to explore the natural structure at multiple resolutions

2. **Understanding Relationships:** You want to see how groups relate to each other hierarchically (e.g., understanding how cancer subtypes might share characteristics)

3. **Quality Control:** Checking if replicates cluster together or if there are unexpected sample groupings that might indicate batch effects

4. **Small to Medium Datasets:** Your dataset size is manageable (typically up to a few hundred samples or a few thousand genes)

5. **Cancer Subtype Analysis:** As demonstrated in our [previous tutorial on cancer subtype prediction](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/), hierarchical clustering excels at revealing relationships between cancer molecular subtypes without requiring prior knowledge of subtype number

**Use K-means Clustering When:**

1. **Known Group Numbers:** You have a hypothesis about the number of distinct groups (e.g., from prior knowledge or preliminary analysis)

2. **Large Datasets:** You’re working with many samples or genes where hierarchical clustering becomes computationally prohibitive

3. **Discrete Partitioning Needed:** You need clear, non-overlapping assignments of genes or samples to specific groups for downstream analysis

4. **Iterative Refinement:** You plan to test multiple K values and select the optimal one using statistical criteria

5. **Co-expression Module Discovery:** Identifying groups of genes that show similar expression patterns for pathway analysis or regulatory network inference

**Practical Example – Cancer Subtype Analysis:**In our [cancer subtype prediction tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-6-a-comprehensive-guide-for-cancer-subtype-prediction/), hierarchical clustering revealed how different cancer samples naturally group based on their gene expression profiles. The dendrogram showed not only the main subtypes but also revealed which subtypes were more similar to each other, providing biological insights into disease evolution and potential treatment strategies. This exploratory approach was ideal because we wanted to discover patterns without imposing assumptions about the number of groups.

### The Workflow Ahead

In this tutorial, we’ll walk through both clustering methods step-by-step using a real RNA-seq dataset. You’ll learn how to:

- Prepare expression data for clustering through proper normalization and scaling

- Identify differentially expressed genes for focused clustering analysis

- Apply hierarchical clustering to reveal gene co-expression patterns

- Use dendrogram cutting to extract discrete gene clusters

- Determine the optimal number of clusters for K-means analysis

- Visualize clustering results through informative heatmaps

- Compare results between hierarchical and K-means approaches

By the end of this tutorial, you’ll have practical skills to apply clustering analysis to your own RNA-seq data, whether you’re exploring disease heterogeneity, identifying co-regulated genes, or validating experimental groupings.

## Setting Up Your Analysis Environment

### Installing Essential R Packages

We’ll install packages for data manipulation, statistical analysis, and visualization. Each package serves a specific purpose in our clustering workflow:

```r
#-----------------------------------------------
# STEP 0: Install and load required R packages
#-----------------------------------------------

# Install packages from CRAN (Comprehensive R Archive Network)
install.packages("BiocManager")      # Manages Bioconductor package installation
install.packages("pheatmap")         # Creates annotated heatmaps with clustering
install.packages("RColorBrewer")     # Provides color palettes for visualizations
install.packages("ggplot2")          # Advanced plotting system for R
install.packages("dplyr")            # Data manipulation and transformation
install.packages("tibble")           # Modern data frame implementation
install.packages("mclust")           # For Adjusted Rand Index calculation
install.packages("reshape2")         # For data reshaping
install.packages("cluster")          # For silhouette analysis

# Install packages from Bioconductor
BiocManager::install("DESeq2")       # Differential expression and normalization
BiocManager::install("GEOquery")     # Downloads data from GEO database

# Load all packages into the R session
library(DESeq2)          # For count normalization
library(GEOquery)        # For downloading GEO data
library(pheatmap)        # For heatmap visualization
library(RColorBrewer)    # For color schemes
library(ggplot2)         # For publication-quality plots
library(dplyr)           # For data wrangling
library(tibble)          # For enhanced data frames
library(mclust)          # For comparing clustering methods
library(reshape2)        # For melting confusion matrix

# Set working directory for analysis outputs
setwd("~/RNAseq_Clustering_Tutorial")
```

> 

**Note:** If you encounter installation errors, ensure your R version is 4.0 or higher. Some Bioconductor packages require recent R versions for compatibility.

## Preparing Example Data for Analysis

For this tutorial, we’ll use dataset GSE261875, which examines gene expression changes in a well-defined biological system. Let’s walk through downloading, processing, and preparing this data for clustering analysis.

### Downloading and Extracting Data from GEO

```r
#-----------------------------------------------
# STEP 1: Download and extract count data from GEO
#-----------------------------------------------

# Download the complete GEO dataset
# This command downloads the supplementary files and creates "GSE261875" folder
getGEOSuppFiles("GSE261875")

# List all files in the downloaded folder
files <- list.files("GSE261875", full.names = TRUE)

# Read the count data file (first file in the list)
count_data <- read.delim(files[1])

# Extract sample information (metadata)
sample_info <- getGEO("GSE261875", GSEMatrix = TRUE)
sample_info <- pData(sample_info[[1]])
```

> 

**What’s Happening Here:** GEO stores RNA-seq data in multiple formats. We download the supplementary files (which contain the raw count matrix) and separately retrieve sample metadata that describes experimental conditions.

### Extracting and Organizing Sample Groups

Understanding which samples belong to which experimental groups is crucial for both quality control and biological interpretation:

```r
#-----------------------------------------------
# STEP 2: Extract and organize sample group information
#-----------------------------------------------

# Create a clean sample metadata dataframe
# Extract treatment information from sample descriptions
sample_info <- data.frame(
  SampleID = sample_info$description,
  Treatment = gsub("\\d+$", "", sample_info$description)  # Remove trailing numbers
)
rownames(sample_info) <- sample_info[[1]]

# Save sample_info table
write.table(sample_info, "sample_info.tsv", sep = "\t", row.names = FALSE, quote = FALSE)

# Match sample IDs between sample_info and count_data
# Keep gene ID columns and matching samples
count_data <- cbind(count_data[, c(1, 2)], count_data[, rownames(sample_info)])

# Verify that sample IDs are matched correctly
identical(rownames(sample_info), colnames(count_data)[-c(1, 2)])
```

> 

**Data Cleaning Note:** The `gsub("\\d+$", "", ...)` command removes trailing numbers from sample names (e.g., “EV_AKO1” becomes “EV_AKO”), giving us clean treatment group labels.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/raw_counts-e1762916694852-1024x346.png?resize=1024%2C346&ssl=1)

### Preparing Count Matrix for Analysis

Before normalization, we need to properly format the count matrix:

```r
#-----------------------------------------------
# STEP 3: Prepare count matrix
#-----------------------------------------------

# Remove rows with missing values
count_data <- na.omit(count_data)

# Remove duplicate gene entries (keep first occurrence)
count_data <- count_data[!duplicated(count_data[[2]]), ]

# Set gene IDs as row names
rownames(count_data) <- count_data[[2]]

# Create a separate gene annotation dataframe
gene_data <- count_data[, c(2, 1)]
rownames(gene_data) <- gene_data[[1]]

# Extract only the count columns (remove gene ID columns)
count_data <- count_data[, -c(1, 2)]

# Filter low-count genes
# Keep genes with >5 counts in at least 50% of samples
keep <- rowSums(count_data > 5) >= 0.5 * ncol(count_data)
count_data <- count_data[keep, ]

# Update gene annotations to match filtered counts
gene_data <- gene_data[rownames(count_data), ]

# Save gene data
write.table(gene_data, "gene_data.tsv", sep = "\t", row.names = FALSE, quote = FALSE)
```

> 

**Filtering Rationale:** We require genes to have at least 5 counts in 50% of samples. This more stringent filtering removes lowly expressed genes that provide little biological information and can introduce noise into clustering analysis.

### Normalizing Count Data with DESeq2

Raw RNA-seq counts require normalization to account for technical variations like sequencing depth and RNA composition differences between samples:

```r
#-----------------------------------------------
# STEP 4: Normalize count data using DESeq2
#-----------------------------------------------

# Create DESeq2 dataset object
dds <- DESeqDataSetFromMatrix(
  countData = count_data,
  colData = sample_info,
  design = ~ Treatment
)

# Attach gene annotations to the DESeq2 object
rowData(dds) <- gene_data

# Perform size factor normalization
dds <- estimateSizeFactors(dds)

# Apply variance stabilizing transformation (VST)
vst_data <- vst(dds, blind = TRUE)
normalized_counts <- assay(vst_data)

# Save normalized expression data
write.table(normalized_counts, "normalized_counts.tsv", sep = "\t", row.names = TRUE, quote = FALSE)
```

> 

**Why VST?** Unlike simple log-transformation, variance stabilizing transformation (VST) accounts for the mean-variance relationship in RNA-seq data. Setting `blind = TRUE` means the transformation doesn’t use treatment information, making it appropriate for exploratory analyses like clustering.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/normalized_counts.png?resize=1024%2C122&ssl=1)

### Identifying Differentially Expressed Genes

For our clustering analyses, we’ll focus on differentially expressed genes (DEGs) between experimental conditions. This approach reduces noise and computational burden while highlighting biologically relevant patterns:

```r
#-----------------------------------------------
# STEP 5: Identify differentially expressed genes
#-----------------------------------------------

# Perform differential expression analysis
dds <- DESeq(dds)

# Extract results comparing EV_AKO treatment to EV control
res <- results(dds, contrast = c("Treatment", "EV_AKO", "EV"))

# Filter for significant DEGs
# Adjusted p-value < 0.05 and absolute log2 fold change > 1
sig_genes <- res[which(res$padj < 0.05 & abs(res$log2FoldChange) > 1), ]

# Extract DEG names and their expression values
deg_names <- rownames(sig_genes)
deg_expression <- normalized_counts[deg_names, ]
```

> 

**Parameter Selection:** We use `padj < 0.05` (5% false discovery rate) and `|log2FC| > 1` (2-fold change). This balances statistical significance with biological relevance, ensuring we focus on genes with substantial expression differences.

## Understanding Scaling in Clustering Analysis

Before performing clustering, we must address a critical data preparation step: scaling. This concept often confuses beginners, but understanding it is essential for meaningful clustering results.

### Why Scaling Matters

In gene expression data, different genes have vastly different expression magnitudes.

**The Problem:**

- Gene A: expression ranges from 1000 to 1200 (variation = 200)

- Gene B: expression ranges from 5 to 25 (variation = 20)

- Without scaling, Gene A’s large values will dominate distance calculations

- Gene B’s potentially important pattern might be ignored

**The Solution:**Scaling transforms each gene to have mean = 0 and standard deviation = 1, giving all genes equal weight in clustering regardless of their absolute expression levels.

### Applying Scaling to Our Data

```r
#-----------------------------------------------
# STEP 6: Scale gene expression data
#-----------------------------------------------

# Create annotation dataframe for samples
annotation_col <- data.frame(
  Group = colData(dds)$Treatment,
  row.names = rownames(colData(dds))
)

# Scale DEG expression data (row-wise/gene-wise scaling)
# Each gene is centered to mean=0 and scaled to sd=1
deg_scaled <- t(scale(t(deg_expression)))
```

> 

**Critical Note:** The `t(scale(t(data)))` construction is essential. The inner `t()` transposes the matrix so genes become columns (scale operates on columns), `scale()` standardizes each gene, and the outer `t()` returns to the original orientation with genes as rows.

## Hierarchical Clustering Analysis of Differentially Expressed Genes

Now we’ll apply hierarchical clustering to reveal gene expression patterns in our DEGs. By focusing on genes that differ significantly between conditions, we capture the most biologically relevant variation.

### Performing Hierarchical Clustering

```r
#-----------------------------------------------
# STEP 7: Hierarchical clustering of DEGs
#-----------------------------------------------

# Perform hierarchical clustering on DEGs (genes as rows)
# Distance: 1 - Pearson correlation (emphasizes pattern similarity)
# Linkage: complete (creates compact, well-separated clusters)
deg_gene_dist <- as.dist(1 - cor(t(deg_scaled), method = "pearson"))
deg_gene_hclust <- hclust(deg_gene_dist, method = "complete")

# Perform hierarchical clustering on samples (samples as columns)
# Distance: Euclidean (captures overall expression differences)
deg_sample_dist <- dist(t(deg_scaled), method = "euclidean")
deg_sample_hclust <- hclust(deg_sample_dist, method = "complete")

# Generate heatmap with hierarchical clustering
pheatmap(
  deg_scaled,
  cluster_rows = deg_gene_hclust,
  cluster_cols = deg_sample_hclust,
  annotation_col = annotation_col,
  show_rownames = FALSE,
  show_colnames = TRUE,
  color = colorRampPalette(c("blue", "white", "red"))(100),
  main = "Hierarchical Clustering: Differentially Expressed Genes",
  fontsize = 10,
  filename = "hierarchical_clustering_DEGs.png",
  width = 10,
  height = 8
)
```

> 

**Parameter Choices:**

- **Correlation-based distance** for genes: Focuses on expression patterns rather than magnitudes. Genes with similar patterns (both increasing or both decreasing) will cluster together.

- **Euclidean distance** for samples: Captures overall expression differences between samples.

- **Complete linkage**: Creates compact clusters with clear separation, ideal for biological interpretation.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/hierarchical_clustering_DEGs.png?resize=1024%2C819&ssl=1)

### Extracting Discrete Gene Clusters

While the dendrogram reveals hierarchical relationships, we often want discrete gene clusters for downstream analysis such as pathway enrichment or biomarker identification:

```r
#-----------------------------------------------
# STEP 8: Extract gene clusters from dendrogram
#-----------------------------------------------

# Cut the dendrogram to create discrete clusters
n_clusters <- 2
gene_clusters <- cutree(deg_gene_hclust, k = n_clusters)

# Create a dataframe showing cluster assignments
cluster_assignments <- data.frame(
  Gene = names(gene_clusters),
  Cluster = gene_clusters
)

# Count genes in each cluster
print(table(gene_clusters))
```

> 

**Choosing Cluster Number:** For this dataset, we use 2 clusters to separate genes upregulated in one condition from those downregulated. In your own analyses, examine the dendrogram structure to identify natural breakpoints, or use gap statistics to determine optimal cluster numbers.

### Visualizing Clusters with Separated Gaps

To better visualize the distinct gene clusters, we create a heatmap with gaps between clusters:

```r
#-----------------------------------------------
# STEP 9: Create heatmap with cluster gaps
#-----------------------------------------------

# Reorder genes by cluster assignment
gene_order <- order(gene_clusters)
deg_scaled_ordered <- deg_scaled[gene_order, ]
cluster_ordered <- gene_clusters[gene_order]

# Create cluster annotation for genes
cluster_annotation <- data.frame(
  Cluster = factor(cluster_ordered),
  row.names = rownames(deg_scaled_ordered)
)

# Calculate gap positions (where clusters change)
gap_positions <- which(diff(cluster_ordered) != 0)

# Create annotated heatmap with gaps between clusters
pheatmap(
  deg_scaled_ordered,
  cluster_rows = FALSE,  # Already ordered by cluster
  cluster_cols = deg_sample_hclust,
  annotation_row = cluster_annotation,
  annotation_col = annotation_col,
  gaps_row = gap_positions,  # Add visual gaps between clusters
  show_rownames = FALSE,
  show_colnames = TRUE,
  color = colorRampPalette(c("blue", "white", "red"))(100),
  main = "DEG Hierarchical Clusters (Separated by Gaps)",
  fontsize = 10,
  filename = "hierarchical_clustering_DEGs_gapped.png",
  width = 10,
  height = 8
)
```

> 

**Biological Insight:** The gaps make cluster boundaries immediately visible. You can clearly see which genes are coordinately upregulated versus downregulated, and how samples group by treatment. Genes within each cluster often share biological functions or regulatory mechanisms.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/hierarchical_clustering_DEGs_gapped.png?resize=1024%2C819&ssl=1)

## K-means Clustering Analysis of Differentially Expressed Genes

While hierarchical clustering reveals nested relationships, K-means provides discrete partitioning of genes into distinct groups. This approach excels when you need clear cluster assignments for downstream analysis.

### Determining Optimal K: The Elbow Method

A critical challenge in K-means is selecting the optimal number of clusters (K). The elbow method helps us make this data-driven decision:

```r
#-----------------------------------------------
# STEP 10: Determine optimal K using elbow method
#-----------------------------------------------

# Calculate within-cluster sum of squares (WSS) for different K values
# WSS measures cluster compactness - lower values indicate tighter clusters
wss <- sapply(1:15, function(k) {
  kmeans(deg_scaled, centers = k, nstart = 25)$tot.withinss
})

# Create data frame for plotting
elbow_data <- data.frame(
  K = 1:15,
  WSS = wss
)

# Plot the elbow curve
ggplot(elbow_data, aes(x = K, y = WSS)) +
  geom_line(color = "steelblue", linewidth = 1) +
  geom_point(color = "steelblue", size = 3) +
  labs(
    title = "Elbow Method for Optimal K",
    x = "Number of Clusters (K)",
    y = "Within-Cluster Sum of Squares"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, size = 14, face = "bold"),
    axis.title = element_text(size = 12),
    axis.text = element_text(size = 10)
  ) +
  scale_x_continuous(breaks = 1:15)

ggsave("elbow_plot.png", width = 8, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/elbow_plot.png?resize=1024%2C768&ssl=1)

> 

**Interpretation:** Look for the “elbow” – the point where the curve transitions from steep to shallow. Adding clusters beyond the elbow provides diminishing improvements in compactness. For example, if WSS drops sharply from K=1 to K=3 then levels off, K=3 is likely optimal.

### Silhouette Analysis for K Selection

The silhouette method provides an alternative, often more reliable approach to selecting optimal K:

```r
#-----------------------------------------------
# STEP 11: Silhouette analysis for K selection
#-----------------------------------------------

# Load cluster package for silhouette calculation
library(cluster)

# Function to calculate average silhouette width
calculate_silhouette <- function(data, max_k = 15) {
  sil_width <- sapply(2:max_k, function(k) {
    km <- kmeans(data, centers = k, nstart = 25)
    ss <- silhouette(km$cluster, dist(data))
    mean(ss[, 3])  # Average silhouette width
  })

  data.frame(
    K = 2:max_k,
    Silhouette = sil_width
  )
}

# Calculate silhouette scores for K from 2 to 15
sil_data <- calculate_silhouette(deg_scaled)

# Plot silhouette scores
ggplot(sil_data, aes(x = K, y = Silhouette)) +
  geom_line(color = "darkgreen", size = 1) +
  geom_point(color = "darkgreen", size = 3) +
  labs(
    title = "Silhouette Analysis for Optimal K",
    x = "Number of Clusters (K)",
    y = "Average Silhouette Width"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, size = 14, face = "bold"),
    axis.title = element_text(size = 12),
    axis.text = element_text(size = 10)
  )

ggsave("silhouette_plot.png", width = 8, height = 6, dpi = 300)

# Identify optimal K (K with maximum silhouette width)
optimal_k <- sil_data$K[which.max(sil_data$Silhouette)]
cat("Optimal K based on silhouette analysis:", optimal_k, "\n")
```

> 

**Silhouette Interpretation:**

- Values range from -1 to 1

- Higher values indicate better-defined clusters

- Values > 0.5 suggest strong cluster structure

- Values < 0.25 suggest weak or artificial clustering

- Select K with the maximum average silhouette width

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/silhouette_plot.png?resize=1024%2C768&ssl=1)

### Applying K-means Clustering

Now we apply K-means using the optimal K determined from silhouette analysis:

```r
#-----------------------------------------------
# STEP 12: K-means clustering of DEGs
#-----------------------------------------------

# Perform K-means clustering on DEGs
# nstart = 25 runs algorithm 25 times with different random starts
# and returns the best result (lowest within-cluster SS)
set.seed(123)  # For reproducibility
kmeans_deg <- kmeans(deg_scaled, centers = optimal_k, nstart = 25)

# Extract cluster assignments
deg_kmeans_clusters <- kmeans_deg$cluster

# Order genes by cluster for visualization
deg_cluster_order <- order(deg_kmeans_clusters)
deg_scaled_ordered_km <- deg_scaled[deg_cluster_order, ]
deg_clusters_ordered <- deg_kmeans_clusters[deg_cluster_order]

# Calculate gap positions between clusters
deg_gap_positions <- which(diff(deg_clusters_ordered) != 0)

# Create annotated heatmap with cluster separation
pheatmap(
  deg_scaled_ordered_km,
  cluster_rows = FALSE,  # Already ordered by K-means cluster
  cluster_cols = deg_sample_hclust,
  annotation_row = data.frame(
    Cluster = factor(deg_clusters_ordered),
    row.names = rownames(deg_scaled_ordered_km)
  ),
  annotation_col = annotation_col,
  gaps_row = deg_gap_positions,  # Visual separation between clusters
  show_rownames = FALSE,
  show_colnames = TRUE,
  color = colorRampPalette(c("blue", "white", "red"))(100),
  main = "K-means Clustering: Differentially Expressed Genes",
  fontsize = 10,
  filename = "kmeans_clustering_DEGs.png",
  width = 10,
  height = 8
)

# Save cluster assignments for downstream analysis
deg_cluster_assignments <- data.frame(
  Gene = names(deg_kmeans_clusters),
  Cluster = deg_kmeans_clusters
)
write.csv(deg_cluster_assignments, 
          "kmeans_DEG_cluster_assignments.csv", 
          row.names = FALSE)
```

> 

**Set Seed Importance:** K-means involves random initialization, so results can vary slightly between runs. Always use `set.seed()` before K-means to ensure reproducible results.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/kmeans_clustering_DEGs.png?resize=1024%2C819&ssl=1)

### Visualizing Cluster Centroids

Cluster centroids represent the “average” expression pattern for each cluster, providing a clear summary of each cluster’s characteristics:

```r
#-----------------------------------------------
# STEP 13: Visualize cluster centroids
#-----------------------------------------------

# Extract cluster centroids from K-means results
# Each row represents the mean scaled expression pattern for one cluster
cluster_centroids <- kmeans_deg$centers

# Create heatmap of cluster centroids
pheatmap(
  cluster_centroids,
  cluster_rows = FALSE,  # Keep cluster order from K-means
  cluster_cols = deg_sample_hclust,
  annotation_col = annotation_col,
  show_rownames = TRUE,   # Show cluster labels
  show_colnames = TRUE,
  color = colorRampPalette(c("blue", "white", "red"))(100),
  main = "K-means Cluster Centroids",
  fontsize = 10,
  filename = "kmeans_cluster_centroids.png",
  width = 10,
  height = 6
)
```

> 

**Interpreting Centroids:** Each row shows the typical expression pattern for genes in that cluster. For example, a centroid with positive values in EV_AKO samples and negative values in EV samples represents genes upregulated by the EV_AKO treatment.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/kmeans_cluster_centroids.png?resize=1024%2C614&ssl=1)

## Comparing Hierarchical and K-means Results

Understanding how these two methods produce different results helps you assess the robustness of your findings and choose the right approach for future analyses:

```r
#-----------------------------------------------
# STEP 14: Compare clustering methods
#-----------------------------------------------

# Create comparison dataframe
comparison_df <- data.frame(
  Gene = rownames(deg_scaled),
  Hierarchical = gene_clusters,
  Kmeans = deg_kmeans_clusters
)

# Calculate Adjusted Rand Index (ARI)
# ARI measures agreement between two clustering results
# Values range from -1 to 1 (1 = perfect agreement, 0 = random)
ari <- adjustedRandIndex(gene_clusters, deg_kmeans_clusters)
cat("Adjusted Rand Index between methods:", round(ari, 3), "\n")
# Adjusted Rand Index between methods: 0.904 

# Create confusion matrix
# Shows how genes are distributed across clusters in each method
confusion <- table(
  Hierarchical = gene_clusters,
  Kmeans = deg_kmeans_clusters
)
print(confusion)

# Visualize the comparison
confusion_melt <- melt(confusion)
confusion_melt$Hierarchical <- as.factor(confusion_melt$Hierarchical)
confusion_melt$Kmeans <- as.factor(confusion_melt$Kmeans)

ggplot(confusion_melt, aes(x = Kmeans, y = Hierarchical, fill = value)) +
  geom_tile() +
  geom_text(aes(label = value), color = "white", size = 4) +
  scale_fill_gradient(low = "lightblue", high = "darkblue") +
  labs(
    title = "Comparison of Hierarchical vs K-means Clustering",
    x = "K-means Cluster",
    y = "Hierarchical Cluster",
    fill = "Gene Count"
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, size = 14, face = "bold"),
    axis.title = element_text(size = 12)
  )

ggsave("clustering_method_comparison.png", width = 8, height = 6, dpi = 300)
```

> 

**Interpretation Guidelines:**

- **High ARI (>0.7):** Strong agreement between methods, indicating robust clusters

- **Moderate ARI (0.3-0.7):** Partial agreement, methods capture different aspects of structure

- **Low ARI (<0.3):** Little agreement, clusters may not be well-defined

The confusion matrix diagonal shows genes assigned to corresponding clusters by both methods. Large off-diagonal values indicate disagreement.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/clustering_method_comparison.png?resize=1024%2C768&ssl=1)

## Best Practices for Clustering Analysis

To ensure robust and reproducible clustering results, follow these guidelines:

### Data Quality and Preparation

**Always Filter Low-Count Genes:** Remove genes with very low expression across samples. We used a stringent filter requiring >5 counts in at least 50% of samples. This removes noise while retaining biologically relevant genes.

**Choose Appropriate Normalization:** Use variance-stabilizing transformation (VST) or regularized log transformation (rlog) from DESeq2 for clustering. These methods handle the mean-variance relationship in RNA-seq data better than simple log-transformation.

**Scale Your Data:** Always apply row-wise (gene-wise) scaling before clustering. This ensures all genes contribute equally regardless of their absolute expression levels. Use `t(scale(t(data)))` in R.

**Focus on Informative Genes:** Clustering on differentially expressed genes (DEGs) captures biologically meaningful variation while reducing computational requirements and noise from non-varying genes.

### Parameter Selection Guidelines

**For Hierarchical Clustering:**

- **Distance Metric:** Use correlation-based distance (1 – correlation) for genes to emphasize pattern similarity. Use Euclidean distance for samples.

- **Linkage Method:** Complete linkage produces compact clusters with clear boundaries, ideal for biological interpretation.

- **Cutting Height:** Choose `k` in `cutree()` based on dendrogram structure and biological knowledge. Look for stable branches.

**For K-means Clustering:**

- **Optimal K Selection:** Use both elbow method and silhouette analysis. Prioritize silhouette if they disagree.

- **Initialization:** Always use `nstart >= 25` to run multiple initializations and select the best result.

- **Set Random Seed:** Use `set.seed()` before K-means for reproducibility, as the algorithm involves random initialization.

### Validation and Quality Control

**Check Cluster Stability:** Compare hierarchical and K-means results using Adjusted Rand Index. High agreement (ARI > 0.7) indicates robust clusters.

**Validate Biological Relevance:** Perform pathway enrichment analysis on genes within clusters using tools like clusterProfiler, DAVID, or Enrichr. Biologically meaningful clusters show enrichment for specific biological processes.

**Examine Cluster Sizes:** Very small clusters (<5 genes) may be outliers. Very large clusters may indicate insufficient resolution.

**Verify Sample Grouping:** In the sample dendrogram, biological replicates should cluster together. If they don’t, investigate batch effects or sample quality issues.

### Visualization Best Practices

**Use Appropriate Color Schemes:** Diverging color palettes (blue-white-red) work well for scaled expression data centered at zero. This makes upregulation (red) and downregulation (blue) immediately apparent.

**Include Annotations:** Always annotate heatmaps with sample groups and cluster assignments. This contextualizes patterns and aids interpretation.

**Add Cluster Gaps:** Use the `gaps_row` parameter in pheatmap to visually separate clusters. This makes cluster boundaries clear and improves readability.

**Control Row Display:** For large gene sets, use `show_rownames = FALSE` to prevent overcrowding. Gene names can be retrieved from saved cluster assignment files.

### Common Pitfalls to Avoid

**Forgetting to Scale Data:** This is the most common error. Without scaling, high-expression genes dominate clustering, potentially masking important patterns in lower-expression genes.

**Using Raw Counts:** Never cluster raw RNA-seq counts. Always normalize first with appropriate methods (DESeq2 VST or rlog).

**Inappropriate Distance Metrics:** Using Euclidean distance on genes emphasizes magnitude rather than pattern. For genes, correlation-based distances usually make more biological sense.

**Over-interpreting Small Differences:** Not all clusters represent distinct biological states. Some separation may reflect subtle quantitative differences rather than qualitative biological distinctions.

**Ignoring Batch Effects:** If your data spans multiple sequencing batches, correct for batch effects before clustering using tools like ComBat or include batch in the DESeq2 design formula.

**Not Setting Seeds:** Forgetting `set.seed()` before K-means makes results non-reproducible, complicating manuscript preparation and result sharing.

### Downstream Analysis Recommendations

After identifying clusters, extend your analysis:

**Pathway Enrichment Analysis:** Use tools like clusterProfiler, DAVID, or Enrichr to identify biological processes enriched in each cluster. This reveals coordinated biological functions.

**Transcription Factor Analysis:** Analyze promoter regions for shared transcription factor binding motifs using tools like HOMER or MEME to understand co-regulation mechanisms.

**Integration with Other Data:** Combine clustering results with:

- ChIP-seq data to link clusters to regulatory mechanisms

- ATAC-seq data to understand chromatin accessibility

- Clinical data to identify prognostic gene signatures

- Protein-protein interaction networks to reveal functional modules

**Validation Experiments:** Design follow-up experiments to validate key findings, such as qPCR validation of representative genes from each cluster.

## Conclusion: From Patterns to Biology

Through this tutorial, you’ve gained practical skills in two powerful clustering approaches for RNA-seq analysis. You now understand when to apply hierarchical clustering to explore data structure and reveal relationships, versus when to use K-means for efficient, discrete partitioning. More importantly, you’ve learned that clustering is not merely a mathematical exercise – it’s a bridge between complex data and biological understanding.

The clusters you identify represent coordinated biological programs: genes working together to execute cellular functions, respond to stimuli, or define disease states. Each cluster tells a story about biological regulation, whether it’s a set of metabolic genes responding to nutrient availability, cell cycle genes orchestrating division, or immune genes coordinating inflammatory responses.

## References

1. Hastie, T., Tibshirani, R., & Friedman, J. (2009). The Elements of Statistical Learning: Data Mining, Inference, and Prediction. Springer Science & Business Media.

2. Love, M. I., Huber, W., & Anders, S. (2014). Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biology, 15(12), 550.

3. Eisen, M. B., Spellman, P. T., Brown, P. O., & Botstein, D. (1998). Cluster analysis and display of genome-wide expression patterns. Proceedings of the National Academy of Sciences, 95(25), 14863-14868.

4. D’haeseleer, P. (2005). How does gene expression clustering work?. Nature Biotechnology, 23(12), 1499-1501.

5. Langfelder, P., & Horvath, S. (2008). WGCNA: an R package for weighted correlation network analysis. BMC Bioinformatics, 9(1), 559.

6. Rousseeuw, P. J. (1987). Silhouettes: a graphical aid to the interpretation and validation of cluster analysis. Journal of Computational and Applied Mathematics, 20, 53-65.

7. Hartigan, J. A., & Wong, M. A. (1979). Algorithm AS 136: A k-means clustering algorithm. Journal of the Royal Statistical Society. Series C (Applied Statistics), 28(1), 100-108.

8. Kaufman, L., & Rousseeuw, P. J. (2009). Finding Groups in Data: An Introduction to Cluster Analysis. John Wiley & Sons.

9. Conesa, A., Madrigal, P., Tarazona, S., et al. (2016). A survey of best practices for RNA-seq data analysis. Genome Biology, 17(1), 13.

10. Hubert, L., & Arabie, P. (1985). Comparing partitions. Journal of Classification, 2(1), 193-218.

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, **please comment and share** **your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.*
