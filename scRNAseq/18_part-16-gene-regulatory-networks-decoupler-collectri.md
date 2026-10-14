# How to Analyze Single-Cell RNA-seq Data &#8212; Complete Beginner's Guide Part 16: Build Gene Regulatory Networks with decoupleR and CollecTRI

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** June 10, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/)

---

*Infer per-cell transcription factor activity from a curated, signed regulatory network — no de novo network learning, no motif databases, just one fast linear model.*

In [Part 15](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/) you learned to redraw the entire scRNA-seq series with cleaner code. Now we return to biology with a new question: **which transcription factors are driving each cell type, and do those regulatory programs shift after periodontitis treatment?**

This is gene regulatory network (GRN) analysis at single-cell resolution. We will use the same GSE174609 periodontitis PBMC dataset from earlier in the series, the annotated Seurat object from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/), and two tools from the Saez-Rodriguez lab: **decoupleR** (the engine) and **CollecTRI** (the knowledge). By the end you will have a per-cell transcription factor activity score for every cell, a heatmap of the dominant regulators of each cell type, and a framework for comparing regulatory activity between conditions.

> 

**Prerequisites:** The downsampled Seurat object from [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Strategic_Downsampling_Maintaining_Biological_Diversity) (`seurat_subset.rds`) — the same balanced ~7,262-cell subset reused throughout this series. It already carries the [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) cell-type labels and condition metadata, so no new data is required.

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/)

- [Introduction: From Bulk Co-Expression Networks to Single-Cell TF Activity](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Introduction_From_Bulk_Co-Expression_Networks_to_Single-Cell_TF_Activity)

  - [Quick Recap: GRN Analysis for Bulk RNA-seq with GENIE3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Quick_Recap_GRN_Analysis_for_Bulk_RNA-seq_with_GENIE3)
  - [Why GENIE3 Does Not Translate Cleanly to scRNA-seq](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Why_GENIE3_Does_Not_Translate_Cleanly_to_scRNA-seq)
  - [Why Not SCENIC, pySCENIC, or SCENIC+?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Why_Not_SCENIC_pySCENIC_or_SCENIC)
  - [Introducing decoupleR and CollecTRI](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Introducing_decoupleR_and_CollecTRI)

- [Environment Setup and Package Installation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Environment_Setup_and_Package_Installation)
- [Example Dataset: The Downsampled PBMC Object from Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Example_Dataset_The_Downsampled_PBMC_Object_from_Part_7-2)

  - [Loading Libraries and the Downsampled Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Loading_Libraries_and_the_Downsampled_Seurat_Object)
  - [Detecting the Correct UMAP Reduction](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Detecting_the_Correct_UMAP_Reduction)

- [Step 3: Retrieve the CollecTRI Network](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_3_Retrieve_the_CollecTRI_Network)

  - [Which Species Are Supported, and How to List Them](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Which_Species_Are_Supported_and_How_to_List_Them)
  - [When the Live Download Fails: A Robust, Species-Independent Fallback](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#When_the_Live_Download_Fails_A_Robust_Species-Independent_Fallback)

- [Step 4: Infer Transcription Factor Activity with a Univariate Linear Model (ULM)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_4_Infer_Transcription_Factor_Activity_with_a_Univariate_Linear_Model_ULM)
- [Step 5: Store Activities as a New Seurat Assay](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_5_Store_Activities_as_a_New_Seurat_Assay)
- [Step 6: Activity Versus Expression — Comparing the Two Readouts](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_6_Activity_Versus_Expression_%E2%80%94_Comparing_the_Two_Readouts)
- [Downstream Analysis: Which Transcription Factors Define Each Cell Type?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Downstream_Analysis_Which_Transcription_Factors_Define_Each_Cell_Type)

  - [Step 7: Mean Activity Heatmap of the Top Variable TFs](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_7_Mean_Activity_Heatmap_of_the_Top_Variable_TFs)

- [Comparing Regulatory Activity Between Conditions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Comparing_Regulatory_Activity_Between_Conditions)

  - [Step 8: Mean TF Activity by Cell Type and Condition](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_8_Mean_TF_Activity_by_Cell_Type_and_Condition)
  - [Step 9: Visualizing a Single Regulator Split by Condition](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_9_Visualizing_a_Single_Regulator_Split_by_Condition)
  - [Step 10: Export the Activity Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Step_10_Export_the_Activity_Results)

- [Best Practices for Single-Cell TF Activity Inference](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Best_Practices_for_Single-Cell_TF_Activity_Inference)
- [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Common_Pitfalls_and_How_to_Avoid_Them)
- [Conclusion: What You Built in Part 16](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Conclusion_What_You_Built_in_Part_16)
- [Moving Forward](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Moving_Forward)
- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/#Like_this)

## Introduction: From Bulk Co-Expression Networks to Single-Cell TF Activity

### Quick Recap: GRN Analysis for Bulk RNA-seq with GENIE3

Earlier on NGS101 we built a gene regulatory network from bulk RNA-seq using [GENIE3](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/). The idea there was to **learn the network from scratch**: GENIE3 trains a random-forest regression for every gene, asking “how well can the expression of candidate regulators predict this gene’s expression?” The result is a ranked list of regulator-to-target edges — a co-expression network inferred purely from the data, with no outside knowledge required.

That approach works well for bulk RNA-seq, where each sample is a clean average of millions of cells and gene expression varies smoothly across a few dozen samples. Single-cell data is a different animal.

### Why GENIE3 Does Not Translate Cleanly to scRNA-seq

GENIE3 was designed and benchmarked on bulk data. Pointing it at a single-cell matrix runs into several problems at once:

1. **Dropout and zero-inflation.** A typical scRNA-seq cell detects only a fraction of the genes it actually expresses. The matrix is mostly zeros. Random-forest regression on this sparse signal produces unstable, noisy edges, because “gene not detected” and “gene not expressed” look identical.

2. **Scale.** GENIE3 fits one model per target gene across all samples. With tens of thousands of cells (the full GSE174609 dataset has roughly 72,000), this becomes extremely slow. Bulk studies have dozens of samples; single-cell studies have tens of thousands of “samples.”

3. **No direction, no sign, no prior.** GENIE3 returns an undirected, unsigned importance score. It cannot tell you whether a regulator activates or represses a target, and it ignores decades of curated biology about which transcription factor controls which gene.

4. **No per-cell readout.** GENIE3 gives you one network for the whole dataset. It does not tell you how active a regulator is *in each individual cell*, which is exactly what we want when cells belong to many different types and conditions.

The single-cell field solved this in two broad ways, and it is worth understanding both before we choose ours.

### Why Not SCENIC, pySCENIC, or SCENIC+?

The most famous single-cell GRN tool is **SCENIC**. SCENIC adapts the GENIE3/GRNBoost2 idea to single cells: it learns co-expression modules, prunes them against transcription-factor binding motifs to build “regulons,” and then scores each cell with AUCell. It is powerful, but it comes with practical costs for a beginner workflow:

- The original R **SCENIC** is old and slow, and its **pySCENIC** Python re-implementation has effectively stalled — its last release was in 2022 and package trackers now classify it as an inactive project. Getting its motif-ranking databases and dependency stack working is a frequent source of frustration.

- **SCENIC+**, the lab’s modern successor, is excellent but solves a different problem: it infers *enhancer-driven* networks and **requires single-cell multiomic data (paired scRNA-seq + scATAC-seq)**. Our dataset is scRNA-seq only, so SCENIC+ simply does not apply.

For an scRNA-seq-only dataset, we want something fast, reproducible, easy to install, and grounded in curated biology. That is exactly what decoupleR plus CollecTRI gives us.

### Introducing decoupleR and CollecTRI

Instead of *learning* a network and then scoring cells, we will **start from a known, curated network** and ask a simpler, more robust question for every cell: given which target genes are up or down, **how active is each transcription factor?**

This is called **footprint-based activity inference**. The key insight: a transcription factor’s own mRNA level is a poor proxy for its activity. TFs are often expressed at low levels, are heavily affected by dropout, and are frequently regulated *after* transcription. But a TF leaves a “footprint” in the genes it controls. If a TF’s activating targets are collectively high and its repressing targets are collectively low, the TF is probably active — regardless of what its own transcript is doing. Aggregating many target genes is far more robust to single-cell noise than reading any one gene.

#### What Is decoupleR and What Can It Do?

[decoupleR](https://saezlab.github.io/decoupleR/) is an R (and Python) package that infers biological activities from omics data. It is an **ensemble of methods** (over a dozen, including ULM, MLM, VIPER, AUCell, GSVA, and over-representation analysis) that all share the same two inputs:

1. **A matrix** of molecular readouts — here, the normalized single-cell expression matrix (genes in rows, cells in columns).

2. **A prior-knowledge network** — a set of “source -> target” relationships with weights (for example, transcription factor -> target gene, weighted by activation or repression).

Given those two inputs, decoupleR returns an **activity score for each source in each cell**. The same machinery works for transcription factors (with a TF-target network), for signaling pathways (with PROGENy), or for cell-type markers — you just swap the prior network. In this tutorial we use it for transcription factor activity.

#### What Is CollecTRI and Why Does decoupleR Need It?

decoupleR is the engine; it needs fuel. The fuel is a prior-knowledge network, and for transcription factors the recommended resource is **[CollecTRI](https://github.com/saezlab/CollecTRI)**.

CollecTRI is a curated collection of **signed transcription factor -> target gene interactions** assembled from 12 different resources. “Signed” is the important word: each interaction carries a **mode of regulation (mor)** of +1 (the TF activates the target) or -1 (the TF represses the target). The human network contains roughly 43,000 interactions covering over 1,000 transcription factors. Compared with the lab’s older DoRothEA network, CollecTRI covers more transcription factors and identifies perturbed TFs more accurately.

decoupleR can download CollecTRI for you directly through the OmniPath database with a single function call, so there are no manual file downloads to manage. The division of labor is clean:

- **CollecTRI** answers “which genes does each transcription factor control, and in which direction?”

- **decoupleR** answers “given that map and this cell’s expression, how active is each transcription factor?”

With the concepts in place, let us build the workflow.

---

## Environment Setup and Package Installation

This is a standalone tutorial, so the block below installs **every package the code uses**, even if you already have most of them from earlier parts. We deliberately install only what this tutorial actually calls:

- **decoupleR** — the activity-inference engine (Bioconductor).

- **OmnipathR** — the Bioconductor package `get_collectri()` uses to download the network. decoupleR only *suggests* it (does not require it), so installing decoupleR alone is not enough; we install OmnipathR explicitly below.

- **Seurat** — to load the annotated object and to manage the new activity assay.

- **ggplot2** and **patchwork** — for plotting and for arranging multi-panel figures.

- **dplyr**, **tibble**, **tidyr** — to reshape decoupleR’s long-format results into the wide matrices Seurat and the heatmap expect.

- **pheatmap** and **RColorBrewer** — to draw the mean-activity heatmaps with a diverging color palette.

```r
#-----------------------------------------------
# STEP 1: Install all packages used in this tutorial (only if missing)
#-----------------------------------------------

# CRAN packages
cran_packages <- c("BiocManager", "Seurat", "ggplot2", "patchwork",
                   "dplyr", "tibble", "tidyr", "pheatmap", "RColorBrewer")
missing_cran  <- cran_packages[!(cran_packages %in% rownames(installed.packages()))]
if (length(missing_cran) > 0) {
  install.packages(missing_cran)
}

# Bioconductor packages.
# OmnipathR is required by get_collectri() (Step 3) but is only a SUGGESTED
# dependency of decoupleR, so it must be installed explicitly.
bioc_packages <- c("decoupleR", "OmnipathR")
missing_bioc  <- bioc_packages[!(bioc_packages %in% rownames(installed.packages()))]
if (length(missing_bioc) > 0) {
  BiocManager::install(missing_bioc, update = FALSE, ask = FALSE)
}
```

> 

**OmniPath needs internet access.** `get_collectri()` (Step 3) downloads the network from the OmniPath web service the first time you run it. If that download is blocked, fails with an SSL/certificate error, or crashes inside OmnipathR, do not get stuck here — Step 3 includes a species-independent fallback that fetches the network once (in R or Python) and reads it from a small CSV, which works offline and on locked-down machines.

---

## Example Dataset: The Downsampled PBMC Object from Part 7-2

We use the **same balanced subset** that the rest of this series relies on for compute-heavy steps: the downsampled GSE174609 periodontitis PBMC object created in [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Strategic_Downsampling_Maintaining_Biological_Diversity). There is nothing new to download. That object was built by taking 10% of the cells from each cell type (with a 50-cell floor and Platelets excluded), which keeps roughly **7,262 cells across all 7 cell types** while preserving each type’s relative abundance and both experimental conditions.

Using the downsampled object here is a deliberate, practical choice. The activity step (Step 4) densifies the expression matrix, and a dense matrix of ~72,000 cells is large enough to strain a laptop. The ~7,262-cell subset reduces that matrix roughly ten-fold (to about 1 GB), so the entire workflow runs comfortably on a laptop in minutes while still representing every cell type and condition. As with the other tutorials in this series, you can re-run on the full annotated object from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) on the HPC cluster once you are confident the workflow is correct.

> 

**Don’t have the downsampled object yet?** Follow the *Strategic Downsampling* section of [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Strategic_Downsampling_Maintaining_Biological_Diversity) (or [Part 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/)) to create it. It is saved as `seurat_subset.rds` in your working directory.

### Loading Libraries and the Downsampled Seurat Object

```r
#-----------------------------------------------
# STEP 2: Load libraries and the downsampled object from Part 7-2
#-----------------------------------------------

library(decoupleR)
library(Seurat)
library(ggplot2)
library(patchwork)
library(dplyr)
library(tibble)
library(tidyr)
library(pheatmap)
library(RColorBrewer)

# Reproducible directory for every figure we save
dir.create("plots/grn", recursive = TRUE, showWarnings = FALSE)

# Downsampled object from Part 7-2 (adjust the filename if you saved it
# under a different name, e.g. "seurat_downsampled.rds")
seurat_obj <- readRDS("seurat_subset.rds")

# Seurat 5 splits expression into per-sample layers after integration.
# Join them so the RNA assay has a single 'data' layer to extract below.
# (Idempotent: safe to run even if the layers are already joined.)
seurat_obj <- JoinLayers(seurat_obj)

# Use the curated cell-type labels as the active identity
Idents(seurat_obj) <- "final_annotation"
```

This object carries the metadata we rely on below: `final_annotation` (cell types from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) — CD4+ T cells, CD8+ T cells, T cells, NK cells, B cells, Classical Monocytes, and Monocytes) and `condition` (Healthy / Post_Treatment). The expression values live in the `RNA` assay; the normalized values produced back in [Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/) and [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) are in its `data` layer, which is exactly what footprint inference needs.

### Detecting the Correct UMAP Reduction

In [Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/) you chose an integration method (Harmony, CCA, RPCA, or FastMNN), and the UMAP was named accordingly (for example, `umap.harmony`). The snippet below detects whichever UMAP is present so the rest of the tutorial works regardless of the method you used.

```r
#-----------------------------------------------
# STEP 2b: Detect the UMAP reduction produced in Part 3
#-----------------------------------------------

reductions_present <- names(seurat_obj@reductions)

umap_reduction <- dplyr::case_when(
  "umap.harmony" %in% reductions_present ~ "umap.harmony",
  "umap.cca"     %in% reductions_present ~ "umap.cca",
  "umap.rpca"    %in% reductions_present ~ "umap.rpca",
  "umap.mnn"     %in% reductions_present ~ "umap.mnn",
  TRUE                                   ~ "umap"
)
```

We will pass `umap_reduction` to every UMAP plot so the figures land on the embedding you actually built.

---

## Step 3: Retrieve the CollecTRI Network

The network is the prior knowledge that turns raw expression into transcription factor activity. CollecTRI ships in three species — **human, mouse, and rat** — and decoupleR downloads whichever you request through OmniPath. Our PBMC dataset is human, so we use `organism = "human"`; if you are analyzing mouse or rat data, change that one word to `"mouse"` or `"rat"` and everything downstream stays the same. The `split_complexes = FALSE` argument keeps transcription factor complexes (such as the NF-kB or AP-1 dimers) as single regulators rather than breaking them into subunits, which is the recommended default.

```r
#-----------------------------------------------
# STEP 3: Download the CollecTRI TF-target network
#-----------------------------------------------

# Set this to match YOUR data: "human", "mouse", or "rat"
organism <- "human"

# source = transcription factor, target = regulated gene,
# mor = mode of regulation (+1 activation, -1 repression)
net <- decoupleR::get_collectri(organism = organism, split_complexes = FALSE)

# Inspect the structure (a long-format tibble)
head(net)
```

**Expected output (first rows, human):**

```bash
# A tibble: 43,178 x 3
   source target   mor
 1 MYC    TERT       1
 2 SPI1   BGLAP      1
 3 SMAD3  JUN        1
 4 SMAD4  JUN        1
 5 STAT5A IL2        1
 6 STAT5B IL2        1
```

> 

**Reading the network:** Each row is one regulatory edge. `source` is the transcription factor, `target` is a gene it controls, and `mor` is the sign of that control (+1 = the TF turns the target on, -1 = the TF turns it off). All of CollecTRI’s biology lives in these three columns, and they are exactly the three column names decoupleR asks for in the next step. (Mouse and rat return the same three columns, with that species’ gene symbols — for example `Myc`, `Spi1` for mouse.)

### Which Species Are Supported, and How to List Them

CollecTRI is curated in human, and the OmniPath web service serves **human, mouse, and rat directly** — these three are the best supported and the ones used in most published workflows. The `organism` argument is not limited to them, though. It accepts a common name (`"human"`), a Latin name (`"Mus musculus"`), an Ensembl name, or an **NCBI Taxonomy ID** (`9606` human, `10090` mouse, `10116` rat), and for **any organism other than human it builds the network by translating the human regulons through orthology**. In principle that means any species with orthology data in Ensembl — pig, zebrafish, macaque, and hundreds more.

To see the full list of organisms available for translation, query the Ensembl organisms table through OmnipathR:

```r
#-----------------------------------------------
# List all species available for orthology translation
#-----------------------------------------------

organisms <- OmnipathR::ensembl_organisms()

# Each row is one organism, with its common name, Latin name, and Ensembl/NCBI identifiers.
# Use any of those values as the 'organism' argument to get_collectri().
head(organisms)
nrow(organisms)   # hundreds of organisms
```

Two honest caveats before you rely on a non-human, non-mouse, non-rat network:

- **The orthology path is the heavy, network-dependent one.** Human, mouse, and rat download as ready-made tables. Every other species triggers the orthology machinery — the same Ensembl lookup that produces the SSL/`CERT_EXPIRED` failures discussed below (and in Python, it additionally requires the larger `pypath-omnipath` backend, not just `omnipath`). If `ensembl_organisms()` itself errors, that is the same connectivity problem, and the CSV fallback in the next section is the way around it.

- **Orthology-translated networks are approximations.** A regulon validated in human is only as transferable as its genes’ orthologs. Coverage shrinks and uncertainty grows with evolutionary distance, and the returned gene symbols are in the target species’ nomenclature. For human, mouse, and rat this is well validated; for distant species, treat the activities as exploratory and confirm key findings against species-specific literature.

### When the Live Download Fails: A Robust, Species-Independent Fallback

The live `get_collectri()` download depends on the OmniPath web service and an Ensembl organism lookup, and that chain can break for reasons that have nothing to do with your analysis.

The durable solution is to download the `net` manually. Remember what `net` actually is: a plain table of `source`, `target`, and `mor`. decoupleR does not care how you obtained that table. So get it once, from whatever route works for your species and setup, save it as a CSV, and read that CSV from then on. Two reliable routes:

**Route 1 — Fetch with Python `decoupler` (most reliable cross-platform).** The Python package downloads the identical CollecTRI network but uses its own bundled certificates and a different code path, so it commonly succeeds on the exact machines where the R download fails. Run this once (set `organism` to `human`, `mouse`, or `rat`):

```python
# pip install "decoupler<2" omnipath
import decoupler as dc

organism = "human"   # or "mouse" / "rat"
net = dc.get_collectri(organism=organism, split_complexes=False)

# Keep the three columns decoupleR needs and name them source/target/mor
net = net[['source', 'target', 'weight']].rename(columns={'weight': 'mor'})
net.to_csv(f"collectri_{organism}.csv", index=False)
```

**Route 2 — Generate in R on any machine where the download works** (an HPC login node, a cloud session, a colleague’s laptop), then copy the file over:

```r
organism <- "human"   # or "mouse" / "rat"
net <- decoupleR::get_collectri(organism = organism, split_complexes = FALSE)
write.csv(net, paste0("collectri_", organism, ".csv"), row.names = FALSE)
```

**Then, in your analysis session, simply read the file** — no internet, no OmniPath, no Ensembl:

```r
#-----------------------------------------------
# STEP 3 (offline): Load CollecTRI from a static CSV
#-----------------------------------------------

organism <- "human"   # match the file you created
# Columns: source (TF), target (gene), mor (+1 activation / -1 repression)
net <- read.csv(paste0("collectri_", organism, ".csv"), stringsAsFactors = FALSE)

head(net)
```

The CSV has the same `source` / `target` / `mor` columns as the live download (about 43,000 human interactions across ~1,180 TFs; ~40,000 for mouse and ~39,000 for rat), so **everything from Step 4 onward is identical regardless of how you loaded the network or which species you use.** This same “obtain the table once, then read a CSV” pattern also applies to other decoupleR prior-knowledge resources — for example DoRothEA (`get_dorothea()`) for an alternative TF network, or PROGENy (`get_progeny()`) for pathway activity — since they all return a tidy source/target/weight table.

---

## Step 4: Infer Transcription Factor Activity with a Univariate Linear Model (ULM)

Now we estimate, for every cell and every transcription factor, an activity score. We use the **univariate linear model (ULM)** method. The intuition is approachable even without statistics:

For one cell and one transcription factor, ULM fits a simple linear regression. The response variable is the cell’s expression across all genes. The single predictor is the TF’s target weight for each gene — `+1` for activated targets, `-1` for repressed targets, and `0` for genes the TF does not control. If the activated targets are high and the repressed targets are low, the regression slope is steeply positive, and its **t-value becomes the activity score**. A positive score means the TF looks active in that cell; a negative score means it looks inactive. Repeat for every TF and every cell, and you get an activity matrix.

```r
#-----------------------------------------------
# STEP 4: Run ULM to score TF activity per cell
#-----------------------------------------------

# Extract the normalized, log-transformed expression matrix (genes x cells).
# Seurat 5 stores this in the 'data' layer of the RNA assay; LayerData() is the
# version-safe accessor. run_ulm() needs a dense matrix, so we coerce here.
mat <- as.matrix(LayerData(seurat_obj, assay = "RNA", layer = "data"))

# Fit one univariate linear model per (cell, TF).
# minsize = 5 drops TFs with fewer than 5 measured targets, where the
# t-value would be unreliable.
acts <- decoupleR::run_ulm(
  mat     = mat,
  net     = net,
  .source = "source",
  .target = "target",
  .mor    = "mor",
  minsize = 5
)

head(acts)
```

**Expected output (first rows):**

```bash
# A tibble: ... x 5
  statistic source condition        score p_value
1 ulm       ABL1   AAACCCAAGA...-1   2.64 0.00820
2 ulm       ABL1   AAACCCACAT...-1   0.89 0.372
3 ulm       ABL1   AAACCCAGTC...-1   2.79 0.00525
...
```

> 

**Reading the result:** decoupleR returns a long-format table with one row per (TF, cell). `source` is the transcription factor, `condition` is decoupleR’s generic name for a column of the input matrix — here that is a **cell barcode**, not the Healthy/Post-Treatment condition. `score` is the activity (the ULM t-value) and `p_value` is its significance. We will reshape this into a TF-by-cell matrix in the next step.

---

## Step 5: Store Activities as a New Seurat Assay

To visualize activities with the same Seurat functions you already know (`DimPlot`, `FeaturePlot`), we fold the activity matrix back into the object as a brand-new assay called `tfsulm`. This keeps the original gene expression (`RNA` assay) untouched and lets you switch between “what genes are expressed” and “what TFs are active” by changing the default assay.

```r
#-----------------------------------------------
# STEP 5: Store TF activities in a new 'tfsulm' assay
#-----------------------------------------------

# Reshape long results into a wide TF (rows) x cell (cols) matrix,
# then add it as a new assay.
tf_assay_data <- acts %>%
  tidyr::pivot_wider(
    id_cols     = "source",
    names_from  = "condition",   # cell barcodes become columns
    values_from = "score"
  ) %>%
  tibble::column_to_rownames("source") %>%
  as.matrix()

seurat_obj[["tfsulm"]] <- CreateAssayObject(data = tf_assay_data)

# Work in the activity space from here on
DefaultAssay(seurat_obj) <- "tfsulm"

# Scale activities (center and unit-variance per TF) so that high- and
# low-magnitude TFs are visually comparable, then use the scaled values
# as the assay's data layer for plotting.
seurat_obj <- ScaleData(seurat_obj)
LayerData(seurat_obj, assay = "tfsulm", layer = "data") <-
  LayerData(seurat_obj, assay = "tfsulm", layer = "scale.data")
```

After this step, `seurat_obj` carries two assays. `RNA` holds gene expression; `tfsulm` holds transcription factor activity, one value per TF per cell. Anything you could plot for a gene, you can now plot for a TF.

---

## Step 6: Activity Versus Expression — Comparing the Two Readouts

A good first check is to plot a transcription factor’s activity next to its raw expression. **PAX5**, the master regulator of B-cell identity, is a clean example: its activity should light up the B-cell population sharply, and comparing that to its transcript shows how the two readouts relate — sometimes they agree, and sometimes activity recovers a signal that a dropout-prone transcript would blur.

```r
#-----------------------------------------------
# STEP 6: Compare TF activity vs TF expression on the UMAP
#-----------------------------------------------

# A diverging palette: blue = low/inactive, white = neutral, red = high/active
diverging_cols <- rev(RColorBrewer::brewer.pal(n = 11, name = "RdBu"))[c(2, 10)]

# Panel 1: cell types for context
p_celltypes <- DimPlot(
  seurat_obj, reduction = umap_reduction,
  label = TRUE, repel = TRUE, pt.size = 0.1
) + NoLegend() + ggtitle("Cell types")

# Panel 2: PAX5 ACTIVITY (from the tfsulm assay)
DefaultAssay(seurat_obj) <- "tfsulm"
p_pax5_activity <- FeaturePlot(
  seurat_obj, features = "PAX5", reduction = umap_reduction
) +
  scale_colour_gradient2(low = diverging_cols[1], mid = "white",
                         high = diverging_cols[2]) +
  ggtitle("PAX5 activity")

# Panel 3: PAX5 EXPRESSION (from the RNA assay)
DefaultAssay(seurat_obj) <- "RNA"
p_pax5_expression <- FeaturePlot(
  seurat_obj, features = "PAX5", reduction = umap_reduction
) + ggtitle("PAX5 expression")

# Return to activity space for the rest of the tutorial
DefaultAssay(seurat_obj) <- "tfsulm"

p_pax5 <- p_celltypes | p_pax5_activity | p_pax5_expression
ggsave("plots/grn/06_pax5_activity_vs_expression.png", p_pax5,
       width = 18, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/06_pax5_activity_vs_expression.png?resize=1024%2C341&ssl=1)

> 

**Reading the three panels (actual result):** The left panel is your familiar cell-type map (note the axes are `umapcca_1/2`, the CCA-integrated UMAP from Part 3). In the middle panel, PAX5 *activity* lights up the B-cell cluster (lower left) as a clean, confident block of red, with the rest of the map hovering near zero — exactly what you want from a B-cell master regulator. The right panel shows PAX5 *expression*, and in this dataset the transcript is actually captured reasonably well: the same B-cell cluster is clearly purple. What still distinguishes the two panels is *smoothness*: activity is uniformly high across essentially every B cell, whereas expression is patchier from cell to cell within the cluster, because any single transcript is subject to dropout while the footprint aggregates dozens of target genes. That smoothing is the practical advantage of activity inference, and it grows for transcription factors that are expressed at lower levels than PAX5. You can swap `"PAX5"` for other canonical regulators to sanity-check your data — for example **SPI1** (PU.1) for the monocyte lineage or **EBF1** for B cells; both behave as expected in this object (see the heatmap below).

---

## Downstream Analysis: Which Transcription Factors Define Each Cell Type?

A per-cell activity matrix is rich but overwhelming. The standard summary is the **mean activity of the most variable transcription factors across cell types**, drawn as a heatmap. TFs that are uniformly active everywhere are uninformative; the interesting regulators are the ones whose activity *differs* between cell types.

### Step 7: Mean Activity Heatmap of the Top Variable TFs

```r
#-----------------------------------------------
# STEP 7: Mean TF activity per cell type (top variable TFs)
#-----------------------------------------------

n_tfs <- 25  # how many of the most variable TFs to display

# Long table of scaled activity per cell, tagged with its cell type
activity_long <- t(as.matrix(LayerData(seurat_obj, assay = "tfsulm", layer = "data"))) %>%
  as.data.frame() %>%
  dplyr::mutate(cell_type = Idents(seurat_obj)) %>%
  tidyr::pivot_longer(cols = -cell_type, names_to = "source", values_to = "score") %>%
  dplyr::group_by(cell_type, source) %>%
  dplyr::summarise(mean_activity = mean(score), .groups = "drop")

# Rank TFs by how variable their mean activity is across cell types
top_tfs <- activity_long %>%
  dplyr::group_by(source) %>%
  dplyr::summarise(variability = stats::sd(mean_activity), .groups = "drop") %>%
  dplyr::arrange(dplyr::desc(variability)) %>%
  head(n_tfs) %>%
  dplyr::pull(source)

# Wide cell-type x TF matrix for the heatmap
heatmap_mat <- activity_long %>%
  dplyr::filter(source %in% top_tfs) %>%
  tidyr::pivot_wider(id_cols = "cell_type", names_from = "source",
                     values_from = "mean_activity") %>%
  tibble::column_to_rownames("cell_type") %>%
  as.matrix()

# Diverging palette centered on zero
palette_cols <- grDevices::colorRampPalette(
  rev(RColorBrewer::brewer.pal(n = 11, name = "RdBu"))
)(100)

# pheatmap writes directly to disk via its 'filename' argument
pheatmap::pheatmap(
  mat          = heatmap_mat,
  color        = palette_cols,
  border_color = "white",
  cellwidth    = 14,
  cellheight   = 14,
  treeheight_row = 20,
  treeheight_col = 20,
  main         = "Top variable TF activity by cell type",
  filename     = "plots/grn/07_top_tf_activity_heatmap.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/activity_long-e1780872432492.png?resize=480%2C160&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/07_top_tf_activity_heatmap.png?resize=1024%2C390&ssl=1)

> 

**Reading the heatmap (actual result):** Rows are cell types, columns are the 25 most variable transcription factors, and color is mean activity (red = active, blue = inactive). The column dendrogram splits the TFs into two clear modules. On the left is a compact **B-cell module** — RFXAP, RFX5, RFXANK, EBF1, and PAX5 — glowing red in the B-cell row and nowhere else. The much larger right-hand module — CEBPA, CEBPB, CEBPG, SPI1, STAT3, HIF1A, RARA, NFE2L2, JUN, JUND, and more — is intensely red in **Classical Monocytes** and, strikingly, deep blue (actively repressed) in B cells. In other words, the top-variance TFs are dominated by a single strong axis: monocyte identity versus B-cell identity.

Two honest observations follow from that. First, the T and NK populations (CD4+ T, CD8+ T, T cells, NK cells) stay pale across this particular top-25 set — their distinguishing regulators are more subtly varying and simply did not rank among the highest-variance TFs, which are dominated by the much more distinct myeloid and B-cell programs. If you want to resolve lymphoid regulators (TBX21, EOMES, TCF7), subset to those cell types and recompute the most variable TFs within them. Second, the two monocyte labels behave differently: **Classical Monocytes** show the full, strong myeloid program, while the smaller **Monocytes** cluster stays muted — a useful flag that the secondary label is less cleanly defined, worth cross-checking against the annotation from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/). This is exactly how TF activity earns its keep: it is an independent readout that both confirms confident labels (B cells, Classical Monocytes) and highlights the shakier ones.

---

## Comparing Regulatory Activity Between Conditions

The biological payoff of this dataset is the Healthy vs Post-Treatment contrast. Differential expression ([Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/) told you *which genes* change; TF activity tells you *which regulators* change, which is often a more interpretable, lower-dimensional summary of the same biology. Because periodontitis is an inflammatory condition, transcription factors in inflammatory programs (the NF-kB family, the STAT and IRF families) are natural candidates to watch — and, as you will see, this analysis localizes the treatment effect to a specific cell type rather than spreading it evenly across the immune compartment.

### Step 8: Mean TF Activity by Cell Type and Condition

```r
#-----------------------------------------------
# STEP 8: Mean TF activity per cell type x condition
#-----------------------------------------------

# A small, biologically motivated panel of inflammatory regulators to track.
# Edit this vector to follow whichever TFs are relevant to your question.
candidate_tfs <- c("NFKB1", "RELA", "STAT1", "STAT3", "IRF1", "JUN", "FOS")
candidate_tfs <- candidate_tfs[candidate_tfs %in% rownames(seurat_obj[["tfsulm"]])]

# Long table tagged with BOTH cell type and condition
condition_long <- t(as.matrix(LayerData(seurat_obj, assay = "tfsulm", layer = "data"))) %>%
  as.data.frame() %>%
  dplyr::mutate(
    cell_type = Idents(seurat_obj),
    condition = seurat_obj$condition
  ) %>%
  tidyr::pivot_longer(cols = c(-cell_type, -condition),
                      names_to = "source", values_to = "score") %>%
  dplyr::filter(source %in% candidate_tfs) %>%
  dplyr::group_by(cell_type, condition, source) %>%
  dplyr::summarise(mean_activity = mean(score), .groups = "drop")

# Build a cell-type::condition x TF matrix so each row is one group
condition_mat <- condition_long %>%
  dplyr::mutate(group = paste(cell_type, condition, sep = " :: ")) %>%
  tidyr::pivot_wider(id_cols = "group", names_from = "source",
                     values_from = "mean_activity") %>%
  tibble::column_to_rownames("group") %>%
  as.matrix()

pheatmap::pheatmap(
  mat          = condition_mat,
  color        = palette_cols,
  border_color = "white",
  cluster_rows = FALSE,     # keep groups in a readable, fixed order
  cellwidth    = 16,
  cellheight   = 14,
  main         = "Inflammatory TF activity: cell type x condition",
  filename     = "plots/grn/08_tf_activity_by_condition_heatmap.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/condition_mat.png?resize=1024%2C428&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/08_tf_activity_by_condition_heatmap.png?resize=1024%2C934&ssl=1)

> 

**Reading the comparison (actual result):** Each row is a cell-type/condition group, each column an inflammatory transcription factor. The result is clear and biologically sensible. **Classical Monocytes carry almost all of the inflammatory signal** — their two rows are deep red across every TF, while every other cell type sits near zero or below. And within Classical Monocytes there is a consistent **drop from Healthy to Post_Treatment**: every one of the seven regulators is less active after treatment, with JUN showing the largest change (the darkest red cell in the whole map is `Classical Monocytes :: Healthy` at JUN), followed by STAT3, IRF1, and FOS. That is exactly the direction you would hope to see — non-surgical periodontal therapy dampening the inflammatory transcriptional program of circulating monocytes. The lymphoid rows (T, CD4+ T, NK) hover near zero with only faint condition differences, and B cells and CD8+ T cells are net-negative for these inflammatory TFs in both conditions. Because this is a downsampled object and the heatmap shows group means rather than a formal statistical test, treat the monocyte finding as a strong, well-directed hypothesis: the natural confirmation is to recompute on the full dataset and, ideally, run a pseudobulk differential test of monocyte activity between conditions.

### Step 9: Visualizing a Single Regulator Split by Condition

To inspect one transcription factor in spatial context, project its activity onto the UMAP and split the panels by condition.

```r
#-----------------------------------------------
# STEP 9: Project one TF's activity onto the UMAP, split by condition
#-----------------------------------------------

# Pick any TF present in the assay; NFKB1 is a canonical inflammatory regulator
tf_to_plot <- "NFKB1"

p_tf_split <- FeaturePlot(
  seurat_obj, features = tf_to_plot, reduction = umap_reduction,
  split.by = "condition"
) &
  scale_colour_gradient2(low = diverging_cols[1], mid = "white",
                         high = diverging_cols[2])

ggsave(paste0("plots/grn/09_", tf_to_plot, "_activity_by_condition.png"),
       p_tf_split, width = 12, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/09_NFKB1_activity_by_condition.png?resize=1024%2C512&ssl=1)

> 

**Reading the split UMAP (actual result):** The two panels share a layout and a color scale (the `&` operator applies one diverging scale to both, so differences are real rather than cosmetic). NFKB1 activity is governed first and foremost by cell type: the Classical Monocytes cluster (upper left) is solidly red in **both** conditions, while B cells (lower left) are blue and the T/NK regions are mixed and pale. The Healthy-versus-Post_Treatment difference that stood out so clearly in the aggregated heatmap is, by contrast, hard to see here — at single-cell resolution the monocyte cluster looks only marginally less uniform after treatment. That is an instructive result in its own right: a single-TF split UMAP is excellent for showing *where* a regulator is active but is noisy for *condition* comparisons, because per-cell scores scatter widely. When the question is “did activity change between conditions,” the cell-type-by-condition aggregation in Step 8 is the more sensitive and reliable view; the split UMAP is best treated as a spatial sanity check on top of it.

### Step 10: Export the Activity Results

Save both the raw per-cell scores and the cell-type summary so you can reuse them without recomputing.

```r
#-----------------------------------------------
# STEP 10: Export TF activity tables and the updated object
#-----------------------------------------------

# Full long-format per-cell scores (TF, cell, score, p-value)
write.csv(acts, "plots/grn/tf_activity_per_cell.csv", row.names = FALSE)

# Tidy cell-type-level summary used by the Step 7 heatmap
write.csv(activity_long, "plots/grn/tf_activity_by_celltype.csv", row.names = FALSE)

# The Seurat object now carries the 'tfsulm' assay for future sessions
saveRDS(seurat_obj, "plots/grn/seurat_with_tf_activity.rds")
```

---

## Best Practices for Single-Cell TF Activity Inference

**1. Score activity from a footprint, not from the TF’s own transcript.** This is the central principle. A TF’s mRNA is noisy and dropout-prone in scRNA-seq; the aggregate behavior of its target genes is far more stable. When activity and expression disagree (as with PAX5 in Step 6), trust the activity for inference and use the expression only as supporting context.

**2. Use the curated network as-is, and keep complexes together.** CollecTRI is the recommended, benchmarked resource for human, mouse, and rat. Keep `split_complexes = FALSE` so multi-subunit regulators (NF-kB, AP-1) are scored as the functional units they are. Only swap to DoRothEA if you specifically need its confidence-level tiers.

**3. Run ULM on the normalized data layer, scaled afterward, not before.** Feed `run_ulm()` the log-normalized expression (`LayerData(..., layer = "data")`). Scaling happens *after* inference, on the activity assay, purely to make TFs visually comparable in heatmaps. Scaling the gene matrix before ULM is not the intended workflow.

**4. Downsample cells, never genes, to control memory.** This tutorial uses the balanced ~7,262-cell subset from [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Strategic_Downsampling_Maintaining_Biological_Diversity) precisely so the dense matrix stays small while every cell type and condition is preserved. If you ever need a quick subset of a different object, `subset(obj, downsample = N)` keeps N cells per identity. Do **not** instead shrink the matrix by keeping only CollecTRI target genes: ULM uses every gene as the regression background, so removing “non-target” genes changes the activity scores.

**5. Treat TF activity as an independent check on annotation.** If each cell type’s top regulators match its known biology (Step 7), that is strong, orthogonal evidence your [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) labels are sound. A mismatch is a signal to revisit the annotation.

**6. Match condition panels on a shared, diverging, zero-centered color scale.** Activity scores are signed (positive = active, negative = inactive), so a palette centered on white at zero (RdBu) reads correctly. When comparing conditions, force the same scale across panels (the `&` trick in Step 9) so differences are real, not artifacts of independent rescaling.

**7. Save the network and the object.** `get_collectri()` needs internet; cache it with `saveRDS()` for offline or HPC reuse. Likewise, save the object with the `tfsulm` assay so downstream parts of your analysis do not recompute scores.

---

## Common Pitfalls and How to Avoid Them

**Pitfall 1: Confusing decoupleR’s “condition” with your experimental condition.** In `run_ulm()` output, the `condition` column is decoupleR’s generic name for “a column of the input matrix” — here, a **cell barcode**. Your Healthy/Post-Treatment variable lives in `seurat_obj$condition`. Keep the two straight, especially in the Step 8 reshaping.

**Pitfall 2: Forgetting to switch the default assay.** After Step 5 the default assay is `tfsulm`. If you call `FeaturePlot()` expecting gene expression and get activity (or vice versa), check `DefaultAssay(seurat_obj)` and set it explicitly, as we do throughout Step 6.

**Pitfall 3: Layers not joined.** In Seurat 5 an integrated object often holds per-sample layers (`data.1`, `data.2`, …). `LayerData(..., layer = "data")` on a split object can error or return only one sample. Run `JoinLayers()` first (Step 2).

**Pitfall 4: Out-of-memory on `as.matrix()`.** On the downsampled object this step is light, so it should not trip here. It becomes the single most common failure point only if you swap in the **full** ~72,000-cell object: densifying ~72,000 cells x ~19,000 genes needs roughly 10 GB. If R crashes there, move that run to the HPC cluster or request more memory.

**Pitfall 5: A TF you want is missing.** Not every gene symbol is a CollecTRI source. Confirm a TF exists with `"NFKB1" %in% rownames(seurat_obj[["tfsulm"]])` before plotting it; Step 8 filters the candidate list this way so missing TFs are skipped rather than throwing an error.

**Pitfall 6: Over-reading a single significant cell.** A positive `p_value` for one TF in one cell is weak evidence. Activity inference is most reliable as a *group* summary (per cell type, per condition), which is why the downstream steps aggregate before interpreting.

---

## Conclusion: What You Built in Part 16

In this tutorial you moved from gene-level expression to regulator-level activity, the cleaner and more interpretable layer of single-cell biology. Specifically, you:

1. **Understood why bulk GRN tools and SCENIC do not fit this task** — GENIE3 is built for bulk and stumbles on single-cell sparsity and scale, pySCENIC is effectively unmaintained, and SCENIC+ requires multiomic data you do not have.

2. **Learned footprint-based inference** — estimating a transcription factor’s activity from the collective behavior of its target genes rather than its own noisy transcript.

3. **Retrieved CollecTRI**, a curated, signed TF-target network, directly through decoupleR and OmniPath.

4. **Computed per-cell TF activity** with the ULM method on the balanced ~7,262-cell downsampled dataset from Part 7-2.

5. **Stored activity as a Seurat assay** so every Seurat plotting function works on regulators just as it does on genes.

6. **Saw why footprints behave well** with the PAX5 comparison — activity is uniformly high across the entire B-cell cluster, smoother than the cell-to-cell patchiness of even a well-captured transcript.

7. **Profiled the dominant regulators of each cell type** and found a strong B-cell module (RFX complex, EBF1, PAX5) and a large myeloid module (CEBP family, SPI1, STAT3, HIF1A) that sharply separate Classical Monocytes and B cells, while flagging the secondary “Monocytes” label as less distinct.

8. **Localized the treatment effect**: among inflammatory regulators, Classical Monocytes show consistently lower activity (JUN, STAT3, IRF1, and others) after periodontal therapy, while lymphoid populations barely move — a focused, biologically sensible hypothesis about where treatment acts.

9. **Exported reusable activity tables and an updated object** for downstream work.

## Moving Forward

Transcription factor activity is one footprint analysis; decoupleR’s real strength is that the same engine works on any prior network. The most natural next step is **pathway activity inference with PROGENy**, which swaps the CollecTRI network for a pathway-response signature and tells you which signaling pathways (such as TNFa, NF-kB, JAK-STAT) are active per cell — a perfect complement to the TF activities you just computed, and a strong follow-up for an inflammatory disease like periodontitis. You can also feed your differential-activity results into the cell-communication picture from [Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/) to ask whether changed signaling lines up with changed downstream regulators. As always, redraw any of these figures with the [scplotter](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-15-better-visualization-with-scplotter/) toolkit from Part 15 when you need publication-ready output.

## References

1. Badia-i-Mompel P, Velez Santiago J, Braunger J, et al. decoupleR: ensemble of computational methods to infer biological activities from omics data. *Bioinformatics Advances*. 2022;2(1):vbac016. doi:10.1093/bioadv/vbac016 [decoupleR]

2. Muller-Dott S, Tsirvouli E, Vazquez M, et al. Expanding the coverage of regulons from high-confidence prior knowledge for accurate estimation of transcription factor activities. *Nucleic Acids Research*. 2023;51(20):10934-10949. doi:10.1093/nar/gkad841 [CollecTRI]

3. Turei D, Valdeolivas A, Gul L, et al. Integrated intra- and intercellular signaling knowledge for multicellular omics analysis. *Molecular Systems Biology*. 2021;17(3):e9923. doi:10.15252/msb.20209923 [OmniPath]

4. Aibar S, Gonzalez-Blas CB, Moerman T, et al. SCENIC: single-cell regulatory network inference and clustering. *Nature Methods*. 2017;14(11):1083-1086. doi:10.1038/nmeth.4463 [SCENIC, for context]

5. Bravo Gonzalez-Blas C, De Winter S, Hulselmans G, et al. SCENIC+: single-cell multiomic inference of enhancers and gene regulatory networks. *Nature Methods*. 2023;20(9):1355-1367. doi:10.1038/s41592-023-01938-4 [SCENIC+, multiomic]

6. Huynh-Thu VA, Irrthum A, Wehenkel L, Geurts P. Inferring regulatory networks from expression data using tree-based methods. *PLoS ONE*. 2010;5(9):e12776. doi:10.1371/journal.pone.0012776 [GENIE3]

7. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

8. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. *Journal of Translational Medicine*. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source — GSE174609]

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
