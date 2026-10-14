# How to Analyze Single-Cell RNA-seq Data – Complete Beginner’s Guide Part 6: Understanding Seurat and SingleCellExperiment Objects

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** September 28, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/)

- [Introduction: Why Understanding Data Objects Matters](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Introduction_Why_Understanding_Data_Objects_Matters)

  - [The “Black Box” Problem in Single-Cell Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#The_%E2%80%9CBlack_Box%E2%80%9D_Problem_in_Single-Cell_Analysis)
  - [What You’ll Learn in This Tutorial](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#What_Youll_Learn_in_This_Tutorial)
  - [Why Multiple Data Structures Exist in Single-Cell Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Why_Multiple_Data_Structures_Exist_in_Single-Cell_Analysis)
  - [Learning Approach: Hands-On Exploration](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Learning_Approach_Hands-On_Exploration)

- [Setting Up Your R Environment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Setting_Up_Your_R_Environment)

  - [Using the Environment from Previous Tutorials](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Using_the_Environment_from_Previous_Tutorials)
  - [Loading Libraries and Configuration](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Loading_Libraries_and_Configuration)
  - [Loading Example Data from Previous Tutorials](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Loading_Example_Data_from_Previous_Tutorials)

- [Understanding R Objects: The Foundation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Understanding_R_Objects_The_Foundation)

  - [What Is an R Object?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#What_Is_an_R_Object)
  - [Object Classes in R](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Object_Classes_in_R)
  - [S3 vs S4 Objects: What’s the Difference?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#S3_vs_S4_Objects_Whats_the_Difference)
  - [Inspecting Object Structure](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Inspecting_Object_Structure)

- [Deep Dive into Seurat Objects (Seurat 5)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Deep_Dive_into_Seurat_Objects_Seurat_5)

  - [What Is a Seurat Object?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#What_Is_a_Seurat_Object)
  - [Creating a Simple Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Creating_a_Simple_Seurat_Object)
  - [The Complete Seurat Object Architecture](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#The_Complete_Seurat_Object_Architecture)
  - [Component 1: Assays – Where Expression Data Lives](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Component_1_Assays_%E2%80%93_Where_Expression_Data_Lives)
  - [Component 2: Metadata – Cell and Gene Information](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Component_2_Metadata_%E2%80%93_Cell_and_Gene_Information)
  - [Component 3: Dimensionality Reductions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Component_3_Dimensionality_Reductions)
  - [Component 4: Nearest Neighbor Graphs](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Component_4_Nearest_Neighbor_Graphs)
  - [Component 5: Analysis History (@commands)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Component_5_Analysis_History_commands)

- [Understanding SingleCellExperiment Objects](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Understanding_SingleCellExperiment_Objects)

  - [What Is a SingleCellExperiment Object?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#What_Is_a_SingleCellExperiment_Object)
  - [Creating a SingleCellExperiment Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Creating_a_SingleCellExperiment_Object)
  - [SingleCellExperiment Architecture](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#SingleCellExperiment_Architecture)
  - [Converting Seurat to SingleCellExperiment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Converting_Seurat_to_SingleCellExperiment)
  - [Accessing Data in SingleCellExperiment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Accessing_Data_in_SingleCellExperiment)
  - [When to Use SingleCellExperiment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#When_to_Use_SingleCellExperiment)

- [Understanding SummarizedExperiment Objects](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Understanding_SummarizedExperiment_Objects)

  - [What Is a SummarizedExperiment Object?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#What_Is_a_SummarizedExperiment_Object)
  - [SummarizedExperiment Structure](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#SummarizedExperiment_Structure)
  - [Creating a SummarizedExperiment for Pseudobulk Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Creating_a_SummarizedExperiment_for_Pseudobulk_Analysis)
  - [Accessing SummarizedExperiment Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Accessing_SummarizedExperiment_Data)
  - [When to Use SummarizedExperiment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#When_to_Use_SummarizedExperiment)

- [Practical Use Cases and Object Conversions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Practical_Use_Cases_and_Object_Conversions)

  - [When to Use Each Object Type](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#When_to_Use_Each_Object_Type)
  - [Converting Between Objects](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Converting_Between_Objects)

- [Common Issues and Troubleshooting](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Common_Issues_and_Troubleshooting)

  - [Issue 1: “Cannot Find Layer/Slot”](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Issue_1_%E2%80%9CCannot_Find_LayerSlot%E2%80%9D)
  - [Issue 2: “Assay Does Not Exist”](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Issue_2_%E2%80%9CAssay_Does_Not_Exist%E2%80%9D)
  - [Issue 3: “Subscript Out of Bounds”](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Issue_3_%E2%80%9CSubscript_Out_of_Bounds%E2%80%9D)
  - [Issue 4: Metadata Column Doesn’t Exist](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Issue_4_Metadata_Column_Doesnt_Exist)
  - [Issue 5: Conversion Fails – Missing Logcounts](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Issue_5_Conversion_Fails_%E2%80%93_Missing_Logcounts)
  - [Issue 6: Memory Errors with Large Objects](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Issue_6_Memory_Errors_with_Large_Objects)
  - [Issue 7: Layer Joining Errors](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Issue_7_Layer_Joining_Errors)
  - [Debugging Checklist](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Debugging_Checklist)

- [Best Practices for Working with scRNA-seq Objects](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Best_Practices_for_Working_with_scRNA-seq_Objects)

  - [1. Use Accessor Functions Over Direct Access](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#1_Use_Accessor_Functions_Over_Direct_Access)
  - [2. Set and Verify Default Assay](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#2_Set_and_Verify_Default_Assay)
  - [3. Keep Metadata Clean and Documented](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#3_Keep_Metadata_Clean_and_Documented)
  - [4. Version Your Objects](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#4_Version_Your_Objects)
  - [5. Validate After Conversion](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#5_Validate_After_Conversion)
  - [6. Use Object-Specific Functions When Possible](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#6_Use_Object-Specific_Functions_When_Possible)
  - [7. Handle Missing Data Gracefully](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#7_Handle_Missing_Data_Gracefully)
  - [8. Document Object Modifications](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#8_Document_Object_Modifications)
  - [9. Subset Carefully](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#9_Subset_Carefully)
  - [10. Regular Object Validation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#10_Regular_Object_Validation)

- [Conclusion: From Confusion to Mastery](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Conclusion_From_Confusion_to_Mastery)

  - [From Black Box to Transparency](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#From_Black_Box_to_Transparency)
  - [Empowering Your Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Empowering_Your_Analysis)
  - [Impact on Downstream Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Impact_on_Downstream_Analysis)
  - [Final Thoughts](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Final_Thoughts)

- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/#Like_this)

## Introduction: Why Understanding Data Objects Matters

### The “Black Box” Problem in Single-Cell Analysis

If you’ve worked through Parts 1-5 of this tutorial series, you’ve successfully:

- Processed raw sequencing data ([Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/))

- Applied quality control filters ([Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/))

- Integrated and clustered your data ([Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Annotated cell types ([Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/))

- Performed differential expression analysis ([Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/))

But here’s what many beginners (and even experienced analysts) struggle with:

**Where is my data actually stored?**

**How do I access specific information?**

**Why do some functions only work with SingleCellExperiment object?**

**What’s the difference between `seurat_obj@assays$RNA@counts` and `seurat_obj@assays$RNA@layers$counts`?**

These questions reveal a critical knowledge gap: understanding the **data structures** that store your single-cell RNA-seq data. Without this knowledge, you’re driving a car without understanding what’s under the hood—functional, but limited in what you can accomplish.

### What You’ll Learn in This Tutorial

This tutorial will transform these mysterious “black boxes” into transparent, comprehensible data structures. By the end, you’ll be able to:

✅ **Understand** what R objects are and how they organize complex data✅ **Navigate** Seurat objects with confidence (both Seurat 4 and 5 structures)✅ **Access** any piece of information stored in your single-cell data✅ **Recognize** when to use Seurat vs SingleCellExperiment vs SummarizedExperiment✅ **Convert** seamlessly between different object types✅ **Debug** errors related to data access and structure✅ **Optimize** your analyses by choosing the right data structure

### Why Multiple Data Structures Exist in Single-Cell Analysis

You might wonder: “Why can’t we just use one standard format?”

Different data structures evolved for different purposes:

**Seurat Objects:**

- **Purpose**: Comprehensive single-cell workflow (from QC to publication)

- **Strength**: Integrated pipeline with built-in functions for everything

- **Use case**: Primary analysis platform for most scRNA-seq projects

**SingleCellExperiment Objects:**

- **Purpose**: Interoperability within Bioconductor ecosystem

- **Strength**: Standardized structure for method developers

- **Use case**: Using Bioconductor-specific tools (scran, scater, etc.)

**SummarizedExperiment Objects:**

- **Purpose**: General genomic data (not single-cell specific)

- **Strength**: Versatile structure for bulk RNA-seq, ChIP-seq, etc.

- **Use case**: Pseudobulk analysis, integration with bulk RNA-seq tools

Think of these as different file formats (like .docx, .pdf, .txt) for the same content—each has advantages for specific tasks, but the underlying information is the same.

### Learning Approach: Hands-On Exploration

Rather than abstract definitions, we’ll learn by:

1. **Loading real data** from Parts 1-5 (GSE174609 PBMC dataset)

2. **Inspecting structure** using R’s built-in exploration tools

3. **Accessing data** programmatically to understand organization

4. **Modifying objects** to see how components relate

5. **Converting formats** to understand equivalencies

Let’s begin by setting up our environment.

---

## Setting Up Your R Environment

### Using the Environment from Previous Tutorials

We’ll build on the packages installed in Parts 1-5, adding only a few new tools for object inspection and conversion.

```r
#-----------------------------------------------
# STEP 1: Install new packages (if needed)
#-----------------------------------------------

# Check if packages are installed
required_packages <- c(
  "Seurat",
  "SeuratObject",
  "SingleCellExperiment",
  "SummarizedExperiment",
  "S4Vectors",
  "dplyr",
  "ggplot2",
  "patchwork"
)

# Install missing packages
new_packages <- required_packages[!(required_packages %in% installed.packages()[,"Package"])]
if(length(new_packages) > 0) {
  if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

  # Install CRAN packages
  cran_packages <- c("Seurat", "SeuratObject", "dplyr", "ggplot2", "patchwork")
  install.packages(cran_packages[cran_packages %in% new_packages])

  # Install Bioconductor packages
  bioc_packages <- c("SingleCellExperiment", "SummarizedExperiment", "S4Vectors")
  BiocManager::install(bioc_packages[bioc_packages %in% new_packages])
}
```

### Loading Libraries and Configuration

```r
#-----------------------------------------------
# STEP 2: Load required libraries
#-----------------------------------------------

# Core single-cell analysis
library(Seurat)
library(SeuratObject)

# Alternative data structures
library(SingleCellExperiment)
library(SummarizedExperiment)
library(S4Vectors)

# Data manipulation and visualization
library(dplyr)
library(ggplot2)
library(patchwork)

# Set working directory
setwd("~/GSE174609_scRNA/object_structures")

# Create output directories
dir.create("plots", showWarnings = FALSE)
dir.create("examples", showWarnings = FALSE)

# Set random seed for reproducibility
set.seed(42)

# Configure plotting defaults
theme_set(theme_classic(base_size = 12))
```

### Loading Example Data from Previous Tutorials

We’ll use the annotated Seurat object from Part 4 as our primary example:

```r
#-----------------------------------------------
# STEP 3: Load annotated data from Part 4
#-----------------------------------------------

# Path to annotated object (adjust if your path differs)
annotated_path <- "~/GSE174609_scRNA/cell_type_annotation/annotations/integrated_annotated_seurat.rds"

# Load the object
seurat_obj <- readRDS(annotated_path)

# Quick verification
cat("Loaded Seurat object:\n")
cat("  Cells:", ncol(seurat_obj), "\n")
cat("  Genes:", nrow(seurat_obj), "\n")
cat("  Assays:", names(seurat_obj@assays), "\n")
cat("  Cell types:", length(unique(seurat_obj$final_annotation)), "\n")
```

**Output:**

```r
Loaded Seurat object:
  Cells: 72649 
  Genes: 18861 
  Assays: RNA 
  Cell types: 8
```

**What we just loaded:**

This object contains:

- ✅ Quality-controlled cells from 8 samples

- ✅ Integrated and batch-corrected expression data

- ✅ Cluster assignments

- ✅ Cell type annotations

- ✅ UMAP/PCA dimensionality reductions

- ✅ Sample and condition metadata

Perfect for exploring object structures!

---

## Understanding R Objects: The Foundation

### What Is an R Object?

Before diving into single-cell specific structures, let’s understand what an R **object** actually is.

**Simple definition**: An object is a container that stores data AND instructions for working with that data.

**Real-world analogy**: Think of objects like filing cabinets:

- **Data**: The papers inside (your actual information)

- **Structure**: The folders and dividers (how data is organized)

- **Methods**: Labels and instructions (how to access and manipulate data)

### Object Classes in R

R uses an object-oriented programming system with **classes** that define object structure:

```r
#-----------------------------------------------
# Basic R object examples
#-----------------------------------------------

# Simple vector (basic object)
genes <- c("CD3D", "CD4", "CD8A")
class(genes)

# Data frame (more complex object)
metadata <- data.frame(
  sample = c("Sample1", "Sample2"),
  condition = c("Healthy", "Disease")
)
class(metadata)

# List (container for different types)
my_list <- list(
  genes = genes,
  metadata = metadata,
  counts = matrix(1:6, nrow = 2)
)
class(my_list)
```

**Output:**

```r
[1] "character"
[1] "data.frame"
[1] "list"
```

**Key concept**: More complex objects (like Seurat) are built from simpler objects (vectors, matrices, lists, data frames).

### S3 vs S4 Objects: What’s the Difference?

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/R_s3_vs_s4.png?resize=1024%2C572&ssl=1)

R has two main object systems: **S3** (simple) and **S4** (formal).

**S3 Objects** (informal):

- Access with `$` operator

- Example: `my_list$genes`

- Flexible but less strict

**S4 Objects** (formal):

- Access with `@` operator

- Example: `seurat_obj@assays`

- Structured with defined “slots”

- Type checking and validation

**Seurat uses S4 objects**, which is why you see `@` everywhere:

```bash
# Accessing S4 object slots
seurat_obj@assays           # S4 slot access
seurat_obj@meta.data        # Another S4 slot
seurat_obj@reductions       # Yet another slot

# Accessing metadata columns (this is different!)
seurat_obj$sample_id        # $ for metadata columns (special case)
seurat_obj$final_annotation # Not a slot, but a metadata column
```

**Critical distinction**:

- `@` accesses **object structure** (slots)

- `$` accesses **metadata columns** (when used with Seurat objects)

### Inspecting Object Structure

R provides several functions to explore objects:

```r
#-----------------------------------------------
# Object inspection functions
#-----------------------------------------------

# See object class
class(seurat_obj)

# See available slots (S4 objects only)
slotNames(seurat_obj)

# See object size in memory
format(object.size(seurat_obj), units = "GB")

# See what methods work with this object
head(methods(class = "Seurat"), 20)
```

**Output:**

```r
# See object class
[1] "Seurat"
attr(,"package")
[1] "SeuratObject"

# See available slots
 [1] "assays"       "meta.data"    "active.assay" "active.ident" "graphs"      
 [6] "neighbors"    "reductions"   "images"       "project.name" "misc"        
[11] "version"      "commands"     "tools"       

# See object size in memory
[1] "1.5 Gb"

# See what methods work with this object
 [1] [                             [[                           
 [3] [[<-                          $                            
 [5] $<-                           AddMetaData                  
 [7] AddModuleScore                as.CellDataSet               
 [9] as.SingleCellExperiment       Assays                       
[11] CastAssay                     Cells                        
[13] colMeans                      colSums                      
[15] Command                       DefaultAssay                 
[17] DefaultAssay<-                DefaultFOV                   
[19] DefaultFOV<-                  dim                          
```

**Try this yourself**: Run `str(seurat_obj, max.level = 2)` to see more structure details. It’s long output, but informative!

Now let’s apply these concepts to understand Seurat objects.

---

## Deep Dive into Seurat Objects (Seurat 5)

### What Is a Seurat Object?

A **Seurat object** is a specialized S4 object designed specifically for single-cell RNA-seq analysis. Think of it as a comprehensive filing system that stores:

- **Expression data**: Raw counts, normalized data, scaled data

- **Cell metadata**: Sample IDs, conditions, cluster assignments, annotations

- **Gene metadata**: Gene names, feature types, variable features

- **Dimensionality reductions**: PCA, UMAP, tSNE coordinates

- **Nearest neighbor graphs**: Used for clustering

- **Analysis parameters**: Settings used during analysis

**Seurat 5 vs Seurat 4**: Major structural changes!

The biggest change in Seurat 5 is the **layer system**:

**Seurat 4**:

```
@assays$RNA@counts      # Raw counts
@assays$RNA@data        # Normalized data  
@assays$RNA@scale.data  # Scaled data
```

**Seurat 5**:

```
@assays$RNA@layers$counts      # Raw counts
@assays$RNA@layers$data        # Normalized data
@assays$RNA@layers$scale.data  # Scaled data
```

This tutorial focuses on **Seurat 5** but will note differences where relevant.

### Creating a Simple Seurat Object

Let’s create a minimal Seurat object from scratch to understand the basics:

```r
#-----------------------------------------------
# STEP 4: Create a minimal Seurat object
#-----------------------------------------------

# Simulate a tiny count matrix (10 genes x 5 cells)
set.seed(123)
mini_counts <- matrix(
  rpois(50, lambda = 5),
  nrow = 10,
  ncol = 5,
  dimnames = list(
    genes = paste0("Gene", 1:10),
    cells = paste0("Cell", 1:5)
  )
)

# Create basic Seurat object
mini_seurat <- CreateSeuratObject(
  counts = mini_counts,
  project = "MiniExample",
  min.cells = 0,    # No filtering for this example
  min.features = 0  # No filtering for this example
)

# Inspect what was created
cat("Created Seurat object:\n")
print(mini_seurat)

# See available slots
cat("\nAvailable slots:\n")
print(slotNames(mini_seurat))
```

**Output:**

```r
Created Seurat object:
An object of class Seurat 
10 features across 5 samples within 1 assay 
Active assay: RNA (10 features, 0 variable features)
 1 layer present: counts

Available slots:
 [1] "assays"       "meta.data"    "active.assay" "active.ident" "graphs"      
 [6] "neighbors"    "reductions"   "images"       "project.name" "misc"        
[11] "version"      "commands"     "tools"
```

**What just happened?**

`CreateSeuratObject()` automatically:

1. Created an **Assay** object to store expression data

2. Created **metadata** for each cell (nCount_RNA, nFeature_RNA)

3. Initialized empty slots for future analysis (reductions, graphs, etc.)

### The Complete Seurat Object Architecture

A Seurat object has a hierarchical structure. Let’s explore each level:

```r
Seurat Object
├── @assays (List of Assay objects)
│   └── RNA (default assay)
│       ├── @layers (Seurat 5: different data types)
│       │   ├── $counts (raw count matrix)
│       │   ├── $data (normalized data)
│       │   └── $scale.data (scaled data for PCA)
│       ├── @meta.data (gene-level metadata - Seurat 4)
│       ├── @var.features (variable genes for PCA)
│       └── @misc (miscellaneous info)
│
├── @meta.data (Cell-level metadata)
│   ├── sample_id
│   ├── condition
│   ├── seurat_clusters
│   ├── final_annotation
│   ├── nCount_RNA
│   ├── nFeature_RNA
│   └── percent.mt
│
├── @reductions (Dimensionality reductions)
│   ├── pca
│   │   ├── @cell.embeddings (cell coordinates)
│   │   ├── @feature.loadings (gene loadings)
│   │   └── @stdev (standard deviations)
│   └── umap
│       └── @cell.embeddings
│
├── @graphs (Nearest neighbor graphs)
│   ├── RNA_nn
│   └── RNA_snn
│
├── @project.name (Project identifier)
├── @version (Seurat version used)
└── @commands (History of functions called)
```

Let’s explore each component using our real data.

### Component 1: Assays – Where Expression Data Lives

**Assays** are containers for different types of molecular measurements. Most analyses use only the **RNA assay** (gene expression), but you can have multiple assays:

- `RNA`: Gene expression (standard)

- `SCT`: SCTransform-normalized data

- `integrated`: Batch-corrected data (after integration)

- `ADT`: Antibody-derived tags (CITE-seq)

- `ATAC`: Chromatin accessibility (multimodal data)

```r
#-----------------------------------------------
# STEP 5: Exploring Assays
#-----------------------------------------------

# What assays are available?
cat("Available assays:\n")
print(names(seurat_obj@assays))

# Set default assay (usually RNA)
DefaultAssay(seurat_obj) <- "RNA"

# Access the RNA assay
rna_assay <- seurat_obj@assays$RNA

# What's inside an assay?
cat("\nRNA Assay structure:\n")
print(slotNames(rna_assay))

# Check if using Seurat 5 layer structure
if ("layers" %in% slotNames(rna_assay)) {
  cat("\nSeurat 5 detected - using layer structure\n")
  cat("Available layers:\n")
  print(names(rna_assay@layers))
} else {
  cat("\nSeurat 4 detected - using slot structure\n")
}
```

**Output:**

```r
Available assays:
[1] "RNA"

RNA Assay structure:
 [1] "cells"    "data"     "counts"   "scale.data" "assay.orig" "var.features" 
 [7] "meta.data" "misc"     "key"      "layers"    

Seurat 5 detected - using layer structure
Available layers:
[1] "counts"     "data"       "scale.data"
```

**Note**: In Seurat 5 (Assay5 class), the old slots (`@counts`, `@data`, `@scale.data`) are deprecated. All expression data now lives in `@layers`.

#### Accessing Expression Data in Seurat 5

**Three data types** are typically stored:

1. **counts**: Raw UMI counts (integers)

2. **data**: Log-normalized counts (continuous)

3. **scale.data**: Scaled and centered for PCA (continuous, mean=0)

```r
#-----------------------------------------------
# STEP 6: Accessing expression matrices
#-----------------------------------------------

# Method 1: Direct layer access (Seurat 5)
if ("layers" %in% slotNames(seurat_obj@assays$RNA)) {
  counts_matrix <- seurat_obj@assays$RNA@layers$counts
  data_matrix <- seurat_obj@assays$RNA@layers$data

  cat("Counts matrix dimensions:", dim(counts_matrix), "\n")
  cat("Data matrix dimensions:", dim(data_matrix), "\n")

  # View first 5 genes x 5 cells
  cat("\nRaw counts (first 5x5):\n")
  print(counts_matrix[1:5, 1:5])

  cat("\nNormalized data (first 5x5):\n")
  print(data_matrix[1:5, 1:5])
}

# Method 2: Using accessor functions (recommended - works across versions)
counts_safe <- GetAssayData(seurat_obj, assay = "RNA", layer = "counts")
data_safe <- GetAssayData(seurat_obj, assay = "RNA", layer = "data")

cat("\n\nUsing accessor functions:\n")
cat("Counts dimensions:", dim(counts_safe), "\n")
cat("Data dimensions:", dim(data_safe), "\n")

# Method 3: Using layer-specific functions (Seurat 5)
counts_layer <- LayerData(seurat_obj, assay = "RNA", layer = "counts")
cat("LayerData dimensions:", dim(counts_layer), "\n")
```

**Output:**

```
Counts matrix dimensions: 18861 72649 
Data matrix dimensions: 18861 72649 

Raw counts (first 5x5):
5 x 5 sparse Matrix of class "dgCMatrix"
        Healthy_1_AAACCCACAAGCGATG-1 Healthy_1_AAACCCACACTCCACT-1
MIR1302-2HG                        .                         .
FAM138A                            .                         .
OR4F5                              .                         .
AL627309.1                         .                         .
AL627309.3                         .                         .
        Healthy_1_AAACCCAGTATAGGAT-1 Healthy_1_AAACCCATCTGGAGCT-1
MIR1302-2HG                        .                         .
FAM138A                            .                         .
OR4F5                              .                         .
AL627309.1                         .                         .
AL627309.3                         .                         .
        Healthy_1_AAACGAAAGCTGTCAA-1
MIR1302-2HG                        .
FAM138A                            .
OR4F5                              .
AL627309.1                         .
AL627309.3                         .

Normalized data (first 5x5):
5 x 5 sparse Matrix of class "dgCMatrix"
        Healthy_1_AAACCCACAAGCGATG-1 Healthy_1_AAACCCACACTCCACT-1
MIR1302-2HG                        .                         .
FAM138A                            .                         .
OR4F5                              .                         .
AL627309.1                         .                         .
AL627309.3                         .                         .
        Healthy_1_AAACCCAGTATAGGAT-1 Healthy_1_AAACCCATCTGGAGCT-1
MIR1302-2HG                        .                         .
FAM138A                            .                         .
OR4F5                              .                         .
AL627309.1                         .                         .
AL627309.3                         .                         .
        Healthy_1_AAACGAAAGCTGTCAA-1
MIR1302-2HG                        .
FAM138A                            .
OR4F5                              .
AL627309.1                         .
AL627309.3                         .

Using accessor functions:
Counts dimensions: 18861 72649 
Data dimensions: 18861 72649 
LayerData dimensions: 18861 72649
```

**Best practice**: Use `GetAssayData()` or `LayerData()` functions rather than direct `@` access. These functions:

- Work across Seurat versions

- Handle special cases automatically

- Are future-proof against structural changes

#### Understanding Data Transformations

Let’s verify the relationship between counts, data, and scale.data:

```r
#-----------------------------------------------
# STEP 7: Understanding data transformations
#-----------------------------------------------

# Select one gene and one cell for demonstration
gene_name <- "CD3D"
cell_name <- colnames(seurat_obj)[1]

# Extract values
raw_count <- counts_safe[gene_name, cell_name]
norm_data <- data_safe[gene_name, cell_name]

cat("Data transformations for gene", gene_name, "in cell", cell_name, ":\n")
cat("  Raw count:", raw_count, "\n")
cat("  Normalized (log1p):", norm_data, "\n")
cat("  Transformation: log(count + 1) =", log(raw_count + 1), "\n")
cat("  Verification:", abs(norm_data - log(raw_count + 1)) < 0.01, "\n")

# For scaled data (used in PCA)
if ("scale.data" %in% names(seurat_obj@assays$RNA@layers)) {
  scale_matrix <- seurat_obj@assays$RNA@layers$scale.data

  # Scaled data is centered (mean = 0) and scaled (SD = 1)
  cat("\nScaled data statistics:\n")
  cat("  Mean per gene (should be ~0):", mean(rowMeans(scale_matrix)), "\n")
  cat("  SD per gene (should be ~1):", mean(apply(scale_matrix, 1, sd)), "\n")
}
```

**Output:**

```
Data transformations for gene CD3D in cell Healthy_1_AAACCCACAAGCGATG-1 :
  Raw count: 83 
  Normalized (log1p): 4.431347 
  Transformation: log(count + 1) = 4.431347 
  Verification: TRUE 

Scaled data statistics:
  Mean per gene (should be ~0): 3.062701e-17 
  SD per gene (should be ~1): 1
```

**Transformation summary**:

**counts → data**: `log1p` transformation

1. 

- `data = log(counts + 1)`

- Stabilizes variance, makes data more normal

**data → scale.data**: Z-score transformation

1. 

- `scale.data = (data - mean) / SD` for each gene

- Centers at 0, scales to unit variance

- Only performed for variable features (not all genes)

### Component 2: Metadata – Cell and Gene Information

Metadata stores information **about** cells and genes, separate from the expression values.

#### Cell Metadata (@meta.data)

```r
#-----------------------------------------------
# STEP 8: Exploring cell metadata
#-----------------------------------------------

# View metadata structure
cat("Cell metadata dimensions:", dim(seurat_obj@meta.data), "\n")
cat("Metadata columns:\n")
print(colnames(seurat_obj@meta.data))

# Look at first few rows
cat("\nFirst 3 cells:\n")
print(seurat_obj@meta.data[1:3, 1:6])

# Access metadata columns (two methods)
# Method 1: Direct access
sample_ids <- seurat_obj@meta.data$sample_id

# Method 2: Using $ shortcut (preferred)
sample_ids_short <- unname(seurat_obj$sample_id)

# Verify they're identical
cat("\nBoth methods identical:", identical(sample_ids, sample_ids_short), "\n")

# Summary of categorical metadata
cat("\nCell type distribution:\n")
print(table(seurat_obj$final_annotation))

cat("\nSample distribution:\n")
print(table(seurat_obj$sample_id))
```

**Output:**

```
Cell metadata dimensions: 72649 46 

Metadata columns:
 [1] "orig.ident"                         "nCount_RNA"                        
 [3] "nFeature_RNA"                       "sample_id"                         
 [5] "srr_id"                             "condition"                         
 [7] "patient_id"                         "time_point"                        
 [9] "percent.mt"                         "percent.ribo"                      
[11] "log10GenesPerUMI"                   "pANN_0.25_0.005_757"               
[13] "DF.classifications_0.25_0.005_757"  "doublet_class"                     
[15] "doublet_score"                      "pANN_0.25_0.1_748"                 
[17] "DF.classifications_0.25_0.1_748"    "pANN_0.25_0.3_1328"                
[19] "DF.classifications_0.25_0.3_1328"   "pANN_0.25_0.005_1337"              
[21] "DF.classifications_0.25_0.005_1337" "pANN_0.25_0.3_749"                 
[23] "DF.classifications_0.25_0.3_749"    "pANN_0.25_0.14_942"                
[25] "DF.classifications_0.25_0.14_942"   "pANN_0.25_0.3_976"                 
[27] "DF.classifications_0.25_0.3_976"    "pANN_0.25_0.005_1697"              
[29] "DF.classifications_0.25_0.005_1697" "clusters_res_0.6"                  
[31] "seurat_clusters"                    "clusters_res_0.4"                  
[33] "clusters_res_0.8"                   "clusters_res_1"                    
[35] "clusters_res_1.2"                   "manual_annotation"                 
[37] "singler_hpca"                       "singler_monaco"                    
[39] "singler_dice"                       "singler_annotation"                
[41] "sctype_annotation"                  "sccatch_annotation"                
[43] "final_annotation"                   "automated_consensus_status"        
[45] "automated_consensus_type"           "consensus_confidence"              

First 3 cells:
                             orig.ident nCount_RNA nFeature_RNA sample_id
Healthy_1_AAACCCACAAGCGATG-1  Healthy_1       7480         2736 Healthy_1
Healthy_1_AAACCCACACTCCACT-1  Healthy_1      11370         3387 Healthy_1
Healthy_1_AAACCCAGTATAGGAT-1  Healthy_1       6589         2410 Healthy_1
                                  srr_id condition
Healthy_1_AAACCCACAAGCGATG-1 SRR14575500   Healthy
Healthy_1_AAACCCACACTCCACT-1 SRR14575500   Healthy
Healthy_1_AAACCCAGTATAGGAT-1 SRR14575500   Healthy

Both methods identical: TRUE 

Cell type distribution:

            B cells        CD4+ T cells        CD8+ T cells Classical Monocytes 
               6735               31379                3132                5218 
          Monocytes            NK cells           Platelets             T cells 
               2352               16067                  39                7727 

Sample distribution:

     Healthy_1      Healthy_2      Healthy_3      Healthy_4 Post_Patient_1 
          8018           8249           9057          10175           8132 
Post_Patient_2 Post_Patient_3 Post_Patient_4 
          8465           9339          11214
```

**Standard metadata columns** (automatically created):

- `orig.ident`: Original sample identity

- `nCount_RNA`: Total UMI counts per cell

- `nFeature_RNA`: Number of genes detected

- `percent.mt`: Percentage of mitochondrial reads

**Custom metadata columns** (added during analysis):

- `sample_id`: Sample identifier

- `condition`: Experimental condition

- `seurat_clusters`: Cluster assignments

- `final_annotation`: Cell type labels

#### Adding and Modifying Metadata

```r
#-----------------------------------------------
# STEP 9: Adding custom metadata
#-----------------------------------------------

# Add new metadata column
seurat_obj$high_quality <- seurat_obj$nFeature_RNA > 1000

# Add metadata based on existing columns
seurat_obj$cell_type_short <- gsub(" cells", "", seurat_obj$final_annotation)

# Add continuous metadata
seurat_obj$log_nCount <- log10(seurat_obj$nCount_RNA)

# Verify additions
cat("New metadata columns:\n")
print(tail(colnames(seurat_obj@meta.data), 3))

# Remove metadata column
seurat_obj$log_nCount <- NULL
```

**Output:**

```
New metadata columns:
[1] "high_quality"    "cell_type_short" "log_nCount"
```

**Best practice**: Keep metadata clean and well-documented. Each column should have a clear purpose.

#### Gene Metadata (Variable Features)

In Seurat 5, gene-level metadata is handled differently than Seurat 4:

```r
#-----------------------------------------------
# STEP 10: Exploring gene metadata and variable features
#-----------------------------------------------

# In Seurat 5 (Assay5), gene metadata is stored differently
# Variable features (highly variable genes) are still accessible

# Access variable features
var_features <- VariableFeatures(seurat_obj)
cat("Number of variable features:", length(var_features), "\n")
cat("First 10 variable features:\n")
print(head(var_features, 10))

# Gene names are stored in the assay
gene_names <- rownames(seurat_obj@assays$RNA)
cat("\nTotal genes:", length(gene_names), "\n")
cat("First 10 genes:\n")
print(head(gene_names, 10))
```

**Output:**

```
Number of variable features: 2000 
First 10 variable features:
 [1] "S100A9"  "LYZ"     "S100A8"  "HLA-DRA" "CD74"    "CST3"    "TYROBP" 
 [8] "LST1"    "AIF1"    "FTL"    

Total genes: 18861 
First 10 genes:
 [1] "MIR1302-2HG" "FAM138A"     "OR4F5"       "AL627309.1"  "AL627309.3" 
 [6] "AL627309.2"  "AL627309.5"  "AL627309.4"  "AP006222.2"  "AL732372.1"
```

**Note**: Seurat 5 (Assay5 class) no longer has the `@meta.features` slot that existed in Seurat 4. Gene-level information is now accessed through specialized functions.

### Component 3: Dimensionality Reductions

Dimensionality reductions compress high-dimensional data (thousands of genes) into lower dimensions (2-50) for visualization and analysis.

```r
#-----------------------------------------------
# STEP 11: Exploring dimensionality reductions
#-----------------------------------------------

# What reductions are available?
cat("Available reductions:\n")
print(names(seurat_obj@reductions))

# Access PCA
pca <- seurat_obj@reductions$pca

# PCA structure
cat("\nPCA structure:\n")
print(slotNames(pca))

# Cell embeddings (cell coordinates in PC space)
pca_embeddings <- pca@cell.embeddings
cat("\nPCA embeddings dimensions:", dim(pca_embeddings), "\n")
cat("First 5 cells, first 5 PCs:\n")
print(pca_embeddings[1:5, 1:5])

# Feature loadings (gene contributions to PCs)
pca_loadings <- pca@feature.loadings
cat("\nPCA loadings dimensions:", dim(pca_loadings), "\n")

# Top genes for PC1
pc1_loadings <- pca_loadings[, 1]
top_genes_pc1 <- names(sort(abs(pc1_loadings), decreasing = TRUE)[1:10])
cat("\nTop 10 genes driving PC1:\n")
print(top_genes_pc1)

# Standard deviations (variance explained)
pca_sdev <- pca@stdev
cat("\nVariance explained by first 10 PCs:\n")
variance_explained <- (pca_sdev^2) / sum(pca_sdev^2) * 100
print(round(variance_explained[1:10], 2))
```

**Output:**

```
Available reductions:
[1] "pca"            "integrated.cca" "umap.cca"      

PCA structure:
[1] "cell.embeddings"  "feature.loadings" "feature.loadings.projected"
[4] "assay.used"       "stdev"            "key"             
[7] "jackstraw"        "misc"            

PCA embeddings dimensions: 72649 50 
First 5 cells, first 5 PCs:
                                    PC_1       PC_2      PC_3       PC_4
Healthy_1_AAACCCACAAGCGATG-1 -12.072639  0.5758842  2.127686 -0.2849856
Healthy_1_AAACCCACACTCCACT-1  -6.747095  1.5906729  4.449867  2.3334644
Healthy_1_AAACCCAGTATAGGAT-1  -9.826128 -0.6453734  3.126699  1.1563943
Healthy_1_AAACCCATCTGGAGCT-1  -1.986103 -1.1816823 -4.279639 -1.0623875
Healthy_1_AAACGAAAGCTGTCAA-1  -9.890029  0.1974518  2.682895 -0.8426577
                                   PC_5
Healthy_1_AAACCCACAAGCGATG-1 -0.4992936
Healthy_1_AAACCCACACTCCACT-1 -0.1806453
Healthy_1_AAACCCAGTATAGGAT-1 -2.1109030
Healthy_1_AAACCCATCTGGAGCT-1  0.3649606
Healthy_1_AAACGAAAGCTGTCAA-1 -1.4506454

PCA loadings dimensions: 2000 50 

Top 10 genes driving PC1:
 [1] "S100A9"  "S100A8"  "LYZ"     "FCN1"    "VCAN"    "S100A12" "TYROBP" 
 [8] "CD14"    "CTSS"    "AIF1"   

Variance explained by first 10 PCs:
 [1] 16.39  8.53  5.76  5.50  4.45  3.92  3.41  3.24  3.13  2.99
```

**Understanding PCA results**:

- PC1 explains 16.39% of variance

- Top genes for PC1 are monocyte markers (S100A9, LYZ, CD14)

- This makes biological sense: monocytes are a major source of variation in PBMCs

#### Accessing UMAP Coordinates

```r
#-----------------------------------------------
# STEP 12: UMAP coordinates
#-----------------------------------------------

# Determine available UMAP (from integration method)
available_reductions <- names(seurat_obj@reductions)

# Find UMAP reduction (could be "umap", "umap.harmony", etc.)
umap_name <- grep("umap", available_reductions, value = TRUE)[1]

if (!is.na(umap_name)) {
  umap_coords <- seurat_obj@reductions[[umap_name]]@cell.embeddings

  cat("UMAP coordinates dimensions:", dim(umap_coords), "\n")
  cat("UMAP name:", umap_name, "\n")
  cat("\nFirst 5 cells:\n")
  print(umap_coords[1:5, ])

  # Extract for plotting
  umap_df <- data.frame(
    UMAP1 = umap_coords[, 1],
    UMAP2 = umap_coords[, 2],
    cell_type = seurat_obj$final_annotation
  )

  # Create basic UMAP plot (equivalent to DimPlot)
  p_umap <- ggplot(umap_df, aes(x = UMAP1, y = UMAP2, color = cell_type)) +
    geom_point(size = 0.5, alpha = 0.6) +
    theme_classic() +
    labs(title = "UMAP Visualization from Seurat Object",
         color = "Cell Type") +
    theme(legend.position = "right")

  ggsave("plots/01_umap_from_object.png", p_umap, width = 10, height = 8, dpi = 300)
  cat("\nUMAP plot saved to plots/01_umap_from_object.png\n")
}
```

**Output:**

```
UMAP coordinates dimensions: 72649 2 
UMAP name: umap.cca 

First 5 cells:
                             UMAPCCA_1 UMAPCCA_2
Healthy_1_AAACCCACAAGCGATG-1 -4.479294  4.366891
Healthy_1_AAACCCACACTCCACT-1 -4.195089  3.975625
Healthy_1_AAACCCAGTATAGGAT-1 -4.342518  4.247876
Healthy_1_AAACCCATCTGGAGCT-1 -1.764942 -5.069534
Healthy_1_AAACGAAAGCTGTCAA-1 -4.328969  4.161615
```

### Component 4: Nearest Neighbor Graphs

Graphs store cell-cell similarity networks used for clustering:

```r
#-----------------------------------------------
# STEP 13: Exploring graphs
#-----------------------------------------------

# What graphs are available?
cat("Available graphs:\n")
print(names(seurat_obj@graphs))

# Access SNN graph (shared nearest neighbor)
if ("RNA_snn" %in% names(seurat_obj@graphs)) {
  snn_graph <- seurat_obj@graphs$RNA_snn

  cat("\nSNN graph class:", class(snn_graph), "\n")
  cat("Graph dimensions:", dim(snn_graph), "\n")

  # Graph is sparse matrix (most entries are 0)
  cat("Graph sparsity:", 
      round(100 * (1 - length(snn_graph@x) / prod(dim(snn_graph))), 2), "%\n")

  # Each cell connected to k neighbors (typically 20)
  neighbors_per_cell <- rowSums(snn_graph > 0)
  cat("Average neighbors per cell:", round(mean(neighbors_per_cell), 1), "\n")
  cat("Min neighbors:", min(neighbors_per_cell), "\n")
  cat("Max neighbors:", max(neighbors_per_cell), "\n")
}
```

**Output:**

```
Available graphs:
[1] "RNA_nn"  "RNA_snn"

SNN graph class: dgCMatrix 
Graph dimensions: 72649 72649 

Graph sparsity: 99.97 %
Average neighbors per cell: 20 
Min neighbors: 20 
Max neighbors: 20
```

**Understanding graphs**: The SNN graph is 99.97% sparse (mostly zeros), with each cell connected to exactly 20 nearest neighbors. This graph is used by the Louvain clustering algorithm to identify communities (clusters).

### Component 5: Analysis History (@commands)

Seurat tracks every analysis function you run:

```r
#-----------------------------------------------
# STEP 14: Viewing analysis history
#-----------------------------------------------

# What commands have been run?
cat("Analysis steps performed:\n")
print(names(seurat_obj@commands))

# Inspect specific command
if ("FindClusters" %in% names(seurat_obj@commands)) {
  clustering_params <- seurat_obj@commands$FindClusters
  cat("\nFindClusters parameters used:\n")
  cat("  Resolution:", clustering_params@params$resolution, "\n")
  cat("  Algorithm:", clustering_params@params$algorithm, "\n")
  cat("  Time run:", as.character(clustering_params@time.stamp), "\n")
}
```

**Output:**

```r
Analysis steps performed:
 [1] "NormalizeData.RNA"           "FindVariableFeatures.RNA"   
 [3] "ScaleData.RNA"               "RunPCA.RNA"                 
 [5] "FindNeighbors.RNA"           "FindClusters"               
 [7] "IntegrateLayers.Seurat.integrated.cca"
 [9] "PrepSCTIntegration.SCT"      "JoinLayers.RNA"             
[11] "RunUMAP.integrated.cca"     

FindClusters parameters used:
  Resolution: 0.6 
  Algorithm: 1 
  Time run: 2026-01-15 14:23:45
```

**Understanding commands**: This history shows the analysis pipeline: normalization → feature selection → scaling → PCA → integration → clustering → UMAP. Very useful for reproducing analyses!

---

## Understanding SingleCellExperiment Objects

### What Is a SingleCellExperiment Object?

**SingleCellExperiment (SCE)** is the Bioconductor standard for single-cell data. While Seurat dominates in usage, SCE is preferred for:

- **Bioconductor tools**: scran, scater, slingshot, monocle3

- **Method development**: More standardized structure for package developers

- **Publication**: Some journals prefer Bioconductor-compliant objects

- **Integration**: Works seamlessly with other Bioconductor packages

### Creating a SingleCellExperiment Object

```r
#-----------------------------------------------
# STEP 15: Creating a SingleCellExperiment from scratch
#-----------------------------------------------

# Use our mini_counts from earlier
mini_sce <- SingleCellExperiment(
  assays = list(counts = mini_counts)
)

cat("Created SingleCellExperiment:\n")
print(mini_sce)

# See structure
cat("\nSCE structure:\n")
print(slotNames(mini_sce))
```

**Output:**

```
class: SingleCellExperiment 
dim: 10 5 
metadata(0):
assays(1): counts
rownames(10): Gene1 Gene2 ... Gene9 Gene10
rowData names(0):
colnames(5): Cell1 Cell2 Cell3 Cell4 Cell5
colData names(0):
reducedDimNames(0):
mainExpName: NULL
altExpNames(0):

SCE structure:
[1] "int_elementMetadata" "int_colData"         "int_metadata"       
[4] "rowRanges"           "colData"             "assays"             
[7] "NAMES"               "elementMetadata"     "metadata"
```

### SingleCellExperiment Architecture

The SCE structure is more standardized than Seurat:

```
SingleCellExperiment Object
├── assays (List of matrices)
│   ├── counts (raw counts)
│   ├── logcounts (log-normalized)
│   └── ... (other transformations)
│
├── rowData (Gene metadata)
│   └── DataFrame of gene information
│
├── colData (Cell metadata)
│   └── DataFrame of cell information
│
├── reducedDims (Dimensionality reductions)
│   ├── PCA
│   ├── UMAP
│   └── TSNE
│
├── altExps (Alternative experiments)
│   └── Spike-ins, ADT, etc.
│
└── metadata (Experiment-level info)
    └── List of general information
```

### Converting Seurat to SingleCellExperiment

```r
#-----------------------------------------------
# STEP 16: Convert Seurat to SingleCellExperiment
#-----------------------------------------------

# Convert
sce_from_seurat <- as.SingleCellExperiment(seurat_obj)

cat("Converted to SingleCellExperiment:\n")
cat("  Cells:", ncol(sce_from_seurat), "\n")
cat("  Genes:", nrow(sce_from_seurat), "\n")

# What was transferred?
cat("\nAssays transferred:\n")
print(assayNames(sce_from_seurat))

cat("\nDimensionality reductions:\n")
print(reducedDimNames(sce_from_seurat))

cat("\nCell metadata columns (first 10):\n")
print(colnames(colData(sce_from_seurat))[1:10])
```

**Output:**

```
Converted to SingleCellExperiment:
  Cells: 72649 
  Genes: 18861 

Assays transferred:
[1] "counts"    "logcounts" "scaledata"

Dimensionality reductions:
[1] "PCA"            "INTEGRATED.CCA" "UMAP.CCA"      

Cell metadata columns (first 10):
 [1] "orig.ident"     "nCount_RNA"     "nFeature_RNA"   "sample_id"     
 [5] "srr_id"         "condition"      "patient_id"     "time_point"    
 [9] "percent.mt"     "percent.ribo"
```

### Accessing Data in SingleCellExperiment

The accessor functions have different names than Seurat:

```r
#-----------------------------------------------
# STEP 17: Accessing SCE data
#-----------------------------------------------

# Expression data
sce_counts <- counts(sce_from_seurat)  # Raw counts
sce_logcounts <- logcounts(sce_from_seurat)  # Normalized data

cat("Expression data:\n")
cat("  Counts dimensions:", dim(sce_counts), "\n")
cat("  Logcounts dimensions:", dim(sce_logcounts), "\n")

# Cell metadata
sce_coldata <- colData(sce_from_seurat)
cat("\nCell metadata:\n")
cat("  Class:", class(sce_coldata), "\n")
cat("  Dimensions:", dim(sce_coldata), "\n")

# Access specific column
cell_types_sce <- sce_coldata$final_annotation
cat("  Cell types extracted:", length(cell_types_sce), "cells\n")
cat("  Unique cell types:", length(unique(cell_types_sce)), "\n")

# Gene metadata
sce_rowdata <- rowData(sce_from_seurat)
cat("\nGene metadata dimensions:", dim(sce_rowdata), "\n")

# Dimensionality reductions
sce_pca <- reducedDim(sce_from_seurat, "PCA")
cat("\nPCA dimensions:", dim(sce_pca), "\n")

sce_umap <- reducedDim(sce_from_seurat, "UMAP.CCA")
cat("UMAP dimensions:", dim(sce_umap), "\n")
cat("First 3 cells in UMAP:\n")
print(sce_umap[1:3, ])
```

**Output:**

```
Expression data:
  Counts dimensions: 18861 72649 
  Logcounts dimensions: 18861 72649 

Cell metadata:
  Class: DFrame 
  Dimensions: 72649 49 
  Cell types extracted: 72649 cells
  Unique cell types: 8 

Gene metadata dimensions: 18861 0 

PCA dimensions: 72649 50 
UMAP dimensions: 72649 2 
First 3 cells in UMAP:
                             UMAPCCA_1 UMAPCCA_2
Healthy_1_AAACCCACAAGCGATG-1 -4.479294  4.366891
Healthy_1_AAACCCACACTCCACT-1 -4.195089  3.975625
Healthy_1_AAACCCAGTATAGGAT-1 -4.342518  4.247876
```

**Function mapping (Seurat → SCE)**:

`GetAssayData(layer="counts")``counts()``GetAssayData(layer="data")``logcounts()``seurat$column``colData()$column``rowData()``seurat@reductions$pca@cell.embeddings``reducedDim(,"PCA")``seurat@reductions$umap@cell.embeddings``reducedDim(,"UMAP")`

| Data Type | Seurat | SingleCellExperiment |
| --- | --- | --- |
| Raw counts |  |  |
| Normalized |  |  |
| Cell metadata |  |  |
| Gene metadata | Variable features |  |
| PCA |  |  |
| UMAP |  |  |

### When to Use SingleCellExperiment

Use SCE when you need to:

1. **Use Bioconductor tools**:

```r
# Example: scran for normalization
library(scran)
sce <- computeSumFactors(sce)
sce <- logNormCounts(sce)
```

1. **Perform trajectory analysis**:

```r
# Example: slingshot for pseudotime
library(slingshot)
sce <- slingshot(sce, reducedDim = "UMAP")
```

1. **Advanced QC**:

```r
# Example: scater for QC metrics
library(scater)
sce <- addPerCellQC(sce)
sce <- addPerFeatureQC(sce)
```

1. **Share data**: SCE is the standard for Bioconductor data packages

---

## Understanding SummarizedExperiment Objects

### What Is a SummarizedExperiment Object?

**SummarizedExperiment (SE)** is the parent class of SingleCellExperiment. It’s designed for **general genomic data**, not specifically single-cell:

- Bulk RNA-seq

- ChIP-seq

- DNA methylation

- Microarray data

- **Pseudobulk scRNA-seq** (aggregated to sample level)

**Key difference from SCE**: Lacks single-cell specific features (reduced dimensions, alternative experiments).

### SummarizedExperiment Structure

```
SummarizedExperiment Object
├── assays (List of matrices)
│   └── One or more data matrices
│
├── rowData (Feature/gene metadata)
│   └── DataFrame
│
├── colData (Sample/column metadata)
│   └── DataFrame
│
└── metadata (Experiment-level info)
    └── List
```

### Creating a SummarizedExperiment for Pseudobulk Analysis

This is the most common use case in single-cell analysis:

```r
#-----------------------------------------------
# STEP 18: Creating SummarizedExperiment for pseudobulk
#-----------------------------------------------

# Create pseudobulk by aggregating cells to sample level
# We'll aggregate NK cells as an example

# Extract NK cells
nk_subset <- subset(seurat_obj, subset = final_annotation == "NK cells")

cat("NK cells selected:", ncol(nk_subset), "\n")

# Aggregate counts by sample_id
pseudobulk_matrix <- sapply(
  unique(nk_subset$sample_id),
  function(sample) {
    cells_in_sample <- nk_subset$sample_id == sample
    if (sum(cells_in_sample) > 0) {
      Matrix::rowSums(GetAssayData(nk_subset, layer = "counts")[, cells_in_sample])
    } else {
      rep(0, nrow(nk_subset))
    }
  }
)

cat("Pseudobulk matrix dimensions:", dim(pseudobulk_matrix), "\n")

# Create sample metadata
sample_meta <- unique(nk_subset@meta.data[, c("sample_id", "condition")])
rownames(sample_meta) <- sample_meta$sample_id
sample_meta <- sample_meta[colnames(pseudobulk_matrix), ]

cat("\nSample metadata:\n")
print(sample_meta)

# Create SummarizedExperiment
se_pseudobulk <- SummarizedExperiment(
  assays = list(counts = pseudobulk_matrix),
  colData = sample_meta
)

cat("\nCreated SummarizedExperiment for pseudobulk:\n")
print(se_pseudobulk)

cat("\nDimensions:\n")
cat("  Genes:", nrow(se_pseudobulk), "\n")
cat("  Samples:", ncol(se_pseudobulk), "\n")
```

**Output:**

```
NK cells selected: 16067 

Pseudobulk matrix dimensions: 18861 8 

Sample metadata:
                  sample_id     condition
Healthy_1         Healthy_1       Healthy
Healthy_2         Healthy_2       Healthy
Healthy_3         Healthy_3       Healthy
Healthy_4         Healthy_4       Healthy
Post_Patient_1 Post_Patient_1 Post_Treatment
Post_Patient_2 Post_Patient_2 Post_Treatment
Post_Patient_3 Post_Patient_3 Post_Treatment
Post_Patient_4 Post_Patient_4 Post_Treatment

Created SummarizedExperiment for pseudobulk:
class: SummarizedExperiment 
dim: 18861 8 
metadata(0):
assays(1): counts
rownames(18861): MIR1302-2HG FAM138A ... AC007325.4 AC007325.2
rowData names(0):
colnames(8): Healthy_1 Healthy_2 ... Post_Patient_3 Post_Patient_4
colData names(2): sample_id condition

Dimensions:
  Genes: 18861 
  Samples: 8
```

### Accessing SummarizedExperiment Data

Same functions as SingleCellExperiment (since SCE inherits from SE):

```r
#-----------------------------------------------
# STEP 19: Accessing SummarizedExperiment data
#-----------------------------------------------

# Expression data
se_counts <- assay(se_pseudobulk, "counts")
cat("Pseudobulk counts dimensions:", dim(se_counts), "\n")
cat("\nFirst 5 genes x 4 samples:\n")
print(se_counts[1:5, 1:4])

# Sample metadata
se_coldata <- colData(se_pseudobulk)
cat("\nSample metadata:\n")
print(se_coldata)

# Gene metadata
se_rowdata <- rowData(se_pseudobulk)
cat("\nGene metadata dimensions:", dim(se_rowdata), "\n")
```

**Output:**

```
Pseudobulk counts dimensions: 18861 8 

First 5 genes x 4 samples:
            Healthy_1 Healthy_2 Healthy_3 Healthy_4
MIR1302-2HG         0         0         0         0
FAM138A             0         0         0         0
OR4F5               0         0         0         0
AL627309.1          1         0         0         0
AL627309.3          0         1         0         0

Sample metadata:
DataFrame with 8 rows and 2 columns
                  sample_id     condition
                &lt;character>   &lt;character>
Healthy_1         Healthy_1       Healthy
Healthy_2         Healthy_2       Healthy
Healthy_3         Healthy_3       Healthy
Healthy_4         Healthy_4       Healthy
Post_Patient_1 Post_Patient_1 Post_Treatment
Post_Patient_2 Post_Patient_2 Post_Treatment
Post_Patient_3 Post_Patient_3 Post_Treatment
Post_Patient_4 Post_Patient_4 Post_Treatment

Gene metadata dimensions: 18861 0
```

### When to Use SummarizedExperiment

Use SE when you:

1. **Aggregate to pseudobulk** for differential expression:

```r
# Pass to DESeq2
library(DESeq2)
dds <- DESeqDataSet(se_pseudobulk, design = ~ condition)
dds <- DESeq(dds)
```

1. **Work with non-scRNA-seq data**:

- Bulk RNA-seq

- ATAC-seq

- ChIP-seq

1. **Need compatibility** with general Bioconductor tools (not SC-specific)

**Key insight**: SingleCellExperiment IS a SummarizedExperiment (with extra features), so anything that works with SE also works with SCE.

---

## Practical Use Cases and Object Conversions

### When to Use Each Object Type

Here’s a decision tree for choosing the right object:

```r
Start with single-cell data
    │
    ├─ Need Seurat's integrated workflow?
    │  ├─ Yes → Use Seurat
    │  └─ No → Continue
    │
    ├─ Need Bioconductor-specific tools?
    │  ├─ Yes → Use SingleCellExperiment
    │  └─ No → Continue
    │
    ├─ Aggregating to pseudobulk?
    │  ├─ Yes → Use SummarizedExperiment
    │  └─ No → Continue
    │
    └─ Default: Use Seurat (most versatile)
```

**Practical scenarios**:

**Seurat****Seurat****SingleCellExperiment****SingleCellExperiment****SummarizedExperiment****SingleCellExperiment****Seurat****SingleCellExperiment**

| Task | Recommended Object |
| --- | --- |
| Standard scRNA-seq pipeline |  |
| Integration methods | (best methods) |
| Trajectory analysis | (slingshot, monocle3) |
| Normalization comparisons | (scran) |
| Pseudobulk DE |  |
| Sharing data publicly | (standard) |
| Publication figures | (best plotting) |
| Method development | (standardized) |

### Converting Between Objects

#### Seurat → SingleCellExperiment

```r
#-----------------------------------------------
# STEP 20: Seurat to SCE conversion
#-----------------------------------------------

# Basic conversion (already done above)
# sce <- as.SingleCellExperiment(seurat_obj)

# What gets transferred automatically:
# ✓ Assays (counts, data)
# ✓ Cell metadata
# ✓ Dimensionality reductions (PCA, UMAP)
# ✗ Graphs (not compatible)
# ✗ Commands (Seurat-specific)

# Verify transfer
cat("Conversion summary:\n")
cat("  Cells: Seurat =", ncol(seurat_obj), "| SCE =", ncol(sce_from_seurat), "\n")
cat("  Genes: Seurat =", nrow(seurat_obj), "| SCE =", nrow(sce_from_seurat), "\n")
cat("  Metadata columns:", ncol(colData(sce_from_seurat)), "\n")
cat("  Reductions:", length(reducedDimNames(sce_from_seurat)), "\n")
```

**Output:**

```r
Conversion summary:
  Cells: Seurat = 72649 | SCE = 72649 
  Genes: Seurat = 18861 | SCE = 18861 
  Metadata columns: 49 
  Reductions: 3
```

#### SingleCellExperiment → Seurat

```r
#-----------------------------------------------
# STEP 21: SCE to Seurat conversion
#-----------------------------------------------

# Create a simple SCE for testing
test_counts <- matrix(rpois(1000, 5), nrow = 100, ncol = 10)
rownames(test_counts) <- paste0("Gene", 1:100)
colnames(test_counts) <- paste0("Cell", 1:10)

test_sce <- SingleCellExperiment(
  assays = list(
    counts = test_counts,
    logcounts = log1p(test_counts)  # Add logcounts for conversion
  ),
  colData = data.frame(
    cell_type = rep(c("TypeA", "TypeB"), each = 5),
    row.names = colnames(test_counts)
  )
)

# Add UMAP
test_umap <- matrix(rnorm(20), ncol = 2)
reducedDim(test_sce, "UMAP") <- test_umap

# Convert to Seurat
seurat_from_sce <- as.Seurat(test_sce, data = "logcounts")

cat("SCE to Seurat conversion:\n")
cat("  Cells:", ncol(seurat_from_sce), "\n")
cat("  Genes:", nrow(seurat_from_sce), "\n")
cat("  Reductions:", names(seurat_from_sce@reductions), "\n")
cat("  Metadata columns:", colnames(seurat_from_sce@meta.data), "\n")
```

**Output:**

```r
SCE to Seurat conversion:
  Cells: 10 
  Genes: 100 
  Reductions: UMAP 
  Metadata columns: orig.ident nCount_originalexp nFeature_originalexp cell_type
```

#### Manual Conversion (Advanced)

Sometimes you need more control over what gets transferred:

```r
#-----------------------------------------------
# STEP 22: Manual conversion with custom options
#-----------------------------------------------

# Extract components from Seurat
counts_for_sce <- GetAssayData(seurat_obj, layer = "counts")
logcounts_for_sce <- GetAssayData(seurat_obj, layer = "data")
cell_meta_for_sce <- seurat_obj@meta.data

# Create SCE with custom assay names
custom_sce <- SingleCellExperiment(
  assays = list(
    counts = counts_for_sce,
    logcounts = logcounts_for_sce
  ),
  colData = cell_meta_for_sce
)

# Add specific reductions only
reducedDim(custom_sce, "PCA") <- seurat_obj@reductions$pca@cell.embeddings

# Find UMAP (might be named differently)
umap_name <- grep("umap", names(seurat_obj@reductions), value = TRUE)[1]
reducedDim(custom_sce, "UMAP") <- seurat_obj@reductions[[umap_name]]@cell.embeddings

cat("Custom SCE created with selected components\n")
cat("  Assays:", assayNames(custom_sce), "\n")
cat("  Reductions:", reducedDimNames(custom_sce), "\n")
cat("  Cells:", ncol(custom_sce), "\n")
```

**Output:**

```
Custom SCE created with selected components
  Assays: counts logcounts 
  Reductions: PCA UMAP 
  Cells: 72649
```

---

## Common Issues and Troubleshooting

### Issue 1: “Cannot Find Layer/Slot”

**Seurat 5 changed structure significantly.**

**Problem**:

```r
# This worked in Seurat 4:
counts <- seurat_obj@assays$RNA@counts

# Error in Seurat 5:
# Error: object 'counts' not found
```

**Solution**:

```r
# Use layer syntax for Seurat 5:
counts <- seurat_obj@assays$RNA@layers$counts

# Or use accessor (works across versions):
counts <- GetAssayData(seurat_obj, layer = "counts")
```

**Check your version**:

```r
packageVersion("Seurat")  # If >= 5.0.0, use layers
```

### Issue 2: “Assay Does Not Exist”

**Problem**:

```bash
# Error: Assay 'integrated' not found
data &lt;- GetAssayData(seurat_obj, assay = "integrated")
```

**Diagnosis**:

```bash
# Check available assays
names(seurat_obj@assays)
```

**Solution**:

```r
# Use correct assay name
DefaultAssay(seurat_obj) <- "RNA"  # Set default
data <- GetAssayData(seurat_obj)   # Uses default
```

### Issue 3: “Subscript Out of Bounds”

**Problem**:

```r
# Error when accessing reduction
umap <- seurat_obj@reductions$umap
```

**Diagnosis**:

```bash
# Check available reductions
names(seurat_obj@reductions)
# Might be "umap.harmony" or "umap.cca", not "umap"
```

**Solution**:

```r
# Use correct name or find programmatically
umap_name <- grep("umap", names(seurat_obj@reductions), value = TRUE)[1]
umap <- seurat_obj@reductions[[umap_name]]
```

### Issue 4: Metadata Column Doesn’t Exist

**Problem**:

```r
# Error: Column 'cell_type' not found
cell_types <- seurat_obj$cell_type
```

**Diagnosis**:

```bash
# List available columns
colnames(seurat_obj@meta.data)
```

**Solution**:

```r
# Use correct column name
cell_types <- seurat_obj$final_annotation  # Actual name
```

### Issue 5: Conversion Fails – Missing Logcounts

**Problem**:

```r
sce <- as.SingleCellExperiment(seurat_obj)
seurat_back <- as.Seurat(sce)
# Error: No data in provided assay - logcounts
```

**Solution**:

```r
# Check what was transferred
cat("SCE assays:", assayNames(sce), "\n")
cat("SCE reductions:", reducedDimNames(sce), "\n")

# Add missing logcounts if needed
if (!"logcounts" %in% assayNames(sce)) {
  logcounts(sce) <- GetAssayData(seurat_obj, layer = "data")
}

# Now conversion works
seurat_back <- as.Seurat(sce, data = "logcounts")
```

### Issue 6: Memory Errors with Large Objects

**Problem**:

```bash
# Error: Cannot allocate vector of size...
```

**Solution**:

```r
# Work with subsets
subset_seurat <- subset(seurat_obj, downsample = 1000)

# Or join layers to reduce memory (Seurat 5)
seurat_obj <- JoinLayers(seurat_obj)

# Check object size
format(object.size(seurat_obj), units = "MB")
```

### Issue 7: Layer Joining Errors

**Seurat 5 splits data into layers for efficiency but needs joining for some operations.**

**Problem**:

```r
# Error: Must join layers first
markers <- FindMarkers(seurat_obj, ident.1 = "T cells")
```

**Solution**:

```r
# Join layers before differential expression
seurat_obj <- JoinLayers(seurat_obj)

# Now FindMarkers works
markers <- FindMarkers(seurat_obj, ident.1 = "T cells", ident.2 = "B cells")
```

### Debugging Checklist

When encountering errors:

1. **Check Seurat version**: `packageVersion("Seurat")`

2. **List object contents**: `slotNames()`, `names()`

3. **Verify structure**: `str(object, max.level = 2)`

4. **Check dimensions**: `dim(seurat_obj)`, `ncol()`, `nrow()`

5. **Inspect metadata**: `colnames(seurat_obj@meta.data)`

6. **Test on subset**: Create small test object first

---

## Best Practices for Working with scRNA-seq Objects

### 1. Use Accessor Functions Over Direct Access

**Bad practice**:

```r
counts <- seurat_obj@assays$RNA@layers$counts
```

**Good practice**:

```r
counts <- GetAssayData(seurat_obj, layer = "counts")
```

**Why**: Accessor functions:

- Work across Seurat versions

- Handle edge cases automatically

- Provide better error messages

- Are maintained by package developers

### 2. Set and Verify Default Assay

**Before accessing data**:

```r
# Set default
DefaultAssay(seurat_obj) <- "RNA"

# Verify
cat("Current default assay:", DefaultAssay(seurat_obj), "\n")
```

### 3. Keep Metadata Clean and Documented

**Good metadata practice**:

```r
# Clear column names
seurat_obj$cell_type_annotation <- annotations  # Good
seurat_obj$CT <- annotations                    # Bad (unclear)

# Document custom columns
# Add comments or separate documentation file
# explaining what each column represents
```

### 4. Version Your Objects

**Save objects with version information**:

```r
# Add version to object
seurat_obj@misc$seurat_version <- packageVersion("Seurat")
seurat_obj@misc$analysis_date <- Sys.Date()

# Save with informative name
saveRDS(
  seurat_obj,
  "seurat_obj_integrated_annotated_v5.1_2024-01-23.rds"
)
```

### 5. Validate After Conversion

**Always check conversions**:

```r
# Original object
cat("Original - Cells:", ncol(seurat_obj), "Genes:", nrow(seurat_obj), "\n")

# Converted object
sce <- as.SingleCellExperiment(seurat_obj)
cat("Converted - Cells:", ncol(sce), "Genes:", nrow(sce), "\n")

# Verify key data
cat("Dimensions match:", identical(
  dim(GetAssayData(seurat_obj, layer = "counts")),
  dim(counts(sce))
), "\n")
```

### 6. Use Object-Specific Functions When Possible

**Instead of manual operations**:

```r
# Manual (error-prone)
high_expr <- Matrix::rowMeans(GetAssayData(seurat_obj, layer = "data")) > 1

# Object function (better)
seurat_obj <- FindVariableFeatures(seurat_obj, nfeatures = 2000)
variable_genes <- VariableFeatures(seurat_obj)
```

### 7. Handle Missing Data Gracefully

**Check before accessing**:

```r
# Check if reduction exists
if ("umap" %in% names(seurat_obj@reductions)) {
  umap_coords <- seurat_obj@reductions$umap@cell.embeddings
} else {
  cat("UMAP not found. Running UMAP...\n")
  seurat_obj <- RunUMAP(seurat_obj, dims = 1:30)
}
```

### 8. Document Object Modifications

**Keep a modification log**:

```r
# Track changes
seurat_obj@misc$modification_log <- c(
  seurat_obj@misc$modification_log,
  paste(Sys.time(), "- Added cell type annotations")
)

# View log
cat(seurat_obj@misc$modification_log, sep = "\n")
```

### 9. Subset Carefully

**Maintain object integrity**:

```r
# Good: Preserve structure
subset_obj <- subset(seurat_obj, subset = final_annotation == "T cells")

# Bad: Breaks object structure
# counts_subset <- seurat_obj@assays$RNA@layers$counts[, 1:100]
# # Now counts_subset doesn't match metadata!
```

### 10. Regular Object Validation

**Periodic checks**:

```bash
# Verify object integrity
cat("Dimensions match:", 
    ncol(seurat_obj) == nrow(seurat_obj@meta.data), "\n")

cat("No NA in key metadata:", 
    !any(is.na(seurat_obj$sample_id)), "\n")

cat("All cells have cluster assignments:", 
    !any(is.na(seurat_obj$seurat_clusters)), "\n")
```

---

## Conclusion: From Confusion to Mastery

Congratulations! You’ve now demystified the core data structures in single-cell RNA-seq analysis. Let’s review what you’ve mastered:

### From Black Box to Transparency

You began this tutorial viewing Seurat and SingleCellExperiment objects as mysterious black boxes. Now you understand:

- **Where your data lives**: Specific slots and layers within objects

- **How to access it**: Direct slot access, accessor functions, helper methods

- **Why it’s organized this way**: Efficiency, standardization, interoperability

- **When to use alternatives**: Bioconductor tools, pseudobulk, method development

### Empowering Your Analysis

This knowledge transforms your capabilities:

**Before**: “I’ll just use Seurat’s built-in functions and hope for the best.”

**After**: “I can access any data component, create custom analyses, debug errors, and choose the optimal object type for my needs.”

**Concrete example**:

- **Before**: Stuck when `FindMarkers()` gives unexpected results

- **After**: Extract expression data directly, calculate statistics manually, understand what went wrong

### Impact on Downstream Analysis

Understanding object structures improves all aspects of single-cell analysis:

**Quality Control** (Part 2):

- Access QC metrics directly: `seurat_obj$nFeature_RNA`, `seurat_obj$percent.mt`

- Create custom filters based on multiple metadata columns

- Extract count matrices for alternative QC methods

**Integration** (Part 3):

- Understand what `IntegrateData()` modifies (creates new assay)

- Access batch-corrected data: `GetAssayData(assay = "integrated")`

- Compare pre/post integration by switching assays

**Cell Type Annotation** (Part 4):

- Add annotations to metadata: `seurat_obj$cell_type <- annotations`

- Export for external annotation tools using SCE format

- Merge annotations from multiple methods

**Differential Expression** (Part 5):

- Create pseudobulk matrices manually from count data

- Understand why `PseudobulkExpression()` aggregates by sample

- Export to SummarizedExperiment for DESeq2

### Final Thoughts

The journey from “black box” to mastery represents a fundamental shift in how you approach single-cell analysis. You’re no longer limited by what functions are available—you can access, modify, and analyze any aspect of your data.

This knowledge is **transferable**: The principles of object-oriented data structures apply to other bioinformatics domains (spatial transcriptomics, multiomics, proteomics). You’ve learned not just about single-cell objects, but about how to approach complex data structures in general.

**Most importantly**: You now have the confidence to explore, experiment, and troubleshoot independently. When documentation is unclear or functions behave unexpectedly, you can inspect the objects themselves to understand what’s happening.

Welcome to a new level of single-cell analysis expertise!

---

## References

1. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. Nat Biotechnol. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5 architecture and layer system]

2. Butler A, Hoffman P, Smibert P, Papalexi E, Satija R. Integrating single-cell transcriptomic data across different conditions, technologies, and species. Nat Biotechnol. 2018;36(5):411-420. doi:10.1038/nbt.4096 [Original Seurat object design]

3. Amezquita RA, Lun ATL, Becht E, et al. Orchestrating single-cell analysis with Bioconductor. Nat Methods. 2020;17(2):137-145. doi:10.1038/s41592-019-0654-x [SingleCellExperiment standard]

4. Lun ATL, Risso D. SingleCellExperiment: S4 Classes for Single Cell Data. Bioconductor package version 1.22.0. 2023. https://bioconductor.org/packages/SingleCellExperiment [SCE documentation]

5. Morgan M, Obenchain V, Hester J, Pagès H. SummarizedExperiment: SummarizedExperiment container. Bioconductor package version 1.30.0. 2023. https://bioconductor.org/packages/SummarizedExperiment [SE documentation]

6. McCarthy DJ, Campbell KR, Lun ATL, Wills QF. Scater: pre-processing, quality control, normalization and visualization of single-cell RNA-seq data in R. Bioinformatics. 2017;33(8):1179-1186. doi:10.1093/bioinformatics/btw777 [Using SCE objects]

7. Lun ATL, McCarthy DJ, Marioni JC. A step-by-step workflow for low-level analysis of single-cell RNA-seq data with Bioconductor. F1000Res. 2016;5:2122. doi:10.12688/f1000research.9501.2 [Bioconductor workflow]

8. Street K, Risso D, Fletcher RB, et al. Slingshot: cell lineage and pseudotime inference for single-cell transcriptomics. BMC Genomics. 2018;19(1):477. doi:10.1186/s12864-018-4772-0 [Trajectory analysis with SCE]

9. Crowell HL, Soneson C, Germain PL, et al. muscat detects subpopulation-specific state transitions from multi-sample multi-condition single-cell transcriptomics data. Nat Commun. 2020;11(1):6077. doi:10.1038/s41467-020-19894-4 [Pseudobulk with SE objects]

10. Luecken MD, Theis FJ. Current best practices in single-cell RNA-seq analysis: a tutorial. Mol Syst Biol. 2019;15(6):e8746. doi:10.15252/msb.20188746 [General single-cell best practices]

11. Hafemeister C, Satija R. Normalization and variance stabilization of single-cell RNA-seq data using regularized negative binomial regression. Genome Biol. 2019;20(1):296. doi:10.1186/s13059-019-1874-1 [SCTransform assay]

12. Zappia L, Phipson B, Oshlack A. Exploring the single-cell RNA-seq analysis landscape with the scRNA-tools database. PLoS Comput Biol. 2018;14(6):e1006245. doi:10.1371/journal.pcbi.1006245 [Tool interoperability]

13. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. J Transl Med. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source – GSE174609]

---

**This tutorial is part of the NGS101.com series on single cell sequencing analysis. If this tutorial helped advance your research, **please comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.**
