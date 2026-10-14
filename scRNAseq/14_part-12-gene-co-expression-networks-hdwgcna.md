# How to Analyze Single-Cell RNA-seq Data – Complete Beginner's Guide Part 12: Build Gene Co-expression Networks Using hdWGCNA

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/)

---

*Discover how genes work together inside specific cell types — and how those cooperative programs shift between disease and treatment*

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/)

- [Introduction: Gene Co-expression Networks in Single-Cell Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Introduction_Gene_Co-expression_Networks_in_Single-Cell_Data)

  - [What We Have Learned So Far](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#What_We_Have_Learned_So_Far)
  - [A Quick Review of WGCNA](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#A_Quick_Review_of_WGCNA)
  - [Why Classic WGCNA Struggles with Single-Cell Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Why_Classic_WGCNA_Struggles_with_Single-Cell_Data)
  - [What Is hdWGCNA and How Does It Solve These Problems?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#What_Is_hdWGCNA_and_How_Does_It_Solve_These_Problems)
  - [WGCNA vs hdWGCNA: A Side-by-Side Comparison](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#WGCNA_vs_hdWGCNA_A_Side-by-Side_Comparison)
  - [What Biological Questions Can hdWGCNA Answer?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#What_Biological_Questions_Can_hdWGCNA_Answer)
  - [The hdWGCNA Workflow at a Glance](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#The_hdWGCNA_Workflow_at_a_Glance)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Setting_Up_Your_Analysis_Environment)

  - [Installing hdWGCNA and Its Dependencies](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Installing_hdWGCNA_and_Its_Dependencies)

- [Example Dataset: PBMC Periodontitis scRNA-seq (GSE174609)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Example_Dataset_PBMC_Periodontitis_scRNA-seq_GSE174609)
- [Step 1: Load Libraries and Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_1_Load_Libraries_and_Data)
- [Step 2: Initialize hdWGCNA with SetupForWGCNA](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_2_Initialize_hdWGCNA_with_SetupForWGCNA)
- [Step 3: Construct Metacells](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_3_Construct_Metacells)
- [Step 4: Set the Expression Data for Network Construction](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_4_Set_the_Expression_Data_for_Network_Construction)
- [Step 5: Select the Soft-Thresholding Power](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_5_Select_the_Soft-Thresholding_Power)
- [Step 6: Construct the Co-expression Network](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_6_Construct_the_Co-expression_Network)
- [Step 7: Compute Module Eigengenes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_7_Compute_Module_Eigengenes)
- [Step 8: Compute Intramodular Connectivity (kME)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_8_Compute_Intramodular_Connectivity_kME)

  - [Visualize kME Score Distributions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Visualize_kME_Score_Distributions)

- [Step 9: Visualize Module Activity on the UMAP](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_9_Visualize_Module_Activity_on_the_UMAP)

  - [Plot Harmonized Module Eigengenes on Cells](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Plot_Harmonized_Module_Eigengenes_on_Cells)
  - [Module Correlation Heatmap](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Module_Correlation_Heatmap)

- [Step 10: Compute and Visualize Single-Cell Module Scores](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_10_Compute_and_Visualize_Single-Cell_Module_Scores)
- [Step 11: Functional Enrichment Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_11_Functional_Enrichment_Analysis)
- [Step 12: Differential Module Eigengene (DME) Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_12_Differential_Module_Eigengene_DME_Analysis)
- [Step 13: Visualize the Gene Co-expression Network](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_13_Visualize_the_Gene_Co-expression_Network)

  - [Hub Gene Network Plot](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Hub_Gene_Network_Plot)
  - [Gene Co-expression Network UMAP](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Gene_Co-expression_Network_UMAP)

- [Step 14: Save Your Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Step_14_Save_Your_Results)
- [Best Practices, Common Pitfalls, and Troubleshooting](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Best_Practices_Common_Pitfalls_and_Troubleshooting)

  - [Best Practices](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Best_Practices)
  - [Common Pitfalls](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Common_Pitfalls)

- [Conclusion](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Conclusion)

  - [What Sets hdWGCNA Apart](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#What_Sets_hdWGCNA_Apart)
  - [Where to Go from Here](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Where_to_Go_from_Here)

- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-12-build-gene-co-expression-networks-using-hdwgcna/#Like_this)

## Introduction: Gene Co-expression Networks in Single-Cell Data

### What We Have Learned So Far

If you have followed [Parts 1-11](https://ngs101.com/tutorials/#single-cell-seq) of this tutorial series, you have built a comprehensive picture of the PBMC periodontitis dataset (GSE174609):

You now know who the cells are, what genes they express, how they change across conditions, how they communicate, and whether they have genomic alterations. But there is one more dimension to explore: **how genes collaborate inside a specific cell type to execute coordinated biological programs**.

This is the question that gene co-expression network analysis answers.

---

### A Quick Review of WGCNA

In a [previous tutorial on bulk RNA-seq analysis](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/), we introduced **WGCNA (Weighted Gene Co-expression Network Analysis)** as a method for identifying groups of genes that are expressed together across samples. The core idea is simple: if two genes go up and down in perfect synchrony across your samples, they are likely co-regulated and potentially part of the same biological pathway or regulatory program.

WGCNA works by:

1. Computing pairwise correlations between all genes across samples

2. Transforming those correlations into a weighted adjacency matrix using a soft-thresholding power

3. Grouping genes with similar connectivity patterns into **modules**

4. Summarizing each module’s overall activity with a **module eigengene** (the first principal component of gene expression within the module)

5. Associating module eigengenes with clinical or experimental traits to find biologically meaningful modules

WGCNA is a powerful and widely used tool for bulk RNA-seq data. However, when applied to single-cell RNA-seq data, it runs into a fundamental problem.

---

### Why Classic WGCNA Struggles with Single-Cell Data

In bulk RNA-seq, you have tens to hundreds of samples, each representing a mixture of many cells. Gene correlations are computed across those samples, and the averaging effect of mixing many cells makes the data relatively smooth.

In single-cell RNA-seq, the picture is very different:

**Problem 1: Dropout**

Single-cell RNA-seq captures only a fraction of the mRNA molecules in each cell. A gene that is truly expressed can appear as zero simply because its transcripts were not captured during sequencing. This is called **dropout**, and it is pervasive — often 80-90% of a scRNA-seq count matrix consists of zeros. Standard WGCNA correlations are destroyed by this sparsity: two genes that are truly co-expressed will appear uncorrelated because their expression values are dominated by random zeros.

**Problem 2: Cell-Type Mixing**

If you run WGCNA on the full scRNA-seq dataset treating each cell as a “sample,” the correlations you compute reflect differences between cell types, not meaningful co-regulation within cell types. A module might be “expressed” simply because it contains cell-type marker genes, not because those genes are functionally linked inside a specific cell type.

**Problem 3: Treating Cells as Independent Samples**

Cells from the same sample (same donor) are not independent observations. Treating them as independent inflates your effective sample size and produces artificially confident but biologically misleading correlations.

---

### What Is hdWGCNA and How Does It Solve These Problems?

**hdWGCNA (high-dimensional WGCNA)** is an R package developed by Morabito et al. that adapts WGCNA for single-cell and spatial transcriptomics data. It solves all three problems above through a strategy called **metacell aggregation**.

**The metacell concept:**

Instead of treating each individual cell as a sample, hdWGCNA groups cells that are highly similar to each other — their nearest neighbors in the dimensionality reduction space — from the same sample, and averages their gene expression into a single “metacell.” Each metacell is an aggregate of ~20-25 individual cells.

This approach:

- **Eliminates dropout effects**: Averaging many cells fills in the random zeros. If a gene is truly expressed, most of its neighboring cells will have captured at least a few transcripts, and the average will be non-zero.

- **Preserves biological heterogeneity**: Metacells are built from locally similar cells, so the diversity of cell states within a cell type is preserved across many metacells.

- **Respects sample boundaries**: Metacells are never built across samples — all cells contributing to a metacell must come from the same biological replicate.

- **Maintains cell-type specificity**: You run hdWGCNA separately for each cell type of interest, ensuring that co-expression patterns reflect biology within that cell type.

After constructing metacells, hdWGCNA runs the familiar WGCNA pipeline — soft power selection, network construction, module detection — on the metacell expression matrix. Module eigengenes are then projected back onto individual cells for visualization and downstream analysis.

---

### WGCNA vs hdWGCNA: A Side-by-Side Comparison

**Designed for****Input unit****Sparsity handling****Cell-type specificity****Seurat integration****Module eigengenes****Comparative analysis**

| Feature | WGCNA | hdWGCNA |
| --- | --- | --- |
|  | Bulk RNA-seq (samples as observations) | Single-cell RNA-seq (cells as observations) |
|  | Samples / conditions | Cells aggregated into metacells |
|  | Not designed for dropout-heavy data | Metacell aggregation reduces sparsity |
|  | Averages over all cell types simultaneously | Analyzes one cell type at a time |
|  | Requires manual data extraction | Native Seurat object support throughout |
|  | Summarized at sample level | Computed at single-cell level; Harmony-corrected across donors |
|  | Correlate eigengenes with traits | Differential module eigengene (DME) analysis between conditions |

---

### What Biological Questions Can hdWGCNA Answer?

Running hdWGCNA on the periodontitis PBMC dataset can answer questions like:

**1. What gene programs define monocyte biology in this dataset?**

Are there distinct modules for inflammatory signaling, antigen presentation, metabolic function, and cytoskeletal remodeling? Do these modules correspond to known monocyte functional states?

**2. Which modules are dysregulated in disease or restored by treatment?**

Differential module eigengene (DME) analysis can identify modules whose activity is specifically higher or lower in Healthy cells compared to Post_Treatment cells — revealing which gene programs are shifted by periodontal therapy.

**3. Which are the most connected “hub genes” within each module?**

Hub genes have the highest intramodular connectivity (kME) and are the most likely candidates for regulatory drivers of the module. They are prime targets for experimental validation.

**4. What pathways and biological processes are enriched in each module?**

Gene ontology and pathway enrichment analysis of module members reveals the biological functions of each co-expression program.

---

### The hdWGCNA Workflow at a Glance

The full hdWGCNA workflow we will implement in this tutorial follows these steps:

1. Initialize hdWGCNA on a pre-processed Seurat object

2. Build metacells by aggregating nearest-neighbor cells per sample

3. Select the expression matrix for a specific cell type

4. Test soft-thresholding powers and choose the best one

5. Construct the weighted co-expression network

6. Identify co-expression modules by hierarchical clustering

7. Compute and harmonize module eigengenes

8. Calculate intramodular connectivity to identify hub genes

9. Visualize modules on the single-cell UMAP

10. Perform gene ontology enrichment for each module

11. Run differential module eigengene (DME) analysis between conditions

12. Visualize the gene co-expression network

---

## Setting Up Your Analysis Environment

> 

**Tested environment:** The code in this tutorial is written for R 4.3 or higher. hdWGCNA is actively developed, so we recommend using the latest version from GitHub. The WGCNA package (a Bioconductor dependency) requires R >= 3.6. All steps were tested and validated on Linux.

### Installing hdWGCNA and Its Dependencies

hdWGCNA is installed from GitHub and depends on several packages. Install them in order:

```r
# Step 1: Install BiocManager for Bioconductor packages
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

# Step 2: Install Bioconductor dependencies
BiocManager::install("WGCNA")       # Core network analysis engine
BiocManager::install("UCell")       # Single-cell module scoring
BiocManager::install("GeneOverlap") # Required for module overlap statistics

# Step 3: Install CRAN dependencies
install.packages(c("tidyverse", "cowplot", "patchwork", "igraph", "harmony"))

# Step 4: Install devtools if needed
if (!requireNamespace("devtools", quietly = TRUE))
  install.packages("devtools")

# Step 5: Install enrichR from GitHub (the CRAN version is outdated)
devtools::install_github("wjawaid/enrichR")

# Step 6: Install hdWGCNA from the dev branch on GitHub
devtools::install_github("smorabit/hdWGCNA", ref = "dev")
```

---

## Example Dataset: PBMC Periodontitis scRNA-seq (GSE174609)

Throughout this series, we have been working with GSE174609, a human PBMC dataset from three conditions:

- **Healthy**: Disease-free donors

- **Pre_Treatment**: Patients with active periodontitis

- **Post_Treatment**: Patients after non-surgical periodontal therapy

For this tutorial we will use the **same downsampled Seurat object** prepared in [Part 7-2 (Slingshot trajectory analysis)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/). If you have not completed that tutorial, please follow it first to generate `seurat_subset.rds`. That object contains:

- Annotated cell types in the `final_annotation` metadata column

- Condition labels in the `condition` column (`"Healthy"` or `"Post_Treatment"`)

- Sample IDs in the `orig.ident` column

- CCA-integrated dimensionality reduction in the `integrated.cca` reduction slot

We will focus our hdWGCNA analysis on **Classical Monocytes** as a demonstration. Classical Monocytes are a well-characterized innate immune population with known roles in periodontitis, making them an interpretable choice for a first co-expression network analysis. The same workflow can be repeated for any other cell type in the dataset.

---

## Step 1: Load Libraries and Data

```r
library(hdWGCNA)
library(WGCNA)
library(Seurat)
library(tidyverse)
library(cowplot)
library(patchwork)
library(igraph)
library(enrichR)

# Enable multi-threading for WGCNA correlation computations
# Use the number of CPU cores available on your machine
enableWGCNAThreads(nThreads = 4)

# Create output directory for all saved plots
dir.create("plots", recursive = TRUE, showWarnings = FALSE)

# Load the downsampled annotated Seurat object saved in Part 7 / Part 7-2
seurat_obj <- readRDS("seurat_subset.rds")

# Confirm key metadata columns are present
colnames(seurat_obj@meta.data)
table(seurat_obj$final_annotation)
table(seurat_obj$condition)
```

You should see the `final_annotation`, `condition`, and `orig.ident` columns, along with cell counts for each condition and cell type. Confirm that “Classical Monocytes” has enough cells — ideally at least 200 across all samples — before proceeding. The minimum requirement is roughly `k * number_of_samples` cells, where `k` is the metacell aggregation size we set in Step 3.

---

## Step 2: Initialize hdWGCNA with SetupForWGCNA

The first step is to tell hdWGCNA which genes to consider for network construction and to initialize an hdWGCNA slot inside the Seurat object that will store all downstream results.

```r
seurat_obj <- SetupForWGCNA(
  seurat_obj,
  gene_select = "fraction",   # Include genes expressed in at least 'fraction' of cells
  fraction    = 0.05,         # 5% minimum cell fraction -- filters very rare transcripts
  wgcna_name  = "Mono_WGCNA" # Label for this analysis instance inside the Seurat object
)
```

> 

**What does `gene_select = "fraction"` do?**

It keeps only genes that are detected (count > 0) in at least 5% of all cells. This removes extremely lowly expressed genes whose correlations would be dominated by noise. For a focused analysis, 5% is a reasonable starting point. If you end up with very few genes (< 2,000), try lowering the threshold to 0.03. If you want to use the variable features already computed by Seurat instead, set `gene_select = "variable"`.

> 

**What is `wgcna_name`?**

hdWGCNA can store multiple independent analyses inside the same Seurat object (for example, separate analyses for monocytes and T cells). The `wgcna_name` argument is the label that distinguishes them. All downstream functions will automatically use the most recently set analysis unless you specify `wgcna_name` explicitly.

---

## Step 3: Construct Metacells

This is the step that makes hdWGCNA fundamentally different from classic WGCNA. Instead of treating individual cells as observations for correlation, we aggregate groups of locally similar cells from the same sample into “metacells.”

```r
seurat_obj <- MetacellsByGroups(
  seurat_obj  = seurat_obj,
  group.by    = c("final_annotation", "orig.ident"), # Never mix cell types or samples
  k           = 20,         # Aggregate the 20 nearest neighbors into each metacell
  max_shared  = 10,         # Max cells shared between any two metacells (controls overlap)
  reduction   = "integrated.cca",  # Use CCA integration embeddings for nearest-neighbor search
  ident.group = "final_annotation"
)

# Normalize the metacell expression matrix
seurat_obj <- NormalizeMetacells(seurat_obj)
```

> 

**Understanding the key parameters:**

- `group.by = c("final_annotation", "orig.ident")`: This is critical. It ensures that when building metacells, the algorithm only considers cells from the **same cell type AND the same sample**. A monocyte from one donor is never grouped with a T cell, and cells from different donors are never merged. This preserves biological replicates.

- `k = 20`: Each metacell is formed by averaging the expression of 20 neighboring cells. A higher k reduces sparsity further but decreases the number of metacells you get. For smaller datasets, reduce k to 15 or even 10.

- `max_shared = 10`: Controls how much two metacells can overlap in cell membership. Lower values produce more independent metacells. The default of 10 is appropriate for most datasets.

- `reduction = "integrated.cca"`: Specifies which dimensionality reduction to use for nearest-neighbor search. Since we used CCA integration in [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/), we use `"integrated.cca"` here to find neighbors in the batch-corrected embedding space.

> 

**How many metacells should I expect?**

For each sample, hdWGCNA creates approximately `(number of cells of that type in that sample) / k` metacells, subject to the `max_shared` constraint. With 20 cells per metacell and 100 Classical Monocytes per sample across 8 samples, you would get roughly 40 metacells in total. Aim for at least 20-30 metacells for stable network construction.

---

## Step 4: Set the Expression Data for Network Construction

Now we tell hdWGCNA which cell type’s metacells to use for building the co-expression network. This is where you specify your cell type of interest.

```r
seurat_obj <- SetDatExpr(
  seurat_obj,
  group_name = "Classical Monocytes", # The cell type to analyze
  group.by   = "final_annotation",    # Metadata column containing cell type labels
  assay      = "RNA",
  slot       = "data"                 # Use log-normalized expression
)
```

> 

**Why “data” and not “counts”?**

The `data` slot contains log-normalized expression values (computed by `NormalizeData()` in Seurat). Log-normalization compresses the dynamic range of gene expression and makes the distribution closer to normal, which improves the quality of correlations. Using raw counts (`"counts"`) would be dominated by highly expressed genes and yield poor correlations.

---

## Step 5: Select the Soft-Thresholding Power

WGCNA does not use raw Pearson correlations to build the network. Instead, it raises all correlations to a power `beta` — the **soft-thresholding power** — to emphasize strong correlations and suppress weak ones. The resulting network approximates a **scale-free topology**, meaning a small number of hub genes have many connections and most genes have few. Scale-free topology is a universal property of biological networks.

The key question is: what value of `beta` to use?

```r
# Test a range of soft powers (1-20 for signed networks)
seurat_obj <- TestSoftPowers(seurat_obj, networkType = "signed")

# Assigning to a variable avoids any attempt to render on screen (important on HPC)
plot_list <- PlotSoftPowers(seurat_obj)
p_softpower <- wrap_plots(plot_list, ncol = 2)
ggsave("plots/soft_power.pdf", plot = p_softpower, width = 10, height = 5)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_soft_power.png?resize=1024%2C510&ssl=1)

> 

**How to read the soft power plot:**

The plot shows two panels:

- **Left panel (Scale-Free Topology R^2):** How well the network approximates scale-free topology at each soft power. You want this to reach at least 0.80.

- **Right panel (Mean Connectivity):** The average number of connections per gene. Higher powers reduce mean connectivity. Avoid powers where connectivity drops near zero, as the network becomes too sparse.

**Rule of thumb:** Choose the **lowest soft power where the scale-free R^2 first reaches 0.80 or higher**. Using a higher power than necessary does not improve results and may over-prune real connections.

```r
# After inspecting the plot, set your chosen soft power
soft_power <- 6
```

> 

**Signed vs unsigned networks:** We use `networkType = "signed"` throughout this tutorial. A signed network preserves the direction of correlation — genes that are positively correlated end up in the same module, while negatively correlated genes end up in different modules. This biological distinction is lost in unsigned networks. Always use signed networks for co-expression analysis unless you have a specific reason not to.

---

## Step 6: Construct the Co-expression Network

With the soft power chosen, we can now build the full weighted co-expression network. hdWGCNA calls WGCNA’s `blockwiseModules` function internally, which:

1. Computes the weighted adjacency matrix (correlations raised to `soft_power`)

2. Calculates the Topological Overlap Matrix (TOM), which measures how much two genes share neighbors

3. Performs hierarchical clustering on the TOM-based dissimilarity matrix

4. Cuts the dendrogram into modules using dynamic tree cutting

5. Merges modules that are too similar to each other

```r
seurat_obj <- ConstructNetwork(
  seurat_obj,
  soft_power      = soft_power,
  setDatExpr      = FALSE,    # SetDatExpr was already called in Step 4
  networkType     = "signed",
  TOMType         = "signed",
  minModuleSize   = 30,       # Minimum number of genes required to form a module
  mergeCutHeight  = 0.25      # Modules with < 25% dissimilarity are merged together
)
```

> 

**What is the TOM (Topological Overlap Matrix)?**

The TOM is a smarter measure of gene-gene similarity than raw correlation. Instead of asking “are gene A and gene B correlated?”, it asks “do gene A and gene B share the same set of correlated neighbors?” Two genes with similar neighborhoods in the network are functionally related even if their direct correlation is moderate. The TOM makes module detection more robust.

> 

**minModuleSize:** For smaller datasets (fewer metacells), consider reducing `minModuleSize` to 20 to avoid forcing too many genes into the “grey” (unassigned) module.

```r
# PlotDendrogram uses base R graphics, not ggplot -- must use pdf() on HPC
pdf("plots/dendrogram.pdf", width = 10, height = 5)
PlotDendrogram(seurat_obj, main = "Classical Monocyte hdWGCNA Dendrogram")
dev.off()
```

The dendrogram plot shows how genes cluster by their co-expression similarity. Each colored bar beneath the dendrogram represents a module. The **grey module** contains genes that did not cluster strongly enough to be assigned to any module — these are typically ignored in downstream analysis.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_tree-e1776515791190-1024x506.png?resize=1024%2C506&ssl=1)

---

## Step 7: Compute Module Eigengenes

The **module eigengene (ME)** is the first principal component of gene expression within a module. It captures the overall “activity level” of the module: a high ME value means the module’s genes are collectively highly expressed in that cell.

By computing module eigengenes at the single-cell level — not just at the metacell level — we can visualize module activity on the UMAP, correlate it with pseudotime, and compare it between conditions.

```r
# Scale features before computing eigengenes (required by PCA-based eigengene calculation)
seurat_obj <- ScaleData(seurat_obj, features = VariableFeatures(seurat_obj))

# Compute module eigengenes on individual cells
# group.by.vars enables Harmony correction of eigengenes across samples
seurat_obj <- ModuleEigengenes(
  seurat_obj,
  group.by.vars = "orig.ident"  # Harmonize across biological replicates
)
```

> 

**What does sample-level correction of eigengenes do?**

Just as we used CCA in [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) to remove batch effects from the cell embeddings, `ModuleEigengenes` applies Harmony to harmonize module eigengene values across samples. This is an internal correction step within hdWGCNA that is independent of whichever integration method you used earlier in the Seurat workflow. Without this correction, a module might appear “active” in one sample simply because that sample has a higher overall expression level, not because the module is genuinely more active. Setting `group.by.vars = "orig.ident"` tells hdWGCNA to harmonize across all individual samples (donors).

After this step, the Seurat object stores two sets of module eigengenes:

- **MEs**: Raw module eigengenes

- **hMEs**: Harmony-corrected harmonized module eigengenes (preferred for visualization and downstream analysis)

---

## Step 8: Compute Intramodular Connectivity (kME)

**kME** (also called module membership) measures how strongly a gene’s expression pattern correlates with its module’s eigengene. A gene with high kME for its assigned module is a **hub gene** — it is the most representative and connected gene in that module, and often the most biologically important.

```r
# Compute kME for all genes in their assigned modules
seurat_obj <- ModuleConnectivity(
  seurat_obj,
  group.by   = "final_annotation",
  group_name = "Classical Monocytes"
)
```

After running `ModuleConnectivity`, each gene in the module table has a kME score. You can retrieve the full module assignment table:

```r
# Get the full module assignment table
modules <- GetModules(seurat_obj, wgcna_name = "Mono_WGCNA")
head(modules)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_modules.png?resize=1024%2C299&ssl=1)

The table contains columns for gene name, module color (hdWGCNA uses colors to name modules), and kME scores for every module. The `kME` column for a gene’s own module is the most relevant value.

```r
# Get the top 10 hub genes from each module
hub_df <- GetHubGenes(seurat_obj, n_hubs = 10)
head(hub_df, 20)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_hub_df.png?resize=1024%2C425&ssl=1)

> 

**Hub genes in monocytes:**

For Classical Monocytes, you might expect hub genes like `LYZ`, `S100A8`, `S100A9`, `VCAN`, `FCN1`, and `CD14` in inflammatory modules, or `HLA-DRA`, `HLA-DRB1`, `CD74` in antigen presentation modules. Finding these known monocyte markers as hub genes is a good sign that the modules are biologically meaningful.

### Visualize kME Score Distributions

```r
# Plot the distribution of kME values across all modules
# Each panel shows one module; genes with high kME are the hub genes
p_kme <- PlotKMEs(seurat_obj, ncol = 5)
ggsave("plots/kme_distributions.pdf", plot = p_kme, width = 14, height = 8)
```

Each panel in this plot shows the distribution of kME scores for genes assigned to that module. A good module has a wide range of kME values (some high-kME hub genes, many lower-kME members). A flat distribution suggests the module may be poorly defined.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_kme.png?resize=1024%2C585&ssl=1)

---

## Step 9: Visualize Module Activity on the UMAP

### Plot Harmonized Module Eigengenes on Cells

```r
# Project harmonized module eigengenes onto the single-cell UMAP
# Each panel shows the activity of one module across all cells
plot_list_hme <- ModuleFeaturePlot(
  seurat_obj,
  features  = "hMEs",
  order     = TRUE,
  reduction = "umap.cca"  # Specify the correct UMAP reduction name
)

# Verify plots were generated before saving -- stops early if ModuleFeaturePlot failed
stopifnot(length(plot_list_hme) > 0)

p_hme <- wrap_plots(plot_list_hme, ncol = 4)
ggsave("plots/module_eigengene_umap.pdf", plot = p_hme, width = 16, height = 12)
```

> 

**Interpreting the module UMAP plots:**

Each UMAP plot shows all cells colored by the harmonized eigengene value (hME) of one module. High values indicate cells where that module’s genes are collectively highly active. Because we are running hdWGCNA specifically on Classical Monocytes, the most meaningful signal will be within the monocyte cluster(s) of the UMAP. Activity in other cell types is informative but secondary.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_eigengene_umap-e1776515975783-1024x632.png?resize=1024%2C632&ssl=1)

### Module Correlation Heatmap

```bash
# ModuleCorrelogram uses base R graphics -- must use pdf() on HPC
pdf("plots/module_correlogram.pdf", width = 8, height = 7)
ModuleCorrelogram(seurat_obj)
dev.off()
```

This heatmap shows how strongly the different module eigengenes correlate with each other. Modules that cluster together in the correlogram likely represent related biological processes.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_correlation-e1776516015347-1024x961.png?resize=1024%2C961&ssl=1)

---

## Step 10: Compute and Visualize Single-Cell Module Scores

Module eigengenes (MEs) capture module activity using PCA, but **module scores** computed with UCell use a rank-based enrichment approach that is more robust to outlier cells. Module scores are useful for feature plots and comparing module activity between individual cells.

```r
# Compute module scores using the UCell algorithm
# n_genes: number of top hub genes per module to use for scoring
seurat_obj <- ModuleExprScore(
  seurat_obj,
  n_genes = 25,
  method  = "UCell"
)

# Plot module scores on the UMAP
plot_list_scores <- ModuleFeaturePlot(
  seurat_obj,
  features  = "scores",
  order     = TRUE,
  ucell     = TRUE,
  reduction = "umap.cca"  # Specify the correct UMAP reduction name
)

p_scores <- wrap_plots(plot_list_scores, ncol = 4)
ggsave("plots/module_scores_umap.pdf", plot = p_scores, width = 16, height = 12)
```

> 

**ME vs module score: which to use?**

- Use **hMEs** for statistical analyses (DME analysis, correlation with pseudotime or clinical traits) because they are continuous, well-defined, and directly comparable across cells.

- Use **module scores** for visualization because they are more visually intuitive and less affected by outlier cells.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_score_umap-e1776516052739-1024x660.png?resize=1024%2C660&ssl=1)

---

## Step 11: Functional Enrichment Analysis

To understand the biological functions of each co-expression module, we run gene ontology and pathway enrichment analysis on the module members using Enrichr.

```r
# Define the databases to query
# Use the most up-to-date versions of GO and KEGG
dbs <- c("GO_Biological_Process_2023", "KEGG_2021_Human")

# Run enrichment analysis on all modules
seurat_obj <- RunEnrichr(
  seurat_obj,
  dbs      = dbs,
  max_genes = 100  # Use up to 100 genes per module for enrichment
)

# Retrieve the full enrichment results table
enrich_df <- GetEnrichrTable(seurat_obj)
head(enrich_df)

# Generate bar plots of the top enriched terms for each module
# Plots are saved to the specified output directory
EnrichrBarPlot(
  seurat_obj,
  outdir     = "enrichr_plots",
  n_terms    = 10,           # Show the top 10 enriched terms per module
  plot_size  = c(5, 7),      # Width x height in inches
  logscale   = TRUE          # Plot -log10(adjusted p-value) on the x-axis
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_GO_Enrichment-e1776516476481-1024x353.png?resize=1024%2C353&ssl=1)

> 

**How to interpret enrichment results:**

For Classical Monocytes in the periodontitis dataset, you might find:

- An inflammatory module enriched for “innate immune response,” “cytokine production,” and “NF-kappaB signaling”

- An antigen presentation module enriched for “MHC class II protein complex,” “antigen processing and presentation”

- A metabolic module enriched for “oxidative phosphorylation” or “fatty acid metabolism”

Modules that are enriched for known monocyte functions validate that the co-expression network is capturing real biology. Modules with no clear enrichment may represent cell-cycle genes, ribosomal proteins, or technical artifacts.

> 

**Enrichr database choice:** The GO Biological Process database is broad and informative. KEGG gives more pathway-level context. For immune cell studies, you might also add `"WikiPathway_2023_Human"` or `"Reactome_2022"` to the `dbs` vector.

---

## Step 12: Differential Module Eigengene (DME) Analysis

The most biologically powerful analysis in hdWGCNA for our dataset is **DME analysis**: comparing module eigengene values between Healthy and Post_Treatment Classical Monocytes to identify which co-expression programs are significantly different between conditions.

```r
# Direct pairwise DME test: Healthy vs Post_Treatment
# Positive log2FC = higher module activity in Healthy
# Negative log2FC = higher module activity in Post_Treatment
DMEs <- FindDMEs(
  seurat_obj,
  barcodes1  = colnames(seurat_obj)[seurat_obj$condition == "Healthy"],
  barcodes2  = colnames(seurat_obj)[seurat_obj$condition == "Post_Treatment"],
  test.use   = "wilcox",
  wgcna_name = "Mono_WGCNA"
)

# Inspect the results table
# Columns: module, avg_ME, p_val, p_val_adj, log2FC
head(DMEs)

p_dme <- PlotDMEsVolcano(
  seurat_obj,
  DMEs       = DMEs,
  wgcna_name = "Mono_WGCNA"
)

ggsave("plots/DME_volcano.pdf", plot = p_dme, width = 8, height = 6)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_eigengene_volcano.png?resize=1024%2C761&ssl=1)

> 

**Reading the DME volcano plot:**

Each point on the volcano plot represents a co-expression module. The x-axis shows the log2 fold change of the module eigengene (positive = higher in one condition, negative = higher in the other). The y-axis shows the statistical significance. Points in the upper-right quadrant are modules with significantly higher activity in one condition; points in the upper-left are significantly higher in the other.

Cross-reference the DME results with the enrichment analysis from Step 11 to build a complete story: “Module X, which is enriched for cytokine signaling genes, is significantly more active in Healthy monocytes than in Post_Treatment monocytes.”

> 

**DME vs standard differential expression:**

Standard differential expression (as in [Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/)) tests each gene individually. DME analysis tests each module as a unit, treating the module eigengene as a summary statistic for the entire co-expression program. This gives you more statistical power because you are testing a coherent biological signal (the shared variation of many co-expressed genes) rather than individual noisy gene counts.

---

## Step 13: Visualize the Gene Co-expression Network

### Hub Gene Network Plot

The hub gene network plot shows the most connected hub genes within and between modules as a graph, with edges representing the strength of co-expression.

```bash
# Plot a network graph of hub genes from all modules
# n_hubs: number of hub genes per module to include
# n_other: additional non-hub genes to include per module
# edge_prop: proportion of edges to display (0.75 = top 75% strongest connections)
# HubGeneNetworkPlot uses future.apply internally -- raise the globals size limit
options(future.globals.maxSize = 8 * 1024^3)

# HubGeneNetworkPlot renders directly to the graphics device -- use pdf() on HPC
pdf("plots/hub_gene_network.pdf", width = 10, height = 10)
HubGeneNetworkPlot(
  seurat_obj,
  n_hubs     = 3,
  n_other    = 5,
  edge_prop  = 0.75,
  mods       = "all"
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_hub_gene.png?resize=1003%2C1024&ssl=1)

> 

**Reading the hub gene network:**

- Each **node** is a gene, colored by its module assignment

- **Node size** reflects the gene’s kME (larger = higher module membership)

- **Edges** connect co-expressed genes; thicker/darker edges mean stronger co-expression

- Hub genes (highest kME) are labeled by name

This visualization is useful for presentations and for identifying central genes within each module. Genes that bridge two modules (appear at the junction between two color clusters) may have roles in coordinating multiple programs.

### Gene Co-expression Network UMAP

hdWGCNA can also create a UMAP of the gene network itself, where genes are nodes and proximity reflects co-expression strength. This gives a “bird’s eye view” of the entire network structure.

```r
# Build a UMAP representation of the gene co-expression network
# n_hubs: number of hub genes per module used to define the UMAP layout
seurat_obj <- RunModuleUMAP(
  seurat_obj,
  n_hubs      = 10,
  n_neighbors = 15,
  min_dist    = 0.1
)

# Plot the gene network UMAP with ModuleUMAPPlot
# edge_prop = 0.05: show only the top 5% strongest gene-gene connections for clarity
# ModuleUMAPPlot renders directly to the graphics device -- use pdf() on HPC
pdf("plots/gene_network_umap.pdf", width = 10, height = 9)
ModuleUMAPPlot(
  seurat_obj,
  edge_prop        = 0.05,
  label_hubs       = 5,
  keep_grey_edges  = FALSE
)
dev.off()
```

> 

**Network UMAP vs cell UMAP:**

The cell UMAP (produced in [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/#The_Clustering_Algorithm_Graph-Based_Community_Detection)) shows cells organized by their gene expression similarity. The gene network UMAP shows **genes** organized by their co-expression similarity within Classical Monocytes. Gene clusters in the UMAP correspond to co-expression modules. Genes at the center of each cluster are the hub genes; genes at the periphery are more weakly connected to the module.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/hdwgcna_module_umap.png?resize=1024%2C981&ssl=1)

---

## Step 14: Save Your Results

```r
# Save the Seurat object with all hdWGCNA results embedded
saveRDS(seurat_obj, "seurat_hdwgcna.rds")

# Export the module assignment table as a CSV
write.csv(modules, "monocyte_modules.csv", row.names = FALSE)

# Export hub genes
write.csv(hub_df, "monocyte_hub_genes.csv", row.names = FALSE)

# Export DME results
write.csv(DMEs, "monocyte_DMEs.csv", row.names = FALSE)

# Export enrichment results
write.csv(enrich_df, "monocyte_enrichment.csv", row.names = FALSE)
```

All hdWGCNA analysis results are stored in the `@misc` slot of the Seurat object under the `wgcna_name` label (`"Mono_WGCNA"`). The saved RDS file contains the complete analysis and can be loaded in a future session without re-running any steps.

---

## Best Practices, Common Pitfalls, and Troubleshooting

### Best Practices

**1. Always run hdWGCNA separately for each cell type**

Mixing cell types in `SetDatExpr` will produce modules that reflect cell-type identity rather than within-cell-type co-regulation. Run separate `SetupForWGCNA` analyses (with different `wgcna_name` labels) for each population you want to study.

**2. Validate soft power selection carefully**

Do not skip inspecting the `PlotSoftPowers` plot. Different datasets, different cell types, and different metacell sizes can all require different soft powers. Blindly using the same power as another study can lead to networks that are either too sparse (too high a power) or not scale-free (too low a power).

**3. Check metacell counts before proceeding**

After `MetacellsByGroups`, use `GetMetacellObject(seurat_obj)` to inspect how many metacells were created per sample and per cell type. If a sample has fewer than 5 metacells for your cell type of interest, that sample will contribute almost no information to the network and may introduce noise. Consider excluding samples with too few metacells.

**4. Interpret hub genes in biological context**

The top hub genes by kME are the most co-expressed with their module’s eigengene, but “most co-expressed” does not automatically mean “most functionally important.” Always cross-reference hub genes with existing literature, pathway databases, and your differential expression results from Part 5 to build a coherent biological narrative.

**5. Validate DME results with module score plots**

After identifying significant DMEs, visualize the module scores (from Step 10) separately for Healthy and Post_Treatment cells to visually confirm the difference. Statistical significance should align with a visually apparent difference in module activity on the UMAP.

---

### Common Pitfalls

**Pitfall 1: Too few metacells**

If you have fewer than 20-30 metacells in total for your cell type, the correlation matrix will be unstable and the soft power selection plot will look erratic. Solutions: reduce `k`, reduce `minModuleSize`, or analyze a more abundant cell type.

**Pitfall 2: Grey module is very large**

If the grey (unassigned) module contains the majority of your genes, your network parameters are too stringent. Try: reducing `soft_power` by 1-2 units, reducing `minModuleSize` to 20, or lowering `mergeCutHeight` to 0.20.

**Pitfall 3: Module eigengenes are dominated by batch effects**

If module eigengenes cluster by sample (donor) rather than by cell state or condition on the UMAP, Harmony correction may be insufficient. Inspect the `ModuleCorrelogram` — if all modules are highly correlated with each other, this suggests a global batch signal is present. Ensure `group.by.vars = "orig.ident"` is set in `ModuleEigengenes`.

**Pitfall 4: Over-interpreting uncorrected enrichment results**

Enrichr does not correct for the fact that you are running enrichment on many modules simultaneously. Use the adjusted p-value (`Adjusted.P.value` in the enrichment table) and require at least 3-5 genes overlap for any term you report.

---

## Conclusion

In this tutorial, we applied **hdWGCNA** to the Classical Monocytes of the PBMC periodontitis dataset (GSE174609), discovering the co-expression modules that define monocyte gene programs and identifying which of those modules shift in activity between Healthy and Post_Treatment conditions.

### What Sets hdWGCNA Apart

hdWGCNA fills a genuine gap in the single-cell analysis toolkit. Most scRNA-seq analyses ask: “Which individual genes change?” hdWGCNA asks a complementary question: “Which **programs** of co-regulated genes change?” This systems-level view is more robust, more interpretable, and often reveals coordinated biological processes that would be invisible in gene-by-gene testing.

By identifying hub genes — the most connected, most representative genes in each module — hdWGCNA also prioritizes candidates for experimental follow-up. A hub gene in a disease-associated module is a much better hypothesis for a causal driver than an individual differentially expressed gene with moderate fold change.

### Where to Go from Here

- **Apply to other cell types:** Re-run the analysis from Step 2 with `wgcna_name = "Tcell_WGCNA"` and `group_name = "CD4+ T cells"` to discover T cell co-expression programs and compare them with monocyte modules

- **Correlate modules with pseudotime:** Use the hME values alongside the pseudotime scores from [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/) to identify modules that change along the trajectory

- **Integrate with CellChat results:** Cross-reference hub genes from inflammatory modules with the ligand-receptor interactions identified in [Part 9](https://ngs101.com/) to connect co-expression programs with intercellular signaling

---

## References

1. Morabito S, Reese F, Rahimzadeh N, Miyoshi E, Bhatt DL, Swarup V. hdWGCNA identifies co-expression networks in high-dimensional transcriptomics data. Cell Rep Methods. 2023;3(6):100498. doi:10.1016/j.crmeth.2023.100498 [hdWGCNA]

2. Langfelder P, Horvath S. WGCNA: an R package for weighted correlation network analysis. BMC Bioinformatics. 2008;9:559. doi:10.1186/1471-2105-9-559 [WGCNA]

3. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. Nat Biotechnol. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

4. Korsunsky I, Millard N, Fan J, et al. Fast, sensitive and accurate integration of single-cell data with Harmony. Nat Methods. 2019;16(12):1289-1296. doi:10.1038/s41592-019-0619-0 [Harmony]

5. Andreatta M, Carmona SJ. UCell: Robust and scalable single-cell gene signature scoring. Comput Struct Biotechnol J. 2021;19:3796-3798. doi:10.1016/j.csbj.2021.06.043 [UCell]

6. Kuleshov MV, Jones MR, Rouillard AD, et al. Enrichr: a comprehensive gene set enrichment analysis web server 2016 update. Nucleic Acids Res. 2016;44(W1):W90-W97. doi:10.1093/nar/gkw377 [Enrichr]

7. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. J Transl Med. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [GSE174609 dataset]

8. Luecken MD, Theis FJ. Current best practices in single-cell RNA-seq analysis: a tutorial. Mol Syst Biol. 2019;15(6):e8746. doi:10.15252/msb.20188746

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
