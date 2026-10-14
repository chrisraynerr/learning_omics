# How to Analyze Single-Cell RNA-seq Data – Complete Beginner’s Guide Part 4: Cell Type Identification

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** August 31, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/)

- [Introduction: From Clusters to Biological Identities](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Introduction_From_Clusters_to_Biological_Identities)

  - [Why Cell Type Identification Matters](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Why_Cell_Type_Identification_Matters)
  - [The Complexity of Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#The_Complexity_of_Cell_Type_Annotation)
  - [Common Misconceptions About Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Common_Misconceptions_About_Cell_Type_Annotation)
  - [Overview of Cell Type Identification Methods](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Overview_of_Cell_Type_Identification_Methods)
  - [Our Tutorial Strategy: Multi-Method Validation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Our_Tutorial_Strategy_Multi-Method_Validation)
  - [Dataset Context: GSE174609 PBMC Study](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Dataset_Context_GSE174609_PBMC_Study)

- [Setting Up Your R Environment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Setting_Up_Your_R_Environment)

  - [Required Packages for Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Required_Packages_for_Cell_Type_Annotation)
  - [Loading Libraries and Configuration](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Loading_Libraries_and_Configuration)

- [Loading Integrated Data from Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Loading_Integrated_Data_from_Part_3)

  - [Loading the Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Loading_the_Seurat_Object)
  - [Visualizing Starting Point](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Visualizing_Starting_Point)

- [Normalizing and Scaling Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Normalizing_and_Scaling_Data)
- [Manual Cell Type Annotation: The Gold Standard](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Manual_Cell_Type_Annotation_The_Gold_Standard)

  - [Understanding PBMC Cell Types](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Understanding_PBMC_Cell_Types)
  - [Step 1: Canonical Marker Genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Step_1_Canonical_Marker_Genes)
  - [Step 2: Visualizing Canonical Markers](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Step_2_Visualizing_Canonical_Markers)
  - [Step 3: Data-Driven Marker Discovery](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Step_3_Data-Driven_Marker_Discovery)
  - [Step 4: Manual Cluster Assignment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Step_4_Manual_Cluster_Assignment)

- [SingleR: Reference-Based Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#SingleR_Reference-Based_Annotation)

  - [Understanding SingleR’s Algorithm](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Understanding_SingleRs_Algorithm)
  - [Available Reference Datasets](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Available_Reference_Datasets)
  - [Testing Multiple References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Testing_Multiple_References)
  - [Comparing References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Comparing_References)

- [scType: Marker-Based Gene Set Scoring](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#scType_Marker-Based_Gene_Set_Scoring)

  - [Loading scType Functions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Loading_scType_Functions)
  - [Loading Marker Database](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Loading_Marker_Database)
  - [Calculating scType Scores](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Calculating_scType_Scores)

- [scCATCH: Tissue-Specific Automated Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#scCATCH_Tissue-Specific_Automated_Annotation)

  - [Understanding scCATCH’s Approach](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Understanding_scCATCHs_Approach)
  - [Running scCATCH](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Running_scCATCH)

- [Comparing Annotation Methods](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Comparing_Annotation_Methods)

  - [Confusion Matrices](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Confusion_Matrices)
  - [Sankey Diagrams](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Sankey_Diagrams)
  - [Side-by-Side UMAP Comparison](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Side-by-Side_UMAP_Comparison)

- [Building Consensus Annotations: Manual Comparison Strategy](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Building_Consensus_Annotations_Manual_Comparison_Strategy)

  - [Step 1: Export Annotations for Manual Review](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Step_1_Export_Annotations_for_Manual_Review)
  - [Step 2: Manual Comparison Workflow](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Step_2_Manual_Comparison_Workflow)
  - [Step 3: Import Consensus and Finalize](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Step_3_Import_Consensus_and_Finalize)

- [Saving Annotated Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Saving_Annotated_Data)
- [Best Practices for Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Best_Practices_for_Cell_Type_Annotation)

  - [General Principles](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#General_Principles)
  - [Method Selection Guidelines](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Method_Selection_Guidelines)
  - [Reference Selection (SingleR)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Reference_Selection_SingleR)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Common_Pitfalls_to_Avoid)
  - [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Troubleshooting_Common_Issues)

- [Conclusion: From Clusters to Cell Types](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Conclusion_From_Clusters_to_Cell_Types)

  - [Key Takeaways](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Key_Takeaways)

- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/#Like_this)

## Introduction: From Clusters to Biological Identities

In Part [1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/), [2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/), [3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) of this tutorial series, we’ve taken our scRNA-seq data from raw FASTQ files through quality control, integration, and clustering. We now have groups of cells that cluster together based on transcriptional similarity—but **what are these cells?**

Cell type identification transforms abstract “Cluster 0, Cluster 1, Cluster 2…” into meaningful biological entities: “CD4+ T cells, B cells, Monocytes…” This step is **critical** because all downstream biological interpretation depends on knowing which cells you’re analyzing.

### Why Cell Type Identification Matters

Consider these scenarios where accurate cell type annotation is essential:

**Research Question 1: “How does periodontitis treatment affect immune cell composition?”**

- Without annotation: “Cluster 3 increases after treatment” (meaningless)

- With annotation: “CD14+ classical monocytes expand 2-fold after treatment” (actionable biology)

**Research Question 2: “Which cell types respond to treatment?”**

- Without annotation: Can’t perform cell-type-specific differential expression

- With annotation: Identify treatment-responsive genes in specific immune populations

**Research Question 3: “Are there novel cell populations in disease?”**

- Without annotation: Can’t distinguish rare cell types from artifacts

- With annotation: Validate rare populations using multiple orthogonal methods

**The fundamental challenge**: Cells don’t come with labels. We must infer their identity from gene expression patterns, leveraging:

1. **Biological knowledge** (known marker genes from literature)

2. **Reference atlases** (curated cell type profiles from published studies)

3. **Computational methods** (algorithms that match patterns to databases)

### The Complexity of Cell Type Annotation

Cell type identification is more nuanced than beginners often expect. Here’s why it’s challenging:

**Challenge 1: Biology Exists on a Spectrum**

Cell types aren’t discrete categories but exist along continuous gradients:

- **Developmental trajectories**: Stem cells gradually differentiate through intermediate states

- **Activation states**: Resting vs activated cells of the same type show different expression

- **Tissue context**: The same cell type (e.g., macrophages) varies by tissue location

- **Disease effects**: Pathological conditions create novel cell states

This means: **Your clusters may not correspond 1:1 with textbook cell types.**

**Challenge 2: Technical Limitations**

scRNA-seq data has inherent limitations that complicate annotation:

- **Sparsity**: Many genes aren’t detected even when expressed (dropout)

- **Shallow depth**: Individual cells have fewer reads than bulk RNA-seq

- **Batch effects**: Technical variation can obscure biological identity

- **Doublets**: Two cells captured together appear as hybrid “cell types”

**Challenge 3: Incomplete Biological Knowledge**

Our understanding of cell types is incomplete:

- **Marker genes vary by context**: CD4 marks T helper cells in blood but also appears in brain microglia

- **Species differences**: Mouse and human markers don’t always align

- **Novel populations**: Rare cell types may lack characterized markers

- **Terminology inconsistencies**: Different fields use different names for the same cells

**Challenge 4: Subjectivity**

Cell type definitions involve subjective decisions:

- How distinct must expression be to call cells different types?

- Are activated cells a new “type” or a “state” of existing types?

- Should you merge similar clusters or keep them separate?

There are no universally correct answers to these questions.

**Challenge 5: Computational Complexity**

With 20,000+ genes and thousands of cells:

- Which genes are most informative for cell type identity?

- How do you weight conflicting marker evidence?

- What if automated methods disagree with each other?

- How confident should you be in edge cases?

### Common Misconceptions About Cell Type Annotation

Before diving into methods, let’s address common beginner misunderstandings:

**Misconception 1: “One marker gene is enough”**

- ✗ Wrong: Single markers are rarely specific

- ✓ Reality: Cell types require **combinations** of markers

- Example: CD3D marks T cells, but you need CD4/CD8 to distinguish subtypes

**Misconception 2: “Automated methods are always correct”**

- ✗ Wrong: Algorithms can be wrong, especially for rare types

- ✓ Reality: **Always validate** automated results with marker expression

- Example: SingleR might call stressed cells “activated” based on reference data

**Misconception 3: “Every cluster is a distinct cell type”**

- ✗ Wrong: Clustering can over-split or under-split populations

- ✓ Reality: **Biological interpretation** determines if clusters should be merged

- Example: Three clusters of CD4+ T cells might all be naive T cells with technical variation

**Misconception 4: “Published markers work universally”**

- ✗ Wrong: Markers are context-dependent (tissue, species, disease state)

- ✓ Reality: **Validate markers in your dataset** before trusting them

- Example: Some PBMC markers from mouse don’t translate to human

**Misconception 5: “FindAllMarkers reveals cell types directly”**

- ✗ Wrong: Statistical DE genes ≠ biological cell type markers

- ✓ Reality: DE results need **biological interpretation**

- Example: Ribosomal genes may be top DE genes but don’t define cell types

**Misconception 6: “The top marker alone is sufficient”**

- ✗ Wrong: Ranking doesn’t equal biological importance

- ✓ Reality: Evaluate **multiple markers** and their expression patterns

- Example: Top gene might be highly variable but expressed in multiple types

### Overview of Cell Type Identification Methods

There are three major approaches to cell type annotation, each with distinct strengths and limitations:

#### 1. Manual Annotation (Biological Expertise)

**How it works:**

- Examine known marker genes in your clusters

- Use biological knowledge to assign identities

- Iteratively refine based on multiple lines of evidence

**Strengths:**

- ✓ Incorporates biological understanding

- ✓ Flexible for novel cell types

- ✓ Transparent decision-making

- ✓ Works when automated methods fail

**Limitations:**

- ✗ Time-consuming

- ✗ Requires domain expertise

- ✗ Subjective (different experts may disagree)

- ✗ Doesn’t scale to 100+ clusters

**When to use:**

- Novel tissues or conditions

- When you have biological expertise

- For research-grade depth

- To validate automated methods

#### 2. Reference-Based Methods (Transfer from Atlases)

**How they work:**

- Compare your cells to annotated reference datasets

- Transfer labels based on expression similarity

- Use correlation, nearest neighbors, or probabilistic matching

**Example: SingleR**

- Correlates each cell with reference cell types

- Assigns label of best-matching reference

- Provides confidence scores for assignments

**Strengths:**

- ✓ Fast and scalable

- ✓ Leverages expert-curated references

- ✓ Quantitative confidence scores

- ✓ No marker curation needed

**Limitations:**

- ✗ Reference must match your tissue/species

- ✗ Can’t identify novel cell types (not in reference)

- ✗ Batch effects between reference and query

- ✗ Limited by reference quality

**When to use:**

- Standard tissues (blood, brain, etc.)

- Large datasets (>50,000 cells)

- When good reference atlases exist

- For initial rapid annotation

#### 3. Marker-Based Methods (Gene Set Scoring)

**How they work:**

- Use curated marker gene lists for each cell type

- Score cells based on marker expression

- Assign type with highest enrichment score

**Example: scType**

- Loads cell type marker databases

- Calculates enrichment scores per cluster

- Assigns type if score exceeds threshold

**Example: scCATCH**

- Uses tissue-specific marker databases

- Identifies markers per cluster automatically

- Matches to known cell types in database

**Strengths:**

- ✓ Transparent (can see which markers drive decisions)

- ✓ Tissue-specific (databases optimized per tissue)

- ✓ No reference data needed

- ✓ Can detect novel types (if markers expressed)

**Limitations:**

- ✗ Requires accurate marker lists

- ✗ Markers may not work in all contexts

- ✗ Doesn’t capture continuous variation

- ✗ Database completeness varies

**When to use:**

- When you have curated markers

- For tissues with established marker sets

- When reference data unavailable

- To validate reference-based results

### Our Tutorial Strategy: Multi-Method Validation

We’ll use **four complementary approaches** to maximize annotation accuracy:

1. **Manual annotation** – Gold standard using biological expertise

2. **SingleR** – Reference-based (testing multiple reference datasets)

3. **scType** – Marker-based scoring with custom databases

4. **scCATCH** – Tissue-specific automated marker identification

**Why use multiple methods?**

- **Different errors**: Methods fail in different ways (complementary weaknesses)

- **Consensus builds confidence**: Agreement across methods validates assignments

- **Method-specific strengths**: Each method excels in different scenarios

- **Biological validation**: Multiple lines of evidence support annotations

**The workflow we’ll follow:**

```
Integrated, Clustered Data (from Part 3)
    ↓
Manual Annotation
    → Examine canonical markers
    → Run FindAllMarkers for data-driven discovery
    → Assign cell types based on biological knowledge
    ↓
SingleR Annotation (Multiple References)
    → Test: HumanPrimaryCellAtlas (broad coverage)
    → Test: MonacoImmuneData (immune-specific)
    → Test: DatabaseImmuneCellExpression (detailed immune)
    → Select best reference for PBMC data
    ↓
scType Annotation
    → Load ScTypeDB marker database
    → Calculate enrichment scores
    → Assign types with confidence thresholds
    ↓
scCATCH Annotation
    → Identify cluster markers automatically
    → Match to tissue-specific database (Blood)
    → Assign types with marker evidence
    ↓
Comparison & Manual Consensus Building
    → Compare annotations side-by-side
    → Identify agreements and conflicts
    → Manually reconcile differences based on marker validation
    → Document consensus decisions
    ↓
Final Annotated Dataset
```

### Dataset Context: GSE174609 PBMC Study

We continue working with the periodontitis study:

- **8 samples**: 4 Healthy donors + 4 Post-treatment patients

- **Expected cell types**: Standard PBMC populations (T, B, NK, monocytes, DCs)

This is an ideal dataset for learning annotation because:

- ✓ Well-characterized tissue (extensive PBMC literature)

- ✓ Clear marker genes (decades of immunology research)

- ✓ Good reference atlases available

- ✓ Multiple cell types (8-12 expected populations)

- ✓ Biological variation to interpret (healthy vs treated)

Let’s begin by setting up our environment for multi-method annotation.

## Setting Up Your R Environment

We’ll install packages for all four annotation methods, ensuring HPC compatibility by avoiding packages with complex compilation dependencies.

### Required Packages for Cell Type Annotation

```r
#-----------------------------------------------
# Installation: Cell Type Annotation Packages
#-----------------------------------------------

# Set CRAN mirror
options(repos = c(CRAN = "https://cloud.r-project.org"))

# Install Bioconductor manager
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

# SingleR and Reference Datasets
BiocManager::install(c(
  "SingleR",
  "celldex",
  "SingleCellExperiment"
), update = FALSE, ask = FALSE)

# scCATCH (Tissue-Specific Markers)
install.packages("scCATCH")

# scType Dependencies
install.packages(c(
  "HGNChelper",
  "openxlsx"
))

# Visualization and Utilities
install.packages(c(
  "ggalluvial",
  "scales",
  "viridis"
))
```

**What we’re installing:**

- **SingleR + celldex**: Reference-based annotation with curated cell type atlases

- **scCATCH**: Tissue-specific marker databases for automated annotation

- **scType dependencies**: Tools for gene symbol validation and Excel file reading

- **Visualization packages**: For creating comparison plots and Sankey diagrams

### Loading Libraries and Configuration

```r
#-----------------------------------------------
# STEP 1: Load required libraries
#-----------------------------------------------

# Core single-cell analysis
library(Seurat)
library(SeuratObject)

# Automated annotation methods
library(SingleR)
library(celldex)
library(scCATCH)

# Data structures
library(SingleCellExperiment)

# Marker processing
library(HGNChelper)

# Visualization and data manipulation
library(ggplot2)
library(ggalluvial)
library(dplyr)
library(patchwork)
library(scales)
library(viridis)

# Set working directory
setwd("~/GSE174609_scRNA/cell_type_annotation")

# Create output directories
dir.create("plots", showWarnings = FALSE)
dir.create("plots/manual_annotation", showWarnings = FALSE)
dir.create("plots/automated_annotation", showWarnings = FALSE)
dir.create("plots/method_comparison", showWarnings = FALSE)
dir.create("annotations", showWarnings = FALSE)

# Set random seed for reproducibility
set.seed(42)

# Configure plotting defaults
theme_set(theme_classic(base_size = 12))
```

**Directory structure we’re creating:**

- `plots/manual_annotation/`: Violin plots, UMAPs, heatmaps for manual curation

- `plots/automated_annotation/`: SingleR, scType, scCATCH visualizations

- `plots/method_comparison/`: Side-by-side comparisons and Sankey diagrams

- `annotations/`: CSV files with cell-level and cluster-level annotations

## Loading Integrated Data from Part 3

We’ll load the integrated and clustered Seurat object created in [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/), which has batch effects corrected and cells grouped into clusters.

### Loading the Seurat Object

```r
#-----------------------------------------------
# STEP 2: Load integrated data from Part 3
#-----------------------------------------------

# Path to integrated Seurat object from Part 3
integrated_path <- "~/GSE174609_scRNA/integration_analysis/integrated_data/integrated_clustered_seurat.rds"

# Load the object
integrated_seurat <- readRDS(integrated_path)
```

**What we just loaded:**

- Integrated dataset with batch effects corrected (from [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Cells clustered into biologically meaningful groups

- UMAP dimensionality reduction for visualization

- Sample and condition metadata

**Quick verification:**

```r
# Determine which UMAP to use (from best integration method in Part 3)
available_reductions <- names(integrated_seurat@reductions)
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
```

**Understanding UMAP reductions**: The integrated object from [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) contains a UMAP based on the best integration method (Harmony, CCA, RPCA, or FastMNN). We automatically detect which UMAP is available and use it for all visualizations.

### Visualizing Starting Point

Before annotation, let’s visualize our clusters:

```r
#-----------------------------------------------
# STEP 3: Visualize clusters before annotation
#-----------------------------------------------

# Create multi-panel overview
p1 <- DimPlot(integrated_seurat, reduction = umap_reduction,
              group.by = "seurat_clusters", label = TRUE, label.size = 5,
              pt.size = 0.1) +
  ggtitle("Clusters (Pre-Annotation)") +
  theme(plot.title = element_text(face = "bold", size = 14))

p2 <- DimPlot(integrated_seurat, reduction = umap_reduction,
              group.by = "sample_id", pt.size = 0.1) +
  ggtitle("Samples") +
  theme(legend.text = element_text(size = 7))

p3 <- DimPlot(integrated_seurat, reduction = umap_reduction,
              group.by = "condition", pt.size = 0.1) +
  ggtitle("Condition") +
  scale_color_manual(values = c("Healthy" = "#2E86AB", 
                                 "Post_Treatment" = "#F18F01"))

# Combine
p_overview <- (p1 | p2) / (p3 | plot_spacer())

ggsave("plots/00_starting_clusters.png", p_overview, 
       width = 14, height = 10, dpi = 300)
```

**What to look for in this plot:**

- **Top left**: Numbered clusters showing computational grouping

- **Top right**: Sample mixing (good integration shows intermixed colors)

- **Bottom left**: Condition distribution (healthy vs post-treatment)

This baseline visualization helps you understand the data structure before assigning biological identities.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/00_starting_clusters.png?resize=1024%2C731&ssl=1)

## Normalizing and Scaling Data

Before annotation, we must ensure data is properly normalized and scaled for downstream analyses.

```r
#-----------------------------------------------
# STEP 4: Normalize and scale data
#-----------------------------------------------

# Normalize data (log-normalization)
integrated_seurat <- NormalizeData(integrated_seurat, 
                                   normalization.method = "LogNormalize",
                                   scale.factor = 10000, 
                                   verbose = FALSE)

# Scale all genes (required for DoHeatmap and marker analysis)
integrated_seurat <- ScaleData(integrated_seurat, 
                               features = rownames(integrated_seurat), 
                               verbose = FALSE)
```

**Why we normalize:**

- **Raw counts vary** by sequencing depth (some cells have more reads than others)

- **Log-normalization** makes gene expression comparable across cells

- **Scaling** centers and scales genes for visualization (heatmaps, etc.)

**Critical**: FindAllMarkers, FeaturePlot, DotPlot, and DoHeatmap all require normalized data. We handle this automatically in Step 4.

## Manual Cell Type Annotation: The Gold Standard

Manual annotation leverages biological expertise to assign cell type identities. While time-consuming, it remains the gold standard because it incorporates biological understanding that automated methods cannot replicate.

### Understanding PBMC Cell Types

Before annotating, let’s review the expected cell types in peripheral blood mononuclear cells (PBMCs):

**T cells****B cells****NK cells****Monocytes****Dendritic cells****Plasmacytoid DC****Platelets**

| Cell Type | Expected % | Key Markers | Function |
| --- | --- | --- | --- |
|  | 50-70% | CD3D, CD3E, CD3G | Adaptive immunity |
| ├─ CD4+ T cells | 30-40% | CD3+, CD4+, IL7R | Helper T cells |
| └─ CD8+ T cells | 20-30% | CD3+, CD8A, CD8B | Cytotoxic T cells |
|  | 10-15% | CD79A, MS4A1 (CD20), CD19 | Antibody production |
|  | 5-10% | NKG7, GNLY, NCAM1 (CD56) | Innate cytotoxicity |
|  | 10-20% | CD14, LYZ, S100A8, S100A9 | Phagocytosis |
| ├─ Classical | ~80% of mono | CD14+, FCGR3A- (CD16-) | CD14+ monocytes |
| └─ Non-classical | ~20% of mono | CD14low, FCGR3A+ (CD16+) | CD16+ monocytes |
|  | 1-2% | FCER1A, CST3 | Antigen presentation |
|  | <1% | IL3RA, GZMB, SERPINF1 | Type I interferon |
|  | Variable | PPBP, PF4, GP9 | Usually contamination |

**Frequency note**: These percentages are typical for healthy blood but can vary with disease, age, and technical factors.

### Step 1: Canonical Marker Genes

We’ll start by examining well-established marker genes for each PBMC cell type:

```r
#-----------------------------------------------
# STEP 5: Define and check canonical markers
#-----------------------------------------------

# Define comprehensive marker panel for PBMC cell types
canonical_markers <- list(
  "T_cells" = c("CD3D", "CD3E", "CD3G"),
  "CD4_T" = c("CD3D", "CD4", "IL7R"),
  "CD8_T" = c("CD3D", "CD8A", "CD8B"),
  "B_cells" = c("CD79A", "MS4A1", "CD19"),
  "NK_cells" = c("NKG7", "GNLY", "NCAM1"),
  "Monocytes" = c("CD14", "LYZ", "S100A8", "S100A9"),
  "Classical_Mono" = c("CD14", "S100A8", "FCGR3A"),
  "Non_Classical_Mono" = c("FCGR3A", "MS4A7"),
  "Dendritic_cells" = c("FCER1A", "CST3"),
  "Plasmacytoid_DC" = c("IL3RA", "GZMB", "SERPINF1"),
  "Platelets" = c("PPBP", "PF4", "GP9")
)

# Check which markers are present in dataset
all_genes <- rownames(integrated_seurat)

for (cell_type in names(canonical_markers)) {
  markers <- canonical_markers[[cell_type]]
  present <- markers %in% all_genes

  cat(sprintf("%-20s: %d/%d markers present\n", 
              cell_type, sum(present), length(markers)))

  if (!all(present)) {
    missing <- markers[!present]
    cat(sprintf("  Missing: %s\n", paste(missing, collapse = ", ")))
  }
}

# Platelets: Missing GP9
```

**What we’re checking:**

- **Marker presence**: Not all markers may be detected (gene dropout or filtering)

- **Dataset completeness**: Verify we have the markers needed for annotation

- **Missing markers**: Identify which cell types may be harder to annotate

If key markers are missing, you may need to rely more heavily on automated methods or use alternative markers from the literature.

### Step 2: Visualizing Canonical Markers

```r
#-----------------------------------------------
# STEP 6: Visualize canonical markers
#-----------------------------------------------

# Function to create violin plots for a marker set
plot_violin_markers <- function(markers, cell_type_name, filename_suffix) {
  present_markers <- markers[markers %in% rownames(integrated_seurat)]

  if (length(present_markers) == 0) {
    cat("  ⚠ No markers present for", cell_type_name, "\n")
    return(NULL)
  }

  cat("  •", cell_type_name, ":", length(present_markers), "markers\n")

  p <- VlnPlot(integrated_seurat,
               features = present_markers,
               group.by = "seurat_clusters",
               pt.size = 0,
               ncol = 3) &
    theme(legend.position = "none",
          axis.text.x = element_text(angle = 0, hjust = 0.5, size = 8),
          axis.title.x = element_blank(),
          plot.title = element_text(size = 11, face = "bold"))

  n_rows <- ceiling(length(present_markers) / 3)
  height <- max(4, n_rows * 2.5)

  ggsave(paste0("plots/manual_annotation/violin_", filename_suffix, ".png"),
         p, width = 14, height = height, dpi = 300)

  return(p)
}

# Function to create UMAP feature plots for a marker set
plot_umap_markers <- function(markers, cell_type_name, filename_suffix) {
  present_markers <- markers[markers %in% rownames(integrated_seurat)]

  if (length(present_markers) == 0) {
    return(NULL)
  }

  p <- FeaturePlot(integrated_seurat,
                   features = present_markers,
                   reduction = umap_reduction,
                   ncol = 4,
                   pt.size = 0.1) &
    theme(plot.title = element_text(size = 10),
          axis.text = element_blank(),
          axis.ticks = element_blank(),
          legend.position = "right")

  n_rows <- ceiling(length(present_markers) / 4)
  height <- max(4, n_rows * 3)

  ggsave(paste0("plots/manual_annotation/umap_", filename_suffix, ".png"),
         p, width = 16, height = height, dpi = 300)

  return(p)
}

# Generate plots for ALL cell types
for (cell_type_key in names(canonical_markers)) {
  markers <- canonical_markers[[cell_type_key]]
  cell_type_name <- gsub("_", " ", cell_type_key)

  plot_violin_markers(markers, cell_type_name, cell_type_key)
  plot_umap_markers(markers, cell_type_name, cell_type_key)
}

# Create comprehensive DotPlot showing all key markers
all_markers <- unique(unlist(canonical_markers))
all_markers_present <- all_markers[all_markers %in% rownames(integrated_seurat)]

p_dotplot <- DotPlot(integrated_seurat,
                     features = all_markers_present,
                     group.by = "seurat_clusters") +
  coord_flip() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 9),
        axis.text.y = element_text(size = 8)) +
  labs(title = "All Canonical Markers by Cluster",
       subtitle = paste(length(all_markers_present), "markers across all PBMC cell types"))

ggsave("plots/manual_annotation/dotplot_all_canonical_markers.png", p_dotplot,
       width = 12, height = max(8, length(all_markers_present) * 0.15), dpi = 300)
```

**How to read these plots:**

**Violin plots (cluster-specific expression):**

- **High expression** = Wide/tall violin with high values (>1-2 on y-axis)

- **Low/absent** = Thin/short violin near zero

- **Cluster-specific** = Only certain clusters show high expression

**DotPlot (comprehensive overview):**

- **Dot size** = Percentage of cells expressing the gene

- **Dot color** = Average expression level

- **Pattern matching** = Look for marker combinations that define cell types

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/scRNAseq_Marker_Gene_Expression.png?resize=1024%2C678&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/dotplot_all_canonical_markers.png?resize=1024%2C683&ssl=1)

### Step 3: Data-Driven Marker Discovery

Use `FindAllMarkers` to discover cluster-specific genes:

```r
#-----------------------------------------------
# STEP 7: Find cluster-specific marker genes
#-----------------------------------------------

# Find markers for each cluster vs all other cells
cluster_markers <- FindAllMarkers(
  integrated_seurat,
  only.pos = TRUE,
  min.pct = 0.25,
  logfc.threshold = 0.25,
  verbose = FALSE
)

# Remove ribosomal and mitochondrial genes
cluster_markers_filtered <- cluster_markers %>%
  filter(!grepl("^RP[SL]", gene)) %>%
  filter(!grepl("^MT-", gene))

# Get top 5 markers per cluster
top_markers <- cluster_markers_filtered %>%
  group_by(cluster) %>%
  top_n(n = 5, wt = avg_log2FC) %>%
  arrange(cluster, desc(avg_log2FC))

cat("\nTop 5 markers per cluster:\n")
print(top_markers %>% select(cluster, gene, avg_log2FC, pct.1, pct.2))

# Save all markers
write.csv(cluster_markers_filtered, 
          "annotations/cluster_markers_all.csv",
          row.names = FALSE)

write.csv(top_markers,
          "annotations/top5_markers_per_cluster.csv",
          row.names = FALSE)

# Create heatmap of top markers
top_genes <- top_markers$gene

p_heatmap <- DoHeatmap(
  integrated_seurat,
  features = top_genes,
  group.by = "seurat_clusters",
  size = 3
) +
  theme(axis.text.y = element_text(size = 6)) +
  labs(title = "Top 5 Markers per Cluster")

ggsave("plots/manual_annotation/06_heatmap_top_markers.png", p_heatmap,
       width = 12, height = 14, dpi = 300)
```

**How FindAllMarkers works:**

- **Compares each cluster** against all other cells

- **Identifies genes** that are significantly upregulated in that cluster

- **Statistical testing**: Uses Wilcoxon rank-sum test by default

- **Filters**: min.pct = 0.25 means gene must be in ≥25% of cluster cells

**Interpreting the results:**

- **avg_log2FC**: Average log2 fold-change (higher = more specific to cluster)

- **pct.1**: % of cells in cluster expressing the gene

- **pct.2**: % of cells in other clusters expressing the gene

- **Good markers**: High pct.1, low pct.2, high avg_log2FC

**Common issue**: Top markers may include ribosomal (RP) or mitochondrial (MT) genes, which reflect cell quality/metabolism rather than cell type identity. We filter these out.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/06_heatmap_top_markers.png?resize=878%2C1024&ssl=1)

### Step 4: Manual Cluster Assignment

Now combine canonical markers + FindAllMarkers results to assign cell types:

```r
#-----------------------------------------------
# STEP 8: Manual cell type assignment
#-----------------------------------------------

# EXAMPLE MAPPING - CUSTOMIZE FOR YOUR DATA
cluster_to_celltype <- c(
  "0" = "CD4+ T cells",
  "1" = "CD4+ T cells",
  "2" = "NK cells",
  "3" = "NK cells",
  "4" = "CD4+ T cells",
  "5" = "NK cells",
  "6" = "Classical Mono",
  "7" = "B Cells",
  "8" = "CD8+ T Cells",
  "9" = "B Cells",
  "10" = "CD4+ T cells",
  "11" = "CD4+ T cells",
  "12" = "Monocytes",
  "13" = "CD4+ T cells",
  "14" = "NK cells",
  "15" = "NK cells",
  "16" = "Platelets"
)

# Apply to all cells
cluster_ids <- as.character(integrated_seurat$seurat_clusters)
integrated_seurat$manual_annotation <- unname(cluster_to_celltype[cluster_ids])

# Visualize manual annotation
p_manual <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "manual_annotation",
  label = TRUE,
  label.size = 4,
  pt.size = 0.1,
  repel = TRUE
) +
  ggtitle("Manual Cell Type Annotation") +
  theme(plot.title = element_text(face = "bold", size = 14))

ggsave("plots/manual_annotation/07_manual_annotation_umap.png", p_manual,
       width = 10, height = 8, dpi = 300)

# Split by condition
p_manual_split <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "manual_annotation",
  split.by = "condition",
  pt.size = 0.1,
  ncol = 2
) +
  ggtitle("Manual Annotation by Condition")

ggsave("plots/manual_annotation/08_manual_annotation_by_condition.png",
       p_manual_split, width = 14, height = 6, dpi = 300)
```

**Critical**: The mapping above is an **EXAMPLE**. You must customize it based on YOUR marker expression plots. The assignment process:

1. **Examine violin plots** for canonical markers (Step 6)

2. **Check FindAllMarkers results** for cluster-specific genes (Step 7)

3. **Look for marker combinations** that uniquely identify cell types

4. **Assign labels** based on strongest evidence

5. **Merge similar clusters** if they share the same markers

**Decision framework:**

| Cluster | CD3D | CD4 | CD8A | CD14 | CD79A | NKG7 | Assignment |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | High | High | Low | Low | Low | Low | CD4+ T cells |
| 1 | High | Low | High | Low | Low | Low | CD8+ T cells |
| 2 | Low | Low | Low | Low | High | Low | B cells |
| 3 | Low | Low | Low | High | Low | Low | Monocytes |
| 4 | Low | Low | Low | Low | Low | High | NK cells |

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/08_manual_annotation_by_condition.png?resize=1024%2C439&ssl=1)

## SingleR: Reference-Based Annotation

SingleR (Single-cell Recognition) annotates cells by correlating their expression profiles with annotated reference datasets. We’ll test multiple references to find the best match for our PBMC data.

### Understanding SingleR’s Algorithm

**How SingleR works:**

1. **For each cell**: Calculate correlation with every cell type in the reference

2. **Fine-tuning**: Re-run using only genes variable within top-scoring types

3. **Quality control**: Calculate delta scores (difference between top and second-best match)

4. **Output**: Predicted cell type label + confidence scores per cell

**Key advantage**: Leverages expert-curated references rather than requiring manual marker curation.

### Available Reference Datasets

celldex provides several human references:

**HumanPrimaryCellAtlas****MonacoImmuneData****DatabaseImmuneCellExpression****BlueprintEncodeData****NovershternHematopoieticData**

| Reference | Cell Types | Source | Best For |
| --- | --- | --- | --- |
|  | 38 types | Microarray | Broad tissue coverage |
|  | 29 immune types | RNA-seq | Immune cells (PBMCs) |
|  | 15 immune types | Microarray | Basic immune |
|  | 43 types | RNA-seq | Mixed tissues |
|  | 38 hematopoietic | Microarray | Blood differentiation |

**For PBMC data**, MonacoImmuneData is typically best because it was specifically designed for immune cells and uses RNA-seq data (matching our technology).

### Testing Multiple References

```r
#-----------------------------------------------
# STEP 9: SingleR with multiple reference datasets
#-----------------------------------------------

# Convert Seurat to SingleCellExperiment for SingleR
sce <- as.SingleCellExperiment(integrated_seurat)

# Reference 1: HumanPrimaryCellAtlas (Broad)
hpca_ref <- celldex::HumanPrimaryCellAtlasData()

singler_hpca <- SingleR(
  test = sce,
  ref = hpca_ref,
  labels = hpca_ref$label.main,
  assay.type.test = "logcounts"
)

integrated_seurat$singler_hpca <- singler_hpca$labels

# Reference 2: MonacoImmuneData (Immune-Specific)
monaco_ref <- celldex::MonacoImmuneData()

singler_monaco <- SingleR(
  test = sce,
  ref = monaco_ref,
  labels = monaco_ref$label.main,
  assay.type.test = "logcounts"
)

integrated_seurat$singler_monaco <- singler_monaco$labels

# Reference 3: DatabaseImmuneCellExpression
dice_ref <- celldex::DatabaseImmuneCellExpressionData()

singler_dice <- SingleR(
  test = sce,
  ref = dice_ref,
  labels = dice_ref$label.main,
  assay.type.test = "logcounts"
)

integrated_seurat$singler_dice <- singler_dice$labels
```

**What each reference provides:**

- **HPCA**: Broad coverage (38 types) but includes non-immune cells → May misclassify PBMCs

- **Monaco**: Immune-specific (29 types), RNA-seq data → Best match for PBMC technology

- **DICE**: Basic immune types (15 categories) → Simpler, less granular

### Comparing References

```r
#-----------------------------------------------
# STEP 10: Compare SingleR references
#-----------------------------------------------

# Visualize all three references
p_hpca <- DimPlot(integrated_seurat, reduction = umap_reduction,
                  group.by = "singler_hpca", pt.size = 0.1) +
  labs(title = "HPCA Reference") +
  theme(legend.text = element_text(size = 7))

p_monaco <- DimPlot(integrated_seurat, reduction = umap_reduction,
                    group.by = "singler_monaco", pt.size = 0.1) +
  labs(title = "Monaco Reference") +
  theme(legend.text = element_text(size = 7))

p_dice <- DimPlot(integrated_seurat, reduction = umap_reduction,
                  group.by = "singler_dice", pt.size = 0.1) +
  labs(title = "DICE Reference") +
  theme(legend.text = element_text(size = 7))

p_manual_ref <- DimPlot(integrated_seurat, reduction = umap_reduction,
                        group.by = "manual_annotation", pt.size = 0.1) +
  labs(title = "Manual") +
  theme(legend.text = element_text(size = 7))

p_ref_comparison <- (p_manual_ref | p_hpca) / (p_monaco | p_dice)

ggsave("plots/automated_annotation/10_singler_reference_comparison.png",
       p_ref_comparison, width = 16, height = 12, dpi = 300)
```

**How to evaluate references:**

1. **Visual comparison**: Which reference’s UMAP pattern most closely matches manual annotation?

2. **Cell type names**: Do the reference labels make biological sense for PBMCs?

3. **Granularity**: Too many rare cell types suggests over-classification; too few suggests under-classification

4. **Spatial coherence**: Cell types should form spatially coherent clusters, not scattered randomly

**Expected outcome**: For PBMCs, Monaco typically shows the best spatial coherence and most appropriate immune cell type labels. HPCA may include irrelevant tissue types. DICE provides simpler categorization.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/10_singler_reference_comparison.png?resize=1024%2C768&ssl=1)

**Choosing the best reference:**

```r
# For this tutorial, we'll use Monaco as the primary SingleR annotation
integrated_seurat$singler_annotation <- integrated_seurat$singler_monaco

# Visualize chosen reference
p_singler_final <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "singler_annotation",
  label = TRUE,
  label.size = 3,
  pt.size = 0.1,
  repel = TRUE
) +
  ggtitle("SingleR Annotation (Monaco Reference)") +
  theme(plot.title = element_text(face = "bold", size = 14))

ggsave("plots/automated_annotation/11_singler_final_annotation.png",
       p_singler_final, width = 11, height = 8, dpi = 300)
```

**Why Monaco usually wins**: For PBMC data, Monaco typically shows the highest agreement because it was designed specifically for immune cells, uses RNA-seq data (same technology as scRNA-seq), and contains detailed immune cell subtypes well-annotated by immunology experts.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/11_singler_final_annotation.png?resize=1024%2C745&ssl=1)

## scType: Marker-Based Gene Set Scoring

scType scores each cluster based on enrichment of known marker genes, providing a transparent, marker-driven annotation approach.

### Loading scType Functions

```bash
#-----------------------------------------------
# STEP 11: Load scType functions
#-----------------------------------------------

# Load scType functions from GitHub
source("https://raw.githubusercontent.com/IanevskiAleksandr/sc-type/master/R/gene_sets_prepare.R")
source("https://raw.githubusercontent.com/IanevskiAleksandr/sc-type/master/R/sctype_score_.R")
```

**What scType provides:**

- **Marker database**: Curated lists of positive and negative markers per cell type

- **Scoring function**: Calculates enrichment of marker sets in each cluster

- **Transparent**: You can see exactly which markers drive each assignment

### Loading Marker Database

```r
#-----------------------------------------------
# STEP 12: Load scType marker database
#-----------------------------------------------

# Download the full database
db_url <- "https://raw.githubusercontent.com/IanevskiAleksandr/sc-type/master/ScTypeDB_full.xlsx"
temp_file <- tempfile(fileext = ".xlsx")
download.file(db_url, destfile = temp_file, mode = "wb")

# Read the database for immune system
library(openxlsx)
db <- read.xlsx(temp_file)
db_immune <- db[db$tissueType == "Immune system", ]

# Prepare gene sets
gs_list <- gene_sets_prepare(temp_file, "Immune system")

cat("Available cell types in database:\n")
print(names(gs_list$gs_positive))
```

**Database structure:**

- **Positive markers**: Genes that should be expressed in the cell type

- **Negative markers**: Genes that should NOT be expressed (help distinguish similar types)

- **Tissue-specific**: Database includes markers for multiple tissues (we’re using “Immune system”)

**Available cell types**: The database contains markers for common PBMC populations like T cells, B cells, NK cells, monocytes, dendritic cells, and their subtypes.

### Calculating scType Scores

```r
#-----------------------------------------------
# STEP 13: Calculate scType scores
#-----------------------------------------------

# Get scaled data
scaled_data <- as.matrix(LayerData(integrated_seurat, layer = "scale.data"))

# Calculate scores
es_max <- sctype_score(
  scRNAseqData = scaled_data,
  scaled = TRUE,
  gs = gs_list$gs_positive,
  gs2 = gs_list$gs_negative
)

# Assign cell types to clusters
clusters <- integrated_seurat$seurat_clusters
cL_results <- do.call("rbind", lapply(unique(clusters), function(cl) {
  es_max_cl <- sort(rowSums(es_max[, clusters == cl]), decreasing = TRUE)

  top_score <- es_max_cl[1]
  top_type <- names(es_max_cl)[1]

  data.frame(
    cluster = cl,
    type = top_type,
    scores = top_score,
    ncells = sum(clusters == cl)
  )
}))

# Filter low-confidence assignments
score_threshold <- quantile(cL_results$scores, 0.6)
cL_results$type[cL_results$scores < score_threshold] <- "Unknown"

# Create mapping and apply to cells
cluster_to_sctype <- setNames(cL_results$type, cL_results$cluster)
integrated_seurat$sctype_annotation <- unname(cluster_to_sctype[
  as.character(integrated_seurat$seurat_clusters)
])

# Visualize
p_sctype <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "sctype_annotation",
  label = TRUE,
  label.size = 4,
  pt.size = 0.1,
  repel = TRUE
) +
  ggtitle("scType: Marker-Based Annotation") +
  theme(plot.title = element_text(face = "bold", size = 14))

ggsave("plots/automated_annotation/12_sctype_annotation.png", p_sctype,
       width = 11, height = 8, dpi = 300)

# Save scType results
write.csv(cL_results, "annotations/sctype_cluster_scores.csv", row.names = FALSE)
```

**How scoring works:**

1. **For each cluster**: Calculate enrichment of positive markers and depletion of negative markers

2. **Assign cell type**: Cluster gets the label with the highest score

3. **Confidence filtering**: Clusters with scores below the 60th percentile are labeled “Unknown”

**Understanding scores:**

- **High score (>0)**: Strong enrichment of positive markers

- **Low score (<0)**: Weak or absent marker expression

- **Unknown label**: Score too low to confidently assign cell type

**Interpreting “Unknown”**: These clusters may represent:

- Cell types not in the scType database

- Mixed populations requiring higher clustering resolution

- Poor-quality cells that passed QC

- Transitional cell states

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/12_sctype_annotation.png?resize=1024%2C745&ssl=1)

## scCATCH: Tissue-Specific Automated Annotation

scCATCH (Single Cell Cluster-based Annotation Toolkit for Cellular Heterogeneity) automatically identifies cluster markers and matches them to tissue-specific cell type databases.

### Understanding scCATCH’s Approach

**How scCATCH works:**

1. **Marker Identification**: Automatically finds cluster-specific markers using built-in algorithms

2. **Database Matching**: Compares markers to tissue-specific reference databases

3. **Evidence-Based Assignment**: Provides marker evidence supporting each assignment

4. **Confidence Scoring**: Calculates confidence based on marker overlap

**Key advantage**: You don’t need to manually curate markers—scCATCH identifies them automatically and matches to its tissue-specific database.

### Running scCATCH

```r
#-----------------------------------------------
# STEP 14: scCATCH annotation
#-----------------------------------------------

# Prepare data for scCATCH
obj_sccatch <- createscCATCH(
  data = LayerData(integrated_seurat, layer = "data"),
  cluster = as.character(integrated_seurat$seurat_clusters)
)

# Find marker genes for each cluster
obj_sccatch <- findmarkergene(
  object = obj_sccatch,
  species = "Human",
  marker = cellmatch,
  tissue = "Blood",
  cancer = "Normal",
  cell_min_pct = 0.25,
  logfc = 0.25,
  pvalue = 0.05
)

# Find cell types based on markers
obj_sccatch <- findcelltype(obj_sccatch)

# Extract results
sccatch_celltype <- obj_sccatch@celltype

# Create cluster to cell type mapping
cluster_to_sccatch <- setNames(
  sccatch_celltype$cell_type,
  sccatch_celltype$cluster
)

# Apply to all cells
integrated_seurat$sccatch_annotation <- unname(cluster_to_sccatch[
  as.character(integrated_seurat$seurat_clusters)
])

# Handle unmapped clusters
integrated_seurat$sccatch_annotation[
  is.na(integrated_seurat$sccatch_annotation)
] <- "Unknown"

# Visualize
p_sccatch <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "sccatch_annotation",
  label = TRUE,
  label.size = 4,
  pt.size = 0.1,
  repel = TRUE
) +
  ggtitle("scCATCH: Tissue-Specific Database") +
  theme(plot.title = element_text(face = "bold", size = 14))

ggsave("plots/automated_annotation/13_sccatch_annotation.png", p_sccatch,
       width = 11, height = 8, dpi = 300)

# Save detailed results
write.csv(sccatch_celltype, 
          "annotations/sccatch_cluster_assignments.csv",
          row.names = FALSE)

# Save marker evidence
marker_evidence <- obj_sccatch@markergene
write.csv(marker_evidence,
          "annotations/sccatch_marker_evidence.csv",
          row.names = FALSE)
```

**Parameters explained:**

- **species = “Human”**: Use human gene symbols

- **tissue = “Blood”**: Use blood-specific marker database (optimal for PBMCs)

- **cancer = “Normal”**: Normal tissue (not tumor)

- **cell_min_pct = 0.25**: Marker must be in ≥25% of cluster cells

- **logfc = 0.25**: Minimum log fold-change for marker detection

- **pvalue = 0.05**: Statistical significance threshold

**Marker evidence**: scCATCH saves the markers it used for each cluster assignment. This transparency allows you to validate that assignments are based on appropriate markers.

**Understanding celltype_score**: Represents the overlap between your cluster’s markers and the reference database markers for that cell type. Higher scores indicate stronger evidence.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/13_sccatch_annotation.png?resize=1024%2C745&ssl=1)

## Comparing Annotation Methods

Now we’ll systematically compare all four annotation approaches to identify agreements, disagreements, and patterns.

### Confusion Matrices

```r
#-----------------------------------------------
# STEP 15: Generate confusion matrices
#-----------------------------------------------

# Function to create confusion matrix
create_confusion_matrix <- function(method1_labels, method2_labels, 
                                   method1_name, method2_name) {
  conf_table <- table(
    Method1 = method1_labels,
    Method2 = method2_labels
  )

# Compare each automated method to manual
conf_manual_vs_singler <- create_confusion_matrix(
  integrated_seurat$manual_annotation,
  integrated_seurat$singler_annotation,
  "Manual", "SingleR"
)

conf_manual_vs_sctype <- create_confusion_matrix(
  integrated_seurat$manual_annotation,
  integrated_seurat$sctype_annotation,
  "Manual", "scType"
)

conf_manual_vs_sccatch <- create_confusion_matrix(
  integrated_seurat$manual_annotation,
  integrated_seurat$sccatch_annotation,
  "Manual", "scCATCH"
)

# Save confusion matrices
write.csv(conf_manual_vs_singler, 
          "annotations/confusion_matrix_manual_vs_singler.csv")
write.csv(conf_manual_vs_sctype,
          "annotations/confusion_matrix_manual_vs_sctype.csv")
write.csv(conf_manual_vs_sccatch,
          "annotations/confusion_matrix_manual_vs_sccatch.csv")
```

**How to read confusion matrices:**

Rows = Method 1 labels, Columns = Method 2 labels, Values = Number of cells

**Perfect agreement**: All values are assigned to the same cell type.**Partial agreement**: A majority (or substantial portion) of values are assigned to the same cell type, with some scattered elsewhere.**Disagreement**: Values are distributed across multiple cell types with no clear dominant category

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/confusion_matrix_sctype.png?resize=1024%2C410&ssl=1)

### Sankey Diagrams

Sankey diagrams visualize how cells flow from one annotation to another:

```r
#-----------------------------------------------
# STEP 16: Create Sankey diagrams
#-----------------------------------------------

# Function to prepare Sankey data
prepare_sankey_data <- function(method1_labels, method2_labels, 
                               method1_name, method2_name) {
  sankey_df <- data.frame(
    Method1 = method1_labels,
    Method2 = method2_labels
  ) %>%
    group_by(Method1, Method2) %>%
    summarise(Count = n(), .groups = "drop") %>%
    mutate(Comparison = paste(method1_name, "vs", method2_name))

  return(sankey_df)
}

# Prepare data for each comparison
sankey_manual_singler <- prepare_sankey_data(
  integrated_seurat$manual_annotation,
  integrated_seurat$singler_annotation,
  "Manual", "SingleR"
)

sankey_manual_sctype <- prepare_sankey_data(
  integrated_seurat$manual_annotation,
  integrated_seurat$sctype_annotation,
  "Manual", "scType"
)

sankey_manual_sccatch <- prepare_sankey_data(
  integrated_seurat$manual_annotation,
  integrated_seurat$sccatch_annotation,
  "Manual", "scCATCH"
)

# Function to create Sankey plot
create_sankey_plot <- function(sankey_data, method1_name, method2_name) {
  p <- ggplot(sankey_data,
              aes(axis1 = Method1, axis2 = Method2, y = Count)) +
    geom_alluvium(aes(fill = Method1), width = 1/12, alpha = 0.7) +
    geom_stratum(width = 1/12, fill = "white", color = "grey") +
    geom_text(stat = "stratum", aes(label = after_stat(stratum)), 
              size = 3, fontface = "bold") +
    scale_x_discrete(limits = c(method1_name, method2_name), 
                     expand = c(0.05, 0.05)) +
    labs(title = paste(method1_name, "vs", method2_name),
         subtitle = "Straight flows = agreement | Crossed flows = disagreement",
         y = "Number of Cells") +
    theme_minimal() +
    theme(legend.position = "none",
          axis.text.y = element_blank(),
          axis.title.x = element_blank(),
          plot.title = element_text(face = "bold"))

  return(p)
}

# Create Sankey plots
p_sankey_singler <- create_sankey_plot(sankey_manual_singler, "Manual", "SingleR")
ggsave("plots/method_comparison/14_sankey_manual_vs_singler.png",
       p_sankey_singler, width = 12, height = 10, dpi = 300)

p_sankey_sctype <- create_sankey_plot(sankey_manual_sctype, "Manual", "scType")
ggsave("plots/method_comparison/15_sankey_manual_vs_sctype.png",
       p_sankey_sctype, width = 12, height = 10, dpi = 300)

p_sankey_sccatch <- create_sankey_plot(sankey_manual_sccatch, "Manual", "scCATCH")
ggsave("plots/method_comparison/16_sankey_manual_vs_sccatch.png",
       p_sankey_sccatch, width = 12, height = 10, dpi = 300)
```

**Reading Sankey diagrams:**

- **Straight vertical flows**: Methods agree (same cell assigned to corresponding type)

- **Crossed/angled flows**: Methods disagree (cell assigned to different types)

- **Flow thickness**: Number of cells following that path

- **Many thin flows**: High disagreement, scattered assignments

- **Few thick flows**: Strong consensus

**What good agreement looks like:**

- Most flows are straight or minimally angled

- Few very thin scattered flows

- Logical correspondences (e.g., Manual “CD4+ T” → SingleR “T cells, CD4+”)

**What poor agreement looks like:**

- Many crossed flows

- Assignments scattered across unrelated types

- Thick flows to “Unknown” or ambiguous labels

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/15_sankey_manual_vs_sctype.png?resize=1024%2C853&ssl=1)

### Side-by-Side UMAP Comparison

```r
#-----------------------------------------------
# STEP 17: Create comprehensive UMAP comparison
#-----------------------------------------------

# Create individual plots with consistent styling
p_manual_comp <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "manual_annotation",
  pt.size = 0.1
) +
  labs(title = "Manual") +
  theme(legend.text = element_text(size = 7),
        plot.title = element_text(face = "bold"))

p_singler_comp <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "singler_annotation",
  pt.size = 0.1
) +
  labs(title = "SingleR (Monaco)") +
  theme(legend.text = element_text(size = 7),
        plot.title = element_text(face = "bold"))

p_sctype_comp <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "sctype_annotation",
  pt.size = 0.1
) +
  labs(title = "scType (Markers)") +
  theme(legend.text = element_text(size = 7),
        plot.title = element_text(face = "bold"))

p_sccatch_comp <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "sccatch_annotation",
  pt.size = 0.1
) +
  labs(title = "scCATCH (Tissue DB)") +
  theme(legend.text = element_text(size = 7),
        plot.title = element_text(face = "bold"))

# Combine all four methods
p_all_methods <- (p_manual_comp | p_singler_comp) /
                 (p_sctype_comp | p_sccatch_comp)

ggsave("plots/method_comparison/17_all_methods_comparison.png",
       p_all_methods, width = 18, height = 14, dpi = 300)
```

**How to evaluate this comparison:**

**Look for spatial coherence:**

- Do cells of the same type form contiguous regions on the UMAP?

- Are there “islands” of one cell type scattered throughout?

- Do boundaries between cell types make sense?

**Compare label positions:**

- Where Manual labels a region “CD4+ T cells”, what do automated methods call it?

- Are there systematic differences (e.g., SingleR calls it “T cells, CD4+” while scType says “T cells”)?

- Do methods agree on rare cell types (dendritic cells, platelets)?

**Identify problem areas:**

- Regions where all methods disagree (ambiguous populations)

- Cell types unique to one method (potential errors or discoveries)

- Areas labeled “Unknown” by automated methods (may need manual curation)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/17_all_methods_comparison.png?resize=1024%2C796&ssl=1)

## Building Consensus Annotations: Manual Comparison Strategy

Different annotation methods use different naming conventions (e.g., “CD4+ T cells” vs “T cells, CD4+” vs “CD4 T”), making automated consensus challenging. Instead, we’ll **manually compare** methods to build a final consensus annotation.

### Step 1: Export Annotations for Manual Review

```r
#-----------------------------------------------
# STEP 18: Export annotations for manual comparison
#-----------------------------------------------

# Create comprehensive annotation table
annotation_comparison <- data.frame(
  cell_barcode = colnames(integrated_seurat),
  cluster = integrated_seurat$seurat_clusters,
  manual = integrated_seurat$manual_annotation,
  singler = integrated_seurat$singler_annotation,
  sctype = integrated_seurat$sctype_annotation,
  sccatch = integrated_seurat$sccatch_annotation,
  sample_id = integrated_seurat$sample_id,
  condition = integrated_seurat$condition,
  stringsAsFactors = FALSE
)

# Export to CSV for manual review
write.csv(annotation_comparison,
          "annotations/all_methods_comparison.csv",
          row.names = FALSE)

# Create cluster-level summary (easier to review)
cluster_summary <- annotation_comparison %>%
  group_by(cluster) %>%
  summarise(
    n_cells = n(),
    manual_type = names(sort(table(manual), decreasing = TRUE))[1],
    singler_type = names(sort(table(singler), decreasing = TRUE))[1],
    sctype_type = names(sort(table(sctype), decreasing = TRUE))[1],
    sccatch_type = names(sort(table(sccatch), decreasing = TRUE))[1],
    .groups = "drop"
  )

write.csv(cluster_summary,
          "annotations/cluster_level_comparison.csv",
          row.names = FALSE)
```

**What we’ve created:**

1. **Cell-level file**: Every cell with labels from all 4 methods (useful for detailed analysis)

2. **Cluster-level file**: One row per cluster showing the dominant label from each method (easier to review)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/cluster_level_comparison.png?resize=1024%2C495&ssl=1)

### Step 2: Manual Comparison Workflow

Open `cluster_level_comparison.csv` in Excel, Google Sheets, or any spreadsheet software and follow this process:

**A. Create a consensus column:**

Add a new column called `consensus_annotation` where you’ll record your final decision.

**B. Decision rules for consensus:**

**All 4 methods agree (same biology, different naming)** → Accept consensus

- Example: Manual “CD4+ T cells”, SingleR “T cells, CD4+”, scType “T cells”, scCATCH “CD4 T cell”

- Consensus: “CD4+ T cells”

**3/4 methods agree** → Usually accept majority

1. 

- Check marker expression to confirm

- Example: If 3 methods say “B cells” and one says “Unknown”, validate with CD79A/MS4A1

**2/4 methods agree (split decision)** → Requires investigation

1. 

- Go back to marker plots (Step 6-7)

- Check which markers support each assignment

- Choose based on strongest marker evidence

- Document reasoning in Notes column

**All methods disagree** → Manual investigation required

1. 

- Return to violin plots and FindAllMarkers results

- This cluster may represent:

  - Mixed population (needs higher resolution)

  - Transitional state (between two types)

  - Low-quality cells (should have been filtered in QC)

  - Novel population (requires literature research)

### Step 3: Import Consensus and Finalize

After manual review, import your consensus decisions:

```r
#-----------------------------------------------
# STEP 19: Import consensus annotations
#-----------------------------------------------

# Read your manually edited cluster consensus file
# (Save your Excel/Google Sheets file as CSV first)
cluster_consensus <- read.csv("annotations/cluster_level_comparison.csv",
                              stringsAsFactors = FALSE)

# Create mapping from cluster to consensus annotation
cluster_to_consensus <- setNames(
  cluster_consensus$consensus_annotation,  # Column you created
  cluster_consensus$cluster
)

# Apply consensus to all cells
integrated_seurat$final_annotation <- unname(cluster_to_consensus[
  as.character(integrated_seurat$seurat_clusters)
])

# Visualize final consensus
p_consensus <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "final_annotation",
  label = TRUE,
  label.size = 4,
  pt.size = 0.1,
  repel = TRUE
) +
  ggtitle("Final Consensus Annotation") +
  theme(plot.title = element_text(face = "bold", size = 14))

ggsave("plots/method_comparison/18_final_consensus_annotation.png",
       p_consensus, width = 11, height = 8, dpi = 300)

# Split by condition to check biological validity
p_consensus_split <- DimPlot(
  integrated_seurat,
  reduction = umap_reduction,
  group.by = "final_annotation",
  split.by = "condition",
  pt.size = 0.1,
  ncol = 2
) +
  ggtitle("Final Consensus by Condition")

ggsave("plots/method_comparison/19_consensus_by_condition.png",
       p_consensus_split, width = 14, height = 6, dpi = 300)
```

**What to check in consensus plots:**

- **Spatial coherence**: Each cell type should form contiguous regions

- **Biological plausibility**: Cell type proportions should match expected PBMC composition

- **Condition effects**: Some types may differ between Healthy and Post-Treatment (expected biology)

- **No “Unknown” if possible**: Every cluster should have a confident label

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/18_final_consensus_annotation.png?resize=1024%2C745&ssl=1)

## Saving Annotated Data

```r
#-----------------------------------------------
# STEP 20: Save final annotated Seurat object
#-----------------------------------------------

# Save the complete annotated Seurat object
saveRDS(integrated_seurat, "annotations/integrated_annotated_seurat.rds")

# Create metadata export
metadata_export <- integrated_seurat@meta.data %>%
  select(
    sample_id, condition, patient_id,
    seurat_clusters,
    manual_annotation,
    singler_annotation,
    sctype_annotation,
    sccatch_annotation,
    final_annotation
  )

write.csv(metadata_export,
          "annotations/cell_metadata_final.csv",
          row.names = TRUE)
```

**What we’ve saved:**

1. **integrated_annotated_seurat.rds**: Complete Seurat object with all annotations

2. **cell_metadata_final.csv**: Cell-level metadata for sharing/publication

3. **All comparison files**: Documentation of how consensus was reached

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/cell_metadata_final.png?resize=1024%2C90&ssl=1)

## Best Practices for Cell Type Annotation

### General Principles

**1. Multiple Lines of Evidence**

- Never rely on a single marker or method

- Combine automated methods with manual validation

- Use both positive and negative markers

**2. Biological Context**

- Consider tissue type (markers vary by tissue)

- Account for species differences (human vs mouse)

- Remember disease state effects (pathology changes expression)

**3. Appropriate Resolution**

- Start broad (T cells, B cells, monocytes)

- Refine to subtypes only if confident

- Don’t over-split populations without evidence

**4. Conservative When Uncertain**

- Label ambiguous cells as “Unknown” rather than guessing

- Merge similar clusters if distinction is unclear

- Broader categories are safer than wrong fine-grained labels

### Method Selection Guidelines

**Use Manual Annotation When:**

- ✓ You have domain expertise in the tissue

- ✓ Novel tissue or condition (no good references)

- ✓ Small dataset (<5,000 cells)

- ✓ Research requires deep understanding

**Use Reference-Based (SingleR) When:**

- ✓ Standard tissue with good reference atlases

- ✓ Large dataset (>50,000 cells)

- ✓ Need fast, scalable annotation

- ✓ Reference matches your tissue/technology

**Use Marker-Based (scType/scCATCH) When:**

- ✓ Well-characterized tissue (known markers)

- ✓ No appropriate reference available

- ✓ Want transparent, interpretable assignments

- ✓ Need to understand which markers drive decisions

**Use Multiple Methods When:**

- ✓ Publication-quality annotation needed

- ✓ Important biological conclusions depend on cell types

- ✓ Dataset is challenging (mixed states, transitions)

- ✓ Budget allows time for thorough validation

### Reference Selection (SingleR)

**For PBMCs:**

- **Best**: MonacoImmuneData (immune-specific, RNA-seq)

- **Alternative**: DatabaseImmuneCellExpression (basic immune types)

- **Avoid**: HumanPrimaryCellAtlas (too broad, includes non-immune)

**For Other Tissues:**

- **Brain**: HumanPrimaryCellAtlas (good neural coverage)

- **Mixed**: BlueprintEncodeData (diverse tissue types)

- **Development**: NovershternHematopoieticData (hematopoietic lineages)

**Reference Quality Checks:**

- Verify reference matches your tissue type

- Check if reference used same technology (RNA-seq vs microarray)

- Confirm reference includes expected cell types

- Test multiple references if available

### Common Pitfalls to Avoid

**Pitfall 1: Over-Reliance on Automated Methods**

- **Problem**: Algorithms can be confidently wrong

- **Solution**: Always validate with marker expression

- **Example**: SingleR might call stressed cells “activated” based on reference

**Pitfall 2: Wrong Reference Dataset**

- **Problem**: Using brain reference for blood data

- **Solution**: Match reference to tissue type and technology

- **Example**: HumanPrimaryCellAtlas for PBMCs gives poor results vs Monaco

**Pitfall 3: Ignoring Biological Context**

- **Problem**: Markers work differently in disease vs health

- **Solution**: Consider activation states, stress responses

- **Example**: Inflamed monocytes express different genes than steady-state

**Pitfall 4: Over-Splitting Cell Types**

- **Problem**: Creating 50 clusters for 8 biological cell types

- **Solution**: Merge clusters with same markers and function

- **Example**: Three CD4+ T cell clusters might all be naive T cells

**Pitfall 5: Trusting Single Markers**

- **Problem**: Single markers are rarely specific

- **Solution**: Require combinations of markers

- **Example**: CD4 alone doesn’t distinguish T cells from some myeloid cells

**Pitfall 6: Not Validating Rare Cell Types**

- **Problem**: Artifacts or doublets called as rare types

- **Solution**: Extra scrutiny for <1% populations

- **Example**: “Plasmacytoid DC” cluster that’s actually ambient RNA

**Pitfall 7: Automated Consensus Without Manual Review**

- **Problem**: Different naming conventions prevent accurate automated comparison

- **Solution**: Manually compare methods in a spreadsheet, reconcile differences

- **Example**: “CD4+ T cells” vs “T cells, CD4+” vs “CD4 T cell” are the same biologically

### Troubleshooting Common Issues

**Issue 0: All Markers Have Extremely High or Low Values**

**Diagnosis:**

- FindAllMarkers returns genes with fold-changes of 100x or more

- Marker genes show values in thousands instead of 0-10 range

- All cells show nearly identical expression patterns

**Cause:**

- Using raw UMI counts instead of log-normalized values

- Critical error affecting all downstream analyses

**Solution:** We handle this automatically in Step 4, but if you encounter this issue:

**Prevention:**

- Our tutorial includes automatic normalization in Step 4

- Always normalize immediately after loading integrated data

- Normalization is required before: FindAllMarkers, FeaturePlot, DoHeatmap, differential expression

**Issue 1: Methods Strongly Disagree**

**Diagnosis:**

- Confusion matrices show scattered values

- Sankey diagrams have many crossed flows

- UMAP comparisons show completely different patterns

**Solutions:**

- Return to marker expression validation

- Check for batch effects or low-quality clusters

- Use more conservative annotation (broader types)

- Consider that cells may be in transition states

- Try different references or update marker lists

**Issue 2: Automated Methods Label Everything “Unknown”**

**Diagnosis:**

- scType assigns >50% clusters as “Unknown”

- scCATCH fails to match clusters to database

- Low confidence scores across the board

**Causes:**

- Reference doesn’t match tissue (wrong tissue type)

- Marker databases incomplete for your cell types

- Poor data quality (should have been caught in QC)

- Novel cell types not in any database

**Solutions:**

- Try different reference (tissue-specific)

- Manually curate marker lists for your tissue

- Check if QC was too lenient (keeping low-quality cells)

- Consider that you may have discovered novel populations

**Issue 3: Too Many Cell Types Assigned**

**Diagnosis:**

- 20+ cell types identified in PBMC data (expect 8-12)

- Many rare cell types (<1% of cells)

- Cell types that shouldn’t exist in tissue

**Causes:**

- Over-splitting due to batch effects

- Reference too granular

- Clustering resolution too high

- Doublets creating artificial hybrid types

**Solutions:**

- Lower clustering resolution (merge similar clusters)

- Use broader reference categories

- Check for batch-driven clusters

- Re-examine doublet detection from QC

**Issue 4: Expected Cell Types Missing**

**Diagnosis:**

- Known cell types absent (e.g., no B cells in PBMC data)

- Cell type present but mislabeled

- Rare cells merged with abundant types

**Causes:**

- Over-filtering in QC (removed rare types)

- Rare cells have low UMI/gene counts

- Markers not detected (dropout)

- Clustering resolution too low

**Solutions:**

- Re-examine QC thresholds (may have been too strict)

- Check if rare cells exist but mislabeled (examine FindAllMarkers)

- Use more sensitive marker lists

- Increase clustering resolution for rare types

- Use reference-based methods (better at detecting rare types)

## Conclusion: From Clusters to Cell Types

Congratulations! You’ve completed comprehensive cell type annotation using four complementary methods:

✓ **Manual annotation** – Biological expertise and domain knowledge✓ **SingleR** – Reference-based matching (tested multiple atlases)✓ **scType** – Marker-based gene set scoring✓ **scCATCH** – Tissue-specific automated identification

### Key Takeaways

**1. Multiple Methods Provide Confidence**

Agreement across diverse approaches (reference-based + marker-based + manual) gives you confidence that annotations are correct. Disagreement highlights ambiguous populations that need investigation.

**2. Manual Validation is Essential**

Even the best automated methods can be wrong. Always validate with marker expression, biological knowledge, and common sense.

**3. Context Matters**

The same gene can mean different things in different tissues, species, or disease states. Choose references and markers appropriate for your specific biological context.

**4. Conservative Annotation is Safer**

When uncertain, use broader categories (“T cells”) rather than guessing at subtypes (“Th17 cells”). You can always refine later with additional evidence.

## References

1. Aran D, Looney AP, Liu L, et al. Reference-based analysis of lung single-cell sequencing reveals a transitional profibrotic macrophage. Nat Immunol. 2019;20(2):163-172. doi:10.1038/s41590-018-0276-y [SingleR]

2. Ianevski A, Giri AK, Aittokallio T. Fully-automated and ultra-fast cell-type identification using specific marker combinations from single-cell transcriptomic data. Nat Commun. 2022;13(1):1246. doi:10.1038/s41467-022-28803-w [scType]

3. Shao X, Liao J, Lu X, et al. scCATCH: Automatic Annotation on Cell Types of Clusters from Single-Cell RNA Sequencing Data. iScience. 2020;23(3):100882. doi:10.1016/j.isci.2020.100882 [scCATCH]

4. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. Nat Biotechnol. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

5. Luecken MD, Theis FJ. Current best practices in single-cell RNA-seq analysis: a tutorial. Mol Syst Biol. 2019;15(6):e8746. doi:10.15252/msb.20188746

6. Abdelaal T, Michielsen L, Cats D, et al. A comparison of automatic cell identification methods for single-cell RNA sequencing data. Genome Biol. 2019;20(1):194. doi:10.1186/s13059-019-1795-z

7. Pasquini G, Rojo Arias JE, Schäfer P, Busskamp V. Automated methods for cell type annotation on scRNA-seq data. Comput Struct Biotechnol J. 2021;19:961-969. doi:10.1016/j.csbj.2021.01.015

8. Pliner HA, Shendure J, Trapnell C. Supervised classification enables rapid annotation of cell atlases. Nat Methods. 2019;16(10):983-986. doi:10.1038/s41592-019-0535-3 [Garnett]

9. Lotfollahi M, Naghipourfar M, Luecken MD, et al. Mapping single-cell data to reference atlases by transfer learning. Nat Biotechnol. 2022;40(1):121-130. doi:10.1038/s41587-021-01001-7

10. Zhang AW, O’Flanagan C, Chavez EA, et al. Probabilistic cell-type assignment of single-cell RNA-seq for tumor microenvironment profiling. Nat Methods. 2019;16(10):1007-1015. doi:10.1038/s41592-019-0529-1

11. Franzén O, Gan LM, Björkegren JLM. PanglaoDB: a web server for exploration of mouse and human single-cell RNA sequencing data. Database (Oxford). 2019;2019:baz046. doi:10.1093/database/baz046

12. Cao Y, Wang X, Peng G. SCSA: a cell type annotation tool for single-cell RNA-seq data. Front Genet. 2020;11:490. doi:10.3389/fgene.2020.00490

13. Kiselev VY, Yiu A, Hemberg M. scmap: projection of single-cell RNA-seq data across data sets. Nat Methods. 2018;15(5):359-362. doi:10.1038/nmeth.4644

14. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. J Transl Med. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source]

15. Monaco G, Lee B, Xu W, et al. RNA-Seq Signatures Normalized by mRNA Abundance Allow Absolute Deconvolution of Human Immune Cell Types. Cell Rep. 2019;26(6):1627-1640.e7. doi:10.1016/j.celrep.2019.01.041 [Monaco reference]

---

**This tutorial is part of the NGS101.com series on single cell sequencing analysis. If this tutorial helped advance your research, **please comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.**
