# How to Analyze Single-Cell RNA-seq Data – Complete Beginner’s Guide Part 7: Trajectory and Pseudotime Analysis Using Monocle 3

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/)

- [Introduction: Understanding Cell State Transitions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Introduction_Understanding_Cell_State_Transitions)

  - [What Is Trajectory Analysis and Why Do We Need It?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#What_Is_Trajectory_Analysis_and_Why_Do_We_Need_It)
  - [What Biological Questions Does Trajectory Analysis Answer?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#What_Biological_Questions_Does_Trajectory_Analysis_Answer)
  - [When Should You Perform Trajectory Analysis?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#When_Should_You_Perform_Trajectory_Analysis)
  - [How Does Trajectory Analysis Work Conceptually?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#How_Does_Trajectory_Analysis_Work_Conceptually)
  - [What Is Monocle 3 and How Does It Infer Trajectories?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#What_Is_Monocle_3_and_How_Does_It_Infer_Trajectories)
  - [What You’ll Learn in This Tutorial](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#What_Youll_Learn_in_This_Tutorial)

- [Setting Up Your R Environment for Trajectory Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Setting_Up_Your_R_Environment_for_Trajectory_Analysis)

  - [Installing Monocle 3 and Dependencies](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Installing_Monocle_3_and_Dependencies)
  - [Loading Required Libraries](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Loading_Required_Libraries)

- [Preparing Data for Monocle 3 Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Preparing_Data_for_Monocle_3_Analysis)

  - [Understanding the Monocle 3 Workflow](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Understanding_the_Monocle_3_Workflow)
  - [Loading the Annotated Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Loading_the_Annotated_Seurat_Object)
  - [Strategic Downsampling: Maintaining Biological Diversity](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Strategic_Downsampling_Maintaining_Biological_Diversity)
  - [Visualizing the Downsampled Dataset](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Visualizing_the_Downsampled_Dataset)

- [Trajectory Inference with Monocle 3: Step-by-Step Workflow](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Trajectory_Inference_with_Monocle_3_Step-by-Step_Workflow)

  - [Converting Seurat Object to Monocle 3 CDS Format](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Converting_Seurat_Object_to_Monocle_3_CDS_Format)
  - [Preprocessing: Normalization and Dimensionality Reduction](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Preprocessing_Normalization_and_Dimensionality_Reduction)
  - [Aligning to Original Seurat UMAP Coordinates](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Aligning_to_Original_Seurat_UMAP_Coordinates)
  - [Learning the Trajectory Graph Structure](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Learning_the_Trajectory_Graph_Structure)
  - [Handling Multiple Partitions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Handling_Multiple_Partitions)
  - [Understanding Pseudotime and Root Selection](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Understanding_Pseudotime_and_Root_Selection)
  - [Step 10a: Order Partition 2 (Manual Root — CD4+ T Cells)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Step_10a_Order_Partition_2_Manual_Root_%E2%80%94_CD4_T_Cells)
  - [Step 10b: Order Partition 1 (Automatic Root — Classical Monocytes)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Step_10b_Order_Partition_1_Automatic_Root_%E2%80%94_Classical_Monocytes)
  - [Step 10c: Order Partition 3 (Automatic Root — B Cells)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Step_10c_Order_Partition_3_Automatic_Root_%E2%80%94_B_Cells)
  - [Verify All Partitions Are Ordered](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Verify_All_Partitions_Are_Ordered)
  - [Visualizing Trajectories Across All Partitions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Visualizing_Trajectories_Across_All_Partitions)
  - [Creating Custom Pseudotime Visualizations](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Creating_Custom_Pseudotime_Visualizations)

- [Interpreting Trajectory Analysis Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Interpreting_Trajectory_Analysis_Results)

  - [Understanding What Trajectories Represent](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Understanding_What_Trajectories_Represent)
  - [Biological Validation: Does the Trajectory Make Sense?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Biological_Validation_Does_the_Trajectory_Make_Sense)
  - [Understanding Branch Points and Cell Fate Decisions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Understanding_Branch_Points_and_Cell_Fate_Decisions)
  - [Integrating Results Back to Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Integrating_Results_Back_to_Seurat_Object)

- [Identifying Genes That Change Along Trajectories](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Identifying_Genes_That_Change_Along_Trajectories)

  - [Finding Genes Correlated with Pseudotime](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Finding_Genes_Correlated_with_Pseudotime)
  - [Visualizing Top Trajectory Genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Visualizing_Top_Trajectory_Genes)

- [Best Practices and Common Pitfalls in Trajectory Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Best_Practices_and_Common_Pitfalls_in_Trajectory_Analysis)

  - [Best Practices for Robust Trajectory Inference](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Best_Practices_for_Robust_Trajectory_Inference)
  - [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Common_Pitfalls_and_How_to_Avoid_Them)
  - [Troubleshooting: Trajectory Doesn’t Match Expectations](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Troubleshooting_Trajectory_Doesnt_Match_Expectations)

- [Conclusion: From Clusters to Continuous Cell States](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Conclusion_From_Clusters_to_Continuous_Cell_States)

  - [Key Conceptual Takeaways](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Key_Conceptual_Takeaways)
  - [Reporting Trajectory Analysis in Publications](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Reporting_Trajectory_Analysis_in_Publications)

- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/#Like_this)

## Introduction: Understanding Cell State Transitions

### What Is Trajectory Analysis and Why Do We Need It?

If you’ve completed Parts 1-6 of this tutorial series, you’ve successfully:

- Processed raw sequencing data into count matrices ([Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/))

- Quality-controlled your cells ([Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/))

- Integrated samples and identified clusters ([Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Assigned biological identities to clusters ([Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/))

- Performed differential expression analysis ([Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/))

- Mastered data object structures ([Part 6](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/))

But here’s a fundamental limitation of clustering: **cells don’t exist in discrete categories**.

**The biological reality:**

- Stem cells gradually differentiate into specialized cell types

- Immune cells continuously respond to stimuli, transitioning through activation states

- Cancer cells evolve through progressive stages of transformation

- Developmental processes unfold as smooth, continuous progressions

**The computational challenge:** Clustering algorithms force cells into discrete groups, creating artificial boundaries where none exist biologically. Two cells in different clusters might actually represent consecutive points along a continuous developmental pathway.

**Trajectory analysis solves this** by modeling cells as existing along continuous paths of gene expression change, revealing:

- **Differentiation pathways**: How stem/progenitor cells become specialized

- **Temporal ordering**: The sequence of gene expression changes during processes

- **Branch points**: Where cells make fate decisions (e.g., becoming cell type A vs B)

- **Pseudotime**: A measure of biological progression independent of real time

### What Biological Questions Does Trajectory Analysis Answer?

**Question 1: What is the differentiation path from stem cells to mature cells?**

Example: In hematopoiesis (blood cell development), trajectory analysis can reveal:

- Common myeloid progenitors → Monocytes → Macrophages/Dendritic cells

- Branch points where cells commit to different lineages

- Genes driving each developmental decision

**Question 2: How do cells respond to treatment or stimuli over time?**

Example: In our periodontitis dataset (GSE174609), trajectory analysis might show:

- How immune cells transition from resting to activated states

- Whether treatment reverses cells along activation trajectories

- Genes that change early vs late in activation

**Question 3: What is the temporal order of gene expression changes?**

Example: During T cell activation:

- Early genes: Immediate response transcription factors (FOS, JUN)

- Middle genes: Cytokine receptors and signaling molecules

- Late genes: Effector molecules and exhaustion markers

**Question 4: Where do cells make fate decisions?**

Example: At a branch point, cells might choose between:

- Becoming cytotoxic T cells (high GZMB) vs regulatory T cells (high FOXP3)

- Identifying the genes that determine this choice

### When Should You Perform Trajectory Analysis?

**✅ Ideal scenarios for trajectory analysis:**

1. **Developmental systems**: Stem cell differentiation, organogenesis, tissue regeneration

2. **Time-course experiments**: Treatment response over hours/days, disease progression

3. **Activation/response**: Immune cell activation, stress responses, cell cycle

4. **Continuous processes**: Any biological process where cells transition smoothly between states

**❌ When trajectory analysis may not be appropriate:**

1. **Stable, terminally differentiated cells**: Mature neurons, hepatocytes (no transitions occurring)

2. **Discrete cell types with no developmental relationship**: Comparing liver cells to brain cells

3. **Technical artifacts**: Batch effects or quality gradients masquerading as biology

4. **Insufficient cellular diversity**: Need cells across the full trajectory, not just endpoints

5. **Very rare cell types**: <50 cells cannot reliably define trajectories (e.g., Platelets with 39 cells)

**Critical requirement**: Your dataset must contain **intermediate states** between start and end points. If you only have fully differentiated cells with no transitional forms, trajectory analysis cannot infer the path between them.

### How Does Trajectory Analysis Work Conceptually?

Think of trajectory analysis as **reverse-engineering a road map** from GPS coordinates:

**Input**: Positions of many cars (cells) in gene expression space (UMAP/PCA)

**Goal**: Infer the underlying road network (differentiation paths)

**Method**:

1. **Assume cells are at different points along a journey** (early, middle, late in differentiation)

2. **Connect nearby cells** based on gene expression similarity

3. **Find the most likely path** that connects cells from start to end

4. **Order cells along this path** (pseudotime = position on the road)

5. **Identify branch points** where roads diverge (fate decisions)

**Key insight**: We don’t need to know which cells are “early” vs “late” in advance. The algorithm infers this from the data structure—cells form natural progressions in gene expression space.

### What Is Monocle 3 and How Does It Infer Trajectories?

**Monocle 3** is one of the most widely used trajectory inference tools, developed by Cole Trapnell’s lab. It works through several key steps:

**Step 1: Principal Graph Learning**

Instead of forcing cells onto predefined paths, Monocle 3 learns the trajectory structure directly from the data:

- Constructs a “principal graph” that fits through the cloud of cells in reduced dimension space

- Graph is flexible, allowing complex topologies (linear, branched, cyclic)

- Captures the manifold structure of cell state transitions

**Step 2: Partition Detection**

- Identifies **partitions**: Groups of cells connected by continuous trajectories

- Cells in different partitions have no inferred connection (represent separate biological processes)

- Example: Lymphoid and myeloid lineages might be separate partitions

**Step 3: Pseudotime Calculation**

- Assigns each cell a **pseudotime** value representing its position along the trajectory

- Pseudotime = 0 at the start (root) of the trajectory

- Increases as cells progress toward differentiated states

- Note: Pseudotime is relative, not absolute time

**Step 4: Branch Assignment**

- Identifies **branch points** where the trajectory splits

- Assigns cells to specific branches

- Enables analysis of genes specific to each fate choice

**Why Monocle 3 for this tutorial?**

- **Flexible**: Handles simple linear and complex branched trajectories

- **Well-integrated**: Works seamlessly with Seurat objects

- **Interpretable**: Clear visualization of trajectories and pseudotime

- **Widely adopted**: Extensive documentation and community support

**Important conceptual note**: Trajectory analysis makes an assumption—that the developmental process you’re studying is actually occurring in your data. If you have only mature cell types with no intermediate states, the algorithm may still create a trajectory, but it won’t be biologically meaningful. Always validate that the inferred trajectory makes biological sense!

### What You’ll Learn in This Tutorial

By the end of this tutorial, you’ll be able to:

✅ **Understand** when trajectory analysis is appropriate for your data✅ **Install and configure** Monocle 3 correctly (avoiding common dependency issues)✅ **Convert** Seurat objects to Monocle 3 format while preserving metadata✅ **Run** complete trajectory inference workflows step-by-step✅ **Handle multiple partitions** representing disconnected biological processes✅ **Use both root selection methods** (manual and automatic) programmatically✅ **Visualize** trajectories and pseudotime on your existing UMAP coordinates✅ **Identify** genes that change along developmental paths✅ **Interpret** branch points and fate decisions✅ **Troubleshoot** common Monocle 3 errors and issues✅ **Integrate** trajectory results back into your Seurat workflow

**Special features of this tutorial:**

- **HPC-compatible**: Fully non-interactive code suitable for batch jobs and servers without graphical displays

- **Reproducible**: Programmatic root selection instead of interactive clicking

- **Real-world data**: Handles multiple disconnected partitions (myeloid, lymphoid lineages)

- **Dual methodology**: Demonstrates both manual (biologically informed) and automatic (data-driven) root selection

Let’s begin by setting up the necessary software environment.

---

## Setting Up Your R Environment for Trajectory Analysis

### Installing Monocle 3 and Dependencies

Monocle 3 has specific dependencies that can be challenging to install. We’ll use a systematic approach that handles these properly.

**Why is Monocle 3 installation tricky?**

- Requires specific Bioconductor packages with version compatibility

- Depends on archived CRAN package (grr)

- Needs renv package for GitHub installation

- Some dependencies (terra, ggrastr) have system-level requirements

**The solution**: Follow the exact installation sequence below to avoid conflicts.

```r
#-----------------------------------------------
# Installation: Monocle 3 and dependencies
#-----------------------------------------------

# Set CRAN mirror
options(repos = c(CRAN = "https://cloud.r-project.org"))

# Install BiocManager for Bioconductor dependencies
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

# Install Bioconductor dependencies for Monocle 3
BiocManager::install(c(
  "BiocGenerics",
  "DelayedArray",
  "DelayedMatrixStats",
  "limma",
  "lme4",
  "S4Vectors",
  "SingleCellExperiment",
  "SummarizedExperiment",
  "batchelor",
  "Matrix.utils",
  "HDF5Array",
  "terra",
  "ggrastr"
))

# Install CRAN packages
install.packages(c(
  "Seurat",         # Already installed from previous parts
  "SeuratObject",   # Seurat object infrastructure
  "dplyr",          # Data manipulation
  "ggplot2",        # Visualization
  "patchwork",      # Combine plots
  "RColorBrewer",   # Color palettes
  "viridis",        # Perceptually uniform colors
  "Matrix",         # Sparse matrix operations
  "renv",           # Required for Monocle 3 installation
  "remotes"         # Required for SeuratWrappers installation
))

# Install grr (archived CRAN package required by Monocle 3)
install.packages("https://cran.r-project.org/src/contrib/Archive/grr/grr_0.9.5.tar.gz", 
                 repos = NULL, type = "source")

# Install Monocle 3 from GitHub
renv::install('cole-trapnell-lab/monocle3')

# Install SeuratWrappers from GitHub (for Seurat <-> Monocle conversion)
remotes::install_github('satijalab/seurat-wrappers')
```

### Loading Required Libraries

```r
#-----------------------------------------------
# STEP 1: Load required libraries
#-----------------------------------------------

# Core single-cell analysis
library(Seurat)
library(SeuratObject)
library(SeuratWrappers)

# Trajectory inference
library(monocle3)

# Data manipulation and visualization
library(dplyr)
library(ggplot2)
library(patchwork)
library(RColorBrewer)
library(viridis)

# Set working directory
setwd("~/GSE174609_scRNA/trajectory_analysis")

# Create output directories
dir.create("plots", showWarnings = FALSE)
dir.create("plots/trajectories", showWarnings = FALSE)
dir.create("plots/pseudotime", showWarnings = FALSE)
dir.create("plots/genes_over_pseudotime", showWarnings = FALSE)
dir.create("results", showWarnings = FALSE)

# Set random seed for reproducibility
set.seed(42)

# Configure plotting defaults
theme_set(theme_classic(base_size = 12))
```

**What we’ve done:**

- ✅ Loaded Seurat for data management and visualization

- ✅ Loaded monocle3 for trajectory inference

- ✅ Loaded visualization tools for publication-quality plots

- ✅ Created organized directory structure for outputs

- ✅ Set reproducible random seed (important for consistent results)

---

## Preparing Data for Monocle 3 Analysis

### Understanding the Monocle 3 Workflow

Before diving into code, let’s understand the complete Monocle 3 workflow:

```r
Step 1: Load annotated Seurat object (from Part 4)
   ↓
Step 2: Subset and downsample cells (computational efficiency for demonstration purpose)
   ↓
Step 3: Convert Seurat → Monocle 3 cell_data_set (CDS) object
   ↓
Step 4: Preprocess data (PCA dimensionality reduction)
   ↓
Step 5: Align to original UMAP (maintain consistency with Seurat)
   ↓
Step 6: Learn trajectory graph structure
   ↓
Step 7: Order cells in pseudotime
   ↓
Step 8: Visualize trajectories and pseudotime
   ↓
Step 9: Identify trajectory-associated genes
   ↓
Step 10: Integrate results back to Seurat object
```

### Loading the Annotated Seurat Object

```r
#-----------------------------------------------
# STEP 2: Load annotated Seurat object from Part 4
#-----------------------------------------------

# Load the complete annotated object
seurat_full <- readRDS("~/GSE174609_scRNA/cell_type_annotation/annotations/integrated_annotated_seurat.rds")

# Verify data structure
cat("Original dataset:\n")
cat("  Total cells:", ncol(seurat_full), "\n")
cat("  Total genes:", nrow(seurat_full), "\n")
cat("  Samples:", length(unique(seurat_full$sample_id)), "\n")
cat("  Cell types:", length(unique(seurat_full$final_annotation)), "\n\n")

# Check cell type distribution
cat("Cell type distribution (full dataset):\n")
cell_type_counts <- table(seurat_full$final_annotation)
print(cell_type_counts)
cat("\n")
```

**Expected output:**

```
Original dataset:
  Total cells: 72649 
  Total genes: 18861 
  Samples: 8 
  Cell types: 8 

Cell type distribution (full dataset):

            B cells        CD4+ T cells        CD8+ T cells Classical Monocytes 
               6735               31379                3132                5218 
          Monocytes             NK cells           Platelets             T cells 
               2352               16067                  39                7727 
```

**Understanding the data:**

- **7 cell types**: Representing major immune populations in PBMCs (Platelets excluded)

- **72,649 cells**: Large dataset suitable for trajectory analysis

- **Uneven distribution**: T cells are most abundant (~55%), Monocytes rarest (~3%)

- **Source**: Integrated, batch-corrected data with consensus annotations

- **Platelets excluded**: Only 39 cells total (too rare for meaningful trajectory analysis)

### Strategic Downsampling: Maintaining Biological Diversity

**Why downsample all cell types?**

In trajectory analysis, we want to:

- Capture transitions **across** different cell types (if they exist)

- Maintain representation of rare cell types (dendritic cells, plasma cells)

- Balance computational efficiency with biological coverage

**Strategy**: Take 10% of cells from **each cell type** separately, ensuring:

- Rare cell types aren’t lost (at least ~165 dendritic cells retained)

- Cell type proportions are preserved

- Computational time is reduced ~10-fold

```r
#-----------------------------------------------
# STEP 3: Downsample to 10% of cells per cell type (exclude Platelets)
#-----------------------------------------------

# Set seed for reproducible downsampling
set.seed(42)

# Get unique cell types and exclude Platelets (too rare for trajectory analysis)
all_cell_types <- unique(seurat_full$final_annotation)
cell_types <- all_cell_types[all_cell_types != "Platelets"]

cat("Cell types to analyze (excluding Platelets):\n")
print(cell_types)
cat("\n")

# Sample 10% from each cell type
downsampled_cells <- c()

for (cell_type in cell_types) {
  # Get cells of this type
  cells_of_type <- colnames(seurat_full)[seurat_full$final_annotation == cell_type]

  # Calculate 10% (minimum 50 cells for rare types)
  n_sample <- max(50, round(length(cells_of_type) * 0.1))

  # Don't sample more cells than exist
  n_sample <- min(n_sample, length(cells_of_type))

  # Random sample
  sampled <- sample(cells_of_type, n_sample)
  downsampled_cells <- c(downsampled_cells, sampled)

  cat(sprintf("  %s: %d → %d cells (%.1f%%)\n", 
              cell_type, length(cells_of_type), n_sample, 
              100 * n_sample / length(cells_of_type)))
}

# Create downsampled Seurat object
seurat_subset <- subset(seurat_full, cells = downsampled_cells)

cat("\n")
cat("Downsampled dataset:\n")
cat("  Total cells:", ncol(seurat_subset), "\n")
cat("  Cell types retained:", length(unique(seurat_subset$final_annotation)), "\n")
cat("  Reduction from full dataset:", 
    round(100 * (1 - ncol(seurat_subset)/ncol(seurat_full)), 1), "%\n")

# Verify cell type representation
cat("\nCell type distribution (downsampled):\n")
print(table(seurat_subset$final_annotation))
```

**Expected output:**

```
Cell types to analyze (excluding Platelets):
[1] "B cells"             "CD4+ T cells"        "CD8+ T cells"        
[4] "Classical Monocytes" "Monocytes"           "NK cells"            
[7] "T cells"            

  B cells: 6735 → 674 cells (10.0%)
  CD4+ T cells: 31379 → 3138 cells (10.0%)
  CD8+ T cells: 3132 → 313 cells (10.0%)
  Classical Monocytes: 5218 → 522 cells (10.0%)
  Monocytes: 2352 → 235 cells (10.0%)
  NK cells: 16067 → 1607 cells (10.0%)
  T cells: 7727 → 773 cells (10.0%)

Downsampled dataset:
  Total cells: 7262 
  Cell types retained: 7 
  Reduction from full dataset: 90.0%

Cell type distribution (downsampled):
            B cells        CD4+ T cells        CD8+ T cells Classical Monocytes 
                674                3138                 313                 522 
          Monocytes             NK cells             T cells 
                235                1607                 773
```

**Why this sampling strategy works:**

- ✅ **Proportional representation**: Each cell type maintains its relative abundance

- ✅ **Rare types preserved**: Even Monocytes (3% of data) have 235 cells

- ✅ **Computational feasibility**: 7,262 cells processable in minutes vs hours

- ✅ **Reproducible**: Fixed seed ensures consistent results

**Important note**: For publication-quality analysis, you would typically run on the full dataset after validating parameters on the downsampled version. This tutorial uses the downsampled data throughout for pedagogical clarity and faster execution.

### Visualizing the Downsampled Dataset

Before trajectory analysis, let’s verify the downsampled data maintains the biological structure:

```r
#-----------------------------------------------
# STEP 4: Visualize downsampled data
#-----------------------------------------------

# Check which UMAP is available (from Part 3 integration)
available_reductions <- names(seurat_subset@reductions)
if ("umap.harmony" %in% available_reductions) {
  umap_reduction <- "umap.harmony"
} else if ("umap.cca" %in% available_reductions) {
  umap_reduction <- "umap.cca"
} else if ("umap.rpca" %in% available_reductions) {
  umap_reduction <- "umap.rpca"
} else if ("umap.mnn" %in% available_reductions) {
  umap_reduction <- "umap.mnn"
} else {
  umap_reduction <- "umap"
}

cat("Using UMAP reduction:", umap_reduction, "\n\n")

# Create multi-panel overview
p1 <- DimPlot(seurat_subset, reduction = umap_reduction,
              group.by = "final_annotation", label = TRUE, 
              label.size = 3.5, pt.size = 0.8, repel = TRUE) +
  ggtitle("Cell Types (Downsampled Data)") +
  theme(plot.title = element_text(face = "bold", size = 14),
        legend.position = "right")

p2 <- DimPlot(seurat_subset, reduction = umap_reduction,
              group.by = "condition", pt.size = 0.8) +
  ggtitle("Condition Distribution") +
  scale_color_manual(values = c("Healthy" = "#2E86AB", 
                                 "Post_Treatment" = "#F18F01")) +
  theme(plot.title = element_text(face = "bold", size = 14))

p3 <- DimPlot(seurat_subset, reduction = umap_reduction,
              group.by = "sample_id", pt.size = 0.8) +
  ggtitle("Sample Distribution") +
  theme(plot.title = element_text(face = "bold", size = 14),
        legend.text = element_text(size = 8))

# Combine plots
p_overview <- (p1 | p2) / (p3 | plot_spacer())

ggsave("plots/01_downsampled_data_overview.png", p_overview,
       width = 16, height = 12, dpi = 300)

cat("Saved: plots/01_downsampled_data_overview.png\n")
```

**What to look for in this plot:**

- **Top left (Cell Types)**: All 7 cell types should be visible and spatially separated

- **Top right (Condition)**: Healthy and Post-Treatment should be well-mixed (good integration)

- **Bottom left (Samples)**: All 8 samples should be represented

**Critical validation**: If cell types aren’t spatially separated or samples show strong batch effects, go back to [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) (Integration) before proceeding with trajectory analysis. Trajectories learned from batch-driven gradients are artifacts, not biology!

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/01_downsampled_data_overview.png?resize=1024%2C768&ssl=1)

---

## Trajectory Inference with Monocle 3: Step-by-Step Workflow

### Converting Seurat Object to Monocle 3 CDS Format

Monocle 3 uses a different data structure called `cell_data_set` (CDS). We’ll use `SeuratWrappers` for easy conversion that automatically preserves:

- Expression data (counts, normalized data)

- Cell metadata (cell types, conditions, samples)

- Gene metadata

- Dimensionality reductions (PCA, UMAP)

```r
#-----------------------------------------------
# STEP 5: Convert Seurat to Monocle 3 CDS object using SeuratWrappers
#-----------------------------------------------

# Convert Seurat object to cell_data_set
# This automatically transfers counts, metadata, and reductions
cds <- as.cell_data_set(seurat_subset)

cat("Created Monocle 3 cell_data_set using SeuratWrappers:\n")
cat("  Cells:", ncol(cds), "\n")
cat("  Genes:", nrow(cds), "\n")
cat("  Cell metadata columns:", ncol(colData(cds)), "\n\n")

# Monocle 3's plot_cells() requires this for gene plotting
# Add gene names to rowData
rowData(cds)$gene_short_name <- rownames(cds)

cat("Fixed gene names for Monocle 3 compatibility\n")
cat("  Gene names set:", "gene_short_name" %in% colnames(rowData(cds)), "\n")
cat("  First 5 genes:", head(rowData(cds)$gene_short_name, 5), "\n\n")

# Verify what was transferred
cat("Verifying transferred information:\n")
cat("  Expression data: counts layer =", 
    !is.null(counts(cds)), "\n")
cat("  Cell metadata: final_annotation =", 
    "final_annotation" %in% colnames(colData(cds)), "\n")
cat("  Gene names: gene_short_name =", 
    "gene_short_name" %in% colnames(rowData(cds)), "\n")
```

**Expected output:**

```
Created Monocle 3 cell_data_set using SeuratWrappers:
  Cells: 7262 
  Genes: 18861 
  Cell metadata columns: 49

Fixed gene names for Monocle 3 compatibility
  Gene names set: TRUE 
  First 5 genes: MIR1302-2HG FAM138A OR4F5 AL627309.1 AL627309.3 

Verifying transferred information:
  Expression data: counts layer = TRUE 
  Cell metadata: final_annotation = TRUE 
  Gene names: gene_short_name = TRUE
```

**Understanding the CDS object:** CDS is based on SingleCellExperiment (see [Part 6](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/) for object structure details)

```bash
# The CDS object structure (from SingleCellExperiment)
# - assays: Expression matrices (counts, logcounts, etc.)
# - colData: Cell metadata (cell types, conditions, etc.)
# - rowData: Gene metadata (gene names, IDs, etc.)
# - reducedDims: Dimensionality reductions (PCA, UMAP, etc.)
```

**Alternative: Manual conversion (for reference)**

If you need more control or SeuratWrappers isn’t available, you can build CDS manually:

```r
# Manual approach (optional - not used in this tutorial)
counts_matrix <- GetAssayData(seurat_subset, layer = "counts")
cell_metadata <- seurat_subset@meta.data
gene_metadata <- data.frame(
  gene_short_name = rownames(seurat_subset),  # ← Explicitly sets gene_short_name
  row.names = rownames(seurat_subset)
)

cds_manual <- new_cell_data_set(
  expression_data = counts_matrix,
  cell_metadata = cell_metadata,
  gene_metadata = gene_metadata
)
```

### Preprocessing: Normalization and Dimensionality Reduction

Monocle 3 needs to perform its own preprocessing to prepare for trajectory learning:

```r
#-----------------------------------------------
# STEP 6: Preprocess data (normalization and PCA)
#-----------------------------------------------

# Normalize and reduce dimensions
cds <- preprocess_cds(
  cds,
  num_dim = 50,              # Number of PCA dimensions (same as Seurat)
  norm_method = "log",       # Log-normalization (matching Seurat)
  use_genes = NULL,          # Use all genes (can specify variable features)
  scaling = TRUE,            # Scale genes (mean = 0, variance = 1)
  verbose = FALSE
)
```

**What `preprocess_cds` does:**

1. **Size factor normalization**: Accounts for library size differences between cells

2. **Log transformation**: `log(count + 1)` to stabilize variance

3. **Scaling**: Centers and scales each gene (important for PCA)

4. **PCA**: Projects data into 50 dimensions capturing major variation

**Parameter notes:**

- `num_dim = 50`: Match the number used in Seurat (Parts 2-3)

- `norm_method = "log"`: Standard for scRNA-seq; alternatives include “size_only”

- `use_genes = NULL`: Uses all genes; can specify variable features for efficiency

### Aligning to Original Seurat UMAP Coordinates

**Critical decision**: Where should we visualize trajectories?

**Option 1**: Let Monocle 3 create its own UMAP

- ❌ Results in different coordinates than all previous analyses

- ❌ Difficult to compare with Part 3-4 results

- ❌ Confusing for interpretation

**Option 2**: Use the original UMAP from Seurat (recommended)

- ✅ Maintains consistency with all previous visualizations

- ✅ Easy to compare trajectories with cell type annotations

- ✅ Leverages high-quality integration from Part 3

**We’ll use Option 2**: Ensure Seurat UMAP coordinates are in the CDS object.

```r
#-----------------------------------------------
# STEP 7: Verify/transfer original UMAP coordinates to CDS
#-----------------------------------------------

# Check if SeuratWrappers already transferred UMAP
existing_reductions <- names(reducedDims(cds))
cat("Reductions in CDS after SeuratWrappers conversion:\n")
print(existing_reductions)
cat("\n")

# SeuratWrappers should have transferred UMAP, but let's verify and ensure
# it's named correctly for Monocle 3

# Get the UMAP from Seurat object (our original high-quality UMAP)
umap_coords <- seurat_subset@reductions[[umap_reduction]]@cell.embeddings

# Verify dimensions match
cat("UMAP coordinates dimension:", dim(umap_coords), "\n")
cat("Expected (cells, 2):", c(ncol(cds), 2), "\n\n")

# Add/overwrite UMAP in CDS object
# Monocle 3 expects reducedDims to be named "UMAP" for visualization
reducedDims(cds)[["UMAP"]] <- umap_coords

cat("UMAP coordinates set in CDS object\n")
cat("Available reductions:", names(reducedDims(cds)), "\n")
```

**Expected output:**

```
Reductions in CDS after SeuratWrappers conversion:
[1] "PCA" "INTEGRATED.CCA" "UMAP.CCA"

UMAP coordinates dimension: 7262 2 
Expected (cells, 2): 7262 2 

UMAP coordinates set in CDS object
Available reductions: PCA INTEGRATED.CCA UMAP.CCA UMAP
```

**Why this matters:**

- Trajectories will be overlaid on the **same UMAP** you’ve been using since Part 3

- Cell types, clusters, and conditions maintain their familiar positions

- Enables direct comparison: “Does the trajectory follow the T cell activation gradient I saw before?”

**Technical note**:

- SeuratWrappers typically transfers PCA and UMAP automatically

- We explicitly set UMAP to ensure it’s the correct reduction and properly named

- The UMAP reduction must be named exactly “UMAP” for `plot_cells()` to use it automatically

**What if SeuratWrappers didn’t transfer UMAP?**

If `existing_reductions` is empty or doesn’t include UMAP, the code above adds it manually from the Seurat object. This ensures compatibility regardless of SeuratWrappers version.

### Learning the Trajectory Graph Structure

Now comes the core of trajectory inference: learning the principal graph that represents developmental paths.

**What is the principal graph?**

- A tree-like or graph structure that fits through the cloud of cells

- Represents the “skeleton” of the developmental process

- Cells are then projected onto this graph to determine their position

**Monocle 3’s graph learning algorithm:**

1. Identifies nearest neighbors in reduced dimension space

2. Constructs a graph connecting similar cells

3. Partitions the graph into connected components

4. Learns a principal curve/tree within each partition

5. Simplifies to a minimal spanning tree

**Important**: Before learning the graph, Monocle 3 requires clustering cells to identify partitions.

```r
#-----------------------------------------------
# STEP 8: Cluster cells and learn principal graph
#-----------------------------------------------

# First, cluster cells (required before learn_graph)
# This identifies partitions and communities in the data
cds <- cluster_cells(
  cds,
  reduction_method = "UMAP",
  k = 20,                      # Number of nearest neighbors
  cluster_method = "louvain",  # Clustering algorithm
  num_iter = 2,                # Iterations for refinement
  partition_qval = 0.05,       # Significance threshold for partitions
  verbose = FALSE
)

cat("Number of partitions:", length(unique(partitions(cds))), "\n")
cat("Number of clusters:", length(unique(clusters(cds))), "\n\n")

# Now learn the trajectory graph structure
cds <- learn_graph(
  cds,
  use_partition = TRUE,         # Learn separate graphs per partition
  close_loop = FALSE,            # Don't force circular trajectories
  learn_graph_control = list(
    ncenter = 500,               # Number of centers for graph construction
    nn.k = 15,                   # k-nearest neighbors
    prune_graph = TRUE           # Remove spurious connections
  ),
  verbose = FALSE
)
```

**Expected output:**

```
Number of partitions: 3 
Number of clusters: 26 
```

**Understanding parameters:**

**cluster_cells parameters:**

- `reduction_method = "UMAP"`: Use UMAP coordinates for clustering

- `k = 20`: Number of nearest neighbors for graph construction

- `cluster_method = "louvain"`: Algorithm for community detection

- `partition_qval = 0.05`: Significance for splitting into partitions (lower = more partitions)

**learn_graph parameters:**

- `use_partition = TRUE`: Cells in different partitions won’t be connected

- `close_loop = FALSE`: For linear/branched trajectories (set TRUE for cell cycle)

- `ncenter = 500`: Higher = more detailed graph (but slower and potentially overfitted)

- `nn.k = 15`: Fewer neighbors = less connected graph; more = smoother trajectories

**What are partitions?**

- **Partitions** separate disconnected cellular processes

- Example: Lymphoid and myeloid lineages might be different partitions

- Cells within a partition are connected by continuous transitions

- Cells in different partitions have no inferred developmental relationship

**In our PBMC data**: Typically 1-3 partitions, depending on whether immune lineages are connected through common progenitors or separate.

### Handling Multiple Partitions

**Critical insight**: Your data likely has multiple partitions representing disconnected biological processes. Let’s analyze **all partitions** to capture complete biology.

```r
#-----------------------------------------------
# STEP 9: Analyze all partitions
#-----------------------------------------------

# Count cells per partition
partition_counts <- table(partitions(cds))
cat("Cells per partition:\n")
print(partition_counts)

# Identify which cell types are in each partition
cat("\nCell type distribution by partition:\n")
for (i in 1:length(partition_counts)) {
  partition_cells <- colnames(cds)[partitions(cds) == i]
  cell_types_in_partition <- table(colData(cds)$final_annotation[partitions(cds) == i])
  cat("\nPartition", i, "(", partition_counts[i], "cells ):\n")
  print(cell_types_in_partition)
}

# Keep all partitions for complete analysis
cds_main <- cds

cat("\nRetaining all partitions for analysis:\n")
cat("  Total cells:", ncol(cds_main), "\n")
cat("  Total partitions:", length(unique(partitions(cds_main))), "\n")
cat("  Genes:", nrow(cds_main), "\n")
```

**Expected output (multiple partitions):**

```
Cells per partition:

   1    2    3 
578 5989  695 

Cell type distribution by partition:

Partition 1 ( 578 cells ):

Classical Monocytes           Monocytes 
                522                  56 

Partition 2 ( 5989 cells ):
CD4+ T cells CD8+ T cells    Monocytes     NK cells      T cells 
        3138          313          158         1607          773

Partition 3 ( 695 cells ):
  B cells Monocytes 
      674        21

Retaining all partitions for analysis:
  Total cells: 7262 
  Total partitions: 3 
  Genes: 18861
```

**Understanding partitions in this data:**

- **Partition 1** (Classical Monocytes + Monocytes): Myeloid lineage trajectory (578 cells)

- **Partition 2** (T cells, NK cells, CD4+/CD8+ T cells): Main adaptive immune trajectory (6,040 cells)

- **Partition 3** (B cells + few Monocytes): B lymphoid lineage (695 cells)

**Why multiple partitions?**

- Different hematopoietic lineages (myeloid vs T/NK vs B lymphoid)

- No intermediate cells connecting them in this peripheral blood dataset

- Each represents a separate biological trajectory

- Monocytes appear in all partitions – may reflect:

  - Biological heterogeneity (different monocyte subtypes)

  - Transitional states

  - Small number of doublets or contamination

**Strategy**: We’ll analyze all partitions to capture complete cellular diversity, demonstrating both manual and automatic root selection methods.

### Understanding Pseudotime and Root Selection

**What is pseudotime?**

Pseudotime is a number assigned to each cell representing its position along a developmental path. Pseudotime = 0 marks the **root** (the starting cell state), and higher values represent more differentiated or activated states. It is not real time — it is an inferred biological progression.

**Each partition has its own independent pseudotime.** Values are not comparable across partitions.

**Choosing a root** means telling Monocle 3 where the trajectory begins:

- **Manual root**: You pick a biologically meaningful starting cell type. In our data, CD4+ T cells include naive/resting populations, making them a good starting point for the T/NK cell trajectory (Partition 2).

- **Automatic root**: You provide a small set of candidate cells and let Monocle 3 decide. Useful when the developmental hierarchy is unclear, as with the Classical Monocyte and B cell partitions.

We have 3 partitions, so we run `order_cells()` three times — once per partition.

### Step 10a: Order Partition 2 (Manual Root — CD4+ T Cells)

Partition 2 is the largest partition (6,040 cells) containing CD4+ T cells, CD8+ T cells, T cells, NK cells, and Monocytes. We use CD4+ T cells as the root because they include naive populations that represent the earliest state in this trajectory.

```r
#-----------------------------------------------
# STEP 10a: Order Partition 2 - manual root (CD4+ T cells)
#-----------------------------------------------

# Get all cells in Partition 2
cells_p2 <- colnames(cds_main)[partitions(cds_main) == 2]

# Select CD4+ T cells as root (biologically informed starting point)
root_cells_p2 <- cells_p2[
  colData(cds_main)$final_annotation[partitions(cds_main) == 2] == "CD4+ T cells"
]

cat("Partition 2 root cells (CD4+ T cells):", length(root_cells_p2), "\n")

# Order cells using CD4+ T cells as root
cds_main <- order_cells(
  cds_main,
  root_cells = root_cells_p2,
  verbose = FALSE
)

cat("Partition 2 pseudotime range:",
    round(range(pseudotime(cds_main)[partitions(cds_main) == 2], na.rm = TRUE), 2), "\n")
```

**Expected output:**

```
Partition 2 root cells (CD4+ T cells): 3138
Partition 2 pseudotime range: 0 23.66
```

### Step 10b: Order Partition 1 (Automatic Root — Classical Monocytes)

Partition 1 contains Classical Monocytes and Monocytes (578 cells). The developmental hierarchy is not well established for this isolated myeloid cluster, so we provide 50 candidate cells and let Monocle 3 choose the best root.

```r
#-----------------------------------------------
# STEP 10b: Order Partition 1 - automatic root (Classical Monocytes)
#-----------------------------------------------

# Get all cells in Partition 1
cells_p1 <- colnames(cds_main)[partitions(cds_main) == 1]

# Provide first 50 cells as candidates - Monocle 3 picks the best starting point
root_cells_p1 <- cells_p1[1:min(50, length(cells_p1))]

cat("Partition 1 root candidates provided:", length(root_cells_p1), "\n")

# Order cells
cds_main <- order_cells(
  cds_main,
  root_cells = root_cells_p1,
  verbose = FALSE
)

cat("Partition 1 pseudotime range:",
    round(range(pseudotime(cds_main)[partitions(cds_main) == 1], na.rm = TRUE), 2), "\n")
```

**Expected output:**

```
Partition 1 root candidates provided: 50
Partition 1 pseudotime range: 0 1.2
```

### Step 10c: Order Partition 3 (Automatic Root — B Cells)

Partition 3 contains B cells and a few Monocytes (695 cells). We use the same automatic approach as Partition 1.

```r
#-----------------------------------------------
# STEP 10c: Order Partition 3 - automatic root (B cells)
#-----------------------------------------------

# Get all cells in Partition 3
cells_p3 <- colnames(cds_main)[partitions(cds_main) == 3]

# Provide first 50 cells as candidates
root_cells_p3 <- cells_p3[1:min(50, length(cells_p3))]

cat("Partition 3 root candidates provided:", length(root_cells_p3), "\n")

# Order cells
cds_main <- order_cells(
  cds_main,
  root_cells = root_cells_p3,
  verbose = FALSE
)

cat("Partition 3 pseudotime range:",
    round(range(pseudotime(cds_main)[partitions(cds_main) == 3], na.rm = TRUE), 2), "\n")
```

**Expected output:**

```
Partition 3 root candidates provided: 50
Partition 3 pseudotime range: 0 1.07
```

### Verify All Partitions Are Ordered

```r
#-----------------------------------------------
# Verify pseudotime across all partitions
#-----------------------------------------------

cat("All cells ordered in pseudotime\n")
cat("Cells with NA pseudotime:", sum(is.na(pseudotime(cds_main))), "\n\n")

cat("Summary by partition:\n")
for (i in 1:3) {
  pt_range <- round(range(pseudotime(cds_main)[partitions(cds_main) == i], na.rm = TRUE), 2)
  n <- sum(partitions(cds_main) == i)
  cat("  Partition", i, "(", n, "cells ): pseudotime", pt_range[1], "to", pt_range[2], "\n")
}
```

**Expected output:**

```
All cells ordered in pseudotime
Cells with NA pseudotime: 0

Summary by partition:
  Partition 1 ( 578 cells ): pseudotime 0 to 1.2
  Partition 2 ( 6040 cells ): pseudotime 0 to 23.66
  Partition 3 ( 695 cells ): pseudotime 0 to 1.07
```

**Why are Partitions 1 and 3 pseudotime ranges so narrow?**

Partitions 1 (Classical Monocytes) and 3 (B cells) contain primarily one homogeneous cell type, resulting in a short, simple trajectory. Partition 2 spans five distinct cell types across a wide activation spectrum (naive CD4+ T cells → activated NK cells), producing a much longer trajectory.

**HPC note:** Always provide `root_cells` explicitly in `order_cells()`. Calling it without this parameter launches an interactive browser interface that will fail on servers without a graphical display.

### Visualizing Trajectories Across All Partitions

Now let’s visualize what we’ve learned across all partitions:

```r
#-----------------------------------------------
# STEP 11: Visualize trajectories and pseudotime
#-----------------------------------------------

# Plot 1: Trajectory colored by cell type
p_trajectory_celltype <- plot_cells(
  cds_main,
  color_cells_by = "final_annotation",
  label_cell_groups = TRUE,
  label_groups_by_cluster = FALSE,
  label_leaves = FALSE,
  label_branch_points = FALSE,
  graph_label_size = 3,
  cell_size = 1
) +
  ggtitle("Trajectory Structure (Colored by Cell Type)") +
  theme(plot.title = element_text(face = "bold", size = 14),
        legend.text = element_text(size = 9))

# Plot 2: Trajectory colored by pseudotime
p_trajectory_pseudotime <- plot_cells(
  cds_main,
  color_cells_by = "pseudotime",
  label_cell_groups = FALSE,
  label_leaves = FALSE,
  label_branch_points = TRUE,
  label_roots = TRUE,
  graph_label_size = 4,
  cell_size = 1
) +
  scale_color_viridis(option = "plasma", name = "Pseudotime") +
  ggtitle("Trajectory Colored by Pseudotime") +
  theme(plot.title = element_text(face = "bold", size = 14))

# Plot 3: Trajectory colored by condition
p_trajectory_condition <- plot_cells(
  cds_main,
  color_cells_by = "condition",
  label_cell_groups = FALSE,
  label_leaves = FALSE,
  label_branch_points = FALSE,
  cell_size = 1
) +
  scale_color_manual(values = c("Healthy" = "#2E86AB", 
                                 "Post_Treatment" = "#F18F01")) +
  ggtitle("Trajectory Colored by Condition") +
  theme(plot.title = element_text(face = "bold", size = 14))

# Plot 4: Trajectory colored by partition (if multiple exist)
n_partitions <- length(unique(partitions(cds_main)))

if (n_partitions > 1) {
  p_trajectory_partition <- plot_cells(
    cds_main,
    color_cells_by = "partition",
    label_cell_groups = TRUE,
    label_groups_by_cluster = FALSE,
    label_leaves = FALSE,
    label_branch_points = FALSE,
    cell_size = 1
  ) +
    ggtitle("Trajectory Colored by Partition") +
    theme(plot.title = element_text(face = "bold", size = 14))

  # Combine plots (4-panel)
  p_trajectory_overview <- (p_trajectory_celltype | p_trajectory_pseudotime) / 
                           (p_trajectory_condition | p_trajectory_partition)
} else {
  # Combine plots (3-panel)
  p_trajectory_overview <- (p_trajectory_celltype | p_trajectory_pseudotime) / 
                           (p_trajectory_condition | plot_spacer())
}

ggsave("plots/trajectories/02_trajectory_overview.png", 
       p_trajectory_overview, width = 16, height = 12, dpi = 300) 
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/02_trajectory_overview.png?resize=1024%2C768&ssl=1)

**What to look for in these plots:**

**Plot 1 (Cell Type)**:

- **Main partition** (center): T cells, NK cells, Monocytes with smooth transitions

- **Separate clusters** (corners): Classical Monocytes and B cells disconnected

- Black lines show trajectory paths within each partition

- Do different cell types occupy different regions of their respective trajectories?

**Plot 2 (Pseudotime)**:

- **Color gradient per partition**: Each partition has independent pseudotime (0 to max)

- **Main partition**: Purple (low) → Yellow (high) shows T cell progression

- **Classical Monocytes**: Separate pseudotime scale (automatic root)

- **B cells**: Separate pseudotime scale (automatic root)

- Note: Pseudotime values NOT comparable across partitions!

**Plot 3 (Condition)**:

- Are Healthy and Post-Treatment cells evenly distributed within each partition?

- Does treatment preferentially drive cells along certain trajectories?

- Look for condition-specific enrichment at high vs low pseudotime

**Plot 4 (Partition) – if multiple partitions**:

- Clearly shows which cells belong to which disconnected trajectory

- Partition 2: Largest, contains main immune cell trajectory

- Partitions 1, 3: Separate lineages (Classical Monocytes, B cells)

- Confirms biological separation vs technical artifacts

**Trajectory features visible**:

- **Black lines**: Principal graphs (inferred developmental paths)

- **Branch points**: Where trajectories split (fate decisions) – mainly in Partition 1

- **Leaves**: Terminal states (differentiated cell types)

- **Roots**: Starting points (marked with circles if label_roots = TRUE)

- Partition 2: Manual root in CD4+ T cells

- Partitions 1, 3: Automatic roots

**Key observation**: Multiple disconnected partitions are **biologically meaningful** in PBMC data:

- Partition 2: Adaptive immune cells (T cells, NK cells) with activation/differentiation trajectories

- Partition 1: Myeloid lineage (Classical Monocytes)

- Partition 3: B lymphocyte lineage

These represent distinct hematopoietic lineages that don’t directly interconvert in peripheral blood.

### Creating Custom Pseudotime Visualizations

The default `plot_cells()` is useful, but for publications you often need more control. Let’s create custom plots that properly handle multiple partitions:

```r
#-----------------------------------------------
# STEP 12: Custom pseudotime visualizations
#-----------------------------------------------

# Extract data for custom plotting
plot_data <- data.frame(
  UMAP_1 = reducedDims(cds_main)[["UMAP"]][,1],
  UMAP_2 = reducedDims(cds_main)[["UMAP"]][,2],
  pseudotime = pseudotime(cds_main),
  cell_type = colData(cds_main)$final_annotation,
  condition = colData(cds_main)$condition,
  sample = colData(cds_main)$sample_id,
  partition = factor(partitions(cds_main))
)

# Custom pseudotime plot with ggplot2
p_custom_pseudotime <- ggplot(plot_data, aes(x = UMAP_1, y = UMAP_2, color = pseudotime)) +
  geom_point(size = 1.5, alpha = 0.8) +
  scale_color_viridis(option = "plasma", name = "Pseudotime") +
  labs(title = "Pseudotime Distribution Across All Partitions",
       x = "UMAP 1", y = "UMAP 2") +
  theme_classic(base_size = 12) +
  theme(plot.title = element_text(face = "bold", size = 14),
        legend.position = "right")

ggsave("plots/pseudotime/03_custom_pseudotime.png", 
       p_custom_pseudotime, width = 10, height = 8, dpi = 300)

# Pseudotime distribution by cell type (violin plot)
p_pseudotime_violin <- ggplot(plot_data, aes(x = reorder(cell_type, pseudotime, median), 
                                              y = pseudotime, fill = cell_type)) +
  geom_violin(trim = FALSE, scale = "width", alpha = 0.7) +
  geom_boxplot(width = 0.15, fill = "white", outlier.shape = NA) +
  coord_flip() +
  labs(title = "Pseudotime Distribution by Cell Type",
       subtitle = "Note: Pseudotime not comparable across partitions",
       x = "Cell Type", y = "Pseudotime") +
  theme_classic(base_size = 12) +
  theme(plot.title = element_text(face = "bold", size = 14),
        legend.position = "none")

ggsave("plots/pseudotime/04_pseudotime_by_celltype.png", 
       p_pseudotime_violin, width = 10, height = 6, dpi = 300)

# Pseudotime distribution by condition (density plot with facets for partitions)
n_partitions <- length(unique(plot_data$partition))

if (n_partitions > 1) {
  p_pseudotime_density <- ggplot(plot_data, aes(x = pseudotime, fill = condition)) +
    geom_density(alpha = 0.6) +
    facet_wrap(~ partition, ncol = 1, 
               labeller = labeller(partition = function(x) paste("Partition", x))) +
    scale_fill_manual(values = c("Healthy" = "#2E86AB", 
                                  "Post_Treatment" = "#F18F01")) +
    labs(title = "Pseudotime Distribution by Condition (Per Partition)",
         subtitle = "Each partition has independent pseudotime scale",
         x = "Pseudotime", y = "Density", fill = "Condition") +
    theme_classic(base_size = 12) +
    theme(plot.title = element_text(face = "bold", size = 14))
} else {
  p_pseudotime_density <- ggplot(plot_data, aes(x = pseudotime, fill = condition)) +
    geom_density(alpha = 0.6) +
    scale_fill_manual(values = c("Healthy" = "#2E86AB", 
                                  "Post_Treatment" = "#F18F01")) +
    labs(title = "Pseudotime Distribution by Condition",
         x = "Pseudotime", y = "Density", fill = "Condition") +
    theme_classic(base_size = 12) +
    theme(plot.title = element_text(face = "bold", size = 14))
}

ggsave("plots/pseudotime/05_pseudotime_by_condition.png", 
       p_pseudotime_density, width = 10, height = 8, dpi = 300)

# Additional plot: Partition-specific pseudotime (if multiple partitions)
if (n_partitions > 1) {
  # Create separate plots for each partition
  partition_plots <- list()

  for (i in 1:n_partitions) {
    partition_data <- plot_data[plot_data$partition == i, ]
    cell_types_in_partition <- unique(partition_data$cell_type)

    partition_plots[[i]] <- ggplot(partition_data, 
                                    aes(x = UMAP_1, y = UMAP_2, color = pseudotime)) +
      geom_point(size = 2, alpha = 0.8) +
      scale_color_viridis(option = "plasma", name = "Pseudotime") +
      labs(title = paste0("Partition ", i),
           subtitle = paste("Cell types:", paste(cell_types_in_partition, collapse = ", ")),
           x = "UMAP 1", y = "UMAP 2") +
      theme_classic(base_size = 10) +
      theme(plot.title = element_text(face = "bold", size = 12))
  }

  # Combine partition plots
  p_partition_combined <- wrap_plots(partition_plots, ncol = 2)

  ggsave("plots/pseudotime/06_pseudotime_per_partition.png",
         p_partition_combined, width = 14, height = 10, dpi = 300)
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/monocle3_3456.png?resize=1024%2C678&ssl=1)

**Interpreting the violin plot** (pseudotime by cell type):

- **Wide violins**: Cell type spans broad pseudotime range (cells at many developmental stages)

- **Narrow violins**: Cell type confined to specific pseudotime window (discrete developmental state)

- **Important**: Only compare cell types **within the same partition**

- CD4+ T cells, NK cells, T cells in Partition 2 are comparable

- Classical Monocytes (Partition 1) have independent pseudotime scale

- B cells (Partition 3) have independent pseudotime scale

**Interpreting the density plot** (pseudotime by condition):

- **Partition-specific facets**: Each partition shown separately with its own pseudotime scale

- **Overlapping peaks**: Both conditions have cells at similar developmental stages

- **Shifted peaks**: Treatment drives cells toward earlier or later pseudotime

- **Different shapes**: Treatment changes distribution of cells along trajectory

---

## Interpreting Trajectory Analysis Results

### Understanding What Trajectories Represent

**Critical conceptual point**: A trajectory is a **model**, not ground truth.

**What trajectories tell us:**

✅ The most likely **path** cells take through gene expression space✅ The **order** of gene expression changes during a process✅ **Branch points** where cells diverge into different fates✅ Candidate **driver genes** that change along the path

**What trajectories do NOT tell us:**

❌ Absolute time (pseudotime ≠ hours/days)❌ Causal mechanisms (correlation ≠ causation)❌ Whether all cells actually traverse the full path❌ Directionality (without additional evidence)

### Biological Validation: Does the Trajectory Make Sense?

Before interpreting results, validate that the trajectory is biologically plausible:

**1. Does the trajectory structure match known biology?**

```r
#-----------------------------------------------
# STEP 13: Validation - trajectory structure per partition
#-----------------------------------------------

# Count cells in each cell type along pseudotime
# Group by partition for proper interpretation
pseudotime_summary <- plot_data %>%
  group_by(partition, cell_type) %>%
  summarise(
    n_cells = n(),
    median_pseudotime = median(pseudotime, na.rm = TRUE),
    min_pseudotime = min(pseudotime, na.rm = TRUE),
    max_pseudotime = max(pseudotime, na.rm = TRUE),
    .groups = 'drop'
  ) %>%
  arrange(partition, median_pseudotime)

cat("Pseudotime summary by cell type and partition:\n")
print(as.data.frame(pseudotime_summary))
```

**Expected pattern for multiple partitions:**

```bash
# Pseudotime summary by partition and cell type
  partition           cell_type n_cells median_pseudotime min_pseudotime max_pseudotime
1         1 Classical Monocytes     522         0.1538738   0.000000e+00      1.2040175
2         1           Monocytes      56         0.1921420   5.064492e-03      0.8355923
3         2        CD4+ T cells    3138         0.2238169   0.000000e+00      5.6224804
4         2           Monocytes     158         1.4736496   2.779210e-06     23.5694842
5         2             T cells     773         3.3443203   0.000000e+00     23.3278436
6         2        CD8+ T cells     313         6.0913848   5.558420e-06      6.7880647
7         2            NK cells    1607        10.6601735   1.620208e+00     23.6647194
8         3             B cells     674         0.2204692   0.000000e+00      1.0740133
9         3           Monocytes      21         0.4649489   4.405602e-06      0.8723964
```

**Red flags** (suggests trajectory may not be biological):

- Terminally differentiated cells at pseudotime = 0 **within their partition** (should be high)

- Progenitor-like cells at highest pseudotime **within their partition** (should be low)

- No correlation between known activation markers and pseudotime

- Trajectory driven by technical factors (batch, library size, %mitochondrial genes)

**2. Do known marker genes align with trajectory?**

```r
#-----------------------------------------------
# STEP 14: Validation - marker gene expression
#-----------------------------------------------

# Test known markers along pseudotime
# Naive T cell markers should decrease: CCR7, LEF1, TCF7
# Activation markers should increase: CD69, HLA-DRA, IL2RA

marker_genes <- c("CCR7", "LEF1", "CD69", "HLA-DRA", "IL2RA", "GZMB")

# Check if markers are in dataset
marker_genes <- marker_genes[marker_genes %in% rownames(cds_main)]

if (length(marker_genes) > 0) {
  # Plot each marker along pseudotime
  for (gene in marker_genes) {
    p <- plot_cells(
      cds_main,
      genes = gene,
      label_cell_groups = FALSE,
      show_trajectory_graph = TRUE,
      cell_size = 1
    ) +
      scale_color_viridis(option = "magma", name = "Expression") +
      ggtitle(paste0("Expression of ", gene, " Along Trajectory"))

    ggsave(paste0("plots/genes_over_pseudotime/marker_", gene, ".png"),
           p, width = 8, height = 6, dpi = 300)
  }
  cat("Saved marker gene trajectory plots\n")
} else {
  cat("Marker genes not found in dataset\n")
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/monocle3_ccr7_gzmb-e1770866075293-1024x381.png?resize=1024%2C381&ssl=1)

**Expected patterns**:

- **CCR7, LEF1**: High at low pseudotime (naive state), decrease along trajectory

- **CD69, HLA-DRA**: Low at low pseudotime, increase (activation)

- **GZMB**: Low early, high late (effector function)

**If markers don’t follow expected patterns**: Trajectory may be capturing something other than the biological process you expected (e.g., cell cycle, batch effects, or a different biological process).

**3. Does treatment affect pseudotime distribution?**

```r
#-----------------------------------------------
# STEP 15: Validation - condition effects
#-----------------------------------------------

# Statistical test: Do conditions differ in pseudotime distribution?
wilcox_test <- wilcox.test(pseudotime ~ condition, data = plot_data)

cat("\nWilcoxon test (Healthy vs Post-Treatment pseudotime):\n")
cat("  W statistic:", wilcox_test$statistic, "\n")
cat("  P-value:", format(wilcox_test$p.value, scientific = TRUE), "\n")

# Calculate median pseudotime per condition
median_summary <- plot_data %>%
  group_by(condition) %>%
  summarise(
    median_pseudotime = median(pseudotime, na.rm = TRUE),
    mean_pseudotime = mean(pseudotime, na.rm = TRUE)
  )

cat("\nMedian pseudotime by condition:\n")
print(median_summary)
```

**Example Output:**

```
Wilcoxon test (Healthy vs Post-Treatment pseudotime):

  W statistic: 6726000
  P-value: 1.297578e-01

Median pseudotime by condition:

  condition      median_pseudotime mean_pseudotime
1 Healthy                    0.678            3.78
2 Post_Treatment             0.652            3.35
```

**Interpretation:**

- **P < 0.05**: Treatment significantly shifts cells along trajectory

- **Higher pseudotime in Post-Treatment**: Treatment drives differentiation/activation

- **Lower pseudotime in Post-Treatment**: Treatment returns cells to earlier state

- **No difference**: Treatment effect independent of this trajectory

### Understanding Branch Points and Cell Fate Decisions

If your trajectory has branches, these represent critical decision points:

```r
#-----------------------------------------------
# STEP 16: Analyze branch points (if present)
#-----------------------------------------------

# Check for branches in the graph
graph_test_results <- graph_test(
  cds_main,
  neighbor_graph = "principal_graph",
  cores = 1
)

# Identify genes with significant spatial autocorrelation (branch-associated)
sig_genes <- subset(graph_test_results, q_value < 0.05)

cat("\nGraph test results:\n")
cat("  Total genes tested:", nrow(graph_test_results), "\n")
cat("  Significant genes (q < 0.05):", nrow(sig_genes), "\n")

if (nrow(sig_genes) > 0) {
  cat("\nTop 10 branch-associated genes (by Moran's I):\n")
  top_genes <- head(sig_genes[order(sig_genes$morans_I, decreasing = TRUE), ], 10)
  print(top_genes[, c("gene_short_name", "morans_I", "q_value")])
}
```

**Example Output:**

```
Graph test results:

  Total genes tested: 18861
  Significant genes (q &lt; 0.05): 8992

Top 10 branch-associated genes (by Moran's I):

        gene_short_name  morans_I q_value
CD74               CD74 0.8354366       0
LYZ                 LYZ 0.7947756       0
NKG7               NKG7 0.7895408       0
BANK1             BANK1 0.7815003       0
HLA-DRA         HLA-DRA 0.7807247       0
AFF3               AFF3 0.7702808       0
CD79A             CD79A 0.7613293       0
LST1               LST1 0.7536135       0
FTL                 FTL 0.7531880       0
IGHM               IGHM 0.7401368       0
```

**Understanding graph_test output:**

- **Moran’s I**: Spatial autocorrelation statistic

- High values (close to 1): Gene expression changes smoothly along trajectory

- Low values (close to 0): Gene expression independent of trajectory position

- **q_value**: FDR-adjusted p-value

- q < 0.05: Gene significantly associated with trajectory structure

**Biological interpretation:**

- Genes with high Moran’s I are **trajectory markers**: They define the developmental path

- Genes specific to one branch are **fate determinants**: They specify cell fate choices

- Can be used to identify **driver transcription factors**: Regulators of differentiation

### Integrating Results Back to Seurat Object

For downstream analysis and cross-referencing with other Parts, add pseudotime to your Seurat object:

```r
#-----------------------------------------------
# STEP 17: Add pseudotime to Seurat metadata
#-----------------------------------------------

# Create pseudotime vector with cell names (includes all partitions)
pseudotime_vector <- pseudotime(cds_main)
names(pseudotime_vector) <- colnames(cds_main)

# Also add partition information
partition_vector <- partitions(cds_main)
names(partition_vector) <- colnames(cds_main)

# Add to Seurat object
seurat_subset$monocle3_pseudotime <- NA  # Initialize
seurat_subset$monocle3_partition <- NA   # Initialize

seurat_subset$monocle3_pseudotime[colnames(cds_main)] <- pseudotime_vector
seurat_subset$monocle3_partition[colnames(cds_main)] <- as.character(partition_vector)

# Visualize pseudotime in Seurat
p_seurat_pseudotime <- FeaturePlot(
  seurat_subset,
  features = "monocle3_pseudotime",
  reduction = umap_reduction,
  pt.size = 1
) +
  scale_color_viridis(option = "plasma", name = "Pseudotime") +
  ggtitle("Monocle 3 Pseudotime (All Partitions in Seurat)")

ggsave("plots/pseudotime/07_pseudotime_in_seurat.png",
       p_seurat_pseudotime, width = 10, height = 8, dpi = 300)

# Also visualize partition assignment
p_seurat_partition <- DimPlot(
  seurat_subset,
  group.by = "monocle3_partition",
  reduction = umap_reduction,
  pt.size = 1
) +
  ggtitle("Monocle 3 Partition Assignment (in Seurat)")

ggsave("plots/pseudotime/08_partition_in_seurat.png",
       p_seurat_partition, width = 10, height = 8, dpi = 300)

# Save updated Seurat object
saveRDS(seurat_subset, "results/seurat_with_pseudotime.rds")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/monocle3_seurat_78-e1770866351854-1024x408.png?resize=1024%2C408&ssl=1)

**Why integrate back to Seurat?**

- Use Seurat’s powerful visualization tools with pseudotime

- Cross-reference with differential expression results (Part 5)

- **Partition information** enables partition-specific analyses

- Subset cells by pseudotime windows for focused analysis

- Correlate pseudotime with other metadata (sample, condition, QC metrics)

**Important note**: The Seurat object now contains:

- `monocle3_pseudotime`: Pseudotime values (remember: NOT comparable across partitions!)

- `monocle3_partition`: Partition assignment (1, 2, 3, etc.)

---

## Identifying Genes That Change Along Trajectories

### Finding Genes Correlated with Pseudotime

The most basic trajectory analysis: Which genes increase or decrease along pseudotime?

```r
#-----------------------------------------------
# STEP 18: Identify pseudotime-dependent genes
#-----------------------------------------------

# Fit generalized additive models (GAMs) to identify genes changing with pseudotime
trajectory_genes <- graph_test(
  cds_main,
  neighbor_graph = "principal_graph",
  cores = 1,
  verbose = FALSE
)

# Filter for significant genes
sig_trajectory_genes <- subset(trajectory_genes, q_value < 0.05)

cat("Genes significantly associated with trajectory:\n")
cat("  Total tested:", nrow(trajectory_genes), "\n")
cat("  Significant (q < 0.05):", nrow(sig_trajectory_genes), "\n")
cat("  Top genes by Moran's I:\n")

# Show top 20 genes
top20_genes <- head(sig_trajectory_genes[order(sig_trajectory_genes$morans_I, decreasing = TRUE), ], 20)
print(top20_genes[, c("gene_short_name", "morans_I", "q_value")])

# Save results
write.csv(sig_trajectory_genes, 
          "results/trajectory_associated_genes.csv",
          row.names = FALSE)
```

**Expected output:**

```
Genes significantly associated with trajectory:
  Total tested: 18861 
  Significant (q &lt; 0.05): 8992 

  Top genes by Moran's I:
        gene_short_name  morans_I q_value
CD74               CD74 0.8354366       0
LYZ                 LYZ 0.7947756       0
NKG7               NKG7 0.7895408       0
BANK1             BANK1 0.7815003       0
...
```

**Interpreting Moran’s I:**

- **0.4-0.5**: Very strong spatial autocorrelation (classic trajectory markers)

- **0.3-0.4**: Strong association (important trajectory genes)

- **0.2-0.3**: Moderate association (may be trajectory-relevant)

- **< 0.2**: Weak association (likely not trajectory-specific)

### Visualizing Top Trajectory Genes

```r
#-----------------------------------------------
# STEP 19: Visualize top trajectory genes
#-----------------------------------------------

# Select top 12 genes for visualization
top_genes_to_plot <- head(top20_genes$gene_short_name, 12)

# Create multi-panel plot
gene_plots <- list()
for (i in 1:length(top_genes_to_plot)) {
  gene <- top_genes_to_plot[i]

  gene_plots[[i]] <- plot_cells(
    cds_main,
    genes = gene,
    label_cell_groups = FALSE,
    show_trajectory_graph = TRUE,
    cell_size = 0.75
  ) +
    scale_color_viridis(option = "magma", name = "Expression") +
    ggtitle(gene) +
    theme(plot.title = element_text(size = 10, face = "bold"),
          legend.position = "right",
          legend.key.size = unit(0.3, "cm"))
}

# Combine into grid
p_top_genes <- wrap_plots(gene_plots, ncol = 3)

ggsave("plots/genes_over_pseudotime/07_top_trajectory_genes.png",
       p_top_genes, width = 14, height = 12, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/07_top_trajectory_genes.png?resize=1024%2C878&ssl=1)

**What to look for:**

- **Gradual color transitions**: Gene expression changes smoothly along trajectory

- **Spatial patterns**: Expression highest at specific pseudotime windows

- **Biological coherence**: Do these genes make sense for the process?

---

## Best Practices and Common Pitfalls in Trajectory Analysis

### Best Practices for Robust Trajectory Inference

**1. Start with high-quality, integrated data**

✅ **Do:**

- Use data that passed stringent QC ([Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/))

- Ensure batch effects are corrected ([Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Verify cell type annotations are accurate ([Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/))

❌ **Don’t:**

- Run trajectory analysis on unfiltered data (doublets, low-quality cells)

- Use data with strong batch effects (trajectories will follow batch gradients)

- Proceed if cell types don’t make biological sense

**2. Ensure your dataset contains the full trajectory**

✅ **Do:**

- Include cells across all developmental stages (early → late)

- Verify intermediate states exist (not just endpoints)

- Check that trajectory connects expected start and end points

❌ **Don’t:**

- Attempt trajectory analysis with only terminally differentiated cells

- Assume a trajectory exists without biological evidence

- Force unrelated cell types onto a single trajectory

**3. Validate biological plausibility**

✅ **Do:**

- Check that known markers align with pseudotime

- Verify trajectory direction makes biological sense

- Compare with published developmental hierarchies

- Test multiple parameter settings to assess robustness

❌ **Don’t:**

- Accept default results without validation

- Ignore trajectories that contradict known biology

- Over-interpret technical artifacts as biological signals

**4. Use appropriate sample sizes**

✅ **Do:**

- Ensure ≥100 cells per developmental stage (more is better)

- Include biological replicates to validate trajectory consistency

- Use sufficient cells to capture rare intermediate states

❌ **Don’t:**

- Run trajectory analysis on <500 total cells (underpowered)

- Rely on single samples (batch effects may confound)

- Expect to resolve rare transitions without sufficient cells

**5. Integrate with orthogonal evidence**

✅ **Do:**

- Correlate pseudotime with known temporal markers

- Validate with RNA velocity (future transcriptional state)

- Compare with lineage tracing if available

- Test predictions experimentally (perturbations, time-course)

❌ **Don’t:**

- Claim causality based on trajectory alone

- Ignore contradictions with other data modalities

- Present trajectory as proof without supporting evidence

**6. Handle multiple partitions appropriately**

✅ **Do:**

- Analyze all biologically meaningful partitions separately

- Use partition-specific root selection (manual or automatic as appropriate)

- Report pseudotime separately per partition (values NOT comparable across partitions)

- Validate that partitions represent real biology (not batch effects)

- Visualize each partition’s trajectory independently

❌ **Don’t:**

- Ignore small partitions without investigating what they contain

- Compare pseudotime values across partitions (fundamentally incomparable)

- Force unrelated partitions onto single trajectory (use partition_qval carefully)

- Assume partitions are artifacts (may be genuine biological separation)

### Common Pitfalls and How to Avoid Them

**Pitfall 1: Trajectory driven by batch effects, not biology**

**Diagnosis:**

```bash
# Check if pseudotime correlates with batch
cor.test(plot_data$pseudotime, as.numeric(factor(plot_data$sample)))
```

**Symptoms:**

- Cells from same batch cluster at same pseudotime

- Pseudotime gradient follows sample/batch structure

- Batch-specific genes have highest Moran’s I

**Solutions:**

- Re-run integration with stronger batch correction ([Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Regress out batch effects before trajectory inference

- Use batch-balanced downsampling

**Pitfall 2: Cell cycle confounded with differentiation**

**Diagnosis:**

```bash
# Check cell cycle phase distribution along pseudotime
table(colData(cds_main)$Phase, cut(pseudotime(cds_main), breaks = 5))
```

**Symptoms:**

- Cell cycle genes (MKI67, TOP2A) dominate trajectory genes

- G2/M cells enriched at specific pseudotime

- Trajectory reflects proliferation, not differentiation

**Solutions:**

- Regress out cell cycle scores (Seurat: CellCycleScoring + ScaleData)

- Subset to non-cycling cells (G1 phase only)

- Use cell cycle-agnostic dimension reduction

**Pitfall 3: Choosing wrong root cells**

**Diagnosis:**

```bash
# Check if "mature" markers are high at pseudotime=0
# Should be LOW at root
```

**Symptoms:**

- Differentiated cells have pseudotime = 0

- Progenitor markers high at high pseudotime (backward)

- Biological interpretation doesn’t make sense

**Solutions:**

- Re-run `order_cells()` with correct root specification

- Use known progenitor cells as root (naive, stem, resting)

- Validate root choice with marker gene expression

**Pitfall 4: Over-interpreting noise as biological variation**

**Diagnosis:**

```bash
# Check trajectory genes for biological coherence
# Are top genes known to be involved in this process?
```

**Symptoms:**

- Top trajectory genes are ribosomal proteins (technical)

- Mitochondrial genes highly ranked (stress/quality)

- No biological pathway enrichment in trajectory genes

**Solutions:**

- Remove technical confounders (ribosomal, mitochondrial genes)

- Increase `ncenter` parameter (smoother trajectory)

- Filter genes by minimum expression and prevalence

**Pitfall 5: Forcing disconnected processes onto single trajectory**

**Diagnosis:**

```bash
# Check number of partitions
length(unique(partitions(cds)))
```

**Symptoms:**

- Multiple disconnected partitions (e.g., 3 partitions in PBMC data)

- Unrelated cell types connected by spurious edges

- Trajectory spans biologically impossible transitions (e.g., T cells → B cells)

**Solutions:**

- Analyze each partition separately (as shown in Steps 9-10)

- Validate that partitions represent real biology (check cell type composition)

- Use partition-specific root selection (manual for known hierarchies, automatic for unclear ones)

- **Do NOT force partitions together** by setting `partition_qval = 1.0` without biological justification

**Example from this tutorial:**

- Partition 1: T cells, NK cells, Monocytes (connected immune trajectory) ✅

- Partition 2: Classical Monocytes (distinct myeloid lineage) ✅

- Partition 3: B cells (separate lymphoid lineage) ✅

- These SHOULD be separate—forcing connection would create artificial biology!

**Pitfall 6: Misinterpreting pseudotime across partitions**

**Diagnosis:**

```bash
# Check if you're comparing pseudotime across partitions
plot_data %>% group_by(partition) %>% summarise(max_pt = max(pseudotime))
```

**Symptoms:**

- Comparing median pseudotime of B cells (Partition 3) vs T cells (Partition 1)

- Claiming “B cells are earlier in development than T cells” based on lower pseudotime values

- Merging partition-specific analyses without accounting for independent scales

**Solutions:**

- **Never compare pseudotime values across partitions** (they’re on different scales!)

- Analyze each partition independently

- Report findings partition-specifically: “Within the T cell trajectory (Partition 1), NK cells are at later pseudotime…”

- If biological connections exist between partitions, validate with orthogonal data

**Pitfall 7: Insufficient cells for complex trajectories**

**Symptoms:**

- Trajectory graph is overly simplistic (straight line) despite expecting branches

- Many cells have NA pseudotime

- High variance in pseudotime between biological replicates

**Solutions:**

- Increase cell numbers (use full dataset, not downsampled)

- Merge technical replicates to increase power

- Focus on subset of trajectory with sufficient cells

### Troubleshooting: Trajectory Doesn’t Match Expectations

**“I expected a branched trajectory but got a linear one”**

Possible causes:

- Insufficient cells at branch point

- Branch point exists but wasn’t captured in sampling

- Biology is actually linear (assumption was wrong)

- Parameter `ncenter` too low (increase to 1000-2000)

**“Trajectory connects cells that shouldn’t be related”**

Possible causes:

- Batch effects creating artificial continuity

- Need to subset to biologically related cell types only

- UMAP/PCA doesn’t separate populations well (redo with more PCs)

**“Pseudotime doesn’t correlate with known markers”**

Possible causes:

- Wrong root cells chosen (reverse the trajectory)

- Process isn’t actually occurring (static cell types)

- Markers are cell type-specific, not trajectory-specific

**“All genes have low Moran’s I”**

Possible causes:

- No biological trajectory in data

- Trajectory is technical artifact (batch, quality)

- Need more cells to resolve smooth transitions

---

## Conclusion: From Clusters to Continuous Cell States

Congratulations! You’ve completed a comprehensive trajectory analysis using Monocle 3.

### Key Conceptual Takeaways

**1. Trajectories reveal continuous processes that clustering obscures**

- Cell differentiation isn’t a series of discrete jumps

- Activation, stress responses, and development are gradual

- Pseudotime provides temporal ordering without real time-course data

**2. Trajectory inference is model-based, requiring validation**

- Always verify trajectories match biological expectations

- Check that marker genes align with pseudotime

- Integrate multiple lines of evidence (RNA velocity, lineage tracing)

- Be conservative in claims—correlation ≠ causation

**3. Technical quality determines biological interpretability**

- Poor integration → batch-driven trajectories

- Insufficient cells → missed branch points

- Low-quality data → noise masquerading as biology

- Parts 1-3 (QC, integration) are prerequisites for meaningful trajectories

**4. Multiple partitions are often biologically meaningful**

- **Don’t force connection**: Disconnected partitions may represent genuine biological separation

- **In PBMC data**: T/NK cells (Partition 2), Classical Monocytes (Partition 1), B cells (Partition 3)

- **Each partition = independent trajectory**: Separate developmental processes with independent pseudotime

- **Biological interpretation**: Hematopoietic lineages that don’t interconvert in peripheral blood

- **Analysis strategy**: Treat each partition separately, use appropriate root selection per partition

**5. Root selection impacts biological interpretation**

- **Manual roots**: Biologically meaningful when developmental hierarchy is known (e.g., naive T cells)

- **Automatic roots**: Useful when hierarchy is unclear (e.g., isolated monocyte cluster)

- **This tutorial demonstrated both**: Mixed approach based on biological knowledge availability

- **Validation essential**: Check that inferred direction matches known marker expression patterns

### Reporting Trajectory Analysis in Publications

**Methods section should include:**

1. **Software versions**: Monocle 3 (version), Seurat (version), R (version)

2. **Parameter settings**: Number of PCA dimensions, ncenter, nn.k

3. **Root specification**: How root cells were chosen (automated vs manual)

4. **Validation approach**: Marker gene concordance, biological plausibility checks

5. **Cell numbers**: Total cells, cells per developmental stage

6. **Partition handling**: How multiple partitions were analyzed

**Results should report:**

1. **Trajectory structure**: Linear, branched, number of branches/leaves

2. **Pseudotime range**: Min/max values, biological meaning

3. **Cell distribution**: How cell types/conditions distribute along trajectory

4. **Key findings**: Genes/pathways associated with trajectory

5. **Validation**: Correspondence with known biology, orthogonal evidence

**Figures should show:**

1. **Trajectory structure**: Graph overlaid on UMAP with pseudotime coloring

2. **Cell type distribution**: How annotated types map to trajectory

3. **Marker genes**: Expression along pseudotime (heatmap + individual genes)

4. **Branch point analysis**: If applicable, genes distinguishing fates

5. **Condition effects**: How treatment/perturbation affects pseudotime

---

## References

1. Cao J, Spielmann M, Qiu X, et al. The single-cell transcriptional landscape of mammalian organogenesis. Nature. 2019;566(7745):496-502. doi:10.1038/s41586-019-0969-x [Monocle 3 method]

2. Qiu X, Mao Q, Tang Y, et al. Reversed graph embedding resolves complex single-cell trajectories. Nat Methods. 2017;14(10):979-982. doi:10.1038/nmeth.4402 [Monocle 2 principal graph]

3. Trapnell C, Cacchiarelli D, Grimsby J, et al. The dynamics and regulators of cell fate decisions are revealed by pseudotemporal ordering of single cells. Nat Biotechnol. 2014;32(4):381-386. doi:10.1038/nbt.2859 [Original Monocle pseudotime]

4. Street K, Risso D, Fletcher RB, et al. Slingshot: cell lineage and pseudotime inference for single-cell transcriptomics. BMC Genomics. 2018;19(1):477. doi:10.1186/s12864-018-4772-0 [Slingshot alternative]

5. Wolf FA, Hamey FK, Plass M, et al. PAGA: graph abstraction reconciles clustering with trajectory inference through a topology preserving map of single cells. Genome Biol. 2019;20(1):59. doi:10.1186/s13059-019-1663-x [PAGA method]

6. La Manno G, Soldatov R, Zeisel A, et al. RNA velocity of single cells. Nature. 2018;560(7719):494-498. doi:10.1038/s41586-018-0414-6 [RNA velocity]

7. Bergen V, Lange M, Peidli S, et al. Generalizing RNA velocity to transient cell states through dynamical modeling. Nat Biotechnol. 2020;38(12):1408-1414. doi:10.1038/s41587-020-0591-3 [scVelo]

8. Saelens W, Cannoodt R, Todorov H, Saeys Y. A comparison of single-cell trajectory inference methods. Nat Biotechnol. 2019;37(5):547-554. doi:10.1038/s41587-019-0071-9 [Comprehensive method comparison]

9. Tritschler S, Büttner M, Fischer DS, et al. Concepts and limitations for learning developmental trajectories from single cell genomics. Development. 2019;146(12):dev170506. doi:10.1242/dev.170506 [Trajectory inference concepts]

10. Haghverdi L, Büttner M, Wolf FA, Buettner F, Theis FJ. Diffusion pseudotime robustly reconstructs lineage branching. Nat Methods. 2016;13(10):845-848. doi:10.1038/nmeth.3971 [Diffusion pseudotime]

11. Setty M, Kiseliovas V, Levine J, et al. Characterization of cell fate probabilities in single-cell data with Palantir. Nat Biotechnol. 2019;37(4):451-460. doi:10.1038/s41587-019-0068-4 [Palantir method]

12. Cannoodt R, Saelens W, Deconinck L, Saeys Y. Spearheading future omics analyses using dyngen, a multi-modal simulator of single cells. Nat Commun. 2021;12(1):3942. doi:10.1038/s41467-021-24152-2 [Trajectory simulation/benchmarking]

13. Weinreb C, Wolock S, Tusi BK, Socolovsky M, Klein AM. Fundamental limits on dynamic inference from single-cell snapshots. Proc Natl Acad Sci USA. 2018;115(10):E2467-E2476. doi:10.1073/pnas.1714723115 [Theoretical limitations]

14. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. J Transl Med. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source – GSE174609]

15. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. Nat Biotechnol. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

---

***This tutorial is part of the NGS101.com series on single cell sequencing analysis. If this tutorial helped advance your research, **please comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.***
