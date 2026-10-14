# How to Analyze Single-Cell RNA-seq Data from Patient-Derived Xenograft (PDX) Models — Complete Beginner's Guide Part 8: Processing Human-Mouse Mixed Samples

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/)

- [Introduction: What Makes PDX Single-Cell Data Unique?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Introduction_What_Makes_PDX_Single-Cell_Data_Unique)

  - [What Are Patient-Derived Xenograft (PDX) Models?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#What_Are_Patient-Derived_Xenograft_PDX_Models)
  - [What Are the Challenges of Analyzing PDX scRNA-seq Data?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#What_Are_the_Challenges_of_Analyzing_PDX_scRNA-seq_Data)
  - [What Is the Workflow for Analyzing PDX scRNA-seq Data?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#What_Is_the_Workflow_for_Analyzing_PDX_scRNA-seq_Data)
  - [How Do We Separate Human and Mouse Cells?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#How_Do_We_Separate_Human_and_Mouse_Cells)

- [Setting Up Your Environment for PDX Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Setting_Up_Your_Environment_for_PDX_Analysis)

  - [Reusing the Conda Environment from Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Reusing_the_Conda_Environment_from_Part_1)
  - [Installing Singularity](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Installing_Singularity)
  - [Installing XenoCell](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Installing_XenoCell)
  - [Setting Up Your Project Directory](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Setting_Up_Your_Project_Directory)

- [Strategy 1: Align First, Separate Later with Cell Ranger](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Strategy_1_Align_First_Separate_Later_with_Cell_Ranger)

  - [Step 1.1: Run Cell Ranger with the Combined Reference Genome](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_11_Run_Cell_Ranger_with_the_Combined_Reference_Genome)
  - [Step 1.2: Understanding gem_classification.csv — Your Species Map](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_12_Understanding_gem_classificationcsv_%E2%80%94_Your_Species_Map)
  - [Step 1.3: Calculating and Plotting Mouse Contamination](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_13_Calculating_and_Plotting_Mouse_Contamination)
  - [Step 1.4: Subsetting the Combined Matrix by Species](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_14_Subsetting_the_Combined_Matrix_by_Species)

- [Strategy 2: Separate First, Align Later with XenoCell](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Strategy_2_Separate_First_Align_Later_with_XenoCell)

  - [Step 2.1: How XenoCell Works — A Conceptual Overview](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_21_How_XenoCell_Works_%E2%80%94_A_Conceptual_Overview)
  - [Step 2.2: Building the Xenome Index](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_22_Building_the_Xenome_Index)
  - [Step 2.3: Classifying Reads](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_23_Classifying_Reads)
  - [Step 2.4: Extracting Species-Specific FASTQ Files](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_24_Extracting_Species-Specific_FASTQ_Files)
  - [Step 2.5: Running Cell Ranger on Species-Specific FASTQ Files](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Step_25_Running_Cell_Ranger_on_Species-Specific_FASTQ_Files)

- [Comparing the Two Strategies: Which Should You Choose?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Comparing_the_Two_Strategies_Which_Should_You_Choose)
- [Downstream Analysis: What to Do with Your Species-Specific Count Matrices](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Downstream_Analysis_What_to_Do_with_Your_Species-Specific_Count_Matrices)

  - [For Human Tumor Cell Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#For_Human_Tumor_Cell_Analysis)
  - [For Mouse Host Cell Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#For_Mouse_Host_Cell_Analysis)

- [Best Practices for PDX scRNA-seq Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Best_Practices_for_PDX_scRNA-seq_Analysis)
- [Conclusion](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-from-patient-derived-xenograft-pdx-models-complete-beginners-guide-part-8-processing-human-mouse-mixed-samples/#Like_this)

## Introduction: What Makes PDX Single-Cell Data Unique?

### What Are Patient-Derived Xenograft (PDX) Models?

If you’ve followed Parts 1–7 of this series, you’ve been working with single-species scRNA-seq data — cells from one organism, aligned to one reference genome. Part 8 introduces a fundamentally different type of sample: **Patient-Derived Xenograft (PDX) models**.

In a PDX experiment, **fresh human tumor tissue is surgically implanted into immunodeficient mice**. The human tumor cells engraft, grow, and can be expanded while preserving the original tumor’s genetic and cellular heterogeneity far better than cell lines grown in a dish. Researchers then harvest the tumor from the mouse, dissociate it into single cells, and run it through a standard 10x Genomics scRNA-seq workflow.

The result? A sample containing a **mixture of two species**: human tumor cells (the “graft”) and mouse stromal, immune, and endothelial cells (the “host”). This is sometimes called a **xenograft** sample — “xeno” from Greek, meaning “foreign.”

PDX models are widely used in cancer research for several reasons:

- **Preserve tumor heterogeneity**: Unlike cell lines, PDX tumors maintain the diverse cell states and subclones found in the original patient tumor.

- **Enable in vivo drug testing**: Researchers can treat PDX-bearing mice with candidate drugs and study single-cell responses.

- **Capture the tumor microenvironment**: The mouse host provides a living stromal and immune context, allowing scientists to study how host cells interact with human tumor cells.

- **Study tumor evolution**: Tumors can be passaged through multiple mouse generations and profiled at each stage to track clonal dynamics.

> 

**Analogy for beginners:** Imagine transplanting a piece of a patient’s garden (human tumor) into a neighboring lot (the mouse). The original plants (human cancer cells) grow alongside the neighbor’s local weeds and grass (mouse cells). When you harvest the lot and sequence everything, you need to sort out which plants came from the original garden and which grew locally.

### What Are the Challenges of Analyzing PDX scRNA-seq Data?

The mixed-species nature of PDX samples creates a set of challenges that don’t arise in single-species experiments:

**1. You cannot simply align to the human genome.**Human and mouse genomes are about 85% similar at the coding sequence level. If you align PDX reads only to the human reference, mouse reads that happen to match human genes will align there too — contaminating your human count matrix with mouse signal.

**2. Standard Cell Ranger output won’t tell you which cell is human and which is mouse.**Cell Ranger (the 10x Genomics pipeline you used in Part 1) doesn’t separate species by default. Unless you use a combined reference genome, all cells are treated as one species.

**3. Doublets in PDX data include real cross-species doublets.**In standard single-species data, doublets are two cells of the same species captured in one droplet. In PDX data, you can also get human-mouse doublets — a human tumor cell and a mouse stromal cell in the same droplet. These are not technically errors in the sequencing chemistry; they represent real biological co-capture events and must be identified and removed separately.

**4. Mouse contamination levels vary by tumor type and passage number.**Early-passage PDX tumors tend to have more mouse stromal infiltration than late-passage tumors. The fraction of mouse cells can range from under 5% to over 50%, depending on the tumor model. Failing to account for this skews every downstream analysis.

### What Is the Workflow for Analyzing PDX scRNA-seq Data?

The core challenge — separating human and mouse cells — can be addressed at two different points in the analysis pipeline:

```
Raw FASTQ files (mixed human + mouse reads)
          ↓
┌─────────────────────────────────────────────┐
│  STRATEGY 1: Align first, separate later    │
│  Cell Ranger → combined reference genome    │
│  ↓                                          │
│  gem_classification.csv (per-cell species)  │
│  ↓                                          │
│  Subset combined matrix by species          │
│  (barcodes from gem_classification.csv +    │
│   gene prefix GRCh38_ / GRCm39_)           │
└─────────────────────────────────────────────┘
          OR
┌─────────────────────────────────────────────┐
│  STRATEGY 2: Separate first, align later    │
│  XenoCell → splits FASTQ by species         │
│  ↓                                          │
│  Human FASTQ + Mouse FASTQ                  │
│  ↓                                          │
│  Cell Ranger (human ref) + (mouse ref)      │
│  ↓                                          │
│  Species-specific count matrices            │
└─────────────────────────────────────────────┘
          ↓
Downstream analysis (QC → clustering → annotation → DEG)
```

Both strategies produce the same end result: separate count matrices for human and mouse cells. The choice between them depends on your dataset, computational resources, and whether you want to keep the mouse data. We’ll cover both in detail in this tutorial.

### How Do We Separate Human and Mouse Cells?

There are two main computational approaches:

**Approach 1: Cell Ranger with a combined human-mouse reference genome**

10x Genomics provides a pre-built reference genome that concatenates the human (GRCh38) and mouse (GRCm39) genomes together. When you run Cell Ranger against this combined reference, every read is assigned to the species whose genome it aligns to best — a process called **competitive alignment**. Cell Ranger then classifies each cell barcode as `GRCh38` (human), `GRCm39` (mouse), or `Multiplet` (captured reads from both species) and writes this information — along with per-species read counts — to a file called `gem_classification.csv`. You then subset the combined count matrix directly in R using these species labels.

*Pros:* Single Cell Ranger run; official 10x support; competitive alignment means every read was tested against both genomes before being assigned, so the per-gene counts in the combined matrix already reflect clean species separation.*Cons:* The combined feature matrix uses prefixed gene names (`GRCh38_TP53`, `GRCm39_Trp53`) that must be stripped before downstream analysis.

**Approach 2: XenoCell**

[XenoCell](https://gitlab.com/XenoCell/XenoCell) is a dedicated tool that operates on the raw FASTQ files, before any alignment. It builds a k-mer index from both genomes and uses it to classify each read to a species, then writes separate FASTQ files for human and mouse reads. You then run Cell Ranger separately on each species-specific FASTQ set using the standard single-species reference.

*Pros:* Produces clean single-species FASTQ files; downstream Cell Ranger runs use standard single-species references with no prefixed gene names.*Cons:* More complex setup requiring Singularity; index build requires enormous RAM (one-time); adds an extra classification step before Cell Ranger.

**Which one should you use?**

In practice, **Strategy 1 is recommended for most analyses** — the combined reference’s competitive alignment produces reliably clean species assignments, and direct matrix subsetting is both correct and computationally efficient. **Strategy 2 is worth the extra effort** when your contamination scatter plot shows significant overlap between species clouds. This tutorial walks you through both.

---

## Setting Up Your Environment for PDX Analysis

### Reusing the Conda Environment from Part 1

The good news: you don’t need a brand-new environment for this tutorial. The conda environment you created in [Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/) already contains Cell Ranger and the shell utilities you’ll need. Activate it now:

```bash
# Activate your existing scRNA-seq conda environment from Part 1
conda activate scrnaseq
```

> 

**If you haven’t completed Part 1 yet**, please follow the environment setup instructions there before continuing. Cell Ranger must be installed and accessible from your `$PATH`.

Verify Cell Ranger is available:

```bash
cellranger --version
# Expected output: cellranger cellranger-10.0.0
```

> 

**Cell Ranger version note:** This tutorial uses **Cell Ranger 10.0.0**. The structure of `gem_classification.csv` changed in recent Cell Ranger versions — notably, the `call` column now uses genome build names (`GRCh38`, `GRCm39`) rather than plain labels like “Human” or “Mouse”. If you are on an older version, adjust your filtering code accordingly.

The combined human-mouse reference genome is maintained and distributed by 10x Genomics. It is **not** something you need to build from scratch.

Navigate to the 10x Genomics downloads page:

> 

**Download URL:** https://www.10xgenomics.com/support/software/cell-ranger/downloads

Look for the section **“Reference packages”** and download the latest version of the combined human-mouse reference. Cell Ranger 10.0.0 uses **GRCh38 + GRCm39**:

```
GRCh38-and-GRCm39-2024-A.tar.gz
```

Once downloaded, extract it:

```bash
# Move to your reference directory
mkdir -p ~/references
cd ~/references

# Extract the combined reference (adjust filename for the version you downloaded)
tar -xzf GRCh38-and-GRCm39-2024-A.tar.gz
```

After extraction, you should see a directory called `GRCh38_and_GRCm39-2024-A/` containing subdirectories like `fasta/` and `star/`. This is your combined reference path for Cell Ranger.

### Installing Singularity

Before installing XenoCell, you need Singularity (also distributed as **Apptainer** since 2021 — both names refer to the same tool). Singularity is a container runtime designed for HPC environments: unlike Docker, it runs without root privileges, making it the standard container solution on shared research computing clusters.

**Option A — Install via conda (recommended for most users):**

The simplest approach on an HPC system where you don’t have root access is to install Apptainer through conda:

```
conda activate scrnaseq
conda install -c conda-forge apptainer
apptainer --version
```

> 

**Note:** The conda-forge package is named `apptainer` (the newer name). Once installed, you can use either `apptainer` or `singularity` as the command — they are aliases for each other in Apptainer ≥ 1.0.

**Option B — Load as an HPC module (common on institutional clusters):**

Many HPC clusters provide Singularity or Apptainer as a pre-installed environment module. Check what’s available:

```
module avail singularity
module avail apptainer
```

If you see a module listed, load it:

```bash
module load singularity   # adjust the module name to what your cluster provides
singularity --version
```

### Installing XenoCell

XenoCell is distributed as a **Docker/Singularity container** hosted on Docker Hub. On most HPC systems, Singularity is the preferred container runtime because it doesn’t require root access. Pull the XenoCell container using Singularity:

```bash
# Pull the XenoCell Docker image and convert it to a Singularity image file (.sif)
# This only needs to be done once; store it somewhere permanent on your HPC storage
singularity build xenocell_1.0.sif docker://romanhaa/xenocell:1.0
```

This creates a portable `xenocell_1.0.sif` file in your current directory. All XenoCell commands are then run by prefixing with `singularity exec xenocell_1.0.sif`.

Verify that XenoCell is accessible:

```bash
singularity exec xenocell_1.0.sif xenocell.py --help
```

> 

**Why a container?** XenoCell depends on Xenome, a specialized k-mer-based read classifier that has complex dependencies. Packaging everything into a container means you don’t need to install Xenome or any of its libraries separately — the container handles it all. Check my [previous tutorial](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/) for more details about containers.

### Setting Up Your Project Directory

Create a clean working directory for your PDX project:

```
mkdir -p ~/PDX_scRNA/{fastq,cellranger_combined,cellranger_human,cellranger_mouse,xenocell_index,xenocell_output}
cd ~/PDX_scRNA
```

Your directory structure will look like this throughout the tutorial:

```
~/PDX_scRNA/
├── fastq/                    # Raw FASTQ files (input)
├── cellranger_combined/      # Cell Ranger output (combined reference)
├── cellranger_human/         # Cell Ranger output (human reference only)
├── cellranger_mouse/         # Cell Ranger output (mouse reference only)
├── xenocell_index/           # XenoCell Xenome index (built once, reused)
└── xenocell_output/          # XenoCell classified FASTQ files per sample
```

---

## Strategy 1: Align First, Separate Later with Cell Ranger

Strategy 1 uses Cell Ranger’s built-in multi-species support. You run Cell Ranger once against the combined human-mouse reference, inspect the species classifications, assess contamination, and then re-run with species-specific references on the appropriate cell barcodes.

### Step 1.1: Run Cell Ranger with the Combined Reference Genome

The Cell Ranger command for PDX data is identical to what you learned in [Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/), with one change: `--transcriptome` now points to the combined reference:

```
cellranger count \
  --id=PDX_sample1_combined \
  --transcriptome=~/references/GRCh38_and_GRCm39-2024-A \
  --fastqs=~/PDX_scRNA/fastq/sample1 \
  --sample=sample1 \
  --localcores=16 \
  --localmem=64 \
  --output-dir=~/PDX_scRNA/cellranger_combined/sample1
```

**Key parameters explained:**

`--id``--transcriptome``--fastqs``--sample``--localcores``--localmem`

| Parameter | What it does |
| --- | --- |
|  | Name for this Cell Ranger run; creates an output folder with this name |
|  | Path to your reference genome (combined here) |
|  | Directory containing your FASTQ files |
|  | The sample name prefix on your FASTQ files |
|  | CPU threads to use; adjust to your system |
|  | RAM in GB; combined reference needs at least 64 GB |

Cell Ranger will run for a couple of hours depending on sequencing depth and available CPUs. When it finishes, the key output files are in:

```
~/PDX_scRNA/cellranger_combined/sample1/outs/
```

### Step 1.2: Understanding gem_classification.csv — Your Species Map

When Cell Ranger aligns to a combined reference, it writes a critical file to the output directory:

```
outs/analysis/gem_classification.csv
```

This file assigns every cell barcode to a species. Let’s look at its structure:

```bash
# Preview the classification file
head -5 ~/PDX_scRNA/cellranger_combined/sample1/outs/analysis/gem_classification.csv
```

Expected output (Cell Ranger 10.0.0):

```
              barcode GRCh38 GRCm39   call
1: AAACCCAAGATAACGT-1    124   4217 GRCm39
2: AAACCCACAGGCTTGC-1  31281     37 GRCh38
3: AAACCCACAGTCCGTG-1    201   3847 GRCm39
4: AAACCCAGTCGAACGA-1  28687     33 GRCh38
5: AAACCCAGTGCCCAGT-1   7465     23 GRCh38
```

**Column descriptions:**

`barcode``GRCh38``GRCm39``call``GRCh38``GRCm39``Multiplet`

| Column | Meaning |
| --- | --- |
|  | The cell barcode (matches barcodes in the count matrix) |
|  | Number of reads from this barcode that aligned to the human genome |
|  | Number of reads from this barcode that aligned to the mouse genome |
|  | Cell Ranger’s species classification:  (human),  (mouse), or |

> 

**Note on column naming:** Unlike earlier Cell Ranger versions that used generic labels like “Human” or “Mouse” in the `call` column, **Cell Ranger 10.0.0 uses the genome build names directly** — `GRCh38` for human cells and `GRCm39` for mouse cells. Make sure your filtering code uses these exact strings.

**What does “Multiplet” mean here?** A multiplet is a droplet whose reads come from both species in roughly equal proportions — either a real human-mouse doublet or cross-species ambient RNA contamination. These barcodes should be **excluded from all downstream analysis**.

### Step 1.3: Calculating and Plotting Mouse Contamination

This is one of the most important quality control steps for PDX data. Before running any downstream analysis, you need to know exactly how much mouse signal is present in your sample. A sample with 10% mouse contamination and one with 60% mouse contamination require very different handling. This section walks through four complementary visualizations that together give you a complete picture of your sample’s species composition.

Start by loading the file and computing a per-cell contamination ratio:

```r
library(data.table)
library(ggplot2)
library(patchwork)

# Load the gem classification file
gem_class <- fread(
  "~/PDX_scRNA/cellranger_combined/sample1/outs/analysis/gem_classification.csv"
)

# Calculate total reads and mouse contamination ratio per cell
# mouse_ratio = fraction of a cell's reads that mapped to mouse genome
gem_class[, total_reads  := GRCh38 + GRCm39]
gem_class[, mouse_ratio  := GRCm39 / total_reads]
gem_class[, human_ratio  := GRCh38 / total_reads]

# Define color palette used consistently across all plots
species_colors <- c("GRCh38" = "#E63946", "GRCm39" = "#457B9D", "Multiplet" = "#A8DADC")
```

#### Plot 1: Species Composition Bar Chart

The simplest summary — what fraction of cells were called as human, mouse, or multiplet:

```r
# Summarize cell counts and percentages per species call
species_summary <- gem_class[, .N, by = call][, pct := round(N / sum(N) * 100, 1)]

p1 <- ggplot(species_summary, aes(x = call, y = pct, fill = call)) +
  geom_col(width = 0.6) +
  geom_text(aes(label = paste0(pct, "%\n(n=", N, ")")),
            vjust = -0.4, size = 3.5) +
  scale_fill_manual(values = species_colors) +
  scale_y_continuous(expand = expansion(mult = c(0, 0.15))) +
  labs(
    title = "Species Composition",
    x = NULL,
    y = "Percentage of Cells (%)"
  ) +
  theme_classic() +
  theme(legend.position = "none")
```

**What to look for:** The GRCm39 bar is your mouse contamination level. Values under 30% are generally acceptable for PDX tumor analysis. Values above 50% suggest poor engraftment or heavy stromal infiltration — interpret results with caution and consider whether the sample is usable.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/PDX_scRNAseq_composition_bar-e1771825407717-1024x806.png?resize=1024%2C806&ssl=1)

#### Plot 2: Human vs. Mouse Reads Scatter Plot

This plot reveals whether Cell Ranger’s species separation is clean or ambiguous. Each point is one cell barcode, positioned by its human read count (x-axis) and mouse read count (y-axis):

```r
# Add a small pseudocount before log-transform to handle zeros
gem_class[, GRCh38_plot := GRCh38 + 1]
gem_class[, GRCm39_plot := GRCm39 + 1]

p2 <- ggplot(gem_class, aes(x = GRCh38_plot, y = GRCm39_plot, color = call)) +
  geom_point(alpha = 0.3, size = 0.4) +
  scale_color_manual(values = species_colors) +
  scale_x_log10(labels = scales::comma) +
  scale_y_log10(labels = scales::comma) +
  labs(
    title = "Human vs. Mouse Reads per Cell",
    x = "GRCh38 Reads (log10)",
    y = "GRCm39 Reads (log10)",
    color = "Species"
  ) +
  theme_classic() +
  guides(color = guide_legend(override.aes = list(size = 2, alpha = 1)))
```

**What to look for:** A clean sample produces two tightly separated clusters — one in the upper-left (high GRCm39, low GRCh38: mouse cells) and one in the lower-right (high GRCh38, low GRCm39: human cells). Multiplets fall in the middle diagonal. If the two clusters bleed into each other with no clear gap, the combined reference alignment is struggling to distinguish the species — consider switching to Strategy 2 (XenoCell).

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/PDX_scRNAseq_species_plot-e1771825371413-1024x831.png?resize=1024%2C831&ssl=1)

#### Plot 3: Mouse Contamination Ratio Distribution

A histogram of the per-cell mouse ratio (GRCm39 reads / total reads) reveals the bimodal structure of a PDX sample in a different way — and makes it easy to spot cells that fall in an ambiguous grey zone:

```r
p3 <- ggplot(gem_class, aes(x = mouse_ratio, fill = call)) +
  geom_histogram(bins = 100, color = NA) +
  scale_fill_manual(values = species_colors) +
  scale_x_continuous(labels = scales::percent, breaks = seq(0, 1, 0.2)) +
  labs(
    title = "Mouse Read Fraction per Cell",
    x = "Mouse Reads / Total Reads",
    y = "Number of Cells",
    fill = "Species"
  ) +
  theme_classic()
```

**What to look for:** You should see two sharp peaks — one near 0 (human cells with almost no mouse reads) and one near 1 (mouse cells with almost no human reads). A broad valley between the peaks is normal. A large, flat distribution between 0.2 and 0.8 suggests many ambiguous cells; this is a warning sign that your separation may not be reliable.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/PDX_scRNAseq_mouse_contamination_hist-e1771825356725-1024x691.png?resize=1024%2C691&ssl=1)

#### Plot 4: Per-Species UMI Depth

Finally, check whether human and mouse cells have comparable sequencing depths. Large differences in UMI counts between species can indicate that one population is being captured less efficiently, or that the cell-calling thresholds need adjustment:

```r
p4 <- ggplot(gem_class[call != "Multiplet"], aes(x = call, y = total_reads, fill = call)) +
  geom_violin(scale = "width", trim = TRUE) +
  geom_boxplot(width = 0.08, fill = "white", outlier.shape = NA) +
  scale_fill_manual(values = species_colors) +
  scale_y_log10(labels = scales::comma) +
  labs(
    title = "UMI Depth by Species",
    x = NULL,
    y = "Total Reads per Cell (log10)"
  ) +
  theme_classic() +
  theme(legend.position = "none")
```

**What to look for:** Human and mouse cells should have broadly similar UMI distributions, typically peaking in the thousands. If mouse cells have drastically fewer UMIs than human cells (e.g., median of 200 vs. 3,000), it may indicate that the mouse cells are low-quality stromal fragments rather than intact cells.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/PDX_scRNAseq_umi_depth_violin-e1771825385693-1024x806.png?resize=1024%2C806&ssl=1)

#### Combining All Four Plots and Printing a Summary

```r
# Combine into a 2×2 panel and save
combined_plot <- (p1 | p2) / (p3 | p4)
ggsave("~/PDX_scRNA/contamination_qc.pdf", combined_plot, width = 12, height = 9)

# Print a concise contamination report to the console
total_cells  <- nrow(gem_class)
human_cells  <- gem_class[call == "GRCh38", .N]
mouse_cells  <- gem_class[call == "GRCm39",  .N]
multiplets   <- gem_class[call == "Multiplet", .N]

cat("=== PDX Contamination Summary ===\n")
cat("Total cells called:   ", total_cells, "\n")
cat("Human (GRCh38):       ", human_cells,
    sprintf("(%.1f%%)\n", human_cells  / total_cells * 100))
cat("Mouse (GRCm39):       ", mouse_cells,
    sprintf("(%.1f%%)\n", mouse_cells  / total_cells * 100))
cat("Multiplets:           ", multiplets,
    sprintf("(%.1f%%)\n", multiplets   / total_cells * 100))
cat("Mouse contamination:  ",
    sprintf("%.1f%% of non-multiplet cells\n",
            mouse_cells / (human_cells + mouse_cells) * 100))
```

Example output:

```
=== PDX Contamination Summary ===
Total cells called:    5423
Human (GRCh38):        3812  (70.3%)
Mouse (GRCm39):        1489  (27.4%)
Multiplets:             122  (2.2%)
Mouse contamination:   28.1% of non-multiplet cells
```

> 

**Decision guide based on mouse contamination:**

| Mouse contamination | Interpretation | Recommended action |
| --- | --- | --- |
| < 10% | Excellent engraftment | Proceed with either strategy |
| 10–30% | Typical PDX range | Either strategy works; Strategy 1 sufficient |
| 30–50% | High stromal infiltration | Prefer Strategy 2 (XenoCell) for cleaner separation |
| > 50% | Poor engraftment or early passage | Re-evaluate sample; consider excluding |

### Step 1.4: Subsetting the Combined Matrix by Species

This is the final step of Strategy 1. Rather than re-running Cell Ranger, you subset the combined-reference count matrix directly in R using the species labels from `gem_classification.csv`. This is the correct approach because the combined alignment already used **competitive mapping** — every read was tested against both genomes simultaneously and assigned to whichever it matched best. Those clean per-gene counts are already in the matrix; you just need to extract the right rows (genes) and columns (barcodes).

```r
library(Seurat)
library(data.table)

# Load the combined-reference count matrix
combined_counts <- Read10X(
  "~/PDX_scRNA/cellranger_combined/sample1/outs/filtered_feature_bc_matrix/"
)

# Extract species-specific barcodes from gem_classification.csv
# (gem_class was loaded in Step 1.3)
human_barcodes <- gem_class[call == "GRCh38", barcode]
mouse_barcodes <- gem_class[call == "GRCm39",  barcode]

#-----------------------------------------------
# Build human Seurat object
#-----------------------------------------------
# Keep only human genes (rows prefixed "GRCh38_") and human barcodes
human_counts <- combined_counts[
  grepl("^GRCh38_", rownames(combined_counts)),
  colnames(combined_counts) %in% human_barcodes
]

# Strip the genome prefix so gene names are compatible with
# marker databases, PercentageFeatureSet(), FindMarkers(), etc.
rownames(human_counts) <- sub("^GRCh38_", "", rownames(human_counts))

human_seurat <- CreateSeuratObject(counts = human_counts, project = "PDX_sample1_human")
human_seurat$species   <- "human"
human_seurat$sample_id <- "PDX_sample1"

#-----------------------------------------------
# Build mouse Seurat object
#-----------------------------------------------
mouse_counts <- combined_counts[
  grepl("^GRCm39_", rownames(combined_counts)),
  colnames(combined_counts) %in% mouse_barcodes
]

rownames(mouse_counts) <- sub("^GRCm39_", "", rownames(mouse_counts))

mouse_seurat <- CreateSeuratObject(counts = mouse_counts, project = "PDX_sample1_mouse")
mouse_seurat$species   <- "mouse"
mouse_seurat$sample_id <- "PDX_sample1"

cat("Human Seurat object:", ncol(human_seurat), "cells ×", nrow(human_seurat), "genes\n")
cat("Mouse Seurat object:", ncol(mouse_seurat), "cells ×", nrow(mouse_seurat), "genes\n")
```

> 

**Don’t forget to strip the gene prefix.** Gene names in the combined matrix are prefixed (`GRCh38_TP53`, `GRCm39_Trp53`). Forgetting to strip this prefix is one of the most common errors in PDX analysis — it will silently break `PercentageFeatureSet(pattern = "^MT-")`, all marker gene lookups, and any tool that expects standard HGNC or MGI gene symbols.

You now have two Seurat objects ready for the standard downstream pipeline — proceed to [Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/) for QC and filtering.

---

## Strategy 2: Separate First, Align Later with XenoCell

Strategy 2 uses XenoCell to classify and separate reads at the FASTQ level, before any Cell Ranger alignment. This approach often produces cleaner separation, especially in samples where significant cross-species ambient RNA is present.

### Step 2.1: How XenoCell Works — A Conceptual Overview

XenoCell uses Xenome, a k-mer-based read classifier, under the hood. Unlike STAR-based alignment, Xenome doesn’t try to align reads end-to-end — instead it builds a k-mer index from both genomes and uses it to probabilistically assign each read to a species based on k-mer matches. This is very fast and requires no splice junction annotation.

The XenoCell workflow has **three sequential steps**:

```
Step 1: generate_index
  Build a Xenome k-mer index from the host (mouse) and graft (human) FASTA files.
  This is done once and reused for all samples.
          ↓
Step 2: classify_reads
  Classify every read in your FASTQ files as host, graft, both, or neither.
  Outputs per-barcode species statistics.
          ↓
Step 3: extract_cellular_barcodes
  Use thresholds on the host-read fraction to write species-specific FASTQ files.
  Run once for human cells (low host fraction) and once for mouse cells (high host fraction).
```

### Step 2.2: Building the Xenome Index

The index only needs to be built once per species pair and can be reused across all your PDX samples. XenoCell needs the **genome FASTA files** directly — not STAR indices. You can use the FASTA files that come with the Cell Ranger reference packages:

```bash
singularity exec xenocell_1.0.sif xenocell.py generate_index \
  --graft ~/references/refdata-gex-GRCh38-2024-A/fasta/genome.fa \
  --host  ~/references/refdata-gex-GRCm39-2024-A/fasta/genome.fa \
  --output ~/PDX_scRNA/xenocell_index \
  --threads 16 \
  --memory 800
```

**Key parameters:**

`--graft``--host``--output``--threads``--memory`

| Parameter | Description |
| --- | --- |
|  | FASTA file of the graft (human) reference genome |
|  | FASTA file of the host (mouse) reference genome |
|  | Directory where the Xenome index files will be written |
|  | Number of CPU threads |
|  | RAM in GB allocated for index building |

> 

⚠️ **Critical memory warning:** Building a Xenome index for the combined human + mouse genome is **extremely RAM-intensive**. Despite what the `--memory` flag suggests, Xenome’s actual peak memory usage is determined by the genome sizes, not by the value you pass. For the GRCh38 + GRCm39 combination, reported memory requirements from users range from **500 GB to over 1 TB RAM**. Before attempting this step, check with your HPC administrator whether a high-memory node with 1 TB+ RAM is available on your cluster. If not, **Strategy 1 (Cell Ranger combined reference) is the practical choice for your project** — the `generate_index` memory requirement is the main reason Strategy 2 is out of reach for many research groups despite its conceptual advantages.

### Step 2.3: Classifying Reads

For each sample, run `classify_reads` to assign every read to a species. In 10x Genomics 3′ data, **R1 contains the cell barcode + UMI** and **R2 contains the transcript**. XenoCell needs these specified separately:

```bash
singularity exec xenocell_1.0.sif xenocell.py classify_reads \
  --barcode    ~/PDX_scRNA/fastq/sample1/sample1_S1_L001_R1_001.fastq.gz \
  --transcript ~/PDX_scRNA/fastq/sample1/sample1_S1_L001_R2_001.fastq.gz \
  --barcode_start  1 \
  --barcode_length 16 \
  --index  ~/PDX_scRNA/xenocell_index \
  --output ~/PDX_scRNA/xenocell_output/sample1 \
  --threads 16 \
  --memory 128
```

**Key parameters:**

`--barcode``--transcript``--barcode_start`**always 1**`--barcode_length`**16**`--index``--output`

| Parameter | Description |
| --- | --- |
|  | R1 FASTQ file (contains cell barcode + UMI) |
|  | R2 FASTQ file (contains cDNA transcript read) |
|  | Position in R1 where the barcode starts;  for 10x Genomics |
|  | Length of the cell barcode;  for 10x 3′ v2/v3 |
|  | Path to the Xenome index built in Step 2.2 |
|  | Directory for per-barcode classification results |

> 

**If you have multiple lanes:** Run `classify_reads` once per lane FASTQ pair, each with its own `--output` directory, then merge the outputs before Step 2.4. Alternatively, if your FASTQs have already been concatenated across lanes by Cell Ranger (the default when you ran Part 1), you will only have one R1/R2 pair per sample.

### Step 2.4: Extracting Species-Specific FASTQ Files

The `extract_cellular_barcodes` step uses the per-barcode host-read fraction (mouse reads / total reads) to split barcodes into species groups. You run it **twice** — once for human cells and once for mouse cells — using different thresholds.

**Extract human (graft) cells:**

Human cells should have a very low fraction of mouse (host) reads. Set tight thresholds to keep only barcodes where fewer than 10% of reads are mouse:

```bash
singularity exec xenocell_1.0.sif xenocell.py extract_cellular_barcodes \
  --input          ~/PDX_scRNA/xenocell_output/sample1 \
  --barcode_start  1 \
  --barcode_length 16 \
  --subset_name    human \
  --lower_threshold 0.0 \
  --upper_threshold 0.1 \
  --threads 16
```

**Extract mouse (host) cells:**

Mouse cells should have a high fraction of mouse reads — above 90%:

```bash
singularity exec xenocell_1.0.sif xenocell.py extract_cellular_barcodes \
  --input          ~/PDX_scRNA/xenocell_output/sample1 \
  --barcode_start  1 \
  --barcode_length 16 \
  --subset_name    mouse \
  --lower_threshold 0.9 \
  --upper_threshold 1.0 \
  --threads 16
```

**Understanding the threshold parameters:**

`--lower_threshold``--upper_threshold``--subset_name`

| Parameter | Meaning |
| --- | --- |
|  | Minimum host (mouse) read fraction to include |
|  | Maximum host (mouse) read fraction to include |
|  | Label applied to output FASTQ files for this group |

Barcodes with a host fraction between 0.1 and 0.9 are treated as multiplets and are excluded from both outputs — equivalent to the “Multiplet” category in Strategy 1.

> 

**Adjusting thresholds:** The 0.1 / 0.9 defaults work well for samples with clean separation. If your scatter plot (Plot 2 from Step 1.3) shows that cell clouds don’t extend close to 0% or 100%, you may need to tighten these thresholds. Check the histogram from Step 1.3 to pick cutoffs that fall cleanly in the empty valley between the two peaks.

### Step 2.5: Running Cell Ranger on Species-Specific FASTQ Files

After `extract_cellular_barcodes`, the output directory contains species-labeled FASTQ files. Run Cell Ranger separately on each — these are standard single-species runs identical to [Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/):

**Cell Ranger for human cells (graft):**

```
cellranger count \
  --id=PDX_sample1_human \
  --transcriptome=~/references/refdata-gex-GRCh38-2024-A \
  --fastqs=~/PDX_scRNA/xenocell_output/sample1 \
  --sample=human \
  --localcores=16 \
  --localmem=64 \
  --output-dir=~/PDX_scRNA/cellranger_human/sample1
```

**Cell Ranger for mouse cells (host):**

```
cellranger count \
  --id=PDX_sample1_mouse \
  --transcriptome=~/references/refdata-gex-GRCm39-2024-A \
  --fastqs=~/PDX_scRNA/xenocell_output/sample1 \
  --sample=mouse \
  --localcores=16 \
  --localmem=64 \
  --output-dir=~/PDX_scRNA/cellranger_mouse/sample1
```

Note the `--sample` flag matches the `--subset_name` you used in `extract_cellular_barcodes` (`human` and `mouse` respectively). This tells Cell Ranger which FASTQ files in the output directory belong to each run. Because XenoCell has already separated the reads at the FASTQ level, each Cell Ranger run processes only one species and produces standard single-species output with no gene name prefixes — ready for downstream analysis without any additional subsetting step.

---

## Comparing the Two Strategies: Which Should You Choose?

Now that you’ve seen both approaches in detail, here is a practical comparison:

**Number of Cell Ranger runs****Memory for species separation****500 GB–1 TB+ RAM****Classification method****Gene name handling****Separation accuracy****Time to results****Contamination assessment****Installation****Official 10x support****HPC accessibility****Best for**

| Factor | Strategy 1 (Cell Ranger Combined) | Strategy 2 (XenoCell) |
| --- | --- | --- |
|  | 1 (combined reference only) | 2 (human + mouse) |
|  | ~64 GB (combined Cell Ranger run) | (Xenome index build) |
|  | Competitive alignment (STAR) | K-mer-based (Xenome) |
|  | Prefix stripping required in R | Standard gene names from single-species refs |
|  | Excellent for most samples | Better when species clusters overlap |
|  | 1 Cell Ranger run + R subsetting | Index build + 2 Cell Ranger runs |
|  | ✅ Built-in (gem_classification.csv) | Via threshold tuning in Step 2.4 |
|  | ✅ Included with Cell Ranger | Singularity + XenoCell container |
|  | ✅ Yes | ❌ Community tool |
|  | ✅ Standard nodes (~64 GB) | ❌ Requires rare high-memory node (1 TB+) |
|  | Most PDX analyses | When 1 TB+ node is available and separation is ambiguous |

**Our recommendation:** **Strategy 1 is the practical choice for the vast majority of PDX projects.** The Xenome index building step in Strategy 2 requires 500 GB–1 TB or more of RAM — a resource that is unavailable on most standard HPC nodes. Strategy 1’s competitive alignment approach is also conceptually sound: because both genomes are present simultaneously during alignment, reads are assigned to whichever species they match best, producing clean species separation in a single run. Direct matrix subsetting in R then extracts the species-specific counts without any additional Cell Ranger runs.

Strategy 2 remains an option if your institution has a high-memory node available and your contamination scatter plot shows genuinely ambiguous species separation where Strategy 1’s classifications are unreliable. In practice, this situation is uncommon for well-engrafted PDX tumors.

---

## Downstream Analysis: What to Do with Your Species-Specific Count Matrices

Once you have clean, species-specific count matrices — whether from Strategy 1 or Strategy 2 — the downstream analysis follows the same pipeline as any standard single-species scRNA-seq experiment.

### For Human Tumor Cell Analysis

The human count matrix from your PDX data is structurally identical to any human scRNA-seq dataset. Apply the complete workflow from Parts 1–7 of this series:

**[Part 2: Quality Control and Cell Filtering](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/)**

1. 

- Load the human count matrix with `Read10X()` from the `cellranger_human/` output directory.

- Apply standard QC metrics: nFeature_RNA, nCount_RNA, and percent.mt thresholds.

- Remove low-quality cells, empty droplets, and doublets.

**[Part 3: Integration and Clustering](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/)**

1. 

- If you have multiple PDX samples (e.g., from different patients or treatment time points), integrate them to correct batch effects.

- Perform dimensionality reduction and clustering.

**[Part 4: Cell Type Annotation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/)**

1. 

- Annotate tumor cell clusters using cancer-type-specific markers.

- Identify tumor subclones, cancer stem cells, or epithelial-to-mesenchymal transition states.

**[Part 5: Differential Expression Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/)**

1. 

- Compare gene expression between tumor cell subpopulations or treatment conditions.

**[Part 7: Trajectory Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/)**

1. 

- Model tumor cell state transitions or differentiation hierarchies using Monocle 3.

### For Mouse Host Cell Analysis

If you are interested in the mouse stromal and immune compartment (e.g., studying how the tumor microenvironment responds to treatment), load the mouse count matrix exactly the same way, but use mouse-specific marker genes for cell type annotation in [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/).

---

## Best Practices for PDX scRNA-seq Analysis

**1. Always run Strategy 1 first for a contamination check.**Even if you plan to use XenoCell for final analysis, the gem_classification.csv from a combined Cell Ranger run gives you a quick, reliable estimate of your mouse contamination percentage. This helps you decide whether downstream analyses are feasible and whether the PDX model is performing as expected.

**2. Set a consistent contamination threshold across samples.**If you are comparing multiple PDX samples (e.g., untreated vs. treated), establish a minimum human cell purity cutoff (e.g., >80% human cells) and apply it uniformly. Samples with very high mouse contamination may need to be flagged or excluded.

**3. Treat cross-species doublets as rigorously as same-species doublets.**Multiplets in gem_classification.csv are true cross-species contamination events and should always be excluded. After re-running Cell Ranger with species-specific references, also run standard doublet detection (scDblFinder as in [Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/)) to catch same-species doublets that Cell Ranger’s species classifier missed.

**4. Keep your human and mouse count matrices well-labeled.**When building Seurat objects, add clear metadata fields to distinguish PDX origin:

```r
library(Seurat)

# Load human count matrix
human_counts <- Read10X("~/PDX_scRNA/cellranger_human/sample1/outs/filtered_feature_bc_matrix/")
human_seurat <- CreateSeuratObject(counts = human_counts, project = "PDX_sample1_human")

# Add PDX-specific metadata
human_seurat$species     <- "human"
human_seurat$sample_id   <- "PDX_sample1"
human_seurat$pdx_model   <- "breast_cancer_PDX_01"  # adjust to your model
human_seurat$passage     <- 3                         # passage number if applicable
```

**5. Use the same Cell Ranger version and reference genome version consistently.**Mixing Cell Ranger versions or reference genome builds across samples will introduce batch effects that are difficult to distinguish from biological variation. Document all software versions for reproducibility.

## Conclusion

PDX models are powerful tools for cancer research, but they require careful computational handling to separate human tumor cells from mouse host cells before any meaningful analysis can begin.

Once your species-specific count matrices are ready, the rest of the analysis follows the same path you’ve already mastered — QC, integration, clustering, annotation, and beyond.

---

## References

1. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nat Biotechnol.* 2024;42(2):293–304.

2. Zheng GXY, Terry JM, Belgrader P, et al. Massively parallel digital transcriptional profiling of single cells. *Nat Commun.* 2017;8:14049.

3. Ben-David U, Ha G, Tseng YY, et al. Patient-derived xenografts undergo mouse-specific tumor evolution. *Nat Genet.* 2017;49(11):1567–1575.

4. Gómez-Flores-Reyes JC, Camacho-Ríos V, Fernández-Ramírez F, et al. XenoCell: deconvolution of host and graft species in single cell RNA-seq data derived from patient-derived xenograft models. *bioRxiv.* 2021. doi:10.1101/2021.03.15.435474

5. Luecken MD, Theis FJ. Current best practices in single-cell RNA-seq analysis: a tutorial. *Mol Syst Biol.* 2019;15(6):e8746.

6. Young MD, Behjati S. SoupX removes ambient RNA contamination from droplet-based single-cell RNA sequencing data. *Gigascience.* 2020;9(12):giaa151.

7. Germain PL, Lun A, Garcia Meixide C, Macnair W, Robinson MD. Doublet identification in single-cell sequencing data using scDblFinder. *F1000Res.* 2021;10:979.

8. Tirosh I, Izar B, Prakadan SM, et al. Dissecting the multicellular ecosystem of metastatic melanoma by single-cell RNA-seq. *Science.* 2016;352(6282):189–196.

9. Wai Nam Liu, et al. Single-cell RNA sequencing reveals anti-tumor potency of CD56+ NK cells and CD8+ T cells in humanized mice via PD-1 and TIGIT co-targeting. Molecular Therapy. https://doi.org/10.1016/j.ymthe.2024.09.025

---

*This tutorial is part of the NGS101.com series on single cell sequencing analysis. If this tutorial helped advance your research, please comment and share your experience to help other researchers! Subscribe to stay updated with our latest bioinformatics tutorials and resources.*
