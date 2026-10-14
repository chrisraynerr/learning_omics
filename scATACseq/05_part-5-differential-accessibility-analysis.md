# How to Analyze Single-Cell ATAC-seq Data &#8212; A Complete Beginner's Guide Part 5: Differential Accessibility Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** September 20, 2026
- **Categories:** ATAC-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/)

---

*Compare chromatin accessibility between disease and health inside a single cell type — with a single-cell test, a pseudobulk test, motif enrichment, and pathway analysis — and learn why the two tests disagree.*

---

[Part 4](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/) ended with 30,571 cells carrying biological names: CD14 monocytes, CD8 T effectors, B cells, NK cells. Naming cells is not the point of an experiment, though. The point is the comparison the experiment was designed to make, and **scATAC-seq differential accessibility analysis** is how you make it. In this dataset the comparison is severe COVID-19 versus healthy, and it has been waiting since Part 1.

**Differential accessibility (DA) analysis** works one cell type at a time. It asks which of the 185,581 consensus peaks are more open, or less open, in one condition than the other. A peak that gains accessibility in disease is a regulatory element that a disease-driven transcription factor has pried open. A peak that closes is a program being shut down. Unlike differential expression, which tells you what a cell is currently making, differential accessibility tells you what a cell is currently *prepared* to make — the regulatory potential that precedes transcription.

This tutorial walks through that analysis end to end: choosing a valid comparison, running it two different ways, understanding why the two answers differ by orders of magnitude, and then converting a list of genomic coordinates into transcription factors and biological pathways you can write about.

> 

**Prerequisites:** This tutorial assumes you have completed [Part 1: From FASTQ to Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/), [Part 2: Thorough Quality Control with Signac](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/), [Part 3: Integration and Clustering](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/) and [Part 4: Cell Type Identification](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/). You need the `atac_annotated.rds` object Part 4 produced, the same Pixi environment, and roughly 60 GB of RAM on a compute node. No new raw data is downloaded in this tutorial.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/)

- [🧭 Introduction: What Differential Accessibility Analysis Actually Asks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%F0%9F%A7%AD_Introduction_What_Differential_Accessibility_Analysis_Actually_Asks)

  - [Two Different Questions That Both Get Called “DA”](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Two_Different_Questions_That_Both_Get_Called_%E2%80%9CDA%E2%80%9D)
  - [How scATAC-seq DA Differs from scRNA-seq Differential Expression](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#How_scATAC-seq_DA_Differs_from_scRNA-seq_Differential_Expression)
  - [How scATAC-seq DA Compares to Bulk ATAC-seq Differential Binding](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#How_scATAC-seq_DA_Compares_to_Bulk_ATAC-seq_Differential_Binding)
  - [What the Benchmark Literature Says About DA Methods](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#What_the_Benchmark_Literature_Says_About_DA_Methods)
  - [The Part 5 Workflow](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#The_Part_5_Workflow)

- [⚙️ Setting Up the R Environment](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%E2%9A%99%EF%B8%8F_Setting_Up_the_R_Environment)

  - [Adding the Four New Packages to Your Pixi Environment](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Adding_the_Four_New_Packages_to_Your_Pixi_Environment)
  - [The Bioconda Stub Problem, Again — And This Time There Are Three](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#The_Bioconda_Stub_Problem_Again_%E2%80%94_And_This_Time_There_Are_Three)
  - [Loading Libraries and Creating the Part 5 Directory Structure](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Loading_Libraries_and_Creating_the_Part_5_Directory_Structure)

- [📦 Example Data: The Annotated Object from Part 4](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%F0%9F%93%A6_Example_Data_The_Annotated_Object_from_Part_4)

  - [Loading and Inspecting the Object](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Loading_and_Inspecting_the_Object)
  - [Choosing a Cell Type and Checking the Design](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Choosing_a_Cell_Type_and_Checking_the_Design)
  - [The Depth Confounder You Must Quantify First](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#The_Depth_Confounder_You_Must_Quantify_First)

- [🔬 Differential Accessibility Analysis](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%F0%9F%94%AC_Differential_Accessibility_Analysis)

  - [Method 1: The Single-Cell Logistic Regression Test](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Method_1_The_Single-Cell_Logistic_Regression_Test)
  - [Annotating Peaks With Their Nearest Gene](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Annotating_Peaks_With_Their_Nearest_Gene)
  - [Method 2: Pseudobulk Aggregation and DESeq2](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Method_2_Pseudobulk_Aggregation_and_DESeq2)
  - [Comparing the Two Result Sets](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Comparing_the_Two_Result_Sets)
  - [Transcription Factor Motif Enrichment on Differential Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Transcription_Factor_Motif_Enrichment_on_Differential_Peaks)

- [🧬 Pathway Enrichment Analysis on Differentially Accessible Regions](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%F0%9F%A7%AC_Pathway_Enrichment_Analysis_on_Differentially_Accessible_Regions)

  - [Why This Step Is Harder Than It Looks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Why_This_Step_Is_Harder_Than_It_Looks)
  - [Building the Gene List and the Correct Universe](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Building_the_Gene_List_and_the_Correct_Universe)
  - [GO Over-Representation Analysis with clusterProfiler](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#GO_Over-Representation_Analysis_with_clusterProfiler)

- [📊 Visualization](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%F0%9F%93%8A_Visualization)

  - [The Volcano Plot: Effect Size Against Significance](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#The_Volcano_Plot_Effect_Size_Against_Significance)
  - [Coverage Plots: Looking at the Raw Signal](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Coverage_Plots_Looking_at_the_Raw_Signal)
  - [The Mistake This Section Exists to Prevent](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#The_Mistake_This_Section_Exists_to_Prevent)
  - [Pseudobulk Heatmap of the Top Differential Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Pseudobulk_Heatmap_of_the_Top_Differential_Peaks)
  - [Motif Enrichment and GO Enrichment Plots](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Motif_Enrichment_and_GO_Enrichment_Plots)

- [💾 Saving Results for Reuse and Supplementary Material](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%F0%9F%92%BE_Saving_Results_for_Reuse_and_Supplementary_Material)

  - [Running the Same Analysis for Other Cell Types](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Running_the_Same_Analysis_for_Other_Cell_Types)

- [✅ Best Practices for scATAC-seq Differential Accessibility Analysis](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%E2%9C%85_Best_Practices_for_scATAC-seq_Differential_Accessibility_Analysis)
- [⚠️ Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%E2%9A%A0%EF%B8%8F_Common_Pitfalls_and_How_to_Avoid_Them)
- [📚 References](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#%F0%9F%93%9A_References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-5-differential-accessibility-analysis/#Like_this)

## 🧭 Introduction: What Differential Accessibility Analysis Actually Asks

### Two Different Questions That Both Get Called “DA”

The phrase “differential accessibility” covers two analyses that share a function call and share nothing else.

**Marker peaks (one-versus-rest).** You already did this in Part 4, Step 21. Every cluster was tested against all other clusters combined, and the output was a set of peaks that define the cluster’s identity. The comparison groups are cell types, and the biological question is “what makes this population distinct?”

**Condition contrasts (group-versus-group).** That is Part 5. You fix the cell type — CD14 monocytes, say — and split those cells by an experimental variable — severe COVID-19 versus healthy. The comparison groups are conditions, and the biological question is “what did the disease do to this cell type’s regulatory landscape?”

The distinction matters because the statistics behave completely differently. In a one-versus-rest marker test the effect sizes are large: a B cell peak is wide open in B cells and essentially closed everywhere else. In a condition contrast, effect sizes are small, the two groups are the same cell type, and every technical artifact in your data — sequencing depth, batch, donor genetics — is now a plausible alternative explanation for anything you find. Condition contrasts are where scATAC-seq analyses go wrong.

### How scATAC-seq DA Differs from scRNA-seq Differential Expression

If you have worked through [Part 5 of the scRNA-seq series](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/), the workflow will feel familiar and the data will not. The differences are structural, not cosmetic.

**Feature***your***Feature meaning****Counts per feature per cell****Sparsity****Zero means****Multiple testing****Interpretation**`chr14-99245000-99246000`**Where the signal is**

|  | scRNA-seq differential expression | scATAC-seq differential accessibility |
| --- | --- | --- |
|  | ~20,000 genes, defined by an annotation | 185,581 peaks, defined by  peak calling |
|  | A gene is a gene in every dataset | A peak exists only because it was called in this dataset |
|  | Tens to thousands of UMIs | 0, 1 or 2 fragments — two copies of the genome |
|  | Roughly 90 percent zeros | Roughly 97 percent zeros |
|  | Not expressed, or not captured | Closed, or open but not sampled — indistinguishable |
|  | ~20,000 tests | ~185,581 tests, an order of magnitude more |
|  | Gene symbol, immediately meaningful | , meaningless until annotated |
|  | The gene body | Mostly distal enhancers, far from any gene |

Three consequences follow from that table, and they shape every decision in this tutorial.

**A single cell cannot vote.** A diploid cell has two copies of each locus, so the count at any peak is capped at 2 in practice. There is no such thing as a “highly accessible peak in this cell.” All quantitative information lives in the *proportion of cells* in which a peak is observed, which is why DA analysis is fundamentally a comparison of detection rates.

**Sequencing depth is the dominant nuisance variable.** A cell with 8,000 fragments detects more peaks than a cell with 2,000 fragments, everywhere, for purely technical reasons. If your two conditions differ in median depth — and they usually do — an uncorrected test will hand you thousands of “differential” peaks that are nothing but depth. Every method below controls for this, and Step 5 shows you exactly how large the problem is in this dataset.

**A peak is not a gene.** The DA result is a list of intervals. Converting intervals to biology requires an extra assignment step, and that step is the single largest source of over-interpretation in the field. We handle it explicitly in the pathway section.

### How scATAC-seq DA Compares to Bulk ATAC-seq Differential Binding

The NGS101 bulk ATAC-seq tutorial [Part 2: Differential Binding Analysis using DiffBind](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-2-differential-binding-analysis-using-diffbind/) solves the same biological problem with a different data shape. Understanding the relationship makes the single-cell version much less mysterious.

**Unit of observation****Cell types****Replicates****Counts per peak****Statistical model***or***Main confounder****Number of tests**

|  | Bulk ATAC-seq (DiffBind) | scATAC-seq (this tutorial) |
| --- | --- | --- |
|  | One library per biological sample | One library per sample, but thousands of cells inside it |
|  | Averaged together, unresolvable | Separated — you analyze one at a time |
|  | 3 to 6 samples per group, typically | The same 2 to 6 samples per group, still |
|  | Hundreds to thousands of reads | 0 to 2 fragments per cell |
|  | Negative binomial (DESeq2 or edgeR) | Logistic regression per cell,  negative binomial after aggregation |
|  | Library size, batch | Per-cell depth, cell number imbalance, donor |
|  | One test set | One test set per cell type |

The most useful thing to notice is the row on replicates. **Going single-cell does not increase your number of biological replicates.** You still have four donors in this dataset; you simply learned how to look at each cell type separately within them. That is the entire conceptual foundation for the pseudobulk approach introduced later: if you sum a cell type’s fragments back up within each donor, you recover a bulk ATAC-seq experiment for that cell type, and you can hand it straight to DESeq2 exactly as DiffBind would.

### What the Benchmark Literature Says About DA Methods

This is not a matter of taste. In 2024, Teo, Squair, Courtine and Skinnider published a registered-report benchmark of eleven DA methods in *Nature Communications*, using scATAC-seq datasets with matched bulk ATAC-seq or matched RNA from the same cells as ground truth. Two of their findings should change how you run this analysis.

First, in **null comparisons** — randomly splitting cells from the *same* biological condition into two artificial groups, where the correct answer is zero DA peaks — the three most widely used methods produced a median of 4,664 (Wilcoxon), 2,505 (logistic regression on clusters) and 8,721 (logistic regression on peaks) false discoveries per cell type. Methods that first aggregated cells into pseudobulk profiles never produced a median above 9.

Second, across the accuracy experiments, **pseudobulk methods consistently ranked at the top**, and the authors recommend them as the first-choice approach for scATAC-seq DA analysis.

A third finding is worth memorizing because it contradicts common practice: filtering peaks by log-fold change did not improve accuracy relative to discarding an equal number of peaks at random, and consistently *increased* the number of false discoveries. Do not set an aggressive `logfc.threshold`.

> 

**So why does this tutorial still run the single-cell logistic regression?** Because it is what Signac’s own vignettes use, what most published papers used, and what you will be asked to reproduce or review. You should be able to run it, read it, and state its limitations precisely. We run both methods, compare them side by side, and let the disagreement teach the lesson. That is more useful than pretending the popular method does not exist.

### The Part 5 Workflow

```r
atac_annotated.rds (Part 4)
        |
        v
[1] Choose one cell type, split by condition
        |
        +--> check cells per sample, per condition
        +--> check sequencing depth per condition   <-- the confounder
        |
        v
[2] Single-cell DA          [3] Pseudobulk DA
    FindMarkers(LR,             sum counts per donor
    latent.vars = depth)        DESeq2, design = ~ condition
        |                           |
        +-----------+---------------+
                    v
[4] Compare the two result sets -- overlap and direction
                    |
        +-----------+-----------+
        v                       v
[5] TF motif enrichment   [6] Peak-to-gene, then GO enrichment
    FindMotifs(),             ClosestFeature() + clusterProfiler
    GC-matched background     with a peak-derived universe
                    |
                    v
[7] Visualization: volcano, coverage, heatmap, motif and GO plots
```

---

## ⚙️ Setting Up the R Environment

### Adding the Four New Packages to Your Pixi Environment

You are continuing in the same Pixi project used since Part 2. If you have not built it, follow [Setting Up a Single-Cell Analysis Environment with Pixi](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/) first.

Only four packages are new in Part 5. Everything else — Signac, Seurat, ggplot2, dplyr, patchwork, ggrepel — is already installed and already loaded in your Part 4 session.

```bash
# Run from a login node with internet access, in your Pixi project directory
cd /projects/mylab/shared/scatac-analysis

# DESeq2      -- negative binomial testing on pseudobulk peak counts
# clusterProfiler -- GO over-representation analysis
# org.Hs.eg.db    -- human gene symbol / Entrez / GO mappings
# enrichplot      -- publication-ready plots for clusterProfiler results
pixi add bioconductor-deseq2 bioconductor-clusterprofiler \
         bioconductor-org.hs.eg.db bioconductor-enrichplot
```

### The Bioconda Stub Problem, Again — And This Time There Are Three

Part 4 documented a trap that will bite you again here, harder. Bioconda ships large Bioconductor **data** packages as roughly 10 KB placeholder packages whose real content is downloaded by a post-link shell script at install time. Pixi does not execute post-link scripts. The package therefore appears installed, and R cannot find it.

Part 5 pulls in two of these, and they surface one at a time as you work down your `library()` calls:

`org.Hs.eg.db``there is no package called 'org.Hs.eg.db'``GO.db``clusterProfiler``there is no package called 'GO.db'`

| Package | Pulled in by | Error you see |
| --- | --- | --- |
|  | requested directly |  |
|  |  |  |

Fixing them individually just reveals the next one, so repair the whole environment in one pass. First see what is waiting:

```bash
# Enter the environment
pixi shell

# Every stub that Pixi installed without running its download script
ls -1 $CONDA_PREFIX/bin/.*-post-link.sh
```

Then run all of them, with `PREFIX` set to the Pixi environment prefix, which is what the scripts expect:

```r
# Run from a LOGIN node. These scripts download from Bioconductor, and on a
# compute node with no internet access they fail without an obvious error.
for f in $CONDA_PREFIX/bin/.*-post-link.sh; do
  echo "== $f"
  PREFIX=$CONDA_PREFIX bash "$f" || echo "FAILED: $f"
done

# Verify from R. You want TRUE for all three.
Rscript -e 'sapply(c("org.Hs.eg.db","GO.db","GOSemSim"), requireNamespace, quietly = TRUE)'

# The test that actually matters: the packages load together.
Rscript -e 'library(clusterProfiler); library(org.Hs.eg.db); library(enrichplot)'
```

Launch R inside the environment as before:

```
pixi run R
```

### Loading Libraries and Creating the Part 5 Directory Structure

```r
#-----------------------------------------------
# STEP 1: Load the packages used in this tutorial
#-----------------------------------------------

library(Signac)
library(Seurat)
library(DESeq2)
library(clusterProfiler)
library(org.Hs.eg.db)
library(enrichplot)
library(ggplot2)
library(ggrepel)
library(patchwork)
library(dplyr)

# Reproducibility: background peak selection in FindMotifs() is stochastic.
# The same seed is used across every part of this series.
set.seed(1234)

# Subsetting and pseudobulk aggregation move large sparse matrices around.
options(future.globals.maxSize = 60 * 1024^3)
```

Part 5 output goes in its own directory, a sibling of `integrated/` and `annotation/`, following the convention established in Part 3.

```r
#-----------------------------------------------
# STEP 2: Define and create the project directories
#-----------------------------------------------

base_dir  <- "/projects/mylab/shared/scatac_tutorial"
annot_dir <- file.path(base_dir, "annotation")
da_dir    <- file.path(base_dir, "differential")

# Subdirectories: objects for RDS files, plots for figures,
# tables for the CSV files that become supplementary material.
obj_dir   <- file.path(da_dir, "objects")
plot_dir  <- file.path(da_dir, "plots")
table_dir <- file.path(da_dir, "tables")

invisible(lapply(
  c(da_dir, obj_dir, plot_dir, table_dir),
  dir.create, recursive = TRUE, showWarnings = FALSE
))
```

Your project tree now looks like this:

```
/projects/mylab/shared/scatac_tutorial/
├── results/               # Cell Ranger ATAC output (Part 1)
├── downstream_qc/         # per-sample QC objects (Part 2)
├── integrated/            # consensus peaks, integrated object (Part 3)
├── annotation/            # annotated object and evidence tables (Part 4)
│   └── objects/
│       └── atac_annotated.rds
└── differential/          # this tutorial
    ├── objects/
    ├── plots/
    └── tables/
```

---

## 📦 Example Data: The Annotated Object from Part 4

### Loading and Inspecting the Object

```r
#-----------------------------------------------
# STEP 3: Load the annotated object produced by Part 4
#-----------------------------------------------

atac <- readRDS(file.path(annot_dir, "objects", "atac_annotated.rds"))

# Peaks are the assay we test. The ACTIVITY assay from Part 4 stays untouched.
DefaultAssay(atac) <- "peaks"

# Confirm the object is what Part 4 saved.
dim(atac)
table(atac$cell_type, atac$condition)
```

**Output:**

```
[1] 185581  30571

                       Healthy Severe
  B cell                  1182   1534
  CD14 monocyte           3382   1831
  CD16 monocyte           1298    287
  CD4 T memory            2495    653
  CD4 T naive             2658   1170
  CD8 T effector          4608    718
  CD8 T naive             1578    112
  Dendritic cell            54     33
  Low quality              641    222
  NK cell                 1733    198
  NK/T cell unresolved    2593   1420
  Unresolved                91     80
```

> 

**Reading this table.** This is the design matrix for every comparison in Part 5, and it deserves more than a glance.

**Not every cell type is testable.** `Dendritic cell` has 54 and 33 cells. `CD8 T naive` has 112 severe cells. The benchmark by Teo et al. found that DA accuracy begins to saturate above roughly 300 cells per condition and that useful analysis is possible from about 50 to 100 cells per condition — but “possible” is not “reliable” when the two groups are also unbalanced. Testing dendritic cells here would produce a result you could not defend.

**Three labels are not cell types.** `Low quality`, `Unresolved` and `NK/T cell unresolved` must be excluded from any biological claim. They are honest bookkeeping from Part 4, not populations.

**The composition itself is a finding.** CD8 T effectors drop from 4,608 to 718 cells and NK cells from 1,733 to 198, while B cells rise from 1,182 to 1,534. Lymphopenia with relative myeloid expansion is the textbook peripheral picture of severe COVID-19. That is a *compositional* difference, and DA analysis does not test it — DA asks whether the cells that remain have changed their chromatin, not whether there are fewer of them. Keep the two questions separate.

### Choosing a Cell Type and Checking the Design

We will run the full workflow on **CD14 monocytes**: the largest well-annotated population, present in useful numbers in both conditions, and the compartment most consistently implicated in severe COVID-19.

```r
#-----------------------------------------------
# STEP 4: Subset to one cell type and inspect the design
#-----------------------------------------------

target_type <- "CD14 monocyte"

# subset() keeps all assays, reductions and the fragment file links intact.
mono <- subset(atac, subset = cell_type == target_type)

# Condition becomes the identity for every test below.
Idents(mono) <- "condition"

# How many cells per donor, and per condition?
table(mono$sample_id, mono$condition)
```

**Output:**

```
           Healthy Severe
  healthy1     807      0
  healthy2    2575      0
  severe1        0   1323
  severe2        0    508
```

> 

**Reading this table, because it constrains everything that follows.**

**You have two biological replicates per group, not 5,213.** The 5,213 cells come from four donors. Cells within a donor share that donor’s genetics, batch, library preparation and clinical course, so they are not independent observations. This single fact is why the two statistical approaches below will disagree so violently.

**The donors are unbalanced.** `healthy2` contributes 2,575 of the 3,382 healthy cells and `severe1` contributes 1,323 of the 1,831 severe cells. A single-cell test that treats cells as replicates is, in practice, comparing donor `healthy2` to donor `severe1`. Any difference specific to those two individuals will appear as a disease effect.

**Two replicates per group is the minimum DESeq2 will accept**, and it is genuinely underpowered. We proceed because this is what public datasets look like, and because a correctly executed underpowered analysis is far more useful than an overpowered wrong one. Design your own experiments with at least three, preferably five or more, donors per group — the benchmark showed that false discoveries fall as replicate number rises, and rise as cell number rises.

### The Depth Confounder You Must Quantify First

```bash
#-----------------------------------------------
# STEP 5: Quantify the sequencing depth difference between conditions
#-----------------------------------------------

# nCount_peaks is the total fragments in peaks per cell -- the technical
# covariate that drives apparent differential accessibility.
tapply(mono$nCount_peaks, mono$condition, median)
tapply(mono$nCount_peaks, mono$sample_id, median)

# Detection rate: how many distinct peaks are observed per cell.
tapply(mono$nFeature_peaks, mono$condition, median)
```

**Output:**

```
Healthy  Severe
   4446    4622

healthy1 healthy2  severe1  severe2
    5550     4232     4784     4221

Healthy  Severe
 3952.5  4148.0
```

> 

**Reading this output, which is better news than it could have been.** There is no large systematic depth gap between the groups to distort the comparison.

**The per-donor row is where the real variation lives.** Across all four, the deepest library is a healthy donor and the shallowest is a severe donor, so depth does not track condition.

This is the pattern you hope to see, and it still carries a lesson: **donor-level technical variation exceeds condition-level variation**, which is precisely the argument for treating the donor as the unit of replication. Had the numbers come out the other way — severe systematically shallower — an uncorrected test would have reported thousands of peaks “closing” in disease that were nothing but missing fragments. You cannot know which situation you are in without looking.

**Report these numbers in your methods section.** Reviewers who know scATAC-seq will ask, and a balanced table like this one is a point in your favor.

---

## 🔬 Differential Accessibility Analysis

### Method 1: The Single-Cell Logistic Regression Test

Signac’s recommended approach passes `test.use = "LR"` to `FindMarkers()` with the per-cell fragment count as a latent variable. Logistic regression models the probability that a peak is detected in a cell as a function of condition *while holding depth constant*, which is the correct way to handle the confounder identified in Step 5.

```r
#-----------------------------------------------
# STEP 6: Single-cell differential accessibility (logistic regression)
#-----------------------------------------------

DefaultAssay(mono) <- "peaks"

# ident.1 = "Severe" makes positive log2FC mean "more open in severe disease".
# min.pct = 0.05 skips peaks detected in under 5 percent of cells in both
#   groups; the Signac default of 0.1 was designed for scRNA-seq and is too
#   strict for sparse chromatin data.
# logfc.threshold = 0 keeps every tested peak. Teo et al. (2024) showed that
#   fold-change filtering does not improve accuracy and increases false
#   discoveries, so filter on the adjusted p-value afterwards instead.
# latent.vars = "nCount_peaks" is the depth correction.
da_sc <- FindMarkers(
  object          = mono,
  ident.1         = "Severe",
  ident.2         = "Healthy",
  test.use        = "LR",
  latent.vars     = "nCount_peaks",
  min.pct         = 0.05,
  logfc.threshold = 0
)

# Move peak coordinates from rownames into a column for later joins.
da_sc$peak <- rownames(da_sc)

nrow(da_sc)
sum(da_sc$p_val_adj < 0.05)
table(sign(da_sc$avg_log2FC[da_sc$p_val_adj < 0.05]))
```

**Output:**

```
[1] 27167

[1] 6445

  -1    1
2943 3502
```

> 

**Expect one to three hours on a compute node.** Logistic regression fits one model per peak per cell. Submit it as a batch job rather than running it interactively, and save the result immediately — you do not want to repeat it.

**Reading this output.** Only 27,167 of the 185,581 peaks — 15 percent — are detected in at least 5 percent of cells in one of the groups. That is a useful reminder of how sparse this data is: most consensus peaks called across the whole experiment are barely present in any single cell type.

Of those 27,167 tested peaks, 6,445 come back significant at a 5 percent FDR. **That is 24 percent of everything tested.** Hold the number in mind. When Method 2 returns 96, the gap is the lesson of this tutorial, not a bug.

**The direction split is nearly even**, with a slight excess of opening (3,502 up, 2,943 down), consistent with the small depth advantage the severe cells had in Step 5. Logistic regression reduces that artifact but does not erase it, which is why you check depth before you interpret direction.

### Annotating Peaks With Their Nearest Gene

A peak coordinate is not interpretable. `ClosestFeature()` attaches the nearest annotated gene and the distance to it, using the annotation carried inside the object since Part 3.

```r
#-----------------------------------------------
# STEP 7: Annotate the significant peaks with their nearest gene
#-----------------------------------------------

sig_sc <- da_sc[da_sc$p_val_adj < 0.05, ]

# ClosestFeature returns gene_name, gene_biotype, type and distance (bp).
# distance == 0 means the peak overlaps the feature.
near_sc <- ClosestFeature(mono, regions = sig_sc$peak)

# Join annotation onto the statistics by peak coordinate.
sig_sc <- merge(sig_sc, near_sc,
                by.x = "peak", by.y = "query_region",
                all.x = TRUE)

# Sort so the most strongly opened peaks come first.
sig_sc <- sig_sc[order(-sig_sc$avg_log2FC), ]

head(sig_sc[, c("peak", "avg_log2FC", "p_val_adj", "gene_name", "distance")], 5)

# How far are these regulatory elements from the genes we will assign them to?
table(cut(sig_sc$distance,
          breaks = c(-1, 0, 2000, 10000, 100000, Inf),
          labels = c("overlapping", "<2kb", "2-10kb", "10-100kb", ">100kb")))
```

**Output:**

```
                         peak avg_log2FC    p_val_adj gene_name distance
3133  chr19-45475104-45475961   4.683436 9.602303e-57      FOSB        0
759   chr10-47256468-47257308   4.575110 1.757973e-68     GDF10    42888
1098  chr11-62814963-62815827   4.560575 1.796005e-36      STX5        0
5289 chr6-169654381-169655367   4.214881 2.114389e-26                  0
1518  chr12-68367656-68368560   4.167105 7.001112e-80      MDM1    35274

overlapping        &lt;2kb      2-10kb    10-100kb      >100kb
       4418         206         443        1058         320
```

> 

**Reading this table.** Most significant peaks — 4,418 of 6,445, about 69 percent — overlap an annotated feature directly. That is higher than the textbook expectation for ATAC-seq peaks, and it reflects the filtering chain: peaks detected in at least 5 percent of cells are skewed towards promoters, which are the most consistently accessible regions in any cell type. The distal peaks are still there, though: 1,058 sit 10 kb to 100 kb away and 320 are more than 100 kb from anything annotated.

**Notice the blank gene name in row four.** `chr6-169654381-169655367` overlaps a feature whose `gene_name` is an empty string — an annotated transcript with no assigned symbol, which `EnsDb` carries plenty of. It is not an error and not a missing value, so `is.na()` will not catch it. Any downstream filtering has to test `gene_name != ""` as well, which is why Step 13 does both.

**“Nearest gene” is a heuristic, not a fact.** A distal enhancer physically loops to whichever promoter it contacts, and that promoter is frequently not the nearest one. Assigning `chr10-47256468-47257308` to *GDF10* because *GDF10* is 43 kb away is a hypothesis, and one that looks especially shaky given that *GDF10* is a bone morphogenetic factor with no obvious monocyte role. Treat every gene-level statement downstream as provisional, and if the claim matters, verify it with co-accessibility (`LinkPeaks()`), Hi-C, or the ATAC-plus-RNA integration approach in [bulk ATAC-seq Part 4](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-4-atac-seq-and-rna-seq-integration/).

### Method 2: Pseudobulk Aggregation and DESeq2

Now the approach the benchmark recommends. The idea is simple: sum the raw fragment counts of all CD14 monocytes within each donor. Four donors give four columns. The 97-percent-sparse single-cell matrix becomes a dense bulk-style count matrix, and the unit of observation becomes the donor, which is what it should have been all along.

```r
#-----------------------------------------------
# STEP 8: Aggregate raw counts to one profile per donor
#-----------------------------------------------

# method = "aggregate" sums counts rather than averaging them, and
# layer = "counts" uses raw fragments rather than TF-IDF normalized values.
# Both matter: DESeq2 models raw counts and will misbehave on anything else.
pb_list <- PseudobulkExpression(
  object   = mono,
  assays   = "peaks",
  layer    = "counts",
  method   = "aggregate",
  group.by = "sample_id"
)

# DESeq2 requires an integer matrix.
pb <- round(as.matrix(pb_list[["peaks"]]))

# Build the sample table from the object's own metadata rather than typing it,
# so the condition labels can never be misaligned with the columns.
samp_meta <- unique(mono@meta.data[, c("sample_id", "condition")])
rownames(samp_meta) <- samp_meta$sample_id
coldata <- samp_meta[colnames(pb), , drop = FALSE]
coldata$condition <- factor(coldata$condition, levels = c("Healthy", "Severe"))

dim(pb)
colSums(pb)
coldata
```

**Output:**

```
[1] 185581      4

healthy1 healthy2  severe1  severe2
 4838159 11979419  7019769  2363335

         sample_id condition
healthy1  healthy1   Healthy
healthy2  healthy2   Healthy
severe1    severe1    Severe
severe2    severe2    Severe
```

> 

**Reading this output.** Four columns, 185,581 rows — a bulk ATAC-seq experiment for CD14 monocytes, reconstructed from single-cell data. `PseudobulkExpression()` works on a `ChromatinAssay` without complaint, because `ChromatinAssay` extends Seurat’s `Assay` class and inherits its aggregation method.

Column sums span five-fold, from 2.36 million fragments in `severe2` to 11.98 million in `healthy2`. That is the product of cell number and per-cell depth compounding: `healthy2` contributed both the most cells and near-median depth, while `severe2` contributed the fewest cells at the lowest depth. DESeq2’s median-of-ratios size factors are designed to normalize exactly this, which is why you hand it raw sums rather than anything pre-normalized.

**If you know Seurat’s differential expression vignette, you will have seen `AggregateExpression()` used for this step.** It calls the same underlying machinery with `method` fixed to `"aggregate"`, but its documented arguments do not include `layer`, so the choice of raw counts over normalized values is implicit rather than visible in the call. `PseudobulkExpression()` is used here because both decisions appear in the code, which matters when someone else has to check that you did not hand DESeq2 a normalized matrix.

**Check the column order.** `PseudobulkExpression()` builds column names from the grouping variable, and Seurat sanitizes names containing underscores or leading digits. Indexing `samp_meta` by `colnames(pb)` rather than assuming an order protects you from a silent condition swap — the most damaging error possible in this analysis.

```r
#-----------------------------------------------
# STEP 9: Test for differential accessibility with DESeq2
#-----------------------------------------------

# Drop peaks with almost no support. Requiring at least 10 counts in at
# least 2 samples removes peaks that carry no information and reduces the
# multiple testing burden.
keep <- rowSums(pb >= 10) >= 2

dds <- DESeqDataSetFromMatrix(
  countData = pb[keep, ],
  colData   = coldata,
  design    = ~ condition
)

dds <- DESeq(dds)

res_pb <- results(dds, contrast = c("condition", "Severe", "Healthy"), alpha = 0.05)
res_pb <- as.data.frame(res_pb)
res_pb$peak <- rownames(res_pb)
res_pb <- res_pb[!is.na(res_pb$padj), ]

sum(keep)
sum(res_pb$padj < 0.05)
summary(results(dds, contrast = c("condition", "Severe", "Healthy"), alpha = 0.05))
```

**Output:**

```
[1] 77570

[1] 96

out of 77570 with nonzero total read count
adjusted p-value &lt; 0.05
LFC > 0 (up)       : 46, 0.059%
LFC &lt; 0 (down)     : 50, 0.064%
outliers [1]       : 0, 0%
low counts [2]     : 24063, 31%
(mean count &lt; 18)
```

> 

**Reading this output, and this is the central lesson of Part 5.** The single-cell test called 6,445 peaks. The pseudobulk test, on exactly the same cells, calls 96 — a 67-fold difference. Split almost evenly, 46 opening and 50 closing. The difference is not that pseudobulk is insensitive: it is that the single-cell test counted 5,213 cells as 5,213 independent replicates when the experiment contains four donors.

This is **pseudoreplication**. Squair et al. (2021) demonstrated it for scRNA-seq differential expression, and Teo et al. (2024) confirmed it for scATAC-seq differential accessibility: methods that treat cells as replicates produce thousands of significant results even when there is nothing to find. With two donors per group, the honest answer is that only very large, very consistent differences can be detected. A few hundred peaks is what your experimental design actually supports.

**What to do with this in a paper.** Report the pseudobulk result as your primary analysis. If you also report the single-cell result, describe it as an exploratory ranking rather than a set of discoveries, and say how many donors you had.

### Comparing the Two Result Sets

Do not choose blindly between the two. Compare them, because the comparison is itself a quality-control check.

```r
#-----------------------------------------------
# STEP 10: How much do the two methods agree?
#-----------------------------------------------

both <- merge(
  da_sc[, c("peak", "avg_log2FC", "p_val_adj")],
  res_pb[, c("peak", "log2FoldChange", "padj")],
  by = "peak"
)

# Direction agreement across all commonly tested peaks.
mean(sign(both$avg_log2FC) == sign(both$log2FoldChange))

# Effect size correlation, Spearman to avoid outlier dominance.
cor(both$avg_log2FC, both$log2FoldChange, method = "spearman")

# Of the peaks pseudobulk calls significant, how many did the
# single-cell test also call?
sc_sig <- both$peak[both$p_val_adj < 0.05]
pb_sig <- both$peak[both$padj < 0.05]
length(intersect(sc_sig, pb_sig)) / length(pb_sig)
```

**Output:**

```
[1] 0.9252467

[1] 0.9712003

[1] 1
```

> 

**Reading this output, which is the most reassuring result in the tutorial.** Directions agree for 93 percent of commonly tested peaks and the effect sizes correlate at rho = 0.97. The two methods are not measuring different things. They produce almost the same ranking of the same peaks.

**The third number is 1: every single pseudobulk hit is also a single-cell hit.** The 96 peaks are a strict subset of the 6,445. That is the cleanest possible statement of what separates the two approaches — not the biology they see, but where they draw the significance line. The single-cell test is a well-ordered ranking with a badly calibrated threshold; pseudobulk takes the same ranking and cuts it where four donors can actually support a claim.

**This also tells you how to use the single-cell result honestly.** It is a ranking, so use it as one: to prioritize loci for follow-up, or to break ties among the pseudobulk hits. Do not report its 6,445 peaks as discoveries.

**If direction agreement drops below about 0.6, stop and investigate.** That pattern usually means one donor dominates one condition and the two methods are describing different comparisons.

### Transcription Factor Motif Enrichment on Differential Peaks

A list of regions is more interpretable when you ask which transcription factors bind there. `FindMotifs()` performs a hypergeometric test on the peak-by-motif matrix that `AddMotifs()` attached to this object in Part 4, comparing your DA peaks to a background set matched for GC content and peak width.

```r
#-----------------------------------------------
# STEP 11: Motif enrichment in peaks opened in severe disease
#-----------------------------------------------

# Split the pseudobulk hits by direction. Testing both together would
# cancel opposing regulatory programs and return nothing.
up_peaks   <- res_pb$peak[res_pb$padj < 0.05 & res_pb$log2FoldChange > 0]
down_peaks <- res_pb$peak[res_pb$padj < 0.05 & res_pb$log2FoldChange < 0]

# Feature-level metadata holds GC content and peak width, written by
# AddMotifs() in Part 4. Two chained [[ calls: mono[["peaks"]] pulls out the
# ChromatinAssay, and the empty [[]] on an assay returns its full
# meta.features data frame (supply a name instead to get one column).
meta_feat <- mono[["peaks"]][[]]

# The background must be peaks that are actually open in these cells, not
# all 185,581 peaks -- otherwise you measure cell-type identity, not disease.
open_peaks <- AccessiblePeaks(mono, idents = c("Healthy", "Severe"))

# MatchRegionStats draws a background matched to the query's sequence
# characteristics, which removes GC bias from the enrichment test.
bg_up <- MatchRegionStats(
  meta.feature  = meta_feat[open_peaks, ],
  query.feature = meta_feat[up_peaks, ],
  n             = 50000
)

motifs_up <- FindMotifs(object = mono, features = up_peaks, background = bg_up)

head(motifs_up[, c("motif.name", "fold.enrichment", "pvalue", "p.adjust")], 10)
```

**Output:**

```
Testing motif enrichment in 46 regions
          motif.name fold.enrichment       pvalue     p.adjust
MA0600.2        RFX2        5.522218 1.042517e-06 0.0006599133
MA0833.2        ATF4        2.806407 7.597021e-05 0.0240445729
MA0798.2        RFX3        2.986144 1.301904e-04 0.0274701672
MA0799.1        RFX4        3.456142 4.809206e-04 0.0752264846
MA0509.2        RFX1        2.710095 6.267763e-04 0.0752264846
MA0510.2        RFX5        3.605827 7.130472e-04 0.0752264846
MA1530.1      NKX6-3        2.644663 3.627687e-03 0.3280465124
MA0492.1 JUND(var.2)        2.255486 5.055260e-03 0.3920489822
MA0895.1      HMBOX1        3.215848 5.574156e-03 0.3920489822
MA0836.2       CEBPD        2.192550 6.343672e-03 0.4015544430
```

> 

**Reading this table, starting with what is significant and what is not.** Only three motifs clear a 5 percent FDR: `RFX2`, `ATF4` and `RFX3`. `RFX4`, `RFX1` and `RFX5` sit at p.adjust = 0.075, just outside. Everything from `NKX6-3` down is not significant, and reporting `CEBPD` at p.adjust = 0.40 as a finding would be straightforwardly wrong. With only 46 query regions there is very little to work with, and the table is honest about that.

**The signal that is there is a single family.** Five of the top six entries are RFX proteins. All RFX factors share a highly conserved 76-residue winged-helix DNA-binding domain that recognizes the same X-box motif, so they are not five findings — they are one, reported five times by five near-identical position weight matrices.

**And this is the clearest possible illustration of why motif families matter.** The RFX members have sharply different biology. `RFX5` is the DNA-binding subunit of the MHC class II enhanceosome, and loss-of-function mutations in it cause bare lymphocyte syndrome. `RFX2` and `RFX3`, the two that actually reached significance here, are best characterized as regulators of motile ciliogenesis. The assay cannot distinguish them, because the DNA they bind is the same. Writing “RFX2 drives the severe monocyte phenotype” would be unsupportable; writing “regions opened in severe disease are enriched for the X-box motif bound by the RFX family” is exactly what the data says.

**Given the cell type, the MHC class II link is the hypothesis worth testing** — monocyte HLA-DR downregulation is among the most reproducible findings in severe COVID-19 — but you would test it with expression data and with footprinting ([bulk ATAC-seq Part 3](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/)), not by picking the family member whose story you like.

**Always run the closing peaks too.** Repeat the block with `down_peaks` and a background matched to them. If the two lists return the same motifs, your background matching has failed and neither result is interpretable.

```r
#-----------------------------------------------
# STEP 12: Motif enrichment in peaks closed in severe disease
#-----------------------------------------------

bg_down <- MatchRegionStats(
  meta.feature  = meta_feat[open_peaks, ],
  query.feature = meta_feat[down_peaks, ],
  n             = 50000
)

motifs_down <- FindMotifs(object = mono, features = down_peaks, background = bg_down)

head(motifs_down[, c("motif.name", "fold.enrichment", "p.adjust")], 10)

# Sanity check: how many motifs appear in both directions?
length(intersect(
  motifs_up$motif.name[motifs_up$p.adjust < 0.05],
  motifs_down$motif.name[motifs_down$p.adjust < 0.05]
))
```

**Output:**

```
Testing motif enrichment in 50 regions
         motif.name fold.enrichment     p.adjust
MA0489.1 JUN(var.2)        3.055229 8.803895e-06
MA1141.1  FOS::JUND        3.046551 8.803895e-06
MA0478.1      FOSL2        3.044696 8.803895e-06
MA1128.1 FOSL1::JUN        3.012426 1.842833e-05
MA0835.2      BATF3        2.824540 2.272392e-05
MA0476.1        FOS        2.917933 2.272392e-05
MA0462.2  BATF::JUN        2.772233 2.488645e-05
MA1634.1       BATF        2.675884 8.795912e-05
MA1130.1 FOSL2::JUN        2.822960 2.371043e-04
MA0477.2      FOSL1        2.731174 3.645294e-04

[1] 0
```

> 

**Reading this table, which is much stronger than the opened-peak result.** Nineteen of the 633 tested motifs clear a 5 percent FDR, against three for the opened peaks, and the top ten do so by three to five orders of magnitude — from only 50 regions. Every one of them is AP-1 or an AP-1-family dimer: JUN, FOS, FOSL1, FOSL2, JUNB, JDP2 and the BATF group. Nineteen rows, one finding: the composite TRE site. Report it as “AP-1 family motifs,” not as nineteen transcription factors.

**The tail of that list is worth a second look.** Below the AP-1 entries sit `MAF::NFE2`, `NFE2L1` and `BACH1`, which are usually described as antioxidant response element factors rather than AP-1. They appear because the ARE contains a TRE-like core that the AP-1 matrices also match — the same degeneracy problem one family over. Three more rows, still not three more findings.

**The sanity check returns zero, and that is the number you want.** No motif is significantly enriched in both the opened and the closed sets. The GC-matched backgrounds are doing their job, and the two directions genuinely describe different regulatory programs — X-box regions opening, AP-1 regions closing. Had this returned a large overlap, both results would have been uninterpretable and the correct response would have been to rebuild the backgrounds.

**How this sits against the literature.** Brauns et al. (2022) profiled CD14 monocytes by ATAC-seq across acute and convalescent severe COVID-19 and found the recovering-patient signature characterized by *increased* accessibility at AP-1 and MAF loci, alongside a distinct transcriptomic and epigenomic state in acute patients that accounted for their functional refractoriness to TLR stimulation. A loss of AP-1 accessibility during acute severe disease is directionally consistent with that picture. It is a hypothesis your four donors are consistent with, not a result they establish — different cohort, different assay, different definition of severity.

---

## 🧬 Pathway Enrichment Analysis on Differentially Accessible Regions

### Why This Step Is Harder Than It Looks

Gene Ontology enrichment on ATAC-seq data has a failure mode that is easy to fall into and hard to notice. Two decisions determine whether the output means anything.

**The peak-to-gene assignment.** Covered in Step 7. Nearest-gene assignment is a hypothesis, and errors here propagate directly into the pathway result.

**The universe.** This is the one people get wrong. If you test your DA genes against *all human genes*, you are not asking “which pathways changed with disease” — you are asking “which pathways are near open chromatin in monocytes,” and the answer will always be immune system process, immune response, leukocyte activation, no matter what your data says. The universe must be **the genes assigned to the peaks you actually tested**, so that the enrichment is measured against your own assay’s baseline.

### Building the Gene List and the Correct Universe

```r
#-----------------------------------------------
# STEP 13: Convert peaks to genes, for both the query and the universe
#-----------------------------------------------

# Query: genes near peaks opened in severe disease. Restricting to 50 kb
# keeps assignments that are at least plausible; peaks further away are
# dropped rather than assigned to a gene they probably do not regulate.
near_up <- ClosestFeature(mono, regions = up_peaks)
genes_up <- unique(near_up$gene_name[near_up$distance < 50000])
genes_up <- genes_up[genes_up != "" & !is.na(genes_up)]

# Universe: genes near every peak that entered the DESeq2 test.
near_all <- ClosestFeature(mono, regions = res_pb$peak)
genes_bg <- unique(near_all$gene_name[near_all$distance < 50000])
genes_bg <- genes_bg[genes_bg != "" & !is.na(genes_bg)]

length(genes_up)
length(genes_bg)
```

**Output:**

```
[1] 30

[1] 14336
```

> 

**Reading this output, and being blunt about it.** 46 opened peaks collapse to just 30 unique gene symbols after the 50 kb cutoff and the empty-name filter. The universe holds 14,336 genes, which is the honest denominator: those are the genes your assay was in a position to detect a change near.

**Thirty genes is a very small input for over-representation analysis.** A hypergeometric test on 30 genes has almost no resolution: a single gene moving in or out of a term changes its p-value substantially, and any term you hit with two genes rests on two observations. Run it, read it as hypothesis-generating, and do not build a figure legend on it.

**The honest alternative here is to say so.** With 46 differentially accessible peaks, the motif result in Step 12 is the stronger line of evidence, because it uses all 50 closing regions directly as sequence rather than routing them through a lossy gene assignment first.

### GO Over-Representation Analysis with clusterProfiler

```r
#-----------------------------------------------
# STEP 14: GO biological process enrichment
#-----------------------------------------------

ego_up <- enrichGO(
  gene          = genes_up,
  universe      = genes_bg,
  OrgDb         = org.Hs.eg.db,
  keyType       = "SYMBOL",
  ont           = "BP",
  pAdjustMethod = "BH",
  pvalueCutoff  = 0.05,
  qvalueCutoff  = 0.20
)

# GO terms are highly redundant; simplify() merges terms whose gene sets
# overlap above the cutoff, keeping the most significant representative.
ego_up <- simplify(ego_up, cutoff = 0.7, by = "p.adjust", select_fun = min)

as.data.frame(ego_up)[1:8, c("Description", "GeneRatio", "BgRatio", "p.adjust")]
```

**Output:**

```
                                                           Description
GO:0050728                negative regulation of inflammatory response
GO:0002468          dendritic cell antigen processing and presentation
GO:0016045                                      detection of bacterium
GO:0002840 regulation of T cell mediated immune response to tumor cell
GO:0002424               T cell mediated immune response to tumor cell
GO:0032967        positive regulation of collagen biosynthetic process
GO:0032102        negative regulation of response to external stimulus
GO:0050777                      negative regulation of immune response
           GeneRatio   BgRatio  p.adjust
GO:0050728      4/26 141/12895 0.0379309
GO:0002468      2/26  14/12895 0.0379309
GO:0016045      2/26  14/12895 0.0379309
GO:0002840      2/26  15/12895 0.0379309
GO:0002424      2/26  16/12895 0.0379309
GO:0032967      2/26  16/12895 0.0379309
GO:0032102      5/26 332/12895 0.0379309
GO:0050777      4/26 189/12895 0.0379309
```

> 

**Reading this table, with three warnings that matter more than the terms themselves.**

**First, look at the denominators.** `GeneRatio` is out of 26, not 30 — four of your symbols have no GO biological process annotation at all and were silently dropped. `BgRatio` is out of 12,895 rather than 14,336, for the same reason. Report the number that was actually tested, not the number you supplied.

**Second, every p.adjust is identical: 0.0379309.** That is not a coincidence and not a bug. With 26 genes the underlying p-values are coarse and heavily tied, and Benjamini-Hochberg correction flattens a block of them onto a single adjusted value. Across the full result — 18 terms after `simplify()` — 17 share that exact value and one sits at 0.0487. The consequence is that **this list has no meaningful rank order**. Presenting “negative regulation of inflammatory response” as the top hit implies it beat the others; it did not.

**Third, five of the eight terms rest on two genes each.** `2/26` against `14/12895` is a large fold enrichment and almost no evidence. “Detection of bacterium” here means two genes. Terms of that shape are the single most common source of over-interpreted ATAC-seq pathway figures.

**What the table can support.** The terms that draw on four or five genes point one way — negative regulation of inflammatory response (4/26), negative regulation of immune response (4/26), negative regulation of response to external stimulus (5/26). A dampening rather than an activating signature, coherent with the loss of AP-1 accessibility in Step 12, AP-1 being a core driver of inflammatory gene induction in myeloid cells. Two independent lines of evidence pointing the same direction is worth a sentence in a discussion section. It is not worth a claim in an abstract.

**Cross-checking the two lines of evidence is the point of running both.** Motifs come from DNA sequence; GO terms come from gene proximity. When they agree, the story is stronger than either alone. When they disagree, one of the two assignments is wrong — most often the peak-to-gene step.

**If nothing had been significant, that would also have been a result.** With 30 input genes it is a common and legitimate outcome. Report it. Do not switch to the larger gene list from the single-cell test to populate the figure, because those extra genes come from peaks your design cannot support.

**A more rigorous alternative worth knowing about.** GREAT (McLean et al., 2010) models enrichment over *genomic regions* rather than genes, assigning each gene a regulatory domain and using a binomial test over the genome. This handles the distal-enhancer problem more honestly than nearest-gene plus a hypergeometric test. The `rGREAT` Bioconductor package provides an R interface. We use clusterProfiler here because it is the same tool you already know from [bulk RNA-seq Part 5: From DEGs to Pathways](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/), which keeps the learning curve flat — but if regulatory-region enrichment is central to your paper, GREAT is the better model.

---

## 📊 Visualization

### The Volcano Plot: Effect Size Against Significance

```r
#-----------------------------------------------
# STEP 15: Volcano plot of the pseudobulk result
#-----------------------------------------------

# Attach nearest-gene labels so the plot can be annotated with symbols.
volc <- merge(res_pb,
              near_all[, c("query_region", "gene_name", "distance")],
              by.x = "peak", by.y = "query_region", all.x = TRUE)

volc$status <- "Not significant"
volc$status[volc$padj < 0.05 & volc$log2FoldChange > 0] <- "Opened in severe"
volc$status[volc$padj < 0.05 & volc$log2FoldChange < 0] <- "Closed in severe"

# Label only the ten strongest hits in each direction to keep the plot legible.
# Require the gene to be within 50 kb and to have a symbol, so the plot does
# not label a peak with a gene that sits 100 kb away (see the note below).
lab <- volc[volc$padj < 0.05 &
            !is.na(volc$gene_name) & volc$gene_name != "" &
            volc$distance < 50000, ]
lab <- rbind(head(lab[order(-lab$log2FoldChange), ], 10),
             head(lab[order(lab$log2FoldChange), ], 10))

p_volcano <- ggplot(volc, aes(x = log2FoldChange, y = -log10(padj), color = status)) +
  geom_point(size = 0.6, alpha = 0.5) +
  geom_hline(yintercept = -log10(0.05), linetype = "dashed", color = "grey40") +
  geom_text_repel(data = lab, aes(label = gene_name),
                  size = 3, max.overlaps = 25, show.legend = FALSE) +
  scale_color_manual(values = c("Opened in severe" = "#C0392B",
                                "Closed in severe" = "#2E86AB",
                                "Not significant"  = "grey80")) +
  labs(x = "log2 fold change (Severe / Healthy)",
       y = "-log10 adjusted p-value",
       title = paste0("Differential accessibility in ", target_type),
       subtitle = "Pseudobulk DESeq2, 2 donors per group",
       color = NULL) +
  theme_classic(base_size = 12)

ggsave(file.path(plot_dir, "01_volcano_pseudobulk.png"), p_volcano,
       width = 8, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/01_volcano_pseudobulk.png?resize=1024%2C768&ssl=1)

> 

**Reading this figure, which is mostly grey and should be.** 96 colored points out of 53,507 plotted. That is the honest visual impression of a four-donor experiment. Resist the urge to make it look busier by switching to the single-cell result or coloring on nominal p-values — the sparseness *is* the finding, and a reviewer reads it correctly in two seconds.

With two donors per group, only very large effects clear the FDR threshold, so the plot has a hollow centre and two distant wings rather than the continuous cone you see in a well-powered bulk experiment. If your volcano looks like this, do not describe the result as a list of subtle regulatory changes.

**Large fold changes here come with a warning.** The 96 significant peaks have a median `baseMean` of 62 counts summed across four libraries. Fold changes computed on counts that small are unstable, and the biggest ones are the least trustworthy, not the most. Rank by adjusted p-value when you choose loci to follow up, not by fold change.

**The labels are nearest-gene assignments and need a distance filter.** The code above requires the gene to be within 50 kb, matching the cutoff used for the enrichment analysis. Without it, `geom_text_repel` will happily print a gene symbol next to a peak 100 kb away — see the next section for exactly that failure.

### Coverage Plots: Looking at the Raw Signal

Statistics can be wrong. Fragment pileups cannot. Always look at your top hit directly before you build an argument on it.

```r
#-----------------------------------------------
# STEP 16: Coverage plot of the top differential region by condition
#-----------------------------------------------

# The most strongly opened significant peak.
top_hit  <- volc[volc$peak %in% up_peaks, ]
top_hit  <- top_hit[order(-top_hit$log2FoldChange), ]
top_peak <- top_hit$peak[1]
top_gene <- top_hit$gene_name[1]

# Plot the PEAK coordinates, not the gene name. The nearest gene can be far
# outside the peak's neighbourhood, in which case a gene-centred window shows
# a locus that was never differential -- see the note below.
p_cov <- CoveragePlot(
  object            = mono,
  region            = top_peak,
  group.by          = "condition",
  extend.upstream   = 10000,
  extend.downstream = 10000,
  annotation        = TRUE,
  peaks             = TRUE
)

ggsave(file.path(plot_dir, paste0("02_coverage_", top_peak, ".png")), p_cov,
       width = 9, height = 5, dpi = 300)

# The same locus split by donor rather than by condition -- the check that
# reveals whether one individual is driving the entire result.
p_cov_donor <- CoveragePlot(
  object            = mono,
  region            = top_peak,
  group.by          = "sample_id",
  extend.upstream   = 10000,
  extend.downstream = 10000
)

ggsave(file.path(plot_dir, paste0("03_coverage_", top_peak, "_by_donor.png")),
       p_cov_donor, width = 9, height = 6, dpi = 300)

# Confirm the peak really is inside the plotted window before you trust
# the figure. This should print a small number, not a six-figure one.
top_hit[1, c("peak", "gene_name", "distance", "log2FoldChange")]
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/02_coverage_chr7-12805615-12806434.png?resize=1024%2C569&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/03_coverage_chr7-12805615-12806434_by_donor.png?resize=1024%2C683&ssl=1)

> 

**Reading the condition panel.** The peak in the middle of the window is present in the severe track and absent from the healthy track. The `Peaks` bar underneath confirms you are looking at the tested region, and the empty `Genes` panel across the full 21 kb window is the visual proof that the *ARL4A* label attached to this peak refers to something far outside the frame.

**Reading the donor panel, which is the check that decides whether this hit survives.** Both healthy donors are flat at the peak. Both severe donors have signal there. The direction is consistent across individuals, which is what you need before writing about a locus — a single donor producing the whole effect would have been disqualifying.

**The magnitudes are not balanced, though.** `severe1` shows a modest peak and `severe2` a tall one, and `severe2` also carries more background everywhere in the window. `severe2` is the donor with the fewest cells (508) and the lowest median depth, so its normalized track is the noisiest of the four. The honest summary is that the effect is present in both severe donors and its size is dominated by one of them.

**This is exactly the judgement two replicates cannot make for you.** Consistent direction across two donors is encouraging; it is not the same as a demonstrated effect size. Phrase the result as “accessible in both severe donors and not in either healthy donor at this locus,” and let the reader see the figure.

### The Mistake This Section Exists to Prevent

An earlier version of this tutorial passed `region = top_gene` to `CoveragePlot()`, because plotting a gene symbol makes for a more readable figure than plotting coordinates. On this dataset that produced the following two figures, and both are wrong in a way that is almost impossible to notice.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/02_coverage_ARL4A.png?resize=1024%2C569&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/03_coverage_ARL4A_by_donor.png?resize=1024%2C683&ssl=1)

> 

**What went wrong.** The most strongly opened peak is `chr7-12805615-12806434`, with a log2 fold change of 5.4. Its nearest annotated gene is *ARL4A* — but *ARL4A* sits near chr7:12,686,000, more than 100 kb away. Passing the gene symbol plotted a window around *ARL4A*, which does not contain the differential peak at all.

**That is why the tracks look identical.** They are identical. The figure is a faithful picture of the *ARL4A* promoter, which is equally accessible in both conditions and in all four donors, and it has nothing to do with the statistical result it was captioned as illustrating. Had this gone into a paper, the sanity check would have silently confirmed a locus nobody tested.

**The general lesson.** The gene symbol attached to a peak is an annotation, not a location. Always plot coordinates for the thing you tested, and print the `distance` column to confirm the peak is inside the window. If the distance is large, the figure needs the peak, and the gene label needs a caveat — or, given that 100 kb assignment, no gene label at all.

This is also a reminder about the 50 kb filter in Step 13: `chr7-12805615-12806434` was excluded from the GO gene list for exactly this reason, while the unfiltered volcano code happily labelled it *ARL4A*. Two parts of the same analysis disagreeing about whether a peak has a gene is a sign your distance policy is not applied consistently.

> 

**The general rule these two figures illustrate.** Coverage is normalized, so track heights are comparable: in the condition panel you want a visible difference in pileup at the peak itself, and in the donor panel you want both members of each pair to agree with each other and the pairs to differ. One donor standing alone while the other three match is donor variation, not disease biology, and no test on two replicates per group can separate those explanations for you. Looking is the only method available.

### Pseudobulk Heatmap of the Top Differential Peaks

```r
#-----------------------------------------------
# STEP 17: Heatmap of the top 40 differential peaks across donors
#-----------------------------------------------

# Variance-stabilized counts put all peaks on a comparable scale.
# blind = FALSE uses the design when estimating dispersion trends.
vsd <- vst(dds, blind = FALSE)

top40 <- head(res_pb$peak[order(res_pb$padj)], 40)
mat <- assay(vsd)[top40, ]

# Z-score each peak so the heatmap shows relative, not absolute, signal.
mat_z <- t(scale(t(mat)))

# Convert the matrix to long format for ggplot without extra dependencies.
hm_df <- as.data.frame(as.table(mat_z))
colnames(hm_df) <- c("peak", "sample", "z")
hm_df$condition <- coldata$condition[match(hm_df$sample, rownames(coldata))]

p_heat <- ggplot(hm_df, aes(x = sample, y = peak, fill = z)) +
  geom_tile() +
  facet_grid(. ~ condition, scales = "free_x", space = "free_x") +
  scale_fill_gradient2(low = "#2E86AB", mid = "white", high = "#C0392B",
                       midpoint = 0, name = "z-score") +
  labs(x = NULL, y = NULL,
       title = paste0("Top 40 differential peaks -- ", target_type)) +
  theme_minimal(base_size = 9) +
  theme(axis.text.y = element_text(size = 5),
        panel.grid = element_blank())

ggsave(file.path(plot_dir, "04_pseudobulk_heatmap.png"), p_heat,
       width = 7, height = 8, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/04_pseudobulk_heatmap.png?resize=896%2C1024&ssl=1)

> 

**Reading this figure:** you are looking for whether the two donors within each condition agree. Most rows here do exactly what you want — both healthy columns one color, both severe columns the other, with the facets making the split easy to scan. That consistency across individuals is what makes the 96 peaks worth reporting at all.

**Now find the rows that look uneven, because they are the useful ones.** `chr7-12805615-12806434` — the largest fold change in the entire result — is pale in both healthy columns, near white in `severe1`, and deep red only in `severe2`. Read on its own, that row says the top hit is carried by a single donor, and `severe2` is the one with the fewest cells (508) and the lowest median depth.

**The coverage plot in the previous section says something slightly different**, and it is the more reliable of the two. There, `severe1` does have a peak at that locus — modest, but clearly present, and clearly absent in both healthy donors. The direction is consistent across both severe donors; only the magnitude is skewed.

**Both readings are correct, and the discrepancy is instructive.** Z-scoring happens within a row, so on a peak with a `baseMean` of 22 counts across four libraries it amplifies small absolute differences into strong color. The heatmap is a good tool for spotting rows worth examining and a poor tool for judging whether an effect is real. When a row looks donor-driven, open the coverage tracks before you decide.

**This is also why you rank by adjusted p-value rather than fold change.** The extreme effect sizes sit on the lowest counts, and the lowest counts are where both z-scores and fold changes become unstable.

With only four columns this is a small figure, and that is honest. A heatmap with four columns tells a reviewer exactly how many replicates you had, which a heatmap of 5,213 individual cells would have concealed.

### Motif Enrichment and GO Enrichment Plots

```r
#-----------------------------------------------
# STEP 18: Bar plot of the most enriched motif families
#-----------------------------------------------

# FindMotifs returns rows ordered by p-value. Reorder by fold enrichment so
# that bar length and bar order agree -- otherwise a shorter bar can sit above
# a longer one and the figure reads as an error.
top_motifs <- head(motifs_up[motifs_up$p.adjust < 0.05, ], 15)
top_motifs <- top_motifs[order(top_motifs$fold.enrichment), ]
top_motifs$motif.name <- factor(top_motifs$motif.name,
                                levels = top_motifs$motif.name)

p_motif <- ggplot(top_motifs, aes(x = fold.enrichment, y = motif.name,
                                  fill = -log10(p.adjust))) +
  geom_col() +
  scale_fill_gradient(low = "#F5B7B1", high = "#7B241C",
                      name = "-log10 p.adjust") +
  labs(x = "Fold enrichment over GC-matched background", y = NULL,
       title = "TF motif families in peaks opened in severe disease") +
  theme_classic(base_size = 11)

ggsave(file.path(plot_dir, "05_motif_enrichment.png"), p_motif,
       width = 7, height = 5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/05_motif_enrichment.png?resize=1024%2C731&ssl=1)

```r
#-----------------------------------------------
# STEP 19: Dot plot of enriched GO biological processes
#-----------------------------------------------

p_go <- dotplot(ego_up, showCategory = 15) +
  labs(title = "GO biological process -- genes near opened peaks") +
  theme(axis.text.y = element_text(size = 8))

ggsave(file.path(plot_dir, "06_go_dotplot.png"), p_go,
       width = 8, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/09/06_go_dotplot.png?resize=1024%2C768&ssl=1)

> 

**Reading the motif figure:** bar length is enrichment over a GC-matched background, color is confidence. Families rather than individual proteins, as discussed above.

**It has three bars**, because only `RFX2`, `ATF4` and `RFX3` cleared the threshold out of 633 motifs tested on 46 regions. Three bars is a fair representation of that evidence, and a three-bar figure in a paper tells the reader the truth faster than any caption could. If you want a panel with more substance, plot `motifs_down` — 19 significant AP-1 family entries from 50 regions — and label it honestly as the closing-peak result.

**Reading the GO dot plot, where the legend is the finding.** Dot size is the number of query genes in the term and x-position is `GeneRatio`. Look at the `p.adjust` color scale: it collapses to a single value, 0.0379309, because 17 of the 18 terms share it exactly. A continuous color legend that degenerates into one tick is a visual signature of an underpowered enrichment, and it is worth leaving in the figure rather than hiding by switching to a discrete palette.

**Because the p-values tie, the vertical ordering is by `GeneRatio` alone** — which is why terms supported by five genes sit at the top and the two-gene terms cluster at the bottom left. That is a ranking by input size, not by evidence strength.

---

## 💾 Saving Results for Reuse and Supplementary Material

```r
#-----------------------------------------------
# STEP 20: Save objects and tables
#-----------------------------------------------

# The DESeq2 object carries counts, size factors, dispersions and results,
# so a reviewer can re-derive every number in the paper from it.
saveRDS(dds, file.path(obj_dir, "dds_cd14_monocyte.rds"))
saveRDS(da_sc, file.path(obj_dir, "da_singlecell_cd14_monocyte.rds"))

write.csv(res_pb, file.path(table_dir, "da_pseudobulk_deseq2.csv"),
          row.names = FALSE)
write.csv(sig_sc, file.path(table_dir, "da_singlecell_annotated.csv"),
          row.names = FALSE)
write.csv(motifs_up, file.path(table_dir, "motifs_opened_severe.csv"),
          row.names = FALSE)
write.csv(motifs_down, file.path(table_dir, "motifs_closed_severe.csv"),
          row.names = FALSE)
write.csv(as.data.frame(ego_up), file.path(table_dir, "go_bp_opened_severe.csv"),
          row.names = FALSE)

# The design summary that belongs in every methods section.
write.csv(as.data.frame(table(mono$sample_id, mono$condition)),
          file.path(table_dir, "design_cells_per_donor.csv"),
          row.names = FALSE)

writeLines(capture.output(sessionInfo()),
           file.path(da_dir, "sessionInfo_part5.txt"))
```

### Running the Same Analysis for Other Cell Types

Everything above is a function of one variable: `target_type`. Wrap Steps 4 through 9 in a function and apply it across the cell types that have enough cells in both conditions.

```r
#-----------------------------------------------
# STEP 21: Repeat the pseudobulk analysis across testable cell types
#-----------------------------------------------

# Exclude non-biological labels and any type with fewer than 200 cells
# in either condition.
cell_counts <- table(atac$cell_type, atac$condition)
exclude <- c("Low quality", "Unresolved", "NK/T cell unresolved")
testable <- rownames(cell_counts)[
  apply(cell_counts, 1, min) >= 200 & !rownames(cell_counts) %in% exclude
]

run_pseudobulk_da <- function(ct) {
  sub <- subset(atac, subset = cell_type == ct)
  m <- round(as.matrix(PseudobulkExpression(
    sub, assays = "peaks", layer = "counts",
    method = "aggregate", group.by = "sample_id")[["peaks"]]))
  sm <- unique(sub@meta.data[, c("sample_id", "condition")])
  rownames(sm) <- sm$sample_id
  cd <- sm[colnames(m), , drop = FALSE]
  cd$condition <- factor(cd$condition, levels = c("Healthy", "Severe"))
  d <- DESeqDataSetFromMatrix(m[rowSums(m >= 10) >= 2, ], cd, ~ condition)
  r <- as.data.frame(results(DESeq(d),
                             contrast = c("condition", "Severe", "Healthy")))
  r$peak <- rownames(r)
  r$cell_type <- ct
  r[!is.na(r$padj), ]
}

da_all <- do.call(rbind, lapply(testable, run_pseudobulk_da))

testable
table(da_all$cell_type[da_all$padj < 0.05])

write.csv(da_all, file.path(table_dir, "da_pseudobulk_all_celltypes.csv"),
          row.names = FALSE)
```

**Output:**

```
[1] "CD4 T naive"    "CD4 T memory"   "CD8 T effector" "B cell"
[5] "CD14 monocyte"  "CD16 monocyte"

        B cell  CD14 monocyte   CD4 T memory    CD4 T naive CD8 T effector
            30             98              1              2              6
```

> 

**Reading this output, which reframes the whole tutorial.** Six cell types cleared the 200-cell floor. Across all of them, the total number of differentially accessible peaks this experiment supports is 137.

**CD14 monocytes carry the result.** 98 peaks, against 30 in B cells and single digits in the T cell compartments. Choosing monocytes in Step 4 turned out to be the right call, but notice that you could not have known that in advance from cell numbers alone — CD8 T effectors had more cells and returned 6 peaks.

**CD16 monocytes are missing from the table entirely**, meaning zero significant peaks. They passed the cell-count filter at 1,298 and 287 cells, so this is not a power failure you could have predicted from the design table. Absence from the output is a result: report it rather than dropping the cell type quietly.

**One number does not match, and the mismatch is deliberate.** This loop reports 98 peaks for CD14 monocytes where Step 9 reported 96 — identical cells, identical filtering, identical design. The cause is DESeq2’s independent filtering. `results()` tunes its filter threshold to maximize rejections at whatever `alpha` you pass, Step 9 passed `alpha = 0.05`, and the function above accepts the default of 0.1. A different filter cutoff produces slightly different adjusted p-values near the boundary, and two peaks change status.

**One multiple-testing note.** Each cell type is corrected independently, which is standard practice and what the field expects. If a specific peak matters across several cell types, correct across the combined result instead — the numbers will be more conservative and more defensible.

---

## ✅ Best Practices for scATAC-seq Differential Accessibility Analysis

**1. Use pseudobulk as your primary method.** Teo et al. (2024) found pseudobulk methods at the top across every accuracy criterion, and essentially free of false discoveries in null comparisons where single-cell tests produced thousands. Report a single-cell test only as a secondary, exploratory ranking.

**2. Count your donors, not your cells.** Statistical power in a condition contrast comes from biological replicates. Adding 10,000 more cells from the same four donors increases false discoveries, it does not increase power. When designing an experiment, three donors per group is the minimum and five or more is where results become robust.

**3. Always correct for sequencing depth, and always report the depth difference.** Use `latent.vars = "nCount_peaks"` for single-cell tests; DESeq2’s size factors handle it for pseudobulk. Report medians per condition and per donor, as in Step 5.

**4. Do not filter by fold change before testing.** The benchmark showed that log-fold-change filtering does not improve concordance with matched bulk ATAC-seq relative to removing random peaks, and consistently increases false discoveries. Filter on adjusted p-value; use fold change to rank, not to exclude.

**5. Test one cell type at a time.** Pooling cell types confounds composition with regulation. If severe patients have fewer NK cells and you test all cells together, the NK chromatin signature will appear “closed in severe” purely because there are fewer NK cells.

**6. Analyze opened and closed regions separately.** Gains and losses are different regulatory programs. Merging them into one motif or pathway test cancels both.

**7. Match your background to your foreground.** For motif enrichment, use `AccessiblePeaks()` plus `MatchRegionStats()` rather than all peaks. For GO enrichment, use genes near your tested peaks as the universe, never all human genes.

**8. Look at raw coverage split by donor before believing any hit.** It takes two minutes and it catches the single most common failure — one individual driving the whole result.

**9. Report the composition change separately from the accessibility change.** Fewer CD8 T effectors is a real biological finding, and it is a different finding from “CD8 T effector chromatin has changed.” Proportion testing methods are covered in [scRNA-seq Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/) and apply unchanged here.

**10. Pass `alpha` explicitly to every `results()` call, and use the same value throughout.** DESeq2 tunes its independent filter to the alpha you supply, so the same data analyzed with `alpha = 0.05` and with the default `alpha = 0.1` returns different adjusted p-values and different peak counts. Step 21 shows the discrepancy: 96 peaks versus 98, same cells, same design.

**11. Plot what you tested.** Every figure that illustrates a differential peak should be built from that peak’s coordinates. Gene symbols are annotations attached after the fact, and on real data they are routinely tens or hundreds of kilobases away.

**12. Record versions.** Signac 1.17 changed background sampling in `MatchRegionStats()` and removed `RunChromVAR()`. Analyses run before and after that release are not numerically comparable. Save `sessionInfo()` with every result.

---

## ⚠️ Common Pitfalls and How to Avoid Them

**Pitfall 1: Treating cells as biological replicates.** The default in every single-cell package, and the source of most irreproducible scATAC-seq findings. *Avoid it by* running pseudobulk as your headline analysis and stating your donor count explicitly.

**Pitfall 2: Comparing conditions across all cells instead of within a cell type.** Produces a result that is entirely composition. *Avoid it by* subsetting first, always.

**Pitfall 3: Using all peaks as the motif background.** Your DA peaks are open in monocytes; most of the 185,581 peaks are not. Comparing them returns the monocyte identity program every time, no matter what the disease did. *Avoid it by* using `AccessiblePeaks()` to build the background pool.

**Pitfall 4: Using all human genes as the GO universe.** Returns immune terms for any immune dataset, regardless of the actual comparison. *Avoid it by* passing `universe = genes_bg` built from your own tested peaks.

**Pitfall 5: Naming a specific transcription factor from a motif result.** Motifs identify families. *Avoid it by* writing “the CEBP family motif” and confirming individual factors with expression data or footprinting.

**Pitfall 6: Ignoring the peak-to-gene distance.** A peak 300 kb from its nearest gene tells you almost nothing about that gene. *Avoid it by* applying a distance cutoff, reporting the distance distribution, and describing gene assignments as hypotheses.

**Pitfall 7: Interpreting a fold change without checking detection rates.** In sparse data a peak detected in 3 percent of cells in one group and 1.5 percent in the other is a two-fold change built on very few observations. *Avoid it by* reading `pct.1` and `pct.2` alongside `avg_log2FC`.

**Pitfall 8: Ranking GO terms that share an identical adjusted p-value.** With a small gene list, BH correction flattens a block of tied raw p-values onto one adjusted value, and the row order that `clusterProfiler` returns is then arbitrary. *Avoid it by* checking whether your `p.adjust` column has repeated values before you describe anything as the “top” term, and by reporting the terms as an unordered set when it does.

**Pitfall 9: Judging whether an effect is real from a z-scored heatmap.** Scaling within a row turns a handful of counts into saturated color, so low-coverage peaks look more donor-driven than they are. *Avoid it by* treating the heatmap as a tool for choosing which rows to inspect, and settling the question with coverage tracks.

**Pitfall 11: Passing a gene symbol to `CoveragePlot()` when you meant a peak.** The nearest gene can be 100 kb from the peak you tested, so the figure shows a locus that was never differential — and it will look convincingly unremarkable. *Avoid it by* plotting peak coordinates and printing the `distance` column to confirm the peak is inside the window.

**Pitfall 12: Filtering gene names with `is.na()` alone.** `ClosestFeature()` returns an empty string, not `NA`, when the nearest transcript has no symbol. An `is.na()` filter passes those straight through into your gene list, where they become a phantom entry. *Avoid it by* testing `gene_name != "" & !is.na(gene_name)`, as Step 13 does.

**Pitfall 13: Assuming a closed peak means a repressed gene.** Accessibility is regulatory potential. Genes can be transcribed from unchanged promoters, and open enhancers can be unbound. *Avoid it by* phrasing conclusions in terms of accessibility and validating with expression data where you have it.

---

## 📚 References

1. Teo AYY, Squair JW, Courtine G, Skinnider MA. Best practices for differential accessibility analysis in single-cell epigenomics. *Nature Communications*. 2024;15:8805. doi:10.1038/s41467-024-53089-5

2. Squair JW, Gautier M, Kathe C, et al. Confronting false discoveries in single-cell differential expression. *Nature Communications*. 2021;12:5692. doi:10.1038/s41467-021-25960-2

3. Zimmerman KD, Espeland MA, Langefeld CD. A practical solution to pseudoreplication bias in single-cell studies. *Nature Communications*. 2021;12:738. doi:10.1038/s41467-021-21038-1

4. Murphy AE, Skene NG. A balanced measure shows superior performance of pseudobulk methods in single-cell RNA-sequencing analysis. *Nature Communications*. 2022;13:7851. doi:10.1038/s41467-022-35519-4

5. Stuart T, Srivastava A, Madad S, Lareau CA, Satija R. Single-cell chromatin state analysis with Signac. *Nature Methods*. 2021;18:1333-1341. doi:10.1038/s41592-021-01282-5

6. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42:293-304. doi:10.1038/s41587-023-01767-y

7. Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology*. 2014;15:550. doi:10.1186/s13059-014-0550-8

8. Robinson MD, McCarthy DJ, Smyth GK. edgeR: a Bioconductor package for differential expression analysis of digital gene expression data. *Bioinformatics*. 2010;26(1):139-140. doi:10.1093/bioinformatics/btp616

9. Gontarz P, Fu S, Xing X, et al. Comparison of differential accessibility analysis strategies for ATAC-seq data. *Scientific Reports*. 2020;10:10150. doi:10.1038/s41598-020-66998-4

10. Ross-Innes CS, Stark R, Teschendorff AE, et al. Differential oestrogen receptor binding is associated with clinical outcome in breast cancer. *Nature*. 2012;481:389-393. doi:10.1038/nature10730

11. Wu T, Hu E, Xu S, et al. clusterProfiler 4.0: A universal enrichment tool for interpreting omics data. *The Innovation*. 2021;2(3):100141. doi:10.1016/j.xinn.2021.100141

12. McLean CY, Bristor D, Hiller M, et al. GREAT improves functional interpretation of cis-regulatory regions. *Nature Biotechnology*. 2010;28:495-501. doi:10.1038/nbt.1630

13. Bourgon R, Gentleman R, Huber W. Independent filtering increases detection power for high-throughput experiments. *Proceedings of the National Academy of Sciences*. 2010;107(21):9546-9551. doi:10.1073/pnas.0914005107

14. Brauns E, Azouz A, Grimaldi D, et al. Functional reprogramming of monocytes in patients with acute and convalescent severe COVID-19. *JCI Insight*. 2022;7(9):e154183. doi:10.1172/jci.insight.154183

15. Lemeille S, Paschaki M, Baas D, et al. Interplay of RFX transcription factors 1, 2 and 3 in motile ciliogenesis. *Nucleic Acids Research*. 2020;48(16):9019-9036. doi:10.1093/nar/gkaa625

16. Online Mendelian Inheritance in Man, entry *601863: Regulatory Factor X, 5 (RFX5) — the X-box-binding subunit of the MHC class II enhanceosome. [https://omim.org/entry/601863](https://omim.org/entry/601863)

17. Schep AN, Wu B, Buenrostro JD, Greenleaf WJ. chromVAR: inferring transcription-factor-associated accessibility from single-cell epigenomic data. *Nature Methods*. 2017;14:975-978. doi:10.1038/nmeth.4401

18. Granja JM, Corces MR, Pierce SE, et al. ArchR is a scalable software package for integrative single-cell chromatin accessibility analysis. *Nature Genetics*. 2021;53:403-411. doi:10.1038/s41588-021-00790-6

19. Fornes O, Castro-Mondragon JA, Khan A, et al. JASPAR 2020: update of the open-access database of transcription factor binding profiles. *Nucleic Acids Research*. 2020;48(D1):D87-D92. doi:10.1093/nar/gkz1001

20. Buenrostro JD, Giresi PG, Zaba LC, Chang HY, Greenleaf WJ. Transposition of native chromatin for fast and sensitive epigenomic profiling of open chromatin, DNA-binding proteins and nucleosome position. *Nature Methods*. 2013;10:1213-1218. doi:10.1038/nmeth.2688

21. Signac changelog and vignettes, including the motif analysis vignette and the removal of `RunChromVAR()` in version 1.17.0. [https://stuartlab.org/signac/](https://stuartlab.org/signac/) (2026)

22. Gene Expression Omnibus accession GSE282769. [https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE282769](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE282769)

---

*This tutorial is part of the comprehensive NGS101.com single-cell ATAC-seq analysis series for beginners.*
