# How to Analyze Single-Cell RNA-seq Data &#8212; Complete Beginner's Guide Part 15: Better Visualization with scplotter

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/)

---

*Recreate every plot from this series with cleaner code and publication-ready output — one function, one line, no patchwork gymnastics.*

---

Parts 1 through 14 of this series took the GSE174609 periodontitis PBMC dataset from raw FASTQ files all the way through quality control, integration, clustering, annotation, differential expression, trajectory inference, cell communication, and RNA velocity. Along the way you produced dozens of figures with Seurat’s built-in functions — `DimPlot`, `FeaturePlot`, `VlnPlot`, `DotPlot`, `DoHeatmap` — and stitched them together with `ggplot2` and `patchwork`.

Those functions work, but they share a common frustration: making a figure look the way a journal wants takes many lines of `theme()` tweaks, manual color palettes, and `ggsave()` calls. Part 15 introduces **scplotter**, a single visualization package that reads your existing Seurat object and reproduces nearly every plot from this series in one concise, well-labeled call — with attractive defaults already built in.

This is a visualization tutorial, not a new analysis. We will not change any of your results. We will simply re-draw them. By the end you will be able to swap your figure-generation code for something shorter, more consistent, and easier to read.

> 

**Prerequisites:** The annotated Seurat object from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) (`integrated_annotated_seurat.rds`). No new data is required. If you have completed any part of the [scRNA-seq series](https://ngs101.com/tutorials/#single-cell-seq), you already have everything you need.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/)

- [Introduction: What Is scplotter and Why Use It?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Introduction_What_Is_scplotter_and_Why_Use_It)

  - [What Is scplotter?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#What_Is_scplotter)
  - [scplotter vs. Seurat’s Built-In Plotting Functions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#scplotter_vs_Seurats_Built-In_Plotting_Functions)
  - [When Do You Need scplotter?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#When_Do_You_Need_scplotter)

- [How scplotter Is Organized: A Mental Model](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#How_scplotter_Is_Organized_A_Mental_Model)
- [Environment Setup and Installation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Environment_Setup_and_Installation)

  - [Installing the Required Packages](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Installing_the_Required_Packages)
  - [Loading Libraries and Configuring the Session](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Loading_Libraries_and_Configuring_the_Session)

- [Example Dataset: The Same PBMC Object You Already Have](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Example_Dataset_The_Same_PBMC_Object_You_Already_Have)

  - [Loading the Annotated Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Loading_the_Annotated_Seurat_Object)
  - [Detecting the Correct UMAP Reduction](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Detecting_the_Correct_UMAP_Reduction)

- [Visualization: Recreating the Series, One Function at a Time](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Visualization_Recreating_the_Series_One_Function_at_a_Time)

  - [Quality-Control Distributions (Recreating Part 2)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Quality-Control_Distributions_Recreating_Part_2)
  - [The Cluster and Cell-Type Map (Recreating Parts 3 and 4)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#The_Cluster_and_Cell-Type_Map_Recreating_Parts_3_and_4)
  - [Marker Gene Expression (Recreating Part 4)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Marker_Gene_Expression_Recreating_Part_4)
  - [Cell-Type Composition (Recreating Parts 3 and 5)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Cell-Type_Composition_Recreating_Parts_3_and_5)
  - [Marker and Differential Expression Plots (Recreating Parts 4 and 5)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Marker_and_Differential_Expression_Plots_Recreating_Parts_4_and_5)
  - [Cell-Cell Communication (Recreating Part 9)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Cell-Cell_Communication_Recreating_Part_9)
  - [Choosing a Clustering Resolution with a Cluster Tree (Extending Part 3)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Choosing_a_Clustering_Resolution_with_a_Cluster_Tree_Extending_Part_3)
  - [RNA Velocity Streams (Extending Part 14)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#RNA_Velocity_Streams_Extending_Part_14)

- [Best Practices and Common Pitfalls](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Best_Practices_and_Common_Pitfalls)

  - [Best Practices for Visualization with scplotter](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Best_Practices_for_Visualization_with_scplotter)
  - [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Common_Pitfalls_and_How_to_Avoid_Them)

- [Key Takeaways](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Key_Takeaways)
- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/#Like_this)

## Introduction: What Is scplotter and Why Use It?

### What Is scplotter?

**[scplotter](https://github.com/pwwang/scplotter)** is an R package for visualizing single-cell sequencing data. It is built on top of a lower-level plotting engine called [plotthis](https://github.com/pwwang/plotthis), and it understands the data structures you already use: Seurat objects, AnnData (`.h5ad`) files, and Giotto spatial objects. You point scplotter at your object, tell it what to plot, and it extracts the right data and returns a polished `ggplot` figure.

The package organizes the entire single-cell visualization landscape into a small number of high-level functions. Instead of remembering five different Seurat functions with five different argument styles, you learn a handful of scplotter functions that share a consistent grammar.

### scplotter vs. Seurat’s Built-In Plotting Functions

Both libraries can draw the same figures. The difference is in how much typing, theming, and palette management you do to get there. The table below maps the Seurat functions used earlier in this series to their scplotter equivalents.

`DimPlot()``CellDimPlot()``FeaturePlot()``FeatureStatPlot(plot_type = "dim")``VlnPlot()``FeatureStatPlot(plot_type = "violin")``DotPlot()``FeatureStatPlot(plot_type = "dot")``DoHeatmap()``pheatmap()``FeatureStatPlot(plot_type = "heatmap")``ggplot()``CellStatPlot()``ggplot()``ggrepel``MarkersPlot()``ggplot()``MarkersPlot(plot_type = "jitter")``clustree``ClustreePlot()``CCCPlot()``CellVelocityPlot()`

| What you want to show | Seurat function (Parts 1-14) | scplotter function |
| --- | --- | --- |
| Clusters or cell types on a UMAP |  |  |
| Gene expression on a UMAP |  |  |
| Gene expression as violins |  |  |
| Gene expression as a dot plot |  |  |
| Expression heatmap | / |  |
| Cell counts / proportions | manual  bar charts |  |
| Differential expression volcano | manual  + |  |
| Marker strength jitter plot | manual |  |
| Cluster tree across resolutions | the  package |  |
| Cell-cell communication | CellChat plotting functions |  |
| RNA velocity arrows | scVelo (Python) |  |

The practical differences come down to three things:

- **Less code.** A split, labeled, faceted UMAP that took a block of `DimPlot()` plus `theme()` calls in [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) becomes a single line in scplotter.

- **Better defaults.** scplotter ships with attractive color palettes, sensible legends, and clean themes, so most figures look presentation-ready without manual tuning.

- **Consistent grammar.** Almost every function accepts the same core arguments — `group_by`, `split_by`, `facet_by`, `palette`, `theme` — so once you learn one function, you know how to drive the others.

### When Do You Need scplotter?

scplotter is a convenience layer, not a requirement. Reach for it when:

- You want **publication-quality figures quickly** without writing long `theme()` blocks.

- You need **consistent styling** across many panels in a paper or report.

- You want **plot types Seurat does not offer out of the box**, such as Sankey/alluvial composition plots, ring/donut charts, radar plots, or cell-cell communication chord diagrams.

- You are **comparing conditions** (Healthy vs. Post-Treatment) and want easy splitting, faceting, and proportion calculations.

You do **not** need scplotter for the analysis itself. All the statistics, clustering, and modeling still come from Seurat, DESeq2, CellChat, and the other tools used earlier in the series. scplotter only changes how the results are drawn.

---

## How scplotter Is Organized: A Mental Model

Before writing code, it helps to have a map. scplotter groups its scRNA-seq functions into three “workhorses” plus a few specialists.

The three workhorses cover the vast majority of everyday figures:

1. **`CellDimPlot()`** — anything plotted on a dimensionality reduction (UMAP/t-SNE/PCA), colored by a metadata column. This is your cluster and cell-type map.

2. **`FeatureStatPlot()`** — anything that shows a *feature* (a gene’s expression or a numeric metadata column like a QC metric or a pathway score). One function, many `plot_type` options: `"dim"`, `"violin"`, `"box"`, `"dot"`, `"heatmap"`, `"ridge"`, and more.

3. **`CellStatPlot()`** — anything that counts or computes proportions of *cells* across groups. Bar charts, pies, rings, Sankey diagrams, and composition heatmaps all come from here.

The specialists handle specific analyses:

- **`MarkersPlot()`** — jitter plots, volcano plots, and ranked heatmaps directly from `FindMarkers()`/`FindAllMarkers()` output.

- **`CCCPlot()`** — cell-cell communication networks, chord diagrams, and dot plots.

- **`ClustreePlot()`** — cluster trees showing how clusters split and merge across clustering resolutions.

- **`CellVelocityPlot()`** — RNA velocity arrows, grids, and streams.

- **`EnrichmentPlot()`** and the GSEA helpers — pathway enrichment results.

Keep this map in mind as you read: nearly every plot below is just one of these functions with a different `plot_type` or a different `group_by` column.

---

## Environment Setup and Installation

This is a standalone tutorial, so the block below installs **every package the code uses**, even if you already have most of them from earlier parts. The list is:

- **scplotter** — the visualization package (installed from GitHub; it pulls in its plotting engine, plotthis, plus core dependencies like ggplot2).

- **Seurat** — used to load the annotated object and to run `FindMarkers()` / `FindAllMarkers()`.

- **ggplot2** — provides `ggsave()`, which we use to write every figure to disk.

- **dplyr** — used to detect the UMAP reduction name (`dplyr::case_when`) and to rename a column (`dplyr::rename`).

- **metap** — required by `CCCPlot()` to combine p-values when aggregating cell-cell communication results. metap in turn depends on **multtest** (Bioconductor); CRAN’s metap install does not pull it, so we install it explicitly below.

- **ComplexHeatmap** — the engine behind the dot-plot, heatmap, and CCC heatmap styles (Bioconductor package).

- **patchwork** — to lay out Healthy and Post-Treatment CCC panels side by side.

- **gglogger** — a plotthis suggestion that several scplotter functions need at runtime; CRAN install of scplotter does not pull it.

- **clustree** — the underlying engine for `ClustreePlot()`; also a suggested-not-imported dependency.

- **hdf5r** — the HDF5 reader scplotter uses to read your `.h5ad` file in Step 23. Requires the system HDF5 library; if installation fails, install it first (`brew install hdf5` on macOS, `apt-get install libhdf5-dev` on Debian/Ubuntu).

- **proxyC** — a sparse-matrix proximity computation used internally by `CellVelocityPlot`; also a suggested-not-imported dependency.

- **metR** — provides `geom_streamline`, which `CellVelocityPlot(plot_type = "stream")` uses to draw the flow lines.

> 

**CellChat is also used in Step 21 but is not installed here.** Its installation is involved (many system dependencies) and is fully walked through in [Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/). Anyone arriving at Step 21 should already have CellChat from that tutorial; if not, follow Part 9 first.

### Installing the Required Packages

```r
#-----------------------------------------------
# STEP 1: Install all packages used in this tutorial (only if missing)
#-----------------------------------------------

# CRAN packages
cran_packages <- c("remotes", "BiocManager", "Seurat", "ggplot2", "dplyr",
                   "metap", "patchwork", "gglogger", "clustree", "hdf5r",
                   "proxyC", "metR")
missing_cran  <- cran_packages[!(cran_packages %in% rownames(installed.packages()))]
if (length(missing_cran) > 0) {
  install.packages(missing_cran)
}

# Bioconductor packages
bioc_packages <- c("ComplexHeatmap", "multtest")
missing_bioc  <- bioc_packages[!(bioc_packages %in% rownames(installed.packages()))]
if (length(missing_bioc) > 0) {
  BiocManager::install(missing_bioc, update = FALSE, ask = FALSE)
}

# GitHub packages
if (!requireNamespace("scplotter", quietly = TRUE)) {
  remotes::install_github("pwwang/scplotter")
}
```

### Loading Libraries and Configuring the Session

```r
#-----------------------------------------------
# STEP 2: Load libraries and set up the session
#-----------------------------------------------

library(Seurat)
library(scplotter)
library(ggplot2)
library(ComplexHeatmap)
library(CellChat)
library(patchwork)

# Set the working directory used throughout the series
setwd("~/GSE174609_scRNA/visualization_scplotter")

# Create an output folder for the figures
dir.create("plots", showWarnings = FALSE)

# Fix the random seed so jittered points and palettes are reproducible
set.seed(42)
```

> 

**How figures are saved in this tutorial:** Most scplotter functions return a standard `ggplot` object, which we write with `ggsave()`. The exceptions are the dot-plot and heatmap styles, which are drawn by the ComplexHeatmap engine (installed automatically with plotthis) and return a different object type; those are saved with a base graphics device, `png(...); print(plot); dev.off()`. This mirrors the general R convention used across this series: `ggsave()` for ggplot-returning functions, and a graphics device for grid/base graphics.

---

## Example Dataset: The Same PBMC Object You Already Have

We use the **exact same dataset** as the rest of this series: the GSE174609 periodontitis PBMC data. There is nothing new to download. We start from the annotated Seurat object saved at the end of [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/), which already contains clusters, cell-type annotations, sample and condition metadata, and the UMAP embedding.

### Loading the Annotated Seurat Object

```r
#-----------------------------------------------
# STEP 3: Load the annotated object from Part 4
#-----------------------------------------------

# Path to the annotated object (adjust if your directory differs)
annotated_path <- "~/GSE174609_scRNA/cell_type_annotation/annotations/integrated_annotated_seurat.rds"

seurat_obj <- readRDS(annotated_path)

# Remove the Platelets cell type up front
seurat_obj <- subset(seurat_obj, subset = final_annotation != "Platelets")

# Drop the now-unused factor level so "Platelets" does not linger in legends
if (is.factor(seurat_obj$final_annotation)) {
  seurat_obj$final_annotation <- droplevels(seurat_obj$final_annotation)
}

# Free memory by dropping the dense all-genes scaled layer created in Part 4.
seurat_obj[["RNA"]]$scale.data <- NULL

# Set the active identity to the final cell-type labels.
Idents(seurat_obj) <- "final_annotation"
```

This object carries the metadata columns we rely on below: clustering results at several resolutions (`clusters_res_0.4`, `clusters_res_0.6`, `clusters_res_0.8`, `clusters_res_1`, `clusters_res_1.2` from [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/)), `final_annotation` (cell types from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/), now with Platelets removed), `condition` (Healthy / Post-Treatment), and `sample_id` (the individual donors). It also stores quality-control metrics (`nFeature_RNA`, `nCount_RNA`, `percent.mt`) computed back in [Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/). If your object also has a `seurat_clusters` column (Seurat’s default), the cluster UMAP below uses it; otherwise substitute one of your `clusters_res_*` columns.

### Detecting the Correct UMAP Reduction

In [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) you chose an integration method (Harmony, CCA, RPCA, or FastMNN), and the UMAP was named accordingly (for example, `umap.harmony`). The snippet below detects whichever UMAP is present so the rest of the tutorial works regardless of the method you used.

```r
#-----------------------------------------------
# STEP 4: Detect the UMAP reduction name
#-----------------------------------------------

# Prefer an integrated UMAP if one exists; otherwise fall back to a plain "umap"
available_reductions <- names(seurat_obj@reductions)
umap_reduction <- dplyr::case_when(
  "umap.harmony" %in% available_reductions ~ "umap.harmony",
  "umap.cca"     %in% available_reductions ~ "umap.cca",
  "umap.rpca"    %in% available_reductions ~ "umap.rpca",
  "umap.mnn"     %in% available_reductions ~ "umap.mnn",
  TRUE                                     ~ "umap"
)
```

With the object loaded and the reduction identified, we are ready to redraw the series.

---

## Visualization: Recreating the Series, One Function at a Time

The sections below follow the natural order of the analysis — quality control, the cluster map, marker expression, cell composition, differential expression, and cell communication — and recreate the key figure from each corresponding tutorial. Two final sections extend the series with a clustering-resolution tree ([Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/)) and an RNA velocity stream drawn from your [Part 14](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/) `.h5ad` file. Every scplotter function returns a standard `ggplot` object (except the dot-plot and heatmap styles, noted below), so most figures are saved with `ggsave()`.

### Quality-Control Distributions (Recreating Part 2)

In [Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/) you inspected per-cell QC metrics as violin plots, one panel per metric, split by sample. `FeatureStatPlot` reproduces all three panels in a single call by passing the metric names as `features` and the sample column as the identity.

```r
#-----------------------------------------------
# STEP 5: QC metric violins (replaces Seurat::VlnPlot)
#-----------------------------------------------

# 'features' here are numeric metadata columns, not genes.
# 'ident' puts one violin per sample; free y-scales keep each metric readable.
p_qc <- FeatureStatPlot(
  seurat_obj,
  features      = c("nFeature_RNA", "nCount_RNA", "percent.mt"),
  ident         = "sample_id",
  plot_type     = "violin",
  add_box       = TRUE,        # overlay a box plot to show the median and quartiles
  x_text_angle  = 45,          # rotate sample labels so they do not overlap
  facet_scales  = "free_y"     # each metric gets its own y-axis range
)

ggsave("plots/01_qc_violins.png", p_qc, width = 14, height = 5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/01_qc_violins.png?resize=1024%2C366&ssl=1)

The result is the same three-panel QC view from [Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/), but produced by one function instead of three `VlnPlot()` calls wrapped in `patchwork`.

> 

**Reading the QC violins:** All eight samples (Healthy_1-4 and Post_Patient_1-4) sit in tight, comparable ranges — median `nFeature_RNA` around 2,500 (about 2,000-3,000 across donors), median `nCount_RNA` around 5,000-8,000 with no donor obviously elevated, and `percent.mt` medians of 3-5% with Healthy_3 the slightly highest. No sample stands out as a dropout. That sample-to-sample consistency is exactly what you want before trusting integration; if one violin’s body sat dramatically higher or lower than the others (e.g. one donor at median 800 features instead of 2,500), that donor would need to be re-inspected before downstream analysis.

### The Cluster and Cell-Type Map (Recreating Parts 3 and 4)

This is the single most-used figure in any single-cell project, and where scplotter saves the most effort. `CellDimPlot` draws a reduction and colors it by any metadata column.

```r
#-----------------------------------------------
# STEP 6: UMAP colored by clusters and by cell type (replaces Seurat::DimPlot)
#-----------------------------------------------

# Unsupervised clusters from Part 3, with cluster numbers drawn on the plot.
# If your object has no "seurat_clusters" column, use one of your resolution
# columns instead, e.g. group_by = "clusters_res_0.6".
p_clusters <- CellDimPlot(
  seurat_obj,
  group_by  = "seurat_clusters",
  reduction = umap_reduction,
  label     = TRUE             # place the cluster label at each cluster's center
)

# Final cell-type annotations from Part 4, with labels repelled to avoid overlap
p_celltypes <- CellDimPlot(
  seurat_obj,
  group_by     = "final_annotation",
  reduction    = umap_reduction,
  label        = TRUE,
  label_insitu = TRUE,         # draw the actual labels on the clusters
  label_repel  = TRUE          # nudge labels apart so they stay readable
)

ggsave("plots/02_umap_clusters.png",  p_clusters,  width = 8, height = 7, dpi = 300)
ggsave("plots/03_umap_celltypes.png", p_celltypes, width = 9, height = 7, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/02_03_umap_clusters_celltype-e1780190076805-1024x387.png?resize=1024%2C387&ssl=1)

#### Checking Integration: Color by Sample and Condition

In Part 3 you confirmed that batch effects were corrected by coloring the UMAP by `sample_id` and `condition`. Good integration shows the colors thoroughly intermixed rather than forming separate islands.

```r
#-----------------------------------------------
# STEP 7: UMAP colored by sample and condition (batch-effect check)
#-----------------------------------------------

# Color by donor: well-mixed colors indicate corrected batch effects
p_by_sample <- CellDimPlot(
  seurat_obj,
  group_by  = "sample_id",
  reduction = umap_reduction
)

# Color by biological condition: Healthy and Post-Treatment should also intermix
p_by_condition <- CellDimPlot(
  seurat_obj,
  group_by  = "condition",
  reduction = umap_reduction,
  palette   = "Set1"           # swap palettes by name; no manual hex codes needed
)

ggsave("plots/04_umap_by_sample.png",    p_by_sample,    width = 9, height = 7, dpi = 300)
ggsave("plots/05_umap_by_condition.png", p_by_condition, width = 8, height = 7, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/04_05_umap-e1780190270754-1024x403.png?resize=1024%2C403&ssl=1)

> 

**Reading the integration checks:** In plot 04 the eight donors are thoroughly intermixed within every cluster — no island is dominated by a single donor’s color. Plot 05 shows the same picture at condition resolution: **Healthy (35,476 cells, red)** and **Post_Treatment (37,134 cells, blue)** overlap throughout. That intermixing is the visual sign that the integration in [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) succeeded at removing donor- and condition-level batch effects; remaining structure can be trusted as biology rather than technical artifact. If any island looked monochrome here, you would go back and revisit the integration before drawing biological conclusions.

#### Splitting the UMAP by Condition

[Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) split the annotated UMAP into side-by-side panels by condition to spot population shifts after treatment. In Seurat this needed `split.by` plus theme adjustments; in scplotter the choice is between `split_by` (separate plot objects per panel) and `facet_by` (one plot with shared color scale and legend across panels). We use **`facet_by`** here because it guarantees each cell type keeps the same color across panels — with `split_by`, each panel orders its legend by its own per-panel frequencies and can assign different colors to the same cell type.

```r
#-----------------------------------------------
# STEP 8: One UMAP panel per condition (replaces DimPlot split.by)
#-----------------------------------------------

# facet_by keeps a single color scale and legend across panels, so a cell type's
# color is consistent in every panel. (split_by would produce separate plots,
# each ranking the legend by its own frequencies and re-assigning colors.)
p_split <- CellDimPlot(
  seurat_obj,
  group_by  = "final_annotation",
  facet_by  = "condition",     # one panel per condition with shared coloring
  reduction = umap_reduction
)

ggsave("plots/06_umap_split_condition.png", p_split, width = 14, height = 7, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/06_umap_split_condition.png?resize=1024%2C512&ssl=1)

> 

**Reading the split UMAP:** The two panels (**Healthy, N = 35,476** on the left and **Post_Treatment, N = 37,134** on the right) show the same UMAP shape with the seven cell types in matching positions and colors, which is the point of using `facet_by`. The biggest visible difference is in the central region: **Monocytes (pink) are noticeably more abundant in Post_Treatment** — they are barely visible in the Healthy panel but form a clear band between the NK cells (green) and CD4+ T cells (light green) in Post_Treatment. The Classical Monocytes island (top-left) also looks slightly sparser in Post_Treatment. Quantify these eyeball impressions with the composition bar in Step 16 before drawing conclusions.

> 

**Want per-panel cell counts in each legend?** Use `split_by = "condition"` instead, but lock the colors by passing a named palette: `palcolor = setNames(scales::hue_pal()(nlevels(seurat_obj$final_annotation)), levels(seurat_obj$final_annotation))`. This gives each panel its own legend (with per-condition counts) while still mapping each cell type to a fixed color.

#### A New Trick: Highlighting One Population

scplotter makes it trivial to spotlight a specific population — useful for figures that need to draw the reader’s eye to one cell type. There is no clean single-argument equivalent for this in base Seurat.

```r
#-----------------------------------------------
# STEP 9: Highlight a single cell type (a scplotter convenience)
#-----------------------------------------------

# The highlight expression is evaluated against the metadata
p_highlight <- CellDimPlot(
  seurat_obj,
  group_by  = "final_annotation",
  reduction = umap_reduction,
  highlight = 'final_annotation == "NK cells"'   # grey out everything else
)

ggsave("plots/07_umap_highlight_nk.png", p_highlight, width = 8, height = 7, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/07_umap_highlight_nk-e1780191361870-1024x740.png?resize=1024%2C740&ssl=1)

> 

**Reading the highlight:** NK cells (dark green with outlines) sit in a coherent block at the top of the central mass; everything else is greyed out so the reader’s eye lands immediately on the population of interest. A few stray NK-labeled cells appear in the T-cell zone just below the main NK block and one or two in the B-cell island — those are candidates for further inspection (possible doublets, transitional states, or annotation noise). Use this trick whenever a figure needs to draw attention to one population without removing the surrounding context.

### Marker Gene Expression (Recreating Part 4)

Part 4 leaned heavily on three marker-visualization styles: feature plots on the UMAP, violin plots per cluster, and a summary dot plot. All three come from one scplotter function, `FeatureStatPlot`, by changing `plot_type`. We use the canonical PBMC markers from Part 4.

```r
#-----------------------------------------------
# STEP 10: Define canonical PBMC markers (same set used in Part 4)
#-----------------------------------------------

pbmc_markers <- c(
  "CD3D",   # T cells
  "CD4",    # CD4+ T cells
  "CD8A",   # CD8+ T cells
  "NKG7",   # NK cells
  "CD79A",  # B cells
  "CD14",   # Classical Monocytes
  "FCGR3A"  # Non-classical / CD16 Monocytes
)
```

#### Marker Expression on the UMAP

```r
#-----------------------------------------------
# STEP 11: Marker expression on the UMAP (replaces Seurat::FeaturePlot)
#-----------------------------------------------

# plot_type = "dim" overlays expression on the reduction.
# Passing several genes returns one panel per gene automatically.
p_feature <- FeatureStatPlot(
  seurat_obj,
  features  = pbmc_markers,
  plot_type = "dim",
  reduction = umap_reduction,
  ncol      = 4                # arrange the marker panels in 4 columns
)

ggsave("plots/08_markers_featureplot.png", p_feature, width = 16, height = 8, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/08_markers_featureplot-scaled-e1780190375299-1019x1024.png?resize=1019%2C1024&ssl=1)

> 

**Reading the feature plots:** Each panel shows where one canonical marker is expressed on the UMAP, with red indicating high expression.

#### Marker Expression as Stacked Violins

A stacked violin plot is a compact way to confirm that each marker is specific to its cell type. The `stack` argument turns the usual one-panel-per-gene layout into a single tidy stack.

```r
#-----------------------------------------------
# STEP 12: Stacked marker violins (replaces Seurat::VlnPlot, stack = TRUE)
#-----------------------------------------------

p_violin <- FeatureStatPlot(
  seurat_obj,
  features  = pbmc_markers,
  ident     = "final_annotation",  # one column of violins per cell type
  plot_type = "violin",
  stack     = TRUE,                # stack genes vertically into one figure
  add_bg    = TRUE,                # shaded background bands aid readability
  flip      = TRUE                 # cell types on the x-axis, genes stacked on y
)

ggsave("plots/09_markers_violin_stacked.png", p_violin, width = 10, height = 8, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/09_markers_violin_stacked.png?resize=1024%2C819&ssl=1)

> 

**Reading the stacked violins:** The same markers as the feature plot, summarized per cell type. The diagonal pattern of “fat” violins is the visual signal you want — each cell type lights up its own canonical marker and is quiet for the others. This format is much more compact than the per-marker feature plots when you have many markers to inspect at once.

#### Marker Expression as a Dot Plot

The dot plot is the standard summary figure for marker panels: dot size encodes the fraction of cells expressing a gene, and color encodes average expression. `plot_type = "dot"` produces it directly.

Before drawing it, we create a lightweight copy of the object that keeps **all cells and metadata but only the marker genes**. The dot-plot and heatmap engines build a matrix from the object, and on a large object (72,649 cells with a dense all-gene scaled layer from Part 4) that matrix can exceed available RAM, producing `Error: vector memory limit ... reached`. Restricting to the seven marker genes makes the computation tiny and avoids the error entirely. (The earlier UMAP, feature, and violin plots fetch only the requested genes per cell, so they do not need this step.)

```r
#-----------------------------------------------
# STEP 13: Marker dot plot (replaces Seurat::DotPlot)
#-----------------------------------------------

# Keep all cells/metadata but only the marker genes. This makes the dot-plot
# and heatmap matrices trivially small and prevents large dense-matrix memory use.
seurat_markers <- subset(seurat_obj, features = pbmc_markers)

p_dot <- FeatureStatPlot(
  seurat_markers,                  # lightweight, marker-only object
  features  = pbmc_markers,
  ident     = "final_annotation",
  plot_type = "dot"
)

# The dot plot is a ComplexHeatmap object, so save it with a graphics device
png("plots/10_markers_dotplot.png", width = 9, height = 6, units = "in", res = 300)
print(p_dot)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/10_markers_dotplot-scaled-e1780190464113-1024x648.png?resize=1024%2C648&ssl=1)

> 

**Low on memory?** Step 3 already dropped the dense scaled layer, which removes the worst offender. If you skipped that step or are still tight, macOS users can raise R’s vector limit with `mem.maxVSize(vsize = 32000)` (in MB), but only if your machine physically has that much RAM.

> 

**Reading the dot plot:** Two visual encodings per cell — circle size = percent of cells in that group expressing the gene, color = average expression. A marker that is a large dark dot in exactly one cell type and small/pale everywhere else is an ideal single-cell-type marker; markers that are large across many cell types are less specific and should be combined with others.

> 

**Note on saving:** This is the first plot built on the ComplexHeatmap engine (the `"dot"` and `"heatmap"` styles), so it returns a `HeatmapList` rather than a `ggplot`. `ggsave()` cannot write that object type, so we save it with a `png(); print(); dev.off()` device instead. The ggplot-returning plots elsewhere in the tutorial continue to use `ggsave()`.

#### Marker Expression as a Heatmap

Part 4 and Part 5 both used heatmaps to summarize expression across cell types. `plot_type = "heatmap"` builds one with clustered rows and a clean legend.

```r
#-----------------------------------------------
# STEP 14: Expression heatmap (replaces Seurat::DoHeatmap / pheatmap)
#-----------------------------------------------

# Reuse the lightweight marker-only object from Step 13 for the same memory reason
p_heatmap <- FeatureStatPlot(
  seurat_markers,
  features  = pbmc_markers,
  ident     = "final_annotation",
  plot_type = "heatmap",
  name      = "Avg. Expression"   # the legend title
)

# The heatmap is a ComplexHeatmap object, so save it with a graphics device
png("plots/11_markers_heatmap.png", width = 9, height = 6, units = "in", res = 300)
print(p_heatmap)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/11_markers_heatmap-scaled-e1780190510698-1024x630.png?resize=1024%2C630&ssl=1)

> 

**Reading the heatmap:** Same marker-by-cell-type data as the dot plot, but with hierarchical dendrograms on both axes that group similar genes and similar cell types. Use this view when you want a cell-type similarity tree on top of the expression signal.

### Cell-Type Composition (Recreating Parts 3 and 5)

Throughout the series you built bar charts of cell counts and proportions with hand-written `ggplot()` code. `CellStatPlot` replaces all of that. It counts cells across groups and can compute fractions automatically.

```r
#-----------------------------------------------
# STEP 15: Cell counts per cell type (replaces manual ggplot bar charts)
#-----------------------------------------------

# With no group_by, this simply counts cells in each identity (cell type)
p_counts <- CellStatPlot(
  seurat_obj,
  ident        = "final_annotation",
  plot_type    = "bar",
  x_text_angle = 90               # vertical labels so long cell-type names do not overlap
)

ggsave("plots/12_celltype_counts.png", p_counts, width = 8, height = 5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/12_celltype_counts-e1780190565447-1024x837.png?resize=1024%2C837&ssl=1)

> 

**Reading the cell-count bar chart:** Cell counts per annotated cell type (bars sorted alphabetically). The skew of cell counts matters for downstream analysis: rare populations like Monocytes and CD8+ T cells will have less statistical power in any per-cell-type test, and any DE result driven by only a handful of cells should be interpreted cautiously. Anyone planning to subsample for balanced comparisons should make that decision here, before running the DE analyses below.

#### Composition by Condition

The more informative version compares cell-type proportions between Healthy and Post-Treatment. Setting `frac = "group"` makes each condition’s bar sum to 1, so the plot shows composition rather than raw counts.

```r
#-----------------------------------------------
# STEP 16: Cell-type composition per condition (stacked proportions)
#-----------------------------------------------

p_composition <- CellStatPlot(
  seurat_obj,
  ident     = "final_annotation",  # cell types -> stacked fill of each bar
  group_by  = "condition",         # condition -> one bar per condition (x-axis)
  frac      = "group",             # proportions within each condition (sum to 1)
  swap      = TRUE,                 # put condition on the x-axis, cell types as fill
  position  = "stack",             # stack cell types into a single bar
  plot_type = "bar"
)

ggsave("plots/13_composition_by_condition.png", p_composition, width = 7, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/13_composition_by_condition-e1780190666799-1024x684.png?resize=1024%2C684&ssl=1)

> 

**Why `swap = TRUE`?** By default `CellStatPlot` places the `ident` (here, the cell types) on the x-axis, which crowds the axis with all eight long cell-type names. `swap = TRUE` moves the `group_by` column (condition) onto the x-axis instead — just two or three short labels — and stacks the cell types as the fill, which is the “one bar per condition” composition figure we want. If you ever do keep many categories on the x-axis, rotate the labels with `x_text_angle = 90` to stand them upright.

#### Bonus Composition Views scplotter Adds for Free

Because all of these share the same `CellStatPlot` grammar, switching `plot_type` gives you views Seurat does not provide out of the box.

```r
#-----------------------------------------------
# STEP 17: Alternative composition plots (one argument changes the chart)
#-----------------------------------------------

# Concentric ring (donut) chart comparing composition between conditions.
# 'ring' draws ONE ring per level of group_by; each ring is sliced by the
# active identity (final_annotation), so the slices show cell-type fractions
# inside that condition. (With no group_by, every ring would contain only one
# category and the plot would look like a stack of solid bands, not a donut.)
p_ring <- CellStatPlot(
  seurat_obj,
  group_by  = "condition",
  plot_type = "ring",
  palette   = "Spectral"
)

# Sankey diagram from condition to cell type, useful for flow-style figures.
# ggalluvial requires two columns in 'group_by' to form the alluvial flow,
# so we pass both 'condition' and 'final_annotation' here. You will notice
# that the output has an extra "Identity" legend whose categories duplicate
# the cell-type colors with a different palette. This is a cosmetic side
# effect of how plotthis layers multiple fill scales (the ribbon color is
# tied to the active identity, while the right bar is colored independently
# by final_annotation); the diagram itself is correct, and the duplicate
# legend can be ignored or cropped out when including the figure in a report.
p_sankey <- CellStatPlot(
  seurat_obj,
  group_by  = c("condition", "final_annotation"),
  plot_type = "sankey"
)

ggsave("plots/14_composition_ring.png",   p_ring,   width = 7, height = 6, dpi = 300)
ggsave("plots/15_composition_sankey.png", p_sankey, width = 9, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/14_15_cell_proportion-e1780190872905-1024x408.png?resize=1024%2C408&ssl=1)

> 

**Reading the ring chart:** Two concentric rings — **outer = Healthy, inner = Post_Treatment** — each sliced into wedges by cell type. The sankey diagram on the right (`plot_type = "sankey"`) carries the same information flowing from the condition bar on the left to the cell-type bar on the right; ribbon thickness encodes the count.

### Marker and Differential Expression Plots (Recreating Parts 4 and 5)

This section covers two related but distinct questions. **[Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/)** asked *what defines each cell type* (cell-type markers) — we visualize that with a jitter plot. **[Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/)** asked *what changes between Healthy and Post-Treatment inside each cell type* (condition DE) — we visualize that with per-cell-type volcanoes. Both rely on the same scplotter function, `MarkersPlot`, which takes a `FindMarkers`/`FindAllMarkers` data frame and draws several styles with automatic gene labeling.

```r
#-----------------------------------------------
# STEP 18: Find cell-type markers (same call used in Part 4)
#-----------------------------------------------

celltype_markers <- FindAllMarkers(
  seurat_obj,
  only.pos            = TRUE,
  min.pct             = 0.25,
  logfc.threshold     = 0.25,
  max.cells.per.ident = 500     # downsample for memory and speed
)

# FindAllMarkers always names its grouping column "cluster", even though our
# groups are cell types. Rename it to "cell_type" so the plots read correctly.
celltype_markers <- dplyr::rename(celltype_markers, cell_type = cluster)
```

> 

**Why `max.cells.per.ident`?** Wilcoxon-based `FindAllMarkers` builds dense per-comparison matrices over thousands of genes and tens of thousands of cells, which can exceed memory on a large object even after the scaled layer is dropped. Capping each cell type at 500 cells turns those matrices into a few-hundred-by-eighteen-thousand size — fast and memory-light — without sacrificing marker quality. For an extra speed boost, install the `presto` package (`remotes::install_github("immunogenomics/presto")`); Seurat picks it up automatically and runs the Wilcoxon test in a sparse-aware C++ implementation.

#### A Jitter Plot of Marker Strength

A jitter plot is a clean, attractive way to see every cell type’s markers at a glance: each point is a gene, its height is the log2 fold-change, and its size reflects significance. It avoids the dense, hard-to-read look of many small volcano panels.

```r
#-----------------------------------------------
# STEP 19: Jitter plot of markers per cell type (a clear, attractive overview)
#-----------------------------------------------

# x-axis = cell types, y-axis = log2FC, point size = significance.
# 'select' labels the top markers in each cell type.
p_jitter <- MarkersPlot(
  celltype_markers,
  subset_by = "cell_type",
  plot_type = "jitter",
  select    = 5
)

ggsave("plots/16_markers_jitter.png", p_jitter, width = 12, height = 7, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/16_markers_jitter.png?resize=1024%2C597&ssl=1)

> 

**Reading the jitter plot:** Each column is a cell type, every point is one marker gene; y-axis is log2 fold-change (higher = more enriched in that cell type) and point size is statistical significance.

#### Condition DE Volcano Plots (Recreating Part 5)

The [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) question was “what defines each cell type” (the jitter above). The [Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/) question is different and more clinically interesting: **within each cell type, which genes change between Healthy and Post-Treatment?** Below, we run that condition-vs-condition DE inside every cell type and then volcano the results in a single `MarkersPlot` call.

```r
#-----------------------------------------------
# STEP 20: Condition DE within each cell type (Healthy vs Post-Treatment)
#-----------------------------------------------

# For every cell type, compare Post-Treatment vs Healthy cells of THAT type.
# We use lapply (functional style) instead of a for-loop, downsample with
# max.cells.per.ident to control memory, and set logfc.threshold = 0 so the
# volcano shows the full distribution (not just strong effects).
cell_types_present <- as.character(unique(seurat_obj$final_annotation))

condition_de <- do.call(rbind, lapply(cell_types_present, function(ct) {
  de <- tryCatch(
    FindMarkers(
      seurat_obj,
      ident.1             = "Post_Treatment",   # positive log2FC = up after treatment
      ident.2             = "Healthy",          # negative log2FC = up in Healthy
      group.by            = "condition",
      subset.ident        = ct,                 # restrict to one cell type at a time
      min.pct             = 0.1,
      logfc.threshold     = 0,                  # keep all genes for a complete volcano
      max.cells.per.ident = 500
    ),
    error = function(e) NULL                    # skip cell types with too few cells
  )
  if (is.null(de) || nrow(de) == 0) return(NULL)
  de$gene      <- rownames(de)
  de$cell_type <- ct
  de
}))

# One volcano per cell type, top genes labeled automatically
p_volcano <- MarkersPlot(
  condition_de,
  subset_by       = "cell_type",
  subset_as_facet = TRUE,        # arrange the per-cell-type panels as facets
  plot_type       = "volcano",
  select          = 5,           # label the 5 strongest DEGs per panel
  ncol            = 4
)

ggsave("plots/17_condition_de_volcano.png", p_volcano, width = 16, height = 10, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/17_condition_de_volcano-scaled-e1780190939249-1024x936.png?resize=1024%2C936&ssl=1)

> 

**Reading the volcanoes:** One panel per cell type; x-axis is log2 fold-change (Post-Treatment over Healthy, so **right = up after treatment, left = up in Healthy**), y-axis is `-log10` adjusted p-value, and point color (`pct_diff`) shows how much more frequently the gene is detected in one condition.

### Cell-Cell Communication (Recreating Part 9)

[Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/) inferred cell-cell communication with CellChat and visualized it with CellChat’s own functions. scplotter’s `CCCPlot` offers an alternative set of communication figures — network, chord, heatmap, dot, and Sankey — drawn from a tidy, long-format results table.

`CCCPlot` does not read a CellChat object directly. It expects a data frame with one row per ligand-receptor interaction and, at minimum, the columns `source`, `target`, `ligand`, `receptor`, plus a numeric **magnitude** column for interaction strength and (optionally) a **specificity** column for significance. CellChat’s `subsetCommunication()` returns exactly that, with the strength under `prob` and the p-value under `pval`. We pass those column names to `CCCPlot` explicitly so its auto-detection does not latch on to a non-numeric column like CellChat’s `evidence` field.

Below we load the two CellChat objects you saved in [Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/) — `cellchat_Healthy.rds` and `cellchat_Post_Treatment.rds` — and plot Healthy versus Post-Treatment side by side.

```r
#-----------------------------------------------
# STEP 21: Cell-cell communication plots from your Part 9 CellChat results
#-----------------------------------------------

# CellChat and patchwork were attached in Step 2.

# Load the two CellChat objects produced in Part 9 (adjust the paths if needed)
cc_healthy <- readRDS("~/GSE174609_scRNA/cellchat/cellchat_Healthy.rds")
cc_post    <- readRDS("~/GSE174609_scRNA/cellchat/cellchat_Post_Treatment.rds")

# Extract inferred communications as long-format data frames.
# subsetCommunication() returns source, target, ligand, receptor, prob, pval,
# interaction_name, pathway_name, ... (one row per significant LR interaction).
df_healthy <- CellChat::subsetCommunication(cc_healthy)
df_post    <- CellChat::subsetCommunication(cc_post)

# CCCPlot's defaults auto-scan the data frame for the strength and significance
# columns, which can latch on to non-numeric columns like CellChat's 'evidence'
# (a references field). Pass them explicitly: CellChat names interaction
# probability 'prob' and significance 'pval'.

# Build the three views for each condition, then stitch with patchwork.
# Network (nodes = cell types, edges = aggregated interactions)
p_net_h <- CCCPlot(df_healthy, plot_type = "network",
                   magnitude = "prob", specificity = "pval") + ggtitle("Healthy")
p_net_p <- CCCPlot(df_post,    plot_type = "network",
                   magnitude = "prob", specificity = "pval") + ggtitle("Post-Treatment")
p_ccc_network <- p_net_h | p_net_p

# Chord (the circular diagram familiar from CellChat)
p_chord_h <- CCCPlot(df_healthy, plot_type = "chord",
                     magnitude = "prob", specificity = "pval") + ggtitle("Healthy")
p_chord_p <- CCCPlot(df_post,    plot_type = "chord",
                     magnitude = "prob", specificity = "pval") + ggtitle("Post-Treatment")
p_ccc_chord <- p_chord_h | p_chord_p

ggsave("plots/18_ccc_network.png", p_ccc_network, width = 14, height = 7, dpi = 300)
ggsave("plots/19_ccc_chord.png",   p_ccc_chord,   width = 14, height = 8, dpi = 300)

# Heatmap view (senders as rows, receivers as columns). These are
# ComplexHeatmap objects rather than ggplots, so we save each condition
# to its own PNG with a graphics device (patchwork cannot combine them).
p_heat_h <- CCCPlot(df_healthy, plot_type = "heatmap",
                    magnitude = "prob", specificity = "pval")
p_heat_p <- CCCPlot(df_post,    plot_type = "heatmap",
                    magnitude = "prob", specificity = "pval")

png("plots/20a_ccc_heatmap_healthy.png", width = 8, height = 6, units = "in", res = 300)
print(p_heat_h)
dev.off()

png("plots/20b_ccc_heatmap_post_treatment.png", width = 8, height = 6, units = "in", res = 300)
print(p_heat_p)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/18_19_20ab_ccc-e1780191221461-1024x592.png?resize=1024%2C592&ssl=1)

> 

**Reading the CCC comparison:** All three views agree on the same biology.

- **Network (plot 18):** Healthy has a clearly denser network — the thickest single edge in Healthy is ~40 interactions. In Post-Treatment the maximum edge weight drops to ~30, signaling reduced overall cell-cell signaling after treatment.

- **Chord (plot 19):** The same pattern in circular form. The axis scales help quantify the drop: Classical Monocytes’ interaction count goes from a peak around 350 in Healthy to ~280 in Post-Treatment.

- **Heatmaps (plots 20a/20b):** Rows are senders, columns are receivers, color is the interaction count.

> 

**What if your column names differ?** If `subsetCommunication()` in your CellChat version returns the strength under a different name (some older builds use `lr.value` or `lr.mean`), substitute it on the right-hand side of `magnitude = "..."` — and similarly for `specificity = "..."`. The other four required columns (`source`, `target`, `ligand`, `receptor`) are stable across CellChat versions.

### Choosing a Clustering Resolution with a Cluster Tree (Extending Part 3)

In [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) you picked a clustering resolution, which controls how finely the cells are split. A **cluster tree** (clustree) shows how clusters split and merge as the resolution increases, which helps you choose a stable value: a good resolution is one where clusters stop reshuffling and the tree branches cleanly rather than crossing back and forth.

`ClustreePlot` reads the clustering-resolution columns in the metadata, identified by their shared `prefix`. Our object already contains several — `clusters_res_0.4`, `clusters_res_0.6`, `clusters_res_0.8`, `clusters_res_1`, and `clusters_res_1.2` — so their shared prefix is `clusters_res_`, and `ClustreePlot` uses them directly.

```r
#-----------------------------------------------
# STEP 22: Cluster tree across resolutions (helps choose a stable resolution)
#-----------------------------------------------

# The object already stores clusters at several resolutions named
# clusters_res_0.4, clusters_res_0.6, ... so the shared prefix is "clusters_res_".
# (To confirm the names in any object: grep("_res", colnames(seurat_obj@meta.data), value = TRUE))
p_clustree <- ClustreePlot(seurat_obj, prefix = "clusters_res_")

ggsave("plots/21_clustree.png", p_clustree, width = 9, height = 8, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/21_clustree.png?resize=1024%2C910&ssl=1)

> 

**No multiple resolutions yet?** If your object has only one clustering, generate a few first by reusing the neighbor graph from Part 3 — `seurat_obj <- FindClusters(seurat_obj, resolution = c(0.4, 0.6, 0.8, 1.0, 1.2))` — then set `prefix` to match the new column names (Seurat’s default is `RNA_snn_res.`). `FindClusters` resets the active identity, so run `Idents(seurat_obj) <- "final_annotation"` afterward.

> 

**Reading the cluster tree:** Each horizontal row is one clustering resolution — 0.4 at the top through 1.2 at the bottom — and each node is a cluster at that resolution, labeled with its cluster number. Three visual encodings tell you what is happening between rows:

- **Node size** (size legend) — the number of cells in that cluster. Large circles are big populations; small circles are rare ones.

- **Arrow color** (count legend) — the number of cells flowing along that edge from one row to the next. Bright red arrows carry many cells; dark blue arrows carry few.

- **Arrow transparency** (in_prop legend) — the proportion of the destination cluster contributed by this particular source. A fully opaque arrow means the destination came almost entirely from this one parent; faint arrows are minor contributions.

The tree reads downward. A stable subdivision looks like vertical, mostly-opaque arrows — a cluster splits cleanly into a few children that inherit most of its cells. Instability looks like faint criss-crossing arrows, where cells flow into far-away clusters and destinations are stitched together from many small contributions. Choose the deepest resolution that still looks clean. In this tree the structure stays orderly down to roughly resolution 0.8-1.0; the long faint diagonals appearing between 1.0 and 1.2 are the signal that the data is starting to be over-split.

### RNA Velocity Streams (Extending Part 14)

In [Part 14](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/) you used CellRank to compute velocity-driven cell-fate probabilities on the scVelo endocrinogenesis dataset – `endocrinogenesis_day15.5_preprocessed-kernel.h5ad`. scplotter reads `.h5ad` files directly, so `CellVelocityPlot` can draw the velocity stream from that file in one R call.

`CellVelocityPlot` needs the velocity vectors **projected onto an embedding** (typically UMAP), stored in `adata.obsm["velocity_umap"]`. CellRank’s velocity kernel does not require that projected layer, so the Part 14 file may not contain it. Step 23a is a one-time Python check that adds the projection if it is missing; Step 23b then plots in R.

#### Step 23a: Ensure the UMAP velocity embedding exists (Python, one-time)

> 

**Activate your Part 13 velocity environment first.** In Part 13 you created a conda env with scVelo (`scvelo`, `scanpy`, `anndata`, …). Activate it before running the Python below — e.g., `conda activate velocity` (or whatever you named it in [Part 13](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/)). The Part 14 env works as well if it has scVelo installed.

`CellVelocityPlot` needs the velocity vectors projected onto UMAP in `adata.obsm["velocity_umap"]`. Computing that projection requires the cosine-correlation matrix from `scv.tl.velocity_graph()`, which CellRank’s kernel does not need and therefore is usually missing from a Part 14 file. The snippet below adds both pieces if they are absent, and then writes a slim “plot-only” copy of the AnnData containing only what scplotter needs: the UMAP coordinates, the velocity-UMAP projection, and the cluster labels. The slim copy is important because the original Part 14 file also contains CellRank-specific structures (lineage memberships, the transition matrix, kernel parameters) that can confuse scplotter’s h5ad reader when it builds a per-cell data frame for `CellDimPlot`.

```python
# --- Run once in Python, inside your Part 13 'velocity' env ---
import anndata as ad
import scvelo as scv

# Source: your Part 14 file (kept intact; we only read it)
src_path = "~/cellrank/endocrinogenesis_day15.5_preprocessed-kernel.h5ad"
# Destination: a slim plot-only file written next to it
dst_path = "~/cellrank/endocrinogenesis_for_scplotter.h5ad"

adata = ad.read_h5ad(src_path)

# velocity_embedding() needs the velocity graph (cosine correlations);
# CellRank's kernel does not require it, so add it if missing.
if "velocity_graph" not in adata.uns:
    scv.tl.velocity_graph(adata)
if "velocity_umap" not in adata.obsm:
    scv.tl.velocity_embedding(adata, basis="umap")

# Write a stripped AnnData with only what scplotter needs to plot.
# This avoids reader issues with CellRank-specific objects in uns/obsp/obs.
adata_min = ad.AnnData(
    X    = adata.X,
    obs  = adata.obs[["clusters"]].copy(),
    obsm = {"X_umap":        adata.obsm["X_umap"],
            "velocity_umap": adata.obsm["velocity_umap"]},
)
adata_min.write(dst_path)
```

#### Step 23b: Draw the velocity stream (R)

```r
#-----------------------------------------------
# STEP 23: RNA velocity stream plot from your Part 14 .h5ad file
#-----------------------------------------------

# Path to the slim plot-only file written in Step 23a (adjust if you saved it elsewhere)
h5ad_path <- "~/cellrank/endocrinogenesis_for_scplotter.h5ad"

# Panel 1: Cell positions colored by cluster (context for the velocity panels).
p_cells <- CellDimPlot(
  h5ad_path,
  reduction = "umap",
  group_by  = "clusters",
  label     = TRUE
)

# Panel 2: Per-cell velocity arrows, colored by cluster.
# The default plot type draws one arrow per cell; group_by adds cluster colors.
p_arrows <- CellVelocityPlot(
  h5ad_path,
  reduction   = "umap",
  v_reduction = "velocity_umap",
  group_by    = "clusters"
)

# Panel 3: Grid-smoothed velocity arrows.
# 'grid' bins cells into a regular grid and draws one averaged arrow per bin,
# de-noising the per-cell vectors so the global direction is easier to see.
p_grid <- CellVelocityPlot(
  h5ad_path,
  reduction   = "umap",
  v_reduction = "velocity_umap",
  plot_type   = "grid"
)

# Panel 4: Smooth velocity streamlines (the most readable summary view).
# 'stream' shows the velocity field as continuous flow lines colored by speed.
p_stream <- CellVelocityPlot(
  h5ad_path,
  reduction   = "umap",
  v_reduction = "velocity_umap",
  plot_type   = "stream"
)

# 2x2 layout: top row = cells | per-cell arrows; bottom row = grid | stream.
# Each panel becomes more abstract from top-left to bottom-right.
p_velocity <- (p_cells | p_arrows) / (p_grid | p_stream)

ggsave("plots/22_velocity_stream.png", p_velocity, width = 16, height = 14, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/22_velocity_stream.png?resize=1024%2C896&ssl=1)

> 

**Reading the four panels:** They show the same velocity field at increasing levels of abstraction. The dataset is the **scVelo endocrinogenesis day 15.5 dataset (N = 2,531 cells, 7 clusters)**: Ngn3 low EP (138), Ngn3 high EP (529), Fev+ (587), Beta (591), Alpha (477), Delta (70), Epsilon (139).

- **Top-left (cells)** — where each cluster sits in UMAP space. The Ngn3-low / Ngn3-high precursors form the top-right “tail,” Fev+ sits in the middle, and the four terminal endocrine fates (Beta, Alpha, Delta, Epsilon) spread out to the lower-left and bottom.

- **Top-right (per-cell arrows)** — one arrow per cell, colored by cluster. Arrows in the Ngn3-high region point downward and leftward toward Fev+ and the terminal fates; arrows inside the Beta, Alpha, Delta, and Epsilon clusters mostly point inward (terminal states with small velocity).

- **Bottom-left (grid arrows)** — arrows averaged over a regular grid (black). Noise from individual cells is smoothed out, and the dominant flow from the upper Ngn3 region downward and leftward into the terminal fates becomes unmistakable.

- **Bottom-right (streamlines)** — continuous flow lines colored by velocity magnitude (0.05 = slow blue, 0.15 = fast red). The hot red bands trace the **Ngn3-high → Fev+ → terminal-fate trajectory**, with the strongest flow in the middle of the differentiation trajectory and slower flow in the terminal endocrine clusters (where cells have committed and slowed down).

Read the four together: the **streamlines converge** into the Beta, Alpha, Delta, and Epsilon regions of the top-left cluster plot (terminal sinks) and **diverge** from the Ngn3-high region (the source). Cross-checking the streams against your Part 14 fate probabilities is a useful sanity check — cells with high Alpha probability should sit in regions flowing toward the Alpha sink, and the same for Beta, Delta, and Epsilon.

> 

**Reduction names not matching?** If `reduction = "umap"` or `v_reduction = "velocity_umap"` errors, the keys in your file may use slightly different names. Inspect them in Python with `print(list(adata.obsm.keys()))` — common variants are `X_umap`/`velocity_umap` or `X_pca`/`velocity_pca`. Use whichever names appear there.

> 

**Don’t have the Part 14 file handy?** scplotter ships the `pancreas_sub` dataset, which already contains a velocity embedding, for practice: `data(pancreas_sub); CellVelocityPlot(pancreas_sub, reduction = "PCA", v_reduction = "stochastic_PCA", group_by = "SubCellType", plot_type = "stream")`.

---

## Best Practices and Common Pitfalls

### Best Practices for Visualization with scplotter

**1. Keep the analysis in Seurat; keep the drawing in scplotter.** scplotter changes how results are visualized, not what they are. Run statistics, clustering, and modeling with the appropriate tools, then hand the finished object to scplotter purely for figures.

**2. Learn the shared grammar once.** The arguments `group_by`, `split_by`, `facet_by`, `palette`, and `theme` behave the same way across functions. The difference between a cluster map and a sample map is one word in `group_by`; the difference between a violin and a dot plot is one word in `plot_type`.

**3. Prefer built-in palettes over manual hex codes.** Pass `palette = "Set1"`, `"Spectral"`, `"Blues"`, and so on. This keeps colors consistent across every panel in a figure and avoids the long manual color vectors used earlier in the series.

**4. Let one call handle many features.** Passing a vector of genes to `FeatureStatPlot` produces a multi-panel figure in one call. This replaces the `for` loops and `patchwork` assembly used in Parts 4 and 5, and it keeps your code short and declarative.

**5. Match the save method to the object.** Most scplotter functions return `ggplot` objects that `ggsave(..., dpi = 300)` writes directly. The dot-plot and heatmap styles return ComplexHeatmap objects instead, which you save with a graphics device: `png(...); print(plot); dev.off()`. This mirrors the convention used across this series. Use 300 DPI for publication figures.

**6. Use `frac` deliberately in composition plots.** `frac = "group"` answers “what is the composition within each condition?”, while `frac = "none"` shows raw counts. Mismatching these is the most common source of misleading composition figures.

**7. Subset genes before dot plots and heatmaps on large objects.** These two plot types build a matrix from the object. On a dataset with tens of thousands of cells — especially one carrying a dense all-gene scaled layer from `ScaleData()` — that can exhaust memory. Subsetting to just the genes you are plotting with `subset(obj, features = genes)` keeps every cell but makes the computation trivially small.

### Common Pitfalls and How to Avoid Them

**Pitfall 1: Wrong reduction name.** A plot that comes out empty or errors on `reduction` usually means the name does not exist in your object. Check with `names(seurat_obj@reductions)` — integrated UMAPs are typically named `umap.harmony`, `umap.cca`, and so on, not plain `umap`. The detection step in Step 4 handles this for you.

**Pitfall 2: Feature not found.** If a gene appears blank, confirm it exists with `gene %in% rownames(seurat_obj)`. Gene symbols are case-sensitive (`CD3D`, not `cd3d`), and some genes use aliases that differ from your reference.

**Pitfall 3: Reading proportions as counts.** Stacked composition bars with `frac = "group"` show relative proportions. An apparent increase in one cell type may simply reflect a decrease in another. Always cross-check with the raw-count plot.

**Pitfall 4: Feeding a CellChat object to `CCCPlot`.** `CCCPlot` needs a long-format data frame, not a CellChat object. Reshape your results into `source`/`target`/`ligand`/`receptor`/magnitude columns first (see the note in Step 21).

**Pitfall 5: Expecting scplotter to recompute statistics.** scplotter visualizes existing results. It will not rerun differential expression or re-cluster your data. Generate results with the analysis tools first, then visualize.

---

## Key Takeaways

- scplotter is a visualization layer, not an analysis tool: the science still comes from Seurat, DESeq2, CellChat, and the rest of the series.

- Three workhorse functions — `CellDimPlot`, `FeatureStatPlot`, and `CellStatPlot` — cover the large majority of single-cell figures, with specialists like `MarkersPlot`, `CCCPlot`, `ClustreePlot`, and `CellVelocityPlot` for specific analyses.

- A shared argument grammar (`group_by`, `split_by`, `facet_by`, `palette`, `plot_type`) means learning one function teaches you the others.

- Passing multiple features or groups to one call replaces the loops and `patchwork` assembly used earlier, producing shorter and more readable code.

- Most functions return a `ggplot` object that `ggsave()` writes directly; the ComplexHeatmap-based dot and heatmap styles are the exception and use a `png()` device.

---

## References

1. Wang P. scplotter: Visualize single-cell sequencing and spatial data. R package. [https://github.com/pwwang/scplotter](https://github.com/pwwang/scplotter)

2. Wang P. plotthis: A comprehensive plotting library built upon ggplot2. R package. [https://github.com/pwwang/plotthis](https://github.com/pwwang/plotthis)

3. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

4. Wickham H. ggplot2: Elegant Graphics for Data Analysis. Springer-Verlag New York; 2016. [https://ggplot2.tidyverse.org](https://ggplot2.tidyverse.org) [Underlying grammar of graphics]

5. Dimitrov D, Turei D, Garrido-Rodriguez M, et al. Comparison of methods and resources for cell-cell communication inference from single-cell RNA-seq data. *Nature Communications*. 2022;13(1):3224. doi:10.1038/s41467-022-30755-0 [LIANA / long-format CCC results]

6. Jin S, Plikus MV, Nie Q. CellChat for systematic analysis of cell-cell communication from single-cell transcriptomics. *Nature Protocols*. 2025;20(1):180-219. doi:10.1038/s41596-024-01045-4 [CellChat — Part 9]

7. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. *Journal of Translational Medicine*. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source — GSE174609]

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
