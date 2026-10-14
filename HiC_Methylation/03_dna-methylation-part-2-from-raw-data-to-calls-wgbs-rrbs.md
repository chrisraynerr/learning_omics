# How To Analyze DNA Methylation Data For Absolute Beginners Part 2: From Raw Data to Methylation Calls with WGBS and RRBS

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/)

- [Introduction: Understanding DNA Methylation Sequencing Technologies](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Introduction_Understanding_DNA_Methylation_Sequencing_Technologies)

  - [What is DNA Methylation and Why Study It?](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#What_is_DNA_Methylation_and_Why_Study_It)
  - [Comparing DNA Methylation Analysis Methods](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Comparing_DNA_Methylation_Analysis_Methods)
  - [The WGBS and RRBS Preprocessing Workflow](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#The_WGBS_and_RRBS_Preprocessing_Workflow)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Setting_Up_Your_Analysis_Environment)

  - [Creating a Specialized Conda Environment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Creating_a_Specialized_Conda_Environment)
  - [Preparing Reference Genome Files](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Preparing_Reference_Genome_Files)

- [Downloading Example Dataset](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Downloading_Example_Dataset)

  - [Understanding the Dataset](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_the_Dataset)

- [Quality Control Assessment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Quality_Control_Assessment)

  - [Running FastQC for Quality Assessment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Running_FastQC_for_Quality_Assessment)
  - [Key Quality Metrics for Paired-End Bisulfite Sequencing](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Key_Quality_Metrics_for_Paired-End_Bisulfite_Sequencing)

- [Adapter Trimming and Quality Filtering](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Adapter_Trimming_and_Quality_Filtering)

  - [Trimming with Trim Galore](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Trimming_with_Trim_Galore)
  - [Understanding Paired-End WGBS vs RRBS Trimming](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_Paired-End_WGBS_vs_RRBS_Trimming)

- [Bisulfite Sequencing Alignment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Bisulfite_Sequencing_Alignment)

  - [Alignment with Bismark](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Alignment_with_Bismark)
  - [Understanding Bismark Output Files](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_Bismark_Output_Files)
  - [Understanding Paired-End Bisulfite Alignment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_Paired-End_Bisulfite_Alignment)
  - [Understanding RRBS-Specific Alignment Parameters](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_RRBS-Specific_Alignment_Parameters)
  - [Understanding Bismark Alignment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_Bismark_Alignment)

- [Post-Alignment Processing and Quality Control](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Post-Alignment_Processing_and_Quality_Control)

  - [Deduplication and Quality Assessment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Deduplication_and_Quality_Assessment)
  - [Understanding Deduplication Output Files](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_Deduplication_Output_Files)
  - [Critical Quality Control Metrics for Paired-End Data](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Critical_Quality_Control_Metrics_for_Paired-End_Data)

- [Methylation Calling and Quantification](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Methylation_Calling_and_Quantification)

  - [Extracting Methylation Data](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Extracting_Methylation_Data)
  - [Understanding Methylation Extraction Output Files](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_Methylation_Extraction_Output_Files)
  - [Paired-End Specific Methylation Extraction Parameters](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Paired-End_Specific_Methylation_Extraction_Parameters)
  - [Understanding Methylation Output Formats](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Understanding_Methylation_Output_Formats)

- [Best Practices for Methylation Preprocessing](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Best_Practices_for_Methylation_Preprocessing)

  - [Experimental Design Considerations](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Experimental_Design_Considerations)
  - [Common Pitfalls and Solutions](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Common_Pitfalls_and_Solutions)
  - [Technology Selection for Future Studies](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Technology_Selection_for_Future_Studies)

- [References](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-part-2-from-raw-data-to-methylation-calls-with-wgbs-and-rrbs/#Like_this)

## Introduction: Understanding DNA Methylation Sequencing Technologies

DNA methylation represents one of the most important epigenetic modifications in mammalian genomes, playing crucial roles in gene regulation, development, and disease. This chemical modification occurs when a methyl group is added to cytosine bases, primarily in CpG dinucleotide contexts. Understanding where and how methylation patterns change across different conditions, tissues, or disease states provides invaluable insights into biological processes and potential therapeutic targets.

For researchers entering the field of epigenomics, choosing the right methylation profiling method and understanding how to analyze the resulting data can seem overwhelming. This tutorial provides a comprehensive, beginner-friendly guide to preprocessing two of the most powerful DNA methylation sequencing technologies: Whole Genome Bisulfite Sequencing (WGBS) and Reduced Representation Bisulfite Sequencing (RRBS).

### What is DNA Methylation and Why Study It?

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/DNA-Methylation-Transcription.png?resize=1024%2C947&ssl=1)

DNA methylation is an epigenetic mark that can regulate gene expression without changing the underlying DNA sequence. In mammals, methylation predominantly occurs at cytosine residues in CpG dinucleotides, where it can silence gene expression by preventing transcription factor binding or recruiting repressive proteins.

Methylation patterns are established during development and maintained through cell divisions, making them particularly important for:

- **Tissue-specific gene expression:** Different cell types have unique methylation signatures that help maintain their identity

- **Genomic imprinting:** Parent-of-origin specific gene expression controlled by differential methylation

- **X-chromosome inactivation:** Silencing of one X chromosome in female mammals

- **Disease progression:** Cancer cells often show global hypomethylation and focal hypermethylation of tumor suppressor genes

> 

**Beginner’s Tip:** Think of DNA methylation as a “molecular switch” that can turn genes on or off without changing the genetic code itself. Just as dimmer switches control light brightness, methylation fine-tunes gene expression levels.

### Comparing DNA Methylation Analysis Methods

Before diving into analysis, it’s crucial to understand the different technologies available for studying DNA methylation. Each method has distinct advantages and is suited for different research questions:

**Feature****Illumina EPIC BeadChip****RRBS****WGBS****Coverage****Genome Coverage****Resolution****Input DNA Required****Cost per Sample****Sequencing Required****Analysis Complexity****Best Use Cases****Pros****Cons****Typical Data Size**

|  |  |  |  |
| --- | --- | --- | --- |
|  | ~850,000 CpG sites | 1-4 million CpGs | ~28 million CpGs (human) |
|  | <3% of CpGs | 5-10% of CpGs | >95% of CpGs |
|  | Single CpG | Single CpG | Single CpG |
|  | 250 ng – 1 μg | 100 ng – 1 μg | 1-5 μg |
|  | Low ($50-100) | Medium ($200-400) | High ($500-1000) |
|  | No | Yes | Yes |
|  | Low | Medium | High |
|  | Large cohort studies, clinical samples | Focused studies, limited samples | Comprehensive methylome profiling |
|  | Cost-effective, standardized, easy analysis | Good coverage/cost balance, CpG island focus | Complete methylome, novel region discovery |
|  | Limited coverage, probe bias | Enzyme bias, incomplete coverage | Expensive, large data files |
|  | ~20 MB per sample | 2-5 GB per sample | 10-30 GB per sample |

**When to Choose Each Method:**

- **EPIC BeadChip:** Population studies, biomarker discovery, clinical applications where cost is a major factor

- **RRBS:** Pilot studies, focused analyses of CpG islands and promoters, when sample material is limited

- **WGBS:** Comprehensive methylome mapping, discovery of novel methylated regions, studies requiring complete coverage

### The WGBS and RRBS Preprocessing Workflow

Both WGBS and RRBS share a common preprocessing framework, though with method-specific considerations:

**1. Experimental Design and Sample Preparation**

- Bisulfite treatment converts unmethylated cytosines to uracil (read as thymine after PCR)

- RRBS includes MspI digestion followed by size selection to enrich for CpG-rich regions

- WGBS processes the entire genome without restriction enzyme digestion

**2. Quality Control and Preprocessing**

- Assess sequencing quality and adapter contamination

- Remove low-quality reads and adapter sequences

- Check for bisulfite conversion efficiency

**3. Alignment to Reference Genome**

- Specialized aligners account for C-to-T conversions from bisulfite treatment

- Reference genome is converted to match expected bisulfite-treated sequences

- Both forward and reverse strand alignments are considered

**4. Methylation Calling and Quantification**

- Count methylated (C) and unmethylated (T) reads at each CpG site

- Calculate methylation levels as a percentage

- Filter sites based on coverage thresholds

## Setting Up Your Analysis Environment

Before beginning any methylation analysis, we need to establish a robust computational environment with all necessary tools and dependencies.

### Creating a Specialized Conda Environment

Let’s create a dedicated environment for methylation analysis that includes all the tools we’ll need:

```bash
# Create a dedicated conda environment
conda create -n methylation_analysis python=3.9
conda activate methylation_analysis

# Configure conda channels
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict

# Install essential tools for methylation analysis
conda install -y \
    wget sra-tools fastqc multiqc trim-galore \
    bismark bowtie2 samtools bedtools methyldackel \
    deeptools ucsc-wigtobigwig ucsc-bedgraphtobigwig \
    ucsc-fetchchromsizes
```

### Preparing Reference Genome Files

For this tutorial, we’ll use the human genome (hg38). Bismark requires specially prepared reference files:

```bash
# Create directory structure for reference files
mkdir -p ~/methylation_analysis/references
cd ~/methylation_analysis/references

# Download human genome reference (hg38)
wget http://hgdownload.cse.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz
gunzip ~/methylation_analysis/references/hg38.fa.gz

# Download chromosome sizes file for hg38
fetchChromSizes hg38 > ~/methylation_analysis/references/hg38.chrom.sizes

# Prepare Bismark reference genome
bismark_genome_preparation \
    --path_to_aligner bowtie2 \
    --verbose \
    --parallel 16 \
    ~/methylation_analysis/references/
```

> 

**Important Note:** The genome preparation step creates bisulfite-converted versions of the reference genome and can take substantial time and disk space (~12GB for human genome).

## Downloading Example Dataset

For this tutorial, we’ll analyze data from GEO dataset [GSE46644](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE46644), which contains WGBS data comparing DNA methylation patterns between normal and Alzheimer’s disease samples.

### Understanding the Dataset

The GSE46644 dataset contains paired-end WGBS profiles from:

- **SRR949193:** Normal sample 1

- **SRR949196:** Normal sample 2

- **SRR949199:** Disease sample 1 (Alzheimer’s disease)

- **SRR949202:** Disease sample 2 (Alzheimer’s disease)

This design allows us to perform differential methylation analysis between normal and Alzheimer’s disease conditions.

```bash
# Create project directory structure
mkdir -p ~/methylation_analysis/GSE46644/raw_data
cd ~/methylation_analysis/GSE46644/raw_data

# Download one sample (repeat for all samples)
fasterq-dump SRR949193.sra
gzip *.fastq

# Rename with descriptive names for paired-end files
mv ~/methylation_analysis/GSE46644/raw_data/SRR949193_1.fastq.gz ~/methylation_analysis/GSE46644/raw_data/Normal_1_R1.fastq.gz
mv ~/methylation_analysis/GSE46644/raw_data/SRR949193_2.fastq.gz ~/methylation_analysis/GSE46644/raw_data/Normal_1_R2.fastq.gz
```

> 

**Note:** Repeat the download process for all four samples (SRR949196, SRR949199, SRR949202), renaming them as Normal_2, Disease_1, and Disease_2 respectively. Each sample will produce two files (R1 and R2) for paired-end sequencing.

## Quality Control Assessment

Before proceeding with analysis, we must assess the quality of our sequencing data to identify any potential issues that could affect downstream results.

### Running FastQC for Quality Assessment

```bash
# Create directory for QC results
mkdir -p ~/methylation_analysis/GSE46644/qc/raw_data
cd ~/methylation_analysis/GSE46644

# Run FastQC on one paired-end sample (repeat for all samples)
fastqc \
    ~/methylation_analysis/GSE46644/raw_data/Normal_1_R1.fastq.gz \
    ~/methylation_analysis/GSE46644/raw_data/Normal_1_R2.fastq.gz \
    --outdir ~/methylation_analysis/GSE46644/qc/raw_data \
    --threads 20

# Aggregate QC results with MultiQC
multiqc ~/methylation_analysis/GSE46644/qc/raw_data \
    --outdir ~/methylation_analysis/GSE46644/qc/raw_data \
    --title "WGBS Raw Data QC"
```

### Key Quality Metrics for Paired-End Bisulfite Sequencing

When reviewing your FastQC and MultiQC reports, pay particular attention to:

**1. Sequence Quality Scores**

- Overall quality should be high (Phred score >20 for most bases)

- Quality typically decreases toward the 3′ end of reads

- R2 reads often show lower quality than R1 reads

**2. Base Composition**

- Expect reduced cytosine content due to bisulfite conversion

- Look for adapter contamination (especially in RRBS data)

- Check for consistent patterns between R1 and R2 reads

**3. Sequence Length Distribution**

- WGBS should show more uniform fragment sizes

- RRBS shows characteristic size distribution from MspI digestion

- Paired-end reads should have similar length distributions

**4. Duplication Levels**

- WGBS should show <30% duplication

- Some duplication is expected in RRBS due to size selection

## Adapter Trimming and Quality Filtering

Bisulfite sequencing libraries often contain adapter sequences that must be removed before alignment. For paired-end WGBS data, we need to process both reads simultaneously to maintain proper pairing.

### Trimming with Trim Galore

```bash
# Create directory for trimmed files
mkdir -p ~/methylation_analysis/GSE46644/trimmed

# Trim adapters for one paired-end sample (repeat for all samples)
# For paired-end WGBS (current dataset):
trim_galore \
    --fastqc \
    --paired \
    --cores 16 \
    ~/methylation_analysis/GSE46644/raw_data/Normal_1_R1.fastq.gz \
    ~/methylation_analysis/GSE46644/raw_data/Normal_1_R2.fastq.gz \
    -o ~/methylation_analysis/GSE46644/trimmed

# For paired-end RRBS data, you would use:
# trim_galore \
#     --fastqc \
#     --paired \
#     --cores 16 \
#     --rrbs \
#     --non_directional \
#     ~/methylation_analysis/GSE46644/raw_data/sample_R1.fastq.gz \
#     ~/methylation_analysis/GSE46644/raw_data/sample_R2.fastq.gz \
#     -o ~/methylation_analysis/GSE46644/trimmed

# Generate QC report for trimmed data
mkdir -p ~/methylation_analysis/GSE46644/qc/trimmed
multiqc ~/methylation_analysis/GSE46644/trimmed/*_fastqc.zip \
    --outdir ~/methylation_analysis/GSE46644/qc/trimmed \
    --title "WGBS Trimmed Data QC"
```

### Understanding Paired-End WGBS vs RRBS Trimming

For paired-end WGBS data, we use the `--paired` flag with standard trimming parameters:

- **`--paired`:** Processes both reads simultaneously and maintains proper pairing

- **Standard adapter trimming:** Removes common Illumina adapters from both reads

- **Quality filtering:** Removes low-quality bases from read ends

- **Length filtering:** Discards read pairs where either read becomes too short after trimming

For paired-end RRBS data, additional parameters are needed:

- **`--rrbs`:** Applies RRBS-specific trimming (M-bias removal, fragment end trimming)

- **`--non_directional`:** For non-directional RRBS libraries (some protocols)

- **More aggressive filtering:** Due to size selection in RRBS protocol

> 

**Paired-End Considerations:** Trim Galore automatically maintains read pairing by discarding both reads if either fails quality filters. This ensures proper alignment and prevents orphaned reads.

## Bisulfite Sequencing Alignment

Aligning bisulfite-treated sequencing reads requires specialized algorithms that account for the C-to-T conversions introduced during bisulfite treatment.

### Alignment with Bismark

```bash
# Create directory for alignment results
mkdir -p ~/methylation_analysis/GSE46644/alignment/Normal_1
cd ~/methylation_analysis/GSE46644

# Set variables for convenience
REF_GENOME="~/methylation_analysis/references"
THREADS=16

# Align one paired-end sample (repeat for all samples)
# For paired-end WGBS (current dataset):
bismark \
    --genome_folder ${REF_GENOME} \
    --parallel ${THREADS} \
    -1 ~/methylation_analysis/GSE46644/trimmed/Normal_1_R1_val_1.fq.gz \
    -2 ~/methylation_analysis/GSE46644/trimmed/Normal_1_R2_val_2.fq.gz \
    --output_dir ~/methylation_analysis/GSE46644/alignment/Normal_1/

# For paired-end RRBS data, you would use additional parameters:
# bismark \
#     --genome_folder ${REF_GENOME} \
#     --parallel ${THREADS} \
#     --non_directional \
#     --score_min L,0,-0.6 \
#     -1 ~/methylation_analysis/GSE46644/trimmed/sample_R1_val_1.fq.gz \
#     -2 ~/methylation_analysis/GSE46644/trimmed/sample_R2_val_2.fq.gz \
#     --output_dir ~/methylation_analysis/GSE46644/alignment/sample/
```

### Understanding Bismark Output Files

After Bismark alignment, you’ll see several important output files. Let’s understand what each file represents:

**Primary Alignment File:**

```
Normal_1_R1_val_1_bismark_bt2_pe.bam (~40-50G)
```

- Contains all aligned paired-end reads with methylation information

- Large file size reflects high-coverage WGBS data

- Essential for all downstream methylation analysis

**Alignment Report:**

```
Normal_1_R1_val_1_bismark_bt2_PE_report.txt (few KB)
```

- Contains detailed alignment statistics

- Reports mapping efficiency, bisulfite conversion rates

- Critical for quality assessment

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/Normal_1_R1_val_1_bismark_bt2_PE_report.png?resize=1024%2C876&ssl=1)

### Understanding Paired-End Bisulfite Alignment

For paired-end bisulfite sequencing, Bismark uses both reads to improve alignment accuracy:

**1. Paired-End Advantages**

- **Improved mapping accuracy:** Both reads must align consistently to the same fragment

- **Better unique mapping:** Paired information helps resolve ambiguous alignments

- **Fragment size validation:** Insert size information helps validate proper alignment

**2. Bismark Paired-End Processing**

- Aligns both reads independently to all four converted reference genomes

- Validates that paired reads align to the same genomic fragment

- Determines the original strand and calculates insert sizes

- Reports concordant pairs with proper orientation and distance

### Understanding RRBS-Specific Alignment Parameters

For RRBS analysis, several additional Bismark parameters are important:

**1. `--non_directional`**

- Required for non-directional RRBS libraries

- Allows alignment to all four bisulfite conversion states

- Essential for some RRBS protocols that don’t preserve strand information

**2. `--score_min L,0,-0.6`**

- More lenient alignment scoring for RRBS

- Accounts for increased mismatches due to bisulfite conversion

- Default is `-0.2`, but RRBS may need `-0.6` for short fragments

**3. Coverage Considerations**

- RRBS typically has lower per-CpG coverage than WGBS

- May need to adjust minimum coverage thresholds in downstream analysis

- Fragment size selection affects coverage distribution

### Understanding Bismark Alignment

Bismark uses a unique approach to handle bisulfite-converted sequences:

1. **Reference Conversion:** Creates four versions of the reference genome (C→T and G→A conversions for both strands)

2. **Read Conversion:** Converts reads to match the reference conversions

3. **Alignment:** Uses Bowtie2 to align converted reads to converted references

4. **Post-processing:** Determines original strand and methylation status

> 

**Alignment Quality Metrics:**

- Mapping efficiency should be >70% for good quality data

- Bisulfite conversion rate should be >99%

- Balanced alignment to both strands indicates unbiased conversion

## Post-Alignment Processing and Quality Control

After alignment, we need to remove duplicates and assess the quality of our bisulfite conversion and alignment.

### Deduplication and Quality Assessment

```bash
# Create directories for processed alignments
mkdir -p ~/methylation_analysis/GSE46644/qc/alignment

# Remove PCR duplicates from one paired-end sample (repeat for all samples)
deduplicate_bismark \
    -p \
    ~/methylation_analysis/GSE46644/alignment/Normal_1/Normal_1_R1_val_1_bismark_bt2_pe.bam \
    --output_dir ~/methylation_analysis/GSE46644/alignment/Normal_1/

# Generate alignment and methylation QC reports for one sample (repeat for all)
bismark2report \
    --alignment_report ~/methylation_analysis/GSE46644/alignment/Normal_1/Normal_1_R1_val_1_bismark_bt2_PE_report.txt \
    --dedup_report ~/methylation_analysis/GSE46644/alignment/Normal_1/Normal_1_R1_val_1_bismark_bt2_pe.deduplication_report.txt \
    --output_dir ~/methylation_analysis/GSE46644/alignment/Normal_1/

# Aggregate all QC reports
multiqc ~/methylation_analysis/GSE46644/alignment/ \
    --outdir ~/methylation_analysis/GSE46644/qc/alignment \
    --title "WGBS Alignment QC"
```

### Understanding Deduplication Output Files

After deduplication, you’ll get additional important files:

**Deduplicated Alignment File:**

```
Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bam (~35-40G)
```

- PCR duplicates removed from the original BAM file

- Slightly smaller than original (~5-15% size reduction typical)

- Used for all downstream methylation extraction

**Deduplication Report:**

```
Normal_1_R1_val_1_bismark_bt2_pe.deduplication_report.txt (few KB)
```

- Reports duplicate removal statistics

- Shows percentage of duplicates removed

- Important for assessing library quality

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/Normal_1_R1_val_1_bismark_bt2_pe.deduplication_report.png?resize=1024%2C139&ssl=1)

### Critical Quality Control Metrics for Paired-End Data

Review these key metrics in your alignment reports:

**1. Mapping Efficiency**

- Good: >70% concordant pairs (both reads mapping properly)

- Concerning: <50% concordant pairs

**2. Bisulfite Conversion Rate**

- Good: >98% conversion efficiency

- Concerning: <95% conversion efficiency

**3. Insert Size Distribution**

- Should show expected fragment size distribution

- Typical WGBS fragments: 200-800 bp

- RRBS fragments: shorter, 40-300 bp due to size selection

**4. Duplication Rate**

- WGBS should show <30% duplication

- RRBS typically shows 20-60% duplication (due to size selection)

**5. Strand Bias and Directionality**

- Reads should align approximately equally to both strands

- Proper paired-end orientation (forward-reverse)

## Methylation Calling and Quantification

Now we extract methylation information from our aligned reads to quantify methylation levels at individual CpG sites.

### Extracting Methylation Data

```bash
# Create directories for methylation data
mkdir -p ~/methylation_analysis/GSE46644/methylation/raw
mkdir -p ~/methylation_analysis/GSE46644/methylation/bedgraph

# Extract methylation information from one paired-end sample (repeat for all samples)
# For paired-end WGBS (current dataset):
bismark_methylation_extractor \
    -p \
    --bedGraph \
    --parallel 16 \
    --gzip \
    ~/methylation_analysis/GSE46644/alignment/Normal_1/Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bam \
    --output ~/methylation_analysis/GSE46644/alignment/Normal_1/

# For paired-end RRBS data, you might use additional parameters:
# bismark_methylation_extractor \
#     -p \
#     --bedGraph \
#     --parallel 16 \
#     --gzip \
#     --ignore 5 \
#     --ignore_3prime 5 \
#     --ignore_r2 2 \
#     --ignore_3prime_r2 2 \
#     ~/methylation_analysis/GSE46644/alignment/sample/sample_R1_val_1_bismark_bt2_pe.deduplicated.bam \
#     --output ~/methylation_analysis/GSE46644/alignment/sample/

# Process bedGraph file for genome browser viewing
# Step 1: Decompress the bedGraph file
gunzip ~/methylation_analysis/GSE46644/alignment/Normal_1/Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bedGraph.gz

# Step 2: Sort bedGraph file by chromosome and position
sort -k1,1 -k2,2n \
    ~/methylation_analysis/GSE46644/alignment/Normal_1/Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bedGraph \
    > ~/methylation_analysis/GSE46644/methylation/bedgraph/Normal_1_sorted.bedGraph

# Step 3: Filter for standard chromosomes only
grep -E "^chr([1-9]|1[0-9]|2[0-2]|X|Y|M)\b" \
    ~/methylation_analysis/GSE46644/methylation/bedgraph/Normal_1_sorted.bedGraph \
    > ~/methylation_analysis/GSE46644/methylation/bedgraph/Normal_1_sorted_chr.bedGraph

# Step 4: Convert to BigWig format
bedGraphToBigWig \
    ~/methylation_analysis/GSE46644/methylation/bedgraph/Normal_1_sorted_chr.bedGraph \
    ~/methylation_analysis/references/hg38.chrom.sizes \
    ~/methylation_analysis/GSE46644/methylation/bedgraph/Normal_1_methylation.bw

# Generate methylation extraction reports
multiqc ~/methylation_analysis/GSE46644/alignment/ \
    --outdir ~/methylation_analysis/GSE46644/qc/methylation \
    --title "WGBS Methylation Extraction"
```

### Understanding Methylation Extraction Output Files

The methylation extraction step produces the most files and is crucial for understanding your results:

**1. Methylation Context Files (Large Text Files)**

```
CHG_OB_Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.txt.gz (~4.1G)
CHG_OT_Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.txt.gz (~4.0G)
CHH_OB_Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.txt.gz (~7.9G)
CHH_OT_Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.txt.gz (~7.8G)
CpG_OB_Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.txt.gz (~1.6G)
CpG_OT_Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.txt.gz (~1.6G)
```

**Context Explanation:**

- **CpG:** Main focus of mammalian methylation (smallest files, ~1.6G each)

- **CHG:** Cytosine-Any base-Guanine context (medium files, ~4G each)

- **CHH:** Cytosine-Non-guanine-Non-guanine context (largest files, ~8G each)

**Strand Explanation:**

- **OT (Original Top):** Forward strand alignments

- **OB (Original Bottom):** Reverse strand alignments

**File Purpose:**

- **What are they**: Raw methylation calls for each individual read at specific cytosine contexts and strands

- **When needed:** Advanced strand-specific analysis, plant methylation studies, custom pipeline development

- **Typical user:** Can delete – information is summarized in .cov files

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/CPG-CHG-CHH.png?resize=1024%2C481&ssl=1)

**2. Summary and Browser Files**

```
Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov.gz (285M)
Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bedGraph.gz (1.4G)
Normal_1_sorted.bedGraph (1.4G)
Normal_1_sorted_chr.bedGraph (1.4G)
Normal_1_methylation.bw (302M)
```

**File Purpose:**

- **.cov.gz:** Coverage file with methylation percentages per CpG (Column Names shown below)

  - Chromosome name

  - Start position of the CpG site

  - End position of the CpG site

  - Methylation percentage

  - Number of reads supporting methylation

  - Number of reads supporting no methylation

- **.bedGraph:** Genome browser tracks showing methylation levels

- **.bw (BigWig):** Compressed, indexed format for fast genome browser viewing

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov_.png?resize=922%2C384&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/wgbs-igv.png?resize=1024%2C438&ssl=1)

**3. Quality Control Files**

```
Normal_1_R1_val_1_bismark_bt2_pe.deduplicated.M-bias.txt (32K)
Normal_1_R1_val_1_bismark_bt2_pe.deduplicated_splitting_report.txt (512 bytes)
```

**File Purpose:**

- **M-bias**: Detect systematic methylation bias at read ends (guides trimming decisions)

- **Splitting report**: Summary of how many calls were made per context/strand

**Quality Indicators from File Sizes:**

- **Low duplication:** Small difference between original and deduplicated BAM sizes

- **Balanced strands:** Similar sizes for OT and OB files

- **Expected context distribution:** CHH > CHG > CpG reflects genomic abundance

- **Good compression:** bedGraph compresses well to BigWig format

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/SRR949193_R1_val_1_bismark_bt2_pe.deduplicated_splitting_report.png?resize=990%2C694&ssl=1)

### Paired-End Specific Methylation Extraction Parameters

For paired-end RRBS analysis, additional parameters may be needed:

**1. `--paired-end`**

- Processes both reads from paired-end sequencing

- Combines methylation information from overlapping reads

- Handles insert size validation

**2. Read-Specific Bias Removal**

- **`--ignore 5` and `--ignore_3prime 5`:** Ignores biased bases at R1 read ends

- **`--ignore_r2 2` and `--ignore_3prime_r2 2`:** Ignores biased bases at R2 read ends

- **Particularly important for RRBS** due to fill-in reactions and end-repair

**3. Coverage and Overlap Handling**

- Paired-end reads often overlap, especially in RRBS

- Bismark automatically handles overlapping regions to avoid double-counting

- Reports combined methylation status from both reads when possible

### Understanding Methylation Output Formats

Bismark generates several output formats:

**1. Cytosine Reports (.cov)**

- Tab-separated format with methylation information for each cytosine

- Columns: chromosome, position, strand, methylated_count, unmethylated_count, context, trinucleotide_context

**2. BedGraph Files (.bedGraph)**

- Genome browser compatible format

- Shows methylation percentage at each covered CpG site

- Ideal for visualization in UCSC Genome Browser or IGV

**3. Methylation Calls (.txt)**

- Detailed methylation information for each read

- Useful for specialized analyses but typically too large for routine use

> 

**Coverage Filtering:** For reliable methylation quantification, most analyses require a minimum coverage of 5-10 reads per CpG site. Sites with lower coverage are typically excluded from downstream analysis.

## Best Practices for Methylation Preprocessing

### Experimental Design Considerations

**1. Sample Size and Power**

- Minimum 4-6 biological replicates per condition for WGBS

- Consider effect sizes typical for your research area (usually 10-20% methylation difference)

- Account for multiple testing correction in power calculations

**2. Quality Control Thresholds**

- Bisulfite conversion efficiency >98%

- Minimum coverage: 3-5 reads per CpG for WGBS, 5-10 for RRBS

- Remove samples with <60% unique mapping rates

- Filter CpG sites with coverage in <75% of samples

**3. Statistical Considerations**

- Use appropriate multiple testing correction (FDR < 0.05 recommended)

- Consider biological effect size (>10% methylation difference)

- Include relevant covariates (age, sex, batch effects)

- Validate key findings with independent methods when possible

### Common Pitfalls and Solutions

**1. Incomplete Bisulfite Conversion**

- **Problem:** False positive methylation calls

- **Detection:** Check CHG and CHH methylation rates (should be <2% in mammals)

- **Solution:** Include unmethylated spike-in controls, optimize bisulfite treatment

**2. Coverage Bias**

- **Problem:** Uneven coverage across genomic regions

- **Detection:** Plot coverage distribution across chromosomes and features

- **Solution:** Apply appropriate coverage filters, normalize by sequencing depth

**3. Batch Effects**

- **Problem:** Technical variation overwhelming biological signal

- **Detection:** PCA clustering by processing batch rather than biology

- **Solution:** Randomize sample processing, include batch in statistical models

**4. Reference Genome Considerations**

- **Problem:** Alignment artifacts due to genetic variation

- **Solution:** Filter SNP-affected probes, consider population-specific references

### Technology Selection for Future Studies

Choose the right technology for your research question:

**WGBS:**

- Comprehensive methylome coverage (>95% of CpGs)

- Novel region discovery and unbiased profiling

- Best for: Discovery studies, non-model organisms, comprehensive profiling

- Cost: High ($500-1000 per sample)

**RRBS:**

- Cost-effective genome-wide coverage (5-10% of CpGs)

- CpG island and promoter focus

- Best for: Focused studies, pilot experiments, limited budgets

- Cost: Medium ($200-400 per sample)

**EPIC Arrays:**

- Standardized, high-throughput analysis

- Large sample sizes and population studies

- Best for: Clinical studies, biomarker discovery, epidemiological research

- Cost: Low ($50-100 per sample)

## References

- Krueger, F. et al. (2012). “DNA methylome analysis using short bisulfite sequencing data.” *Nature Methods*, 9(2), 145-151.

- Ziller, M.J. et al. (2015). “Coverage recommendations for methylation analysis by whole-genome bisulfite sequencing.” *Nature Methods*, 12(3), 230-232.

- Gu, H. et al. (2011). “Preparation of reduced representation bisulfite sequencing libraries for genome-scale DNA methylation profiling.” *Nature Protocols*, 6(4), 468-481.

- Peck et al. (2024). “Can DNA methylation shape climate response in trees?” Trends in Plant Science, Volume 29, Issue 10, p1089-1102 October 2024.

- Nair et al. (2024). “Protocol for high-throughput DNA methylation profiling in rat tissues using automated reduced representation bisulfite sequencing” STAR Protocols 5, 103007 June 21, 2024

---

*This tutorial is part of the NGS101.com comprehensive guide to next-generation sequencing analysis. The preprocessed data generated here serves as the foundation for downstream statistical analysis and biological interpretation of DNA methylation patterns.*
