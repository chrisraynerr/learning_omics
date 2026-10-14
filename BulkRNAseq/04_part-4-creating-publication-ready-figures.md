# How to Analyze RNAseq Data for Absolute Beginners Part 4: A Complete Guide to Creating Publication-Ready Figures

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/)

- [Introduction](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Introduction)
- [Why Use R for RNA-seq Visualization?](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Why_Use_R_for_RNA-seq_Visualization)
- [Essential R Packages for Data Visualization](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Essential_R_Packages_for_Data_Visualization)
- [Creating Volcano Plots: Two Powerful Approaches](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Creating_Volcano_Plots_Two_Powerful_Approaches)

  - [Method 1: Quick Visualization with EnhancedVolcano](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Method_1_Quick_Visualization_with_EnhancedVolcano)
  - [Method 2: Custom Volcano Plots with ggplot2](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Method_2_Custom_Volcano_Plots_with_ggplot2)

- [Creating Informative Heatmaps](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Creating_Informative_Heatmaps)
- [Visualizing Individual Gene Expression](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Visualizing_Individual_Gene_Expression)

  - [1. Violin Plots: Showing Distribution and Individual Points](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#1_Violin_Plots_Showing_Distribution_and_Individual_Points)
  - [2. Box Plots: Traditional and Clear](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#2_Box_Plots_Traditional_and_Clear)
  - [3. Bar Plots: Showing Means with Error Bars](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#3_Bar_Plots_Showing_Means_with_Error_Bars)

- [Best Practices for RNA-seq Visualization](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Best_Practices_for_RNA-seq_Visualization)
- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Conclusion)
- [Next Steps](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Next_Steps)
- [Additional Resources](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Additional_Resources)
- [Keywords](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Keywords)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-4-a-complete-guide-to-creating-publication-ready-figures/#Like_this)

## Introduction

After identifying differentially expressed genes (DEGs), the next crucial step is visualizing your results effectively. Strong visualizations don’t just make your data look pretty – they transform complex genomic information into clear, interpretable insights that can reveal hidden patterns and biological stories within your data.

In this tutorial, you’ll learn how to create publication-quality visualizations that will help you:

- Identify meaningful patterns in gene expression data

- Discover potential biological mechanisms

- Present your findings effectively to both technical and non-technical audiences

- Enhance collaborative research efforts

## Why Use R for RNA-seq Visualization?

You might be wondering why we’re using R instead of other programming languages. As someone who started with Python and later transitioned to R during my postdoc, I can tell you that R’s bioinformatics ecosystem is unmatched. Whether you’re working with RNA-seq data, clinical statistics, or even geographical analyses, R provides specialized tools that make complex analyses straightforward and reproducible. While Python excels in many areas, R remains the go-to choice for biological data analysis.

## Essential R Packages for Data Visualization

Let’s start by setting up our R environment. If you’ve followed our previous tutorials, you should already have these packages installed. If not, check the “Install R Packages” section in our [differential expression analysis tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/).

```r
# Load required packages
library(data.table)    # For efficient data handling
library(tidyverse)    # For versatile data manipulation
library(ggplot2)       # For creating custom plots
library(ggrepel)       # For non-overlapping text labels
library(ggfortify)     # For statistical visualizations
library(ggprism)       # For publication-ready styling
library(pheatmap)      # For creating heatmaps
library(EnhancedVolcano) # For volcano plots
```

## Creating Volcano Plots: Two Powerful Approaches

### Method 1: Quick Visualization with EnhancedVolcano

The EnhancedVolcano package offers a straightforward way to create informative volcano plots. While it has some limitations with gene label placement, it’s perfect for getting a quick overview of your differential expression results.

First, let’s load our data:

```r
# Read differential expression results
deg_tbl <- fread("DEG_P1_lfc0.tsv")
dge_v <- readRDS("dge_v.rds")

# Set significance thresholds
FC_threshold <- 2
P_threshold <- 0.05

# Create color scheme for different gene categories
keyvals.colour_rd <- ifelse(
    (deg_tbl$logFC < -log2(FC_threshold) & deg_tbl$adj.P.Val < P_threshold), 'blue',
    ifelse((deg_tbl$logFC > log2(FC_threshold) & deg_tbl$adj.P.Val < P_threshold), 'red', 'grey30')
)

# Add informative labels showing number of genes in each category
names(keyvals.colour_rd)[keyvals.colour_rd == 'blue'] <- paste0(
    "Downregulated (n=", 
    sum(deg_tbl$logFC < -log2(FC_threshold) & deg_tbl$adj.P.Val < P_threshold), 
    ")"
)

names(keyvals.colour_rd)[keyvals.colour_rd == 'red'] <- paste0(
    "Upregulated (n=", 
    sum(deg_tbl$logFC > log2(FC_threshold) & deg_tbl$adj.P.Val < P_threshold), 
    ")"
)

names(keyvals.colour_rd)[keyvals.colour_rd == 'grey30'] <- 'Non-significant'
```

Now, let’s create our volcano plot:

```r
volcano_plot <- EnhancedVolcano(
    deg_tbl,
    lab = NA,
    x = 'logFC',
    y = 'adj.P.Val',
    title = '',
    subtitle = '',
    pCutoff = P_threshold,
    FCcutoff = log2(FC_threshold),
    gridlines.major = FALSE,
    gridlines.minor = FALSE,
    ylim = c(0, max(-log10(deg_tbl$adj.P.Val), na.rm=TRUE) + 0.5),
    colCustom = keyvals.colour_rd,
    axisLabSize = 20,
    labSize = 3,
    legendLabSize = 16,
    legendIconSize = 7,
    captionLabSize = 16,
    colAlpha = 1,
    pointSize = 0.3
)

# Save your plot
ggsave(
    "DEG_Volcano_Plot.png",
    volcano_plot,
    device = "png",
    units = "cm",
    width = 16,
    height = 18
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/DEG_Volcano_Plot_Enhanced.png?resize=910%2C1024&ssl=1)

### Method 2: Custom Volcano Plots with ggplot2

When you need more control over your visualization, particularly for highlighting specific genes, ggplot2 provides the flexibility you need:

```r
# Prepare data
deg_tbl$neg_log10_adj.P.Val <- -log10(deg_tbl$adj.P.Val)
deg_tbl$significance <- ifelse(
    deg_tbl$adj.P.Val < P_threshold & abs(deg_tbl$logFC) > log2(FC_threshold),
    ifelse(deg_tbl$logFC > 0, "Upregulated", "Downregulated"),
    "Non-Significant"
)

# Select top genes to label
genes_to_label <- deg_tbl[order(deg_tbl$adj.P.Val)]$V1[1:15]

# Create enhanced volcano plot
custom_volcano <- ggplot(deg_tbl, aes(x = logFC, y = neg_log10_adj.P.Val)) +
    geom_point(aes(color = significance), alpha = 0.8, size = 1) +
    scale_color_manual(
        values = c("Upregulated" = "red", "Downregulated" = "blue", "Non-Significant" = "gray80")
    ) +
    geom_vline(xintercept = c(-log2(FC_threshold), log2(FC_threshold)), 
               linetype = "dashed", color = "gray50") +
    geom_hline(yintercept = -log10(P_threshold), 
               linetype = "dashed", color = "gray50") +
    geom_text_repel(
        data = subset(deg_tbl, V1 %in% genes_to_label),
        aes(label = V1),
        size = 3,
        box.padding = 0.5,
        max.overlaps = Inf
    ) +
    labs(
        x = expression(log[2]~"Fold Change"),
        y = expression(-log[10]~"adjusted p-value"),
        color = "Differential Expression"
    ) +
    theme_minimal() +
    theme(
        legend.position = "right",
        panel.grid.minor = element_blank(),
        text = element_text(size = 12),
        axis.text = element_text(size = 10)
    )
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/DEG_Volcano_ggplot2.png?resize=1024%2C1024&ssl=1)

## Creating Informative Heatmaps

Heatmaps are excellent for visualizing expression patterns across multiple genes and samples. They can reveal gene clusters and expression patterns that might not be obvious from other visualizations.

```r
# Prepare sample annotations
col_annot_df <- data.frame(
    Treatment = dge_v$targets$Treatment,
    row.names = rownames(dge_v$targets)
)
col_annot_df <- col_annot_df[order(col_annot_df$Treatment), , drop = FALSE]

# Select significant DEGs
deg_sig <- deg_tbl[abs(deg_tbl$logFC) > log2(FC_threshold) & 
                   deg_tbl$adj.P.Val < P_threshold]$V1
expr_deg <- dge_v$E[deg_sig, rownames(col_annot_df), drop = FALSE]

# Create heatmap
heatmap_custom <- pheatmap(
    expr_deg,
    scale = "row",
    cluster_rows = TRUE,
    cluster_cols = FALSE,
    show_rownames = FALSE,
    show_colnames = TRUE,
    color = colorRampPalette(c("navy", "white", "red"))(100),
    annotation_col = col_annot_df,
    main = "Differential Expression Heatmap"
)

save_pheatmap_pdf <- function(x, filename, width=8, height=8) {
  stopifnot(!missing(x))
  stopifnot(!missing(filename))
  pdf(filename, width=width, height=height)
  grid::grid.newpage()
  grid::grid.draw(x$gtable)
  dev.off()
}

save_pheatmap_pdf(heatmap_custom, "heatmap_custom.pdf", width=4, height=4)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-21-at-11.20.31%E2%80%AFPM.png?resize=1018%2C1024&ssl=1)

## Visualizing Individual Gene Expression

Sometimes you need to focus on specific genes of interest. Here are three effective ways to visualize individual gene expression:

### 1. Violin Plots: Showing Distribution and Individual Points

```r
gene_to_plot <- "Acan"  # Replace with your gene of interest

# Prepare data
dge_v$E <- dge_v$E + abs(min(dge_v$E)) + 1 # transform all gene expression values to positive

gene_data <- data.frame(
    Treatment = dge_v$targets$Treatment,
    Expression = dge_v$E[gene_to_plot,]
)

# Create violin plot
violin_plot <- ggplot(gene_data, aes(x = Treatment, y = Expression)) +
    geom_violin(aes(fill = Treatment), alpha = 0.7) +
    geom_jitter(width = 0.2, alpha = 0.6) +
    stat_summary(fun = mean, geom = "point", size = 3, color = "black") +
    stat_summary(fun.data = mean_se, geom = "errorbar", width = 0.2) +
    theme_minimal() +
    labs(title = paste(gene_to_plot, "Expression"), y = "log2 Expression") +
    theme(legend.position = "none")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/violin_plot_claude-1.png?resize=477%2C481&ssl=1)

### 2. Box Plots: Traditional and Clear

```r
box_plot <- ggplot(gene_data, aes(x = Treatment, y = Expression)) +
    geom_boxplot(aes(fill = Treatment), alpha = 0.7) +
    geom_jitter(width = 0.2, alpha = 0.6) +
    theme_minimal() +
    labs(title = paste(gene_to_plot, "Expression"), y = "log2 Expression") +
    theme(legend.position = "none")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/boxplot_claude-1.png?resize=477%2C481&ssl=1)

### 3. Bar Plots: Showing Means with Error Bars

```r
# Calculate means and standard errors
summary_data <- gene_data %>%
    group_by(Treatment) %>%
    summarise(
        mean = mean(Expression),
        se = sd(Expression)/sqrt(n())
    )

# Create bar plot
bar_plot <- ggplot(summary_data, aes(x = Treatment, y = mean, fill = Treatment)) +
    geom_bar(stat = "identity", alpha = 0.7) +
    geom_errorbar(aes(ymin = mean-se, ymax = mean+se), width = 0.2) +
    theme_minimal() +
    labs(title = paste(gene_to_plot, "Expression"), y = "Mean log2 Expression") +
    theme(legend.position = "none")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/barplot_claude.png?resize=477%2C481&ssl=1)

## Best Practices for RNA-seq Visualization

- **Choose the Right Plot Type**

- Use volcano plots for global differential expression overview

- Use heatmaps for expression patterns across multiple genes

- Use violin/box plots for detailed views of individual genes

- **Color Selection**

- Use colorblind-friendly palettes

- Maintain consistency across related figures

- Use appropriate color scales for different data types

- **Labels and Annotations**

- Include clear axis labels

- Add informative titles

- Label key features or genes of interest

- **Statistical Information**

- Show significance thresholds

- Include error bars where appropriate

- Indicate sample sizes

## Conclusion

Creating effective visualizations is crucial for understanding and communicating your RNA-seq results. The techniques covered in this tutorial will help you create clear, informative, and publication-ready figures that effectively tell your data’s story.

  Want to run this whole pipeline on your own data? [Join the live workshop →](https://ngs101.com/workshop/rna-seq-workshop/)

## Next Steps

Ready to dig deeper into your RNA-seq analysis? Our next tutorial will cover pathway analysis, where we’ll explore the biological mechanisms underlying your differential expression results.

## Additional Resources

- [Bioconductor visualization guidelines](https://bioconductor.org)

- [ggplot2 documentation](https://ggplot2.tidyverse.org)

- [R color palettes for biological data](https://cran.r-project.org/web/packages/RColorBrewer)

## Keywords

RNA-seq, data visualization, differential expression, volcano plot, heatmap, R programming, bioinformatics, gene expression analysis, ggplot2, pheatmap
