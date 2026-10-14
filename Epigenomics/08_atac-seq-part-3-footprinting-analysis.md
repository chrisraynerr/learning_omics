# How To Analyze ATAC-seq Data For Absolute Beginners Part 3: Footprinting Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/)

- [Introduction: Unveiling Transcription Factor Binding Sites with ATAC-seq Footprinting](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Introduction_Unveiling_Transcription_Factor_Binding_Sites_with_ATAC-seq_Footprinting)

  - [What is Footprinting Analysis?](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#What_is_Footprinting_Analysis)
  - [Why Do We Need Footprinting Analysis?](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Why_Do_We_Need_Footprinting_Analysis)
  - [Applications in Medical Research: From Cancer Biology to Personalized Medicine](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Applications_in_Medical_Research_From_Cancer_Biology_to_Personalized_Medicine)
  - [The Footprinting Analysis Workflow: Step-by-Step Guide to Transcription Factor Discovery](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#The_Footprinting_Analysis_Workflow_Step-by-Step_Guide_to_Transcription_Factor_Discovery)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Setting_Up_Your_Analysis_Environment)

  - [Required Software Installation](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Required_Software_Installation)
  - [Reference File Preparation](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Reference_File_Preparation)

- [The ATAC-seq Footprinting Analysis Pipeline](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#The_ATAC-seq_Footprinting_Analysis_Pipeline)

  - [Preparing Your Data for Footprinting Analysis](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Preparing_Your_Data_for_Footprinting_Analysis)
  - [Correcting Sequence Bias in ATAC-seq Data](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Correcting_Sequence_Bias_in_ATAC-seq_Data)
  - [Calculating Footprinting Scores](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Calculating_Footprinting_Scores)
  - [Detecting and Comparing Specific Transcription Factor Binding Sites](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Detecting_and_Comparing_Specific_Transcription_Factor_Binding_Sites)
  - [Visualizing Footprints for Individual Transcription Factors](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Visualizing_Footprints_for_Individual_Transcription_Factors)
  - [Creating Heatmaps of Transcription Factor Binding](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Creating_Heatmaps_of_Transcription_Factor_Binding)

- [Interpreting Footprinting Results](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Interpreting_Footprinting_Results)

  - [Understanding Differential Binding Results](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Understanding_Differential_Binding_Results)
  - [Connecting Footprints to Biological Function](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Connecting_Footprints_to_Biological_Function)

- [Best Practices for ATAC-seq Footprinting Analysis](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Best_Practices_for_ATAC-seq_Footprinting_Analysis)

  - [Experimental Design Considerations](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Experimental_Design_Considerations)
  - [Data Processing Best Practices](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Data_Processing_Best_Practices)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Common_Pitfalls_to_Avoid)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Troubleshooting_Common_Issues)

  - [No or Few Detected Footprints](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#No_or_Few_Detected_Footprints)
  - [Inconsistent Results Between Replicates](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Inconsistent_Results_Between_Replicates)
  - [Computational Resource Limitations](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Computational_Resource_Limitations)
  - [Difficulty Interpreting Biological Significance](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Difficulty_Interpreting_Biological_Significance)

- [Advanced ATAC-seq Footprinting Applications](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Advanced_ATAC-seq_Footprinting_Applications)

  - [Integrating Footprinting with Single-cell ATAC-seq](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Integrating_Footprinting_with_Single-cell_ATAC-seq)
  - [Multimodal Integration for Comprehensive Regulatory Analysis](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Multimodal_Integration_for_Comprehensive_Regulatory_Analysis)
  - [Using Footprinting to Study Non-coding Variants](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Using_Footprinting_to_Study_Non-coding_Variants)

- [Conclusion](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-3-footprinting-analysis/#Like_this)

## Introduction: Unveiling Transcription Factor Binding Sites with ATAC-seq Footprinting

After mastering the basics of ATAC-seq data processing and peak calling in our previous tutorials, we now delve into one of the most powerful analytical techniques this assay enables: footprinting analysis. While standard ATAC-seq analysis reveals regions of open chromatin, footprinting takes this a step further by identifying the precise locations where transcription factors are bound within these accessible regions.

### What is Footprinting Analysis?

Footprinting analysis in ATAC-seq is based on a fascinating biological phenomenon: when a transcription factor binds tightly to DNA, it physically protects that specific DNA segment from transposase cutting. This protection creates a characteristic pattern in ATAC-seq data – a localized “dip” or “footprint” in the cutting frequency surrounded by highly accessible regions. These footprints represent the actual binding sites of transcription factors at single-nucleotide resolution, offering a much more precise view than simple peak calling.

> 

**Beginner’s Tip:** Think of footprinting as looking for footprints in the sand. Just as footprints reveal where someone has walked, ATAC-seq footprints reveal where transcription factors have bound to DNA, leaving their mark by protecting those specific bases from transposase activity.

Unlike ChIP-seq, which requires a separate experiment for each transcription factor of interest, ATAC-seq footprinting allows you to simultaneously detect binding sites for hundreds of different transcription factors from a single experiment. This makes it an exceptionally powerful and cost-effective approach for comprehensively mapping the regulatory landscape of cells.

### Why Do We Need Footprinting Analysis?

While standard ATAC-seq peak calling identifies regions of open chromatin, these peaks typically span hundreds of base pairs and don’t tell us which specific transcription factors are bound within these regions or exactly where they bind. Footprinting analysis addresses these limitations by:

1. **Increasing resolution** – Narrowing down binding sites from hundreds of base pairs to the exact nucleotides where transcription factors interact with DNA

2. **Providing factor specificity** – Identifying which transcription factors are likely bound at each site by matching footprints to known binding motifs

3. **Capturing binding dynamics** – Revealing the occupancy status of transcription factor binding sites, distinguishing between sites that contain the recognition sequence but aren’t actually bound versus those that are actively occupied

4. **Enabling multi-factor analysis** – Allowing researchers to study the combinatorial binding of multiple transcription factors and their potential interactions

This level of detail is crucial for understanding the intricate regulatory networks that control gene expression in different cell types and conditions.

### Applications in Medical Research: From Cancer Biology to Personalized Medicine

The applications of ATAC-seq footprinting in medical research are vast and continue to expand:

- **Cancer Research**: Footprinting analysis has revolutionized our understanding of how mutations in non-coding regulatory regions contribute to cancer development. By identifying aberrant transcription factor binding patterns, researchers can uncover novel oncogenic mechanisms beyond protein-coding mutations. For example, studies have used footprinting to reveal how specific transcription factors drive therapy resistance in breast cancer and leukemia.

- **Autoimmune Disorders**: Genome-wide association studies (GWAS) have identified numerous disease-associated variants in non-coding regions, but understanding their functional impact has been challenging. Footprinting analysis helps bridge this gap by showing how these variants disrupt transcription factor binding sites, altering gene expression patterns in immune cells. This approach has provided new insights into diseases like rheumatoid arthritis, lupus, and multiple sclerosis.

- **Drug Discovery**: By mapping the binding dynamics of transcription factors, footprinting can identify new therapeutic targets and help predict drug responses. This is particularly valuable for developing compounds that modulate transcription factor activity or disrupt pathological protein-DNA interactions.

- **Developmental Disorders**: Footprinting analysis of patient-derived cells can reveal how genetic variants affect the developmental regulatory landscape, providing mechanistic insights into congenital disorders and potential avenues for intervention.

- **Personalized Medicine**: Individual differences in transcription factor binding patterns, identified through footprinting analysis, can help explain variable drug responses and disease susceptibility, moving us closer to truly personalized treatment approaches.

### The Footprinting Analysis Workflow: Step-by-Step Guide to Transcription Factor Discovery

For newcomers to computational biology, footprinting analysis might seem daunting, but the process can be broken down into manageable steps:

1. **High-quality alignment** – Ensuring your ATAC-seq reads are properly processed and aligned

2. **Cut site determination** – Converting aligned reads to precise transposase cut sites

3. **Footprint detection** – Identifying regions with characteristic protection patterns

4. **Motif matching** – Connecting footprints to specific transcription factor binding motifs

5. **Differential footprinting** – Comparing footprints across conditions to identify changes in factor binding

6. **Visualization** – Creating intuitive visual representations of footprinting results

7. **Functional analysis** – Connecting footprints to gene regulation and biological pathways

In this tutorial, we’ll guide you through each of these steps using TOBIAS (Transcription factor Occupancy prediction By Investigation of ATAC-seq Signal), a powerful and user-friendly software suite designed specifically for ATAC-seq footprinting analysis. TOBIAS integrates all the necessary analytical steps into a cohesive workflow, making footprinting analysis accessible even to beginners.

Let’s get started with setting up your analysis environment and preparing your data for footprinting analysis.

## Setting Up Your Analysis Environment

Before diving into the footprinting analysis, we need to install the necessary software and prepare the reference files.

### Required Software Installation

We’ll use TOBIAS (Transcription factor Occupancy prediction By Investigation of ATAC-seq Signal) for footprinting analysis. To avoid potential conflicts with other tools, we’ll create a dedicated Conda environment.

```bash
#-----------------------------------------------
# STEP 1: Install TOBIAS and supporting tools
#-----------------------------------------------

# Clone the TOBIAS repository
git clone https://github.com/loosolab/TOBIAS.git
cd TOBIAS

# Create a dedicated conda environment using the provided YAML file
conda env create -f tobias_env.yaml

# Activate the TOBIAS environment
conda activate TOBIAS_ENV

# Install additional tools needed for the analysis
# MACS2 for peak calling (if you haven't already called peaks)
conda install -c bioconda macs2

# Install peak annotation tool UROPA
pip install uropa

# Verify TOBIAS installation
TOBIAS --version
```

> 

**Troubleshooting Tip:** If you encounter errors during installation, ensure you have the latest version of Conda installed. For permission errors, you might need to use `sudo` or contact your system administrator. If specific dependencies fail to install, try installing them individually with `conda install package_name`.

### Reference File Preparation

For footprinting analysis, we need transcription factor (TF) binding motifs to identify which TFs might be binding in our footprints. The most widely used collection of TF binding motifs is the [JASPAR database](https://jaspar.elixir.no/).

```bash
#-----------------------------------------------
# STEP 2: Download transcription factor motifs
#-----------------------------------------------

# Create a directory for TF binding profiles
mkdir -p ~/ATACseq_Footprinting/JASPAR_Database
cd ~/ATACseq_Footprinting/JASPAR_Database

# Download all vertebrate TF binding profiles in a single file
# This contains position frequency matrices (PFMs) in JASPAR format
wget https://jaspar.elixir.no/download/data/2024/CORE/JASPAR2024_CORE_vertebrates_non-redundant_pfms_jaspar.txt

# Alternatively, download individual files for each TF binding profile
wget https://jaspar.elixir.no/download/data/2024/CORE/JASPAR2024_CORE_vertebrates_non-redundant_pfms_jaspar.zip

# Extract the individual motif files
unzip JASPAR2024_CORE_vertebrates_non-redundant_pfms_jaspar.zip

# Check the number of motif files downloaded
ls -l JASPAR2024_CORE_vertebrates_non-redundant_pfms_jaspar | wc -l
```

> 

**Understanding JASPAR Format:** Each TF binding profile is represented as a position frequency matrix (PFM) that shows the frequency of each nucleotide (A, C, G, T) at each position of the binding site. These matrices allow us to scan genomic sequences and identify potential binding sites for specific transcription factors.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-08-at-8.56.00%E2%80%AFAM.png?resize=1024%2C355&ssl=1)

## The ATAC-seq Footprinting Analysis Pipeline

The inputs for footprinting analysis include:

1. BAM files containing aligned ATAC-seq reads

2. Peak files identifying open chromatin regions

If you haven’t generated these files yet, please refer to our first [ATAC-seq tutorial](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/) which covers the entire process from FASTQ to peaks.

### Preparing Your Data for Footprinting Analysis

TOBIAS is designed for comparing single samples. If your experiment involves replicates (as most do), we need to merge these replicates first to increase statistical power and ensure more robust footprint detection.

```bash
#-----------------------------------------------
# STEP 3: Merge BAM and peak files by condition
#-----------------------------------------------

# Create directories for merged files
mkdir -p ~/ATACseq_Footprinting/merged/bam/GroupA \
         ~/ATACseq_Footprinting/merged/bam/GroupB \
         ~/ATACseq_Footprinting/merged/peaks/GroupA \
         ~/ATACseq_Footprinting/merged/peaks/GroupB

# Copy your BAM and peak files to the corresponding folders
# For GroupA (replicates 1-3)
cp ~/ATAC_Dataset/Alignment/Sample{1,2,3}/Sample{1,2,3}.bam ~/ATACseq_Footprinting/merged/bam/GroupA/

# For GroupB (replicates 4-6)
cp ~/ATAC_Dataset/Alignment/Sample{4,5,6}/Sample{4,5,6}.bam ~/ATACseq_Footprinting/merged/bam/GroupB/

# Copy peak files for each replicate
cp ~/ATAC_Dataset/Peaks/Sample{1,2,3}/Sample{1,2,3}.narrowPeak ~/ATACseq_Footprinting/merged/peaks/GroupA/

cp ~/ATAC_Dataset/Peaks/Sample{4,5,6}/Sample{4,5,6}.narrowPeak ~/ATACseq_Footprinting/merged/peaks/GroupB/

# Merge the BAM files for GroupA
cd ~/ATACseq_Footprinting/merged/bam/GroupA
samtools merge -@ 16 GroupA_merged.bam Sample*.bam
rm Sample*.bam  # Remove individual files to save space

# Merge the BAM files for GroupB
cd ~/ATACseq_Footprinting/merged/bam/GroupB
samtools merge -@ 16 GroupB_merged.bam Sample*.bam
rm Sample*.bam  # Remove individual files to save space

# Merge the peak files for GroupA
cd ~/ATACseq_Footprinting/merged/peaks/GroupA
cat Sample*.narrowPeak | sort -k1,1 -k2,2n | bedtools merge > GroupA_merged.narrowPeak
rm Sample*.narrowPeak  # Remove individual files to save space

# Merge the peak files for GroupB
cd ~/ATACseq_Footprinting/merged/peaks/GroupB
cat Sample*.narrowPeak | sort -k1,1 -k2,2n | bedtools merge > GroupB_merged.narrowPeak
rm Sample*.narrowPeak  # Remove individual files to save space
```

> 

**What’s Happening Here:**

- We’re organizing our data into condition-specific directories

- We use `samtools merge` to combine BAM files from replicates

- We concatenate peak files and use `bedtools merge` to combine overlapping peaks

- This approach increases statistical power and provides a cleaner dataset for footprinting analysis

#### Directly Calling Peaks from Merged BAM Files (Alternative Approach)

If you haven’t called peaks for individual replicates or prefer to call peaks on the merged data, you can directly use MACS2 on the merged BAM files:

```bash
#-----------------------------------------------
# STEP 3 (Alternative): Call peaks from merged BAM files
#-----------------------------------------------

# Call peaks for GroupA using merged BAM
macs2 callpeak \
    -t ~/ATACseq_Footprinting/merged/bam/GroupA/GroupA_merged.bam \
    --nomodel \
    --shift -100 \
    --extsize 200 \
    -f BAM \
    -g hs \
    -n GroupA \
    --outdir ~/ATACseq_Footprinting/merged/peaks/GroupA

# Call peaks for GroupB using merged BAM
macs2 callpeak \
    -t ~/ATACseq_Footprinting/merged/bam/GroupB/GroupB_merged.bam \
    --nomodel \
    --shift -100 \
    --extsize 200 \
    -f BAM \
    -g hs \
    -n GroupB \
    --outdir ~/ATACseq_Footprinting/merged/peaks/GroupB
```

> 

**Parameter Explanation:**

- `--nomodel` skips the model building step since ATAC-seq has a different profile than ChIP-seq

- `--shift -100 --extsize 200` adjusts for Tn5 transposase binding characteristics

- `-g hs` specifies the human genome (use “mm” for mouse)

- These parameters are optimized for ATAC-seq data based on best practices

### Correcting Sequence Bias in ATAC-seq Data

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/atacorrect.png?resize=1024%2C515&ssl=1)

The Tn5 transposase used in ATAC-seq has inherent sequence preferences that can bias the cutting patterns. Before we can accurately identify footprints, we need to correct for this sequence bias to distinguish true protein protection from sequence-driven cutting preferences. Refer to my [previous ATAC-seq tutorial](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/) for genome index and black list preparation.

```bash
#-----------------------------------------------
# STEP 4: Correct sequence bias in ATAC-seq data
#-----------------------------------------------

# Create output directory for bias-corrected files
mkdir -p ~/ATACseq_Footprinting/atacorrect/GroupA \
         ~/ATACseq_Footprinting/atacorrect/GroupB

# Correct bias for GroupA
TOBIAS ATACorrect \
    --bam ~/ATACseq_Footprinting/merged/bam/GroupA/GroupA_merged.bam \
    --genome ~/Genome_Index/Bowtie2_hg38/genome.fa \
    --peaks ~/ATACseq_Footprinting/merged/peaks/GroupA/GroupA_merged.narrowPeak \
    --blacklist ~/Genome_Index/ENCODE_BlackList/hg38-blacklist.v2.bed \
    --outdir ~/ATACseq_Footprinting/atacorrect/GroupA \
    --cores 16

# Correct bias for GroupB
TOBIAS ATACorrect \
    --bam ~/ATACseq_Footprinting/merged/bam/GroupB/GroupB_merged.bam \
    --genome ~/Genome_Index/Bowtie2_hg38/genome.fa \
    --peaks ~/ATACseq_Footprinting/merged/peaks/GroupB/GroupB_merged.narrowPeak \
    --blacklist ~/Genome_Index/ENCODE_BlackList/hg38-blacklist.v2.bed \
    --outdir ~/ATACseq_Footprinting/atacorrect/GroupB \
    --cores 16
```

> 

**Understanding the Outputs:**

- `uncorrected.bw`: Raw cut site signal before bias correction (normalized for sequencing depth)

- `bias.bw`: The calculated sequence bias score

- `expected.bw`: The expected cut site signal based on sequence bias alone

- `corrected.bw`: The bias-corrected signal (uncorrected – expected), representing true accessibility

- `atacorrect.pdf`: Visualization of the bias correction process

The bias correction step is crucial for accurate footprinting. Without it, sequence preferences of Tn5 can be mistaken for true protein-DNA interactions, leading to false positive footprints.

### Calculating Footprinting Scores

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/footprinting.png?resize=1024%2C788&ssl=1)

Now that we have bias-corrected cut site data, we can calculate footprinting scores across our regions of interest. These scores quantify the likelihood of protein binding at each position based on the pattern of transposase cuts.

```bash
#-----------------------------------------------
# STEP 5: Calculate footprinting scores
#-----------------------------------------------

# Calculate scores for GroupA
TOBIAS FootprintScores \
    --signal ~/ATACseq_Footprinting/atacorrect/GroupA/GroupA_merged_corrected.bw \
    --regions ~/ATACseq_Footprinting/merged/peaks/GroupA/GroupA_merged.narrowPeak \
    --output ~/ATACseq_Footprinting/atacorrect/GroupA/GroupA_footprint.bw \
    --cores 16

# Calculate scores for GroupB
TOBIAS FootprintScores \
    --signal ~/ATACseq_Footprinting/atacorrect/GroupB/GroupB_merged_corrected.bw \
    --regions ~/ATACseq_Footprinting/merged/peaks/GroupB/GroupB_merged.narrowPeak \
    --output ~/ATACseq_Footprinting/atacorrect/GroupB/GroupB_footprint.bw \
    --cores 16
```

> 

**What’s Happening Here:**

- The FootprintScores tool is scanning each position in our open chromatin regions

- It analyzes the pattern of cuts around each position

- Positions protected from cutting (fewer cuts than expected) receive high footprinting scores

- These scores are stored in a bigWig (.bw) file that can be visualized in genome browsers

The footprinting scores represent the calculated protection at each position – higher scores indicate stronger protection, suggesting protein binding.

### Detecting and Comparing Specific Transcription Factor Binding Sites

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/bindetect.png?resize=1024%2C880&ssl=1)

With our footprinting scores calculated, we can now identify which specific transcription factors are likely bound at each footprint. This step combines our footprinting data with transcription factor motif information to predict transcription factor occupancy.

#### Combining Peaks Across Conditions for Comparative Analysis

First, we need to create a unified set of peaks that encompasses all regions of interest across our conditions:

```bash
#-----------------------------------------------
# STEP 6: Prepare unified peak set for comparative analysis
#-----------------------------------------------

# Create a directory for comparative analysis
mkdir -p ~/ATACseq_Footprinting/GroupA_GroupB

# Combine peaks from both conditions into a unified set
cat ~/ATACseq_Footprinting/merged/peaks/GroupA/GroupA_merged.narrowPeak \
    ~/ATACseq_Footprinting/merged/peaks/GroupB/GroupB_merged.narrowPeak | \
    sort -k1,1 -k2,2n | \
    bedtools merge > ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined.narrowPeak

# Download genome annotation file (human)
mkdir -p ~/genome_index/GTF
cd ~/genome_index/GTF
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_48/gencode.v48.annotation.gtf.gz
gunzip gencode.v48.annotation.gtf.gz

# Annotate combined peaks with nearby genes
uropa \
    --bed ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined.narrowPeak \
    --gtf ~/genome_index/GTF/gencode.v48.annotation.gtf \
    --show_attributes gene_id gene_name \
    --feature_anchor start \
    --distance 20000 10000 \
    --feature gene \
    -o ~/ATACseq_Footprinting/GroupA_GroupB

# Extract header for the annotated peaks
cut -f 1-6,16-17 ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits.txt | \
    head -n 1 > ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits_header.txt

# Match the header and the annotated peaks
cut -f 1-6,16-17 ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits.txt > \
~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits_2.bed

# Filter for standard chromosomes only (removes contigs and alternative assemblies)
grep -E '^chr[0-9XY]+\s' ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits_2.bed > \
    ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits_3.bed
```

> 

**Understanding UROPA Annotation:**

- UROPA (Universal RObust Peak Annotator) links peaks to nearby genes

- `--feature_anchor start` uses the transcription start site as the reference point

- `--distance 20000 10000` considers genes up to 20kb upstream and 10kb downstream

- This annotation helps us interpret the biological significance of differential footprints

#### Identifying Differential Transcription Factor Binding Between Conditions

Now we can perform the differential binding analysis using the TOBIAS BINDetect tool, which combines footprinting data with motif information to identify transcription factors with altered binding between conditions:

```bash
#-----------------------------------------------
# STEP 7: Detect differential TF binding
#-----------------------------------------------

# Identify differential TF binding sites between GroupA and GroupB
TOBIAS BINDetect \
    --motifs ~/ATACseq_Footprinting/JASPAR_Database/JASPAR2024_CORE_vertebrates_non-redundant_pfms_jaspar.txt \
    --signals ~/ATACseq_Footprinting/atacorrect/GroupA/GroupA_footprint.bw \
             ~/ATACseq_Footprinting/atacorrect/GroupB/GroupB_footprint.bw \
    --genome ~/Genome_Index/Bowtie2_hg38/genome.fa \
    --peaks ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits_3.bed \
    --peak_header ~/ATACseq_Footprinting/GroupA_GroupB/GroupA_GroupB_combined_finalhits_header.txt \
    --outdir ~/ATACseq_Footprinting/GroupA_GroupB \
    --cond_names GroupA GroupB \
    --cores 16
```

> 

**Parameter Explanation:**

- `--motifs` specifies the database of transcription factor binding motifs

- `--signals` provides the footprinting scores for both conditions

- `--genome` is needed to scan for motif occurrences in the DNA sequence

- `--cond_names` assigns human-readable names to the conditions being compared

**What BINDetect Does:**

1. Scans the genome for occurrences of each transcription factor motif

2. Calculates the footprint score at each motif site

3. Classifies sites as “bound” or “unbound” based on the footprint pattern

4. Compares binding patterns between conditions to identify differential binding

5. Generates statistical metrics and visualizations of the results

#### Understanding the BINDetect Output Files

The BINDetect command generates a comprehensive set of output files:

- **bindetect_results.txt/xlsx**: A table of differential binding results for all transcription factors, including:

- Binding scores for each condition

- Statistical significance of binding differences

- Number of bound sites in each condition

- Classification of transcription factors as condition-specific, shared, or non-differential

- **bindetect_figures.pdf**: Visual summary of the differential binding analysis, including:

- Volcano plot showing significance vs. effect size for all transcription factors

- Score distribution plots for selected factors

- Dendrograms showing clustering of transcription factors based on binding patterns

- **bindetect_GroupA_GroupB.html**: An interactive volcano plot for exploring the results

- **Individual TF Directories**: For each transcription factor, a separate directory contains:

- BED files of bound sites in each condition

- Footprint plots showing the average binding pattern

- Motif logos and position weight matrices

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-08-at-5.31.19%E2%80%AFPM.png?resize=1024%2C1014&ssl=1)

### Visualizing Footprints for Individual Transcription Factors

The comprehensive analysis provides a global view, but we often want to examine specific transcription factors of interest in more detail. TOBIAS provides tools for visualizing footprints for individual factors:

```bash
#-----------------------------------------------
# STEP 8: Visualize individual TF footprints
#-----------------------------------------------

# Plot aggregate footprints for MYOG (Myogenin) across conditions
TOBIAS PlotAggregate \
    --TFBS ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/beds/MYOG_MA0500.3_GroupA_bound.bed \
           ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/beds/MYOG_MA0500.3_GroupB_bound.bed \
    --signals ~/ATACseq_Footprinting/atacorrect/GroupA/GroupA_merged_corrected.bw \
              ~/ATACseq_Footprinting/atacorrect/GroupB/GroupB_merged_corrected.bw \
    --output ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/plots/MYOG_footprint_GroupA-GroupB.png \
    --share_y both \
    --plot_boundaries \
    --signal_labels GroupA GroupB \
    --smooth 5
```

This command generates a 3×3 plot matrix:

- The top row shows sites bound in GroupA

- The middle row shows sites bound in GroupB

- The bottom row shows sites bound in both conditions

- The columns display the signal in GroupA, GroupB, and a direct comparison

> 

**Interpreting Footprint Plots:**

- A clear “dip” in the center of the plot indicates transcription factor binding

- The width of the dip corresponds to the size of the protein footprint

- The depth of the dip reflects the strength of binding

- Differences in the footprint pattern between conditions suggest changes in binding affinity or occupancy

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/MYOG_footprint_GroupA-GroupB.png?resize=1024%2C1024&ssl=1)

### Creating Heatmaps of Transcription Factor Binding

For a more comprehensive view of binding patterns across all sites, we can generate heatmaps:

```bash
#-----------------------------------------------
# STEP 9: Generate binding heatmaps
#-----------------------------------------------

# Create heatmaps comparing bound and unbound sites between conditions
TOBIAS PlotHeatmap \
    --TFBS ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/beds/MYOG_MA0500.3_GroupA_bound.bed \
           ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/beds/MYOG_MA0500.3_GroupA_unbound.bed \
    --TFBS ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/beds/MYOG_MA0500.3_GroupB_bound.bed \
           ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/beds/MYOG_MA0500.3_GroupB_unbound.bed \
    --signals ~/ATACseq_Footprinting/atacorrect/GroupA/GroupA_merged_corrected.bw \
              ~/ATACseq_Footprinting/atacorrect/GroupB/GroupB_merged_corrected.bw \
    --output ~/ATACseq_Footprinting/GroupA_GroupB/MYOG_MA0500.3/plots/MYOG_footprint_GroupA-GroupB_heatmap.png \
    --signal_labels GroupA GroupB \
    --share_colorbar \
    --sort_by -1
```

> 

**What’s Happening Here:**

- We’re visualizing the footprinting pattern across all binding sites

- Each row in the heatmap represents one site

- The color intensity indicates the cut site frequency

- Sites are sorted by binding strength (–sort_by -1)

- The heatmap allows us to see the heterogeneity of binding patterns across all sites

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/MYOG_footprint_GroupA-GroupB_heatmap.png?resize=1024%2C395&ssl=1)

## Interpreting Footprinting Results

The footprinting analysis provides a wealth of information about transcription factor binding dynamics. Here’s how to interpret the key outputs:

### Understanding Differential Binding Results

The `bindetect_results.txt` file contains the statistical summary of differential binding for each transcription factor. Key columns to focus on include:

- **name:** The name of each motif

- **GroupA_GroupB_change**: The difference in binding score between conditions

- **GroupA_GroupB_pvalue**: The statistical significance of the differential binding

Transcription factors with the highest absolute binding_differential and significance values represent the most dramatically altered regulatory factors between your conditions. Check the [TOBIAS documentation](https://github.com/loosolab/TOBIAS/wiki/BINDetect) for more column details.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-08-at-5.36.05%E2%80%AFPM.png?resize=1024%2C111&ssl=1)

### Connecting Footprints to Biological Function

To derive biological meaning from footprinting results:

1. **Focus on the most significant differential TFs first** – These represent the largest regulatory changes between conditions

2. **Look for known master regulators of your biological system** – Changes in key lineage-specific factors often drive phenotypic differences

3. **Examine the genes near differential binding sites** – Use the peak annotations to identify potentially regulated genes

4. **Perform pathway analysis on genes near differential footprints** – Tools like DAVID, GSEA, or Enrichr can identify enriched biological processes

5. **Integrate with gene expression data** – Correlate changes in TF binding with changes in gene expression to identify direct regulatory targets

6. **Check for co-occurring motifs** – Transcription factors often work in complexes; look for co-occurring footprints

## Best Practices for ATAC-seq Footprinting Analysis

To ensure high-quality footprinting results, keep these best practices in mind:

### Experimental Design Considerations

- **Sufficient Sequencing Depth**: Footprinting requires deeper sequencing than standard ATAC-seq. Aim for at least 50-100 million reads per condition after merging replicates.

- **Biological Replicates**: Include at least 3 biological replicates per condition to ensure reproducible footprints.

- **Cell Type Purity**: Footprinting analysis is highly sensitive to cellular heterogeneity. Use purified cell populations whenever possible.

- **Timing Considerations**: Transcription factor binding can be dynamic. Consider time-course experiments for developmental or stimulus-response studies.

### Data Processing Best Practices

- **Quality Control**: Start with high-quality ATAC-seq data. Low-quality data with excessive PCR duplicates or poor fragment size distributions will compromise footprinting accuracy.

- **Bias Correction**: Always perform bias correction to account for Tn5 sequence preferences.

- **Peak Selection**: Focus on high-confidence peaks with strong signal-to-noise ratio.

- **Blacklist Filtering**: Always remove ENCODE blacklisted regions to avoid artifacts.

- **Motif Database Selection**: Choose appropriate motif databases for your biological system (vertebrate, invertebrate, plant, etc.).

- **Multiple Testing Correction**: Apply appropriate multiple testing correction when identifying significant differential binding.

### Common Pitfalls to Avoid

- **Ignoring Sequence Bias**: Failing to correct for Tn5 sequence preferences will lead to false positive footprints.

- **Overly Stringent Thresholds**: Starting with overly strict thresholds may miss important but subtle regulatory changes. Consider a tiered approach, starting with relaxed thresholds.

- **Focusing Only on Known Factors**: Don’t ignore uncharacterized motifs; they may represent important novel regulators in your system.

- **Neglecting Factor Cooperativity**: Remember that transcription factors often function in complexes. Look beyond individual factors to identify cooperative interactions.

- **Confusing Statistical with Biological Significance**: The most statistically significant changes are not always the most biologically relevant. Always interpret results in the context of your specific biological question.

## Troubleshooting Common Issues

### No or Few Detected Footprints

**Problem**: Your analysis identifies very few footprints or differential transcription factors.

**Solutions**:

- **Check Sequencing Depth**: Footprinting requires deep sequencing. If your dataset has insufficient depth, consider merging more replicates or performing additional sequencing.

- **Examine ATAC-seq Quality**: Look at the fragment size distribution. Proper ATAC-seq data should have a clear nucleosome-free peak (~100bp) and nucleosome peaks (multiples of ~200bp).

- **Adjust Peak Calling**: If you’re using too stringent peak calling thresholds, you might miss regions with actual footprints. Try more lenient peak calling parameters.

- **Check Bias Correction**: Ensure the bias correction step worked properly. The ATACorrect output PDF should show a clear difference between the pre- and post-correction patterns.

- **Try Different Motif Databases**: The default JASPAR database might not include all relevant factors for your biological system. Consider using additional motif collections.

### Inconsistent Results Between Replicates

**Problem**: Different replicates show poor agreement in footprinting patterns.

**Solutions**:

- **Check Replicate Quality**: Assess the correlation between replicates at the peak level. Poor correlation suggests technical or biological variation that needs to be addressed.

- **Consider Batch Effects**: If replicates were processed in different batches, normalize for batch effects before footprinting analysis.

- **Improve Cell Purification**: Heterogeneous cell populations can lead to variable footprinting results. Consider improving cell purification protocols.

- **Use IDR Framework**: Apply the Irreproducible Discovery Rate framework to identify consistent footprints across replicates.

### Computational Resource Limitations

**Problem**: TOBIAS analyses are consuming excessive computational resources or crashing.

**Solutions**:

- **Limit Analysis to Specific Chromosomes**: For testing purposes, you can run the analysis on a single chromosome first.

- **Reduce Motif Set**: Instead of using the entire JASPAR database, focus on a subset of motifs relevant to your biological question.

- **Optimize Thread Usage**: Adjust the `--cores` parameter based on your available computational resources.

- **Use High-Performance Computing**: For large genomes or many conditions, consider running the analysis on a high-performance computing cluster.

- **Pre-filter Peaks**: Focus the analysis on the most significant peaks to reduce computational load.

### Difficulty Interpreting Biological Significance

**Problem**: You’ve identified differential footprints but struggle to connect them to biological function.

**Solutions**:

- **Integrate with Expression Data**: Correlate footprinting changes with gene expression changes.

- **Focus on Known Master Regulators**: Begin by examining factors with established roles in your biological system.

- **Look for Motif Co-occurrence**: Identify groups of transcription factors that change together, suggesting coordinated regulation.

- **Pathway Analysis**: Perform pathway enrichment analysis on genes near differential footprints.

- **Literature Mining**: Use tools like Enrichr to connect your differential factors to published literature and known biological processes.

## Advanced ATAC-seq Footprinting Applications

Once you’ve mastered basic footprinting analysis, consider these advanced applications to extract even deeper insights from your data:

### Integrating Footprinting with Single-cell ATAC-seq

Single-cell ATAC-seq provides unprecedented resolution of cellular heterogeneity but presents challenges for footprinting due to sparse data. Recent computational approaches enable footprinting at the single-cell level:

- **Pseudo-bulk Approaches**: Aggregate cells from similar clusters to increase depth for footprinting

- **Imputation Methods**: Computational approaches to predict missing values in sparse scATAC-seq data

- **Cell Type-Specific Footprinting**: Identify lineage-specific transcription factor binding patterns

- **Trajectory Analysis**: Map changes in transcription factor binding along developmental trajectories

### Multimodal Integration for Comprehensive Regulatory Analysis

Combining footprinting with other data types provides a more complete picture of gene regulation:

- **ATAC-seq + RNA-seq**: Connect transcription factor binding changes to transcriptional outcomes

- **ATAC-seq + ChIP-seq**: Validate predicted binding of key transcription factors

- **ATAC-seq + HiChIP/Hi-C**: Link distal regulatory elements to target genes through 3D genome architecture

- **ATAC-seq + DNA Methylation**: Understand how epigenetic modifications impact transcription factor binding

### Using Footprinting to Study Non-coding Variants

Footprinting is particularly valuable for understanding the functional impact of non-coding genetic variants:

- **Allele-Specific Footprinting**: Detect differential binding between alleles to identify functional SNPs

- **eQTL Integration**: Connect variants affecting transcription factor binding to changes in gene expression

- **Disease-Associated Variant Analysis**: Prioritize causal variants from GWAS by identifying those that disrupt key footprints

- **Personalized Regulatory Landscapes**: Map individual-specific transcription factor binding patterns based on genotype

## Conclusion

ATAC-seq footprinting analysis represents a powerful approach for comprehensively mapping transcription factor binding dynamics across the genome. By moving beyond simple chromatin accessibility to identify precise transcription factor binding sites, footprinting provides much deeper insights into the regulatory mechanisms controlling gene expression in different biological contexts.

In this tutorial, we’ve covered the complete workflow for ATAC-seq footprinting analysis using TOBIAS, from data preparation through bias correction, footprint detection, and differential binding analysis. We’ve also discussed best practices, troubleshooting strategies, and advanced applications to help you get the most from your data.

By mastering these techniques, you’ll be equipped to extract maximum value from your ATAC-seq experiments, identifying the key transcription factors driving your biological system and generating testable hypotheses about their regulatory roles.

## References

- Bentsen, M., Goymann, P., Schultheis, H. et al. (2020). ATAC-seq footprinting unravels kinetics of transcription factor binding during zygotic genome activation. Nature Communications, 11, 4267. https://doi.org/10.1038/s41467-020-18035-1

- Li, Z., Schulz, M.H., Look, T. et al. (2019). Identification of transcription factor binding sites using ATAC-seq. Genome Biology, 20, 45. https://doi.org/10.1186/s13059-019-1642-2

- Yan, F., Powell, D.R., Curtis, D.J. et al. (2020). From reads to insight: a hitchhiker’s guide to ATAC-seq data analysis. Genome Biology, 21, 22. https://doi.org/10.1186/s13059-020-1929-3

- [TOBIAS documentation](https://github.com/loosolab/TOBIAS/wiki) – Comprehensive guide to all TOBIAS functions and parameters

- [JASPAR database](https://jaspar.elixir.no/) – Collection of transcription factor binding profiles

---

*This tutorial is part of the NGS101.com beginner’s guide to next-generation sequencing analysis. If you have questions or suggestions, please leave a comment below or contact us through the website.*
