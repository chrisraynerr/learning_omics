# How To Analyze CUT&RUN/Tag Data For Absolute Beginners: From FASTQ to Peaks

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/)

---

*A comprehensive step-by-step guide to understanding and analyzing CUT&RUN and CUT&Tag data for high-precision chromatin profiling*

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/)

- [Introduction: The Evolution of Chromatin Profiling Technologies](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Introduction_The_Evolution_of_Chromatin_Profiling_Technologies)
- [What Are CUT&RUN and CUT&Tag?](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#What_Are_CUT_RUN_and_CUT_Tag)

  - [CUT&RUN: Precision Cleavage for Chromatin Mapping](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#CUT_RUN_Precision_Cleavage_for_Chromatin_Mapping)
  - [CUT&Tag: Combining Cleavage with In-Situ Tagmentation](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#CUT_Tag_Combining_Cleavage_with_In-Situ_Tagmentation)
  - [Key Differences Between CUT&RUN and CUT&Tag](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Key_Differences_Between_CUT_RUN_and_CUT_Tag)

- [Method Comparison: CUT&RUN/Tag vs Traditional Approaches](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Method_Comparison_CUT_RUNTag_vs_Traditional_Approaches)

  - [Advantages and Limitations](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Advantages_and_Limitations)
  - [Key Advantages](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Key_Advantages)
  - [Current Limitations](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Current_Limitations)

- [When to Choose CUT&RUN/Tag: Decision Tree for Method Selection](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#When_to_Choose_CUT_RUNTag_Decision_Tree_for_Method_Selection)

  - [Decision Tree for Method Selection](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Decision_Tree_for_Method_Selection)
  - [Method Selection Matrix](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Method_Selection_Matrix)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Setting_Up_Your_Analysis_Environment)

  - [Required Software Installation](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Required_Software_Installation)
  - [Download Example Data](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Download_Example_Data)

- [The CUT&RUN/Tag Analysis Pipeline](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#The_CUT_RUNTag_Analysis_Pipeline)

  - [Step 1: Quality Control and Adapter Trimming](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_1_Quality_Control_and_Adapter_Trimming)
  - [Step 2: Align Reads to the Reference Genome](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_2_Align_Reads_to_the_Reference_Genome)
  - [Step 3: Process and Quality Control Alignments](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_3_Process_and_Quality_Control_Alignments)
  - [Step 4: Peak Calling with MACS2](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_4_Peak_Calling_with_MACS2)
  - [Step 5: Peak Calling with SEACR](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_5_Peak_Calling_with_SEACR)
  - [Step 6: Peak Annotation and Functional Analysis](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_6_Peak_Annotation_and_Functional_Analysis)
  - [Step 7: Peak Visualization](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_7_Peak_Visualization)
  - [Step 8: Motif Analysis and Advanced Functional Analysis](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Step_8_Motif_Analysis_and_Advanced_Functional_Analysis)

- [Best Practices for CUT&RUN/Tag Analysis](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Best_Practices_for_CUT_RUNTag_Analysis)

  - [Experimental Quality Control](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Experimental_Quality_Control)
  - [Computational Analysis Guidelines](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Computational_Analysis_Guidelines)
  - [Data Interpretation Guidelines](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Data_Interpretation_Guidelines)

- [Troubleshooting Common CUT&RUN/Tag Issues](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Troubleshooting_Common_CUT_RUNTag_Issues)

  - [Low Peak Counts](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Low_Peak_Counts)
  - [High Mitochondrial Content](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#High_Mitochondrial_Content)
  - [Poor Fragment Size Distribution](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Poor_Fragment_Size_Distribution)
  - [Inconsistent Replicates](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Inconsistent_Replicates)
  - [High Background Signal](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#High_Background_Signal)

- [Conclusion and Next Steps](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Conclusion_and_Next_Steps)
- [References](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-cutrun-tag-data-for-absolute-beginners-from-fastq-to-peaks/#Like_this)

## Introduction: The Evolution of Chromatin Profiling Technologies

Understanding protein-DNA interactions and chromatin organization remains one of molecular biology’s most fundamental challenges. While ChIP-seq and ATAC-seq have been the gold standards for chromatin profiling, CUT&RUN (Cleavage Under Targets and Release Using Nuclease) and CUT&Tag (Cleavage Under Targets and Tagmentation) represent revolutionary advances in the field.

These techniques offer unprecedented precision and efficiency in mapping protein-DNA interactions, addressing many limitations of traditional chromatin profiling methods. For researchers studying gene regulation, epigenetic modifications, or chromatin architecture, CUT&RUN and CUT&Tag provide powerful alternatives that deliver high-quality results with lower input requirements and reduced experimental complexity.

> 

**Beginner’s Tip:** Think of CUT&RUN and CUT&Tag as “molecular scissors” that cut DNA precisely where your protein of interest is bound, rather than randomly fragmenting chromatin like traditional ChIP-seq. This targeted approach creates cleaner, more precise maps of protein binding sites.

## What Are CUT&RUN and CUT&Tag?

### CUT&RUN: Precision Cleavage for Chromatin Mapping

CUT&RUN is an innovative chromatin profiling method that directly cleaves DNA at sites where specific proteins are bound. The technique begins with live cells that are gently permeabilized and treated with an antibody against the protein of interest. Instead of cross-linking proteins to DNA and fragmenting chromatin, CUT&RUN employs protein A-micrococcal nuclease (pA-MNase).

The pA-MNase fusion protein binds to the antibody attached to the target protein, positioning the micrococcal nuclease directly at the protein-DNA binding site. When activated with calcium, the nuclease cleaves DNA immediately adjacent to where the protein is bound, releasing small DNA fragments for sequencing. This targeted approach generates extremely precise maps with minimal background noise.

### CUT&Tag: Combining Cleavage with In-Situ Tagmentation

CUT&Tag builds upon the CUT&RUN principle but uses protein A-Tn5 transposase (pA-Tn5) instead of micrococcal nuclease. The Tn5 transposase simultaneously fragments DNA and adds sequencing adapters through tagmentation, eliminating several downstream library preparation steps.

The process begins with antibody binding to the target protein in permeabilized cells. The pA-Tn5 complex binds to the antibody, positioning the transposase at the protein binding site. Upon activation, Tn5 fragments DNA and inserts sequencing adapters directly into cleaved ends, creating a library ready for PCR amplification. This streamlined approach reduces both time and complexity while maintaining targeted cleavage precision.

### Key Differences Between CUT&RUN and CUT&Tag

**Feature****CUT&RUN****CUT&Tag****Cleavage Enzyme****Library Prep****Fragment Sizes****Hands-on Time****Signal Quality****Throughput****Cost per Sample****Best For**

|  |  |  |
| --- | --- | --- |
|  | Micrococcal nuclease (MNase) | Tn5 transposase |
|  | Multi-step (end repair, ligation) | Single-step (integrated tagmentation) |
|  | Uniform, nucleosome patterns | Broader size distribution |
|  | Longer (2-3 days) | Shorter (1-2 days) |
|  | Slightly higher S/N ratio | Comparable quality |
|  | Moderate | Higher |
|  | Higher | Lower |
|  | Precision mapping | High-throughput studies |

## Method Comparison: CUT&RUN/Tag vs Traditional Approaches

### Advantages and Limitations

**Criteria****CUT&RUN/Tag****ChIP-seq****ATAC-seq****Cell Input****Background Signal****Resolution****Protein Specificity****Protocol Time****Cost****Cross-linking****Antibody Quality**

|  |  |  |  |
| --- | --- | --- | --- |
|  | 1K-100K cells | 1M-10M cells | 50K-500K cells |
|  | Very low | Moderate-high | Low-moderate |
|  | Excellent | Good | Excellent |
|  | High (antibody-dependent) | High (antibody-dependent) | None (global accessibility) |
|  | 1-2 days | 3-5 days | 1 day |
|  | Low-moderate | High | Low |
|  | Not required | Required | Not required |
|  | Critical | Critical | Not applicable |

### Key Advantages

- **Enhanced Precision:** Direct cleavage at binding sites produces sharper peaks and better resolution

- **Low Input Requirements:** Works with 100-1000x fewer cells than ChIP-seq

- **Minimal Background:** Targeted cleavage eliminates random chromatin fragmentation noise

- **Streamlined Workflow:** No cross-linking, sonication, or extensive purification steps

- **Cost Effective:** Lower reagent costs and reduced hands-on time

- **Better Antibody Performance:** Native chromatin improves antibody accessibility

### Current Limitations

- **Antibody Dependency:** Results quality depends entirely on antibody specificity

- **Limited Multiplexing:** Current protocols focus on single targets per reaction

- **Reagent Availability:** Specialized enzymes (pA-MNase, pA-Tn5) less widely available

- **High Mitochondrial Content:** Often requires computational filtering of mitochondrial reads

- **Protocol Optimization:** May need adjustment for different cell types or factors

## When to Choose CUT&RUN/Tag: Decision Tree for Method Selection

Understanding when to use CUT&RUN/Tag versus other chromatin profiling methods is crucial for experimental design:

### Decision Tree for Method Selection

- **Do you need protein-specific information?**

- No → Use ATAC-seq for general accessibility

- Yes → Continue to next question

- **Do you have limited cell numbers (<100K)?**

- Yes → Use CUT&RUN/Tag

- No → Continue to next question

- **Is your factor well-studied with established ChIP-seq protocols?**

- Yes → Consider ChIP-seq for comparison with literature

- No → CUT&RUN/Tag likely better choice

- **Do you need high-throughput processing of many samples?**

- Yes → CUT&Tag preferred over CUT&RUN

- No → Either CUT&RUN or CUT&Tag suitable

### Method Selection Matrix

**Use Case****Best Method****Rationale**

|  |  |  |
| --- | --- | --- |
| Limited cell input (<100K) | CUT&RUN/Tag | Exceptional sensitivity |
| High-throughput studies | CUT&Tag | Streamlined workflow |
| Precise factor mapping | CUT&RUN | Highest resolution |
| Established workflows | ChIP-seq | Literature comparison |
| General accessibility | ATAC-seq | No antibody required |

## Setting Up Your Analysis Environment

The data analysis workflow for CUT&RUN/Tag shares fundamental similarities with other chromatin accessibility analysis, building upon the computational environment from my previous [ChIP-seq](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/) and [ATAC-seq tutorials](https://ngs101.com/how-to-analyze-atac-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks/). We’ll add SEACR (Sparse Enrichment Analysis for CUT&RUN), a specialized peak calling tool optimized for CUT&RUN/Tag data.

### Required Software Installation

```bash
#-----------------------------------------------
# STEP 0: Setup conda environment for CUT&RUN/Tag analysis
#-----------------------------------------------

# Activate the HOMER environment created for previous tutorials
conda activate ~/Env_Homer

# Add channels (only if you haven't done this before)
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict

# Install SEACR for CUT&RUN/Tag-specific peak calling
conda install -y seacr

# Install additional useful tools for CUT&RUN/Tag analysis
# Faster BAM file manipulation
conda install -y sambamba

# Verify SEACR installation
SEACR_1.3.sh --help

# Verify other key tools
bwa --version
macs2 --version
```

> 

**Troubleshooting Tip:** If you encounter errors during SEACR installation, you can also install it manually from the [GitHub repository](https://github.com/FredHutch/SEACR) or use alternative peak callers like MACS2 with adjusted parameters.

### Download Example Data

For this tutorial, we’ll use a publicly available CUT&RUN dataset from **[GSE104550](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE104550)**. We’ll download data for the transcription factor CTCF and a corresponding control sample:

```bash
#-----------------------------------------------
# STEP 1: Download example CUT&RUN dataset
#-----------------------------------------------

# Create a directory structure for the project
mkdir -p ~/GSE104550/{raw,trimmed,alignment,peaks,visualization}
cd ~/GSE104550/raw

# Download example CUT&RUN data for CTCF - SRR6128974
prefetch SRR6128974
fasterq-dump --split-files SRR6128974/SRR6128974.sra

# Download CONTROL sample (Input DNA) - SRR6128981
prefetch SRR6128981
fasterq-dump --split-files SRR6128981/SRR6128981.sra

gzip *.fastq
rm -r SRR6128974 SRR6128981

mv SRR6128974_1.fastq.gz CTCF_CUT_Run_R1.fastq.gz
mv SRR6128974_2.fastq.gz CTCF_CUT_Run_R2.fastq.gz
mv SRR6128981_1.fastq.gz Control_CUT_Run_R1.fastq.gz
mv SRR6128981_2.fastq.gz Control_CUT_Run_R2.fastq.gz
```

## The CUT&RUN/Tag Analysis Pipeline

The computational workflow for CUT&RUN/Tag data follows the same general structure as ChIP-seq and ATAC-seq, with method-specific optimizations. Let’s walk through each step systematically.

### Step 1: Quality Control and Adapter Trimming

CUT&RUN/Tag libraries may contain adapter contamination, particularly CUT&Tag libraries using Tn5 transposase. Quality control and trimming are essential first steps:

```bash
#-----------------------------------------------
# STEP 2: Quality control and adapter trimming
#-----------------------------------------------

# Create directories for trimmed outputs
mkdir -p ~/GSE104550/trimmed/{CTCF,Control}

# Variables for readability
RAW_DIR="~/GSE104550/raw"
TRIM_DIR="~/GSE104550/trimmed"
THREADS=12

#=============================================
# 2.1: Process CTCF CUT&RUN sample
#=============================================
echo "Processing CTCF CUT&RUN sample..."

trim_galore \
    --fastqc \                      # Run FastQC after trimming
    --paired \                      # Data is paired-end
    --cores ${THREADS} \            # Use multiple CPU cores
    --quality 20 \                  # Trim low-quality bases (Q<20)
    --stringency 3 \                # Require at least 3bp overlap with adapter
    --length 20 \                   # Discard reads shorter than 20bp
    --basename CTCF_CUT_Run \       # Output file prefix
    --output_dir ${TRIM_DIR}/CTCF \ # Output directory
    ${RAW_DIR}/CTCF_CUT_Run_R1.fastq.gz \
    ${RAW_DIR}/CTCF_CUT_Run_R2.fastq.gz

#=============================================
# 2.2: Process Control CUT&RUN sample
#=============================================
echo "Processing Control CUT&RUN sample..."

trim_galore \
    --fastqc \                      # Run FastQC after trimming
    --paired \                      # Data is paired-end
    --cores ${THREADS} \            # Use multiple CPU cores
    --quality 20 \                  # Trim low-quality bases (Q<20)
    --stringency 3 \                # Require at least 3bp overlap with adapter
    --length 20 \                   # Discard reads shorter than 20bp
    --basename Control_CUT_Run \    # Output file prefix
    --output_dir ${TRIM_DIR}/Control \ # Output directory
    ${RAW_DIR}/Control_CUT_Run_R1.fastq.gz \
    ${RAW_DIR}/Control_CUT_Run_R2.fastq.gz

echo "Quality control and adapter trimming complete!"
```

### Step 2: Align Reads to the Reference Genome

We’ll use BWA-MEM for alignment with parameters optimized for CUT&RUN/Tag data characteristics:

```bash
#-----------------------------------------------
# STEP 3: Align reads to the reference genome
#-----------------------------------------------

# Create directories for alignment files
mkdir -p ~/GSE104550/alignment/{CTCF,Control}

# Variables for readability
INDEX="~/BWA_Index_hg38/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4.fa"
TRIM_DIR="~/GSE104550/trimmed"
ALIGN_DIR="~/GSE104550/alignment"
THREADS=16

#=============================================
# 3.1: Align CTCF CUT&RUN sample
#=============================================
echo "Aligning CTCF CUT&RUN reads to reference genome..."

bwa mem \
    -t ${THREADS} \                 # Number of threads
    -M \                            # Mark shorter split hits as secondary (Picard compatibility)
    -R "@RG\tID:CTCF_CUT_Run\tSM:CTCF_CUT_Run\tPL:ILLUMINA" \  # Read group information
    ${INDEX} \                      # Reference genome
    ${TRIM_DIR}/CTCF/CTCF_CUT_Run_val_1.fq.gz \
    ${TRIM_DIR}/CTCF/CTCF_CUT_Run_val_2.fq.gz \
    > ${ALIGN_DIR}/CTCF/CTCF_CUT_Run.sam

#=============================================
# 3.2: Align Control CUT&RUN sample
#=============================================
echo "Aligning Control CUT&RUN reads to reference genome..."

bwa mem \
    -t ${THREADS} \                 # Number of threads
    -M \                            # Mark shorter split hits as secondary (Picard compatibility)
    -R "@RG\tID:Control_CUT_Run\tSM:Control_CUT_Run\tPL:ILLUMINA" \  # Read group information
    ${INDEX} \                      # Reference genome
    ${TRIM_DIR}/Control/Control_CUT_Run_val_1.fq.gz \
    ${TRIM_DIR}/Control/Control_CUT_Run_val_2.fq.gz \
    > ${ALIGN_DIR}/Control/Control_CUT_Run.sam

echo "Alignment complete!"
```

### Step 3: Process and Quality Control Alignments

After alignment, we convert, sort, filter, and quality control alignment files with special attention to CUT&RUN/Tag-specific characteristics:

```bash
#-----------------------------------------------
# STEP 4: Process and filter alignment files
#-----------------------------------------------

# Variables for readability
THREADS=16
ALIGN_DIR="~/GSE104550/alignment"
BLACKLIST="~/Genome_Index/ENCODE_BlackList/hg38-blacklist.v2.bed"

#=============================================
# 4.1: Process CTCF sample
#=============================================
echo "Processing CTCF CUT&RUN alignment..."

# Convert SAM to BAM
samtools view \
    -h -S -b \
    -@ ${THREADS} \
    -o ${ALIGN_DIR}/CTCF/CTCF_CUT_Run.bam \
    ${ALIGN_DIR}/CTCF/CTCF_CUT_Run.sam

# Sort BAM file
sambamba sort \
    -t ${THREADS} \
    -o ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_sorted.bam \
    ${ALIGN_DIR}/CTCF/CTCF_CUT_Run.bam

# Remove duplicates
picard MarkDuplicates \
    I=${ALIGN_DIR}/CTCF/CTCF_CUT_Run_sorted.bam \
    O=${ALIGN_DIR}/CTCF/CTCF_CUT_Run_dedup.bam \
    M=${ALIGN_DIR}/CTCF/CTCF_CUT_Run_dedup_metrics.txt \
    REMOVE_DUPLICATES=true \
    VALIDATION_STRINGENCY=LENIENT

# Filter for high-quality alignments and remove blacklisted regions
sambamba view \
    -h -f bam \
    -F "mapping_quality >= 30 and not duplicate" \
    -t ${THREADS} \
    ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_dedup.bam \
    -o ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_filtered.bam

# Remove blacklisted regions
bedtools intersect \
    -v \
    -abam ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_filtered.bam \
    -b ${BLACKLIST} \
    > ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_final.bam

# Index final BAM
samtools index ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_final.bam

#=============================================
# 4.2: Process Control sample (same steps)
#=============================================
echo "Processing Control CUT&RUN alignment..."

# Convert SAM to BAM
samtools view \
    -h -S -b \
    -@ ${THREADS} \
    -o ${ALIGN_DIR}/Control/Control_CUT_Run.bam \
    ${ALIGN_DIR}/Control/Control_CUT_Run.sam

# Sort BAM file
sambamba sort \
    -t ${THREADS} \
    -o ${ALIGN_DIR}/Control/Control_CUT_Run_sorted.bam \
    ${ALIGN_DIR}/Control/Control_CUT_Run.bam

# Remove duplicates
picard MarkDuplicates \
    I=${ALIGN_DIR}/Control/Control_CUT_Run_sorted.bam \
    O=${ALIGN_DIR}/Control/Control_CUT_Run_dedup.bam \
    M=${ALIGN_DIR}/Control/Control_CUT_Run_dedup_metrics.txt \
    REMOVE_DUPLICATES=true \
    VALIDATION_STRINGENCY=LENIENT

# Filter for high-quality alignments
sambamba view \
    -h -f bam \
    -F "mapping_quality >= 30 and not duplicate" \
    -t ${THREADS} \
    ${ALIGN_DIR}/Control/Control_CUT_Run_dedup.bam \
    -o ${ALIGN_DIR}/Control/Control_CUT_Run_filtered.bam

# Remove blacklisted regions
bedtools intersect \
    -v \
    -abam ${ALIGN_DIR}/Control/Control_CUT_Run_filtered.bam \
    -b ${BLACKLIST} \
    > ${ALIGN_DIR}/Control/Control_CUT_Run_final.bam

# Index final BAM
samtools index ${ALIGN_DIR}/Control/Control_CUT_Run_final.bam

#=============================================
# 4.3: Remove mitochondrial reads (important for CUT&RUN/Tag)
#=============================================
echo "Removing mitochondrial reads..."

# CUT&RUN/Tag experiments often have high mitochondrial DNA content
# Filter out mitochondrial reads for both samples
samtools view \
    -h -b \
    -@ ${THREADS} \
    ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_final.bam \
    $(echo $(seq 1 22) X Y | tr ' ' '\n' | sed 's/^/chr/') \
    > ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_no_mito.bam

samtools view \
    -h -b \
    -@ ${THREADS} \
    ${ALIGN_DIR}/Control/Control_CUT_Run_final.bam \
    $(echo $(seq 1 22) X Y | tr ' ' '\n' | sed 's/^/chr/') \
    > ${ALIGN_DIR}/Control/Control_CUT_Run_no_mito.bam

# Index the mitochondria-free BAM files
samtools index ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_no_mito.bam
samtools index ${ALIGN_DIR}/Control/Control_CUT_Run_no_mito.bam

echo "Alignment processing complete!"

# Clean up intermediate files to save space
rm ${ALIGN_DIR}/CTCF/CTCF_CUT_Run.sam ${ALIGN_DIR}/CTCF/CTCF_CUT_Run.bam
rm ${ALIGN_DIR}/Control/Control_CUT_Run.sam ${ALIGN_DIR}/Control/Control_CUT_Run.bam
```

### Step 4: Peak Calling with MACS2

MACS2 can be used for CUT&RUN/Tag data with adjusted parameters. We’ll use the narrow peak calling method:

```bash
#-----------------------------------------------
# STEP 5: Peak calling with MACS2
#-----------------------------------------------

# Create directory for MACS2 analysis
mkdir -p ~/GSE104550/peaks/MACS2

# Variables
ALIGN_DIR="~/GSE104550/alignment"
PEAKS_DIR="~/GSE104550/peaks"

#=============================================
# 5.1: Call narrow peaks using MACS2
#=============================================
echo "Calling narrow peaks with MACS2..."

macs2 callpeak \
    -t ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_no_mito.bam \     # Treatment sample
    -c ${ALIGN_DIR}/Control/Control_CUT_Run_no_mito.bam \ # Control sample
    -f BAMPE \                                           # Paired-end BAM format
    -g hs \                                              # Human genome size
    -n CTCF_CUT_Run_narrow \                             # Output prefix
    --outdir ${PEAKS_DIR}/MACS2 \                        # Output directory
    -q 0.01 \                                            # q-value (FDR) cutoff
    --keep-dup all \                                     # Keep duplicates (already removed)
    --call-summits                                       # Call peak summits

echo "MACS2 narrow peak calling complete!"
```

![MACS2 Peaks](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-24-at-2.12.35%E2%80%AFPM.png?resize=1024%2C172&ssl=1)

### Step 5: Peak Calling with SEACR

SEACR is specifically designed for CUT&RUN/Tag data and excels with the low background characteristic of these methods. We’ll use normalized control as the primary approach:

```bash
#-----------------------------------------------
# STEP 6: Peak calling with SEACR
#-----------------------------------------------

# Create directory for SEACR analysis
mkdir -p ~/GSE104550/peaks/SEACR

# Variables
ALIGN_DIR="~/GSE104550/alignment"
PEAKS_DIR="~/GSE104550/peaks"

#=============================================
# 6.1: Convert BAM files to bedgraph format for SEACR
#=============================================
echo "Converting BAM files to bedgraph format..."

# Convert CTCF sample to bedgraph
bedtools genomecov \
    -ibam ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_no_mito.bam \
    -bg \
    -pc \
    > ${PEAKS_DIR}/SEACR/CTCF_CUT_Run.bedgraph

# Convert Control sample to bedgraph
bedtools genomecov \
    -ibam ${ALIGN_DIR}/Control/Control_CUT_Run_no_mito.bam \
    -bg \
    -pc \
    > ${PEAKS_DIR}/SEACR/Control_CUT_Run.bedgraph

# Sort bedgraph files (required by SEACR)
sort -k1,1 -k2,2n ${PEAKS_DIR}/SEACR/CTCF_CUT_Run.bedgraph > ${PEAKS_DIR}/SEACR/CTCF_CUT_Run_sorted.bedgraph
sort -k1,1 -k2,2n ${PEAKS_DIR}/SEACR/Control_CUT_Run.bedgraph > ${PEAKS_DIR}/SEACR/Control_CUT_Run_sorted.bedgraph

#=============================================
# 6.2: Call peaks using SEACR with normalized control
#=============================================
echo "Calling peaks with SEACR using normalized control..."

# SEACR with normalized control sample (RECOMMENDED)
SEACR_1.3.sh \
    ${PEAKS_DIR}/SEACR/CTCF_CUT_Run_sorted.bedgraph \    # Treatment bedgraph
    ${PEAKS_DIR}/SEACR/Control_CUT_Run_sorted.bedgraph \ # Control bedgraph
    norm \                                               # Use normalized control
    relaxed \                                            # Relaxed threshold
    ${PEAKS_DIR}/SEACR/CTCF_SEACR_control                # Output prefix

#=============================================
# 6.3: Call peaks using SEACR with numeric threshold
#=============================================
echo "Calling peaks with SEACR using numeric threshold..."

# SEACR with numeric threshold (USE WHEN: no control sample available, 
# or control sample is of poor quality, or for exploratory analysis)
SEACR_1.3.sh \
    ${PEAKS_DIR}/SEACR/CTCF_CUT_Run_sorted.bedgraph \    # Treatment bedgraph
    0.01 \                                               # Numeric threshold (top 1%)
    non \                                                # No Control mode
    relaxed \                                            # Relaxed threshold
    ${PEAKS_DIR}/SEACR/CTCF_SEACR_numeric                # Output prefix

echo "SEACR peak calling complete!"
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-24-at-2.14.32%E2%80%AFPM.png?resize=972%2C228&ssl=1)

### Step 6: Peak Annotation and Functional Analysis

Annotate peaks with genomic features and nearby genes using HOMER:

```bash
#-----------------------------------------------
# STEP 7: Peak annotation with HOMER
#-----------------------------------------------

# Create directory for annotation results
mkdir -p ~/GSE104550/annotation

# Variables
PEAKS_DIR="~/GSE104550/peaks"
ANNOT_DIR="~/GSE104550/annotation"

#=============================================
# 7.1: Annotate MACS2 narrow peaks
#=============================================
echo "Annotating MACS2 narrow peaks..."

annotatePeaks.pl \
    ${PEAKS_DIR}/MACS2/CTCF_CUT_Run_narrow_peaks.narrowPeak \
    hg38 \
    -go ${ANNOT_DIR}/MACS2_GO \
    -genomeOntology ${ANNOT_DIR}/MACS2_genomeOntology \
    > ${ANNOT_DIR}/CTCF_MACS2_narrow_annotated.txt

#=============================================
# 7.2: Annotate SEACR peaks
#=============================================
echo "Annotating SEACR peaks..."

# Annotate SEACR normalized control peaks
annotatePeaks.pl \
    ${PEAKS_DIR}/SEACR/CTCF_SEACR_control.relaxed.bed \
    hg38 \
    -go ${ANNOT_DIR}/SEACR_control_GO \
    -genomeOntology ${ANNOT_DIR}/SEACR_control_genomeOntology \
    > ${ANNOT_DIR}/CTCF_SEACR_control_annotated.txt

# Annotate SEACR numeric peaks
annotatePeaks.pl \
    ${PEAKS_DIR}/SEACR/CTCF_SEACR_numeric.relaxed.bed \
    hg38 \
    -go ${ANNOT_DIR}/SEACR_numeric_GO \
    -genomeOntology ${ANNOT_DIR}/SEACR_numeric_genomeOntology \
    > ${ANNOT_DIR}/CTCF_SEACR_numeric_annotated.txt

echo "Peak annotation complete!"
```

![MACS2 Peaks Annotation](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-24-at-2.16.33%E2%80%AFPM.png?resize=1024%2C81&ssl=1)

![SEACR Peaks Annotation](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-24-at-2.17.31%E2%80%AFPM.png?resize=1024%2C80&ssl=1)

### Step 7: Peak Visualization

Create visualization files for genome browsers:

```bash
#-----------------------------------------------
# STEP 8: Create visualization files
#-----------------------------------------------

# Create directory for visualization files
mkdir -p ~/GSE104550/visualization

# Variables
ALIGN_DIR="~/GSE104550/alignment"
PEAKS_DIR="~/GSE104550/peaks"
VIS_DIR="~/GSE104550/visualization"

#=============================================
# 8.1: Create bigWig tracks for signal visualization
#=============================================
echo "Creating bigWig tracks..."

# Create bigWig files using deepTools
bamCoverage \
    --bam ${ALIGN_DIR}/CTCF/CTCF_CUT_Run_no_mito.bam \
    --outFileName ${VIS_DIR}/CTCF_CUT_Run.bw \
    --outFileFormat bigwig \
    --binSize 10 \
    --normalizeUsing RPGC \
    --effectiveGenomeSize 2913022398 \
    --extendReads \
    --centerReads \
    --numberOfProcessors 12

bamCoverage \
    --bam ${ALIGN_DIR}/Control/Control_CUT_Run_no_mito.bam \
    --outFileName ${VIS_DIR}/Control_CUT_Run.bw \
    --outFileFormat bigwig \
    --binSize 10 \
    --normalizeUsing RPGC \
    --effectiveGenomeSize 2913022398 \
    --extendReads \
    --centerReads \
    --numberOfProcessors 12

#=============================================
# 8.2: Create normalized signal tracks
#=============================================
echo "Creating normalized signal comparison tracks..."

# Create log2 ratio track (CTCF vs Control)
bigwigCompare \
    --bigwig1 ${VIS_DIR}/CTCF_CUT_Run.bw \
    --bigwig2 ${VIS_DIR}/Control_CUT_Run.bw \
    --outFileName ${VIS_DIR}/CTCF_vs_Control_log2ratio.bw \
    --operation log2 \
    --pseudocount 1 \
    --binSize 10 \
    --numberOfProcessors 12

#=============================================
# 8.3: Convert peaks to BED format for easy sharing
#=============================================
echo "Converting peaks to standard BED format..."

# Convert MACS2 narrowPeak to BED
awk 'OFS="\t" {print $1, $2, $3, "Peak_"NR, $8}' \
    ${PEAKS_DIR}/MACS2/CTCF_CUT_Run_narrow_peaks.narrowPeak \
    > ${VIS_DIR}/CTCF_MACS2_narrow_peaks.bed

# SEACR peaks are already in BED format
cp ${PEAKS_DIR}/SEACR/CTCF_SEACR_control.relaxed.bed ${VIS_DIR}/CTCF_SEACR_peaks.bed

#=============================================
# 8.4: Create peak heatmaps and profiles
#=============================================
echo "Creating peak heatmaps and profiles..."

# Compute matrix for heatmap around peak centers
computeMatrix reference-point \
    --referencePoint center \
    --beforeRegionStartLength 2000 \
    --afterRegionStartLength 2000 \
    --binSize 10 \
    --regionsFileName ${VIS_DIR}/CTCF_MACS2_narrow_peaks.bed \
    --scoreFileName ${VIS_DIR}/CTCF_CUT_Run.bw ${VIS_DIR}/Control_CUT_Run.bw \
    --outFileName ${VIS_DIR}/CTCF_peak_matrix.gz \
    --numberOfProcessors 12

# Plot heatmap
plotHeatmap \
    --matrixFile ${VIS_DIR}/CTCF_peak_matrix.gz \
    --outFileName ${VIS_DIR}/CTCF_peak_heatmap.png \
    --samplesLabel "CTCF CUT&RUN" "Control CUT&RUN" \
    --regionsLabel "CTCF Peaks" \
    --plotTitle "CUT&RUN Signal at CTCF Binding Sites" \
    --colorMap Blues \
    --whatToShow 'heatmap and colorbar'

# Plot profile
plotProfile \
    --matrixFile ${VIS_DIR}/CTCF_peak_matrix.gz \
    --outFileName ${VIS_DIR}/CTCF_peak_profile.png \
    --samplesLabel "CTCF CUT&RUN" "Control CUT&RUN" \
    --regionsLabel "CTCF Peaks" \
    --plotTitle "Average CUT&RUN Signal at CTCF Binding Sites" \
    --colors blue red

echo "Visualization files created successfully!"
```

![CTCF Cut&Run Peaks IGV bigWig](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-24-at-2.47.46%E2%80%AFPM.png?resize=1024%2C223&ssl=1)

![CTCF Peak Profile](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/CTCF_peak_profile.png?resize=1024%2C326&ssl=1)

![CTCF Cut&Run Peaks Heat Map](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/CTCF_peak_heatmap.png?resize=1024%2C324&ssl=1)

### Step 8: Motif Analysis and Advanced Functional Analysis

Discover DNA sequence motifs within your CUT&RUN/Tag peaks:

```bash
#-----------------------------------------------
# STEP 9: Motif analysis with HOMER
#-----------------------------------------------

# Create directory for motif analysis
mkdir -p ~/GSE104550/motifs

# Variables
PEAKS_DIR="~/GSE104550/peaks"
MOTIF_DIR="~/GSE104550/motifs"

#=============================================
# 9.1: De novo motif discovery
#=============================================
echo "Performing de novo motif discovery..."

# Find motifs in MACS2 narrow peaks
findMotifsGenome.pl \
    ${PEAKS_DIR}/MACS2/CTCF_CUT_Run_narrow_peaks.narrowPeak \
    hg38 \
    ${MOTIF_DIR}/MACS2_narrow \
    -size 200 \
    -len 8,10,12,15 \
    -S 25 \
    -mask \
    -fdr 0.05

# Find motifs in SEACR peaks
findMotifsGenome.pl \
    ${PEAKS_DIR}/SEACR/CTCF_SEACR_control.relaxed.bed \
    hg38 \
    ${MOTIF_DIR}/SEACR_control \
    -size 200 \
    -len 8,10,12,15 \
    -S 25 \
    -mask \
    -fdr 0.05

echo "Motif analysis complete!"
```

![CTCF Cut&Run Peaks Motif Analysis](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/05/Screenshot-2025-05-24-at-2.22.37%E2%80%AFPM.png?resize=1024%2C250&ssl=1)

## Best Practices for CUT&RUN/Tag Analysis

### Experimental Quality Control

**Pre-Analysis Checks:**

- Verify adequate sequencing depth (>10 million reads per sample minimum)

- Check fragment size distribution for expected patterns

- Assess mitochondrial DNA content (can be high in CUT&RUN/Tag)

- Evaluate library complexity and duplication rates

**Method-Specific Considerations:**

- **CUT&RUN:** Expect cleaner background but may require optimization of MNase concentration

- **CUT&Tag:** Generally more robust but may show broader fragment size distribution

- **Both methods:** Mitochondrial filtering is often crucial for data quality

### Computational Analysis Guidelines

**Peak Calling Strategy:**

- Use both MACS2 and SEACR for comprehensive analysis

- SEACR is specifically designed for CUT&RUN/Tag’s low background

- Consider using control samples when available

- Apply appropriate FDR thresholds (0.01 or stricter)

**Quality Metrics to Monitor:**

- **Signal-to-noise ratio:** Should be excellent with CUT&RUN/Tag

- **Peak reproducibility:** Compare biological replicates using IDR

- **Fragment size distribution:** Should reflect factor-specific binding patterns

- **Genomic distribution:** Peaks should align with expected factor localization

### Data Interpretation Guidelines

**Peak Validation:**

- Focus on high-confidence peaks with strong statistical support

- Validate key findings with orthogonal methods (qPCR, ChIP-seq)

- Compare with published datasets for well-studied factors

- Consider biological context when interpreting peak locations

**Motif Analysis:**

- CUT&RUN/Tag should produce high-quality motifs due to precise binding site mapping

- Compare discovered motifs with known factor preferences

- Examine motif positioning relative to peak summits

- Consider co-factor binding patterns

## Troubleshooting Common CUT&RUN/Tag Issues

### Low Peak Counts

**Problem:** Fewer peaks than expected compared to literature or ChIP-seq.

**Solutions:**

- Check antibody quality and specificity

- Verify cell viability and treatment conditions

- Adjust peak calling parameters (less stringent thresholds)

- Increase sequencing depth if library complexity allows

- Compare different peak callers (MACS2 vs SEACR)

### High Mitochondrial Content

**Problem:** Excessive mitochondrial DNA reads (>50% of total).

**Solutions:**

- Implement mitochondrial filtering during analysis

- Optimize cell permeabilization conditions in future experiments

- Consider deeper sequencing to compensate for lost reads

- Use mitochondrial depletion kits during library preparation

### Poor Fragment Size Distribution

**Problem:** Fragment sizes don’t match expected patterns for your factor.

**Solutions:**

- Check enzyme activity (MNase for CUT&RUN, Tn5 for CUT&Tag)

- Optimize incubation conditions and concentrations

- Verify proper cell permeabilization

- Consider factor-specific binding characteristics

### Inconsistent Replicates

**Problem:** Poor correlation between biological replicates.

**Solutions:**

- Use IDR (Irreproducible Discovery Rate) to identify reproducible peaks

- Check for technical issues in problematic samples

- Increase stringency of peak calling to focus on most confident peaks

- Consider pooling replicates if experimental design permits

### High Background Signal

**Problem:** Elevated background despite CUT&RUN/Tag’s typically clean profile.

**Solutions:**

- Ensure proper washing steps during experimental protocol

- Check antibody specificity with western blot or other validation

- Use more stringent peak calling parameters

- Implement additional filtering steps (blacklisted regions, low mappability)

## Conclusion and Next Steps

You’ve successfully completed a comprehensive CUT&RUN/Tag analysis pipeline, transforming raw sequencing data into high-quality, annotated binding site maps. This analysis demonstrates the power and precision of CUT&RUN/Tag methods for chromatin profiling.

The precision and efficiency of CUT&RUN/Tag methods, combined with these analytical approaches, represent powerful tools for advancing our understanding of genome regulation in health and disease. As these methods continue to mature and find new applications, their impact on molecular biology research promises to be profound and lasting.

## References

1. Kaya-Okur, H.S., Janssens, D.H., Henikoff, J.G. *et al.* Efficient low-cost chromatin profiling with CUT&Tag. *Nat Protoc* **15**, 3264–3283 (2020). https://doi.org/10.1038/s41596-020-0373-x

2. Skene, P.J., and Henikoff, S. (2017). An efficient targeted nuclease strategy for high-resolution mapping of DNA binding sites. *eLife* 6, e21856.

3. Meers, M.P., Bryson, T.D., Henikoff, J.G., and Henikoff, S. (2019). Improved CUT&RUN chromatin profiling tools. *eLife* 8, e46314.

4. Kaya-Okur, H.S., Janssens, D.H., Henikoff, J.G., Ahmad, K., and Henikoff, S. (2020). Efficient low-cost chromatin profiling with CUT&Tag. *Nature Protocols* 15, 3264–3283.

5. Zheng, Y., and Gehring, M. (2019). Low-input chromatin profiling in Arabidopsis endosperm using CUT&RUN. *Plant Reproduction* 32, 63–75.

---

*This tutorial is part of the NGS101.com beginner’s guide to next-generation sequencing analysis. The methods and analysis approaches covered here represent current best practices for CUT&RUN/Tag data analysis. As the field continues to evolve, we recommend staying current with the latest literature and method developments.*
