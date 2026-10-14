# How to Analyze RNAseq Data for Absolute Beginners Part 3: From Count Table to DEGs – Best Practices

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/)

---

As we move forward in our RNAseq analysis journey, we’ll be transitioning from the Linux environment to R, a powerful and versatile statistical analysis tool. R is not only a programming language but also a platform widely used in data science, statistical computing, and predictive modeling. Tech giants like Microsoft, Meta, Google, Amazon, and Netflix rely on R for data analysis, modeling, and machine learning. The good news is that your laptop should be more than capable of running R for differential expression (DE) analysis, data visualization, and other downstream statistical analyses. Let’s dive into the step-by-step process of performing DE analysis in R.

Before we proceed, a quick reminder for our readers: If you haven’t obtained your gene expression matrix yet, don’t worry! We’ve covered this crucial step in our previous posts. Be sure to check out “[Part 1](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/)” and “[Part 2](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/)” of our series, where we walk you through the process of quantifying gene expression from raw sequencing data to a count matrix. These posts provide step-by-step guidance on how to process your FASTQ files and generate the gene expression data we’ll be using in this analysis.

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/)

- [Installing R and RStudio](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Installing_R_and_RStudio)

  - [Install R](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Install_R)
  - [Install RStudio](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Install_RStudio)

- [Setting Up Your R Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Setting_Up_Your_R_Environment)

  - [Installing Required Packages](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Installing_Required_Packages)
  - [Loading Installed Packages](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Loading_Installed_Packages)
  - [Setting Your Working Directory](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Setting_Your_Working_Directory)

- [Data Preparation and Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Data_Preparation_and_Analysis)

  - [Reading Data](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Reading_Data)
  - [Data Cleaning](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Data_Cleaning)
  - [Creating Metadata](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Creating_Metadata)
  - [Creating a DGE List](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Creating_a_DGE_List)
  - [Normalization](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Normalization)
  - [Creating a PCA Plot](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Creating_a_PCA_Plot)
  - [Differential Expression Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Differential_Expression_Analysis)

- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/#Like_this)

## Installing R and RStudio

### Install R

Installing R is straightforward and similar to installing any other program on your computer:

1. Visit the official [R website](https://cran.r-project.org/).

2. Download the latest version for your operating system (Windows, Mac, or Linux).

3. Double-click the installation package and follow the prompts.

### Install RStudio

RStudio, a user-friendly graphical interface for R, can be installed in a similar manner:

1. Go to the [RStudio download page](https://posit.co/download/rstudio-desktop/).

2. Download the appropriate version for your system.

3. Install it like any other application.

RStudio enhances the R experience by providing a comprehensive set of tools and features specifically designed for data analysis, visualization, and statistical computing. Here’s a quick overview of the RStudio interface:

- **Code Editor Window**: This is where you write and edit your code. Create a new R script by clicking the green button with a white cross in the top left corner. You can run sections of code by highlighting them and clicking “Run,” or execute the entire script by clicking “Source.”

- **R Console Window**: This window allows real-time interaction with R. Any code you run from the script editor will appear here, along with errors, results, and print statements.

- **Work Space Window**: This area displays a list of all objects in your current R session, including variables, data sets, and functions.

- **Plot & Files Window**: Here, you can view visualizations generated by your code, such as graphs and charts. You can also export or zoom into plots from this window.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Pasted-image-20241016210818.png?resize=1024%2C772&ssl=1)

## Setting Up Your R Environment

### Installing Required Packages

R packages are collections of functions, data sets, and documentation bundled together to perform specialized tasks. We’ll need several packages for our DE analysis. These packages are stored on different repositories, primarily CRAN and Bioconductor.

To install the necessary packages, copy and paste the following code into the “R Console” window of RStudio:

```r
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

BiocManager::install(c("sva", "edgeR", "limma", "Biobase", "biomaRt", 
                       "clusterProfiler", "EnhancedVolcano", "org.Hs.eg.db", 
                       "org.Mm.eg.db", "org.Rn.eg.db"))

install.packages(c("data.table", "readxl", "stringr", "ggplot2", "ggrepel", 
                   "ggfortify", "ggprism", "pheatmap", "VennDiagram", 
                   "corrplot", "Hmisc", "stats", "tidyverse"))
```

Be patient as the packages install. If prompted for package updates, you can press “N” to skip them.

### Loading Installed Packages

Once the packages are installed, load them into your RStudio session. Select the following code and click “Run” in the Code Editor window:

```r
library(data.table)
library(edgeR)
library(limma)
library(ggplot2)
library(ggrepel)
library(ggfortify)
library(stats)
library(sva)
```

### Setting Your Working Directory

Create a folder on your computer for this project and set it as your working directory in R. This ensures all output files will be saved in this location unless specified otherwise. Here are examples for both MacOS and Windows:

```bash
# MacOS
setwd("/Users/yourusername/Documents/RNAseq")

# Windows
setwd("C:\Users\yourusername\Documents\RNAseq")
```

Replace “yourusername” with your actual username.

## Data Preparation and Analysis

### Reading Data

If you haven’t combined the count files into a single expression matrix, here’s how to do it in R:

```r
# Retrieve count file paths
file_names <- list.files(count_file_path, pattern = "featureCounts_exon\\.txt$", recursive = T, full.names = T)
  
# Read all count files into a DGElist
dgls <- readDGE(file_names, columns = c(1, 7), skip = 1)
counts_raw <- as.data.frame(dgls$counts)
  
# Rename samples
sample_name <- gsub("_featureCounts_exon", "", basename(colnames(counts_raw)))
colnames(counts_raw) <- sample_name

# Save the combined count table
fwrite(counts_raw, "counts_raw.tsv", sep = "\t")
```

Start by reading your raw count data:

```r
count_tbl <- fread("counts_raw.tsv", data.table = F)
```

This command reads a file named “counts_raw.tsv” and stores its contents in a variable called `count_tbl`. You can view this data frame by clicking on `count_tbl` in the “Work Space” window or by typing `View(count_tbl)` in the “R Console” window.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Pasted-image-20241016210417.png?resize=1024%2C617&ssl=1)

### Data Cleaning

#### Adding Row Names to the Count Table

Transform the first column into row names and remove it from the table:

```r
rownames(count_tbl) <- count_tbl[[1]]
count_tbl <- count_tbl[, -1]
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-16-at-9.22.09-PM-1.png?resize=860%2C1024&ssl=1)

#### Filtering Low-Expressed Genes

Remove genes with low or no expression:

```r
perc_keep <- 0.8
gene_keep <- rowSums(count_tbl > 0) >= ceiling(perc_keep * ncol(count_tbl))
count_tbl_low_rm <- count_tbl[gene_keep, ]
```

This code keeps genes that are expressed in at least 80% of the samples.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-16-at-9.39.12-PM.png?resize=1024%2C829&ssl=1)

### Creating Metadata

Create a metadata table with information about each sample:

```r
meta <- data.frame(SampleID = colnames(count_tbl),
                   Treatment = c("KRAS_SPIB", "KRAS_SPIB", "KRAS", "KRAS"))
rownames(meta) <- meta$SampleID
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-16-at-9.33.39-PM.png?resize=538%2C244&ssl=1)

### Creating a DGE List

Combine the count data and sample information into a DGEList object:

```r
dge <- DGEList(counts=count_tbl_low_rm, samples = meta)
```

### Normalization

Normalize the data to adjust for technical differences between samples:

```r
dge <- calcNormFactors(dge, method = "TMM")
dge_v <- voom(dge, plot=TRUE)
# save the dge_v object for later use
saveRDS(dge_v, "dge_v.rds")
```

This process will generate a mean-variance plot to visualize the normalization results.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-16-at-9.50.58-PM.png?resize=906%2C930&ssl=1)

### Creating a PCA Plot

Generate a Principal Component Analysis (PCA) plot to visualize the overall structure of your data:

```r
shape_column <- "Treatment"
color_column <- "Treatment"
label <- TRUE
label_size <- 4
plot_save_name <- "PCA_Plot.pdf"

meta_table <- dge_v$targets
count_table_t <- as.data.frame(t(dge_v$E))
pca_prep <- prcomp(count_table_t, scale. = TRUE)

pca_plot <- autoplot(pca_prep, label, shape = shape_column, data = meta_table, colour = color_column) +
  geom_text_repel(aes(label = rownames(meta_table)), size = label_size) +
  theme(panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        panel.background = element_blank(),
        axis.line = element_line(colour = "black"),
        panel.grid.minor.y=element_blank(),
        panel.grid.major.y=element_blank())

ggsave(plot_save_name, device = "pdf", units = "cm", width = 16, height = 14)
```

A PCA plot (Principal Component Analysis plot) is a graphical representation used in RNA-Seq analysis to visualize the overall structure of the data and to identify patterns such as sample clustering, batch effects, or outliers. In PCA plot, samples from the same treatment group tend to cluster.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-16-at-10.03.42-PM.png?resize=1024%2C899&ssl=1)

### Differential Expression Analysis

Perform the differential expression analysis using limma (special characters are not allowed in sample group names):

```r
comparison <- "KRAS_SPIB-KRAS"

design <- model.matrix(~ 0 + dge_v$targets[["Treatment"]])
colnames(design) <- gsub(".*]]", "", colnames(design))

contrast_matrix <- makeContrasts(contrasts = comparison, levels = design)

fit <- lmFit(dge_v, design)
fit_2 <- contrasts.fit(fit, contrast_matrix)
fit_2 <- eBayes(fit_2)

deg_tbl <- topTable(fit_2, coef = colnames(contrast_matrix), n = Inf, p.value=1, lfc=0, sort.by = "p")
fwrite(deg_tbl, "DEG_P1_lfc0.tsv", sep = "\t", row.names = T)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Picture1.jpg?resize=740%2C303&ssl=1)

This analysis compares the “KRAS_SPIB” group to the “KRAS” group. The results are stored in `deg_tbl` and saved to a file named “DEG_P1_lfc0.tsv” in your working directory. 

- logFC stands for log2 Fold Change.

- AveExpr shows the average expression level of the gene across all sample.

- t-statistic is a measure of how strongly the gene is differentially expressed between the conditions.

- The p-value tests the null hypothesis that there is no difference in gene expression between the two groups.

- The adjusted p-value** **account for multiple testing.

- The B-statisti**c** (log-odds) represents the log-odds that a gene is differentially expressed.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-16-at-10.18.48-PM.png?resize=1024%2C924&ssl=1)

## Conclusion

Congratulations! You’ve successfully completed the differential expression analysis of your RNAseq data. The resulting list of differentially expressed genes (DEGs) opens up numerous possibilities for deeper exploration of gene expression patterns and functional enrichment. In future tutorials, we’ll delve into interpreting these results and performing downstream analyses to gain biological insights from your data.

Remember, this is just the beginning of your RNAseq analysis journey. As you become more comfortable with these techniques, you’ll be able to customize and expand upon this workflow to suit your specific research needs.

  Want to run this whole pipeline on your own data? [Join the live workshop →](https://ngs101.com/workshop/rna-seq-workshop/)

## References

- Burger T. Fudging the volcano-plot without dredging the data. Nat Commun. 2024 Feb 15;15(1):1392. doi: 10.1038/s41467-024-45834-7. PMID: 38360828; PMCID: PMC10869345.
