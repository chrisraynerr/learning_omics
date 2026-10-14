# How To Analyze Whole Genome Sequencing Data For Absolute Beginners Part 2B: Unmatched Sample Mutation Calling Strategies

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/)

- [Introduction: Real-World Mutation Calling Challenges](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Introduction_Real-World_Mutation_Calling_Challenges)

  - [Common Unmatched Sample Scenarios](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Common_Unmatched_Sample_Scenarios)
  - [Why This Tutorial Matters](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Why_This_Tutorial_Matters)
  - [Analysis Strategies We’ll Cover](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Analysis_Strategies_Well_Cover)

- [Strategy 1: Pooled Normal Approach](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Strategy_1_Pooled_Normal_Approach)

  - [Understanding the Pooled Normal Concept](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Understanding_the_Pooled_Normal_Concept)
  - [Setting Up for Pooled Normal Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Setting_Up_for_Pooled_Normal_Analysis)
  - [Creating the Pooled Normal BAM](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Creating_the_Pooled_Normal_BAM)
  - [Running Mutect2 with Pooled Normal](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Running_Mutect2_with_Pooled_Normal)
  - [Filtering for Pooled Normal Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Filtering_for_Pooled_Normal_Analysis)

- [Strategy 2: Custom Panel of Normals Approach](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Strategy_2_Custom_Panel_of_Normals_Approach)

  - [Understanding Panel of Normals](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Understanding_Panel_of_Normals)
  - [Creating Custom Panel of Normals](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Creating_Custom_Panel_of_Normals)
  - [Running Tumor-Only Analysis with Custom PON](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Running_Tumor-Only_Analysis_with_Custom_PON)
  - [Filtering for Custom PON Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Filtering_for_Custom_PON_Analysis)

- [Strategy 3: Tumor-Only Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Strategy_3_Tumor-Only_Analysis)

  - [When to Use Tumor-Only Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#When_to_Use_Tumor-Only_Analysis)
  - [Running Tumor-Only Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Running_Tumor-Only_Analysis)
  - [Aggressive Filtering for Tumor-Only](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Aggressive_Filtering_for_Tumor-Only)

- [Converting Results to Analysis-Ready Formats](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Converting_Results_to_Analysis-Ready_Formats)
- [Quality Assessment and Validation Guidelines](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Quality_Assessment_and_Validation_Guidelines)

  - [Expected Variant Counts (WGS, hg38)](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Expected_Variant_Counts_WGS_hg38)
  - [Quality Control Red Flags](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Quality_Control_Red_Flags)
  - [Key Quality Metrics](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Key_Quality_Metrics)
  - [Validation Strategy by Analysis Type](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Validation_Strategy_by_Analysis_Type)

- [Best Practices and Troubleshooting](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Best_Practices_and_Troubleshooting)

  - [Strategy Selection Guidelines](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Strategy_Selection_Guidelines)
  - [Common Issues and Solutions](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Common_Issues_and_Solutions)
  - [Computational Considerations](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Computational_Considerations)

- [Conclusion](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Conclusion)

  - [Key Takeaways](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Key_Takeaways)
  - [Your Somatic Mutation Analysis Journey](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Your_Somatic_Mutation_Analysis_Journey)

- [References](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2b-unmatched-sample-mutation-calling-strategies/#Like_this)

## Introduction: Real-World Mutation Calling Challenges

Welcome to Part 2B of our somatic mutation analysis series! In [Part 2A](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/), we learned the gold standard approach using matched tumor-normal pairs. However, in real-world scenarios, you often face situations where matched normal samples aren’t available.

### Common Unmatched Sample Scenarios

**Clinical Archives:** Historical tumor samples without corresponding normal tissue**Population Studies:** Large cohorts where matched normals are cost-prohibitive**Tissue Constraints:** Limited biopsy material preventing normal collection**Research Collections:** Existing datasets with unmatched sample combinations

### Why This Tutorial Matters

Understanding unmatched sample analysis is crucial because:

- **90% of archival samples** lack matched controls

- **Population studies** often use shared normal references

- **Cost considerations** drive many research designs

- **Clinical applications** sometimes require tumor-only analysis

### Analysis Strategies We’ll Cover

1. **Pooled Normal Approach** – Combining multiple normals into a super-reference

2. **Custom Panel of Normals** – Creating study-specific artifact databases

3. **Tumor-Only Analysis** – Working without any normal controls

Each strategy has specific use cases, advantages, and limitations that we’ll explore in detail.

## Strategy 1: Pooled Normal Approach

**Best for:** 5-20 tumor samples with 3-10 available normal samples**Example:** 10 tumor samples with 5 normal samples from different patients

### Understanding the Pooled Normal Concept

The pooled normal approach creates a “super normal” by combining multiple normal samples. This strategy provides several advantages over individual normal samples:

- **Higher coverage depth** from combined reads

- **Better germline variant representation** across the population

- **Reduced single-sample bias** from any individual normal

- **Consistent reference** for all tumor comparisons

### Setting Up for Pooled Normal Analysis

We’ll create a new analysis directory specifically for the pooled normal approach while maintaining our organized project structure.

```bash
#-----------------------------------------------
# STEP 1: Prepare environment for pooled normal analysis
#-----------------------------------------------

# Activate the WGS data analysis environment from Part 1
# If you haven't completed Part 1, please follow that tutorial first
conda activate wgs_analysis

# Navigate to our analysis directory (continuing from Part 2A)
cd ~/somatic_analysis_matched

# Create directory for pooled normal analysis
mkdir -p pooled_normal_analysis
cd pooled_normal_analysis

# Create subdirectories
mkdir -p {pooled_bam,raw_calls,filtered_calls,contamination,converted_tables,maf_files}

echo "Pooled normal analysis environment ready!"
```

### Creating the Pooled Normal BAM

This step combines multiple normal BAM files into a single, high-coverage normal reference. The merged BAM will have significantly higher depth and better representation of population-level variants.

```bash
#-----------------------------------------------
# STEP 2: Create pooled normal BAM file
#-----------------------------------------------

# List available normal samples (adjust based on your available samples)
# For this example, we'll use normal1 and normal2
INPUT_DIR="~/somatic_analysis_matched/input_data"

# Merge normal BAM files using samtools
samtools merge \
    -@ 8 \                                    # Use 8 threads for faster processing
    pooled_bam/pooled_normal.bam \           # Output pooled BAM
    ${INPUT_DIR}/normal1_recalibrated.bam \
    ${INPUT_DIR}/normal2_recalibrated.bam

# Index the pooled normal BAM
samtools index pooled_bam/pooled_normal.bam

echo "Pooled normal BAM created successfully!"
```

### Running Mutect2 with Pooled Normal

The analysis proceeds similarly to matched tumor-normal calling, but using our pooled normal as the control sample. This approach maintains the specificity benefits of having a normal control while maximizing the available normal tissue data.

```bash
#-----------------------------------------------
# STEP 3: Run Mutect2 using pooled normal as control
#-----------------------------------------------

# Set up reference files (same as Part 2A)
REFERENCE="~/references/somatic_resources/Homo_sapiens_assembly38.fasta"
GERMLINE_RESOURCE="~/references/somatic_resources/af-only-gnomad.hg38.vcf.gz"
PON="~/references/somatic_resources/1000g_pon.hg38.vcf.gz"

# Run Mutect2 comparing tumor1 against pooled normal
gatk Mutect2 \
    -R $REFERENCE \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \       # Tumor sample
    -I pooled_bam/pooled_normal.bam \        # Pooled normal control
    -tumor tumor1 \                          # Tumor sample name
    -normal pooled_normal \                  # Pooled normal sample name
    --germline-resource $GERMLINE_RESOURCE \ # Population frequencies
    --panel-of-normals $PON \                # Technical artifact filter
    --f1r2-tar-gz raw_calls/tumor1_pooled_f1r2.tar.gz \  # Orientation data
    -O raw_calls/tumor1_pooled_raw.vcf.gz \  # Output raw calls
    --native-pair-hmm-threads 8 \            # Use 8 CPU threads
    --max-reads-per-alignment-start 50       # Limit high-coverage regions

echo "Mutect2 with pooled normal complete!"
```

### Filtering for Pooled Normal Analysis

Since the pooled normal isn’t perfectly matched to our tumor sample, we apply slightly more stringent filtering criteria compared to matched analysis to compensate for potential population-level differences.

```bash
#-----------------------------------------------
# STEP 4: Apply filtering optimized for pooled normal approach
#-----------------------------------------------

# Generate contamination estimates (pooled normal approach)
COMMON_VARIANTS="~/references/somatic_resources/small_exac_common_3.hg38.vcf.gz"

# Pileup summary for tumor
gatk GetPileupSummaries \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \
    -V $COMMON_VARIANTS \
    -L $COMMON_VARIANTS \
    -O contamination/tumor1_pileups.table

# Pileup summary for pooled normal
gatk GetPileupSummaries \
    -I pooled_bam/pooled_normal.bam \
    -V $COMMON_VARIANTS \
    -L $COMMON_VARIANTS \
    -O contamination/pooled_normal_pileups.table

# Calculate contamination
gatk CalculateContamination \
    -I contamination/tumor1_pileups.table \
    -matched contamination/pooled_normal_pileups.table \
    -O contamination/tumor1_pooled_contamination.table

# Learn read orientation model
gatk LearnReadOrientationModel \
    -I raw_calls/tumor1_pooled_f1r2.tar.gz \
    -O raw_calls/tumor1_pooled_orientation_model.tar.gz

# Apply FilterMutectCalls
gatk FilterMutectCalls \
    -R $REFERENCE \
    -V raw_calls/tumor1_pooled_raw.vcf.gz \
    --contamination-table contamination/tumor1_pooled_contamination.table \
    --ob-priors raw_calls/tumor1_pooled_orientation_model.tar.gz \
    -O filtered_calls/tumor1_pooled_filtered.vcf.gz

# Extract PASS variants
bcftools view -f PASS \
    filtered_calls/tumor1_pooled_filtered.vcf.gz \
    -O z \
    -o filtered_calls/tumor1_pooled_pass.vcf.gz

# Apply moderate quality filters (less stringent than matched pairs)
# Pooled normals provide good but not perfect germline filtering
bcftools filter \
    -i 'FORMAT/AF[0:0] >= 0.06 && FORMAT/DP[0:0] >= 12 && INFO/TLOD >= 6.3 && (FORMAT/AF[0:1] <= 0.03 || FORMAT/AF[0:1] == ".")' \
    filtered_calls/tumor1_pooled_pass.vcf.gz \
    -O z \
    -o filtered_calls/tumor1_pooled_high_confidence.vcf.gz

# Index final VCF
bcftools index -t filtered_calls/tumor1_pooled_high_confidence.vcf.gz

echo "Quality filter criteria for pooled normal analysis:"
echo "  Tumor AF ≥ 6% (slightly higher than matched)"
echo "  Tumor depth ≥ 12 reads (higher confidence threshold)"
echo "  TLOD ≥ 6.3 (same statistical evidence)"
echo "  Normal AF ≤ 3% (strict germline filtering)"

# Generate final statistics
hc_variants=$(bcftools view -H filtered_calls/tumor1_pooled_high_confidence.vcf.gz | wc -l)
echo "High-confidence variants with pooled normal: $hc_variants"
```

## Strategy 2: Custom Panel of Normals Approach

**Best for:** Studies with 10+ normal samples available**Goal:** Maximum technical artifact removal using study-specific patterns

### Understanding Panel of Normals

A Panel of Normals (PON) is a database of technical artifacts observed across many normal samples. Creating a custom PON from your study samples helps remove study-specific artifacts including:

- **Systematic sequencing errors** that appear across samples

- **Mapping artifacts** in repetitive genomic regions

- **PCR amplification biases** from library preparation

- **Platform-specific errors** from sequencing technology

### Creating Custom Panel of Normals

The process involves running Mutect2 in tumor-only mode on normal samples to catalog all variants (including artifacts), then combining these into a comprehensive artifact database.

```bash
#-----------------------------------------------
# STEP 5: Create custom Panel of Normals from available samples
#-----------------------------------------------

# Create directory for custom PON analysis
mkdir -p custom_pon_analysis
cd custom_pon_analysis
mkdir -p {pon_creation,raw_calls,filtered_calls,converted_tables}

# Step 1: Run Mutect2 in tumor-only mode on each normal sample
# This identifies all variants (including artifacts) in each normal
for normal in normal1 normal2; do
    echo "Processing ${normal} for PON..."

    # Run Mutect2 in tumor-only mode on normal sample
    gatk Mutect2 \
        -R $REFERENCE \
        -I ${INPUT_DIR}/${normal}_recalibrated.bam \
        --max-mnp-distance 0 \              # Disable complex variant calling
        -O pon_creation/${normal}_for_pon.vcf.gz

    echo "${normal} processed for PON"
done

# Step 2: Create the Panel of Normals database
gatk CreateSomaticPanelOfNormals \
    -vcfs pon_creation/normal1_for_pon.vcf.gz \    # Normal sample 1
    -vcfs pon_creation/normal2_for_pon.vcf.gz \    # Normal sample 2
    -O pon_creation/custom_pon.vcf.gz              # Output custom PON

echo "Custom Panel of Normals created!"

# Display PON statistics
pon_sites=$(bcftools view -H pon_creation/custom_pon.vcf.gz | wc -l)
echo "Custom PON contains $pon_sites artifact sites"

# Note: In production, use 40+ normals for effective PON
echo "Note: This PON uses only 2 normals (demo). Production PONs need 40+ samples."
```

### Running Tumor-Only Analysis with Custom PON

With our custom PON created, we can now run tumor-only analysis with enhanced artifact filtering specific to our study’s technical characteristics.

```bash
#-----------------------------------------------
# STEP 6: Run tumor-only analysis with custom PON
#-----------------------------------------------

echo "Running tumor-only analysis with custom Panel of Normals..."

# Run Mutect2 in tumor-only mode using our custom PON
gatk Mutect2 \
    -R $REFERENCE \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \       # Tumor sample only
    -tumor tumor1 \                          # Tumor sample name
    --germline-resource $GERMLINE_RESOURCE \ # Population frequencies
    --panel-of-normals pon_creation/custom_pon.vcf.gz \  # Our custom PON
    --f1r2-tar-gz raw_calls/tumor1_custom_pon_f1r2.tar.gz \
    -O raw_calls/tumor1_custom_pon_raw.vcf.gz \
    --native-pair-hmm-threads 8

echo "Tumor-only calling with custom PON complete!"

# Generate statistics
custom_pon_variants=$(bcftools view -H raw_calls/tumor1_custom_pon_raw.vcf.gz | wc -l)
echo "Raw variants with custom PON: $custom_pon_variants"
```

### Filtering for Custom PON Analysis

The custom PON provides good technical artifact removal, allowing us to use standard filtering criteria while maintaining confidence in our results.

```bash
#-----------------------------------------------
# STEP 7: Apply filtering for custom PON analysis
#-----------------------------------------------

echo "Applying filtering for custom PON approach..."

# Contamination analysis (tumor-only mode)
gatk GetPileupSummaries \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \
    -V $COMMON_VARIANTS \
    -L $COMMON_VARIANTS \
    -O contamination/tumor1_custom_pon_pileups.table

gatk CalculateContamination \
    -I contamination/tumor1_custom_pon_pileups.table \
    -O contamination/tumor1_custom_pon_contamination.table

# Learn read orientation model
gatk LearnReadOrientationModel \
    -I raw_calls/tumor1_custom_pon_f1r2.tar.gz \
    -O raw_calls/tumor1_custom_pon_orientation_model.tar.gz

# Apply FilterMutectCalls
gatk FilterMutectCalls \
    -R $REFERENCE \
    -V raw_calls/tumor1_custom_pon_raw.vcf.gz \
    --contamination-table contamination/tumor1_custom_pon_contamination.table \
    --ob-priors raw_calls/tumor1_custom_pon_orientation_model.tar.gz \
    -O filtered_calls/tumor1_custom_pon_filtered.vcf.gz

# Extract PASS variants
bcftools view -f PASS \
    filtered_calls/tumor1_custom_pon_filtered.vcf.gz \
    -O z \
    -o filtered_calls/tumor1_custom_pon_pass.vcf.gz

# Apply standard quality filters (custom PON provides good artifact removal)
bcftools filter \
    -i 'FORMAT/AF[0:0] >= 0.05 && FORMAT/DP[0:0] >= 10 && INFO/TLOD >= 6.3' \
    filtered_calls/tumor1_custom_pon_pass.vcf.gz \
    -O z \
    -o filtered_calls/tumor1_custom_pon_high_confidence.vcf.gz

# Index final VCF
bcftools index -t filtered_calls/tumor1_custom_pon_high_confidence.vcf.gz

# Final statistics
custom_pon_hc=$(bcftools view -H filtered_calls/tumor1_custom_pon_high_confidence.vcf.gz | wc -l)
echo "High-confidence variants with custom PON: $custom_pon_hc"

# Return to main analysis directory
cd ~/somatic_analysis_matched
```

## Strategy 3: Tumor-Only Analysis

**Best for:** Archival samples with no available normal controls**Limitation:** Higher false positive rate, requires aggressive filtering

### When to Use Tumor-Only Analysis

This approach should be used judiciously, as it has the highest risk of false positives due to the inability to distinguish somatic mutations from germline variants directly.

**Appropriate for:**

- Historical/archival samples

- Rapid screening studies

- Samples with no available normal tissue

- Cost-constrained large population studies

**Use with caution for:**

- Clinical decision-making

- Low-frequency mutation detection

- Publication-quality research requiring high specificity

### Running Tumor-Only Analysis

Without any normal control, we rely heavily on population databases and Panel of Normals to filter germline variants and technical artifacts.

```bash
#-----------------------------------------------
# STEP 8: Tumor-only analysis without normal controls
#-----------------------------------------------

# Create directory for tumor-only analysis
mkdir -p tumor_only_analysis
cd tumor_only_analysis
mkdir -p {raw_calls,filtered_calls,contamination,converted_tables}

# Run Mutect2 in tumor-only mode
gatk Mutect2 \
    -R $REFERENCE \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \       # Tumor sample only
    -tumor tumor1 \                          # Tumor sample name (no normal)
    --germline-resource $GERMLINE_RESOURCE \ # Critical for germline filtering
    --panel-of-normals $PON \                # Use public PON for artifact removal
    --f1r2-tar-gz raw_calls/tumor1_only_f1r2.tar.gz \
    -O raw_calls/tumor1_only_raw.vcf.gz \    # Output raw calls
    --native-pair-hmm-threads 8 \
    --max-mnp-distance 0                     # Disable complex variants

echo "Tumor-only Mutect2 calling complete!"

# Generate statistics
tumor_only_variants=$(bcftools view -H raw_calls/tumor1_only_raw.vcf.gz | wc -l)
echo "Raw tumor-only variants: $tumor_only_variants"
```

### Aggressive Filtering for Tumor-Only

To compensate for the lack of a normal control, we apply very stringent filtering criteria. This reduces sensitivity but maintains acceptable specificity for most applications.

```bash
#-----------------------------------------------
# STEP 9: Apply aggressive filtering for tumor-only analysis
#-----------------------------------------------

echo "Applying aggressive filtering for tumor-only analysis..."

# Contamination analysis (tumor-only - less reliable)
gatk GetPileupSummaries \
    -I ${INPUT_DIR}/tumor1_recalibrated.bam \
    -V $COMMON_VARIANTS \
    -L $COMMON_VARIANTS \
    -O contamination/tumor1_only_pileups.table

gatk CalculateContamination \
    -I contamination/tumor1_only_pileups.table \
    -O contamination/tumor1_only_contamination.table

# Learn read orientation model
gatk LearnReadOrientationModel \
    -I raw_calls/tumor1_only_f1r2.tar.gz \
    -O raw_calls/tumor1_only_orientation_model.tar.gz

# Apply FilterMutectCalls
gatk FilterMutectCalls \
    -R $REFERENCE \
    -V raw_calls/tumor1_only_raw.vcf.gz \
    --contamination-table contamination/tumor1_only_contamination.table \
    --ob-priors raw_calls/tumor1_only_orientation_model.tar.gz \
    -O filtered_calls/tumor1_only_filtered.vcf.gz

# Extract PASS variants
bcftools view -f PASS \
    filtered_calls/tumor1_only_filtered.vcf.gz \
    -O z \
    -o filtered_calls/tumor1_only_pass.vcf.gz

# Apply very stringent quality filters for tumor-only analysis
# Higher thresholds compensate for lack of normal sample
bcftools filter \
    -i 'FORMAT/AF[0:0] >= 0.10 && FORMAT/DP[0:0] >= 20 && INFO/TLOD >= 10.0 && INFO/POPAF < 0.001' \
    filtered_calls/tumor1_only_pass.vcf.gz \
    -O z \
    -o filtered_calls/tumor1_only_high_confidence.vcf.gz

# Index final VCF
bcftools index -t filtered_calls/tumor1_only_high_confidence.vcf.gz

echo "Aggressive filter criteria for tumor-only analysis:"
echo "  Tumor AF ≥ 10% (high frequency threshold)"
echo "  Tumor depth ≥ 20 reads (high confidence requirement)"
echo "  TLOD ≥ 10.0 (very strong statistical evidence)"
echo "  Population AF < 0.1% (aggressive germline filtering)"

# Final statistics
tumor_only_hc=$(bcftools view -H filtered_calls/tumor1_only_high_confidence.vcf.gz | wc -l)
echo "High-confidence tumor-only variants: $tumor_only_hc"

# Return to main analysis directory
cd ~/somatic_analysis_matched

echo "Tumor-only analysis complete!"
```

## Converting Results to Analysis-Ready Formats

Each analysis strategy produces VCF files that need conversion to human-readable formats. The conversion process is identical to Part 2A, using GATK’s VariantsToTable for comprehensive data extraction.

```bash
#-----------------------------------------------
# STEP 10: Convert all strategy results to tables and MAF format
#-----------------------------------------------

# Function to convert VCF to table and MAF
convert_results() {
    local strategy=$1
    local vcf_path=$2
    local output_prefix=$3

    echo "Converting $strategy results..."

    # Convert to human-readable table
    gatk VariantsToTable \
        -V $vcf_path \
        -F CHROM -F POS -F ID -F REF -F ALT -F QUAL -F FILTER \
        -F TLOD -F NLOD -F ECNT \
        -GF GT -GF AD -GF AF -GF DP \
        -O ${output_prefix}.tsv

    # Count mutations
    mutation_count=$(tail -n +2 ${output_prefix}.tsv | wc -l)
    echo "$strategy: $mutation_count high-confidence mutations"
}

# Convert pooled normal results
convert_results "Pooled Normal" \
    "pooled_normal_analysis/filtered_calls/tumor1_pooled_high_confidence.vcf.gz" \
    "pooled_normal_analysis/converted_tables/tumor1_pooled_mutations"

# Convert custom PON results
convert_results "Custom PON" \
    "custom_pon_analysis/filtered_calls/tumor1_custom_pon_high_confidence.vcf.gz" \
    "custom_pon_analysis/converted_tables/tumor1_custom_pon_mutations"

# Convert tumor-only results
convert_results "Tumor-Only" \
    "tumor_only_analysis/filtered_calls/tumor1_only_high_confidence.vcf.gz" \
    "tumor_only_analysis/converted_tables/tumor1_only_mutations"

echo "All results converted to analysis-ready formats!"
```

## Quality Assessment and Validation Guidelines

### Expected Variant Counts (WGS, hg38)

Understanding typical mutation counts helps assess the quality of your analysis:

- **Matched analysis:** 500-5,000 high-confidence mutations

- **Pooled normal:** 400-4,000 high-confidence mutations

- **Custom PON:** 800-8,000 high-confidence mutations

- **Tumor-only:** 100-1,000 high-confidence mutations (after stringent filtering)

### Quality Control Red Flags

Monitor these indicators that suggest potential issues:

- **Too few mutations:** <100 in any strategy (possible over-filtering)

- **Too many mutations:** >10,000 in matched analysis (possible artifacts)

- **High contamination:** >5% cross-sample contamination

- **Low TLOD scores:** Median TLOD <10 suggests poor quality

### Key Quality Metrics

- **Ti/Tv ratio:** Should be 2.0-3.0 for most cancer types

- **VAF distribution:** Should show expected clonal patterns

- **Chromosome distribution:** Should be roughly proportional to chromosome size

### Validation Strategy by Analysis Type

**Matched Analysis:**

- Validation rate: 5-10% of mutations

- Focus: Clinical actionable variants, novel findings

- Methods: Sanger sequencing, digital PCR

**Pooled Normal Analysis:**

- Validation rate: 10-15% of mutations

- Focus: Low-frequency variants, recurrent mutations

- Methods: Sanger sequencing, amplicon sequencing

**Custom PON Analysis:**

- Validation rate: 15-20% of mutations

- Focus: All clinical variants, suspicious patterns

- Methods: Multiple orthogonal methods

**Tumor-Only Analysis:**

- Validation rate: 25-50% of mutations

- Focus: All reported variants if used clinically

- Methods: Comprehensive validation panel

## Best Practices and Troubleshooting

### Strategy Selection Guidelines

| Available Samples | Recommended Strategy | Expected Specificity |
| --- | --- | --- |
| Matched tumor-normal | Part 2A approach | Highest (>95%) |
| 3-10 normals | Pooled normal | High (>90%) |
| 10+ normals | Custom PON | High (>85%) |
| No normals | Tumor-only | Moderate (>70%) |

### Common Issues and Solutions

**Low Mutation Counts:**

- Check filtering parameters are appropriate for your strategy

- Verify tumor purity and sample quality

- Consider tumor type-specific mutation rates

**High False Positive Rate:**

- Increase filtering stringency

- Validate suspicious patterns with orthogonal methods

- Consider creating study-specific PON

**Inconsistent Results Across Strategies:**

- Expected – different strategies have different sensitivity/specificity trade-offs

- Focus on mutations consistently called across multiple strategies

- Use matched analysis as gold standard when available

### Computational Considerations

- **Memory requirements:** Tumor-only < Custom PON < Pooled normal < Matched

- **Processing time:** Similar across all strategies

- **Storage needs:** Plan for intermediate files and multiple strategy outputs

## Conclusion

You’ve now mastered the complete spectrum of somatic mutation calling strategies, from the gold standard matched tumor-normal approach in Part 2A to the practical alternatives when matched normals aren’t available. Each strategy serves specific research scenarios:

**Matched tumor-normal** remains the gold standard for clinical applications and high-impact research. **Pooled normal approaches** provide an excellent balance of specificity and practicality for medium-scale studies. **Custom Panel of Normals** strategies excel when you have sufficient normal samples to create study-specific artifact databases. **Tumor-only analysis** serves as a last resort for archival samples, requiring careful validation.

### Key Takeaways

1. **Strategy selection** should be based on available samples and required specificity

2. **Quality control** is critical for all approaches, with increased importance for unmatched strategies

3. **Validation rates** should increase as you move away from matched analysis

4. **Filtering stringency** must be adjusted based on the analysis strategy

### Your Somatic Mutation Analysis Journey

With Parts 2A and 2B complete, you now have professional-level competency in somatic mutation detection. You can confidently:

- Choose appropriate analysis strategies based on available samples

- Execute multiple mutation calling approaches

- Apply strategy-specific quality control measures

- Generate publication-ready mutation datasets

---

## References

1. **Cibulskis, K., et al.** (2013). Sensitive detection of somatic point mutations in impure and heterogeneous cancer samples. *Nature Biotechnology*, 31(3), 213-219. [doi:10.1038/nbt.2514](https://doi.org/10.1038/nbt.2514)

2. **Benjamin, D., et al.** (2019). Calling somatic SNVs and indels with Mutect2. *bioRxiv*. [doi:10.1101/861054](https://doi.org/10.1101/861054)

3. **Ellrott, K., et al.** (2018). Scalable open science approach for mutation calling of tumor exomes using multiple genomic pipelines. *Cell Systems*, 6(3), 271-281. [doi:10.1016/j.cels.2018.03.002](https://doi.org/10.1016/j.cels.2018.03.002)

4. **Fang, L. T., et al.** (2021). Establishing community reference samples, data and call sets for benchmarking cancer mutation detection using whole-genome sequencing. *Nature Biotechnology*, 39(9), 1151-1160. [doi:10.1038/s41587-021-00993-6](https://doi.org/10.1038/s41587-021-00993-6)

5. **GATK Best Practices Documentation** (2023). Somatic short variant discovery (SNVs + Indels). Broad Institute. [https://gatk.broadinstitute.org/hc/en-us/articles/360035894731](https://gatk.broadinstitute.org/hc/en-us/articles/360035894731)

6. **Chen, Z., et al.** (2022). A survey of somatic mutation calling from next-generation sequencing data. *Computational and Structural Biotechnology Journal*, 20, 892-902. [doi:10.1016/j.csbj.2022.02.013](https://doi.org/10.1016/j.csbj.2022.02.013)

7. **Krøigård, A. B., et al.** (2016). Evaluation of nine somatic variant callers for detection of somatic mutations in exome and targeted deep sequencing data. *PLOS ONE*, 11(3), e0151664. [doi:10.1371/journal.pone.0151664](https://doi.org/10.1371/journal.pone.0151664)

8. **Alioto, T. S., et al.** (2015). A comprehensive assessment of somatic mutation detection in cancer using whole-genome sequencing. *Nature Communications*, 6, 10001. [doi:10.1038/ncomms10001](https://doi.org/10.1038/ncomms10001)

9. **Nishioka, M., *et al.*** Somatic mutations in the human brain: implications for psychiatric research. *Mol Psychiatry* **24**, 839–856 (2019). [https://doi.org/10.1038/s41380-018-0129-y](https://doi.org/10.1038/s41380-018-0129-y)

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, please **comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.*

**Keywords:** unmatched samples, pooled normal, Panel of Normals, tumor-only analysis, somatic mutations, GATK Mutect2, cancer genomics, population studies, archival samples, mutation calling strategies, bioinformatics
