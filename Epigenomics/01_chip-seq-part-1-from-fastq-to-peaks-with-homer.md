# How To Analyze ChIP-seq Data For Absolute Beginners Part 1: From FASTQ To Peaks With HOMER

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/)

- [Introduction: Understanding ChIP-seq](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Introduction_Understanding_ChIP-seq)

  - [What is ChIP-seq?](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#What_is_ChIP-seq)
  - [Why ChIP-seq Matters in Biological Research](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Why_ChIP-seq_Matters_in_Biological_Research)
  - [Sequencing Depth and Strategy for ChIP-seq](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Sequencing_Depth_and_Strategy_for_ChIP-seq)
  - [The ChIP-seq Data Analysis Journey](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#The_ChIP-seq_Data_Analysis_Journey)
  - [Why HOMER is Ideal for Beginners](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Why_HOMER_is_Ideal_for_Beginners)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Setting_Up_Your_Analysis_Environment)

  - [Required Software Installation](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Required_Software_Installation)
  - [Reference File Preparation](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Reference_File_Preparation)
  - [Download Example Data](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Download_Example_Data)

- [The ChIP-seq Analysis Pipeline](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#The_ChIP-seq_Analysis_Pipeline)

  - [Step 1: Trim Adapters and Quality Control](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Step_1_Trim_Adapters_and_Quality_Control)
  - [Step 2: Align Reads to the Reference Genome](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Step_2_Align_Reads_to_the_Reference_Genome)
  - [Step 3: Process and Quality Control Alignments](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Step_3_Process_and_Quality_Control_Alignments)
  - [Step 4: Peak Calling and Annotation](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Step_4_Peak_Calling_and_Annotation)

- [Conclusion](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Conclusion)
- [Best Practices for ChIP-seq Analysis](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Best_Practices_for_ChIP-seq_Analysis)

  - [Quality Control at Every Step](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Quality_Control_at_Every_Step)
  - [Handling Controls Properly](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Handling_Controls_Properly)
  - [Data Interpretation](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Data_Interpretation)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Common_Pitfalls_to_Avoid)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Troubleshooting_Common_Issues)

  - [Low Peak Counts](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Low_Peak_Counts)
  - [High Background Signal](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#High_Background_Signal)
  - [Inconsistent Replicates](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Inconsistent_Replicates)

- [Further Resources](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Further_Resources)
- [References](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/#Like_this)

## Introduction: Understanding ChIP-seq

At the heart of molecular biology lies a fundamental question: how do cells regulate which genes are expressed and when? One of the most powerful techniques to explore this question is Chromatin Immunoprecipitation followed by sequencing, commonly known as ChIP-seq. This tutorial introduces beginners to the fascinating world of ChIP-seq data analysis using HOMER, a versatile software suite that has become a mainstay in genomic research.

### What is ChIP-seq?

ChIP-seq represents a remarkable marriage of biochemistry and next-generation sequencing technology. The technique allows researchers to capture a snapshot of where specific proteins interact with DNA across the entire genome. These protein-DNA interactions are fundamental to understanding gene regulation, as they reveal how transcription factors find their target genes, how chromatin is modified, and how the genome is organized within the nucleus.

The experimental process begins within living cells, where proteins are chemically cross-linked to their DNA binding sites. The cells are then broken open, and the DNA is fragmented into small pieces. Using antibodies that specifically recognize the protein of interest, scientists can selectively enrich for DNA fragments bound by this protein. These enriched DNA pieces are then sequenced and mapped to a reference genome, creating a genome-wide map of where the protein binds.

> 

**Beginner’s Tip:** Think of ChIP-seq as taking a photograph of a protein “in action” as it interacts with DNA inside the cell. Just as wildlife photographers track animals to understand their behavior, scientists use ChIP-seq to track proteins on DNA to understand genome regulation.

### Why ChIP-seq Matters in Biological Research

The applications of ChIP-seq extend far beyond academic curiosity:

- **Gene Regulation:** By identifying where transcription factors bind, researchers can uncover the regulatory networks controlling gene expression during development, in different tissues, or in response to environmental stimuli.

- **Epigenetics:** ChIP-seq for histone modifications reveals the epigenetic landscape that influences gene accessibility.

- **Disease Research:** ChIP-seq helps identify how mutations in regulatory regions might disrupt normal protein binding, potentially leading to dysregulated gene expression.

For example, cancer researchers use ChIP-seq to understand how oncogenic transcription factors rewire gene regulation networks. Developmental biologists use it to track how master regulators orchestrate tissue formation. Immunologists apply ChIP-seq to reveal how immune cells respond to pathogens through changes in gene regulation. The insights gained from these studies not only advance our fundamental understanding of biology but also inform the development of new therapeutic strategies.

### Sequencing Depth and Strategy for ChIP-seq

Before beginning a ChIP-seq experiment, it’s important to understand sequencing requirements:

For most standard transcription factor ChIP-seq experiments, single-end sequencing at sufficient depth (20-30 million reads) is adequate. However, for histone modification ChIP-seq, especially when studying broader domains or when working with complex genomes with many repetitive elements, paired-end sequencing might provide meaningful benefits.

Recommended sequencing depths:

- **Transcription Factors:** 20-30 million reads per sample

- **Histone Modifications:** 40-60 million reads, particularly for broad marks (H3K27me3, H3K36me3)

- **Low Enrichment Factors:** Some proteins with weaker binding may require deeper sequencing

While this tutorial focuses on analysis of data that has already been generated, understanding these experimental design considerations is important when interpreting your results or planning future experiments.

### The ChIP-seq Data Analysis Journey

For newcomers to computational biology, the prospect of analyzing ChIP-seq data might seem daunting. However, the process can be broken down into a comprehensible sequence of steps:

1. **Quality Assessment:** Ensuring the sequencing reads are reliable before proceeding

2. **Alignment:** Mapping millions of short DNA sequences to their correct locations in the reference genome

3. **Peak Calling:** Identifying regions of significant enrichment that represent likely binding sites

4. **Annotation:** Connecting peaks to nearby genomic features such as genes, enhancers, or promoters

Throughout this analytical journey, computational tools serve as the researcher’s compass and map, guiding the way from raw data to biological meaning.

### Why HOMER is Ideal for Beginners

Among the various software options available for ChIP-seq analysis, HOMER (Hypergeometric Optimization of Motif EnRichment) stands out as particularly suitable for beginners for several reasons:

- **Balanced Accessibility:** HOMER offers sophisticated analytical capabilities while maintaining a relatively straightforward command-line interface with consistent syntax patterns

- **Educational Documentation:** Its comprehensive documentation includes not just technical details but also explanations of the underlying biological concepts

- **Strong Annotation Features:** HOMER excels in connecting binding sites to potential gene targets and discovering both known and novel DNA binding motifs

- **Integrated Workflow:** The software makes it straightforward to compare binding patterns across different conditions, correlate binding with gene expression changes, and visualize results in genome browsers

These capabilities allow researchers to move beyond simple binding site identification toward more sophisticated questions about the functional consequences of protein-DNA interactions.

## Setting Up Your Analysis Environment

Before diving into the analysis, we need to set up our computational environment with all the necessary tools and reference files.

### Required Software Installation

Let’s begin by creating a Conda environment for ChIP-seq analysis on a Linux system:

```bash
#-----------------------------------------------
# STEP 0: Setup conda environment for ChIP-seq analysis
#-----------------------------------------------

# Create a dedicated conda environment with Python 3.10
conda create -p ~/Env_Homer python=3.10

# Activate the newly created environment
conda activate ~/Env_Homer

# Configure conda channels in order of priority
# (packages will be searched in this order)
conda config --add channels defaults       # Standard packages
conda config --add channels bioconda       # Bioinformatics packages
conda config --add channels conda-forge    # Community-maintained packages
conda config --set channel_priority strict # Prevent package conflicts

# Install all required tools for ChIP-seq analysis
conda install -y \
    wget \                          # For downloading files
    samtools \                      # For manipulating SAM/BAM files
    ucsc-bedgraphtobigwig \         # For creating browser tracks
    ucsc-fetchchromsizes \          # For chromosome information
    ucsc-bedtobigbed \              # For bed file conversion
    r-essentials \                  # R for statistical analysis
    bioconductor-deseq2 \           # For differential analysis
    bioconductor-edger \            # Another differential analysis tool
    sra-tools \                     # For downloading SRA data
    trim-galore \                   # For adapter trimming
    bedtools \                      # For genomic interval manipulation
    picard \                        # For BAM file processing
    bwa\                            # For read alignment
    deeptools                       # For results visualization

# Create a directory for HOMER installation
mkdir -p ~/homer
cd ~/homer

# Download the HOMER installation script
wget http://homer.ucsd.edu/homer/configureHomer.pl

# Install HOMER with default settings
perl configureHomer.pl -install

# Add HOMER executables to the PATH in this conda environment
conda env config vars set PATH="~/homer/bin:$PATH"

# Display all available reference genomes in HOMER
perl configureHomer.pl -list

# Install human genome (hg38) for our analysis
perl configureHomer.pl -install hg38

# Restart the conda environment to apply the PATH changes
conda deactivate
conda activate ~/Env_Homer
```

> 

**Troubleshooting Tip:** If you encounter errors during installation, make sure you have sufficient disk space and permissions to install software. For permission errors, you might need to use `sudo` or contact your system administrator.

### Reference File Preparation

We will use BWA for reads alignment in this tutorial. Let’s download the pre-built BWA index for human genome from [refgenie](http://refgenomes.databio.org/v3/assets/splash/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/bwa_index?tag=default):

```bash
#-----------------------------------------------
# STEP 1: Download reference genome index files
#-----------------------------------------------

# Create a dedicated directory for BWA Index files
mkdir -p ~/BWA_Index_hg38
cd ~/BWA_Index_hg38

# Define the base URL for all index files
BASE_URL="http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/bwa_index__default"
FILE_PREFIX="2230c535660fb4774114bfa966a62f823fdb6d21acf138d4"

# Download main reference sequence file
echo "Downloading main reference sequence file..."
wget ${BASE_URL}/${FILE_PREFIX}.fa

# Download BWA index component files
# .amb - stores information about ambiguous regions
echo "Downloading .amb index file..."
wget ${BASE_URL}/${FILE_PREFIX}.fa.amb

# .ann - stores information about where sequences are stored in the file
echo "Downloading .ann index file..."
wget ${BASE_URL}/${FILE_PREFIX}.fa.ann

# .bwt - contains the Burrows-Wheeler transformed string
echo "Downloading .bwt index file..."
wget ${BASE_URL}/${FILE_PREFIX}.fa.bwt

# .pac - contains the packed sequence
echo "Downloading .pac index file..."
wget ${BASE_URL}/${FILE_PREFIX}.fa.pac

# .sa - contains the suffix array
echo "Downloading .sa index file..."
wget ${BASE_URL}/${FILE_PREFIX}.fa.sa

echo "All reference genome index files downloaded successfully!"
```

> 

**Best Practice:** Store reference files in a centralized location on your system to avoid duplicating large files for different projects.

### Download Example Data

For this tutorial, we’ll use the GEO dataset [GSE104247](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE104247) (ChIP-Seq analysis of 208 Factors in HepG2). We’ll download the FASTQ file for the transcription factor USF2 and its control sample (Input):

```bash
#-----------------------------------------------
# STEP 2: Download example ChIP-seq dataset
#-----------------------------------------------

# Create a directory structure for the project
# ~/GSE104247/ is the main project directory
# ~/GSE104247/raw/ will store the raw sequencing files
mkdir -p ~/GSE104247/raw
cd ~/GSE104247/raw

# Download example ChIP-seq data from GEO dataset GSE104247
# (ChIP-Seq analysis of 208 Factors in HepG2 cells)

# Download CONTROL sample (Input DNA) - SRR6117732
echo "Downloading control (Input) sample..."
# First, fetch the SRA file
prefetch SRR6117732
# Then, convert SRA to FASTQ format
fasterq-dump --split-files SRR6117732/SRR6117732.sra
# Note: --split-files will separate paired-end reads if present
# This dataset is single-end so it will create just one file

# Download USF2 ChIP sample - SRR6117703
echo "Downloading USF2 ChIP sample..."
prefetch SRR6117703
fasterq-dump --split-files SRR6117703/SRR6117703.sra

# Compress the FASTQ files to save disk space
# gzip compression is standard for NGS data
echo "Compressing FASTQ files..."
gzip *.fastq

# Remove the temporary SRA directories to save space
echo "Cleaning up SRA directories..."
rm -r SRR6117703 SRR6117732

# Rename the FASTQ files to more descriptive names
# This helps keep track of sample identity
echo "Renaming files with descriptive names..."
mv SRR6117703.fastq.gz SRR6117703_USF2.fastq.gz
mv SRR6117732.fastq.gz SRR6117732_USF2_Input.fastq.gz

echo "Dataset download complete!"
```

> 

**What’s Happening Here:** We’re downloading experimental data from a public repository. The USF2 sample contains DNA fragments where the USF2 transcription factor was bound, while the Input sample serves as a control representing background DNA without specific enrichment.

## The ChIP-seq Analysis Pipeline

Now that we have our environment set up, let’s walk through the analysis pipeline step by step.

### Step 1: Trim Adapters and Quality Control

Sequencing reads often contain adapter sequences that must be removed before analysis. We’ll also perform quality control to ensure our data meets high standards:

```bash
#-----------------------------------------------
# STEP 3: Quality control and adapter trimming
#-----------------------------------------------

# Create a directory for trimmed FASTQ files
mkdir -p ~/GSE104247/trim

# Trim adapters and low-quality bases from the USF2 ChIP sample
echo "Processing USF2 ChIP sample..."
trim_galore \
    --fastqc \                # Run FastQC after trimming for QC report
    --cores 8 \               # Use 8 CPU cores for faster processing
    --quality 20 \            # Trim low-quality bases (Q<20)
    --stringency 3 \          # Require at least 3bp overlap with adapter
    --length 20 \             # Discard reads shorter than 20bp after trimming
    --output_dir ~/GSE104247/trim \
    ~/GSE104247/raw/SRR6117703_USF2.fastq.gz

# Trim adapters and low-quality bases from the Input control sample
echo "Processing Input control sample..."
trim_galore \
    --fastqc \                # Run FastQC after trimming for QC report
    --cores 8 \               # Use 8 CPU cores for faster processing
    --quality 20 \            # Trim low-quality bases (Q<20)
    --stringency 3 \          # Require at least 3bp overlap with adapter
    --length 20 \             # Discard reads shorter than 20bp after trimming
    --output_dir ~/GSE104247/trim \
    ~/GSE104247/raw/SRR6117732_USF2_Input.fastq.gz

echo "Quality control and adapter trimming complete!"
# Trimmed files will be named with '_trimmed.fq.gz' suffix
```

> 

**What This Does:**

- `--fastqc` generates quality reports for our data

- `--cores 8` uses 8 CPU cores to speed up processing

- The output will be trimmed FASTQ files with adapter sequences removed

### Step 2: Align Reads to the Reference Genome

Next, we need to map our sequencing reads to the human genome to determine where they originated:

```bash
#-----------------------------------------------
# STEP 4: Align reads to the reference genome
#-----------------------------------------------

# Create directory for alignment files
mkdir -p ~/GSE104247/bam

# Set variables for readability and reusability
REFERENCE="~/BWA_Index_hg38/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4.fa"
THREADS=16  # Adjust based on your system's capabilities

# 1. Align USF2 ChIP sample to the reference genome
echo "Aligning USF2 ChIP sample to reference genome..."
bwa mem \
    -t ${THREADS} \             # Use multiple threads for faster alignment
    -M \                        # Mark shorter split hits as secondary (Picard compatibility)
    -R "@RG\tID:USF2\tSM:USF2\tPL:ILLUMINA" \  # Add read group information
    ${REFERENCE} \              # Reference genome
    ~/GSE104247/trim/SRR6117703_USF2_trimmed.fq.gz \  # Input trimmed reads
    > ~/GSE104247/bam/SRR6117703_USF2.sam      # Output alignment file

# 2. Align USF2 Input control sample to the reference genome
echo "Aligning Input control sample to reference genome..."
bwa mem \
    -t ${THREADS} \             # Use multiple threads for faster alignment
    -M \                        # Mark shorter split hits as secondary (Picard compatibility)
    -R "@RG\tID:INPUT\tSM:INPUT\tPL:ILLUMINA" \  # Add read group information
    ${REFERENCE} \              # Reference genome
    ~/GSE104247/trim/SRR6117732_USF2_Input_trimmed.fq.gz \  # Input trimmed reads
    > ~/GSE104247/bam/SRR6117732_USF2_Input.sam  # Output alignment file

echo "Alignment complete! SAM files created."

# BWA 'mem' algorithm explanation:
# - Suited for 70bp-1Mbp Illumina reads
# - Fast and accurate, handles chimeric reads
# - Output is in SAM format (Sequence Alignment/Map)
```

> 

**Common Pitfall:** Ensure you’re using the correct reference genome version that matches your experimental design. Using the wrong genome version can lead to misalignment and inaccurate results.

### Step 3: Process and Quality Control Alignments

After alignment, we need to convert, sort, and filter our alignment files to prepare them for peak calling:

```bash
#-----------------------------------------------
# STEP 5: Process and filter alignment files
#-----------------------------------------------

# Set variables for readability
THREADS=16                  # Number of threads to use
USF2_PREFIX="SRR6117703_USF2"
INPUT_PREFIX="SRR6117732_USF2_Input"
BAM_DIR="~/GSE104247/bam"

#=============================================
# 5.1: SAM to BAM conversion
#=============================================
echo "Converting SAM to BAM format (compressed binary format)..."

# Convert USF2 ChIP sample
samtools view \
    -h \                   # Include header in output
    -S \                   # Input is SAM format
    -b \                   # Output BAM format
    -@ ${THREADS} \        # Use multiple threads
    -o ${BAM_DIR}/${USF2_PREFIX}.bam \  # Output file
    ${BAM_DIR}/${USF2_PREFIX}.sam       # Input file

# Convert Input control sample
samtools view \
    -h -S -b -@ ${THREADS} \
    -o ${BAM_DIR}/${INPUT_PREFIX}.bam \
    ${BAM_DIR}/${INPUT_PREFIX}.sam

# Remove large SAM files to save space
rm ${BAM_DIR}/${USF2_PREFIX}.sam ${BAM_DIR}/${INPUT_PREFIX}.sam

#=============================================
# 5.2: Sort BAM files by genomic coordinates
#=============================================
echo "Sorting BAM files by chromosome position..."

# Sort USF2 ChIP sample
samtools sort \
    -@ ${THREADS} \        # Use multiple threads
    -m 2G \                # Use 2GB memory per thread
    -o ${BAM_DIR}/${USF2_PREFIX}_sorted.bam \  # Output sorted BAM
    ${BAM_DIR}/${USF2_PREFIX}.bam              # Input BAM

# Sort Input control sample
samtools sort \
    -@ ${THREADS} \
    -m 2G \
    -o ${BAM_DIR}/${INPUT_PREFIX}_sorted.bam \
    ${BAM_DIR}/${INPUT_PREFIX}.bam

# Remove unsorted BAM files to save space
rm ${BAM_DIR}/${USF2_PREFIX}.bam ${BAM_DIR}/${INPUT_PREFIX}.bam

#=============================================
# 5.3: Index BAM files for random access
#=============================================
echo "Indexing BAM files for faster access..."

# Index USF2 ChIP sample
samtools index \
    -@ ${THREADS} \
    ${BAM_DIR}/${USF2_PREFIX}_sorted.bam

# Index Input control sample
samtools index \
    -@ ${THREADS} \
    ${BAM_DIR}/${INPUT_PREFIX}_sorted.bam

#=============================================
# 5.4: Mark and remove PCR duplicates
#=============================================
echo "Marking and removing PCR duplicates..."

# Process USF2 ChIP sample
picard MarkDuplicates \
    I=${BAM_DIR}/${USF2_PREFIX}_sorted.bam \            # Input BAM
    O=${BAM_DIR}/${USF2_PREFIX}_sorted_dedup.bam \      # Output BAM
    M=${BAM_DIR}/${USF2_PREFIX}_sorted_dedup_metrics.txt \  # Metrics file
    REMOVE_DUPLICATES=true \                            # Remove duplicates
    VALIDATION_STRINGENCY=LENIENT                       # Less strict validation

# Process Input control sample
picard MarkDuplicates \
    I=${BAM_DIR}/${INPUT_PREFIX}_sorted.bam \
    O=${BAM_DIR}/${INPUT_PREFIX}_sorted_dedup.bam \
    M=${BAM_DIR}/${INPUT_PREFIX}_sorted_dedup_metrics.txt \
    REMOVE_DUPLICATES=true \
    VALIDATION_STRINGENCY=LENIENT

# Index the deduplicated BAM files
samtools index ${BAM_DIR}/${USF2_PREFIX}_sorted_dedup.bam
samtools index ${BAM_DIR}/${INPUT_PREFIX}_sorted_dedup.bam

#=============================================
# 5.5: Filter out blacklisted regions
#=============================================
echo "Downloading and preparing blacklist regions..."

# Download the ENCODE blacklist regions
mkdir -p ~/references/blacklists
cd ~/references/blacklists
wget https://github.com/Boyle-Lab/Blacklist/raw/master/lists/hg38-blacklist.v2.bed.gz
gunzip hg38-blacklist.v2.bed.gz

echo "Removing blacklisted regions from alignments..."

# Filter USF2 ChIP sample
bedtools intersect \
    -v \                   # Only keep reads that DO NOT overlap blacklist
    -abam ${BAM_DIR}/${USF2_PREFIX}_sorted_dedup.bam \  # Input BAM
    -b ~/references/blacklists/hg38-blacklist.v2.bed \  # Blacklist BED
    > ${BAM_DIR}/${USF2_PREFIX}_sorted_dedup_filtered.bam  # Output filtered BAM

# Filter Input control sample
bedtools intersect \
    -v \
    -abam ${BAM_DIR}/${INPUT_PREFIX}_sorted_dedup.bam \
    -b ~/references/blacklists/hg38-blacklist.v2.bed \
    > ${BAM_DIR}/${INPUT_PREFIX}_sorted_dedup_filtered.bam

# Index the filtered BAM files
samtools index ${BAM_DIR}/${USF2_PREFIX}_sorted_dedup_filtered.bam
samtools index ${BAM_DIR}/${INPUT_PREFIX}_sorted_dedup_filtered.bam

echo "Alignment processing and filtering complete!"
```

> 

**What’s Happening Here:**

1. We convert SAM files to the more efficient BAM format

2. We sort reads by genomic position for faster processing

3. We mark and remove PCR duplicates that could bias our analysis

4. We filter out genomic regions known to give false positive signals (blacklisted regions)

### Step 4: Peak Calling and Annotation

Now comes the most exciting part – identifying where our protein of interest (USF2) binds to DNA:

```bash
#-----------------------------------------------
# STEP 6: Peak calling and annotation with HOMER
#-----------------------------------------------

# Create directory for HOMER analysis
mkdir -p ~/GSE104247/homer

#=============================================
# 6.1: Create HOMER tag directories
#=============================================
echo "Creating HOMER tag directories..."

# Variables for readability
USF2_PREFIX="SRR6117703_USF2"
INPUT_PREFIX="SRR6117732_USF2_Input"
BAM_DIR="~/GSE104247/bam"
HOMER_DIR="~/GSE104247/homer"

# Create tag directory for USF2 ChIP sample
# Tag directories contain processed alignment data optimized for HOMER analysis
echo "Creating tag directory for USF2 ChIP sample..."
makeTagDirectory \
    ${HOMER_DIR}/${USF2_PREFIX} \          # Output directory
    ${BAM_DIR}/${USF2_PREFIX}_sorted_dedup_filtered.bam \  # Input BAM
    -genome hg38                           # Reference genome

# Create tag directory for Input control sample
echo "Creating tag directory for Input control sample..."
makeTagDirectory \
    ${HOMER_DIR}/${INPUT_PREFIX} \
    ${BAM_DIR}/${INPUT_PREFIX}_sorted_dedup_filtered.bam \
    -genome hg38 

#=============================================
# 6.2: Call peaks to identify binding sites
#=============================================
echo "Identifying peaks (protein binding sites)..."

# Find peaks for USF2 using the Input as control
findPeaks \
    ${HOMER_DIR}/${USF2_PREFIX} \          # ChIP sample tag directory
    -style factor \                         # For transcription factor ChIP-seq
    -o ${HOMER_DIR}/${USF2_PREFIX}/${USF2_PREFIX}_peaks.tsv \  # Output file
    -i ${HOMER_DIR}/${INPUT_PREFIX} \       # Input control tag directory
    -fdr 0.001 \                            # False discovery rate threshold

# Peak style options explained:
# -style factor: for sharp peaks (transcription factors)
# -style histone: for broad peaks (histone modifications)
# -style dnase: for DNase hypersensitivity sites
# -style groseq: for GRO-seq transcription start sites

#=============================================
# 6.3: Annotate peaks with genomic features
#=============================================
echo "Annotating peaks with genomic features..."

# Annotate the peaks with nearby genes and genomic features
annotatePeaks.pl \
    ${HOMER_DIR}/${USF2_PREFIX}/${USF2_PREFIX}_peaks.tsv \  # Input peak file
    hg38 \                                  # Reference genome
    -go ${HOMER_DIR}/${USF2_PREFIX}/go \    # Output directory for GO analysis
    -genomeOntology ${HOMER_DIR}/${USF2_PREFIX}/genomeOntology \  # Genome feature enrichment
    > ${HOMER_DIR}/${USF2_PREFIX}/${USF2_PREFIX}_peaks_annotated.tsv  # Output file

echo "Peak calling and annotation complete!"

# The annotated peaks file contains:
# - Peak locations (chromosome, start, end)
# - Peak scores and statistics
# - Nearby genes and distances to TSS
# - Gene descriptions and functions
# - Genomic features (promoter, intron, exon, etc.)
```

> 

**Parameter Explanation:**

- `-style factor` tells HOMER to look for sharp peaks typical of transcription factors

- `-fdr 0.001` sets a stringent false discovery rate threshold of 0.1%

- The annotation step connects each peak to its nearest gene and identifies whether it falls in a promoter, enhancer, or other genomic feature

- `-go` performs Gene Ontology analysis on nearby genes and creates output in the specified directory

- `-genomeOntology` analyzes the distribution of peaks relative to genomic features (promoters, introns, etc.) and identifies enriched locations

**Annotated Peaks:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/03/Screenshot-2025-03-14-at-9.29.10%E2%80%AFPM.png?resize=1024%2C255&ssl=1)

## Conclusion

Congratulations! You’ve successfully completed a basic ChIP-seq analysis pipeline, from raw sequencing data to annotated peaks. This analysis has identified regions of the genome where the USF2 transcription factor binds, potentially regulating nearby genes.

With the annotated peak table, you’re now ready to explore more advanced analyses:

- Pathway enrichment of genes near binding sites

- Integration with gene expression data

- Comparison with other transcription factors or conditions

- More detailed motif analysis and co-factor identification

Remember that ChIP-seq is just one tool in the genomics toolkit. The most compelling biological insights often come from integrating multiple data types to build a comprehensive picture of gene regulation.

## Best Practices for ChIP-seq Analysis

To ensure high-quality results from your ChIP-seq analysis, keep these best practices in mind:

### Quality Control at Every Step

- **Before Analysis:** Check sequencing quality with FastQC

- **After Alignment:** Verify mapping rates (>70% is ideal for mammalian genomes)

- **After Peak Calling:** Assess reproducibility between replicates

### Handling Controls Properly

- Always include an appropriate control (Input DNA or IgG)

- Process control samples identically to ChIP samples

- Use the same sequencing depth for ChIP and control when possible

### Data Interpretation

- Focus on high-confidence peaks (stringent FDR/p-value)

- Consider peak location relative to genes (promoters vs. enhancers)

- Integrate with other data types (RNA-seq, ATAC-seq) for biological insights

### Common Pitfalls to Avoid

- **Insufficient Sequencing Depth:** For transcription factors, aim for at least 20 million uniquely mapped reads

- **Poor Antibody Specificity:** This can lead to non-specific binding and false positives

- **Ignoring Batch Effects:** Process all samples in parallel to minimize technical variation

- **Over-interpretation:** Remember that binding doesn’t always equate to function

## Troubleshooting Common Issues

### Low Peak Counts

**Problem:** You detected very few peaks compared to expectations.

**Solutions:**

- Check antibody efficiency and specificity

- Decrease the stringency of your peak calling parameters

- Inspect browser tracks to see if enrichment is visible but below threshold

### High Background Signal

**Problem:** Your Input control shows patterns similar to your ChIP sample.

**Solutions:**

- Improve experimental protocol to reduce non-specific binding

- Increase washing stringency in future experiments

- Try alternative peak callers that handle high background better

### Inconsistent Replicates

**Problem:** Poor overlap between biological replicates.

**Solutions:**

- Use IDR (Irreproducible Discovery Rate) methodology to identify consistent peaks

- Check for batch effects or technical issues in problematic samples

- Consider pooling replicates if appropriate for your experimental design

## Further Resources

For those interested in deepening their understanding of ChIP-seq analysis:

- [HOMER Documentation](http://homer.ucsd.edu/homer/index.html): Comprehensive guide to all HOMER functions

- [ENCODE ChIP-seq Guidelines](https://www.encodeproject.org/chip-seq/transcription_factor/): Best practices from the ENCODE consortium

- [Galaxy ChIP-seq Tutorials](https://galaxyproject.org/tutorials/chip/): GUI-based alternatives to command-line analysis

## References

- Alexandros Kanterakis, George Potamias, George P. Patrinos. Chapter 4 – An Introduction to Tools, Databases, and Practical Guidelines for NGS Data Analysis. Human Genome Informatics, Academic Press, 2018, Pages 61-89, ISBN 9780128094143

*This tutorial is part of the NGS101.com beginner’s guide to next-generation sequencing analysis. If you have questions or suggestions, please leave a comment below.
