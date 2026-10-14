# How to Analyze Single-Cell ATAC-seq Data &#8212; A Complete Beginner's Guide Part 3: Integration and Clustering

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** August 14, 2026
- **Categories:** ATAC-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/)

---

*Build a consensus peak set, normalize sparse chromatin with TF-IDF, correct batch effects with Harmony and reciprocal LSI, and find biologically meaningful clusters*

---

> 

**Prerequisites:** This tutorial assumes you have completed [Part 1: From FASTQ to Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/) and [Part 2: Thorough Quality Control with Signac](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/). You should have the four `<sample>_qc.rds` objects Part 2 produced, the original Cell Ranger ATAC output directories still intact, the ENCODE blacklist still in `reference/blacklist/`, and the Pixi environment from Parts 1 and 2. Basic R familiarity is assumed, but every command is explained.

In Part 2 you took each sample from a raw Cell Ranger ATAC directory to a clean, doublet-filtered object. You did that **one sample at a time**, deliberately, because quality control decisions have to be made per sample.

Now the four samples have to become one dataset.

This is the step where scATAC-seq stops resembling scRNA-seq. In transcriptomics, merging samples is almost trivial: every sample measures the same genes, so you stack the matrices and move on to batch correction. In chromatin accessibility, **the samples do not even share a feature space**. Before you can merge anything, you have to build one.

By the end of this tutorial you will have a single integrated object containing all four samples, clustered at a resolution you can defend, with a UMAP where cells group by biology rather than by which run they came from.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/)

- [Introduction: Why Single-Cell ATAC-seq Data Must Be Integrated Before Clustering](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Introduction_Why_Single-Cell_ATAC-seq_Data_Must_Be_Integrated_Before_Clustering)

  - [What Goes Wrong If You Skip Integration](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#What_Goes_Wrong_If_You_Skip_Integration)
  - [Where Batch Effects in Chromatin Accessibility Come From](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Where_Batch_Effects_in_Chromatin_Accessibility_Come_From)
  - [The Peak Set Problem: A Challenge Unique to scATAC-seq](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#The_Peak_Set_Problem_A_Challenge_Unique_to_scATAC-seq)
  - [How Integration and Clustering Differ Between scATAC-seq and scRNA-seq](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#How_Integration_and_Clustering_Differ_Between_scATAC-seq_and_scRNA-seq)
  - [The Workflow We Will Follow](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#The_Workflow_We_Will_Follow)

- [Setting Up the R Environment for scATAC-seq Integration](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Setting_Up_the_R_Environment_for_scATAC-seq_Integration)

  - [Adding Harmony and clustree to Your Pixi Environment](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Adding_Harmony_and_clustree_to_Your_Pixi_Environment)
  - [Loading Libraries and Defining Project Paths](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Loading_Libraries_and_Defining_Project_Paths)
  - [Project Folder Layout](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Project_Folder_Layout)

- [Example Data: The Four QC-Filtered Samples from Part 2](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Example_Data_The_Four_QC-Filtered_Samples_from_Part_2)

  - [The Cohort](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#The_Cohort)
  - [Loading the Filtered Objects](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Loading_the_Filtered_Objects)
  - [Confirming the Metadata Part 3 Depends On](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Confirming_the_Metadata_Part_3_Depends_On)
  - [Reusing the Gene Annotation](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Reusing_the_Gene_Annotation)

- [Building a Consensus Peak Set Across All Four Samples](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Building_a_Consensus_Peak_Set_Across_All_Four_Samples)

  - [Reading the Per-Sample Peak Calls](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Reading_the_Per-Sample_Peak_Calls)
  - [Merging the Peak Lists into a Union Set](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Merging_the_Peak_Lists_into_a_Union_Set)
  - [Filtering the Consensus Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Filtering_the_Consensus_Peaks)
  - [Understanding Peak Fusion: When reduce() Goes Too Far](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Understanding_Peak_Fusion_When_reduce_Goes_Too_Far)

- [Requantifying Every Sample Against the Consensus Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Requantifying_Every_Sample_Against_the_Consensus_Peaks)

  - [Creating Fragment Objects for QC-Passed Cells](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Creating_Fragment_Objects_for_QC-Passed_Cells)
  - [Running FeatureMatrix](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Running_FeatureMatrix)
  - [Rebuilding the Chromatin Assays](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Rebuilding_the_Chromatin_Assays)
  - [Merging the Cohort into One Object](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Merging_the_Cohort_into_One_Object)

- [Normalizing Sparse Chromatin Data with TF-IDF](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Normalizing_Sparse_Chromatin_Data_with_TF-IDF)

  - [Why LogNormalize Fails on Chromatin Accessibility Data](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Why_LogNormalize_Fails_on_Chromatin_Accessibility_Data)
  - [The Text Analogy That Makes TF-IDF Click](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#The_Text_Analogy_That_Makes_TF-IDF_Click)
  - [Running TF-IDF](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Running_TF-IDF)
  - [Selecting Top Features by Prevalence, Not Variance](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Selecting_Top_Features_by_Prevalence_Not_Variance)

- [Dimensionality Reduction with SVD and Latent Semantic Indexing](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Dimensionality_Reduction_with_SVD_and_Latent_Semantic_Indexing)

  - [What SVD Does, and Why Not PCA](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#What_SVD_Does_and_Why_Not_PCA)
  - [Running SVD](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Running_SVD)
  - [Diagnosing Depth Correlation: Why Component 1 Gets Discarded](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Diagnosing_Depth_Correlation_Why_Component_1_Gets_Discarded)
  - [Visualizing the Uncorrected UMAP: Does This Dataset Even Need Integration?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Visualizing_the_Uncorrected_UMAP_Does_This_Dataset_Even_Need_Integration)

- [Integration Strategy 1: Harmony](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Integration_Strategy_1_Harmony)

  - [How Harmony Works](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#How_Harmony_Works)
  - [Running Harmony on the LSI Embedding](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Running_Harmony_on_the_LSI_Embedding)

- [Integration Strategy 2: Seurat Anchors with Reciprocal LSI](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Integration_Strategy_2_Seurat_Anchors_with_Reciprocal_LSI)

  - [How Reciprocal LSI Projection Works](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#How_Reciprocal_LSI_Projection_Works)
  - [Preparing Each Sample and Finding Anchors](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Preparing_Each_Sample_and_Finding_Anchors)
  - [Integrating the Embeddings](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Integrating_the_Embeddings)

- [Harmony vs Seurat Anchors: Which Integration Method Should You Choose?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Harmony_vs_Seurat_Anchors_Which_Integration_Method_Should_You_Choose)

  - [Comparing the Two Results Visually](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Comparing_the_Two_Results_Visually)
  - [Measuring Batch Mixing Quantitatively](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Measuring_Batch_Mixing_Quantitatively)
  - [Choosing Between Them](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Choosing_Between_Them)

- [Clustering scATAC-seq Cells Across Multiple Resolutions](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Clustering_scATAC-seq_Cells_Across_Multiple_Resolutions)

  - [Building the Neighbor Graph](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Building_the_Neighbor_Graph)
  - [Sweeping Across Clustering Resolutions](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Sweeping_Across_Clustering_Resolutions)
  - [Choosing a Resolution with a Clustering Tree](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Choosing_a_Resolution_with_a_Clustering_Tree)
  - [Checking Cluster Composition Across Samples](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Checking_Cluster_Composition_Across_Samples)

- [Visualizing the Integrated and Clustered Dataset](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Visualizing_the_Integrated_and_Clustered_Dataset)

  - [The Core Cluster Map](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#The_Core_Cluster_Map)
  - [Overlaying QC Metrics to Rule Out Technical Clusters](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Overlaying_QC_Metrics_to_Rule_Out_Technical_Clusters)
  - [A First Look at Biology: Coverage Plots at Marker Loci](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#A_First_Look_at_Biology_Coverage_Plots_at_Marker_Loci)
  - [Saving the Integrated Object](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Saving_the_Integrated_Object)

- [Best Practices for scATAC-seq Integration and Clustering](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Best_Practices_for_scATAC-seq_Integration_and_Clustering)
- [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Common_Pitfalls_and_How_to_Avoid_Them)
- [Conclusion & Key Takeaways](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Conclusion_Key_Takeaways)
- [References](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/#Like_this)

## Introduction: Why Single-Cell ATAC-seq Data Must Be Integrated Before Clustering

### What Goes Wrong If You Skip Integration

Suppose you merge four samples, run dimensionality reduction, and cluster. You get twelve clusters. You color the UMAP by sample and discover that clusters 1, 4, and 7 are almost entirely `severe1`, clusters 2 and 5 are almost entirely `healthy1`, and so on.

You have not found twelve cell types. You have found four samples, each split into three or four internal states. Every downstream analysis built on those clusters — differential accessibility, motif enrichment, cell type proportions between conditions — is now measuring the wrong thing. Worse, the errors are systematic rather than random, so they will look convincingly significant.

This failure mode is called a **batch effect**: technical variation that tracks with sample identity and swamps the biological variation you care about.

### Where Batch Effects in Chromatin Accessibility Come From

Some sources are familiar from scRNA-seq, and some are specific to ATAC:

**Sequencing depth****Tn5 transposition efficiency****Nuclei isolation quality****Cryopreservation and thaw****Chemistry or kit version****Peak calling per sample****No analogue at all**

| Source | What it does to the data | scRNA-seq analogue? |
| --- | --- | --- |
|  | Deeply sequenced cells have more non-zero peaks, so they look “more accessible” everywhere | Yes, but far weaker |
|  | Variable enzyme activity or enzyme-to-nuclei ratio shifts the global signal-to-background ratio | No direct analogue |
|  | Over-permeabilized nuclei give high background; under-permeabilized give low complexity | Partly (cell dissociation stress) |
|  | Damaged nuclei raise the doublet rate and the background fraction | Partly |
|  | Different fragment size distributions and barcode structures | Yes |
|  | Each sample defines its own feature space |  |

That last row is the one that makes scATAC-seq integration genuinely different, and it deserves its own section.

### The Peak Set Problem: A Challenge Unique to scATAC-seq

In scRNA-seq, features are genes. Genes come from the reference annotation, so `CD3E` means the same thing in every sample, in every experiment, forever. The feature space is fixed before you collect any data.

In scATAC-seq, features are **peaks** — regions the peak caller decided were enriched for Tn5 insertions. Peak calling is a statistical decision made **on your data**, so it depends on how many cells you captured, how deeply you sequenced, and which cell types happened to be present.

The consequence is a trap that catches almost every beginner:

> 

**A zero in a scATAC-seq count matrix is ambiguous.** It can mean “this region is closed in this cell”, or it can mean “this region was never called as a peak in this sample, so it was never quantified.” The first is biology. The second is an artifact of the pipeline. If you merge per-sample matrices naively, the second kind of zero is **perfectly correlated with sample identity** — and no batch correction method can undo that, because the information was never measured in the first place.

A worked example makes this concrete. Suppose a regulatory element at `chr2:87,001,000-87,001,800` is open in NK cells. Sample `severe1` captured 400 NK cells, so the peak caller found the region easily and it appears in `severe1`‘s peak list. Sample `healthy2` captured only 60 NK cells, so the same region fell below the significance threshold and never made it into `healthy2`‘s peak list.

Merge the two matrices on shared peak names and this region is either dropped entirely, or `healthy2` gets a column of zeros there. Either way, an apparent difference between severe and healthy disease states has been manufactured by peak calling.

**The fix is requantification.** You build one consensus peak set that spans all samples, then go back to each sample’s fragment file and count fragments in every consensus region for every cell. Now a zero always means “no fragments observed here”, in every sample, unambiguously.

This is why Part 3 is more work than its scRNA-seq counterpart, and why we spend the first third of this tutorial on peaks before touching a single normalization function.

### How Integration and Clustering Differ Between scATAC-seq and scRNA-seq

If you have worked through [Part 3 of our scRNA-seq series](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/), this table maps what transfers and what does not:

**Feature space****Preparing to merge**`merge()`**Data density****Value distribution****Normalization**`LogNormalize``SCTransform``RunTFIDF`**Feature selection****prevalence**`FindTopFeatures`**Linear reduction****LSI****Depth confounding****Component 1 of LSI is discarded****Batch correction input****Anchor-based integration**`rpca``rlsi`**embeddings****Clustering**`algorithm = 1``algorithm = 3`**Interpreting clusters**

| Step | scRNA-seq | scATAC-seq | Why the difference |
| --- | --- | --- | --- |
|  | Genes from the reference; fixed | Peaks called from your data; sample-specific | Peak calling is a statistical decision, gene annotation is not |
|  | and go | Consensus peaks, then requantify from fragments | Zeros are ambiguous until the feature space is shared |
|  | 10-20 percent non-zero | 1-3 percent non-zero | At most two DNA copies per locus per diploid cell |
|  | Counts, over-dispersed, unbounded | Near-binary (0, 1, occasionally 2) | Copy number ceiling |
|  | or | (term frequency-inverse document frequency) | Count-based variance models do not fit binary data |
|  | Highly variable genes, by variance | Top features by  () | Variance is nearly a function of the mean when data are binary |
|  | PCA on scaled data | SVD on the TF-IDF matrix, giving | Scaling a sparse binary matrix destroys sparsity and blows up memory |
|  | Regressed out or absorbed by SCTransform |  | Depth is the single largest source of variance in sparse binary data |
|  | PCA embedding | LSI embedding (components 2 onward) | Same idea, different input space |
|  | CCA or reciprocal PCA () | Reciprocal LSI (), integrating  not the data matrix | The data matrix is too sparse and too large to integrate directly |
|  | Louvain, | SLM, | Signac’s recommendation for sparse chromatin graphs |
|  | Marker genes, immediately readable | Gene activity scores or motif enrichment, one step removed | Peaks are not genes; interpretation comes in Part 4 |

**What does transfer:** the Seurat object model, `FindNeighbors`, `FindClusters`, `RunUMAP`, `DimPlot`, Harmony, and the entire logic of resolution selection. Once you are past the peak set and into the LSI embedding, the workflow feels familiar.

### The Workflow We Will Follow

```r
Four QC-filtered objects from Part 2
    |
1. Read per-sample peak calls -> union with reduce() -> filter by width,
   chromosome, and blacklist          ==>  CONSENSUS PEAK SET
    |
2. Create Fragment objects for QC-passed cells
   Requantify every sample with FeatureMatrix()
    |
3. Rebuild ChromatinAssays -> merge() into one cohort object
    |
4. RunTFIDF -> FindTopFeatures -> RunSVD          ==>  LSI EMBEDDING
    |
5. DepthCor: identify and discard depth-driven components
    |
6. Uncorrected UMAP  -> is there a batch effect?
    |
7. Integration:  Harmony   OR   Seurat anchors (reciprocal LSI)
    |
8. FindNeighbors -> FindClusters across a resolution sweep -> clustree
    |
9. Visualize, validate, save          ==>  READY FOR PART 4 (annotation)
```

Steps 1 to 3 have no scRNA-seq equivalent. Steps 4 to 9 will feel familiar, with different function names.

---

## Setting Up the R Environment for scATAC-seq Integration

### Adding Harmony and clustree to Your Pixi Environment

We continue with the same Pixi project from Parts 1 and 2, which already provides `r-base`, `r-signac`, `r-seurat`, `r-hdf5r`, `r-ggplot2`, `r-patchwork`, and the Bioconductor annotation and range packages. Only two additions are needed. If Pixi is new to you, our [Pixi setup guide](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/) covers installation.

```bash
# Move into the Pixi project created in Part 1
cd /projects/mylab/shared/scatac-analysis

# Add only what Part 3 actually needs:
#   r-harmony  -- fast batch correction applied directly to the LSI embedding
#   r-clustree -- clustering tree, for choosing a resolution rather than guessing
pixi add r-harmony r-clustree
```

Two packages you will see in the code are already present and do not need installing:

- **`future`** is a hard dependency of Seurat, so it is already in the environment. `FeatureMatrix()` uses it for parallel requantification.

- **`rtracklayer`** was added in Part 2 to import the ENCODE blacklist. We use it again for exactly the same purpose.

Confirm the new packages load before going further:

```r
# Drop into a shell where the environment is active
pixi shell

# Confirm the two new packages load cleanly
Rscript -e 'library(harmony); library(clustree); packageVersion("harmony")'
```

### Loading Libraries and Defining Project Paths

Start R inside the Pixi shell. As in Part 2, we keep Cell Ranger output strictly read-only and write everything new into a separate directory.

```r
#-----------------------------------------------
# STEP 1: Libraries, paths, and parallel settings
#-----------------------------------------------

library(Signac)          # ChromatinAssay, FeatureMatrix, TF-IDF, SVD, CoveragePlot
library(Seurat)          # object model, clustering, UMAP, DimPlot
library(GenomicRanges)   # GRanges arithmetic: reduce(), findOverlaps(), width()
library(GenomeInfoDb)    # standardChromosomes(), seqlevels()
library(rtracklayer)     # import() for the ENCODE blacklist BED file
library(harmony)         # RunHarmony(): batch correction on the LSI embedding
library(clustree)        # clustree(): clustering tree across resolutions
library(ggplot2)         # all plotting
library(patchwork)       # side-by-side figure assembly
library(future)          # parallel backend used by FeatureMatrix()

# Read-only Cell Ranger ATAC output from Part 1 (one subdirectory per sample)
cellranger_dir <- "/projects/mylab/shared/scatac_tutorial/results"

# Part 2's QC output. Read-only from here on -- this tutorial only loads the
# filtered objects from it and never writes into it.
qc_dir <- "/projects/mylab/shared/scatac_tutorial/downstream_qc"

# ENCODE unified hg38 blacklist (ENCFF356LFX), downloaded once in Part 2.
blacklist_bed <- "/projects/mylab/shared/reference/blacklist/ENCFF356LFX.bed"

# Everything Part 3 produces goes here: a sibling of results/ and
# downstream_qc/, not a subfolder of either. Each stage of the series owns
# one top-level directory, so no stage can overwrite another's output.
integ_dir <- "/projects/mylab/shared/scatac_tutorial/integrated"
plot_dir  <- file.path(integ_dir, "plots")
obj_dir   <- file.path(integ_dir, "objects")
invisible(lapply(c(plot_dir, obj_dir), dir.create,
                 recursive = TRUE, showWarnings = FALSE))

# Parallel backend for FeatureMatrix(). Use "multisession" instead of
# "multicore" if you are in RStudio or on Windows, where forking is unavailable.
plan("multicore", workers = 4)

# Seurat refuses to ship objects larger than 500 MB to workers by default.
# Chromatin matrices are much larger than that, so raise the ceiling to 50 GB.
options(future.globals.maxSize = 50 * 1024^3)

# A fixed seed makes UMAP and graph-based clustering reproducible
set.seed(1234)
```

> 

**On `future.globals.maxSize`:** if you skip this line, `FeatureMatrix()` fails with an error about the size of exported globals exceeding the allowed maximum. It is not a memory limit on your machine — it is a safety check on how much data gets copied to each parallel worker. Raising it is the standard fix; the value just has to exceed your largest object.

### Project Folder Layout

Each stage of this series owns exactly one top-level directory under `scatac_tutorial/`. Part 3 reads from the first two and writes only to the third, so no stage can overwrite another’s output — and if a later stage goes wrong, you can delete its directory and start over without touching anything upstream.

```r
/projects/mylab/shared/scatac_tutorial/
    results/            <- Part 1: cellranger-atac output   (read-only)
    downstream_qc/      <- Part 2: per-sample QC objects    (read-only)
    integrated/         <- Part 3: THIS TUTORIAL            (writable)
```

`scatac_tutorial/results/<sample>/outs/``peaks.bed``fragments.tsv.gz``scatac_tutorial/downstream_qc/<sample>/objects/``<sample>_qc.rds``scatac_tutorial/downstream_qc/cohort_qc_summary.csv``reference/blacklist/ENCFF356LFX.bed``scatac_tutorial/integrated/objects/`**This tutorial**`scatac_tutorial/integrated/plots/`**This tutorial**

| Directory | Contents | Written by |
| --- | --- | --- |
|  | , , matrices | Part 1 — read-only |
|  | and per-sample QC tables | Part 2 — read-only |
|  | One row per sample: retention, median metrics | Part 2 — read-only |
|  | ENCODE unified hg38 exclusion list | Part 2 setup |
|  | Merged, integrated, and clustered objects |  |
|  | All figures produced below |  |

---

## Example Data: The Four QC-Filtered Samples from Part 2

### The Cohort

We continue with the COVID-19 PBMC cohort from [GSE282769](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE282769), introduced in Part 1: four 10x Chromium scATAC-seq libraries from human peripheral blood mononuclear cells, two from patients with severe COVID-19 and two from healthy donors.

`severe1``severe2``healthy1``healthy2`

| Sample | Condition | SRA run | Role in this tutorial |
| --- | --- | --- | --- |
|  | Severe | SRR31499777 | Walkthrough sample in Parts 1 and 2 |
|  | Severe | SRR31499804 |  |
|  | Healthy | SRR31499787 |  |
|  | Healthy | SRR31499788 |  |

This is a deliberately small cohort — two per group — which is honest about a limitation you should carry into your own work: **with n = 2 per condition, sample identity and biological condition are almost impossible to separate**. Anything that differs between `severe1` and `severe2` is indistinguishable from donor variation. We integrate on `sample_id` rather than on `condition` precisely for this reason, and we return to the point repeatedly when validating clusters — because this cohort does produce a population found in only one donor, and deciding what to say about it is a real judgement you will face in your own data.

### Loading the Filtered Objects

Part 2 wrote one RDS file per sample into `downstream_qc/<sample>/objects/`. Load all four into a named list.

```r
#-----------------------------------------------
# STEP 2: Load the four QC-filtered objects from Part 2
#-----------------------------------------------

sample_ids <- c("severe1", "severe2", "healthy1", "healthy2")

# Part 2 wrote <sample>_qc.rds into downstream_qc/<sample>/objects/.
qc_files <- file.path(qc_dir, sample_ids, "objects",
                      paste0(sample_ids, "_qc.rds"))
names(qc_files) <- sample_ids

# Fail early and clearly if any path is wrong, rather than deep inside a loop
stopifnot(all(file.exists(qc_files)))

atac_list <- lapply(qc_files, readRDS)

# Cells and peaks retained per sample after Part 2 QC
sapply(atac_list, dim)
```

**Output from this cohort:**

```
      severe1 severe2 healthy1 healthy2
[1,]   88899   53576    76429    80014
[2,]    3485    4773     6861    15452
```

> 

**Before going further, reopen `downstream_qc/cohort_qc_summary.csv` from Part 2.** That table is what decides whether this cohort can safely be merged at all. If one sample retained 40 percent of its cells while the others retained 85 percent, or if one has a median TSS enrichment far below the rest, integration will be fighting a quality difference rather than a batch difference — and no amount of Harmony will fix that. Integration corrects systematic shifts between comparable samples; it does not rescue a failed library.

### Confirming the Metadata Part 3 Depends On

Part 2 stored `sample_id` and `condition` on every object. Verify they are present, because the entire integration hinges on `sample_id` being correct. If either column is missing, rebuild it now rather than after merging.

```bash
#-----------------------------------------------
# STEP 3: Verify the grouping metadata carried over from Part 2
#-----------------------------------------------

# Each object should report a single sample_id and a single condition
sapply(atac_list, function(x) c(unique(x$sample_id), unique(x$condition)))
```

### Reusing the Gene Annotation

Part 2 attached an Ensembl gene annotation to each object’s `peaks` assay. Rather than querying AnnotationHub again — a step that fails when the service is down or the cluster has no outbound network — simply lift the annotation off the object you already have.

```r
#-----------------------------------------------
# STEP 4: Reuse the gene annotation attached in Part 2
#-----------------------------------------------

# Annotation() returns the GRanges of gene models stored in the assay.
# We reattach this to the requantified assays so CoveragePlot() works later.
annotations <- Annotation(atac_list[[1]])

# Should report a large number of ranges on UCSC-style chromosome names
# (chr1, chr2, ...), not Ensembl-style (1, 2, ...)
length(annotations)
seqlevelsStyle(annotations)
```

**Output:**

```
[1] 3870812
[1] "UCSC"
```

Nearly four million ranges looks alarming if you were expecting a gene count. It is correct. `GetGRangesFromEnsDb()` returns **exon-level** ranges for every transcript of every gene, not one range per gene, so the number runs into the millions for a full human annotation. What matters here is the second line: `UCSC`.

> 

**Why this matters:** Signac matches gene models to peaks by chromosome name. If the annotation says `1` and your peaks say `chr1`, every overlap silently returns zero — no error, just empty plots and gene activity matrices full of zeros. Part 2 already converted the style to UCSC, so this check should pass; run it anyway, because it costs one second and catches a failure that is otherwise invisible until Part 4.

---

## Building a Consensus Peak Set Across All Four Samples

This is the section with no scRNA-seq counterpart. The goal is a single list of genomic regions that we will quantify identically in every sample, the same core strategy used in our [bulk ATAC-seq tutorial](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-2-differential-binding-analysis-using-diffbind/).

### Reading the Per-Sample Peak Calls

Cell Ranger ATAC wrote a `peaks.bed` file into each sample’s `outs/` directory. These are the per-sample peak lists we need to combine.

```r
#-----------------------------------------------
# STEP 5: Read each sample's peak calls into GRanges
#-----------------------------------------------

# read.table() skips the "#" comment header Cell Ranger writes at the top
# of peaks.bed by default, so no extra arguments are needed.
peak_list <- lapply(sample_ids, function(sid) {
  bed <- read.table(
    file      = file.path(cellranger_dir, sid, "outs", "peaks.bed"),
    col.names = c("chr", "start", "end")
  )
  makeGRangesFromDataFrame(bed)
})
names(peak_list) <- sample_ids

# How many peaks did each sample call independently?
sapply(peak_list, length)
```

**Output:**

```
 severe1  severe2 healthy1 healthy2
  110909    79621   105424   129516
```

The spread is nearly 50,000 peaks between `severe2` and `healthy2`, and it tracks cell number: the samples that captured more cells gave the peak caller more power, so it found more regions. That is the peak-set problem in numeric form, and it is precisely the pattern that would masquerade as biology if we merged without requantifying.

> 

**Using MACS3 peaks instead.** Part 1 also ran MACS3 alongside Cell Ranger, and we found MACS3 peaks were substantially narrower (roughly 460 bp versus 880 bp) because Cell Ranger’s aggregate calling dilutes signal from rare populations. If you would rather build the consensus from MACS3 output, swap the `peaks.bed` path for your `*_peaks.narrowPeak` file and add `col.names` entries for the extra narrowPeak columns. Narrower peaks give sharper motif enrichment later, at the cost of a slightly noisier matrix. Either choice is defensible; be consistent across samples, and say which you used in your methods.

### Merging the Peak Lists into a Union Set

`GenomicRanges::reduce()` takes a set of ranges and collapses every overlapping group into a single range spanning all of them.

```r
#-----------------------------------------------
# STEP 6: Union the per-sample peaks into one consensus set
#-----------------------------------------------

# Concatenate all four GRanges into one, then merge overlapping intervals.
# do.call(c, ...) is the reliable way to concatenate a list of GRanges;
# unname() prevents the list names becoming range names.
# A region called in ANY sample is retained -- a union, not an intersection.
combined_peaks <- reduce(do.call(c, unname(peak_list)))

length(combined_peaks)
```

**Output:**

```
[1] 186150
```

The four samples called 425,470 peaks between them, which collapsed to 186,150 distinct regions — so the samples agree with each other far more than they disagree.

> 

**Union or intersection?** Taking the intersection (regions called in all four samples) would give a smaller, more conservative set, and it is tempting for that reason. Resist it. Intersection systematically discards peaks belonging to rare cell types, because rare populations are exactly the ones that fail to reach significance in the smallest sample. You would be selecting against the biology you most want to find. Union plus requantification is the standard, and the extra regions cost only compute.

### Filtering the Consensus Peaks

The union inherits every problem in the input peak lists, so filter it before quantifying anything. Three filters, applied in order:

```r
#-----------------------------------------------
# STEP 7: Filter consensus peaks by width, chromosome, and blacklist
#-----------------------------------------------

# --- Filter 1: peak width ---------------------------------------------------
# Very wide ranges are usually fused chains of adjacent peaks; very narrow ones
# carry too few fragments to be informative. Signac's recommended bounds.
peak_widths    <- width(combined_peaks)
peaks_filtered <- combined_peaks[peak_widths > 20 & peak_widths < 10000]

# --- Filter 2: standard chromosomes ----------------------------------------
# Drop unplaced scaffolds and alternate haplotypes, which attract spurious
# multi-mapping signal. "coarse" drops whole ranges rather than trimming them.
peaks_filtered <- keepStandardChromosomes(peaks_filtered, pruning.mode = "coarse")

# --- Filter 3: ENCODE blacklist --------------------------------------------
# This is the same ENCFF356LFX file downloaded in Part 2, loaded exactly the
# same way. rtracklayer converts BED's 0-based half-open coordinates to
# GRanges' 1-based closed convention automatically.
blacklist_hg38 <- import(blacklist_bed, format = "bed")

# Restrict the blacklist to standard chromosomes so it matches our peak set,
# then declare the genome to avoid a mismatch warning on overlap operations
blacklist_hg38 <- keepStandardChromosomes(blacklist_hg38, pruning.mode = "coarse")
genome(blacklist_hg38) <- "hg38"
genome(peaks_filtered) <- "hg38"

# subsetByOverlaps(..., invert = TRUE) keeps peaks with NO blacklist overlap
peaks_filtered <- subsetByOverlaps(peaks_filtered, blacklist_hg38, invert = TRUE)

length(peaks_filtered)
```

**Output:**

```
[1] 185581
```

> 

**Why blacklist filtering happens twice.** Part 2 removed blacklisted peaks from each sample’s own peak set, and here we remove them again from the union. This is not redundant. The union was rebuilt from raw `peaks.bed` files, which are the unfiltered Cell Ranger output — so blacklisted regions came straight back in. Filtering the derived object does not filter the source it was derived from. Whenever you rebuild a feature set from raw files, re-apply every filter that set is supposed to satisfy.

### Understanding Peak Fusion: When reduce() Goes Too Far

`reduce()` has a failure mode worth checking for every time. If sample A calls a peak at 1000-1500, sample B calls 1400-1900, and sample C calls 1800-2300, `reduce()` chains all three into a single 1300 bp region — even though no sample ever called a peak that wide. Chains can keep growing, and in some datasets they reach several kilobases.

Fused peaks are not fatal, but they blur resolution: a fused region mixes signal from two independent regulatory elements, which weakens motif enrichment and makes differential accessibility harder to interpret. So look at the width distribution before quantifying anything.

```r
#-----------------------------------------------
# STEP 8: Inspect the consensus peak width distribution
#-----------------------------------------------

width_df <- data.frame(width = width(peaks_filtered))

p_width <- ggplot(width_df, aes(x = width)) +
  geom_histogram(bins = 100, fill = "#3182bd", colour = NA) +
  scale_x_log10(breaks = c(50, 100, 250, 500, 1000, 2500, 5000, 10000)) +
  labs(
    title = "Consensus peak width distribution",
    x     = "Peak width (bp, log scale)",
    y     = "Number of peaks"
  ) +
  theme_bw(base_size = 12)

ggsave(file.path(plot_dir, "01_consensus_peak_widths.png"),
       plot = p_width, width = 7, height = 4.5, dpi = 300)

# Numeric summary alongside the plot
summary(width(peaks_filtered))
```

**Output:**

```
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  105.0   836.0   901.0   887.6   944.0  2598.0
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/01_consensus_peak_widths.png?resize=1024%2C658&ssl=1)

> 

**Reading this figure:** this consensus set has essentially no fusion, and the numbers say so more clearly than the plot does. The interquartile range is 836 to 944 bp — a spread of about 100 bp across the middle half of 185,581 peaks — and the **maximum is 2,598 bp**, nowhere near the 10 kb ceiling. The histogram matches: one sharp mode just under 1 kb with thin tails on both sides.

The reason is Cell Ranger. Its ATAC peak caller produces peaks of fairly uniform width (the ~880 bp figure we measured in Part 1), and because the four samples largely agree on where the peaks are, overlapping calls tend to be near-duplicates of each other rather than staggered chains. `reduce()` merges a peak with its own near-copy from another sample and the result is still about 900 bp wide.

**What a problem would look like:** a second mode above 2 kb, a fat right tail running toward the 10 kb cutoff, or a mean noticeably higher than the median. Here the mean (887.6) sits just *below* the median (901), which tells you the distribution is very slightly left-skewed — the opposite of what fusion produces. Expect a different picture if you build the consensus from MACS3 peaks, which are narrower and more variable in width, and therefore more prone to chaining.

---

## Requantifying Every Sample Against the Consensus Peaks

With the feature space settled, count fragments in every consensus peak, in every QC-passed cell, in every sample. This step turns four incompatible matrices into four compatible ones.

### Creating Fragment Objects for QC-Passed Cells

A `Fragment` object is Signac’s handle on a fragment file: it stores the path, verifies the tabix index exists, records a checksum so you can tell if the file changed, and — critically here — holds the list of cell barcodes to read.

```r
#-----------------------------------------------
# STEP 9: Create Fragment objects restricted to QC-passed cells
#-----------------------------------------------

# Restricting to QC-passed barcodes means we never spend time counting
# fragments for cells Part 2 already discarded.
frag_list <- lapply(sample_ids, function(sid) {
  CreateFragmentObject(
    path  = file.path(cellranger_dir, sid, "outs", "fragments.tsv.gz"),
    cells = colnames(atac_list[[sid]])
  )
})
names(frag_list) <- sample_ids
```

> 

**A note on barcodes.** All four libraries were built with the same 10x barcode whitelist, so the same barcode sequence appears in every sample and refers to a different cell each time. Right now that is fine, because each object is separate. It becomes a collision the moment we merge — which is why the merge step below adds a sample prefix to every cell name.

### Running FeatureMatrix

This is the expensive step. Each call streams one fragment file and tallies overlaps against every consensus peak.

```r
#-----------------------------------------------
# STEP 10: Requantify all samples against the consensus peak set
#-----------------------------------------------

# Same 'features' argument for every sample -- this is what makes the
# resulting matrices directly comparable.
counts_list <- lapply(sample_ids, function(sid) {
  FeatureMatrix(
    fragments = frag_list[[sid]],
    features  = peaks_filtered,
    cells     = colnames(atac_list[[sid]])
  )
})
names(counts_list) <- sample_ids

# Every matrix must now have identical row counts and identical rownames
sapply(counts_list, dim)
identical(rownames(counts_list[[1]]), rownames(counts_list[[4]]))
```

**Output:**

```
      severe1 severe2 healthy1 healthy2
[1,]  185581  185581   185581   185581
[2,]    3485    4773     6861    15452
[1] TRUE
```

The top row is now constant across all four columns, and the row names are identical. That is the whole point of this section: the samples went in measured on four different rulers and came out measured on one. The number of cells in each sample at the bottom row remains same — requantification adds no cells and removes none.

### Rebuilding the Chromatin Assays

Wrap each requantified matrix back into a `ChromatinAssay`, carrying over the per-cell metadata Part 2 computed so that TSS enrichment, FRiP, and nucleosome signal remain available for validating clusters later.

```r
#-----------------------------------------------
# STEP 11: Rebuild Seurat objects on the consensus peak set
#-----------------------------------------------

atac_requant <- lapply(sample_ids, function(sid) {

  # Reattach the same Fragment object so CoveragePlot() can read raw signal
  chrom_assay <- CreateChromatinAssay(
    counts     = counts_list[[sid]],
    fragments  = frag_list[[sid]],
    annotation = annotations
  )

  # Metadata rows are in the same order as matrix columns, because
  # FeatureMatrix() was given colnames() of this very object
  CreateSeuratObject(
    counts    = chrom_assay,
    assay     = "peaks",
    meta.data = atac_list[[sid]]@meta.data
  )
})
names(atac_requant) <- sample_ids

# Confirm the Part 2 QC metrics survived the rebuild
head(colnames(atac_requant[["severe1"]]@meta.data))
```

**Output:**

```
[1] "orig.ident"     "nCount_peaks"   "nFeature_peaks" "total"
[5] "duplicate"      "chimeric"
```

`total`, `duplicate`, and `chimeric` came from Cell Ranger’s per-barcode summary and were attached in Part 2, so seeing them here confirms the metadata survived the rebuild. The columns we actually rely on later — `sample_id`, `condition`, `TSS.enrichment`, `nucleosome_signal`, `pct_reads_in_peaks` — sit further along the same data frame; use `colnames()` without `head()` to see all of them.

> 

**Note on the new count totals.** `nCount_peaks` and `nFeature_peaks` are recomputed against the consensus peak set, so they will not match the values Part 2 reported. They should generally be **higher**, because each cell is now being counted across a larger set of regions. The Part 2 metrics that came from the fragment file rather than the matrix — `TSS.enrichment`, `nucleosome_signal`, `blacklist_ratio` — are unchanged, since they never depended on the peak set.

### Merging the Cohort into One Object

```r
#-----------------------------------------------
# STEP 12: Merge all four samples into a single object
#-----------------------------------------------

# add.cell.ids prefixes every barcode with its sample name, which is what
# prevents the shared 10x whitelist from causing barcode collisions.
atac_merged <- merge(
  x            = atac_requant[[1]],
  y            = atac_requant[-1],
  add.cell.ids = sample_ids
)

# Total peaks x total cells across the cohort
dim(atac_merged)

# Cell counts per sample, and per condition
table(atac_merged$sample_id)
table(atac_merged$condition)

# Barcodes now carry a sample prefix
head(colnames(atac_merged), 3)
```

**Output:**

```
[1] 185581  30571

 healthy1  healthy2   severe1   severe2
     6861     15452      3485      4773

Healthy  Severe
  22313     8258

[1] "severe1_AAACGAAGTCAGGTGA-1" "severe1_AAACGAAGTCCAAGAG-1"
[3] "severe1_AAACGAAGTTTGTACG-1"
```

One object, 185,581 peaks, 30,571 cells, and barcodes that now carry a sample prefix so the shared 10x whitelist cannot cause collisions.

> 

**Note the condition totals: 22,313 Healthy against 8,258 Severe.** The cohort is 2.7-fold imbalanced by condition, on top of being imbalanced by sample. This matters most in differential accessibility between conditions, but it is worth registering now: any cluster-proportion comparison you make between Healthy and Severe is comparing groups of very different size, and small clusters in the Severe group will be noisier simply because there are fewer cells to draw on.

Save the object before the next stage — rebuilding it costs hours.

```bash
#-----------------------------------------------
# STEP 13: Checkpoint the merged object
#-----------------------------------------------

saveRDS(atac_merged, file.path(obj_dir, "atac_merged_consensus.rds"))
```

---

## Normalizing Sparse Chromatin Data with TF-IDF

### Why LogNormalize Fails on Chromatin Accessibility Data

In scRNA-seq you normalize with `LogNormalize`: divide each cell’s counts by its total, multiply by 10,000, add a pseudocount, take the log. It works because RNA counts are genuinely quantitative — a cell can hold 5 or 500 copies of a transcript, and that range carries information.

Chromatin accessibility has no such range. A diploid cell has **two copies** of every locus. A peak is accessible on neither, one, or both — so the count is 0, 1, or 2, and values above 2 are usually PCR duplicates or overlapping fragments rather than biology. The matrix is effectively binary, and it is roughly 97 to 99 percent zeros.

Two things break as a result:

1. **Log-transforming a binary matrix does nothing useful.** There is no dynamic range to compress. `log(1 + 1)` and `log(2 + 1)` differ by a hair, and neither is meaningful.

2. **Every peak looks equally variable.** For a binary variable, variance is `p(1-p)`, entirely determined by the mean. Ranking peaks by variance ranks them by how close their accessibility rate is to 50 percent — which is not the same as ranking them by biological informativeness. Highly-variable-feature selection, the backbone of scRNA-seq, has no traction here.

What actually distinguishes cell types in ATAC data is **which** peaks are open, and how unusual it is for a peak to be open at all. A promoter open in every cell tells you nothing. An enhancer open in 4 percent of cells is highly informative about those cells. That is a problem information retrieval solved decades ago for text, and Signac borrows the solution wholesale.

### The Text Analogy That Makes TF-IDF Click

**TF-IDF** stands for term frequency-inverse document frequency, and it comes from search engines. Treat each **cell as a document** and each **peak as a word**:

- **Term frequency (TF)** normalizes within a document: divide each peak’s count by that cell’s total count. This corrects for sequencing depth — a deeply sequenced cell no longer looks more accessible everywhere simply because it was sequenced harder.

- **Inverse document frequency (IDF)** weights across documents: divide the total number of cells by the number of cells in which that peak is observed, then take the log. Peaks open in nearly every cell get a weight near zero. Peaks open in a small subset get a large weight.

The analogy is exact. In a search index, “the” appears in every document and carries no information, while “chromatin” appears in few and identifies them. In a chromatin matrix, a housekeeping promoter is “the” and a lineage-specific enhancer is “chromatin”.

### Running TF-IDF

```r
#-----------------------------------------------
# STEP 14: TF-IDF normalization
#-----------------------------------------------

DefaultAssay(atac_merged) <- "peaks"

# method = 1 is Stuart & Butler's log(TF x IDF), Signac's default and the
# variant used throughout the published scATAC-seq literature.
atac_merged <- RunTFIDF(atac_merged, method = 1)
```

The result is written to the `data` layer of the `peaks` assay; the raw counts in the `counts` layer are untouched, so nothing is lost.

> 

**The other `method` values.** `method = 2` and `method = 3` apply the log at different points in the calculation, and `method = 4` skips the log entirely (this is the variant used by ArchR and by SnapATAC). They give broadly similar embeddings. Stay on `method = 1` unless you are deliberately reproducing another tool’s pipeline, and record whichever you used — it is a genuine methodological choice that affects your results.

### Selecting Top Features by Prevalence, Not Variance

`FindTopFeatures()` ranks peaks by **total accessibility across cells** and keeps those above a cutoff. Note that this is prevalence-based, not variance-based — the opposite of `FindVariableFeatures()` in scRNA-seq.

```r
#-----------------------------------------------
# STEP 15: Select the peaks that carry the embedding
#-----------------------------------------------

# min.cutoff = 20 keeps peaks observed in at least 20 cells across the whole
# cohort. Peaks below that are dominated by sampling noise and mostly add
# runtime. An integer means "number of cells"; a string like "q5" would
# instead mean "drop the bottom 5 percent by total accessibility".
atac_merged <- FindTopFeatures(atac_merged, min.cutoff = 20)

# How many peaks survived, out of the full consensus set?
length(VariableFeatures(atac_merged))
nrow(atac_merged)
```

**Output:**

```
[1] 184302
[1] 185581
```

> 

**Choosing `min.cutoff`.** The cutoff removed 1,279 peaks out of 185,581 — less than one percent. With 30,571 cells in the cohort, almost every consensus peak is accessible in at least 20 of them, so the filter barely bites. Signac’s vignettes use `min.cutoff = 10`, or `"q0"` to keep everything; on a cohort this size all three choices give nearly the same feature set.

**Scale the cutoff to your cell count, and when in doubt keep more peaks.** Unlike scRNA-seq, where 2,000 variable genes is a sensible default, scATAC-seq embeddings generally improve with more features.

---

## Dimensionality Reduction with SVD and Latent Semantic Indexing

### What SVD Does, and Why Not PCA

**Singular value decomposition (SVD)** factors your normalized matrix into a small number of components that capture as much of its structure as possible. Each cell gets a coordinate along each component — a compact summary of its accessibility profile, going from ~170,000 dimensions down to 50.

If that sounds like PCA, it nearly is. PCA is SVD applied to a **mean-centred** matrix. That centring step is precisely what we cannot afford: subtracting the column mean from a matrix that is 98 percent zeros replaces every zero with a small non-zero value. A sparse matrix that fits comfortably in memory becomes a dense one that does not. For this cohort’s 185,581 peaks by 30,571 cells, the dense version would be roughly 42 GB of doubles.

So we run SVD on the uncentred TF-IDF matrix. The combination — **TF-IDF followed by SVD** — is called **latent semantic indexing (LSI)**, another term inherited from document retrieval, where it was used to find topics shared across documents. Here the “topics” are co-accessible sets of regulatory regions, which is a reasonable operational definition of a cell state.

### Running SVD

```r
#-----------------------------------------------
# STEP 16: SVD to produce the LSI embedding
#-----------------------------------------------

# n = 50 components is standard. The reduction is stored as "lsi" and its
# columns are named LSI_1, LSI_2, ... for clarity in later plots.
atac_merged <- RunSVD(
  object         = atac_merged,
  n              = 50,
  reduction.key  = "LSI_",
  reduction.name = "lsi"
)

# Cells x components
dim(Embeddings(atac_merged, reduction = "lsi"))
```

### Diagnosing Depth Correlation: Why Component 1 Gets Discarded

Here is the single most important quirk of scATAC-seq dimensionality reduction, and the one that most often gets skipped.

Even after TF-IDF, the largest source of variation in the matrix is usually **how many fragments each cell contributed**. TF-IDF divides by each cell’s total, which fixes the scale, but it cannot fix the *pattern*: a cell with 3,000 fragments has non-zero values at 3,000 positions, while a cell with 30,000 has them at ten times as many. That difference in sparsity structure survives normalization, and SVD — which simply finds the biggest axis of variation — picks it up first.

The result is that **LSI component 1 typically measures sequencing depth, not biology**. Keep it and your UMAP arranges cells along a depth gradient; your clusters split shallow cells from deep cells, and you will spend a long time inventing biological stories for a technical artifact.

`DepthCor()` correlates each component with each cell’s total count so you can see this directly.

```r
#-----------------------------------------------
# STEP 17: Check which components track sequencing depth
#-----------------------------------------------

p_depth <- DepthCor(atac_merged, n = 30)

ggsave(file.path(plot_dir, "02_lsi_depth_correlation.png"),
       plot = p_depth, width = 7, height = 4.5, dpi = 300)

# The same information as numbers, which is easier to act on than a plot
lsi_emb   <- Embeddings(atac_merged, reduction = "lsi")
depth_cor <- apply(lsi_emb, 2, function(x) cor(x, atac_merged$nCount_peaks))
round(head(depth_cor, 10), 3)
```

**Output:**

```
 LSI_1  LSI_2  LSI_3  LSI_4  LSI_5  LSI_6  LSI_7  LSI_8  LSI_9 LSI_10
 0.939  0.030  0.079 -0.225 -0.492 -0.362  0.264  0.066 -0.033  0.035
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/02_lsi_depth_correlation.png?resize=1024%2C658&ssl=1)

> 

**Reading this figure:** the x-axis is the LSI component number, the y-axis is its correlation with per-cell sequencing depth. Component 1 lands at **0.939** — the expected spike, and confirmation that it is measuring depth rather than biology. The sign is irrelevant; SVD component signs are arbitrary.

**But the rest of the plot is not the flat band that textbook examples show, and yours may not be either.** Only past component 13 does the band settle near zero. This is common in real cohorts and is worth understanding rather than ignoring: TF-IDF fixes the *scale* of each cell’s profile but cannot fully fix its *sparsity structure*, so residual depth signal leaks into several mid-range components. Some of that leakage is also genuine biology — cell types differ systematically in how many accessible regions they have, and monocytes really do yield more fragments than lymphocytes.

**Act on what you see, not on what you expect.** The usual `dims = 2:30` assumes component 1 is the only depth-driven one. Rather than assume, apply a threshold and look at what it catches:

```r
# Programmatically identify depth-driven components rather than assuming
depth_driven <- which(abs(depth_cor) > 0.5)
depth_driven

# Components to carry forward: 2 through 30, minus anything else depth-driven
use_dims <- setdiff(2:30, depth_driven)
```

In this run, `depth_driven` contains only component 1, so `use_dims` is `2:30` — 29 components.

### Visualizing the Uncorrected UMAP: Does This Dataset Even Need Integration?

Before applying any correction, look at the uncorrected data. Some datasets genuinely do not need integration, and correcting a dataset that has no batch effect actively removes biological signal.

```r
#-----------------------------------------------
# STEP 18: Uncorrected UMAP, to assess the batch effect
#-----------------------------------------------

atac_merged <- RunUMAP(
  object         = atac_merged,
  reduction      = "lsi",
  dims           = use_dims,
  reduction.name = "umap.uncorrected"
)

p_uncor_sample <- DimPlot(atac_merged, reduction = "umap.uncorrected",
                          group.by = "sample_id", shuffle = TRUE, pt.size = 0.1) +
  ggtitle("Uncorrected: by sample") + theme(aspect.ratio = 1)

p_uncor_cond <- DimPlot(atac_merged, reduction = "umap.uncorrected",
                        group.by = "condition", shuffle = TRUE, pt.size = 0.1) +
  ggtitle("Uncorrected: by condition") + theme(aspect.ratio = 1)

p_uncorrected <- p_uncor_sample | p_uncor_cond

ggsave(file.path(plot_dir, "03_umap_uncorrected.png"),
       plot = p_uncorrected, width = 12, height = 5.5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/03_umap_uncorrected.png?resize=1024%2C469&ssl=1)

> 

**Reading this figure:** you are asking one question — do cells group by cell type with all four samples mixed within each group, or do they group by sample?

**This cohort has a strong batch effect, and it is driven almost entirely by `severe1`.** That asymmetry is the diagnosis: this is not four samples each drifting apart, it is **two healthy libraries that agree with each other and two severe libraries that do not agree with anything**, including each other. Left uncorrected, clustering would carve `severe1` into several private clusters and any cell-type proportion comparison would be meaningless.

The right panel is a trap worth naming. Coloured by condition it looks like clean biological separation — Severe here, Healthy there — which is exactly what you would hope to find. But the left panel shows that “Severe” territory is really `severe1` territory. **With two donors per condition, a condition-coloured UMAP cannot distinguish a disease effect from a single unusual donor.** Never read the right panel without the left panel next to it.

Note also that `shuffle = TRUE` matters: without it the last sample plotted covers the others, and a well-mixed UMAP can look sample-specific purely from draw order.

---

## Integration Strategy 1: Harmony

### How Harmony Works

Harmony operates on the low-dimensional embedding, never on the count matrix. It iterates two steps until the cells stop moving:

1. **Soft cluster** the cells in LSI space, with a penalty that rewards clusters containing a mix of samples rather than one sample.

2. **For each cluster, compute a sample-specific correction** — how far `severe1` cells sit from the cluster centroid, how far `healthy2` cells sit — and shift each cell by a blend of those corrections, weighted by how strongly it belongs to each cluster.

Because a cell belongs partially to several clusters, corrections blend smoothly rather than snapping cells into discrete groups. And because Harmony only ever touches a 29-column matrix, it is fast: minutes rather than hours, even for hundreds of thousands of cells.

### Running Harmony on the LSI Embedding

```r
#-----------------------------------------------
# STEP 19: Harmony batch correction on the LSI embedding
#-----------------------------------------------

# group.by.vars = "sample_id" corrects sample-to-sample technical variation.
# dims.use excludes the depth-driven component identified in Step 17, so
# Harmony never clusters cells on sequencing depth.
# project.dim = FALSE skips recomputing peak loadings, which we do not need
# and which is slow on a matrix this wide.
atac_merged <- RunHarmony(
  object         = atac_merged,
  group.by.vars  = "sample_id",
  reduction.use  = "lsi",
  dims.use       = use_dims,
  reduction.save = "harmony",
  project.dim    = FALSE
)

# Harmony returns one corrected component per input component.
# This should equal length(use_dims) -- typically 29, NOT 50.
ncol(Embeddings(atac_merged, reduction = "harmony"))
length(use_dims)
```

**Output:**

```
Harmony converged after 6 iterations
[1] 29
[1] 29
```

Two things to confirm. The convergence message means Harmony reached a stable solution on its own rather than stopping at the iteration limit — if you instead see it run to the maximum without converging, the correction may be incomplete and the result deserves a closer look. And the two numbers agree at 29, which is `length(use_dims)`, confirming `dims.use` was honoured and the depth component never entered the correction.

> 

**Check that number.** Some older Harmony releases silently ignore `dims.use` and correct all 50 components. If the two values above disagree, `dims.use` was not honoured: your harmony reduction still contains the depth component at position 1, and you must use `dims = 2:30` in the next step instead of `dims = 1:length(use_dims)`. Upgrading Harmony is the better fix, but knowing which situation you are in is what matters.

Now build a UMAP from the corrected embedding.

```r
#-----------------------------------------------
# STEP 20: UMAP on the Harmony-corrected embedding
#-----------------------------------------------

# Every column of the harmony reduction is usable, because the depth
# component was excluded before correction rather than after.
harmony_dims <- 1:ncol(Embeddings(atac_merged, reduction = "harmony"))

atac_merged <- RunUMAP(
  object         = atac_merged,
  reduction      = "harmony",
  dims           = harmony_dims,
  reduction.name = "umap.harmony"
)

p_harmony <- DimPlot(atac_merged, reduction = "umap.harmony",
                     group.by = "sample_id", shuffle = TRUE, pt.size = 0.1) +
  ggtitle("Harmony integrated") + theme(aspect.ratio = 1)

ggsave(file.path(plot_dir, "04_umap_harmony.png"),
       plot = p_harmony, width = 6.5, height = 5.5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/04_umap_harmony.png?resize=1024%2C866&ssl=1)

> 

**Reading this figure:** compare it directly against panel 1 of figure 03. Most of the batch effect is gone — the large islands now contain all four colours speckled together, and `severe2`, which had its own territory before correction, is fully absorbed.

**`severe1` is only partly corrected, and that is the honest result.** Harmony pulled `severe1` in where it had counterparts in the other samples and left it alone where it did not.

**That is the correct behaviour, not a failure.** Harmony is deliberately conservative about cells with no counterpart elsewhere — it will not invent a match. Persistent `severe1`-only groups are therefore telling you one of two things: either that sample has a genuinely distinct population, or it has a quality problem the other three do not share. Which of those it is cannot be settled from this figure. We test it directly in Steps 30 and 32.

**What over-correction would have looked like:** the islands collapsing into one another, with the cluster count dropping sharply relative to the uncorrected embedding. That is the more dangerous failure, because a perfectly mixed UMAP looks like success at a glance.

---

## Integration Strategy 2: Seurat Anchors with Reciprocal LSI

### How Reciprocal LSI Projection Works

Seurat’s anchor framework takes a different approach. Rather than modelling a correction, it finds **anchors** — pairs of cells from different samples that are mutual nearest neighbours, meaning each is among the other’s closest cells. An anchor pair is treated as evidence that those two cells are the same cell type observed in two samples, and the offset between them estimates the batch effect at that point in the embedding.

For scRNA-seq, anchors are found using canonical correlation analysis (CCA). For chromatin data, Signac uses **reciprocal LSI projection (`rlsi`)**: each dataset is projected into the other’s LSI space, and neighbours are sought there. This is far more tractable on sparse chromatin data than CCA.

The second departure from scRNA-seq is what actually gets corrected. In transcriptomics, `IntegrateLayers()` produces a corrected expression matrix. Here we use `IntegrateEmbeddings()`, which corrects the **LSI coordinates** and leaves the data matrix alone. As the Signac authors put it, this is much better suited to chromatin data, where the matrix is very sparse with a very large number of features.

### Preparing Each Sample and Finding Anchors

Reciprocal LSI requires an LSI reduction to exist **in every object being integrated**, so each sample needs its own TF-IDF and SVD first.

```r
#-----------------------------------------------
# STEP 21: Compute per-sample LSI, required for reciprocal projection
#-----------------------------------------------

# Split the merged object back into per-sample objects. Splitting the merged
# object (rather than reusing atac_requant) guarantees every object carries
# the same consensus features and the same prefixed cell names.
atac_split <- SplitObject(atac_merged, split.by = "sample_id")

# Each sample gets its own TF-IDF, feature selection, and SVD
atac_split <- lapply(atac_split, function(obj) {
  obj <- RunTFIDF(obj, method = 1)
  obj <- FindTopFeatures(obj, min.cutoff = 20)
  RunSVD(obj)
})
```

**You will see a warning here, once per sample:**

```
Warning in RunTFIDF.default(object = LayerData(object = object, layer = "counts"),  :
  Some features contain 0 total counts
```

This is expected, and it is worth two minutes to understand rather than dismiss. Every consensus peak *was* measured in every sample — but for some peaks the measurement came back empty. Splitting the merged object asks each sample to normalize the full 185,581-peak union on its own, which is when that emptiness becomes visible.

```r
#-----------------------------------------------
# STEP 22: Find integration anchors using reciprocal LSI
#-----------------------------------------------

# anchor.features = rownames(): all consensus peaks are shared by construction,
# so there is no feature-intersection step as there would be for scRNA-seq.
# dims = 2:30 skips the depth component within each sample's own LSI.
integration_anchors <- FindIntegrationAnchors(
  object.list     = atac_split,
  anchor.features = rownames(atac_merged),
  reduction       = "rlsi",
  dims            = 2:30
)
```

### Integrating the Embeddings

```r
#-----------------------------------------------
# STEP 23: Correct the LSI embedding using the anchors
#-----------------------------------------------

# 'reductions' supplies the uncorrected merged embedding to be corrected.
# dims.to.integrate = 1:30 covers the components we will then subset from.
atac_merged_rlsi <- IntegrateEmbeddings(
  anchorset          = integration_anchors,
  reductions         = atac_merged[["lsi"]],
  new.reduction.name = "integrated_lsi",
  dims.to.integrate  = 1:30
)

# Carry the corrected embedding back onto the main object so both integration
# results live side by side and can be compared directly
atac_merged[["integrated_lsi"]] <- atac_merged_rlsi[["integrated_lsi"]]

atac_merged <- RunUMAP(
  object         = atac_merged,
  reduction      = "integrated_lsi",
  dims           = 2:30,
  reduction.name = "umap.rlsi"
)

p_rlsi <- DimPlot(atac_merged, reduction = "umap.rlsi",
                  group.by = "sample_id", shuffle = TRUE, pt.size = 0.1) +
  ggtitle("Seurat anchors (rLSI)") + theme(aspect.ratio = 1)

ggsave(file.path(plot_dir, "05_umap_rlsi.png"),
       plot = p_rlsi, width = 6.5, height = 5.5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/05_umap_rlsi.png?resize=1024%2C866&ssl=1)

> 

**Reading this figure:** the rLSI result is broadly comparable to Harmony’s, with the same caveat. The four samples mix well through most of the map, and `severe1` again retains partly distinct territory. Two methods with quite different mathematics arriving at the same residual is meaningful: it makes a technical artifact of either algorithm unlikely and points back at the sample itself.

The geometry differs from the Harmony map — groups are arranged differently and some are more compact — but that is UMAP layout, not a difference in what was found. Never compare two UMAPs by their shapes; compare them by which cells end up together, which is what the mixing score below does numerically.

> 

**Note on `dims = 2:30` here.** Unlike the Harmony branch, `IntegrateEmbeddings()` corrects the components it is given and returns them in the same positions, so component 1 of `integrated_lsi` is still the depth-driven component. It must be excluded at the UMAP and clustering stage, exactly as for the uncorrected LSI.

---

## Harmony vs Seurat Anchors: Which Integration Method Should You Choose?

### Comparing the Two Results Visually

```r
#-----------------------------------------------
# STEP 24: Three-way comparison of the integration strategies
#-----------------------------------------------

p_compare <- (p_uncor_sample | p_harmony | p_rlsi) +
  plot_annotation(title = "Integration strategy comparison, coloured by sample")

ggsave(file.path(plot_dir, "06_integration_comparison.png"),
       plot = p_compare, width = 16, height = 5.5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/06_integration_comparison.png?resize=1024%2C352&ssl=1)

> 

**Reading this figure:** Do not try to pick a winner by eye here. The panels have different UMAP layouts, which makes visual comparison close to meaningless, and overplotting hides a great deal at this point density. The numbers below are the actual comparison.

### Measuring Batch Mixing Quantitatively

Eyeballing a UMAP is genuinely unreliable, because point overplotting hides a lot. A simple numeric check is more honest: for each cell, ask what fraction of its nearest neighbours come from a different sample.

```r
#-----------------------------------------------
# STEP 25: Quantify batch mixing in each embedding
#-----------------------------------------------

# For each cell, the proportion of its k nearest neighbours from other samples.
# Under perfect mixing this approaches 1 - sum(sample proportions squared).
mixing_score <- function(obj, reduction, dims, k = 30) {

  # return.neighbor = TRUE gives a Neighbor object, whose Indices() is a
  # cells x k integer matrix -- far faster to work with than an SNN graph.
  nn  <- FindNeighbors(
    Embeddings(obj, reduction)[, dims, drop = FALSE],
    k.param = k, return.neighbor = TRUE, verbose = FALSE
  )
  idx <- Indices(nn)
  sid <- obj$sample_id

  # Column 1 is the cell itself, so drop it before comparing labels.
  # Recycling compares each row of neighbour labels against that cell's own.
  neighbour_labels <- matrix(sid[idx[, -1]], nrow = nrow(idx))
  mean(rowMeans(neighbour_labels != sid))
}

mixing <- c(
  uncorrected = mixing_score(atac_merged, "lsi", use_dims),
  harmony     = mixing_score(atac_merged, "harmony", harmony_dims),
  rlsi        = mixing_score(atac_merged, "integrated_lsi", 2:30)
)
round(mixing, 3)

# The ceiling: expected value if samples were mixed completely at random
p_sample <- prop.table(table(atac_merged$sample_id))
round(1 - sum(p_sample^2), 3)
```

**Output:**

```
uncorrected     harmony        rlsi
      0.162       0.485       0.477

[1] 0.657
```

> 

**How to read these numbers.** The last value, 0.657, is the ceiling — what you would score if sample identity were shuffled at random. It is not 0.75 because the cohort is unbalanced: `healthy2` is half the cells, so even under random mixing a cell’s neighbours are often `healthy2`. **Compute this ceiling for your own cohort rather than assuming a value; it depends entirely on your sample proportions.**

The uncorrected score of **0.162** is the batch effect in one number. Under random mixing a cell would have 66 percent of its neighbours from other samples; in the uncorrected LSI space it has 16 percent. That is a severe batch effect, and it confirms what the left panel of figure 06 showed.

Both corrections roughly **triple** it, to 0.485 (Harmony) and 0.477 (rLSI). The 0.008 gap between them is not a meaningful difference — treat the two as tied on this metric, and choose on the other criteria in the table below.

**Neither reaches the ceiling, and that is the right outcome.** A score at 0.657 would mean every population had been homogenized across samples, including any that are genuinely sample-specific. The gap between 0.485 and 0.657 is largely `severe1` refusing to be merged — which, as the UMAPs showed, is exactly where both methods left structure standing. **A large improvement that stops short of the ceiling is what good integration looks like.** Be suspicious of a score that arrives at the ceiling, not reassured by it.

### Choosing Between Them

**Runtime****Memory****Scales to many samples****Strength of correction**`theta`**Risk of over-correction****Handles unshared cell types****Multiple covariates at once**`group.by.vars = c("sample_id", "chemistry")`**Reproducibility across runs**

| Consideration | Harmony | Seurat anchors (rLSI) |
| --- | --- | --- |
|  | Minutes | Tens of minutes to hours |
|  | Low — operates on a 29-column matrix | High — holds all objects plus anchor sets |
|  | Yes, dozens to hundreds | Degrades; anchor finding is pairwise |
|  | Moderate, tunable via | Stronger by default |
|  | Lower | Higher, especially with few cells per sample |
|  | Well — cells with no counterpart are largely left alone | Less well — can force spurious anchors |
|  | Yes, | Awkward |
|  | Deterministic with a fixed seed | Deterministic with a fixed seed |

**The practical recommendation for this tutorial series: use Harmony.** It is faster, gentler, scales as your cohorts grow, and handles the case where one sample contains a population the others lack — which in a disease cohort is often the finding you are looking for. The Signac authors give the same advice for integrating more than two datasets.

**Reach for anchors when** you have a strong batch effect that Harmony under-corrects, when you are mapping a query dataset onto a curated reference (the anchor framework extends naturally to label transfer, which we use in Part 4), or when the samples come from genuinely different technologies — scATAC-seq combined with the ATAC half of a multiome experiment, for instance.

**What you must not do** is run both, compare downstream biological results, and keep whichever gives a more publishable answer. Choose on the criteria above, before you look at the biology, and report the choice.

We continue with the Harmony embedding for the rest of this tutorial.

---

## Clustering scATAC-seq Cells Across Multiple Resolutions

### Building the Neighbor Graph

Clustering in Seurat and Signac is graph-based, and it happens in two stages. First `FindNeighbors()` builds a **shared nearest neighbor (SNN) graph**: every cell is connected to its `k` closest neighbours in the corrected embedding, and each edge is weighted by how many neighbours the two cells have in common. Then `FindClusters()` partitions that graph into communities — groups of cells more densely connected to each other than to the rest.

```r
#-----------------------------------------------
# STEP 26: Build the SNN graph on the corrected embedding
#-----------------------------------------------

# Note reduction = "harmony": clustering on the corrected embedding is the
# whole point of integration. Using "lsi" here would silently undo it.
atac_merged <- FindNeighbors(
  object    = atac_merged,
  reduction = "harmony",
  dims      = harmony_dims,
  k.param   = 20
)
```

> 

**`k.param` in sparse data.** The default of 20 works well for cohorts of this size. Larger values (30 to 50) smooth the graph and favour fewer, larger clusters — sometimes helpful in scATAC-seq, where sparsity makes individual cells noisy. Smaller values (10 to 15) preserve fine structure but are more likely to fragment a cell type into spurious subclusters. Change `k.param` before you reach for extreme resolutions; the two knobs interact.

### Sweeping Across Clustering Resolutions

`resolution` controls granularity: lower values give fewer, larger clusters, higher values give more, smaller ones. There is no correct value — it depends on whether you want broad lineages or fine cell states. So rather than guessing once, compute several and compare.

```r
#-----------------------------------------------
# STEP 27: Cluster at a range of resolutions in one call
#-----------------------------------------------

res_seq <- c(0.2, 0.4, 0.6, 0.8, 1.0, 1.2)

# algorithm = 3 is SLM (smart local moving), Signac's recommendation for
# chromatin data -- it is less prone than Louvain to fragmenting the sparse,
# noisy graphs that scATAC-seq produces.
# Passing a vector of resolutions stores one metadata column per value.
atac_merged <- FindClusters(
  object     = atac_merged,
  algorithm  = 3,
  resolution = res_seq
)

# How many clusters does each resolution produce?
res_cols  <- paste0("peaks_snn_res.", res_seq)
n_cluster <- sapply(res_cols, function(cl) length(unique(atac_merged[[cl]][, 1])))
data.frame(resolution = res_seq, n_clusters = n_cluster, row.names = NULL)
```

**Output:**

```
  resolution n_clusters
1        0.2         14
2        0.4         17
3        0.6         21
4        0.8         22
5        1.0         24
6        1.2         26
```

> 

**The shape of this table is informative.** The count climbs smoothly from 14 to 26, with no jump large enough to suggest an unstable graph.

### Choosing a Resolution with a Clustering Tree

A **clustering tree** shows how clusters split as resolution increases. Each row is one resolution, each node is a cluster, and each edge shows cells flowing from a cluster at one resolution into clusters at the next.

```r
#-----------------------------------------------
# STEP 28: Clustering tree across the resolution sweep
#-----------------------------------------------

# prefix must match the metadata column names, which Seurat builds from the
# assay name: assay "peaks" gives graph "peaks_snn" gives "peaks_snn_res.".
p_tree <- clustree(atac_merged, prefix = "peaks_snn_res.")

ggsave(file.path(plot_dir, "07_clustering_tree.png"),
       plot = p_tree, width = 9, height = 10, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/07_clustering_tree.png?resize=922%2C1024&ssl=1)

> 

**Reading this figure:** you are looking for the resolution at which the tree stops behaving cleanly. Clean splitting means one cluster divides into two, both of which persist as resolution rises — real substructure being resolved. **Crossing edges**, where a cluster receives cells from two or more parents, mean cells are being reshuffled rather than subdivided; that is the signature of over-clustering. Edge transparency encodes the proportion of cells flowing along each edge, so faint tangled edges are low-volume noise.

**We proceed at 0.4**, the last resolution before the first crossing edge appears. It gives 17 clusters — enough to separate the major PBMC lineages and their main subsets on a 30,000-cell cohort — while every cluster is still a clean descendant of the level above it.

```r
#-----------------------------------------------
# STEP 29: Fix the working resolution
#-----------------------------------------------

chosen_res <- 0.4

# Set both the active identity and a clearly named metadata column, so later
# code never has to guess which resolution the analysis was run at.
Idents(atac_merged)        <- paste0("peaks_snn_res.", chosen_res)
atac_merged$cluster_final  <- Idents(atac_merged)

table(atac_merged$cluster_final)
```

**Output:**

```
   0    1    2    3    4    5    6    7    8    9   10   11   12   13   14   15
5116 4376 4013 3828 3148 1931 1690 1585 1217  863  768  731  459  378  210  171
  16
  87
```

> 

**This is a working resolution, not a final answer.** Resolution selection is genuinely iterative. In Part 4, marker-based annotation may reveal that two clusters share every marker (drop the resolution) or that one cluster contains two clearly different cell types (raise it). Keeping all six resolution columns in the object — which we do — means revisiting the choice later costs one line rather than a re-run.

### Checking Cluster Composition Across Samples

The critical validation: does every cluster contain cells from every sample? A cluster made almost entirely of one sample is either residual batch effect or a genuine sample-specific population, and telling those apart is the whole game.

```r
#-----------------------------------------------
# STEP 30: Cluster composition by sample and by condition
#-----------------------------------------------

comp_df <- as.data.frame(table(
  cluster = atac_merged$cluster_final,
  sample  = atac_merged$sample_id
))

p_comp <- ggplot(comp_df, aes(x = cluster, y = Freq, fill = sample)) +
  geom_col(position = "fill") +
  geom_hline(yintercept = cumsum(rev(prop.table(table(atac_merged$sample_id)))),
             linetype = "dashed", colour = "grey30") +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title    = "Sample composition of each cluster",
    subtitle = "Dashed lines mark whole-cohort proportions",
    x        = "Cluster",
    y        = "Proportion of cells"
  ) +
  theme_bw(base_size = 12)

ggsave(file.path(plot_dir, "08_cluster_composition.png"),
       plot = p_comp, width = 9, height = 5, dpi = 300)

# The same check as numbers: the largest single-sample share per cluster
comp_mat  <- table(atac_merged$cluster_final, atac_merged$sample_id)
max_share <- round(apply(prop.table(comp_mat, 1), 1, max), 3)
sort(max_share, decreasing = TRUE)
```

**Output:**

```
   12    13     6     5     7    11     1    16    14     9     3     0    15
0.998 0.995 0.811 0.738 0.655 0.651 0.588 0.529 0.500 0.496 0.474 0.470 0.468
    2     4     8    10
0.464 0.450 0.437 0.344
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/08_cluster_composition.png?resize=1024%2C569&ssl=1)

> 

**Reading this figure and table:**

**Two clusters are unambiguous outliers: cluster 12 (0.998) and cluster 13 (0.995) are essentially pure `severe1`.** Together they hold 837 cells. Look back at the UMAP: cluster 12 and 13 are precisely the regions Harmony declined to merge, and the composition table now confirms why — there was nothing to merge them with.

**What to do about clusters 12 and 13.** Work through three explanations in order, and do not delete anything yet:

1. **Residual technical batch effect.** If so, these clusters should also stand out on quality metrics. Step 32 tests this directly, and it is the next thing we do.

2. **A genuine donor-specific population.** Entirely plausible in human PBMCs, where clonal T cell expansions, CMV serostatus, and HLA-driven variation routinely produce populations present in one donor and absent in another.

3. **A real disease-associated population**, which in a severe-COVID cohort is exactly what you would hope to find.

> 

**This dataset cannot distinguish (2) from (3), and it is important to be blunt about that.** **With n = 2 per condition, donor identity and disease status are confounded by design.** Carry these clusters forward, annotate them, and report them as hypotheses for a larger cohort rather than as findings.

---

## Visualizing the Integrated and Clustered Dataset

### The Core Cluster Map

```r
#-----------------------------------------------
# STEP 31: UMAP by cluster, sample, and condition
#-----------------------------------------------

p_final_cluster <- DimPlot(atac_merged, reduction = "umap.harmony",
                           group.by = "cluster_final", label = TRUE,
                           label.size = 4, pt.size = 0.1) +
  ggtitle(paste0("Clusters (resolution ", chosen_res, ")")) +
  theme(aspect.ratio = 1) + NoLegend()

p_final_sample <- DimPlot(atac_merged, reduction = "umap.harmony",
                          group.by = "sample_id", shuffle = TRUE, pt.size = 0.1) +
  ggtitle("Sample") + theme(aspect.ratio = 1)

p_final_cond <- DimPlot(atac_merged, reduction = "umap.harmony",
                        group.by = "condition", shuffle = TRUE, pt.size = 0.1) +
  ggtitle("Condition") + theme(aspect.ratio = 1)

p_final_panel <- p_final_cluster | p_final_sample | p_final_cond

ggsave(file.path(plot_dir, "09_umap_clusters_final.png"),
       plot = p_final_panel, width = 16, height = 5.5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/09_umap_clusters_final.png?resize=1024%2C352&ssl=1)

> 

**Reading this figure:** the left panel is the map you will annotate later. The clusters are visually coherent — compact, contiguous, and separated — rather than scattered speckles overlapping each other, which is what you want to see before investing in annotation.

The middle panel repeats the integration check at the resolution actually chosen: the large central and upper islands carry all four colours, with the `severe1`-only regions we already identified still visible at the edges.

The right panel needs restraint. It looks like clean condition separation in places, but the middle panel shows that most of the apparent “Severe” territory is `severe1` specifically. **In a two-versus-two design, a condition-coloured UMAP cannot separate disease from donor.** Treat it as a figure that generates hypotheses, never one that tests them.

### Overlaying QC Metrics to Rule Out Technical Clusters

Before trusting any cluster, confirm it is not simply a group of low-quality cells that survived filtering and found each other in the embedding.

```r
#-----------------------------------------------
# STEP 32: Project Part 2 QC metrics onto the integrated UMAP
#-----------------------------------------------

p_qc_umap <- FeaturePlot(
  object    = atac_merged,
  features  = c("nCount_peaks", "TSS.enrichment",
                "nucleosome_signal", "pct_reads_in_peaks"),
  reduction = "umap.harmony",
  pt.size   = 0.1,
  ncol      = 2
) & theme(aspect.ratio = 1)

ggsave(file.path(plot_dir, "10_umap_qc_metrics.png"),
       plot = p_qc_umap, width = 11, height = 9.5, dpi = 300)

# Per-cluster medians, which are easier to compare than four colour scales
qc_by_cluster <- aggregate(
  cbind(nCount_peaks, TSS.enrichment, nucleosome_signal, pct_reads_in_peaks) ~
    cluster_final,
  data = atac_merged@meta.data, FUN = median
)
qc_by_cluster
```

**Output (abbreviated to the four columns that matter):**

```
   cluster_final nCount_peaks TSS.enrichment nucleosome_signal pct_reads_in_peaks
1              0       5000.5       7.636990         0.4630698           73.79558
2              1       4392.5       6.717988         0.4966956           62.10034
3              2       3568.0       6.263655         0.6004902           46.14470
4              3       3925.0       7.263852         0.5480132           64.57970
5              4       4567.5       7.341144         0.5057230           70.42144
6              5       4210.0       7.613256         0.4781705           71.66524
7              6       4267.5       7.366842         0.5128464           69.25034
8              7       3025.0       5.327203         0.8160920           32.06054
9              8       4101.0       7.392607         0.5112179           64.53988
10             9       5017.0       4.325555         0.5964249           20.29784
11            10       4821.5       6.757879         0.5363358           63.33895
12            11       6551.0       6.149406         0.4888158           66.31548
13            12       6134.0       5.712520         0.5051760           61.22265
14            13       3770.0       5.739969         0.5196010           59.88144
15            14       4807.0       7.642821         0.4728046           73.07846
16            15       3105.0       5.808333         0.8344051           28.55194
17            16       5133.0       6.234791         0.4737440           59.35370
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/10_umap_qc_metrics.png?resize=1024%2C884&ssl=1)

> 

**Reading this figure and table:** the `pct_reads_in_peaks` panel is the informative one, and it is not smooth. The table names the clusters responsible (7, 9, 15).

These clusters move together on exactly the metrics that describe **signal-to-background ratio**: fewer fragments landing in peaks, weaker enrichment at transcription start sites, and a fragment size distribution shifted toward mononucleosomal. That is the profile of nuclei with elevated background transposition — damaged or dying cells, or cells whose chromatin was more accessible to Tn5 everywhere rather than specifically at regulatory elements.

**What to do about it, and what not to do.** These cells passed Part 2’s per-cell thresholds individually; it is only in aggregate, once clustering grouped them, that the pattern became visible. That is precisely why this check exists.

Do not delete them here. Two considerations argue for carrying them into Part 4:

- **They are not one sample’s junk.** Whatever this is, it is a property of the cell population rather than of a run.

- **Some cell types genuinely look like this.** Granulocytes and monocytes have less compact chromatin and lower TSS enrichment than lymphocytes, and low-density granulocytes are expanded in severe COVID. A low-FRiP cluster is not automatically debris.

The honest position is that these clusters are **flagged, not resolved**. Annotate them in Part 4: if they carry coherent lineage signal, they are cells with a distinctive chromatin profile; if they carry no marker signal at all and look like a smear across the map, they are background and should be removed before differential analysis.

### A First Look at Biology: Coverage Plots at Marker Loci

Nothing so far has confirmed that clusters correspond to real cell types. `CoveragePlot()` is the quickest check available: it draws raw Tn5 insertion pileups, per cluster, across a genomic window, directly from the fragment files. If clusters are biologically real, a well-known lineage gene will show a sharp promoter signal in one cluster and flat background in the others.

```r
#-----------------------------------------------
# STEP 33: Raw accessibility at canonical PBMC marker loci
#-----------------------------------------------

# CD3E: T cells. MS4A1: B cells. LYZ: monocytes. NKG7: NK cells.
marker_genes <- c("CD3E", "MS4A1", "LYZ", "NKG7")

# Each region is drawn separately, saved with a distinct object name
invisible(lapply(marker_genes, function(g) {
  p_cov <- CoveragePlot(
    object            = atac_merged,
    region            = g,
    extend.upstream   = 2000,
    extend.downstream = 2000
  )
  ggsave(file.path(plot_dir, paste0("11_coverage_", g, ".png")),
         plot = p_cov, width = 8, height = 8, dpi = 300)
}))
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/11_coverage_marker_gene.png?resize=1024%2C678&ssl=1)

> 

**Reading these figures:** each row is one cluster and the height of the trace is normalized accessibility. The grey bars at the bottom mark consensus peaks, so you can see which signal was actually quantified.

**`LYZ` and `MS4A1` behave exactly as hoped.** `LYZ` shows a tall sharp peak at its promoter in cluster 1, with clusters 12, 13, 16, and 7 also carrying clear signal, while cluster 0 is flat. `MS4A1` shows the reverse: strong, well-defined signal in clusters 8, 10, and 11 and essentially nothing in cluster 1. Two marker loci, two disjoint sets of clusters, clean separation. **This is the first hard evidence that the clusters track lineage identity rather than sample or depth** — no amount of batch effect would produce that pattern.

**`CD3E` teaches the more valuable lesson, because it is messier.** The promoter peak on the right of the panel is present in *every* cluster, including the ones that are clearly B cells and monocytes by `MS4A1` and `LYZ`. If you were expecting “tall in T cells, flat elsewhere”, this looks like a failure. It is not.

Promoter accessibility means a gene is **available** for transcription, not that it is being transcribed. Lineage-defining promoters are frequently accessible in related haematopoietic lineages and in progenitors that never express the gene — accessibility is permissive, expression is the further step. What actually discriminates here are the **distal elements**, the three peaks around 118,304,000 to 118,307,000 on the left: those are strong in some clusters and absent in others. Distal regulatory elements are far more cell-type-restricted than promoters, which is the general rule in ATAC data and the reason enhancer-focused analysis is where scATAC-seq earns its keep.

`NKG7` is intermediate, with the strongest signal in clusters 0, 2, 5, 14, and 15 — consistent with the cytotoxic compartment, which spans both NK cells and cytotoxic T cells and so should not map to a single cluster.

**These are sanity checks, not annotation.** Four loci examined by eye cannot assign identities to 17 clusters. Part 4 does this systematically with gene activity scores across hundreds of markers and label transfer from an annotated reference.

### Saving the Integrated Object

```bash
#-----------------------------------------------
# STEP 34: Save the integrated, clustered object for Part 4
#-----------------------------------------------

saveRDS(atac_merged, file.path(obj_dir, "atac_integrated_clustered.rds"))

# The consensus peak set is worth saving separately: Part 4 calls
# cell-type-specific peaks against it, and it is expensive to rebuild.
saveRDS(peaks_filtered, file.path(obj_dir, "consensus_peaks.rds"))

# Cell-level metadata as a flat table, for record keeping and quick lookups
write.csv(atac_merged@meta.data,
          file.path(integ_dir, "integrated_cell_metadata.csv"))

# Record exact package versions for the methods section of your paper
writeLines(capture.output(sessionInfo()),
           file.path(integ_dir, "sessionInfo_part3.txt"))
```

---

## Best Practices for scATAC-seq Integration and Clustering

**1. Always requantify against a consensus peak set.** Merging per-sample matrices on shared peak names produces zeros that mean “not measured” and zeros that mean “closed”, perfectly confounded with sample identity. No batch correction method can repair information that was never collected. This is the single most consequential difference from scRNA-seq integration, and skipping it invalidates everything downstream.

**2. Take the union of peaks, not the intersection.** Intersection systematically removes peaks belonging to rare cell types, because rare populations are exactly the ones that fail to reach significance in your smallest sample. You would be selecting against the biology you most want to find.

**3. Inspect the consensus peak width distribution before quantifying.** `reduce()` chains overlapping peaks, and long chains produce multi-kilobase features that blur two regulatory elements into one. Filter to 20 bp to 10 kb, look at the histogram, and consider `disjoin()` or an ArchR-style fixed-width procedure if fusion is severe.

**4. Never assume component 1 is the only depth-driven one — check.** Run `DepthCor()` and read the numbers rather than reflexively typing `dims = 2:30`. The convention is right most of the time, and the two-line check that confirms it costs nothing compared to an analysis that unknowingly clusters on sequencing depth.

**5. Look at the uncorrected UMAP first.** Integration is a correction, and every correction removes signal along with noise. If your samples already mix well, integrating anyway costs you biology. Make the decision from evidence.

**6. Exclude the depth component before batch correction, not after.** Harmony’s soft-clustering step uses every component you give it. Feeding it a depth-driven component means it partly corrects batches on the basis of sequencing depth. Pass `dims.use` explicitly, then verify the returned embedding has the expected number of columns.

**7. Prefer Harmony for cohorts of more than two samples.** It is faster, uses less memory, scales to dozens of samples, and is less likely to over-correct genuinely sample-specific populations. Reserve anchor-based integration for reference mapping, for cross-technology integration, and for cases where Harmony visibly under-corrects.

**8. Measure batch mixing numerically, not only by eye.** Overplotting on a UMAP hides a great deal, and the difference between good and marginal integration is often invisible. A nearest-neighbour mixing score takes ten lines and gives you something reportable.

**9. Sweep resolutions and keep every column.** Store the full sweep in the object rather than committing to one value. Cluster resolution is a judgement that should be revisited after annotation, and keeping the sweep makes revisiting it a one-line change.

**10. Validate clusters against QC metrics before interpreting them.** Project `TSS.enrichment`, `nucleosome_signal`, and `nCount_peaks` onto the final UMAP. A cluster defined by a quality metric rather than by accessibility is a technical artifact that survived filtering, and finding it now is much cheaper than finding it after you have written a discussion section about it.

**11. Check cell-number balance before interpreting any proportion.** Cell recovery varies several-fold between libraries for reasons that have nothing to do with biology — this cohort ranges from 3,485 to 15,452 cells per sample. That imbalance shifts the expected composition of every cluster and lowers the ceiling on every mixing metric. Compute your cohort proportions once and put them on the figure.

**12. Set a seed and record `sessionInfo()`.** UMAP and graph-based clustering are stochastic. Signac, Seurat, and Harmony all change behaviour between releases. Six months from now, `sessionInfo_part3.txt` is the only reliable record of what you actually ran.

**13. Be honest about what a small cohort can support.** With two donors per condition, sample and condition are confounded by construction. A cluster restricted to your two severe samples is a hypothesis worth pursuing, not a result. Report it that way.

---

## Common Pitfalls and How to Avoid Them

`FeatureMatrix()``dims = 1:30``DepthCor()``reduce()``peaks.bed``reduce()``disjoin()``FindNeighbors()``pca``reduction = "harmony"``FindNeighbors()``RunUMAP()``min.cutoff``min.cutoff``workers``plan()``plan("sequential")``future.globals.maxSize`

| Pitfall | Why it happens | What it looks like | How to avoid it |
| --- | --- | --- | --- |
| Merging without requantification | Merging feels like the scRNA-seq workflow, where it is correct | Clusters split almost perfectly by sample; batch correction barely helps | Build a consensus peak set and run  on every sample |
| Using  after LSI | Copied from a PCA-based scRNA-seq script | UMAP shows a smooth gradient; clusters separate shallow from deep cells | Run , drop every depth-correlated component |
| Taking the intersection of peak sets | Seems more rigorous and conservative | Rare cell types vanish; cluster count is lower than expected | Union with , then filter by width and blacklist |
| Not re-applying the blacklist to the union | is raw Cell Ranger output, unfiltered | A handful of clusters driven by a few enormous artifact peaks | Filter the consensus set independently of any per-sample filtering |
| Extreme peak fusion from | Many overlapping peaks chain together | Width histogram has a fat tail or a second mode past 2 kb | Enforce the 20 bp to 10 kb bounds; use  if fusion persists |
| Clustering on the uncorrected reduction | defaults to , and the argument is easy to forget | UMAP looks integrated but clusters still split by sample | Pass  to , not just to |
| Over-correction mistaken for success | A perfectly mixed UMAP looks like a job well done | Distinct populations merged into one blob; cluster count drops sharply | Compare cluster counts and island structure before and after; a mixing score at the theoretical ceiling is a warning |
| set too aggressively | Borrowing the “2,000 variable genes” instinct from scRNA-seq | Rare populations disappear; UMAP looks unusually smooth | Keep more peaks than feels necessary;  of 10 to 20 cells is generous by design |
| Interpreting clusters as cell types from peaks alone | Peaks lack the immediate readability of marker genes | Confident lineage claims with no supporting evidence | Defer annotation to Part 4; treat coverage plots as sanity checks only |
| Silencing the zero-count warning by filtering per sample | The warning looks alarming and the fix looks obvious | Feature space becomes a function of each library’s cell count; the consensus set is quietly undone | Leave them; they cannot enter the embedding anyway. Filter cohort-wide or not at all |
| Reading cluster composition against an equal-share baseline | Assuming four samples means 25 percent each | Every cluster looks biased toward the largest sample | Compare against actual cohort proportions; plot them as reference lines |
| Deleting a low-quality-looking cluster immediately | It looks like obvious debris | A real cell type with naturally low TSS enrichment disappears | Flag it, annotate it in Part 4, and remove only if it carries no lineage signal |
| Reading condition differences from n = 2 per group | The UMAP makes the difference look obvious | Condition-specific clusters reported as disease biology | State the confound explicitly; treat as hypothesis-generating |
| Choosing the integration method by downstream result | The “better” method is the one giving a nicer answer | Unreproducible, unreportable analysis | Choose on cohort size, runtime, and correction strength, before looking at biology |
| Running out of memory during requantification | Each parallel worker holds its own copy of a large matrix | Session killed by the scheduler, often without a clear error | Lower  in , or use ; raise |

---

## Conclusion & Key Takeaways

- **The consensus peak set is the step that has no scRNA-seq counterpart**, and skipping it is the most common way scATAC-seq integration fails silently.

- **LSI component 1 is sequencing depth, not biology** — until you have checked and confirmed it, on your own data.

- **Integration is a correction, and corrections remove signal.** Look at the uncorrected embedding first, and treat a perfectly mixed result with suspicion rather than satisfaction.

- **Clusters are not cell types yet.** They are groups of cells with similar accessibility profiles. Turning them into biology is Part 4.

- **Promoter accessibility is permissive, not diagnostic.** `CD3E`‘s promoter was open in every cluster in this dataset. Distal regulatory elements carry the cell-type specificity, and that is where scATAC-seq adds information transcriptomics cannot.

- **A residual that two independent methods both leave standing is data, not algorithm.** Harmony and reciprocal LSI disagree about almost everything mathematically, and both declined to merge `severe1`. That agreement is what made the finding worth pursuing rather than debugging.

---

## References

1. Stuart T, Srivastava A, Madad S, Lareau CA, Satija R. Single-cell chromatin state analysis with Signac. *Nature Methods*. 2021;18(11):1333-1341. doi:10.1038/s41592-021-01282-5

2. Korsunsky I, Millard N, Fan J, et al. Fast, sensitive and accurate integration of single-cell data with Harmony. *Nature Methods*. 2019;16(12):1289-1296. doi:10.1038/s41592-019-0619-0

3. Stuart T, Butler A, Hoffman P, et al. Comprehensive Integration of Single-Cell Data. *Cell*. 2019;177(7):1888-1902.e21. doi:10.1016/j.cell.2019.05.031

4. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y

5. Cusanovich DA, Daza R, Adey A, et al. Multiplex single-cell profiling of chromatin accessibility by combinatorial cellular indexing. *Science*. 2015;348(6237):910-914. doi:10.1126/science.aab1601

6. Granja JM, Corces MR, Pierce SE, et al. ArchR is a scalable software package for integrative single-cell chromatin accessibility analysis. *Nature Genetics*. 2021;53(3):403-411. doi:10.1038/s41588-021-00790-6

7. Blondel VD, Guillaume JL, Lambiotte R, Lefebvre E. Fast unfolding of communities in large networks. *Journal of Statistical Mechanics*. 2008;2008(10):P10008. doi:10.1088/1742-5468/2008/10/P10008

8. Waltman L, van Eck NJ. A smart local moving algorithm for large-scale modularity-based community detection. *European Physical Journal B*. 2013;86:471. doi:10.1140/epjb/e2013-40829-0

9. Zappia L, Oshlack A. Clustering trees: a visualization for evaluating clusterings at multiple resolutions. *GigaScience*. 2018;7(7):giy083. doi:10.1093/gigascience/giy083

10. McInnes L, Healy J, Melville J. UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. *arXiv*. 2018. doi:10.48550/arXiv.1802.03426

11. Amemiya HM, Kundaje A, Boyle AP. The ENCODE Blacklist: Identification of Problematic Regions of the Genome. *Scientific Reports*. 2019;9(1):9354. doi:10.1038/s41598-019-45839-z

12. Lawrence M, Huber W, Pages H, et al. Software for computing and annotating genomic ranges. *PLoS Computational Biology*. 2013;9(8):e1003118. doi:10.1371/journal.pcbi.1003118

13. Luecken MD, Buttner M, Chaichoompu K, et al. Benchmarking atlas-level data integration in single-cell genomics. *Nature Methods*. 2022;19(1):41-50. doi:10.1038/s41592-021-01336-8

14. Signac documentation and vignettes: merging objects, and scATAC-seq data integration. [https://stuartlab.org/signac/](https://stuartlab.org/signac/) (2026)

15. 10x Genomics. Cell Ranger ATAC Algorithms Overview: Peak Calling. [https://www.10xgenomics.com/support/software/cell-ranger-atac/latest](https://www.10xgenomics.com/support/software/cell-ranger-atac/latest) (2026)

---

*This tutorial is part of the comprehensive NGS101.com single-cell ATAC-seq analysis series for beginners.*
