# How To Analyze Whole Genome Sequencing Data For Absolute Beginners Part 6: Identifying Germline Copy Number Variants

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/)

- [Introduction: Understanding Copy Number Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Introduction_Understanding_Copy_Number_Variants)

  - [What Are Copy Number Variants?](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#What_Are_Copy_Number_Variants)
  - [The Biological and Clinical Significance of CNVs](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#The_Biological_and_Clinical_Significance_of_CNVs)
  - [The Challenge of CNV Detection from WGS Data](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#The_Challenge_of_CNV_Detection_from_WGS_Data)
  - [CNV Detection Workflow Overview](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#CNV_Detection_Workflow_Overview)
  - [Why CNVkit for This Tutorial?](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Why_CNVkit_for_This_Tutorial)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Setting_Up_Your_Analysis_Environment)

  - [Prerequisites](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Prerequisites)
  - [Installing CNVkit and Dependencies](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Installing_CNVkit_and_Dependencies)
  - [Download Reference Files for Annotation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Download_Reference_Files_for_Annotation)

- [Understanding Your Data: From Part 1 to CNV Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Understanding_Your_Data_From_Part_1_to_CNV_Analysis)

  - [What You Have From Part 1](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#What_You_Have_From_Part_1)
  - [What CNVkit Will Use](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#What_CNVkit_Will_Use)

- [Preparing Your Example Dataset](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Preparing_Your_Example_Dataset)

  - [Sample Information Review](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Sample_Information_Review)
  - [Quality Considerations for CNV Detection](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Quality_Considerations_for_CNV_Detection)

- [Building a CNV Reference: The Foundation of Accurate Detection](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Building_a_CNV_Reference_The_Foundation_of_Accurate_Detection)

  - [Understanding the Reference Concept](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Understanding_the_Reference_Concept)
  - [Creating an Access File (Optional but Recommended)](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Creating_an_Access_File_Optional_but_Recommended)
  - [Building the Pooled Normal Reference](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Building_the_Pooled_Normal_Reference)
  - [Alternative: Single Sample Reference (When Pooled Normals Aren’t Available)](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Alternative_Single_Sample_Reference_When_Pooled_Normals_Arent_Available)
  - [Understanding the Reference File](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Understanding_the_Reference_File)

- [Single Sample CNV Detection: The Core Workflow](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Single_Sample_CNV_Detection_The_Core_Workflow)

  - [Step-by-Step CNV Calling](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Step-by-Step_CNV_Calling)
  - [Understanding CNVkit Output Files](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Understanding_CNVkit_Output_Files)

- [Visualizing Copy Number Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Visualizing_Copy_Number_Variants)

  - [Genome-Wide Scatter Plots](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Genome-Wide_Scatter_Plots)
  - [Creating a Heatmap for Multiple Samples](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Creating_a_Heatmap_for_Multiple_Samples)
  - [Interpreting Visualization Results](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Interpreting_Visualization_Results)

- [Filtering and Quality Control](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Filtering_and_Quality_Control)

  - [Filtering Strategy](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Filtering_Strategy)

- [Single-Command Germline CNV Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Single-Command_Germline_CNV_Analysis)

  - [Complete Workflow Using CNVkit Batch](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Complete_Workflow_Using_CNVkit_Batch)

- [Best Practices for Germline CNV Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Best_Practices_for_Germline_CNV_Analysis)

  - [Experimental Design Considerations](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Experimental_Design_Considerations)
  - [Computational Best Practices](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Computational_Best_Practices)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Common_Pitfalls_to_Avoid)

- [References and Further Reading](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#References_and_Further_Reading)

  - [Share this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-identifying-germline-copy-number-variants/#Like_this)

## Introduction: Understanding Copy Number Variants

While single nucleotide variants (SNVs) and small insertions/deletions (indels) capture much of the attention in genomic analysis, they represent only part of the story of human genetic variation. Copy Number Variants (CNVs) – duplications and deletions of large segments of DNA – play an equally important role in human genetics, disease susceptibility, and evolution. This tutorial builds upon [Part 1 of our WGS analysis series](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/) to help you identify and interpret these structural variations in germline samples.

### What Are Copy Number Variants?

Copy Number Variants are alterations in the genome where sections of DNA are duplicated or deleted, resulting in an abnormal number of copies of one or more genomic regions. Unlike SNVs that affect single nucleotides, CNVs can span thousands to millions of base pairs and may encompass entire genes or regulatory elements.

To understand CNVs, imagine your genome as a library containing two copies of each book (one from each parent). A CNV would be like discovering that certain books appear three times (duplication), only once (deletion), or are missing entirely (homozygous deletion). These “copy errors” can have profound effects depending on which genomic regions they affect.

**Key Characteristics of CNVs:**

- **Size Range:** Typically defined as variants larger than 1 kilobase (1,000 base pairs), though detection methods vary

- **Frequency:** CNVs collectively account for more nucleotide variation between individuals than SNVs

- **Types:** Deletions (loss of genomic material), duplications (gain of genomic material), and complex rearrangements

- **Inheritance:** Can be inherited from parents or arise de novo during gametogenesis or early development

> 

**Germline vs. Somatic CNVs:** This tutorial focuses on germline CNVs – those present in all cells of an individual and potentially heritable. Somatic CNVs, which occur in specific tissues (particularly tumors), require different analytical approaches that we’ll cover in a future tutorial.

### The Biological and Clinical Significance of CNVs

Copy number variants are far from genomic curiosities – they’re fundamental players in human biology and medicine:

**Normal Variation and Evolution:**

- CNVs contribute to normal human diversity, affecting traits like metabolism, immunity, and sensory perception

- Gene duplications enable evolutionary innovation by allowing one copy to maintain original function while the other evolves new capabilities

- Some CNVs are adaptive; for example, populations with high-starch diets often show increased copy numbers of the amylase gene

**Disease Associations:**

- **Developmental Disorders:** Large CNVs are major causes of intellectual disability, autism spectrum disorders, and congenital anomalies. The 22q11.2 deletion syndrome (DiGeorge syndrome) affects approximately 1 in 4,000 births

- **Cancer Predisposition:** Germline deletions in tumor suppressor genes or duplications in oncogenes can increase cancer risk

- **Pharmacogenomics:** CNVs in drug-metabolizing enzyme genes (like CYP2D6) affect medication response and dosing

- **Complex Disease:** CNVs contribute to susceptibility for conditions including schizophrenia, lupus, and Crohn’s disease

**Diagnostic Value:**

- Clinical cytogenetic testing can identify pathogenic CNVs responsible for unexplained developmental delays

- Prenatal screening increasingly incorporates CNV detection

- Carrier screening programs identify recessive disease-causing CNVs

The ability to accurately detect and interpret CNVs from whole genome sequencing data has therefore become an essential skill for researchers and clinicians working in genomics.

### The Challenge of CNV Detection from WGS Data

Detecting CNVs from sequencing data is more challenging than calling SNVs for several reasons:

**Technical Challenges:**

- **Read Depth Variation:** CNV detection relies on measuring sequencing coverage, which naturally varies due to GC content, mappability, and technical biases

- **Ambiguous Mapping:** Repetitive regions and segmental duplications complicate read alignment, creating false signals

- **Breakpoint Resolution:** Precisely determining where a CNV begins and ends requires sophisticated algorithms

- **Zygosity Determination:** Distinguishing heterozygous from homozygous events in diploid genomes adds complexity

**Biological Challenges:**

- **Size Detection Limits:** While WGS theoretically detects CNVs of all sizes, sensitivity varies with CNV size and coverage depth

- **Population Variation:** Common CNVs must be distinguished from rare pathogenic variants

- **Complex Rearrangements:** Some CNVs involve inversions, translocations, or other structural complexity

Despite these challenges, modern computational tools have made CNV detection from WGS increasingly reliable and accessible.

### CNV Detection Workflow Overview

The general workflow for germline CNV detection from WGS data involves:

1. **Data Preparation:** Using high-quality aligned reads from standard WGS processing

2. **Coverage Calculation:** Measuring read depth across the genome in defined bins

3. **Normalization:** Correcting for technical biases (GC content, mappability)

4. **Segmentation:** Identifying genomic regions with abnormal copy number

5. **Calling:** Determining precise CNV boundaries and copy number states

6. **Filtering:** Removing artifacts and low-confidence calls

7. **Interpretation:** Assessing clinical or biological significance

### Why CNVkit for This Tutorial?

Among the many tools available for CNV detection, we’ve chosen CNVkit for this tutorial based on several compelling advantages:

**Methodological Strengths:**

- **Hybrid Approach:** CNVkit combines read depth analysis with allelic imbalance information for improved accuracy

- **Reference-Based Correction:** Uses pooled normal samples to correct for systematic biases

- **Flexible Design:** Works with both WGS and targeted sequencing (exomes, gene panels)

- **Comprehensive Pipeline:** Provides end-to-end workflow from BAM files to annotated calls

**Practical Advantages:**

- **Well-Documented:** Extensive documentation with clear examples

- **Active Development:** Regular updates and bug fixes from maintainers

- **Visualization Tools:** Built-in plotting functions for quality assessment

- **Published and Validated:** Peer-reviewed methodology with citations in numerous studies

**Beginner-Friendly:**

- **Clear Command Structure:** Intuitive command-line interface

- **Interpretable Output:** Human-readable results with straightforward format

- **Educational:** Good tool for learning CNV detection principles

CNVkit was developed at UCSF and published in *PLOS Computational Biology* (Talevich et al., 2016). It has been widely adopted in both research and clinical settings.

> 

**Alternative Tools:** While we use CNVkit here, other excellent tools exist including Control-FREEC, GATK gCNV, Delly, and LUMPY. Each has strengths for specific use cases. CNVkit’s balance of accuracy, usability, and documentation makes it ideal for beginners.

## Setting Up Your Analysis Environment

Since this tutorial builds on Part 1 of our WGS series, we’ll use and extend the same conda environment you created previously.

### Prerequisites

Before proceeding, ensure you have:

- Completed [Part 1: From Raw Reads to High-Quality Variants Using GATK](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/)

- Aligned BAM files from your WGS samples

- The conda environment from Part 1 (`WGS_env`) still available

- Sufficient disk space (CNVkit outputs are moderate in size, ~100-500MB per sample)

### Installing CNVkit and Dependencies

Let’s activate your existing environment and add the tools needed for CNV analysis:

```bash
# Activate the conda environment from Part 1
conda activate ~/WGS_env

# Install CNVkit and essential dependencies
conda install -y \
    cnvkit \                    # Main CNV calling tool
    r-base \                    # R programming language 
    r-dnacopy \                 # R package for DNA copy number analysis (CBS algorithm)
    bioconductor-biostrings \   # Bioconductor package for DNA sequence manipulation
    bedtools                    # Tool for genomic interval operations

# Verify CNVkit installation
cnvkit.py version
```

> 

**Version Compatibility Note:** CNVkit is actively developed. This tutorial uses version 0.9.x, but commands should work with newer versions. Check the [CNVkit documentation](https://cnvkit.readthedocs.io/) if you encounter compatibility issues.

### Download Reference Files for Annotation

CNVkit needs gene annotations for optimal performance:

```bash
# Create directories for reference files
# This keeps our annotation files organized
mkdir -p ~/references/cnv_references
cd ~/references/cnv_references

# Download RefSeq gene annotations for hg38 human genome
wget https://hgdownload.soe.ucsc.edu/goldenPath/hg38/database/refFlat.txt.gz
gunzip refFlat.txt.gz

# Convert RefSeq to BED format for CNVkit
awk '{print $3"\t"$5"\t"$6"\t"$1}' refFlat.txt | sort -k1,1 -k2,2n > refGene.bed
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/refgene_bed.png?resize=532%2C152&ssl=1)

## Understanding Your Data: From Part 1 to CNV Analysis

Let’s briefly review what we accomplished in Part 1 and how it connects to CNV analysis:

### What You Have From Part 1

After completing the GATK pipeline in Part 1, you should have:

1. **Aligned BAM Files:** High-quality reads mapped to the reference genome (hg38)

2. **Processed BAM Files:** Duplicate-marked and base quality score recalibrated

3. **VCF Files:** SNV and indel calls for each sample

### What CNVkit Will Use

CNVkit primarily works with your aligned BAM files to:

- Calculate read depth across the genome in defined intervals

- Normalize coverage for GC content and mappability biases

- Segment the genome based on copy number changes

- Call discrete CNV events with statistical confidence

The beauty of this approach is that you’re extracting additional information from the same sequencing data you used for SNV calling – no new experiments required!

## Preparing Your Example Dataset

For this tutorial, we’ll use the same example data from Part 1. If you’re working with different samples, the workflow remains identical – simply adjust the sample names.

### Sample Information Review

Let’s establish what we’re working with:

```bash
# Define your working directory (same as Part 1)
PROJECT_DIR=~/WGS_Project
cd ${PROJECT_DIR}

mkdir -p cnv_analysis/{coverage,reference,calls,visualizations,filtered,annotated}

# Define sample information
# Replace these with your actual sample names from Part 1
SAMPLES=(
    "normal1"
    "normal2"
)

# Define important file paths for easy reference
BAM_DIR=${PROJECT_DIR}/aligned_reads
CNV_DIR=${PROJECT_DIR}/cnv_analysis
REFERENCE_GENOME=~/references/hg38.fa
REFGENE_BED=~/references/cnv_references/refGene.bed
```

### Quality Considerations for CNV Detection

Before proceeding, verify your samples meet these quality criteria:

**Sequencing Coverage:**

- **Minimum:** 20x mean coverage genome-wide

- **Recommended:** 30x or higher for reliable CNV detection

- **Higher coverage improves:** Detection of smaller CNVs and heterozygous deletions

**Mapping Quality:**

- At least 70% of reads should map with MAPQ ≥ 20

- Higher mapping quality reduces false positive CNV calls

**Duplication Rate:**

- Ideally < 20% duplicate reads

- High duplication rates can bias coverage and affect CNV calling

```bash
# Quick quality check of your samples
for sample in "${SAMPLES[@]}"; do
    samtools depth ${BAM_DIR}/${sample}_recalibrated.bam | \
        awk '{sum+=$3; count++} END {print "'${sample}' Mean coverage:", sum/count, "x"}'
    samtools view ${BAM_DIR}/${sample}_recalibrated.bam | \
        awk '$5 >= 20 {hq++} {total++} END {print "'${sample}' High-quality mapped:", hq/total*100, "%"}'
done

# Expected output:
# normal1 Mean coverage: 23.3598 x
# normal1 High-quality mapped: 91.452 %
# normal2 Mean coverage: 23.9861 x
# normal2 High-quality mapped: 91.3992 %
```

> 

**Coverage Too Low?** If your coverage is below 20x, CNV detection is still possible but sensitivity will be reduced, particularly for small CNVs and heterozygous deletions. Consider this limitation when interpreting results.

## Building a CNV Reference: The Foundation of Accurate Detection

One of CNVkit’s key innovations is its use of a pooled reference that captures systematic biases in your sequencing data. This reference is crucial for distinguishing true CNVs from technical artifacts.

### Understanding the Reference Concept

Think of the CNVkit reference as a “normal baseline” that represents what coverage should look like in the absence of CNVs. This baseline accounts for:

- **GC Content Bias:** Regions with extreme GC content often show aberrant coverage

- **Mappability Issues:** Repetitive regions have reduced or variable coverage

- **Capture Efficiency:** For WGS, this is less relevant, but target regions in exome sequencing show variable capture

- **Technical Artifacts:** Systematic biases introduced by library preparation or sequencing

By creating a reference from multiple normal samples (or even a single normal sample if that’s all you have), CNVkit learns these patterns and can correct for them when analyzing test samples.

### Creating an Access File (Optional but Recommended)

The access file tells CNVkit which genomic regions are accessible for analysis and should be avoided:

```bash
# Create access file for hg38 human genome
cnvkit.py access ${REFERENCE_GENOME} \
    -o ${CNV_DIR}/reference/access-hg38.bed \
    -s 10000 # we want regions at least 10kb long for analysis
```

> 

**What If I Skip This?** Creating an access file is optional. Without it, CNVkit will analyze the entire genome, including problematic regions that may produce false positives. For best results, especially with WGS data, we recommend creating an access file.

### Building the Pooled Normal Reference

Now we’ll create the reference that CNVkit will use to normalize all future CNV calls. Ideally, use 10-50 normal samples to build a robust reference. For this tutorial, we’ll demonstrate with a smaller number:

```bash
# Step 1: Create target and antitarget regions for whole genome sequencing
cnvkit.py target ${CNV_DIR}/reference/access-hg38.bed \
    --annotate ${REFGENE_BED} \
    --split \
    -o ${CNV_DIR}/reference/targets.bed

# Create antitarget bins in the remaining genomic space
cnvkit.py antitarget ${CNV_DIR}/reference/targets.bed \
    -g ${CNV_DIR}/reference/access-hg38.bed \
    -o ${CNV_DIR}/reference/antitargets.bed

# Step 2: Calculate coverage for each normal sample
for sample in "${SAMPLES[@]}"; do
    # Calculate target coverage (main analysis regions)

    cnvkit.py coverage ${BAM_DIR}/${sample}_recalibrated.bam \
        ${CNV_DIR}/reference/targets.bed \
        -o ${CNV_DIR}/coverage/${sample}.targetcoverage.cnn

    # Calculate antitarget coverage (background regions)
    # This helps normalize for technical biases

    cnvkit.py coverage ${BAM_DIR}/${sample}_recalibrated.bam \
        ${CNV_DIR}/reference/antitargets.bed \
        -o ${CNV_DIR}/coverage/${sample}.antitargetcoverage.cnn
done

# Step 3: Build the pooled reference from all normal samples
# This combines coverage patterns from multiple samples to create a robust baseline
# The reference learns what "normal" coverage looks like across different genomic regions

cnvkit.py reference \
    ${CNV_DIR}/coverage/*.targetcoverage.cnn \
    ${CNV_DIR}/coverage/*.antitargetcoverage.cnn \
    --fasta ${REFERENCE_GENOME} \
    -o ${CNV_DIR}/reference/my_reference.cnn
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/my_reference_cnn.png?resize=1024%2C155&ssl=1)

### Alternative: Single Sample Reference (When Pooled Normals Aren’t Available)

If you don’t have multiple normal samples, you can create a reference from a single normal:

```bash
# If you only have one normal sample or want to use a flat reference
# This is less robust than pooled normals but still functional

cnvkit.py batch ${BAM_DIR}/normal1.bam \
    --normal \
    --targets ${CNV_DIR}/reference/targets.bed \
    --antitargets ${CNV_DIR}/reference/antitargets.bed \
    --fasta ${REFERENCE_GENOME} \
    --annotate ${REFGENE_BED} \
    --output-reference ${CNV_DIR}/reference/my_reference.cnn \  
    --output-dir ${CNV_DIR}/reference/ \
    --diagram \
    --scatter
```

> 

**Which Method Should I Use?** Use pooled normals whenever possible – it provides much more robust correction for technical biases. A single sample reference is acceptable for exploratory analysis or when pooled normals are unavailable, but interpret results more cautiously.

### Understanding the Reference File

The reference file (`.cnn`) contains:

- Expected log2 coverage for each genomic bin

- GC content correction factors

- Mappability weights

- Spread statistics for calling significance

This reference will be used to normalize all your test samples, so it’s worth investing time to create a high-quality one.

## Single Sample CNV Detection: The Core Workflow

Now that we have our reference, we can detect CNVs in individual samples. We’ll walk through the complete workflow for one sample, which you can then repeat for all your samples.

### Step-by-Step CNV Calling

Let’s analyze our first sample in detail:

```bash
# Now we'll analyze one sample in detail using our reference
# This workflow can be repeated for all your samples

# Select the first sample for detailed analysis
TEST_SAMPLE="normal1"

# Create output directory for this sample's results
mkdir -p ${CNV_DIR}/calls/${TEST_SAMPLE}

# Step 1: Calculate read depth coverage for the test sample
# This measures how many sequencing reads map to each genomic region

# Calculate target coverage (main analysis regions)
# This creates a file showing read depth for each target bin
cnvkit.py coverage ${BAM_DIR}/${TEST_SAMPLE}.bam \
    ${CNV_DIR}/reference/targets.bed \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.targetcoverage.cnn

# Calculate antitarget coverage (background regions)
# This helps with normalization and bias correction
cnvkit.py coverage ${BAM_DIR}/${TEST_SAMPLE}.bam \
    ${CNV_DIR}/reference/antitargets.bed \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.antitargetcoverage.cnn

# Step 2: Normalize coverage using our reference
# This corrects for GC bias, mappability, and other technical artifacts
# The output is log2 copy ratios where:
# - log2 = 0: normal copy number (2 copies)
# - log2 = -1: one copy (heterozygous deletion)
# - log2 = 1: three copies (duplication)

cnvkit.py fix \
    ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.targetcoverage.cnn \
    ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.antitargetcoverage.cnn \
    ${CNV_DIR}/reference/my_reference.cnn \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cnr

# The .cnr file now contains normalized log2 copy ratios for each genomic bin

# Step 3: Segment the genome to find CNV boundaries
# This groups adjacent bins with similar copy numbers into discrete segments
# Uses the Circular Binary Segmentation (CBS) algorithm

cnvkit.py segment ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cnr \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cns \
    --method cbs \        # Use CBS algorithm (most common and reliable)
    --smooth-cbs          # Apply smoothing to reduce noise

# The .cns file contains discrete segments with their copy number states

# Step 4: Call discrete CNV events with confidence thresholds
# This converts continuous log2 ratios to discrete copy numbers (0, 1, 2, 3, etc.)

cnvkit.py call ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cns \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.call.cns \
    -m threshold \
    -t=-1.1,-0.4,0.3,0.7

# Threshold explanation:
# < -1.1: Homozygous deletion (0 copies)
# -1.1 to -0.4: Heterozygous deletion (1 copy)
# -0.4 to 0.3: Neutral/normal (2 copies)
# 0.3 to 0.7: Single copy gain (3 copies)
# > 0.7: High-level amplification (4+ copies)

# Step 5: Export results to standard formats for compatibility with other tools

# Export to VCF format (Variant Call Format - standard for genomic variants)
cnvkit.py export vcf ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.call.cns \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cnv.vcf

# Export to BED format (for genome browsers like IGV, UCSC Genome Browser)
cnvkit.py export bed ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.call.cns \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cnv.bed

# Export to SEG format (for IGV and other visualization tools)
cnvkit.py export seg ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.call.cns \
    -o ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cnv.seg

# Step 6: Generate quality metrics for this sample
# These metrics help assess whether the analysis was successful

cnvkit.py metrics ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cnr \
    ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.cns \
    > ${CNV_DIR}/calls/${TEST_SAMPLE}/${TEST_SAMPLE}.metrics.txt
```

**normal1.call.cns:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/normal1_call_cns.png?resize=1024%2C130&ssl=1)

**normal1.metrics.txt (focus on the .cnr row):**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/normal1_metrics.png?resize=1024%2C126&ssl=1)

- stdev: Standard deviation of log2 ratios (lower is better)

- mad: Median absolute deviation (robust measure of variability)

- iqr: Interquartile range of log2 ratios

- bivar: Bivariate spread measure

### Understanding CNVkit Output Files

Let’s examine what each output file contains:

**`.cnr` (Copy Number Ratios):**

- Raw, bin-level log2 coverage ratios

- One line per genomic bin

- Useful for visualization but noisy

- Columns: chromosome, start, end, gene, log2 ratio, depth, weight

**`.cns` (Copy Number Segments):**

- Smoothed segments after CBS algorithm

- Discrete genomic regions with consistent copy number

- Main file for identifying CNVs

- Columns: chromosome, start, end, gene, log2 ratio, probes (number of bins)

**`.call.cns` (Called Copy Numbers):**

- Integer copy number assignments (0, 1, 2, 3, …)

- Includes confidence intervals

- Ready for interpretation

- Additional columns: copy number, baf (B-allele frequency if available)

## Visualizing Copy Number Variants

Visualization is crucial for quality assessment and biological interpretation. CNVkit provides several built-in plotting functions.

### Genome-Wide Scatter Plots

The scatter plot provides an overview of copy number across the entire genome:

```bash
# Select sample for visualization
VIS_SAMPLE="normal1"
mkdir -p ${CNV_DIR}/visualizations/${VIS_SAMPLE}

# Create genome-wide scatter plot
# This shows copy number variation across all chromosomes
# X-axis: genomic position, Y-axis: log2 copy ratio
# Gray dots: individual bins, Red line: smoothed segments

cnvkit.py scatter ${CNV_DIR}/calls/${VIS_SAMPLE}/${VIS_SAMPLE}.cnr \
    -s ${CNV_DIR}/calls/${VIS_SAMPLE}/${VIS_SAMPLE}.cns \
    -o ${CNV_DIR}/visualizations/${VIS_SAMPLE}/${VIS_SAMPLE}_scatter.pdf \
    --title "${VIS_SAMPLE} - Genome-wide Copy Number Profile"

# Create detailed plots for individual chromosomes
# These provide higher resolution views of specific chromosomes
# Useful for examining suspected CNVs in detail

for chr in chr{1..22} chrX chrY; do
    cnvkit.py scatter ${CNV_DIR}/calls/${VIS_SAMPLE}/${VIS_SAMPLE}.cnr \
        -s ${CNV_DIR}/calls/${VIS_SAMPLE}/${VIS_SAMPLE}.cns \
        -c ${chr} \                    # Focus on specific chromosome
        -o ${CNV_DIR}/visualizations/${VIS_SAMPLE}/${VIS_SAMPLE}_${chr}.pdf \
        --title "${VIS_SAMPLE} - ${chr}" \
        --segment-color "#FF6B6B"      # Use red color for segments
done

# Create ideogram-style diagram
# This provides a compact overview similar to a karyotype
# Shows all chromosomes with CNVs marked as colored regions

cnvkit.py diagram ${CNV_DIR}/calls/${VIS_SAMPLE}/${VIS_SAMPLE}.cnr \
    -s ${CNV_DIR}/calls/${VIS_SAMPLE}/${VIS_SAMPLE}.cns \
    -o ${CNV_DIR}/visualizations/${VIS_SAMPLE}/${VIS_SAMPLE}_diagram.pdf \
    --title "${VIS_SAMPLE} - Copy Number Diagram"
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/cnvkit_plots-e1761013024996-1024x257.png?resize=1024%2C257&ssl=1)

### Creating a Heatmap for Multiple Samples

Heatmaps are particularly useful for comparing CNV patterns across multiple samples:

```bash
# Create multi-sample heatmap for comparing patterns across samples
# Rows: samples, Columns: genomic position
# Color intensity: copy number deviation (red=gains, blue=losses)

SEGMENT_FILES=""
for sample in "${SAMPLES[@]}"; do
    SEGMENT_FILES="${SEGMENT_FILES} ${CNV_DIR}/calls/${sample}/${sample}.cns"
done

cnvkit.py heatmap ${SEGMENT_FILES} \
    -o ${CNV_DIR}/visualizations/all_samples_heatmap.pdf \
    --chromosome \
    --desaturate
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/cnv_heatmap.webp?resize=746%2C547&ssl=1)

### Interpreting Visualization Results

When examining your plots, look for:

**Quality Indicators:**

- **Smooth baseline:** Bin-level data should hover around log2 = 0 for most of the genome

- **Clear segments:** Segment boundaries should be distinct, not gradual

- **Consistent sex chromosomes:** Males should show hemizygous X and Y (log2 ≈ -1); females should show diploid X (log2 ≈ 0) and no Y

**Biological Signals:**

- **Sharp transitions:** Abrupt changes in copy number indicate CNV boundaries

- **Recurrent patterns:** CNVs appearing in the same location across multiple samples may be:

- Common polymorphisms (benign)

- Technical artifacts

- Disease-associated hotspots

- **Size distribution:** True germline CNVs range from kilobases to megabases

**Red Flags (Potential Issues):**

- **Noisy baseline:** Excessive scatter around log2 = 0 indicates low quality or low coverage

- **Wavy patterns:** Systematic waves across chromosomes suggest GC bias wasn’t fully corrected

- **Whole-chromosome imbalances:** In germline samples, entire chromosome gains/losses suggest:

- Sample contamination

- Aneuploidy (rare, usually Turner syndrome or Down syndrome mosaicism)

- Technical problems

> 

**Visualization Tip:** Always examine both genome-wide and chromosome-specific plots. Sometimes subtle CNVs are only visible upon zooming in to specific chromosomes.

## Filtering and Quality Control

Raw CNV calls contain many false positives. Rigorous filtering is essential for reliable results.

### Filtering Strategy

Raw CNV calls contain many false positives due to:

- Technical noise in sequencing data

- Repetitive genomic regions

- Systematic biases not fully corrected

- Filtering is essential to identify high-confidence, clinically relevant CNVs

Apply multiple filters to increase confidence:

```bash
# Apply basic quality filters to each sample
for sample in "${SAMPLES[@]}"; do
    # Filter CNVs based on multiple quality criteria:
    # $6 != 2: Copy number different from normal diploid (2 copies)
    # ($3-$2) >= 10000: CNV at least 10kb in size (smaller ones are often artifacts)
    # $7 >= 3: At least 3 genomic bins support the CNV (reduces random noise)
    awk 'NR==1 || ($6 != 2 && ($3-$2) >= 10000 && $7 >= 3)' \
        ${CNV_DIR}/calls/${sample}/${sample}.call.cns \
        > ${CNV_DIR}/filtered/${sample}_filtered.cns
done

# Apply advanced filtering using confidence intervals
for sample in "${SAMPLES[@]}"; do
    # Use stricter thresholds that require higher confidence
    # This removes calls where confidence intervals overlap with neutral copy number
    # --thresholds sets more stringent boundaries:
    # -0.5 to -0.2: confident deletion (narrower range = higher confidence)
    # -0.2 to 0.2: neutral (filtered out as not significant)
    # 0.2 to 0.5: confident gain
    cnvkit.py call ${CNV_DIR}/calls/${sample}/${sample}.cns \
        -o ${CNV_DIR}/filtered/${sample}_ci_filtered.cns \
        --thresholds=-0.5,-0.2,0.2,0.5
done

# Remove common population variants that are likely benign
# Create directory for CNV databases
mkdir -p ~/references/cnv_databases
cd ~/references/cnv_databases

# Download Database of Genomic Variants (DGV) - contains common CNVs
# These are CNVs found frequently in healthy populations (usually > 1% frequency)
# Most common CNVs are benign polymorphisms, not disease-causing
wget http://dgv.tcag.ca/dgv/docs/GRCh38_hg38_variants_2020-02-25.txt

# Convert DGV data to BED format for filtering
tail -n +2 GRCh38_hg38_variants_2020-02-25.txt | \
    awk -F'\t' '$2 != "" {print "chr"$2"\t"$3"\t"$4"\t"$1}' | \
    sort -k1,1 -k2,2n > dgv_common_cnvs.bed

# Filter out CNVs that substantially overlap with known common variants
for sample in "${SAMPLES[@]}"; do
    # Convert our filtered CNS file to BED format for comparison
    # IMPORTANT: Only extract segments with abnormal copy numbers (not equal to 2)
    # Extract chromosome, start, end, gene, log2ratio, and copy number columns
    tail -n +2 ${CNV_DIR}/filtered/${sample}_ci_filtered.cns | \
        awk '$6 != 2 {print $1"\t"$2"\t"$3"\t"$4"\t"$5"\t"$6}' > \
        ${CNV_DIR}/filtered/${sample}_temp.bed
    
    # Remove CNVs that overlap ≥50% with common population variants
    # -v: only keep intervals that DON'T overlap
    # -f 0.5: require 50% overlap to consider it a match
    bedtools intersect \
        -a ${CNV_DIR}/filtered/${sample}_temp.bed \
        -b dgv_common_cnvs.bed \
        -v \
        -f 0.5 \
        > ${CNV_DIR}/filtered/${sample}_rare.bed
    
    # Clean up temporary files
    rm ${CNV_DIR}/filtered/${sample}_temp.bed
done
```

> 

**Filtering Philosophy:** It’s better to miss some true CNVs (false negatives) than to include many false positives. For clinical applications, err on the side of stringency. For research exploring potential CNV associations, you might be more permissive.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/normal_rare_bed.png?resize=780%2C126&ssl=1)

## Single-Command Germline CNV Analysis

For experienced users or production workflows, CNVkit provides a streamlined `batch` command that performs the entire germline CNV analysis workflow in a single step. This method automatically handles target/antitarget creation, coverage calculation, reference building, copy number calculation, segmentation, and calling with optimized parameters.

The batch approach is ideal for:

- **Production workflows** requiring consistent, automated processing

- **Large cohort studies** processing many samples simultaneously

- **Experienced users** who understand the underlying methodology

- **Clinical pipelines** needing standardized, reproducible results

### Complete Workflow Using CNVkit Batch

```bash
# Set up directory for batch analysis
mkdir -p ${CNV_DIR}/batch_analysis
cd ${CNV_DIR}/batch_analysis

# Define sample BAM files (using the same samples from the tutorial)
SAMPLE_BAMS=("${BAM_DIR}/normal1_recalibrated.bam" 
             "${BAM_DIR}/normal2_recalibrated.bam")

# Single command to perform complete germline CNV analysis
cnvkit.py batch \
    ${SAMPLE_BAMS[@]} \
    --fasta ${REFERENCE_GENOME} \
    --annotate ${REFGENE_BED} \
    --output-reference germline_reference.cnn \
    --output-dir results/ \
    --method wgs \
    --segment-method cbs \
    --drop-low-coverage \
    --scatter \
    --diagram
```

## Best Practices for Germline CNV Analysis

To ensure reliable and clinically actionable results, follow these best practices throughout your analysis:

### Experimental Design Considerations

**Sample Quality:**

- Use high-quality DNA with minimal degradation (DIN/RIN scores > 7)

- Avoid heavily contaminated samples (>5% contamination affects CNV calling)

- For clinical samples, obtain informed consent for genetic testing

**Sequencing Parameters:**

- **Coverage depth:** Minimum 30x for reliable germline CNV detection

- **Library preparation:** PCR-free libraries reduce bias but require more input DNA

- **Read length:** 150bp paired-end recommended for WGS, though 100bp single-end is acceptable for CNV analysis

- **Insert size:** 350-500bp provides good coverage uniformity

**Controls:**

- Include technical replicates when validating the pipeline

- Process matched tissue samples (e.g., blood + tumor) identically

- Use sex-matched controls when building references

### Computational Best Practices

**Reference Building:**

- Use ≥10 normal samples for robust reference creation

- Match samples by:

- Sequencing platform and library preparation

- Coverage depth (within 20% of each other)

- Population ancestry when possible

- Update references periodically as you accumulate more normal samples

**Quality Control Checkpoints:**

- Verify coverage uniformity before CNV calling (assess with `mosdepth` or `goleft`)

- Check for batch effects between runs

- Validate sex chromosome copy number matches expected gender

- Compare CNV calls with SNP-based chromosomal ancestry estimates

**Parameter Optimization:**

- Test different bin sizes (5kb, 10kb, 20kb) to find optimal resolution-noise balance

- Adjust segmentation smoothing parameters based on your data quality

- Fine-tune copy number thresholds based on validation results

### Common Pitfalls to Avoid

**Technical Pitfalls:**

- **Using incompatible reference:** Mixing samples with different library prep methods in the reference

- **Insufficient normalization:** Not accounting for GC bias, mappability, or batch effects

- **Ignoring sex chromosomes:** Sex chromosome CNVs require special interpretation

- **Over-reliance on automation:** Always manually review high-priority CNVs

**Interpretation Pitfalls:**

- **Ignoring population frequency:** Common CNVs (>1% MAF) are rarely pathogenic

- **Overcalling in complex regions:** Segmental duplications and centromeres produce false positives

- **Missing mosaicism:** Low-level mosaicism (<30%) is difficult to detect with standard WGS

- **Conflating germline and somatic:** Germline analysis pipelines aren’t optimized for somatic CNVs

**Clinical Pitfalls:**

- **Reporting VUS as pathogenic:** Resist pressure to over-interpret uncertain variants

- **Neglecting phenotype correlation:** CNV interpretation must consider clinical presentation

- **Failing to validate:** Never report clinically actionable CNVs without confirmation

- **Inadequate genetic counseling:** Patients need support to understand complex genetic results

## References and Further Reading

1. Talevich E, Shain AH, Botton T, Bastian BC. (2016). CNVkit: Genome-wide copy number detection and visualization from targeted DNA sequencing. *PLOS Computational Biology*, 12(4): e1004873.

2. Zarrei M, MacDonald JR, Merico D, Scherer SW. (2015). A copy number variation map of the human genome. *Nature Reviews Genetics*, 16(3): 172-183.

3. Hastings PJ, Lupski JR, Rosenberg SM, Ira G. (2009). Mechanisms of change in gene copy number. *Nature Reviews Genetics*, 10(8): 551-564.

4. Riggs ER, Andersen EF, Cherry AM, et al. (2020). Technical standards for the interpretation and reporting of constitutional copy-number variants: a joint consensus recommendation of the American College of Medical Genetics and Genomics (ACMG) and the Clinical Genome Resource (ClinGen). *Genetics in Medicine*, 22(2): 245-257.

5. Abel HJ, Larson DE, Regier AA, et al. (2020). Mapping and characterization of structural variation in 17,795 human genomes. *Nature*, 583(7817): 83-89.

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, **please comment and share** **your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.*
