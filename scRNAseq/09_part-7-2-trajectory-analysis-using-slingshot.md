# How to Analyze Single-Cell RNA-seq Data – Complete Beginner's Guide Part 7-2: Trajectory Analysis Using Slingshot

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/)

- [Introduction: Understanding Alternative Trajectory Methods](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Introduction_Understanding_Alternative_Trajectory_Methods)

  - [Why Learn Multiple Trajectory Methods?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Why_Learn_Multiple_Trajectory_Methods)
  - [What Is Slingshot and How Does It Work?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#What_Is_Slingshot_and_How_Does_It_Work)
  - [How Does Slingshot Differ from Monocle 3?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#How_Does_Slingshot_Differ_from_Monocle_3)
  - [When Should You Use Slingshot vs Monocle 3?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#When_Should_You_Use_Slingshot_vs_Monocle_3)

- [Setting Up Your R Environment for Slingshot Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Setting_Up_Your_R_Environment_for_Slingshot_Analysis)

  - [Installing Slingshot and Dependencies](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Installing_Slingshot_and_Dependencies)
  - [Loading Required Libraries](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Loading_Required_Libraries)

- [Preparing Data for Slingshot Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Preparing_Data_for_Slingshot_Analysis)

  - [Understanding the Slingshot Workflow](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Understanding_the_Slingshot_Workflow)
  - [Loading the Annotated Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Loading_the_Annotated_Seurat_Object)
  - [Strategic Downsampling: Maintaining Biological Diversity](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Strategic_Downsampling_Maintaining_Biological_Diversity)
  - [Visualizing the Downsampled Dataset](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Visualizing_the_Downsampled_Dataset)

- [Critical Decision: Choosing the Right Analysis Strategy](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Critical_Decision_Choosing_the_Right_Analysis_Strategy)

  - [Understanding the Biological Reality of PBMC Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Understanding_the_Biological_Reality_of_PBMC_Data)
  - [The Problem with Analyzing All Cell Types Together](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#The_Problem_with_Analyzing_All_Cell_Types_Together)
  - [The Solution: Cell Type-Specific Trajectory Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#The_Solution_Cell_Type-Specific_Trajectory_Analysis)
  - [Why This Approach Works](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Why_This_Approach_Works)
  - [Tutorial Strategy](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Tutorial_Strategy)

- [Trajectory Inference with Slingshot: T/NK Lineage Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Trajectory_Inference_with_Slingshot_TNK_Lineage_Analysis)

  - [Subsetting to T/NK Cell Lineage](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Subsetting_to_TNK_Cell_Lineage)
  - [Converting T/NK Subset to SingleCellExperiment Format](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Converting_TNK_Subset_to_SingleCellExperiment_Format)
  - [Running Slingshot Trajectory Inference on T/NK Lineage](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Running_Slingshot_Trajectory_Inference_on_TNK_Lineage)
  - [Understanding Lineages and Principal Curves in T/NK Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Understanding_Lineages_and_Principal_Curves_in_TNK_Data)

- [STEP 9: Create Consensus Pseudotime for T/NK Cells](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#STEP_9_Create_Consensus_Pseudotime_for_TNK_Cells)
- [STEP 10: Visualize T/NK Trajectories](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#STEP_10_Visualize_TNK_Trajectories)
- [STEP 11: Biological Validation – Marker Genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#STEP_11_Biological_Validation_%E2%80%93_Marker_Genes)
- [✅ TRAJECTORY DIRECTION IS CORRECT!](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#%E2%9C%85_TRAJECTORY_DIRECTION_IS_CORRECT)
- [STEP 12: Treatment Effects on T/NK Trajectory](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#STEP_12_Treatment_Effects_on_TNK_Trajectory)
- [STEP 13: Identify Trajectory Genes for T/NK Lineage](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#STEP_13_Identify_Trajectory_Genes_for_TNK_Lineage)
- [STEP 14: Integrate T/NK Results Back to Seurat](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#STEP_14_Integrate_TNK_Results_Back_to_Seurat)
- [Best Practices and Common Pitfalls](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Best_Practices_and_Common_Pitfalls)

  - [Best Practices for Robust Slingshot Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Best_Practices_for_Robust_Slingshot_Analysis)
  - [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Common_Pitfalls_and_How_to_Avoid_Them)
  - [Troubleshooting: Trajectory Doesn’t Match Expectations](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Troubleshooting_Trajectory_Doesnt_Match_Expectations)

- [Conclusion: Choosing the Right Trajectory Tool](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Conclusion_Choosing_the_Right_Trajectory_Tool)

  - [Reporting Slingshot Analysis in Publications](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Reporting_Slingshot_Analysis_in_Publications)

- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Like_this)

## Introduction: Understanding Alternative Trajectory Methods

If you completed [Part 7 of this tutorial series](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/), you learned trajectory analysis using Monocle 3. We explored how to:

- Reconstruct developmental processes from snapshot data

- Order cells along pseudotime across multiple partitions

- Identify genes that change during cell state transitions

- Handle disconnected biological processes (myeloid, T/NK, B cell lineages)

**But here’s an important principle in computational biology**: *No single method is perfect for every dataset or biological question.*

Different trajectory inference tools make different assumptions, use different algorithms, and excel at different scenarios. **Slingshot** offers a complementary approach to Monocle 3, with distinct advantages that make it valuable to learn as part of your single-cell analysis toolkit.

### Why Learn Multiple Trajectory Methods?

**1. Method Validation and Robustness**

Running multiple trajectory tools on the same data provides critical validation:

- **Consensus results** → High confidence in biological interpretation

- **Divergent results** → Signals need for closer examination

- **Tool-specific artifacts** → Revealed through comparison

**2. Complementary Strengths for Different Scenarios**

Each tool has specific advantages:

**Simple linear/branching****Complex branching****Large datasets****Small-medium datasets****Uses existing clusters****Clear start/end populations known****Interpretability****HPC/limited resources**

| Scenario | Monocle 3 | Slingshot |
| --- | --- | --- |
| (1-2 lineages) | ○ May over-complicate | ✓ Excellent |
| (3+ lineages, cyclic) | ✓ Excellent | ○ Limited |
| (>100K cells) | ✓ Optimized | ○ Slower |
| (<50K cells) | ○ Slower | ✓ Fast |
|  | ○ Recomputes structure | ✓ Directly leverages |
|  | ○ Needs root specification | ✓ Uses cluster structure |
|  | ○ Complex manifold | ✓ Intuitive cluster paths |
|  | ○ Memory-intensive | ✓ Lightweight |

### What Is Slingshot and How Does It Work?

**Slingshot** is a cluster-based trajectory inference method developed by Koen Van den Berge and colleagues (Street et al., 2018). Unlike Monocle 3’s manifold-learning approach, Slingshot builds trajectories by:

1. **Starting from existing clusters** (e.g., Seurat clusters from [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

2. **Identifying cluster relationships** based on proximity in reduced dimensions

3. **Drawing smooth curves** (principal curves) through the cluster centers

4. **Assigning pseudotime** based on distance along these curves

**The Slingshot Algorithm (Simplified):**

**Step 1: Cluster-Based Graph Construction**

- Uses your existing clusters (from Seurat, Scanpy, or any clustering method)

- Builds minimum spanning tree (MST) connecting cluster centers

- Determines which clusters represent start, middle, and end states

**Step 2: Lineage Identification**

- Traces paths from start cluster(s) to end cluster(s)

- Each path represents a potential developmental lineage

- Supports multiple lineages from single origin (branching trajectories)

**Step 3: Principal Curve Fitting**

- Fits smooth curves through cells along each identified lineage

- Curves pass through cluster centers but adapt to actual cell distribution

- Uses principal curves algorithm (Hastie & Stuetzle, 1989)

**Step 4: Pseudotime Assignment**

- Calculates distance along each curve for each cell

- Assigns pseudotime values (curve length from start)

- Cells near branch points may have multiple pseudotime values (one per lineage)

**Step 5: Cell Weight Calculation**

- For cells near branch points, assigns weights to each lineage

- Weight = probability that cell belongs to each trajectory

- Enables **soft assignment** rather than forcing hard branch decisions

**Key advantages of Slingshot:**

- ✓ **Intuitive approach**: Leverages biological clustering you already performed

- ✓ **Speed**: Much faster than manifold learning methods (minutes vs hours)

- ✓ **Flexibility**: Works with any dimensionality reduction (PCA, UMAP, diffusion maps)

- ✓ **Soft branching**: Assigns probabilistic lineage membership at branch points

- ✓ **Transparent**: Easy to understand how trajectories are constructed

- ✓ **Integration-friendly**: Uses existing Seurat/SCE objects directly

**Limitations to be aware of:**

- ✗ Limited to relatively simple trajectory structures (best for 2-3 main branches)

- ✗ Relies on clustering quality (poor clusters → poor trajectories)

- ✗ May miss rare intermediate states between clusters

- ✗ Less automated than Monocle 3 (requires specifying start/end clusters)

- ✗ Assumes trajectories follow cluster connectivity patterns

### How Does Slingshot Differ from Monocle 3?

Understanding the fundamental differences helps you choose the right tool for your biological question.

**Philosophical Differences:**

**Monocle 3 Approach**:

- *“Let the data reveal the trajectory”*

- Learns manifold structure from scratch

- Discovers clusters, partitions, and trajectories simultaneously

- Agnostic to prior clustering decisions

**Slingshot Approach**:

- *“Use biological knowledge to guide trajectory”*

- Builds on existing biological groupings (your clusters)

- Assumes clusters represent meaningful cell states

- Leverages prior knowledge about cell relationships

**Technical Differences:**

**Input****Algorithm****Clustering****Dimensionality reduction****Start point****End point****Branching detection****Speed****Memory requirement****Complexity handling****Interpretability****Partition handling**

| Aspect | Monocle 3 | Slingshot |
| --- | --- | --- |
|  | Raw counts, learns structure | Existing UMAP/PCA + clusters |
|  | UMAP + SimplePPT graph | MST + principal curves |
|  | Internal (Louvain/Leiden) | External (uses yours) |
|  | Computes own UMAP | Uses existing reduction |
|  | Auto-detected or user-specified root cells | User-specified start cluster(s) |
|  | Auto-detected leaf nodes | Auto-detected or user-specified end cluster(s) |
|  | Automatic | Based on cluster connectivity (MST) |
|  | Slower (learns manifold) | Faster (uses existing structures) |
|  | Higher | Lower |
|  | Handles very complex structures | Best for simpler lineages |
|  | Abstract manifold representation | Intuitive cluster-based paths |
|  | Creates disconnected partitions | Works within connected components |

### When Should You Use Slingshot vs Monocle 3?

**Choose Slingshot when:**

✓ **You have high-quality, well-defined clusters** from thorough QC and integration (Parts [2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/)–[3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Clusters represent distinct biological states

- Clear separation in UMAP/PCA space

- Confident in cluster biological identities

✓ **Trajectory structure is relatively simple**

- Linear progression (A → B → C)

- Single bifurcation (A → B or C)

- At most 2-3 main developmental branches

✓ **You know start and end populations**

- Biological knowledge clearly identifies progenitors/naive cells

- Terminal differentiation states are obvious

- Literature supports expected lineage relationships

✓ **Computational resources are limited**

- Need fast results for exploratory analysis

- Limited memory or CPU availability

- Working on personal computer rather than HPC cluster

✓ **You want highly interpretable results**

- Need to explain trajectory to non-computational collaborators

- Presentation to clinicians or experimental biologists

- Cluster-based approach is more intuitive than manifold learning

✓ **Building on existing Seurat workflow**

- Already performed extensive clustering analysis

- Want to add trajectory without re-processing entire dataset

- Need seamless integration with current analysis pipeline

**Choose Monocle 3 when:**

✓ **Trajectory structure is complex or unknown**

- Expecting multiple branch points (3+ lineages)

- Unsure about number or pattern of developmental paths

- Want algorithm to discover structure without prior assumptions

✓ **Working with very large datasets**

- >100,000 cells

- Computational scalability and optimization are critical

- Have access to HPC resources

✓ **Clusters are unclear or overlapping**

- Continuous variation rather than discrete states

- Difficult to separate populations by standard clustering

- Need manifold learning to reveal underlying structure

✓ **Want comprehensive trajectory analysis features**

- Need gene module identification along pseudotime

- Require partition detection for disconnected processes

- Want extensive built-in trajectory visualization options

**Use BOTH methods when:**

✓ **Publishing high-impact findings**

- Need robust validation through method consensus

- Reviewers will expect comprehensive analysis

- Results will guide experimental follow-up work

✓ **Results will inform clinical decisions or therapeutic development**

- High-stakes biological interpretation

- Need confidence from multi-method agreement

- Regulatory or translational implications

---

## Setting Up Your R Environment for Slingshot Analysis

### Installing Slingshot and Dependencies

Slingshot is part of the Bioconductor ecosystem, which provides well-integrated single-cell analysis tools. The installation is more straightforward than Monocle 3 since it doesn’t require archived packages.

```r
#-----------------------------------------------
# Installation: Slingshot and dependencies
#-----------------------------------------------

# Set CRAN mirror
options(repos = c(CRAN = "https://cloud.r-project.org"))

# Install BiocManager for Bioconductor package management
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

# Install Slingshot and its Bioconductor dependencies
BiocManager::install(c(
  "slingshot",             # Main trajectory inference package
  "SingleCellExperiment",  # SCE object infrastructure
  "princurve",             # Principal curve algorithm
  "TrajectoryUtils",       # Trajectory data structures and utilities
  "DelayedArray",          # Efficient large matrix operations
  "S4Vectors"              # S4 class system support
))

# Install additional CRAN packages for analysis and visualization
install.packages(c(
  "Seurat",         # Already installed from Parts 1-6
  "SeuratObject",   # Seurat object infrastructure
  "dplyr",          # Data manipulation
  "tidyr",          # Data tidying
  "ggplot2",        # Visualization
  "patchwork",      # Combine plots
  "RColorBrewer",   # Color palettes
  "viridis",        # Perceptually uniform colors
  "scales",         # Scale functions for ggplot2
  "Matrix",         # Sparse matrix operations
  "grDevices"       # Color and graphics utilities
))
```

**Why is Slingshot installation simpler than Monocle 3?**

- All dependencies available through standard Bioconductor channels

- No archived CRAN packages required

- No GitHub installation needed

- Fewer system-level dependencies (no terra, ggrastr complications)

### Loading Required Libraries

```r
#-----------------------------------------------
# STEP 1: Load required libraries
#-----------------------------------------------

# Core trajectory analysis
library(slingshot)
library(SingleCellExperiment)
library(princurve)
library(TrajectoryUtils)

# Single-cell analysis infrastructure
library(Seurat)
library(SeuratObject)

# Data manipulation
library(dplyr)
library(tidyr)

# Visualization
library(ggplot2)
library(patchwork)
library(RColorBrewer)
library(viridis)
library(scales)
library(grDevices)

# Set working directory
setwd("~/GSE174609_scRNA/trajectory_slingshot")

# Create organized output directories
dir.create("plots", showWarnings = FALSE)
dir.create("plots/trajectories", showWarnings = FALSE)
dir.create("plots/pseudotime", showWarnings = FALSE)
dir.create("results", showWarnings = FALSE)

# Set random seed for reproducibility
set.seed(42)

# Configure plotting defaults
theme_set(theme_classic(base_size = 12))

cat("Environment setup complete!\n")
cat("Slingshot version:", as.character(packageVersion("slingshot")), "\n")
cat("Seurat version:", as.character(packageVersion("Seurat")), "\n")
```

**What we’ve accomplished:**

- ✅ Loaded Slingshot and Bioconductor trajectory tools

- ✅ Loaded Seurat for data management

- ✅ Set up visualization packages for publication-quality plots

- ✅ Created organized directory structure

- ✅ Established reproducible random seed

---

## Preparing Data for Slingshot Analysis

### Understanding the Slingshot Workflow

Before diving into code, let’s understand the complete Slingshot workflow:

```r
Step 1: Load annotated Seurat object (from Part 4)
   ↓
Step 2: Downsample cells (10% per cell type for tutorial efficiency)
   ↓
Step 3: Convert Seurat → SingleCellExperiment (SCE) object
   ↓
Step 4: Run Slingshot trajectory inference
   ↓
Step 5: Extract lineages and pseudotime
   ↓
Step 6: Visualize trajectories and curves
   ↓
Step 7: Identify trajectory-associated genes
   ↓
Step 8: Integrate results back to Seurat
```

**Key conceptual difference from Monocle 3:**

- Monocle 3 workflow: `Seurat → CDS → preprocess → reduce_dim → cluster → learn_graph → order_cells`

- Slingshot workflow: `Seurat → SCE → slingshot()` (single function!)

Slingshot is remarkably simple because it **reuses your existing analysis** (clusters, UMAP) rather than recomputing everything.

### Loading the Annotated Seurat Object

We’ll use the same annotated dataset from Part 7 for direct methodological comparison.

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

# Check which UMAP reduction is available
available_reductions <- names(seurat_full@reductions)
cat("Available reductions:", paste(available_reductions, collapse = ", "), "\n")
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

Available reductions: pca integrated.cca umap.cca
```

**Understanding our dataset:**

- **8 cell types**: Major immune populations in peripheral blood

- **72,649 cells**: Large integrated dataset from periodontal treatment study

- **Uneven distribution**: T cells most abundant (~55%), Platelets rarest (39 cells)

- **Well-integrated**: From Part 3, batch-corrected with CCA

- **Platelets excluded** from trajectory: Too rare for meaningful analysis (< 50 cells)

### Strategic Downsampling: Maintaining Biological Diversity

**Why downsample for this tutorial?**

1. **Computational efficiency**: Reduce runtime from hours to minutes

2. **Pedagogical clarity**: Faster iteration for learning

3. **Resource accessibility**: Runnable on personal computers

4. **Biological preservation**: 10% sampling maintains cell type diversity

**Important**: For publication-quality analysis, use the full dataset.

```r
#-----------------------------------------------
# STEP 3: Downsample to 10% of cells per cell type (exclude Platelets)
#-----------------------------------------------

# Set seed for reproducible downsampling
set.seed(42)

# Get unique cell types and exclude Platelets (too rare)
all_cell_types <- unique(seurat_full$final_annotation)
cell_types <- all_cell_types[all_cell_types != "Platelets"]

cat("Cell types to analyze (excluding Platelets):\n")
print(cell_types)
cat("\n")

# Sample 10% from each cell type independently
downsampled_cells <- c()

for (cell_type in cell_types) {
  # Get all cells of this type
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
cat("  Reduction from full:", 
    round(100 * (1 - ncol(seurat_subset)/ncol(seurat_full)), 1), "%\n\n")

# Verify cell type representation
cat("Cell type distribution (downsampled):\n")
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
  Reduction from full: 90.0%

Cell type distribution (downsampled):
            B cells        CD4+ T cells        CD8+ T cells Classical Monocytes 
                674                3138                 313                 522 
          Monocytes             NK cells             T cells 
                235                1607                 773
```

**Why this sampling strategy is optimal:**

- ✅ **Proportional representation**: Each cell type maintains relative abundance

- ✅ **Rare types preserved**: Even Monocytes (3% of data) get 235 cells

- ✅ **Computationally feasible**: ~7,000 cells process in minutes

- ✅ **Reproducible**: Fixed seed ensures consistent results across runs

- ✅ **Scalable**: Same code works for full dataset (just change sampling fraction)

### Visualizing the Downsampled Dataset

Verify the downsampled data maintains biological structure before trajectory analysis:

```r
#-----------------------------------------------
# STEP 4: Visualize downsampled data
#-----------------------------------------------

# Determine which UMAP to use (from Part 3 integration)
if ("umap.harmony" %in% names(seurat_subset@reductions)) {
  umap_reduction <- "umap.harmony"
} else if ("umap.cca" %in% names(seurat_subset@reductions)) {
  umap_reduction <- "umap.cca"
} else if ("umap.rpca" %in% names(seurat_subset@reductions)) {
  umap_reduction <- "umap.rpca"
} else if ("umap.mnn" %in% names(seurat_subset@reductions)) {
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
              group.by = "seurat_clusters", label = TRUE,
              pt.size = 0.8) +
  ggtitle("Seurat Clusters") +
  theme(plot.title = element_text(face = "bold", size = 14),
        legend.position = "right")

# Combine plots
p_overview <- (p1 | p2) / (p3 | plot_spacer())

ggsave("plots/01_downsampled_data_overview.png", p_overview,
       width = 16, height = 12, dpi = 300)
```

**Critical validation checklist:**

Before proceeding to trajectory analysis, verify:

- ✓ **Cell types spatially separated**: Distinct clusters visible

- ✓ **Good integration**: Healthy and Post-Treatment well-mixed

- ✓ **All samples represented**: No strong batch effects

- ✓ **Clusters make biological sense**: Match known PBMC structure

**Red flags** (go back to [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) if present):

- ✗ Cell types heavily overlapping

- ✗ Samples/batches forming separate islands

- ✗ Condition driving more variation than biology

- ✗ Unexpected cluster patterns

Trajectories learned from poorly integrated data are **artifacts**, not biology!

---

## Critical Decision: Choosing the Right Analysis Strategy

### Understanding the Biological Reality of PBMC Data

Before running Slingshot, we need to make a crucial decision about our analysis strategy. **This decision determines whether your trajectory results will be biologically meaningful or computational artifacts.**

**The biological reality**: Our dataset (**[GSE174609](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE174609)**) contains **7 distinct cell types** representing **different hematopoietic lineages**:

**Lymphoid lineages**:

1. 

- T cells (CD4+, CD8+, general T cells)

- NK cells

- B cells

**Myeloid lineages**:

1. 

- Classical Monocytes

- Monocytes

**Critical insight**: These cell types **do not transition into each other** in peripheral blood. A mature B cell doesn’t become a T cell. A classical monocyte doesn’t become an NK cell. They represent **separate developmental branches** that diverged much earlier (at the hematopoietic stem cell stage).

### The Problem with Analyzing All Cell Types Together

**What happens if we run Slingshot on all 7 cell types simultaneously?**

Slingshot uses a **minimum spanning tree (MST)** algorithm to connect clusters. When you give it unrelated cell types, it will:

1. **Force connectivity** where none exists biologically

2. **Create artificial hub nodes** with no actual cells

3. **Generate 10+ spurious lineages** radiating from phantom centers

4. **Produce biologically meaningless trajectories**

**Example of what NOT to do:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/02_slingshot_curves.png?resize=1024%2C819&ssl=1)

*Figure: Slingshot trajectory with all 7 cell types analyzed together. Note the artificial central hub with no cells (black node in center) and 12 lineages radiating outward. This structure doesn’t represent real biology – it’s a computational artifact from forcing unrelated cell types into a single trajectory.*

**Problems with this approach:**

- ✗ Central “hub” contains **zero cells** (purely computational)

- ✗ Forces T cells → B cells transitions (impossible)

- ✗ Creates 12 lineages when biology has 2-4 major branches

- ✗ Pseudotime values meaningless across disconnected populations

- ✗ Cannot distinguish biological transitions from forced connectivity

### The Solution: Cell Type-Specific Trajectory Analysis

**Best practice**: Analyze **biologically related cell types separately**:

**Strategy 1: T/NK Cell Lineage** (Primary Example)

- Cell types: CD4+ T cells, CD8+ T cells, T cells, NK cells

- Biological process: T cell activation and differentiation

- Expected trajectory: Naive T cells → Activated T cells → Effector/Memory → NK-like cells

- **This is what we’ll demonstrate in Steps 5-15**

**Strategy 2: Myeloid Lineage**

- Cell types: Classical Monocytes, Monocytes

- Biological process: Monocyte activation

- Expected trajectory: Classical (resting) → Activated monocytes

**Strategy 3: B Cell Lineage** (If Applicable)

- Cell types: B cells only

- Note: Only if expecting activation states (naive → memory → plasma)

- With 674 cells of single type, may not show clear trajectory

### Why This Approach Works

**Advantages of cell type-specific analysis:**

✓ **Biologically meaningful**: Only connects cells that actually transition✓ **Clear start/end points**: Naive vs effector states are well-defined✓ **Simpler topology**: 1-4 lineages instead of 12 spurious branches✓ **Interpretable pseudotime**: Values reflect actual biological progression✓ **Easier validation**: Known markers clearly align with trajectory✓ **Publication-ready**: Reviewers expect biologically justified groupings

### Tutorial Strategy

For the remainder of this tutorial, we will:

Focus on **T/NK lineage** as main example

- Most abundant population (~5,831 cells)

- Clear biological progression expected

- Demonstrates complete Slingshot workflow

This approach teaches both **correct methodology** (cell type-specific) and **common mistakes to avoid** (analyzing unrelated types together).

---

## Trajectory Inference with Slingshot: T/NK Lineage Analysis

### Subsetting to T/NK Cell Lineage

First, we’ll subset our data to focus on the T cell and NK cell populations, which represent a biologically connected developmental trajectory.

```r
#-----------------------------------------------
# STEP 5: Subset to T/NK cell lineage
#-----------------------------------------------

# Define T/NK cell types
t_nk_types <- c("CD4+ T cells", "CD8+ T cells", "T cells", "NK cells")

# Subset Seurat object
seurat_t_nk <- seurat_subset[, seurat_subset$final_annotation %in% t_nk_types]

cat("T/NK lineage subset:\n")
cat("  Total cells:", ncol(seurat_t_nk), "\n")
cat("  Cell types:", length(unique(seurat_t_nk$final_annotation)), "\n\n")

# Cell type distribution
cat("Cell type distribution in T/NK subset:\n")
print(table(seurat_t_nk$final_annotation))
cat("\n")

# Visualize T/NK subset
p_t_nk_subset <- DimPlot(seurat_t_nk, 
                         reduction = umap_reduction,
                         group.by = "final_annotation", 
                         label = TRUE,
                         pt.size = 1) +
  ggtitle("T/NK Cell Lineage Subset") +
  theme(plot.title = element_text(face = "bold", size = 14))

ggsave("plots/02_t_nk_subset.png", p_t_nk_subset,
       width = 10, height = 8, dpi = 300)
```

**Expected output:**

```
T/NK lineage subset:
  Total cells: 5831
  Cell types: 4

Cell type distribution in T/NK subset:
CD4+ T cells CD8+ T cells     NK cells      T cells 
        3138          313         1607          773
```

**Why this subset makes biological sense:**

- **CD4+ T cells**: Contain naive, activated, and memory populations

- **CD8+ T cells**: Cytotoxic T cells, often terminal effectors

- **T cells**: General T cells, intermediate activation states

- **NK cells**: Natural killer cells, highly differentiated effector cells

- **Expected trajectory**: Naive → Activated → Effector/Memory states

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/02_t_nk_subset.png?resize=1024%2C819&ssl=1)

### Converting T/NK Subset to SingleCellExperiment Format

Slingshot operates on SingleCellExperiment (SCE) objects, the Bioconductor standard for single-cell data.

**Why convert to SCE?**

- Slingshot is built on Bioconductor infrastructure

- SCE is the standard format for 100+ Bioconductor scRNA-seq packages

- Easy interoperability with other trajectory tools (see [Part 6](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/) for object structures)

- Seamless conversion back to Seurat after analysis

```r
#-----------------------------------------------
# STEP 6: Convert T/NK subset to SingleCellExperiment
#-----------------------------------------------

# Convert Seurat to SCE (preserves all data, metadata, and reductions)
sce <- as.SingleCellExperiment(seurat_t_nk)

cat("Created SingleCellExperiment object for T/NK lineage:\n")
cat("  Cells:", ncol(sce), "\n")
cat("  Genes:", nrow(sce), "\n")
cat("  Assays:", paste(assayNames(sce), collapse = ", "), "\n")
cat("  Reduced dimensions:", paste(reducedDimNames(sce), collapse = ", "), "\n")
cat("  Cell metadata columns:", ncol(colData(sce)), "\n\n")

# Verify critical components transferred
cat("Verifying transferred information:\n")
cat("  Expression data (counts):", "counts" %in% assayNames(sce), "\n")
cat("  Expression data (logcounts):", "logcounts" %in% assayNames(sce), "\n")
cat("  Cell types (final_annotation):", 
    "final_annotation" %in% colnames(colData(sce)), "\n")
cat("  Clusters (seurat_clusters):", 
    "seurat_clusters" %in% colnames(colData(sce)), "\n\n")

# Extract UMAP coordinates using the reduction determined in Step 4
# When converting Seurat to SCE, reduction names become uppercase
# e.g., "umap.cca" becomes "UMAP.CCA"
sce_umap_name <- toupper(umap_reduction)

cat("UMAP reduction from Seurat:", umap_reduction, "\n")
cat("SCE reduction name:", sce_umap_name, "\n")
cat("Available in SCE:", sce_umap_name %in% reducedDimNames(sce), "\n\n")

# Extract UMAP coordinates
umap_coords <- reducedDim(sce, sce_umap_name)

cat("UMAP coordinates:\n")
cat("  Dimensions:", dim(umap_coords), "\n")
cat("  Range X:", round(range(umap_coords[,1]), 2), "\n")
cat("  Range Y:", round(range(umap_coords[,2]), 2), "\n\n")

# Create standardized "UMAP" entry for Slingshot
# Slingshot expects reducedDim to be named "UMAP"
reducedDim(sce, "UMAP") <- umap_coords
```

**Expected output:**

```r
Created SingleCellExperiment object for T/NK lineage:
  Cells: 5831
  Genes: 18861
  Assays: counts, logcounts, scaledata
  Reduced dimensions: PCA, INTEGRATED.CCA, UMAP.CCA
  Cell metadata columns: 47

Verifying transferred information:
  Expression data (counts): TRUE
  Expression data (logcounts): TRUE
  Cell types (final_annotation): TRUE
  Clusters (seurat_clusters): TRUE

UMAP reduction from Seurat: umap.cca
SCE reduction name: UMAP.CCA
Available in SCE: TRUE

UMAP coordinates:
  Dimensions: 5831 2
  Range X: -6.5 8.4
  Range Y: -10.2 9.8
```

**Understanding the SCE object structure ([Part 6](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/)):**

```bash
# SingleCellExperiment structure
# - assays(): Expression matrices (counts, logcounts, etc.)
# - colData(): Cell metadata (cell types, conditions, QC metrics)
# - rowData(): Gene metadata (gene names, chromosome, etc.)
# - reducedDims(): Dimensionality reductions (PCA, UMAP, etc.)
```

**What’s been preserved:**

- ✅ All expression data (counts and normalized)

- ✅ All cell metadata (cell types, conditions, samples, QC)

- ✅ All dimensionality reductions (PCA, integration, UMAP)

- ✅ Cluster assignments from Seurat

### Running Slingshot Trajectory Inference on T/NK Lineage

**The beauty of Slingshot**: A single function call performs complete trajectory inference!

**Key decision for T/NK trajectory**: Identifying the biological starting point.

In T cell biology:

- **CD4+ T cells** include naive/resting populations (low activation markers)

- **NK cells** are terminally differentiated effector cells (high cytotoxic markers)

- Expected progression: CD4+ naive → Activated T cells → Effector/NK states

**Strategy**: Let’s identify which cluster contains the most CD4+ T cells to use as our start.

```r
#-----------------------------------------------
# STEP 7: Run Slingshot trajectory inference on T/NK lineage
#-----------------------------------------------

# First, examine cluster composition to identify start cluster
cat("Cluster composition in T/NK lineage:\n")
cluster_celltype_table <- table(sce$seurat_clusters, sce$final_annotation)
print(cluster_celltype_table)
cat("\n")

# Identify cluster(s) enriched for CD4+ T cells (likely contains naive populations)
cd4_per_cluster <- cluster_celltype_table[, "CD4+ T cells"]
cd4_enriched_clusters <- names(cd4_per_cluster)[cd4_per_cluster > 100]

cat("Clusters enriched for CD4+ T cells (>100 cells):\n")
print(cd4_enriched_clusters)
cat("\n")

# For this tutorial, we'll let Slingshot auto-detect OR you can manually specify
# If you want manual control, set start.clus to a CD4+ enriched cluster

cat("Running Slingshot trajectory inference...\n")
cat("  Using: UMAP coordinates\n")
cat("  Clustering: Seurat clusters\n")
cat("  Start: Auto-detect (or manually specify CD4+ T cell cluster)\n")
cat("  End: Auto-detect (likely NK cell clusters)\n\n")

# Run Slingshot
sce <- slingshot(
  sce,
  clusterLabels = "seurat_clusters",  # Use Seurat cluster assignments
  reducedDim = "UMAP",                 # Use UMAP coordinates
  start.clus = NULL,                   # Auto-detect OR specify cluster number
  end.clus = NULL,                     # Auto-detect terminal clusters
  allow.breaks = FALSE,                # Don't allow gaps in lineages
  extend = "n",                        # Don't extend curves beyond data
  reweight = TRUE,                     # Refit curves after initial fit
  reassign = TRUE                      # Reassign cells to best-fit curves
)

cat("Slingshot trajectory inference complete!\n\n")

# Quick validation: Check if trajectory makes biological sense
lineages <- slingLineages(sce)
cat("Number of lineages detected:", length(lineages), "\n")

for (i in seq_along(lineages)) {
  cat("Lineage", i, ":", paste(lineages[[i]], collapse = " → "), "\n")
}
```

**Expected output (with auto-detection on T/NK subset):**

```r
=== Running Slingshot on T/NK Lineage ===

Cluster composition in T/NK lineage:

     CD4+ T cells CD8+ T cells NK cells T cells
  0          1044            0        0       0
  1          1034            0        0       0
  2             0            0      751       0
  3             0            0      725       0
  4           659            0        0       0
  5             0            0        0     533
  8             0          313        0       0
  10          227            0        0       0
  11            0            0        0     240
  13          174            0        0       0
  14            0            0       93       0
  15            0            0       38       0

Clusters enriched for CD4+ T cells (>100 cells):
[1] "0"  "1"  "4"  "10" "13"

Running Slingshot trajectory inference...
  Using: UMAP coordinates
  Clustering: Seurat clusters
  Start: Auto-detect
  End: Auto-detect

Slingshot trajectory inference complete!

Number of lineages detected: 4

Lineage 1 : 8 → 10 → 0 → 4 → 1 → 5 → 14 → 2 → 15
Lineage 2 : 8 → 10 → 0 → 4 → 1 → 5 → 3
Lineage 3 : 8 → 10 → 0 → 4 → 1 → 11
Lineage 4 : 8 → 10 → 0 → 4 → 13
```

**Interpreting the T/NK trajectory:**

With auto-detection on the T/NK subset, Slingshot chose:

- **Start cluster**: **Cluster 8** (100% CD8+ T cells, 313 cells total)

- This cluster is **pure CD8+ T cells**

- Unexpected choice since CD4+ clusters are larger and more abundant

- **Critical question**: Does cluster 8 contain naive or effector CD8+ T cells?

- **4 lineages detected**: Still biologically reasonable!

- Compare to 12 spurious lineages with all cell types together ✓

- All lineages share initial path: `8 → 10 → 0 → 4 → 1`

- Major branching occurs after cluster 1

- Different terminal states: clusters 15, 3, 11, 13

**Key observation – Why cluster 8 was chosen:**

Looking at the cluster composition:

- **Cluster 8**: 313 CD8+ T cells (100% pure, homogeneous)

- **Clusters 0, 1**: 1044 and 1034 CD4+ T cells (much larger!)

- Slingshot’s MST algorithm prioritizes graph topology, not cluster size

- Cluster 8 may be at the periphery of UMAP space → detected as root

**Is this correct?** We’ll validate in Step 11 with marker genes!

If cluster 8 contains **naive CD8+ T cells** (high CCR7, low GZMB):

- ✓ Trajectory direction is CORRECT

- ✓ Progression: CD8+ naive → CD4+ → T cells → NK effectors

If cluster 8 contains **effector CD8+ T cells** (low CCR7, high GZMB):

- ✗ Trajectory direction is REVERSED

- ✗ Need to manually specify CD4+ enriched start cluster

**The answer comes in Step 11!** For now, continue with auto-detected result to demonstrate complete workflow.

**Understanding Slingshot parameters:**

**`clusterLabels`** – Which grouping to use for trajectory:

- Option 1: `seurat_clusters` (numeric clusters from [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Option 2: `final_annotation` (cell type labels from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/))

- **Choice**: `seurat_clusters` gives finer resolution (16 clusters vs 7 cell types)

**`reducedDim`** – Where to construct trajectories:

- **UMAP** (default, recommended): Good for visualization, preserves global + local structure

- **PCA**: More linear, may miss non-linear relationships

- **Diffusion maps**: Alternative for complex manifolds

**`start.clus` and `end.clus`**:

- **Auto-detect** (NULL): Slingshot infers start/end based on cluster connectivity

- **Manual specification**: Use biological knowledge if developmental hierarchy is clear

- **Example manual**: `start.clus = "3"` if cluster 3 contains naive T cells

**`allow.breaks`**:

- **FALSE** (recommended): Trajectories must be continuous through cluster graph

- **TRUE**: Allows disconnected lineages (use with caution)

**`reweight` and `reassign`**:

- **TRUE** (recommended): Iteratively refine curve fitting

- Improves curve quality but slightly slower

### Understanding Lineages and Principal Curves in T/NK Data

Slingshot’s output contains several key components. Let’s extract and understand them for our T/NK lineage:

```r
#-----------------------------------------------
# STEP 8: Extract and examine Slingshot results for T/NK lineage
#-----------------------------------------------

# Extract lineages (cluster-level trajectories)
lineages <- slingLineages(sce)

cat("=== Slingshot Lineages for T/NK Cells ===\n")
cat("Number of lineages detected:", length(lineages), "\n\n")

for (i in seq_along(lineages)) {
  cat("Lineage", i, ":", paste(lineages[[i]], collapse = " → "), "\n")
}
cat("\n")

# Extract curves (cell-level fitted principal curves)
curves <- slingCurves(sce)
cat("Number of principal curves fit:", length(curves), "\n\n")

# Extract pseudotime (one column per lineage)
pseudotime_matrix <- slingPseudotime(sce)
cat("Pseudotime matrix dimensions:", dim(pseudotime_matrix), "\n")
cat("  (Rows = cells, Columns = lineages)\n\n")

# Extract cell weights (probability of belonging to each lineage)
curve_weights <- slingCurveWeights(sce)
cat("Cell weights dimensions:", dim(curve_weights), "\n")
cat("  (Rows = cells, Columns = lineages)\n")
cat("  Weight = probability cell belongs to lineage\n\n")

# Summary statistics per lineage
for (i in seq_along(lineages)) {
  pt <- pseudotime_matrix[, i]
  cat("Lineage", i, "pseudotime:\n")
  cat("  Range:", round(range(pt, na.rm = TRUE), 2), "\n")
  cat("  Cells assigned:", sum(!is.na(pt)), "\n")
  cat("  Median:", round(median(pt, na.rm = TRUE), 2), "\n\n")
}
```

**Expected output (T/NK subset with cluster 8 auto-detected as start):**

```
=== Slingshot Lineages for T/NK Cells ===
Number of lineages detected: 4

Lineage 1 : 8 → 10 → 0 → 4 → 1 → 5 → 14 → 2 → 15
Lineage 2 : 8 → 10 → 0 → 4 → 1 → 5 → 3
Lineage 3 : 8 → 10 → 0 → 4 → 1 → 11
Lineage 4 : 8 → 10 → 0 → 4 → 13

Number of principal curves fit: 4

Pseudotime matrix dimensions: 5831 4
  (Rows = cells, Columns = lineages)

Cell weights dimensions: 5831 4
  (Rows = cells, Columns = lineages)
  Weight = probability cell belongs to lineage

Lineage 1 pseudotime:
  Range: 0 27.21
  Cells assigned: 4570
  Median: 10.97

Lineage 2 pseudotime:
  Range: 0 24.8
  Cells assigned: 4406
  Median: 10.69

Lineage 3 pseudotime:
  Range: 0 17.77
  Cells assigned: 3486
  Median: 8.97

Lineage 4 pseudotime:
  Range: 0 14.16
  Cells assigned: 2409
  Median: 6.28
```

**Interpreting T/NK lineages with cluster 8 start:**

**Starting point**:

- **Cluster 8**: Auto-detected as root (CD8+ T cells, 313 cells)

- Pure homogeneous cluster – only CD8+ T cells

- **Critical question**: Does it contain naive or effector CD8+ T cells?

- Marker validation in Step 11 will answer this!

**Lineage structure** (cluster-level paths):

- **All 4 lineages share initial path**: `8 → 10 → 0 → 4 → 1`

- Shows common progression through early-to-mid states

- Major branching occurs after cluster 1 into 4 distinct paths

- **Lineage 1** (longest): `8 → 10 → 0 → 4 → 1 → 5 → 14 → 2 → 15`

- Pseudotime range: 0-27.21 (longest developmental trajectory)

- 4,570 cells along this path

- Median pseudotime: 10.97

- Ends at cluster 15 (38 NK cells)

- **Lineage 2**: `8 → 10 → 0 → 4 → 1 → 5 → 3`

- Range: 0-24.8

- 4,406 cells

- Median: 10.69

- Ends at cluster 3 (725 NK cells – major NK population)

- **Lineage 3**: `8 → 10 → 0 → 4 → 1 → 11`

- Range: 0-17.77 (shorter trajectory)

- 3,486 cells

- Median: 8.97

- Ends at cluster 11 (240 T cells)

- **Lineage 4** (shortest): `8 → 10 → 0 → 4 → 13`

- Range: 0-14.16 (shortest path)

- 2,409 cells

- Median: 6.28

- Ends at cluster 13 (174 CD4+ T cells)

**This structure is biologically meaningful!**

- ✓ **4 lineages**: Reasonable branching complexity (not 12 spurious ones!)

- ✓ **Shared origin**: All paths begin at cluster 8 (CD8+ T cells)

- ✓ **Multiple terminals**: Different endpoints suggest distinct differentiation states

- ✓ **Variable lengths**: Different lineages reach terminal states at different rates

- ✓ **No artificial hubs**: Trajectory passes through actual cells (not empty space)

**Cell assignment to lineages** (by primary lineage):

- Most cells assigned to Lineages 1-2 (longer paths through NK populations)

- Lineages 3-4 represent alternative, shorter differentiation routes

- Overlapping cell assignments (high cell counts sum >5831) indicate cells near branch points belong to multiple lineages with fractional weights

**Biological interpretation – Two scenarios:**

**Scenario A: If cluster 8 contains naive CD8+ T cells** (will validate in Step 11):

- ✓ Trajectory starts from early/naive state

- ✓ Progression: CD8+ naive → activated (CD4+/T) → effector (NK)

- ✓ Makes biological sense

**Scenario B: If cluster 8 contains effector CD8+ T cells**:

- ✗ Trajectory would be reversed (effector → naive)

- ✗ Needs correction by manual start specification or pseudotime reversal

**Step 11 marker validation will determine which scenario is correct!**

**Compare this to the problematic 12-lineage result** we avoided by subsetting to T/NK cells:

- ✓ 4 meaningful lineages (not 12 artifacts)

- ✓ Clear branching structure

- ✓ Curves through actual cells (not through empty UMAP space)

- ✓ Biologically plausible paths even if start cluster unexpected

## STEP 9: Create Consensus Pseudotime for T/NK Cells

```r
#-----------------------------------------------
# STEP 9: Create consensus pseudotime for T/NK cells
#-----------------------------------------------

# Strategy 1: Use primary lineage (highest weight)
primary_lineage <- apply(curve_weights, 1, which.max)
sce$primary_lineage <- paste0("Lineage_", primary_lineage)

# Extract pseudotime from primary lineage for each cell
sce$slingshot_pseudotime <- sapply(1:ncol(sce), function(i) {
  lineage <- primary_lineage[i]
  pseudotime_matrix[i, lineage]
})

cat("Primary pseudotime assignment for T/NK cells:\n")
cat("  Cells per lineage:\n")
print(table(sce$primary_lineage))
cat("\n")

cat("Pseudotime summary (primary lineage method):\n")
cat("  Range:", round(range(sce$slingshot_pseudotime, na.rm = TRUE), 2), "\n")
cat("  Median:", round(median(sce$slingshot_pseudotime, na.rm = TRUE), 2), "\n\n")

# Pseudotime by cell type
pseudotime_by_type <- data.frame(
  pseudotime = sce$slingshot_pseudotime,
  cell_type = sce$final_annotation,
  lineage = sce$primary_lineage,
  condition = sce$condition
)

cat("Median pseudotime by cell type (T/NK lineage):\n")
summary_stats <- pseudotime_by_type %>%
  group_by(cell_type) %>%
  summarise(
    median_pt = median(pseudotime, na.rm = TRUE),
    mean_pt = mean(pseudotime, na.rm = TRUE),
    n_cells = n()
  ) %>%
  arrange(median_pt)

print(as.data.frame(summary_stats))
```

**Expected output (with cluster 8 as auto-detected start):**

```
Primary pseudotime assignment for T/NK cells:
  Cells per lineage:
Lineage_1 Lineage_2 Lineage_3 Lineage_4 
     2596      1164       867      1204

Pseudotime summary (primary lineage method):
  Range: 0 27.21
  Median: 13.54

Median pseudotime by cell type (T/NK lineage):
     cell_type  median_pt   mean_pt n_cells
1 CD8+ T cells  0.8901564  1.217639     313
2 CD4+ T cells  9.8811279  9.378675    3138
3      T cells 17.0378733 16.907975     773
4     NK cells 22.3097337 22.624955    1607
```

**Analyzing pseudotime ordering:**

Since Slingshot auto-detected **cluster 8 (CD8+ T cells)** as the start, observe the cell type ordering:

- **CD8+ T cells**: **LOWEST** pseudotime (median **0.89**)

- At trajectory “start”

- 313 cells total (entire cluster 8)

- **CD4+ T cells**: Low-mid pseudotime (median **9.88**)

- 3,138 cells across multiple clusters

- Intermediate along trajectory

- **T cells**: Mid-high pseudotime (median **17.04**)

- 773 cells

- Later in trajectory progression

- **NK cells**: **HIGHEST** pseudotime (median **22.31**)

- At trajectory “end”

- 1,607 cells (terminal effector population)

**Critical biological interpretation:**

The ordering shows: **CD8+ (0.89) → CD4+ (9.88) → T cells (17.04) → NK (22.31)**

**Key question**: Is this biologically meaningful?

In typical T cell biology:

- CD4+ T cells often include naive populations (CCR7+, TCF7+) → expected EARLY

- CD8+ T cells are often cytotoxic effectors (GZMB+, PRF1+) → expected LATE

- NK cells are terminally differentiated (high GZMB, PRF1, NKG7) → expected LATE

**Two possible scenarios:**

**Scenario A: Cluster 8 contains NAIVE CD8+ T cells**

- If CD8+ T cells in cluster 8 are **early/naive** (high CCR7, low GZMB)

- Then this ordering is **biologically valid**

- Progression: Naive CD8+ → Activated CD4+/T → Terminal NK effectors

- Alternative T cell developmental pathway

**Scenario B: Cluster 8 contains EFFECTOR CD8+ T cells**

- If CD8+ T cells in cluster 8 are **late/effector** (low CCR7, high GZMB)

- Then trajectory is **REVERSED**

- Should be: CD4+ naive → activated → CD8+ effector → NK

- Requires correction

**How to determine which scenario is correct?**

**Step 11 marker validation is the definitive test!**

We will check:

- **CCR7** (naive marker):

- Should **decrease** with pseudotime if trajectory correct (Scenario A)

- Will **increase** if reversed (Scenario B)

- **GZMB** (effector marker):

- Should **increase** with pseudotime if trajectory correct (Scenario A)

- Will **decrease** if reversed (Scenario B)

**Key learning point**:

- Cell type labels alone don’t reveal naive vs effector status

- “CD8+ T cells” can be naive OR activated OR effector depending on cluster

- **Always validate trajectory direction with biological markers**

- Auto-detection uses graph topology, not biological knowledge

## STEP 10: Visualize T/NK Trajectories

```r
#-----------------------------------------------
# STEP 10: Visualize Slingshot trajectories for T/NK lineage
#-----------------------------------------------

# Extract UMAP coordinates and trajectory data
umap_df <- data.frame(
  UMAP1 = reducedDim(sce, "UMAP")[, 1],
  UMAP2 = reducedDim(sce, "UMAP")[, 2],
  pseudotime = sce$slingshot_pseudotime,
  cell_type = sce$final_annotation,
  condition = sce$condition,
  cluster = sce$seurat_clusters,
  lineage = sce$primary_lineage
)

# Plot 1: Slingshot curves on T/NK cells
png("plots/trajectories/02_slingshot_T_NK_curves.png", 
    width = 10, height = 8, units = "in", res = 300)

plot(reducedDim(sce, "UMAP"), 
     col = scales::alpha(as.numeric(factor(sce$final_annotation)), 0.5),
     pch = 16, cex = 0.5,
     xlab = "UMAP 1", ylab = "UMAP 2",
     main = "Slingshot Trajectory: T/NK Cell Lineage")

lines(SlingshotDataSet(sce), lwd = 3, col = "black", type = "lineages")

legend("topright", 
       legend = levels(factor(sce$final_annotation)),
       col = 1:length(levels(factor(sce$final_annotation))),
       pch = 16, cex = 0.8, bty = "n")

dev.off()

# Plot 2: Pseudotime gradient
p_pseudotime <- ggplot(umap_df, aes(x = UMAP1, y = UMAP2, color = pseudotime)) +
  geom_point(size = 1, alpha = 0.7) +
  scale_color_viridis(option = "plasma", name = "Pseudotime") +
  labs(title = "T/NK Lineage: Slingshot Pseudotime",
       x = "UMAP 1", y = "UMAP 2") +
  theme_classic(base_size = 12) +
  theme(plot.title = element_text(face = "bold", size = 14))

# Plot 3: Cell types
p_celltype <- ggplot(umap_df, aes(x = UMAP1, y = UMAP2, color = cell_type)) +
  geom_point(size = 1, alpha = 0.7) +
  labs(title = "T/NK Cell Types",
       x = "UMAP 1", y = "UMAP 2") +
  theme_classic(base_size = 12) +
  theme(plot.title = element_text(face = "bold", size = 14))

# Plot 4: Lineage assignment
p_lineage <- ggplot(umap_df, aes(x = UMAP1, y = UMAP2, color = lineage)) +
  geom_point(size = 1, alpha = 0.7) +
  scale_color_brewer(palette = "Set2") +
  labs(title = "Lineage Assignment",
       x = "UMAP 1", y = "UMAP 2") +
  theme_classic(base_size = 12) +
  theme(plot.title = element_text(face = "bold", size = 14))

# Combine plots
p_overview <- (p_pseudotime | p_celltype) / (p_lineage | plot_spacer())

ggsave("plots/trajectories/03_slingshot_T_NK_overview.png", 
       p_overview, width = 16, height = 12, dpi = 300)

# Violin plot
p_violin <- ggplot(umap_df, aes(x = reorder(cell_type, pseudotime, median),
                                 y = pseudotime, fill = cell_type)) +
  geom_violin(trim = FALSE, alpha = 0.7) +
  geom_boxplot(width = 0.15, fill = "white", outlier.shape = NA) +
  coord_flip() +
  labs(title = "Pseudotime Distribution by Cell Type (T/NK Lineage)",
       x = "Cell Type", y = "Pseudotime") +
  theme_classic(base_size = 12) +
  theme(legend.position = "none")

ggsave("plots/pseudotime/04_pseudotime_by_celltype_T_NK.png", 
       p_violin, width = 10, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/slingshot_tnk_Trajectories-e1772339924410-1024x787.png?resize=1024%2C787&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/04_pseudotime_by_celltype_T_NK.png?resize=1024%2C614&ssl=1)

## STEP 11: Biological Validation – Marker Genes

```r
#-----------------------------------------------
# STEP 11: Validate T/NK trajectory with marker genes
#-----------------------------------------------

# Test known T cell markers
marker_genes <- c("CCR7", "LEF1", "TCF7", "CD69", "GZMB", "PRF1", "NKG7")
available_markers <- marker_genes[marker_genes %in% rownames(sce)]

if (length(available_markers) > 0) {
  cat("Testing known markers along pseudotime...\n\n")

  for (gene in available_markers) {
    expr <- logcounts(sce)[gene, ]
    cor_test <- cor.test(sce$slingshot_pseudotime, expr, 
                         method = "spearman", use = "complete.obs")

    cat(sprintf("%s: correlation = %.3f, p-value = %.2e\n",
                gene, cor_test$estimate, cor_test$p.value))
  }
}
```

**Expected output format:**

```
Testing known markers along pseudotime...

CCR7: correlation = -0.648, p-value = 0.00e+00
LEF1: correlation = -0.722, p-value = 0.00e+00
TCF7: correlation = -0.564, p-value = 0.00e+00
CD69: correlation = -0.057, p-value = 1.47e-05
GZMB: correlation = 0.684, p-value = 0.00e+00
PRF1: correlation = 0.729, p-value = 0.00e+00
NKG7: correlation = 0.786, p-value = 0.00e+00
```

## ✅ TRAJECTORY DIRECTION IS CORRECT!

**ALL MARKER CORRELATIONS MATCH BIOLOGICAL EXPECTATIONS!**

**Naive/early markers (should DECREASE along pseudotime):**

- **CCR7**: correlation = **-0.648** ✓ STRONGLY NEGATIVE

- **LEF1**: correlation = **-0.722** ✓ STRONGLY NEGATIVE

- **TCF7**: correlation = **-0.564** ✓ STRONGLY NEGATIVE

- **CD69**: correlation = **-0.057** ✓ slightly negative

**Effector/late markers (should INCREASE along pseudotime):**

- **GZMB**: correlation = **0.684** ✓ STRONGLY POSITIVE

- **PRF1**: correlation = **0.729** ✓ STRONGLY POSITIVE

- **NKG7**: correlation = **0.786** ✓ STRONGLY POSITIVE

**What this confirms:**

1. ✅ **Cluster 8 contains NAIVE CD8+ T cells** (Scenario A was correct!)

- High CCR7 expression (naive marker)

- Low GZMB, PRF1 expression (low cytotoxic activity)

- These are early-stage, not effector, CD8+ T cells

1. ✅ **Trajectory runs in correct biological direction**:

- **Start** (pseudotime ~0): Naive CD8+ T cells (cluster 8)

- **Middle** (pseudotime ~10): CD4+ T cells transitioning

- **Late** (pseudotime ~17): General T cells activated

- **End** (pseudotime ~22): NK cells terminal effectors

1. ✅ **Auto-detection chose the right start cluster**

- Even though cluster 8 was unexpected (CD8+ instead of CD4+)

- Graph topology correctly identified naive population

- Validation is essential but auto-detection worked!

1. ✅ **Strong correlations** (all |r| > 0.5 except CD69)

- Indicates clear, robust differentiation trajectory

- Not ambiguous or weakly defined

- High statistical significance (all p < 2.2e-16)

**Biological interpretation:**

This T/NK trajectory represents:

- **Naive CD8+ T cells** (cluster 8) as origin

- Progressive **activation and differentiation**

- **CD4+ T cells** as intermediate states

- **NK cells** as terminal cytotoxic effectors

The progression **CD8+ naive → CD4+ activated → T cells → NK effectors** is biologically valid and represents a specific differentiation pathway captured in this PBMC dataset.

**Key lesson learned:**

⚠️ **Don’t assume cell type labels indicate developmental stage!**

- “CD8+ T cells” can be naive (CCR7+, low GZMB) OR effector (low CCR7, high GZMB)

- Cluster 8 happens to be naive CD8+ T cells

- Other CD8+ populations in your data could be effector

- **Always validate with markers** – you cannot know otherwise!

**This demonstrates the CRITICAL importance of Step 11 marker validation:**

- Without validation, we might have incorrectly assumed cluster 8 was “wrong” start

- Might have manually specified CD4+ cluster unnecessarily

- Would have missed this biologically valid CD8+ → NK pathway

- Validation confirmed auto-detection was actually optimal!

**No correction needed – proceed with confidence to Step 12!**

## STEP 12: Treatment Effects on T/NK Trajectory

```r
#-----------------------------------------------
# STEP 12: Test treatment effects on T/NK pseudotime
#-----------------------------------------------

# Statistical test
wilcox_test <- wilcox.test(pseudotime ~ condition, data = umap_df)

cat("Wilcoxon rank-sum test:\n")
cat("  W statistic:", wilcox_test$statistic, "\n")
cat("  P-value:", format(wilcox_test$p.value, scientific = TRUE), "\n\n")

# Median by condition
median_summary <- umap_df %>%
  group_by(condition) %>%
  summarise(
    median_pt = median(pseudotime, na.rm = TRUE),
    mean_pt = mean(pseudotime, na.rm = TRUE)
  )

cat("Pseudotime by condition:\n")
print(as.data.frame(median_summary))

# Density plot
p_density <- ggplot(umap_df, aes(x = pseudotime, fill = condition)) +
  geom_density(alpha = 0.6) +
  scale_fill_manual(values = c("Healthy" = "#2E86AB", 
                                "Post_Treatment" = "#F18F01")) +
  labs(title = "T/NK Pseudotime Distribution by Condition",
       x = "Pseudotime", y = "Density") +
  theme_classic(base_size = 12)

ggsave("plots/pseudotime/05_pseudotime_by_condition_T_NK.png", 
       p_density, width = 10, height = 7, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/05_pseudotime_by_condition_T_NK.png?resize=1024%2C717&ssl=1)

**Expected output:**

```
Wilcoxon rank-sum test:
  W statistic: 4542034
  P-value: 3.618854e-06

Pseudotime by condition:
       condition median_pt  mean_pt
1        Healthy  14.10336 14.04286
2 Post_Treatment  12.91152 13.16754
```

**Interpreting treatment effects:**

**Statistical significance:**

- **W statistic**: 4,542,034

- **P-value**: 3.62e-06 (p < 0.001) **HIGHLY SIGNIFICANT!**

- Treatment has a **significant effect** on T/NK cell differentiation state

**Direction of effect:**

- **Healthy**: median pseudotime = 14.10, mean = 14.04

- **Post-Treatment**: median pseudotime = 12.92, mean = 13.17

- **Post-treatment cells shifted toward LOWER pseudotime** (earlier states)

**Biological interpretation:**

Post-treatment periodontitis patients show:

**Shift toward earlier differentiation states**

1. 

- Lower pseudotime = more naive/early cells

- Treatment may be reversing inflammatory differentiation

**Enrichment of less differentiated T cells**

1. 

- More cells at CD8+ naive and CD4+ T states

- Fewer cells at terminal NK effector state

- Suggests reduction in chronic inflammatory response

**Possible mechanisms:**

1. 

- Treatment reduces chronic antigen stimulation

- Allows immune cells to return to resting state

- Reduces drive toward terminal differentiation

- Shifts population toward homeostatic balance

**Clinical significance:**

This finding suggests that periodontal treatment:

- **Reverses pathological immune activation**

- Shifts T/NK cells from activated/effector → resting/naive states

- Indicates functional immune restoration, not just symptom relief

- May explain long-term benefits beyond immediate inflammation reduction

**Comparison with literature:**

This aligns with studies showing periodontal therapy:

- Reduces systemic inflammation markers

- Normalizes immune cell profiles

- Has systemic health benefits beyond oral cavity

**Caveats:**

- Effect size is modest (median difference ~1.2 pseudotime units)

- Represents population-level shift, not complete reversal

- Cross-sectional comparison (not longitudinal tracking)

- Would need validation in independent cohort

## STEP 13: Identify Trajectory Genes for T/NK Lineage

```r
#-----------------------------------------------
# STEP 13: Identify pseudotime-dependent genes in T/NK lineage
#-----------------------------------------------

# Use T/NK SCE object (not all cell types!)
expr_mat <- logcounts(sce)
pt <- sce$slingshot_pseudotime

cat("Testing", nrow(expr_mat), "genes across", ncol(sce), "T/NK cells\n\n")

# Test correlations
gene_results <- lapply(seq_len(nrow(expr_mat)), function(i) {
  if (i %% 1000 == 0) cat("  Tested", i, "genes...\n")

  gene_expr <- expr_mat[i, ]

  if (sum(gene_expr > 0) < 10) {
    return(data.frame(gene = rownames(expr_mat)[i], correlation = NA, p_value = NA))
  }

  cor_test <- cor.test(pt, gene_expr, method = "spearman", exact = FALSE)

  data.frame(
    gene = rownames(expr_mat)[i],
    correlation = as.numeric(cor_test$estimate),
    p_value = cor_test$p.value
  )
})

gene_results_df <- do.call(rbind, gene_results)
gene_results_df <- gene_results_df[!is.na(gene_results_df$p_value), ]
gene_results_df$q_value <- p.adjust(gene_results_df$p_value, method = "fdr")
gene_results_df <- gene_results_df[order(abs(gene_results_df$correlation), decreasing = TRUE), ]

sig_genes <- gene_results_df[gene_results_df$q_value < 0.05, ]

cat("Significant genes (q < 0.05):", nrow(sig_genes), "\n")
cat("Positively correlated (increase along trajectory):", sum(sig_genes$correlation > 0), "\n")
cat("Negatively correlated (decrease along trajectory):", sum(sig_genes$correlation < 0), "\n\n")

cat("Top 20 genes:\n")
print(head(sig_genes, 20))

write.csv(sig_genes, "results/slingshot_T_NK_pseudotime_genes.csv", row.names = FALSE)
```

**Expected output:**

```
Significant genes (q &lt; 0.05): 9338

Positively correlated (increase along trajectory): 5502

Negatively correlated (decrease along trajectory): 3836

Top 20 genes:

gene correlation p_value q_value
NKG7   0.7860213       0       0
CST7   0.7715449       0       0
GZMA   0.7528684       0       0
ZEB2   0.7421538       0       0
```

## STEP 14: Integrate T/NK Results Back to Seurat

```r
#-----------------------------------------------
# STEP 14: Transfer T/NK trajectory results to Seurat
#-----------------------------------------------

# Create Seurat object for T/NK cells with trajectory info
seurat_t_nk$slingshot_pseudotime &lt;- sce$slingshot_pseudotime
seurat_t_nk$slingshot_lineage &lt;- sce$primary_lineage

# Add lineage weights
for (i in 1:ncol(curve_weights)) {
  seurat_t_nk@meta.data[[paste0("lineage_", i, "_weight")]] &lt;- curve_weights[, i]
}

# Visualize in Seurat
p_seurat_pseudotime &lt;- FeaturePlot(
  seurat_t_nk,
  features = "slingshot_pseudotime",
  reduction = umap_reduction,
  pt.size = 1
) +
  scale_color_viridis(option = "plasma") +
  ggtitle("T/NK Slingshot Pseudotime (Seurat)")

p_seurat_lineage &lt;- DimPlot(
  seurat_t_nk,
  group.by = "slingshot_lineage",
  reduction = umap_reduction,
  pt.size = 1
) +
  ggtitle("T/NK Lineage Assignment (Seurat)")

p_combined &lt;- p_seurat_pseudotime | p_seurat_lineage

ggsave("plots/pseudotime/06_slingshot_T_NK_in_seurat.png", 
       p_combined, width = 16, height = 6, dpi = 300)

# Save T/NK object with trajectory
saveRDS(seurat_t_nk, "results/seurat_T_NK_with_slingshot.rds")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/06_slingshot_T_NK_in_seurat.png?resize=1024%2C384&ssl=1)

---

## Best Practices and Common Pitfalls

### Best Practices for Robust Slingshot Analysis

**1. ALWAYS analyze biologically related cell types separately (CRITICAL!)**

✅ **Do:**

- Subset to cell types that actually transition into each other

- **T/NK lineage**: CD4+ T → CD8+ T → T cells → NK cells (activation/differentiation)

- **Myeloid lineage**: Classical Monocytes → Monocytes (activation)

- **B cell lineage**: Naive B → Memory B → Plasma B (only if expecting these states)

- Validate biological relationships with literature before grouping

❌ **Don’t:**

- Analyze all PBMC cell types together (creates artificial hubs with NO cells)

- Force unrelated lineages onto single trajectory

- Mix myeloid and lymphoid cells

- Analyze cell types with no developmental relationship

- Assume Slingshot will “figure it out” – it will ALWAYS connect clusters!

**Why this is THE most critical principle:**

Slingshot uses a **minimum spanning tree (MST)** algorithm that will ALWAYS find connections between clusters, even where none exist biologically. This results in:

- ✗ Artificial central “hubs” containing zero actual cells

- ✗ Spurious lineages (12 instead of 2-4 biologically meaningful ones)

- ✗ Trajectory curves passing through empty UMAP space

- ✗ Pseudotime values that are biologically meaningless

- ✗ Impossible to validate with marker genes

**The difference is night and day!** Cell type-specific analysis is not optional – it’s essential for meaningful results.

**2. Start with high-quality, integrated data**

✅ **Do:**

- Use well-integrated data from [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/)

- Validate clusters with known markers ([Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/))

- Ensure UMAP separates cell types clearly

- Check that batch effects are corrected

❌ **Don’t:**

- Run Slingshot on poorly integrated data

- Use clusters dominated by doublets or low-quality cells

- Proceed if cell types heavily overlap in UMAP

**3. Choose appropriate dimensionality reduction and specify biological start**

✅ **Do:**

- Use UMAP for most analyses (balances local + global structure)

- **Identify naive/progenitor/resting populations based on markers BEFORE running Slingshot**

- **Specify `start.clus` manually** when biological hierarchy is known

- Example: For T/NK lineage, use cluster enriched for CD4+ T cells (contains naive)

- Check with: `table(sce$seurat_clusters, sce$final_annotation)`

- Find cluster with most naive cell type: `names(which.max(table(...)))`

- Document biological rationale for start cluster choice

- **Auto-detection is convenient but not always biologically optimal**

❌ **Don’t:**

- Always rely on auto-detection without validation

- Assume Slingshot will pick the “right” start (it uses graph connectivity, not biology)

- Use UMAP with extreme parameters (very high `min.dist` over-smooths)

- Ignore biological knowledge about developmental hierarchies

**Real example from this tutorial:**

- Auto-detection chose cluster 8 (CD8+ T enriched) as start

- But CD4+ T cells typically contain more naive populations

- Validate with markers (Step 11) to determine if correction needed

- Manual specification of CD4+ enriched cluster often gives better results

**4. Validate trajectories thoroughly with markers**

✅ **Do:**

- Check known marker gene expression patterns BEFORE accepting results

- Verify early markers (CCR7, LEF1) correlate negatively with pseudotime

- Verify late markers (GZMB, PRF1) correlate positively with pseudotime

- Test robustness to parameter changes

- Compare with alternative methods when publishing

❌ **Don’t:**

- Accept default results without biological validation

- Ignore trajectories that contradict published biology

- Skip marker validation step

- Over-interpret weak or inconsistent patterns

### Common Pitfalls and How to Avoid Them

**Pitfall 1: Batch effects driving trajectory**

**Symptoms:**

- Pseudotime correlates with sample/batch

- Lineages follow batch structure

- Batch-specific genes have strongest trajectory correlations

**Solutions:**

- Revisit [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) integration

- Use stronger batch correction (Harmony, RPCA)

- Check batch distribution in UMAP before trajectory analysis

**Pitfall 2: Cell cycle confounded with differentiation**

**Symptoms:**

- Cell cycle genes (MKI67, TOP2A) dominate trajectory

- G2/M cells clustered at specific pseudotime

- Proliferation markers are highest-correlated genes

**Solutions:**

- Regress out cell cycle in Seurat

- Subset to non-cycling cells (G1 phase only)

- Use cell cycle-corrected PCA/UMAP

**Pitfall 3: Insufficient cells for trajectory**

**Symptoms:**

- Overly simplistic trajectories (straight lines)

- High variance between biological replicates

- Many cells with NA pseudotime

**Solutions:**

- Use full dataset (not downsampled) for final analysis

- Ensure ≥50-100 cells per developmental stage

- Focus on abundant cell types

### Troubleshooting: Trajectory Doesn’t Match Expectations

**“I got 12 lineages when I expected 2-3”**

→ You’re analyzing unrelated cell types together→ Solution: Subset to biologically related cells (see Pitfall 1)

**“Trajectory looks random with no biological structure”**

→ Possible causes: Poor clustering, wrong reduction, no real trajectory→ Solutions: Validate clusters, try PCA instead of UMAP, question if trajectory exists

**“Marker genes show opposite correlations from expected”**

→ Trajectory is reversed – auto-detection chose wrong end as start→ Solutions:

1. Manually specify start cluster enriched for naive/progenitor cells

2. Mathematically reverse pseudotime: `max_pt - pseudotime`

3. Use marker expression to identify correct start cluster→ See Pitfall 1.5 for detailed solutions

**“Auto-detection chose unexpected start cluster (e.g., CD8+ instead of CD4+)”**

→ Graph connectivity doesn’t always match biological hierarchy→ Solution: Manually specify start based on cell type knowledge→ Example: `start.clus = cluster_with_most_CD4_cells`→ Always validate with markers regardless of start method

**“Central hub appears with no cells”**

→ Analyzing unrelated cell types→ Solution: Cell type-specific analysis (demonstrated in Steps 5-11)

---

## Conclusion: Choosing the Right Trajectory Tool

Congratulations! You’ve mastered Slingshot trajectory analysis with **proper cell type-specific methodology**.

### Reporting Slingshot Analysis in Publications

**Methods section should include:**

1. **Subsetting strategy**: Which cell types analyzed together and biological justification

2. **Software versions**: Slingshot, SingleCellExperiment, Seurat, R versions

3. **Parameters**: Which clustering used, dimensionality reduction, start/end specification

4. **Validation**: How trajectory was validated (marker genes, biology, orthogonal data)

**Results should report:**

1. **Cell type subset**: Which cells included and why

2. **Lineage structure**: Number and composition of lineages

3. **Pseudotime distribution**: By cell type and condition

4. **Key genes**: Top trajectory-associated genes with correlations

5. **Validation results**: Marker gene correlations confirming trajectory direction

**Figures should show:**

1. Trajectory curves on UMAP (T/NK example: Steps 10)

2. Pseudotime gradient colored visualization

3. Marker gene expression along pseudotime (validation)

4. Gene dynamics heatmap or individual gene plots

5. Treatment/condition effects on pseudotime distribution

---

## References

1. Street K, Risso D, Fletcher RB, et al. Slingshot: cell lineage and pseudotime inference for single-cell transcriptomics. *BMC Genomics*. 2018;19(1):477. doi:10.1186/s12864-018-4772-0

2. Van den Berge K, Roux de Bézieux H, Street K, et al. Trajectory-based differential expression analysis for single-cell sequencing data. *Nat Commun*. 2020;11(1):1201. doi:10.1038/s41467-020-14766-3

3. Hastie T, Stuetzle W. Principal curves. *J Am Stat Assoc*. 1989;84(406):502-516.

4. Cao J, Spielmann M, Qiu X, et al. The single-cell transcriptional landscape of mammalian organogenesis. *Nature*. 2019;566(7745):496-502. doi:10.1038/s41586-019-0969-x

5. Saelens W, Cannoodt R, Todorov H, Saeys Y. A comparison of single-cell trajectory inference methods. *Nat Biotechnol*. 2019;37(5):547-554. doi:10.1038/s41587-019-0071-9

6. Amezquita RA, Lun ATL, Becht E, et al. Orchestrating single-cell analysis with Bioconductor. *Nat Methods*. 2020;17(2):137-145. doi:10.1038/s41592-019-0654-x

7. Wolf FA, Hamey FK, Plass M, et al. PAGA: graph abstraction reconciles clustering with trajectory inference. *Genome Biol*. 2019;20(1):59. doi:10.1186/s13059-019-1663-x

8. La Manno G, Soldatov R, Zeisel A, et al. RNA velocity of single cells. *Nature*. 2018;560(7719):494-498. doi:10.1038/s41586-018-0414-6

9. Haghverdi L, Büttner M, Wolf FA, et al. Diffusion pseudotime robustly reconstructs lineage branching. *Nat Methods*. 2016;13(10):845-848. doi:10.1038/nmeth.3971

10. Setty M, Kiseliovas V, Levine J, et al. Characterization of cell fate probabilities in single-cell data with Palantir. *Nat Biotechnol*. 2019;37(4):451-460. doi:10.1038/s41587-019-0068-4

11. Tritschler S, Büttner M, Fischer DS, et al. Concepts and limitations for learning developmental trajectories from single cell genomics. *Development*. 2019;146(12):dev170506. doi:10.1242/dev.170506

12. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nat Biotechnol*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y

13. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. *J Transl Med*. 2022;20(1):504. doi:10.1186/s12967-022-03686-8

---

*This tutorial demonstrates **proper cell type-specific trajectory analysis** using Slingshot, including a cautionary example of what happens when unrelated cell types are analyzed together. The key lesson: ALWAYS subset to biologically related cells before trajectory inference!*
