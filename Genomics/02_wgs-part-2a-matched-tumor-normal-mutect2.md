# How To Analyze Whole Genome Sequencing Data For Absolute Beginners Part 2A: Matched Tumor-Normal Mutation Calling With Mutect2

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 9, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/)

- [Introduction to Matched Tumor-Normal Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Introduction_to_Matched_Tumor-Normal_Analysis)

  - [What Are Somatic Mutations?](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#What_Are_Somatic_Mutations)
  - [Why Matched Tumor-Normal Analysis is the Gold Standard](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Why_Matched_Tumor-Normal_Analysis_is_the_Gold_Standard)
  - [Why GATK Mutect2 is the Industry Standard](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Why_GATK_Mutect2_is_the_Industry_Standard)
  - [What This Tutorial Covers](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#What_This_Tutorial_Covers)

- [Setting Up Your Somatic Analysis Environment](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Setting_Up_Your_Somatic_Analysis_Environment)

  - [Activating Your Environment](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Activating_Your_Environment)
  - [Downloading Somatic-Specific Reference Files](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Downloading_Somatic-Specific_Reference_Files)
  - [Creating Project Directory Structure](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Creating_Project_Directory_Structure)

- [Preparing Input Data](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Preparing_Input_Data)

  - [Linking Processed BAM Files from Part 1](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Linking_Processed_BAM_Files_from_Part_1)

- [Running Mutect2 for Somatic Variant Detection](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Running_Mutect2_for_Somatic_Variant_Detection)

  - [Understanding Mutect2 Key Parameters](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Understanding_Mutect2_Key_Parameters)

- [Assessing Sample Contamination](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Assessing_Sample_Contamination)

  - [Generating Pileup Summaries](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Generating_Pileup_Summaries)
  - [Calculating Contamination Estimates](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Calculating_Contamination_Estimates)

- [Comprehensive Filtering Pipeline](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Comprehensive_Filtering_Pipeline)

  - [Learning Read Orientation Artifacts](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Learning_Read_Orientation_Artifacts)
  - [Applying FilterMutectCalls](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Applying_FilterMutectCalls)
  - [Additional Quality Filters](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Additional_Quality_Filters)

- [Converting Results to Analysis-Ready Formats](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Converting_Results_to_Analysis-Ready_Formats)

  - [Creating Human-Readable Tables](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Creating_Human-Readable_Tables)

- [Best Practices and Troubleshooting](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Best_Practices_and_Troubleshooting)

  - [Critical Success Factors](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Critical_Success_Factors)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Common_Pitfalls_to_Avoid)
  - [When to Use This Approach](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#When_to_Use_This_Approach)
  - [Expected Results and Interpretation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Expected_Results_and_Interpretation)

- [Conclusion](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/#Like_this)

## Introduction to Matched Tumor-Normal Analysis

Welcome back to our whole genome sequencing analysis journey! In [Part 1](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/), we learned how to process raw sequencing data and identify germline variants using GATK’s best practices. Now we’re ready to tackle the gold standard approach for detecting somatic mutations: **matched tumor-normal analysis**.

### What Are Somatic Mutations?

Somatic mutations are genetic changes that occur in cells after conception, distinguishing them from germline mutations inherited from parents. In cancer research, these mutations drive tumor development and progression, making their accurate detection crucial for:

- **Precision Medicine:** Identifying targetable mutations for personalized cancer therapy

- **Tumor Biology Research:** Understanding how cancers develop and evolve

- **Biomarker Discovery:** Finding genetic signatures that predict treatment response

- **Drug Development:** Discovering new therapeutic targets

> 

**Key Concept:** Unlike germline variants that appear in ~50% of reads (for heterozygous variants), somatic mutations may appear in much lower frequencies (5-30%) due to tumor purity, clonal heterogeneity, and copy number variations.

### Why Matched Tumor-Normal Analysis is the Gold Standard

The **matched tumor-normal approach** compares a tumor sample directly to normal tissue from the same patient. This strategy provides several critical advantages:

**Maximum Specificity:** Eliminates patient-specific germline variants that would otherwise appear as false positive somatic mutations

**Optimal Sensitivity:** Detects true somatic mutations even at low allele frequencies

**Quality Control:** Identifies technical artifacts by comparing identical processing conditions

**Clinical Reliability:** Provides the confidence needed for clinical decision-making

### Why GATK Mutect2 is the Industry Standard

GATK’s Mutect2 has become the preferred tool for somatic mutation detection because it:

- **Handles Low-Frequency Variants:** Specifically designed to detect mutations present in as few as 5% of reads

- **Advanced Statistical Models:** Uses sophisticated algorithms to distinguish true mutations from sequencing artifacts

- **Comprehensive Filtering:** Provides multiple quality control layers to ensure reliable results

- **Clinical Validation:** Extensively tested and used in major cancer genomics studies worldwide

### What This Tutorial Covers

In this tutorial, we’ll analyze **one matched tumor-normal pair (tumor1 + normal1)** and learn to:

1. Set up the analysis environment with somatic-specific tools and references

2. Run Mutect2 to identify potential somatic mutations

3. Assess contamination to ensure sample quality

4. Apply comprehensive filtering to remove artifacts and false positives

5. Generate analysis-ready outputs including human-readable tables

By the end, you’ll have a complete, publication-quality somatic mutation analysis pipeline!

## Setting Up Your Somatic Analysis Environment

Since we’re building on Part 1, we’ll add only the essential new tools needed for somatic analysis. This section focuses on downloading somatic-specific reference files that weren’t required for germline variant calling.

### Activating Your Environment

First, we’ll activate the conda environment created in Part 1. This environment already contains GATK and other essential bioinformatics tools.

```bash
#-----------------------------------------------
# STEP 1: Activate existing GATK environment
#-----------------------------------------------

# Activate the WGS data analysis environment from Part 1
# If you haven't completed Part 1, please follow that tutorial first
conda activate wgs_analysis
```

### Downloading Somatic-Specific Reference Files

Somatic mutation calling requires specialized reference files beyond those used in germline analysis. These files help distinguish true somatic mutations from technical artifacts and common population variants.

**Download somatic-specific reference files for hg38:**

```bash
#-----------------------------------------------
# STEP 2: Download somatic-specific reference files (hg38)
#-----------------------------------------------

# Create directory for somatic analysis references
mkdir -p ~/references/somatic_resources
cd ~/references/somatic_resources

echo "Downloading somatic analysis reference files..."

# Panel of Normals (PON) - contains common technical artifacts
echo "Downloading Panel of Normals..."
wget https://storage.googleapis.com/gatk-best-practices/somatic-hg38/1000g_pon.hg38.vcf.gz
wget https://storage.googleapis.com/gatk-best-practices/somatic-hg38/1000g_pon.hg38.vcf.gz.tbi

# Germline resource - population allele frequencies from gnomAD
echo "Downloading germline resource..."
wget https://storage.googleapis.com/gatk-best-practices/somatic-hg38/af-only-gnomad.hg38.vcf.gz
wget https://storage.googleapis.com/gatk-best-practices/somatic-hg38/af-only-gnomad.hg38.vcf.gz.tbi

# Common variants for contamination estimation
echo "Downloading contamination resource..."
wget https://storage.googleapis.com/gatk-best-practices/somatic-hg38/small_exac_common_3.hg38.vcf.gz
wget https://storage.googleapis.com/gatk-best-practices/somatic-hg38/small_exac_common_3.hg38.vcf.gz.tbi

# Link to reference genome from Part 1
echo "Linking reference genome from Part 1..."
ln -s ~/wgs_analysis/reference/Homo_sapiens_assembly38.fasta ./
ln -s ~/wgs_analysis/reference/Homo_sapiens_assembly38.fasta.fai ./
ln -s ~/wgs_analysis/reference/Homo_sapiens_assembly38.dict ./

echo "✓ Reference files downloaded successfully!"
```

**Download somatic-specific reference files for hg19:**

```bash
#-----------------------------------------------
# STEP 2: Download somatic-specific reference files (hg19/b37)
#-----------------------------------------------

# Create directory for somatic analysis references
mkdir -p ~/references/somatic_resources_hg19
cd ~/references/somatic_resources_hg19

echo "Downloading somatic analysis reference files for hg19..."

# Panel of Normals (PON) - contains common technical artifacts
echo "Downloading Panel of Normals..."
wget https://storage.googleapis.com/gatk-best-practices/somatic-b37/Mutect2-WGS-panel-b37.vcf
wget https://storage.googleapis.com/gatk-best-practices/somatic-b37/Mutect2-WGS-panel-b37.vcf.idx

# Germline resource - population allele frequencies from gnomAD
echo "Downloading germline resource..."
wget https://storage.googleapis.com/gatk-best-practices/somatic-b37/af-only-gnomad.raw.sites.vcf
wget https://storage.googleapis.com/gatk-best-practices/somatic-b37/af-only-gnomad.raw.sites.vcf.idx

# Common variants for contamination estimation
echo "Downloading contamination resource..."
wget https://storage.googleapis.com/gatk-best-practices/somatic-b37/small_exac_common_3.vcf
wget https://storage.googleapis.com/gatk-best-practices/somatic-b37/small_exac_common_3.vcf.idx

# Download and prepare hg19 reference genome
echo "Downloading hg19 reference genome..."
wget https://storage.googleapis.com/gcp-public-data--broad-references/hg19/v0/Homo_sapiens_assembly19.fasta
wget https://storage.googleapis.com/gcp-public-data--broad-references/hg19/v0/Homo_sapiens_assembly19.fasta.fai
wget https://storage.googleapis.com/gcp-public-data--broad-references/hg19/v0/Homo_sapiens_assembly19.dict

echo "✓ Reference files downloaded successfully!"
```

**Important notes for using hg19:**

- If your BAM files from Part 1 are aligned to hg19, make sure to use these hg19 reference files throughout the entire Mutect2 pipeline

- The chromosome naming convention for b37/hg19 uses “1, 2, 3…” while some hg19 variants use “chr1, chr2, chr3…” – ensure consistency

- You may need to compress and index the VCF files if GATK requires `.vcf.gz` format:

```bash
# Optional: Compress and index VCF files if needed
bgzip Mutect2-WGS-panel-b37.vcf
tabix -p vcf Mutect2-WGS-panel-b37.vcf.gz

bgzip af-only-gnomad.raw.sites.vcf
tabix -p vcf af-only-gnomad.raw.sites.vcf.gz

bgzip small_exac_common_3.vcf
tabix -p vcf small_exac_common_3.vcf.gz
```

### Creating Project Directory Structure

Organizing your analysis with a clear directory structure is crucial for maintaining reproducible workflows and managing large datasets effectively.

```bash
#-----------------------------------------------
# STEP 3: Create organized project directory structure
#-----------------------------------------------

# Create main project directory
mkdir -p ~/somatic_analysis_matched
cd ~/somatic_analysis_matched

# Create subdirectories for different analysis stages
mkdir -p {input_data,raw_calls,filtered_calls,contamination_analysis}
mkdir -p {converted_tables,maf_files,qc_reports}

echo "Project directory structure created:"
echo "~/somatic_analysis_matched/"
echo "├── input_data/              # Links to processed BAM files"
echo "├── raw_calls/               # Unfiltered Mutect2 output"
echo "├── filtered_calls/          # Quality-filtered variants"
echo "├── contamination_analysis/  # Cross-contamination assessment"
echo "├── converted_tables/        # Human-readable tables"
echo "├── maf_files/               # MAF format for R analysis"
echo "└── qc_reports/              # Quality control summaries"
```

## Preparing Input Data

Rather than copying large BAM files, we’ll create symbolic links to the processed files from Part 1. This approach saves disk space while maintaining access to the high-quality, analysis-ready alignments.

### Linking Processed BAM Files from Part 1

```bash
#-----------------------------------------------
# STEP 4: Link to processed BAM files from Part 1
#-----------------------------------------------

# Set paths based on your Part 1 analysis location
PART1_DIR="~/wgs_analysis/results/aligned"
SOMATIC_DIR="~/somatic_analysis_matched"

echo "Linking processed BAM files from Part 1..."

cd ${SOMATIC_DIR}/input_data

# Link tumor1 files (BAM and index)
echo "Linking tumor1 files..."
ln -s ${PART1_DIR}/tumor1/tumor1_recalibrated.bam tumor1_recalibrated.bam
ln -s ${PART1_DIR}/tumor1/tumor1_recalibrated.bai tumor1_recalibrated.bai

# Link normal1 files (BAM and index)
echo "Linking normal1 files..."
ln -s ${PART1_DIR}/normal1/normal1_recalibrated.bam normal1_recalibrated.bam
ln -s ${PART1_DIR}/normal1/normal1_recalibrated.bai normal1_recalibrated.bai

cd ${SOMATIC_DIR}
echo "✓ Input data preparation complete!"
```

## Running Mutect2 for Somatic Variant Detection

This is the core step where Mutect2 compares the tumor and normal samples to identify potential somatic mutations. Mutect2 uses sophisticated statistical models to detect mutations that are present in the tumor but absent in the matched normal sample.

### Understanding Mutect2 Key Parameters

Before running the analysis, it’s important to understand the key parameters:

- `-tumor` and `-normal`: Sample names that must match the read group (@RG) SM tags in your BAM files

- `--germline-resource`: Population allele frequencies to help distinguish somatic from germline variants

- `--panel-of-normals`: Database of technical artifacts observed across many samples

- `--f1r2-tar-gz`: Collects read orientation data needed for downstream filtering

```bash
#-----------------------------------------------
# STEP 5: Run Mutect2 for somatic variant detection
#-----------------------------------------------

# Set up variables for clarity and reusability
REFERENCE="~/references/somatic_resources/Homo_sapiens_assembly38.fasta"
GERMLINE_RESOURCE="~/references/somatic_resources/af-only-gnomad.hg38.vcf.gz"
PON="~/references/somatic_resources/1000g_pon.hg38.vcf.gz"
INPUT_DIR="${SOMATIC_DIR}/input_data"
OUTPUT_DIR="${SOMATIC_DIR}/raw_calls"

echo "Running Mutect2 to identify potential somatic mutations..."

# Run Mutect2 to identify potential somatic mutations
gatk Mutect2 \
    -R $REFERENCE \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \
    -I ${INPUT_DIR}/normal1_recalibrated.bam \
    -tumor tumor1 \
    -normal normal1 \
    --germline-resource $GERMLINE_RESOURCE \
    --panel-of-normals $PON \
    --f1r2-tar-gz ${OUTPUT_DIR}/tumor1_f1r2.tar.gz \
    -O ${OUTPUT_DIR}/tumor1_raw.vcf.gz \
    --native-pair-hmm-threads 8 \
    --max-reads-per-alignment-start 50

echo "✓ Mutect2 variant calling complete!"

# Generate basic statistics about the raw calls
echo "Generating call statistics..."
bcftools stats ${OUTPUT_DIR}/tumor1_raw.vcf.gz > ${OUTPUT_DIR}/tumor1_raw_stats.txt
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/bcftools_stats_tumor_raw.png?resize=1024%2C605&ssl=1)

## Assessing Sample Contamination

Cross-sample contamination can significantly affect mutation detection accuracy. This step estimates contamination levels by comparing allele frequencies at common variant positions between tumor and normal samples.

### Generating Pileup Summaries

Pileup summaries count how many reads support each allele at common variant positions. These counts are then used to estimate contamination levels.

```bash
#-----------------------------------------------
# STEP 6: Generate pileup summaries for contamination analysis
#-----------------------------------------------

COMMON_VARIANTS="~/references/somatic_resources/small_exac_common_3.hg38.vcf.gz"
CONTAM_DIR="${SOMATIC_DIR}/contamination_analysis"

echo "Generating pileup summaries for contamination assessment..."

# Generate pileup summary for tumor sample
echo "Processing tumor1 sample..."
gatk GetPileupSummaries \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \
    -V $COMMON_VARIANTS \
    -L $COMMON_VARIANTS \
    -O ${CONTAM_DIR}/tumor1_pileups.table

# Generate pileup summary for normal sample
echo "Processing normal1 sample..."
gatk GetPileupSummaries \
    -I ${INPUT_DIR}/normal1_recalibrated.bam \
    -V $COMMON_VARIANTS \
    -L $COMMON_VARIANTS \
    -O ${CONTAM_DIR}/normal1_pileups.table

echo "✓ Pileup summaries generated successfully!"
```

### Calculating Contamination Estimates

This step compares the tumor and normal pileup data to estimate contamination levels. High contamination (>5%) can significantly impact the sensitivity of mutation detection.

```bash
#-----------------------------------------------
# STEP 7: Calculate contamination estimates
#-----------------------------------------------

echo "Calculating contamination estimates..."

# Calculate contamination by comparing tumor vs normal allele frequencies
gatk CalculateContamination \
    -I ${CONTAM_DIR}/tumor1_pileups.table \
    -matched ${CONTAM_DIR}/normal1_pileups.table \
    -O ${CONTAM_DIR}/tumor1_contamination.table \
    --tumor-segmentation ${CONTAM_DIR}/tumor1_segments.table

echo "✓ Contamination analysis complete!"
```

> 

**Contamination Benchmarks:**

- **<2%:** Excellent quality – no impact on analysis

- **2-5%:** Good quality – minimal impact on sensitivity

- **>5%:** Poor quality – may miss low-frequency mutations

## Comprehensive Filtering Pipeline

Raw Mutect2 calls contain many false positives due to sequencing artifacts, alignment errors, and other technical issues. This multi-step filtering process removes these artifacts while retaining high-confidence somatic mutations.

### Learning Read Orientation Artifacts

Library preparation can introduce systematic sequencing artifacts that appear as false positive mutations. This step trains a model to identify and filter these artifacts.

```bash
#-----------------------------------------------
# STEP 8: Learn read orientation artifacts for filtering
#-----------------------------------------------

echo "Learning read orientation artifacts..."

# Analyze the read orientation data collected during Mutect2 calling
gatk LearnReadOrientationModel \
    -I ${OUTPUT_DIR}/tumor1_f1r2.tar.gz \
    -O ${OUTPUT_DIR}/tumor1_orientation_model.tar.gz

echo "✓ Read orientation model training complete!"
```

### Applying FilterMutectCalls

This is GATK’s comprehensive filtering step that integrates multiple sources of information including contamination estimates, read orientation bias, and statistical confidence scores.

```bash
#-----------------------------------------------
# STEP 9: Apply comprehensive Mutect2 filtering
#-----------------------------------------------

echo "Applying FilterMutectCalls to remove artifacts..."

# Apply GATK's comprehensive filtering to remove false positive calls
gatk FilterMutectCalls \
    -R $REFERENCE \
    -V ${OUTPUT_DIR}/tumor1_raw.vcf.gz \
    --contamination-table ${CONTAM_DIR}/tumor1_contamination.table \
    --tumor-segmentation ${CONTAM_DIR}/tumor1_segments.table \
    --ob-priors ${OUTPUT_DIR}/tumor1_orientation_model.tar.gz \
    -O ${SOMATIC_DIR}/filtered_calls/tumor1_filtered.vcf.gz

echo "✓ FilterMutectCalls complete!"

# Generate filtering statistics
bcftools view -H ${SOMATIC_DIR}/filtered_calls/tumor1_filtered.vcf.gz | cut -f7 | sort | uniq -c | sort -nr
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/mutation_filtering_categories.png?resize=479%2C231&ssl=1)

### Additional Quality Filters

For the highest confidence results, we apply additional stringent filters focusing on allele frequency, coverage depth, and statistical significance.

```bash
#-----------------------------------------------
# STEP 10: Apply additional quality filters for high-confidence calls
#-----------------------------------------------

echo "Applying additional quality filters..."

# Extract only variants that passed FilterMutectCalls
bcftools view -f PASS \
    ${SOMATIC_DIR}/filtered_calls/tumor1_filtered.vcf.gz \
    -O z \
    -o ${SOMATIC_DIR}/filtered_calls/tumor1_pass.vcf.gz

# Apply stringent quality filters for high-confidence calls
# NOTE: bcftools FORMAT subscripts are [sample:value]. Our sample order is
# tumor=0, normal=1 (verify with: bcftools query -l tumor1_pass.vcf.gz).
# The last clause keeps variants absent from the NORMAL sample -> AF[1:0].
bcftools filter \
-i 'FORMAT/AF[0:0] >= 0.05 && FORMAT/DP[0:0] >= 10 && INFO/TLOD >= 6.3 && FORMAT/DP[1:0] >= 10 && (FORMAT/AF[1:0] <= 0.03 || FORMAT/AF[1:0] == ".")' \
${SOMATIC_DIR}/filtered_calls/tumor1_pass.vcf.gz \
-O z \
-o ${SOMATIC_DIR}/filtered_calls/tumor1_high_confidence.vcf.gz

# Index the final high-confidence VCF file
bcftools index -t ${SOMATIC_DIR}/filtered_calls/tumor1_high_confidence.vcf.gz

# Generate final statistics
raw_count=$(bcftools view -H ${OUTPUT_DIR}/tumor1_raw.vcf.gz | wc -l)
pass_count=$(bcftools view -H ${SOMATIC_DIR}/filtered_calls/tumor1_pass.vcf.gz | wc -l)
hc_count=$(bcftools view -H ${SOMATIC_DIR}/filtered_calls/tumor1_high_confidence.vcf.gz | wc -l)

echo ""
echo "Final filtering cascade results:"
echo "  Raw Mutect2 calls: $raw_count"
echo "  PASS calls: $pass_count"
echo "  High-confidence calls: $hc_count"
echo "  Final success rate: $(echo "scale=2; $hc_count * 100 / $raw_count" | bc)%"

echo "✓ Quality filtering complete"

# Statistics for the tumor sample used in this tutorial (example)
# Raw Mutect2 calls: 107878
# PASS calls: 1686
# High-confidence calls: 1453
# Final success rate: 1.34%
```

> 

**Filter Criteria Explained:**

- **AF ≥ 0.05:** Mutation present in ≥5% of tumor reads (detectable threshold)

- **DP ≥ 10:** At least 10 reads covering the position (statistical confidence)

- **TLOD ≥ 6.3:** Strong statistical evidence for somatic mutation

- **Normal AF ≤ 0.03:** Ensures mutation is not present in normal tissue

## Converting Results to Analysis-Ready Formats

VCF files contain comprehensive mutation information but aren’t easily interpretable. This section converts the results into human-readable tables and MAF format for downstream analysis.

### Creating Human-Readable Tables

Using GATK’s VariantsToTable tool, we extract key information from the VCF file into a tab-separated format that can be easily viewed in spreadsheet applications or analyzed programmatically.

```bash
#-----------------------------------------------
# STEP 11: Convert VCF to human-readable tables
#-----------------------------------------------

echo "Converting VCF files to human-readable tables..."

# Use GATK's VariantsToTable for comprehensive data extraction
gatk VariantsToTable \
    -V ${SOMATIC_DIR}/filtered_calls/tumor1_high_confidence.vcf.gz \
    -F CHROM -F POS -F ID -F REF -F ALT -F QUAL -F FILTER \
    -F TLOD -F NLOD -F ECNT \
    -GF GT -GF AD -GF AF -GF DP \
    -O ${SOMATIC_DIR}/converted_tables/tumor1_mutations.tsv

echo "✓ Human-readable table created!"
```

The gatk VariantsToTable command extracts specified fields from a VCF file and organizes them into a user-friendly table.

**Variant-Level Fields (`-F`):**

- **CHROM**: The **chromosome** where the mutation is located.

- **POS**: The genomic **position** of the mutation.

- **ID**: A unique **identifier** for the mutation, if available (e.g., an rsID).

- **REF**: The **reference** allele (the base in the reference genome).

- **ALT**: The **alternate** allele (the mutated base found in the sample).

- **QUAL**: A **quality** score indicating confidence in the variant call; higher is better.

- **FILTER**: The filter status; **`PASS`** means the variant call is high quality.

- **TLOD**: Confidence score that the variant is real in the **tumor**. Higher is better.

- **NLOD**: Confidence score that the variant is real in the **normal** sample. Lower is better for somatic mutations.

- **ECNT**: The count of evidence supporting the variant.

**Genotype-Level Fields (`-GF`):**

- **GT**: The sample’s **genotype** (e.g., `0/1` for heterozygous).

- **AD**: **Allelic Depth**, or the number of reads supporting the reference vs. alternate alleles.

- **AF**: **Allele Fraction**, the percentage of reads that support the alternate allele.

- **DP**: The total read **depth** or coverage at the mutation’s location.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/tumor1_gatk_mutation_table.png?resize=1024%2C70&ssl=1)

## Best Practices and Troubleshooting

### Critical Success Factors

**Sample Quality Requirements:**

- Use high-quality, matched tumor-normal pairs from the same patient

- Ensure adequate coverage depth (>30x for tumor, >15x for normal)

- Verify sample identity and avoid cross-contamination

**Parameter Selection Guidelines:**

- Use conservative filtering thresholds for clinical applications

- Adjust sensitivity based on tumor purity and research goals

- Always include appropriate controls and quality metrics

**Quality Control Monitoring:**

- Monitor contamination levels throughout analysis

- Validate key findings with orthogonal methods when possible

- Document all analysis parameters for reproducibility

### Common Pitfalls to Avoid

**High Contamination****Overly Permissive Filters****Missing Read Groups****Reference Inconsistency**

| Issue | Problem | Solution |
| --- | --- | --- |
|  | >5% contamination reduces sensitivity | Check sample preparation protocol |
|  | Too many false positives | Use stringent filtering for clinical work |
|  | Mutect2 requires proper @RG tags | Verify BAM file headers |
|  | Mixed genome builds cause errors | Ensure all files use same reference |

### When to Use This Approach

**✅ Perfect for:**

- Clinical mutation detection requiring high specificity

- Research studies with matched normal tissue available

- Publication-quality mutation calling

- Precision medicine applications

**⚠️ Consider alternatives for:**

- Archival samples without matched normals (see Part 2B)

- Large population studies where cost is a major factor

- Very low-purity tumor samples (<20% tumor content)

### Expected Results and Interpretation

**Typical Results for High-Quality Samples:**

- Raw calls: 10,000-100,000 variants

- After filtering: 100-10,000 variants

- High-confidence calls: 50-1,000 variants

**Quality Indicators:**

- Contamination <2%

- >80% of variants pass quality filters

- Transition/transversion ratio ~2-3 for SNVs

## Conclusion

Congratulations! You’ve successfully completed a comprehensive matched tumor-normal mutation analysis using the gold standard approach in cancer genomics. This workflow provides the highest specificity and reliability for somatic mutation detection, making it suitable for both research and clinical applications.

The matched tumor-normal approach you’ve mastered represents the clinical gold standard and will serve you well in both research and clinical genomics applications. Your results are now ready for interpretation, visualization, and integration with other genomic data types.

---

## References

1. **Cibulskis, K., et al.** (2013). Sensitive detection of somatic point mutations in impure and heterogeneous cancer samples. *Nature Biotechnology*, 31(3), 213-219. [doi:10.1038/nbt.2514](https://doi.org/10.1038/nbt.2514)

2. **Benjamin, D., et al.** (2019). Calling somatic SNVs and indels with Mutect2. *bioRxiv*. [doi:10.1101/861054](https://doi.org/10.1101/861054)

3. **Van der Auwera, G. A., et al.** (2013). From FastQ data to high-confidence variant calls: the genome analysis toolkit best practices pipeline. *Current Protocols in Bioinformatics*, 43(1), 11-10. [doi:10.1002/0471250953.bi1110s43](https://doi.org/10.1002/0471250953.bi1110s43)

4. **GATK Best Practices Documentation** (2023). Somatic short variant discovery (SNVs + Indels). Broad Institute. [https://gatk.broadinstitute.org/hc/en-us/articles/360035894731](https://gatk.broadinstitute.org/hc/en-us/articles/360035894731)

5. **Li, H., et al.** (2009). The Sequence Alignment/Map format and SAMtools. *Bioinformatics*, 25(16), 2078-2079. [doi:10.1093/bioinformatics/btp352](https://doi.org/10.1093/bioinformatics/btp352)

6. **Ellrott, K., et al.** (2018). Scalable open science approach for mutation calling of tumor exomes using multiple genomic pipelines. *Cell Systems*, 6(3), 271-281. [doi:10.1016/j.cels.2018.03.002](https://doi.org/10.1016/j.cels.2018.03.002)

7. **Nishioka, M., *et al.*** Somatic mutations in the human brain: implications for psychiatric research. *Mol Psychiatry* **24**, 839–856 (2019). [https://doi.org/10.1038/s41380-018-0129-y](https://doi.org/10.1038/s41380-018-0129-y)

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, please **comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.*
