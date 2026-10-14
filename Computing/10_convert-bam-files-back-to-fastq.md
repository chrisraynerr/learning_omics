# How to Convert BAM Files Back to FASTQ Files: A Practical Guide for NGS Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Quick Tips
- **Source:** [https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/)

- [Introduction: When and Why You Need BAM-to-FASTQ Conversion](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Introduction_When_and_Why_You_Need_BAM-to-FASTQ_Conversion)

  - [The NGS Data Conversion Challenge](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#The_NGS_Data_Conversion_Challenge)
  - [Why Do Data Repositories Store NGS Data in BAM Format?](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Why_Do_Data_Repositories_Store_NGS_Data_in_BAM_Format)
  - [Common Scenarios Requiring BAM-to-FASTQ Conversion](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Common_Scenarios_Requiring_BAM-to-FASTQ_Conversion)

- [Understanding BAM and FASTQ File Formats](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Understanding_BAM_and_FASTQ_File_Formats)

  - [Quick Summary for This Tutorial](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Quick_Summary_for_This_Tutorial)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Setting_Up_Your_Analysis_Environment)
- [Determining if Your BAM is Single-End or Paired-End](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Determining_if_Your_BAM_is_Single-End_or_Paired-End)

  - [Quick Flag Check Method](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Quick_Flag_Check_Method)
  - [Understanding the Check](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Understanding_the_Check)

- [Method 1: Using Samtools (Recommended for Most Cases)](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Method_1_Using_Samtools_Recommended_for_Most_Cases)

  - [Converting Single-End BAM to FASTQ](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Converting_Single-End_BAM_to_FASTQ)
  - [Converting Paired-End BAM to FASTQ](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Converting_Paired-End_BAM_to_FASTQ)
  - [Advanced Samtools Options](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Advanced_Samtools_Options)

- [Method 2: Using bedtools bamtofastq](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Method_2_Using_bedtools_bamtofastq)

  - [Converting Paired-End BAM with bedtools](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Converting_Paired-End_BAM_with_bedtools)
  - [Converting Single-End BAM with bedtools](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Converting_Single-End_BAM_with_bedtools)
  - [Why Choose bedtools?](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Why_Choose_bedtools)

- [Method 3: Using Picard SamToFastq](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Method_3_Using_Picard_SamToFastq)

  - [Converting Paired-End BAM with Picard](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Converting_Paired-End_BAM_with_Picard)
  - [Converting Single-End BAM with Picard](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Converting_Single-End_BAM_with_Picard)
  - [Advanced Picard Options](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Advanced_Picard_Options)
  - [Why Choose Picard?](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Why_Choose_Picard)

- [Tool Comparison and Recommendations](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Tool_Comparison_and_Recommendations)

  - [Performance Comparison](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Performance_Comparison)
  - [My Recommendation for Beginners](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#My_Recommendation_for_Beginners)

- [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Common_Pitfalls_and_How_to_Avoid_Them)

  - [Pitfall 1: Not Sorting BAM by Read Name for Paired-End Data](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Pitfall_1_Not_Sorting_BAM_by_Read_Name_for_Paired-End_Data)
  - [Pitfall 2: Mixing Up Single-End and Paired-End Syntax](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Pitfall_2_Mixing_Up_Single-End_and_Paired-End_Syntax)
  - [Pitfall 3: Ignoring Disk Space Requirements](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Pitfall_3_Ignoring_Disk_Space_Requirements)
  - [Pitfall 4: Losing Read Quality Scores](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Pitfall_4_Losing_Read_Quality_Scores)
  - [Pitfall 5: Not Handling Unmapped and Singleton Reads Properly](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Pitfall_5_Not_Handling_Unmapped_and_Singleton_Reads_Properly)
  - [Pitfall 6: Incorrect Handling of Multi-Sample BAMs](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Pitfall_6_Incorrect_Handling_of_Multi-Sample_BAMs)
  - [Pitfall 7: Forgetting to Validate Output](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Pitfall_7_Forgetting_to_Validate_Output)
  - [Recommended Workflow](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Recommended_Workflow)
  - [Next Steps](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Next_Steps)

- [References](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-convert-bam-files-back-to-fastq-files-a-practical-guide-for-ngs-analysis/#Like_this)

## Introduction: When and Why You Need BAM-to-FASTQ Conversion

### The NGS Data Conversion Challenge

In next-generation sequencing (NGS) analysis, you’ll encounter data in different formats depending on where you are in your workflow. Sometimes you need to convert between these formats, particularly from **BAM** (aligned reads) back to **FASTQ** (raw sequencing reads).

### Why Do Data Repositories Store NGS Data in BAM Format?

Many public genomics databases—like **NCBI SRA (Sequence Read Archive)**, **ENA (European Nucleotide Archive)**, and **DDBJ (DNA Data Bank of Japan)**—store sequencing data in BAM or CRAM format rather than FASTQ. Here’s why:

**Storage Efficiency:**

- BAM files are **compressed binary format** (typically 20-30% smaller than FASTQ)

- CRAM format can be **40-60% smaller** than BAM with reference-based compression

- This saves massive amounts of storage space across millions of datasets

**Standardization:**

- BAM format includes **standardized metadata** (sample info, library prep, alignment parameters)

- Ensures data can be **easily shared** across different analysis platforms

- Maintains **provenance** of how data was generated and processed

**Data Integrity:**

- BAM files include **quality scores** and **alignment information** in a structured format

- Built-in **indexing** (BAI files) enables fast data retrieval

- Reduced risk of data corruption compared to plain text FASTQ files

**Example – NCBI SRA:**When you download data from NCBI’s SRA database, you’ll get SRA files that are converted to BAM/CRAM format. To use this data with tools expecting FASTQ input (like alignment tools, quality control software, or re-analysis pipelines), you need to convert BAM back to FASTQ.

### Common Scenarios Requiring BAM-to-FASTQ Conversion

You’ll need to convert BAM files to FASTQ in these situations:

**1. Re-analyzing Public Data with Different Parameters**

- Downloaded aligned BAM files from a repository

- Want to realign with updated genome reference or different aligner

- Need to apply your own quality control thresholds

**2. Extracting Specific Reads for Focused Analysis**

- Extract unmapped reads for de novo assembly

- Pull out reads from a specific genomic region

- Subset data for method testing or validation

**3. Pipeline Compatibility Requirements**

- Your analysis pipeline requires FASTQ input

- Integration with tools that don’t accept BAM format

- Sharing data with collaborators using FASTQ-based workflows

**4. Quality Control and Reprocessing**

- Re-running quality control from raw reads

- Applying different adapter trimming strategies

- Merging data from multiple sequencing runs

> 

**Beginner’s Note:** While it might seem counterintuitive to convert aligned data back to raw reads, this is a common and necessary step in many bioinformatics workflows. Think of it like converting a formatted document back to plain text when you need to re-process it with different formatting rules.

---

## Understanding BAM and FASTQ File Formats

Before converting between formats, you should understand what these files contain and when to use each. For a comprehensive explanation of NGS file formats, including detailed coverage of FASTQ and BAM structures, see:

**[The Complete Guide to NGS Data Types and Formats – From Raw Reads to Analysis-Ready Files](https://ngs101.com/the-complete-guide-to-ngs-data-types-and-formats-from-raw-reads-to-analysis-ready-files/)**

### Quick Summary for This Tutorial

**FASTQ files:**

- Text format containing raw sequencing reads

- Includes sequences and quality scores

- **Use for:** Initial QC, alignment input, adapter trimming

**BAM files:**

- Binary compressed format with aligned reads

- Includes alignment information and metadata

- **Use for:** Storage, variant calling, genome visualization

**The key difference:** FASTQ is for pre-alignment workflows, BAM is for post-alignment analysis. Converting BAM back to FASTQ lets you re-run alignment with different parameters or tools.

---

## Setting Up Your Analysis Environment

We’ll install all three conversion tools (samtools, bedtools, Picard) in a single conda environment for easy management.

```bash
#-----------------------------------------------
# Create dedicated conda environment with all tools
#-----------------------------------------------

# Create environment with all required tools
conda create -n bam_conversion \
    -c bioconda -c conda-forge \
    samtools bedtools picard

# Activate the environment
conda activate bam_conversion

# Verify installations
echo "Checking installed versions:"
samtools --version | head -n 1
bedtools --version
picard SamToFastq --version 2>&1 | head -n 1
```

## Determining if Your BAM is Single-End or Paired-End

Before converting your BAM file, you **must** determine whether it contains single-end or paired-end data. Using the wrong conversion method will result in errors or incorrect output.

### Quick Flag Check Method

This is the most reliable and easiest method:

```bash
#-----------------------------------------------
# Check if BAM contains single-end or paired-end data
#-----------------------------------------------

# Test if bit 1 (0x1 = paired) is set in SAM flags
samtools view input.bam | head -n 100 | \
  awk '{if (and($2, 1)) print "PAIRED"; else print "SINGLE"}' | \
  sort | uniq -c
```

**Output interpretation:**

**Paired-end data:**

```bash
100 PAIRED
# All 100 reads are paired-end → Your data is PAIRED-END
```

**Single-end data:**

```bash
100 SINGLE
# All 100 reads are single-end → Your data is SINGLE-END
```

**Mixed data (rare, usually indicates a problem):**

```bash
85 PAIRED
15 SINGLE
# 85% paired, 15% single → Something is wrong or unusual
```

### Understanding the Check

The SAM format uses a **FLAG** field (second column) that encodes multiple properties as bits:

- **Bit 0x1 (decimal value 1)**: Read is paired in sequencing

- The `and($2, 1)` function checks if this bit is set

- If set (=1), the read comes from paired-end sequencing

- If unset (=0), the read comes from single-end sequencing

> 

**Important:** Always run this check before conversion! The conversion commands for single-end and paired-end are different, and using the wrong one will fail or produce incorrect results.

---

## Method 1: Using Samtools (Recommended for Most Cases)

**Samtools** is the most widely used tool for BAM file manipulation and is recommended for most BAM-to-FASTQ conversions due to its reliability, speed, and extensive options.

### Converting Single-End BAM to FASTQ

For single-end sequencing data (one read per fragment):

```bash
#-----------------------------------------------
# STEP 1: Convert single-end BAM to FASTQ with samtools
#-----------------------------------------------

# Basic conversion
samtools fastq input.bam > output.fastq

# With gzip compression (recommended to save space)
samtools fastq input.bam | gzip > output.fastq.gz

# Sort by read name first (important for paired-end data, optional for single-end)
samtools sort -n input.bam -o sorted_by_name.bam
samtools fastq sorted_by_name.bam | gzip > output.fastq.gz
```

**Command breakdown:**

- `samtools fastq`: Command to convert BAM to FASTQ

- `input.bam`: Your BAM file

- `> output.fastq`: Redirect output to FASTQ file

- `| gzip`: Compress output on-the-fly to save disk space

### Converting Paired-End BAM to FASTQ

For paired-end sequencing data (R1 and R2 reads):

```bash
#-----------------------------------------------
# STEP 2: Convert paired-end BAM to FASTQ with samtools
#-----------------------------------------------

# CRITICAL: BAM must be sorted by read name for paired-end conversion
# If your BAM is sorted by coordinate, you must re-sort first:

# Step 1: Sort by read name (queryname)
samtools sort -n input.bam -o sorted_by_name.bam

# Step 2: Convert to paired FASTQ files
samtools fastq \
    -1 output_R1.fastq.gz \
    -2 output_R2.fastq.gz \
    -0 /dev/null \
    -s /dev/null \
    -n \
    sorted_by_name.bam

# Verify output files
echo "R1 reads:"
zcat output_R1.fastq.gz | wc -l | awk '{print $1/4 " reads"}'
echo "R2 reads:"
zcat output_R2.fastq.gz | wc -l | awk '{print $1/4 " reads"}'
```

**Command parameters explained:**

- `-n`: Sort by read name (required for paired-end)

- `-1 output_R1.fastq.gz`: First mate reads (forward/R1)

- `-2 output_R2.fastq.gz`: Second mate reads (reverse/R2)

- `-0 /dev/null`: Unpaired reads (discard if not needed)

- `-s /dev/null`: Singleton reads (discard if not needed)

> 

**Critical Note:** For paired-end data, the BAM **must** be sorted by read name, not by genomic coordinate. If your reads are out of order, you’ll get incomplete or incorrect FASTQ files.

### Advanced Samtools Options

For more complex scenarios:

```bash
#-----------------------------------------------
# STEP 3: Advanced samtools options
#-----------------------------------------------

# Extract only mapped reads
samtools fastq -F 4 input.bam | gzip > mapped_only.fastq.gz

# Extract only unmapped reads (useful for de novo assembly)
samtools fastq -f 4 input.bam | gzip > unmapped_only.fastq.gz

# Include quality scores in specific format (default: Sanger/Phred+33)
samtools fastq input.bam | gzip > output.fastq.gz

# Parallel processing for faster conversion (if you have multiple cores)
samtools fastq -@ 8 input.bam | gzip > output.fastq.gz

# For paired-end with separate unmapped reads file
samtools fastq \
    -1 output_R1.fastq.gz \
    -2 output_R2.fastq.gz \
    -0 unmapped.fastq.gz \
    -s singleton.fastq.gz \
    -n \
    sorted_by_name.bam
```

**Flag options:**

- `-F 4`: Exclude unmapped reads (only keep mapped)

- `-f 4`: Include only unmapped reads

- `-@ 8`: Use 8 threads for parallel processing

---

## Method 2: Using bedtools bamtofastq

**Bedtools** provides an alternative method that’s particularly useful when working with other bedtools operations in your pipeline.

### Converting Paired-End BAM with bedtools

```bash
#-----------------------------------------------
# STEP 4: Convert paired-end BAM with bedtools
#-----------------------------------------------

# CRITICAL: BAM must be sorted by read name
samtools sort -n input.bam -o sorted_by_name.bam

# Convert to paired FASTQ
bedtools bamtofastq \
    -i sorted_by_name.bam \
    -fq output_R1.fastq \
    -fq2 output_R2.fastq

# Compress output files
gzip output_R1.fastq output_R2.fastq
```

**Parameters:**

- `-i`: Input BAM file

- `-fq`: Output FASTQ for R1 (first mate)

- `-fq2`: Output FASTQ for R2 (second mate)

### Converting Single-End BAM with bedtools

```bash
#-----------------------------------------------
# STEP 5: Convert single-end BAM with bedtools
#-----------------------------------------------

bedtools bamtofastq \
    -i input.bam \
    -fq output.fastq

# Compress output
gzip output.fastq
```

### Why Choose bedtools?

**Advantages:**

- Simple, straightforward syntax

- Integrates well with other bedtools operations

- Handles paired-end data reliably

**Limitations:**

- Less flexible than samtools for filtering

- Fewer options for quality score manipulation

- Cannot directly output compressed files

---

## Method 3: Using Picard SamToFastq

**Picard** is a Java-based toolkit that provides robust BAM/SAM file processing with extensive validation.

### Converting Paired-End BAM with Picard

```bash
#-----------------------------------------------
# STEP 6: Convert paired-end BAM with Picard
#-----------------------------------------------

# Standard paired-end conversion with compression
# IMPORTANT: Picard determines compression by file extension
# Use .fastq.gz for compressed output, .fastq for uncompressed

picard SamToFastq \
    I=input.bam \
    FASTQ=output_R1.fastq.gz \
    SECOND_END_FASTQ=output_R2.fastq.gz \
    VALIDATION_STRINGENCY=LENIENT

# With additional options for quality control
picard SamToFastq \
    I=input.bam \
    FASTQ=output_R1.fastq.gz \
    SECOND_END_FASTQ=output_R2.fastq.gz \
    UNPAIRED_FASTQ=unpaired.fastq.gz \
    VALIDATION_STRINGENCY=SILENT \
    INCLUDE_NON_PF_READS=false
```

**Key parameters:**

- `I=`: Input BAM file

- `FASTQ=`: Output R1 FASTQ file

- `SECOND_END_FASTQ=`: Output R2 FASTQ file (for paired-end)

- `UNPAIRED_FASTQ=`: Output file for unpaired reads

- `VALIDATION_STRINGENCY=LENIENT`: Less strict validation (useful for problematic BAMs)

- `INCLUDE_NON_PF_READS=false`: Exclude reads that failed platform quality filters

### Converting Single-End BAM with Picard

```bash
#-----------------------------------------------
# STEP 7: Convert single-end BAM with Picard
#-----------------------------------------------

# Picard automatically compresses if you use .fastq.gz extension
picard SamToFastq \
    I=input.bam \
    FASTQ=output.fastq.gz \
    VALIDATION_STRINGENCY=LENIENT
```

### Advanced Picard Options

```bash
#-----------------------------------------------
# STEP 8: Advanced Picard conversion with quality filtering
#-----------------------------------------------

# Split output by read group (useful for multi-sample BAMs)
picard SamToFastq \
    I=input.bam \
    OUTPUT_PER_RG=true \
    OUTPUT_DIR=./fastq_by_readgroup/ \
    COMPRESS_OUTPUTS_PER_RG=true \ 
    VALIDATION_STRINGENCY=LENIENT

# Include additional read information in FASTQ headers
picard SamToFastq \
    I=input.bam \
    FASTQ=output_R1.fastq.gz \
    SECOND_END_FASTQ=output_R2.fastq.gz \
    INCLUDE_NON_PF_READS=false \
    CLIPPING_ATTRIBUTE=XT \
    CLIPPING_ACTION=2 \
    CLIPPING_MIN_LENGTH=30
```

**Advanced parameters:**

- `OUTPUT_PER_RG=true`: Create separate FASTQ files for each read group

- `OUTPUT_DIR=`: Directory for read group-separated outputs

- `COMPRESS_OUTPUTS_PER_RG=true`: Automatically gzip outputs when splitting by read group (only works with `OUTPUT_PER_RG=true`)

- `CLIPPING_ATTRIBUTE=XT`: Use specific SAM tag for read clipping

- `CLIPPING_MIN_LENGTH=30`: Minimum read length after clipping

### Why Choose Picard?

**Advantages:**

- Extensive validation and error checking

- Rich set of options for quality control

- Can split outputs by read group automatically

- Handles complex BAM structures well

**Limitations:**

- Requires Java (more memory overhead)

- Slower than samtools for large files

- More verbose syntax

---

## Tool Comparison and Recommendations

### Performance Comparison

Here’s a practical comparison based on typical use cases:

**Speed****Memory Usage****Ease of Use****Filtering Options****Validation****Parallel Processing**`-@`**Direct Compression**

| Feature | Samtools | bedtools | Picard |
| --- | --- | --- | --- |
|  | Fast | Moderate | Slower (Java overhead) |
|  | Low | Low | Higher (JVM) |
|  | Moderate | Easy | Complex syntax |
|  | Extensive | Limited | Very extensive |
|  | Good | Basic | Excellent |
|  | Yes () | No | No |
|  | Yes (pipe to gzip) | No | Yes (by file extension) |

### My Recommendation for Beginners

**Start with samtools** for these reasons:

1. **Most versatile** – handles 90% of use cases

2. **Best performance** – fastest for large files

3. **Most documentation** – easier to find help online

4. **Industry standard** – used in most published pipelines

5. **Parallel processing** – can utilize multiple CPU cores

**When to switch tools:**

- Use **bedtools** if you’re already using bedtools for other operations

- Use **Picard** when you need extensive validation or dealing with complex multi-sample BAMs

---

## Common Pitfalls and How to Avoid Them

### Pitfall 1: Not Sorting BAM by Read Name for Paired-End Data

**The Problem:**

```bash
# This will FAIL or produce incorrect output for paired-end data
samtools fastq -1 R1.fq -2 R2.fq input.bam  # BAM sorted by coordinate
```

**Error message:**

```bash
[W::sam_read1_sam] Parse error at line 147
[samopen] SAM header is present: 1 sequences.
samtools fastq: error reading file "input.bam"
```

**The Solution:**

```bash
# ALWAYS sort by read name first for paired-end
samtools sort -n input.bam -o sorted_by_name.bam
samtools fastq -1 R1.fq.gz -2 R2.fq.gz -0 /dev/null -s /dev/null -n sorted_by_name.bam
```

> 

**Why this happens:** Paired-end conversion requires R1 and R2 reads to appear consecutively in the file. Coordinate-sorted BAMs have reads ordered by genomic position, not by read pair.

### Pitfall 2: Mixing Up Single-End and Paired-End Syntax

**The Problem:**

```bash
# Using paired-end syntax on single-end data
samtools fastq -1 R1.fq -2 R2.fq single_end.bam  # Wrong!
```

**The Solution:**

```bash
# For single-end, use simple redirection
samtools fastq single_end.bam > output.fastq

# Or explicitly for paired-end only when you have paired data
samtools fastq -1 R1.fq -2 R2.fq paired_end_sorted.bam
```

**How to check if your data is paired-end:**

```bash
# Quick flag check - tests if bit 1 (0x1 = paired) is set
samtools view input.bam | head -n 100 | \
  awk '{if (and($2, 1)) print "PAIRED"; else print "SINGLE"}' | \
  sort | uniq -c

# Output for paired-end data:
#     100 PAIRED

# Output for single-end data:
#     100 SINGLE
```

**Understanding the check:**

- SAM FLAG bit 0x1 (value 1) indicates “read is paired in sequencing”

- The `and($2, 1)` function checks if this bit is set

- If most/all reads show “PAIRED”, your data is paired-end

- If most/all reads show “SINGLE”, your data is single-end

### Pitfall 3: Ignoring Disk Space Requirements

**The Problem:**FASTQ files can be **2-3x larger** than BAM files, especially when uncompressed.

**Example:**

- Input BAM: 50 GB

- Output uncompressed FASTQ (R1 + R2): 150-180 GB

- Output compressed FASTQ (R1 + R2): 60-80 GB

**The Solution:**

```bash
# Always use compression for output
samtools fastq input.bam | gzip > output.fastq.gz

# For paired-end, outputs are automatically compressed if you use .gz extension
samtools fastq -1 R1.fq.gz -2 R2.fq.gz -n sorted.bam

# Monitor disk space before starting
df -h .  # Check available space in current directory
```

### Pitfall 4: Losing Read Quality Scores

**The Problem:**Some BAM files don’t preserve original quality scores, especially if they’ve been through multiple processing steps.

**Check quality scores:**

```bash
# Examine quality score range in BAM
samtools view input.bam | head -n 1000 | awk '{print $11}' | less

# All quality scores showing as "!" means they're missing/uniform
```

**The Solution:**

```bash
# If quality scores are missing, you might need to get original FASTQ
# Or use a quality placeholder (not recommended for most analyses)

# For samtools, quality scores are preserved automatically when present
# If they're missing in BAM, they'll be missing in FASTQ too
```

> 

**Important:** If your downstream analysis requires quality scores (like variant calling with quality-based filtering), you **must** have quality information in your BAM. If it’s missing, you may need to obtain the original FASTQ files instead.

### Pitfall 5: Not Handling Unmapped and Singleton Reads Properly

**The Problem:**Forgetting to account for unpaired or unmapped reads can lead to data loss.

**The Solution:**

```bash
# Capture ALL reads explicitly
samtools fastq \
    -1 R1.fastq.gz \
    -2 R2.fastq.gz \
    -0 unpaired.fastq.gz \    # Reads with missing mate
    -s singleton.fastq.gz \   # Reads whose mate was filtered
    -n \
    sorted_by_name.bam

# Check how many reads went where
echo "Paired R1:"
zcat R1.fastq.gz | wc -l | awk '{print $1/4}'
echo "Paired R2:"
zcat R2.fastq.gz | wc -l | awk '{print $1/4}'
echo "Unpaired:"
zcat unpaired.fastq.gz | wc -l | awk '{print $1/4}'
echo "Singletons:"
zcat singleton.fastq.gz | wc -l | awk '{print $1/4}'
```

### Pitfall 6: Incorrect Handling of Multi-Sample BAMs

**The Problem:**Some BAM files contain reads from multiple samples (identified by read groups). Converting these directly will mix all samples together.

**Check for multiple samples:**

```bash
# Check read groups in BAM header
samtools view -H input.bam | grep '^@RG'

# If multiple @RG lines appear, you have multi-sample BAM
```

**The Solution:**

```bash
# Option 1: Split by read group first with samtools
for RG in $(samtools view -H input.bam | grep '^@RG' | sed 's/.*ID://g' | awk '{print $1}'); do
    samtools view -br ${RG} input.bam > ${RG}.bam
    samtools fastq -1 ${RG}_R1.fq.gz -2 ${RG}_R2.fq.gz -n ${RG}.bam
done

# Option 2: Use Picard's built-in read group splitting
picard SamToFastq \
    I=input.bam \
    OUTPUT_PER_RG=true \
    OUTPUT_DIR=./fastq_split/ \
    COMPRESS_OUTPUTS_PER_RG=true
```

### Pitfall 7: Forgetting to Validate Output

**The Problem:**Not checking if the conversion completed successfully or if read counts match expectations.

**The Solution:**

```bash
# Count reads in original BAM
echo "Reads in BAM:"
samtools view -c input.bam

# Count reads in output FASTQ
echo "Reads in FASTQ:"
zcat output.fastq.gz | wc -l | awk '{print $1/4}'

# For paired-end, both R1 and R2 should have same count
echo "R1 reads:"
zcat R1.fastq.gz | wc -l | awk '{print $1/4}'
echo "R2 reads:"
zcat R2.fastq.gz | wc -l | awk '{print $1/4}'

# Verify FASTQ format integrity
zcat output.fastq.gz | head -n 4
# Should see: @read_name, sequence, +, quality_scores
```

---

### Recommended Workflow

For most use cases, follow this workflow:

```bash
# 1. Check if BAM is single-end or paired-end (CRITICAL FIRST STEP)
samtools view input.bam | head -n 100 | \
  awk '{if (and($2, 1)) print "PAIRED"; else print "SINGLE"}' | \
  sort | uniq -c

# 2. Sort by read name (critical for paired-end)
samtools sort -n input.bam -o sorted_by_name.bam

# 3. Convert with samtools (parallel processing for speed)
# For PAIRED-END data:
samtools fastq -@ 8 \
    -1 output_R1.fastq.gz \
    -2 output_R2.fastq.gz \
    -0 /dev/null \
    -s /dev/null \
    -n \
    sorted_by_name.bam

# For SINGLE-END data:
samtools fastq -@ 8 sorted_by_name.bam | gzip > output.fastq.gz

# 4. Validate outputs
echo "R1 reads: $(zcat output_R1.fastq.gz | wc -l | awk '{print $1/4}')"
echo "R2 reads: $(zcat output_R2.fastq.gz | wc -l | awk '{print $1/4}')"
```

### Next Steps

Now that you can convert BAM to FASTQ, you can:

- **Re-align with updated references** using BWA, STAR, or HISAT2

- **Apply stringent quality control** with FastQC and Trimmomatic

- **Extract specific genomic regions** for targeted analysis

- **Integrate public datasets** into your analysis pipelines

---

## References

1. Li H, Handsaker B, Wysoker A, et al. The Sequence Alignment/Map format and SAMtools. *Bioinformatics*. 2009;25(16):2078-2079. doi:10.1093/bioinformatics/btp352

2. Quinlan AR, Hall IM. BEDTools: a flexible suite of utilities for comparing genomic features. *Bioinformatics*. 2010;26(6):841-842. doi:10.1093/bioinformatics/btq033

3. The Picard Toolkit. Broad Institute. https://broadinstitute.github.io/picard/

4. NCBI Sequence Read Archive (SRA). https://www.ncbi.nlm.nih.gov/sra

5. Cock PJ, Fields CJ, Goto N, Heuer ML, Rice PM. The Sanger FASTQ file format for sequences with quality scores, and the Solexa/Illumina FASTQ variants. *Nucleic Acids Res*. 2010;38(6):1767-1771. doi:10.1093/nar/gkp1137

6. SAM/BAM format specification. https://samtools.github.io/hts-specs/SAMv1.pdf

7. Leinonen R, Sugawara H, Shumway M; International Nucleotide Sequence Database Collaboration. The sequence read archive. *Nucleic Acids Res*. 2011;39(Database issue):D19-D21. doi:10.1093/nar/gkq1019

---

*This Quick Tip is part of the NGS101.com tutorial series designed to help bioinformatics beginners master essential skills for genomic data analysis.*
