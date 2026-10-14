# How to Analyze Single-Cell ATAC-seq Data &#8212; A Complete Beginner's Guide Part 1: From FASTQ to Peaks

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** ATAC-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/)

---

*Learn what chromatin accessibility tells you, then take a real 10x Genomics scATAC-seq dataset from raw FASTQ files all the way to a peak-by-cell matrix with Cell Ranger ATAC and MACS3.*

> 

**Prerequisites:** Comfort on the Linux command line (navigating folders, running commands) and roughly 100 GB of free disk space. No prior epigenomics or programming experience is required — every concept and every command is explained from the ground up. If you have followed our [scRNA-seq Complete Beginner’s Guide](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/), you already have most of the background you need.

---

This is Part 1 of a new NGS101 series on single-cell ATAC-seq (scATAC-seq) analysis. It is designed as the epigenomic companion to our scRNA-seq series: the same beginner-first pace, the same emphasis on understanding what each step actually does, and the same immune-cell focus — human peripheral blood mononuclear cells (PBMCs). Rather than a demonstration sample, we use a real COVID-19 case/control cohort with matched scRNA-seq, so the series can build all the way to differential accessibility between healthy and disease and, later, to integrating chromatin accessibility with gene expression.

By the end of this tutorial you will understand what scATAC-seq measures and how it differs from bulk ATAC-seq and from scRNA-seq, you will have a reproducible analysis environment installed with Pixi, and you will have processed a real patient sample from raw reads into called peaks and a peak-by-cell count matrix — the starting point for all downstream biology in the parts that follow.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/)

- [Introduction: Understanding Single-Cell Chromatin Accessibility](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Introduction_Understanding_Single-Cell_Chromatin_Accessibility)

  - [What Is scATAC-seq and How Is It Different From Bulk ATAC-seq?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#What_Is_scATAC-seq_and_How_Is_It_Different_From_Bulk_ATAC-seq)
  - [scATAC-seq or snATAC-seq? A Note on Terminology](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#scATAC-seq_or_snATAC-seq_A_Note_on_Terminology)
  - [Current scATAC-seq Technologies and Their Use Cases](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Current_scATAC-seq_Technologies_and_Their_Use_Cases)
  - [Why 10x Genomics Chromium Dominates scATAC-seq](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Why_10x_Genomics_Chromium_Dominates_scATAC-seq)
  - [What Biological Insights Come From scATAC-seq?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#What_Biological_Insights_Come_From_scATAC-seq)
  - [scATAC-seq vs scRNA-seq: When Do You Need Which — or Both?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#scATAC-seq_vs_scRNA-seq_When_Do_You_Need_Which_%E2%80%94_or_Both)
  - [The 10x Chromium scATAC-seq Analysis Workflow](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#The_10x_Chromium_scATAC-seq_Analysis_Workflow)

- [Setting Up Your scATAC-seq Analysis Environment](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Setting_Up_Your_scATAC-seq_Analysis_Environment)

  - [Understanding the Two Kinds of Tools You Need](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Understanding_the_Two_Kinds_of_Tools_You_Need)
  - [Installing Cell Ranger ATAC 2.2.0](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Installing_Cell_Ranger_ATAC_220)
  - [Creating the Pixi Environment for Open-Source Tools](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Creating_the_Pixi_Environment_for_Open-Source_Tools)
  - [Downloading the Prebuilt Reference Genome](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Downloading_the_Prebuilt_Reference_Genome)

- [The Example Dataset: A Real COVID-19 Case/Control Cohort (GSE282769)](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#The_Example_Dataset_A_Real_COVID-19_CaseControl_Cohort_GSE282769)

  - [Why scATAC-seq Has R1, R2, and R3 (Not Just R1 and R2)](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Why_scATAC-seq_Has_R1_R2_and_R3_Not_Just_R1_and_R2)
  - [Selecting Your Samples in the SRA Run Selector](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Selecting_Your_Samples_in_the_SRA_Run_Selector)
  - [Downloading One Sample With sra-tools](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Downloading_One_Sample_With_sra-tools)
  - [Identifying and Renaming the Reads for Cell Ranger](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Identifying_and_Renaming_the_Reads_for_Cell_Ranger)
  - [Preparing the Other Samples](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Preparing_the_Other_Samples)

- [Read Alignment and Processing With cellranger-atac count](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Read_Alignment_and_Processing_With_cellranger-atac_count)

  - [Running the Pipeline](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Running_the_Pipeline)
  - [Processing the Remaining Samples](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Processing_the_Remaining_Samples)

- [Understanding Cell Ranger ATAC Input and Output Files](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Understanding_Cell_Ranger_ATAC_Input_and_Output_Files)

  - [The Inputs, Recapped](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#The_Inputs_Recapped)
  - [The Key Outputs](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#The_Key_Outputs)
  - [The Fragments File: The Heart of scATAC Analysis](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#The_Fragments_File_The_Heart_of_scATAC_Analysis)
  - [Inspecting the Aligned Reads](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Inspecting_the_Aligned_Reads)

- [Peak Calling: Cell Ranger ATAC vs MACS3](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Peak_Calling_Cell_Ranger_ATAC_vs_MACS3)

  - [How Cell Ranger ATAC Calls Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#How_Cell_Ranger_ATAC_Calls_Peaks)
  - [How MACS3 Calls Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#How_MACS3_Calls_Peaks)
  - [Why Use MACS3 When Cell Ranger Already Gives You Peaks?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Why_Use_MACS3_When_Cell_Ranger_Already_Gives_You_Peaks)
  - [Which Peak Caller Should You Use?](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Which_Peak_Caller_Should_You_Use)

- [Best Practices for scATAC-seq Preprocessing](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Best_Practices_for_scATAC-seq_Preprocessing)
- [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Common_Pitfalls_and_How_to_Avoid_Them)
- [Troubleshooting Common Errors](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Troubleshooting_Common_Errors)
- [Conclusion: From Raw Reads to Analysis-Ready Peaks](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Conclusion_From_Raw_Reads_to_Analysis-Ready_Peaks)
- [Moving Forward](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Moving_Forward)
- [References](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-atac-seq-data-a-complete-beginners-guide-part-1-from-fastq-to-peaks/#Like_this)

## Introduction: Understanding Single-Cell Chromatin Accessibility

### What Is scATAC-seq and How Is It Different From Bulk ATAC-seq?

ATAC-seq stands for **Assay for Transposase-Accessible Chromatin using sequencing**. The idea is elegant. Inside every cell, DNA is wrapped around histone proteins and packaged into chromatin. Regions that the cell is actively using — promoters, enhancers, and other regulatory elements — are held in an “open” state so that proteins can reach the DNA. Regions the cell has switched off are packed away tightly and are “closed.” ATAC-seq uses a hyperactive enzyme called **Tn5 transposase** that can only cut and tag DNA where the chromatin is open. Sequence those tagged fragments, map them back to the genome, and the places where reads pile up mark the accessible, regulatory regions of the genome.

**Bulk ATAC-seq** applies this to a population of thousands or millions of cells at once. The result is a single, averaged accessibility profile: it tells you which regions are open *on average* across the whole sample. That is powerful, but it hides heterogeneity. If 10% of your cells are a rare cell type with a distinctive regulatory program, their signal is diluted into the bulk average and effectively invisible. If you are new to accessibility data entirely, our [bulk ATAC-seq beginner’s guide](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/) walks through the FASTQ-to-peaks workflow for a population sample and is a gentle place to build intuition before adding the single-cell layer.

**Single-cell ATAC-seq (scATAC-seq)** measures accessibility in each individual cell. Instead of one averaged profile, you get thousands of per-cell profiles, which lets you discover cell types, identify the regulatory elements specific to each of them, and study how chromatin state changes across a population — all without knowing the cell types in advance.

The trade-off is data sparsity. A diploid cell has only two copies of each genomic locus, so at any single position a cell can contribute at most two fragments. Combined with limited capture efficiency, this means each cell yields only a few thousand to a few tens of thousands of fragments spread across a 3-billion-base genome. Any individual accessible region is either detected or not in a given cell — the data is essentially binary and extremely sparse. This sparsity is the single most important thing to understand about scATAC-seq, and it shapes every downstream method, from peak calling to dimensionality reduction.

| Feature | Bulk ATAC-seq | Single-cell ATAC-seq |
| --- | --- | --- |
| Resolution | One averaged profile per sample | One profile per cell |
| Detects cell heterogeneity | No | Yes |
| Signal per unit | Deep, quantitative coverage | Sparse, near-binary per cell |
| Rare populations | Diluted into the average | Recoverable as clusters |
| Typical use | Comparing conditions in a known population | Discovering and characterizing cell states |

### scATAC-seq or snATAC-seq? A Note on Terminology

You will see two closely related terms in the literature: **scATAC-seq** (single-cell) and **snATAC-seq** (single-nucleus). The difference is only what gets loaded onto the instrument — intact cells, or nuclei isolated from them beforehand. Since chromatin lives in the nucleus, both measure exactly the same thing.

This matters for one practical reason: the **10x Genomics Chromium “Single Cell ATAC” assay operates on isolated nuclei.** Its standard protocol begins with a nucleus-isolation step, so the workflow in this series is technically single-nucleus, despite the product name. That is why Cell Ranger reports “nuclei” rather than “cells” in its metrics, and it is also why the assay works on frozen and archived tissue, where intact viable cells cannot be recovered. Following common usage, we write “scATAC-seq” throughout.

### Current scATAC-seq Technologies and Their Use Cases

Several platforms can generate single-cell chromatin accessibility data, and they differ mainly in how they attach a unique barcode to each cell or nucleus:

- **Droplet-based (10x Genomics Chromium).** Each nucleus is co-encapsulated in an oil droplet with a barcoded gel bead. High throughput (thousands to tens of thousands of nuclei), a turnkey commercial kit, and a supported analysis pipeline. This is the dominant platform and the focus of this series.

- **Combinatorial indexing (sci-ATAC-seq, dscATAC-seq).** Barcodes are added in successive rounds of split-pool labeling, so no physical isolation of single cells is needed. Very high throughput and low cost per cell, but more hands-on and less standardized.

- **Plate-based (e.g., Pi-ATAC).** Individual cells are sorted into wells. Lower throughput but allows pairing accessibility with indexed sorting information (such as surface markers).

- **Multiome (10x Genomics Chromium Single Cell Multiome ATAC + Gene Expression).** Measures chromatin accessibility and gene expression in the *same* nucleus. Ideal when directly linking regulation to expression is the goal, at higher cost and complexity.

For most researchers starting out — especially those coming from a wet-lab background — droplet-based 10x Chromium offers the best balance of throughput, reliability, and analytical support.

### Why 10x Genomics Chromium Dominates scATAC-seq

Three factors explain why 10x Chromium is the most widely used scATAC-seq platform:

1. **A turnkey commercial workflow.** The kit, instrument, and protocols are standardized and quality-controlled, which removes much of the trial-and-error that custom methods require.

2. **A supported, integrated software pipeline.** Cell Ranger ATAC takes raw sequencing reads to analysis-ready outputs with a single command, and it is actively maintained and documented by 10x Genomics.

3. **A large ecosystem and community.** Because so many labs use it, public datasets, tutorials, and downstream tools (Signac, ArchR) are built and tested around the 10x output format first. When you hit a problem, someone has almost certainly hit it before you.

Standardization also means reproducibility: a 10x scATAC experiment run in one lab produces output in the same format as one run in another, which is exactly what a beginner-friendly, transferable workflow needs.

### What Biological Insights Come From scATAC-seq?

Where scRNA-seq tells you *which genes a cell is expressing*, scATAC-seq tells you *which regulatory switches a cell has turned on*. That opens up questions expression data alone cannot answer:

- **Mapping regulatory elements.** Identify active promoters and, crucially, distal **enhancers** — the regulatory regions that turn genes on and off — in a cell-type-specific way.

- **Inferring transcription factor (TF) activity.** Accessible regions are enriched for the DNA motifs of the TFs binding there. Motif enrichment and footprinting reveal which TFs are active in each cell type, often before their target genes change expression.

- **Defining cell identity from the epigenome.** Cell types cluster cleanly by accessibility, and the accessible regions themselves point to the regulatory logic that defines each identity.

- **Interpreting disease-associated variants.** Many GWAS risk variants fall in non-coding regions. Overlapping them with cell-type-specific accessible regions can pinpoint which cell type and which regulatory element a variant likely affects.

- **Tracing regulatory dynamics.** Along a differentiation trajectory, enhancers often open *before* their target genes are expressed, so accessibility can reveal the regulatory events that precede and drive changes in cell state.

### scATAC-seq vs scRNA-seq: When Do You Need Which — or Both?

These two assays answer different, complementary questions.

| Question | Best assay |
| --- | --- |
| What cell types are present, and what are they expressing? | scRNA-seq |
| Which regulatory elements and TFs define each cell type? | scATAC-seq |
| Which enhancer controls which gene, in which cell? | Multiome (both, same nucleus) |
| What drives a cell-fate decision before expression changes? | scATAC-seq (or multiome) |

A practical rule of thumb: reach for **scRNA-seq** when you want to catalog cell types and their expressed programs; reach for **scATAC-seq** when you want the regulatory mechanism underneath those programs; and reach for the **multiome** assay when the core of your question is linking a specific regulatory element to a specific gene in a specific cell. Many modern studies integrate separately generated scRNA-seq and scATAC-seq datasets computationally — a topic we will cover later in this series, and the reason we deliberately use PBMCs in both series.

### The 10x Chromium scATAC-seq Analysis Workflow

Here is the road map for the whole series. This tutorial (Part 1) covers the first block, from raw reads to peaks.

1. **Raw processing (this tutorial):** align reads, correct barcodes, build the fragments file, call peaks, and identify which barcodes are real cells. Handled by Cell Ranger ATAC.

2. **Quality control:** filter low-quality nuclei and doublets using metrics such as fragment counts, TSS enrichment, and nucleosome banding.

3. **Dimensionality reduction and clustering:** normalize the sparse peak matrix (TF-IDF), reduce dimensions (latent semantic indexing, LSI), and cluster.

4. **Cell type annotation:** summarize accessibility into per-gene “gene activity” scores and label clusters, often by integrating with reference scRNA-seq.

5. **Downstream biology:** cell-type-specific peak recalling, motif enrichment and TF footprinting, peak-to-gene links, and integration with expression data.

With the destination in view, let’s build the environment.

---

## Setting Up Your scATAC-seq Analysis Environment

### Understanding the Two Kinds of Tools You Need

A quick but important distinction, because it determines how we install things:

- **Cell Ranger ATAC** is 10x Genomics’ proprietary pipeline. It is **not** distributed through Conda, Bioconda, or Pixi — you download it as a self-contained bundle directly from 10x Genomics after accepting their license. It ships with all of its own dependencies, so there is nothing to compile.

- **The open-source tools** we use around it (MACS3 for alternative peak calling, plus a few utilities for inspecting and manipulating the outputs) are all on Bioconda, so we manage them cleanly with **Pixi** in a project environment. This is the same environment we will keep extending in later parts of the series (adding Signac, ArchR, and the other downstream analysis packages), so create it once and reuse it.

We will install Cell Ranger ATAC manually first, then build the Pixi environment for everything else.

### Installing Cell Ranger ATAC 2.2.0

Go to the official download page and accept the license to reveal your download command:

[https://www.10xgenomics.com/support/software/cell-ranger-atac/downloads#download-links](https://www.10xgenomics.com/support/software/cell-ranger-atac/downloads#download-links)

The page gives you a `curl` command with a **signed, time-limited URL** (it contains an `Expires=` parameter and expires after a short window). Because that signature is personal and expires, always copy the *current* command from the page rather than reusing an old link. It will look like the command below — paste your own version, do not copy this one verbatim:

```bash
# Move to a location with plenty of space, then paste the signed curl
# command from the 10x download page.
cd /opt
curl -o cellranger-atac-2.2.0.tar.gz "paste the cellranger-atac-2.2.0 download link here"

# Unpack the bundle (creates the directory cellranger-atac-2.2.0)
tar -xzvf cellranger-atac-2.2.0.tar.gz
```

Add the tool to your `PATH` so you can call it by name. To make this permanent, append the same `export` line to your `~/.bashrc`:

```bash
# Make cellranger-atac callable from any directory
export PATH=/opt/cellranger-atac-2.2.0:$PATH
```

Verify the installation and confirm the version:

```
which cellranger-atac
cellranger-atac --version
```

Cell Ranger ATAC also ships a small built-in test dataset. Running it end to end is the fastest way to confirm your installation actually works before committing to a multi-hour real run:

```bash
# Runs the full pipeline on a tiny bundled dataset (a few minutes)
cellranger-atac testrun --id=tiny
```

A message reporting that the pipestance completed successfully means your installation is sound.

> 

**Note on running location:** Cell Ranger ATAC needs a 64-bit Linux system (CentOS/RedHat 8.0+ or Ubuntu 18.04+). It is resource-hungry, so plan to run the alignment step on an HPC cluster or a well-provisioned workstation, not a laptop. See our [HPC and SLURM beginner’s guide](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/) if the cluster environment is new to you.

### Creating the Pixi Environment for Open-Source Tools

[Pixi](https://prefix.dev/) is a modern, fast, reproducible package manager built on the Conda ecosystem. It records the exact versions of every tool in a project file, so your environment is reproducible on any machine — which is exactly what beginners (and reviewers) want. If Pixi is new to you, our [Pixi setup guide](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/) walks through installation.

Create a dedicated project and add the tools this tutorial actually uses:

```bash
# Create a Pixi project folder and move into it
pixi init scatac-analysis
cd scatac-analysis

# Add the Bioconda and conda-forge channels this project draws from
pixi workspace channel add conda-forge bioconda

# Install only the tools Part 1 needs:
#   sra-tools -- download raw FASTQs from the SRA (prefetch, fasterq-dump)
#   pigz      -- parallel gzip, much faster than gzip on large FASTQ files
#   macs3     -- alternative, community-standard peak caller
#   samtools  -- inspect and index the aligned BAM
#   bedtools  -- manipulate and compare peak (BED) files
#   htslib    -- provides bgzip and tabix for the fragments file
pixi add sra-tools pigz macs3 samtools bedtools htslib
```

Every tool now lives inside this project rather than being installed system-wide, which is what makes the setup reproducible. That also means the tools are not on your `PATH` by default — you have to activate the environment first. There are two ways to do that:

```bash
# Option A: activate once, then use the tools normally for the rest of the session
pixi shell
macs3 --version
samtools --version
prefetch --version

# Option B: run a single command through Pixi without activating
pixi run macs3 --version
```

**Convention used in this tutorial:** we activate once with `pixi shell` and then call the tools by name. Every command in the sections that follow — `prefetch`, `fasterq-dump`, `pigz`, `macs3`, `samtools`, `bedtools`, `tabix` — assumes you are inside that activated shell. If you open a new terminal, `cd` back to the project and run `pixi shell` again.

The one exception is **`cellranger-atac`**, which is not a Pixi package. It was installed separately and is available through the `PATH` line you exported earlier, so it works either inside or outside the Pixi shell.

> 

**Beginner’s Tip:** If a command returns `command not found`, the usual cause is a shell that is not activated. Run `pixi shell` from the project directory and try again.

We will keep adding to this same environment in later parts (Signac, ArchR, and so on), so there is no need to create a new one each time.

### Downloading the Prebuilt Reference Genome

Cell Ranger ATAC aligns reads against a reference package. Conveniently, it uses the **same reference packages as Cell Ranger ARC**, and 10x provides prebuilt human, mouse, and rat references. Because our dataset is human PBMCs, we use the GRCh38 reference.

Copy the download link from the [Cell Ranger ATAC downloads page](https://www.10xgenomics.com/support/software/cell-ranger-atac/downloads#download-links) and unpack it into a shared reference directory:

```bash
# Download the prebuilt human GRCh38 reference (paste the link from the downloads page)
cd /projects/mylab/shared/reference
curl -O "<paste the GRCh38 2024-A reference link here>"

# Unpack it (creates refdata-cellranger-arc-GRCh38-2024-A/)
tar -xzvf refdata-cellranger-arc-GRCh38-2024-A.tar.gz
```

We use the **2024-A** human reference throughout this series. Mouse and rat equivalents are on the same page. The rule that matters: use the **same reference version for every sample**, so peaks and matrices stay directly comparable across the cohort.

> 

**Beginner’s Tip:** Store large, rarely-changing files — Cell Ranger ATAC itself and the reference — on persistent project storage, not on scratch space that gets purged. You will reuse them across every run in this series, so download them once and keep them.

---

## The Example Dataset: A Real COVID-19 Case/Control Cohort (GSE282769)

Consistent with NGS101’s practical, real-data approach, this series is built on a real patient cohort rather than a demonstration sample. The data come from a study of chromatin remodeling in severe Delta COVID-19, which profiled the accessibility landscape of roughly 140,000 PBMC nuclei from healthy individuals and COVID-19 patients across severity groups using 10x Chromium scATAC-seq.

Why this dataset anchors the whole series:

- **A real case/control design with biological replicates** — the prerequisite for the differential accessibility analysis we build toward in later parts. (A single sample, however clean, cannot support condition-level comparison.)

- **PBMCs**, keeping the immune-cell focus that parallels our scRNA-seq series.

- **Matched scRNA-seq from the same study** (SRA BioProject PRJNA1164162), which sets up cross-modal integration in a later tutorial.

- **Publicly available raw reads on the SRA**, so we can start from FASTQ exactly as the rest of the series does.

**Data locations:**

- scATAC-seq raw reads (used here): SRA BioProject [PRJNA1190389](https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA1190389)

- Matched scRNA-seq raw reads: SRA BioProject PRJNA1164162

- Processed data and study record: GEO GSE282769

**Our working subset.** The full study spans several severity groups. To keep downloads and compute tractable, we use four samples: **two healthy controls and two severe/critical COVID-19 patients**. That balanced 2-vs-2 design demonstrates every step of the workflow, including a group comparison later in the series. If you have the storage and compute, the same commands extend to more samples by adding accessions — the study contains twelve healthy and five severe/critical runs, and more replicates give more reliable differential results.

In this tutorial we walk through **one sample end to end**, from FASTQ to peaks. The other three samples are processed with the identical commands — just repeat the same steps for each, changing only the accession and the sample name.

### Why scATAC-seq Has R1, R2, and R3 (Not Just R1 and R2)

Before downloading, understand the 10x scATAC read structure — it differs from scRNA-seq, and it is exactly what lets us sort out the SRA files in a moment.

In 10x scRNA-seq, each captured molecule yields **two reads plus a sample index**: Read 1 carries the 16 bp cell barcode and a UMI — no biological sequence at all — while Read 2 carries the transcript. Two reads suffice because gene expression needs only three facts: which cell, which gene, and how many distinct molecules.

scATAC-seq measures something different, and the read layout follows from that. A fragment is defined by *both* of its genomic coordinates, because the interval between the two Tn5 cuts is the accessible region itself. One end cannot describe an interval, so both ends must be sequenced. That uses up two long reads, and the 16 bp cell barcode is pushed onto a read of its own.

There is also no UMI in ATAC, and that is deliberate rather than an omission. Two fragments from the same cell with identical start and end coordinates must be PCR duplicates of a single insertion event, so the coordinates *are* the molecular identifier. This is why the fragments file we meet later carries a read-support count instead of a UMI count.

> 

**Important — the read names do not mean the same thing in both assays.** “R2” is the transcript in scRNA-seq but the cell barcode in scATAC-seq. If you are coming from scRNA-seq, do not carry that intuition over; it is the single most common source of confusion here.

Side by side, with our assay first:

`R1``R2``R3``I1`

| Read | In 10x scATAC-seq (this tutorial) | Role in scATAC-seq | In 10x scRNA-seq (for contrast) |
| --- | --- | --- | --- |
|  | ~50 bp genomic DNA | One end of the accessible fragment | 16 bp cell barcode + UMI |
|  | 16 bp cell barcode | Identifies which nucleus the fragment came from | ~90 bp transcript |
|  | ~50 bp genomic DNA | The other end of the fragment | does not exist — scRNA-seq has no R3 |
|  | 8 bp sample index | Demultiplexes samples on the sequencer | 8 bp sample index |

**All three of R1, R2, and R3 are used, and all three are required** by `cellranger-atac count`. Only `I1` is optional, because our FASTQs arrive from the SRA already demultiplexed.

The exact genomic read lengths vary a little between runs (in this study, mostly 50 bp, with some runs at 49 bp) — what matters is that they are *long* relative to the 16 bp barcode and 8 bp index.

The key facts to carry forward: **the cell barcode lives in the 16 bp read (R2)**, R1 and R3 are the two genomic ends, and the 8 bp read is the sample index. When we pull files off the SRA, these lengths are how we identify the barcode and index; the two equal-length genomic reads are then told apart by file order.

### Selecting Your Samples in the SRA Run Selector

Open the Run Selector for the scATAC project:

[https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA1190389](https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA1190389)

The Run Selector lists every sequencing run with its metadata. Two buttons matter:

- **Metadata** downloads `SraRunTable.csv` — one row per run, with columns describing each sample (disease state, severity group, assay type, and so on).

- **Accession List** downloads `SraAccList.csv` — a plain list of run accessions (SRR IDs).

Open `SraRunTable.csv` in a spreadsheet and inspect the `disease_state` column. This study contains four groups (Healthy, Mild/Moderate, Reconvalescence, and Severe/Critical), and all runs are ATAC-seq on PBMCs. We take two Severe/Critical and two Healthy samples, matched for sex (one male and one female per group) and comparable in age, since age and sex both shape immune-cell chromatin. The four samples used throughout the series are:

| sample_id | Run (SRR) | Group | GEO sample | Age | Sex |
| --- | --- | --- | --- | --- | --- |
| severe1 | SRR31499777 | Severe | GSM8650274 | 47 | M |
| severe2 | SRR31499804 | Severe | GSM8650278 | 47 | F |
| healthy1 | SRR31499787 | Healthy | GSM8650264 | 45 | M |
| healthy2 | SRR31499788 | Healthy | GSM8650263 | 28 | F |

Keep this mapping as your sample sheet — the `sample_id` names become the identifiers used throughout the series, and the `Group` column drives the differential comparison later.

> 

**Note on depth:** The Healthy libraries in this cohort are deeper (~55 Gb) than the Severe/Critical ones (~26–31 Gb). That is inherent to the dataset. Per-cell and pseudobulk normalization handle depth differences, and you can downsample to a common depth before differential testing if you want to be strict; we flag it here so it is a known quantity rather than a surprise later.

### Downloading One Sample With sra-tools

We walk through a single sample in full — `severe1` (SRR31499777). Downloading is a two-step process: `prefetch` fetches the compressed `.sra` archive, then `fasterq-dump` converts it to FASTQ.

```bash
# Activate the Pixi environment so sra-tools and pigz are available
cd /projects/mylab/shared/scatac-analysis
pixi shell

cd /projects/mylab/shared/scatac_data

# Step 1: download the .sra archive
#   --max-size raises the default 20 GB cap, which scATAC runs can exceed
prefetch --max-size 100G SRR31499777

# Step 2: convert to FASTQ
#   --split-files       write each read in the spot to its own file
#   --include-technical KEEP the barcode read (see the critical note below)
#   --threads           parallel conversion
fasterq-dump --split-files --include-technical --threads 16 SRR31499777

# Step 3: compress with pigz (parallel gzip -- far faster than gzip at this size)
pigz -p 16 SRR31499777_*.fastq
```

> 

**Critical — never omit `--include-technical`.** By default, `fasterq-dump` treats the 16 bp cell-barcode read as a “technical” read and silently drops it. Without the barcode read, Cell Ranger ATAC cannot assign fragments to nuclei and the entire run is useless. Always include this flag for 10x data.

> 

**Plan your disk space.** As a rule of thumb from the SRA Toolkit documentation, extracted FASTQs are roughly seven times the size of the `.sra` archive, and `fasterq-dump` needs about 1.5 times that again as scratch during conversion — so budget on the order of seventeen times the archive size while the conversion runs. `prefetch` also caches the `.sra` file, which you can delete once the FASTQs are verified.

### Identifying and Renaming the Reads for Cell Ranger

Here is the second SRA gotcha: the numbered files `fasterq-dump` produces (`_1`, `_2`, `_3`, `_4`) do **not** map to R1/R2/R3/I1 in a fixed order. The mapping depends on how the original submitter uploaded the data, so never assume `_1` is R1. Measure the reads instead.

Check the sequence length of the first read in each file:

```bash
# Print the length of the first read's sequence (line 2) in each FASTQ
for f in SRR31499777_*.fastq.gz; do
  echo -n "$f: "
  zcat "$f" | head -2 | tail -1 | awk '{print length($0)}'
done
```

For this study the four files measure **8 bp, 50 bp, 16 bp, and 50 bp**. Two of them are identified by length alone:

- the **16 bp** file is the cell barcode -> `R2`

- the **8 bp** file is the sample index -> `I1`

The two remaining ~50 bp files are the genomic ends, `R1` and `R3`. Because they are the *same length*, length cannot tell them apart — so keep them in file order: the lower-numbered file becomes `R1`, the higher-numbered becomes `R3`. (These are the two ends of one fragment, so this is a matter of consistency rather than biology, but keep it stable across samples.)

Applying that rule to these runs gives:

`fasterq-dump``_1``I1``_2``R1``_3``R2``_4``R3`

| File from | Length | Cell Ranger name | Contents |
| --- | --- | --- | --- |
|  | 8 bp |  | Sample index |
|  | 50 bp |  | Genomic end (first of the pair) |
|  | 16 bp |  | Cell barcode |
|  | 50 bp |  | Genomic end (second of the pair) |

Note that `_1` is the **index**, not `R1`. Naively mapping `_1` to `R1` would hand Cell Ranger an 8 bp index read as genomic sequence, which is exactly the mistake that makes alignment fail. The metadata corroborates the layout: 8 + 50 + 16 + 50 = 124, matching the reported average spot length of 124 bp (runs reporting 123 bp simply have a 49 bp second genomic read).

**Do not hardcode this mapping for other datasets.** The file order reflects how the submitter uploaded the data and can differ between studies, and occasionally between runs in the same study. Always measure, then assign.

Now rename each file to the Cell Ranger convention `<sample>_S1_L001_<read>_001.fastq.gz`, placing them in a per-sample folder. For the layout above (`_1`=I1, `_2`=R1, `_3`=R2, `_4`=R3), that is:

```
mkdir -p severe1_fastqs
mv SRR31499777_2.fastq.gz severe1_fastqs/severe1_S1_L001_R1_001.fastq.gz  # 50 bp genomic
mv SRR31499777_3.fastq.gz severe1_fastqs/severe1_S1_L001_R2_001.fastq.gz  # 16 bp barcode
mv SRR31499777_4.fastq.gz severe1_fastqs/severe1_S1_L001_R3_001.fastq.gz  # 50 bp genomic
mv SRR31499777_1.fastq.gz severe1_fastqs/severe1_S1_L001_I1_001.fastq.gz  # 8 bp index
```

**Match the file numbers to your own length check — do not copy the numbers above blindly.** If a different run puts the reads in a different order, adjust the `mv` commands so that the 16 bp file becomes R2, the 8 bp file becomes I1, and the two ~50 bp files become R1 and R3.

Confirm the result before going further. R1 and R3 must both be long genomic reads (about 50 bp) and R2 must be exactly 16 bp:

```bash
# Verify the renamed files
for f in severe1_fastqs/*.fastq.gz; do
  echo "$(basename "$f") = $(zcat "$f" | head -2 | tail -1 | awk '{print length($0)}') bp"
done
```

```
severe1_S1_L001_I1_001.fastq.gz = 8 bp
severe1_S1_L001_R1_001.fastq.gz = 50 bp
severe1_S1_L001_R2_001.fastq.gz = 16 bp
severe1_S1_L001_R3_001.fastq.gz = 50 bp
```

If R1 or R3 shows 8 or 16 bp, the assignment is wrong — a short read has been handed to Cell Ranger as genomic sequence, and alignment will fail. Fix it before spending hours on the run.

R1, R2, and R3 are required by `cellranger-atac count`; the I1 index file is optional.

> 

**If the SRA download looks wrong (a real-world caveat):** Some 10x submissions on the SRA are incomplete — occasionally a run yields only two files, or read lengths that do not match the table above. This usually means the data were uploaded in a non-standard way. Two reliable fallbacks: download the FASTQs directly from the **ENA** (which often stores them pre-split with the correct R1/R2/R3 structure), or, if the submitter uploaded a Cell Ranger BAM, regenerate exact FASTQs with the official 10x `bamtofastq` tool. Verifying the read lengths before committing to a multi-hour alignment saves a great deal of wasted compute.

### Preparing the Other Samples

Download and prepare the remaining three samples the same way — run the same commands (`prefetch`, `fasterq-dump`, `pigz`), then check read lengths and rename, changing only the accession and the sample name each time. Remember to re-check the lengths for each sample rather than assuming the file order carries over.

---

## Read Alignment and Processing With cellranger-atac count

The `cellranger-atac count` command is the workhorse of the whole pipeline. From your raw FASTQs it performs, in one automated run: barcode correction (matching each R2 to the known 10x barcode whitelist), read alignment to the reference with BWA, duplicate marking, generation of the fragments file, peak calling, cell calling (separating real nuclei from empty droplets), and construction of the peak-by-cell matrix.

### Running the Pipeline

```bash
# Run the full scATAC pipeline on our sample.
#   --id         name for the output folder
#   --reference  the prebuilt GRCh38 reference we unpacked earlier
#   --fastqs     folder containing the R1/R2/R3/I1 FASTQ files
#   --sample     the FASTQ filename prefix (before _S1_L001_...)
#   --localcores/--localmem  cap CPU and RAM for a single machine
cellranger-atac count \
  --id=severe1 \
  --reference=/projects/mylab/shared/reference/refdata-cellranger-arc-GRCh38-2024-A \
  --fastqs=/projects/mylab/shared/scatac_data/severe1_fastqs \
  --sample=severe1 \
  --localcores=16 \
  --localmem=64
```

The `--sample` value must exactly match the prefix you gave the FASTQ files — everything before the `_S1_L00...` part (here, `severe1`). This is how Cell Ranger knows which files belong to this sample.

Note that `cellranger-atac` is the one tool here that does *not* come from Pixi — it resolves through the `PATH` you exported during installation, so this step works whether or not the Pixi shell is active.

> 

**Beginner’s Tip:** This run is computationally heavy. On 16 cores with 64 GB of RAM it typically takes several hours per sample. Do not be alarmed by the long runtime or the large temporary files — that is normal.

> 

**If you are re-running after a failure:** Cell Ranger caches completed stages and will *resume* an existing `--id` directory rather than start over. If you have fixed the inputs, that cached state is stale — the pipeline can re-report the original error within seconds without ever reading your corrected files. Always `rm -rf <id>` (the output directory, not the FASTQ folder) or choose a new `--id` before re-running.

### Processing the Remaining Samples

Run the remaining samples the same way — only `--id`, `--fastqs`, and `--sample` change. Each run takes several hours, so on a shared cluster it is worth submitting them as separate batch jobs rather than back to back; see our [HPC and SLURM beginner’s guide](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/) for how to script that on your system.

> 

**Note:** The older `cellranger-atac mkfastq` step (converting raw BCL files to FASTQ) is deprecated; 10x now recommends Illumina’s BCL Convert for that conversion. Because we download FASTQs directly, we skip it entirely.

---

## Understanding Cell Ranger ATAC Input and Output Files

When the run finishes, your results live in `severe1/outs/`. Knowing what each file is — and which ones matter for downstream analysis — saves enormous confusion later.

### The Inputs, Recapped

Cell Ranger ATAC takes two inputs: the **FASTQ files** (R1, R2, R3, I1, described above) and the **reference package** (the GRCh38 folder). That is all.

### The Key Outputs

```bash
# List the primary outputs
ls severe1/outs/
```

A complete run produces the files and folders below. They fall into a few natural groups — quality-control reports, the fragment and peak files, the count matrices, and the alignment and browser tracks. Every item you will see is described here.

**Quality-control and summary reports**

`web_summary.html``summary.csv``summary.json``singlecell.csv``is_cell_barcode`

| Output | What it is | Why it matters |
| --- | --- | --- |
|  | Interactive HTML QC report | Your first stop — open it in a browser to judge run quality (cells called, median fragments, TSS enrichment, fraction of fragments in peaks) |
|  | The headline metrics as a flat table | Machine-readable QC, easy to collate across samples |
|  | The same metrics in JSON | Same numbers as the CSV, convenient for scripting |
|  | Per-barcode metrics (total, duplicate, mitochondrial, peak-region fragments, , and more) | Drives the quality-control filtering in Part 2 |

**Fragments and peaks**

`fragments.tsv.gz``.tbi``peaks.bed``peak_annotation.tsv``peak_motif_mapping.bed`

| Output | What it is | Why it matters |
| --- | --- | --- |
| (+ ) | Every unique fragment: chromosome, start, end, cell barcode, read support — with its tabix index | The single most important output; feeds nearly all downstream tools |
|  | All accessible regions called across the sample | Defines the “features” of scATAC data (the role genes play in scRNA-seq) |
|  | Each peak annotated to nearby genes, based on distance to transcription start sites | Links regulatory regions to the genes they may control |
|  | Which transcription-factor motifs match which peaks | The basis for the TF matrix below and for motif analysis later |

**Count matrices**

Cell Ranger writes each matrix twice: a `.h5` file (a single compact HDF5 file) and a directory of the same name (the Matrix Market “MEX” format — three plain files: barcodes, features, and the sparse matrix). They hold identical data; use whichever your downstream tool prefers.

`filtered_peak_bc_matrix/``filtered_peak_bc_matrix.h5`**called cells only**`raw_peak_bc_matrix/``raw_peak_bc_matrix.h5`**all barcodes**`filtered_tf_bc_matrix/``filtered_tf_bc_matrix.h5`

| Output | What it is | Why it matters |
| --- | --- | --- |
| and | Peak-by-barcode counts for | The main matrix for clustering and analysis |
| and | Peak-by-barcode counts for , cells and empty droplets alike | Needed for custom cell-calling or ambient-signal analysis |
| and | Transcription-factor-by-cell counts, built by pooling cut sites across peaks that share each TF’s motif | A ready-made view of per-cell TF motif activity |

**Alignment, tracks, and interactive exploration**

`possorted_bam.bam``.bai``cut_sites.bigwig``cloupe.cloupe`

| Output | What it is | Why it matters |
| --- | --- | --- |
| (+ ) | Position-sorted aligned reads, with index | For custom re-analysis or viewing pileups in a genome browser |
|  | A smoothed track of Tn5 transposition sites across the genome (counts summed in a +/- 200 bp window) | The signal Cell Ranger thresholds for peak calling; load it in IGV to see accessibility as a coverage track |
|  | Loupe Browser file | Point-and-click exploration of clusters and peaks, no coding required |

**analysis/**

`analysis/`

| Output | What it is | Why it matters |
| --- | --- | --- |
|  | Cell Ranger’s own secondary analysis: dimensionality reduction, clustering, and per-cluster differential accessibility and TF enrichment | A quick first look; in this series we redo these steps ourselves for full control |

### The Fragments File: The Heart of scATAC Analysis

Almost everything downstream — QC, peak recalling, coverage tracks, gene activity — is computed from `fragments.tsv.gz`. It is worth looking at directly. Each line is one fragment:

```bash
# Peek at the first few fragments (skipping the header comment lines)
zcat severe1/outs/fragments.tsv.gz | grep -v "^#" | head
```

**Expected output:**

```
chr1	10085	10474	CCACAGGTCCGTTAGA-1	1	+
chr1	10091	10285	CAACGTAGTAATGCCT-1	1	-
chr1	10091	10345	CGCAATATCACTGGTA-1	1	+
chr1	10097	10345	TAAGCCAGTTTCCGGG-1	1	+
chr1	10151	10179	CGCTAGGTCAATTGGC-1	1	+
chr1	10151	10179	TTCGGTCAGAGGCCTA-1	1	+
```

The columns are: chromosome, start position, end position, the cell barcode the fragment belongs to, and the number of reads supporting that fragment (PCR duplicates are already collapsed). As of Cell Ranger ATAC 2.2, strand information inferred from the adapter sequences is also included, which improves downstream doublet and contamination filters.

The file is block-compressed with `bgzip` and indexed with `tabix` (the `.tbi` file), which lets tools jump instantly to any genomic region without reading the whole file. If you ever move or regenerate a fragments file, remember to re-create its index:

```bash
# Rebuild the tabix index if it is missing (tabix comes from htslib in the Pixi env)
tabix -p bed severe1/outs/fragments.tsv.gz
```

### Inspecting the Aligned Reads

You can sanity-check the alignment with `samtools`:

```bash
# View the BAM header and count aligned records
samtools view -H severe1/outs/possorted_bam.bam | head
samtools flagstat severe1/outs/possorted_bam.bam
```

**Expected output:**

```bash
# BAM header
@HD	VN:cellranger-atac-2.2.0	SO:coordinate
@SQ	SN:chr1	LN:248956422
@SQ	SN:chr10	LN:133797422
@SQ	SN:chr11	LN:135086622
@SQ	SN:chr12	LN:133275309
@SQ	SN:chr13	LN:114364328
@SQ	SN:chr14	LN:107043718
@SQ	SN:chr15	LN:101991189
@SQ	SN:chr16	LN:90338345
@SQ	SN:chr17	LN:83257441

# flagstat
418751216 + 0 in total (QC-passed reads + QC-failed reads)
418751216 + 0 primary
0 + 0 secondary
0 + 0 supplementary
130352440 + 0 duplicates
130352440 + 0 primary duplicates
415378768 + 0 mapped (99.19% : N/A)
415378768 + 0 primary mapped (99.19% : N/A)
418751216 + 0 paired in sequencing
209375608 + 0 read1
209375608 + 0 read2
412744348 + 0 properly paired (98.57% : N/A)
414112806 + 0 with itself and mate mapped
1265962 + 0 singletons (0.30% : N/A)
1167498 + 0 with mate mapped to a different chr
505719 + 0 with mate mapped to a different chr (mapQ>=5)
```

`flagstat` reports the fraction of reads that mapped and were properly paired — a quick, high-level confirmation that alignment worked before you dig into the detailed QC report.

---

## Peak Calling: Cell Ranger ATAC vs MACS3

A **peak** is a genomic region with significantly more accessible-chromatin signal than the background — in other words, a putative regulatory element. Peaks are the “features” of scATAC data, playing the role that genes play in scRNA-seq. There are two common ways to call them, and knowing when to use each is one of the more valuable things a beginner can learn.

### How Cell Ranger ATAC Calls Peaks

Cell Ranger ATAC calls peaks automatically as part of `count`. Since version 2.0 it uses a **wavelet-based peak caller** applied to the aggregate signal — that is, it pools fragments from all barcodes into a single pseudobulk profile and finds regions of enrichment. The result is `peaks.bed`, with peaks that are tightly distributed around ~1 kb in width and reproducible between technical replicates.

```bash
# Inspect the Cell Ranger peaks and count how many were called
grep -v "^#" severe1/outs/peaks.bed | head
grep -v "^#" severe1/outs/peaks.bed | wc -l
```

**Expected output:**

```
chr1	9772	10662
chr1	180602	181521
chr1	191195	192101
chr1	267596	268582

110909
```

This aggregate approach is fast, automatic, and a perfectly good starting point. Its limitation is inherent to pooling: a regulatory element that is open only in a small, rare population contributes little to the aggregate signal and may fall below the detection threshold — its peak gets diluted, just as it would in bulk ATAC-seq.

### How MACS3 Calls Peaks

**MACS3** (Model-based Analysis of ChIP-Seq, version 3) is the community-standard, ENCODE-recommended peak caller, and it is what downstream frameworks like Signac and ArchR use under the hood. It is the same tool used for bulk ATAC-seq peak calling — if you followed our [bulk ATAC-seq guide](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/), you have already met it. MACS3 added a dedicated single-cell fragment parser (`-f FRAG`), so it can read a Cell Ranger fragments file directly. We can call peaks on the whole dataset for comparison:

If you have opened a new terminal since the download step, re-activate the environment first (`cd` to the Pixi project and run `pixi shell`), since `macs3` lives there.

```bash
# Call peaks with MACS3 directly from the fragments file.
#   -f FRAG        read the 10x single-cell fragment format
#   -t             the input fragments file
#   -g hs          human effective genome size
#   -n             output file prefix
#   --outdir       where to write results
#   --keep-dup all fragments are already deduplicated by Cell Ranger
#   -q 0.01        FDR (q-value) cutoff for calling a peak
macs3 callpeak \
  -f FRAG \
  -t severe1/outs/fragments.tsv.gz \
  -g hs \
  -n severe1 \
  --outdir macs3_peaks \
  --keep-dup all \
  -q 0.01
```

MACS3 writes three output files, all prefixed with the name you gave `-n`:

`severe1_peaks.narrowPeak``severe1_peaks.xls``.xls``severe1_summits.bed`

| File | What it is |
| --- | --- |
|  | The main result: one line per peak in ENCODE narrowPeak format (a BED file with extra columns for fold-enrichment, p-value, q-value, and the summit position). This is the file downstream tools read |
|  | The same peaks in a tab-delimited table with a header block recording the exact parameters and read counts used — a human-readable log despite the  name |
|  | The single highest-signal base (the summit) of each peak, useful for motif analysis where the precise center matters |

Compare the MACS3 peak set to the Cell Ranger one:

```bash
# Count MACS3 peaks and find how many overlap the Cell Ranger peaks
wc -l macs3_peaks/severe1/severe1_peaks.narrowPeak
bedtools intersect -u \
  -a macs3_peaks/severe1/severe1_peaks.narrowPeak \
  -b severe1/outs/peaks.bed | wc -l
```

The two callers agree on the strong, common peaks but differ at the margins, which is expected: they use different statistical models and thresholds.

### Why Use MACS3 When Cell Ranger Already Gives You Peaks?

At the whole-sample level, running MACS3 on the aggregate fragments (as we just did) produces a peak set broadly similar to Cell Ranger’s — so on its own, that command is more of a sanity check than a reason to switch tools. Two concrete differences are still worth knowing:

- **Peak shape and width.** Cell Ranger’s peaks are relatively broad. In a published comparison on 10x PBMC data, Cell Ranger peaks had a median width of about 880 bp, versus about 460 bp for MACS3 on the same data. Narrower, better-centered peaks localize the underlying regulatory element more precisely — which matters when you later scan peaks for transcription-factor motifs, since a tighter peak carries less flanking, non-functional sequence.

- **Transparency and control.** MACS3 exposes the statistics (fold-enrichment, p-value, q-value per peak) and every threshold, so you can tune stringency and reason about why a region was or was not called. Cell Ranger’s caller is effectively a fixed black box.

But the decisive reason is neither of those. It is **what you can do with MACS3 that you cannot do with Cell Ranger at all: call peaks separately within each cell type.**

Here is the core problem. Both Cell Ranger and our aggregate MACS3 run pool every nucleus into one pseudobulk profile before calling peaks. Pooling is fine for regulatory elements that are open in common cell types, because those contribute strong signal to the aggregate. But a regulatory element that is open in only a small population — say a rare progenitor or a specific T-cell subset that is 3% of your sample — contributes almost nothing to the pooled signal. Its peak is diluted below the detection threshold and never called, by either tool. The accessible region is real and biologically important, but it is invisible in the aggregate, exactly as a rare-cell signal would be lost in bulk ATAC-seq.

The fix is to call peaks **after** clustering, once per cell group, on that group’s pooled fragments, and then merge the results into a unified peak set. Within the rare population’s own cluster, its regulatory elements are no longer diluted — they are the dominant signal — so they get called. This per-cluster (pseudobulk-per-cell-type) strategy is the standard in the field, and it is why frameworks like Signac and ArchR wrap MACS for peak calling rather than relying on the Cell Ranger peaks: it recovers cell-type-specific regulatory elements that whole-sample calling systematically misses.

Cell Ranger cannot do this, because it only ever sees the whole sample and has no notion of your clusters. That is the real answer to “why another peak caller”: not because MACS3 is a better aggregate caller, but because it is the tool that lets you re-call peaks per cell type once you know what your cell types are.

The catch is that this requires clusters, and we do not have them yet — clustering comes later in the series. So in this tutorial the aggregate MACS3 run above simply introduces the tool and lets you compare it against Cell Ranger’s output. We return to per-cluster peak recalling in the clustering and downstream parts, where it becomes genuinely useful.

**A note if you used MACS2 in bulk ATAC-seq.** Our [bulk ATAC-seq guide](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/) calls peaks with MACS2. MACS3 is the successor to MACS2, from the same lab, and for peak calling the two are conceptually identical — same model, same `narrowPeak`/`.xls`/`summits.bed` outputs, largely the same options. The practical differences that matter here: MACS3 is rewritten for speed and lower memory on large datasets, and it adds the `-f FRAG` parser that reads a 10x fragments file directly. With MACS2 you cannot pass a fragments file as-is; the usual bulk-style workflow treats each fragment end as a read (`-f BED` with shifting/extension), which is why the bulk guide uses different flags. In short: same method and same output format, but MACS3’s `-f FRAG` is purpose-built for single-cell fragments, so we use it here.

### Which Peak Caller Should You Use?

`peaks.bed`**per cluster**

| Situation | Recommended approach |
| --- | --- |
| A first pass, or a straightforward dataset | Cell Ranger’s built-in  is sufficient |
| You have clustered your cells and want cell-type-specific elements | Recall peaks with MACS3 |
| Rare populations are central to your question | MACS3 per-cluster recalling, then merge |
| You need consistency with a Signac/ArchR workflow | MACS3 (both frameworks default to it) |

The real payoff of MACS3 comes *after* clustering: by calling peaks separately within each cell-type group (on that group’s pooled fragments) and then merging the results, you recover regulatory elements specific to even rare populations — peaks the whole-dataset aggregate would miss. In this tutorial we called MACS3 on the entire dataset to demonstrate the mechanics; the per-cluster strategy requires clusters, so we return to it in the clustering and downstream parts of this series. For now, Cell Ranger’s `peaks.bed` and the filtered peak-by-cell matrix are the outputs we carry forward.

---

## Best Practices for scATAC-seq Preprocessing

**1. Always open `web_summary.html` before anything else.** It surfaces the metrics that decide whether a run is usable — number of cells, median fragments per cell, fraction of fragments in peaks, and TSS enrichment. Judging quality here takes minutes and prevents hours of wasted downstream analysis on a bad run.

**2. Match the reference to the platform and version.** Cell Ranger ATAC requires a `cellranger-arc`/`cellranger-atac` reference (not a plain Cell Ranger gene-expression reference), and the reference must be compatible with your Cell Ranger ATAC version. Mixing incompatible references is a frequent and confusing source of preflight errors.

**3. Pin every version, and record them.** Note your Cell Ranger ATAC version, reference version, and — via Pixi — your open-source tool versions. Chromatin-accessibility results depend on the peak-calling and cell-calling algorithms, which change between releases. Pinning is what makes a result reproducible months later.

**4. Verify raw reads before aligning.** When pulling 10x data from the SRA, always use `fasterq-dump --include-technical` so the barcode read survives, then confirm the assignment by measuring each file: 16 bp is the barcode (R2), 8 bp is the index (I1), and the two long reads are R1 and R3 in file order. A five-second length check prevents a multi-hour alignment on mislabeled files.

**5. Keep the fragments file and its `.tbi` index together.** Nearly every downstream tool needs both. If you copy or move the fragments file, move the index with it, or regenerate the index with `tabix`.

**6. Provision enough compute and disk.** Budget ~16 cores, ~96 GB RAM, and generous free disk per sample — remember that FASTQ extraction alone can transiently need many times the archive size. Under-provisioning causes jobs to fail late, after hours of work.

**7. Separate persistent from transient storage.** Cell Ranger ATAC, the reference, and your final `outs/` belong on durable project storage; large intermediate files can live on scratch. Getting this split right avoids both quota errors and accidental data loss.

**8. Name samples consistently and keep a sample sheet.** Use clear, stable names (`healthy1`, `healthy2`, `severe1`, `severe2`) and record the SRR-to-group mapping in a small table. That sheet drives every downstream comparison in the series.

**9. Understand cell calling before trusting it.** Cell Ranger separates real nuclei from empty droplets automatically, but the call is a model, not ground truth. Review the barcode-rank plot in the web summary, and revisit filtering in Part 2 rather than assuming the defaults are perfect for your sample.

**10. Plan peak calling around your biological question.** If rare populations matter, budget for MACS3 per-cluster recalling later. Deciding this early shapes how you cluster and annotate.

---

## Common Pitfalls and How to Avoid Them

`fasterq-dump``--include-technical``_1/_2/_3/_4``_1``SraRunTable.csv``--sample``--sample``_S1_L00...``refdata-cellranger-arc-*``refdata-gex-*``curl``.tbi``fragments.tsv.gz.tbi``tabix -p bed``--localmem``--localcores``--localmem`

| Pitfall | Why it happens | How to avoid it |
| --- | --- | --- |
| Barcode read dropped on download | skips “technical” reads by default | Always pass  for 10x data |
| Wrong R1/R2/R3 assignment from SRA | The  numbering is submitter-dependent and can differ between studies | Never assume  is R1. Measure each file: 16 bp = R2, 8 bp = I1, the two long reads = R1/R3 in file order, then rename accordingly |
| Mixing ATAC and RNA runs | The study deposited both modalities | Filter  by assay before choosing accessions |
| does not match FASTQ names | The prefix must be exact | Set  to the exact name you gave the files, before |
| Using a gene-expression reference | Cell Ranger references look similar | Use a  reference for ATAC, never |
| Expired download link fails | 10x URLs are signed and time-limited | Re-copy the current  command from the download page each time |
| Missing  index | Index left behind when moving files | Keep  with its file, or rebuild with |
| Run fails late from low memory | set too high or node too small | Match / to the resources actually requested |
| Expecting dense, quantitative counts | scATAC data is sparse and near-binary | Interpret the peak matrix as accessible/not per cell, not as expression-like levels |
| Assuming aggregate peaks capture everything | Rare-population peaks dilute in pseudobulk | Plan MACS3 per-cluster recalling for cell-type-specific elements |

---

## Troubleshooting Common Errors

`fasterq-dump``--include-technical``bamtofastq``prefetch``.sra``0.0% of read pairs have both an aligned 5' and aligned 3' end``Reattaching``chunks_complete``rm -rf <id>``--id``--fastqs``--sample``--sample``cellranger-arc``--mem``--localmem``tabix``.tbi``tabix -p bed fragments.tsv.gz``FRAG``pixi add "macs3>=3.0"``prefetch``macs3``pigz``cd``pixi shell``cellranger-atac: command not found``PATH``PATH``~/.bashrc`

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Only two FASTQs after | Barcode read skipped, or non-standard SRA upload | Re-run with ; if still wrong, download from ENA or use 10x |
| is slow or times out | Large  file, network limits | Let it resume (re-run the same command); prefetch caches partial downloads |
| Cell Ranger: read lengths incompatible with chemistry | R1/R2/R3 mislabeled during renaming | Recheck read lengths and re-map: 16 bp = R2, 8 bp = I1, ~50 bp = R1/R3 |
|  | A short barcode or index read was assigned as R1 or R3, so genomic alignment is impossible | Measure the renamed files; R1 and R3 must be ~50 bp and R2 exactly 16 bp. Rename correctly and re-run |
| Re-run fails in seconds with the same error as before | Cell Ranger resumed the previous pipestance (, ) and reused cached results from the old inputs | Delete the output directory () or use a new  before re-running; a real run takes hours, not seconds |
| Preflight error: no FASTQs found | Wrong  path or  prefix | Confirm the directory holds the FASTQs and that  matches the file prefix exactly |
| Preflight error: incompatible reference | Wrong reference type or version | Use a  reference compatible with your Cell Ranger ATAC version |
| Job killed / out-of-memory | Insufficient RAM for the sample | Increase  and ; a PBMC scATAC sample needs ~64 GB |
| / “index” errors downstream | Missing or stale | Rebuild: |
| MACS3: unrecognized format | MACS3 too old | Upgrade inside Pixi: |
| Very few cells called | Low-quality library or damaged sample | Check the web summary’s fragment and TSS metrics before troubleshooting the pipeline |
| //: command not found | The Pixi environment is not activated in this shell | to the Pixi project and run  before calling the tools |
|  | not set in the shell/job | Re-export the  line; add it to  or the SLURM script |

---

## Conclusion: From Raw Reads to Analysis-Ready Peaks

You have taken one sample of a real COVID-19 case/control scATAC-seq cohort from raw sequencing reads to a called set of peaks and a peak-by-cell matrix. Along the way you have:

1. **Learned what scATAC-seq measures** — chromatin accessibility, the regulatory layer above gene expression — and how it differs from bulk ATAC-seq and from scRNA-seq.

2. **Built a reproducible environment**, installing the proprietary Cell Ranger ATAC pipeline manually and managing open-source tools (including sra-tools) cleanly with Pixi.

3. **Downloaded real patient data from the SRA**, correctly recovering the barcode read with `--include-technical` and mapping the SRA files to R1/R2/R3/I1 by measured read length and file order.

4. **Aligned and processed a sample** with `cellranger-atac count`, understanding why scATAC FASTQs come as R1/R2/R3/I1 and where the cell barcode lives.

5. **Interpreted the Cell Ranger outputs**, especially the all-important fragments file and the filtered peak-by-cell matrix, and **compared two peak-calling strategies** — Cell Ranger’s aggregate wavelet caller and MACS3.

**Key Takeaways:**

- **scATAC-seq data is sparse and near-binary at the single-cell level.** This one fact drives every downstream method choice, from normalization to clustering.

- **The fragments file is the central output.** Guard it, keep its index, and understand its columns — nearly everything downstream is built from it.

- **The 10x “single-cell” ATAC assay profiles nuclei.** Remember this when interpreting metrics and when comparing to whole-cell protocols.

- **Peak calling is a choice, not a default.** Cell Ranger’s aggregate peaks are a fine start; MACS3 per-cluster recalling is how you recover cell-type-specific regulatory elements.

- **Downloading 10x data from the SRA has two traps.** Recover the barcode read with `--include-technical`, and never assume `_1` is R1 — measure each file, using length to find the 16 bp barcode and 8 bp index, and file order to separate the two genomic reads.

- **Reproducibility comes from pinning versions.** Record your Cell Ranger ATAC, reference, and Pixi tool versions so your results hold up over time.

## Moving Forward

Process the remaining three samples exactly as you did this one — same commands, same naming scheme — so that both healthy and both severe/critical COVID-19 samples are ready as a set. The next tutorial then tackles **quality control**: separating high-quality nuclei from empty droplets, low-quality cells, and doublets using scATAC-specific metrics such as fragments per cell, fraction of fragments in peaks, TSS enrichment, and nucleosome banding. From there the series moves into dimensionality reduction (TF-IDF and LSI), clustering, cell-type annotation via gene activity scores, and — because we now have a real case/control cohort — **differential accessibility between healthy and severe COVID-19**, along with motif enrichment, TF footprinting, and integration with the study’s matched scRNA-seq.

If you have not yet worked through the transcriptomic side, the companion [scRNA-seq Complete Beginner’s Guide](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/) covers the PBMC transcriptome workflow, and the two series meet when we integrate expression and accessibility.

---

## References

1. Buenrostro JD, Giresi PG, Zaba LC, Chang HY, Greenleaf WJ. Transposition of native chromatin for fast and sensitive epigenomic profiling of open chromatin, DNA-binding proteins and nucleosome position. *Nature Methods*. 2013;10(12):1213-1218. doi:10.1038/nmeth.2688

2. Buenrostro JD, Wu B, Litzenburger UM, et al. Single-cell chromatin accessibility reveals principles of regulatory variation. *Nature*. 2015;523(7561):486-490. doi:10.1038/nature14590

3. Cusanovich DA, Daza R, Adey A, et al. Multiplex single-cell profiling of chromatin accessibility by combinatorial cellular indexing. *Science*. 2015;348(6237):910-914. doi:10.1126/science.aab1601

4. Satpathy AT, Granja JM, Yost KE, et al. Massively parallel single-cell chromatin landscapes of human immune cell development and intratumoral T cell exhaustion. *Nature Biotechnology*. 2019;37(8):925-936. doi:10.1038/s41587-019-0206-z

5. Zhang Y, Liu T, Meyer CA, et al. Model-based Analysis of ChIP-Seq (MACS). *Genome Biology*. 2008;9(9):R137. doi:10.1186/gb-2008-9-9-r137

6. Stuart T, Srivastava A, Madad S, Lareau CA, Satija R. Single-cell chromatin state analysis with Signac. *Nature Methods*. 2021;18(11):1333-1341. doi:10.1038/s41592-021-01282-5

7. Granja JM, Corces MR, Pierce SE, et al. ArchR is a scalable software package for integrative single-cell chromatin accessibility analysis. *Nature Genetics*. 2021;53(3):403-411. doi:10.1038/s41588-021-00790-6

8. Baek S, Lee I. Single-cell ATAC sequencing analysis: From data preprocessing to hypothesis generation. *Computational and Structural Biotechnology Journal*. 2020;18:1429-1439. doi:10.1016/j.csbj.2020.06.012

9. Remodeling of the chromatin landscape in peripheral blood cells in patients with severe Delta COVID-19. *Frontiers in Immunology*. 2024;15:1415317. doi:10.3389/fimmu.2024.1415317 (scATAC-seq data: NCBI GEO GSE282769; SRA BioProject PRJNA1190389)

10. Leinonen R, Sugawara H, Shumway M; International Nucleotide Sequence Database Collaboration. The Sequence Read Archive. *Nucleic Acids Research*. 2011;39(Database issue):D19-D21. doi:10.1093/nar/gkq1019

11. 10x Genomics. Cell Ranger ATAC 2.2 Release Notes and Documentation. [https://www.10xgenomics.com/support/software/cell-ranger-atac/latest](https://www.10xgenomics.com/support/software/cell-ranger-atac/latest) (2025)

12. MACS3 Project. MACS3 Documentation. [https://macs3-project.github.io/MACS/](https://macs3-project.github.io/MACS/) (2025)

---

*This tutorial is Part 1 of the NGS101.com Single-Cell ATAC-seq Complete Beginner’s Guide. Continue to Part 2 to learn quality control and cell filtering for chromatin accessibility data.*
