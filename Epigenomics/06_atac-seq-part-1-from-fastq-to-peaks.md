# How To Analyze ATAC-seq Data For Absolute Beginners Part 1: From FASTQ To Peaks

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** November 20, 2025
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/)

- [Introduction: Understanding ATAC-seq](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Introduction_Understanding_ATAC-seq)

  - [What is ATAC-seq?](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#What_is_ATAC-seq)
  - [ATAC-seq vs. ChIP-seq: Key Differences and Complementary Roles](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#ATAC-seq_vs_ChIP-seq_Key_Differences_and_Complementary_Roles)
  - [Differences in Data Analysis Between ATAC-seq and ChIP-seq](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Differences_in_Data_Analysis_Between_ATAC-seq_and_ChIP-seq)
  - [Why ATAC-seq Matters in Biological Research](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Why_ATAC-seq_Matters_in_Biological_Research)
  - [The ATAC-seq Data Analysis Workflow](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#The_ATAC-seq_Data_Analysis_Workflow)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Setting_Up_Your_Analysis_Environment)

  - [Required Software Installation](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Required_Software_Installation)
  - [Reference File Preparation](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Reference_File_Preparation)
  - [Download Example Data](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Download_Example_Data)

- [The ATAC-seq Analysis Pipeline](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#The_ATAC-seq_Analysis_Pipeline)

  - [Step 1: Quality Control and Adapter Trimming](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Step_1_Quality_Control_and_Adapter_Trimming)
  - [Step 2: Align Reads to the Reference Genome](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Step_2_Align_Reads_to_the_Reference_Genome)
  - [Step 3: Process and Quality Control Alignments](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Step_3_Process_and_Quality_Control_Alignments)
  - [Step 4: Peak Calling and Annotation using HOMER](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Step_4_Peak_Calling_and_Annotation_using_HOMER)
  - [Step 5: Peak Calling and Annotation using MACS2](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Step_5_Peak_Calling_and_Annotation_using_MACS2)
  - [Step 6: Peak Visualization](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Step_6_Peak_Visualization)
  - [Step 7: Motif Analysis and Differential Binding Analysis](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Step_7_Motif_Analysis_and_Differential_Binding_Analysis)

- [Summary of What We’ve Accomplished](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Summary_of_What_Weve_Accomplished)
- [Best Practices for ATAC-seq Analysis](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Best_Practices_for_ATAC-seq_Analysis)

  - [Quality Control at Every Step](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Quality_Control_at_Every_Step)
  - [Technical Considerations](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Technical_Considerations)
  - [Data Interpretation](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Data_Interpretation)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Troubleshooting_Common_Issues)

  - [Low Peak Counts](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Low_Peak_Counts)
  - [High Mitochondrial Content](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#High_Mitochondrial_Content)
  - [Inconsistent Fragment Size Distribution](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Inconsistent_Fragment_Size_Distribution)
  - [Predominant Signal from Promoters](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Predominant_Signal_from_Promoters)

- [Further Resources](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Further_Resources)
- [References](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/#Like_this)

## Introduction: Understanding ATAC-seq

At the heart of molecular biology lies a fundamental question: how do cells regulate which genes are expressed and when? One powerful technique to explore this question is Assay for Transposase-Accessible Chromatin sequencing, commonly known as ATAC-seq. This tutorial introduces beginners to the fascinating world of ATAC-seq data analysis using HOMER and MACS2, versatile software suites that have become mainstays in genomic research.

### What is ATAC-seq?

ATAC-seq represents a revolutionary method for mapping chromatin accessibility across the genome. The technique allows researchers to identify open regions of chromatin where DNA is not tightly bound to nucleosomes and is therefore accessible to regulatory proteins such as transcription factors. These accessible regions often correspond to active regulatory elements like promoters, enhancers, and other functional regions crucial for gene expression.

The experimental process begins with living cells, where a hyperactive Tn5 transposase enzyme is used to simultaneously cut open chromatin and insert sequencing adapters into these regions. The enzyme preferentially targets accessible DNA, effectively serving as a probe for open chromatin. The resulting DNA fragments are then purified, amplified by PCR, and sequenced. When mapped back to a reference genome, these sequences create a genome-wide profile of chromatin accessibility.

> 

**Beginner’s Tip:** Think of ATAC-seq as taking a snapshot of all the “open doors” in the genome where regulatory proteins can enter and influence gene expression. While ChIP-seq tracks specific proteins at work, ATAC-seq reveals all potentially active regulatory regions regardless of which specific proteins might be binding there.

### ATAC-seq vs. ChIP-seq: Key Differences and Complementary Roles

While both ATAC-seq and ChIP-seq (Chromatin Immunoprecipitation sequencing) are powerful techniques for studying gene regulation, they answer different questions and have distinct advantages:

#### Fundamental Differences:

1. **Target of Investigation:**

  - **ATAC-seq:** Directly measures chromatin accessibility, revealing all potentially active regulatory regions

  - **ChIP-seq:** Targets specific protein-DNA interactions, showing exactly where a particular protein binds

2. **Experimental Process:**

  - **ATAC-seq:** Uses Tn5 transposase to preferentially insert adapters into open chromatin; requires no antibodies

  - **ChIP-seq:** Uses antibodies to immunoprecipitate specific proteins along with their bound DNA fragments

3. **Required Input:**

  - **ATAC-seq:** Can work with as few as 500-50,000 cells (even single cells with specialized protocols)

  - **ChIP-seq:** Typically requires millions of cells, especially for factors with few binding sites

#### Advantages of ATAC-seq:

- **Simplicity:** Streamlined “two-step” protocol (transposition and PCR) compared to ChIP-seq’s multi-day procedure

- **Low Input Requirements:** Ideal for rare cell populations or limited clinical samples

- **Unbiased Discovery:** Not limited to studying known proteins or requiring specific antibodies

- **Global Regulatory Landscape:** Captures all accessible regions in a single experiment

- **Higher Signal-to-Noise Ratio:** Often provides cleaner, more distinct peaks

#### Advantages of ChIP-seq:

- **Protein Specificity:** Directly identifies where specific proteins bind

- **Functional Insight:** Connects specific proteins to their genomic targets

- **Validated Approach:** Longer history of use with established analysis pipelines

- **Histone Modifications:** Uniquely able to map specific histone marks across the genome

#### When to Use Each Technique:

- **Choose ATAC-seq when:**

  - Exploring the overall regulatory landscape without a specific protein in mind

  - Working with limited biological material (rare cell types, clinical samples)

  - Identifying all potential regulatory elements

  - Comparing global accessibility changes between conditions

  - Needing a faster, more straightforward protocol

- **Choose ChIP-seq when:**

  - Studying a specific protein’s binding sites or histone modification

  - Investigating how a particular factor responds to treatments or conditions

  - Exploring direct protein-DNA interactions

  - Cell material is abundant

  - Antibody quality is high for your protein of interest

The two techniques are highly complementary. Many researchers use ATAC-seq to identify accessible regions genome-wide and then follow up with ChIP-seq to determine which specific factors bind within those regions.

### Differences in Data Analysis Between ATAC-seq and ChIP-seq

While ATAC-seq and ChIP-seq data analysis share similar workflows, there are important distinctions to consider:

1. **Control Samples:**

  - **ChIP-seq:** Requires input control or IgG control samples

  - **ATAC-seq:** Typically analyzed without control samples, using genomic background instead

2. **Fragment Size Distribution:**

  - **ATAC-seq:** Shows a characteristic nucleosome-free pattern (<100bp) and nucleosome-bound fragments (multiples of ~200bp)

  - **ChIP-seq:** Fragment sizes depend on sonication conditions, typically 200-500bp

3. **Peak Calling Considerations:**

  - **ATAC-seq:** Peaks represent open chromatin regions; callers need to account for transposase insertion bias

  - **ChIP-seq:** Peaks represent protein binding sites; specific to the protein being studied

4. **Advanced Analysis:**

  - **ATAC-seq:** Enables nucleosome positioning and transcription factor footprinting analysis

  - **ChIP-seq:** Focuses more on motif discovery for the specific factor being studied

5. **Peak Shapes:**

  - **ATAC-seq:** Generally produces sharp, well-defined peaks at nucleosome-free regions

  - **ChIP-seq:** Peak shapes vary depending on the factor (sharp for transcription factors, broad for some histone marks)

Despite these differences, many analysis tools developed for ChIP-seq have been adapted for ATAC-seq, with some adjustments to parameters and preprocessing steps.

### Why ATAC-seq Matters in Biological Research

The applications of ATAC-seq extend to numerous biological fields:

- **Gene Regulation:** By identifying accessible chromatin regions, researchers can uncover the regulatory networks controlling gene expression during development, in different tissues, or in response to stimuli.

- **Epigenetics:** ATAC-seq reveals the epigenetic landscape that influences gene accessibility, complementing histone modification and DNA methylation studies.

- **Disease Research:** Changes in chromatin accessibility can indicate dysregulated gene expression in conditions like cancer, autoimmune diseases, and neurological disorders.

For example, cancer researchers use ATAC-seq to understand how chromatin accessibility is altered in tumor cells. Developmental biologists apply it to track how chromatin accessibility changes during cell differentiation. Immunologists employ ATAC-seq to reveal how immune cells respond to infections through changes in chromatin organization. The insights gained from these studies advance our fundamental understanding of biology and inform the development of new therapeutic strategies.

### The ATAC-seq Data Analysis Workflow

For newcomers to computational biology, analyzing ATAC-seq data can be broken down into a logical sequence of steps:

1. **Quality Assessment:** Ensuring the sequencing reads are reliable before proceeding

2. **Adapter Trimming:** Removing Tn5 transposase adapter sequences

3. **Alignment:** Mapping millions of short DNA sequences to their correct locations in the reference genome

4. **Post-alignment Processing:** Removing duplicates, filtering for high-quality alignments, and adjusting for Tn5 transposition

5. **Peak Calling:** Identifying regions of significant enrichment that represent accessible chromatin

6. **Annotation:** Connecting accessible regions to nearby genomic features such as genes, enhancers, or promoters

7. **Visualization:** Transforming complex datasets into intuitive graphical representations

8. **Motif Discovery:** Identifying common DNA sequence patterns within accessible regions, revealing potential transcription factor binding sites

Throughout this analytical journey, computational tools serve as the researcher’s compass and map, guiding the way from raw data to biological meaning. In this tutorial, we’ll specifically focus on using HOMER for preprocessing and annotation, and MACS2 for peak calling, as these tools offer a good balance of accessibility and analytical power for beginners.

## Setting Up Your Analysis Environment

Now that we understand what ATAC-seq is and how it differs from ChIP-seq, we’ll proceed with setting up our computational environment for ATAC-seq data analysis. Tools used for ChIP-seq analysis also work for ATAC-seq data. Here we use the same Conda environment and the tools installed within the environment from our [previous ChIP-seq data analysis tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/). You can keep using BWA for alignment as we did in the ChIP-seq data analysis tutorial. Here I demonstrate how to use Bowtie2, another popular aligner, for reads alignment.

### Required Software Installation

```bash
#-----------------------------------------------
# STEP 0: Setup conda environment for ATAC-seq analysis
#-----------------------------------------------

# Activate the HOMER environment created for ChIP-seq data analysis
conda activate ~/Env_Homer

# Install Bowtie2 for alignment
conda install -c bioconda bowtie2

# Install MACS2 for peak calling
conda install -c bioconda macs2

# Check installed versions
bowtie2 --version
macs2 --version

# Ensure we have all necessary packages
conda install -y \
    sambamba \               # Faster BAM file manipulation than samtools
    deeptools \              # For visualization and quality control
    multiqc \                # For aggregating QC reports
    bioconductor-diffbind    # For differential binding analysis
```

> 

**Troubleshooting Tip:** If you encounter errors during installation, make sure you have sufficient disk space and permissions to install software. For permission errors, you might need to use `sudo` or contact your system administrator.

### Reference File Preparation

The reference files used for ATAC-seq data analysis are the same as those for ChIP-seq data. Please refer to my [previous ChIP-seq data analysis tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/) for reference file preparation. The prebuilt Bowtie2 index can be downloaded from [refgenie](http://refgenomes.databio.org/v3/assets/splash/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/bowtie2_index?tag=default):

```bash
#-----------------------------------------------
# STEP 1: Download reference genome index files
#-----------------------------------------------

# Create a dedicated directory for Bowtie2 Index files
mkdir -p ~/Genome_Index/Bowtie2_hg38
cd ~/Genome_Index/Bowtie2_hg38

# Define the base URL for all index files
BASE_URL="http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/bowtie2_index__default"
FILE_PREFIX="2230c535660fb4774114bfa966a62f823fdb6d21acf138d4"

# Download all Bowtie2 index files
echo "Downloading Bowtie2 index files..."
wget ${BASE_URL}/${FILE_PREFIX}.1.bt2
wget ${BASE_URL}/${FILE_PREFIX}.2.bt2
wget ${BASE_URL}/${FILE_PREFIX}.3.bt2
wget ${BASE_URL}/${FILE_PREFIX}.4.bt2
wget ${BASE_URL}/${FILE_PREFIX}.rev.1.bt2
wget ${BASE_URL}/${FILE_PREFIX}.rev.2.bt2

echo "All reference genome index files downloaded successfully!"

# Download ENCODE blacklisted regions for later filtering
mkdir -p ~/Genome_Index/ENCODE_BlackList
cd ~/Genome_Index/ENCODE_BlackList
wget https://github.com/Boyle-Lab/Blacklist/raw/master/lists/hg38-blacklist.v2.bed.gz
gunzip hg38-blacklist.v2.bed.gz
```

> 

**Best Practice:** Store reference files in a centralized location on your system to avoid duplicating large files for different projects.

### Download Example Data

For this tutorial, we’ll use a paired-end ATAC-seq dataset from mouse embryonic stem cells. Let’s create a directory structure and download some sample data:

```bash
#-----------------------------------------------
# STEP 2: Download example ATAC-seq dataset
#-----------------------------------------------

# Create a directory structure for the project
mkdir -p ~/ATAC_Dataset/{raw,trimmed,Alignment,Homer,MACS2}

# For demonstration, we will assume you have:
# ~/ATAC_Dataset/raw/Sample1_R1_001.fastq.gz (Read 1)
# ~/ATAC_Dataset/raw/Sample1_R2_001.fastq.gz (Read 2)
```

> 

**Note:** For real analyses, you would typically use your own data or download data from repositories like GEO or SRA using tools like `prefetch` and `fasterq-dump` from the SRA toolkit, as shown in our ChIP-seq tutorial.

## The ATAC-seq Analysis Pipeline

Now that we have our environment set up, let’s walk through the analysis pipeline step by step. Unlike ChIP-seq, ATAC-seq data typically don’t require input control samples for peak calling, though they can be included if available.

### Step 1: Quality Control and Adapter Trimming

First, we need to assess data quality and trim adapter sequences. ATAC-seq libraries often contain Tn5 transposase adapter sequences, which must be removed:

```bash
#-----------------------------------------------
# STEP 3: Quality control and adapter trimming
#-----------------------------------------------

# Create directories for the trimmed outputs
mkdir -p ~/ATAC_Dataset/trimmed/Sample1

# Trim the adapters
echo "Trimming adapters from paired-end ATAC-seq data..."
trim_galore \
    --fastqc \                # Run FastQC after trimming for QC report
    --paired \                # Data is paired-end
    --cores 12 \              # Use 12 CPU cores for faster processing
    --quality 20 \            # Trim low-quality bases (Q<20)
    --stringency 3 \          # Require at least 3bp overlap with adapter
    --length 20 \             # Discard reads shorter than 20bp after trimming
    --output_dir ~/ATAC_Dataset/trimmed/Sample1 \
    ~/ATAC_Dataset/raw/Sample1_R1_001.fastq.gz \
    ~/ATAC_Dataset/raw/Sample1_R2_001.fastq.gz

echo "Quality control and adapter trimming complete!"
```

> 

**What This Does:**

- FastQC generates quality reports before and after trimming

- Trim Galore removes adapter sequences and low-quality bases

- The paired-end parameter ensures reads remain properly paired after trimming

- Output files will be named with ‘_val_1.fq.gz’ and ‘_val_2.fq.gz’ suffixes

### Step 2: Align Reads to the Reference Genome

Next, we map our trimmed reads to the reference genome using Bowtie2. For ATAC-seq, we use parameters optimized for the characteristics of Tn5 transposition:

```bash
#-----------------------------------------------
# STEP 4: Align reads to the reference genome
#-----------------------------------------------

# Create directory for alignment files
mkdir -p ~/ATAC_Dataset/Alignment/Sample1

# Set variables for readability
INDEX="~/Genome_Index/Bowtie2_hg38/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4"
THREADS=12

echo "Aligning ATAC-seq reads to reference genome..."
bowtie2 \
    -q \                    # Input is in FASTQ format
    -x ${INDEX} \           # Bowtie2 index prefix
    -p ${THREADS} \         # Number of threads
    -1 ~/ATAC_Dataset/trimmed/Sample1/Sample1_R1_001_val_1.fq.gz \  # Read 1
    -2 ~/ATAC_Dataset/trimmed/Sample1/Sample1_R2_001_val_2.fq.gz \  # Read 2
    --sensitive \           # Alignment preset (balanced speed and sensitivity)
    --no-mixed \            # Only report paired alignments
    --no-discordant \       # Only report concordant alignments
    --no-unal \             # Suppress unaligned reads
    -S ~/ATAC_Dataset/Alignment/Sample1/Sample1.sam  # Output SAM file

echo "Alignment complete! SAM file created."
```

> 

**ATAC-seq Alignment Tip:**

- For ATAC-seq, we use `--no-mixed` and `--no-discordant` to ensure only properly paired reads are reported

- This helps avoid artificial peaks caused by misaligned read pairs

- The `--sensitive` preset offers a good balance of speed and sensitivity for most applications

### Step 3: Process and Quality Control Alignments

After alignment, we need to convert, sort, and filter our alignment files to prepare them for peak calling. This step is crucial for ATAC-seq as it helps remove technical artifacts:

```bash
#-----------------------------------------------
# STEP 5: Process and filter alignment files
#-----------------------------------------------

# Set variables for readability
THREADS=12
SAMPLE_PREFIX="Sample1"
ALIGN_DIR="~/ATAC_Dataset/Alignment/Sample1"

#=============================================
# 5.1: SAM to BAM conversion
#=============================================
echo "Converting SAM to BAM format (compressed binary format)..."

samtools view \
    -h \                   # Include header in output
    -S \                   # Input is SAM format
    -b \                   # Output BAM format
    -@ ${THREADS} \        # Use multiple threads
    -o ${ALIGN_DIR}/${SAMPLE_PREFIX}.bam \  # Output file
    ${ALIGN_DIR}/${SAMPLE_PREFIX}.sam       # Input file

# Remove large SAM file to save space
rm ${ALIGN_DIR}/${SAMPLE_PREFIX}.sam

#=============================================
# 5.2: Sort BAM files by genomic coordinates
#=============================================
echo "Sorting BAM files by chromosome position..."

# Using sambamba for faster sorting
sambamba sort \
    -t ${THREADS} \        # Use multiple threads
    -o ${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted.bam \  # Output sorted BAM
    ${ALIGN_DIR}/${SAMPLE_PREFIX}.bam              # Input BAM

# Remove unsorted BAM file to save space
rm ${ALIGN_DIR}/${SAMPLE_PREFIX}.bam

#=============================================
# 5.3: Mark and remove PCR duplicates
#=============================================
echo "Marking and removing PCR duplicates..."

# ATAC-seq libraries can have high duplication rates due to PCR amplification
# Use Picard to mark and remove duplicates
picard MarkDuplicates \
    I=${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted.bam \            # Input BAM
    O=${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted_dedup.bam \      # Output BAM
    M=${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted_dedup_metrics.txt \  # Metrics file
    REMOVE_DUPLICATES=true \                            # Remove duplicates
    VALIDATION_STRINGENCY=LENIENT                       # Less strict validation

# Create index for the deduplicated BAM file
samtools index ${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted_dedup.bam

#=============================================
# 5.4: Filter for high-quality alignments
#=============================================
echo "Filtering for high-quality alignments..."

# Filter out:
# 1. Multi-mapped reads (XS tag exists)
# 2. Unmapped reads
# 3. Duplicates (already removed, but to be safe)
# 4. Low mapping quality reads
sambamba view \
    -h \                   # Include header
    -f bam \               # Output in BAM format
    -F "[XS] == null and not unmapped and not duplicate and mapping_quality >= 30" \  # Filter expression
    -t ${THREADS} \        # Number of threads
    ${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted_dedup.bam \  # Input BAM
    -o ${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted_filtered.bam  # Output filtered BAM

#=============================================
# 5.5: Filter out blacklisted regions
#=============================================
echo "Removing blacklisted regions from alignments..."

# Blacklisted regions are known problematic regions
# that tend to show artifactual signal in many assays
bedtools intersect \
    -v \                   # Only keep reads that DO NOT overlap blacklist
    -abam ${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted_filtered.bam \  # Input BAM
    -b ~/Genome_Index/ENCODE_BlackList/hg38-blacklist.v2.bed \  # Blacklist BED
    > ${ALIGN_DIR}/${SAMPLE_PREFIX}_filtered_blacklist_removed.bam  # Output filtered BAM

#=============================================
# 5.6: Final sorting of filtered BAM
#=============================================
echo "Final sorting of filtered BAM file..."

sambamba sort \
    -t ${THREADS} \        # Use multiple threads
    -o ${ALIGN_DIR}/${SAMPLE_PREFIX}_final.bam \  # Output final BAM
    ${ALIGN_DIR}/${SAMPLE_PREFIX}_filtered_blacklist_removed.bam  # Input BAM

# Index the final BAM file
samtools index ${ALIGN_DIR}/${SAMPLE_PREFIX}_final.bam

#=============================================
# 5.7: Generate alignment statistics
#=============================================
echo "Generating alignment statistics..."

# Count total reads in original BAM
TOTAL=$(samtools view -c ${ALIGN_DIR}/${SAMPLE_PREFIX}_sorted.bam)

# Count reads after all filtering
FINAL=$(samtools view -c ${ALIGN_DIR}/${SAMPLE_PREFIX}_final.bam)

# Calculate percentage retained
PERCENT=$(echo "scale=2; 100*${FINAL}/${TOTAL}" | bc)

echo "Original read count: ${TOTAL}"
echo "Final read count: ${FINAL}"
echo "Percentage retained: ${PERCENT}%"

echo "Alignment processing and filtering complete!"
```

> 

**Why These Filtering Steps Matter for ATAC-seq:**

- Removing duplicates is crucial as PCR amplification can artificially inflate signal

- Multi-mapped reads (with XS tag) can create false positives

- Blacklisted regions often show artifactual enrichment across many assays

- High mapping quality ensures reliable alignment locations

### Step 4: Peak Calling and Annotation using HOMER

Now let’s identify the accessible chromatin regions using HOMER, which offers good integration with downstream annotation and motif analysis:

```bash
#-----------------------------------------------
# STEP 6: Peak calling and annotation with HOMER
#-----------------------------------------------

# Create directory for HOMER analysis
mkdir -p ~/ATAC_Dataset/Homer/Sample1

#=============================================
# 6.1: Create HOMER tag directory
#=============================================
echo "Creating HOMER tag directory..."

# Variables for readability
SAMPLE_PREFIX="Sample1"
ALIGN_DIR="~/ATAC_Dataset/Alignment/Sample1"
HOMER_DIR="~/ATAC_Dataset/Homer"

# Create tag directory for ATAC-seq sample
# Tag directories contain processed alignment data optimized for HOMER analysis
echo "Creating tag directory for ATAC-seq sample..."
makeTagDirectory \
    ${HOMER_DIR}/${SAMPLE_PREFIX} \          # Output directory
    ${ALIGN_DIR}/${SAMPLE_PREFIX}_final.bam \  # Input BAM
    -genome hg38 \                          # Reference genome
    -checkGC \                              # Calculate GC content distribution
    -tbp 1 \                               # Tags per bp (normalization factor)
    -format sam                             # Input format (BAM is a binary SAM)

#=============================================
# 6.2: Call peaks to identify accessible regions
#=============================================
echo "Identifying accessible chromatin regions (peaks)..."

# Find peaks for ATAC-seq
findPeaks \
    ${HOMER_DIR}/${SAMPLE_PREFIX} \          # Tag directory
    -style dnase \                         # For ATAC-seq/DNase-style peaks
    -o ${HOMER_DIR}/${SAMPLE_PREFIX}/peaks_${SAMPLE_PREFIX}_fdr1e-05.txt \  # Output file
    -fdr 1e-05 \                          # False discovery rate threshold (strict)
    -region \                             # Report concise peak boundaries
    -minDist 150 \                        # Minimum distance between peaks
    -size 150                             # Size of region for ATAC-seq peaks

# Peak style explanation:
# -style dnase: optimized for ATAC-seq/DNase-seq data (sharp, open chromatin peaks)

#=============================================
# 6.3: Annotate peaks with genomic features
#=============================================
echo "Annotating peaks with genomic features..."

# Annotate the peaks with nearby genes and genomic features
annotatePeaks.pl \
    ${HOMER_DIR}/${SAMPLE_PREFIX}/peaks_${SAMPLE_PREFIX}_fdr1e-05.txt \  # Input peak file
    hg38 \                                  # Reference genome
    -d ${HOMER_DIR}/${SAMPLE_PREFIX} \      # Tag directory for visualization
    -go ${HOMER_DIR}/${SAMPLE_PREFIX}/go \    # Output directory for GO analysis
    -genomeOntology ${HOMER_DIR}/${SAMPLE_PREFIX}/genomeOntology \  # Genome feature enrichment
    > ${HOMER_DIR}/${SAMPLE_PREFIX}/peaks_${SAMPLE_PREFIX}_fdr1e-05_annotated.txt  # Output file

echo "HOMER peak calling and annotation complete!"
```

**Parameter Explanation:**

- `-style dnase` tells HOMER to look for sharp peaks typical of open chromatin regions

- `-fdr 1e-05` sets a stringent false discovery rate threshold of 0.001%

- `-region` focuses on the precise peak boundaries rather than fixed-width windows

- `-minDist 150` prevents calling multiple peaks within 150bp of each other

- The annotation step connects each peak to its nearest gene and identifies whether it falls in a promoter, enhancer, or other genomic feature

- See my earlier [ChIP-seq tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/) for a detailed explanation of HOMER outputs.

### Step 5: Peak Calling and Annotation using MACS2

MACS2 is another popular peak caller widely used for ATAC-seq analysis. The peak calling and annotation process is very similar to the one demonstrated in my previous [ChIP-seq tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/), requiring only a few adjustments specific to ATAC-seq data:

```bash
#-----------------------------------------------
# STEP 7: Peak calling with MACS2
#-----------------------------------------------

# Create directory for MACS2 analysis
mkdir -p ~/ATAC_Dataset/MACS2/Sample1

#=============================================
# 7.1: Call peaks using MACS2
#=============================================
echo "Calling peaks with MACS2..."

# Variables for readability
SAMPLE_PREFIX="Sample1"
ALIGN_DIR="~/ATAC_Dataset/Alignment/Sample1"
MACS2_DIR="~/ATAC_Dataset/MACS2"

# Call peaks with MACS2
# ATAC-seq specific parameters: --nomodel --shift -100 --extsize 200
macs2 callpeak \
    -t ${ALIGN_DIR}/${SAMPLE_PREFIX}_final.bam \  # Input BAM file
    -f BAMPE \                                    # Format: paired-end BAM
    -g hs \                                       # Genome size: human (hs)
    -n ${SAMPLE_PREFIX} \                         # Output name prefix
    --outdir ${MACS2_DIR}/${SAMPLE_PREFIX} \      # Output directory
    -q 0.01 \                                     # q-value (FDR) cutoff
    --nomodel \                                   # Skip model building
    --keep-dup all \                              # Keep all duplicates (we already removed them)
    --call-summits                                # Call peak summits

echo "MACS2 peak calling complete!"

# MACS2 parameter explanation:
# -f BAMPE: Format is paired-end BAM, uses actual fragment size
# --nomodel: Skip the model building step, important for ATAC-seq
# -q 0.01: FDR threshold of 1%
# --call-summits: Find the precise summit positions within peaks
```

> 

**MACS2 vs. HOMER for ATAC-seq:**

- MACS2 is often preferred for ATAC-seq due to its handling of paired-end data and shift/extension parameters

- HOMER offers more integrated downstream analysis (annotation, motif discovery)

- Both tools are widely accepted in the field, and comparing results can increase confidence

- For details on MACS2 output, refer to my [previous ChIP-seq tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/).

### Step 6: Peak Visualization

The visualization of ATAC-seq peaks follows the same principles and techniques outlined in my [ChIP-seq data visualization tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-2-visualizing-chip-seq-data/).

```bash
#-----------------------------------------------
# STEP 8: Creating browser tracks and visualizations
#-----------------------------------------------

#=============================================
# 8.1: Create bigWig tracks for visualization using deepTools
#=============================================
echo "Creating bigWig tracks for genome browser visualization..."

# Get chromosome sizes for bigWig conversion
fetchChromSizes hg38 > ~/ATAC_Dataset/hg38.chrom.sizes

# Variables for readability
SAMPLE_PREFIX="Sample1"
ALIGN_DIR="~/ATAC_Dataset/Alignment/Sample1"
VISUAL_DIR="~/ATAC_Dataset/Visualization"
mkdir -p ${VISUAL_DIR}

# Method 1: Make bigWig files using HOMER
makeUCSCfile ~/ATAC_Dataset/Homer/Sample1 \
             -o auto \
             -bigWig ChromSizes_hg38.txt \
             -style dnase

# Method 2: Make bigWig files using deepTools
bamCoverage \
    --bam ${ALIGN_DIR}/${SAMPLE_PREFIX}_final.bam \  # Input BAM
    --outFileName ${VISUAL_DIR}/${SAMPLE_PREFIX}.bw \  # Output bigWig
    --outFileFormat bigwig \
    --binSize 10 \                         # 10bp resolution
    --normalizeUsing RPKM \                # Normalize by reads per kilobase per million
    --extendReads \                        # Extend reads to fragment size
    --ignoreDuplicates \                   # Skip duplicate reads
    --numberOfProcessors 10                # Use 10 threads

#=============================================
# 8.2: Create a BED file of peaks for sharing
#=============================================
echo "Creating BED files of peaks for sharing..."

# Convert peak files to standard BED format
pos2bed.pl ${HOMER_DIR}/${SAMPLE_PREFIX}/peaks_${SAMPLE_PREFIX}_fdr1e-05.txt \
    > ${VISUAL_DIR}/${SAMPLE_PREFIX}_homer_peaks.bed

# Copy MACS2 narrowPeak file (already in BED-like format)
cp ${MACS2_DIR}/${SAMPLE_PREFIX}/${SAMPLE_PREFIX}_peaks.narrowPeak \
    ${VISUAL_DIR}/${SAMPLE_PREFIX}_macs2_peaks.bed

echo "Visualization files created successfully!"
```

### Step 7: Motif Analysis and Differential Binding Analysis

The methodology for motif analysis in ATAC-seq data mirrors that of ChIP-seq analysis, as detailed in my [previous tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-3-differential-binding-analysis-and-motif-discovery/).

# Conclusion and Next Steps

Congratulations! You’ve successfully completed a basic ATAC-seq analysis pipeline, from raw sequencing data to annotated accessible chromatin regions. This analysis has identified regions of the genome where chromatin is accessible, potentially revealing active regulatory elements such as promoters and enhancers.

## Summary of What We’ve Accomplished

1. **Quality Control and Preprocessing:** We ensured high-quality data by trimming adapters and removing low-quality reads

2. **Alignment:** We mapped reads to the reference genome using Bowtie2 with parameters optimized for ATAC-seq

3. **Filtering:** We filtered alignments to remove duplicates, multi-mapped reads, and blacklisted regions

4. **Peak Calling:** We identified accessible chromatin regions using both HOMER and MACS2

5. **Annotation:** We annotated peaks with genomic features such as promoters, enhancers, and nearby genes

6. **Visualization:** We created tracks for genome browsers to visualize and share our results

7. **Motif Analysis:** We discovered DNA sequence motifs enriched in accessible regions, revealing potential transcription factor binding sites

## Best Practices for ATAC-seq Analysis

To ensure high-quality results from your ATAC-seq analysis, keep these best practices in mind:

### Quality Control at Every Step

- **Before Analysis:** Check sequencing quality with FastQC

- **After Alignment:** Verify mapping rates (>70% is ideal for mammalian genomes)

- **After Peak Calling:** Assess peak characteristics and reproducibility between replicates

### Technical Considerations

- **Fragment Size Distribution:** ATAC-seq data should show characteristic nucleosome-free (~50-100bp) and nucleosome-bound (~200bp, ~400bp) peaks

- **Library Complexity:** Ensure sufficient complexity (low duplication rates) to capture the true signal

- **Mitochondrial Contamination:** Mitochondrial DNA is highly accessible and can dominate ATAC-seq libraries; filtering these reads is often necessary

- **Tn5 Insertion Bias:** Tn5 has slight sequence preferences that can be corrected in advanced analyses

### Data Interpretation

- **Focus on High-confidence Regions:** Use stringent thresholds for peak calling

- **Consider Peak Location:** Promoter-proximal peaks often indicate active genes, while distal peaks suggest enhancers

- **Integrate with Expression Data:** Correlate accessibility with gene expression to identify functional elements

- **Validate Key Findings:** Consider experimental validation of important regulatory regions

## Troubleshooting Common Issues

### Low Peak Counts

**Problem:** You detected very few peaks compared to expectations.

**Solutions:**

- Check library quality and complexity

- Decrease the stringency of your peak calling parameters

- Ensure you have sufficient sequencing depth (>50 million reads recommended)

- Verify cell viability in the original sample (dead cells show global accessibility changes)

### High Mitochondrial Content

**Problem:** A large percentage of reads map to mitochondrial DNA.

**Solutions:**

- Filter out mitochondrial reads before analysis

- Optimize cell lysis conditions in future experiments to reduce mitochondrial DNA release

- Consider deeper sequencing to compensate for lost reads

### Inconsistent Fragment Size Distribution

**Problem:** Missing the characteristic nucleosome-free and mono-nucleosome peaks in fragment size distribution.

**Solutions:**

- Check Tn5 transposase activity and concentration

- Ensure proper cell permeabilization without over-digestion

- Optimize size selection during library preparation

### Predominant Signal from Promoters

**Problem:** Most accessible regions are at promoters, with few distal elements.

**Solutions:**

- Increase sequencing depth to detect weaker distal signals

- Optimize cell lysis and tagmentation conditions

- Consider cell type specificity (some cell types have fewer active enhancers)

## Further Resources

For those interested in deepening their understanding of ATAC-seq analysis:

- [ENCODE ATAC-seq Guidelines](https://www.encodeproject.org/atac-seq/): Best practices from the ENCODE consortium

- [Galaxy ATAC-seq Tutorials](https://training.galaxyproject.org/training-material/topics/epigenetics/tutorials/atac-seq/tutorial.html): GUI-based alternatives to command-line analysis

- [Schep et al. (2017) – chromVAR](https://www.nature.com/articles/nmeth.4401): Methods for analyzing variation in chromatin accessibility

- [Yan et al. (2020) – TOBIAS](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-01978-5): Advanced transcription factor footprinting analysis

## References

- Buenrostro, J.D., Giresi, P.G., Zaba, L.C. et al. Transposition of native chromatin for fast and sensitive epigenomic profiling of open chromatin, DNA-binding proteins and nucleosome position. Nature Methods 10, 1213–1218 (2013).

- Corces, M.R., Trevino, A.E., Hamilton, E.G. et al. An improved ATAC-seq protocol reduces background and enables interrogation of frozen tissues. Nature Methods 14, 959–962 (2017).

- Grandi, F.C., Modi, H., Kampman, L. et al. Chromatin accessibility profiling by ATAC-seq. Nat Protoc 17, 1518–1552 (2022). https://doi.org/10.1038/s41596-022-00692-9

*This tutorial is part of the NGS101.com beginner’s guide to next-generation sequencing analysis. If you have questions or suggestions, please leave a comment below.*
