# How to Analyze Single-Cell ATAC-seq Data: A Complete Beginner's Guide Part 4: Cell Type Identification

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** August 27, 2026
- **Categories:** ATAC-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/)

---

You have 30,571 cells sorted into 17 clusters. Every cluster is a real group of cells that share an open-chromatin landscape. And not one of them has a name.

That is where [Part 3](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/) left off, and it is where every scATAC-seq analysis stalls if you stop there. “Cluster 4 is more accessible than cluster 9 at 1,204 regions” is not a finding. “Memory CD4 T cells from severe COVID-19 patients show increased accessibility at 1,204 regions” is a finding. The distance between those two sentences is cell type identification, and this tutorial closes it.

In scRNA-seq this step is almost pleasant. You run `FindAllMarkers()`, you see `CD3E` and `IL7R` at the top of a cluster, you type “T cells”, and you move on. In scATAC-seq the same instinct fails, because the features are not genes. They are 185,581 anonymous genomic intervals, most of them tens of kilobases from the nearest gene, and none of them arrive with a label saying which cell type they belong to.

This tutorial walks through every practical route out of that problem: borrowing labels from an annotated scRNA-seq dataset, borrowing them from bulk profiles or chromatin atlases when no single-cell reference exists, and reading cell identity directly out of the chromatin using marker gene activity, cluster marker peaks, and transcription factor motifs. It then reconciles all of that evidence into one annotation, including honest decisions about the clusters where the evidence does not converge.

**Every number and every code block below comes from a real run on real data.** That includes the results that are messy, the clusters that stayed unresolved, and the methods that performed worse than their reputation suggests.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/)

- [🧭 Introduction: Why Naming Cells Is the Hardest Step in scATAC-seq](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%A7%AD_Introduction_Why_Naming_Cells_Is_the_Hardest_Step_in_scATAC-seq)

  - [How Cell Type Identification in scATAC-seq Differs from scRNA-seq](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#How_Cell_Type_Identification_in_scATAC-seq_Differs_from_scRNA-seq)
  - [The Two Roads: Annotation With and Without a Reference](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#The_Two_Roads_Annotation_With_and_Without_a_Reference)
  - [What You Will Build in This Tutorial](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#What_You_Will_Build_in_This_Tutorial)

- [⚙️ Setting Up the R Environment for scATAC-seq Annotation](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%E2%9A%99%EF%B8%8F_Setting_Up_the_R_Environment_for_scATAC-seq_Annotation)

  - [Adding the Part 4 Packages to Your Existing Pixi Environment](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Adding_the_Part_4_Packages_to_Your_Existing_Pixi_Environment)
  - [Finishing the Install: Running the Post-Link Scripts](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Finishing_the_Install_Running_the_Post-Link_Scripts)
  - [Loading Libraries and Creating the Part 4 Directory Structure](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Loading_Libraries_and_Creating_the_Part_4_Directory_Structure)

- [📦 Example Data: The Part 3 Object and the Reference Dataset](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%93%A6_Example_Data_The_Part_3_Object_and_the_Reference_Dataset)

  - [Loading the Integrated, Clustered Object from Part 3](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Loading_the_Integrated_Clustered_Object_from_Part_3)
  - [Choosing and Downloading the Reference Dataset](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Choosing_and_Downloading_the_Reference_Dataset)

- [🧬 Translating Chromatin into Genes: Gene Activity Scores](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%A7%AC_Translating_Chromatin_into_Genes_Gene_Activity_Scores)

  - [What a Gene Activity Score Actually Measures](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#What_a_Gene_Activity_Score_Actually_Measures)
  - [Computing the Gene Activity Matrix](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Computing_the_Gene_Activity_Matrix)
  - [Sanity-Checking Gene Activity on Canonical Markers](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Sanity-Checking_Gene_Activity_on_Canonical_Markers)

- [🔗 Reference-Based Annotation: Transferring Labels from scRNA-seq](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%94%97_Reference-Based_Annotation_Transferring_Labels_from_scRNA-seq)

  - [How Cross-Modality Label Transfer Works](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#How_Cross-Modality_Label_Transfer_Works)
  - [Finding Transfer Anchors](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Finding_Transfer_Anchors)
  - [Transferring the Cell Type Labels](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Transferring_the_Cell_Type_Labels)
  - [Reading the Prediction Scores: Which Calls Can You Trust?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Reading_the_Prediction_Scores_Which_Calls_Can_You_Trust)
  - [Converting Per-Cell Predictions into Per-Cluster Labels](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Converting_Per-Cell_Predictions_into_Per-Cluster_Labels)

- [🗂️ When No scRNA-seq Reference Exists: Three Alternative Reference Types](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%97%82%EF%B8%8F_When_No_scRNA-seq_Reference_Exists_Three_Alternative_Reference_Types)

  - [Option 1: Bulk RNA-seq and Microarray Profiles with SingleR](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Option_1_Bulk_RNA-seq_and_Microarray_Profiles_with_SingleR)
  - [Option 2: Public Epigenomic Maps](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Option_2_Public_Epigenomic_Maps)
  - [Option 3: Cross-Species Label Transfer](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Option_3_Cross-Species_Label_Transfer)
  - [Which External Reference Should You Reach For?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Which_External_Reference_Should_You_Reach_For)

- [🔍 Reference-Free Annotation: Letting the Chromatin Speak for Itself](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%94%8D_Reference-Free_Annotation_Letting_the_Chromatin_Speak_for_Itself)

  - [Comparing the Reference-Free Strategies](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Comparing_the_Reference-Free_Strategies)
  - [Method 1: Marker Gene Activity Scores for Broad Lineages](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Method_1_Marker_Gene_Activity_Scores_for_Broad_Lineages)
  - [Method 2: Cluster Marker Peaks and Their Nearest Genes](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Method_2_Cluster_Marker_Peaks_and_Their_Nearest_Genes)
  - [Method 3: Transcription Factor Motif Enrichment in Cluster Marker Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Method_3_Transcription_Factor_Motif_Enrichment_in_Cluster_Marker_Peaks)
  - [A Structural Check: Where Do the Marker Peaks Sit Relative to Genes?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#A_Structural_Check_Where_Do_the_Marker_Peaks_Sit_Relative_to_Genes)

- [🧩 Building the Final Consensus Annotation](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%A7%A9_Building_the_Final_Consensus_Annotation)

  - [Assembling the Evidence Table](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Assembling_the_Evidence_Table)
  - [Handling the Unresolved and Low-Quality Clusters](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Handling_the_Unresolved_and_Low-Quality_Clusters)
  - [Assigning the Final Labels](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Assigning_the_Final_Labels)

- [📊 Visualizing the Annotated Dataset](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%93%8A_Visualizing_the_Annotated_Dataset)

  - [The Annotated UMAP](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#The_Annotated_UMAP)
  - [Marker Gene Activity Across Cell Types](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Marker_Gene_Activity_Across_Cell_Types)
  - [Cell Type Composition by Sample](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Cell_Type_Composition_by_Sample)
  - [Coverage Plots at Definitive Marker Loci](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Coverage_Plots_at_Definitive_Marker_Loci)
  - [Annotation Confidence by Cluster and Label](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Annotation_Confidence_by_Cluster_and_Label)

- [💾 Saving Your Annotated Object](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%92%BE_Saving_Your_Annotated_Object)
- [✅ Best Practices for scATAC-seq Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%E2%9C%85_Best_Practices_for_scATAC-seq_Cell_Type_Annotation)
- [⚠️ Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%E2%9A%A0%EF%B8%8F_Common_Pitfalls_and_How_to_Avoid_Them)
- [🎓 Conclusion: From Anonymous Clusters to Named Cell Types](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%8E%93_Conclusion_From_Anonymous_Clusters_to_Named_Cell_Types)

  - [What You Now Have](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#What_You_Now_Have)

- [🚀 Moving Forward: What Comes in Part 5](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%9A%80_Moving_Forward_What_Comes_in_Part_5)
- [📚 References](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#%F0%9F%93%9A_References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-4-cell-type-identification/#Like_this)

## 🧭 Introduction: Why Naming Cells Is the Hardest Step in scATAC-seq

### How Cell Type Identification in scATAC-seq Differs from scRNA-seq

The gap between the two assays is not a matter of difficulty. It is a matter of **what the measurement is made of**.

In scRNA-seq, the feature space is genes, and biologists have spent forty years building a shared vocabulary that maps genes to cell identity. `MS4A1` means B cell. `LYZ` means monocyte. `NKG7` means cytotoxic lymphocyte. That vocabulary is the reason annotation feels easy: the hard work was done by decades of immunology, and the analysis just looks the answer up.

In scATAC-seq, the feature space is peaks — regions of open chromatin discovered in the data itself. No equivalent vocabulary exists. Nobody has published a lookup table saying “chr11-60455000-60455800 means B cell”, because that interval is not a gene, it is a regulatory element, and its identity depends on the genome build, the peak caller, and the tissue.

Four consequences follow, and they shape everything below.

**Feature identity****Marker vocabulary****Signal per feature****Distance to biology***permission***Regulatory reach**

| Aspect | scRNA-seq | scATAC-seq | Practical consequence |
| --- | --- | --- | --- |
|  | Genes, universally named and annotated | Peaks, defined per dataset | No portable marker list exists at the feature level |
|  | Decades of curated marker genes | Essentially none at peak level | Chromatin must be translated into gene space first |
|  | Counts, 0 to thousands | Near-binary, 0 or 1 or 2 | Single-cell marker signal is far noisier |
|  | Expression is the phenotype | Accessibility is  for the phenotype | An open promoter does not mean the gene is transcribed |
|  | Gene body defines the feature | Enhancers act over 10 kb to 1 Mb | Nearest-gene assignment is often wrong |

The fourth row is the one beginners underestimate, so it is worth stating plainly.

> 

**Accessibility is permission, not expression.** A promoter can be fully open in a cell that transcribes the gene at zero copies. Chromatin opens ahead of transcription during differentiation, stays open after transcription stops, and in lymphocytes many lineage promoters are held in a permissive state across several lineages at once. Part 3 demonstrated this directly: the `CD3E` promoter was accessible in every cluster, including unambiguous B cells and monocytes. Promoter accessibility is a weak discriminator. **Distal elements are where cell identity actually lives.**

The other consequence is sparsity. In a typical scRNA-seq cell you detect 1,000 to 5,000 genes. In a scATAC-seq cell you observe fragments in perhaps 3,000 to 15,000 peaks out of 185,581, and each observation is a 1 rather than a graded count. Ask “is `MS4A1` accessible in this cell?” and for most individual cells the honest answer is “no fragments were sampled there, and it is impossible to say why.”

**This is why almost all scATAC-seq annotation happens at the cluster level.** Pooling a few thousand cells converts a binary, sparse, uninterpretable signal into a smooth, quantitative one. Every strategy in this tutorial exploits that, and any strategy that claims to work reliably cell-by-cell deserves suspicion.

### The Two Roads: Annotation With and Without a Reference

There are two fundamentally different ways to give a cluster a name, and the choice turns on one question: **does an annotated dataset of the same tissue already exist?**

**Road 1: reference-based annotation (label transfer).** Take a dataset where somebody has already assigned cell types, find corresponding cells between that dataset and yours, and copy the labels across. The reference is usually scRNA-seq, which means the transfer must cross modalities — and that crossing is the whole technical problem.

```
chromatin peaks -> gene activity scores -> shared space with scRNA-seq -> anchors -> transferred labels -> per-cluster consensus
```

**Road 2: reference-free annotation.** With no reference, or with references that do not contain your cell types, interrogate the chromatin directly using prior biological knowledge: which marker genes have accessible loci, which peaks distinguish each cluster and what genes they sit near, which transcription factor motifs are enriched in those peaks.

```
chromatin peaks -> marker gene activity + cluster marker peaks + motif enrichment -> manual lineage assignment
```

Here is how to choose.

**Both**

| Situation | Recommended road | Why |
| --- | --- | --- |
| Well-characterized tissue (PBMC, brain, gut), good reference exists | Reference-based, validated reference-free | Fastest and most reproducible, with validation catching reference gaps |
| Matched scRNA-seq from the same donors or study | Reference-based | Best possible case; batch and donor effects mostly cancel |
| Reference exists but from a different disease state | Reference-based, but expect gaps | Disease-specific states are forced into the nearest healthy label |
| Non-model organism or poorly annotated genome | Reference-free | Gene activity scores are unreliable when gene models are wrong |
| Novel or engineered cell populations | Reference-free | By definition no reference contains them |
| Any published analysis |  | Reviewers ask, and agreement between independent methods is the strongest evidence available |

> 

**For most projects the answer is “both.”** Label transfer is fast and reproducible but inherits every quirk, gap and mislabel of the reference. Reference-free annotation is laborious and subjective but sees what is actually in the data. Running both is not redundant work — it is the only way to find out where the reference falls short. In this dataset the two roads disagree on five of seventeen clusters, and those disagreements turn out to be the most instructive part of the analysis.

### What You Will Build in This Tutorial

Starting from the Part 3 object, you will produce:

- A **gene activity assay** translating 185,581 peaks into 19,723 gene-level scores

- **Transferred cell type labels** from an annotated PBMC scRNA-seq reference, with per-cell confidence scores

- An **independent SingleR annotation** built from bulk sorted-population immune profiles

- **Marker panel accessibility scores** for twelve PBMC lineages

- **Marker peaks** for each cluster, with their nearest genes

- **Transcription factor motif enrichment** within each cluster’s marker peaks

- A **final consensus annotation** reconciling all of the above, including explicit decisions about the clusters that could not be resolved

- Publication-quality figures: annotated UMAP, marker dot plot, composition barplots, coverage tracks, and confidence overlays

> 

**Prerequisites.** This tutorial assumes completion of [Part 1: From FASTQ to Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/), [Part 2: Thorough Quality Control with Signac](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/), and [Part 3: Integration and Clustering](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-3-integration-and-clustering/). You need the saved object `atac_integrated_clustered.rds`, the original Cell Ranger fragment files (gene activity scoring reads them again), the Pixi environment from Part 1, and roughly 64 GB of RAM plus 100 GB of free disk. Readers who have worked through [Part 4 of the scRNA-seq series](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/) will find the *logic* familiar even though almost none of the *code* is the same.

---

## ⚙️ Setting Up the R Environment for scATAC-seq Annotation

### Adding the Part 4 Packages to Your Existing Pixi Environment

Continue in the **same Pixi environment** built in [Part 1](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/) and extended in Parts 2 and 3. Signac, Seurat, Harmony, GenomicRanges, ggplot2, patchwork and dplyr are already installed. Do not build a new environment — the Part 3 object was serialized against these package versions, and reloading it under a different Seurat build is a common source of cryptic errors in a multi-part analysis.

Four new capabilities are needed, and nothing else:

```bash
# Move into the shared scATAC-seq environment created in Part 1
cd /projects/mylab/shared/scatac-analysis

# SingleR + celldex : annotation against bulk / sorted-population reference profiles
# motifmatchr + TFBSTools + JASPAR2020 : transcription factor motif enrichment
# BSgenome hg38 : the genome sequence needed to scan peaks for motif matches
pixi add \
  bioconductor-singler \
  bioconductor-celldex \
  bioconductor-motifmatchr \
  bioconductor-tfbstools \
  bioconductor-jaspar2020 \
  bioconductor-bsgenome.hsapiens.ucsc.hg38

# Confirm the solve succeeded before starting a long job
pixi list | grep -E "singler|celldex|motifmatchr|jaspar|bsgenome"
```

> 

`BSgenome.Hsapiens.UCSC.hg38` is roughly 700 MB because it contains the entire human genome sequence, which Signac needs to read the DNA under each peak and match it against motif matrices.

### Finishing the Install: Running the Post-Link Scripts

`pixi add` alone is not enough for three of these packages. `BSgenome.Hsapiens.UCSC.hg38`, `JASPAR2020` and `celldex` are Bioconductor *data* packages, too large to ship through a package channel. Bioconda distributes them as small stubs carrying a `post-link.sh` script that downloads the real content at install time — and Pixi does not run link scripts. The stub installs, `pixi list` reports success, and R still cannot find the package.

Run the scripts yourself. `PREFIX` must be set, since Conda would normally provide it:

```
pixi shell

for f in "$CONDA_PREFIX"/bin/.*post-link.sh; do
  PREFIX="$CONDA_PREFIX" bash "$f"
done
```

Make it a task in `pixi.toml`, because this is needed after every `pixi add`:

```
[tasks]
postlink = """
for f in $PIXI_PROJECT_ROOT/.pixi/envs/default/bin/.*post-link.sh; do
  PREFIX=$PIXI_PROJECT_ROOT/.pixi/envs/default bash "$f"
done
"""
```

Then verify, before committing to any long job:

```
pixi run Rscript -e 'sapply(c("BSgenome.Hsapiens.UCSC.hg38", "JASPAR2020", "celldex", "SingleR", "motifmatchr", "TFBSTools"), requireNamespace, quietly = TRUE)'
```

All six must return `TRUE`. If any is `FALSE`, its pinned version has aged off Bioconductor’s release path and the script’s URL now 404s; install that one through R instead, on a login node:

```r
options(timeout = 3600)
BiocManager::install("JASPAR2020", lib = .libPaths()[1])
```

**For reference, the versions this tutorial was tested against:**

```r
R 4.5.3              Bioconductor 3.22        Signac 1.17.1
Seurat 5.5.1         SeuratObject 5.4.0       SingleR 2.12.0
celldex 1.20.0       JASPAR2020 0.99.10       TFBSTools 1.48.0
motifmatchr 1.32.0   BSgenome.Hsapiens.UCSC.hg38 1.4.5
```

Launch R inside the environment as before:

```
pixi run R
```

### Loading Libraries and Creating the Part 4 Directory Structure

```r
#-----------------------------------------------
# STEP 1: Load all packages used in this tutorial
#-----------------------------------------------

library(Signac)
library(Seurat)
library(GenomicRanges)
library(BSgenome.Hsapiens.UCSC.hg38)
library(motifmatchr)
library(TFBSTools)
library(JASPAR2020)
library(SingleR)
library(celldex)
library(ggplot2)
library(patchwork)
library(dplyr)

# future is not used directly below, but loading it makes plan() available if a
# step needs to be forced back to sequential execution (see Troubleshooting).
library(future)

# Reproducibility: UMAP, anchor finding and background peak selection are all
# stochastic. The same seed is used across every part of this series.
set.seed(1234)

# Anchor finding and motif scanning move large objects between parallel workers.
options(future.globals.maxSize = 60 * 1024^3)
```

Part 4 output goes in its own directory, a sibling of `results/`, `downstream_qc/` and `integrated/`.

```r
#-----------------------------------------------
# STEP 2: Define and create the project directories
#-----------------------------------------------

base_dir   <- "/projects/mylab/shared/scatac_tutorial"
integ_dir  <- file.path(base_dir, "integrated")
annot_dir  <- file.path(base_dir, "annotation")

# Subdirectories: objects for RDS files, plots for figures, reference for
# downloads, tables for the CSV summaries that become supplementary files.
obj_dir    <- file.path(annot_dir, "objects")
plot_dir   <- file.path(annot_dir, "plots")
ref_dir    <- file.path(annot_dir, "reference")
table_dir  <- file.path(annot_dir, "tables")

# recursive = TRUE creates parents; showWarnings = FALSE keeps reruns quiet.
invisible(lapply(
  c(annot_dir, obj_dir, plot_dir, ref_dir, table_dir),
  dir.create, recursive = TRUE, showWarnings = FALSE
))
```

The directory tree now looks like this:

```
/projects/mylab/shared/scatac_tutorial/
|-- results/               # Cell Ranger ATAC output (Part 1)
|-- downstream_qc/         # per-sample QC objects (Part 2)
|-- integrated/            # consensus peaks, integrated object (Part 3)
|   `-- objects/
|       |-- atac_integrated_clustered.rds
|       `-- consensus_peaks.rds
`-- annotation/            # this tutorial
    |-- objects/
    |-- plots/
    |-- reference/
    `-- tables/
```

---

## 📦 Example Data: The Part 3 Object and the Reference Dataset

### Loading the Integrated, Clustered Object from Part 3

```r
#-----------------------------------------------
# STEP 3: Load the integrated object produced by Part 3
#-----------------------------------------------

atac <- readRDS(file.path(integ_dir, "objects", "atac_integrated_clustered.rds"))

# Peaks stay the default assay until gene activity scores are built.
DefaultAssay(atac) <- "peaks"

# The working identity is the resolution 0.4 clustering chosen in Part 3.
Idents(atac) <- "cluster_final"

# Confirm the object is what Part 3 saved: dimensions, reductions, identities.
dim(atac)
names(atac@reductions)

# The gene annotation carried over from Part 3, which GeneActivity(),
# ClosestFeature() and CoveragePlot() all read from the object.
length(Annotation(atac))

table(atac$cluster_final)
```

**Output:**

```
[1] 185581  30571

[1] "lsi"  "harmony"  "integrated_lsi"  "umap"  "umap.harmony"  "umap.rlsi"

[1] 3870812

   0    1    2    3    4    5    6    7    8    9   10   11   12   13   14   15
5116 4376 4013 3828 3148 1931 1690 1585 1217  863  768  731  459  378  210  171
  16
  87
```

> 

**Reading this output:** 185,581 consensus peaks across 30,571 cells, six dimensionality reductions, 3,870,812 exon-level annotation ranges, and 17 clusters — exactly the state Part 3 ended in. If `length(Annotation(atac))` returns `0`, stop: `GeneActivity()` in Step 6 will fail silently.

Three facts from Part 3 matter repeatedly below:

1. **The corrected embedding is `harmony`**, and every one of its components is usable, because the depth-driven component was excluded before correction rather than after. `harmony_dims` is therefore `1:29`, not `2:30`.

2. **Clusters 7, 9 and 15 are QC-suspect**, sharing a low-FRiP, low-TSS-enrichment, high-nucleosome-signal signature — cluster 9 sits at 20.3 percent fraction of reads in peaks against a typical 60 to 74 percent. They span all four samples, so this is not one library failing.

3. **Clusters 12 and 13 are approximately 99.8 percent and 99.5 percent `severe1`** — a single donor — but with entirely ordinary QC metrics (66 and 61 percent FRiP).

Recover the Harmony dimensions so the rest of the tutorial can reference them:

```r
#-----------------------------------------------
# STEP 4: Recover the corrected embedding dimensions from Part 3
#-----------------------------------------------

# Harmony returned one corrected component per input component (29 of them).
# All are usable because component 1 of LSI was dropped before correction.
harmony_dims <- 1:ncol(Embeddings(atac, reduction = "harmony"))

length(harmony_dims)
```

**Output:**

```
[1] 29
```

### Choosing and Downloading the Reference Dataset

The ideal reference would be scRNA-seq from the same cohort: same donors, same disease states, same sample preparation. The study behind GSE282769 did generate it, but deposited only raw reads (BioProject PRJNA1164162), with no processed matrix and no cell type annotations. There is nothing to transfer from.

An external reference is needed instead — the situation almost every reader will be in anyway, since matched scRNA-seq from your own donors is rare. The query is human PBMCs, so the natural substitute is the annotated 10x Genomics 10k PBMC v3 dataset processed and labeled by the Satija lab, which is the reference used in Signac’s own PBMC vignettes. It carries 14 cell type labels at a granularity roughly matching what 30,571 scATAC-seq cells can resolve.

```bash
# Run from a login node with internet access, not a compute node
cd /projects/mylab/shared/scatac_tutorial/annotation/reference

# Annotated PBMC scRNA-seq reference (10x v3 chemistry, ~9,400 cells)
wget https://signac-objects.s3.amazonaws.com/pbmc_10k_v3.rds

# Confirm the download completed rather than truncating
ls -lh pbmc_10k_v3.rds
```

```r
#-----------------------------------------------
# STEP 5: Load and inspect the annotated scRNA-seq reference
#-----------------------------------------------

pbmc_rna <- readRDS(file.path(ref_dir, "pbmc_10k_v3.rds"))

# The object predates Seurat 5, so migrate its internal structure. This does not
# change any data -- it rewrites slot layout for the current class definitions.
pbmc_rna <- UpdateSeuratObject(pbmc_rna)

# What cell types does the reference actually contain, and in what numbers?
sort(table(pbmc_rna$celltype), decreasing = TRUE)
```

**Output:**

```
       CD14+ Monocytes             CD4 Memory              CD4 Naive
                  2992                   1596                   1047
            pre-B cell Double negative T cell      B cell progenitor
                   959                    592                    460
...
                   pDC               Platelet
                    68                     52
```

**Truncated: 14 cell types.**

> 

**Reading this table, and one important warning about it.** The composition is a healthy adult PBMC profile: monocytes and CD4 T cells dominate, dendritic cells and platelets are rare. That is the right shape for the query. But two labels deserve scrutiny: **“B cell progenitor” and “pre-B cell” are misnomers here.** True B cell progenitors and pre-B cells reside in bone marrow, not peripheral blood. These two populations are, biologically, naive and memory B cells that were given developmental names during the original annotation.

**This cannot be fixed from the outside, and it is precisely why transferred labels must never be accepted uncritically.** Whatever those labels mean, they will be copied verbatim onto the ATAC cells — and Step 13 will show exactly that happening. Every reference carries inherited baggage of this kind: naming conventions from a particular lab, granularity choices, and populations merged or split for study-specific reasons. Read the reference’s label set *before* transferring, not after writing the results section.

Two further gaps shape what appears later:

1. **The reference is from healthy donors.** Two of the four query samples are severe COVID-19 patients. Disease-specific cell states have no corresponding cells in the reference. Label transfer cannot return a label that does not exist; it forces those cells into the nearest healthy match and reports a mediocre confidence score. **Low prediction scores are therefore data, not noise.**

2. **The reference contains no granulocytes.** PBMC preparation is designed to exclude them, but severe COVID-19 is associated with low-density granulocytes that can survive Ficoll separation. Whether any are present in this dataset is an empirical question, and Step 17 answers it.

---

## 🧬 Translating Chromatin into Genes: Gene Activity Scores

Every reference-based method in this tutorial, and one of the three reference-free ones, depends on a single bridge: converting peak accessibility into something with gene names attached.

### What a Gene Activity Score Actually Measures

A **gene activity score** is the number of ATAC fragments falling within a gene’s body plus its promoter region, counted per cell. That is the whole definition. `GeneActivity()` extends each gene’s coordinates 2 kb upstream to capture the promoter, then counts fragments in the resulting interval using the same `FeatureMatrix()` machinery that built the consensus peak matrix in Part 3.

It is a *proxy* for expression, and a deliberately crude one. Three limitations carry through the rest of the analysis:

- **It ignores distal enhancers.** A gene controlled by an enhancer 200 kb away gets no credit for it.

- **It cannot represent repression.** Accessibility has a floor at zero and no negative range. A gene that is actively silenced looks identical to a gene that is simply not being examined.

- **It correlates with gene length.** Longer genes accumulate more fragments by being larger targets. Normalization mitigates this but does not remove it.

> 

**Why use it anyway?** It is the only thing that puts chromatin and transcriptome in the same coordinate system, and for “is this cluster lymphoid or myeloid?” it works. Correlation with matched scRNA-seq is modest per gene — typically 0.3 to 0.5 — but higher for the cell-type-specific markers annotation relies on. **Use it to decide lineage, not to make quantitative claims about expression.**

### Computing the Gene Activity Matrix

```r
#-----------------------------------------------
# STEP 6: Build the gene activity matrix from fragment files
#-----------------------------------------------

# GeneActivity() re-reads every fragment file listed in the object's Fragment
# objects, so the paths recorded in Part 2 must still be valid. Gene bodies come
# from the annotation attached in Part 3; extend.upstream = 2000 adds the promoter.
DefaultAssay(atac) <- "peaks"

gene_activities <- GeneActivity(
  object            = atac,
  extend.upstream   = 2000,
  extend.downstream = 0
)

dim(gene_activities)
```

**Output:**

```
Warning messages:
1: In SingleFeatureMatrix(fragment = fragments[[x]], features = features,  :
  13 features are on seqnames not present in the fragment file. These will be removed.
...

[1] 19723 30571
```

> 

**The warning is expected**, repeating once per fragment file: thirteen genes sit on contigs absent from the Cell Ranger output. Thousands would signal a genome build or chromosome naming mismatch; thirteen is a handful of unplaced scaffolds.

Now attach the matrix as a second assay and normalize it. Gene activity behaves like count data, not like binary accessibility, so it receives standard log-normalization rather than TF-IDF.

```r
#-----------------------------------------------
# STEP 7: Add gene activity as an assay and normalize it
#-----------------------------------------------

atac[["ACTIVITY"]] <- CreateAssayObject(counts = gene_activities)

DefaultAssay(atac) <- "ACTIVITY"

# scale.factor set to the median per-cell total, following Signac's convention.
# Using the default 10,000 would over-inflate cells with few fragments.
atac <- NormalizeData(
  object               = atac,
  assay                = "ACTIVITY",
  normalization.method = "LogNormalize",
  scale.factor         = median(atac$nCount_ACTIVITY)
)

# Variable features and scaling are needed for anchor finding in the next section.
atac <- FindVariableFeatures(atac, assay = "ACTIVITY", nfeatures = 3000)
atac <- ScaleData(atac, assay = "ACTIVITY", features = rownames(atac[["ACTIVITY"]]))

median(atac$nCount_ACTIVITY)
```

**Output:**

```
[1] 5660
```

> 

**Two assays now coexist in the object.** `peaks` holds the 185,581-feature accessibility matrix and remains the source of truth for anything involving genomic coordinates. `ACTIVITY` holds the 19,723-gene proxy and is used only for gene-level reasoning. Switching `DefaultAssay()` between them is a constant source of confusion, so make the switch explicit at the top of each block rather than relying on whatever it happened to be.

### Sanity-Checking Gene Activity on Canonical Markers

Before trusting gene activity for anything consequential, look at it. If well-known PBMC markers do not light up in visually coherent regions of the UMAP, something is wrong upstream and no downstream cleverness will rescue it.

```r
#-----------------------------------------------
# STEP 8: Visualize canonical marker gene activity on the UMAP
#-----------------------------------------------

DefaultAssay(atac) <- "ACTIVITY"

# One clear marker per major PBMC lineage.
sanity_markers <- c("CD3E", "CD8A", "MS4A1", "LYZ", "NKG7", "FCGR3A")

p_sanity <- FeaturePlot(
  object     = atac,
  features   = sanity_markers,
  reduction  = "umap.harmony",
  pt.size    = 0.05,
  max.cutoff = "q95",   # clip the top 5 percent so outliers do not flatten the scale
  ncol       = 3
) & theme(aspect.ratio = 1)

ggsave(file.path(plot_dir, "01_marker_gene_activity_sanity.png"),
       plot = p_sanity, width = 13, height = 8, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/01_marker_gene_activity_sanity.png?resize=1024%2C630&ssl=1)

> 

**Reading this figure:** the thing to check is *spatial coherence*, not brightness. Each marker should be elevated across a contiguous region of the UMAP rather than scattered at random. `LYZ` should mark one large territory (monocytes), `MS4A1` a separate compact island (B cells), and `NKG7` should overlap partially with `CD8A` because cytotoxic T cells and NK cells share that program.

**`CD3E` will look disappointing, and that is expected.** As Part 3 showed with coverage plots, the `CD3E` promoter is accessible in nearly every cluster, so its gene activity is broadly elevated rather than T-cell-restricted. This is the “accessibility is permission” problem in its purest form, and the best argument for never annotating scATAC-seq from one marker.

**What genuine failure looks like:** every marker uniformly grey (gene names in the annotation are not matching, revisit Part 3), or every marker uniformly bright everywhere (this is sequencing depth, not biology — check that `NormalizeData` actually ran on the `ACTIVITY` assay).

---

## 🔗 Reference-Based Annotation: Transferring Labels from scRNA-seq

### How Cross-Modality Label Transfer Works

Seurat’s transfer framework finds **anchors**: pairs of cells, one from the reference and one from the query, that are mutual nearest neighbors in a shared low-dimensional space. Each anchor pair is treated as evidence that those two cells are the same cell type observed through two different instruments. Labels then flow from reference cells to query cells, weighted by how close each query cell sits to each anchor.

Two parameter choices distinguish cross-modality transfer from ordinary scRNA-seq-to-scRNA-seq transfer, and both are easy to get wrong.

**First, `reduction = "cca"`.** Within a single modality, Seurat projects the reference’s PCA structure onto the query. Across modalities that fails, because gene activity and gene expression have different scales, different noise structures and different dynamic ranges — the reference’s principal components simply do not describe the query. **Canonical correlation analysis** instead finds the axes of maximum correlation *between* the two datasets, which is the shared structure that survives the modality change.

**Second, `weight.reduction` must point at an ATAC-derived embedding.** After anchors are found, Seurat computes how much each anchor should influence each query cell, based on distances between query cells. Those distances must be measured in a space that describes the ATAC data well. Using the CCA space would let reference geometry contaminate the weighting. The **Harmony-corrected embedding from Part 3** is the right choice — the batch-corrected representation that best captures the query’s internal structure.

> 

**Why `harmony` and not `lsi`.** Part 3 established that raw LSI still carries a substantial batch effect: the uncorrected mixing score was 0.162 against a ceiling of 0.657. Weighting by an uncorrected embedding would let sample identity influence which anchors a cell listens to, biasing labels toward whichever samples the anchors came from. Using `harmony` with `dims = harmony_dims`, all 29 components, preserves the batch correction paid for in Part 3.

### Finding Transfer Anchors

```r
#-----------------------------------------------
# STEP 9: Find anchors between the scRNA-seq reference and the ATAC query
#-----------------------------------------------

DefaultAssay(atac) <- "ACTIVITY"

# features: the reference's variable genes are the shared vocabulary. Restricting
#   to genes present in both objects avoids a silent mismatch.
# reduction = "cca": required for cross-modality transfer (see above).
shared_features <- intersect(VariableFeatures(pbmc_rna), rownames(atac[["ACTIVITY"]]))

transfer_anchors <- FindTransferAnchors(
  reference       = pbmc_rna,
  query           = atac,
  features        = shared_features,
  reference.assay = "RNA",
  query.assay     = "ACTIVITY",
  reduction       = "cca",
  dims            = 1:30
)

length(shared_features)
```

**Output:**

```
[1] 2458
```

> 

**Check that number before proceeding.** A few thousand shared genes means the two objects speak the same vocabulary. Seeing a few hundred would indicate different gene identifier systems — most often Ensembl IDs in one object and gene symbols in the other — and every anchor found from that point on would be meaningless. Seeing zero produces `No features to use in finding transfer anchors`, which is the same problem announcing itself more loudly.

### Transferring the Cell Type Labels

```r
#-----------------------------------------------
# STEP 10: Transfer cell type labels onto the ATAC cells
#-----------------------------------------------

# refdata: the reference metadata column to copy across.
# weight.reduction / dims: the Harmony embedding from Part 3, all 29 components.
predicted_labels <- TransferData(
  anchorset        = transfer_anchors,
  refdata          = pbmc_rna$celltype,
  weight.reduction = atac[["harmony"]],
  dims             = harmony_dims
)

# TransferData returns predicted.id, prediction.score.max, and one score column
# per reference cell type. AddMetaData attaches all of them at once.
atac <- AddMetaData(atac, metadata = predicted_labels)

sort(table(atac$predicted.id), decreasing = TRUE)
```

**Output:**

```
       CD14+ Monocytes             CD4 Memory              CD4 Naive
                  7500                   5485                   5469
          CD8 effector                 NK dim             pre-B cell
                  2966                   2406                   2131
...
                   pDC               Platelet
                    94                     29
```

*Truncated: 14 cell types.*

> 

**Reading this table:** the ranking is plausible for PBMCs — monocytes and CD4 T cells dominate, pDCs and platelets are vanishingly rare. Compare it against the reference composition from Step 5 rather than against intuition. The proportions are similar but not identical: `CD14+ Monocytes` fall from 32 percent of the reference to 25 percent of the query, while `CD8 effector` rises from 4 percent to 10 percent. Some divergence is expected and healthy. **A predicted composition that mirrored the reference exactly would be a warning sign**, since it can mean the transfer defaulted to reference proportions instead of reading the data.

### Reading the Prediction Scores: Which Calls Can You Trust?

`TransferData` returns a confidence score for every cell, and ignoring it is the most common mistake in reference-based annotation. `prediction.score.max` is the weighted fraction of anchor influence that voted for the winning label. A score of 0.95 means the anchors were nearly unanimous. A score of 0.35 means the cell sat between three labels and the winner won by a plurality.

```r
#-----------------------------------------------
# STEP 11: Examine the distribution of prediction confidence
#-----------------------------------------------

summary(atac$prediction.score.max)

# Overall proportion of cells above the conventional 0.5 threshold
mean(atac$prediction.score.max > 0.5)

p_score_hist <- ggplot(atac@meta.data, aes(x = prediction.score.max)) +
  geom_histogram(bins = 50, fill = "steelblue", colour = "white") +
  geom_vline(xintercept = 0.5, linetype = "dashed", colour = "firebrick") +
  labs(
    title    = "Confidence of transferred cell type labels",
    subtitle = "Dashed line marks the conventional 0.5 threshold",
    x        = "Maximum prediction score",
    y        = "Number of cells"
  ) +
  theme_bw(base_size = 12)

ggsave(file.path(plot_dir, "02_prediction_score_distribution.png"),
       plot = p_score_hist, width = 7, height = 5, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/02_prediction_score_distribution.png?resize=1024%2C731&ssl=1)

**Output:**

```
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.1627  0.4712  0.6642  0.6602  0.8629  1.0000

[1] 0.7139446
```

> 

**Reading these numbers.** About 71 percent of cells clear the conventional 0.5 threshold, with a median of 0.66. For a cross-modality transfer using a healthy reference against a half-diseased cohort, that is a reasonable result — but it also means **roughly 8,750 cells carry a label the method is not confident about**, and those cells are not distributed at random.

Now look at where the low-confidence cells are. Confidence scattered at random means noise. Confidence concentrated in particular clusters means those clusters contain something the reference does not describe.

```r
#-----------------------------------------------
# STEP 12: Map prediction confidence onto the UMAP and onto clusters
#-----------------------------------------------

p_score_umap <- FeaturePlot(
  object    = atac,
  features  = "prediction.score.max",
  reduction = "umap.harmony",
  pt.size   = 0.05
) +
  scale_colour_viridis_c(option = "magma") +
  ggtitle("Label transfer confidence") +
  theme(aspect.ratio = 1)

ggsave(file.path(plot_dir, "03_prediction_score_umap.png"),
       plot = p_score_umap, width = 6.5, height = 5.5, dpi = 300)

# The same information as numbers: median confidence per cluster, worst first.
cluster_conf <- aggregate(
  prediction.score.max ~ cluster_final,
  data = atac@meta.data,
  FUN  = median
)
cluster_conf[order(cluster_conf$prediction.score.max), ]
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/03_prediction_score_umap.png?resize=1024%2C866&ssl=1)

**Output:**

```
   cluster_final prediction.score.max
              15            0.3047375
               2            0.3736780
              14            0.4133244
               9            0.4534342
               0            0.4850606
               7            0.5782249
...
              13            0.9855295
              12            0.9898688
```

*Truncated: 17 rows, ascending by confidence.*

> 

**Reading this table — the most important diagnostic in this section.** The spread is enormous: cluster 12 sits at 0.99 while cluster 15 sits at 0.30. That range, not the overall average, is what tells you where the annotation is trustworthy.

**Two of the three QC-suspect clusters from Part 3 appear at the bottom.** Cluster 15 is worst (0.30) and cluster 9 is fourth-worst (0.45). Two independent lines of evidence — chromatin QC metrics computed in Part 3, and label transfer confidence computed here from entirely different data — have converged on the same clusters. **Cluster 7, the third QC-suspect cluster, sits mid-table at 0.58**, which is an early hint that it may be a legitimate population with unusual chromatin rather than a technical artifact.

**Cluster 2 is the surprise.** With 4,013 cells it is the third-largest cluster in the dataset, its Part 3 QC metrics were unremarkable, and yet its median confidence is 0.37 — second worst overall. Large, clean clusters that the reference cannot describe are exactly the cases worth investigating, and the reference-free analysis returns to this cluster repeatedly.

Clusters 12 and 13 — the near-pure `severe1` clusters — sit at the *top* of this ranking at 0.99. Their labels are highly confident, which argues against dismissing them as artifacts.

### Converting Per-Cell Predictions into Per-Cluster Labels

Individual scATAC-seq cells are too sparse to annotate reliably. Clusters are not. The standard approach is a **majority vote**: whatever label most cells in a cluster received becomes the cluster’s label. A bare majority vote hides how close the vote was, so compute the margin at the same time.

```r
#-----------------------------------------------
# STEP 13: Majority-vote label per cluster, with the margin of victory
#-----------------------------------------------

# Cross-tabulate clusters against predicted labels, then convert to row proportions.
vote_counts <- table(atac$cluster_final, atac$predicted.id)
vote_prop   <- prop.table(vote_counts, margin = 1)

# For each cluster: the winning label, its share, and the runner-up's share.
# A large gap between first and second means the cluster is homogeneous.
vote_summary <- data.frame(
  cluster      = rownames(vote_prop),
  n_cells      = as.integer(rowSums(vote_counts)),
  top_label    = colnames(vote_prop)[apply(vote_prop, 1, which.max)],
  top_share    = round(apply(vote_prop, 1, max), 3),
  second_share = round(apply(vote_prop, 1, function(x) sort(x, decreasing = TRUE)[2]), 3),
  row.names    = NULL
)
vote_summary$margin <- vote_summary$top_share - vote_summary$second_share

vote_summary[order(vote_summary$margin), ]
```

**Output:**

```
   cluster n_cells         top_label top_share second_share margin
         2    4013         CD4 Naive     0.282        0.240  0.042
        15     171               pDC     0.368        0.251  0.117
        14     210      CD8 effector     0.600        0.400  0.200
         0    5116      CD8 effector     0.523        0.203  0.320
         9     863         CD4 Naive     0.569        0.202  0.367
...
        10     768 B cell progenitor     0.956        0.023  0.933
         8    1217        pre-B cell     0.989        0.007  0.982
        11     731        pre-B cell     0.990        0.003  0.987
        13     378   CD14+ Monocytes     1.000        0.000  1.000
```

*Truncated: 17 rows, ascending by margin. Written to `label_transfer_cluster_votes.csv`.*

> 

**Reading this table:** the `margin` column does the real work. A cluster where 100 percent of cells voted one way (cluster 13) is unambiguous. A cluster where the winner took 28 percent and the runner-up took 24 percent (cluster 2) has not been annotated at all — it has been assigned the label that happened to edge out the others, and reporting that as a confident call would be misleading.

**Cluster 2 again, and more starkly.** Margin 0.042 across 4,013 cells. The transfer is effectively refusing to answer for the third-largest cluster in the dataset.

**The reference’s naming problem has now propagated exactly as predicted.** Clusters 8 and 11 are labelled `pre-B cell` with margins of 0.98 and 0.99, and cluster 10 is labelled `B cell progenitor` with a margin of 0.93. These are among the most confident calls in the entire table — and all three names are developmentally wrong for peripheral blood. **High confidence means the transfer is certain these cells match those reference cells. It says nothing about whether the reference named them correctly.** This distinction is the single most important thing to take from this section.

**Cluster 14 is a coin flip between two labels.** Its 210 cells split 60/40 between `CD8 effector` and one other label, which is a resolution problem rather than a quality problem.

Store the transferred labels for comparison against the reference-free results later:

```r
#-----------------------------------------------
# STEP 14: Attach the transferred cluster labels to the object
#-----------------------------------------------

# match() maps each cell's cluster onto the winning label for that cluster.
atac$label_transfer <- vote_summary$top_label[
  match(as.character(atac$cluster_final), vote_summary$cluster)
]

# Keep the margin as well, so low-confidence clusters stay visible downstream.
atac$label_transfer_margin <- vote_summary$margin[
  match(as.character(atac$cluster_final), vote_summary$cluster)
]

write.csv(vote_summary,
          file.path(table_dir, "label_transfer_cluster_votes.csv"),
          row.names = FALSE)
```

---

## 🗂️ When No scRNA-seq Reference Exists: Three Alternative Reference Types

Label transfer from annotated scRNA-seq presupposes that somebody has already published annotated single-cell transcriptomes of your tissue. For PBMCs that is a safe bet. For a non-model organism, an unusual tissue, or a developmental stage nobody has profiled, it is not.

Three other classes of reference can stand in. They are not interchangeable, and the choice depends on what prior information exists for the system.

### Option 1: Bulk RNA-seq and Microarray Profiles with SingleR

Sorted-population bulk profiles predate single-cell methods and cover a wider range of cell types than most single-cell atlases. `SingleR` correlates each query profile against every reference profile using the genes that best discriminate between reference types, then assigns the best match.

**Why it complements label transfer:** bulk references are independent of the single-cell world — different labs, different platforms, populations defined by surface-protein sorting rather than transcriptional clustering. Agreement between the two methods is meaningful because they share almost nothing except the query data.

**The critical implementation detail:** run SingleR **per cluster, not per cell**. Gene activity for a single scATAC-seq cell is far too sparse for a reliable correlation against a dense bulk profile. `SingleR` accepts a `clusters` argument that aggregates cells before correlating.

```r
#-----------------------------------------------
# STEP 15: Annotate clusters against bulk immune reference profiles
#-----------------------------------------------

# The Monaco reference contains bulk RNA-seq of 29 sorted immune populations
# from human PBMCs -- the closest bulk match to this tissue.
# NOTE: fetchReference() is available from celldex 1.20.0 onward. 
monaco_ref <- celldex::fetchReference("monaco_immune", "2024-02-26")

# The log-normalized gene activity matrix is the query.
activity_data <- LayerData(atac, assay = "ACTIVITY", layer = "data")

# clusters = : aggregate cells within each cluster before correlating.
# label.main gives broad populations; label.fine gives 29 narrower ones.
singler_main <- SingleR(
  test     = activity_data,
  ref      = monaco_ref,
  labels   = monaco_ref$label.main,
  clusters = atac$cluster_final
)

data.frame(
  cluster   = rownames(singler_main),
  singler   = singler_main$labels,
  delta     = round(singler_main$delta.next, 3),
  row.names = NULL
)
```

**Output:**

```
   cluster         singler delta
         0         T cells 0.006
         1       Monocytes 0.058
         2    CD4+ T cells 0.000
         3    CD4+ T cells 0.123
...
         7 Dendritic cells 0.058
         8         B cells 0.118
         9    CD4+ T cells 0.058
...
        16 Dendritic cells 0.231
```

*Truncated: 17 rows, one per cluster.*

> 

**Reading this table — and a caution that applies to the whole method.** `delta.next` is the gap between the best-scoring label and the runner-up, which is SingleR’s own confidence measure. **Every value here is small.** The largest is 0.231 and the median is around 0.07. Cluster 2 returns exactly 0.000, meaning the top two labels scored identically.

**SingleR on gene activity is a weak discriminator, and this run demonstrates it honestly.** The broad strokes are right — clusters 8, 10 and 11 are B cells, clusters 1, 12 and 13 are monocytes, cluster 5 is NK — and those calls agree with label transfer, which is genuinely useful corroboration. But the fine distinctions are not trustworthy: cluster 9 is called `CD4+ T cells` at delta 0.058, and cluster 15 is called `CD4+ T cells` at 0.072 despite label transfer calling it `pDC`.

**This is a limitation of the input, not of SingleR.** The method was designed for dense expression matrices. Gene activity scores are sparse, length-biased, and blind to distal regulation, so the correlations that SingleR computes are all compressed toward each other. Use it as a coarse lineage check that either corroborates or contradicts label transfer. Do not use it to adjudicate between related subtypes.

**One disagreement worth carrying forward.** Cluster 7 is `CD14+ Monocytes` by label transfer and `Dendritic cells` by SingleR. Both are myeloid, so the lineage is not in doubt, but the subtype is. The reference-free analysis resolves this.

**A note on which bulk reference to choose.** `celldex` ships several, and they are not equivalent:

`monaco_immune``dice``hpca``blueprint_encode`

| Reference | Populations | Best for | Watch out for |
| --- | --- | --- | --- |
|  | 29 sorted PBMC populations | Blood and immune tissue | Blood only; useless for solid tissue |
|  | 15 sorted immune populations | Immune, with strong T cell subsetting | Fewer myeloid categories |
| (Human Primary Cell Atlas) | 157 cell types, microarray | Solid tissue, broad surveys | Microarray-era; noisier, older annotations |
|  | 24 types, bulk RNA-seq | Immune plus stromal and endothelial | Coarser immune subtypes |

For PBMCs, `monaco_immune` is the right default. For tissue containing epithelium or stroma, `blueprint_encode` or `hpca` cover populations Monaco does not.

### Option 2: Public Epigenomic Maps

Every method above translates chromatin into gene space, and every translation discards the distal enhancers where cell identity is most sharply encoded. An epigenomic reference avoids the translation, comparing **peaks to peaks**: a cluster’s pseudobulk accessibility profile against bulk ATAC-seq from sorted populations. `scATAcat` formalizes this approach, and useful reference sets include Corces et al. (2016) bulk ATAC-seq of 13 sorted hematopoietic populations, the ENCODE cCRE catalogue, and Zhang et al. (2021) single-cell chromatin accessibility across 30 adult tissues.

**Two things make this harder than it sounds.** Much published bulk ATAC-seq is on hg19, and overlapping hg19 intervals with hg38 peaks produces near-zero overlap that reads exactly like a negative biological result — LiftOver first, and check how many intervals failed. And peak-set overlap needs a permuted background, because two sets of 50,000 intervals overlap substantially by chance; the enrichment ratio is the interpretable statistic, not the raw overlap fraction.

**Worth the effort when** your tissue has good bulk ATAC coverage but poor single-cell transcriptomic coverage, particularly if the cell types are defined by distal regulatory programs that gene activity flattens away.

### Option 3: Cross-Species Label Transfer

With no reference in your species but a good one in a related species, map genes to their one-to-one orthologs (via `biomaRt`), rename the reference’s features, and run label transfer as in Steps 9 and 10. Keep strict one-to-one orthologs only — a one-to-many mapping silently averages paralogs with different expression patterns.

**It works for deeply conserved lineages and fails below them.** The broad divisions of the immune and nervous systems are recognizable across mammals, and their specifying transcription factors are among the most conserved genes in the genome. Subtypes are another matter: human and mouse immune systems differ substantially in subset composition and in several marker genes. There is also a chromatin-specific caveat — regulatory elements evolve far faster than coding sequence, so even an unambiguous ortholog may sit in a non-conserved enhancer landscape. Use it for broad lineage, then switch to reference-free methods.

### Which External Reference Should You Reach For?

**Annotated scRNA-seq****Bulk RNA-seq / microarray****Public epigenomic maps****Cross-species**

| Reference type | Use when | Strength | Weakness |
| --- | --- | --- | --- |
|  | A single-cell atlas of the tissue exists | Finest granularity; best-supported tooling | Inherits reference labeling quirks; disease states often missing |
|  | Sorted populations exist but no single-cell atlas | Clean, deep profiles; broader cell type coverage | Coarse labels; weak discrimination on gene activity input |
|  | Bulk ATAC or DHS data exists for the tissue | Compares like with like; retains distal signal | Build mismatches; needs a background model; patchy coverage |
|  | Nothing exists in your species | Better than nothing for broad lineages | Unreliable below lineage level; enhancers poorly conserved |

> 

**Combine them.** Nothing prevents running label transfer, SingleR against a bulk reference, and a peak-level comparison on the same dataset. Clusters where all three agree are settled. Clusters where they disagree are where the biology is.

---

## 🔍 Reference-Free Annotation: Letting the Chromatin Speak for Itself

Everything so far has borrowed an answer from somewhere else. Now the data is asked directly.

Reference-free annotation matters even when references exist, for a reason Steps 13 and 15 made concrete: **the reference gave confident names to the B cell clusters that are developmentally wrong, and gave no usable answer at all for cluster 2.** Only the chromatin itself can settle either question.

> 

**What “reference-free” does and does not mean.** It means free of an *annotated dataset* — no other experiment’s cell labels are copied onto your cells. It does **not** mean free of prior biological knowledge. All three methods below depend on knowing in advance that `MS4A1` marks B cells, that `PAX5` specifies the B lineage, and that `CD300LB` is myeloid. **Nothing here discovers a cell type from scratch.** The prior simply lives in the literature rather than in a reference object, which is what makes it portable across datasets, species and assays.

The three methods differ in *where* the prior enters, and that difference is what makes them worth running together:

**1. Marker gene activity****2. Cluster marker peaks****3. Motif enrichment**

| Method | Discovery step | Where prior knowledge enters |
| --- | --- | --- |
|  | None — you supply the gene list | At the start: the panels decide what can be found |
|  | Unbiased: every peak is tested | At the end: you must recognize the gene names that come back |
|  | Unbiased: all 633 motifs are tested | At the end: you must know which factors specify which lineage |

**Method 1 can only find what you thought to look for.** Methods 2 and 3 can surface something you did not anticipate, but only if you recognize it when it appears. **The practical consequence:** in a well-characterized tissue like PBMCs, all three work. In a non-model organism, or for a genuinely novel population, the prior does not exist and none of them will name it — the honest outcome there is a described cluster with no label, which is exactly what happens to cluster 2 later in this tutorial.

### Comparing the Reference-Free Strategies

The literature offers more options than a beginner needs, and several are not yet worth the setup cost. Here is the landscape.

**Marker gene activity scoring****Use first.****Cluster marker peaks + nearest gene****Use second.****TF motif enrichment (`FindMotifs`)****Use third, as orthogonal confirmation.****Pseudobulk vs bulk ATAC prototypes**`scATAcat`**Deep learning on peak features**

| Strategy | How it works | Verdict for this tutorial |
| --- | --- | --- |
|  | Score each cluster for accessibility at curated marker gene sets | Interpretable, no new dependency, reuses the gene activity assay already built |
|  | One-versus-rest test on the peak assay, then label peaks by nearest gene | Finds distinctive regions without a prior marker list |
|  | Test cluster marker peaks for over-represented TF binding motifs against a matched background | Reads sequence only, so distal peaks count fully and gene annotation cannot mislead it |
|  | Correlate cluster pseudobulk against sorted bulk ATAC () | Promising and peak-native, but a Python package and a language switch mid-pipeline |
|  | Neural networks trained on peak or sequence input | Active research, not yet a beginner default; several still require a reference despite the framing |

All three recommended methods ship with Signac and need nothing beyond what Step 1 already loaded.

**On the ordering.** A natural instinct is to start with motif analysis to establish broad lineages, then refine with gene activity. That is the right order when the genome’s gene models are unreliable, since motifs depend only on DNA sequence and survive bad annotation.

For this dataset motifs come last, for three reasons:

1. **Gene activity is already computed.** It cost 30 to 90 minutes in Step 6. Reusing it costs nothing and answers the lineage question directly.

2. **Motif families are degenerate.** Transcription factors within a family bind nearly identical sequences. The bZIP factors — FOS, JUN, ATF, BATF — are close to indistinguishable by motif, as are the PAX, CEBP, ETS and T-box families. Step 22 shows this happening in practice, and it is the single biggest trap in interpreting motif results.

3. **There is a hard dependency.** `FindMotifs()` tests a *set of peaks*, so it needs the marker peaks from Method 2 as its input. That fixes the order regardless of preference.

So: **marker gene activity first for lineage, cluster marker peaks second to find what the panels missed, motif enrichment third as sequence-based confirmation.** They run as Steps 16 through 24 below.

### Method 1: Marker Gene Activity Scores for Broad Lineages

Rather than eyeballing one gene at a time, score each cluster against a **panel** of markers per lineage. Panels are dramatically more robust than single genes in sparse data — if `CD3E` is uninformative in chromatin, as Part 3 showed, a panel that also includes `CD3D`, `IL7R` and `LCK` still works.

**The panels below are the prior, and they set the ceiling on this method.** They come from the standard PBMC immunology literature, not from anything computed in this dataset, and a cluster whose identity is not represented among them cannot be found by this step — which is why a `Granulocyte` panel is included even though PBMC preparation should exclude granulocytes. **Choosing panels is the analytical decision here; running `AddModuleScore` is bookkeeping.** For a different tissue, replace the whole list.

```r
#-----------------------------------------------
# STEP 16: Define marker panels for the major PBMC lineages
#-----------------------------------------------

DefaultAssay(atac) <- "ACTIVITY"

# Panels of 4-6 canonical markers per lineage. Genes were chosen for chromatin
# specificity rather than expression magnitude -- a gene that is highly expressed
# but whose locus is permissively open everywhere is useless here.
marker_panels <- list(
  T_cell      = c("CD3D", "CD3E", "CD3G", "LCK", "IL7R", "THEMIS"),
  CD4_T       = c("CD4", "CD40LG", "MAL", "TRAT1"),
  CD8_T       = c("CD8A", "CD8B", "GZMK", "LINC02446"),
  NK_cell     = c("NCAM1", "KLRD1", "NKG7", "GNLY", "KLRF1", "PRF1"),
  B_cell      = c("MS4A1", "CD79A", "CD79B", "BANK1", "PAX5", "EBF1"),
  Monocyte    = c("LYZ", "CD14", "VCAN", "S100A8", "CSF1R", "FCN1"),
  Mono_CD16   = c("FCGR3A", "MS4A7", "CDKN1C", "LST1"),
  Dendritic   = c("FLT3", "CLEC9A", "CD1C", "ZBTB46"),
  pDC         = c("IRF8", "LILRA4", "TCF4", "CLEC4C"),
  Granulocyte = c("FCGR3B", "CSF3R", "ELANE", "MPO", "CEACAM8"),
  Megakaryo   = c("PF4", "PPBP", "ITGA2B", "GP9"),
  Progenitor  = c("CD34", "KIT", "SPINK2", "PROM1")
)

# Keep only genes actually present in the gene activity matrix -- a missing gene
# would silently distort the panel average.
marker_panels <- lapply(marker_panels, intersect, rownames(atac[["ACTIVITY"]]))

sapply(marker_panels, length)
```

**Output:**

```
     T_cell       CD4_T       CD8_T     NK_cell      B_cell    Monocyte
          6           4           3           6           5           6
  Mono_CD16   Dendritic         pDC Granulocyte   Megakaryo  Progenitor
          4           4           4           5           4           4
```

> 

**Two panels lost a member:** `CD8_T` dropped from 4 to 3 and `B_cell` from 6 to 5, because `GeneActivity()` quantifies protein-coding gene bodies and does not cover every annotated feature. Always check this output — a panel silently reduced to one or two genes loses the robustness that motivated using panels at all. Three genes is workable; one is not.

Now score every cell against every panel. `AddModuleScore` compares each panel’s average activity against the average of a randomly chosen control set matched for overall accessibility, which controls for some genes being more accessible than others regardless of cell type.

```r
#-----------------------------------------------
# STEP 17: Score every cell against every marker panel
#-----------------------------------------------

# name = "panel": AddModuleScore writes columns panel1, panel2, ... in the same
#   order as the list, renamed below. Using a single prefix rather than a vector
#   of names keeps the column mapping unambiguous.
# ctrl = 50: size of the accessibility-matched background set for each panel.
atac <- AddModuleScore(
  object   = atac,
  features = marker_panels,
  assay    = "ACTIVITY",
  name     = "panel",
  ctrl     = 50,
  seed     = 1234
)

# Rename panel1..panel12 to readable names, matching by position in the list.
score_cols_raw <- paste0("panel", seq_along(marker_panels))
score_cols     <- paste0("score_", names(marker_panels))
colnames(atac@meta.data)[match(score_cols_raw, colnames(atac@meta.data))] <- score_cols

# Mean score per cluster: this matrix is the reference-free annotation evidence.
score_matrix <- sapply(score_cols, function(col) {
  tapply(atac@meta.data[[col]], atac$cluster_final, mean)
})
colnames(score_matrix) <- names(marker_panels)

round(score_matrix, 3)
```

**Output:**

```
   T_cell  CD4_T  CD8_T NK_cell B_cell Monocyte Mono_CD16 Dendritic    pDC Granulocyte Megakaryo Progenitor
0   0.054 -0.028  0.143   0.109 -0.060   -0.038    -0.002    -0.026 -0.074       0.002    -0.002      0.020
1  -0.120 -0.020 -0.075  -0.051 -0.036    0.162     0.048     0.041  0.054       0.018    -0.002     -0.022
2  -0.025 -0.019 -0.011   0.048 -0.021   -0.033    -0.010    -0.015 -0.017      -0.006     0.001     -0.006
5  -0.073 -0.052 -0.009   0.184 -0.068   -0.036    -0.009    -0.041  0.009      -0.003     0.007      0.054
6   0.133  0.025  0.225  -0.050 -0.044   -0.030    -0.027    -0.014 -0.054      -0.009     0.001      0.029
...
8  -0.086 -0.055 -0.071  -0.076  0.345   -0.034    -0.016     0.015  0.164      -0.011    -0.005     -0.007
10 -0.105 -0.047 -0.075  -0.090  0.364   -0.036    -0.003     0.022  0.158      -0.010    -0.003     -0.025
11 -0.096 -0.049 -0.077  -0.099  0.369   -0.044     0.029     0.011  0.213      -0.009    -0.000     -0.033
...
16 -0.100 -0.026 -0.074  -0.026  0.013    0.063    -0.017     0.120  0.115      -0.011     0.006      0.050
```

*Truncated: 17 clusters. Written to `marker_panel_scores.csv`.*

A heatmap makes the block structure obvious in a way a table does not.

```r
#-----------------------------------------------
# STEP 18: Heatmap of marker panel scores across clusters
#-----------------------------------------------

# Reshape the wide matrix into long format for ggplot.
score_long <- data.frame(
  cluster = rep(rownames(score_matrix), times = ncol(score_matrix)),
  panel   = rep(colnames(score_matrix), each  = nrow(score_matrix)),
  score   = as.vector(score_matrix)
)

# Preserve numeric cluster ordering rather than lexicographic (0, 1, 10, 11...)
score_long$cluster <- factor(score_long$cluster,
                             levels = as.character(sort(as.numeric(rownames(score_matrix)))))
score_long$panel   <- factor(score_long$panel, levels = colnames(score_matrix))

p_score_heat <- ggplot(score_long, aes(x = panel, y = cluster, fill = score)) +
  geom_tile(colour = "white", linewidth = 0.3) +
  scale_fill_gradient2(low = "#2166AC", mid = "white", high = "#B2182B", midpoint = 0) +
  labs(
    title    = "Marker panel accessibility scores by cluster",
    subtitle = "Red indicates accessibility above the matched background",
    x        = "Marker panel",
    y        = "Cluster",
    fill     = "Module\nscore"
  ) +
  theme_bw(base_size = 12) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))

ggsave(file.path(plot_dir, "04_marker_panel_heatmap.png"),
       plot = p_score_heat, width = 9, height = 7, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/04_marker_panel_heatmap.png?resize=1024%2C796&ssl=1)

> 

**Reading this figure.** Look for **one red block per row**. The colour scale tops out near 0.35, far below what scRNA-seq module scores reach, so judge each row against the rest of that row rather than against an absolute threshold.

**Four blocks stand out immediately.** A deep red column in `B_cell` for clusters 8, 10 and 11 — by far the strongest signal in the plot. A `Monocyte` block in clusters 1, 12 and 13, which also spreads faintly rightward across `Mono_CD16` and `Dendritic`. A `T_cell`/`CD4_T` pair in clusters 3 and 4. And a cytotoxic block where `CD8_T` and `NK_cell` light up together in clusters 0, 6, 5 and 14.

**Where two panels light up in the same row, ask whether they share genes.** The three B cell rows are also red under `pDC`, which does not mean pDCs are hiding there — `IRF8` and `TCF4` are two of that panel’s four genes and both are active in B cells. Step 28’s dot plot shows the `IRF8` signal directly. **A panel is only as specific as its least specific member**, and with four-gene panels one shared gene is enough to produce a false block.

**The `CD8_T` and `NK_cell` columns overlapping in clusters 0 and 14 is real biology**, not a panel artifact — effector CD8 T cells and NK cells run the same cytotoxic program. It is the reason those clusters need a judgment call later.

**Three columns are white top to bottom:** `Granulocyte`, `Megakaryo` and `Progenitor`. The granulocyte panel was included specifically to test for the low-density granulocytes reported in severe COVID-19, which can survive Ficoll separation. **It came back empty, and that is a real result:** the PBMC preparation worked as intended.

**Four rows are white across every column** — clusters 2, 7, 9 and 15. No panel rises above background anywhere in those rows, while every confidently annotated cluster shows at least one clear block. **A row with no block has not been annotated by this method**, and cluster 2 is the third-largest cluster in the dataset.

### Method 2: Cluster Marker Peaks and Their Nearest Genes

Marker panels can only report on the genes you thought to include. This method asks the data which regions distinguish each cluster, without deciding in advance what to look for.

Before testing, restrict the peak set to the primary assembly. Scaffolds and alternate contigs carry few reads, contribute nothing interpretable, and have no matching genome sequence, which would abort the motif scan in Step 21. Doing it now keeps the peak set identical between this test and the motif enrichment that consumes its output.

```r
#-----------------------------------------------
# STEP 19: Restrict to primary assembly, then find cluster marker peaks
#-----------------------------------------------

DefaultAssay(atac) <- "peaks"
Idents(atac) <- "cluster_final"

# Subset the peaks assay -- not the whole object -- so the ACTIVITY assay is untouched.
main_chroms <- standardChromosomes(BSgenome.Hsapiens.UCSC.hg38)
keep_peaks  <- rownames(atac[["peaks"]])[
  as.character(seqnames(granges(atac))) %in% main_chroms
]
atac[["peaks"]] <- subset(atac[["peaks"]], features = keep_peaks)

# ident.1 = cl with no ident.2: compare this cluster against all other cells
#   pooled, so each pass returns one cluster's marker peaks.
# test.use = "LR" + latent.vars: logistic regression with sequencing depth as a
#   covariate, so depth cannot masquerade as accessibility (see above).
# only.pos = TRUE: keep peaks MORE open in this cluster, which is what a marker is.
# max.cells.per.ident = 500: caps each regression at 500 cells per group, which
#   is what makes this tractable.
# try(): one failing cluster cannot destroy the results for the other sixteen.
da_list <- lapply(levels(atac$cluster_final), function(cl) {
  res <- try(FindMarkers(
    object   = atac, ident.1 = cl,
    test.use = "LR", latent.vars = "nCount_peaks",
    min.pct  = 0.1, only.pos = TRUE,
    max.cells.per.ident = 500
  ), silent = TRUE)
  if (inherits(res, "try-error")) return(NULL)
  res$cluster <- cl
  res$gene    <- rownames(res)
  res
})
names(da_list) <- levels(atac$cluster_final)
sapply(da_list, function(x) if (is.null(x)) NA else nrow(x))
```

**Output:**

```
    0     1     2     3     4     5     6     7     8     9    10    11    12
11116  4835  4305  5278  8542  6638  6298  2512  4637   757  7466 13782  9843
   13    14    15    16
 4999  9180  2834  7588
```

> 

**Why the loop rather than `FindAllMarkers()`.** Three practical reasons, all learned the hard way.

**Runtime.** `FindAllMarkers()` on 30,571 cells across 185,581 peaks with the LR test runs for many hours. `max.cells.per.ident = 500` caps each regression at 500 cells per group, which brings the whole loop down to minutes. The largest cluster has 5,116 cells and the smallest 87, so 500 per group is ample for detecting peaks with large accessibility differences — which is all annotation requires.

**Failure isolation.** `FindAllMarkers()` returns whatever survives if a worker dies, producing a ragged result that looks like a finding. Wrapping each cluster in `try()` means a failure shows up as an explicit `NA` rather than as a cluster with suspiciously few peaks.

**Parallelism is not the answer here.** `plan("multicore")` inside RStudio is unsupported and forked workers get terminated, producing `FutureInterruptError` messages that look like memory problems but are not. If you want parallelism, run it as a batch job through `Rscript`, not in an interactive session. Downsampling gets the speed without the risk.

**Reading the counts:** every cluster returned, and no `NA` appears. Cluster 9 is the lowest at 757 peaks, roughly a fifth of the next-lowest. Cluster 11 is highest at 13,782.

Now filter to the strongest, most significant peaks per cluster and attach nearest genes.

**A peak like `chr2-85694612-85695431` is unreadable on its own**, so `ClosestFeature()` attaches whichever annotated feature lies nearest and reports the distance in base pairs.

**Does this assume the peak regulates that gene? A weak version of it, yes — and the assumption is worth stating out loud.** `ClosestFeature()` itself asserts nothing; it performs an interval lookup. But the moment you read `TCL1A` next to a cluster 8 peak and conclude “B cell”, you have assumed that a cluster-specific peak near a lineage gene reflects that lineage’s regulatory activity at that locus. That assumption is reasonable for peaks inside a gene body or promoter. It is unreliable for distal peaks, where enhancers routinely skip over intervening genes to reach a promoter hundreds of kilobases away, and Step 23 will show that more than a quarter of these marker peaks sit over 10 kb from any annotated feature.

**Treat the gene column as a hypothesis generator, not as evidence of regulation.** It tells you which recognizable genes turn up near a cluster’s most distinctive regions, which is enough to suggest a lineage and to corroborate the other methods. It does not establish that the peak controls the gene. Establishing that needs `LinkPeaks()`, which correlates peak accessibility against measured expression across cells and therefore requires paired RNA from the same nuclei — unavailable in an ATAC-only dataset like this one.

```r
#-----------------------------------------------
# STEP 20: Filter to top peaks, then annotate with their nearest gene
#-----------------------------------------------

# Stack the per-cluster results into one data frame.
da_peaks <- do.call(rbind, da_list[!sapply(da_list, is.null)])

top_markers <- da_peaks %>%
  filter(p_val_adj < 0.01) %>%
  group_by(cluster) %>%
  slice_max(order_by = avg_log2FC, n = 200) %>%
  ungroup() %>%
  as.data.frame()

table(top_markers$cluster)

# ClosestFeature returns the nearest annotated feature and its distance in bp.
closest_genes <- ClosestFeature(atac, regions = top_markers$gene)

marker_peak_anno <- data.frame(
  cluster  = top_markers$cluster,
  peak     = top_markers$gene,
  log2FC   = round(top_markers$avg_log2FC, 3),
  p_adj    = top_markers$p_val_adj,
  gene     = closest_genes$gene_name,
  distance = closest_genes$distance
)

write.csv(marker_peak_anno,
          file.path(table_dir, "cluster_marker_peaks_annotated.csv"),
          row.names = FALSE)

# The top five genes per cluster, by peak fold change.
marker_peak_anno %>%
  group_by(cluster) %>%
  slice_max(order_by = log2FC, n = 5) %>%
  summarise(top_genes = paste(unique(gene), collapse = ", ")) %>%
  as.data.frame()
```

**Output:**

```
  0   1  10  11  12  13  14  15  16   2   3   4   5   6   7   8   9
200 200 200 200 200 200 186 151 200 111 200 200 200 200 200 200   4

   cluster                                top_genes
         0           NFIB, VRK1, RCAN2, SGCD, LTBP1
         1     COL23A1, TMEM138, MMP9, RNASE3, MRAS
        10 CBLN2, KLHL1, ZBTB20, TRAM2, CORO7-PAM16
...
         2      SPON2, MORN3, PTGDS, TAF4, C17orf97
         3              PROX1, TM9SF2, MYT1L, KRT73
...
         7   TNFAIP2, CD300LB, CCRL2, GPIHBP1, KLF4
         8     PLD5, PCDH9, DPF3, ST6GALNAC3, TCL1A
         9                  GOLGA6L6, TUBA3C, FRG2C
```

*Gene table truncated: 17 rows, in lexicographic cluster order. Written to `cluster_marker_peaks_annotated.csv`.*

> 

**Start with the peak counts, because one of them is a result in its own right.** Every cluster hits the 200 cap except three: cluster 15 retained 151, cluster 2 retained 111, and **cluster 9 retained just 4** — out of the 757 peaks that had passed the test before multiple-testing correction.

**A cluster that cannot produce significant marker peaks has no chromatin program setting it apart.** Cluster 9 has 863 cells, poor QC metrics from Part 3, low transfer confidence, no marker panel signal, and now four significant peaks. Four independent observations point the same way.

**Now the gene table, which will surprise anyone coming from scRNA-seq.** Look at the top genes: `NFIB`, `VRK1`, `RCAN2` for cluster 0. `CBLN2`, `KLHL1`, `ZBTB20` for cluster 10. `MYT1L`, `KRT73` for cluster 3. **These are not immune marker genes.** `MYT1L` is a neuronal transcription factor. `KRT73` is a hair keratin. `CBLN2` is a cerebellin.

**This is not a bug, and the peaks are real.** Ranking by `avg_log2FC` in near-binary data preferentially surfaces peaks that are accessible in a small fraction of one cluster and essentially zero elsewhere — large fold change, low absolute signal. Those peaks are genuinely cluster-specific, but they are rarely the canonical marker loci, and the nearest gene to a distal element frequently has nothing to do with its target. **Do not annotate from this table alone.** It is a source of hypotheses and a check on the other methods, not a substitute for them.

**Some clusters do yield interpretable genes.** Cluster 1 shows `MMP9` and `RNASE3`, both myeloid granule genes, supporting the monocyte call. Cluster 7 shows `TNFAIP2`, `CD300LB`, `CCRL2` and `KLF4` — a coherent myeloid activation set, with `CD300LB` and `KLF4` pointing to the monocyte lineage rather than to dendritic cells, which begins to settle the label-transfer-versus-SingleR disagreement. Cluster 8 shows `TCL1A`, a naive B cell marker, consistent with the B cell panel score. Cluster 2 shows `SPON2` and `PTGDS`, both associated with NK cells.

**Cluster 9’s four genes are diagnostic of a different problem.** `GOLGA6L6`, `TUBA3C` and `FRG2C` all sit in segmental duplications and low-complexity regions where short-read alignment is unreliable. Marker peaks drawn exclusively from such regions indicate mapping artifact rather than biology.

### Method 3: Transcription Factor Motif Enrichment in Cluster Marker Peaks

The two methods so far both reason through gene names — gene activity directly, nearest-gene assignment indirectly. Both therefore lose the distal elements where cell identity is most sharply encoded: gene activity never counts them, and nearest-gene assignment mislabels them.

Motif analysis has no such blind spot. It asks whether the peaks distinguishing a cluster are enriched for a particular transcription factor’s binding sequence, reading only the DNA underneath them. **A peak 200 kb from the nearest gene contributes exactly as much as a promoter peak**, because no gene assignment is involved at any stage. That also makes its errors independent of everything above — it cannot be misled by a wrong nearest gene or a missing gene model.

**First, scan every peak for motif matches.**

```r
#-----------------------------------------------
# STEP 21: Add motif information to the peaks assay
#-----------------------------------------------

DefaultAssay(atac) <- "peaks"

# Retrieve vertebrate core motifs from JASPAR 2020. collection = "CORE" gives
# experimentally validated matrices; all_versions = FALSE keeps only the latest.
pfm <- getMatrixSet(
  x    = JASPAR2020,
  opts = list(species = 9606, collection = "CORE", all_versions = FALSE)
)

# AddMotifs scans the genome sequence under every peak for every motif, stores
# the sparse peak-by-motif match matrix, and computes the per-peak GC content
# and sequence length that FindMotifs needs to build a matched background.
atac <- AddMotifs(
  object = atac,
  genome = BSgenome.Hsapiens.UCSC.hg38,
  pfm    = pfm
)

length(pfm)
dim(GetMotifData(atac, assay = "peaks"))
```

**Output:**

```
[1] 633

[1] 185581    633
```

> 

**All 185,581 peaks survived the primary-assembly filter in Step 19**, confirming that Part 3’s consensus peak set was already restricted to standard chromosomes. If this number were dramatically smaller, the likely cause would be peaks using Ensembl-style chromosome names (`1`) rather than UCSC-style (`chr1`).

**Then test each cluster’s marker peaks for motif enrichment.**

```r
#-----------------------------------------------
# STEP 22: Test each cluster's marker peaks for enriched TF motifs
#-----------------------------------------------

cluster_levels <- levels(atac$cluster_final)

# FindMotifs compares the motif content of the supplied peaks against a
# background of peaks matched for GC content, length and overall accessibility,
# so enrichment is not simply a restatement of sequence composition.
motif_enrichment <- lapply(cluster_levels, function(cl) {
  cl_peaks    <- top_markers$gene[top_markers$cluster == cl]
  res         <- FindMotifs(object = atac, features = cl_peaks)
  res$cluster <- cl
  res
})
motif_enrichment <- do.call(rbind, motif_enrichment)

write.csv(motif_enrichment,
          file.path(table_dir, "cluster_motif_enrichment.csv"),
          row.names = FALSE)

# The three most enriched motifs per cluster, by fold enrichment among
# significant hits.
motif_enrichment %>%
  filter(p.adjust < 0.05) %>%
  group_by(cluster) %>%
  slice_max(order_by = fold.enrichment, n = 3) %>%
  summarise(top_motifs = paste(motif.name, collapse = ", ")) %>%
  as.data.frame()
```

**Output:**

```
   cluster                       top_motifs
1        0               EOMES, TBR1, TBX20
2        1           FOSL2, BACH2, FOS::JUN
3       10              PAX9, PAX1, POU5F1B
4       11                 PAX1, PAX9, PAX5
5       12              CEBPE, CEBPB, CEBPG
6       13              CEBPG, CEBPB, CEBPE
...
11       3              TCF7L2, TCF7, RUNX2
...
15       7                 GMEB2, TEF, CREM
16       8       CREB3L4(var.2), PAX9, PAX1
17       9                            MEIS3
```

*Truncated: 17 rows. Written to `cluster_motif_enrichment.csv`.*

> 

**This table is the clearest lineage evidence in the tutorial, and also the clearest demonstration of motif family degeneracy.**

**Read the families, not the individual factors.** Clusters 10, 11 and 8 return `PAX9`, `PAX1` and `PAX5`. `PAX5` is the B cell master regulator; `PAX1` and `PAX9` are developmental factors with no role in lymphocytes whatsoever. **They appear because all three bind nearly identical sequences.** The correct reading is “the PAX family motif is enriched”, and in the context of B cell panel scores of 0.345 to 0.369, the responsible member is unambiguously `PAX5`. Anyone reporting `PAX1` activity in peripheral blood B cells has misread this output.

**The same trap in a different family.** Clusters 12 and 13 return `CEBPE`, `CEBPB` and `CEBPG`. `CEBPE` is granulocyte-restricted and would, taken literally, suggest neutrophils. But the granulocyte marker panel was flat everywhere in Step 17, the monocyte panel put these clusters at 0.105 and 0.099, and label transfer called them `CD14+ Monocytes` with margins of 0.996 and 1.000. **The CEBP family motif is enriched; the responsible member is `CEBPB`, the monocyte factor, not `CEBPE`.** This is exactly why motif results must be read alongside marker evidence rather than on their own.

**And a third.** Clusters 0, 14 and 5 return `EOMES`, `TBR1`, `TBX20`, `TBX2` and `TBX21` — the T-box family. `EOMES` and `TBX21` are the NK and effector-CD8 factors; `TBR1` and `TBX20` are neuronal and cardiac. Same principle.

**With that caveat applied, the lineage blocks are clean.** T-box in clusters 0, 5 and 14 marks the cytotoxic compartment. `TCF7`/`TCF7L2` in clusters 3 and 6 marks conventional T cells. PAX family in clusters 8, 10 and 11 marks B cells. CEBP family in clusters 12 and 13, and AP-1 factors in cluster 1, mark myeloid. NF-kB (`REL`, `RELA`) in cluster 16 marks activated myeloid or dendritic cells.

**Cluster 9 returned exactly one significant motif, and R warned about it.** `Testing motif enrichment using a small number of regions is not recommended` is Signac telling you that four peaks cannot support an enrichment test. The single `MEIS3` hit should be disregarded entirely. **A warning of this kind is a result: it says the cluster had nothing to test.**

**Clusters 2, 7 and 15 return no lineage information.** `GMEB2`, `ATF7`, `JUNB` for cluster 2; `GMEB2`, `TEF`, `CREM` for cluster 7; `NFYB`, `TCFL5`, `HES1` for cluster 15. These are ubiquitous stress-response and housekeeping factors present in every cell type. Their appearance at the top means no lineage-specific program dominated those peak sets.

Step 22 asked an open question — *which* motifs are enriched, out of all 633. Step 23 asks a closed one: how do a fixed set of lineage-defining factors score in every cluster? The two are complementary, and it matters that the second one is hypothesis-driven.

**Where the twelve factors come from.** They are not selected from the Step 22 output. They are a curated list from the hematopoiesis literature, chosen the same way the marker panels in Step 17 were chosen, on one criterion: **a factor qualifies if removing it abolishes the lineage, not merely if it is active there.** `PAX5` and `EBF1` are on the list because knocking either out blocks B cell commitment. `SPI1` (PU.1) is required for myeloid and B lymphoid development, `GATA1` for erythroid and megakaryocytic, `TBX21` and `EOMES` for NK and effector CD8. Factors that are merely correlated with a lineage — most AP-1 members, most stress-response factors — are excluded, because they turn up in whichever cluster happens to be most activated and carry no identity information.

**Why query a fixed list at all, when Step 22 already gave an unbiased answer?** Because a top-3 list only shows what won. It cannot tell you that `PAX5` is *absent* from the T cell clusters, or that `SPI1` is flat everywhere. A fixed panel scored across all clusters shows presence and absence on the same scale, which is what the consensus table in Step 26 needs.

```r
#-----------------------------------------------
# STEP 23: Extract enrichment for lineage-defining transcription factors
#-----------------------------------------------

# Curated master regulators, from the hematopoiesis literature rather than from
# this dataset. Criterion: loss of the factor abolishes the lineage.
lineage_tfs <- c(
  "SPI1",   # PU.1 -- myeloid and B lineage specification
  "CEBPA",  # myeloid, strongly monocyte and granulocyte
  "CEBPE",  # granulocyte-restricted
  "PAX5",   # B cell commitment
  "EBF1",   # B cell commitment
  "TCF7",   # T cell development
  "GATA3",  # T cell, strongly CD4
  "RUNX3",  # CD8 T and NK
  "EOMES",  # NK and effector CD8
  "TBX21",  # T-bet -- NK and Th1
  "IRF8",   # dendritic cell and pDC
  "GATA1"   # erythroid and megakaryocyte
)

# Fold enrichment per cluster for each factor. NA means the motif was not
# returned for that cluster, which happens when too few marker peaks were tested.
tf_matrix <- sapply(cluster_levels, function(cl) {
  sub <- motif_enrichment[motif_enrichment$cluster == cl, ]
  sub$fold.enrichment[match(lineage_tfs, sub$motif.name)]
})
rownames(tf_matrix) <- lineage_tfs

round(t(tf_matrix), 2)
```

**Output:**

```
   SPI1 CEBPA CEBPE PAX5 EBF1 TCF7 GATA3 RUNX3 EOMES TBX21 IRF8 GATA1
0  0.82  1.16  1.07 1.02 0.73 0.81  1.15  1.32  3.27  2.92 0.91  1.04
1  1.62  1.79  2.50 0.88 1.06 0.91  0.84  1.11  0.86  0.84 0.97  0.96
2  0.99  0.19  1.75 1.46 0.61 0.87  0.75  0.78  0.56  0.76 0.31  0.60
3  1.06  0.74  0.92 0.83 0.81 2.26  0.86  1.83  0.84  1.02 0.94  0.85
...
9  0.00  0.00  0.00 5.61 0.00 1.89  0.00  1.68  2.41  2.49 4.16  0.00
...
11 1.53  0.70  0.43 2.75 1.25 1.07  1.04  1.05  1.37  1.19 1.30  1.36
12 0.98  3.20  4.27 1.57 0.82 0.73  0.74  0.84  1.05  1.02 0.94  0.73
...
16 1.32  0.88  0.81 0.83 1.38 1.11  0.80  1.02  0.68  0.72 1.30  1.01
```

*Truncated: 17 clusters. Written to `lineage_tf_motif_enrichment.csv`.*

> 

**Reading this table.** Values near 1.0 mean “as common as in the matched background”. Above roughly 2 is a strong signal.

**Four coherent blocks, and now with the paired factors visible.** `EOMES` and `TBX21` move together in clusters 0 (3.27 / 2.92), 14 (2.84 / 2.77) and 5 (2.38 / 2.40) — the cytotoxic compartment, marked by both T-box factors rather than one. `TCF7` is high in clusters 3 (2.26) and 6 (2.33), with `RUNX3` also elevated (1.83, 1.63), marking conventional T cells. `PAX5` and `EBF1`, the two B cell commitment factors, mark clusters 11 (2.75 / 1.25), 8 (1.89 / 1.81) and 10 (1.52 / 1.94). `CEBPA` and `CEBPE` together mark clusters 12 (3.20 / 4.27), 13 (2.52 / 3.19), 7 (1.45 / 2.74) and 1 (1.79 / 2.50).

**Reading the CEBP pair rather than `CEBPE` alone resolves the granulocyte scare.** Taken by itself, `CEBPE` at 4.27 in cluster 12 would suggest neutrophils. But `CEBPA`, the monocyte and general myeloid factor, is elevated in exactly the same clusters, and the granulocyte marker panel was flat everywhere. The family is enriched; `CEBPA` is the plausible member.

**`SPI1` is uninformative throughout**, ranging only from 0.82 to 1.62. PU.1 is required for both myeloid and B lymphoid development, so its motif is present across a wide swathe of blood cell types and does not discriminate between them here.

**Cluster 9’s row is a warning, not a measurement.** Seven of its twelve values are exactly `0.00` and `PAX5` is 5.61, the highest number in the table. Those come from a test on **four peaks**. **When the denominator is four, a single peak containing or lacking a motif swings the fold enrichment enormously, and any motif absent from all four peaks reads as a hard zero.** Taken at face value this row would make cluster 9 the most convincing B cell in the dataset, which its marker panel score of 0.064 flatly contradicts. **Always check how many regions went into an enrichment before reading the enrichment.**

**Cluster 2 is flat in a way no other real population is.** Its highest value is `CEBPE` at 1.75 and its lowest are `CEBPA` at 0.19 and `IRF8` at 0.31. No lineage factor is enriched above 2 and several are actively depleted. Combined with a panel maximum of 0.048, this cluster has no detectable regulatory program of any kind.

**Cluster 16’s `EBF1` at 1.38 is not a B cell signal.** It is simply the largest of twelve values that all sit near 1.0, which is what `apply(tf_matrix, 2, which.max)` returns when nothing is enriched. The real signal for cluster 16 is the `REL` and `RELA` enrichment from Step 22, which never enters this lineage-factor table. **A “top TF” column is only meaningful when the top value is actually elevated.**

---

### A Structural Check: Where Do the Marker Peaks Sit Relative to Genes?

A final structural check on the marker peaks, which costs nothing and calibrates how much to trust nearest-gene reasoning.

```r
#-----------------------------------------------
# STEP 24: Distance from marker peaks to the nearest annotated feature
#-----------------------------------------------

# distance = 0 means the peak overlaps the annotated feature (an exon or
# promoter); larger distances indicate distal elements.
marker_peak_anno$element <- cut(
  marker_peak_anno$distance,
  breaks = c(-1, 0, 2000, 10000, 100000, Inf),
  labels = c("Overlapping", "Proximal (<2kb)", "Near (2-10kb)",
             "Distal (10-100kb)", "Far (>100kb)")
)

round(prop.table(table(marker_peak_anno$element)), 3)
```

**Output:**

```
      Overlapping   Proximal (&lt;2kb)     Near (2-10kb) Distal (10-100kb)
            0.645             0.027             0.062             0.190
     Far (>100kb)
            0.076
```

> 

**Reading this distribution.** Roughly 65 percent of marker peaks overlap an annotated feature, and a further 3 percent sit within 2 kb. But **more than a quarter of all cluster-defining peaks sit over 10 kb from any annotated feature**, and 7.6 percent are more than 100 kb away.

**The high overlapping fraction needs a caveat.** The annotation carried from Part 3 contains 3,870,812 exon-level ranges covering entire gene bodies including introns, so a peak in the middle of a large intron counts as “overlapping”. This is not the same as “promoter-proximal”, and reading it that way would overstate how much of the signal is interpretable through gene names.

**This is the structural reason the top-genes table in Step 20 returned `MYT1L` and `KRT73` rather than immune markers**, and the reason motif enrichment — which needs no gene assignment at all — is a valuable third line of evidence.

---

## 🧩 Building the Final Consensus Annotation

Five independent lines of evidence now exist for every cluster. This section reconciles them.

### Assembling the Evidence Table

```r
#-----------------------------------------------
# STEP 25: Collect all annotation evidence into one table
#-----------------------------------------------

# Highest-scoring marker panel per cluster.
best_panel <- colnames(score_matrix)[apply(score_matrix, 1, which.max)]

# Most enriched lineage TF motif per cluster. which.max ignores NA, but returns
# integer(0) if a cluster has no values at all, so guard for that.
best_tf <- apply(tf_matrix, 2, function(x) {
  if (all(is.na(x))) NA_character_ else rownames(tf_matrix)[which.max(x)]
})

evidence <- data.frame(
  cluster        = rownames(score_matrix),
  n_cells        = as.integer(table(atac$cluster_final)[rownames(score_matrix)]),
  label_transfer = vote_summary$top_label[match(rownames(score_matrix), vote_summary$cluster)],
  lt_margin      = vote_summary$margin[match(rownames(score_matrix), vote_summary$cluster)],
  singler        = singler_main$labels[match(rownames(score_matrix), rownames(singler_main))],
  top_panel      = best_panel,
  top_tf         = best_tf,
  row.names      = NULL
)

# Order numerically rather than lexicographically.
evidence <- evidence[order(as.numeric(evidence$cluster)), ]
evidence
```

**Output:**

```
   cluster n_cells    label_transfer lt_margin         singler top_panel top_tf
1        0    5116      CD8 effector     0.320         T cells     CD8_T  EOMES
2        1    4376   CD14+ Monocytes     0.958       Monocytes  Monocyte  CEBPE
3        2    4013         CD4 Naive     0.042    CD4+ T cells   NK_cell  CEBPE
...
11      10     768 B cell progenitor     0.933         B cells    B_cell   EBF1
12      11     731        pre-B cell     0.987         B cells    B_cell   PAX5
...
16      15     171               pDC     0.117    CD4+ T cells Dendritic   IRF8
17      16      87   CD14+ Monocytes     0.552 Dendritic cells Dendritic   EBF1
```

*Truncated: 17 rows. Written to `annotation_evidence_summary.csv`.*

> 

**Reading this table.** Rows where the evidence columns agree are settled. Rows where they diverge require a judgment call, and recording the reasoning matters as much as recording the label.

**Eight clusters are unambiguous:** 1 (monocyte), 3 and 4 (conventional T), 5 (NK), 6 (T), 8 and 11 (B), 12 and 13 (monocyte). In each, at least three of the four evidence columns point to the same lineage.

**Cluster 0 needs a decision between CD8 effector and NK.** Label transfer says `CD8 effector`, SingleR says the generic `T cells` at delta 0.006, the top panel is `CD8_T` (0.143), and `EOMES` reaches 3.27. The panel table also puts it at 0.109 on `NK_cell`. **This is the CD8-effector/NK boundary, genuinely blurred in chromatin** because both run the same cytotoxic program. Label transfer and the panel both favour CD8, so `CD8 T effector` is the defensible call, with the caveat that NK contamination is likely.

**Cluster 16 is a dendritic cell despite label transfer calling it a monocyte.** Its `Dendritic` panel score of 0.120 is more than double any other cluster’s, SingleR agrees at its highest delta in the table (0.231), and Step 22 returned `REL`, `RELA` and `FOS::JUNB` — NF-kB activation, characteristic of dendritic cells. Its `top_tf` of `EBF1` at 1.38 is noise, as noted above. **Three of four columns outvote a 0.552-margin transfer call.**

**Cluster 10 is a B cell despite `EBF1` rather than `PAX5` topping its motif list.** Its B cell panel score is 0.364, the second highest in the dataset. `EBF1` and `PAX5` are the two B cell commitment factors, and either topping the list is consistent.

The evidence table shows *what* each method concluded. When methods disagree or return nothing, the next question is *why* — and two cheap diagnostics answer it. The first asks whether a cluster had enough signal to work with at all.

```r
# Per-cluster chromatin quality, worst first. All three columns come from the
# QC metrics computed in Part 2 and carried through the object.
qc_by_cluster <- data.frame(
  cluster = levels(atac$cluster_final),
  frip    = round(tapply(atac$pct_reads_in_peaks, atac$cluster_final, median), 2),
  tss     = round(tapply(atac$TSS.enrichment,     atac$cluster_final, median), 2),
  n_peaks = round(tapply(atac$nFeature_peaks,     atac$cluster_final, median)),
  row.names = NULL
)
qc_by_cluster[order(qc_by_cluster$frip), ]
```

**Output:**

```
   cluster  frip  tss n_peaks
         9 20.30 4.33    4181
        15 28.55 5.81    2549
         7 32.06 5.33    2383
         2 46.14 6.26    2817
        16 59.35 6.23    4597
...
         5 71.67 7.61    3669
        14 73.08 7.64    4138
         0 73.80 7.64    4267
```

*Truncated: 17 rows, ascending by FRiP.*

> 

**Reading this table.** The four clusters that resisted annotation — 9, 15, 7 and 2 — are exactly the four lowest on fraction of reads in peaks, and they are separated from the rest by a wide gap: 46.14 percent for cluster 2 against 59.35 percent for the next cluster up. **Annotation difficulty in this dataset is almost perfectly predicted by chromatin quality.**

**This reframes cluster 2 entirely.** Its Part 3 QC metrics were unremarkable when read against the whole-dataset thresholds, but per cluster it sits fourth from the bottom, with a median of 2,817 peaks per cell against 3,400 to 5,500 for the confidently annotated clusters. **Low complexity attenuates every derived score at once** — module scores shrink toward zero, motif enrichments flatten toward 1.0, and label transfer scatters across labels. That is the whole explanation for why four independent methods all returned nothing on this cluster, and it is a much more useful diagnosis than “novel population”.

**Cluster 9 is the exception that confirms the pattern.** Its 4,181 median peaks per cell is mid-range, yet its FRiP is 20.30 percent. Plenty of fragments, very few of them in peaks — the signature of ambient or poorly-nucleated material rather than undersampling.

The second diagnostic asks *how* the transferred labels failed: a cluster that is one cell type with poor data spreads across related labels, while a genuine mixture spreads across unrelated ones.

```r
# For the clusters that resisted annotation: how are the transferred labels
# distributed within each one?
lapply(c("2", "7", "9", "15"), function(cl) {
  tab <- prop.table(table(atac$predicted.id[atac$cluster_final == cl]))
  round(head(sort(tab, decreasing = TRUE), 4) * 100, 1)
})
```

**Output:**

```
[[1]]
      CD4 Naive      CD4 Memory CD14+ Monocytes          NK dim
           28.2            24.0            22.4             9.0

[[2]]
CD14+ Monocytes CD16+ Monocytes       CD4 Naive      pre-B cell
           74.8            12.9            10.6             0.6

[[3]]
      CD4 Naive CD14+ Monocytes      CD4 Memory      pre-B cell
           56.9            20.2            13.0             3.8

[[4]]
            pDC       CD4 Naive      CD4 Memory CD14+ Monocytes
           36.8            25.1            12.3             8.2
```

> 

**Reading these four distributions — this is what separates a salvageable cluster from a lost one.**

**Cluster 7 is coherent.** 87.7 percent of its cells received a monocyte label, split between classical and non-classical. Only the subtype is in question, not the lineage. **A cluster this internally consistent is a real population with poor data**, and deleting it on its 32.06 percent FRiP would discard 1,585 genuine monocytes.

**Clusters 2, 9 and 15 are incoherent.** Each spreads across lineages that have nothing to do with each other — cluster 2 puts 22.4 percent of its cells in monocytes alongside 52.2 percent in CD4 T cells; cluster 9 does the same at 20.2 and 69.9 percent. **Labels scattered across unrelated lineages mean the transfer had nothing to grip.** Whether that reflects a true mixture of cell types or uniformly weak signal cannot be settled from this table alone — but Step 30 settles it for cluster 2 using raw coverage, and the answer is not what these numbers suggest.

**Cluster 15’s 36.8 percent pDC plurality is the one weak positive** among the three, and it is why that cluster gets `Unresolved` rather than `Low quality` below.

### Handling the Unresolved and Low-Quality Clusters

Part 3 flagged clusters 7, 9 and 15 as QC-suspect and correctly declined to delete them. The evidence assembled here separates them, and they turn out to require three different decisions. Cluster 2, which passed QC comfortably, turns out to need a fourth.

**7**`Mono_CD16``CDKN1C``CD300LB``KLF4`**Keep. CD16 monocyte.****2****Keep, label as unresolved.****15**`Dendritic`**Label as unresolved.****9****Flag as low quality.**

| Cluster | Cells | Median FRiP | What the evidence says | Decision |
| --- | --- | --- | --- | --- |
|  | 1,585 | 32.06% | 87.7% myeloid-predicted;  top panel;  accessible in Step 28; ,  peaks; CEBP motifs | Coherent lineage, poor data |
|  | 4,013 | 46.14% | Transfer margin 0.042; SingleR delta 0.000; all panels below 0.05; all lineage TFs below 1.8; labels spread across unrelated lineages | Resolved further in Step 30 |
|  | 171 | 28.55% | Lowest transfer confidence (0.305); 36.8% pDC plurality;  top panel (0.056); housekeeping motifs; 151 peaks | A weak dendritic lean, nothing confirmatory |
|  | 863 | 20.30% | 4 significant peaks, all in segmental duplications; no panel above 0.064; seven motif values exactly zero | No chromatin program of any kind |

> 

**Three QC-suspect clusters, three different answers.** Cluster 7 is a real monocyte population whose low FRiP reflects genuinely sparse chromatin rather than dying cells — 87.7 percent of its cells received a monocyte label, which no artifact would produce. Cluster 9 is an artifact. Cluster 15 is too small and too weak to call. **A pipeline that deleted every cluster below a QC threshold would have discarded 1,585 real monocytes along with the artifacts.**

**Cluster 2 is the instructive case, and the QC table in Step 25 changed the diagnosis.** Read against whole-dataset thresholds its metrics looked unremarkable, which is why Part 3 did not flag it. Read per cluster, it sits fourth from the bottom on FRiP with a median of 2,817 peaks per cell. **Every annotation method failed on it because low complexity attenuates every derived score simultaneously** — not because the cells are exotic. That distinction matters, because the remedies are different: an exotic population needs a better reference, while an under-sampled one needs either deeper data or a method that does not depend on derived scores.

**What to do about cluster 2 in practice.** Step 30 takes the second route and inspects raw fragment coverage, which is the one form of evidence that sparsity does not average away. Before that, two other options are worth knowing: subcluster at higher resolution to see whether it separates, or check whether it merges into a neighbour at resolution 0.2, since Part 3 retained all six resolution columns for exactly this purpose. Labelling it `CD4 Naive` because that label won a four-percent plurality would not be defensible.

**On whether to delete the low-quality clusters.** The recommendation is to **label rather than remove**. Removing cells changes the composition denominator, silently distorting every proportion reported downstream. Labelling keeps the accounting honest, keeps the decision visible to reviewers, and allows explicit exclusion in the specific analyses that require it.

### Assigning the Final Labels

```r
#-----------------------------------------------
# STEP 26: Map clusters to final cell type labels
#-----------------------------------------------

# One entry per cluster, in cluster order 0 through 16. Every label is
# justified by the evidence table and the marker peaks in Step 20.
final_labels <- c(
  "0"  = "CD8 T effector",
  "1"  = "CD14 monocyte",
  "2"  = "NK/T cell unresolved",
  "3"  = "CD4 T naive",
  "4"  = "CD4 T memory",
  "5"  = "NK cell",
  "6"  = "CD8 T naive",
  "7"  = "CD16 monocyte",
  "8"  = "B cell",
  "9"  = "Low quality",
  "10" = "B cell",
  "11" = "B cell",
  "12" = "CD14 monocyte",
  "13" = "CD14 monocyte",
  "14" = "CD8 T effector",
  "15" = "Unresolved",
  "16" = "Dendritic cell"
)

# unname() is required: indexing a named vector by name carries the cluster IDs
# through as names, and Seurat's $<- checks those names against cell barcodes.
atac$cell_type <- unname(final_labels[as.character(atac$cluster_final)])

# A factor with a deliberate lineage ordering, so downstream plots group
# related cell types together instead of ordering them alphabetically.
atac$cell_type <- factor(atac$cell_type, levels = c(
  "CD4 T naive", "CD4 T memory", "CD8 T naive", "CD8 T effector",
  "NK cell", "NK/T cell unresolved", "B cell",
  "CD14 monocyte", "CD16 monocyte", "Dendritic cell",
  "Unresolved", "Low quality"
))

Idents(atac) <- "cell_type"
sort(table(atac$cell_type), decreasing = TRUE)
```

**Output:**

```
      CD8 T effector        CD14 monocyte NK/T cell unresolved
                5326                 5213                 4013
         CD4 T naive         CD4 T memory               B cell
                3828                 3148                 2716
             NK cell          CD8 T naive        CD16 monocyte
                1931                 1690                 1585
         Low quality           Unresolved       Dendritic cell
                 863                  171                   87
```

Four labelling decisions deserve explanation, since each involved a judgment:

1. **The reference’s “pre-B cell” and “B cell progenitor” labels were discarded.** Clusters 8, 10 and 11 received those names with transfer margins of 0.98, 0.93 and 0.99 — among the most confident calls in the dataset. They are still developmentally wrong for peripheral blood. All three are labelled `B cell` here. **Report what the data show, not what the reference happened to call it.**

2. **The three B cell clusters were not subtyped.** Cluster 8’s marker peaks include `TCL1A`, which is suggestive of naive B cells, but clusters 10 and 11 offer no comparable signal, and the panel scores (0.345, 0.364, 0.369) are indistinguishable. Splitting them into naive and memory on the strength of one gene would be over-reach. **Match label granularity to what the evidence supports.**

3. **Clusters 0 and 14 are merged as `CD8 T effector` despite the NK ambiguity.** Both have `EOMES` and `TBX21` as top motifs (3.27 / 2.92 and 2.84 / 2.77) and `CD8_T` as top panel, and both received `CD8 effector` from label transfer. The cytotoxic program is shared with NK cells, so some mixing is likely, and this belongs in any methods section.

4. **Cluster 7 is labelled `CD16 monocyte` against a majority transfer vote.** Label transfer put 74.8 percent of its cells in `CD14+ Monocytes` and only 12.9 percent in `CD16+ Monocytes`. The label follows the other lines instead: `Mono_CD16` is its top panel, and Step 28 shows clear `CDKN1C` accessibility — a canonical non-classical marker — with little `CD14` or `VCAN`. **At 32.06 percent FRiP its transferred labels are the least reliable evidence available for it**, which is why they lose here. Flag this one as tentative.

5. **Two clusters carry non-biological labels.** `Unresolved` for cluster 15 and `Low quality` for cluster 9 record that the analysis reached no conclusion, rather than hiding that behind a plausible-looking name. `NK/T cell unresolved` for cluster 2 records the lineage the coverage plots in Step 30 support — cytotoxic lymphoid — while declining a subtype the data cannot sustain.

> 

**A note on scope.** Clusters 12 and 13, the near-pure `severe1` clusters, are labelled `CD14 monocyte` alongside cluster 1. Every method agrees on the lineage, and their transfer margins of 0.996 and 1.000 are the highest in the dataset. Whether their separation from cluster 1 reflects a disease-associated monocyte state or donor-specific variation cannot be determined with two donors per condition. The composition plots in Step 29 make the donor imbalance visible, and the responsible framing in a manuscript is a hypothesis for a larger cohort, not a finding.

---

## 📊 Visualizing the Annotated Dataset

### The Annotated UMAP

```r
#-----------------------------------------------
# STEP 27: Annotated UMAP, alongside the cluster numbering
#-----------------------------------------------

p_umap_clusters <- DimPlot(
  object     = atac,
  reduction  = "umap.harmony",
  group.by   = "cluster_final",
  label      = TRUE,
  label.size = 3.5,
  pt.size    = 0.05
) + ggtitle("Clusters (resolution 0.4)") + NoLegend() + theme(aspect.ratio = 1)

p_umap_types <- DimPlot(
  object     = atac,
  reduction  = "umap.harmony",
  group.by   = "cell_type",
  label      = TRUE,
  label.size = 3,
  repel      = TRUE,
  pt.size    = 0.05
) + ggtitle("Annotated cell types") + NoLegend() + theme(aspect.ratio = 1)

p_umap_pair <- p_umap_clusters | p_umap_types

ggsave(file.path(plot_dir, "05_annotated_umap.png"),
       plot = p_umap_pair, width = 13, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/05_annotated_umap.png?resize=1024%2C473&ssl=1)

> 

**Reading this figure.** The two panels should be readable as the same map, with each cell type occupying a contiguous territory.

**Most of the annotation holds up spatially.** The three B cell clusters form one island at the bottom. The conventional T clusters (3, 4, 6) sit together on the left. Clusters 0 and 5 — CD8 effector and NK — are adjacent along the bottom right, which is the expected arrangement for two populations sharing a cytotoxic program. Cluster 7 forms its own well-separated blob at upper right, supporting a distinct monocyte subset rather than a smear of low-quality cells. Cluster 16 sits immediately beside cluster 1, consistent with a dendritic cell population next to monocytes.

**Two clusters break the adjacency rule, and both deserve a note in the methods.** Cluster 14 (`CD8 T effector`) is an isolated island at the far right, nowhere near cluster 0 which carries the same label. Cluster 13 (`CD14 monocyte`) is an isolated island at the far left, nowhere near clusters 1 and 12. **Sharing a label while sitting at opposite ends of the embedding is a signal to check the call.**

For cluster 14 the check passes: its QC is the second-best in the dataset (FRiP 73.08, TSS 7.64), and its panel scores, `EOMES` enrichment and transferred label all match cluster 0. Two populations can share an identity and still separate in an embedding when one is small. For cluster 13 the explanation is donor structure rather than biology — it is 99.5 percent `severe1`, as Part 3 reported, and single-donor clusters routinely detach.

**Cluster 2 sits contiguous with the lymphoid mass**, between the T territory and the NK/CD8-effector arm rather than off on its own. That position is consistent with lymphoid identity and argues against it being an unrelated contaminant. **Cluster 9 sits in the middle of the plot, bridging several territories** — the classic position for cells with no distinctive signal, which fall between real populations rather than joining one.

### Marker Gene Activity Across Cell Types

```r
#-----------------------------------------------
# STEP 28: Dot plot of canonical markers by annotated cell type
#-----------------------------------------------

DefaultAssay(atac) <- "ACTIVITY"

# One or two definitive markers per annotated type, ordered by lineage so the
# diagonal structure is visible.
dotplot_markers <- c(
  "CCR7", "LEF1", "IL7R", "S100A4", "CD8A", "GZMK",
  "NKG7", "GNLY", "KLRD1", "KLRF1",
  "MS4A1", "CD79A", "BANK1", "TCL1A",
  "LYZ", "CD14", "VCAN", "FCGR3A", "MS4A7", "CDKN1C",
  "CD1C", "FLT3", "IRF8"
)

p_dot <- DotPlot(
  object   = atac,
  features = dotplot_markers,
  assay    = "ACTIVITY"
) +
  scale_colour_gradient2(low = "#2166AC", mid = "grey90", high = "#B2182B") +
  labs(
    title = "Marker gene activity across annotated cell types",
    x     = "Marker gene",
    y     = "Cell type"
  ) +
  theme_bw(base_size = 11) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))

ggsave(file.path(plot_dir, "06_marker_dotplot.png"),
       plot = p_dot, width = 12, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/06_marker_dotplot.png?resize=1024%2C512&ssl=1)

> 

**Reading this figure:** dot **colour** is mean gene activity within the cell type; dot **size** is the fraction of cells with any signal at that locus. The goal is a visible diagonal, with each marker’s largest and reddest dot on its own row. **Expect it to be blurrier than its scRNA-seq equivalent** — dot sizes top out near 60 percent because in sparse chromatin data most cells have zero fragments at any given gene. Compare across rows, not against absolute values.

**The diagonal is clean for four compartments.** B cells carry `MS4A1`, `CD79A` and `TCL1A`. CD14 monocytes carry `CD14`, `VCAN`, `LYZ` and `MS4A7`. NK cells carry `NKG7`, `GNLY`, `KLRD1` and `KLRF1`. CD4 and CD8 naive T cells carry `CCR7`, `LEF1` and `IL7R`, with `CD8A` cleanly separating CD8 naive from CD4.

**Two rows resolve questions the earlier methods left open.** The `CD16 monocyte` row shows strong `CDKN1C` — a canonical non-classical monocyte marker — and little `CD14` or `VCAN`, which is the strongest single piece of evidence for that label and the reason it survived the 74.8 percent `CD14+ Monocytes` transfer vote. The `Dendritic cell` row shows `FLT3` as its brightest marker, confirming the 87-cell call.

**The pDC panel contamination from Step 18 is visible and explained here.** `IRF8` is strongly accessible in the B cell row. Since `IRF8` and `TCF4` are two of the four genes in the pDC panel, that panel necessarily reported B cell signal. **A dot plot of the individual genes is the fastest way to diagnose a panel that is misbehaving**, and it is worth building one whenever two panels light up together.

**The `NK/T cell unresolved` row shows weak `NKG7` and `KLRD1` and nothing else.** Faint, but in the right place — the cytotoxic markers rather than the monocyte or B cell ones. That is the first positive evidence for a lymphoid identity on cluster 2, and Step 30 tests it directly.

**The `Low quality` row is diagnostic in its own way.** It carries scattered mid-intensity signal at `LEF1`, `VCAN` and `S100A4` with no coherent pattern — exactly what a mixture of ambient fragments looks like when averaged.

### Cell Type Composition by Sample

```r
#-----------------------------------------------
# STEP 29: Cell type composition by sample
#-----------------------------------------------

comp_df <- as.data.frame(table(
  cell_type = atac$cell_type,
  sample    = atac$sample_id
))

p_comp_sample <- ggplot(comp_df, aes(x = cell_type, y = Freq, fill = sample)) +
  geom_col(position = "fill") +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title    = "Sample composition of each annotated cell type",
    subtitle = "Cell numbers are strongly unbalanced across samples -- see note below",
    x = NULL, y = "Proportion of cells"
  ) +
  theme_bw(base_size = 11) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))

# The same data as proportions within each sample, which is the comparison that
# matters when sample sizes differ by more than fourfold.
prop_df <- as.data.frame(prop.table(
  table(cell_type = atac$cell_type, sample = atac$sample_id), margin = 2
))

p_comp_prop <- ggplot(prop_df, aes(x = sample, y = Freq, fill = cell_type)) +
  geom_col() +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Cell type composition within each sample",
    x = NULL, y = "Proportion of that sample's cells"
  ) +
  theme_bw(base_size = 11)

p_comp_pair <- p_comp_sample / p_comp_prop

ggsave(file.path(plot_dir, "07_celltype_composition.png"),
       plot = p_comp_pair, width = 11, height = 10, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/07_celltype_composition.png?resize=1024%2C931&ssl=1)

> 

**Reading this figure, and a warning carried over from Part 3.** The four samples contribute unequal cell numbers — `healthy2` 15,452, `healthy1` 6,861, `severe2` 4,773, `severe1` 3,485. In the **top** panel, a cell type that is 50 percent `healthy2` is therefore perfectly average, not `healthy2`-biased. Reading that panel against a naive 25 percent baseline would manufacture donor effects everywhere.

The **bottom** panel is the one to interpret, because normalizing within each sample removes the imbalance. Read that way, the two severe samples share a pattern the healthy samples do not:

| Cell type | healthy1 | healthy2 | severe1 | severe2 |
| --- | --- | --- | --- | --- |
| B cell | 3.9% | 5.9% | 19.9% | 17.6% |
| CD14 monocyte | 11.8% | 16.7% | 38.0% | 10.6% |
| CD8 T effector | 30.6% | 16.2% | 8.8% | 8.6% |
| NK cell | 4.5% | 9.2% | 2.8% | 2.1% |
| CD16 monocyte | 3.8% | 6.7% | 0.8% | 5.4% |

**B cells are three to four times more abundant in both severe samples, and the cytotoxic compartment is roughly a third of its healthy level.** Lymphopenia with relative myeloid and B expansion is the textbook peripheral blood picture in severe COVID-19, so this is a reassuring sanity check on the annotation rather than a novel finding. **An annotation that produced a biologically implausible composition would be the thing to worry about.**

**Two caveats before reading anything further into it.** The `CD14 monocyte` expansion is driven almost entirely by `severe1` (38.0 percent against 10.6 in `severe2`), and clusters 12 and 13 — 99.8 and 99.5 percent `severe1` — account for much of it. That is donor structure, not a condition effect. And `Low quality` sits between 2.1 and 3.1 percent in all four samples, which is the reassuring result: technical failure is spread evenly rather than concentrated in one library.

**With two donors per condition, none of this supports a statistical claim.** Compositional differences between conditions require many more donors. What exists here is a description of four individuals, and methods sections should say so explicitly.

### Coverage Plots at Definitive Marker Loci

Coverage plots are the most direct evidence available. They display actual fragment pileups rather than derived scores, and they are the figure most likely to convince a skeptical reviewer.

```r
#-----------------------------------------------
# STEP 30: Coverage plots at loci supporting the main annotations
#-----------------------------------------------

DefaultAssay(atac) <- "peaks"

# MS4A1 is an uncontested B cell marker and serves as the positive control:
# if this locus does not look clean, distrust the other two plots.
p_cov_ms4a1 <- CoveragePlot(
  object            = atac,
  region            = "MS4A1",
  extend.upstream   = 10000,
  extend.downstream = 10000,
  annotation        = TRUE,
  peaks             = TRUE
) & theme(strip.text.y.left = element_text(angle = 0, size = 7))

ggsave(file.path(plot_dir, "08_coverage_MS4A1.png"),
       plot = p_cov_ms4a1, width = 9, height = 9, dpi = 300)

# CD14 separates the monocyte compartment from everything else.
p_cov_cd14 <- CoveragePlot(
  object            = atac,
  region            = "CD14",
  extend.upstream   = 10000,
  extend.downstream = 10000,
  annotation        = TRUE,
  peaks             = TRUE
) & theme(strip.text.y.left = element_text(angle = 0, size = 7))

ggsave(file.path(plot_dir, "09_coverage_CD14.png"),
       plot = p_cov_cd14, width = 9, height = 9, dpi = 300)

# GNLY is the sharpest test of the unresolved cluster: it should be open in NK
# and CD8 effector rows, and its behaviour in the unresolved row is diagnostic.
p_cov_gnly <- CoveragePlot(
  object            = atac,
  region            = "GNLY",
  extend.upstream   = 10000,
  extend.downstream = 10000,
  annotation        = TRUE,
  peaks             = TRUE
) & theme(strip.text.y.left = element_text(angle = 0, size = 7))

ggsave(file.path(plot_dir, "10_coverage_GNLY.png"),
       plot = p_cov_gnly, width = 9, height = 9, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/scatacseq_coverage_MS4A1_CD14_GNLY.png?resize=1024%2C340&ssl=1)

> 

**Reading these figures.** Each row is one cell type; the grey peak track at the bottom shows which regions are in the consensus peak set. **Look at the whole locus, not just the promoter.** Part 3 taught this lesson with `CD3E`, whose promoter was open everywhere while distal elements discriminated cleanly.

**`MS4A1` and `CD14` behave as positive controls should**, with the B cell and CD14 monocyte rows unmistakable against flat traces elsewhere. If either had looked ambiguous, nothing concluded from the third plot would be trustworthy.

**`GNLY` is the figure that resolves cluster 2, and it does so against every score-based method.** The CD8 effector and NK rows show a series of sharp peaks across the gene body and downstream. The `NK/T cell unresolved` row shows **the same peaks at the same positions**, lower and broader but unmistakably present — while CD4 naive, CD4 memory, CD8 naive, B cell and CD14 monocyte rows are flat across the same interval.

**This is a real cytotoxic lymphoid population, not a mixture and not an artifact.** Every derived measure said otherwise: a panel maximum of 0.048, no lineage motif above 1.8, a transfer margin of 0.042, and 22.4 percent of its cells predicted as monocytes. All of those failed for the same reason — 46.14 percent FRiP and 2,817 median peaks per cell flatten averages toward zero and scatter per-cell predictions. **Raw coverage does not average away.** Pooling 4,013 cells at a specific locus recovers a signal that pooling the same cells into a module score destroyed.

**The practical lesson generalizes past this dataset.** When several score-based methods fail on the same cluster, before concluding the population is novel or the cells are junk, plot coverage at two or three loci for the lineages you suspect. It costs minutes and it is the one form of evidence that sparsity does not erase. The `NK/T cell unresolved` label stays — the subtype is still undetermined — but its meaning has changed from “no idea” to “cytotoxic lymphoid at insufficient depth to subtype”.

**The `Low quality` and `Unresolved` rows confirm their own labels.** Cluster 9 shows broad low-level signal across the entire window with no peak structure, the classic ambient profile. Cluster 15 shows sparse isolated spikes, the profile of a population too small and too shallow to build a coverage track from.

### Annotation Confidence by Cluster and Label

The final table exists to keep the analysis honest, showing where the annotation is well-supported and where it is not.

```r
#-----------------------------------------------
# STEP 31: Report transfer confidence alongside the final label
#-----------------------------------------------

p_conf_split <- FeaturePlot(
  object    = atac,
  features  = "prediction.score.max",
  reduction = "umap.harmony",
  pt.size   = 0.05,
  split.by  = "condition"
) & scale_colour_viridis_c(option = "magma") & theme(aspect.ratio = 1)

ggsave(file.path(plot_dir, "11_confidence_by_condition.png"),
       plot = p_conf_split, width = 11, height = 5, dpi = 300)

# Per-cluster median confidence next to the final label, worst first. This
# reads more usefully than a per-cell-type median, which would average away
# exactly the clusters that need attention.
conf_table <- data.frame(
  cluster    = cluster_conf$cluster_final,
  confidence = round(cluster_conf$prediction.score.max, 3),
  cell_type  = unname(final_labels[as.character(cluster_conf$cluster_final)]),
  row.names  = NULL
)
conf_table[order(conf_table$confidence), ]
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/08/11_confidence_by_condition.png?resize=1024%2C465&ssl=1)

**Output:**

```
   cluster confidence            cell_type
        15      0.305           Unresolved
         2      0.374 NK/T cell unresolved
        14      0.413       CD8 T effector
         9      0.453          Low quality
         0      0.485       CD8 T effector
         7      0.578        CD16 monocyte
...
         8      0.842               B cell
        11      0.972                B cell
        13      0.986        CD14 monocyte
        12      0.990        CD14 monocyte
```

*Truncated: 17 rows, ascending by confidence.*

> 

**Reading this table alongside the split-by-condition figure.** The two clusters labelled non-biologically — `Unresolved` and `Low quality` — sit at positions one and four. That is the expected behaviour of a well-functioning method meeting the edge of its evidence, and it is reassuring that the labels and the confidence scores agree.

**The cytotoxic compartment is the weakest well-labelled part of the annotation.** Clusters 14 and 0, both `CD8 T effector`, sit at 0.413 and 0.485 — below the conventional 0.5 threshold. This reflects the genuine CD8-effector/NK ambiguity discussed in Step 25 rather than a technical failure, and it should temper any downstream claim that depends on separating those two populations cleanly.

**The B cell and monocyte compartments are the strongest**, all above 0.84. Those calls can carry weight in downstream analysis. **A confidence column of this kind belongs in supplementary material**, because it lets a reader see which of your conclusions rest on solid ground.

---

## 💾 Saving Your Annotated Object

```bash
#-----------------------------------------------
# STEP 32: Save the annotated object and supporting tables for Part 5
#-----------------------------------------------

saveRDS(atac, file.path(obj_dir, "atac_annotated.rds"))

# The full evidence table: the most useful supplementary file this tutorial
# produces, because it shows a reader exactly how each label was reached.
write.csv(evidence,
          file.path(table_dir, "annotation_evidence_summary.csv"),
          row.names = FALSE)

# Marker panel scores and motif enrichment, for anyone checking the work.
write.csv(round(score_matrix, 4),
          file.path(table_dir, "marker_panel_scores.csv"))
write.csv(round(t(tf_matrix), 4),
          file.path(table_dir, "lineage_tf_motif_enrichment.csv"))
write.csv(conf_table,
          file.path(table_dir, "cluster_confidence_and_labels.csv"),
          row.names = FALSE)

# Per-cluster chromatin quality from Step 25: the table that explains which
# annotation failures were caused by data rather than by biology.
write.csv(qc_by_cluster,
          file.path(table_dir, "cluster_qc_summary.csv"),
          row.names = FALSE)

# Cell-level metadata as a flat table.
write.csv(atac@meta.data,
          file.path(annot_dir, "annotated_cell_metadata.csv"))

# Exact package versions for the methods section.
writeLines(capture.output(sessionInfo()),
           file.path(annot_dir, "sessionInfo_part4.txt"))
```

> 

**Expect a 6 to 12 GB object.** It carries two assays (`peaks`, `ACTIVITY`), six reductions, and the sparse peak-by-motif match matrix attached by `AddMotifs()`. The `ACTIVITY` assay is cheaper to keep than to regenerate, since rebuilding it means re-reading every fragment file.

---

## ✅ Best Practices for scATAC-seq Cell Type Annotation

**1. Annotate clusters, not cells.** A single scATAC-seq cell observes fragments in a few thousand of 185,581 peaks, and the observation at any marker locus is a 1 or a 0 with no way to distinguish “closed” from “not sampled”. Pooling thousands of cells converts that into an interpretable signal. Per-cell predictions remain useful as a *diagnostic* — the confidence distribution in Step 11 is genuinely informative — but the annotation belongs at the cluster level.

**2. Never rely on a single line of evidence.** Every method here has a characteristic failure mode. Label transfer cannot report a cell type the reference lacks, and it inherits the reference’s naming errors with high confidence. SingleR on gene activity is a weak discriminator, with every delta in this run below 0.24. Gene activity discards distal enhancers entirely, and nearest-gene assignment mislabels the quarter of peaks that sit over 10 kb from any feature. Motif enrichment cannot distinguish within families. **Convergence across methods with different failure modes is what makes an annotation defensible.**

**3. Read the reference’s label set before transferring, not after.** This reference labels peripheral blood B cells as “B cell progenitor” and “pre-B cell”, and those labels transferred onto clusters 8, 10 and 11 with margins of 0.98, 0.93 and 0.99. **High transfer confidence means the cells match; it says nothing about whether the reference named them correctly.**

**4. Treat low prediction scores as data.** A score of 0.37 is not noise to be filtered away — it reports that this cluster has no good match in the reference. Cluster the low-confidence cells, examine them, and ask what the reference is missing. In this dataset that question identified cluster 2, a 4,013-cell population that no method could annotate.

**5. When every score-based method fails on a cluster, plot raw coverage before concluding anything.** Cluster 2 defeated marker panels, motif enrichment, SingleR and label transfer, then showed an unmistakable `GNLY` signature in the coverage track at the same peak positions as NK and CD8 effector cells. Module scores, fold enrichments and prediction scores are all averages, and averages of sparse data collapse toward zero. Coverage does not. This one check takes minutes and can convert a discarded cluster into an annotated one.

**6. Diagnose *why* a method failed, not just *that* it did.** Two cheap tables separate the cases. Per-cluster median FRiP identifies clusters that had insufficient signal to work with — in this dataset the four clusters that resisted annotation were exactly the four lowest. The spread of transferred labels within a cluster separates coherent populations with poor data (cluster 7: 87.7 percent myeloid) from clusters where the transfer had nothing to grip (cluster 9: labels scattered across monocytes and T cells). The remedies differ: the first needs better evidence, the second needs better data.

**7. Check whether marker panels share genes before believing a double-positive.** The B cell clusters scored 0.158 to 0.213 on the pDC panel. There are no pDCs among them — `IRF8` and `TCF4` are two of that panel’s four genes and both are active in B cells. **A panel is only as specific as its least specific member**, and with four-gene panels a single shared gene moves the score substantially.

**8. Check how many features went into any enrichment test.** Cluster 9 returned a `PAX5` fold enrichment of 5.61, the highest value in the entire motif table, computed from **four peaks**. Signac warned about it; the warning was correct. An enrichment statistic without its denominator is uninterpretable.

**9. Read motif families, not individual factors.** `PAX1` and `PAX9` topped the B cell clusters ahead of `PAX5`. `CEBPE`, a granulocyte factor, topped monocyte clusters. `TBR1` and `TBX20`, neuronal and cardiac factors, appeared in lymphocytes. **None of these indicates the named factor is active.** Family members bind near-identical sequences; identify the family from the motif result and the responsible member from the marker evidence.

**10. Distinguish QC failure from unusual biology before deleting anything.** Clusters 7, 9 and 15 shared a QC signature and required three different decisions. Cluster 7 is a real CD16 monocyte population with genuinely sparse chromatin. Deleting on QC thresholds alone would have discarded 1,585 real cells. **Flag on QC; adjudicate on markers.**

**11. Read QC per cluster, not only per dataset.** Cluster 2’s metrics looked unremarkable against whole-dataset thresholds, which is why Part 3 did not flag it. Per cluster it ranked fourth from the bottom on FRiP, and that alone explained why four independent methods returned nothing on it. A per-cluster QC table costs three lines and reframes every annotation failure it touches.

**12. Use marker panels, never single genes.** `CD3E` promoter accessibility is uninformative in this dataset — it is open in B cells and monocytes too. A panel scored against a matched background is robust to any one member being uninformative. Check the panel sizes after intersection, since silently losing members defeats the purpose.

**13. Calibrate expectations to the assay.** Module scores of 0.3 are strong in gene activity data. Dot plot dots will be small. Transfer confidence of 0.66 median is reasonable for cross-modality work. Anyone importing thresholds from scRNA-seq will misread all three.

**14. Label unresolved clusters as unresolved.** `NK/T cell unresolved` and `Unresolved` are more useful to a reader than a confident-looking label backed by a four-percent plurality. Recording the lineage that *is* supported while declining the subtype that is not costs nothing and preserves the option to revisit.

**15. Record the reasoning, not just the labels.** The evidence table from Step 25 is the most valuable supplementary file this tutorial produces. It lets a reader see that cluster 7 was called CD16 monocyte over dendritic cell on the basis of `CD300LB` and `KLF4`, rather than asking them to take the label on faith.

**16. Revisit clustering resolution when annotation demands it.** Part 3 deliberately kept all six resolution columns for this reason. Cluster 2 is the obvious candidate: subclustering may separate it, or resolution 0.2 may merge it into a neighbour. Annotation and clustering are a loop, not a pipeline.

**17. Be explicit about what the sample size supports.** Two donors per condition describes four individuals. It does not support compositional claims about severe COVID-19.

---

## ⚠️ Common Pitfalls and How to Avoid Them

**Trusting a confident transferred label**`table(reference$celltype)`**Reading a motif hit as that factor**`PAX1``CEBPE``TBR1`**Reading enrichment without the denominator**`PAX5`**Ranking marker peaks by fold change alone**`MYT1L``KRT73`**Annotating from promoter accessibility**`CD3E`**Ignoring prediction scores**`predicted.id``prediction.score.max`**Deleting clusters on QC metrics alone****Reading QC only at the dataset level****Giving up on a cluster when the scores fail****Trusting a panel that shares genes with another**`IRF8``TCF4`**Using `lsi` as `weight.reduction`**`harmony`**Wrong `dims` on the Harmony embedding**`2:30``harmony_dims`**`plan("multicore")` inside RStudio**`max.cells.per.ident``Rscript`**`FindAllMarkers()` without failure isolation**`try()``NA`**Scanning motifs on non-standard chromosomes**`AddMotifs``standardChromosomes()`**Ignoring sample imbalance in composition plots**`healthy2`**Over-fine labels****Forgetting which assay is default**`DefaultAssay()`

| Pitfall | Why it happens | Consequence | How to avoid it |
| --- | --- | --- | --- |
|  | High margins look authoritative | Reference naming errors propagate — “pre-B cell” for blood B cells, at margin 0.99 | Inspect  before transferring; sanity-check against tissue biology |
|  | The output names one factor | in B cells,  in monocytes,  in lymphocytes — all family artifacts | Identify the family from the motif, the member from the marker evidence |
|  | Fold enrichment is scale-free | Cluster 9’s  = 5.61 came from four peaks | Always report and check the number of regions tested |
|  | It is the default sort | Surfaces rare high-fold-change peaks near irrelevant genes (, ) | Treat the top-genes table as hypotheses; corroborate with panels and motifs |
|  | Habit from scRNA-seq | Promoters are permissively open across lineages;  was accessible everywhere in Part 3 | Use panels plus distal peaks; check coverage across the whole locus |
|  | is the obvious column | Confidently labels cells the reference cannot describe | Inspect  per cluster and on the UMAP |
|  | Standard filtering advice applied mechanically | Cluster 7’s 1,585 real monocytes would have been removed | Flag on QC, adjudicate with markers before removing anything |
|  | Whole-dataset thresholds hide per-cluster variation | Cluster 2 looked fine in Part 3 and ranked fourth from bottom per cluster | Tabulate median FRiP, TSS and feature count per cluster before annotating |
|  | Panels, motifs and transfer all returning nothing looks conclusive | Cluster 2’s cytotoxic identity was invisible to every derived score | Plot raw coverage at candidate marker loci; sparsity does not average it away |
|  | Panels are written per lineage, not checked against each other | B cell clusters scored 0.16-0.21 on the pDC panel via  and | Dot-plot the individual genes whenever two panels light up together |
|  | It is the reduction most tutorials show | Reintroduces the batch effect Part 3 corrected | Use , all 29 components |
|  | The  convention is memorized from LSI | Silently drops a real component, or errors out | Harmony component 1 is not the depth component; use |
|  | It appears to work | Forked workers are terminated; partial results look like findings | Downsample with , or run parallel code via  in a batch job |
|  | It is the obvious function | One dead worker returns a ragged result that resembles biology | Loop per cluster with  so failures surface as explicit |
|  | Scaffolds survive from Cell Ranger peak calls | aborts with an opaque sequence-lookup error | Subset the peaks assay to  first |
|  | The plot renders fine either way | is half the cohort, so “50 percent healthy2” is average, not biased | Normalize within sample, or draw cohort-proportion reference lines |
|  | Finer sounds better | Splitting three B cell clusters on one gene, or naming cluster 2 from a 4 percent plurality | Match granularity to cell numbers and method resolution |
|  | Two assays coexist in one object | Functions silently operate on the wrong matrix | Set  explicitly at the top of every block |

---

## 🎓 Conclusion: From Anonymous Clusters to Named Cell Types

Seventeen numbered clusters became ten annotated populations plus two honest admissions of uncertainty. Here is what the process established.

1. **Cell type identification in scATAC-seq requires translating chromatin into gene space, and every translation loses information.** Gene activity scores are the standard bridge, but Step 24 showed that over a quarter of cluster-defining peaks sit more than 10 kb from any annotated feature. Gene-level methods are necessary and never sufficient.

2. **The companion scRNA-seq data for this study exists as raw reads only.** PRJNA1164162 is a BioProject, not a GEO series, which structurally means no processed matrix and no annotations. Checking what a repository accession implies costs ten minutes and can save a week.

3. **Reference-based annotation is fast, reproducible, and confidently wrong in specific ways.** The reference labelled peripheral blood B cells as “pre-B cell” and “B cell progenitor”, and those labels transferred onto three clusters with margins above 0.93. High confidence measures agreement between datasets, not correctness of the original annotation.

4. **SingleR against bulk profiles corroborated the broad lineages and resolved nothing finer.** Every `delta.next` value fell below 0.24 and one was exactly zero. On sparse gene activity input the method’s correlations compress toward each other. It remains worth running as an independent check; it is not an adjudicator.

5. **Reference-free methods carried the analysis, and each contributed something the others could not.** Marker panels gave the cleanest lineage assignment, with B cell scores double any other value. Marker peaks resolved cluster 7 as myeloid. Motif enrichment confirmed every major lineage block using sequence alone, with `EOMES` and `TBX21` for cytotoxic cells, `TCF7` and `RUNX3` for conventional T, `PAX5` and `EBF1` for B, and `CEBPA` with `CEBPE` for myeloid.

6. **Motif family degeneracy is the largest interpretive trap in this workflow.** `PAX1` and `PAX9` outranked `PAX5` in B cells. `CEBPE`, a granulocyte-restricted factor, topped two monocyte clusters. `TBR1` and `TBX20` appeared in lymphocytes. Reading any of those literally would produce a false biological claim.

7. **A negative result is still a result.** The granulocyte panel was flat across all seventeen clusters. Low-density granulocytes are reported in severe COVID-19 and were specifically tested for; they are not present in this dataset, and the PBMC preparation worked as intended.

8. **QC metrics describe data quality and cannot describe biology, in both directions.** Cluster 7 failed QC and is a real CD16 monocyte population. Cluster 2 passed QC and defeated every annotation method. Neither property predicts the other.

9. **Raw coverage rescued a cluster that every derived score had written off.** Cluster 2 — 4,013 cells, 13 percent of the dataset — returned a panel maximum of 0.048, no lineage motif above 1.8, a transfer margin of 0.042, and labels scattered across unrelated lineages. Its `GNLY` coverage track shows the cytotoxic peaks at the same positions as NK and CD8 effector cells. **Every failed method was an average, and averages of sparse data collapse toward zero.** The cluster remains unsubtyped, but “cytotoxic lymphoid at insufficient depth” is a far more useful conclusion than “unknown”.

10. **Annotation difficulty tracked chromatin quality almost perfectly.** The four clusters that resisted annotation — 9, 15, 7 and 2 — were the four lowest on per-cluster fraction of reads in peaks, separated from the rest by a wide gap. Reading QC per cluster rather than per dataset turned four separate mysteries into one explanation.

11. **The final composition is biologically plausible, which is itself a check.** Both severe samples show three to four times more B cells and roughly a third the cytotoxic compartment of the healthy samples — the textbook peripheral blood picture in severe COVID-19. An annotation producing an implausible composition would be the thing to worry about. With two donors per condition it remains a description of four individuals.

### What You Now Have

`atac_annotated.rds` carries the accessibility and gene activity assays plus the peak-by-motif match matrix, alongside cluster assignments, transferred labels with confidence scores, marker panel scores, motif enrichment results, and final consensus cell types. Every annotation decision is documented in `annotation_evidence_summary.csv`, and every confidence score in `cluster_confidence_and_labels.csv`.

---

## 🚀 Moving Forward: What Comes in Part 5

With named cell types, the questions that motivated the experiment become askable.

**Part 5: Differential Accessibility Analysis** will cover comparing chromatin accessibility between conditions *within* each cell type — severe COVID-19 against healthy, in monocytes, in T cells, in B cells. It will address the statistical pitfalls specific to sparse binary data, pseudobulk approaches that handle donor-level variation properly, and how to interpret differential peaks in intergenic space.

---

## 📚 References

1. Stuart T, Srivastava A, Madad S, Lareau CA, Satija R. Single-cell chromatin state analysis with Signac. *Nature Methods*. 2021;18(11):1333-1341. doi:10.1038/s41592-021-01282-5

2. Stuart T, Butler A, Hoffman P, et al. Comprehensive integration of single-cell data. *Cell*. 2019;177(7):1888-1902.e21. doi:10.1016/j.cell.2019.05.031

3. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y

4. Aran D, Looney AP, Liu L, et al. Reference-based analysis of lung single-cell sequencing reveals a transitional profibrotic macrophage. *Nature Immunology*. 2019;20(2):163-172. doi:10.1038/s41590-018-0276-y

5. Monaco G, Lee B, Xu W, et al. RNA-Seq signatures normalized by mRNA abundance allow absolute deconvolution of human immune cell types. *Cell Reports*. 2019;26(6):1627-1640.e7. doi:10.1016/j.celrep.2019.01.041

6. Fornes O, Castro-Mondragon JA, Khan A, et al. JASPAR 2020: update of the open-access database of transcription factor binding profiles. *Nucleic Acids Research*. 2020;48(D1):D87-D92. doi:10.1093/nar/gkz1001

7. Corces MR, Buenrostro JD, Satpathy AT, et al. Lineage-specific and single-cell chromatin accessibility charts human hematopoiesis and leukemia evolution. *Nature Genetics*. 2016;48(10):1193-1203. doi:10.1038/ng.3646

8. Aybey B, Zhao S, Brors B, Staub E. scATAcat: cell-type annotation for scATAC-seq data. *NAR Genomics and Bioinformatics*. 2024;6(4):lqae135. doi:10.1093/nargab/lqae135

9. Wang Y, Sun X, Zhao H. Benchmarking automated cell type annotation tools for single-cell ATAC-seq data. *Frontiers in Genetics*. 2022;13:1063233. doi:10.3389/fgene.2022.1063233

10. Zhang K, Hocker JD, Miller M, et al. A single-cell atlas of chromatin accessibility in the human genome. *Cell*. 2021;184(24):5985-6001.e19. doi:10.1016/j.cell.2021.10.024

11. ENCODE Project Consortium, Moore JE, Purcaro MJ, et al. Expanded encyclopaedias of DNA elements in the human and mouse genomes. *Nature*. 2020;583(7818):699-710. doi:10.1038/s41586-020-2493-4

12. Meuleman W, Muratov A, Rynes E, et al. Index and biological spectrum of human DNase I hypersensitive sites. *Nature*. 2020;584(7820):244-251. doi:10.1038/s41586-020-2559-3

13. Amosova V, Tikhonov D, Antipova O, et al. Remodeling of the chromatin landscape in peripheral blood cells in patients with severe Delta COVID-19. *Frontiers in Immunology*. 2024;15:1415317. doi:10.3389/fimmu.2024.1415317

14. Lawrence M, Huber W, Pages H, et al. Software for computing and annotating genomic ranges. *PLoS Computational Biology*. 2013;9(8):e1003118. doi:10.1371/journal.pcbi.1003118

15. Signac documentation and vignettes: analyzing PBMC scATAC-seq, motif analysis, and joint RNA-ATAC analysis. [https://stuartlab.org/signac/](https://stuartlab.org/signac/) (2026)

16. 10x Genomics. Cell Type Annotation Using Single Cell ATAC Data. Technical Note CG000234. [https://www.10xgenomics.com/support/](https://www.10xgenomics.com/support/) (2026)

---

*This tutorial is part of the comprehensive NGS101.com single-cell ATAC-seq analysis series for beginners.*
