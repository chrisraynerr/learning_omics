# How to Analyze Circular RNA-seq Data for Absolute Beginners Part 13-2: Advanced CircRNA Detection and Differential Expression with CIRI3

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/)

- [Introduction: Advancing Beyond CIRCexplorer2 with CIRI3](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Introduction_Advancing_Beyond_CIRCexplorer2_with_CIRI3)

  - [Quick Recap: CircRNA Biology and Analysis Challenges](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Quick_Recap_CircRNA_Biology_and_Analysis_Challenges)
  - [Introducing CIRI3: Next-Generation CircRNA Analysis](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Introducing_CIRI3_Next-Generation_CircRNA_Analysis)
  - [CIRI3’s Features](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#CIRI3s_Features)
  - [Dataset: GSE97239 Exosomal CircRNAs from Gastric Cancer](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Dataset_GSE97239_Exosomal_CircRNAs_from_Gastric_Cancer)

- [Understanding BSJ and FSJ: The Foundation of CircRNA Quantification](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Understanding_BSJ_and_FSJ_The_Foundation_of_CircRNA_Quantification)

  - [BSJ (Back-Splice Junction): The CircRNA Signature](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#BSJ_Back-Splice_Junction_The_CircRNA_Signature)
  - [FSJ (Forward-Splice Junction): Linear RNA Context](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#FSJ_Forward-Splice_Junction_Linear_RNA_Context)

- [Environment Setup: Installing CIRI3 and All Dependencies](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Environment_Setup_Installing_CIRI3_and_All_Dependencies)

  - [Step 1: Install Conda Package Manager](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Step_1_Install_Conda_Package_Manager)
  - [Step 2: Clone CIRI3 Repository from GitHub](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Step_2_Clone_CIRI3_Repository_from_GitHub)
  - [Step 3: Create CIRI3 Conda Environment](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Step_3_Create_CIRI3_Conda_Environment)
  - [Step 4: Activate the CIRI3 Environment](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Step_4_Activate_the_CIRI3_Environment)
  - [Step 5: Install Additional Required Tools](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Step_5_Install_Additional_Required_Tools)
  - [Step 6: Verify Complete Installation](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Step_6_Verify_Complete_Installation)

- [Data Preparation: Downloading and Organizing GSE97239](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Data_Preparation_Downloading_and_Organizing_GSE97239)

  - [Create Project Directory Structure](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Create_Project_Directory_Structure)
  - [Download Reference Genome and Annotation](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Download_Reference_Genome_and_Annotation)
  - [Index Reference Genome for BWA](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Index_Reference_Genome_for_BWA)
  - [Download RNA-seq Data from SRA](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Download_RNA-seq_Data_from_SRA)

- [Define Paths for Analysis](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Define_Paths_for_Analysis)
- [Read Alignment with BWA-MEM](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Read_Alignment_with_BWA-MEM)

  - [Understanding BWA-MEM Parameters for CircRNA Detection](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Understanding_BWA-MEM_Parameters_for_CircRNA_Detection)

- [CircRNA Detection with CIRI3](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#CircRNA_Detection_with_CIRI3)

  - [Single-Sample Mode (Required for DE_BSJ)](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Single-Sample_Mode_Required_for_DE_BSJ)
  - [Multi-Sample Mode (Required for DE_Ratio and DE_Relative)](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Multi-Sample_Mode_Required_for_DE_Ratio_and_DE_Relative)

- [Gene Expression Quantification for DE_BSJ Normalization](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Gene_Expression_Quantification_for_DE_BSJ_Normalization)

  - [Convert SAM to Sorted BAM](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Convert_SAM_to_Sorted_BAM)
  - [Run featureCounts for Gene Expression](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Run_featureCounts_for_Gene_Expression)
  - [Format Gene Expression Matrix for CIRI3](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Format_Gene_Expression_Matrix_for_CIRI3)

- [Differential Expression Analysis](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Differential_Expression_Analysis)

  - [DE_BSJ: Absolute CircRNA Abundance Changes](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#DE_BSJ_Absolute_CircRNA_Abundance_Changes)
  - [DE_Ratio: Junction Ratio Changes](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#DE_Ratio_Junction_Ratio_Changes)
  - [DE_Relative: Isoform Switching Events](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#DE_Relative_Isoform_Switching_Events)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Troubleshooting_Common_Issues)

  - [Issue 1: Very Few CircRNAs Detected](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Issue_1_Very_Few_CircRNAs_Detected)
  - [Issue 2: Sample Name Mismatches in DE Analysis](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Issue_2_Sample_Name_Mismatches_in_DE_Analysis)
  - [Issue 3: DE_Ratio or DE_Relative Failures](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Issue_3_DE_Ratio_or_DE_Relative_Failures)

- [Conclusion](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Conclusion)

  - [Key Takeaways](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Key_Takeaways)
  - [Next Steps](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Next_Steps)
  - [Community Resources](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Community_Resources)

- [References](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#References)

  - [Key Publications](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Key_Publications)
  - [Share this:](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-circular-rna-seq-data-for-absolute-beginners-part-13-2-advanced-circrna-detection-and-differential-expression-with-ciri3/#Like_this)

## Introduction: Advancing Beyond CIRCexplorer2 with CIRI3

In [Part 13 of my RNA-seq tutorial series](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/), we explored circular RNA (circRNA) analysis using CIRCexplorer2, learning how these fascinating non-linear RNA molecules form through back-splicing and play important roles in gene regulation, disease mechanisms, and potential therapeutic applications.

While CIRCexplorer2 provides an excellent introduction to circRNA analysis, the field has advanced significantly with the development of **CIRI3**, which addresses several limitations of earlier tools and offers substantial improvements in accuracy, sensitivity, and biological interpretation.

In this tutorial, we’ll use **CIRI3**, a comprehensive Java-based circRNA detection tool developed by the Gao Lab at Tsinghua University. CIRI3 integrates circRNA detection, quantification, and differential expression analysis into a single unified pipeline.

### Quick Recap: CircRNA Biology and Analysis Challenges

Before diving into CIRI3, let’s briefly review key concepts from [my CIRCexplorer2 tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/):

**What are circular RNAs?**

- Non-linear RNA molecules formed through back-splicing

- The 3′ downstream splice site joins with the 5′ upstream splice site (creating a circular structure)

- More stable than linear RNAs (resistant to exonuclease degradation)

- Functions include: miRNA sponges, protein scaffolds, translation templates, and gene regulation

**Why are circRNAs challenging to detect?**

- Chimeric junctions (back-splice sites) can resemble RNA editing or trans-splicing events

- Low abundance compared to linear transcripts (often <1% of gene expression)

- Similar sequences between circular and linear isoforms make discrimination difficult

- Potential for false positives from PCR artifacts or tandem duplications

**Why study circRNAs?**

- Biomarkers for disease diagnosis (especially cancer detection)

- Novel therapeutic targets for precision medicine

- Understanding complex gene regulatory mechanisms

- Exploring non-coding RNA biology

### Introducing CIRI3: Next-Generation CircRNA Analysis

CIRI3 is a comprehensive Java-based package that integrates circRNA detection, quantification, and differential expression analysis in a single unified tool.

### CIRI3’s Features

**1. Unified Analysis Pipeline**

- Single Java application handles detection, quantification, and differential expression

- Simplified workflow with consistent input/output formats

- Reduced opportunities for file format incompatibilities

**2. Comprehensive Quantification**

- **BSJ (Back-Splice Junction) Matrix**: Circular junction read counts measuring absolute circRNA abundance

- **FSJ (Forward-Splice Junction) Matrix**: Linear junction read counts providing expression context

- **Junction Ratio**: Proportion of circular vs linear transcripts (circularization efficiency)

- **Relative Expression**: Isoform switching analysis to detect regulatory changes

**3. Multiple Differential Expression Models**

- **DE_BSJ**: Tests absolute circRNA abundance changes between conditions

- **DE_Ratio**: Tests junction ratio changes (circularization efficiency shifts)

- **DE_Relative**: Tests isoform switching events (circular vs linear balance)

**Key advantages**:

- Everything in one package – streamlined workflow

- Produces multiple quantification metrics for comprehensive analysis

- Built-in statistical testing with biological replicates

**Key limitations** (discovered through extensive testing):

- Some differential expression modules (DE_Ratio and DE_Relative) may have Linux system incompatibilities

- Users need to run both single-sample and multi-sample modes as different DE functions require different input formats

### Dataset: GSE97239 Exosomal CircRNAs from Gastric Cancer

We’ll use **GSE97239**: *“Circular RNA profiling in plasma exosomes from patients with gastric cancer”*

**Study details:**

- **Sample type**: Plasma exosomes (naturally enriched for circRNAs)

- **Comparison**: Gastric cancer patients vs healthy controls

- **Technology**: Illumina HiSeq (paired-end 100bp reads)

- **Samples**: 3 cancer + 3 control biological replicates

**Sample information:**

| Sample | Condition | SRA Accession |
| --- | --- | --- |
| Sample 1 | Cancer | SRR5398213 |
| Sample 2 | Cancer | SRR5398214 |
| Sample 3 | Cancer | SRR5398215 |
| Sample 4 | Control | SRR5398216 |
| Sample 5 | Control | SRR5398217 |
| Sample 6 | Control | SRR5398218 |

---

## Understanding BSJ and FSJ: The Foundation of CircRNA Quantification

Before diving into CIRI3 installation and analysis, it’s crucial to understand **how circRNAs are quantified** at the molecular level. This knowledge will help you interpret CIRI3 outputs correctly.

### BSJ (Back-Splice Junction): The CircRNA Signature

**BSJ = Back-Splice Junction** – The defining molecular feature of circular RNAs.

In normal linear RNA splicing, exons join in the forward direction (5′ → 3′):

```
Linear RNA splicing:
Exon1 → Exon2 → Exon3
    FSJ     FSJ

(FSJ = Forward-Splice Junction)
```

In circular RNA formation, the **3′ end joins back to the 5′ start**, creating a closed loop:

```
Circular RNA back-splicing:
            ┌────┐
             ↓                       |
Exon2   ←   Exon3
      |←─BSJ─|

Back-splice: 3' end of Exon3 connects to 5' start of Exon2
(This creates a circular molecule with no free ends)
```

**Why BSJ is critically important:**

- **Only circular RNAs have back-splice junctions** – they’re unique molecular signatures

- BSJ reads provide direct proof of circularity (not inference)

- **BSJ read count = primary measure of circRNA abundance**

- BSJ detection distinguishes true circRNAs from linear transcripts

### FSJ (Forward-Splice Junction): Linear RNA Context

**FSJ = Forward-Splice Junction** – Normal linear RNA splicing pattern.

FSJ represents the **linear transcripts produced from the same gene** that also generates circRNAs:

```
Linear mRNA structure:
Exon1 → Exon2 → Exon3
    FSJ     FSJ

(Both FSJs follow normal 5' → 3' directionality)
```

**Why measure FSJ for circRNA analysis?**

Even when a gene produces circRNAs, it **simultaneously produces linear transcripts**. Measuring both FSJ and BSJ provides the complete picture of gene regulation:

**Example scenario: Gene X Expression Analysis**

| Sample | BSJ Reads | FSJ Reads | Total Expression | Interpretation |
| --- | --- | --- | --- | --- |
| Healthy tissue | 100 | 900 | 1,000 | Gene makes 10% circular, 90% linear |
| Cancer sample A | 200 | 1,800 | 2,000 | Gene upregulated 2×, same proportion |
| Cancer sample B | 300 | 700 | 1,000 | Same total, but MORE circular! |

**Key insights from this example:**

- **Cancer A**: Total gene expression doubled (2× upregulation), but the circular/linear ratio stayed the same (10%)

- **Cancer B**: Total expression unchanged, but circularization machinery is more active (30% circular vs 10% in healthy)

- **Different biological meanings**: Cancer A shows transcriptional upregulation; Cancer B shows post-transcriptional regulation

This demonstrates why measuring **both** BSJ and FSJ is essential for understanding circRNA biology.

**Junction Ratio (JR) Calculation:**

```
Junction Ratio (JR) = BSJ / (BSJ + FSJ)
```

This ratio quantifies **circularization efficiency** – what proportion of transcripts from this locus are processed into circular vs linear forms.

---

## Environment Setup: Installing CIRI3 and All Dependencies

CIRI3 provides a conda environment file that installs core dependencies, but additional tools must be installed separately for a complete analysis pipeline.

### Step 1: Install Conda Package Manager

If you don’t have conda installed on your system:

```bash
# Download Miniconda installer for Linux (64-bit)
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Run the installer
bash Miniconda3-latest-Linux-x86_64.sh

# Activate conda in your current shell session
source ~/.bashrc

# Verify successful installation
conda --version
```

This downloads and installs Miniconda, a minimal conda distribution. The installer will ask you to accept the license and choose an installation location. Accepting the default location (~/miniconda3) is recommended.

### Step 2: Clone CIRI3 Repository from GitHub

```bash
# Create a dedicated directory for bioinformatics tools
mkdir -p ~/bioinfo_tools
cd ~/bioinfo_tools

# Clone the CIRI3 repository
git clone https://github.com/gyjames/CIRI3.git

# Navigate into the CIRI3 directory
cd CIRI3
```

This downloads the CIRI3 source code and documentation from GitHub. The repository includes the Java executable files, conda environment specification, and example data.

### Step 3: Create CIRI3 Conda Environment

```bash
# Create the conda environment from the YAML file
conda env create -n CIRI3 -f ./environment.yaml
```

This creates a new conda environment named “CIRI3” with all the dependencies specified in the environment.yaml file. This includes:

- OpenJDK 17 (Java runtime for CIRI3)

- R version 4.2+ with essential packages

- Bioconductor packages (edgeR, limma) for differential expression

- STAR aligner (alternative to BWA-MEM)

- rMATS for differential splicing analysis

- Python scientific computing libraries

### Step 4: Activate the CIRI3 Environment

```bash
# Activate the newly created CIRI3 environment
conda activate CIRI3
```

This switches your shell to use the CIRI3 environment. Your command prompt should now show `(CIRI3)` prefix. You must activate this environment every time you want to use CIRI3 in a new terminal session.

### Step 5: Install Additional Required Tools

```bash
# Install alignment, processing, and data download tools
conda install -c bioconda bwa samtools sra-tools subread -y
```

This installs critical tools not included in the CIRI3 environment file:

- **bwa**: Burrows-Wheeler Aligner for read mapping (required for CIRI3)

- **samtools**: SAM/BAM file manipulation toolkit

- **sra-tools**: Download data from NCBI SRA database (includes fastq-dump)

- **subread**: Package containing featureCounts for gene quantification

**Why separate installation?**

CIRI3 supports multiple aligners (both BWA-MEM and STAR). The environment.yaml includes STAR but not BWA. We install BWA separately because it’s the recommended aligner for CIRI3. SRA Toolkit is optional (only needed if downloading from NCBI). featureCounts is required specifically for DE_BSJ analysis to generate gene expression normalization data.

### Step 6: Verify Complete Installation

```r
# Test Java (CIRI3 is Java-based)
java -version

# Test CIRI3 JAR file
java -jar ~/bioinfo_tools/CIRI3/CIRI3_Java_1.8.0.jar

# Test BWA aligner
bwa 2>&1 | head -n 3

# Test SAMtools
samtools --version

# Test SRA Toolkit
fastq-dump --version

# Test featureCounts (from Subread package)
featureCounts -v

# Test R packages (edgeR and limma)
R -e "library(edgeR); library(limma)"
```

These commands verify that all required software is installed and accessible. Each command should display version information or usage instructions without errors. If any tool fails, revisit the installation steps for that specific component.

---

## Data Preparation: Downloading and Organizing GSE97239

Proper data organization prevents errors and makes analysis reproducible. We’ll create a structured directory system for all inputs and outputs.

### Create Project Directory Structure

```bash
# Create main project directory
mkdir -p ~/CIRI3_analysis
cd ~/CIRI3_analysis

# Create subdirectories for organized workflow
mkdir -p data/raw_fastq          # Raw FASTQ files from SRA
mkdir -p data/reference           # Genome and annotation files
mkdir -p results/alignment        # BWA-MEM output (SAM files)
mkdir -p results/ciri3            # All CIRI3 outputs (single & multi-sample)
mkdir -p results/DE               # Differential expression results
mkdir -p logs                     # Log files for troubleshooting
```

This creates a hierarchical directory structure that separates raw data, reference files, analysis results, and log files. Keeping files organized prevents confusion and makes it easier to locate specific outputs later.

**Verify directory structure:**

```
tree -L 2 ~/CIRI3_analysis
```

**Expected output:**

```
~/CIRI3_analysis/
├── data/
│   ├── raw_fastq/
│   └── reference/
├── results/
│   ├── alignment/
│   ├── ciri3/
│   └── DE/
└── logs/
```

### Download Reference Genome and Annotation

```bash
cd ~/CIRI3_analysis/data/reference

# Download human genome (GRCh38/hg38) from Ensembl
wget ftp://ftp.ensembl.org/pub/release-110/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz

# Download gene annotation (GTF format)
wget ftp://ftp.ensembl.org/pub/release-110/gtf/homo_sapiens/Homo_sapiens.GRCh38.110.gtf.gz

# Decompress files
gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
gunzip Homo_sapiens.GRCh38.110.gtf.gz
```

This downloads the human reference genome (GRCh38/hg38) and gene annotation from Ensembl. The primary assembly contains only primary chromosomes (excluding alternative haplotypes and unplaced scaffolds), which is appropriate for most analyses. The GTF file provides gene and transcript annotations needed for circRNA annotation and gene expression quantification.

### Index Reference Genome for BWA

```bash
cd ~/CIRI3_analysis/data/reference

# Build BWA index
bwa index -a bwtsw Homo_sapiens.GRCh38.dna.primary_assembly.fa
```

This creates the BWA index files required for read alignment. The `-a bwtsw` algorithm is appropriate for large genomes like human. This step is performed **only once** – the index files will be reused for all samples. The indexing process generates several files with extensions .amb, .ann, .bwt, .pac, and .sa.

### Download RNA-seq Data from SRA

```bash
cd ~/CIRI3_analysis/data/raw_fastq

# Download all 6 samples using fastq-dump
# Cancer samples (n=3)
fastq-dump --split-files --gzip SRR5398213
fastq-dump --split-files --gzip SRR5398214
fastq-dump --split-files --gzip SRR5398215

# Control samples (n=3)
fastq-dump --split-files --gzip SRR5398216
fastq-dump --split-files --gzip SRR5398217
fastq-dump --split-files --gzip SRR5398218
```

This downloads the paired-end FASTQ files from NCBI’s Sequence Read Archive. The `--split-files` option separates paired-end reads into two files (_1.fastq.gz and _2.fastq.gz). The `--gzip` option compresses the output to save disk space. Each sample generates two files (Read 1 and Read 2).

---

## Define Paths for Analysis

```bash
# Define paths (used throughout the analysis)
CIRI3_JAR=~/bioinfo_tools/CIRI3/CIRI3_Java_1.8.0.jar
GENOME=~/CIRI3_analysis/data/reference/Homo_sapiens.GRCh38.dna.primary_assembly.fa
GTF=~/CIRI3_analysis/data/reference/Homo_sapiens.GRCh38.110.gtf
FASTQ_DIR=~/CIRI3_analysis/data/raw_fastq
SAM_DIR=~/CIRI3_analysis/results/alignment
CIRI_DIR=~/CIRI3_analysis/results/ciri3
DE_DIR=~/CIRI3_analysis/results/DE
```

Defining these path variables once at the beginning makes the subsequent commands cleaner and reduces the chance of typos. These variables will be used throughout all analysis steps. Make sure to run this block in the same terminal session before running any analysis commands.

---

## Read Alignment with BWA-MEM

### Understanding BWA-MEM Parameters for CircRNA Detection

```bash
cd ~/CIRI3_analysis

# Align all samples with circRNA-optimized parameters
for SRR in SRR5398213 SRR5398214 SRR5398215 SRR5398216 SRR5398217 SRR5398218; do
    bwa mem -T 19 -t 8 \
        ${GENOME} \
        ${FASTQ_DIR}/${SRR}_1.fastq.gz \
        ${FASTQ_DIR}/${SRR}_2.fastq.gz \
        > ${SAM_DIR}/${SRR}.sam
done
```

This performs BWA-MEM alignment for all six samples using a loop. The **critical parameter** is `-T 19`, which sets the minimum alignment score threshold. This lower threshold (compared to default `-T 30`) allows BWA to report split alignments that span back-splice junctions.

---

## CircRNA Detection with CIRI3

CIRI3 provides two operational modes. **Both modes should be run** because different downstream analyses require different input formats.

### Single-Sample Mode (Required for DE_BSJ)

```bash
# Run CIRI3 on each sample individually
for SRR in SRR5398213 SRR5398214 SRR5398215 SRR5398216 SRR5398217 SRR5398218; do
    java -jar ${CIRI3_JAR} \
        -I ${SAM_DIR}/${SRR}.sam \
        -O ${CIRI_DIR}/${SRR}_ciri3 \
        -F ${GENOME} \
        -A ${GTF}
done
```

This runs CIRI3 in single-sample mode (default `-W 0`) on each sample separately. Each sample is processed independently, and CIRI3 detects circRNAs by identifying reads with back-splice junction patterns. The `-A ${GTF}` parameter provides gene annotation, which allows CIRI3 to classify circRNAs by type (exonic, intronic, intergenic) and associate them with host genes. Single-sample mode outputs are required for the DE_BSJ (BSJ-based differential expression) analysis.

**Output files**: 6 individual files (`SRR*_ciri3.txt`), one per sample.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/Single_BSJ_output.png?resize=1024%2C75&ssl=1)

### Multi-Sample Mode (Required for DE_Ratio and DE_Relative)

```bash
# Create sample list file with absolute paths
cat > ${CIRI_DIR}/sample_list.txt << EOF
${SAM_DIR}/SRR5398213.sam
${SAM_DIR}/SRR5398214.sam
${SAM_DIR}/SRR5398215.sam
${SAM_DIR}/SRR5398216.sam
${SAM_DIR}/SRR5398217.sam
${SAM_DIR}/SRR5398218.sam
EOF

# Run CIRI3 in multi-sample mode
java -jar ${CIRI3_JAR} \
    -I ${CIRI_DIR}/sample_list.txt \
    -O ${CIRI_DIR}/all_samples.txt \
    -F ${GENOME} \
    -A ${GTF} \
    -W 1 \
    -T 8
```

This runs CIRI3 in multi-sample mode (`-W 1`), analyzing all samples together. The sample_list.txt file contains absolute paths to all SAM files. Multi-sample mode is more efficient than running samples separately and automatically generates combined expression matrices. The `-T 8` parameter uses 8 threads for parallel processing. This mode is required for DE_Ratio and DE_Relative analyses because these methods need the BSJ and FSJ matrices that are only generated in multi-sample mode.

**Output files:**

- `all_samples.txt`: CircRNA information for all detected circRNAs across all samples

- `all_samples.txt.BSJ_Matrix`: BSJ read counts matrix (rows = circRNAs, columns = samples)

- `all_samples.txt.FSJ_Matrix`: FSJ read counts matrix (rows = circRNAs, columns = samples)

These matrices provide comprehensive quantification data needed for junction ratio and isoform switching analyses.

**`all_samples.txt`:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/ciri3_multi_sample_all_samples.png?resize=1024%2C73&ssl=1)

**`all_samples.txt.BSJ_Matrix`: **

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/ciri3_multi_sample_all_samples.txt.BSJ_Matrix.png?resize=1024%2C145&ssl=1)

**`all_samples.txt.FSJ_Matrix`:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/ciri3_multi_sample_all_samples.txt.FSJ_Matrix.png?resize=1024%2C147&ssl=1)

---

## Gene Expression Quantification for DE_BSJ Normalization

**Why is this needed?** The DE_BSJ function uses gene expression levels to normalize circRNA counts. This accounts for differences in gene transcription between samples, allowing us to distinguish between:

- Transcriptional changes (gene upregulated → more circRNA)

- Circularization changes (same gene expression → more circular isoform)

### Convert SAM to Sorted BAM

```bash
# Convert SAM to BAM, sort, and index
for SRR in SRR5398213 SRR5398214 SRR5398215 SRR5398216 SRR5398217 SRR5398218; do
    samtools view -bS ${SAM_DIR}/${SRR}.sam > ${SAM_DIR}/${SRR}.bam
    samtools sort ${SAM_DIR}/${SRR}.bam -o ${SAM_DIR}/${SRR}.sorted.bam
    samtools index ${SAM_DIR}/${SRR}.sorted.bam
done
```

This converts SAM files to compressed BAM format, sorts them by genomic coordinate, and creates index files. featureCounts requires sorted BAM files for efficient gene-level counting. The `samtools view -bS` converts SAM to BAM, `samtools sort` sorts by coordinate, and `samtools index` creates .bai index files.

### Run featureCounts for Gene Expression

```bash
# Quantify gene expression using featureCounts
featureCounts -p -T 8 -t exon -g gene_id \
    -a ${GTF} \
    -o ${DE_DIR}/gene_counts.txt \
    ${SAM_DIR}/*.sorted.bam
```

This counts how many reads map to each gene using the GTF annotation. The `-p` flag indicates paired-end data, `-T 8` uses 8 threads, `-t exon` counts reads mapping to exons, and `-g gene_id` summarizes counts by gene. The output includes read counts for all genes across all samples, which will be used to normalize circRNA expression in DE_BSJ analysis.

### Format Gene Expression Matrix for CIRI3

```bash
# Extract gene counts and clean up column names
cut -f1,7-12 ${DE_DIR}/gene_counts.txt | \
    tail -n +2 > ${DE_DIR}/gene_expression_temp.txt

# Remove file paths and .sorted.bam suffix from sample names
sed -i 's|.*/||g; s/\.sorted\.bam//g' ${DE_DIR}/gene_expression_temp.txt

# Add proper header
echo -e "Geneid\tSRR5398213\tSRR5398214\tSRR5398215\tSRR5398216\tSRR5398217\tSRR5398218" \
    > ${DE_DIR}/gene_expression.txt
tail -n +2 ${DE_DIR}/gene_expression_temp.txt >> ${DE_DIR}/gene_expression.txt
```

This reformats the featureCounts output for CIRI3. We extract only the gene IDs (column 1) and count columns (7-12), remove the metadata lines, clean up sample names by removing file paths and suffixes, and add a clean header row. The final gene_expression.txt file has gene IDs in column 1 and read counts for each sample in subsequent columns, with sample names matching those used in other analysis files.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/ciri3_gene_expression.png?resize=1024%2C156&ssl=1)

---

## Differential Expression Analysis

CIRI3 provides three statistical models for differential expression analysis, each testing different biological hypotheses.

### DE_BSJ: Absolute CircRNA Abundance Changes

This is the differential expression method in CIRI3. It tests whether absolute circRNA abundance changes between conditions, similar to standard RNA-seq differential expression analysis.

**Create sample information file:**

```
cat > ${DE_DIR}/sample_info_bsj.tsv << EOF
Sample    Path    Class   Num
SRR5398213    ${CIRI_DIR}/SRR5398213_ciri3.txt   Cancer  1
SRR5398214    ${CIRI_DIR}/SRR5398214_ciri3.txt   Cancer  2
SRR5398215    ${CIRI_DIR}/SRR5398215_ciri3.txt   Cancer  3
SRR5398216    ${CIRI_DIR}/SRR5398216_ciri3.txt   Control 1
SRR5398217    ${CIRI_DIR}/SRR5398217_ciri3.txt   Control 2
SRR5398218    ${CIRI_DIR}/SRR5398218_ciri3.txt   Control 3
EOF
```

This creates a tab-separated file linking each sample to its CIRI3 output file, experimental condition (Cancer vs Control), and replicate number. The “Path” column contains absolute paths to single-sample CIRI3 outputs. The “Class” column defines experimental groups. The “Num” column indicates biological replicates (use sequential numbers for unpaired analysis).

**Run DE_BSJ analysis:**

```
java -jar ${CIRI3_JAR} DE_BSJ \
    -I ${DE_DIR}/sample_info_bsj.tsv \
    -G ${DE_DIR}/gene_expression.txt \
    -O ${DE_DIR}/BSJ_DE_results.txt \
    -P 0.05
```

This performs BSJ-based differential expression testing using edgeR. The `-I` parameter provides sample information, `-G` provides gene expression data for normalization (this is why we generated gene_expression.txt), `-O` specifies the output file, and `-P 0.05` sets the p-value threshold. CIRI3 normalizes circRNA counts by their host gene expression, then applies statistical testing to identify significantly different circRNAs between conditions.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/02/ciri3_BSJ_DE_results.png?resize=1024%2C178&ssl=1)

### DE_Ratio: Junction Ratio Changes

**Important Note**: This analysis may encounter Linux system incompatibilities on some systems. Attempt it, but if it fails, report the issue to [CIRI3 GitHub Issues](https://github.com/gyjames/CIRI3/issues) and rely on DE_BSJ results instead.

This tests whether the **proportion of circular vs linear transcripts** changes between conditions, revealing changes in circularization efficiency independent of total gene expression.

**Create sample information file:**

```
cat > ${DE_DIR}/sample_info_ratio.tsv << EOF
Sample    Class
SRR5398213    Cancer
SRR5398214    Cancer
SRR5398215    Cancer
SRR5398216    Control
SRR5398217    Control
SRR5398218    Control
EOF
```

This creates a simpler sample info file with just sample names and experimental classes (no paths needed since this analysis uses the matrices from multi-sample mode).

**Run DE_Ratio analysis:**

```
java -jar ${CIRI3_JAR} DE_Ratio \
    -I ${DE_DIR}/sample_info_ratio.tsv \
    -BM ${CIRI_DIR}/all_samples.txt.BSJ_Matrix \
    -FM ${CIRI_DIR}/all_samples.txt.FSJ_Matrix \
    -O ${DE_DIR}/JR_DE_results.txt \
    -T 8
```

This performs junction ratio-based differential expression. The `-BM` parameter provides the BSJ matrix, `-FM` provides the FSJ matrix (both from multi-sample mode), and `-T 8` uses 8 threads. This analysis tests whether the ratio BSJ/(BSJ+FSJ) differs significantly between conditions. Higher ratio in cancer means increased circularization efficiency even if total gene expression stays the same.

### DE_Relative: Isoform Switching Events

**Important Note**: Like DE_Ratio, this analysis may have Linux system incompatibilities. It’s valuable when it works, but failures are possible.

This tests for **isoform switching events** where the balance between circular and linear isoforms changes, or where different circular isoforms from the same gene shift in proportion.

**Create circRNA-gene mapping file:**

```bash
# Extract circRNA_ID (column 1) and gene_id (column 10) from CIRI3 output
awk -F'\t' 'NR>1 {print $1"\t"$10}' ${CIRI_DIR}/all_samples.txt > ${DE_DIR}/circ_gene.txt
```

This creates a two-column file mapping each circRNA to its host gene. The `awk` command skips the header line (NR>1), then extracts column 1 (circRNA_ID) and column 10 (gene_id) from the multi-sample CIRI3 output. This mapping is required for DE_Relative to identify isoforms from the same gene.

**Run DE_Relative analysis:**

```
java -jar ${CIRI3_JAR} DE_Relative \
    -I ${DE_DIR}/sample_info_ratio.tsv \
    -M ${CIRI_DIR}/all_samples.txt.BSJ_Matrix \
    -GC ${DE_DIR}/circ_gene.txt \
    -O ${DE_DIR}/RE_switching_results.txt \
    -T 8
```

This performs isoform switching analysis. The `-M` parameter provides the BSJ matrix, `-GC` provides the circRNA-to-gene mapping, and the analysis identifies genes where the relative proportions of different circular isoforms change between conditions.

---

## Troubleshooting Common Issues

### Issue 1: Very Few CircRNAs Detected

**Cause**: BWA alignment didn’t use the required `-T 19` parameter.

**Solution**: Check your BWA command and re-run alignment if needed.

```bash
# Verify the parameter was used
grep "bwa mem" ${CIRI_DIR}/../logs/*.log

# Should show: bwa mem -T 19 ...
```

The `-T 19` parameter is absolutely critical. Without it, BWA filters out most BSJ-spanning reads, and CIRI3 will detect very few circRNAs.

### Issue 2: Sample Name Mismatches in DE Analysis

**Symptoms**: “undefined columns selected” or similar errors during DE analysis.

**Solution**: Ensure sample names match exactly across all files.

```bash
# Check sample names in gene expression file
head -n 1 ${DE_DIR}/gene_expression.txt

# Check sample names in BSJ matrix (if using multi-sample mode)
head -n 1 ${CIRI_DIR}/all_samples.txt.BSJ_Matrix

# Check sample names in sample info file
cat ${DE_DIR}/sample_info_bsj.tsv
```

All sample names must be identical (case-sensitive). Common issues include extra path components, file extensions (.sam, .bam), or inconsistent naming.

### Issue 3: DE_Ratio or DE_Relative Failures

**Symptoms**: “Error executing R script” or similar messages.

**Expected**: These functions may fail on some Linux systems due to internal R script incompatibilities.

**Solution**: If DE_Ratio or DE_Relative fail:

1. **Use DE_BSJ results** – this is sufficient for most circRNA studies

2. **Report the issue** on [CIRI3 GitHub Issues](https://github.com/gyjames/CIRI3/issues) with:

- Your Linux distribution and version

- R version (`R --version`)

- Complete error message

DE_BSJ provides robust identification of cancer-associated circRNAs, which is the primary goal for most biomarker studies.

---

## Conclusion

Congratulations! You’ve completed a comprehensive circRNA analysis using CIRI3, from raw sequencing data through differential expression analysis.

### Key Takeaways

1. **Run both detection modes**: Single-sample for DE_BSJ, multi-sample for DE_Ratio/DE_Relative

2. **DE_BSJ is most reliable**: Identifies cancer-associated circRNAs effectively

3. **Gene expression normalization matters**: Distinguishes transcriptional from post-transcriptional regulation

4. **Tool limitations exist**: DE_Ratio and DE_Relative may fail on some systems

5. **Always validate experimentally**: RT-PCR, Sanger sequencing, RNase R treatment

### Next Steps

**Immediate Actions:**

1. Validate top differentially expressed circRNAs by RT-PCR with divergent primers

2. Search circRNA databases (circBase, circAtlas) for known functions

3. Predict miRNA binding sites for circRNAs of interest

4. Check if circRNAs are detectable in plasma/exosomes (biomarker potential)

**Advanced Analyses:**

1. Functional enrichment analysis of host genes

2. CircRNA-miRNA-mRNA network construction

3. Clinical correlation with patient outcomes

4. Mechanistic studies (knockdown/overexpression)

### Community Resources

**Official Resources:**

- **CIRI3 GitHub**: https://github.com/gyjames/CIRI3

**CircRNA Databases:**

- **circBase**: [http://www.circbase.org/](http://www.circbase.org/) (curated circRNA database)

- **circAtlas**: [http://circatlas.biols.ac.cn/](http://circatlas.biols.ac.cn/) (tissue expression atlas)

- **circRNADb**: [http://reprod.njmu.edu.cn/circrnadb](http://reprod.njmu.edu.cn/circrnadb) (functional annotations)

- **circInteractome**: [https://circinteractome.irp.nia.nih.gov/](https://circinteractome.irp.nia.nih.gov/) (miRNA binding predictions)

---

## References

### Key Publications

1. **CIRI3 Method Paper**Zhang J, Chen S, Yang J, Zhao F. *Accurate quantification of circular RNAs identifies extensive circular isoform switching events.* Nat Commun. 2020;11(1):90. doi:10.1038/s41467-019-13840-9

2. **CIRI2 Algorithm**Gao Y, Zhang J, Zhao F. *Circular RNA identification based on multiple seed matching.* Brief Bioinform. 2018;19(5):803-810. doi:10.1093/bib/bbx014

3. **CircRNA Review**Kristensen LS, Andersen MS, Stagsted LVW, et al. *The biogenesis, biology and characterization of circular RNAs.* Nat Rev Genet. 2019;20(11):675-691. doi:10.1038/s41576-019-0158-7

4. **Exosomal CircRNAs**Li Y, Zheng Q, Bao C, et al. *Circular RNA is enriched and stable in exosomes: a promising biomarker for cancer diagnosis.* Cell Res. 2015;25(8):981-984. doi:10.1038/cr.2015.82

5. **BWA Aligner**Li H, Durbin R. *Fast and accurate short read alignment with Burrows-Wheeler transform.* Bioinformatics. 2009;25(14):1754-1760. doi:10.1093/bioinformatics/btp324

6. **edgeR Package**Robinson MD, McCarthy DJ, Smyth GK. *edgeR: a Bioconductor package for differential expression analysis of digital gene expression data.* Bioinformatics. 2010;26(1):139-140. doi:10.1093/bioinformatics/btp616

7. **featureCounts**Liao Y, Smyth GK, Shi W. *featureCounts: an efficient general purpose program for assigning sequence reads to genomic features.* Bioinformatics. 2014;30(7):923-930. doi:10.1093/bioinformatics/btt656

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, please comment and share your experience to help other researchers! Subscribe to stay updated with our latest bioinformatics tutorials and resources.*
