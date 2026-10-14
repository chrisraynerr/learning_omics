# How to Analyze Single-Cell RNA-seq Data &#8212; Complete Beginner's Guide Part 11: Copy Number Variation Analysis Using CopyKAT

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/)

---

*Learn how to detect tumor cells, infer chromosomal copy number changes, and uncover subclonal structure directly from single-cell RNA-seq data — no matched DNA sequencing required*

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/)

- [Introduction: Reading the Cancer Genome Through Gene Expression](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Introduction_Reading_the_Cancer_Genome_Through_Gene_Expression)

  - [What Is Copy Number Variation and Why Does It Matter in Cancer?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#What_Is_Copy_Number_Variation_and_Why_Does_It_Matter_in_Cancer)
  - [Why CopyKAT? The Case for a Modern Tool](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Why_CopyKAT_The_Case_for_a_Modern_Tool)
  - [How Does CopyKAT Work?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#How_Does_CopyKAT_Work)
  - [Where Does CopyKAT Fit in the Single-Cell Pipeline?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Where_Does_CopyKAT_Fit_in_the_Single-Cell_Pipeline)

- [Example Dataset: GSE131907 (Human Lung Adenocarcinoma)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Example_Dataset_GSE131907_Human_Lung_Adenocarcinoma)

  - [Study Design and Biological Questions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Study_Design_and_Biological_Questions)
  - [Two-Sample Design: Tumor vs. Normal](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Two-Sample_Design_Tumor_vs_Normal)
  - [Software and Package Versions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Software_and_Package_Versions)
  - [Required Input Files](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Required_Input_Files)

- [Part 1: Preparing Input Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Part_1_Preparing_Input_Data)

  - [Step 1.1 — Install and Load Libraries](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_11_%E2%80%94_Install_and_Load_Libraries)
  - [Step 1.2 — Load Cell Metadata](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_12_%E2%80%94_Load_Cell_Metadata)
  - [Step 1.3 — Load the Raw Count Matrix](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_13_%E2%80%94_Load_the_Raw_Count_Matrix)
  - [Step 1.4 — Align Metadata and Count Matrix](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_14_%E2%80%94_Align_Metadata_and_Count_Matrix)
  - [Step 1.5 — Create the Seurat Object and Subset](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_15_%E2%80%94_Create_the_Seurat_Object_and_Subset)
  - [Step 1.6 — Extract Raw Counts](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_16_%E2%80%94_Extract_Raw_Counts)

- [Part 2: Running CopyKAT](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Part_2_Running_CopyKAT)

  - [Installing CopyKAT](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Installing_CopyKAT)
  - [Understanding the Key Parameters](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Understanding_the_Key_Parameters)
  - [Step 2.1 — Run CopyKAT on Both Samples](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_21_%E2%80%94_Run_CopyKAT_on_Both_Samples)
  - [Step 2.2 — Output Files and Object Structure](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_22_%E2%80%94_Output_Files_and_Object_Structure)

- [Part 3: Interpreting CopyKAT Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Part_3_Interpreting_CopyKAT_Results)

  - [Step 3.1 — Add CopyKAT Labels to Each Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_31_%E2%80%94_Add_CopyKAT_Labels_to_Each_Seurat_Object)
  - [Step 3.2 — Generate UMAP Embeddings](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_32_%E2%80%94_Generate_UMAP_Embeddings)
  - [Step 3.3 — Visualize CopyKAT Labels on UMAP](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_33_%E2%80%94_Visualize_CopyKAT_Labels_on_UMAP)
  - [Step 3.4 — Validate Against the Pre-Existing Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_34_%E2%80%94_Validate_Against_the_Pre-Existing_Cell_Type_Annotation)
  - [Step 3.5 — Understanding the Results: True Signals vs. False Positives](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_35_%E2%80%94_Understanding_the_Results_True_Signals_vs_False_Positives)
  - [Step 3.6 — Reading the CNV Heatmaps](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_36_%E2%80%94_Reading_the_CNV_Heatmaps)
  - [Step 3.7 — Quality Checks Before Proceeding](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_37_%E2%80%94_Quality_Checks_Before_Proceeding)
  - [Step 3.8 — Identifying Tumor Subclones](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_38_%E2%80%94_Identifying_Tumor_Subclones)
  - [Step 3.9 — Visualize Subclones on UMAP](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_39_%E2%80%94_Visualize_Subclones_on_UMAP)

- [Part 4: Integrating CopyKAT with Downstream Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Part_4_Integrating_CopyKAT_with_Downstream_Analysis)

  - [Step 4.1 — Refine Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_41_%E2%80%94_Refine_Cell_Type_Annotation)
  - [Step 4.2 — Differential Expression Between Subclones Reveals Their Identity](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_42_%E2%80%94_Differential_Expression_Between_Subclones_Reveals_Their_Identity)
  - [Step 4.3 — Formal Tumor vs. Normal Comparison Table](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Step_43_%E2%80%94_Formal_Tumor_vs_Normal_Comparison_Table)

- [Part 5: Saving Your Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Part_5_Saving_Your_Results)
- [Part 6: Practical Tips, Caveats, and Best Practices](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Part_6_Practical_Tips_Caveats_and_Best_Practices)

  - [When CopyKAT Works Best](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#When_CopyKAT_Works_Best)
  - [Known False Positive Patterns](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Known_False_Positive_Patterns)
  - [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Common_Pitfalls_and_How_to_Avoid_Them)

- [Conclusion](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-11-copy-number-variation-analysis-using-copykat/#Like_this)

## Introduction: Reading the Cancer Genome Through Gene Expression

### What Is Copy Number Variation and Why Does It Matter in Cancer?

If you have followed this tutorial series, you have built a rich picture of your single-cell data: you have processed raw reads into count matrices ([Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/)), filtered low-quality cells ([Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/)), integrated samples and identified clusters ([Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/)), annotated cell types ([Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/)), and explored differential expression and cell-cell communication ([Parts 5-10](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/)). You know *who* the cells are, *what* they express, and *how* they signal to each other.

But in cancer biology, there is a deeper layer of information hiding in your data: the **genome itself**.

In a healthy diploid cell, virtually every gene has exactly two copies — one from each chromosome. In cancer cells, chromosomal instability causes large genomic blocks to be amplified (gaining extra copies) or deleted (losing copies). These changes are called **copy number variations (CNVs)**. For example:

- A gain on chromosome 7 means tumor cells may carry 3-5 copies of *EGFR*, fueling growth signaling.

- A deletion on chromosome 17p removes a copy of *TP53*, disabling a key tumor suppressor.

- Whole-arm gains or losses are the hallmark of **aneuploidy** — the defining genomic feature of most solid tumors.

**CopyKAT** (Copy number Karyotyping of Tumors) detects these large-scale genomic alterations directly from your scRNA-seq count matrix, without any matched DNA sequencing.

---

### Why CopyKAT? The Case for a Modern Tool

If you have read older scRNA-seq papers, you may have seen CNV analysis performed with **inferCNV** from the Broad Institute. We use CopyKAT in this tutorial for a concrete practical reason: **inferCNV is deprecated**. The developers have archived the repository and explicitly recommend seeking alternative tools for scRNA-seq CNV calling. CopyKAT is the leading actively maintained replacement, with several key advantages over the now-archived inferCNV:

**Maintenance status****Reference cells****Subclone detection****Speed****Output**

| Feature | CopyKAT | inferCNV (deprecated) |
| --- | --- | --- |
|  | Actively maintained | Archived; no longer supported |
|  | Automatically detected via GMM | Must be manually specified |
|  | Built in | Requires separate post-hoc steps |
|  | Faster (minutes to hours) | Slower (hours to days) |
|  | Binary aneuploid/diploid label + CNV matrix | CNV scores only |

> 

**A note of caution**: CopyKAT predictions are probabilistic inferences from expression data, not direct DNA measurements. Concordance with whole-genome sequencing is approximately 80%. Always visually validate predictions against the CNV heatmap, and always cross-reference with your cell type annotation to distinguish true positives from false positives. We cover this in detail in the interpretation section.

---

### How Does CopyKAT Work?

The key insight behind CopyKAT is elegant: **the expression levels of physically adjacent genes are correlated with the underlying genomic copy number**. If a large chromosomal region is amplified, all the genes in that region will, on average, be expressed at higher levels. If a region is deleted, expression drops. By smoothing expression across genomic windows and comparing each cell’s profile to a diploid baseline, CopyKAT infers copy number states at approximately 5 Mb resolution.

Here is the logic in three steps:

1. **Order genes by chromosomal position** — genes are arranged along each chromosome in genomic order.

2. **Average expression within sliding windows** — each ~220 kb bin is summarized to reduce the noise from individual gene dropouts.

3. **Compare each cell to a diploid reference** — cells that deviate from the diploid baseline across many chromosomal regions are classified as **aneuploid (tumor)**; cells with flat profiles are classified as **diploid (normal)**.

CopyKAT identifies the diploid reference cells automatically using a **Gaussian Mixture Model (GMM)**. It then clusters cells hierarchically by their inferred CNV profiles to reveal **tumor subclones**.

---

### Where Does CopyKAT Fit in the Single-Cell Pipeline?

```
Raw FASTQ reads
     |
     v
Cell Ranger (count matrix)          [Part 1]
     |
     v
Quality control &amp; filtering         [Part 2]
     |
     v
Integration &amp; clustering            [Part 3]
     |
     v
Cell type annotation                [Part 4]
     |
     v
+-----------------------------------------------+
|  CopyKAT (this tutorial)                      |
|  Input:  raw UMI count matrix (one sample)    |
|  Output: tumor/diploid labels per cell        |
|           CNV matrix (genomic bins x cells)   |
|           hierarchical clustering (subclones) |
+-----------------------------------------------+
     |
     v
Refined annotation + downstream analysis
```

CopyKAT runs on **raw UMI counts** — before normalization, scaling, or batch correction. Its labels feed back into your Seurat object as new metadata.

---

## Example Dataset: [GSE131907](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131907) (Human Lung Adenocarcinoma)

### Study Design and Biological Questions

This tutorial uses **[GSE131907](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131907)**, a landmark publicly available single-cell RNA-seq dataset accompanying the study *“Single cell RNA sequencing demonstrates the molecular and cellular reprogramming of metastatic lung adenocarcinoma”* (Kim et al., *Nature Communications*, 2020).

**Background**: This dataset profiles **208,506 cells** from **44 patients** across 58 samples, covering primary lung tumors, adjacent normal lung, lymph node and brain metastases, and pleural effusions. All primary tumor and normal lung samples were obtained from patients undergoing surgery **without prior treatment**, making the genomic profiles of tumor cells unconfounded by therapy-induced changes.

**Organism**: *Homo sapiens* (human). All downstream analysis uses the `hg20` (hg38) human reference genome.

**Pre-annotated cell types:**

| Cell Type | Total cells |
| --- | --- |
| T lymphocytes | 79,676 |
| Myeloid cells | 42,245 |
| Epithelial cells | 36,467 |
| B lymphocytes | 27,657 |
| NK cells | 11,551 |
| Fibroblasts | 4,172 |
| MAST cells | 3,396 |
| Endothelial cells | 1,996 |

---

### Two-Sample Design: Tumor vs. Normal

This tutorial runs CopyKAT on **two samples side by side**: a primary tumor sample to detect aneuploid cancer cells, and a matched normal lung sample as a negative control. Running both with identical parameters is critical — it teaches you what genuine CNV signal looks like versus background noise.

**LUNG_T06****LUNG_N06**

| Sample | Origin | Cells | Epithelial cells | Expected CopyKAT result |
| --- | --- | --- | --- | --- |
|  | Primary lung tumor | 3,426 | 145 (4.2%) | Mixed: aneuploid (epithelial) + diploid (immune/stromal) |
|  | Normal lung tissue | 2,839 | 178 (6.3%) | Predominantly diploid across all cell types |

**Cell type composition:**

*LUNG_T06 (primary tumor):*

| Cell Type | Count |
| --- | --- |
| T lymphocytes | 1,499 |
| Myeloid cells | 588 |
| B lymphocytes | 521 |
| Fibroblasts | 485 |
| Epithelial cells | 145 |
| MAST cells | 94 |
| Endothelial cells | 54 |
| NK cells | 40 |

*LUNG_N06 (normal lung):*

| Cell Type | Count |
| --- | --- |
| Myeloid cells | 1,310 |
| T lymphocytes | 1,107 |
| Epithelial cells | 178 |
| NK cells | 166 |
| MAST cells | 30 |
| B lymphocytes | 29 |
| Fibroblasts | 10 |
| Endothelial cells | 9 |

> 

**Note on tumor purity**: LUNG_T06 contains only 145 epithelial cells out of 3,426 total (~4.2%). This is a low-purity sample in terms of malignant cell content, which is common in real clinical biopsies where immune infiltration can dominate. CopyKAT handles this well precisely because the abundant immune and stromal cells provide strong diploid reference signal.

---

### Software and Package Versions

- **R**: 4.4.x

- **Seurat**: 5.x (layer-based architecture; `LayerData()` required)

- **data.table**: 1.15.x

- **CopyKAT**: 1.1.0 (from GitHub)

- **Genome reference**: hg20 (hg38, human)

---

### Required Input Files

Download both files from GEO accession **[GSE131907](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE131907)**:

`[GSE131907_Lung_Cancer_cell_annotation.txt](https://www.ncbi.nlm.nih.gov/geo/download/?acc=GSE131907&format=file&file=GSE131907%5FLung%5FCancer%5Fcell%5Fannotation%2Etxt%2Egz)``[GSE131907_Lung_Cancer_raw_UMI_matrix.rds](https://www.ncbi.nlm.nih.gov/geo/download/?acc=GSE131907&format=file&file=GSE131907%5FLung%5FCancer%5Fraw%5FUMI%5Fmatrix%2Erds%2Egz)`

| File | Description |
| --- | --- |
|  | Cell-level metadata for all 208,506 cells |
|  | Full raw count matrix as a sparse R matrix (~several GB) |

---

## Part 1: Preparing Input Data

### Step 1.1 — Install and Load Libraries

```r
library(Seurat)
library(data.table)
library(copykat)
library(ggplot2)
library(patchwork)

# Create all output directories upfront
dir.create("data/",    showWarnings = FALSE, recursive = TRUE)
dir.create("results/", showWarnings = FALSE, recursive = TRUE)
dir.create("plots/",   showWarnings = FALSE, recursive = TRUE)

results_dir <- "results/"
plots_dir   <- "plots/"
```

---

### Step 1.2 — Load Cell Metadata

```r
# Load the full metadata for all 208,506 cells
meta <- fread("GSE131907_Lung_Cancer_cell_annotation.txt", data.table = FALSE)

# The first column ("Index") contains unique cell identifiers
rownames(meta) <- meta[[1]]
```

The metadata contains these key columns:

`Index``Barcode``Sample``LUNG_T06``LUNG_N06``Sample_Origin``tLung``nLung``Cell_type``Cell_type.refined``Cell_subtype`

| Column | Description |
| --- | --- |
|  | Unique cell identifier (barcode + sample ID) |
|  | Raw 10x Genomics barcode |
|  | Sample ID (e.g., , ) |
|  | Tissue of origin (, , etc.) |
|  | Broad cell type label (8 categories) |
|  | Finer cell type annotation |
|  | Finest-level annotation |

---

### Step 1.3 — Load the Raw Count Matrix

```r
# Load the full raw count matrix
exprmx <- readRDS("GSE131907_Lung_Cancer_raw_UMI_matrix.rds")
```

---

### Step 1.4 — Align Metadata and Count Matrix

```r
# Reorder metadata rows to match the column order of the expression matrix
meta <- meta[colnames(exprmx), ]

# This MUST return TRUE before proceeding.
identical(rownames(meta), colnames(exprmx))
```

---

### Step 1.5 — Create the Seurat Object and Subset

We create one Seurat object from the full dataset, then subset to the two samples. This approach loads the large RDS file only once.

```r
# Create the full Seurat object
seu_obj <- CreateSeuratObject(
  counts    = exprmx,
  meta.data = meta,
  project   = "GSE131907"
)

# Subset to the primary tumor and normal lung samples
seu_obj_lnt06 <- subset(seu_obj, subset = Sample == "LUNG_T06")
seu_obj_lnn06 <- subset(seu_obj, subset = Sample == "LUNG_N06")

# Free the full object from memory -- it is no longer needed
rm(seu_obj, exprmx)
gc()
```

---

### Step 1.6 — Extract Raw Counts

CopyKAT requires **raw UMI counts** — never normalized, scaled, or batch-corrected data. In Seurat 5, raw counts are accessed with `LayerData()`:

```r
# Extract raw counts for each sample
raw_lnt06 <- LayerData(seu_obj_lnt06, assay = "RNA", layer = "counts")
raw_lnn06 <- LayerData(seu_obj_lnn06, assay = "RNA", layer = "counts")

dim(raw_lnt06)  # [1] 29634  3426
dim(raw_lnn06)  # [1] 29634  2839
```

---

## Part 2: Running CopyKAT

### Installing CopyKAT

```
if (!requireNamespace("devtools", quietly = TRUE))
  install.packages("devtools")

devtools::install_github("navinlabcode/copykat")
```

---

### Understanding the Key Parameters

`rawmat``id.type``"S"``"S"``"E"``genome``"hg20"``"hg20"``"mm10"``ngene.chr``5``win.size``25``KS.cut``0.1``LOW.DR``0.05``UP.DR``0.1``distance``"euclidean"``"euclidean"``"pearson"``"spearman"``norm.cell.names``""``cell.line``"no"``"yes"``n.cores``1``sam.name``""`

| Parameter | Default | What it controls |
| --- | --- | --- |
|  | — | Raw count matrix (required) |
|  |  | Gene ID type:  = gene symbol,  = Ensembl ID |
|  |  | Genome build:  (human/hg38) or  (mouse) |
|  |  | Min genes per chromosome for a cell to be analyzed |
|  |  | Genes per genomic smoothing window (range: 15-150) |
|  |  | Segmentation sensitivity: 0 = strict, 1 = permissive |
|  |  | Min fraction of cells a gene must be expressed in |
|  |  | Max fraction of cells a gene can be expressed in |
|  |  | Clustering distance: , , or |
|  |  | Optional: known diploid cell barcodes as a character vector |
|  |  | Set  for pure cell line data (no normal cells present) |
|  |  | Number of CPU cores for parallel computation |
|  |  | Output file prefix |

---

### Step 2.1 — Run CopyKAT on Both Samples

```r
# -------------------------------------------------------------------
# Run 1: Primary tumor (LUNG_T06)
# -------------------------------------------------------------------
ck_tumor <- copykat(
  rawmat    = as.matrix(raw_lnt06), # convert sparse to dense matrix
  id.type   = "S",                  # HGNC gene symbols
  genome    = "hg20",               # human hg38 annotation
  ngene.chr = 5,                    # min 5 genes per chromosome per cell
  win.size  = 25,                   # genes per smoothing window
  KS.cut    = 0.1,                  # segmentation sensitivity
  LOW.DR    = 0.05,                 # min gene detection fraction
  UP.DR     = 0.1,                  # max gene detection fraction
  distance  = "euclidean",          # hierarchical clustering distance
  cell.line = "no",                 # tissue sample with mixed cell types
  n.cores   = 4,
  sam.name  = file.path(results_dir, "LUNG_T06")
)

# -------------------------------------------------------------------
# Run 2: Normal lung (LUNG_N06) -- negative control
# -------------------------------------------------------------------
ck_normal <- copykat(
  rawmat    = as.matrix(raw_lnn06),
  id.type   = "S",
  genome    = "hg20",
  ngene.chr = 5,
  win.size  = 25,
  KS.cut    = 0.1,
  LOW.DR    = 0.05,
  UP.DR     = 0.1,
  distance  = "euclidean",
  cell.line = "no",
  n.cores   = 4,
  sam.name  = file.path(results_dir, "LUNG_N06")
)
```

> 

**”WARNING! NOT CONVERGENT!”: You will likely see many of these warnings. This means the Gaussian Mixture Model’s EM algorithm reached its maximum iterations before fully converging for some cells. CopyKAT still produces results, but classification confidence is reduced for those cells — they are more likely to end up as `not.defined`. This warning is common in samples with complex cell type mixtures and is not a sign that something went wrong.

---

### Step 2.2 — Output Files and Object Structure

After each run, the following files are written to `results/`:

```
results/LUNG_T06_copykat_prediction.txt               # per-cell labels
results/LUNG_T06_copykat_CNA_results.txt              # CNV values per 220kb bin
results/LUNG_T06_copykat_CNA_raw_results_gene_by_cell.txt  # raw gene-level CNVs
results/LUNG_T06_copykat_heatmap.jpeg                 # auto-generated heatmap
results/LUNG_T06_copykat_with_genes_heatmap.pdf       # heatmap with gene labels
results/LUNG_T06_copykat_clustering_results.rds       # hierarchical clustering object

results/LUNG_N06_copykat_prediction.txt
results/LUNG_N06_copykat_CNA_results.txt
results/LUNG_N06_copykat_CNA_raw_results_gene_by_cell.txt
results/LUNG_N06_copykat_heatmap.jpeg
results/LUNG_N06_copykat_with_genes_heatmap.pdf
results/LUNG_N06_copykat_clustering_results.rds
```

The R objects returned by `copykat()` contain three key components:

`$prediction``copykat.pred``$CNAmat``$hclustering`

| Slot | Content | How you use it |
| --- | --- | --- |
|  | Data frame: cell barcode +  label | Add to Seurat metadata for visualization |
|  | Matrix: columns 1-6 = chromosomal position; columns 7+ = per-cell CNV values | Heatmap and subclone analysis |
|  | Hierarchical clustering of CNV profiles | Dendrogram cutting for subclone definition |

The three possible labels in `$prediction$copykat.pred`:

- **`"aneuploid"`** — predicted malignant cell; high CNV burden.

- **`"diploid"`** — predicted normal cell; flat CNV profile.

- **`"not.defined"`** — CopyKAT could not make a confident call. Treat as ambiguous.

---

## Part 3: Interpreting CopyKAT Results

### Step 3.1 — Add CopyKAT Labels to Each Seurat Object

```r
# --- LUNG_T06 ---
pred_tumor <- ck_tumor$prediction
colnames(pred_tumor)[colnames(pred_tumor) == "copykat.pred"] <- "copykat_label"

seu_obj_lnt06$copykat_label <- pred_tumor$copykat_label[
  match(colnames(seu_obj_lnt06), pred_tumor$cell.names)
]
seu_obj_lnt06$copykat_label[is.na(seu_obj_lnt06$copykat_label)] <- "not.defined"

# --- LUNG_N06 ---
pred_normal <- ck_normal$prediction
colnames(pred_normal)[colnames(pred_normal) == "copykat.pred"] <- "copykat_label"

seu_obj_lnn06$copykat_label <- pred_normal$copykat_label[
  match(colnames(seu_obj_lnn06), pred_normal$cell.names)
]
seu_obj_lnn06$copykat_label[is.na(seu_obj_lnn06$copykat_label)] <- "not.defined"

# Check label distributions
table(seu_obj_lnt06$copykat_label)
table(seu_obj_lnn06$copykat_label)
```

**Expected output:**

```
LUNG_T06:
  aneuploid     diploid not.defined
       1063        1893         470

LUNG_N06:
  aneuploid     diploid not.defined
       1097        1502         240
```

---

### Step 3.2 — Generate UMAP Embeddings

```r
process_for_umap <- function(seu) {
  seu <- NormalizeData(seu, verbose = FALSE)
  seu <- FindVariableFeatures(seu, verbose = FALSE)
  seu <- ScaleData(seu, verbose = FALSE)
  seu <- RunPCA(seu, verbose = FALSE)
  seu <- RunUMAP(seu, dims = 1:20, verbose = FALSE)
  seu
}

seu_obj_lnt06 <- process_for_umap(seu_obj_lnt06)
seu_obj_lnn06 <- process_for_umap(seu_obj_lnn06)
```

---

### Step 3.3 — Visualize CopyKAT Labels on UMAP

```r
copykat_colors <- c(
  "aneuploid"   = "#D62728",
  "diploid"     = "#1F77B4",
  "not.defined" = "#AAAAAA"
)

p_t06_celltype <- DimPlot(seu_obj_lnt06, group.by = "Cell_type",
                           label = TRUE, repel = TRUE, pt.size = 0.4) +
  labs(title = "LUNG_T06: Cell Types") +
  theme_minimal(base_size = 12) + theme(legend.position = "none")

p_t06_copykat  <- DimPlot(seu_obj_lnt06, group.by = "copykat_label",
                           cols = copykat_colors, pt.size = 0.4, alpha = 0.6) +
  labs(title = "LUNG_T06: CopyKAT Labels") +
  theme_minimal(base_size = 12)

p_n06_celltype <- DimPlot(seu_obj_lnn06, group.by = "Cell_type",
                           label = TRUE, repel = TRUE, pt.size = 0.4) +
  labs(title = "LUNG_N06: Cell Types") +
  theme_minimal(base_size = 12) + theme(legend.position = "none")

p_n06_copykat  <- DimPlot(seu_obj_lnn06, group.by = "copykat_label",
                           cols = copykat_colors, pt.size = 0.4, alpha = 0.6) +
  labs(title = "LUNG_N06: CopyKAT Labels") +
  theme_minimal(base_size = 12)

png(file.path(plots_dir, "01_umap_copykat_comparison.png"),
    width = 2400, height = 1600, res = 150)
(p_t06_celltype | p_t06_copykat) / (p_n06_celltype | p_n06_copykat)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/01_umap_copykat_comparison.png?resize=1024%2C683&ssl=1)

**How to interpret the 2×2 panel:**

In the **LUNG_T06** row (top), you will see aneuploid labels (red) concentrated in the fibroblast cluster and the myeloid cluster, as well as in part of the epithelial cluster. T lymphocytes and NK cells remain predominantly diploid (blue). This mixed picture is expected and is fully explained in Step 3.5.

You may also notice that **the epithelial cells form more than one cluster on the UMAP** (there are two to three small separated groups in the upper-right area), but only a subset of them is labeled red. This is biologically meaningful. The multiple epithelial sub-clusters likely represent different transcriptional states — for example, one cluster may contain more proliferating tumor cells with strong CNV signal while another contains more quiescent or differentiated epithelial cells whose expression profile does not deviate enough from the diploid baseline for CopyKAT to classify them as aneuploid. CopyKAT classifies cells based on genome-wide expression variance relative to the baseline, not based on UMAP proximity, so neighboring clusters can receive different labels.

> 

**Note on reproducibility**: CopyKAT uses a Gaussian Mixture Model with random initialization. Re-running the same data may produce modestly different aneuploid/diploid assignments, particularly for cells near the classification boundary. The overall pattern should be consistent across runs, but exact cell counts may vary.

In the **LUNG_N06** row (bottom), the myeloid cluster appears predominantly red just as in the tumor sample, while T cells, NK cells, and epithelial cells are largely blue. This confirms the myeloid aneuploid signal is a systematic artifact rather than a tumor-specific finding. Step 3.5 explains why.

---

### Step 3.4 — Validate Against the Pre-Existing Cell Type Annotation

This is the most important step. Because GSE131907 comes with a `Cell_type` annotation, we can directly measure how well CopyKAT’s labels align with the expected biology.

```r
# Aneuploid fraction per cell type -- LUNG_T06
aneuploid_by_celltype_tumor <- sort(
  tapply(seu_obj_lnt06$copykat_label == "aneuploid",
         seu_obj_lnt06$Cell_type, mean, na.rm = TRUE),
  decreasing = TRUE
)
print(round(aneuploid_by_celltype_tumor, 3))

# Aneuploid fraction per cell type -- LUNG_N06
aneuploid_by_celltype_normal <- sort(
  tapply(seu_obj_lnn06$copykat_label == "aneuploid",
         seu_obj_lnn06$Cell_type, mean, na.rm = TRUE),
  decreasing = TRUE
)
print(round(aneuploid_by_celltype_normal, 3))
```

**Actual output:**

```
LUNG_T06:
      Fibroblasts     Myeloid cells  Epithelial cells Endothelial cells
            0.893             0.871             0.559             0.278
       MAST cells     T lymphocytes     B lymphocytes          NK cells
            0.021             0.010             0.010             0.000

LUNG_N06:
    Myeloid cells  Epithelial cells     B lymphocytes Endothelial cells
            0.836             0.011             0.000             0.000
      Fibroblasts        MAST cells          NK cells     T lymphocytes
            0.000             0.000             0.000             0.000
```

---

### Step 3.5 — Understanding the Results: True Signals vs. False Positives

The results above reveal something important: **CopyKAT produces both true signals and systematic false positives in certain cell types**. This is not a sign that the tool has failed — it is a known behavior that every practitioner needs to understand and account for. Let us work through the results cell type by cell type.

**Myeloid cells — a systematic false positive in both samples:**

The most revealing finding is that myeloid cells show 87.1% aneuploid in LUNG_T06 *and* 83.6% aneuploid in LUNG_N06 (normal tissue). Normal myeloid cells in healthy lung tissue cannot be 84% aneuploid — this is biologically impossible. This is a **false positive driven by cell-type-specific expression patterns**. Tissue-resident myeloid cells, particularly alveolar macrophages, have a highly distinctive transcriptional profile compared to circulating monocytes or other immune cells. Their unique gene expression program, spread across the genome, creates systematic expression deviations that CopyKAT’s algorithm interprets as copy number changes. Because this pattern appears in both samples equally, it is definitively an artifact, not a biological CNV signal.

**Fibroblasts in LUNG_T06 — likely cancer-associated fibroblasts:**

Fibroblasts at 89.3% aneuploid in LUNG_T06, but 0% in LUNG_N06. This is a more nuanced situation. The fibroblasts in a tumor microenvironment are largely **cancer-associated fibroblasts (CAFs)** — cells that have been transcriptionally reprogrammed by the tumor to support its growth. CAFs have dramatically altered gene expression compared to normal fibroblasts, and these widespread expression changes can create spurious CNV-like patterns in CopyKAT. Whether a small fraction are genuinely aneuploid tumor cells that express fibroblast-like markers is impossible to determine from CopyKAT alone and would require orthogonal validation.

**Epithelial cells — the credible signal:**

Epithelial cells show 55.9% aneuploid in LUNG_T06 and only 1.1% in LUNG_N06. This is a biologically credible result: approximately half of the tumor epithelial cells carry detectable CNV burden, while normal lung epithelial cells are largely diploid. This differential between tumor and normal (55.9% vs. 1.1%) is the signal we can trust.

**Why does LUNG_N06 appear to have more aneuploid cells overall than LUNG_T06?**

LUNG_N06 has 1,310 myeloid cells (vs. 588 in LUNG_T06). Since ~84% of myeloid cells are false-positive aneuploid in both samples, this larger myeloid compartment in LUNG_N06 produces more total aneuploid calls (roughly 1,095 cells from myeloid alone), making LUNG_N06 appear to have more aneuploidy than the tumor sample. This is an artifact of sample composition, not biology.

**The key lesson**: Raw CopyKAT output must always be cross-referenced with your cell type annotation. The aneuploid labels for myeloid cells and, in this case, fibroblasts should not be interpreted as evidence of malignancy.

---

### Step 3.6 — Reading the CNV Heatmaps

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/copykat_heatmap-e1775530035380-1024x318.png?resize=1024%2C318&ssl=1)

CopyKAT automatically saves a heatmap for each sample. Open both side by side:

- `results/LUNG_T06_copykat_heatmap.jpeg`

- `results/LUNG_N06_copykat_heatmap.jpeg`

```
How to read the CopyKAT CNV heatmap (human hg20 format):

Axes:
  Rows    = individual cells (one row per cell), ordered by hierarchical
            clustering of CNV profiles
  Columns = ~220 kb genomic bins, ordered from chr1 to chrX
  Side bar (orange) = pred.aneuploid cells
  Side bar (green)  = pred.diploid cells

Color scale:
  RED    = chromosomal GAIN vs. diploid baseline
  BLUE   = chromosomal LOSS vs. diploid baseline
  WHITE  = no significant deviation from diploid

What you will observe in LUNG_T06:
  The aneuploid rows (orange bar, top portion) show scattered red/blue
  patterns across chromosomes, but the patterns are relatively noisy
  rather than showing the large coherent arm-level blocks you would see
  in a highly aneuploid cancer cell line. This reflects the heterogeneous
  nature of the aneuploid cells -- a mix of true tumor epithelial cells
  (genuine CNVs) and false-positive myeloid/fibroblast cells (noise).

What you will observe in LUNG_N06:
  The aneuploid rows also show scattered patterns similar to LUNG_T06.
  The diploid rows look relatively flat. Critically, the overall
  heatmap does NOT look dramatically different from LUNG_T06 because
  both samples' aneuploid fractions are dominated by the same myeloid
  false positive signal.

What "data quality is ok" means:
  CopyKAT prints this message when it determines that the input data
  passed internal quality thresholds and the analysis proceeded normally.
  It does not indicate that all predictions are correct.
```

---

### Step 3.7 — Quality Checks Before Proceeding

- Epithelial cells in LUNG_T06 show a substantially higher aneuploid fraction than in LUNG_N06 — this is the genuine signal.

- Myeloid cells show high aneuploid fractions in *both* samples — this is a known false positive artifact.

- T cells, NK cells, and B cells have near-zero aneuploid fractions in both samples — this is the expected behavior for lymphocytes.

- The `not.defined` fraction is 13.7% in LUNG_T06 (related to the GMM convergence warnings) and 8.5% in LUNG_N06.

---

### Step 3.8 — Identifying Tumor Subclones

CopyKAT’s hierarchical clustering of CNV profiles reveals groups of cells that share the same pattern of chromosomal aberrations — these are **subclones**. Here we run the subclone analysis on all aneuploid cells, exactly as CopyKAT’s built-in workflow does, and then interpret what the resulting subclones represent by cross-referencing with the cell type annotation.

```r
# Get barcodes of all aneuploid cells from the tumor sample
aneuploid_cells <- ck_tumor$prediction$cell.names[
  ck_tumor$prediction$copykat.pred == "aneuploid"
]

# The CNAmat has 6 chromosomal annotation columns (chr, abspos, start, end,
# length, gene_counts); per-cell CNV values start at column 7
cna_tumor <- t(ck_tumor$CNAmat[, 7:ncol(ck_tumor$CNAmat)])  # cells x bins
cna_tumor <- cna_tumor[aneuploid_cells, , drop = FALSE]

# Hierarchical clustering of all aneuploid cells by CNV profile
hc_tumor  <- hclust(dist(cna_tumor, method = "euclidean"), method = "ward.D2")

# Cut the dendrogram into k subclones
# Start with k=2; inspect the heatmap for the number of visually distinct
# row blocks to guide your choice
k         <- 2
subclones <- cutree(hc_tumor, k = k)

subclone_df <- data.frame(
  cell.names = names(subclones),
  subclone   = paste0("Subclone_", subclones),
  stringsAsFactors = FALSE
)

# Add subclone labels to Seurat object; non-aneuploid cells get "Non-tumor"
seu_obj_lnt06$subclone <- subclone_df$subclone[
  match(colnames(seu_obj_lnt06), subclone_df$cell.names)
]
seu_obj_lnt06$subclone[is.na(seu_obj_lnt06$subclone)] <- "Non-tumor"
```

> 

**Important**: Running subclone analysis on all aneuploid cells will produce subclones that reflect the dominant false-positive signal — in this dataset, the two “subclones” will correspond largely to the myeloid cell cluster vs. the fibroblast cluster, not to distinct tumor cell lineages. This is a feature of the raw output, not a bug, and the differential expression step below will make this explicit. For a publication-ready subclone analysis of true tumor subclones, you would filter to aneuploid epithelial cells only before clustering — we demonstrate this interpretation in Step 4.2.

---

### Step 3.9 — Visualize Subclones on UMAP

```r
subclone_colors <- c(
  "Subclone_1" = "#E41A1C",
  "Subclone_2" = "#FF7F00",
  "Non-tumor"  = "#BBBBBB"
)

p_subclone <- DimPlot(seu_obj_lnt06, group.by = "subclone",
                       cols = subclone_colors, pt.size = 0.5, alpha = 0.7) +
  labs(title = "LUNG_T06: CopyKAT Subclones",
       color = "Subclone") +
  theme_minimal(base_size = 13)

png(file.path(plots_dir, "02_umap_subclones_LUNG_T06.png"),
    width = 900, height = 750, res = 150)
p_subclone
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/02_umap_subclones_LUNG_T06-e1775530153597.png?resize=900%2C693&ssl=1)

---

## Part 4: Integrating CopyKAT with Downstream Analysis

### Step 4.1 — Refine Cell Type Annotation

Combine the pre-existing `Cell_type` annotation with CopyKAT’s labels to create a `refined_annotation` column. We prefix all aneuploid-labeled cells with `"Malignant_"` regardless of cell type. This preserves the raw CopyKAT output in the annotation and makes it easy to subset any cell type by malignancy status for downstream analysis.

```r
# Prefix all aneuploid-labeled cells with "Malignant_"
# This includes myeloid and fibroblast false positives -- they are labeled
# honestly here so you can filter them out in downstream steps using Cell_type
seu_obj_lnt06$refined_annotation <- ifelse(
  seu_obj_lnt06$copykat_label == "aneuploid",
  paste0("Malignant_", seu_obj_lnt06$Cell_type),
  seu_obj_lnt06$Cell_type
)

# In LUNG_N06: annotation stands as-is
seu_obj_lnn06$refined_annotation <- seu_obj_lnn06$Cell_type

table(seu_obj_lnt06$refined_annotation)
```

**Expected output:**

```
              B lymphocytes           Endothelial cells        Epithelial cells
                        516                          39                      64
            Fibroblasts       Malignant_B lymphocytes  Malignant_Endothelial cells
                         52                           5                          15
 Malignant_Epithelial cells       Malignant_Fibroblasts     Malignant_MAST cells
                         81                         433                           2
    Malignant_Myeloid cells     Malignant_T lymphocytes               MAST cells
                        512                          15                          92
              Myeloid cells                    NK cells               T lymphocytes
                         76                          40                        1484
```

The `Malignant_Fibroblasts` (433 cells) and `Malignant_Myeloid cells` (512 cells) entries reflect the false positive patterns discussed in Step 3.5. When using `refined_annotation` for downstream analyses such as cell-cell communication or differential expression, filter to `Malignant_Epithelial cells` as the biologically credible malignant population.

---

### Step 4.2 — Differential Expression Between Subclones Reveals Their Identity

Running DE between the two subclones is the most direct way to understand what they actually represent. We run it first on all aneuploid cells (the raw CopyKAT output), then interpret the results.

```r
# Subset to all aneuploid cells
seu_tumor_only <- subset(seu_obj_lnt06, subset = copykat_label == "aneuploid")
Idents(seu_tumor_only) <- "subclone"

subclone_markers <- FindAllMarkers(
  seu_tumor_only,
  assay           = "RNA",
  only.pos        = TRUE,
  min.pct         = 0.25,
  logfc.threshold = 0.25
)

write.csv(subclone_markers,
          file.path(results_dir, "LUNG_T06_subclone_markers.csv"),
          row.names = FALSE)

# Top 10 markers per subclone
top10 <- do.call(rbind, lapply(
  split(subclone_markers, subclone_markers$cluster),
  function(df) head(df[order(df$avg_log2FC, decreasing = TRUE), ], 10)
))
print(top10[, c("cluster", "gene", "avg_log2FC", "p_val_adj")])
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/lnt06_subclone_top10_markers.png?resize=1024%2C919&ssl=1)

**What the markers tell us:**

The top markers for each subclone immediately reveal that neither represents a genuine tumor subclone:

**Subclone_1** (EGFL6, MEG3, SGIP1, ECM2, ANGPTL2, SFRP4, TBX2, ASPN, FMOD, FAP) is a textbook **cancer-associated fibroblast (CAF) signature**:

- *FAP* (fibroblast activation protein) and *ASPN* (asporin) are canonical CAF markers routinely used to identify activated stromal fibroblasts in tumor tissue.

- *FMOD* (fibromodulin) and *ECM2* are extracellular matrix proteins produced by fibroblasts.

- *SFRP4* is a WNT signaling antagonist highly expressed in CAFs.

- *ANGPTL2* and *EGFL6* are secreted factors associated with stromal cell activity.

This “subclone” is not a tumor clone — it is the CAF population whose widespread transcriptional reprogramming was falsely classified as aneuploid by CopyKAT.

**Subclone_2** (NLRP3, P2RY13, PILRA, CSF1R, C1QC, CD33, LILRB2, C5AR1, CLEC12A) is a textbook **macrophage/myeloid signature**:

- *CSF1R* (colony-stimulating factor 1 receptor) and *CD33* are defining markers of the myeloid lineage.

- *C1QC* is a complement component characteristic of tumor-associated macrophages (TAMs).

- *LILRB2*, *PILRA*, and *CLEC12A* are myeloid inhibitory receptors expressed on macrophages and monocytes.

- *C5AR1* (complement C5a receptor) and *NLRP3* (inflammasome component) further confirm macrophage identity.

This “subclone” is the myeloid cell population whose tissue-specific expression program was systematically misclassified as aneuploid, as predicted by the LUNG_N06 negative control result.

Neither subclone represents a genuine tumor cell lineage. **This outcome demonstrates exactly why Step 3.5 matters**: CopyKAT’s raw subclone output can be cell-type contamination, not tumor evolution. For a biologically meaningful subclone analysis, filter to aneuploid cells within the annotated `Epithelial cells` population:

```r
# Publication-ready subclone analysis: aneuploid epithelial cells only
seu_epithelial_aneuploid <- subset(
  seu_obj_lnt06,
  subset = copykat_label == "aneuploid" & Cell_type == "Epithelial cells"
)
Idents(seu_epithelial_aneuploid) <- "subclone"

epithelial_subclone_markers <- FindAllMarkers(
  seu_epithelial_aneuploid,
  assay           = "RNA",
  only.pos        = TRUE,
  min.pct         = 0.25,
  logfc.threshold = 0.25
)
```

> 

**Publication note**: `FindAllMarkers` is appropriate for exploratory analysis. For publication-quality comparisons across multiple patients, use a pseudobulk DESeq2 approach as described in [Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/).

---

### Step 4.3 — Formal Tumor vs. Normal Comparison Table

```r
cell_types <- union(unique(seu_obj_lnt06$Cell_type), unique(seu_obj_lnn06$Cell_type))

summary_df <- data.frame(
  cell_type          = cell_types,
  aneuploid_LUNG_T06 = sapply(cell_types, function(ct) {
    cells <- seu_obj_lnt06$Cell_type == ct
    if (sum(cells) == 0) return(NA)
    mean(seu_obj_lnt06$copykat_label[cells] == "aneuploid", na.rm = TRUE)
  }),
  aneuploid_LUNG_N06 = sapply(cell_types, function(ct) {
    cells <- seu_obj_lnn06$Cell_type == ct
    if (sum(cells) == 0) return(NA)
    mean(seu_obj_lnn06$copykat_label[cells] == "aneuploid", na.rm = TRUE)
  })
)

summary_df <- summary_df[order(summary_df$aneuploid_LUNG_T06, decreasing = TRUE), ]
summary_df[, 2:3] <- round(summary_df[, 2:3], 3)
print(summary_df)

write.csv(summary_df,
          file.path(results_dir, "aneuploid_fraction_summary.csv"),
          row.names = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/aneuploid_fraction_summary.png?resize=958%2C430&ssl=1)

---

## Part 5: Saving Your Results

```r
# Save updated Seurat objects (requires the "data/" directory created in Step 1.1)
saveRDS(seu_obj_lnt06, "data/seu_obj_LUNG_T06_copykat.rds")
saveRDS(seu_obj_lnn06, "data/seu_obj_LUNG_N06_copykat.rds")

# Export per-cell summary tables
write.csv(
  data.frame(
    cell_id            = colnames(seu_obj_lnt06),
    sample             = seu_obj_lnt06$Sample,
    cell_type          = seu_obj_lnt06$Cell_type,
    copykat_label      = seu_obj_lnt06$copykat_label,
    refined_annotation = seu_obj_lnt06$refined_annotation,
    subclone           = seu_obj_lnt06$subclone
  ),
  file.path(results_dir, "LUNG_T06_cell_summary.csv"),
  row.names = FALSE
)

write.csv(
  data.frame(
    cell_id            = colnames(seu_obj_lnn06),
    sample             = seu_obj_lnn06$Sample,
    cell_type          = seu_obj_lnn06$Cell_type,
    copykat_label      = seu_obj_lnn06$copykat_label,
    refined_annotation = seu_obj_lnn06$refined_annotation
  ),
  file.path(results_dir, "LUNG_N06_cell_summary.csv"),
  row.names = FALSE
)
```

---

## Part 6: Practical Tips, Caveats, and Best Practices

### When CopyKAT Works Best

- **The dataset contains a genuine mixture of tumor and non-malignant cells.** The automatic diploid detection depends on finding a “flat” population to serve as the baseline.

- **Read depth is adequate.** Aim for at least 2,000 UMIs per cell. Lower depth produces noisy profiles where expression variation mimics CNVs.

- **The tumor has real chromosomal instability.** Lung adenocarcinoma is known for substantial genomic instability, making it a good target.

- **Your dataset is human or mouse.** CopyKAT supports only `hg20` (hg38) and `mm10`.

---

### Known False Positive Patterns

CopyKAT performs best on **epithelial tumors** (carcinomas) with clear aneuploid signal. Certain cell types are systematically prone to false positives and their aneuploid calls should be treated with skepticism:

**Myeloid cells / Macrophages****High****Cancer-associated fibroblasts****Moderate-High****Endothelial cells in tumors****Moderate****B cells / Plasma cells****Low-Moderate****T cells / NK cells****Low**

| Cell Type | False Positive Risk | Biological Reason |
| --- | --- | --- |
|  |  | Tissue-specific activation states create genome-wide expression patterns that mimic CNVs; seen in both tumor AND normal samples |
|  |  | Tumor-induced transcriptional reprogramming creates widespread expression changes |
|  |  | Tumor-induced angiogenic states alter expression broadly |
|  |  | Immunoglobulin gene expression clusters on specific chromosomes can create false gains at those loci |
|  |  | TCR gene expression rarely generates strong false positives |

**The practical rule**: Always calculate aneuploid fractions per cell type and compare between tumor and matched normal. Any cell type with high aneuploid fractions in *both* tumor and normal is almost certainly a false positive in both.

---

### Common Pitfalls and How to Avoid Them

**Pitfall 1: Accepting all aneuploid calls without cell type cross-referencing**

Never use raw `aneuploid` labels without checking which cell types they fall in. In this tutorial, the `aneuploid_fraction_summary.csv` table is essential for distinguishing signal from noise.

**Pitfall 2: Using normalized or batch-corrected counts as input**

```r
# CORRECT
raw_mat <- LayerData(seu_obj_lnt06, assay = "RNA", layer = "counts")

# WRONG
wrong1 <- LayerData(seu_obj_lnt06, assay = "RNA", layer = "data")
wrong2 <- LayerData(seu_obj_lnt06, assay = "integrated", layer = "data")
```

**Pitfall 3: Mixing samples in a single run**

Always run each sample separately. Inter-patient expression differences confound the diploid reference detection.

**Pitfall 4: Running subclone analysis on all aneuploid cells**

As demonstrated in this tutorial, running subclone analysis on all aneuploid cells produces “subclones” that reflect cell type differences rather than tumor evolution. Restrict to the cell type where you have credible aneuploid signal (e.g., epithelial cells in a carcinoma).

---

## Conclusion

In this tutorial, you learned how to:

1. **Run CopyKAT** on a tumor and matched normal sample.

2. **Interpret results honestly**: CopyKAT’s aneuploid labels must always be cross-referenced with cell type annotation. Myeloid cells are systematically prone to false positives; the credible signal in LUNG_T06 is concentrated in Epithelial cells (55.9% aneuploid vs. 1.1% in normal).

3. **Run subclone analysis correctly**: restrict to the annotated cell type with credible aneuploid signal to avoid “subclones” that actually represent cell type differences.

4. **Diagnose unexpected results**: the apparent higher aneuploidy in LUNG_N06 versus LUNG_T06 is explained by LUNG_N06’s larger myeloid compartment (1,310 cells) producing more false-positive aneuploid calls.

**What comes next?**

With confirmed malignant epithelial labels and confident diploid labels for the immune compartment, you can:

- Run **cell-cell communication analysis** (Parts [9-10](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/)) on the tumor microenvironment.

- Apply **gene regulatory network analysis** (WGCNA) to co-expression modules specific to each tumor subclone.

---

## References

1. Gao R, Bai S, Henderson YC, et al. Delineating copy number and clonal substructure in human tumors from single-cell transcriptomes. *Nature Biotechnology*. 2021;39(5):599-608. doi:10.1038/s41587-020-00795-2

2. Kim N, Kim HK, Lee K, et al. Single-cell RNA sequencing demonstrates the molecular and cellular reprogramming of metastatic lung adenocarcinoma. *Nature Communications*. 2020;11(1):2285. doi:10.1038/s41467-020-16164-1

3. Sikkema L, Ramirez-Suastegui C, Strobl DC, et al. Benchmarking scRNA-seq copy number variation callers. *Nature Communications*. 2025. doi:10.1038/s41467-025-62359-9

4. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y

5. CopyKAT GitHub repository: [https://github.com/navinlabcode/copykat](https://github.com/navinlabcode/copykat)

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
