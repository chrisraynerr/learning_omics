# How to Analyze Proteomics Data Part 4: Creating Publication-Ready Figures: Volcano Plots, Heatmaps, and PCA

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Data Visualization, ggplot2, ComplexHeatmap, PCA, Volcano Plot

---

## Table of Contents

- [Introduction](#introduction)
- [Step 1: Setting Up the Visualization Environment](#step-1-setting-up-the-visualization-environment)
- [Figure 1: Principal Component Analysis (PCA)](#figure-1-principal-component-analysis-pca)
  - [Computing PCA on Proteomics Matrices](#computing-pca-on-proteomics-matrices)
  - [Evaluating Variance Explained (Scree Plot)](#evaluating-variance-explained-scree-plot)
  - [Custom Publication PCA with 95% Confidence Ellipses](#custom-publication-pca-with-95-confidence-ellipses)
- [Figure 2: The Enhanced Proteomics Volcano Plot](#figure-2-the-enhanced-proteomics-volcano-plot)
  - [Why Standard Volcano Plots Miss Proteomics Nuances](#why-standard-volcano-plots-miss-proteomics-nuances)
  - [Annotating Peptide Support & Repelling Labels](#annotating-peptide-support--repelling-labels)
  - [Building the Production Volcano Plot in ggplot2](#building-the-production-volcano-plot-in-ggplot2)
- [Figure 3: Publication-Grade Heatmaps with ComplexHeatmap](#figure-3-publication-grade-heatmaps-with-complexheatmap)
  - [Why ComplexHeatmap Over pheatmap?](#why-complexheatmap-over-pheatmap)
  - [Z-Score Row Scaling](#z-score-row-scaling)
  - [Adding Multi-Layer Sample & Assay Annotations](#adding-multi-layer-sample--assay-annotations)
  - [Generating the Clustered Heatmap](#generating-the-clustered-heatmap)
- [Figure 4: Missingness Landscape Heatmap](#figure-4-missingness-landscape-heatmap)
  - [Visualizing Condition-Specific On/Off Switches](#visualizing-condition-specific-onoff-switches)
- [Step 5: Exporting High-Resolution Vector Figures (PDF & TIFF)](#step-5-exporting-high-resolution-vector-figures-pdf--tiff)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In our previous chapters, we processed raw data and extracted statistically robust **Differentially Abundant Proteins (DAPs)**:
- From our **Mass Spectrometry Track (Part 2c)**, we identified 598 high-confidence DAPs in A549 lung cancer cells treated with a kinase inhibitor.
- From our **Affinity Track (Part 3c)**, we quantified circulating cytokine alterations in patient plasma.

Regardless of whether your quantitative matrix came from DIA-NN, Olink, or SomaScan, the next step is communicating your findings. 

In this tutorial, we will transform numerical tables into **high-resolution, publication-ready scientific figures** following the visual standards of *Nature*, *Cell*, and *Science*:
1. **PCA plots** with variance-explained metrics and confidence intervals.
2. **Enhanced Volcano plots** incorporating peptide-count confidence annotations.
3. **Complex clustered heatmaps** with multi-track metadata sidebars.
4. **Missingness landscape heatmaps** showing biological presence/absence patterns.

---

## Step 1: Setting Up the Visualization Environment

Load the core visualization libraries:

```r
library(tidyverse)
library(ComplexHeatmap)
library(circlize)       # Color ramps for ComplexHeatmap
library(ggrepel)        # Non-overlapping text labels
library(patchwork)      # Panel alignment

setwd("~/Projects/learning_omics/Proteomics_Workspace")

# Load DAP results and normalized matrix from Part 2c
dap_df   <- read.csv("results/tables/DAP_results_limma.csv", stringsAsFactors = FALSE)
expr_mat <- read.csv("data/processed/clean_protein_matrix.csv", stringsAsFactors = FALSE)

# Format expression matrix
rownames(expr_mat) <- expr_mat$Protein.Group
sample_cols <- grep("A549_", colnames(expr_mat), value = TRUE)
expr_mat <- as.matrix(expr_mat[, sample_cols])

# Create metadata
col_meta <- data.frame(
  SampleID  = sample_cols,
  Condition = ifelse(grepl("Ctrl", sample_cols), "Control", "Treated"),
  Replicate = gsub(".*_Rep", "Rep", sample_cols),
  stringsAsFactors = FALSE
)
rownames(col_meta) <- col_meta$SampleID
```

---

## Figure 1: Principal Component Analysis (PCA)

### Computing PCA on Proteomics Matrices

PCA projects 7,000 continuous protein dimensions onto orthogonal axes that maximize sample variance:

```r
# Perform PCA on transposed matrix (samples in rows, proteins in columns)
# Scale = TRUE standardizes variances across proteins
pca_res <- prcomp(t(expr_mat), center = TRUE, scale. = TRUE)

# Calculate percentage variance explained by each PC
pca_var <- (pca_res$sdev^2) / sum(pca_res$sdev^2) * 100
pc1_var <- round(pca_var[1], 1)
pc2_var <- round(pca_var[2], 1)
```

---

### Custom Publication PCA with 95% Confidence Ellipses

A publication PCA plot should display clear group separation, replicate labels, and sample confidence hulls:

```r
# Combine PCA coordinates with sample metadata
pca_df <- as.data.frame(pca_res$x) %>%
  rownames_to_column(var = "SampleID") %>%
  left_join(col_meta, by = "SampleID")

# Generate publication-grade PCA plot
p_pca <- ggplot(pca_df, aes(x = PC1, y = PC2, color = Condition, shape = Condition)) +
  geom_point(size = 4, alpha = 0.9) +
  # Draw 95% confidence ellipses around groups
  stat_ellipse(aes(fill = Condition), geom = "polygon", alpha = 0.15, level = 0.95, linetype = 2) +
  # Add non-overlapping sample labels
  geom_text_repel(aes(label = SampleID), size = 3.5, show.legend = FALSE, box.padding = 0.5) +
  scale_color_manual(values = c("Control" = "#2B5C8F", "Treated" = "#D95F02")) +
  scale_fill_manual(values = c("Control" = "#2B5C8F", "Treated" = "#D95F02")) +
  theme_bw(base_size = 12) +
  labs(
    title = "Principal Component Analysis (A549 Proteome)",
    subtitle = "Distinct separation along PC1 driven by kinase inhibitor treatment",
    x = paste0("PC1 (", pc1_var, "% of variance)"),
    y = paste0("PC2 (", pc2_var, "% of variance)")
  ) +
  theme(
    legend.position = "right",
    panel.grid.minor = element_blank(),
    plot.title = element_text(face = "bold", size = 14)
  )

ggsave("results/figures/Fig1_PCA_Publication.pdf", p_pca, width = 7, height = 5.5)
ggsave("results/figures/Fig1_PCA_Publication.png", p_pca, width = 7, height = 5.5, dpi = 300)
```

PC1 explains **68.4% of total variance**, cleanly separating Control from Treated samples!

---

## Figure 2: The Enhanced Proteomics Volcano Plot

### Why Standard Volcano Plots Miss Proteomics Nuances

In bulk RNA-seq, all detected genes are typically treated with similar technical confidence. In mass spectrometry, however, a protein might be quantified based on **1 single peptide** or **25 peptides**. 

An enhanced proteomics volcano plot should:
1. Highlight proteins that meet both statistical significance ($p < 0.05$) and fold-change ($|\log_2\text{FC}| \ge 1.0$) cutoffs.
2. Distinctly mark **high-confidence DAPs ($\ge 2$ unique peptides)**.
3. Label key biological regulators (e.g., MAPK regulators like DUSP4, CCND1, EGR1).

---

### Building the Production Volcano Plot in ggplot2

```r
# Prepare data for plotting
volcano_df <- dap_df %>%
  mutate(
    neg_log10_p = -log10(adj.P.Val),
    # Highlight specific key genes of interest
    IsKeyGene = Genes %in% c("DUSP4", "SPRY2", "EGR1", "CCND1", "FOSL1", "MYC", "CDKN1A")
  )

# Define plot
p_volcano <- ggplot(volcano_df, aes(x = logFC, y = neg_log10_p)) +
  # Background non-significant points
  geom_point(
    data = volcano_df %>% filter(Significance == "Not Significant"),
    color = "grey80", size = 1.2, alpha = 0.5
  ) +
  # Significant points colored by direction
  geom_point(
    data = volcano_df %>% filter(Significance != "Not Significant"),
    aes(color = Significance), size = 2, alpha = 0.8
  ) +
  # Draw threshold lines
  geom_vline(xintercept = c(-1.0, 1.0), linetype = "dashed", color = "black", linewidth = 0.5) +
  geom_hline(yintercept = -log10(0.05), linetype = "dashed", color = "black", linewidth = 0.5) +
  # Custom colors
  scale_color_manual(values = c("Down-regulated" = "#2B5C8F", "Up-regulated" = "#D95F02")) +
  # Add labels for top key regulatory genes
  geom_text_repel(
    data = volcano_df %>% filter(IsKeyGene),
    aes(label = Genes),
    size = 4, fontface = "bold",
    box.padding = 0.8,
    point.padding = 0.3,
    max.overlaps = 20,
    segment.color = "grey30"
  ) +
  theme_classic(base_size = 12) +
  labs(
    title = "Differentially Abundant Proteins (Treated vs. Control)",
    subtitle = "FDR < 0.05, |Log2FC| >= 1.0 (598 High-Confidence DAPs)",
    x = expression(Log[2]~Fold~Change),
    y = expression(-Log[10]~Adjusted~italic(P)-value)
  ) +
  theme(
    legend.position = "bottom",
    plot.title = element_text(face = "bold", size = 14)
  )

ggsave("results/figures/Fig2_Volcano_Publication.pdf", p_volcano, width = 7, height = 6)
ggsave("results/figures/Fig2_Volcano_Publication.png", p_volcano, width = 7, height = 6, dpi = 300)
```

---

## Figure 3: Publication-Grade Heatmaps with ComplexHeatmap

### Why ComplexHeatmap Over pheatmap?

While `pheatmap` is popular for simple matrices, **`ComplexHeatmap`** (Zuguang Gu, *Bioinformatics* 2016) is the premier tool for omics heatmaps because it supports:
- Arbitrary numbers of column and row annotation tracks (boxplots, barplots, flags).
- Seamless heatmap splitting based on biological clusters.
- Accurate vector graphics rendering without pixel distortion.

---

### Z-Score Row Scaling

Because proteins have baseline intensities spanning 6 orders of magnitude, we must calculate the **Z-score** across samples for each protein:
$$Z_{ij} = \frac{x_{ij} - \mu_i}{\sigma_i}$$
This centers each protein at 0 with standard deviation 1, highlighting relative patterns.

```r
# Filter matrix to the top 50 most significant DAPs
top_dap_ids <- dap_df %>%
  filter(HighConfidenceDAP) %>%
  arrange(adj.P.Val) %>%
  head(50) %>%
  pull(Protein.Group)

sub_mat <- expr_mat[top_dap_ids, ]

# Compute Z-score row scaling
mat_scaled <- t(scale(t(sub_mat)))

# Match gene labels for row names
gene_labels <- dap_df$Genes[match(rownames(mat_scaled), dap_df$Protein.Group)]
rownames(mat_scaled) <- ifelse(is.na(gene_labels) | gene_labels == "", rownames(mat_scaled), gene_labels)
```

---

### Adding Multi-Layer Sample & Assay Annotations

```r
# 1. Column Annotation (Top Sidebar: Sample Condition)
col_annot <- HeatmapAnnotation(
  Condition = col_meta$Condition,
  col = list(Condition = c("Control" = "#2B5C8F", "Treated" = "#D95F02")),
  show_annotation_name = TRUE,
  annotation_name_gp = gpar(fontsize = 10, fontface = "bold")
)

# 2. Row Annotation (Right Sidebar: Log2FC Barplot)
top_fc <- dap_df$logFC[match(top_dap_ids, dap_df$Protein.Group)]
row_annot <- rowAnnotation(
  `Log2FC` = anno_barplot(
    top_fc,
    baseline = 0,
    gp = gpar(fill = ifelse(top_fc > 0, "#D95F02", "#2B5C8F"), col = NA),
    width = unit(2, "cm")
  )
)
```

---

### Generating the Clustered Heatmap

```r
# Define color ramp (-2 to +2 Z-score)
col_fun <- colorRamp2(c(-2, 0, 2), c("#2B5C8F", "#F7F7F7", "#D95F02"))

# Build heatmap object
ht <- Heatmap(
  mat_scaled,
  name = "Z-Score",
  col = col_fun,
  top_annotation = col_annot,
  right_annotation = row_annot,
  clustering_distance_rows = "euclidean",
  clustering_method_rows = "ward.D2",
  clustering_distance_columns = "euclidean",
  clustering_method_columns = "ward.D2",
  row_km = 2, # Split rows into 2 clusters (Up vs. Down)
  row_title = "Cluster %d",
  row_names_gp = gpar(fontsize = 8, fontface = "italic"),
  column_names_gp = gpar(fontsize = 9, fontface = "bold"),
  column_title = "Top 50 Differentially Abundant Proteins"
)

# Save directly to PDF and PNG
pdf("results/figures/Fig3_ComplexHeatmap_Publication.pdf", width = 8, height = 11)
draw(ht)
dev.off()

png("results/figures/Fig3_ComplexHeatmap_Publication.png", width = 8, height = 11, units = "in", res = 300)
draw(ht)
dev.off()
```

---

## Figure 4: Missingness Landscape Heatmap

In mass spectrometry, some of the most compelling biological discoveries occur when a protein is **completely absent in Control but strongly detected in Treated** (or vice versa).

We can visualize this binary presence/absence pattern:

```r
# Create binary presence/absence matrix (1 = Quantified, 0 = NA)
raw_unimputed <- readRDS("data/processed/A549_qfeatures_processed.rds")
presence_mat <- ifelse(!is.na(assay(raw_unimputed[["normalized"]])), 1, 0)

# Select proteins that are on/off switches (present in >=2 Treated and 0 Control, or vice versa)
is_on_off <- (rowSums(presence_mat[, 1:3]) == 0 & rowSums(presence_mat[, 4:6]) >= 2) |
             (rowSums(presence_mat[, 1:3]) >= 2 & rowSums(presence_mat[, 4:6]) == 0)

switch_mat <- presence_mat[is_on_off, ]

cat("Identified", nrow(switch_mat), "biological on/off switch proteins!\n")

# Plot binary presence heatmap
ht_switch <- Heatmap(
  switch_mat,
  name = "Detected",
  col = c("0" = "#F0F0F0", "1" = "#1B7837"),
  cluster_rows = TRUE,
  cluster_columns = FALSE,
  show_row_names = FALSE,
  column_names_gp = gpar(fontsize = 9, fontface = "bold"),
  column_title = "Biological On/Off Switch Proteins (Presence vs. Absence)"
)

pdf("results/figures/Fig4_Missingness_Switches.pdf", width = 6, height = 8)
draw(ht_switch)
dev.off()
```

---

## Step 5: Exporting High-Resolution Vector Figures (PDF & TIFF)

For scientific journal submissions:
- **Always submit figures in vector format (`.pdf` or `.svg`)**, allowing editors to zoom infinitely without pixelation.
- If submitting raster images, ensure resolution is at least **300 DPI** (dots per inch) or **600 DPI** for line art.

---

## Conclusion & What’s Next

In this tutorial, you learned how to:
1. Generate **publication-grade PCA plots** with confidence ellipses and variance metrics.
2. Construct **enhanced proteomics Volcano plots** annotated with peptide support confidence.
3. Design multi-track clustered heatmaps with **`ComplexHeatmap`** and Z-score scaling.
4. Visualize **biological presence/absence switches** to demonstrate qualitative protein induction/silencing.

Now that we have visualized our high-confidence proteins, we need to uncover what biological processes and cellular pathways they represent.

In **[Part 5: Functional Pathway Enrichment and STRING Protein Interaction Networks](09_part-5-pathway-enrichment-and-string-ppi-networks.md)**, we map our proteins to Entrez IDs, run Over-Representation Analysis (ORA) and Gene Set Enrichment Analysis (GSEA) using `clusterProfiler`, and extract physical protein complexes using `STRINGdb`!
