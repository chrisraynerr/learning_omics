# How to Analyze Single-Cell RNA-seq Data — Complete Beginner's Guide Part 13: RNA Velocity Analysis with scVelo

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/)

---

*From static snapshots to directional transcriptional dynamics — discover where your cells are going, not just where they are*

---

If you have followed [Parts 1–12](https://ngs101.com/tutorials/#single-cell-seq) of this tutorial series, you have built a comprehensive picture of the immune cell landscape in the GSE174609 periodontitis dataset.

Every one of these analyses worked with a **static snapshot** of gene expression — a photograph of each cell frozen at the moment of capture. But biology is not static. Cells are constantly producing new RNA, splicing pre-mRNA into mature transcripts, and degrading old molecules at regulated rates. **RNA velocity analysis** turns this molecular metabolism into a signal, revealing the *direction* of each cell’s transcriptional change and predicting where it is heading.

This tutorial introduces **scVelo**, the leading Python tool for RNA velocity analysis, and walks you step by step through the entire workflow — from regenerating velocity-aware count matrices to visualizing directional arrows on your UMAP and statistically comparing transcriptional kinetics between disease conditions.

> 

**Note on language:** This is the first tutorial in the series that uses **Python** instead of R. Do not be alarmed — all code is provided with full explanations, and the transition is gradual. The biological context and dataset you have worked with throughout the series remains exactly the same.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/)

- [Introduction: What Is RNA Velocity and Why Does It Matter?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Introduction_What_Is_RNA_Velocity_and_Why_Does_It_Matter)

  - [The Problem with Static Snapshots](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#The_Problem_with_Static_Snapshots)
  - [The Core Idea: Spliced vs. Unspliced RNA as a Speed Camera](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#The_Core_Idea_Spliced_vs_Unspliced_RNA_as_a_Speed_Camera)
  - [What Biological Questions Can RNA Velocity Answer?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#What_Biological_Questions_Can_RNA_Velocity_Answer)
  - [RNA Velocity vs. Trajectory Analysis: Complementary, Not Competing](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#RNA_Velocity_vs_Trajectory_Analysis_Complementary_Not_Competing)
  - [What Is scVelo and Why Use It?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#What_Is_scVelo_and_Why_Use_It)
  - [Workflow Overview](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Workflow_Overview)

- [Environment Setup and Tool Installation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Environment_Setup_and_Tool_Installation)

  - [Part A: Create the Conda Environment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Part_A_Create_the_Conda_Environment)
  - [Part B: Download Reference Files for STARsolo](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Part_B_Download_Reference_Files_for_STARsolo)
  - [Part B: R Packages](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Part_B_R_Packages)
  - [Part C: Introducing JupyterLab](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Part_C_Introducing_JupyterLab)

- [Dataset: GSE174609 PBMC Periodontitis Cohort](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Dataset_GSE174609_PBMC_Periodontitis_Cohort)
- [Step 1: Generate Spliced and Unspliced Counts with STARsolo](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_1_Generate_Spliced_and_Unspliced_Counts_with_STARsolo)

  - [Why We Need to Re-run Alignment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Why_We_Need_to_Re-run_Alignment)
  - [Running STARsolo with Velocity Counting](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Running_STARsolo_with_Velocity_Counting)

- [Step 2: Export Cell Metadata from the Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_2_Export_Cell_Metadata_from_the_Seurat_Object)
- [Step 3: Load and Merge Data in Python](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_3_Load_and_Merge_Data_in_Python)

  - [3.1 — Understanding AnnData: Python’s Answer to the Seurat Object](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#31_%E2%80%94_Understanding_AnnData_Pythons_Answer_to_the_Seurat_Object)
  - [3.2 — Import Libraries and Configure Settings](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#32_%E2%80%94_Import_Libraries_and_Configure_Settings)
  - [3.3 — Load the Processed Seurat Metadata](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#33_%E2%80%94_Load_the_Processed_Seurat_Metadata)
  - [3.4 — Load Spliced and Unspliced Counts from STARsolo](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#34_%E2%80%94_Load_Spliced_and_Unspliced_Counts_from_STARsolo)
  - [3.5 — Merge All Samples and Transfer Seurat Annotations](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#35_%E2%80%94_Merge_All_Samples_and_Transfer_Seurat_Annotations)

- [Step 4: Preprocessing and Moment Estimation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_4_Preprocessing_and_Moment_Estimation)

  - [Why Preprocessing Is Necessary](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Why_Preprocessing_Is_Necessary)

- [Step 5: Estimate RNA Velocity — Fitting the Dynamical Model](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_5_Estimate_RNA_Velocity_%E2%80%94_Fitting_the_Dynamical_Model)

  - [Choosing a Velocity Model](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Choosing_a_Velocity_Model)
  - [Step 5.1 — Recover Gene Dynamics](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_51_%E2%80%94_Recover_Gene_Dynamics)
  - [Step 5.2 — Compute Velocity and Velocity Graph](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_52_%E2%80%94_Compute_Velocity_and_Velocity_Graph)

- [Step 6: Project and Visualize RNA Velocity](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_6_Project_and_Visualize_RNA_Velocity)

  - [6.1 — Velocity Stream Plot](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#61_%E2%80%94_Velocity_Stream_Plot)
  - [6.2 — Per-Cell Velocity Arrows](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#62_%E2%80%94_Per-Cell_Velocity_Arrows)
  - [6.3 — Compute and Visualize Latent Time](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#63_%E2%80%94_Compute_and_Visualize_Latent_Time)
  - [6.4 — Phase Portrait of Top Velocity Genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#64_%E2%80%94_Phase_Portrait_of_Top_Velocity_Genes)
  - [6.5 — Velocity Patterns Per Condition](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#65_%E2%80%94_Velocity_Patterns_Per_Condition)
  - [6.6 — Velocity Confidence Score](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#66_%E2%80%94_Velocity_Confidence_Score)

- [Step 7: Differential Kinetics Analysis Between Conditions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Step_7_Differential_Kinetics_Analysis_Between_Conditions)

  - [Why Go Beyond Visual Comparison?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Why_Go_Beyond_Visual_Comparison)
  - [7.1 — Run the Differential Kinetics Test](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#71_%E2%80%94_Run_the_Differential_Kinetics_Test)
  - [7.2 — Inspect and Visualize Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#72_%E2%80%94_Inspect_and_Visualize_Results)
  - [7.3 — Compare Latent Time Distributions Between Conditions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#73_%E2%80%94_Compare_Latent_Time_Distributions_Between_Conditions)

- [Best Practices and Common Pitfalls](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Best_Practices_and_Common_Pitfalls)

  - [Best Practices](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Best_Practices)
  - [Common Pitfalls](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Common_Pitfalls)

- [Key Takeaways](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Key_Takeaways)
- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/#Like_this)

## Introduction: What Is RNA Velocity and Why Does It Matter?

### The Problem with Static Snapshots

Every scRNA-seq experiment produces what is, fundamentally, a snapshot: the expression state of each cell at the precise moment its RNA was captured. When you look at a UMAP plot with annotated clusters, you see thousands of cells arranged by transcriptional similarity — but you have no information about which direction any of those cells are moving. A Classical Monocyte at the boundary between the monocyte cluster and the macrophage-like region: is it transitioning toward macrophage identity, or was it recently a more immature progenitor? A T cell cluster in the Post_Treatment condition: are these cells returning toward a resting state, or still actively responding to residual inflammation?

Static analysis cannot answer these questions. RNA velocity can.

### The Core Idea: Spliced vs. Unspliced RNA as a Speed Camera

RNA velocity is built on a simple but powerful observation about how genes are expressed. When a gene is transcribed, the cell first produces **pre-mRNA** — an unspliced transcript that still contains introns. This unspliced RNA is then processed (spliced) to produce **mature mRNA** — the functional spliced transcript that is translated into protein.

These two molecular species carry different information:

- **Unspliced (pre-mRNA)** reflects what is *currently being transcribed* — the immediate transcriptional activity of the cell right now.

- **Spliced (mature mRNA)** reflects the *accumulated result of recent transcription* — what the gene has been doing over the past minutes to hours.

If you compare the two for a given gene in a given cell, you can infer whether that gene is being **turned on** or **turned off**:

- If unspliced RNA is **higher than expected** for the current spliced level, the gene’s transcription rate exceeds its degradation rate — the gene is being **induced**. The cell will have more of this transcript in the future.

- If unspliced RNA is **lower than expected**, the gene’s transcription has slowed — the gene is being **repressed**. The cell will have less of this transcript in the future.

By computing this relationship simultaneously for thousands of genes, RNA velocity analysis produces a **velocity vector** for each cell: a high-dimensional arrow pointing toward that cell’s predicted future transcriptional state. When projected onto the 2D UMAP embedding, these arrows form directional flow patterns that reveal the dynamics of cell state transitions.

### What Biological Questions Can RNA Velocity Answer?

**1. Where is each cell heading in transcriptional space?**

Velocity stream plots overlay smooth directional arrows on the UMAP, showing the collective “flow” of cells through transcriptional space. Dense, coherent arrows indicate populations actively transitioning between states. Diverging arrows indicate branching fate decisions.

**2. What is the natural root of a developmental or activation process?**

In trajectory analysis ([Parts 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/) and [7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/)), you had to manually specify a root cell. RNA velocity arrows point *away* from the root state automatically — the root is simply where the arrows originate. This makes velocity a powerful validation tool for pseudotime analyses.

**3. What are the kinetic rates of gene regulation?**

The scVelo dynamical model estimates, for each gene, a set of kinetic parameters: the transcription rate (how fast RNA is made), the splicing rate (how fast pre-mRNA is processed), and the degradation rate (how fast mature mRNA is destroyed). These parameters describe the “speed” of gene regulatory changes and can be compared between conditions.

**4. Which genes drive transcriptional dynamics — and do they change with disease or treatment?**

By identifying genes with the strongest, most consistent velocity signal, you can pinpoint the molecular drivers of cell state transitions. Comparing these drivers between Healthy and Post_Treatment conditions reveals how non-surgical periodontal therapy alters transcriptional kinetics.

### RNA Velocity vs. Trajectory Analysis: Complementary, Not Competing

[Part 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/) (Monocle 3) and [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/) (Slingshot) covered trajectory analysis. Students often ask: if I have already done trajectory analysis, do I need RNA velocity? The answer is yes — they answer different questions and should be used together.

**Primary data used****What it reveals****Pseudotime****Root specification****Best suited for****Quantifies kinetics****Condition comparison****Stable cell types**

| Feature | RNA Velocity (scVelo) | Trajectory Analysis (Monocle 3 / Slingshot) |
| --- | --- | --- |
|  | Spliced + unspliced counts | Spliced counts only |
|  | Direction and speed of state change | Sequence of states along a path |
|  | Latent time from kinetic model | Topological distance from root |
|  | Automatic (emerges from data) | Manual (user-defined) |
|  | Active, dynamic populations | Any ordered biological process |
|  | Yes (transcription/splicing/degradation rates) | No |
|  | Differential kinetics testing | Gene expression along pseudotime |
|  | Low signal (expected behavior) | Works well |

**Use RNA velocity when:** you want to know the *direction* and *rate* of transcriptional change, validate trajectory roots, or compare kinetic parameters between conditions.

**Use trajectory analysis when:** you want to order cells along a continuous path, quantify pseudotime, or identify genes that change progressively along a trajectory.

> 

**A note on our dataset:** GSE174609 is a PBMC dataset containing a mixture of immune cell types. Some populations — such as Classical Monocytes, T cells, and their progenitor-like intermediates — have active transcriptional dynamics and will show strong, coherent velocity signals. Other fully differentiated, stable populations (NK cells, resting B cells) may show weak or noisy arrows. This is biologically expected and does not indicate a problem with your analysis.

### What Is scVelo and Why Use It?

**scVelo** (Single-Cell Velocity) is a Python package developed by Volker Bergen et al. in the Theis Lab (Helmholtz Munich). It is the most widely cited RNA velocity tool and is actively maintained.

Several RNA velocity tools exist (velocyto, TF-Velo, UniTVelo), but scVelo’s advantages are significant:

**1. The Dynamical Model**

The original RNA velocity paper (La Manno et al., 2018) used a simple “steady-state” model that assumed each gene exists in either an induction phase or a repression phase at any given time. scVelo’s **dynamical model** is substantially more powerful: it fits a full transcriptional kinetic model with four parameters per gene — transcription rate (α), splicing rate (β), degradation rate (γ), and a switching time (t_s) marking when the gene transitions from induction to repression. This model does not assume steady state and handles complex non-linear dynamics accurately.

**2. Likelihood-Based Gene Selection**

Not every gene provides useful velocity information. scVelo scores each gene by how well the kinetic model fits its spliced/unspliced data and selects **high-likelihood velocity genes** — those with clear kinetic patterns. This data-driven filtering reduces noise.

**3. Latent Time**

scVelo computes a **latent time** value for each cell — a cell-intrinsic pseudotime derived entirely from RNA kinetics, without any user-specified root. This is more biologically grounded than topology-based pseudotime and serves as an independent validation of trajectory analyses.

**4. Differential Kinetics Testing**

scVelo provides a statistical framework for identifying genes whose kinetic parameters (transcription, splicing, degradation rates) differ significantly between cell groups or experimental conditions. This enables quantitative comparison of transcriptional dynamics across your experimental design.

### Workflow Overview

This tutorial proceeds through seven steps:

1. **Generate spliced and unspliced counts** — Re-run STARsolo on the original FASTQ files with a velocity-aware counting flag

2. **Export cell annotations from Seurat** — Transfer cell type labels, UMAP coordinates, and condition metadata from the Part 4 Seurat object

3. **Load and merge data in Python** — Combine velocity counts with Seurat annotations in the AnnData framework

4. **Preprocess and estimate moments** — Filter, normalize, and smooth the spliced/unspliced matrices

5. **Fit the dynamical model** — Recover per-gene kinetic parameters

6. **Project and visualize** — Overlay velocity arrows on UMAP and compute latent time

7. **Differential kinetics analysis** — Compare transcriptional kinetics between conditions

---

## Environment Setup and Tool Installation

This tutorial uses tools from three distinct environments: a command-line alignment tool (STAR), R for exporting the Seurat object, and Python for the velocity analysis itself. This section walks through the installation of every tool you will need.

### Part A: Create the Conda Environment

We install everything — the STAR aligner, all Python analysis packages, and JupyterLab — into a **single Conda environment** called `velocity`. STAR is a compiled C++ binary with no Python dependencies, so it coexists cleanly with scVelo and Scanpy.

> 

**Why STAR and not Cell Ranger?** [Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/) of this series used Cell Ranger to generate the gene count matrix. Cell Ranger does not support unspliced/spliced count separation, which is required for RNA velocity. **STAR** (Spliced Transcripts Alignment to a Reference) is an alternative high-performance aligner whose `STARsolo` mode natively outputs the spliced, unspliced, and ambiguous count matrices we need — with a single additional flag. STAR and Cell Ranger use the same FASTQ input files, so no new data download is required.

```python
# Create a single environment for the entire tutorial
conda create -n velocity python=3.10 -y
conda activate velocity

# STAR aligner + samtools (via Bioconda)
conda install -c bioconda star samtools -y

# Python analysis packages
pip install scvelo        # RNA velocity analysis
pip install scanpy        # single-cell toolkit (required by scVelo)
pip install anndata       # the AnnData data structure
pip install jupyterlab    # interactive notebook interface
pip install matplotlib seaborn   # visualization

# Verify
STAR --version            # should print 2.7.x
python -c "import scvelo; import scanpy; print('OK')"
```

### Part B: Download Reference Files for STARsolo

STARsolo requires two reference files: a genome sequence (FASTA) and a gene annotation (GTF). We also need the 10x Chromium cell barcode whitelist to correctly identify valid cell barcodes. These are the same reference versions used throughout this tutorial series.

**1. Download the human genome FASTA (GRCh38, GENCODE release 43):**

```
mkdir -p ~/references/GRCh38
cd ~/references/GRCh38

wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_43/GRCh38.primary_assembly.genome.fa.gz
gunzip GRCh38.primary_assembly.genome.fa.gz
```

**2. Download the gene annotation GTF (GENCODE release 43):**

```
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_43/gencode.v43.primary_assembly.annotation.gtf.gz
gunzip gencode.v43.primary_assembly.annotation.gtf.gz
```

**3. Get the 10x Chromium v3 cell barcode whitelist:**

The whitelist is a text file containing all valid 10x Chromium v3 cell barcodes (~6.8 million sequences). STARsolo uses it to identify which reads come from real cells.

> 

**Filename note:** In Cell Ranger v8.0 and above (including v10.0.0), the file was renamed from `3M-february-2018.txt.gz` to `3M-february-2018_TRU.txt.gz`. The barcode content is identical — only the filename changed. We copy it under the original name so the STARsolo command stays consistent regardless of Cell Ranger version.

**Option A — Copy from your Cell Ranger installation:**

```bash
# Cell Ranger v8.0 and above (including v10.0.0) — file is named _TRU
cp /path/to/cellranger-10.0.0/lib/python/cellranger/barcodes/3M-february-2018_TRU.txt.gz \
   ~/references/GRCh38/3M-february-2018.txt.gz
gunzip ~/references/GRCh38/3M-february-2018.txt.gz
```

**Option B — Download directly** (if you don’t have Cell Ranger, or prefer a direct download):

```
wget https://github.com/Lab-of-Adaptive-Immunity/cr_whitelists/raw/main/3M-february-2018.txt.gz \
     -O ~/references/GRCh38/3M-february-2018.txt.gz
gunzip ~/references/GRCh38/3M-february-2018.txt.gz
```

Confirm the file is complete:

```bash
wc -l ~/references/GRCh38/3M-february-2018.txt
# Expected output: ~3,686,400 if copied from Cell Ranger v8.0+ (_TRU file)
#                  ~6,794,880 if downloaded from the community mirror
# Both are valid for standard 3' scRNA-seq — see note below
```

> 

**Why two different counts?** Older Cell Ranger versions bundled gene expression barcodes and Feature Barcoding barcodes together in one ~6.8M-entry file. Cell Ranger v8.0+ split them: the `_TRU` file contains only the ~3.7M gene expression barcodes, which is exactly what we need for standard scRNA-seq. If you downloaded the community mirror file (Option B), you will see ~6.8M lines — this also works fine; STARsolo will simply match against a slightly larger list.

**4. Build the STAR genome index:**

This step processes the genome FASTA and GTF into a binary index that STAR can load quickly during alignment. It only needs to be run once. It requires approximately **30 GB of RAM** and takes 30–60 minutes.

```
conda activate velocity

STAR \
  --runMode genomeGenerate \
  --runThreadN 8 \
  --genomeDir ~/references/GRCh38/star_index \
  --genomeFastaFiles ~/references/GRCh38/GRCh38.primary_assembly.genome.fa \
  --sjdbGTFfile ~/references/GRCh38/gencode.v43.primary_assembly.annotation.gtf \
  --sjdbOverhang 100
```

> 

**`--sjdbOverhang`:** This value should be set to your read length minus 1. Most 10x Chromium v3 libraries use 150 bp reads (cDNA read, R2), so `--sjdbOverhang 100` is a commonly used default. If your R2 reads are a different length, adjust accordingly.

---

### Part B: R Packages

**Seurat** is already installed if you have followed [Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/) of this series. No reinstallation is needed — we use it only to load the downsampled object and export two CSV files in Step 2.

---

### Part C: Introducing JupyterLab

All previous tutorials in this series used R, typically run interactively in RStudio. In this tutorial, we use **Python** — the dominant language for tools like scVelo, Scanpy, and CellRank that extend beyond what R packages currently offer.

The interactive environment we will use is **JupyterLab** — a browser-based development environment that lets you write and execute Python code in small chunks called “cells,” with plots rendered inline. JupyterLab notebooks (`.ipynb` files) integrate code, outputs, and written explanations in a single document that is easy to share and reproduce.

**How JupyterLab compares to RStudio:**

`.R``.Rmd``.ipynb`

| Feature | RStudio | JupyterLab |
| --- | --- | --- |
| Language | R | Python (primarily) |
| Interface | Desktop application | Browser-based |
| Code execution | Scripts / console / Notebook | Notebook cells |
| Plot rendering | Plots pane | Inline in notebook |
| File format | , |  |

To launch JupyterLab:

```bash
conda activate velocity
jupyter lab
# JupyterLab opens automatically in your default browser at http://localhost:8888
# Create a new notebook: File → New → Notebook → select the Python 3 kernel
```

---

## Dataset: GSE174609 PBMC Periodontitis Cohort

This tutorial continues with the same dataset used throughout the entire series: **GSE174609** (Lee et al., 2022), a single-cell RNA-seq dataset profiling human peripheral blood mononuclear cells (PBMCs) from two groups:

- **Healthy** — periodontally healthy controls (Healthy_1 through Healthy_4; SRR14575500–SRR14575503)

- **Post_Treatment** — periodontitis patients after non-surgical periodontal therapy (Post_Patient_1 through Post_Patient_4; SRR14575508–SRR14575511)

These 8 samples were processed as 10x Chromium single-cell libraries. (The full GSE174609 dataset contains a third Pre_Treatment group, but as in all previous parts of this series, our analysis focuses on the Healthy vs. Post_Treatment comparison.)

**What you need from previous parts of this series:**

- **Raw FASTQ files** (downloaded in [Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/)) — we will re-run alignment on the same files

- **Downsampled Seurat object** (`seurat_downsampled.rds` from [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/)) — contains cell type labels, CCA-based UMAP coordinates, and condition metadata

> 

**Have you completed [Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/)?** Raw FASTQ files must already be downloaded and available on your machine. If not, please follow the [Part 1 download instructions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/) before continuing.

---

## Step 1: Generate Spliced and Unspliced Counts with STARsolo

### Why We Need to Re-run Alignment

In [Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/), we used **Cell Ranger** to generate the gene count matrix. Cell Ranger is excellent for standard gene expression quantification, but it does not separate reads into spliced and unspliced categories. For RNA velocity, we need both. **STARsolo** — installed above — fills this role: it uses the same FASTQ files and the same genome reference, but with the `--soloFeatures Gene Velocyto` flag it simultaneously outputs spliced, unspliced, and ambiguous count matrices.

> 

**Do you need to re-download the FASTQ files?** No. You will reuse the exact same FASTQ files downloaded in Part 1. We are simply re-running the alignment with a different tool and one additional flag.

### Running STARsolo with Velocity Counting

We demonstrate the command for **one sample — `Healthy_1`**. Once you have confirmed the output looks correct, use the SLURM script (provided separately as `run_starsolo_velocity.sh`) to process all samples on the cluster.

All FASTQ files for this dataset live in a **flat directory** — there are no per-sample subfolders. The naming convention is `{SAMPLE}_S{N}_L001_R{1,2}_001.fastq.gz`.

```bash
conda activate velocity

# Reference paths — set up in the Environment Setup section above
GENOME_DIR=~/references/GRCh38/star_index
WHITELIST=~/references/GRCh38/3M-february-2018.txt

# All FASTQ files are in one flat directory — no per-sample subfolders
FASTQ_DIR="/path/to/fastq_link"
SAMPLE="Healthy_1"
OUT_DIR="results/velocity/${SAMPLE}"

mkdir -p "${OUT_DIR}"

# Raise the open file handle limit before running STAR
# STAR's BAM sorting opens many temporary files simultaneously;
# the default OS limit (~1024) is too low and causes a fatal error
ulimit -n 65536

STAR \
  --runThreadN 8 \
  --genomeDir "${GENOME_DIR}" \
  --readFilesIn "${FASTQ_DIR}/${SAMPLE}_S1_L001_R2_001.fastq.gz" \
               "${FASTQ_DIR}/${SAMPLE}_S1_L001_R1_001.fastq.gz" \
  --readFilesCommand zcat \
  --soloType CB_UMI_Simple \
  --soloCBwhitelist "${WHITELIST}" \
  --soloCBstart 1 --soloCBlen 16 \
  --soloUMIstart 17 --soloUMIlen 12 \
  --soloFeatures Gene Velocyto \
  --soloCellFilter EmptyDrops_CR \
  --outSAMtype BAM SortedByCoordinate \
  --outSAMattributes NH HI nM AS CR UR CB UB GX GN sS sQ sM \
  --outFileNamePrefix "${OUT_DIR}/"
```

> 

**Processing all 8 samples:** Rather than running the command above eight times manually, submits all samples as a job array on an HPC. The `ulimit -n 65536` fix is already included in that script.

> 

**Key parameter: `--soloFeatures Gene Velocyto`**Adding `Velocyto` alongside `Gene` instructs STARsolo to classify each read into one of three categories based on its overlap with the gene annotation: reads overlapping annotated exons only are counted as **spliced**; reads overlapping intron regions only are counted as **unspliced**; reads spanning an exon-intron boundary are counted as **ambiguous**. The results are written as separate MTX-format count matrices — one directory per category.

---

## Step 2: Export Cell Metadata from the Seurat Object

In this step we transfer all biological annotations accumulated in [Parts 1–4](https://ngs101.com/tutorials/#single-cell-seq) from R into files that Python can read. We use the **downsampled Seurat object from [Part 7-2 (Slingshot)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/)** — a representative subset of cells that makes the velocity computation faster while preserving the full cell-type and condition diversity of the dataset.

We export two CSV files:

- **`seurat_metadata.csv`** — cell barcodes with their cell type labels, condition, and sample ID

- **`seurat_umap.csv`** — the CCA-based UMAP coordinates computed in Part 7-2, so velocity arrows land on the same layout used throughout the series

Run the following in RStudio or an R terminal:

```r
library(Seurat)

# Load the downsampled Seurat object from Part 7-2 (Slingshot tutorial)
seurat_obj <- readRDS("seurat_downsampled.rds")

# Export cell metadata: condition, sample_id, final_annotation, and QC columns
meta_df <- seurat_obj@meta.data
meta_df$barcode <- rownames(meta_df)
write.csv(meta_df, "seurat_metadata.csv", row.names = FALSE)

# Export CCA-based UMAP coordinates from Part 7-2
# Rename columns to UMAP_1 / UMAP_2 so the Python loading code stays consistent
umap_df <- as.data.frame(Embeddings(seurat_obj, reduction = "umap.cca"))
umap_df$barcode <- rownames(umap_df)
colnames(umap_df)[1:2] <- c("UMAP_1", "UMAP_2")
write.csv(umap_df, "seurat_umap.csv", row.names = FALSE)
```

> 

**Why the downsampled object?** `recover_dynamics` (Step 5) fits a full kinetic model gene by gene and scales with cell count. Starting with the downsampled object used in [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/) reduces runtime substantially while keeping all cell types and both conditions represented. You can always re-run on the full dataset once you are confident the workflow is correct.

> 

**Check your barcode format before continuing:** In R, run `head(rownames(seurat_obj@meta.data))` to confirm the format. After integration and downsampling, barcodes look like `Healthy_1_AAACGAACACGACGTC-1` — sample prefix, underscore, 16-bp sequence, `-1` suffix. The Python loading function in Step 3 replicates this format exactly.

---

## Step 3: Load and Merge Data in Python

Open JupyterLab and create a new notebook. All remaining code in this tutorial runs in Python.

### 3.1 — Understanding AnnData: Python’s Answer to the Seurat Object

Before loading any data, it helps to understand the data structure we will be working with throughout the Python ecosystem: **AnnData** (Annotated Data). AnnData is the Python equivalent of Seurat’s `SeuratObject` — it stores the gene expression matrix alongside all associated metadata in a single self-contained object.

The diagram below shows how the two structures map onto each other. Both objects are organized around a central **cells × genes matrix**. Seurat stores this as a nested assay structure accessed with `@`; AnnData uses a flat attribute-based layout accessed with `.`. The amber rows — **layers** — are the key addition for RNA velocity: a single AnnData can hold multiple named matrices (`spliced`, `unspliced`) all indexed by the same cells and genes, making it straightforward to carry both alongside processed expression data.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/seuratobj_anndata.png?resize=1024%2C678&ssl=1)

### 3.2 — Import Libraries and Configure Settings

Start the notebook with all imports and global settings:

```python
import scvelo as scv
import scanpy as sc
import anndata as ad
import numpy as np
import pandas as pd
import os
import warnings

warnings.filterwarnings("ignore")

scv.settings.verbosity = 0           # suppress scVelo internal messages
scv.settings.figdir   = "figures/"   # directory for all saved plots
os.makedirs("figures", exist_ok=True)
```

### 3.3 — Load the Processed Seurat Metadata

```python
meta_df = pd.read_csv("seurat_metadata.csv", index_col="barcode")
umap_df = pd.read_csv("seurat_umap.csv",     index_col="barcode")

# Confirm key columns are present
print(meta_df[["final_annotation", "condition", "sample_id"]].head())
print(f"\nTotal annotated cells: {len(meta_df)}")
```

**Expected output:**

```
                                 final_annotation condition  sample_id
barcode
Healthy_1_AAACGAACACGACGTC-1  Classical Monocytes   Healthy  Healthy_1
Healthy_1_AAACGAAGTGGACTGA-1              T cells   Healthy  Healthy_1
...

Total annotated cells: 7262
```

You should see 7,262 cells — the downsampled subset from Part 7-2 covering all 7 cell types across both conditions.

### 3.4 — Load Spliced and Unspliced Counts from STARsolo

Before writing any code, it helps to know exactly what the STARsolo output looks like on disk. The diagram below shows the directory structure produced by the `--soloFeatures Gene Velocyto` flag for one sample:

```
results/velocity/Healthy_1/
├── Aligned.sortedByCoord.out.bam        ← coordinate-sorted BAM (all reads)
├── Log.final.out                        ← alignment summary statistics
└── Solo.out/
    ├── Gene/
    │   └── filtered/                    ← standard gene counts (as in Part 1)
    │       ├── matrix.mtx
    │       ├── barcodes.tsv
    │       └── features.tsv
    └── Velocyto/
        └── filtered/                    ← velocity counts (used in this step)
            ├── spliced.mtx              ← reads mapping to exons only
            ├── unspliced.mtx            ← reads mapping to introns only
            ├── ambiguous.mtx            ← reads spanning exon-intron boundaries
            ├── barcodes.tsv             ← shared barcode list for all three matrices
            └── features.tsv             ← shared gene list (Ensembl ID + gene symbol)
```

Key points to notice:

- All five Velocyto files sit **flat** in the `filtered/` directory — there are no per-type subdirectories

- `barcodes.tsv` and `features.tsv` are **shared** across spliced, unspliced, and ambiguous matrices

- Each MTX file is stored as **genes × cells** — the loading function below transposes it to cells × genes

- The `Gene/filtered/` directory contains the same kind of count matrix Cell Ranger produced in Part 1; we do not use it here

The following helper function reads the Velocyto MTX files for a single sample:

```r
from scipy.io import mmread
from scipy.sparse import csr_matrix

def load_velocyto_mtx(sample_dir, sample_id):
    """
    Load spliced and unspliced count matrices from STARsolo Velocyto output.

    Parameters
    ----------
    sample_dir : str
        Path to the STARsolo results directory for one sample
        (e.g., 'results/velocity/Healthy_1').
    sample_id : str
        Sample identifier used as a barcode prefix in Seurat
        (e.g., 'Healthy_1'). Prepended to raw barcodes to match
        the format stored in the Seurat metadata.

    Returns
    -------
    AnnData
        Cells x genes AnnData with:
          .X                     spliced count matrix
          .layers["spliced"]     spliced counts (copy of .X)
          .layers["unspliced"]   unspliced counts
    """
    vel_dir = os.path.join(sample_dir, "Solo.out", "Velocyto", "filtered")

    # All five files are in the same flat directory:
    #   spliced.mtx, unspliced.mtx, ambiguous.mtx (genes x cells — transposed below)
    #   barcodes.tsv, features.tsv (shared across all three matrices)
    spliced   = ad.AnnData(csr_matrix(mmread(os.path.join(vel_dir, "spliced.mtx")).T))
    unspliced = ad.AnnData(csr_matrix(mmread(os.path.join(vel_dir, "unspliced.mtx")).T))

    # features.tsv: column 0 = Ensembl gene ID (guaranteed unique — used as var_names
    #               to avoid duplicate label errors during concat)
    #               column 1 = gene symbol (switched to in Section 3.5 after merge)
    features = pd.read_csv(
        os.path.join(vel_dir, "features.tsv"),
        header=None, sep="\t"
    )
    ensembl_ids  = features[0].values
    gene_symbols = features[1].values

    # Raw barcodes from STARsolo look like 'AAACCCACAAGCGATG' (no suffix)
    # Seurat appends '-1' and a sample prefix, giving 'Healthy_1_AAACCCACAAGCGATG-1'
    # We replicate both transformations here to match the Seurat naming convention
    raw_barcodes = pd.read_csv(
        os.path.join(vel_dir, "barcodes.tsv"),
        header=None
    )[0].values
    prefixed_barcodes = [f"{sample_id}_{bc}-1" for bc in raw_barcodes]

    spliced.var_names   = pd.Index(ensembl_ids)
    spliced.obs_names   = pd.Index(prefixed_barcodes)
    unspliced.var_names = spliced.var_names
    unspliced.obs_names = spliced.obs_names

    # Store gene symbols as a var metadata column for use in plots and downstream steps
    spliced.var["gene_symbols"] = gene_symbols

    # Store both matrices as named layers
    spliced.layers["spliced"]   = spliced.X.copy()
    spliced.layers["unspliced"] = unspliced.X.copy()

    return spliced
```

> 

**About barcode formatting:** STARsolo outputs raw 16-bp barcodes with no suffix (e.g., `AAACCCACAAGCGATG`). Seurat appends `-1` to every barcode and prepends the sample ID after `merge()`, producing `Healthy_1_AAACCCACAAGCGATG-1`. The `load_velocyto_mtx()` function applies both transformations — sample prefix and `-1` suffix — so the resulting barcodes match exactly what is stored in the Seurat metadata. If your Seurat barcodes use a different format, inspect them first with `head(rownames(seurat_obj@meta.data))` in R and adjust the `prefixed_barcodes` line accordingly.

### 3.5 — Merge All Samples and Transfer Seurat Annotations

Load all 8 samples and concatenate them into a single AnnData:

```r
# Sample IDs — must match the prefixes in your Seurat barcodes
sample_ids  = ["Healthy_1", "Healthy_2", "Healthy_3", "Healthy_4",
               "Post_Patient_1", "Post_Patient_2", "Post_Patient_3", "Post_Patient_4"]
sample_dirs = [f"results/velocity/{sid}" for sid in sample_ids]

# Load all samples
ldata_list = [load_velocyto_mtx(d, s) for d, s in zip(sample_dirs, sample_ids)]

# Save gene symbol mapping before concat — ad.concat() drops var metadata columns
gene_symbol_map = ldata_list[0].var["gene_symbols"]

# Concatenate; join="inner" retains only genes present in every sample
ldata = ad.concat(ldata_list, join="inner")

# Restore gene symbols (reindex to the inner-join gene set)
ldata.var["gene_symbols"] = gene_symbol_map.reindex(ldata.var_names)
```

Now build the main working object by intersecting velocity data with Seurat-annotated cells and attaching all metadata:

```r
# Retain only cells present in both the Seurat metadata and the velocity data
# Cells that failed QC in Part 2 or were not annotated in Part 4 are excluded here
shared_barcodes = meta_df.index.intersection(ldata.obs_names)

adata = ldata[shared_barcodes].copy()       # velocity data for QC-passed, annotated cells
adata.obs = meta_df.loc[shared_barcodes]    # transfer Seurat metadata (cell type, condition, etc.)

# Transfer UMAP coordinates into the standard AnnData embedding slot
adata.obsm["X_umap"] = umap_df.loc[shared_barcodes, ["UMAP_1", "UMAP_2"]].values

# Switch var_names from Ensembl IDs to gene symbols for readable plots and tables
# Ensembl IDs were used during loading and concatenation to avoid duplicate label errors;
# now that the object is built we can switch to symbols.
# adata.var_names_make_unique() appends -1, -2 etc. to any duplicate symbol names
adata.var_names = pd.Index(adata.var["gene_symbols"])
adata.var_names_make_unique()

print(f"Cells retained after intersection: {adata.n_obs}")
print(f"Genes:   {adata.n_vars}")
print(f"Layers:  {list(adata.layers.keys())}")
print(f"\nCell types present:\n{adata.obs['final_annotation'].value_counts()}")
```

**Expected output:**

```
Cells retained after intersection: 7100
Genes:   62757
Layers:  ['spliced', 'unspliced']

Cell types present:
final_annotation
CD4+ T cells           3069
NK cells               1590
T cells                 761
B cells                 664
Classical Monocytes     515
CD8+ T cells            308
Monocytes               193
```

Of the 7,262 Seurat-annotated cells, **7,100 are retained** after the barcode intersection. The 162 cells that drop out were present in the Seurat metadata but had no reads counted by STARsolo in the Velocyto output — typically because they contained very few intronic reads under the stricter STARsolo barcode-calling threshold. This is a normal and expected small attrition. All 7 cell types are represented, with CD4+ T cells being the most abundant population as expected in a PBMC dataset.

At this point your AnnData object contains:

- `adata.layers["spliced"]` — spliced count matrix (cells x genes)

- `adata.layers["unspliced"]` — unspliced count matrix (cells x genes)

- `adata.var_names` — gene symbols (e.g. `S100A8`, `LYZ`) — unique, readable

- `adata.var["gene_symbols"]` — original gene symbols before deduplication suffix

- `adata.obs["final_annotation"]` — cell type labels from Part 4

- `adata.obs["condition"]` — Healthy / Post_Treatment

- `adata.obs["sample_id"]` — sample of origin

- `adata.obsm["X_umap"]` — 2D UMAP layout from Seurat

---

## Step 4: Preprocessing and Moment Estimation

### Why Preprocessing Is Necessary

Raw spliced and unspliced counts are noisy. Lowly expressed genes are dominated by technical dropout; cells differ in sequencing depth. Before estimating velocity, we need to:

1. **Filter genes** — keep only those with sufficient counts in both spliced and unspliced matrices, ensuring reliable kinetic signal

2. **Filter cells** — remove any cells with zero total counts to prevent division errors during normalization

3. **Normalize** — scale each cell to 10,000 total counts, removing library size differences between cells

4. **Log-transform** — compress the dynamic range of normalized counts

5. **Select variable genes** — focus on the most dynamically informative genes

The `filter_genes` and `filter_cells` functions in scVelo and Scanpy handle steps 1–2; the remaining steps use Scanpy’s stable preprocessing functions directly.

```python
import numpy as np
from scipy.sparse import issparse

# Step 1: Filter genes requiring a minimum number of shared (spliced + unspliced) counts
scv.pp.filter_genes(adata, min_shared_counts=20)

# Step 2: Remove cells with zero total counts
sc.pp.filter_cells(adata, min_counts=1)

# Step 3: Normalize each cell to exactly 10,000 total counts
# This produces bounded values that cannot overflow during HVG mean computation,
# unlike scv.pp.normalize_per_cell which normalizes to the median library size
# and can produce very large values for lowly-sequenced cells
sc.pp.normalize_total(adata, target_sum=1e4)

# Step 4: Log-transform the normalized counts
sc.pp.log1p(adata)

# Step 5: Select the top 2000 most variably expressed genes
sc.pp.highly_variable_genes(adata, n_top_genes=2000, flavor="seurat")
adata = adata[:, adata.var.highly_variable].copy()

# Build a k-nearest neighbor (kNN) graph in PCA space
# This graph defines each cell's local neighborhood for moment estimation
sc.pp.neighbors(adata, n_neighbors=30, n_pcs=30)

# Compute first and second moments: weighted mean and variance of spliced/unspliced
# counts across each cell's kNN neighborhood
# This smoothing step compensates for single-cell dropout
scv.pp.moments(adata, n_pcs=30, n_neighbors=30)
```

> 

**What are moments?** In statistics, a “moment” is a summary measure of a distribution (the first moment is the mean; the second is the variance). In scVelo, “computing moments” means replacing each cell’s raw spliced/unspliced count with the *weighted average* of counts across that cell’s nearest neighbors in PCA space. Because dropout causes many genuine expression values to appear as zero in a single cell, averaging over 30 similar neighbors gives a much more reliable estimate of the cell’s true transcriptional state. This is conceptually similar to the imputation step used by some other tools, but grounded in the local cell neighborhood.

---

## Step 5: Estimate RNA Velocity — Fitting the Dynamical Model

### Choosing a Velocity Model

scVelo offers three velocity estimation models:

`steady_state``stochastic``dynamical`

| Model | Speed | Accuracy | When to Use |
| --- | --- | --- | --- |
|  | Fastest (seconds) | Lowest — assumes each gene is in a single steady state | Quick exploration only |
|  | Fast (minutes) | Moderate — adds noise estimation to steady-state model | Datasets with shallow sequencing |
|  | Slow (30–90 min) | Highest — full kinetic model with induction and repression phases | Recommended for all publication-quality analyses |

We use the **dynamical model** throughout this tutorial. It is the only model that enables latent time inference and differential kinetics testing, which we perform in Steps 6 and 7.

### Step 5.1 — Recover Gene Dynamics

The `recover_dynamics` function fits the full kinetic model to every selected gene. For each gene, it estimates four parameters:

- **α (alpha)**: transcription rate during the induction phase

- **β (beta)**: splicing rate (pre-mRNA → mature mRNA conversion rate)

- **γ (gamma)**: degradation rate (mature mRNA decay rate)

- **t_s**: switching time (when the gene transitions from induction to repression)

```bash
# Fit the dynamical model — the most compute-intensive step in the workflow
# Estimates transcription, splicing, and degradation rates for each gene
scv.tl.recover_dynamics(adata, n_jobs=8)
```

> 

This is the most time-consuming step. Save a checkpoint immediately after completion to avoid re-running:

```bash
adata.write("adata_dynamics.h5ad")  # save checkpoint
# adata = sc.read("adata_dynamics.h5ad")  # reload if needed
```

### Step 5.2 — Compute Velocity and Velocity Graph

```bash
# Compute RNA velocity for each cell using the fitted dynamical parameters
scv.tl.velocity(adata, mode="dynamical")

# Project velocity into the cell-cell transition graph
# For each cell, this computes the probability of transitioning to each of its neighbors
# based on whether neighboring cells' expression is similar to the current cell's predicted future state
scv.tl.velocity_graph(adata)
```

---

## Step 6: Project and Visualize RNA Velocity

### 6.1 — Velocity Stream Plot

The velocity stream plot is the signature visualization of RNA velocity analysis. It overlays a smoothed directional flow field on the UMAP embedding, showing the collective movement of cells through transcriptional space. Dense, coherent streams indicate populations actively transitioning between states; circular or incoherent patterns suggest stable steady states.

```bash
# Smooth stream plot: averaged velocity field over all cells, colored by cell type
scv.pl.velocity_embedding_stream(
    adata,
    basis="umap",
    color="final_annotation",
    legend_loc="right margin",
    title="RNA Velocity — All Conditions",
    figsize=(10, 7),
    save="velocity_stream_all.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/scvelo_velocity_stream_all.png?resize=1024%2C597&ssl=1)

> 

**Interpreting this plot:** Look for coherent, directional stream arrows in the Classical Monocyte and T cell clusters — these are the most transcriptionally dynamic populations in PBMCs and should show the strongest velocity signal. CD4+ T cells (the dominant population at 3,069 cells) may show diverging streams reflecting internal heterogeneity between resting and activated states. NK cells and B cells are more terminally differentiated and typically show short, diffuse arrows — this is biologically expected.

#### Understanding What Velocity Arrows Actually Mean

A common source of confusion in RNA velocity plots is the assumption that arrows indicate **lineage relationships** — that cells in cluster A are differentiating into cells in cluster B because the arrows point in that direction. This is a misinterpretation.

Each arrow represents the **predicted future transcriptional state** of a cell, derived from the balance between unspliced (pre-mRNA) and spliced (mature mRNA) counts. When a gene has more unspliced RNA than expected for its current spliced level, it is being actively induced. Summed across thousands of genes, this produces a velocity vector pointing toward where that cell’s transcriptome is headed in the near future.

Arrows therefore reflect **local transcriptional dynamics within UMAP space**. When arrows from an NK cell cluster point toward T cell space, it means those NK cells’ current gene expression dynamics are oriented toward the transcriptional profile of the neighboring T cell cluster — not that NK cells are becoming T cells. This is largely a consequence of UMAP geometry: NK cells and T cells share many lymphocyte genes and sit close together in reduced dimensional space, so their velocity vectors naturally orient toward each other.

> 

**Key takeaway:** Arrows tell you where a cell’s transcriptome is heading right now. Biological interpretation of what that means for cell identity or developmental potential requires domain knowledge and should always be evaluated in the context of your specific biology.

### 6.2 — Per-Cell Velocity Arrows

For a less smoothed view that shows each cell’s individual velocity vector:

```bash
# One arrow per cell — useful for inspecting local velocity patterns
scv.pl.velocity_embedding(
    adata,
    basis="umap",
    arrow_length=3,
    arrow_size=1.5,
    color="final_annotation",
    title="Per-Cell Velocity Arrows",
    save="velocity_arrows_all.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/scvelo_velocity_arrows_all.png?resize=1024%2C849&ssl=1)

> 

**Interpreting this plot:** Per-cell arrows are noisier than stream plots but useful for identifying outlier cells with anomalous velocity directions. Cells at cluster boundaries with arrows pointing away from their assigned cluster are candidates for transitioning states. In a PBMC dataset, you may notice a subset of T cells or Monocytes with arrows directed toward each other’s cluster — this can reflect activated states bridging the two lineages.

### 6.3 — Compute and Visualize Latent Time

Latent time is scVelo’s kinetics-derived pseudotime. Unlike the pseudotime in [Parts 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/) and [7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/), which required manual root selection, latent time is computed entirely from the RNA kinetic model — the cell that is most consistent with being at the beginning of the transcriptional program (highest unspliced/spliced ratio across velocity genes) is automatically identified as the root.

```r
# Infer latent time from the dynamical model (no manual root specification needed)
scv.tl.latent_time(adata)

# Visualize latent time on the Seurat UMAP
scv.pl.scatter(
    adata,
    color="latent_time",
    color_map="gnuplot",
    size=80,
    title="Latent Time",
    save="latent_time.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/scvelo_latent_time.png?resize=534%2C417&ssl=1)

> 

**Interpreting this plot:** Latent time is a continuous value from 0 (transcriptional start) to 1 (transcriptional end) derived purely from RNA kinetics. Cells with low latent time (dark colors) are at the beginning of the transcriptional program — they have high unspliced/spliced ratios, meaning genes are actively being turned on. Cells with high latent time (bright colors) have reached near-steady-state expression. Compare this ordering to the Monocle 3 and Slingshot pseudotime plots from [Parts 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/) and [7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/): they should broadly agree on which populations are early and which are late. Disagreements are worth investigating — they often point to populations with complex, non-linear dynamics.

### 6.4 — Phase Portrait of Top Velocity Genes

A **phase portrait** plots unspliced RNA (y-axis) against spliced RNA (x-axis) for a single gene. The fitted kinetic curve divides the phase space into two regions:

- **Above the curve** — unspliced exceeds the steady-state prediction → gene is being **induced** (cells are producing more transcript)

- **Below the curve** — unspliced is below the steady-state prediction → gene is being **repressed** (transcription has slowed)

Cells rotate through this phase space over biological time, and the direction of rotation is the velocity signal for that gene.

```bash
# Identify the top 10 genes with the best-fitting kinetic model (highest likelihood)
top_genes = (
    adata.var["fit_likelihood"]
    .sort_values(ascending=False)
    .head(10)
    .index.tolist()
)

# Phase portrait: spliced (x) vs. unspliced (y) for each top velocity gene
# The fitted kinetic curve is shown in black; cells are colored by cell type
scv.pl.scatter(
    adata,
    var_names=top_genes,
    color="final_annotation",
    ncols=5,
    figsize=(18, 6),
    save="phase_portraits_top10.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/scvelo_phase_portraits_top10.png?resize=1024%2C146&ssl=1)

> 

**Interpreting this plot:** A well-behaved phase portrait shows cells forming a clear arc or loop shape, with the fitted black curve passing cleanly through the data. Genes with high `fit_likelihood` (> 0.3) have the most reliable kinetic signal — their portraits will show tight, well-separated induction and repression arms. Genes with low likelihood (< 0.1) will show scattered, noisy portraits; these genes contribute little to the overall velocity estimate.

**Reading the three curves:** Each plot contains up to three elements. The **dotted diagonal line** is the steady-state line — the theoretical spliced/unspliced ratio if the gene were in perfect equilibrium. The **upper curve** is the induction arm: cells here have more unspliced RNA than expected, meaning the gene is actively being transcribed. The **lower curve** is the repression arm: cells here have less unspliced RNA than expected, meaning transcription has slowed and spliced RNA is decaying. Cells travel around this loop counterclockwise over biological time — up the induction arm, then back down the repression arm toward the origin. The position of a cell relative to the steady-state line is what determines the sign and magnitude of its velocity for that gene.

**Why some genes show only one arm:** Whether both arms are visible depends on which phase the captured cells happen to be in at the moment of collection. A gene only showing the upper curve means all cells are mid-induction — possibly an immune activation gene being turned on in the disease context. A gene only showing the lower curve means transcription has already shut off and spliced RNA is decaying — possibly a gene being suppressed post-treatment. A single arm is biologically meaningful, not a failure; scVelo fits the ODE to whichever portion of the phase space is populated and the velocity estimate remains valid.

### 6.5 — Velocity Patterns Per Condition

To see how velocity patterns differ between the two conditions, create separate stream plots for each:

```
for cond in ["Healthy", "Post_Treatment"]:
    subset = adata[adata.obs["condition"] == cond].copy()
    scv.pl.velocity_embedding_stream(
        subset,
        basis="umap",
        color="final_annotation",
        title=f"RNA Velocity — {cond}",
        figsize=(9, 6),
        save=f"velocity_stream_{cond}.png"
    )
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/velocity_stream_healthy_posttreatment-e1777434186156-1024x358.png?resize=1024%2C358&ssl=1)

> 

**Interpreting this plot:** Compare the two plots side by side. In the **Healthy** condition, velocity streams in Classical Monocytes and T cells reflect homeostatic transcriptional programs — arrows are present but relatively short and diffuse. In the **Post_Treatment** condition, you may observe altered stream directions or magnitudes in monocyte populations, consistent with the transcriptional remodeling that follows periodontal therapy. If velocity patterns look very similar between conditions, this is still informative — it suggests that one month post-treatment, the transcriptional kinetics of circulating PBMCs have largely normalized, which aligns with the clinical expectation of periodontal therapy reducing systemic inflammation gradually.

### 6.6 — Velocity Confidence Score

`scv.tl.velocity_confidence` computes two complementary per-cell metrics that together describe both the speed and the directionality of each cell’s transcriptional change.

```bash
# Compute per-cell velocity magnitude (length) and directional consistency (confidence)
scv.tl.velocity_confidence(adata)

# Visualize both metrics side by side on the UMAP
scv.pl.scatter(
    adata,
    color=["velocity_length", "velocity_confidence"],
    cmap="coolwarm",
    perc=[5, 95],      # clip extreme values for robust color scaling
    ncols=2,
    figsize=(14, 5),
    save="velocity_confidence.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/scvelo_velocity_confidence.png?resize=1024%2C195&ssl=1)

**What the two metrics mean:**`velocity_length` is the magnitude of the velocity vector — the root mean square of per-gene velocity values across all velocity genes for that cell. It measures how fast the cell is changing its transcriptional state. `velocity_confidence` is the average cosine correlation between the cell’s velocity vector and the vectors pointing toward each of its k-nearest neighbors. It measures whether the velocity direction is coherent — whether all the neighbors a cell is heading toward actually lie in that predicted direction.

The two metrics are independent:

`velocity_length``velocity_confidence`

|  |  | Interpretation |
| --- | --- | --- |
| High | High | Actively and directionally transitioning — most informative cells |
| High | Low | Changing fast but incoherently — possible branching point or noise |
| Low | High | Changing slowly but consistently — near steady state |
| Low | Low | Transcriptionally stable — resting or terminally differentiated |

**In the PBMC dataset:** Classical Monocytes and activated T cells should show the highest `velocity_length` values — they are the most transcriptionally dynamic populations. NK cells and resting B cells will show low `velocity_length`; this is biologically correct, not a failure. The generic “T cells” cluster may show lower `velocity_confidence` than Classical Monocytes because it contains heterogeneous subsets pulling the velocity vector in different directions. If essentially all cells show near-zero `velocity_length`, verify both layers are non-empty with `adata.layers["spliced"].sum()` and `adata.layers["unspliced"].sum()`.

---

## Step 7: Differential Kinetics Analysis Between Conditions

### Why Go Beyond Visual Comparison?

The condition-stratified stream plots in Step 6.5 provide a qualitative view of velocity differences. To make these observations statistically rigorous, scVelo provides a **differential kinetics test**: a likelihood ratio test that determines, for each gene, whether fitting condition-specific kinetic parameters (α, β, γ) significantly improves the model fit compared to a single shared parameter set.

A gene that passes this test has significantly different transcriptional dynamics in at least one condition — it may be transcribed faster, spliced more slowly, or degraded at a different rate in disease versus health.

### 7.1 — Run the Differential Kinetics Test

In scVelo, differential kinetics between conditions is tested with `scv.tl.differential_kinetic_test`, which performs a likelihood ratio test for each gene: it compares the fit of a per-condition kinetic model against a single shared model and writes the result to `adata.var["fit_pval_kinetics"]`.

```bash
# Select the top velocity genes by likelihood to test
# Only genes where the dynamical model converged are worth testing
top_velocity_genes = (
    adata.var["fit_likelihood"]
    .dropna()
    .sort_values(ascending=False)
    .head(300)
    .index.tolist()
)

# Test whether kinetic parameters differ between conditions
scv.tl.differential_kinetic_test(
    adata,
    var_names=top_velocity_genes,
    groupby="condition"
)
```

```python
# Confirm results were written
print(f"fit_pval_kinetics non-NaN: {adata.var['fit_pval_kinetics'].notna().sum()}")
print(adata.var["fit_pval_kinetics"].describe())
```

**Expected output:**

```
fit_pval_kinetics non-NaN: 29
fit_diff_kinetics non-NaN: 0
count    2.900000e+01
mean     2.208357e-03
std      3.196344e-03
min      1.907541e-13
25%      3.835431e-06
50%      3.491201e-04
75%      4.028695e-03
max      9.438468e-03
```

Of the 2,000 HVGs, only the **29 genes** where the dynamical model converged with high confidence have a valid `fit_pval_kinetics`. The p-values range from 1.9×10⁻¹³ to 9.4×10⁻³ — every single tested gene is significant at p < 0.05, and the majority are highly significant (median p = 3.5×10⁻⁴). This tells us that the 29 genes for which scVelo successfully fitted a full kinetic model are genuinely dynamic — they show clear evidence of active transcriptional induction and repression in this dataset.

### 7.2 — Inspect and Visualize Results

```python
# Coerce to numeric to handle any non-numeric entries from unconverged genes
adata.var["fit_pval_kinetics"] = pd.to_numeric(
    adata.var["fit_pval_kinetics"], errors="coerce"
)

tested = adata.var["fit_pval_kinetics"].dropna()
print(f"Genes tested:   {len(tested)}")
print(f"p < 0.05:       {(tested < 0.05).sum()}")
print(f"p < 0.20:       {(tested < 0.20).sum()}")
print(f"Min p-value:    {tested.min():.4f}")
print(f"Median p-value: {tested.median():.4f}")
```

**Expected output:**

```
Genes tested:   29
p &lt; 0.05:       29
p &lt; 0.20:       29
Min p-value:    0.0000
Median p-value: 0.0004
```

```python
# Plot p-value distribution — a left spike near 0 confirms strong kinetic signal
import matplotlib.pyplot as plt

plt.figure(figsize=(7, 4))
tested.hist(bins=20, color="steelblue", edgecolor="white")
plt.xlabel("p-value (fit_pval_kinetics)")
plt.ylabel("Number of genes")
plt.title("Differential Kinetics p-value Distribution")
plt.tight_layout()
plt.savefig("figures/diff_kinetics_pvalue_dist.png")
plt.show()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/diff_kinetics_pvalue_dist.png?resize=1024%2C566&ssl=1)

> 

**Interpreting the p-value histogram:** All 29 tested genes cluster near p = 0, producing a strong left spike. This is the ideal outcome — a small but highly confident set of genes with clear kinetic patterns. The histogram will not look uniform because essentially all tested genes are significant.

```python
# Rank all tested genes by p-value and show the top 20
diff_kinetics_df = (
    adata.var[["fit_likelihood", "fit_pval_kinetics"]]
    .dropna(subset=["fit_pval_kinetics"])
    .sort_values("fit_pval_kinetics")
)

print(diff_kinetics_df.head(20))

# Top 9 genes for visualization
top_diff = diff_kinetics_df.head(9).index.tolist()
```

**Expected output (top 5):**

```
              fit_likelihood  fit_pval_kinetics
gene_symbols                                   
SPOCK2              0.258998       1.878376e-10
CTLA4               0.170455       3.219516e-10
LYST                0.259099       5.227083e-10
JUN                 0.214615       1.694328e-08
POU2F2              0.267073       3.846455e-08
```

> 

**Interpreting the ranked gene table:** The top-ranked genes have `fit_likelihood` values between 0.17 and 0.28 — good model fits typical for human PBMC data, where kinetic patterns are subtler than in developmental datasets. The extremely small p-values (down to 10⁻¹³) confirm strong evidence of transcriptional dynamics.

Visualize the top differentially kinetic genes as phase portraits, colored by condition:

```bash
# Phase portraits for top differentially kinetic genes, split by condition
# palette must be a list ordered to match adata.obs["condition"].cat.categories
scv.pl.scatter(
    adata,
    var_names=top_diff,
    color="condition",
    palette=["#2196F3", "#FF5722"],   # Healthy=blue, Post_Treatment=orange-red
    ncols=3,
    figsize=(15, 12),
    legend_loc="right margin",
    save="differential_kinetics_top9.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/scvelo_differential_kinetics_top9.png?resize=1024%2C782&ssl=1)

> 

**Interpreting this plot:** Each panel shows the spliced vs. unspliced phase portrait for one top-ranked gene, with cells colored by condition (Healthy vs. Post_Treatment). A gene with differential kinetics will show the two condition colors occupying different regions of the phase space — for example, Post_Treatment cells may cluster in the induction arm (above the curve) while Healthy cells sit in the repression arm (below the curve), indicating the gene is more actively transcribed after therapy. Genes where both conditions completely overlap have shared kinetics and would not be significant.

Visualize where these genes are expressed on the UMAP, colored by velocity:

```bash
# Velocity plots for differentially kinetic genes
# Arrows are colored by the per-gene velocity magnitude for each top gene
scv.pl.velocity(
    adata,
    var_names=top_diff[:4],
    basis="umap",
    ncols=2,
    figsize=(14, 10),
    save="differential_kinetics_velocity.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/scvelo_differential_kinetics_velocity.png?resize=1024%2C258&ssl=1)

### 7.3 — Compare Latent Time Distributions Between Conditions

Another way to quantify condition differences is to compare the distribution of latent time values across conditions within each cell type. Cells with higher latent time in the disease condition relative to healthy controls may represent more progressed activation states:

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Build a tidy DataFrame for plotting
lt_df = adata.obs[["final_annotation", "condition", "latent_time"]].copy()

# Violin plot: latent time per cell type, split by condition
fig, ax = plt.subplots(figsize=(14, 6))
sns.violinplot(
    data=lt_df,
    x="final_annotation",
    y="latent_time",
    hue="condition",
    palette="Set2",
    inner="box",
    ax=ax
)
ax.set_xticklabels(ax.get_xticklabels(), rotation=30, ha="right")
ax.set_title("Latent Time Distribution by Cell Type and Condition")
ax.set_xlabel("Cell Type")
ax.set_ylabel("Latent Time")
plt.tight_layout()
plt.savefig("figures/latent_time_by_condition.png")
plt.show()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/04/latent_time_by_condition.png?resize=1024%2C413&ssl=1)

> 

**Interpreting this plot:** Each violin shows the latent time distribution for a cell type, split by condition. A shifted violin — where one condition’s cells have systematically higher or lower latent time than the other — suggests that condition alters where cells sit in the transcriptional program. In a PBMC dataset, Classical Monocytes are most likely to show a condition-dependent latent time shift, as they are the most responsive circulating immune cell to periodontal inflammation. If Healthy monocytes show higher latent time than Post_Treatment monocytes, this suggests post-treatment cells are transcriptionally “earlier” — consistent with a return toward a less activated, progenitor-like state after therapy. For cell types with wide, uniform distributions (T cells, B cells), the latent time model may not be capturing a single coherent trajectory, which is expected for highly heterogeneous populations.

**Comparing to the differential kinetics results:** Genes identified by `fit_pval_kinetics` as kinetically dynamic (Step 7.1–7.2) are the molecular drivers underlying any latent time shifts you observe here. The two analyses are complementary: the kinetics test identifies *which genes* are most dynamic; the latent time violin shows *where in the transcriptional program* cells from each condition are concentrated.

---

## Best Practices and Common Pitfalls

### Best Practices

**1. Always verify barcode format before merging**

The most frequent failure in this workflow is a barcode mismatch between the STARsolo output and the Seurat metadata. Before running the full pipeline, print a few barcodes from each source and confirm the format matches exactly.

**2. Use the dynamical model for any quantitative interpretation**

The stochastic model is useful for rapid exploration, but only the dynamical model provides the kinetic parameters needed for latent time and differential kinetics testing. The extra computation is worth it.

**3. Save checkpoints after expensive steps**

`recover_dynamics` and `velocity_graph` are the two most time-consuming steps. Save the AnnData object after each:

```
adata.write("adata_after_dynamics.h5ad")
adata.write("adata_after_velocity_graph.h5ad")
```

**4. Interpret weak velocity signals biologically, not as failures**

Terminally differentiated cells (B cells, NK cells) have stable transcriptomes and will genuinely show low velocity magnitudes and low confidence. This is a correct biological result. Focus your interpretation on dynamically active populations.

**5. Cross-validate latent time with pseudotime**

Compare scVelo’s latent time to the Monocle 3 / Slingshot pseudotime from Parts 7 and 7-2. Strong concordance validates both analyses; disagreement warrants investigation of the underlying biology.

### Common Pitfalls

**Pitfall 1: Zero unspliced counts**

If `adata.layers["unspliced"].sum()` is zero or very small, the STARsolo Velocyto output was not loaded correctly. Check that the path to `Solo.out/Velocyto/filtered/unspliced/matrix.mtx` is correct and that the file is non-empty.

**Pitfall 2: Very few cells pass the barcode intersection**

If `shared_barcodes` contains far fewer cells than expected (less than 50% of Seurat cells), the barcode prefix format does not match. Print both sets of barcodes side by side and identify the discrepancy:

```r
# Inspect a few barcodes from each source to diagnose mismatches
print("Seurat barcodes (sample):", meta_df.index[:5].tolist())
print("STARsolo barcodes (sample):", ldata.obs_names[:5].tolist())
```

**Pitfall 3: `recover_dynamics` runs but most genes have low likelihood**

This can indicate that `filter_and_normalize` was too strict (try lowering `min_shared_counts`) or that the data has very shallow unspliced coverage (check the sequencing read structure — unspliced reads from introns require that reads are long enough to map into intronic regions).

**Pitfall 4: Incoherent velocity arrows across all cell types**

If all cells show random, incoherent velocity arrows, the kNN graph or moments may not have been computed on the correct PCA embedding. Ensure `sc.pp.neighbors()` was run on the same data as `scv.pp.moments()`.

---

## Key Takeaways

**1. RNA velocity reveals direction, not just position**

Every previous analysis in this series described where cells are in transcriptional space. RNA velocity tells you where they are *going*. This directional information is qualitatively different and opens up biological questions that static analyses cannot address.

**2. Unspliced RNA is a clock, not noise**

Pre-mRNA was long regarded as a technical artifact in scRNA-seq data. RNA velocity reframes it as a biological signal — one that encodes the current transcriptional activity of the cell and allows us to infer its future state.

**3. The dynamical model is worth the computational cost**

The stochastic model is fast, but its kinetic parameters are not interpretable in the same way. Only the dynamical model provides per-gene transcription, splicing, and degradation rates that can be statistically compared between conditions.

**4. Weak velocity in stable cells is biology, not failure**

B cells and NK cells have stable transcriptional programs and will genuinely show weak, noisy velocity. Classical Monocytes and T cells — the most dynamically responsive populations in PBMCs — are where velocity analysis is most biologically informative in this dataset.

**5. Python and R are complementary — not competing — tools**

Seurat (R) remains the gold standard for integration, clustering, and cell type annotation. scVelo, CellRank, and Scanpy (Python) extend the toolkit into kinetics and fate probability analysis. The workflow developed here — exporting Seurat annotations into AnnData — is a robust bridge between the two ecosystems.

**6. Barcode harmonization is the most error-prone step**

Always inspect barcode formats from both sources before merging. A mismatch here silently discards most of your data without raising an obvious error.

---

## References

1. Bergen V, Lange M, Peidli S, Wolf FA, Theis FJ. Generalizing RNA velocity to transient cell states through dynamical modeling. *Nature Biotechnology*. 2020;38(12):1408-1414. doi:10.1038/s41587-020-0591-3 [scVelo — primary citation]

2. La Manno G, Soldatov R, Zeisel A, et al. RNA velocity of single cells. *Nature*. 2018;560(7719):494-498. doi:10.1038/s41586-018-0414-6 [Original RNA velocity concept]

3. Bergen V, Soldatov RA, Kharchenko PV, Theis FJ. RNA velocity — current challenges and future perspectives. *Molecular Systems Biology*. 2021;17(8):e10282. doi:10.15252/msb.202110282 [RNA velocity review and benchmarking]

4. Kaminow B, Yunusov D, Dobin A. STARsolo: accurate, fast and versatile mapping/quantification of single-cell and single-nucleus RNA-seq data. *bioRxiv*. 2021. doi:10.1101/2021.05.05.442755 [STARsolo — velocity counting]

5. Dobin A, Davis CA, Schlesinger F, et al. STAR: ultrafast universal RNA-seq aligner. *Bioinformatics*. 2013;29(1):15-21. doi:10.1093/bioinformatics/bts635 [STAR aligner]

6. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. *Journal of Translational Medicine*. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset — GSE174609]

7. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

8. Virshup I, Rybakov S, Theis FJ, Angerer P, Wolf FA. anndata: Access and store annotated data matrices. *Journal of Open Source Software*. 2024;9(101):4371. doi:10.21105/joss.04371 [AnnData]

9. Wolf FA, Angerer P, Theis FJ. SCANPY: large-scale single-cell gene expression data analysis. *Genome Biology*. 2018;19(1):15. doi:10.1186/s13059-017-1382-0 [Scanpy]

10. Lange M, Bergen V, Klein M, et al. CellRank for directed single-cell fate mapping. *Nature Methods*. 2022;19(2):159-170. doi:10.1038/s41592-021-01346-6 [CellRank — recommended next step]

11. Cao J, Spielmann M, Qiu X, et al. The single-cell transcriptional landscape of mammalian organogenesis. *Nature*. 2019;566(7745):496-502. doi:10.1038/s41586-019-0969-x [Monocle 3 — Part 7 reference]

12. Street K, Risso D, Fletcher RB, et al. Slingshot: cell lineage and pseudotime inference for single-cell transcriptomics. *BMC Genomics*. 2018;19(1):477. doi:10.1186/s12864-018-4772-0 [Slingshot — Part 7-2 reference]

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
