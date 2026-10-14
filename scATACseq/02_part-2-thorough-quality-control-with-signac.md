# How to Analyze Single-Cell ATAC-seq Data &#8212; A Complete Beginner's Guide Part 2: Thorough Quality Control with Signac

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** August 5, 2026
- **Categories:** ATAC-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/)

---

*Separate real nuclei from empty droplets, dead cells, doublets, and artifacts using TSS enrichment, nucleosome signal, FRiP, blacklist ratio, and AMULET.*

---

In [Part 1](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/) you took raw FASTQ files from a real COVID-19 case/control cohort, ran `cellranger-atac count`, and ended up with a fragments file, a set of peaks, and a peak-by-barcode matrix. Cell Ranger printed a number in its web summary — “Estimated number of cells” — and it is tempting to treat that number as the truth and move straight to clustering.

Do not. That number is the output of a single heuristic applied to a single metric. Inside your “cells” there are almost certainly empty droplets that captured a little ambient DNA, nuclei that lysed before transposition, barcodes carrying two nuclei instead of one, and barcodes whose signal is dominated by repetitive artifact regions rather than real regulatory elements. In scATAC-seq these problems are both more common and harder to see than in scRNA-seq, because every measurement is sparse: a typical cell has only one or two fragments in any given peak, so you cannot lean on the familiar “percent mitochondrial reads” and “genes per cell” reflexes.

Part 2 fixes that. You will compute the scATAC-specific quality metrics that the field actually uses — TSS enrichment, nucleosome signal, fraction of reads in peaks, blacklist ratio, and total fragments per cell — inspect them visually before touching a threshold, remove doublets with AMULET, and filter cells and peaks in the correct order — finishing with a clean, quality-controlled object that Part 3 will merge and cluster.

Everything runs in R using **Signac**, the Seurat companion package built specifically for single-cell chromatin data. If you have worked through the [scRNA-seq series](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/), the shape of this workflow will feel familiar; the metrics and the reasoning behind them will not.

> 

**Prerequisites:** The `outs/` directory from a completed `cellranger-atac count` run (Part 1), the Pixi project you created in Part 1, and basic comfort with R. New to Seurat objects and subsetting? The [scRNA-seq quality control tutorial](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/) covers the object handling this tutorial assumes.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/)

- [Introduction: Why scATAC-seq Quality Control Needs Its Own Rulebook](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Introduction_Why_scATAC-seq_Quality_Control_Needs_Its_Own_Rulebook)

  - [What a “Cell” Actually Looks Like in scATAC-seq Data](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#What_a_%E2%80%9CCell%E2%80%9D_Actually_Looks_Like_in_scATAC-seq_Data)
  - [How scATAC-seq Quality Control Differs from scRNA-seq Quality Control](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#How_scATAC-seq_Quality_Control_Differs_from_scRNA-seq_Quality_Control)

- [The Six Core scATAC-seq Quality Control Metrics](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#The_Six_Core_scATAC-seq_Quality_Control_Metrics)

  - [1. Total Fragments per Cell: Sequencing Depth and Complexity](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#1_Total_Fragments_per_Cell_Sequencing_Depth_and_Complexity)
  - [2. TSS Enrichment Score: The Signal-to-Noise Workhorse](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#2_TSS_Enrichment_Score_The_Signal-to-Noise_Workhorse)
  - [3. Fragment-Size Distribution: The Nucleosome Ladder](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#3_Fragment-Size_Distribution_The_Nucleosome_Ladder)
  - [4. Nucleosome Signal: The Per-Cell Summary of That Ladder](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#4_Nucleosome_Signal_The_Per-Cell_Summary_of_That_Ladder)
  - [5. Fraction of Reads in Peaks (FRiP): How Much Signal Is On Target](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#5_Fraction_of_Reads_in_Peaks_FRiP_How_Much_Signal_Is_On_Target)
  - [6. Blacklist Ratio: Catching Artifact-Driven Barcodes](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#6_Blacklist_Ratio_Catching_Artifact-Driven_Barcodes)
  - [Putting the Metrics Together](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Putting_the_Metrics_Together)

- [The Four Levels of scATAC-seq Quality Control](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#The_Four_Levels_of_scATAC-seq_Quality_Control)
- [What Does a Good scATAC-seq Dataset Look Like?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#What_Does_a_Good_scATAC-seq_Dataset_Look_Like)
- [Setting Up the R Environment for Signac](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Setting_Up_the_R_Environment_for_Signac)

  - [Why Signac?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Why_Signac)
  - [Adding R and Signac to Your Part 1 Pixi Environment](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Adding_R_and_Signac_to_Your_Part_1_Pixi_Environment)
  - [Loading Libraries and Defining Paths](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Loading_Libraries_and_Defining_Paths)

- [Example Data: The Cell Ranger ATAC Outputs from Part 1](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Example_Data_The_Cell_Ranger_ATAC_Outputs_from_Part_1)

  - [The Three Files Signac Needs](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#The_Three_Files_Signac_Needs)
  - [The Columns of singlecell.csv You Will Actually Use](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#The_Columns_of_singlecellcsv_You_Will_Actually_Use)
  - [Level 1 QC: Read the Library Metrics Before You Touch a Single Cell](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Level_1_QC_Read_the_Library_Metrics_Before_You_Touch_a_Single_Cell)

- [Building the Chromatin Assay and the Seurat Object](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Building_the_Chromatin_Assay_and_the_Seurat_Object)

  - [Loading the Peak Matrix and Per-Barcode Metrics](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Loading_the_Peak_Matrix_and_Per-Barcode_Metrics)
  - [Removing Non-Standard Chromosomes Before Building the Object](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Removing_Non-Standard_Chromosomes_Before_Building_the_Object)
  - [Creating the ChromatinAssay](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Creating_the_ChromatinAssay)
  - [Adding Gene Annotations for TSS Calculations](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Adding_Gene_Annotations_for_TSS_Calculations)

- [Calculating Per-Cell Fragment and Signal Metrics](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Calculating_Per-Cell_Fragment_and_Signal_Metrics)

  - [TSS Enrichment Score](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#TSS_Enrichment_Score)
  - [Nucleosome Signal](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Nucleosome_Signal)
  - [A Note on Signac 1.17: ATACqc, fragtk, and the Deprecation Warnings](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#A_Note_on_Signac_117_ATACqc_fragtk_and_the_Deprecation_Warnings)
  - [FRiP (Fraction of Reads in Peaks)](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#FRiP_Fraction_of_Reads_in_Peaks)
  - [Blacklist Ratio](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Blacklist_Ratio)
  - [Reviewing All Metrics Before Plotting](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Reviewing_All_Metrics_Before_Plotting)

- [Diagnostic QC Plots: Look Before You Filter](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Diagnostic_QC_Plots_Look_Before_You_Filter)

  - [The Fragment-Size Periodicity Plot](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#The_Fragment-Size_Periodicity_Plot)
  - [The TSS Coverage Plot](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#The_TSS_Coverage_Plot)
  - [Violin Plots of Every QC Metric](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Violin_Plots_of_Every_QC_Metric)
  - [Density Scatter: Letting the Data Suggest Thresholds](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Density_Scatter_Letting_the_Data_Suggest_Thresholds)

- [Applying Cell-Level Filtering](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Applying_Cell-Level_Filtering)

  - [Choosing Thresholds That Match Your Data](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Choosing_Thresholds_That_Match_Your_Data)
  - [Applying the Filter](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Applying_the_Filter)
  - [Auditing What You Removed](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Auditing_What_You_Removed)
  - [Detecting Doublets with AMULET](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Detecting_Doublets_with_AMULET)
  - [Confirming the Filtering Worked](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Confirming_the_Filtering_Worked)

- [Feature-Level QC: Cleaning Up the Peak Set](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Feature-Level_QC_Cleaning_Up_the_Peak_Set)

  - [Why This Section Comes After Cell Filtering](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Why_This_Section_Comes_After_Cell_Filtering)
  - [Removing Peaks in ENCODE Blacklist Regions](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Removing_Peaks_in_ENCODE_Blacklist_Regions)
  - [Removing Rare Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Removing_Rare_Peaks)
  - [Saving the Object for Part 3](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Saving_the_Object_for_Part_3)

- [Scaling the Workflow to All Four Samples](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Scaling_the_Workflow_to_All_Four_Samples)
- [Best Practices for scATAC-seq Quality Control](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Best_Practices_for_scATAC-seq_Quality_Control)
- [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Common_Pitfalls_and_How_to_Avoid_Them)
- [Conclusion](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-2-thorough-quality-control-with-signac/#Like_this)

## Introduction: Why scATAC-seq Quality Control Needs Its Own Rulebook

### What a “Cell” Actually Looks Like in scATAC-seq Data

In scRNA-seq, a cell is a barcode with a few thousand mRNA molecules spread across a few thousand genes. Genes are transcribed repeatedly, so a moderately expressed gene contributes many reads and you get a genuinely quantitative measurement per gene per cell.

In scATAC-seq, a cell is a barcode with a few thousand **fragments** — pieces of DNA cut out by the Tn5 transposase wherever the chromatin was open. Here is the crucial difference: a diploid cell has only **two copies** of every genomic locus. A given peak can therefore yield at most a couple of fragments in a given cell, and usually yields zero. The peak-by-cell matrix is consequently far larger than a gene-by-cell matrix (hundreds of thousands of peaks instead of ~20,000 genes), far sparser (typically 95-99 percent zeros), and effectively near-binary: the informative question is usually “was this region accessible at all in this cell?” rather than “how accessible was it?”.

This single fact drives everything in this tutorial. It is why we cannot judge cells by how many “features” they express, why we need signal-to-noise metrics computed from the fragments file rather than from the count matrix, and why normalization uses TF-IDF (a technique borrowed from text search, where documents are also sparse bags of rare terms) instead of the log-normalization used for RNA.

### How scATAC-seq Quality Control Differs from scRNA-seq Quality Control

The table below maps each familiar scRNA-seq QC metric to its scATAC-seq counterpart. Read it as a translation guide, not a one-to-one substitution — some entries have no equivalent at all.

`nCount_RNA`**Total fragments per cell**`passed_filters``nFeature_RNA``nCount_peaks``percent.mt`**TSS enrichment score**`passed_filters`**Fraction of reads in peaks (FRiP)****AMULET****Nucleosome signal / fragment-size distribution****Blacklist ratio**

| scRNA-seq metric | scATAC-seq counterpart | Why the swap is necessary |
| --- | --- | --- |
| (UMIs per cell) | () | Same idea (sequencing depth per cell), different unit: unique Tn5 fragments rather than mRNA molecules |
| (genes per cell) | / fragments in peaks | Peaks-detected-per-cell is a weak signal because the matrix is near-binary and peak number depends on the peak set, not the cell |
| (mitochondrial fraction) |  | Mitochondrial reads exist in ATAC too, but Cell Ranger already excludes them from ; the real dead-cell signature in ATAC is a flat, unfocused signal at promoters |
| Ambient RNA correction (SoupX, DecontX) |  | Ambient DNA in ATAC shows up as fragments scattered across closed chromatin, so the fix is measuring how much signal is on-target rather than subtracting a soup profile |
| Doublet detection (DoubletFinder, scDblFinder) | , plus an upper bound on total fragments | Transcriptome-based callers rely on mixed expression profiles that do not exist in sparse binary accessibility data. AMULET instead exploits ploidy: a diploid cell cannot yield more than two fragments from one locus |
| No equivalent |  | Unique to ATAC: fragment lengths report directly on whether Tn5 cut between nucleosomes as intended |
| No equivalent |  | Unique to chromatin assays: some genomic regions accumulate artifactual signal in every experiment |

Two entries deserve emphasis because beginners get them backwards.

First, **`nFeature_peaks` is not the scATAC-seq version of `nFeature_RNA`.** In scRNA-seq, a cell with only 200 detected genes is almost certainly dying or empty. In scATAC-seq, the number of detected peaks is largely a restatement of sequencing depth, and it changes if you swap in a different peak set. Use total fragments and FRiP instead.

Second, **percent-mitochondrial does not transfer.** Cell Ranger ATAC already removes mitochondrial fragments when computing `passed_filters`, so a high mitochondrial fraction is a library-level observation, not a per-cell filter you apply here. The per-cell equivalent of “this cell was dying” in ATAC is a low TSS enrichment score.

---

## The Six Core scATAC-seq Quality Control Metrics

These six numbers are what the scATAC-seq field converged on. Each one catches a different failure mode, and the reason we compute all six is that a bad barcode can look perfectly acceptable on any one of them alone.

### 1. Total Fragments per Cell: Sequencing Depth and Complexity

The count of unique, high-quality, non-duplicate fragments assigned to a barcode. Cell Ranger reports this as `passed_filters` in `singlecell.csv`.

This metric fails in both directions, which is why it needs a lower **and** an upper bound:

- **Too few fragments** (a few hundred or less) means the barcode is an empty droplet that caught ambient DNA, or a nucleus that was destroyed before transposition. With so few fragments there is not enough information to place the cell anywhere meaningful in the accessibility landscape.

- **Too many fragments** (an order of magnitude above the median) usually means two or more nuclei ended up in one droplet, or a clump of nuclear debris was captured. These barcodes carry a blend of two accessibility profiles and will sit in artificial positions between real clusters.

### 2. TSS Enrichment Score: The Signal-to-Noise Workhorse

Transcription start sites are the most reliably accessible regions in any nucleus, because active promoters must be open for transcription to happen. A working ATAC-seq experiment therefore piles up fragments in a sharp spike centered on TSSs, with much lower coverage in the flanking regions a couple of kilobases away.

The **TSS enrichment score** quantifies that spike: it is essentially the ratio of fragment coverage at the TSS to coverage in the flanking background, computed separately for every cell. The [ENCODE project](https://www.encodeproject.org/data-standards/terms/) formalized this metric for bulk ATAC-seq, and Signac’s `TSSEnrichment()` computes a per-cell version.

Why it is the single most informative ATAC metric: a barcode can have plenty of fragments and still be worthless if those fragments are scattered randomly across the genome instead of concentrated at regulatory elements. That is exactly what you see from ambient DNA in an empty droplet, or from a cell whose nuclear envelope ruptured and whose chromatin structure collapsed. A low TSS enrichment score is the ATAC signature of “this barcode has depth but no biology.”

> 

**Important caveat:** the per-cell score Signac reports is not numerically identical to ENCODE’s bulk score, and both depend on which TSS annotation you use. Treat published cutoffs as starting points to compare against your own distribution, not as absolute physical constants.

### 3. Fragment-Size Distribution: The Nucleosome Ladder

Tn5 transposase can only insert where DNA is not wrapped around a histone. In a successful experiment it therefore cuts on both sides of individual nucleosomes, and the resulting fragment lengths fall into a characteristic ladder:

- **Under ~147 bp** — nucleosome-free fragments, from stretches of genuinely open, unwrapped DNA. This is where most of your regulatory signal lives.

- **Around 200 bp** — mononucleosomal fragments, spanning one nucleosome (147 bp of wrapped DNA plus linker).

- **Around 400 bp** — dinucleosomal fragments, spanning two nucleosomes.

Plot fragment length against fragment count for a healthy library and you see decaying periodic humps at roughly 200 bp intervals. This is the **nucleosome banding pattern**, and it is the clearest single piece of evidence that the assay worked at the biochemical level. If the pattern is absent — a smooth, featureless distribution — something went wrong with the transposition, the nuclei were over-digested, or the chromatin was already degraded.

### 4. Nucleosome Signal: The Per-Cell Summary of That Ladder

The fragment-size distribution is a picture, not a number, so it cannot be used directly as a per-cell filter. Signac condenses it into the **nucleosome signal**: the ratio of mononucleosomal fragments (roughly 147-294 bp) to nucleosome-free fragments (under 147 bp), computed for every cell individually. Low is good. A cell with a high nucleosome signal has proportionally too much wrapped DNA and too little open chromatin, which points to poor transposition in that particular droplet.

The distinction is worth keeping straight: the **fragment-size distribution** diagnoses the library, and the **nucleosome signal** filters individual cells. You look at the first and threshold on the second.

### 5. Fraction of Reads in Peaks (FRiP): How Much Signal Is On Target

Peaks are the regions your dataset identified as accessible. FRiP asks, for each cell, what fraction of its fragments actually landed in one of those regions.

A high FRiP means the cell’s fragments are concentrated where real regulatory activity is. A low FRiP means they are diffusely scattered through closed chromatin — the signature of ambient DNA contamination or a broken nucleus. FRiP is conceptually the closest thing scATAC-seq has to ambient-contamination estimation in scRNA-seq, and it is cheap to compute because Cell Ranger already provides both numerators and denominators.

One property to keep in mind: **FRiP is only as meaningful as your peak set.** If your peaks came from an aggregate caller that missed regulatory elements specific to a rare population, cells from that population will show artificially low FRiP and you may filter out exactly the biology you were hoping to find. This is one more reason Part 1 recalled peaks with MACS3 alongside Cell Ranger’s aggregate caller.

### 6. Blacklist Ratio: Catching Artifact-Driven Barcodes

Certain genomic regions — high-copy repeats, satellite DNA, unresolved assembly problems — accumulate huge pileups of reads in essentially every sequencing assay, regardless of the biology. ENCODE curated these into a published **blacklist** of regions that should be excluded from analysis.

The blacklist ratio is the fraction of a cell’s fragments that fall in those regions. A barcode with an unusually high proportion is being driven by artifact rather than signal, and it will cluster with other artifact-driven barcodes, producing a “cell type” that is really a technical bin.

Note that the blacklist matters at two levels: you filter **cells** with high blacklist ratios, and you also remove **peaks** that overlap blacklist regions from the feature set. Both steps appear later in this tutorial, and the order in which you do them matters — more on that in Step 18.

### Putting the Metrics Together

The five per-cell scalar metrics are summarized below (the fragment-size distribution is a shape rather than a number, so it appears as a plot in Step 11 instead). The reason to compute all of them rather than picking a favorite is that each failure mode evades some of the metrics:

**low****very high****high****high**

| What went wrong with the barcode | Fragments | TSS enrichment | Nucleosome signal | FRiP | Blacklist ratio |
| --- | --- | --- | --- | --- | --- |
| Empty droplet, ambient DNA only | very low | low | uninformative (too few fragments) | low | normal |
| Nucleus lysed before transposition | can be normal |  | often high | low | normal |
| Two nuclei in one droplet |  | normal | normal | normal | normal |
| Poor transposition in that droplet | can be normal | low-ish |  | low-ish | normal |
| Artifact-driven barcode | can be high | low | normal | can look normal |  |

Read the bold entries: a doublet is invisible to every metric except the fragment count, and an artifact barcode is invisible to every metric except the blacklist ratio. Filtering on TSS enrichment alone — a common shortcut — lets both straight through. And a doublet of two cells of the *same* type escapes even the fragment count, since its depth is merely a little above average; catching that case needs a sixth approach based on ploidy rather than signal quality, which is what AMULET provides later in this tutorial.

---

## The Four Levels of scATAC-seq Quality Control

Beginners often think of QC as one step: filter the cells. In practice there are four distinct levels, each with its own failure modes and its own remedy. Working through them in order saves a great deal of wasted compute.

**1. Run / library**`web_summary.html``summary.csv`**before****2. Cell / barcode****3. Feature / peak****4. Cohort / batch****across**

| Level | What you inspect | What can go wrong | What you do about it |
| --- | --- | --- | --- |
|  | ,  from Cell Ranger | Low valid-barcode fraction, poor mapping, no nucleosome banding, low overall TSS enrichment, far fewer or more cells than targeted, low sequencing saturation | If the library itself failed, no amount of cell filtering rescues it. Diagnose here  spending hours in R. Consider resequencing or dropping the sample from the cohort. |
|  | Per-cell metrics: fragments, TSS enrichment, nucleosome signal, FRiP, blacklist ratio; then per-locus fragment coverage | Empty droplets, dead or lysed nuclei, doublets and multiplets, artifact-driven barcodes | Compute all six metrics, plot them, apply explicit thresholds, then run AMULET for doublets. This is Steps 6-17c. |
|  | The peak set itself | Peaks on unplaced scaffolds and random contigs, peaks overlapping blacklist regions, peaks accessible in only a handful of cells (indistinguishable from noise) | Restrict to standard chromosomes, subtract blacklist regions, drop peaks below a minimum-cell threshold. This is Steps 18-19. |
|  | The same metrics compared  samples | One sample with systematically lower depth or TSS enrichment will dominate clustering and masquerade as a biological effect | Compare metric distributions sample by sample before merging. Flag or drop outlier samples. See Scaling the Workflow to All Four Samples, below. |

The single most common beginner mistake is skipping Level 1 and diving into Level 2. If Cell Ranger reports 4 percent of read pairs with a valid barcode, filtering cells in Signac is not going to help.

---

## What Does a Good scATAC-seq Dataset Look Like?

Published rubrics vary, and the right numbers genuinely depend on your tissue, whether you used fresh or frozen material, and how deeply you sequenced. The table below collects widely used reference ranges so you have something concrete to compare against. Treat the middle column as “acceptable, proceed with care” and the right column as “this library worked well.”

| Metric (level) | Concerning | Acceptable | Good |
| --- | --- | --- | --- |
| Fraction of read pairs with a valid barcode (library) | < 90% | 90-95% | > 95% |
| Confidently mapped read pairs (library) | < 50% | 50-75% | > 75% |
| Median high-quality fragments per cell (library) | < 1,000 | 1,000-3,000 | > 3,000 |
| Fraction of fragments in peaks, in cells (library) | < 5% | 5-20% | > 20% |
| Nucleosome-free and mononucleosome peaks visible (library) | absent | present but weak | clearly periodic |
| TSS enrichment score (per cell, median) | < 2 | 2-4 | > 4 |
| FRiP (per cell) | < 15% | 15-30% | > 30% |
| Nucleosome signal (per cell) | > 4 | 2-4 | < 2 |
| Blacklist ratio (per cell) | > 5% | 1-5% | < 1% |
| Estimated cells vs. targeted recovery | off by > 2x | off by up to 2x | within ~25% |

Three notes on reading this table honestly:

**Frozen and clinical samples score lower, and that is not necessarily a failure.** PBMCs that were cryopreserved, shipped, and thawed — which describes most patient cohorts, including the COVID-19 dataset used in this series — routinely give lower TSS enrichment and FRiP than a 10x demonstration library made from fresh cells at the bench. Applying a demonstration-grade threshold to clinical data can delete most of your dataset. Compare against your own distributions, and be consistent across samples in the same cohort.

**Consistency across samples matters more than absolute values.** A cohort where every sample has TSS enrichment around 3.5 is far easier to analyze correctly than a cohort where three samples are at 7 and one is at 2.5. The latter will produce clusters that track sample rather than biology.

**Record your numbers.** Before you filter anything, write down the library-level metrics and the medians of the per-cell metrics for every sample. You will need them for the methods section of your paper, and you will need them to justify your thresholds to a reviewer.

---

## Setting Up the R Environment for Signac

### Why Signac?

Two mature R toolkits dominate downstream scATAC-seq analysis: **Signac** and **ArchR**. Signac is built directly on the Seurat object model, so every function you already know from the scRNA-seq series — `subset()`, `VlnPlot()`, `FindNeighbors()`, `FindClusters()` — works unchanged, and cross-modal integration with matched scRNA-seq later in this series becomes almost trivial. ArchR is faster and more memory-efficient on very large atlases (hundreds of thousands of cells) but uses its own on-disk `ArrowFiles` format and its own function vocabulary.

For a beginner working with a handful of samples, Signac is the better first tool: fewer new concepts, and everything transfers from the transcriptomic side of this series. We will revisit ArchR later in the series when dataset size makes it worth the extra learning curve.

### Adding R and Signac to Your Part 1 Pixi Environment

We keep using the same Pixi project created in Part 1, which already contains `sra-tools`, `pigz`, `macs3`, `samtools`, `bedtools`, and `htslib`. Pixi records every version in `pixi.toml` and `pixi.lock`, so the environment stays reproducible as it grows. If Pixi is new to you, our [Pixi setup guide](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/) walks through installation.

```bash
# Move into the Pixi project created in Part 1
cd /projects/mylab/shared/scatac-analysis

# Add only the R packages this tutorial actually uses:
#   r-base                        -- the R interpreter itself
#   r-signac                      -- scATAC-seq toolkit (ChromatinAssay, QC metrics, TF-IDF, SVD)
#   r-seurat                      -- object model, subsetting, VlnPlot
#   r-hdf5r                       -- required by Read10X_h5() to read the .h5 peak matrix
#   r-ggplot2 / r-patchwork       -- plotting and multi-panel figure assembly
#   bioconductor-genomicranges    -- GRanges objects for peaks and blacklist regions
#   bioconductor-genomeinfodb     -- standardChromosomes(), seqlevels() helpers
#   bioconductor-annotationhub    -- downloads the EnsDb gene annotation
#   bioconductor-ensembldb        -- required to use the EnsDb annotation object
#   bioconductor-biovizbase       -- required by Signac's GetGRangesFromEnsDb()
#   bioconductor-rtracklayer      -- imports the ENCODE blacklist BED (and, optionally, the reference GTF)
#   bioconductor-rsamtools        -- reads contig names from the fragments file tabix index
#   bioconductor-scdblfinder      -- AMULET doublet detection from the fragments file
pixi add r-base r-signac r-seurat r-hdf5r r-ggplot2 r-patchwork \
         bioconductor-genomicranges bioconductor-genomeinfodb \
         bioconductor-annotationhub bioconductor-ensembldb \
         bioconductor-biovizbase bioconductor-rtracklayer \
         bioconductor-rsamtools bioconductor-scdblfinder
```

This solve pulls in a large dependency tree and takes several minutes the first time. Once it finishes, confirm the two key packages load:

```r
# Drop into a shell where the environment is active
pixi shell

# Confirm Signac, Seurat, and the annotation helpers are installed and loadable
Rscript -e 'library(Signac); library(Seurat); library(biovizBase); library(scDblFinder); packageVersion("Signac")'
```

> 

**HPC note:** run this on a login node or an interactive session with a reasonable memory allocation. The QC steps below need roughly 32-64 GB of RAM for a PBMC sample, because computing the TSS coverage profile reads the entire fragments file. If your cluster is new to you, see our [SLURM beginner’s guide](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/).

### Loading Libraries and Defining Paths

Start an R session inside the Pixi shell and set up the workspace. Adjust `cellranger_dir` to wherever your Part 1 run wrote its output, and `project_dir` to wherever you want this tutorial’s figures and objects to land.

```r
#-----------------------------------------------
# STEP 1: Load libraries and define paths
#-----------------------------------------------

library(Signac)
library(Seurat)
library(GenomicRanges)
library(GenomeInfoDb)
library(AnnotationHub)
library(ggplot2)
library(patchwork)
library(Rsamtools)
library(scDblFinder)

# Set the random seed
set.seed(1234)

# --- Paths: edit these three lines to match your own setup ---
cellranger_dir <- "/projects/mylab/shared/scatac_tutorial/results"

# Where THIS tutorial writes its figures and objects.
project_dir <- "/projects/mylab/shared/scatac_tutorial/downstream_qc"

sample_id <- "severe1"

# The Cell Ranger ATAC "outs" directory for this sample
cr_dir <- file.path(cellranger_dir, sample_id, "outs")

# Output folders for figures and saved objects
plot_dir <- file.path(project_dir, sample_id, "plots")
obj_dir  <- file.path(project_dir, sample_id, "objects")
dir.create(plot_dir, recursive = TRUE, showWarnings = FALSE)
dir.create(obj_dir,  recursive = TRUE, showWarnings = FALSE)

# Send AnnotationHub's cache to project space rather than the home directory,
# which is usually small and quota-limited on shared clusters.
ah_cache <- file.path(project_dir, "annotationhub_cache")
dir.create(ah_cache, recursive = TRUE, showWarnings = FALSE)
setAnnotationHubOption("CACHE", ah_cache)
```

---

## Example Data: The Cell Ranger ATAC Outputs from Part 1

### The Three Files Signac Needs

Signac reads three of the twenty-one files that `cellranger-atac count` produced. Everything in this tutorial comes from these three.

`filtered_peak_bc_matrix.h5``ChromatinAssay``singlecell.csv``fragments.tsv.gz``.tbi`

| File | What it contains | What Signac does with it |
| --- | --- | --- |
|  | Peak-by-barcode counts for called cells only, in HDF5 format | Becomes the count matrix of the |
|  | Per-barcode QC metrics computed by Cell Ranger (total fragments, fragments in peaks, fragments in blacklist regions, and more) | Becomes the cell metadata; supplies the numbers behind FRiP |
| (+  index) | Every unique fragment: chromosome, start, end, cell barcode, read support | Stays on disk; read on demand to compute TSS enrichment, nucleosome signal, and fragment-size histograms |

The fragments file is the reason scATAC-seq QC is more powerful than a count matrix alone would allow. Fragment **lengths** and precise fragment **positions** are simply not recoverable from a peak-by-cell matrix, and those are exactly what the nucleosome signal and TSS enrichment need. Signac never loads the whole file into memory; it uses the `.tbi` index to jump to the regions it needs.

```bash
# Confirm all three inputs exist before starting R (run in the Pixi shell)
cd /projects/mylab/shared/scatac_tutorial/results

ls -lh severe1/outs/filtered_peak_bc_matrix.h5 \
       severe1/outs/singlecell.csv \
       severe1/outs/fragments.tsv.gz \
       severe1/outs/fragments.tsv.gz.tbi
```

If the `.tbi` index is missing — which happens if the fragments file was moved, copied without its index, or regenerated — rebuild it before continuing:

```bash
# Rebuild the tabix index for the fragments file
tabix -p bed severe1/outs/fragments.tsv.gz
```

### The Columns of singlecell.csv You Will Actually Use

`singlecell.csv` has one row per barcode and roughly fifteen columns. Four of them do real work in this tutorial:

`passed_filters`**total fragments per cell**`peak_region_fragments``blacklist_region_fragments``is__cell_barcode`

| Column | Meaning |
| --- | --- |
|  | Unique, high-quality, non-mitochondrial fragments for this barcode. This is the . |
|  | Of those, how many fall inside a called peak. Numerator for FRiP. |
|  | How many fall inside ENCODE blacklist regions. |
|  | Cell Ranger’s own 1/0 cell call. Useful for context; we do not rely on it. |

Note the two underscores in `is__cell_barcode` — that is genuinely the column name, and mistyping it is a common source of confusing `NULL` results.

### Level 1 QC: Read the Library Metrics Before You Touch a Single Cell

This is the step most tutorials skip. Open `web_summary.html` in a browser or `summary.csv` in Microsoft Excel and check the library against the reference table above, then pull the same numbers programmatically so they end up in your notes rather than your short-term memory.

Work through this checklist before proceeding:

1. **Fraction of read pairs with a valid barcode** — below 90 percent suggests a barcode-whitelist or read-assignment problem. If this is very low, revisit the R1/R2/R3 file assignment covered in Part 1.

2. **Confidently mapped read pairs** — below 50 percent points to the wrong reference or contaminated material.

3. **Estimated number of cells** — compare against what you loaded. Far too few suggests poor nucleus recovery; far too many suggests the cell caller is picking up ambient background.

4. **Median high-quality fragments per cell** — your library complexity. Under 1,000 and the downstream analysis will be badly underpowered.

5. **Fraction of transposition events in peaks in cells** — the library-level version of FRiP. Under 5 percent is a failed library.

6. **The insert-size distribution plot** — look for the periodic nucleosome ladder described earlier. A smooth curve with no humps means the transposition did not work as intended.

7. **The TSS enrichment plot** — look for a sharp peak centered at zero, not a gentle rise.

If a sample fails items 1, 2, 5, or 6, stop and deal with the library. Nothing in Steps 2-23 can rescue it.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/07/scatac-seq-web-summary.png?resize=1024%2C674&ssl=1)

---

## Building the Chromatin Assay and the Seurat Object

### Loading the Peak Matrix and Per-Barcode Metrics

A `ChromatinAssay` is Seurat’s assay class extended for genomic data. On top of a normal count matrix it stores the genomic coordinates of every feature, a pointer to the on-disk fragments file, gene annotations, and genome information. That extra structure is what lets Signac compute coordinate-aware metrics.

```r
#-----------------------------------------------
# STEP 2: Load the Cell Ranger matrix and metadata
#-----------------------------------------------

# Peak-by-barcode counts for called cells. Rownames arrive as "chr1:9772-10660",
# which is why sep = c(":", "-") is used below to parse them into ranges.
counts <- Read10X_h5(file.path(cr_dir, "filtered_peak_bc_matrix.h5"))

# Per-barcode QC metrics. row.names = 1 makes the barcode the rowname so that
# Seurat can match metadata rows to matrix columns automatically.
metadata <- read.csv(
  file      = file.path(cr_dir, "singlecell.csv"),
  header    = TRUE,
  row.names = 1
)
```

`singlecell.csv` contains every barcode Cell Ranger saw — often hundreds of thousands — while the filtered matrix contains only the called cells. Seurat matches them by rowname and silently discards the rest, so no manual subsetting is needed.

### Removing Non-Standard Chromosomes Before Building the Object

Look at the rownames of `counts` and you will find entries like `KI270713.1:13054-13909` or `GL000009.2:56000-56900` mixed in among the `chr1` through `chrY` peaks. These are unplaced scaffolds and unlocalized contigs: pieces of the human genome that could not be confidently positioned on a chromosome. Peaks called there are usually mapping artifacts, and they add noise to every downstream step.

We drop them **from the count matrix, before creating the assay**. Doing it at this point rather than after means the `ChromatinAssay` is built with a clean set of sequence names from the outset, which avoids a confusing verification problem explained immediately below.

```r
#-----------------------------------------------
# STEP 3: Restrict the peak set to standard chromosomes
#-----------------------------------------------

# Convert the matrix rownames ("chr1:9772-10660") into genomic ranges.
# The GRanges() constructor parses "seqname:start-end" strings directly. We use
# it rather than Signac's StringToGRanges(), which is deprecated in Signac 1.17.
peak_ranges <- GRanges(rownames(counts))

# TRUE for peaks on chr1..chr22, chrX, chrY; FALSE for scaffolds and contigs.
# as.vector() strips the Rle wrapper that seqnames() returns.
peaks_standard <- as.vector(
  seqnames(peak_ranges) %in% standardChromosomes(peak_ranges)
)

# How many peaks are being dropped, and how many remain
table(peaks_standard)

counts <- counts[peaks_standard, ]
```

**Expected output:**

```
peaks_standard
 FALSE   TRUE
    90 110819
```

> 

**Reading this:** only 90 of 110,909 peaks sat on scaffolds — less than a tenth of a percent. That small number is itself reassuring: it says the peak caller was not distracted by unplaceable sequence. Do not skip the step because the number is small, though. Those 90 peaks would otherwise appear in the FRiP and blacklist-ratio denominators, and they carry the scaffold names that make every later `granges()` printout confusing.

### Creating the ChromatinAssay

```r
#-----------------------------------------------
# STEP 4: Create the ChromatinAssay and Seurat object
#-----------------------------------------------

chrom_assay <- CreateChromatinAssay(
  counts       = counts,
  sep          = c(":", "-"),                            # parse "chr1:9772-10660"
  fragments    = file.path(cr_dir, "fragments.tsv.gz"),  # kept on disk, not loaded
  min.cells    = 10,                                     # drop peaks seen in < 10 cells
  min.features = 200                                     # drop barcodes with < 200 peaks
)

atac <- CreateSeuratObject(
  counts    = chrom_assay,
  assay     = "peaks",     # names the assay; drives metric names like nCount_peaks
  meta.data = metadata
)

atac

# Confirm the sequence names are clean: chr1..chr22, chrX, chrY only
seqlevels(granges(atac))
```

**Expected output:**

```r
An object of class Seurat
110667 features across 4614 samples within 1 assay
Active assay: peaks (110667 features, 0 variable features)
 2 layers present: counts, data

 [1] "chr1"  "chr2"  "chr3"  "chr4"  "chr5"  "chr6"  "chr7"  "chr8"  "chr9"
[10] "chr10" "chr11" "chr12" "chr13" "chr14" "chr15" "chr16" "chr17" "chr18"
[19] "chr19" "chr20" "chr21" "chr22" "chrX"  "chrY"
```

> 

**Reading this:** 110,667 peaks across 4,614 cells. Note that `min.cells = 10` removed a further 152 peaks beyond the 90 scaffold peaks (110,819 to 110,667), and that no barcodes were lost to `min.features = 200` — Cell Ranger’s cell calling had already excluded anything that sparse. The `seqlevels()` output is exactly 24 standard chromosomes with no scaffolds, which is the payoff for filtering the matrix before building the assay.

Two arguments are doing quiet but consequential work:

- **`min.cells = 10`** is your first feature-level filter. A peak detected in fewer than ten cells across the whole sample cannot be distinguished from noise, and keeping hundreds of thousands of such peaks inflates memory use and adds nothing to the dimensionality reduction. Step 19 revisits this threshold after cell filtering, because removing bad cells pushes more peaks below the line.

- **`min.features = 200`** removes barcodes with almost no detected peaks. This is a floor, not a real QC filter — it exists to keep obviously empty barcodes out of the metric calculations. The substantive cell filtering happens in Step 15.

Because we set `assay = "peaks"`, Seurat names the automatically computed depth metrics `nCount_peaks` (total counts in peaks per cell) and `nFeature_peaks` (number of peaks detected per cell). Had we named the assay `ATAC`, they would be `nCount_ATAC` and `nFeature_ATAC` — worth knowing, because copy-pasting code between tutorials that use different assay names is a frequent source of “object has no such column” errors.

Print the object and **write down the two numbers it reports**: the number of features (peaks) and the number of samples (cells). You will compare against them after every filtering step.

```bash
# Inspect the assay: note "Fragment files: 1" and "Annotation present: FALSE"
atac[["peaks"]]

# ChromatinAssay data with 110667 features for 4614 cells
# Variable features: 0
# Genome:
# Annotation present: FALSE
# Motifs present: FALSE
# Fragment files: 1

# Look at the genomic ranges attached to each feature
granges(atac)

# GRanges object with 110667 ranges and 0 metadata columns:
#            seqnames            ranges strand
#               <Rle>         <IRanges>  <Rle>
#        [1]     chr1        9772-10662      *
#        [2]     chr1     180602-181521      *
#        [3]     chr1     191195-192101      *
#        [4]     chr1     267596-268582      *
#        [5]     chr1     585756-586646      *
```

> 

**Why remove scaffolds now rather than later, alongside the other feature filtering?** Because two of the per-cell metrics we are about to compute — FRiP and the blacklist ratio — are calculated from the peak matrix. If scaffold peaks are still present, they contribute to those denominators and shift every cell’s score by a small, arbitrary amount. Cleaning the chromosome set first makes the cell-level metrics well defined. The remaining feature-level work (blacklist peaks and rare peaks) genuinely does belong after cell filtering, and Step 18 explains why.

### Adding Gene Annotations for TSS Calculations

`TSSEnrichment()` needs to know where transcription start sites are, which means the object needs a gene annotation. The critical requirement is that the annotation **match the reference you aligned against in Part 1**. Using a mismatched annotation shifts TSS positions and quietly deflates every cell’s score.

Part 1 used the 10x `GRCh38-2024-A` reference, which is built on **GENCODE v44** annotations — equivalent to **Ensembl release 110**. If you used a different reference, check `web_summary.html` for the reference name and match accordingly (for example, `GRCh38-2020-A` corresponds to Ensembl 98).

```r
#-----------------------------------------------
# STEP 5: Attach gene annotations matching the alignment reference
#-----------------------------------------------

ah <- AnnotationHub()

# Find the EnsDb record for the Ensembl release matching GRCh38-2024-A (GENCODE v44).
# Inspect ens_query before proceeding -- it should contain exactly one record.
ens_query <- query(ah, c("EnsDb", "Homo sapiens", "110"))
ens_query

# Expected: exactly one record.
#   AnnotationHub with 1 record
#   # names(): AH113665
#   # $title: Ensembl 110 EnsDb for Homo sapiens
#   # $genome: GRCh38

# Retrieve the record programmatically instead of hardcoding an AH identifier,
# because those identifiers change between AnnotationHub snapshots
ensdb <- ah[[names(ens_query)[1]]]

# Convert to a GRanges of gene/transcript/exon features that Signac understands.
# This call needs biovizBase installed, even though we never call it ourselves.
annotations <- GetGRangesFromEnsDb(ensdb = ensdb)

# Ensembl names chromosomes "1", "2", ...; the 10x reference uses "chr1", "chr2", ...
# The two must match or no TSS will overlap any fragment.
seqlevels(annotations) <- paste0("chr", seqlevels(annotations))
genome(annotations) <- "hg38"

# Attach to the object; TSSEnrichment() and TSSPlot() now know where TSSs are
Annotation(atac) <- annotations
```

That `seqlevels()` line is the single most common failure point in this whole tutorial. If you skip it, `TSSEnrichment()` runs without error and returns a TSS enrichment score of zero for every cell, because no Ensembl-style chromosome name ever matches a UCSC-style one. A column of zeros is your cue to come back here.

> 

**Offline alternative for compute nodes without internet.** `AnnotationHub()` needs network access, which many HPC compute nodes lack, and `GetGRangesFromEnsDb()` additionally needs biovizBase installed. This route avoids both requirements. The most faithful annotation is the GTF inside the reference package you actually aligned against, and you can import it directly. Signac requires the metadata columns `tx_id`, `gene_name`, `gene_id`, `gene_biotype`, and `type`, so the GENCODE column names need renaming:

```r
library(rtracklayer)

gtf_path <- file.path("/projects/mylab/shared/reference",
                      "refdata-cellranger-arc-GRCh38-2024-A", "genes", "genes.gtf.gz")
annotations <- rtracklayer::import(gtf_path)

# Map GENCODE GTF attribute names onto the names Signac expects
annotations$tx_id        <- annotations$transcript_id
annotations$gene_biotype <- annotations$gene_type
genome(annotations) <- "hg38"

Annotation(atac) <- annotations
```

This route needs no internet and guarantees the annotation matches the alignment exactly. The chromosome names already use the `chr` prefix, so no `seqlevels()` conversion is needed.

---

## Calculating Per-Cell Fragment and Signal Metrics

With annotations attached and the peak set cleaned, we can compute the four metrics Cell Ranger does not give us directly. Two of them (`TSSEnrichment()` and `NucleosomeSignal()`) read the fragments file from disk and are the slow steps in this tutorial.

### TSS Enrichment Score

```r
#-----------------------------------------------
# STEP 6: TSS enrichment score per cell
#-----------------------------------------------

# fast = FALSE also stores the full per-base coverage profile around TSSs,
# which TSSPlot() needs in Step 12. It is slower and uses more memory than the
# default fast = TRUE, which returns only the score. Use fast = TRUE if you do
# not need the diagnostic coverage plot.
atac <- TSSEnrichment(object = atac, fast = FALSE)

# The score lands in metadata as TSS.enrichment. Check it is not all zeros --
# a column of zeros means the chromosome-name conversion in Step 5 was skipped.
summary(atac$TSS.enrichment)

#    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
#  0.9535  5.2189  5.7426  5.5494  6.2115 13.8861

# Signac 1.17 prints a deprecation warning here. The function still works
# correctly -- see the note after Step 7 for why we keep using it.
```

This is the step to run first, because it is the one that catches an annotation mismatch. Look at the `summary()` output: you want a distribution with a real spread, typically with a median somewhere between 2 and 8 depending on sample quality. All zeros, or all values crowded below 0.5, means the annotation and the alignment reference do not agree — go back to Step 5.

### Nucleosome Signal

```r
#-----------------------------------------------
# STEP 7: Nucleosome signal per cell
#-----------------------------------------------

# Ratio of mononucleosomal fragments (147-294 bp) to nucleosome-free fragments
# (< 147 bp), computed from fragment lengths in the fragments file.
atac <- NucleosomeSignal(object = atac)

summary(atac$nucleosome_signal)

#    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
#  0.1461  0.4706  0.5164  0.5670  0.5782  4.0710
```

Signac writes two columns: `nucleosome_signal` (the ratio) and `nucleosome_percentile` (each cell’s rank within the sample). We filter on the ratio because it is comparable across samples; the percentile is convenient for exploration within one sample.

Some cells will get `NaN` here — these are barcodes with zero nucleosome-free fragments, so the ratio has a zero denominator. They are low-quality barcodes by definition and the fragment-count filter in Step 15 removes them, but be aware that `NaN` values silently drop rows in `subset()`.

### A Note on Signac 1.17: ATACqc, fragtk, and the Deprecation Warnings

If you are on Signac 1.17 or newer, both of the calls above print a deprecation warning:

```
Warning message:
In NucleosomeSignal(object = atac) : 'NucleosomeSignal' is deprecated.
Use 'ATACqc' instead.
```

This is worth understanding rather than ignoring, because Signac 1.17 deprecated a whole cluster of functions at once: `NucleosomeSignal()`, `TSSEnrichment()`, `TSSPlot()`, `StringToGRanges()`, `GRangesToString()`, and the built-in blacklist data objects.

**What `ATACqc()` is.** It is a single wrapper that replaces both `NucleosomeSignal()` and `TSSEnrichment()`, computing nucleosome signal and TSS enrichment in one pass. It is faster, because it does not do the work in R at all — it shells out to **fragtk**, a fragment-file toolkit written in Rust by the same lab.

**Why this tutorial does not use it.** Two concrete reasons, both practical rather than ideological:

1. **fragtk is not a Conda package.** Signac and Sinto are both on Bioconda; fragtk is distributed through crates.io and as prebuilt binaries on GitHub. Installing it means either a Rust toolchain (`cargo install fragtk`) or manually placing a downloaded binary on your `PATH`. Neither fits the single reproducible `pixi add` that anchors this series, and on a locked-down cluster the Rust route can be a genuine obstacle.

2. **The TSS coverage plot goes away.** `ATACqc()` returns summary metrics, not the per-base coverage matrix around TSSs. `TSSPlot()` — the diagnostic in Step 12 that shows the sharp promoter spike against a flat background — depends on that matrix, which is why `TSSEnrichment(fast = FALSE)` exists. Deprecated or not, it is currently the only route to that figure, and for a tutorial about *learning to see* data quality, losing that plot costs more than the speed gain is worth.

**Deprecated is not removed.** Both functions still run and still return correct values; the warning is a notice about future direction, not a defect. Nothing in this tutorial is broken by it.

The broader lesson generalizes past this one release: **pin your Signac version and record it**. Deprecations in an actively developed package are exactly why `sessionInfo()` belongs alongside every saved object, and why `pixi.lock` is worth committing.

### FRiP (Fraction of Reads in Peaks)

Because we have Cell Ranger output, FRiP is a simple division of two columns that already exist — no additional pass over the fragments file needed.

```r
#-----------------------------------------------
# STEP 8: FRiP and blacklist ratio from Cell Ranger metadata
#-----------------------------------------------

# Fraction of each cell's high-quality fragments that fall inside a called peak,
# expressed as a percentage. peak_region_fragments and passed_filters both come
# from singlecell.csv.
atac$pct_reads_in_peaks <- atac$peak_region_fragments / atac$passed_filters * 100

summary(atac$pct_reads_in_peaks)

#   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
#   4.81   55.06   62.50   56.25   66.33   75.21
```

### Blacklist Ratio

The blacklist is a fixed, published set of genomic regions, so the only real question is how to get it onto disk. We fetch it **directly from ENCODE by accession**, which is the most reproducible option: the accession never changes, the URL is stable, and the file ends up versioned alongside your analysis rather than in a package cache you cannot inspect.

The set we want is **ENCFF356LFX**, the unified ENCODE GRCh38 exclusion list (about 910 regions), curated by Kundaje and Shcherbina. It is what the ENCODE and excluderanges authors both recommend for hg38.

```bash
# Download the ENCODE unified hg38 blacklist once, into project space.
# Run this in the Pixi shell; reuse the same file for every sample.
mkdir -p /projects/mylab/shared/reference/blacklist
cd /projects/mylab/shared/reference/blacklist

curl -L -O https://www.encodeproject.org/files/ENCFF356LFX/@@download/ENCFF356LFX.bed.gz
gunzip -k ENCFF356LFX.bed.gz

# Sanity check: roughly 900 regions, three columns
wc -l ENCFF356LFX.bed
head -3 ENCFF356LFX.bed
```

```r
#-----------------------------------------------
# STEP 9: Blacklist ratio per cell
#-----------------------------------------------

# Import the ENCODE blacklist BED as a GRanges. rtracklayer converts BED's
# 0-based, half-open coordinates to GRanges' 1-based, closed convention
# automatically -- do not adjust the coordinates yourself.
blacklist_hg38 <- rtracklayer::import(
  "/projects/mylab/shared/reference/blacklist/ENCFF356LFX.bed",
  format = "bed"
)

# The blacklist includes chrM and some scaffolds. Our peaks are restricted to
# standard chromosomes, so restrict the blacklist the same way.
blacklist_hg38 <- keepStandardChromosomes(blacklist_hg38, pruning.mode = "coarse")
genome(blacklist_hg38) <- "hg38"

# Fraction of each cell's counts that fall in blacklist regions, on a 0-1 scale
atac$blacklist_ratio <- FractionCountsInRegion(
  object  = atac,
  assay   = "peaks",
  regions = blacklist_hg38
)

summary(atac$blacklist_ratio)

#      Min.   1st Qu.    Median      Mean   3rd Qu.      Max.
# 0.0000000 0.0003804 0.0006378 0.0028487 0.0011288 0.2035831
```

`FractionCountsInRegion()` prints a deprecation warning on Signac 1.17. Unlike `NucleosomeSignal()`, it names no replacement, so for now the function is simply on notice. It still returns correct values.

**Why not AnnotationHub?** Signac’s vignette retrieves this same data through the `excluderanges` collection on AnnotationHub, and that works when the hub is healthy. But it adds two failure modes for no benefit: the hub’s fetch endpoint occasionally returns HTTP 500 errors that no amount of retrying fixes, and AnnotationHub record identifiers shift between snapshots, so a hardcoded ID rots. Since the underlying file is a fixed ENCODE accession, going to the source is both simpler and more reproducible.

### Reviewing All Metrics Before Plotting

```r
#-----------------------------------------------
# STEP 10: Summary of every QC metric
#-----------------------------------------------

qc_metrics <- c("passed_filters", "nCount_peaks", "TSS.enrichment",
                "nucleosome_signal", "pct_reads_in_peaks", "blacklist_ratio")

summary(atac@meta.data[, qc_metrics])
```

```
 passed_filters    nCount_peaks    TSS.enrichment    nucleosome_signal
 Min.   :  1089   Min.   :  1212   Min.   : 0.9535   Min.   :0.1461
 1st Qu.:  6182   1st Qu.:  6161   1st Qu.: 5.2189   1st Qu.:0.4706
 Median : 10720   Median : 11438   Median : 5.7426   Median :0.5164
 Mean   : 18605   Mean   : 14214   Mean   : 5.5494   Mean   :0.5670
 3rd Qu.: 18240   3rd Qu.: 19449   3rd Qu.: 6.2115   3rd Qu.:0.5782
 Max.   :357082   Max.   :112938   Max.   :13.8861   Max.   :4.0710
 pct_reads_in_peaks blacklist_ratio
 Min.   : 4.81      Min.   :0.0000000
 1st Qu.:55.06      1st Qu.:0.0003804
 Median :62.50      Median :0.0006378
 Mean   :56.25      Mean   :0.0028487
 3rd Qu.:66.33      3rd Qu.:0.0011288
 Max.   :75.21      Max.   :0.2035831
```

Read this table before you look at a single plot, compare it against the reference ranges given earlier, and ask three questions of it: does every metric have a plausible median, is any metric constant or all-zero (a sign of a computation that silently failed), and how heavy are the tails? The answers determine what to look for in the plots.

---

## Diagnostic QC Plots: Look Before You Filter

Applying thresholds without looking at the distributions is how people accidentally delete a real cell population. Every plot in this section takes about a minute and can save you a week.

### The Fragment-Size Periodicity Plot

This is the plot that tells you whether the Tn5 transposition worked. Splitting the cells by nucleosome signal makes the contrast explicit: the good cells show the periodic ladder, the bad cells do not.

```r
#-----------------------------------------------
# STEP 11: Fragment-size distribution, split by nucleosome signal
#-----------------------------------------------

# Label cells above and below the conventional nucleosome-signal cutoff.
# Cells with NaN nucleosome_signal (zero nucleosome-free fragments, Step 7) would
# become NA here and show up as a stray third group, so label them explicitly.
atac$nucleosome_group <- ifelse(
  is.na(atac$nucleosome_signal), "Undefined",
  ifelse(atac$nucleosome_signal > 4, "NS > 4", "NS < 4")
)

table(atac$nucleosome_group)

p_fraghist <- FragmentHistogram(object = atac, group.by = "nucleosome_group") +
  ggtitle("Fragment size distribution by nucleosome signal group")

ggsave(file.path(plot_dir, "01_fragment_histogram.png"), p_fraghist,
       width = 10, height = 5, dpi = 300)
```

![Fragment size histogram from scATAC-seq data split by nucleosome signal, showing a nucleosome ladder with a nucleosome-free peak below 147 bp, a mononucleosome hump near 200 bp, and a dinucleosome hump near 400 bp.](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/07/01_fragment_histogram.png?resize=1024%2C512&ssl=1)

**How to read it.** In the `NS < 4` panel you should see a tall peak below 147 bp (nucleosome-free fragments), a distinct second hump around 200 bp (mononucleosomal), and often a smaller third around 400 bp (dinucleosomal). That decaying periodicity is the signature of a working assay.

> 

**Interpretation for our sample.** The `NS < 4` panel is a textbook nucleosome ladder. A dominant nucleosome-free peak rises to roughly 4,700 fragments at about 40-50 bp and falls away steeply, there is a clear trough near 150 bp, a well-defined mononucleosome hump centred around 180-200 bp, a second trough near 280 bp, and a low but visible dinucleosome shoulder around 350-400 bp before the distribution decays into the tail. Three humps at roughly 180 bp spacing is exactly the periodicity that says the Tn5 cut between nucleosomes rather than through them. Whatever else is true of this sample, the transposition worked.

**The `NS > 4` panel is the more instructive one, because it is essentially empty.** Instead of a distribution it shows about eight isolated vertical lines, each one fragment tall, and the y-axis runs from 0 to 1 rather than to thousands. That is not a rendering fault. **Only one cell in this entire sample has a nucleosome signal above 4** — confirmed independently by the filter audit in Step 16, where `high_nucleosome` flags exactly one barcode. The panel is showing you the handful of fragments belonging to that single cell.

This is worth sitting with, because it is a case where the diagnostic works by *failing to find anything*. The plot was designed as a good-versus-bad comparison, and on a poor library the right panel would show a suppressed nucleosome-free peak and an inflated mononucleosome hump. Here there is no bad group to compare against. An empty contrast panel is a strong positive result, not a broken figure — and it tells you in advance that the `nucleosome_signal < 4` filter will do essentially no work on this sample.

If **neither** panel shows periodicity, the problem is library-wide, not cell-specific, and you are back at Level 1 QC. If the contrast panel is nearly empty as it is here, your library is clean on this axis and the nucleosome filter is a formality you should still keep for consistency across the cohort.

### The TSS Coverage Plot

```r
#-----------------------------------------------
# STEP 12: Aggregate Tn5 coverage around transcription start sites
#-----------------------------------------------

# Split cells into high and low TSS groups so both profiles appear on one plot.
# Requires TSSEnrichment(fast = FALSE) from Step 6. TSSPlot() is deprecated in
# Signac 1.17 but still functional, and remains the only route to this figure:
# ATACqc() does not store the per-base coverage matrix that TSSPlot() needs.
atac$tss_group <- ifelse(atac$TSS.enrichment > 2, "High TSS", "Low TSS")

p_tss <- TSSPlot(atac, group.by = "tss_group") +
  NoLegend() +
  ggtitle("Tn5 insertion enrichment around TSS")

ggsave(file.path(plot_dir, "02_tss_enrichment.png"), p_tss,
       width = 10, height = 5, dpi = 300)
```

![Tn5 insertion enrichment around transcription start sites in scATAC-seq data, comparing high-TSS cells with a sharp promoter spike and a +1 nucleosome shoulder against low-TSS cells with a flat profile.](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/07/02_tss_enrichment.png?resize=1024%2C512&ssl=1)

**How to read it.** The x-axis is distance from the TSS in base pairs, the y-axis is normalized coverage. The `High TSS` group should produce a sharp spike centered near zero. The `Low TSS` group should be nearly flat — fragments distributed without regard to promoter position, which is the definition of a barcode carrying noise instead of chromatin structure.

> 

**Interpretation for our sample.** The contrast is about as clean as this plot gets. The `High TSS` panel climbs from a baseline near 1 at -1,000 bp to a sharp maximum of roughly 16.5 just upstream of the TSS, then drops steeply. The `Low TSS` panel is essentially flat across the whole window, drifting between 1 and 2 with only a weak bump of about 2.5 at position zero. Same genome, same annotation, same experiment — the only difference is which barcodes were pooled. That is the entire argument for the TSS threshold, drawn rather than asserted.

**Two features of the high-TSS curve are real biology, not noise.** The profile is asymmetric: signal builds gradually across the upstream region and collapses quickly downstream. And after that collapse there is a distinct secondary shoulder at roughly +250 bp, reaching about 4.5. That shoulder is the **+1 nucleosome** — the first nucleosome downstream of the transcription start site, which sits at a well-defined position and forces Tn5 to cut just before and just after it. The dip between the main peak and the shoulder is the nucleosome itself, protected from transposition. Seeing this structure means your annotation is correctly aligned to your data; a mismatched annotation smears the profile into a broad, featureless bump instead.

**A caveat on the grouping.** We split at `TSS.enrichment > 2`, so the `Low TSS` panel contains only 190 cells (Step 16) against 4,424 in the high group. The low-TSS curve is therefore noticeably noisier — that jitter is small-sample variance, not a property of those cells. The comparison is still valid; just do not over-read wiggles in the flat panel.

### Violin Plots of Every QC Metric

```r
#-----------------------------------------------
# STEP 13: Violin plots of the per-cell QC metrics
#-----------------------------------------------

p_vln <- VlnPlot(
  object   = atac,
  features = qc_metrics,
  pt.size  = 0.1,
  ncol     = 3
) & theme(axis.title.x = element_blank())

ggsave(file.path(plot_dir, "03_qc_violins.png"), p_vln,
       width = 14, height = 8, dpi = 300)
```

![Violin plots of six scATAC-seq quality control metrics across 4,614 unfiltered cells: total fragments, peak counts, TSS enrichment, nucleosome signal, fraction of reads in peaks, and blacklist ratio.](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/07/03_qc_violins.png?resize=1024%2C585&ssl=1)

**How to read it.** You are looking for shape, not location. A single broad mode is normal. Two separated modes — especially in `passed_filters` or `pct_reads_in_peaks` — usually means two populations of barcodes are present: real cells and something else. A long thin tail toward high fragment counts is your doublet population. A cluster of points crushed against zero in `TSS.enrichment` is your dead-cell population.

### Density Scatter: Letting the Data Suggest Thresholds

The most useful single diagnostic in modern Signac is `DensityScatter()`, which plots two metrics against each other and, with `quantiles = TRUE`, overlays quantile lines so you can see where natural breaks in your data fall.

```r
#-----------------------------------------------
# STEP 14: Joint distribution of depth and TSS enrichment
#-----------------------------------------------

p_density <- DensityScatter(
  atac,
  x         = "nCount_peaks",
  y         = "TSS.enrichment",
  log_x     = TRUE,
  quantiles = TRUE
)

ggsave(file.path(plot_dir, "04_density_scatter.png"), p_density,
       width = 8, height = 6, dpi = 300)
```

![Density scatter of TSS enrichment against fragment depth per cell in scATAC-seq data, with quantile lines showing a dense high-quality cloud and a separate low-TSS band spread across all sequencing depths.](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/07/04_density_scatter.png?resize=1024%2C768&ssl=1)

**How to read it.** A good sample shows one dense cloud: cells with both adequate depth and good signal-to-noise. Your job is to place thresholds outside that cloud, not through the middle of it. The quantile lines give you concrete numbers to reach for. It converts threshold selection from guesswork into reading a picture.

---

## Applying Cell-Level Filtering

### Choosing Thresholds That Match Your Data

The thresholds below are a defensible starting point for cryopreserved clinical PBMCs, and they match the ranges reported in published snATAC-seq studies of blood. They are deliberately more permissive than the values in Signac’s demonstration vignette, which uses a fresh, high-quality 10x library and can afford `TSS.enrichment > 4` and `pct_reads_in_peaks > 40`.

`passed_filters``passed_filters``TSS.enrichment``pct_reads_in_peaks``nucleosome_signal``blacklist_ratio`

| Metric | Threshold used here | What it removes | Adjust when |
| --- | --- | --- | --- |
|  | > 3,000 | Empty droplets and destroyed nuclei | Lower to ~1,000 for shallow libraries; raise if your median depth is high |
|  | < 100,000 | Doublets, multiplets, nuclear clumps | Set from your own distribution: roughly 10x the median is a common rule |
|  | > 2 | Cells with depth but no chromatin structure | Raise toward 4 for fresh high-quality material; check the density scatter first |
|  | > 15 | Cells dominated by ambient or off-target DNA | Raise toward 30-40 for high-quality libraries |
|  | < 4 | Cells with poor transposition | This cutoff is stable across studies; rarely needs changing |
|  | < 0.05 | Artifact-driven barcodes | Tighten to 0.01 if your library is clean |

Before committing, it costs nothing to price each alternative. Because every metric is already in the metadata, you can count what any candidate threshold would remove without recomputing anything:

```bash
# What would each candidate FRiP / TSS threshold cost, in cells?
sapply(c(15, 20, 30, 40), function(t) sum(atac$pct_reads_in_peaks <= t))
sapply(c(2, 3, 4),        function(t) sum(atac$TSS.enrichment     <= t))

# [1] 375 419 474 572
# [1] 190 316 409
```

> 

**Interpretation for our sample: the choice barely matters, and that is the result.**

Moving the FRiP floor from 15 percent all the way to 40 — a 25-point swing, and the value Signac’s own vignette uses — costs only **197 additional cells, 4.27 percent of the dataset**. Moving the TSS floor from 2 to 4 costs 219 cells, 4.75 percent. Even adopting both aggressive values together retains at least 76 percent of cells.

**A sparse interval between two thresholds means the result is insensitive to where you draw the line.** If the region between 15 and 40 percent FRiP were densely populated, the threshold would be a consequential scientific choice and would need defending. It is not, so it does not. This is the same conclusion the density scatter suggested visually, now with numbers behind it, and it is the single most useful thing a sensitivity check can tell you: *stop deliberating and move on.*

**When the metrics disagree, trust TSS enrichment over FRiP.** This matters for choosing which knob to turn if you do tighten. TSS enrichment is computed against a gene annotation, so it is independent of your peak set. FRiP is computed against your peaks, so a cell whose regulatory elements were missed by an aggregate peak caller shows an artificially low FRiP through no fault of its own — exactly the rare-population failure mode described earlier. Tightening FRiP therefore carries a specific risk that tightening TSS does not: it preferentially removes cells from populations your peak set under-represents. Given a choice, raise the TSS floor and leave FRiP permissive.

Two principles to hold onto. **Never copy thresholds without looking at your own distributions** — a value that removes 5 percent of cells in one sample can remove 60 percent in another. And **apply identical thresholds to every sample in a cohort**. Per-sample tuning feels responsible but introduces a systematic difference between conditions that will surface later as a fake biological result.

### Applying the Filter

```r
#-----------------------------------------------
# STEP 15: Filter low-quality cells
#-----------------------------------------------

# Record the pre-filter dimensions so we can quantify what was removed
n_cells_before <- ncol(atac)
n_peaks_before <- nrow(atac)

# Save the metric-complete object BEFORE filtering. TSSEnrichment(fast = FALSE)
# is the slowest step in this tutorial, and re-running it just to try a
# different threshold is wasted compute. With this file on disk you can reload
# and re-filter in seconds.
saveRDS(atac, file.path(obj_dir, paste0(sample_id, "_prefilter_metrics.rds")))

atac_filtered <- subset(
  x = atac,
  subset = passed_filters     > 3000 &
           passed_filters     < 100000 &
           TSS.enrichment     > 2 &
           pct_reads_in_peaks > 15 &
           nucleosome_signal  < 4 &
           blacklist_ratio    < 0.05
)

atac_filtered

# An object of class Seurat
# 110667 features across 3924 samples within 1 assay
# Active assay: peaks (110667 features, 0 variable features)
#  2 layers present: counts, data
```

All six conditions are combined with `&`, so a cell must pass every one of them to be retained. Note that `subset()` silently drops any cell with an `NA` or `NaN` in a filtered column — which conveniently handles the `NaN` nucleosome signals mentioned in Step 7, but is worth knowing about in general.

### Auditing What You Removed

Filtering blind is how good data gets deleted. Always quantify the damage, and always attribute it to a specific criterion.

```r
#-----------------------------------------------
# STEP 16: Audit the filtering, criterion by criterion
#-----------------------------------------------

# How many cells each individual criterion would remove, on its own
filter_criteria <- list(
  low_depth        = atac$passed_filters     <= 3000,
  high_depth       = atac$passed_filters     >= 100000,
  low_tss          = atac$TSS.enrichment     <= 2,
  low_frip         = atac$pct_reads_in_peaks <= 15,
  high_nucleosome  = atac$nucleosome_signal  >= 4,
  high_blacklist   = atac$blacklist_ratio    >= 0.05
)

# sapply over the list keeps this compact and avoids a for-loop.
# na.rm = TRUE because nucleosome_signal can be NaN for very shallow barcodes.
filter_summary <- data.frame(
  criterion    = names(filter_criteria),
  cells_failed = sapply(filter_criteria, sum, na.rm = TRUE)
)
filter_summary$pct_of_total <- round(
  filter_summary$cells_failed / n_cells_before * 100, 2
)

filter_summary

# Overall retention
data.frame(
  cells_before = n_cells_before,
  cells_after  = ncol(atac_filtered),
  pct_retained = round(ncol(atac_filtered) / n_cells_before * 100, 1)
)
```

**Expected output:**

```
                      criterion cells_failed pct_of_total
low_depth             low_depth          312         6.76
high_depth           high_depth          141         3.06
low_tss                 low_tss          190         4.12
low_frip               low_frip          375         8.13
high_nucleosome high_nucleosome            1         0.02
high_blacklist   high_blacklist           23         0.50

  cells_before cells_after pct_retained
1         4614        3924           85
```

**How to interpret this table.** The counts overlap — a single dying cell typically fails the depth, TSS, and FRiP criteria simultaneously — so the individual numbers will sum to more than the total removed. That is expected and informative: strongly overlapping failures mean your criteria are consistently identifying the same bad barcodes, which is a good sign.

What you are watching for is one criterion doing all the work. If `low_frip` removes 45 percent of cells on its own while every other criterion removes 3-5 percent, your FRiP threshold is miscalibrated for this library — or your peak set is missing regulatory elements for a whole population, which would be a Part 1 problem rather than a Part 2 one.

As a rough guide, retaining **70-90 percent** of Cell Ranger’s called cells is typical for a good sample. Retaining under 50 percent means either the library is poor or your thresholds are too strict; go back to the density scatter and check whether your lines are cutting through the main cloud.

### Detecting Doublets with AMULET

One failure mode survives everything above. The `passed_filters < 100000` bound removed 141 cells, and it is tempting to call multiplets handled. It is not. That bound catches the conspicuous cases — nuclear clumps and droplets that captured several nuclei — but it fails in both directions. A large, highly accessible single cell can exceed the cap through nothing but genuine biology, while a doublet of two shallow cells lands in the middle of a normal distribution.

The harder case is the **homotypic doublet**: two cells of the *same* type in one droplet. Their combined profile looks exactly like a single cell of that type, only slightly deeper. Normal TSS enrichment, normal FRiP, normal nucleosome signal, normal blacklist ratio. Not one of the five metrics you just computed flags it — and neither do transcriptome-style doublet callers, which work by spotting a mixture of two distinct signatures. A homotypic doublet has no mixture to spot.

Left in the data, doublets do not stay quiet. They form small clusters whose profile is a blend of two real cell types, which look convincingly like novel intermediate or transitional states and are nothing of the sort.

**AMULET** (Thibodeau et al., 2021) sidesteps profile comparison entirely with an argument from ploidy:

> 

A diploid cell has exactly **two copies** of every autosomal locus, so at most two distinct fragments can originate from any one position. A barcode showing a locus covered by **more than two** distinct fragments must contain more than one nucleus.

That is a physical constraint rather than a statistical model of what cell types look like, and three consequences follow. It **detects homotypic doublets**, because two cells of the same type still carry four copies of every locus between them. It needs **no clustering, annotation, or reference** — only the fragments file. And it is **independent of your peak set**, so unlike FRiP it cannot be biased by regulatory elements the aggregate peak caller missed.

The original AMULET shipped as a mix of Java and Python scripts. The Bioconductor package **scDblFinder** reimplements it in R, with performance its authors report as equal to or better than the original — which is what makes this step practical enough to belong in a beginner workflow.

We run it **after** cell-level filtering rather than before, for two reasons: the background expectation is better estimated from good cells, and there is no point spending compute on barcodes already destined for removal.

```r
#-----------------------------------------------
# STEP 17a: Detect doublets with AMULET
#-----------------------------------------------

# Regions to exclude from the per-locus test: sex chromosomes (copy number is
# not reliably two), unplaced scaffolds (repetitive sequence produces spurious
# pileups), and the ENCODE blacklist. chrM needs no handling -- Cell Ranger
# already excludes mitochondrial fragments from passed_filters.
frag_path <- file.path(cr_dir, "fragments.tsv.gz")

# Contigs actually present in the fragments file, read from the tabix index.
# Reading them rather than hardcoding keeps this working on any reference.
frag_contigs <- seqnamesTabix(TabixFile(frag_path))
scaffolds    <- setdiff(frag_contigs, paste0("chr", c(1:22, "X", "Y")))

# width = 10^9 rather than 10^8: chrX is 156 Mb, so a 100 Mb range would leave
# a third of it unexcluded. Over-wide ranges are harmless.
exclude_regions <- suppressWarnings(c(
  GRanges(c("chrX", "chrY"), IRanges(1L, width = 10^9)),
  GRanges(scaffolds,         IRanges(1L, width = 10^9)),
  blacklist_hg38
))

# The tabix index lets scDblFinder stream one chromosome at a time;
# without it the whole fragments file is read into memory.
amulet_res <- amulet(
  frag_path,
  regionsToExclude = exclude_regions,
  barcodes         = colnames(atac_filtered)
)
```

> 

**A printout that looks like a failure but is not.** As this runs, scDblFinder lists every contig it visits — including the `KI270728.1` and `GL000009.2` scaffolds you removed back in Step 3. Nothing has gone wrong. Two separate things are happening. First, Step 3 filtered scaffold **peaks** out of the count matrix; the **fragments file** is an immutable file on disk holding every fragment Cell Ranger produced across all contigs, and Signac only stores a path to it. Second, `regionsToExclude` filters fragments *after* they are read, so it never changes which contigs get visited. You will see that list on every run regardless of what you exclude. As with `seqlevels()` in Step 4, the progress output describes the read loop, not the filter.

The result is one row per barcode. `nAbove2` is the evidence — the number of loci covered by more than two fragments — and `q.value` is the multiple-testing-corrected significance. **A low q-value indicates a doublet**, which is the opposite convention to the `scDblFinder` score and an easy thing to get backwards.

```r
#-----------------------------------------------
# STEP 17b: Remove predicted doublets
#-----------------------------------------------

atac_filtered$amulet_q <- amulet_res[colnames(atac_filtered), "q.value"]

# Barcodes AMULET could not evaluate return NA, and subset() silently drops
# NA rows -- set them to 1 so they are retained rather than lost.
atac_filtered$amulet_q[is.na(atac_filtered$amulet_q)] <- 1

table(doublet = atac_filtered$amulet_q < 0.05)

# doublet
# FALSE  TRUE
# 3519   405

atac_filtered <- subset(atac_filtered, subset = amulet_q >= 0.05)
```

> 

**Keep the fragment cap as well.** AMULET tests per-locus coverage, so it is suited to two-nucleus droplets. The upper `passed_filters` bound catches debris and clumps whose fragments may never concentrate at shared loci. They are complementary, and running both costs nothing.

### Confirming the Filtering Worked

With low-quality cells and predicted doublets both removed, redraw the violin panel from Step 13 and compare the two figures side by side.

```r
#-----------------------------------------------
# STEP 17c: Confirm the filtering worked, visually
#-----------------------------------------------

# Same violins as Step 13, now on the filtered object. The tails should be gone
# and each distribution should be a single clean mode.
p_vln_after <- VlnPlot(
  object   = atac_filtered,
  features = qc_metrics,
  pt.size  = 0.1,
  ncol     = 3
) & theme(axis.title.x = element_blank())

ggsave(file.path(plot_dir, "05_qc_violins_filtered.png"), p_vln_after,
       width = 14, height = 8, dpi = 300)
```

![Violin plots of the same six scATAC-seq quality control metrics after cell filtering and AMULET doublet removal, with each distribution reduced to a single clean mode and the extreme tails gone.](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/07/05_qc_violins_filtered.png?resize=1024%2C585&ssl=1)

> 

**Interpretation for our sample.** Compare panel by panel against the pre-filter figure and every distribution has become a single clean mode.

**What to check, and what not to over-read.** The right thing to verify here is that each panel is *unimodal and continuous* — no residual second population, no cliff in the middle of the distribution. That is satisfied. What you should not do is conclude the data is now “clean” in an absolute sense: filtering guarantees the retained cells pass your criteria, which is a tautology, not evidence. The real test of these thresholds comes in Part 3, when clusters either separate by cell type or by sequencing depth.

---

## Feature-Level QC: Cleaning Up the Peak Set

Cell-level filtering is only half the job. The peak set carries its own problems, and a peak that is pure artifact contributes to the dimensionality reduction exactly as much as a genuine enhancer does.

### Why This Section Comes After Cell Filtering

The order of operations here is not arbitrary, and getting it wrong produces subtly wrong results:

1. **Standard chromosomes were filtered first** (Step 3), because scaffold peaks pollute the denominators of FRiP and the blacklist ratio.

2. **The blacklist ratio was computed before removing blacklist peaks.** If you delete blacklist-overlapping peaks first, every cell’s blacklist ratio becomes zero and the metric is destroyed. Compute the metric, filter the cells, then remove the peaks.

3. **Rare peaks are re-filtered after cell filtering**, because removing several thousand low-quality cells pushes additional peaks below the minimum-cell threshold. A peak that was in twelve cells before filtering may be in three afterwards.

### Removing Peaks in ENCODE Blacklist Regions

```r
#-----------------------------------------------
# STEP 18: Drop peaks overlapping ENCODE blacklist regions
#-----------------------------------------------

# TRUE for peaks with no overlap at all against the blacklist
peaks_not_blacklisted <- countOverlaps(granges(atac_filtered), blacklist_hg38) == 0

# Record how many peaks this removes before subsetting
sum(!peaks_not_blacklisted)
# 268

atac_filtered <- atac_filtered[as.vector(peaks_not_blacklisted), ]
```

This is usually a small number of peaks — a few hundred out of a few hundred thousand — but they are disproportionately high-signal peaks, so removing them has an outsized effect on the dimensionality reduction. If this step removes tens of thousands of peaks, check that your blacklist and your peak set use the same genome build.

### Removing Rare Peaks

`CreateChromatinAssay(min.cells = 10)` applied this filter once, on the unfiltered cell set. Now we re-apply it, and scale the threshold to the dataset size so that it behaves sensibly whether you have 2,000 cells or 20,000.

```r
#-----------------------------------------------
# STEP 19: Remove peaks accessible in too few cells
#-----------------------------------------------

# Binarize the counts and sum per peak: how many cells detect each peak?
peak_counts   <- LayerData(atac_filtered, assay = "peaks", layer = "counts")
cells_per_peak <- Matrix::rowSums(peak_counts > 0)

# Threshold: whichever is larger, 10 cells or 0.5% of the dataset.
# The absolute floor protects tiny datasets; the proportional term keeps the
# filter meaningful as the cell count grows.
min_cells_per_peak <- max(10, ceiling(0.005 * ncol(atac_filtered)))
min_cells_per_peak

peaks_keep <- cells_per_peak >= min_cells_per_peak
sum(!peaks_keep)   # peaks about to be removed

atac_filtered <- atac_filtered[as.vector(peaks_keep), ]

# Final dimensions after all cell and feature filtering
data.frame(
  peaks_before = n_peaks_before,
  peaks_after  = nrow(atac_filtered),
  cells_before = n_cells_before,
  cells_after  = ncol(atac_filtered)
)
```

**Expected output:**

```
min_cells_per_peak
[1] 20

sum(!peaks_keep)
[1] 19747

peaks_before peaks_after cells_before cells_after
1       110667       89129         4614        3519
```

**How aggressive should this be?** A 0.5 percent floor is conventional and conservative. Raising it to 1 percent shrinks the feature set further and speeds up the downstream steps, but a cell type that makes up only 2 percent of your PBMCs has genuinely cell-type-specific peaks that appear in only 2 percent of cells — push the threshold too high and you erase exactly the rare-population signal you wanted. If your biological question centers on a rare population, stay at or below 0.5 percent.

### Saving the Object for Part 3

```r
#-----------------------------------------------
# STEP 20: Save the QC-filtered object
#-----------------------------------------------

# Store the sample identity in metadata now, so that merging samples in Part 3
# does not require reconstructing where each cell came from
atac_filtered$sample_id <- sample_id
atac_filtered$condition <- ifelse(grepl("^severe", sample_id), "Severe", "Healthy")

saveRDS(atac_filtered,
        file.path(obj_dir, paste0(sample_id, "_qc.rds")))

# Record the software versions used, for the methods section of your paper
sessionInfo()
```

Saving `sessionInfo()` output alongside your object is a habit worth building. Signac, Seurat, and the Bioconductor annotation packages all change behavior between versions, and six months from now this file is the only reliable record of what you actually ran.

---

## Scaling the Workflow to All Four Samples

This tutorial covered the complete QC process for a single sample. Because our dataset contains four samples — `severe1`, `severe2`, `healthy1`, and `healthy2` — each requires identical processing. Apply this same workflow to the remaining samples before moving on to the next step.

---

## Best Practices for scATAC-seq Quality Control

**1. Check the library before you check the cells.** Read `web_summary.html` first, every time. A failed library cannot be rescued by filtering, and discovering that after four hours of R is a bad way to spend an afternoon. Levels 1 through 4 exist in that order for a reason.

**2. Match the gene annotation to the alignment reference exactly.** A `GRCh38-2024-A` alignment needs GENCODE v44 / Ensembl 110 annotations, and Ensembl chromosome names must be converted to UCSC style. Getting this wrong does not throw an error — it returns a TSS enrichment score of zero for every cell, which reads like a data problem rather than a code problem.

**3. Plot every metric before you threshold it.** `DensityScatter()` with `quantiles = TRUE` and the violin panel take under two minutes together and convert threshold selection from guesswork into reading a picture. Place thresholds in the gap between populations, not through the middle of the main cloud.

**4. Never transplant thresholds from a demonstration dataset.** Signac’s PBMC vignette uses `TSS.enrichment > 4` and `pct_reads_in_peaks > 40` on a fresh, high-quality 10x library. Applied to cryopreserved clinical samples those values can delete most of your data. Published thresholds are calibration references, not constants.

**5. Apply identical thresholds across every sample in a cohort.** Per-sample tuning feels careful but introduces systematic differences between conditions that later masquerade as biology. Choose one threshold set, apply it everywhere, and record how many cells each sample lost.

**6. Respect the order of operations.** Compute the blacklist ratio before removing blacklist peaks. Filter chromosomes before computing FRiP. Re-filter rare peaks after removing bad cells. Each of these orderings prevents a specific, silent error.

**7. Keep the fragments file with its index, and keep the paths stable.** Signac stores an absolute path to `fragments.tsv.gz` inside the object. Move or rename the file and every fragment-based function in a reloaded object will fail. If you must relocate the data, `UpdatePath()` on the `Fragment` object repairs the reference.

**8. Point AnnotationHub at project storage before you call it.** Use `setAnnotationHubOption("CACHE", path)`, not `Sys.setenv()`, and confirm with `hubCache(ah)`. Annotation and blacklist resources are large, home directories on shared clusters are small, and a quota error partway through a download is an avoidable waste of an afternoon.

**9. Pin Signac and expect deprecations.** Signac is under active development and 1.17 deprecated six functions used in workflows like this one. Record the version in `pixi.lock` and in `sessionInfo()`, and when a warning tells you a function has moved, check what the replacement actually requires before switching — `ATACqc()` looks like a drop-in for two functions until you discover it needs an external Rust binary and renames its output columns.

**10. Run a real doublet caller, and keep the fragment cap as well.** An upper bound on `passed_filters` is a blunt instrument: a large, highly accessible cell can exceed it, and a doublet of two shallow cells slips under. Homotypic doublets are invisible to every signal-quality metric. AMULET tests the one thing that cannot be faked — a diploid cell cannot yield more than two fragments from one locus. Run both; they catch different failure modes.

**11. Save `sessionInfo()` with every saved object.** Signac, Seurat, and the Bioconductor annotation packages change behavior between releases. This one line is what makes your analysis reproducible and your methods section accurate.

---

## Common Pitfalls and How to Avoid Them

`1``2``chr1``chr2``seqlevels(annotations) <- paste0("chr", seqlevels(annotations))``Annotation(atac) <- annotations``summary()``DensityScatter()``seqlevels()``seqlevels()``Seqinfo``seqlevelsInUse()``table(seqnames(...))``nFeature_peaks``UpdatePath()``nCount_peaks``nCount_ATAC``assay =``CreateSeuratObject()``colnames(atac@meta.data)``NaN``nucleosome_signal``subset()``na.rm = TRUE``median()``sum()`

| Pitfall | Why it happens | How to avoid it |
| --- | --- | --- |
| TSS enrichment is zero for every cell | Ensembl-style chromosome names (, ) never match the UCSC-style names (, ) in the 10x reference | Run  in Step 5, before , and always  the result |
| Filtering deletes most of the dataset | Thresholds were copied from a fresh-tissue demonstration vignette and applied to frozen clinical samples | Look at  first; expect 70-90 percent retention and investigate anything below 50 percent |
| Blacklist ratio is zero everywhere | Blacklist-overlapping peaks were removed before the ratio was computed | Compute cell metrics first, filter cells, then filter features — the order used in Steps 9, 15, and 18 |
| A small cluster in Part 3 looks like a novel intermediate cell state | Undetected doublets form a cluster whose profile blends two real types | Run AMULET in Step 17a. Heterotypic doublets are exactly the ones that produce convincing-looking intermediate clusters |
| A rare cell population disappears after QC | Its cell-type-specific peaks were missing from an aggregate peak set, so those cells showed artificially low FRiP; or the rare-peak threshold was too aggressive | Use per-cluster MACS3 peaks alongside Cell Ranger’s aggregate peaks (Part 1), and keep the rare-peak threshold at or below 0.5 percent |
| Believing a filter failed because  is unchanged | shows the declared , not the sequences actually present; subsetting rows never prunes it | Verify with  or , and filter the count matrix before assay creation so the two always agree |
| used as the main quality filter | Habit carried over from scRNA-seq, where genes-per-cell is genuinely informative | In near-binary accessibility data this metric mostly restates sequencing depth and depends on the peak set. Filter on total fragments, TSS enrichment, and FRiP instead |
| Object reloads but fragment-based functions fail | The fragments file was moved after the object was saved; the stored absolute path is now stale | Keep data paths stable, or repair with |
| Different code examples reference different metric names | vs  depends on what you passed to  in | Pick one assay name and use it consistently; check with  when in doubt |
| values in | Barcodes with zero nucleosome-free fragments give a zero denominator | Harmless here because  drops them, but use  in any  or  you compute over that column |

---

## Conclusion

You have taken a raw Cell Ranger ATAC output directory and produced a quality-controlled, doublet-filtered object ready for integration in Part 3. Along the way you have:

1. **Learned why scATAC-seq QC needs its own metrics**, because a diploid cell has only two copies of every locus, making the peak-by-cell matrix sparse and near-binary and rendering scRNA-seq reflexes like genes-per-cell and percent-mitochondrial unusable.

2. **Computed the six core metrics** — total fragments, TSS enrichment, nucleosome signal, FRiP, blacklist ratio, and the fragment-size distribution — and understood which failure mode each one catches and which ones it misses.

3. **Worked through four distinct levels of QC**, from library-level diagnosis in `web_summary.html` through cell filtering, feature filtering, and cross-sample comparison.

4. **Inspected before filtering**, using the fragment-size histogram, the TSS coverage profile, violin panels, and a density scatter to place thresholds in the gap between populations rather than guessing.

5. **Detected and removed doublets with AMULET**, using the fact that a diploid cell cannot yield more than two fragments from a single locus — the only step in this tutorial that catches homotypic doublets, which every signal-quality metric misses by construction.

6. **Filtered cells and peaks in the correct order**, computing the blacklist ratio before removing blacklist peaks and re-filtering rare peaks only after low-quality cells were gone.

The habit worth carrying forward is the one that runs through every step: look at the data before you act on it. Every threshold in this tutorial was chosen by reading a plot, and every ordering decision prevents a specific silent error.

If you have not yet worked through the transcriptomic side, the companion [scRNA-seq Complete Beginner’s Guide](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/) covers the same PBMC workflow for gene expression, and its [quality control tutorial](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/) is a useful contrast to this one. Part 3 then covers [integration and clustering](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/), which is the transcriptomic mirror of what comes next here. For the bulk chromatin foundations behind TSS enrichment and nucleosome banding, see our [ATAC-seq beginner’s guide](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/).

---

## References

1. Stuart T, Srivastava A, Madad S, Lareau CA, Satija R. Single-cell chromatin state analysis with Signac. *Nature Methods*. 2021;18(11):1333-1341. doi:10.1038/s41592-021-01282-5

2. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

3. Cusanovich DA, Daza R, Adey A, et al. Multiplex single-cell profiling of chromatin accessibility by combinatorial cellular indexing. *Science*. 2015;348(6237):910-914. doi:10.1126/science.aab1601 [Origin of LSI for scATAC-seq]

4. Buenrostro JD, Giresi PG, Zaba LC, Chang HY, Greenleaf WJ. Transposition of native chromatin for fast and sensitive epigenomic profiling of open chromatin, DNA-binding proteins and nucleosome position. *Nature Methods*. 2013;10(12):1213-1218. doi:10.1038/nmeth.2688 [Original ATAC-seq method]

5. Amemiya HM, Kundaje A, Boyle AP. The ENCODE Blacklist: Identification of Problematic Regions of the Genome. *Scientific Reports*. 2019;9(1):9354. doi:10.1038/s41598-019-45839-z

6. ENCODE Project Consortium. ENCODE Data Standards and Terms: TSS enrichment and FRiP. [https://www.encodeproject.org/data-standards/terms/](https://www.encodeproject.org/data-standards/terms/) (2026)

7. Granja JM, Corces MR, Pierce SE, et al. ArchR is a scalable software package for integrative single-cell chromatin accessibility analysis. *Nature Genetics*. 2021;53(3):403-411. doi:10.1038/s41588-021-00790-6

8. Thibodeau A, Eroglu A, McGinnis CS, et al. AMULET: a novel read count-based method for effective multiplet detection from single nucleus ATAC-seq data. *Genome Biology*. 2021;22(1):252. doi:10.1186/s13059-021-02469-x

9. Germain PL, Lun A, Garcia Meixide C, Macnair W, Robinson MD. Doublet identification in single-cell sequencing data using scDblFinder. *F1000Research*. 2021;10:979. doi:10.12688/f1000research.73600.2

10. Baek S, Lee I. Single-cell ATAC sequencing analysis: From data preprocessing to hypothesis generation. *Computational and Structural Biotechnology Journal*. 2020;18:1429-1439. doi:10.1016/j.csbj.2020.06.012

11. Remodeling of the chromatin landscape in peripheral blood cells in patients with severe Delta COVID-19. *Frontiers in Immunology*. 2024;15:1415317. doi:10.3389/fimmu.2024.1415317 (scATAC-seq data: NCBI GEO GSE282769; SRA BioProject PRJNA1190389)

12. Rainer J, Gatto L, Weichenberger CX. ensembldb: an R package to create and use Ensembl-based annotation resources. *Bioinformatics*. 2019;35(17):3151-3153. doi:10.1093/bioinformatics/btz031

13. Ou J, Zhu LJ. excluderanges: Genomic exclusion sets for reproducible epigenomics analyses. Bioconductor AnnotationHub resource. [https://bioconductor.org/packages/AnnotationHub/](https://bioconductor.org/packages/AnnotationHub/) (2026)

14. Signac documentation and vignettes. [https://stuartlab.org/signac/](https://stuartlab.org/signac/) (2026)

15. 10x Genomics. Interpreting Cell Ranger ATAC Web Summary Files. [https://www.10xgenomics.com/support/software/cell-ranger-atac/latest](https://www.10xgenomics.com/support/software/cell-ranger-atac/latest) (2026)

---

*This tutorial is part of the comprehensive NGS101.com single-cell ATAC-seq analysis series for beginners.*
