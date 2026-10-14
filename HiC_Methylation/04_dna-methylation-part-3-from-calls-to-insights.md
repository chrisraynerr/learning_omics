# How To Analyze DNA Methylation Data For Absolute Beginners Part 3: From WGBS and RRBS Methylation Calls to Biological Insights

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/)

- [Introduction: From Methylation Calls to Biological Understanding](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Introduction_From_Methylation_Calls_to_Biological_Understanding)

  - [What You’ll Learn in This Tutorial](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#What_Youll_Learn_in_This_Tutorial)
  - [Prerequisites and Data Requirements](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Prerequisites_and_Data_Requirements)

- [Setting Up the R Environment for Methylation Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Setting_Up_the_R_Environment_for_Methylation_Analysis)

  - [Installing Essential R Packages](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Installing_Essential_R_Packages)

- [Loading and Organizing Methylation Data](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Loading_and_Organizing_Methylation_Data)

  - [Defining Analysis Parameters](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Defining_Analysis_Parameters)
  - [Loading Bismark Coverage Files](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Loading_Bismark_Coverage_Files)
  - [Creating the BSseq Object](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Creating_the_BSseq_Object)

- [Quality Control and Exploratory Data Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Quality_Control_and_Exploratory_Data_Analysis)

  - [Global Methylation Assessment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Global_Methylation_Assessment)
  - [Sample Correlation and Principal Component Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Sample_Correlation_and_Principal_Component_Analysis)

- [Differential Methylation Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Differential_Methylation_Analysis)

  - [Statistical Testing with DSS](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Statistical_Testing_with_DSS)
  - [Understanding Your Results Files](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Understanding_Your_Results_Files)

- [Advanced Data Visualization](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Advanced_Data_Visualization)

  - [Volcano Plot for DMPs](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Volcano_Plot_for_DMPs)
  - [Genome-Wide Methylation Visualization](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Genome-Wide_Methylation_Visualization)
  - [Manhattan Plot for DMPs](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Manhattan_Plot_for_DMPs)

- [Genomic Annotation and Functional Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Genomic_Annotation_and_Functional_Analysis)

  - [Annotating Differentially Methylated Sites/Regions](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Annotating_Differentially_Methylated_SitesRegions)
  - [Functional Enrichment Analysis (GO & KEGG)](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Functional_Enrichment_Analysis_GO_KEGG)

- [Advanced Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Advanced_Analysis)

  - [Methylation Profile Heatmaps](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Methylation_Profile_Heatmaps)

- [Best Practices and Interpretation Guidelines](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Best_Practices_and_Interpretation_Guidelines)

  - [Biological Interpretation Guidelines](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Biological_Interpretation_Guidelines)

- [Next Steps and Advanced Applications](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Next_Steps_and_Advanced_Applications)

  - [Validation Strategies](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Validation_Strategies)

- [References](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-3-from-wgbs-and-rrbs-methylation-calls-to-biological-insights/#Like_this)

## Introduction: From Methylation Calls to Biological Understanding

In Part 2 of this series, we successfully preprocessed DNA methylation data from raw FASTQ files through to methylation calls using WGBS and RRBS approaches. Now comes the exciting part: transforming those methylation measurements into meaningful biological insights. This tutorial will guide you through the statistical analysis, visualization, and functional interpretation of your methylation data using R and specialized Bioconductor packages.

Understanding DNA methylation patterns is only the beginning—the real scientific value comes from identifying significant changes between conditions, connecting those changes to biological processes, and interpreting their functional significance. Whether you’re studying disease progression, developmental changes, or environmental responses, the analytical approaches covered in this tutorial will help you extract maximum biological insight from your methylation data.

### What You’ll Learn in This Tutorial

By the end of this tutorial, you will be able to:

**1. Statistical Analysis Mastery**

- Load and organize methylation data from multiple samples

- Perform differential methylation analysis using state-of-the-art methods

- Identify both differentially methylated positions (DMPs) and regions (DMRs)

- Apply appropriate statistical corrections for multiple testing

**2. Advanced Visualization Techniques**

- Create genome-wide methylation profiles and Manhattan plots

- Generate sample correlation heatmaps and principal component analyses

- Produce volcano plots and region-specific methylation tracks

- Design publication-ready figures with professional aesthetics

**3. Functional Interpretation Skills**

- Annotate methylation changes with genomic features

- Perform pathway enrichment analysis on affected genes

- Connect methylation patterns to biological processes

**4. Best Practices Implementation**

- Apply quality control measures throughout the analysis

- Handle common challenges in methylation data interpretation

- Avoid statistical pitfalls and methodological errors

- Prepare results for publication and further validation

### Prerequisites and Data Requirements

This tutorial assumes you have completed Part 2 and have the following files available for each sample:

**Required Input Files:**

- **Coverage files (.cov):** Methylation levels and read counts for each CpG site

- **Sample metadata:** Information about experimental conditions and sample groupings

**Expected File Structure:**

```
~/methylation_analysis/GSE46644/
├── alignment/
│   ├── Normal_1/Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz
│   ├── Normal_2/Normal_2_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz
│   ├── Disease_1/Disease_1_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz
│   └── Disease_2/Disease_2_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz
└── results/  (will be created during this analysis)
```

## Setting Up the R Environment for Methylation Analysis

Before diving into the analysis, we need to establish a comprehensive R environment with all the specialized packages required for methylation data analysis.

### Installing Essential R Packages

```r
# Install essential Bioconductor packages
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install(version = "3.21")

# Core packages for WGBS/RRBS analysis
essential_packages <- c(
    "bsseq",              # Core package for bisulfite sequencing analysis
    "DSS",                # Differential methylation analysis
    "dmrseq",             # DMR detection for WGBS data
    "GenomicRanges",      # Genomic interval operations
    "data.table",         # Fast data manipulation
    "ggplot2",            # Plotting
    "pheatmap",           # Heatmaps
    "RColorBrewer",       # Color palettes
    "dplyr",              # Data manipulation
    "ChIPseeker",         # Genomic annotation
    "TxDb.Hsapiens.UCSC.hg38.knownGene",  # Gene annotations
    "org.Hs.eg.db",       # Gene symbol mappings
    "clusterProfiler",    # Pathway enrichment analysis
    "annotatr",           # Genomic region annotation
    "tidyr",              # Data reshaping
    "scales",             # Scale functions for ggplot2
    "gridExtra"           # Multiple plots arrangement
)

BiocManager::install(essential_packages, update = TRUE, ask = FALSE)

# Load required libraries
suppressPackageStartupMessages({
    library(bsseq)
    library(DSS)
    library(dmrseq)
    library(GenomicRanges)
    library(data.table)
    library(ggplot2)
    library(pheatmap)
    library(RColorBrewer)
    library(dplyr)
    library(ChIPseeker)
    library(TxDb.Hsapiens.UCSC.hg38.knownGene)
    library(org.Hs.eg.db)
    library(clusterProfiler)
    library(annotatr)
    library(tidyr)
    library(scales)
    library(gridExtra)
})

# Set global options for better output
options(stringsAsFactors = FALSE)
options(scipen = 999)

# Configure ggplot2 theme for publication-ready plots
theme_set(theme_minimal() + 
          theme(text = element_text(size = 12),
                plot.title = element_text(size = 14, face = "bold"),
                axis.title = element_text(size = 12),
                legend.text = element_text(size = 10)))
```

## Loading and Organizing Methylation Data

### Defining Analysis Parameters

```r
# Set working directory and create results folder
setwd("~/methylation_analysis/GSE46644")
dir.create("results", showWarnings = FALSE, recursive = TRUE)

# Define analysis parameters
analysis_params <- list(
    min_coverage = 5,        # Minimum read coverage per CpG site
    min_samples = 2,         # Minimum samples with adequate coverage
    min_diff = 0.1,          # Minimum methylation difference (10%)
    fdr_threshold = 0.05,    # False discovery rate threshold
    min_dmr_cpgs = 3,        # Minimum CpGs required for DMR calling
    min_dmr_length = 50      # Minimum DMR length in base pairs
)

# Define sample information
sample_info <- data.frame(
    sample_id = c("Normal_1", "Normal_2", "Disease_1", "Disease_2"),
    condition = c("Normal", "Normal", "Disease", "Disease"),
    file_path = c(
        "alignment/Normal_1/Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz",
        "alignment/Normal_2/Normal_2_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz",
        "alignment/Disease_1/Disease_1_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz",
        "alignment/Disease_2/Disease_2_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz"
    ),
    stringsAsFactors = FALSE
)
```

### Loading Bismark Coverage Files

```r
# Function to read Bismark coverage files efficiently
read_bismark_coverage <- function(file_path, sample_name, min_cov = 5) {
    # Read the coverage file with proper column names
    cov_data <- fread(file_path, 
                      col.names = c("chr", "start", "end", "methylation_pct", 
                                   "meth_count", "unmeth_count"))

    # Calculate total coverage and apply minimum coverage filter
    cov_data$total_count <- cov_data$meth_count + cov_data$unmeth_count
    cov_data <- cov_data[total_count >= min_cov]
    cov_data$sample <- sample_name

    return(cov_data)
}

# Load all samples
sample_data_list <- list()
for (i in 1:nrow(sample_info)) {
    sample_data_list[[sample_info$sample_id[i]]] <- 
        read_bismark_coverage(sample_info$file_path[i], 
                             sample_info$sample_id[i], 
                             analysis_params$min_coverage)
}
```

### Creating the BSseq Object

```r
# Find CpG sites common to all samples
get_site_id <- function(data) paste(data$chr, data$start, sep = ":")

site_lists <- lapply(sample_data_list, get_site_id)
common_sites <- Reduce(intersect, site_lists)

# Filter to common sites
filter_to_common <- function(data, common_sites) {
    site_id <- get_site_id(data)
    return(data[site_id %in% common_sites])
}

sample_data_filtered <- lapply(sample_data_list, filter_to_common, common_sites)

# Filter to keep only standard chromosomes
standard_chromosomes <- paste0("chr", c(1:22, "X", "Y"))

filter_standard_chr <- function(data, standard_chr) {
    return(data[data$chr %in% standard_chr])
}

sample_data_filtered <- lapply(sample_data_filtered, filter_standard_chr, standard_chromosomes)

# Create BSseq object
ref_data <- sample_data_filtered[[1]]
gr <- GRanges(seqnames = ref_data$chr,
              ranges = IRanges(start = ref_data$start, width = 1))

sample_names <- sample_info$sample_id
M_matrix <- matrix(nrow = nrow(ref_data), ncol = length(sample_names))
Cov_matrix <- matrix(nrow = nrow(ref_data), ncol = length(sample_names))

for (i in seq_along(sample_names)) {
    sample_data <- sample_data_filtered[[sample_names[i]]]
    M_matrix[, i] <- sample_data$meth_count
    Cov_matrix[, i] <- sample_data$total_count
}

colnames(M_matrix) <- colnames(Cov_matrix) <- sample_names

bs_obj <- BSseq(gr = gr, M = M_matrix, Cov = Cov_matrix)
pData(bs_obj) <- sample_info[match(sample_names, sample_info$sample_id), ]
rownames(pData(bs_obj)) <- sample_names

saveRDS(bs_obj, "results/bsseq_object.rds")
```

## Quality Control and Exploratory Data Analysis

### Global Methylation Assessment

```r
# Function to calculate methylation statistics
calculate_methylation_stats <- function(bs_obj) {
    beta_matrix <- getMeth(bs_obj, type = "raw")  # Returns beta values directly
    stats_df <- data.frame()

    for (i in 1:ncol(bs_obj)) {
        beta_vals <- beta_matrix[, i]
        beta_vals <- beta_vals[!is.nan(beta_vals)]

        sample_stats <- data.frame(
            sample = colnames(bs_obj)[i],
            condition = pData(bs_obj)$condition[i],
            total_sites = length(beta_vals),
            mean_methylation = mean(beta_vals, na.rm = TRUE),
            median_methylation = median(beta_vals, na.rm = TRUE),
            sd_methylation = sd(beta_vals, na.rm = TRUE),
            sites_high_meth = sum(beta_vals > 0.8, na.rm = TRUE),
            sites_low_meth = sum(beta_vals < 0.2, na.rm = TRUE),
            sites_intermediate = sum(beta_vals >= 0.2 & beta_vals <= 0.8, na.rm = TRUE)
        )

        stats_df <- rbind(stats_df, sample_stats)
    }
    return(stats_df)
}

methylation_stats <- calculate_methylation_stats(bs_obj)
write.table(methylation_stats, "results/global_methylation_stats.txt", 
           sep = "\t", quote = FALSE, row.names = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/global_methylation_stats.png?resize=1024%2C78&ssl=1)

### Sample Correlation and Principal Component Analysis

```r
# Calculate sample correlations
beta_matrix <- getMeth(bs_obj, type = "raw")  # Returns beta values directly
complete_sites <- complete.cases(beta_matrix)
beta_complete <- beta_matrix[complete_sites, ]

sample_cors <- cor(beta_complete)

# Create correlation heatmap
annotation_df <- data.frame(
    Condition = pData(bs_obj)$condition,
    row.names = colnames(sample_cors)
)

pdf("results/sample_correlation_heatmap.pdf", width = 8, height = 6)
pheatmap(sample_cors,
         annotation_col = annotation_df,
         annotation_row = annotation_df,
         color = colorRampPalette(c("#2166AC", "white", "#B2182B"))(100),
         main = "Sample-to-Sample Methylation Correlations",
         fontsize = 12,
         display_numbers = TRUE,
         number_format = "%.3f")
dev.off()

# Perform PCA
pca_result <- prcomp(t(beta_complete), center = TRUE, scale. = FALSE)
var_explained <- summary(pca_result)$importance[2, 1:4] * 100

pca_data <- data.frame(
    PC1 = pca_result$x[, 1],
    PC2 = pca_result$x[, 2],
    Sample = rownames(pca_result$x),
    Condition = pData(bs_obj)$condition
)

# Create PCA plot
p1 <- ggplot(pca_data, aes(x = PC1, y = PC2, color = Condition, label = Sample)) +
    geom_point(size = 4) +
    geom_text(vjust = -1, hjust = 0.5, size = 3) +
    scale_color_manual(values = c("Normal" = "#2E86C1", "Disease" = "#E74C3C")) +
    labs(
        title = "PCA of DNA Methylation Profiles",
        x = sprintf("PC1 (%.1f%%)", var_explained[1]),
        y = sprintf("PC2 (%.1f%%)", var_explained[2])
    ) +
    theme_minimal()

ggsave("results/pca_analysis.pdf", p1, width = 10, height = 8)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/WGBS-pca-correlation-e1754971641726-1024x408.png?resize=1024%2C408&ssl=1)

## Differential Methylation Analysis

### Statistical Testing with DSS

```r
# Perform differential methylation analysis
group1_samples <- c("Normal_1", "Normal_2")
group2_samples <- c("Disease_1", "Disease_2")

dml_test <- DMLtest(bs_obj, 
                    group1 = group1_samples, 
                    group2 = group2_samples,
                    smoothing = TRUE)

dmp_results <- callDML(dml_test, 
                      p.threshold = analysis_params$fdr_threshold,
                      delta = analysis_params$min_diff)

dmr_results <- callDMR(dml_test, 
                      p.threshold = analysis_params$fdr_threshold,
                      delta = analysis_params$min_diff,
                      minlen = analysis_params$min_dmr_length,
                      minCG = analysis_params$min_dmr_cpgs)

# Alternative DMR calling with dmrseq
# dmr_results <- dmrseq(bs_obj, testCovariate = "condition")

# Save results
if (nrow(dmp_results) > 0) {
    write.table(dmp_results, "results/differentially_methylated_positions.txt", 
               sep = "\t", quote = FALSE, row.names = FALSE)
}

if (nrow(dmr_results) > 0) {
    write.table(dmr_results, "results/differentially_methylated_regions.txt",
               sep = "\t", quote = FALSE, row.names = FALSE)
}

# Create summary statistics
summary_stats <- data.frame(
    Metric = c("Total CpG sites analyzed", "Significant DMPs (FDR < 0.05)", 
               "Significant DMRs", "Hypermethylated sites (Disease > Normal)", 
               "Hypomethylated sites (Disease < Normal)", "Mean DMR length (bp)",
               "Mean CpGs per DMR"),
    Value = c(
        format(nrow(dml_test), big.mark = ","),
        format(nrow(dmp_results), big.mark = ","),
        format(nrow(dmr_results), big.mark = ","),
        format(ifelse(nrow(dmp_results) > 0, 
                     sum(dmp_results$fdr < analysis_params$fdr_threshold & 
                         dmp_results$diff > analysis_params$min_diff, na.rm = TRUE), 0), big.mark = ","),
        format(ifelse(nrow(dmp_results) > 0,
                     sum(dmp_results$fdr < analysis_params$fdr_threshold & 
                         dmp_results$diff < -analysis_params$min_diff, na.rm = TRUE), 0), big.mark = ","),
        format(ifelse(nrow(dmr_results) > 0, round(mean(dmr_results$length)), "N/A")),
        format(ifelse(nrow(dmr_results) > 0, round(mean(dmr_results$nCG)), "N/A"))
    )
)

write.table(summary_stats, "results/analysis_summary.txt", sep = "\t", 
           quote = FALSE, row.names = FALSE)

# analysis_summary.txt:
# Total CpG sites analyzed        14,795,689
# Significant DMPs (FDR < 0.05)   64,872
# Significant DMRs        2,788
# Hypermethylated sites (Disease > Normal)        16,030
# Hypomethylated sites (Disease < Normal) 11,067
# Mean DMR length (bp)    226
# Mean CpGs per DMR       8
```

### Understanding Your Results Files

After running the differential methylation analysis, you’ll have two important results files. Let’s understand what each column means:

**Differentially Methylated Positions (DMPs)**

The `differentially_methylated_positions.txt` file contains individual CpG sites with significant methylation differences. Each row represents one CpG site with these key measurements:

- **chr, pos**: The genomic location (chromosome and position) of the CpG site

- **mu1, mu2**: Average methylation levels in your two groups (Normal and Disease). Values range from 0 (0% methylated) to 1 (100% methylated)

- **diff**: The methylation difference between groups (mu2 – mu1). Positive values mean higher methylation in Disease samples, negative values mean lower methylation in Disease

- **pval, fdr**: Statistical significance measures. The `fdr` (false discovery rate) is more reliable as it corrects for testing thousands of sites simultaneously

- **stat**: The test statistic used to calculate the p-value

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/dmp-table-1.png?resize=1024%2C110&ssl=1)

**Differentially Methylated Regions (DMRs)**

The `differentially_methylated_regions.txt` file contains genomic regions where multiple nearby CpG sites show coordinated changes. Each row represents one region:

- **chr, start, end**: The genomic coordinates defining the region boundaries

- **length**: Size of the region in base pairs

- **nCG**: Number of CpG sites within this region that contributed to the finding

- **meanMethy1, meanMethy2**: Average methylation across all CpG sites in the region for each group

- **diff.Methy**: The difference in average methylation between groups

- **areaStat**: A measure of the statistical evidence across the entire region—higher values indicate stronger evidence

DMRs are often more biologically meaningful than individual DMPs because they represent coordinated changes across regulatory elements like gene promoters or enhancers. A DMR spanning 500 base pairs with 15 CpG sites provides much stronger evidence than a single isolated CpG site.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/dmr-table-1.png?resize=1024%2C158&ssl=1)

## Advanced Data Visualization

### Volcano Plot for DMPs

```r
if (nrow(dmp_results) > 0) {
    volcano_data <- data.frame(
        diff = dmp_results$diff,
        pvalue = dmp_results$pval,
        fdr = dmp_results$fdr,
        significant = dmp_results$fdr < analysis_params$fdr_threshold & 
                     abs(dmp_results$diff) > analysis_params$min_diff
    )

    volcano_data$category <- "Not Significant"
    volcano_data$category[volcano_data$significant & volcano_data$diff > 0] <- "Hypermethylated"
    volcano_data$category[volcano_data$significant & volcano_data$diff < 0] <- "Hypomethylated"
    volcano_data$category <- factor(volcano_data$category, 
                                   levels = c("Not Significant", "Hypermethylated", "Hypomethylated"))

    n_hyper <- sum(volcano_data$category == "Hypermethylated")
    n_hypo <- sum(volcano_data$category == "Hypomethylated")

    p_volcano <- ggplot(volcano_data, aes(x = diff, y = -log10(pvalue), color = category)) +
        geom_point(alpha = 0.6, size = 0.8) +
        scale_color_manual(
            values = c("Not Significant" = "gray70", 
                      "Hypermethylated" = "#E74C3C", 
                      "Hypomethylated" = "#3498DB"),
            name = "Methylation Change"
        ) +
        labs(
            title = "Volcano Plot - Differentially Methylated Positions",
            subtitle = sprintf("Hypermethylated: %s | Hypomethylated: %s", 
                              format(n_hyper, big.mark = ","), 
                              format(n_hypo, big.mark = ",")),
            x = "Methylation Difference (Disease - Normal)",
            y = "-log10(p-value)"
        ) +
        geom_vline(xintercept = c(-analysis_params$min_diff, analysis_params$min_diff), 
                   linetype = "dashed", alpha = 0.7, color = "gray50") +
        geom_hline(yintercept = -log10(0.05), 
                   linetype = "dashed", alpha = 0.7, color = "gray50") +
        theme_minimal()

    ggsave("results/volcano_plot_dmps.pdf", p_volcano, width = 10, height = 8)
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/volcano-plot.png?resize=1024%2C774&ssl=1)

### Genome-Wide Methylation Visualization

```r
plot_genome_wide_methylation <- function(bs_obj, sample_name, window_size = 1000000) {
    sample_idx <- which(colnames(bs_obj) == sample_name)
    beta_vals <- getMeth(bs_obj, type = "raw")[, sample_idx]  # Returns beta values directly

    methylation_data <- data.frame(
        chr = as.character(seqnames(bs_obj)),
        pos = start(bs_obj),
        methylation = beta_vals * 100,
        stringsAsFactors = FALSE
    )

    methylation_data <- methylation_data[!is.na(methylation_data$methylation), ]
    chr_order <- paste0("chr", c(1:22, "X", "Y"))
    methylation_data <- methylation_data[methylation_data$chr %in% chr_order, ]
    methylation_data$chr <- factor(methylation_data$chr, levels = chr_order)

    methylation_binned <- methylation_data %>%
        mutate(
            bin = floor(pos / window_size) * window_size,
            chr_num = as.numeric(gsub("chr", "", gsub("X", "23", gsub("Y", "24", chr))))
        ) %>%
        group_by(chr, chr_num, bin) %>%
        summarise(
            mean_methylation = mean(methylation, na.rm = TRUE),
            n_cpgs = n(),
            .groups = "drop"
        ) %>%
        filter(n_cpgs >= 10)

    chr_lengths <- methylation_binned %>%
        group_by(chr, chr_num) %>%
        summarise(max_pos = max(bin), .groups = "drop") %>%
        arrange(chr_num) %>%
        mutate(
            cumulative_length = cumsum(c(0, head(max_pos, -1))),
            chr_center = cumulative_length + max_pos / 2
        )

    methylation_plot <- methylation_binned %>%
        left_join(chr_lengths, by = c("chr", "chr_num")) %>%
        mutate(genome_pos = cumulative_length + bin)

    ggplot(methylation_plot, aes(x = genome_pos, y = mean_methylation)) +
        geom_point(aes(color = chr), alpha = 0.6, size = 0.5) +
        scale_color_manual(values = rep(c("#2E86C1", "#E74C3C"), 12)) +
        scale_x_continuous(
            breaks = chr_lengths$chr_center,
            labels = gsub("chr", "", chr_lengths$chr),
            expand = c(0.01, 0)
        ) +
        scale_y_continuous(limits = c(0, 100), expand = c(0.02, 0)) +
        labs(
            title = paste("Whole-Genome DNA Methylation Profile:", sample_name),
            subtitle = paste("Window size:", window_size / 1000000, "Mb"),
            x = "Chromosome",
            y = "Mean Methylation (%)"
        ) +
        theme_minimal() +
        theme(
            legend.position = "none",
            panel.grid.minor = element_blank(),
            panel.grid.major.x = element_blank(),
            plot.title = element_text(size = 14, face = "bold")
        ) +
        geom_vline(
            xintercept = chr_lengths$cumulative_length[-1],
            color = "gray80", linetype = "dashed", alpha = 0.7
        )
}

pdf("results/genome_wide_methylation.pdf", width = 16, height = 10)
for (sample in colnames(bs_obj)) {
    p_genome <- plot_genome_wide_methylation(bs_obj, sample, window_size = 1000000)
    print(p_genome)
}
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/genome-wide-methylation.png?resize=1024%2C638&ssl=1)

### Manhattan Plot for DMPs

```r
# Manhattan plot
if (nrow(dmp_results) > 0) {
    manhattan_data <- dmp_results %>%
        mutate(
            chr_num = as.numeric(gsub("chr", "", gsub("X", "23", gsub("Y", "24", chr)))),
            significance = ifelse(fdr < analysis_params$fdr_threshold & 
                                 abs(diff) > analysis_params$min_diff, 
                                "Significant", "Not Significant")
        ) %>%
        filter(!is.na(chr_num)) %>%
        arrange(chr_num, pos)

    chr_info <- manhattan_data %>%
        group_by(chr, chr_num) %>%
        summarise(max_pos = max(pos), .groups = "drop") %>%
        arrange(chr_num) %>%
        mutate(
            cumulative_length = cumsum(c(0, head(max_pos, -1))),
            chr_center = cumulative_length + max_pos / 2
        )

    manhattan_plot_data <- manhattan_data %>%
        left_join(chr_info, by = c("chr", "chr_num")) %>%
        mutate(genome_pos = cumulative_length + pos)

    p_manhattan <- ggplot(manhattan_plot_data, aes(x = genome_pos, y = diff)) +
        geom_point(aes(color = significance), alpha = 0.6, size = 0.5) +
        scale_color_manual(
            values = c("Not Significant" = "gray70", "Significant" = "#E74C3C"),
            name = "Significance"
        ) +
        scale_x_continuous(
            breaks = chr_info$chr_center,
            labels = gsub("chr", "", chr_info$chr),
            expand = c(0.01, 0)
        ) +
        labs(
            title = "Manhattan Plot - Methylation Differences Across the Genome",
            subtitle = "Disease vs Normal Comparison",
            x = "Chromosome",
            y = "Methylation Difference (Disease - Normal)"
        ) +
        theme_minimal() +
        theme(
            legend.position = "bottom",
            panel.grid.minor = element_blank(),
            panel.grid.major.x = element_blank()
        ) +
        geom_hline(yintercept = c(-analysis_params$min_diff, analysis_params$min_diff), 
                   linetype = "dashed", alpha = 0.7, color = "gray50") +
        geom_vline(
            xintercept = chr_info$cumulative_length[-1],
            color = "gray80", linetype = "dashed", alpha = 0.3
        )

    ggsave("results/manhattan_plot_methylation_differences.pdf", p_manhattan, 
           width = 16, height = 8)
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/manhattan-plot.png?resize=1024%2C504&ssl=1)

## Genomic Annotation and Functional Analysis

### Annotating Differentially Methylated Sites/Regions

```r
txdb <- TxDb.Hsapiens.UCSC.hg38.knownGene

# Annotate DMPs
if (nrow(dmp_results) > 0) {
    dmp_gr <- GRanges(seqnames = dmp_results$chr,
                      ranges = IRanges(start = dmp_results$pos, width = 1),
                      strand = "*",
                      methylation_diff = dmp_results$diff,
                      pvalue = dmp_results$pval,
                      fdr = dmp_results$fdr)

    dmp_annotation <- annotatePeak(dmp_gr, 
                                  tssRegion = c(-3000, 3000),
                                  TxDb = txdb, 
                                  annoDb = "org.Hs.eg.db")

    dmp_anno_df <- as.data.frame(dmp_annotation)
    write.table(dmp_anno_df, "results/dmps_annotated.txt", 
               sep = "\t", quote = FALSE, row.names = FALSE)

    # Create DMP annotation plots
    pdf("results/dmp_annotation_plots.pdf", width = 12, height = 8)
    plotAnnoPie(dmp_annotation)
    plotDistToTSS(dmp_annotation, 
                  title = "Distribution of DMPs Relative to Transcription Start Sites")
    plotAnnoBar(dmp_annotation)
    dev.off()
}

# Annotate DMRs
if (nrow(dmr_results) > 0) {
    dmr_gr <- GRanges(
        seqnames = dmr_results$chr,
        ranges = IRanges(start = dmr_results$start, end = dmr_results$end)
    )

    dmr_annotation <- annotatePeak(dmr_gr, 
                                  tssRegion = c(-3000, 3000),
                                  TxDb = txdb, 
                                  annoDb = "org.Hs.eg.db")

    dmr_anno_df <- as.data.frame(dmr_annotation)

    # Add back the original DMR information
    dmr_anno_df$original_length <- dmr_results$length[match(paste0(dmr_anno_df$seqnames, ":", dmr_anno_df$start, "-", dmr_anno_df$end), 
                                                           paste0(dmr_results$chr, ":", dmr_results$start, "-", dmr_results$end))]
    dmr_anno_df$original_nCG <- dmr_results$nCG[match(paste0(dmr_anno_df$seqnames, ":", dmr_anno_df$start, "-", dmr_anno_df$end), 
                                                      paste0(dmr_results$chr, ":", dmr_results$start, "-", dmr_results$end))]

    write.table(dmr_anno_df, "results/dmrs_annotated.txt", 
               sep = "\t", quote = FALSE, row.names = FALSE)

    # Create DMR annotation plots
    pdf("results/dmr_annotation_plots.pdf", width = 12, height = 8)
    plotAnnoPie(dmr_annotation)
    plotDistToTSS(dmr_annotation, 
                  title = "Distribution of DMRs Relative to Transcription Start Sites")
    plotAnnoBar(dmr_annotation)
    dev.off()
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/dmr-annotation-pie.png?resize=1024%2C569&ssl=1)

### Functional Enrichment Analysis (GO & KEGG)

```r
# Function to perform enrichment analysis for both DMPs and DMRs
perform_enrichment_analysis <- function(annotation_obj, region_type) {
    if (is.null(annotation_obj)) return(NULL)

    anno_df <- as.data.frame(annotation_obj)
    genes <- unique(anno_df$geneId[!is.na(anno_df$geneId)])

    if (length(genes) < 10) {
        cat("Not enough genes for", region_type, "enrichment analysis (need at least 10, found", length(genes), ")\n")
        return(NULL)
    }

    cat("Performing", region_type, "enrichment analysis with", length(genes), "genes...\n")

    # Analyze all three GO categories
    go_categories <- c("BP", "MF", "CC")
    go_results <- list()
    all_go_results <- data.frame()

    for (category in go_categories) {
        cat("  - Analyzing GO", category, "category...\n")

        go_result <- enrichGO(
            gene = genes,
            OrgDb = org.Hs.eg.db,
            ont = category,
            pAdjustMethod = "BH",
            pvalueCutoff = 0.05,
            qvalueCutoff = 0.2,
            readable = TRUE
        )

        if (!is.null(go_result) && nrow(go_result@result) > 0) {
            go_results[[category]] <- go_result

            # Save individual category results
            write.table(go_result@result, 
                       paste0("results/", tolower(region_type), "_go_", category, "_enrichment.txt"), 
                       sep = "\t", quote = FALSE, row.names = FALSE)

            # Add to combined results
            category_data <- go_result@result
            category_data$Category <- category
            all_go_results <- rbind(all_go_results, category_data)
        }
    }

    # Plot top 20 GO terms from all categories combined
    if (nrow(all_go_results) > 0) {
        top_20_go <- all_go_results[order(all_go_results$p.adjust), ][1:min(20, nrow(all_go_results)), ]

        go_plot <- ggplot(top_20_go, aes(x = Count, y = reorder(Description, -p.adjust))) +
            geom_point(aes(color = p.adjust, size = Count)) +
            scale_color_gradient(low = "red", high = "blue", name = "Adjusted\np-value") +
            facet_wrap(~Category, scales = "free_y") +
            labs(
                title = paste("Top 20 GO Terms Enriched in", region_type, "Genes"),
                x = "Gene Count",
                y = "GO Terms"
            ) +
            theme_minimal() +
            theme(axis.text.y = element_text(size = 8))

        ggsave(paste0("results/", tolower(region_type), "_go_enrichment_top20.pdf"), 
               go_plot, width = 16, height = 10)

        # Save top 20 results
        write.table(top_20_go, paste0("results/", tolower(region_type), "_go_top20_all_categories.txt"), 
                   sep = "\t", quote = FALSE, row.names = FALSE)
    }

    # KEGG pathway analysis
    kegg_results <- enrichKEGG(
        gene = genes,
        organism = 'hsa',
        pvalueCutoff = 0.05,
        pAdjustMethod = "BH"
    )

    if (!is.null(kegg_results) && nrow(kegg_results@result) > 0) {
        write.table(kegg_results@result, paste0("results/", tolower(region_type), "_kegg_pathways.txt"), 
                   sep = "\t", quote = FALSE, row.names = FALSE)

        kegg_plot <- dotplot(kegg_results, showCategory = 20, 
                           title = paste(region_type, "KEGG Pathway Enrichment"))
        ggsave(paste0("results/", tolower(region_type), "_kegg_enrichment.pdf"), 
               kegg_plot, width = 14, height = 10)

        cat("  -", region_type, "KEGG analysis complete!\n")
    } else {
        cat("  - No significant KEGG pathways found for", region_type, "\n")
    }

    return(list(go_results = go_results, kegg_results = kegg_results))
}

# Perform enrichment analysis for DMPs
if (exists("dmp_annotation") && !is.null(dmp_annotation)) {
    dmp_enrichment <- perform_enrichment_analysis(dmp_annotation, "DMP")
}

# Perform enrichment analysis for DMRs
if (exists("dmr_annotation") && !is.null(dmr_annotation)) {
    dmr_enrichment <- perform_enrichment_analysis(dmr_annotation, "DMR")
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/dmr-go-enrich-plot.png?resize=1024%2C642&ssl=1)

## Advanced Analysis

### Methylation Profile Heatmaps

```r
# Function for generating the heatmaps for DMPs
create_dmp_level_heatmap <- function(bs_obj, dmp_results, n_top = 100) {
    if (nrow(dmp_results) == 0) {
        cat("No DMPs found - skipping DMP heatmap\n")
        return()
    }

    n_sites <- min(n_top, nrow(dmp_results))
    top_dmps <- dmp_results[order(abs(dmp_results$diff), decreasing = TRUE)[1:n_sites], ]

    # Create site IDs for matching
    dmp_ids <- paste(top_dmps$chr, top_dmps$pos, sep = ":")
    bs_site_ids <- paste(as.character(seqnames(bs_obj)), start(bs_obj), sep = ":")
    matching_indices <- which(bs_site_ids %in% dmp_ids)

    if (length(matching_indices) == 0) {
        cat("No matching DMP sites found in BSseq object\n")
        return()
    }

    # Extract methylation data
    beta_matrix <- getMeth(bs_obj[matching_indices, ], type = "raw") * 100

    # Create row names with genomic locations
    rownames(beta_matrix) <- paste0("DMP_", 1:nrow(beta_matrix), "_", 
                                   as.character(seqnames(bs_obj[matching_indices, ])), ":", 
                                   start(bs_obj[matching_indices, ]))

    # Quality checks
    cat("DMP heatmap diagnostic:\n")
    cat("  - Selected", n_sites, "top DMPs\n")
    cat("  - Found", length(matching_indices), "matching sites in BSseq object\n")
    cat("  - Matrix dimensions:", nrow(beta_matrix), "x", ncol(beta_matrix), "\n")

    # Remove rows with zero variance or missing data
    row_vars <- apply(beta_matrix, 1, var, na.rm = TRUE)
    valid_rows <- complete.cases(beta_matrix) & 
                  apply(beta_matrix, 1, function(x) all(is.finite(x))) &
                  row_vars > 0 & !is.na(row_vars)

    cat("  - Valid rows after filtering:", sum(valid_rows), "\n")

    beta_matrix <- beta_matrix[valid_rows, ]

    if (nrow(beta_matrix) < 2) {
        cat("Not enough valid DMPs for heatmap (need at least 2)\n")
        return()
    }

    # Create sample annotation
    col_annotation <- data.frame(
        Condition = pData(bs_obj)$condition,
        row.names = colnames(beta_matrix)
    )

    condition_colors <- c("Normal" = "#2E86C1", "Disease" = "#E74C3C")
    col_colors <- list(Condition = condition_colors)

    # Create heatmap
    output_file <- "results/dmp_level_heatmap.pdf"
    pdf(output_file, width = 8, height = 12)

    pheatmap(
        beta_matrix,
        scale = "row",
        clustering_distance_rows = "euclidean",
        clustering_distance_cols = "euclidean",
        clustering_method = "complete",
        annotation_col = col_annotation,
        annotation_colors = col_colors,
        color = colorRampPalette(c("#2166AC", "white", "#B2182B"))(100),
        main = sprintf("Top %d DMPs - Site-Level Methylation", nrow(beta_matrix)),
        fontsize = 8,
        show_rownames = FALSE,  # Too many sites to show names
        show_colnames = TRUE
    )

    dev.off()
    cat("DMP heatmap saved:", output_file, "\n")
}

# Function for generating the heatmaps for DMRs
create_dmr_level_heatmap <- function(bs_obj, dmr_results, n_top = 50) {
    if (nrow(dmr_results) == 0) {
        cat("No DMRs found - skipping DMR heatmap\n")
        return()
    }
    
    n_regions <- min(n_top, nrow(dmr_results))
    top_dmrs <- dmr_results[order(abs(dmr_results$diff.Methy), decreasing = TRUE)[1:n_regions], ]
    
    # Create matrix: rows = DMRs, columns = samples
    dmr_matrix <- matrix(nrow = n_regions, ncol = ncol(bs_obj))
    colnames(dmr_matrix) <- colnames(bs_obj)
    rownames(dmr_matrix) <- paste0("DMR_", 1:n_regions, "_", 
                                  top_dmrs$chr, ":", 
                                  top_dmrs$start, "-", 
                                  top_dmrs$end)
    
    # Create GRanges for BSseq object (once, outside loop)
    bs_ranges <- GRanges(
        seqnames = seqnames(bs_obj),
        ranges = IRanges(start = start(bs_obj), width = 1)
    )
    
    cat("DMR heatmap diagnostic:\n")
    cat("  - Processing", n_regions, "top DMRs\n")
    
    # For each DMR, calculate average methylation across all CpGs in that region
    valid_dmrs <- logical(n_regions)
    
    for (i in 1:n_regions) {
        dmr <- top_dmrs[i, ]
        
        # Create range for this DMR
        dmr_range <- GRanges(
            seqnames = dmr$chr, 
            ranges = IRanges(start = dmr$start, end = dmr$end)
        )
        
        # Find overlapping CpG sites
        overlaps <- findOverlaps(bs_ranges, dmr_range)
        cpg_indices <- queryHits(overlaps)
        
        if (length(cpg_indices) > 0) {
            # Calculate average methylation across all CpGs in this DMR
            dmr_beta <- getMeth(bs_obj[cpg_indices, ], type = "raw") * 100
            
            if (length(cpg_indices) == 1) {
                # Single CpG site
                dmr_matrix[i, ] <- as.numeric(dmr_beta)
            } else {
                # Multiple CpG sites - take mean
                dmr_matrix[i, ] <- colMeans(dmr_beta, na.rm = TRUE)
            }
            
            valid_dmrs[i] <- TRUE
            
            if (i <= 5) {  # Show details for first 5 DMRs
                cat("  - DMR", i, ":", dmr$chr, ":", dmr$start, "-", dmr$end, 
                    "contains", length(cpg_indices), "CpG sites\n")
            }
        } else {
            cat("  - Warning: DMR", i, "has no overlapping CpG sites\n")
            valid_dmrs[i] <- FALSE
        }
    }
    
    # Remove DMRs with missing data
    dmr_matrix <- dmr_matrix[valid_dmrs, ]
    
    cat("  - Valid DMRs after filtering:", nrow(dmr_matrix), "\n")
    cat("  - Matrix dimensions:", nrow(dmr_matrix), "x", ncol(dmr_matrix), "\n")
    
    if (nrow(dmr_matrix) < 2) {
        cat("Not enough valid DMRs for heatmap (need at least 2)\n")
        return()
    }
    
    # Create sample annotation
    col_annotation <- data.frame(
        Condition = pData(bs_obj)$condition,
        row.names = colnames(dmr_matrix)
    )
    
    condition_colors <- c("Normal" = "#2E86C1", "Disease" = "#E74C3C")
    col_colors <- list(Condition = condition_colors)
    
    # Create heatmap
    output_file <- "results/dmr_level_heatmap.pdf"
    pdf(output_file, width = 10, height = 12)
    
    pheatmap(
        dmr_matrix,
        scale = "row",
        clustering_distance_rows = "euclidean",
        clustering_distance_cols = "euclidean",
        clustering_method = "complete",
        annotation_col = col_annotation,
        annotation_colors = col_colors,
        color = colorRampPalette(c("#2166AC", "white", "#B2182B"))(100),
        main = sprintf("Top %d DMRs - Region-Level Methylation", nrow(dmr_matrix)),
        fontsize = 10,
        show_rownames = TRUE,   # Show DMR names since there are fewer
        show_colnames = TRUE
    )
    
    dev.off()
    cat("DMR heatmap saved:", output_file, "\n")
}

# Create both types of heatmaps
if (nrow(dmp_results) > 0) {
    create_dmp_level_heatmap(bs_obj, dmp_results, n_top = 100)
}

if (nrow(dmr_results) > 0) {
    create_dmr_level_heatmap(bs_obj, dmr_results, n_top = 50)
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/heatmap-dmp-dmr-e1754972290711-1024x641.png?resize=1024%2C641&ssl=1)

## Best Practices and Interpretation Guidelines

### Biological Interpretation Guidelines

**1. Regional vs. Single-Site Analysis**

- DMRs are generally more biologically meaningful than individual DMPs

- Consider the genomic context (CpG islands, shores, shelves, open sea)

- Integrate with chromatin state and histone modification data when available

**2. Functional Validation Priorities**

- Prioritize regions in gene promoters and regulatory elements

- Focus on genes with known disease associations

- Consider regions with large effect sizes and high confidence

## Next Steps and Advanced Applications

### Validation Strategies

**1. Targeted Validation**

- Bisulfite PCR followed by sequencing for specific regions

- Pyrosequencing for high-throughput validation

- Methylation-specific PCR (MSP) for clinical applications

**2. Functional Validation**

- Correlation with gene expression data (RNA-seq)

- Chromatin accessibility analysis (ATAC-seq)

- Histone modification mapping (ChIP-seq)

**3. Independent Cohort Validation**

- Replicate findings in independent sample sets

- Cross-platform validation (WGBS vs. EPIC arrays)

- Meta-analysis across multiple studies

## References

- Hansen et al. (2012). “BSmooth: from whole genome bisulfite sequencing reads to differentially methylated regions.” *Genome Biology*

- Feng et al. (2014). “A Bayesian hierarchical model to detect differentially methylated loci.” *Nucleic Acids Research*

- Yu et al. (2015). “ChIPseeker: an R/Bioconductor package for ChIP peak annotation.” *Bioinformatics*

---

*This tutorial is part of the NGS101.com comprehensive guide to next-generation sequencing analysis. Combined with Part 2 (preprocessing), you now have complete coverage of WGBS/RRBS analysis from raw reads to biological insights.*
