# How To Analyze Whole Genome Sequencing Data For Absolute Beginners Part 6-2: Identifying Tumor Copy Number Variants Using CNVkit

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/)

- [Introduction: Understanding Tumor Copy Number Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Introduction_Understanding_Tumor_Copy_Number_Variants)

  - [What Are Tumor Copy Number Variants?](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#What_Are_Tumor_Copy_Number_Variants)
  - [The Cancer Genomics Landscape and Copy Number Alterations](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#The_Cancer_Genomics_Landscape_and_Copy_Number_Alterations)
  - [Challenges in Tumor CNV Detection](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Challenges_in_Tumor_CNV_Detection)
  - [Tumor CNV Detection Workflow Overview](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Tumor_CNV_Detection_Workflow_Overview)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Setting_Up_Your_Analysis_Environment)

  - [Prerequisites](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Prerequisites)
  - [Installing Additional Tools for Tumor Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Installing_Additional_Tools_for_Tumor_Analysis)
  - [Sample Data Overview](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Sample_Data_Overview)
  - [Download Additional Reference Files](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Download_Additional_Reference_Files)

- [Tumor CNV Detection Pipeline](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Tumor_CNV_Detection_Pipeline)

  - [Step 1: Preparing Your Example Dataset](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Step_1_Preparing_Your_Example_Dataset)
  - [Step 2: Create Tumor-Optimized Reference](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Step_2_Create_Tumor-Optimized_Reference)
  - [Step 3: Tumor-Normal Copy Number Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Step_3_Tumor-Normal_Copy_Number_Analysis)
  - [Step 4: Tumor-Specific Filtering and Annotation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Step_4_Tumor-Specific_Filtering_and_Annotation)
  - [Step 5: Visualization and Results Interpretation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Step_5_Visualization_and_Results_Interpretation)
  - [Step 6: Clinical Interpretation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Step_6_Clinical_Interpretation)

- [Advanced Analysis and Integration](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Advanced_Analysis_and_Integration)

  - [Multi-Sample Analysis and Cohort Studies](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Multi-Sample_Analysis_and_Cohort_Studies)

- [Single-Command Tumor CNV Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Single-Command_Tumor_CNV_Analysis)

  - [Complete Workflow Using CNVkit Batch](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Complete_Workflow_Using_CNVkit_Batch)
  - [Tumor-Only Analysis Using CNVkit Batch](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Tumor-Only_Analysis_Using_CNVkit_Batch)

- [Best Practices for Tumor CNV Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Best_Practices_for_Tumor_CNV_Analysis)

  - [Experimental Design for Tumor Studies](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Experimental_Design_for_Tumor_Studies)
  - [Computational Best Practices](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Computational_Best_Practices)
  - [Common Pitfalls in Tumor CNV Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Common_Pitfalls_in_Tumor_CNV_Analysis)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Troubleshooting_Common_Issues)

  - [Low Signal-to-Noise Ratio](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Low_Signal-to-Noise_Ratio)
  - [Ploidy and Purity Estimation Challenges](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Ploidy_and_Purity_Estimation_Challenges)
  - [High Tumor Heterogeneity](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#High_Tumor_Heterogeneity)
  - [Germline vs. Somatic Distinction](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Germline_vs_Somatic_Distinction)

- [References and Further Reading](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#References_and_Further_Reading)

  - [Share this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-6-2-identifying-tumor-copy-number-variants-using-cnvkit/#Like_this)

## Introduction: Understanding Tumor Copy Number Variants

Cancer is fundamentally a disease of genomic instability, where normal cells accumulate mutations that drive uncontrolled growth and metastasis. Among these mutations, somatic copy number alterations (SCNAs) – also known as tumor CNVs – play a pivotal role in cancer initiation, progression, and treatment resistance. This tutorial builds upon our [Part 6: germline CNV analysis guide](https://ngs101.com/revised_germline_cnv_tutorial) and [Part 1 of our WGS analysis series](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/) to help you identify and interpret these critical cancer-driving alterations.

### What Are Tumor Copy Number Variants?

Tumor copy number variants are somatic alterations where cancer cells gain or lose copies of chromosomal segments during tumorigenesis. Unlike germline CNVs that are present in all cells from birth, tumor CNVs are acquired during cancer development and are typically found only in malignant tissue.

These alterations can range from focal changes affecting single genes to massive chromosomal instability involving entire chromosome arms. The cancer genome often becomes a mosaic of cells with different copy number profiles, creating tumor heterogeneity that complicates both detection and treatment.

**Key Characteristics of Tumor CNVs:**

- **Somatic Origin:** Acquired during cancer development, not inherited

- **Heterogeneity:** Different cancer cells may have different copy number profiles

- **Functional Impact:** Often affect oncogenes and tumor suppressor genes

- **Therapeutic Relevance:** Can predict drug response and resistance mechanisms

- **Prognostic Value:** Copy number burden correlates with patient outcomes

> 

**Germline vs. Tumor CNVs:** While germline CNVs affect constitutional DNA and are present in all cells, tumor CNVs are somatic alterations found only in cancer cells. Detection requires comparing tumor samples to matched normal controls to distinguish acquired alterations from inherited variants.

### The Cancer Genomics Landscape and Copy Number Alterations

Copy number alterations are hallmarks of cancer genomes and contribute to tumorigenesis through multiple mechanisms:

**Oncogene Amplification:**

- **ERBB2/HER2** amplification in breast cancer drives aggressive growth and guides targeted therapy with trastuzumab

- **MYC** amplification promotes cell proliferation and is found across many cancer types

- **EGFR** amplification in glioblastoma contributes to treatment resistance

**Tumor Suppressor Loss:**

- **TP53** deletion removes critical cell cycle checkpoints

- **CDKN2A/B** deletions disable senescence pathways

- **RB1** loss disrupts normal growth control

**Chromosomal Instability:**

- Aneuploidy (abnormal chromosome numbers) is nearly universal in solid tumors

- Chromothripsis creates localized genomic chaos through chromosome shattering

- Whole-genome doubling followed by loss creates complex copy number landscapes

**Clinical Applications:**

- **Diagnosis:** Copy number profiles help classify tumor subtypes (e.g., medulloepithelioma vs. ependymoma)

- **Prognosis:** Copy number burden often correlates with patient survival

- **Treatment Selection:** HER2 amplification guides trastuzumab therapy; homologous recombination deficiency predicts PARP inhibitor response

- **Resistance Monitoring:** Acquired amplifications can drive treatment resistance

### Challenges in Tumor CNV Detection

Detecting copy number alterations in tumor samples presents unique challenges compared to germline analysis:

**Biological Challenges:**

- **Tumor Purity:** Normal cell contamination dilutes true copy number signals

- **Tumor Heterogeneity:** Subclonal populations may have different copy number profiles

- **Ploidy Variation:** Many tumors are not diploid, complicating copy number interpretation

- **Stromal Contamination:** Infiltrating immune cells and fibroblasts affect copy number estimates

**Technical Challenges:**

- **Reference Selection:** Requires matched normal samples or carefully selected controls

- **Noise vs. Signal:** Must distinguish true somatic alterations from technical artifacts

- **Clonal Evolution:** Copy number may change during disease progression or treatment

- **Sample Quality:** Degraded FFPE samples or low tumor content affect detection sensitivity

**Analytical Challenges:**

- **Baseline Establishment:** Determining the diploid baseline in aneuploid tumors

- **Segmentation Sensitivity:** Balancing detection of focal events with broad chromosomal changes

- **Integration with Other Data:** Combining copy number with mutation, expression, and methylation data

### Tumor CNV Detection Workflow Overview

The workflow for tumor CNV detection differs from germline analysis in several key respects:

1. **Sample Preparation:** Requires matched tumor-normal pairs or carefully selected normal references

2. **Purity Assessment:** Estimating tumor content and ploidy

3. **Coverage Calculation:** Computing read depth ratios between tumor and normal samples

4. **Normalization:** Correcting for technical biases and systematic effects

5. **Segmentation:** Identifying regions of constant copy number

6. **Calling:** Determining copy number states relative to normal diploid baseline

## Setting Up Your Analysis Environment

This tutorial builds directly on our previous WGS analysis tutorials, using the same computational environment and sample data.

### Prerequisites

Before proceeding, ensure you have:

- Completed [Part 1: From Raw Reads to High-Quality Variants Using GATK](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/)

- Reviewed our [germline CNV analysis tutorial](https://ngs101.com/revised_germline_cnv_tutorial) for CNVkit basics

- Aligned BAM files for tumor and normal samples (normal1, normal2, tumor1, tumor2)

- The conda environment from Part 1 (`WGS_env`) with CNVkit installed

- Sufficient computational resources (16+ GB RAM recommended for WGS)

### Installing Additional Tools for Tumor Analysis

Let’s activate your existing environment and add tumor-specific analysis tools:

```bash
# Activate the conda environment from previous tutorials
conda activate ~/WGS_env
```

### Sample Data Overview

For this tutorial, we’ll use the sample data from Part 1:

- **normal1**: First normal/control sample

- **normal2**: Second normal/control sample

- **tumor1**: First tumor sample

- **tumor2**: Second tumor sample

**Important Note**: While these samples might represent matched tumor-normal pairs in a real study, CNVkit’s approach uses **all available normal samples** to build a robust reference. This pooled normal reference approach is actually superior to individual pair-wise comparisons because:

1. **Increased statistical power**: More normal samples reduce noise in the reference

2. **Better bias correction**: Technical artifacts are better normalized across multiple samples

3. **Improved sensitivity**: Small copy number changes are easier to detect against a stable reference

> 

**Methodology Clarification**: Unlike some somatic variant callers that require strict tumor-normal pairing, CNVkit uses a **pooled normal reference** approach where all normal samples contribute to a single, robust baseline for comparison.

### Download Additional Reference Files

Tumor CNV analysis benefits from additional annotation resources:

```bash
# Create directory for tumor-specific references (separate from germline CNV references)
mkdir -p ~/references/tumor_cnv
cd ~/references/tumor_cnv

# Download COSMIC Cancer Gene Census (requires registration, using backup)
# This contains genes with documented roles in cancer
wget -O cancer_gene_census.csv \
    "https://cancer.sanger.ac.uk/cosmic/file_download/GRCh38/cosmic/v95/cancer_gene_census.csv"

# Download OncoKB cancer gene list (curated oncogenes and tumor suppressors)
wget -O oncokb_cancer_genes.txt \
    "https://www.oncokb.org/api/v1/utils/cancerGeneList.txt"

# Download ClinVar pathogenic CNV annotations
wget -O clinvar_cnvs.vcf.gz \
    "https://ftp.ncbi.nlm.nih.gov/pub/clinvar/vcf_GRCh38/clinvar.vcf.gz"
gunzip clinvar_cnvs.vcf.gz
```

## Tumor CNV Detection Pipeline

Now let’s proceed with the tumor-specific CNV analysis workflow, highlighting the differences from germline analysis.

> 

**Key Difference**: Tumor CNV analysis inherently filters out germline variants by using a reference built exclusively from normal samples. When we compare tumor coverage to this normal reference, only somatic (tumor-acquired) alterations will show up as significant deviations.

### Step 1: Preparing Your Example Dataset

For this tutorial, we’ll use the same example data from Part 1. If you’re working with different samples, the workflow remains identical – simply adjust the sample names.

```bash
# Create directory structure for tumor CNV analysis
PROJECT_DIR=~/WGS_Project
cd ${PROJECT_DIR}

# Create tumor-specific subdirectories within the existing CNV analysis structure
mkdir -p cnv_analysis/tumor/{coverage,reference,calls,plots,annotation,clinical}
TUMOR_CNV_DIR=${PROJECT_DIR}/cnv_analysis/tumor

# Set variables for the analysis
BAM_DIR=${PROJECT_DIR}/aligned_reads
NORMAL_SAMPLES=("normal1" "normal2")
TUMOR_SAMPLES=("tumor1" "tumor2")
ALL_SAMPLES=("normal1" "normal2" "tumor1" "tumor2")
REFERENCE_GENOME=~/references/hg38.fa
```

### Step 2: Create Tumor-Optimized Reference

Unlike germline analysis, tumor CNV detection requires special consideration for reference creation:

```bash
#=============================================
# 2.1: Generate coverage profiles for all samples
#=============================================

# Generate coverage for normal samples (these will form our reference)
# The "targets.bed" and "antitargets.bed" files are from part 6

for sample in "${NORMAL_SAMPLES[@]}"; do

    cnvkit.py coverage \
        "${BAM_DIR}/${sample}_recalibrated.bam" \
        ${TUMOR_CNV_DIR}/reference/targets.bed \
        -o ${TUMOR_CNV_DIR}/coverage/${sample}.targetcoverage.cnn

    cnvkit.py coverage \
        "${BAM_DIR}/${sample}_recalibrated.bam" \
        ${TUMOR_CNV_DIR}/reference/antitargets.bed \
        -o ${TUMOR_CNV_DIR}/coverage/${sample}.antitargetcoverage.cnn
done

# Generate coverage for tumor samples (these will be compared to reference)
for sample in "${TUMOR_SAMPLES[@]}"; do

    cnvkit.py coverage \
        "${BAM_DIR}/${sample}_recalibrated.bam" \
        ${TUMOR_CNV_DIR}/reference/targets.bed \
        -o ${TUMOR_CNV_DIR}/coverage/${sample}.targetcoverage.cnn

    cnvkit.py coverage \
        "${BAM_DIR}/${sample}_recalibrated.bam" \
        ${TUMOR_CNV_DIR}/reference/antitargets.bed \
        -o ${TUMOR_CNV_DIR}/coverage/${sample}.antitargetcoverage.cnn
done

#=============================================
# 2.2: Build normal reference from control samples
#=============================================

cnvkit.py reference \
    ${TUMOR_CNV_DIR}/coverage/normal*.targetcoverage.cnn \
    ${TUMOR_CNV_DIR}/coverage/normal*.antitargetcoverage.cnn \
    --fasta ${REFERENCE_GENOME} \
    -o ${TUMOR_CNV_DIR}/reference/tumor_reference.cnn
```

> 

**Critical Difference from Germline Analysis:** For tumor CNV detection, the reference MUST be built exclusively from normal samples. Including tumor samples in the reference would normalize out the very copy number alterations we’re trying to detect.

### Step 3: Tumor-Normal Copy Number Analysis

Now we perform the core tumor CNV analysis, comparing each tumor sample to our normal reference:

```bash
#=============================================
# 3.1: Calculate copy number ratios for tumor samples
#=============================================

for sample in "${TUMOR_SAMPLES[@]}"; do

    # Calculate log2 ratios relative to normal reference
    cnvkit.py fix \
        ${TUMOR_CNV_DIR}/coverage/${sample}.targetcoverage.cnn \
        ${TUMOR_CNV_DIR}/coverage/${sample}.antitargetcoverage.cnn \
        ${TUMOR_CNV_DIR}/reference/tumor_reference.cnn \
        -o ${TUMOR_CNV_DIR}/calls/${sample}.cnr

done

#=============================================
# 3.2: Segment tumor copy number profiles
#=============================================

for sample in "${TUMOR_SAMPLES[@]}"; do

    # Segment the tumor sample using CBS (Circular Binary Segmentation)
    # This algorithm is particularly well-suited for tumor samples with noise
    cnvkit.py segment \
        ${TUMOR_CNV_DIR}/calls/${sample}.cnr \
        -m cbs \
        --smooth-cbs \
        -o ${TUMOR_CNV_DIR}/calls/${sample}.cns

done

#=============================================
# 3.3: Call discrete copy number states
#=============================================

for sample in "${TUMOR_SAMPLES[@]}"; do

    # Call integer copy number states
    # Tumor samples often have complex ploidy, so we use permissive thresholds
    cnvkit.py call \
        ${TUMOR_CNV_DIR}/calls/${sample}.cns \
        -o ${TUMOR_CNV_DIR}/calls/${sample}_called.cns \
        --purity 0.7 \
        --ploidy 2 \
        --drop-low-coverage

done

#=============================================
# 3.4: Quality control and metrics
#=============================================

for sample in "${TUMOR_SAMPLES[@]}"; do

    # Calculate basic CNV statistics
    cnvkit.py metrics \
        ${TUMOR_CNV_DIR}/calls/${sample}.cnr \
        -s ${TUMOR_CNV_DIR}/calls/${sample}.cns \
        > ${TUMOR_CNV_DIR}/calls/${sample}_metrics.txt

done
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/tumor_called_cns.png?resize=1024%2C167&ssl=1)

### Step 4: Tumor-Specific Filtering and Annotation

Tumor CNV filtering focuses on identifying high-confidence somatic alterations. Note that germline variants are already filtered out through our use of the normal reference in Step 3:

```bash
#=============================================
# 4.1: Filter tumor CNVs for high-confidence calls
#=============================================

mkdir -p ${TUMOR_CNV_DIR}/filtered

for sample in "${TUMOR_SAMPLES[@]}"; do
    # Apply tumor-specific filters
    # More permissive than germline due to tumor heterogeneity and noise
    # Note: --thresholds parameter must be quoted when using comma-separated values
    cnvkit.py call \
        ${TUMOR_CNV_DIR}/calls/${sample}.cns \
        -o ${TUMOR_CNV_DIR}/filtered/${sample}_filtered.cns \
        --thresholds="-1.0,-0.5,0.5,1.0" \
        --purity 0.7 \
        --drop-low-coverage

    # Filter by minimum size (tumors often have focal alterations)
    # Keep alterations ≥100kb for broad analysis, ≥10kb for focal events
    awk 'NR==1 || ($3-$2) >= 100000' \
        ${TUMOR_CNV_DIR}/filtered/${sample}_filtered.cns \
        > ${TUMOR_CNV_DIR}/filtered/${sample}_broad.cns

    awk 'NR==1 || ($3-$2) >= 10000' \
        ${TUMOR_CNV_DIR}/filtered/${sample}_filtered.cns \
        > ${TUMOR_CNV_DIR}/filtered/${sample}_focal.cns

done
```

### Step 5: Visualization and Results Interpretation

Tumor CNV visualization requires specialized plots to capture tumor-specific features:

```bash
#=============================================
# 5.1: Generate tumor CNV visualization plots
#=============================================

for sample in "${TUMOR_SAMPLES[@]}"; do

    # 1. Genome-wide copy number profile
    cnvkit.py scatter \
        ${TUMOR_CNV_DIR}/calls/${sample}.cnr \
        -s ${TUMOR_CNV_DIR}/calls/${sample}.cns \
        -o ${TUMOR_CNV_DIR}/plots/${sample}_genome_wide.pdf \
        --title "Tumor Copy Number Profile: $sample"

    # 2. Individual chromosome plots for detailed review
    for chr in {1..22} X Y; do
        cnvkit.py scatter \
            ${TUMOR_CNV_DIR}/calls/${sample}.cnr \
            -s ${TUMOR_CNV_DIR}/calls/${sample}.cns \
            -c chr${chr} \
            -o ${TUMOR_CNV_DIR}/plots/${sample}_chr${chr}.pdf \
            --title "$sample Chromosome $chr"
    done

    # 3. Heatmap for copy number states
    cnvkit.py heatmap \
        ${TUMOR_CNV_DIR}/calls/${sample}.cns \
        -o ${TUMOR_CNV_DIR}/plots/${sample}_heatmap.pdf

    # 4. Generate copy number distribution plot
    # Create a diagram showing the copy number profile
    cnvkit.py diagram \
        ${TUMOR_CNV_DIR}/calls/${sample}.cnr \
        -s ${TUMOR_CNV_DIR}/calls/${sample}.cns \
        -o ${TUMOR_CNV_DIR}/plots/${sample}_diagram.pdf

done
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/somatic_cnv_plot-e1761610664295-1024x251.png?resize=1024%2C251&ssl=1)

### Step 6: Clinical Interpretation

For tumor CNVs, clinical interpretation focuses on actionable alterations and known pathogenic variants from clinical databases:

```bash
#=============================================
# 6.1: Process ClinVar CNV data for clinical annotation
#=============================================

mkdir -p ${TUMOR_CNV_DIR}/clinical

# Note: ClinVar contains mostly germline variants, but can help identify:
# 1. Tumor suppressor genes where germline + somatic hits cause cancer
# 2. Regions with known pathogenic copy number changes
# 3. Genes where CNVs have established clinical significance

# Extract CNV-related entries from ClinVar VCF
# Look for copy number variants using CLNVC (ClinVar Variant Class) field

grep -E "(CLNVC=Deletion|CLNVC=Duplication|CLNVC=copy_number_gain|CLNVC=copy_number_loss)" ~/references/tumor_cnv/clinvar_cnvs.vcf > \
    ${TUMOR_CNV_DIR}/clinical/clinvar_cnvs_only.vcf

# Convert ClinVar CNVs to BED format for overlap analysis
grep -v "^#" ${TUMOR_CNV_DIR}/clinical/clinvar_cnvs_only.vcf | \
    awk 'BEGIN{OFS="\t"} {
        # Extract basic coordinates and ensure chromosome has "chr" prefix
        chr = $1
        if (chr !~ /^chr/) {
            chr = "chr" chr
        }
        start = $2 - 1  # Convert to 0-based for BED

        # Determine end position based on variant type
        end = start + 1  # default for SNVs

        # For indels, calculate size from REF/ALT
        ref_len = length($4)
        alt_len = length($5)

        if (ref_len > alt_len) {
            # Deletion - use REF length
            end = start + ref_len
        } else if (alt_len > ref_len) {
            # Insertion/duplication - for BED purposes, use minimal coordinates
            end = start + 1
        } else {
            # Same length (substitution) or complex
            end = start + ref_len
        }

        # Extract ClinVar-specific information
        clnvc = "unknown"
        if (match($8, /CLNVC=([^;]+)/, arr)) {
            clnvc = arr[1]
        }

        # Extract clinical significance
        clnsig = "unknown"
        if (match($8, /CLNSIG=([^;]+)/, arr)) {
            clnsig = arr[1]
        }

        # Extract gene info
        gene = "unknown"
        if (match($8, /GENEINFO=([^;:]+)/, arr)) {
            gene = arr[1]
        }

        # Extract disease/phenotype information
        clndn = "unknown"
        if (match($8, /CLNDN=([^;]+)/, arr)) {
            clndn = arr[1]
        }

        # Only include if it could be a CNV-like variant
        if (clnvc ~ /[Dd]eletion|[Dd]uplication|copy_number/ || ref_len > 10 || alt_len > 10) {
            # Print BED entry with ClinVar annotations
            print chr, start, end, $3, clnvc, clnsig, gene, clndn
        }
    }' > ${TUMOR_CNV_DIR}/clinical/clinvar_cnvs.bed

#=============================================
# 6.2: Overlap tumor CNVs with ClinVar pathogenic variants
#=============================================

for sample in "${TUMOR_SAMPLES[@]}"; do

    # Convert our CNV calls to BED format for overlap analysis
    # Fix chromosome naming to match ClinVar (add "chr" prefix if missing)
    tail -n +2 ${TUMOR_CNV_DIR}/filtered/${sample}_filtered.cns | \
        awk 'BEGIN{OFS="\t"} {
            # Ensure chromosome has "chr" prefix
            chr = $1
            if (chr !~ /^chr/) {
                chr = "chr" chr
            }

            # Classify alteration type
            if ($6 >= 0.5) alteration="gain"
            else if ($6 <= -0.5) alteration="loss"  
            else alteration="neutral"

            print chr, $2, $3, $4, alteration, $6
        }' > ${TUMOR_CNV_DIR}/clinical/${sample}_cnvs_for_overlap.bed

    # Find overlaps with ClinVar pathogenic/likely pathogenic CNVs

    bedtools intersect \
        -a ${TUMOR_CNV_DIR}/clinical/${sample}_cnvs_for_overlap.bed \
        -b ${TUMOR_CNV_DIR}/clinical/clinvar_cnvs.bed \
        -wa -wb \
        -f 0.5 \
        > ${TUMOR_CNV_DIR}/clinical/${sample}_clinvar_overlaps.bed
done
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/10/somatic_cnv_clinical-e1761610708988-1024x318.png?resize=1024%2C318&ssl=1)

## Advanced Analysis and Integration

### Multi-Sample Analysis and Cohort Studies

For larger studies involving multiple tumor samples:

```bash
#=============================================
# Cohort-level analysis (when you have multiple samples)
#=============================================

# Create directory for cohort analysis
mkdir -p ${TUMOR_CNV_DIR}/cohort_analysis

# Combine all tumor samples for population-level analysis
cnvkit.py call \
    ${TUMOR_CNV_DIR}/calls/tumor*.cns \
    -o ${TUMOR_CNV_DIR}/cohort_analysis/cohort_calls.cns \
    --center-at 0

# Generate cohort heatmap
cnvkit.py heatmap \
    ${TUMOR_CNV_DIR}/calls/tumor*.cns \
    -o ${TUMOR_CNV_DIR}/cohort_analysis/cohort_heatmap.pdf

# Identify recurrent alterations across the cohort
cnvkit.py genemetrics \
    ${TUMOR_CNV_DIR}/calls/tumor*.cns \
    -t 0.2 \
    -o ${TUMOR_CNV_DIR}/cohort_analysis/recurrent_cnvs.txt

echo "Cohort analysis complete"
```

## Single-Command Tumor CNV Analysis

For experienced users or production workflows, CNVkit provides a streamlined `batch` command that performs the entire tumor CNV analysis workflow in a single step. This method automatically handles coverage calculation, reference building, copy number calculation, segmentation, and calling with optimized parameters.

The batch approach is ideal for:

- **Production workflows** requiring consistent, automated processing

- **Large-scale studies** processing many tumor-normal pairs

- **Experienced users** who understand the underlying methodology

- **Standardized pipelines** needing reproducible results

### Complete Workflow Using CNVkit Batch

```bash
# Set up directory for batch analysis
mkdir -p ${TUMOR_CNV_DIR}/batch_analysis
cd ${TUMOR_CNV_DIR}/batch_analysis

# Define sample paths
NORMAL_BAMS=("${BAM_DIR}/normal1_recalibrated.bam" "${BAM_DIR}/normal2_recalibrated.bam")
TUMOR_BAMS=("${BAM_DIR}/tumor1_recalibrated.bam" "${BAM_DIR}/tumor2_recalibrated.bam")

# Single command to perform complete tumor CNV analysis
cnvkit.py batch \
    ${TUMOR_BAMS[@]} \
    --normal ${NORMAL_BAMS[@]} \
    --fasta ${REFERENCE_GENOME} \
    --annotate ~/references/refFlat_hg38.txt \
    --output-reference pooled_normal_reference.cnn \
    --output-dir results/ \
    --method wgs \
    --segment-method cbs \
    --drop-low-coverage \
    --scatter \
    --diagram
```

### Tumor-Only Analysis Using CNVkit Batch

For tumor-only analysis, we can use CNVkit’s batch command without normal samples to create a flat reference:

```bash
# Set up directory for tumor-only batch analysis
mkdir -p ${TUMOR_CNV_DIR}/tumor_only_analysis
cd ${TUMOR_CNV_DIR}/tumor_only_analysis

# Define tumor sample paths
TUMOR_BAMS=("${BAM_DIR}/tumor1_recalibrated.bam" "${BAM_DIR}/tumor2_recalibrated.bam")

# Single command to perform complete tumor-only CNV analysis
cnvkit.py batch \
    ${TUMOR_BAMS[@]} \
    --fasta ${REFERENCE_GENOME} \
    --annotate ~/references/refFlat_hg38.txt \
    --output-reference flat_reference.cnn \
    --output-dir results/ \
    --method wgs \
    --segment-method cbs \
    --drop-low-coverage \
    --scatter \
    --diagram
```

#### Key Considerations For Tumor-Only Analysis:

**What You Can Detect:**

- Large amplifications (oncogenes like HER2, EGFR, MYC)

- Deep deletions (tumor suppressors like TP53, CDKN2A)

- Broad chromosomal gains/losses

- Clinically actionable alterations

**Limitations:**

- Cannot distinguish somatic vs. germline – inherited CNVs will appear as “tumor” alterations

- Higher false positive rate – normal population variation may be called as alterations

- Reduced sensitivity – subtle copy number changes harder to detect

- Interpretation challenges – requires more careful filtering and validation

**Best Practices for Tumor-Only:**

- Focus on large alterations – Use higher thresholds (log2 > 1.0 for gains, < -1.0 for losses)

- Filter with population databases – Remove common germline CNVs using DGV, gnomAD-SV

- Prioritize cancer genes – Focus on known oncogenes and tumor suppressors

- Validate key findings – Use FISH, qPCR, or array-based methods for actionable alterations

- Consider clinical context – Integrate with pathology, immunohistochemistry, and clinical presentation

**Tumor-only analysis is particularly useful for:**

- Biomarker testing for targeted therapy selection

- Clinical trials enrollment based on copy number alterations

- Retrospective studies using archived FFPE samples

- Resource-limited settings where matched normals aren’t feasible

## Best Practices for Tumor CNV Analysis

Tumor CNV analysis requires specialized considerations beyond those for germline variants:

### Experimental Design for Tumor Studies

**Experimental Design for Tumor Studies**

**Sample Collection:**

- **Normal Control Samples:** Include high-quality normal tissue samples for reference building (blood, adjacent normal tissue, or other confirmed normal samples)

- **Tumor Purity Assessment:** Ensure tumor samples have >50% cancer cells for reliable detection

- **Fresh vs. FFPE:** Fresh-frozen samples provide higher quality data than FFPE, but both are analyzable

- **Multiple Regions:** Consider sampling multiple tumor regions to assess intratumor heterogeneity

**When Tumor-Normal Pairing Matters:**

- **Individual patient analysis**: For clinical reporting, knowing which normal belongs to which tumor patient is important for interpretation

- **Germline variant filtering**: Patient-specific germline variants can be filtered using matched normal

- **Contamination assessment**: Cross-sample contamination is easier to detect with known pairs

**When Pooled Normal References Are Better (CNVkit approach):**

- **Copy number detection**: Pooling multiple normals creates a more robust baseline

- **Technical bias correction**: Systematic artifacts are better normalized across multiple samples

- **Statistical power**: More normal samples improve the signal-to-noise ratio

**Sequencing Considerations:**

- **Coverage Depth:** Minimum 60-80x for tumor samples, 30-40x for normals

- **Paired-End Sequencing:** Recommended for breakpoint resolution and structural variant detection

- **Insert Size:** 350-500bp provides optimal coverage uniformity

- **Quality Metrics:** Monitor DNA integrity, library complexity, and sequencing quality

### Computational Best Practices

**Reference Building:**

- **Pure Normal Samples:** Use only confirmed normal samples in reference construction

- **Population Matching:** Match tumor and normal samples by ancestry when possible

- **Batch Correction:** Process tumor-normal pairs together to minimize batch effects

- **Reference Updates:** Periodically update references as more normal samples become available

**Quality Control Checkpoints:**

- **Purity Estimation:** Validate tumor purity estimates with pathology review

- **Sex Chromosome Analysis:** Verify expected sex chromosome copy number

- **Contamination Assessment:** Check for cross-sample contamination

- **Technical Artifacts:** Filter regions prone to false positives (centromeres, segmental duplications)

### Common Pitfalls in Tumor CNV Analysis

**Technical Pitfalls:**

- **Contaminated References:** Including tumor samples in normal reference panels

- **Ignoring Tumor Purity:** Not accounting for normal cell contamination in interpretation

- **Batch Effects:** Processing tumor and normal samples in different batches

- **Over-segmentation:** Using segmentation parameters that create too many small segments

**Biological Pitfalls:**

- **Assuming Diploidy:** Many tumors are aneuploid with complex ploidy states

- **Ignoring Heterogeneity:** Assuming all tumor cells have identical copy number profiles

- **Temporal Changes:** Not accounting for copy number evolution during treatment

- **Germline Contamination:** Misinterpreting inherited CNVs as somatic alterations

**Clinical Pitfalls:**

- **Over-interpretation:** Calling variants actionable without sufficient evidence

- **Ignoring Context:** Not considering tumor type-specific alteration patterns

- **Single Time Point:** Not monitoring copy number changes during treatment

- **Poor Communication:** Inadequate explanation of results to clinical teams

## Troubleshooting Common Issues

### Low Signal-to-Noise Ratio

**Problem:** CNV signals are weak or noisy, making reliable detection difficult.

**Solutions:**

- Increase sequencing depth (aim for 80-100x for problematic samples)

- Improve tumor purity through macro-dissection or laser capture microdissection

- Use larger bin sizes to reduce noise at the cost of resolution

- Consider alternative normalization strategies (GC correction, mappability filtering)

### Ploidy and Purity Estimation Challenges

**Problem:** Difficulty determining tumor ploidy and purity affecting copy number interpretation.

**Solutions:**

- Use orthogonal methods (flow cytometry, FISH) to estimate ploidy

- Employ allele-specific copy number analysis (ASCAT, ABSOLUTE)

- Integrate with mutation data to estimate sample purity

- Consider using multiple purity/ploidy scenarios in interpretation

### High Tumor Heterogeneity

**Problem:** Different tumor regions show different copy number profiles.

**Solutions:**

- Sample multiple tumor regions to capture heterogeneity

- Use single-cell copy number analysis for detailed characterization

- Focus on early, clonal alterations present across all regions

- Consider bulk tissue analysis as average of heterogeneous populations

### Germline vs. Somatic Distinction

**Problem:** Difficulty distinguishing inherited from acquired copy number alterations.

**Solutions:**

- Always include matched normal tissue controls

- Use population databases (DGV, gnomAD-SV) to filter common variants

- Validate suspicious germline alterations in independent normal tissue

- Consider constitutional CNV testing for patients with multiple rare variants

## References and Further Reading

1. Talevich E, Shain AH, Botton T, Bastian BC. (2016). CNVkit: Genome-wide copy number detection and visualization from targeted DNA sequencing. *PLOS Computational Biology*, 12(4): e1004873.

2. Zack TI, Schumacher SE, Carter SL, et al. (2013). Pan-cancer patterns of somatic copy number alteration. *Nature Genetics*, 45(10): 1134-1140.

3. Cancer Genome Atlas Research Network. (2013). The Cancer Genome Atlas Pan-Cancer analysis project. *Nature Genetics*, 45(10): 1113-1120.

4. Riggs ER, Andersen EF, Cherry AM, et al. (2020). Technical standards for the interpretation and reporting of constitutional copy-number variants: a joint consensus recommendation of the American College of Medical Genetics and Genomics (ACMG) and the Clinical Genome Resource (ClinGen). *Genetics in Medicine*, 22(2): 245-257.

5. Carter SL, Cibulskis K, Helman E, et al. (2012). Absolute quantification of somatic DNA alterations in human cancer. *Nature Biotechnology*, 30(5): 413-421.

6. Van Loo P, Nordgard SH, Lingjærde OC, et al. (2010). Allele-specific copy number analysis of tumors. *Proceedings of the National Academy of Sciences*, 107(39): 16910-16915.

7. Venkatraman ES, Olshen AB. (2007). A faster circular binary segmentation algorithm for the analysis of array CGH data. *Bioinformatics*, 23(6): 657-663.

8. Wolff AC, Hammond MEH, Allison KH, et al. (2018). Human epidermal growth factor receptor 2 testing in breast cancer: American Society of Clinical Oncology/College of American Pathologists clinical practice guideline focused update. *Journal of Clinical Oncology*, 36(20): 2105-2122.

9. Lord CJ, Ashworth A. (2017). PARP inhibitors: Synthetic lethality in the clinic. *Science*, 355(6330): 1152-1158.

10. Sondka Z, Bamford S, Cole CG, et al. (2018). The COSMIC Cancer Gene Census: describing genetic dysfunction across all human cancers. *Nature Reviews Cancer*, 18(11): 696-705.

11. Lasolle, H., Elsensohn, MH., Wierinckx, A. et al. Chromosomal instability in the prediction of pituitary neuroendocrine tumors prognosis. acta neuropathol commun 8, 190 (2020). https://doi.org/10.1186/s40478-020-01067-5

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your cancer genomics research, **please share your experience** and **subscribe** for more comprehensive bioinformatics tutorials.*
