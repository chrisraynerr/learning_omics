# How To Analyze Whole Genome Sequencing Data For Absolute Beginners Part 5: Identifying Disease- or Patient-Specific Variants

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/)

- [Introduction: From Variants to Disease Genes](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Introduction_From_Variants_to_Disease_Genes)

  - [What Are Disease-Specific and Patient-Specific Variants?](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#What_Are_Disease-Specific_and_Patient-Specific_Variants)
  - [The Challenge of Variant Interpretation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#The_Challenge_of_Variant_Interpretation)
  - [Prerequisites and Workflow Overview](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Prerequisites_and_Workflow_Overview)

- [Setting Up Your Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Setting_Up_Your_Analysis)

  - [Project Directory Structure](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Project_Directory_Structure)
  - [Prepare Sample Lists](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Prepare_Sample_Lists)

- [Joint Genotyping: Creating a Cohort VCF](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Joint_Genotyping_Creating_a_Cohort_VCF)

  - [Step 1: Combine GVCFs](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Step_1_Combine_GVCFs)
  - [Step 2: Perform Joint Genotyping](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Step_2_Perform_Joint_Genotyping)

- [Quality Filtering: Removing Unreliable Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Quality_Filtering_Removing_Unreliable_Variants)

  - [Understanding GATK Quality Metrics](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Understanding_GATK_Quality_Metrics)
  - [Apply Variant Filters](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Apply_Variant_Filters)

- [Annotating Variants with Population Frequencies](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Annotating_Variants_with_Population_Frequencies)
- [Calculating Control Allele Frequencies](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Calculating_Control_Allele_Frequencies)
- [Functional Annotation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Functional_Annotation)
- [Identifying Patient-Specific Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Identifying_Patient-Specific_Variants)

  - [Patient-Specific Variant Strategy](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Patient-Specific_Variant_Strategy)
  - [Extract Patient-Specific Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Extract_Patient-Specific_Variants)

- [Identifying Disease-Specific Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Identifying_Disease-Specific_Variants)

  - [Disease-Specific Variant Strategy](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Disease-Specific_Variant_Strategy)
  - [Calculate Case Allele Frequencies](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Calculate_Case_Allele_Frequencies)
  - [Filter for Disease-Enriched Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Filter_for_Disease-Enriched_Variants)
  - [Identify Recurrently Mutated Genes](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Identify_Recurrently_Mutated_Genes)

- [Advanced Filtering Strategies](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Advanced_Filtering_Strategies)

  - [Filter by Variant Type](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Filter_by_Variant_Type)
  - [Identify Compound Heterozygous Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Identify_Compound_Heterozygous_Variants)

- [Interpreting and Prioritizing Results](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Interpreting_and_Prioritizing_Results)

  - [Prioritizing Candidates for Follow-up](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Prioritizing_Candidates_for_Follow-up)
  - [Next Steps](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Next_Steps)

- [Best Practices for Disease Variant Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Best_Practices_for_Disease_Variant_Analysis)

  - [Sample Selection and Study Design](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Sample_Selection_and_Study_Design)
  - [Filtering Strategy Considerations](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Filtering_Strategy_Considerations)
  - [Variant Interpretation Guidelines](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Variant_Interpretation_Guidelines)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Common_Pitfalls_to_Avoid)
  - [Validation and Follow-up](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Validation_and_Follow-up)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Troubleshooting_Common_Issues)

  - [Too Many Candidate Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Too_Many_Candidate_Variants)
  - [Too Few Candidate Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Too_Few_Candidate_Variants)
  - [Poor Case-Control Separation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Poor_Case-Control_Separation)
  - [Inconsistent Results Across Patients](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Inconsistent_Results_Across_Patients)

- [Conclusion](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-5-identifying-disease-or-patient-specific-variants/#Like_this)

## Introduction: From Variants to Disease Genes

After successfully calling variants in your whole genome sequencing samples (as covered in [Part 1 of this series](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/)), you now face an exciting challenge: among the millions of genetic variants present in the human genome, which ones are actually responsible for disease?

Every human genome contains approximately 4-5 million single nucleotide variants (SNVs) compared to the reference genome. Most of these variants are benign polymorphisms that contribute to normal human diversity. The challenge in medical genomics is to identify the needle in this haystack—the rare variants that disrupt gene function and cause disease.

### What Are Disease-Specific and Patient-Specific Variants?

Understanding the distinction between these two concepts is fundamental to variant analysis:

**Disease-Specific Variants** are genetic changes that are significantly more common in individuals affected by a particular disease compared to healthy controls. These variants may be shared among multiple patients with the same condition and point to genes or pathways that are causally involved in disease development. For example, in a study of patients with congenital heart defects, you might discover that variants in a particular cardiac development gene appear in 30% of patients but in less than 1% of healthy individuals.

**Patient-Specific Variants** (also called private or individual variants) are unique or extremely rare genetic changes found in a single patient or family but absent from both healthy controls and population databases. These variants are particularly important when studying rare genetic diseases or when searching for the genetic cause of a patient’s unexplained condition. In clinical genomics, identifying these variants can lead to a definitive molecular diagnosis and inform treatment decisions.

> 

**Clinical Example:** Imagine a child with developmental delays and seizures. After whole genome sequencing, you identify a previously unreported variant in a gene known to cause epilepsy. This variant appears in the patient but not in their unaffected parents (indicating a de novo mutation), is absent from healthy controls, and is predicted to severely disrupt protein function. This patient-specific variant becomes the likely cause of the child’s condition.

### The Challenge of Variant Interpretation

Not every rare variant causes disease. The human genome is surprisingly tolerant of variation, with each individual carrying dozens of variants predicted to disrupt protein function. The key challenges in variant interpretation include:

- **Distinguishing pathogenic variants from benign rare variants:** Many genes tolerate loss-of-function mutations without causing disease

- **Accounting for incomplete penetrance:** Some disease variants don’t always cause disease, even when inherited

- **Identifying compound heterozygous variants:** Two different mutations in the same gene may be required for disease manifestation

- **Separating causative variants from passenger variants:** Particularly important in complex diseases where multiple variants may contribute to risk

This tutorial will guide you through the computational steps to narrow down millions of variants to a manageable list of high-confidence disease or patient-specific candidates that warrant further investigation.

### Prerequisites and Workflow Overview

Before starting this tutorial, you should have:

1. **Completed variant calling** for all your samples using GATK (as described in [Part 1](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/))

2. **GVCF files** for each sample (patient1.g.vcf.gz, patient2.g.vcf.gz, control1.g.vcf.gz, control2.g.vcf.gz, etc.)

3. **At least two groups of samples:** patients (cases) with the disease of interest and healthy controls

4. **The WGS conda environment** already set up from [Part 1](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/)

Our workflow will proceed through these major steps:

1. **Joint Genotyping:** Combine variant calls from all samples to enable accurate comparison

2. **Quality Filtering:** Remove low-confidence variant calls that likely represent technical artifacts

3. **Population Frequency Annotation:** Add allele frequency information from public databases

4. **Control Frequency Calculation:** Calculate how common each variant is in your control samples

5. **Functional Annotation:** Predict variant effects on protein function

6. **Patient-Specific Variant Identification:** Discover unique variants in individual patients

7. **Disease-Specific Variant Identification:** Find variants enriched in patients compared to controls

By the end of this tutorial, you’ll have curated lists of candidate variants most likely to be involved in your disease of interest, ready for experimental validation or clinical interpretation.

## Setting Up Your Analysis

Let’s prepare our project directory and ensure we have the necessary reference data.

### Project Directory Structure

```bash
# Activate the WGS conda environment created in Part 1
conda activate wgs_analysis

# Create project directory structure
mkdir -p ~/disease_variant_analysis
cd ~/disease_variant_analysis
mkdir -p gvcfs joint_calling filtered annotated disease_specific patient_specific sample_lists
```

### Prepare Sample Lists

```bash
# Create list of patient sample names (replace with your actual patient IDs)
cat > sample_lists/patients.txt << EOF
patient1
patient2
patient3
patient4
patient5
EOF

# Create list of control sample names (replace with your actual control IDs)
cat > sample_lists/controls.txt << EOF
control1
control2
control3
control4
control5
EOF

# Create combined list of all samples
cat sample_lists/patients.txt sample_lists/controls.txt > sample_lists/all_samples.txt
```

> 

**Important:** Make sure your GVCF files are named consistently (e.g., patient1.g.vcf.gz, control1.g.vcf.gz) and placed in the `gvcfs/` directory. These GVCF files should have been generated in Part 1 of this tutorial series.

## Joint Genotyping: Creating a Cohort VCF

Joint genotyping is a crucial step that enables accurate comparison of variants across all your samples. Unlike simply merging individual VCF files, joint genotyping re-evaluates the evidence for each variant across all samples simultaneously, resulting in more accurate and consistent variant calls.

### Step 1: Combine GVCFs

```bash
cd ~/disease_variant_analysis
REF_GENOME=~/wgs_analysis/reference/Homo_sapiens_assembly38.fasta

# Create file listing all GVCF paths
find gvcfs/ -name "*.g.vcf.gz" > gvcf_list.txt

# Combine all GVCFs into a single cohort GVCF
gatk CombineGVCFs \
    -R ${REF_GENOME} \
    --variant gvcf_list.txt \
    -O joint_calling/cohort.g.vcf.gz
```

### Step 2: Perform Joint Genotyping

```bash
# Call variants jointly across the entire cohort
gatk GenotypeGVCFs \
    -R ${REF_GENOME} \
    -V joint_calling/cohort.g.vcf.gz \
    -O joint_calling/cohort_raw.vcf.gz \
    --include-non-variant-sites false \
    --max-alternate-alleles 6
```

> 

**What’s Happening:** Joint genotyping considers all samples together when calling variants, leading to more accurate genotype calls and consistent variant detection across your cohort. This step is computationally intensive.

## Quality Filtering: Removing Unreliable Variants

Raw variant calls contain many false positives. Quality filtering removes these unreliable calls to focus on high-confidence variants.

### Understanding GATK Quality Metrics

- **QD (Qual By Depth):** Variant confidence normalized by depth. Low values suggest weak evidence. Threshold: QD < 2.0

- **FS (Fisher Strand):** Measures strand bias. High values indicate variants only on one strand. Threshold: FS > 60.0

- **MQ (Mapping Quality):** Average mapping quality of reads. Low values suggest ambiguous mapping. Threshold: MQ < 40.0

- **SOR (Strand Odds Ratio):** Another measure of strand bias. High values indicate artifacts. Threshold: SOR > 3.0

- **MQRankSum:** Compares mapping quality of reference vs. alternate allele reads

- **ReadPosRankSum:** Tests if variant alleles are consistently at read ends (suggests artifacts)

### Apply Variant Filters

```bash
# Apply hard filters using GATK VariantFiltration
gatk VariantFiltration \
    -R ${REF_GENOME} \
    -V joint_calling/cohort_raw.vcf.gz \
    --filter-expression "QD < 2.0" --filter-name "LowQD" \
    --filter-expression "FS > 60.0" --filter-name "HighFS" \
    --filter-expression "MQ < 40.0" --filter-name "LowMQ" \
    --filter-expression "SOR > 3.0" --filter-name "HighSOR" \
    --filter-expression "MQRankSum < -12.5" --filter-name "LowMQRankSum" \
    --filter-expression "ReadPosRankSum < -8.0" --filter-name "LowReadPosRankSum" \
    -O filtered/cohort_filtered.vcf.gz

# Extract only variants that passed all filters
bcftools view -f PASS -O z -o filtered/cohort_pass.vcf.gz filtered/cohort_filtered.vcf.gz
bcftools index filtered/cohort_pass.vcf.gz
```

## Annotating Variants with Population Frequencies

Before identifying disease-specific variants, we need to annotate our variants with allele frequencies from population databases like gnomAD. This helps us filter out common variants unlikely to cause rare diseases.

In [Part 2A of this series](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/), we downloaded the gnomAD database. We’ll use that same database here.

```bash
# Path to gnomAD database (downloaded in Part 2A tutorial)
GNOMAD_DIR=~/references/somatic_resources

# Annotate with gnomAD frequencies
bcftools annotate \
    -a ${GNOMAD_DIR}/af-only-gnomad.hg38.vcf.gz \
    -c INFO/AF:=INFO/gnomAD_AF \
    -O z \
    -o filtered/cohort_pass_gnomad.vcf.gz \
    filtered/cohort_pass.vcf.gz

bcftools index filtered/cohort_pass_gnomad.vcf.gz
```

> 

**Why This Matters:** A variant present in 10% of the general population is unlikely to cause a rare disease affecting 1 in 10,000 people. By annotating with population frequencies, we can focus on truly rare variants.

## Calculating Control Allele Frequencies

We need to calculate how common each variant is within our own control samples. This provides population-matched frequency information that can reveal population-specific polymorphisms not well-represented in gnomAD.

```bash
# Calculate AF, AC, and AN for controls
bcftools +fill-tags \
    filtered/cohort_pass_gnomad.vcf.gz \
    -O z \
    -o filtered/cohort_temp.vcf.gz \
    -- -t AF,AC,AN -S sample_lists/controls.txt

# Rename to Control_AF, Control_AC, Control_AN
cat > header_control.txt << EOF
##INFO=<ID=Control_AF,Number=A,Type=Float,Description="Allele frequency in control samples">
##INFO=<ID=Control_AC,Number=A,Type=Integer,Description="Allele count in control samples">
##INFO=<ID=Control_AN,Number=1,Type=Integer,Description="Total allele number in control samples">
EOF

bcftools annotate \
    -h header_control.txt \
    -c INFO/AF:=INFO/Control_AF,INFO/AC:=INFO/Control_AC,INFO/AN:=INFO/Control_AN \
    -O z \
    -o filtered/cohort_with_control_AF.vcf.gz \
    filtered/cohort_temp.vcf.gz

bcftools index filtered/cohort_with_control_AF.vcf.gz
rm filtered/cohort_temp.vcf.gz header_control.txt
```

## Functional Annotation

To prioritize variants most likely to affect protein function, we need to annotate variants with their predicted functional consequences. For detailed annotation strategies, see [Part 3 of this series on variant annotation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/).

Here we’ll use SnpEff for functional annotation:

```bash
# Run SnpEff annotation
snpEff ann -v GRCh38.99 filtered/cohort_with_control_AF.vcf.gz | bgzip > annotated/cohort_annotated.vcf.gz
bcftools index annotated/cohort_annotated.vcf.gz
```

SnpEff classifies variants by predicted impact:

- **HIGH Impact:** Stop-gained, frameshift, splice site disruptions (likely protein disruption)

- **MODERATE Impact:** Missense variants, in-frame indels (amino acid changes)

- **LOW Impact:** Synonymous variants (unlikely to change protein)

- **MODIFIER:** Non-coding variants

## Identifying Patient-Specific Variants

Patient-specific variants are rare or unique variants found in individual patients but absent or very rare in both controls and population databases. These are particularly important for diagnosing rare genetic diseases.

### Patient-Specific Variant Strategy

For each patient, we identify variants that meet these criteria:

1. Present in the patient (heterozygous or homozygous)

2. Rare in controls (Control_AF < 0.05)

3. Rare in populations (gnomAD_AF < 0.01 or absent)

4. Functionally significant (HIGH or MODERATE impact)

### Extract Patient-Specific Variants

We’ll demonstrate the analysis for patient1. Repeat these steps for each patient by changing the patient ID.

```bash
mkdir -p patient_specific/patient1

# Extract variants for patient1 and apply filters
bcftools view -s patient1 -i 'GT="alt"' annotated/cohort_annotated.vcf.gz \
    | bcftools view \
        -i '(Control_AF<0.05 | Control_AF=".") && (gnomAD_AF<0.01 | gnomAD_AF=".") && (ANN~"HIGH" | ANN~"MODERATE")' \
        -O z \
        -o patient_specific/patient1/patient1_candidates.vcf.gz

bcftools index patient_specific/patient1/patient1_candidates.vcf.gz

# Create readable table
bcftools query \
    -f '%CHROM\t%POS\t%REF\t%ALT\t[%GT]\t%INFO/gnomAD_AF\t%INFO/Control_AF\t%INFO/ANN\n' \
    patient_specific/patient1/patient1_candidates.vcf.gz \
    > patient_specific/patient1/patient1_candidates_table.txt

# Add header
echo -e "CHROM\tPOS\tREF\tALT\tGENOTYPE\tgnomAD_AF\tControl_AF\tAnnotation" \
    | cat - patient_specific/patient1/patient1_candidates_table.txt \
    > patient_specific/patient1/patient1_candidates_table_header.txt

mv patient_specific/patient1/patient1_candidates_table_header.txt patient_specific/patient1/patient1_candidates_table.txt
```

**Filter Explanation:**

- `-s patient1`: Extract only this patient’s genotypes

- `GT="alt"`: Keep only sites where patient has alternate allele

- `Control_AF<0.05 | Control_AF="."`: Rare or absent in controls

- `gnomAD_AF<0.01 | gnomAD_AF="."`: Rare or absent in gnomAD

- `ANN~"HIGH" | ANN~"MODERATE"`: Only functionally significant variants

> 

**To Analyze Other Patients:** Repeat the above commands for patient2, patient3, etc., by replacing “patient1” with the appropriate patient ID throughout the commands.

## Identifying Disease-Specific Variants

Disease-specific variants reveal the genetic architecture of the disease itself. These are variants that are significantly enriched in patients compared to controls, potentially representing causal genes shared across multiple affected individuals.

### Disease-Specific Variant Strategy

We compare the allele frequency of each variant between cases and controls, looking for variants that are:

1. Significantly more common in patients than controls

2. Rare in the general population (gnomAD_AF < 0.01)

3. Functionally damaging (HIGH or MODERATE impact)

4. Present in multiple patients (suggesting a shared genetic etiology)

### Calculate Case Allele Frequencies

```bash
# Calculate AF, AC, and AN for patients
bcftools +fill-tags \
    annotated/cohort_annotated.vcf.gz \
    -O z \
    -o annotated/cohort_temp_case.vcf.gz \
    -- -t AF,AC,AN -S sample_lists/patients.txt

# Rename to Case_AF, Case_AC, Case_AN
cat > header_case.txt << EOF
##INFO=<ID=Case_AF,Number=A,Type=Float,Description="Allele frequency in patient samples">
##INFO=<ID=Case_AC,Number=A,Type=Integer,Description="Allele count in patient samples">
##INFO=<ID=Case_AN,Number=1,Type=Integer,Description="Total allele number in patient samples">
EOF

bcftools annotate \
    -h header_case.txt \
    -c INFO/AF:=INFO/Case_AF,INFO/AC:=INFO/Case_AC,INFO/AN:=INFO/Case_AN \
    -O z \
    -o annotated/cohort_case_control_AF.vcf.gz \
    annotated/cohort_temp_case.vcf.gz

bcftools index annotated/cohort_case_control_AF.vcf.gz
rm annotated/cohort_temp_case.vcf.gz header_case.txt
```

### Filter for Disease-Enriched Variants

```bash
# Filter for variants enriched in cases
bcftools view \
    -i '(Case_AF > Control_AF) && (Case_AC >= 2) && (Control_AF < 0.05 | Control_AF=".") && (gnomAD_AF < 0.01 | gnomAD_AF=".") && (ANN~"HIGH" | ANN~"MODERATE")' \
    -O z \
    -o disease_specific/disease_enriched_variants.vcf.gz \
    annotated/cohort_case_control_AF.vcf.gz

bcftools index disease_specific/disease_enriched_variants.vcf.gz

# Create detailed table
bcftools query \
    -f '%CHROM\t%POS\t%REF\t%ALT\t%INFO/Case_AC\t%INFO/Case_AF\t%INFO/Control_AC\t%INFO/Control_AF\t%INFO/gnomAD_AF\t%INFO/ANN\n' \
    disease_specific/disease_enriched_variants.vcf.gz \
    > disease_specific/disease_enriched_table.txt

# Add header
echo -e "CHROM\tPOS\tREF\tALT\tCase_AC\tCase_AF\tControl_AC\tControl_AF\tgnomAD_AF\tAnnotation" \
    | cat - disease_specific/disease_enriched_table.txt \
    > disease_specific/disease_enriched_table_header.txt

mv disease_specific/disease_enriched_table_header.txt disease_specific/disease_enriched_table.txt
```

**Filter Logic:**

- `Case_AF > Control_AF`: Variant is more frequent in patients

- `Case_AC >= 2`: Variant appears in at least 2 patients

- `Control_AF < 0.05`: Rare or absent in your control cohort

- `gnomAD_AF < 0.01`: Rare in general population

- `ANN~"HIGH" | ANN~"MODERATE"`: Predicted to affect protein function

### Identify Recurrently Mutated Genes

Genes with multiple disease-enriched variants are particularly interesting as they provide strong evidence for involvement in disease pathogenesis.

```bash
# Extract gene names and count variants per gene
bcftools query -f '%INFO/ANN\n' disease_specific/disease_enriched_variants.vcf.gz \
    | grep -oP '\|[^|]+\|(?=HIGH|MODERATE)' \
    | cut -d'|' -f2 \
    | sort | uniq -c | sort -rn \
    > disease_specific/recurrent_genes.txt

# Example output
# 15 BRCA1
# 8 TP53
# 2 EGFR
```

## Advanced Filtering Strategies

### Filter by Variant Type

Different disease models may prioritize different variant types:

```bash
# Extract loss-of-function variants (most likely to cause recessive diseases)
bcftools view -i 'ANN~"HIGH"' -O z \
    -o disease_specific/disease_LoF_variants.vcf.gz \
    disease_specific/disease_enriched_variants.vcf.gz
bcftools index disease_specific/disease_LoF_variants.vcf.gz

# Extract missense variants (important for dominant diseases)
bcftools view -i 'ANN~"missense_variant"' -O z \
    -o disease_specific/disease_missense_variants.vcf.gz \
    disease_specific/disease_enriched_variants.vcf.gz
bcftools index disease_specific/disease_missense_variants.vcf.gz

# Extract splice site variants
bcftools view -i 'ANN~"splice"' -O z \
    -o disease_specific/disease_splice_variants.vcf.gz \
    disease_specific/disease_enriched_variants.vcf.gz
bcftools index disease_specific/disease_splice_variants.vcf.gz
```

### Identify Compound Heterozygous Variants

For recessive diseases, patients may carry two different mutations in the same gene. Here’s how to find potential compound heterozygous variants for patient1:

```bash
# Extract heterozygous variants for patient1
bcftools view -s patient1 -i 'GT="het"' \
    patient_specific/patient1/patient1_candidates.vcf.gz \
    | bcftools query -f '%INFO/ANN\n' \
    | grep -oP '\|[^|]+\|' \
    | sort | uniq -c \
    | awk '$1 >= 2' \
    > patient_specific/patient1/patient1_compound_het_genes.txt

# Example outpout
# 3 TTN
# 2 BRCA2
# 2 MUC16
```

> 

**Repeat for Other Patients:** Change “patient1” to analyze other patients for potential compound heterozygous variants.

## Interpreting and Prioritizing Results

### Prioritizing Candidates for Follow-up

**Tier 1 (Highest Priority):**

- Variants in genes previously associated with similar phenotypes (check OMIM, ClinVar)

- Loss-of-function variants (nonsense, frameshift, splice site) in constrained genes

- Variants present in multiple unrelated patients

- Variants that segregate with disease in families (if family data available)

**Tier 2 (Medium Priority):**

- Missense variants in critical functional domains with high pathogenicity scores

- Variants in genes in relevant biological pathways

- Novel variants in genes with biological plausibility

**Tier 3 (Lower Priority):**

- Variants of uncertain significance in genes of unknown function

- Variants in genes unrelated to disease biology

### Next Steps

1. **Manual Review:** Review top candidates against literature and databases (OMIM, ClinVar, PubMed)

2. **Validation:** Validate high-priority variants using Sanger sequencing

3. **Segregation Analysis:** If family samples available, check if variants segregate with disease

4. **Functional Studies:** Design experiments to test variant effects on protein function

5. **Clinical Reporting:** Classify variants according to ACMG/AMP guidelines

## Best Practices for Disease Variant Analysis

### Sample Selection and Study Design

- **Adequate Sample Size:** Include at least 5-10 patients and equal or greater number of controls

- **Population Matching:** Controls should match patients by ancestry, age, and sex

- **Clinical Phenotyping:** Precise phenotypic characterization helps identify disease-specific genes

- **Family Studies:** Including parents or unaffected siblings improves variant interpretation

### Filtering Strategy Considerations

- **Disease Model Matters:** Adjust filtering thresholds based on inheritance pattern:

- **Dominant diseases:** Focus on heterozygous HIGH impact variants, stricter frequency (AF < 0.0001)

- **Recessive diseases:** Look for homozygous or compound heterozygous variants (AF < 0.01-0.05)

- **X-linked diseases:** Consider hemizygous variants in male patients

- **Penetrance Considerations:** Incomplete penetrance means some variants may appear occasionally in controls

- **Allelic Heterogeneity:** Multiple different variants in same gene can cause same disease

### Variant Interpretation Guidelines

- **Use Multiple Evidence Lines:** Combine computational predictions, population frequencies, known pathogenic variants, and gene-disease associations

- **ACMG Guidelines:** Follow American College of Medical Genetics guidelines for variant classification

- **Functional Domains:** Variants in critical functional domains more likely pathogenic

- **Conservation:** Highly conserved positions suggest functional importance

### Common Pitfalls to Avoid

- **Over-reliance on Prediction Tools:** In silico predictions are helpful but imperfect

- **Ignoring Inheritance Patterns:** Consider whether variant fits expected inheritance model

- **Batch Effects:** Samples processed in different batches may have systematic differences

- **Population-Specific Variants:** Some variants rare in gnomAD may be common in specific populations

- **Overinterpreting Novel Variants:** Novel variants are not automatically pathogenic

### Validation and Follow-up

- **Sanger Sequencing:** Validate top candidates with orthogonal method

- **Segregation Analysis:** Confirm variants segregate with disease in families

- **Functional Studies:** Functional validation in cell or animal models

- **Replication Cohorts:** Identify additional patients to replicate findings

## Troubleshooting Common Issues

### Too Many Candidate Variants

**Solutions:**

- Apply stricter population frequency thresholds (gnomAD_AF < 0.001)

- Focus only on HIGH impact variants

- Require variants in multiple patients (Case_AC >= 3)

- Prioritize variants in known disease-associated genes

- Filter to genes with high constraint scores (pLI > 0.9)

### Too Few Candidate Variants

**Solutions:**

- Relax frequency thresholds (gnomAD_AF < 0.05)

- Include MODERATE impact variants

- Reduce minimum Case_AC requirement

- Check if controls are too closely related to patients

- Consider complex/polygenic inheritance

### Poor Case-Control Separation

**Solutions:**

- Verify sample labeling is correct

- Check for population stratification using PCA

- Ensure controls don’t have subclinical disease

- Try pathway or gene-set enrichment approaches

### Inconsistent Results Across Patients

**Solutions:**

- Consider genetic heterogeneity (different genes cause same phenotype)

- Look for allelic heterogeneity (different variants in same gene)

- Perform gene-level or pathway-level analysis

- Check for compound heterozygous variants

- Refine phenotyping to ensure patients have same condition

## Conclusion

Congratulations! You have successfully identified disease-specific and patient-specific variants from whole genome sequencing data. This analysis represents a crucial step in translating genomic data into biological insights and clinical applications.

## References

- McKenna A, et al. (2010). “The Genome Analysis Toolkit: a MapReduce framework for analyzing next-generation DNA sequencing data.” *Genome Research*, 20(9):1297-303. doi:10.1101/gr.107524.110

- DePristo MA, et al. (2011). “A framework for variation discovery and genotyping using next-generation DNA sequencing data.” *Nature Genetics*, 43(5):491-8. doi:10.1038/ng.806

- Bamshad MJ, et al. (2011). “Exome sequencing as a tool for Mendelian disease gene discovery.” *Nature Reviews Genetics*, 12(11):745-55. doi:10.1038/nrg3031

- Chong JX, et al. (2015). “The Genetic Basis of Mendelian Phenotypes: Discoveries, Challenges, and Opportunities.” *American Journal of Human Genetics*, 97(2):199-215. doi:10.1016/j.ajhg.2015.06.009

- Lee S, et al. (2014). “Optimal unified approach for rare-variant association testing with application to small-sample case-control whole-exome sequencing studies.” *American Journal of Human Genetics*, 95(2):224-35. doi:10.1016/j.ajhg.2014.07.007

- Richards S, et al. (2015). “Standards and guidelines for the interpretation of sequence variants: a joint consensus recommendation of the American College of Medical Genetics and Genomics and the Association for Molecular Pathology.” *Genetics in Medicine*, 17(5):405-24. doi:10.1038/gim.2015.30

- McLaren W, et al. (2016). “The Ensembl Variant Effect Predictor.” *Genome Biology*, 17(1):122. doi:10.1186/s13059-016-0974-4

- Cingolani P, et al. (2012). “A program for annotating and predicting the effects of single nucleotide polymorphisms, SnpEff.” *Fly*, 6(2):80-92. doi:10.4161/fly.19695

- Karczewski KJ, et al. (2020). “The mutational constraint spectrum quantified from variation in 141,456 humans.” *Nature*, 581(7809):434-443. doi:10.1038/s41586-020-2308-7

- Lek M, et al. (2016). “Analysis of protein-coding genetic variation in 60,706 humans.” *Nature*, 536(7616):285-91. doi:10.1038/nature19057

- Samocha KE, et al. (2014). “A framework for the interpretation of de novo mutation in human disease.” *Nature Genetics*, 46(9):944-50. doi:10.1038/ng.3050

- Kamphans T, et al. (2013). “Filtering for compound heterozygous sequence variants in non-consanguineous pedigrees.” *PLoS One*, 8(8):e70151. doi:10.1371/journal.pone.0070151

- Li B, Leal SM. (2008). “Methods for detecting associations with rare variants for common diseases: application to analysis of sequence data.” *American Journal of Human Genetics*, 83(3):311-21. doi:10.1016/j.ajhg.2008.06.024

- Emily R. Daubney,Christopher Flatley,Liang-Dar Hwang,David M. Evans,Proteome-Wide Mendelian Randomisation Study of Adverse Perinatal Outcomes, Behavior Genetics, (2025). https://doi.org/10.1007/s10519-025-10233-1

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, **please comment and share** **your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.*
