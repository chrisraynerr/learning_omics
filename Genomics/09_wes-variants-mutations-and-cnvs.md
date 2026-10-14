# How To Analyze Whole Exome Sequencing Data For Absolute Beginners: From Raw Reads to High-Quality Variants, Mutations, and CNVs

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/)

- [Introduction: Understanding Whole Exome Sequencing vs. Whole Genome Sequencing](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Introduction_Understanding_Whole_Exome_Sequencing_vs_Whole_Genome_Sequencing)

  - [What is Whole Exome Sequencing (WES)?](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#What_is_Whole_Exome_Sequencing_WES)
  - [Key Differences Between WGS and WES](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Key_Differences_Between_WGS_and_WES)
  - [When to Choose WES vs. WGS](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#When_to_Choose_WES_vs_WGS)
  - [The WES Workflow: What’s Different?](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#The_WES_Workflow_Whats_Different)

- [Prerequisites and Environment Setup](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Prerequisites_and_Environment_Setup)

  - [Required Background](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Required_Background)
  - [WES-Specific Software Installation](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#WES-Specific_Software_Installation)
  - [WES Project Directory Structure](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#WES_Project_Directory_Structure)

- [Key Difference #1: Target Region Files and Reference Preparation](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Key_Difference_1_Target_Region_Files_and_Reference_Preparation)

  - [Understanding Capture Technologies](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Understanding_Capture_Technologies)
  - [Understanding Target Region Formats](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Understanding_Target_Region_Formats)

- [Key Difference #2: WES-Specific Quality Control Metrics](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Key_Difference_2_WES-Specific_Quality_Control_Metrics)

  - [Standard QC Steps](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Standard_QC_Steps)
  - [WES-Specific Quality Metrics](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#WES-Specific_Quality_Metrics)
  - [Understanding WES Quality Metrics](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Understanding_WES_Quality_Metrics)

- [Key Difference #3: Targeted Variant Calling](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Key_Difference_3_Targeted_Variant_Calling)

  - [HaplotypeCaller with Target Focus](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#HaplotypeCaller_with_Target_Focus)
  - [WES-Optimized Variant Filtering](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#WES-Optimized_Variant_Filtering)
  - [Multi-Sample WES Variant Calling](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Multi-Sample_WES_Variant_Calling)

- [Key Difference #4: Somatic Mutation Calling in WES](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Key_Difference_4_Somatic_Mutation_Calling_in_WES)

  - [Tumor-Normal Mutation Calling](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Tumor-Normal_Mutation_Calling)
  - [Advantages of WES for Somatic Mutation Detection](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Advantages_of_WES_for_Somatic_Mutation_Detection)

- [Variant Annotation: Same as WGS Analysis](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Variant_Annotation_Same_as_WGS_Analysis)

  - [WES Annotation Advantages](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#WES_Annotation_Advantages)

- [Key Difference #5: CNV Detection in WES Data](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Key_Difference_5_CNV_Detection_in_WES_Data)

  - [Understanding WES CNV Challenges](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Understanding_WES_CNV_Challenges)
  - [CNV Detection Using CNVkit](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#CNV_Detection_Using_CNVkit)
  - [Alternative WES CNV Tools](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Alternative_WES_CNV_Tools)

- [Clinical Interpretation: WES vs. WGS Considerations](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Clinical_Interpretation_WES_vs_WGS_Considerations)

  - [Understanding WES Diagnostic Capabilities](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Understanding_WES_Diagnostic_Capabilities)

- [Best Practices for WES Analysis](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Best_Practices_for_WES_Analysis)

  - [Quality Control Standards](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Quality_Control_Standards)
  - [Common Pitfalls and Solutions](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Common_Pitfalls_and_Solutions)

- [Conclusion](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Conclusion)

  - [Key Advantages of WES](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Key_Advantages_of_WES)
  - [When to Consider WGS Instead](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#When_to_Consider_WGS_Instead)

- [References](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-exome-sequencing-data-from-raw-reads-to-high-quality-variants-mutations-and-cnvs/#Like_this)

## Introduction: Understanding Whole Exome Sequencing vs. Whole Genome Sequencing

### What is Whole Exome Sequencing (WES)?

Whole Exome Sequencing (WES) is a targeted sequencing approach that focuses specifically on the protein-coding regions of the genome, known as exons. While the human genome contains approximately 3.2 billion base pairs, the exome represents only about 1-2% of this total (~30-50 million base pairs). However, this small fraction contains roughly 85% of known disease-causing variants, making WES an extremely powerful and cost-effective approach for medical genomics.

> 

**Think of it this way:** If the genome were a massive encyclopedia, WES would be like reading only the most important chapters that contain the core information, while WGS reads every single page including footnotes, appendices, and blank spaces.

### Key Differences Between WGS and WES

Understanding these fundamental differences is crucial for choosing the right approach and interpreting results correctly:

**Target Coverage****Cost per Sample****Data Size****Typical Coverage Depth****Analysis Time****Variants Detected****CNV Detection****Regulatory Variants****Clinical Diagnostic Yield**

| Aspect | Whole Genome Sequencing (WGS) | Whole Exome Sequencing (WES) |
| --- | --- | --- |
|  | Entire genome (~3.2 billion bp) | Exons only (~30-50 million bp) |
|  | $800-2,000 | $200-500 |
|  | 30-100 GB per sample | 2-8 GB per sample |
|  | 30-50x | 100-200x |
|  | 8-24 hours | 2-6 hours |
|  | 4-5M SNVs, 0.5-1M indels | 20-25K coding variants |
|  | Excellent (all sizes) | Limited (exon-level resolution) |
|  | All regulatory regions | Misses most regulatory variants |
|  | 35-40% | 25-30% |

### When to Choose WES vs. WGS

**Choose WES when:**

- **Budget is a primary concern** – WES costs 3-4x less than WGS

- **Suspected Mendelian disorders** – Most are caused by coding variants

- **Large population studies** – Cost savings enable larger sample sizes

- **Clinical diagnostics** focused on known disease genes

- **Rapid turnaround needed** – Smaller datasets process faster

- **Pharmacogenomics** – Most drug-related variants are in coding regions

**Choose WGS when:**

- **Comprehensive variant discovery** is required

- **Structural variants and CNVs** are important

- **Regulatory variants** may be disease-relevant

- **Previous WES was negative** but genetic cause is still suspected

- **Research applications** requiring complete genomic picture

- **Complex inheritance patterns** need investigation

### The WES Workflow: What’s Different?

This tutorial focuses specifically on the differences between WES and WGS analysis. The fundamental steps remain similar, but key modifications include:

1. **Target Region Definition** – Using capture kit target files

2. **Quality Control** – Emphasis on on-target rate and coverage uniformity

3. **Variant Calling** – Restricted to captured regions for efficiency

4. **Somatic Mutation Calling** – Optimized for cancer genomics applications

5. **CNV Detection** – Specialized approaches for targeted data

6. **Interpretation** – Focus on protein-coding consequences

> 

**Important:** This tutorial assumes you’ve already reviewed our [WGS analysis series](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/). We’ll reference those tutorials and focus only on WES-specific modifications.

## Prerequisites and Environment Setup

### Required Background

Before starting this WES tutorial, ensure you have:

1. **Completed our WGS analysis foundation:** [Part 1: From Raw Reads to High-Quality Variants Using GATK](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/)

2. **Basic understanding of variant calling** from the WGS tutorial

3. **Familiarity with GATK tools** and command-line operations

4. **The conda environment** (`wgs_analysis`) from Part 1 already set up

### WES-Specific Software Installation

```bash
# Activate the existing WGS analysis environment from Part 1
conda activate wgs_analysis
```

### WES Project Directory Structure

```bash
# Create organized directory structure for WES analysis
mkdir -p ~/wes_analysis/{data,references,results,scripts}
mkdir -p ~/wes_analysis/results/{qc,trimmed,aligned,variants,somatic,cnv,annotation}

# Display the working directory structure
tree ~/wes_analysis -L 2

# Navigate to working directory
cd ~/wes_analysis
```

## Key Difference #1: Target Region Files and Reference Preparation

The most fundamental difference in WES analysis is the use of **target region files** that define which genomic regions were captured during library preparation.

### Understanding Capture Technologies

WES uses biotinylated probes (baits) to selectively capture exonic regions before sequencing. Different capture kits target slightly different sets of exons:

- **Agilent SureSelect** – Uses RNA baits, excellent uniformity

- **Illumina Nextera** – DNA baits, good for degraded samples

- **Roche SeqCap** – DNA baits, customizable designs

- **Twist Bioscience** – Synthetic DNA baits, latest technology

> 

**Critical Point:** Always use the exact target region file that matches your capture kit. Using the wrong target file will lead to incorrect quality metrics and suboptimal results.

### Understanding Target Region Formats

Different tools require different formats. Here’s what each file contains:

**BED Format (exome_targets.bed):**

```
chr1    69090   70008   
chr1    367658  368597
chr1    621095  622034
```

**GATK Interval List Format (exome_targets.interval_list):**

```bash
# Create interval list for GATK (required format for some tools)
gatk BedToIntervalList \
    -I exome_targets.bed \
    -O exome_targets.interval_list \
    -SD ~/wgs_analysis/reference/Homo_sapiens_assembly38.dict

# GATK Interval List Format
# @HD    VN:1.6
# @SQ    SN:chr1 LN:248956422
# chr1:69091-70008
# chr1:367659-368597
```

## Key Difference #2: WES-Specific Quality Control Metrics

WES quality control includes all standard metrics from WGS plus several targeted sequencing-specific metrics.

### Standard QC Steps

The initial quality control steps are identical to our [WGS tutorial Part 1](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/).

```bash
# Set up variables for WES analysis
SAMPLE="exome_sample1"
TARGET_REGIONS="~/wes_analysis/references/exome_targets.bed"
REFERENCE="~/wgs_analysis/reference/Homo_sapiens_assembly38.fasta"
THREADS=16

# FastQC analysis - identical to WGS workflow
fastqc \
    ~/wes_analysis/data/${SAMPLE}_R1.fastq.gz \
    ~/wes_analysis/data/${SAMPLE}_R2.fastq.gz \
    -o ~/wes_analysis/results/qc/

# Adapter trimming - same parameters as WGS
trim_galore \
    --paired \
    --quality 20 \
    --length 50 \
    --fastqc \
    --output_dir ~/wes_analysis/results/trimmed/ \
    ~/wes_analysis/data/${SAMPLE}_R1.fastq.gz \
    ~/wes_analysis/data/${SAMPLE}_R2.fastq.gz

# Alignment - same BWA-MEM algorithm as WGS
bwa mem \
    -t ${THREADS} \
    -M \
    -R "@RG\tID:${SAMPLE}\tSM:${SAMPLE}\tPL:ILLUMINA\tLB:${SAMPLE}_lib" \
    ${REFERENCE} \
    ~/wes_analysis/results/trimmed/${SAMPLE}_R1_val_1.fq.gz \
    ~/wes_analysis/results/trimmed/${SAMPLE}_R2_val_2.fq.gz \
    | samtools sort -@ ${THREADS} -o ~/wes_analysis/results/aligned/${SAMPLE}.bam

# Mark duplicates - identical to WGS workflow
gatk MarkDuplicates \
    -I ~/wes_analysis/results/aligned/${SAMPLE}.bam \
    -O ~/wes_analysis/results/aligned/${SAMPLE}_marked_duplicates.bam \
    -M ~/wes_analysis/results/aligned/${SAMPLE}_duplicate_metrics.txt \
    --CREATE_INDEX true

# Base quality score recalibration - same as WGS
gatk BaseRecalibrator \
    -I ~/wes_analysis/results/aligned/${SAMPLE}_marked_duplicates.bam \
    -R ${REFERENCE} \
    --known-sites ~/wgs_analysis/reference/Homo_sapiens_assembly38.dbsnp138.vcf.gz \
    --known-sites ~/wgs_analysis/reference/Mills_and_1000G_gold_standard.indels.hg38.vcf.gz \
    -O ~/wes_analysis/results/aligned/${SAMPLE}_recal_data.table

gatk ApplyBQSR \
    -I ~/wes_analysis/results/aligned/${SAMPLE}_marked_duplicates.bam \
    -R ${REFERENCE} \
    --bqsr-recal-file ~/wes_analysis/results/aligned/${SAMPLE}_recal_data.table \
    -O ~/wes_analysis/results/aligned/${SAMPLE}_recalibrated.bam
```

### WES-Specific Quality Metrics

Now we add the targeted sequencing-specific quality assessments:

```bash
# Calculate on-target rate and coverage statistics
gatk CollectHsMetrics \
    -I ~/wes_analysis/results/aligned/${SAMPLE}_recalibrated.bam \
    -O ~/wes_analysis/results/qc/${SAMPLE}_hs_metrics.txt \
    -R ${REFERENCE} \
    -BAIT_INTERVALS ~/wes_analysis/references/exome_targets.interval_list \
    -TARGET_INTERVALS ~/wes_analysis/references/exome_targets.interval_list

# Calculate detailed coverage statistics per target
gatk DepthOfCoverage \
    -R ${REFERENCE} \
    -I ~/wes_analysis/results/aligned/${SAMPLE}_recalibrated.bam \
    -O ~/wes_analysis/results/qc/${SAMPLE}_coverage \
    -L ${TARGET_REGIONS} \
    --summaryCoverageThreshold 10 \
    --summaryCoverageThreshold 20 \
    --summaryCoverageThreshold 30 \
    --summaryCoverageThreshold 50 \
    --summaryCoverageThreshold 100
```

### Understanding WES Quality Metrics

**Critical WES-Specific Metrics:**

1. **ON_TARGET_BASES:** Percentage of sequenced bases that fall within target regions

- **Excellent:** >80%

- **Good:** 70-80%

- **Poor:** <70%

2. **MEAN_TARGET_COVERAGE:** Average coverage depth across target regions

- **Clinical applications:** >100x

- **Research applications:** >50x

3. **PCT_TARGET_BASES_10X/20X/30X:** Percentage of targets covered at various depths

- **Clinical quality:** >95% at 20x coverage

4. **FOLD_ENRICHMENT:** How much more coverage targets receive vs. genome average

- **Good enrichment:** >40-fold

- **Poor enrichment:** <20-fold

5. **AT_DROPOUT and GC_DROPOUT:** Coverage uniformity across different GC content

- **Good uniformity:** <10% dropout

- **Problematic:** >20% dropout

## Key Difference #3: Targeted Variant Calling

While the variant calling algorithms remain the same, WES analysis restricts calling to target regions for efficiency and accuracy.

### HaplotypeCaller with Target Focus

```bash
# HaplotypeCaller with target region restriction (much faster than WGS)
gatk HaplotypeCaller \
    -R ${REFERENCE} \
    -I ~/wes_analysis/results/aligned/${SAMPLE}_recalibrated.bam \
    -O ~/wes_analysis/results/variants/${SAMPLE}.g.vcf.gz \
    -ERC GVCF \
    -L ${TARGET_REGIONS} \
    --dbsnp ~/wgs_analysis/reference/Homo_sapiens_assembly38.dbsnp138.vcf.gz

# Convert GVCF to VCF for single-sample analysis
gatk GenotypeGVCFs \
    -R ${REFERENCE} \
    -V ~/wes_analysis/results/variants/${SAMPLE}.g.vcf.gz \
    -O ~/wes_analysis/results/variants/${SAMPLE}_raw_variants.vcf.gz \
    -L ${TARGET_REGIONS}
```

### WES-Optimized Variant Filtering

Higher coverage in WES allows for more stringent quality filters:

```bash
# Apply variant filters optimized for high-coverage WES data
gatk VariantFiltration \
    -R ${REFERENCE} \
    -V ~/wes_analysis/results/variants/${SAMPLE}_raw_variants.vcf.gz \
    --filter-expression "QD < 2.0" --filter-name "LowQD" \
    --filter-expression "FS > 60.0" --filter-name "HighFS" \
    --filter-expression "MQ < 40.0" --filter-name "LowMQ" \
    --filter-expression "SOR > 3.0" --filter-name "HighSOR" \
    --filter-expression "MQRankSum < -12.5" --filter-name "LowMQRankSum" \
    --filter-expression "ReadPosRankSum < -8.0" --filter-name "LowReadPosRankSum" \
    --filter-expression "DP < 20" --filter-name "LowDepth" \
    --filter-expression "DP > 500" --filter-name "HighDepth" \
    -O ~/wes_analysis/results/variants/${SAMPLE}_filtered.vcf.gz

# Extract high-quality variants
bcftools view -f PASS -Oz \
    -o ~/wes_analysis/results/variants/${SAMPLE}_pass.vcf.gz \
    ~/wes_analysis/results/variants/${SAMPLE}_filtered.vcf.gz

bcftools index ~/wes_analysis/results/variants/${SAMPLE}_pass.vcf.gz
```

### Multi-Sample WES Variant Calling

For cohort analysis, joint calling is even more beneficial in WES due to higher coverage depth:

```bash
# Create sample list
ls ~/wes_analysis/results/variants/*.g.vcf.gz > gvcf_list.txt

# Combine GVCFs (faster due to target restriction)
gatk CombineGVCFs \
    -R ${REFERENCE} \
    --variant gvcf_list.txt \
    -L ${TARGET_REGIONS} \
    -O ~/wes_analysis/results/variants/cohort.g.vcf.gz

# Joint genotyping across the cohort
gatk GenotypeGVCFs \
    -R ${REFERENCE} \
    -V ~/wes_analysis/results/variants/cohort.g.vcf.gz \
    -L ${TARGET_REGIONS} \
    -O ~/wes_analysis/results/variants/cohort_raw_variants.vcf.gz

# Apply cohort-level filtering
gatk VariantFiltration \
    -R ${REFERENCE} \
    -V ~/wes_analysis/results/variants/cohort_raw_variants.vcf.gz \
    --filter-expression "QD < 2.0" --filter-name "LowQD" \
    --filter-expression "FS > 60.0" --filter-name "HighFS" \
    --filter-expression "MQ < 40.0" --filter-name "LowMQ" \
    -O ~/wes_analysis/results/variants/cohort_filtered.vcf.gz
```

## Key Difference #4: Somatic Mutation Calling in WES

Somatic mutation detection follows the same Mutect2 approach as our [tumor-normal mutation calling tutorial](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/), but restricted to captured regions.

### Tumor-Normal Mutation Calling

#### Creating Your Panel of Normals

```bash
# Define variables for somatic analysis
TUMOR_SAMPLE="tumor_sample_001"
NORMAL_SAMPLE="normal_sample_001"
GNOMAD="~/references/somatic_resources/af-only-gnomad.hg38.vcf.gz"

#=============================================================================
# CHOOSE YOUR PANEL OF NORMALS STRATEGY
#=============================================================================

# Option 1: Use Public PoN (RECOMMENDED for beginners and small studies)
# - Easier to implement
# - High quality from 1000+ samples
# - Good for most applications
PON="~/references/somatic_resources/1000g_pon.hg38.vcf.gz"

# Option 2: Create Custom PoN (ADVANCED - only if you have ≥40 normal samples)
# - Better for large research studies
# - Can be optimized for your specific capture kit
# - Requires significant computational resources
# See detailed workflow below if you choose this option

#=============================================================================
# ADVANCED: Custom PoN Creation (skip if using public PoN)
#=============================================================================

# Only proceed if you have ≥40 normal samples from the same capture kit
# Create output directory
mkdir -p ~/wes_analysis/results/somatic/pon_vcfs

# Step 1: Run Mutect2 on each normal sample in tumor-only mode
for NORMAL_BAM in ~/wes_analysis/results/aligned/normal*.bam; do
    NORMAL_ID=$(basename ${NORMAL_BAM} _recalibrated.bam)

    gatk Mutect2 \
        -R ${REFERENCE} \
        -I ${NORMAL_BAM} \
        -L ${TARGET_REGIONS} \
        --max-mnp-distance 0 \
        -O ~/wes_analysis/results/somatic/pon_vcfs/${NORMAL_ID}.vcf.gz
done

# Step 2: Create genomicsDB workspace
gatk GenomicsDBImport \
    -R ${REFERENCE} \
    -L ${TARGET_REGIONS} \
    --genomicsdb-workspace-path ~/wes_analysis/results/somatic/pon_db \
    --merge-input-intervals \
    -V ~/wes_analysis/results/somatic/pon_vcfs/*.vcf.gz

# Step 3: Create the final custom PoN
gatk CreateSomaticPanelOfNormals \
    -R ${REFERENCE} \
    -V gendb://~/wes_analysis/results/somatic/pon_db \
    -O ~/wes_analysis/results/somatic/custom_pon.vcf.gz

# Use custom PoN
PON="~/wes_analysis/results/somatic/custom_pon.vcf.gz"
```

#### Somatic Mutation Calling Using Mutect2

```bash
# Call somatic mutations with Mutect2
gatk Mutect2 \
    -R ${REFERENCE} \
    -I ~/wes_analysis/results/aligned/${TUMOR_SAMPLE}_recalibrated.bam \
    -I ~/wes_analysis/results/aligned/${NORMAL_SAMPLE}_recalibrated.bam \
    -normal ${NORMAL_SAMPLE} \
    --germline-resource ${GNOMAD} \
    --panel-of-normals ${PON} \
    -L ${TARGET_REGIONS} \
    -O ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_vs_${NORMAL_SAMPLE}_somatic.vcf.gz \
    -f1r2-tar-gz ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_f1r2.tar.gz

# Learn Read Orientation Model (filters orientation bias artifacts)
gatk LearnReadOrientationModel \
    -I ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_f1r2.tar.gz \
    -O ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_read_orientation_model.tar.gz

# Get Pileup Summaries for contamination estimation
gatk GetPileupSummaries \
    -I ~/wes_analysis/results/aligned/${TUMOR_SAMPLE}_recalibrated.bam \
    -V ${GNOMAD} \
    -L ${TARGET_REGIONS} \
    -O ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_pileups.table

gatk GetPileupSummaries \
    -I ~/wes_analysis/results/aligned/${NORMAL_SAMPLE}_recalibrated.bam \
    -V ${GNOMAD} \
    -L ${TARGET_REGIONS} \
    -O ~/wes_analysis/results/somatic/${NORMAL_SAMPLE}_pileups.table

# Calculate contamination
gatk CalculateContamination \
    -I ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_pileups.table \
    -matched ~/wes_analysis/results/somatic/${NORMAL_SAMPLE}_pileups.table \
    -O ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_contamination.table \
    --tumor-segmentation ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_segments.table

# Filter Mutect2 calls
gatk FilterMutectCalls \
    -R ${REFERENCE} \
    -V ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_vs_${NORMAL_SAMPLE}_somatic.vcf.gz \
    --contamination-table ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_contamination.table \
    --tumor-segmentation ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_segments.table \
    --ob-priors ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_read_orientation_model.tar.gz \
    -L ${TARGET_REGIONS} \
    -O ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_vs_${NORMAL_SAMPLE}_filtered.vcf.gz

# Extract PASS variants only
gatk SelectVariants \
    -R ${REFERENCE} \
    -V ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_vs_${NORMAL_SAMPLE}_filtered.vcf.gz \
    --exclude-filtered \
    -O ~/wes_analysis/results/somatic/${TUMOR_SAMPLE}_vs_${NORMAL_SAMPLE}_filtered_PASS.vcf.gz
```

### Advantages of WES for Somatic Mutation Detection

**Benefits of WES in cancer analysis:**

- **Higher sensitivity** for low-frequency mutations due to increased depth

- **Reduced false positives** from repetitive/low-complexity regions

- **Faster analysis** enables rapid clinical turnaround

- **Cost-effective** for large cancer cohorts

- **Focused interpretation** on actionable coding mutations

**Limitations compared to WGS:**

- **Misses regulatory mutations** in promoters and enhancers

- **Cannot detect** most structural variants

- **Limited CNV detection** to exon-level changes

- **Splice site coverage** may be incomplete

## Variant Annotation: Same as WGS Analysis

Variant annotation for WES follows exactly the same process as our comprehensive [Part 3: Annotating SNVs and Mutations with Multiple Tools](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/) tutorial.

The annotation tools (GATK Funcotator, Ensembl VEP, SnpEff, and ANNOVAR) work identically on VCF files regardless of whether the variants come from WGS or WES data. The key steps remain:

1. **Functional Impact Prediction** – Effect on protein coding sequences

2. **Population Frequency Annotation** – Adding gnomAD and 1000G frequencies

3. **Clinical Significance** – ClinVar pathogenicity classifications

4. **Pathogenicity Scoring** – SIFT, PolyPhen, CADD predictions

### WES Annotation Advantages

Since WES focuses on protein-coding regions, annotation is often more straightforward:

- **Higher proportion of functionally relevant variants** (most are in exons)

- **Better pathogenicity predictions** (tools trained primarily on coding variants)

- **Clearer clinical interpretation** (established for coding mutations)

- **Faster processing** (fewer variants to annotate)

> 

**Recommendation:** Follow [Part 3 annotation tutorial](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/) using your filtered WES VCF files as input. All commands and databases work identically.

## Key Difference #5: CNV Detection in WES Data

CNV detection from WES data requires specialized approaches due to the targeted nature of the sequencing and discrete coverage patterns.

### Understanding WES CNV Challenges

**Technical Challenges:**

- **Capture bias:** Different exons have vastly different capture efficiencies

- **Probe design:** Some exons are difficult to capture due to sequence composition

- **Coverage gaps:** Poorly captured exons create false CNV signals

- **Normalization complexity:** Harder to distinguish true CNVs from technical artifacts

**What WES CNV Analysis Can Detect:**

- **Large gene deletions/duplications** (>50kb, multiple exons)

- **Whole gene copy number changes**

- **Multi-exon CNVs** with clear coverage patterns

- **Dosage-sensitive gene alterations**

### CNV Detection Using CNVkit

```bash
# Create directory for CNV analysis
mkdir -p ~/wes_analysis/results/cnv
cd ~/wes_analysis/results/cnv

# Define sample paths
NORMAL_SAMPLES=(
    "~/wes_analysis/results/aligned/exome_normal1_recalibrated.bam"
    "~/wes_analysis/results/aligned/exome_normal2_recalibrated.bam"
    "~/wes_analysis/results/aligned/exome_normal3_recalibrated.bam"
)

TEST_SAMPLES=(
    "~/wes_analysis/results/aligned/exome_sample1_recalibrated.bam"
    "~/wes_analysis/results/aligned/exome_sample2_recalibrated.bam"
)

# Single command WES CNV analysis with target region specification
cnvkit.py batch \
    ${TEST_SAMPLES[@]} \
    --normal ${NORMAL_SAMPLES[@]} \
    --targets ${TARGET_REGIONS} \
    --fasta ${REFERENCE} \
    --annotate ~/references/cnv_references/refGene.bed \
    --output-reference wes_reference.cnn \
    --output-dir cnv_results/ \
    --method hybrid \
    --segment-method cbs \
    --drop-low-coverage \
    --scatter \
    --diagram

# Alternative: Auto-detect targets (when target file unavailable)
cnvkit.py batch \
    ${TEST_SAMPLES[@]} \
    --normal ${NORMAL_SAMPLES[@]} \
    --fasta ${REFERENCE} \
    --annotate ~/references/cnv_references/refGene.bed \
    --output-reference wes_auto_reference.cnn \
    --output-dir cnv_auto_results/ \
    --method hybrid \
    --target-avg-size 267 \
    --segment-method cbs \
    --scatter \
    --diagram
```

### Alternative WES CNV Tools

**ExomeDepth:** Specifically designed for WES CNV detection

- Accounts for exon-to-exon coverage variation

- Uses matched reference samples from same capture kit

- Lower false positive rate than general CNV tools

- Widely used in clinical exome sequencing

**CODEX:** Another WES-specific CNV caller

- Designed for whole exome and targeted sequencing

- Uses negative binomial model for coverage

- Good performance with small sample sizes

**XHMM:** Hidden Markov Model approach

- Specifically for exome CNV detection

- Good at handling coverage biases

- Requires larger sample sizes for optimal performance

## Clinical Interpretation: WES vs. WGS Considerations

### Understanding WES Diagnostic Capabilities

**What WES Can Diagnose:**

- **Mendelian disorders** caused by coding mutations (~85% of known disease variants)

- **Loss-of-function variants** (nonsense, frameshift, splice site)

- **Missense variants** in critical functional domains

- **Large CNVs** affecting entire genes (with limitations)

- **Pharmacogenomic variants** in drug metabolism genes

**What WES Cannot Diagnose:**

- **Regulatory variants** in promoters, enhancers (~15% of disease variants)

- **Deep intronic variants** affecting splicing

- **Structural variants** not disrupting exons

- **Repeat expansions** (Huntington’s, fragile X, etc.)

- **Balanced chromosomal rearrangements**

- **Mosaic variants** at low levels (<20-30%)

## Best Practices for WES Analysis

### Quality Control Standards

**Coverage Requirements:**

- **Mean target coverage:** >100x for clinical applications, >50x for research

- **Coverage uniformity:** >95% of targets at 20x depth

- **On-target rate:** >80% for optimal results

**Performance Benchmarks:**

- **Variant calling sensitivity:** >95% for variants >20x coverage

- **Specificity:** >99% after proper filtering

- **Reproducibility:** >98% concordance between technical replicates

### Common Pitfalls and Solutions

**Technical Issues:**

1. **Low On-Target Rate (<70%)**

- **Causes:** Wrong target file, poor capture, sample degradation

- **Solutions:** Verify target file, check capture kit lot, assess DNA quality

- **Prevention:** Use exact manufacturer target files, validate protocols

2. **Uneven Coverage Across Targets**

- **Causes:** GC bias, capture probe efficiency, PCR artifacts

- **Solutions:** GC bias correction, optimize PCR conditions

- **Monitoring:** Track coverage uniformity metrics

3. **High False Positive Rate**

- **Causes:** Insufficient filtering, capture artifacts, systematic biases

- **Solutions:** Apply WES-specific filters, use population databases

- **Validation:** Confirm suspicious variants with Sanger sequencing

**Analytical Issues:**

1. **Missing Expected Variants**

- **Causes:** Low coverage in specific exons, variant in non-captured regions

- **Solutions:** Manual review of coverage, consider WGS for negative cases

- **Prevention:** Review capture kit content for genes of interest

2. **CNV False Positives**

- **Causes:** Capture bias, reference sample selection, systematic artifacts

- **Solutions:** Use WES-specific CNV tools, validate with orthogonal methods

- **Prevention:** Include adequate normal samples in reference

3. **Batch Effects**

- **Causes:** Different capture lots, library prep variations, sequencing platforms

- **Solutions:** Batch correction methods, standardized protocols

- **Prevention:** Process samples consistently, track batch information

4. **Somatic Mutation Artifacts**

- **Causes:** DNA degradation, FFPE artifacts, low tumor purity

- **Solutions:** Optimize filtering parameters, use orientation bias models

- **Prevention:** Use high-quality samples, validate tumor content

## Conclusion

Whole Exome Sequencing represents a powerful, cost-effective approach to genomic analysis that has revolutionized clinical genetics. While WES cannot replace WGS for comprehensive genomic analysis, it provides an excellent balance of coverage depth, cost efficiency, and clinical relevance for many applications.

### Key Advantages of WES

- **Cost-effectiveness** for large-scale studies and clinical applications

- **High coverage depth** enabling detection of rare variants

- **Focused analysis** on protein-coding regions with known disease relevance

- **Faster processing** and interpretation compared to WGS

- **Established clinical workflows** and interpretation guidelines

### When to Consider WGS Instead

- Previous WES analysis was negative but genetic cause still suspected

- Regulatory variants or structural variants are clinically relevant

- Comprehensive variant discovery is required for research

- Non-coding regions need investigation for specific phenotypes

As sequencing costs continue to decrease and technologies advance, the choice between WES and WGS will increasingly depend on specific research questions, clinical requirements, and interpretation capabilities rather than cost alone. Both approaches remain valuable tools in the genomicist’s toolkit, each with distinct advantages for different applications.

## References

1. **Bamshad MJ, et al. (2011).** Exome sequencing as a tool for Mendelian disease gene discovery. *Nature Reviews Genetics*, 12(11):745-55. doi:10.1038/nrg3031

2. **Yang Y, et al. (2013).** Clinical whole-exome sequencing for the diagnosis of mendelian disorders. *New England Journal of Medicine*, 369(16):1502-11. doi:10.1056/NEJMoa1306555

3. **Retterer K, et al. (2016).** Clinical application of whole-exome sequencing across clinical indications. *Genetics in Medicine*, 18(7):696-704. doi:10.1038/gim.2015.148

4. **Chong JX, et al. (2015).** The Genetic Basis of Mendelian Phenotypes: Discoveries, Challenges, and Opportunities. *American Journal of Human Genetics*, 97(2):199-215. doi:10.1016/j.ajhg.2015.06.009

5. **Gilissen C, et al. (2012).** Disease gene identification strategies for exome sequencing. *European Journal of Human Genetics*, 20(5):490-7. doi:10.1038/ejhg.2011.258

6. **Rabbani B, et al. (2014).** The promise of whole-exome sequencing in medical genetics. *Journal of Human Genetics*, 59(1):5-15. doi:10.1038/jhg.2013.114

7. **MacArthur DG, et al. (2012).** A systematic survey of loss-of-function variants in human protein-coding genes. *Science*, 335(6070):823-8. doi:10.1126/science.1215040

8. **Lelieveld SH, et al. (2016).** Meta-analysis of 2,104 trios provides support for 10 new genes for intellectual disability. *Nature Neuroscience*, 19(9):1194-6. doi:10.1038/nn.4352

9. **Zook JM, et al. (2016).** Extensive sequencing of seven human genomes to characterize benchmark reference materials. *Scientific Data*, 3:160025. doi:10.1038/sdata.2016.25

10. **Richards S, et al. (2015).** Standards and guidelines for the interpretation of sequence variants: a joint consensus recommendation of the American College of Medical Genetics and Genomics and the Association for Molecular Pathology. *Genetics in Medicine*, 17(5):405-24. doi:10.1038/gim.2015.30

11. **ATIA, T. (2019).** Overview of genetic causes of recurrent miscarriage and the diagnostic approach. *BIOCELL*, 43(4), 253–262. https://doi.org/10.32604/biocell.2019.08180

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, **please comment and share** **your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.*
