# How To Perform Genome-Wide Association Analysis (GWAS) For Absolute Beginners: From Raw Variants to Disease-Associated Loci Using PLINK

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/)

- [Introduction: Understanding Genome-Wide Association Studies](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Introduction_Understanding_Genome-Wide_Association_Studies)

  - [What is GWAS and Why Does It Matter?](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#What_is_GWAS_and_Why_Does_It_Matter)
  - [Biological Insights from GWAS Data](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Biological_Insights_from_GWAS_Data)
  - [Why PLINK for This Tutorial?](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Why_PLINK_for_This_Tutorial)
  - [GWAS Workflow Overview](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#GWAS_Workflow_Overview)

- [Setting Up Your GWAS Analysis Environment](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Setting_Up_Your_GWAS_Analysis_Environment)

  - [Adding GWAS Tools to Your Existing Environment](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Adding_GWAS_Tools_to_Your_Existing_Environment)

- [Preparing Genome-Wide Data for GWAS Analysis](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Preparing_Genome-Wide_Data_for_GWAS_Analysis)

  - [Option 1: Using Your Own WGS Variant Calls](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Option_1_Using_Your_Own_WGS_Variant_Calls)
  - [Option 2: Download 1000 Genomes Data for Practice](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Option_2_Download_1000_Genomes_Data_for_Practice)
  - [Complete Script: Download, Preprocess, and Prepare Data](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Complete_Script_Download_Preprocess_and_Prepare_Data)
  - [Understanding PLINK File Formats](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_PLINK_File_Formats)
  - [Understanding WGS VCF Characteristics](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_WGS_VCF_Characteristics)

- [Quality Control: The Foundation of Reliable GWAS](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Quality_Control_The_Foundation_of_Reliable_GWAS)

  - [Understanding QC Philosophy](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_QC_Philosophy)
  - [Step 1: Sample-Level Quality Control](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Step_1_Sample-Level_Quality_Control)
  - [Step 2: Sex Determination from X Chromosome](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Step_2_Sex_Determination_from_X_Chromosome)
  - [Step 3: Variant-Level Quality Control](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Step_3_Variant-Level_Quality_Control)
  - [Step 4: Generate QC Report](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Step_4_Generate_QC_Report)

- [Population Stratification: Controlling for Genetic Ancestry](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Population_Stratification_Controlling_for_Genetic_Ancestry)

  - [Understanding Population Stratification](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_Population_Stratification)
  - [What is LD Pruning?](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#What_is_LD_Pruning)
  - [Performing PCA for Ancestry Analysis](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Performing_PCA_for_Ancestry_Analysis)
  - [Visualizing Population Structure](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Visualizing_Population_Structure)
  - [Interpreting PCA Results](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Interpreting_PCA_Results)

- [Creating a Simulated Phenotype for GWAS](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Creating_a_Simulated_Phenotype_for_GWAS)

  - [Understanding Phenotype Simulation](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_Phenotype_Simulation)
  - [Simulating the Phenotype](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Simulating_the_Phenotype)

- [Statistical Association Testing: The Heart of GWAS](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Statistical_Association_Testing_The_Heart_of_GWAS)

  - [Understanding Association Test Statistics](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_Association_Test_Statistics)
  - [Performing Genome-Wide Association Analysis](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Performing_Genome-Wide_Association_Analysis)
  - [Understanding the GWAS Output File](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_the_GWAS_Output_File)
  - [Understanding GWAS Significance Thresholds](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_GWAS_Significance_Thresholds)
  - [Extracting Significant Results](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Extracting_Significant_Results)
  - [Understanding Results for Simulated Data](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_Results_for_Simulated_Data)

- [Visualizing GWAS Results: Manhattan and QQ Plots](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Visualizing_GWAS_Results_Manhattan_and_QQ_Plots)

  - [Creating Publication-Quality Visualizations](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Creating_Publication-Quality_Visualizations)
  - [Interpreting Manhattan and QQ Plots](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Interpreting_Manhattan_and_QQ_Plots)

- [Replication and Meta-Analysis: Validating GWAS Findings](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Replication_and_Meta-Analysis_Validating_GWAS_Findings)

  - [Understanding Replication Studies](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Understanding_Replication_Studies)
  - [Meta-Analysis Overview](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Meta-Analysis_Overview)

- [Best Practices for GWAS Analysis](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Best_Practices_for_GWAS_Analysis)

  - [Study Design Considerations](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Study_Design_Considerations)
  - [Quality Control Standards](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Quality_Control_Standards)
  - [Statistical Analysis Guidelines](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Statistical_Analysis_Guidelines)

- [Common Pitfalls and Troubleshooting](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Common_Pitfalls_and_Troubleshooting)

  - [Frequent Issues and Solutions](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Frequent_Issues_and_Solutions)
  - [Quality Control Red Flags](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Quality_Control_Red_Flags)

- [Conclusion: From Analysis to Discovery](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Conclusion_From_Analysis_to_Discovery)

  - [Next Steps in Your GWAS Journey](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Next_Steps_in_Your_GWAS_Journey)

- [References](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#References)

  - [Foundational GWAS Papers](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Foundational_GWAS_Papers)
  - [PLINK Software](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#PLINK_Software)
  - [1000 Genomes Project](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#1000_Genomes_Project)
  - [Population Stratification and Principal Component Analysis](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Population_Stratification_and_Principal_Component_Analysis)
  - [Quality Control Guidelines](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Quality_Control_Guidelines)
  - [Hardy-Weinberg Equilibrium](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Hardy-Weinberg_Equilibrium)
  - [Statistical Methods and Multiple Testing](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Statistical_Methods_and_Multiple_Testing)
  - [Genomic Inflation and Lambda](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Genomic_Inflation_and_Lambda)
  - [Linkage Disequilibrium](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Linkage_Disequilibrium)
  - [Meta-Analysis](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Meta-Analysis)
  - [Replication and Validation](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Replication_and_Validation)
  - [Visualization](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Visualization)
  - [VCF Processing Tools](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#VCF_Processing_Tools)
  - [GWAS Catalog and Databases](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#GWAS_Catalog_and_Databases)
  - [Online Resources](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Online_Resources)
  - [Share this:](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Share_this)
  - [Like this:](https://ngs101.com/how-to-perform-genome-wide-association-analysis-gwas-for-absolute-beginners-from-raw-variants-to-disease-associated-loci-using-plink/#Like_this)

## Introduction: Understanding Genome-Wide Association Studies

After successfully calling variants from whole genome sequencing data (covered in [Part 1 of our WGS series](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/)), you now have VCF files containing millions of genetic variants across multiple individuals. But which of these variants contribute to disease risk or influence quantitative traits? This is where Genome-Wide Association Studies (GWAS) come into play.

### What is GWAS and Why Does It Matter?

Genome-Wide Association Studies (GWAS) are powerful statistical approaches that scan millions of genetic variants across many individuals to identify associations between specific variants and traits or diseases. Think of GWAS as casting a wide net across the entire genome to catch genetic variants that correlate with observable characteristics—whether that’s disease susceptibility, drug response, or quantitative traits like height or cholesterol levels.

The fundamental principle behind GWAS is elegantly simple: if a genetic variant is more common in individuals with a particular trait or disease compared to those without it, that variant may be involved in causing or influencing that trait. However, the implementation requires sophisticated statistical methods to distinguish true associations from random noise across millions of tests.

**A Concrete Example:** Consider Type 2 diabetes. Early GWAS studies identified variants in the TCF7L2 gene that substantially increase diabetes risk. Individuals carrying risk alleles at this locus have approximately 1.4-fold higher odds of developing diabetes. This discovery not only advanced our understanding of diabetes biology but also enabled better risk prediction and suggested new therapeutic targets.

> 

**Key Insight:** GWAS doesn’t just find “disease genes”—it identifies statistical associations between variants and traits. Biological validation and functional studies are essential follow-up steps to understand causality.

### Biological Insights from GWAS Data

GWAS provides several layers of biological insight that extend far beyond simple variant-trait associations:

**Gene Discovery:** GWAS often identifies entirely unexpected genes involved in disease processes. For example, early GWAS for inflammatory bowel disease discovered over 200 genetic loci, many in genes not previously suspected to play roles in immune function or gut biology. These discoveries opened new research avenues and challenged existing disease models.

**Pathway Elucidation:** By examining which genes harbor disease-associated variants, researchers can identify biological pathways crucial for disease development. If multiple associated variants fall in genes involved in lipid metabolism, this suggests that pathway’s importance in the disease process.

**Pleiotropy and Shared Biology:** GWAS reveals that many genetic variants influence multiple traits—a phenomenon called pleiotropy. For instance, variants in the FTO gene associate with both obesity and depression risk, suggesting shared biological mechanisms between metabolic and psychiatric conditions.

### Why PLINK for This Tutorial?

We’ve chosen PLINK for this tutorial because it’s the gold standard tool for GWAS analysis, offering an optimal balance of power and accessibility for beginners. PLINK handles genome-wide data efficiently even on modest hardware, has extensive documentation refined over two decades of use, and most importantly, its straightforward command-line interface makes each analysis step explicit and understandable.

> 

**Learning Path:** Master PLINK first to understand GWAS fundamentals. Once comfortable, explore specialized tools like BOLT-LMM or REGENIE for specific advanced applications.

### GWAS Workflow Overview

The complete GWAS workflow involves multiple critical steps, each designed to ensure accurate and reproducible results:

1. **Data Preparation and Quality Control**

  - Remove low-quality variants and samples with excessive missing data

  - Filter variants by minor allele frequency (MAF) to focus on common variants

  - Test for Hardy-Weinberg equilibrium to identify genotyping errors

  - Identify and handle related individuals or duplicates (when necessary)

2. **Population Stratification Control**

  - Detect and correct for population structure using principal component analysis

  - Prevents spurious associations due to ancestry differences

3. **Statistical Association Testing**

  - Test each variant for association with the trait of interest

  - Account for covariates like age, sex, and principal components

4. **Multiple Testing Correction**

  - Apply stringent significance thresholds to control false discovery rate

  - Standard genome-wide significance: p < 5×10⁻⁸

5. **Results Visualization**

  - Generate Manhattan plots showing association signals across the genome

  - Create QQ plots to assess overall test statistics distribution

6. **Biological Interpretation**

  - Annotate significant variants with functional information

  - Integrate with gene expression, regulatory, and pathway data

  - Prioritize variants and genes for functional follow-up

7. **Replication and Meta-Analysis**

  - Validate findings in independent cohorts

  - Combine results across studies for increased power

Each step includes critical quality control checkpoints and decision points that can profoundly affect results. This tutorial will guide you through every stage with practical examples and interpretation guidelines.

## Setting Up Your GWAS Analysis Environment

If you’ve completed our [WGS variant calling tutorial](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/), you already have the `wgs_analysis` conda environment set up. We’ll add GWAS-specific tools to this existing environment.

### Adding GWAS Tools to Your Existing Environment

```r
# Activate the WGS analysis environment from the previous tutorial
conda activate wgs_analysis

# Install PLINK 1.9 (standard version for most GWAS workflows)
conda install -y plink

# Install additional GWAS-specific tools
conda install -y \
    parallel \
    king \
    gcta

# Install R packages for GWAS visualization
R --vanilla << 'EOF'
# Install packages if not already present
packages <- c("qqman", "CMplot")
new_packages <- packages[!(packages %in% installed.packages()[,"Package"])]
if(length(new_packages)) install.packages(new_packages, repos="https://cloud.r-project.org/")
EOF

# Verify installations
plink --version
parallel --version
king --version
gcta64 --version
```

## Preparing Genome-Wide Data for GWAS Analysis

You have two options for obtaining variant data for GWAS: use your own WGS data from the previous tutorial, or download public data from the 1000 Genomes Project for practice.

### Option 1: Using Your Own WGS Variant Calls

If you completed the [WGS variant calling tutorial](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/), you already have joint-called VCF files that demonstrate the correct data format for GWAS analysis. However, **GWAS requires large sample sizes to detect meaningful associations**—typically at least several hundred individuals, with thousands preferred for adequate statistical power. The small example dataset from our previous tutorial (with just a few samples for demonstration) is not suitable for actual GWAS analysis, as it lacks the statistical power to detect true genetic associations. If you have access to a large cohort with WGS data (N > 500 individuals with phenotype information), you can apply the preprocessing workflow shown below to convert your `cohort_filtered.vcf.gz` file into PLINK format.

**Your VCF files from the WGS tutorial are already optimized for GWAS because:**

- Joint calling provides uniform genotype calls across the entire cohort

- Better accuracy for rare variants through shared information

- Consistent missing data patterns compared to merged single-sample VCFs

- More accurate genotype probabilities for association testing

#### Converting Your WGS VCF to PLINK Format

Here’s how to preprocess and convert your `cohort_filtered.vcf.gz` file for PLINK analysis:

```bash
# Create directory for GWAS analysis
mkdir -p ~/gwas_analysis/{data,results,plots,qc}
cd ~/gwas_analysis/data

# Step 1: Preprocess your VCF file
# This handles multi-allelic variants and ensures PLINK compatibility
echo "Preprocessing VCF file..."

# Split multi-allelic variants, remove duplicates, keep only bi-allelic variants
bcftools norm -m -any ~/wgs_tutorial/cohort_filtered.vcf.gz | \
bcftools norm -d all | \
bcftools view -m2 -M2 -v snps,indels \
    -O z -o my_cohort.clean.vcf.gz

# Index the cleaned VCF
tabix -p vcf my_cohort.clean.vcf.gz

# Step 2: Convert to PLINK binary format
echo "Converting to PLINK format..."
plink --vcf my_cohort.clean.vcf.gz \
    --double-id \
    --biallelic-only strict \
    --set-missing-var-ids "@:#_\$1_\$2" \
    --make-bed \
    --out my_cohort_raw

echo "✓ Conversion complete! PLINK files created:"
echo "  - my_cohort_raw.bed (binary genotype data)"
echo "  - my_cohort_raw.bim (variant information)"
echo "  - my_cohort_raw.fam (sample information)"
```

**What This Preprocessing Does:**

**`bcftools norm -m -any`:** Splits multi-allelic variants

- WGS discovers positions with 3+ different alleles across individuals

- Example: Position chr1:12345 might have A, C, and T alleles

- PLINK requires bi-allelic (2 alleles only), so we split into separate records

**`bcftools norm -d all`:** Removes duplicate positions

- After splitting, true duplicates may exist

- Ensures each position appears once

**`bcftools view -m2 -M2`:** Keeps only bi-allelic variants

- `-m2`: minimum 2 alleles (removes monomorphic sites)

- `-M2`: maximum 2 alleles (enforces bi-allelic)

**`--set-missing-var-ids "@:#_\$1_\$2"`:** Assigns IDs to unnamed variants

- Many WGS variants lack rsIDs

- Creates position-based IDs: chromosome:position_ref_alt

- Example: 11:89920705_C_T

> 

**Important:** This preprocessing is essential for all WGS data, not just the example data used here. It’s a standard practice in professional GWAS pipelines.

### Option 2: Download 1000 Genomes Data for Practice

For this tutorial, we’ll use the 1000 Genomes Project Phase 3 dataset as a practice dataset. This provides a comprehensive example with 2,504 individuals across 26 populations, demonstrating real-world GWAS workflows.

**Why Use the Complete Dataset?**

- **Realistic GWAS:** Demonstrates true genome-wide association analysis across all chromosomes

- **Complete Workflow:** Enables all QC steps including sex checks using X chromosome data

- **Publication-Quality Results:** Produces proper Manhattan plots spanning the entire genome

- **Real Population Structure:** Contains authentic ancestry patterns for stratification analysis

- **Educational Value:** Teaches the complete workflow you’ll use with your own data

### Complete Script: Download, Preprocess, and Prepare Data

This comprehensive script handles everything from downloading raw VCFs to producing PLINK-ready files:

```bash
#!/bin/bash
#================================================================
# GWAS Data Preparation Pipeline
# Downloads, preprocesses, and converts 1000 Genomes VCF data
# Optimized with GNU Parallel for multi-core systems
#================================================================

# Exit on any error
set -e

# Create organized directory structure
echo "Creating directory structure..."
mkdir -p ~/gwas_tutorial/{data,results,plots,qc}
cd ~/gwas_tutorial/data

#================================================================
# STEP 1: Download 1000 Genomes Phase 3 Data
#================================================================
echo ""
echo "========================================"
echo "STEP 1: Downloading 1000 Genomes Data"
echo "========================================"

# Download all autosomal chromosomes (chr1-22)
echo "Downloading chromosomes 1-22..."
for CHR in {1..22}; do
    if [ ! -f "ALL.chr${CHR}.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz" ]; then
        echo "  Downloading chromosome ${CHR}..."
        wget -q https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr${CHR}.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz
        wget -q https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr${CHR}.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz.tbi
    else
        echo "  Chromosome ${CHR} already downloaded, skipping..."
    fi
done

# Download X chromosome for sex checks
if [ ! -f "ALL.chrX.phase3_shapeit2_mvncall_integrated_v1c.20130502.genotypes.vcf.gz" ]; then
    echo "Downloading X chromosome..."
    wget -q https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chrX.phase3_shapeit2_mvncall_integrated_v1c.20130502.genotypes.vcf.gz
    wget -q https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chrX.phase3_shapeit2_mvncall_integrated_v1c.20130502.genotypes.vcf.gz.tbi
else
    echo "X chromosome already downloaded, skipping..."
fi

# Download sample population information
if [ ! -f "integrated_call_samples_v3.20130502.ALL.panel" ]; then
    echo "Downloading sample information..."
    wget -q https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/integrated_call_samples_v3.20130502.ALL.panel
else
    echo "Sample information already downloaded, skipping..."
fi

# Verify downloads
NFILES=$(ls -1 ALL.chr*.vcf.gz 2>/dev/null | wc -l)
if [ "$NFILES" -eq 23 ]; then
    echo "✓ All 23 chromosome files downloaded successfully"
else
    echo "✗ ERROR: Expected 23 files, found ${NFILES}"
    exit 1
fi

#================================================================
# STEP 2: Preprocess VCF Files with Parallel Processing
#================================================================
echo ""
echo "========================================"
echo "STEP 2: Preprocessing VCF Files"
echo "========================================"

# Silence GNU Parallel citation notice
parallel --citation <<< "will cite" 2>/dev/null || true

# Determine optimal parallel job count
NCORES=$(nproc)
NJOBS=$((NCORES / 4))
echo "System has ${NCORES} cores, using ${NJOBS} parallel jobs"

# Process autosomal chromosomes in parallel
echo "Processing chromosomes 1-22 with parallel processing..."
parallel -j ${NJOBS} --progress '
    CHR={}

    # Check if already processed
    if [ -f "1kg_chr${CHR}_final.bed" ]; then
        echo "Chromosome ${CHR} already processed, skipping..."
        exit 0
    fi

    echo "Processing chromosome ${CHR}..."

    # Split multi-allelic variants, remove duplicates, ensure bi-allelic only
    bcftools norm -m -any --threads 2 \
        ALL.chr${CHR}.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz 2>/dev/null | \
    bcftools norm -d all --threads 2 2>/dev/null | \
    bcftools view -m2 -M2 -v snps,indels --threads 2 \
        -O z -o ALL.chr${CHR}.clean.vcf.gz 2>/dev/null

    tabix -p vcf ALL.chr${CHR}.clean.vcf.gz

    # Convert to PLINK format with unique IDs for missing variants
    plink --vcf ALL.chr${CHR}.clean.vcf.gz \
        --threads 2 \
        --double-id \
        --biallelic-only strict \
        --set-missing-var-ids "@:#_\$1_\$2" \
        --make-bed \
        --out 1kg_chr${CHR}_final \
        --silent 2>&1 | grep -v "variant with" || true

    echo "Chromosome ${CHR} complete!"
' ::: {1..22}

# Process X chromosome separately
echo "Processing X chromosome..."
if [ ! -f "1kg_chrX_final.bed" ]; then
    bcftools norm -m -any --threads 4 \
        ALL.chrX.phase3_shapeit2_mvncall_integrated_v1c.20130502.genotypes.vcf.gz 2>/dev/null | \
    bcftools norm -d all --threads 4 2>/dev/null | \
    bcftools view -m2 -M2 -v snps,indels --threads 4 \
        -O z -o ALL.chrX.clean.vcf.gz 2>/dev/null

    tabix -p vcf ALL.chrX.clean.vcf.gz

    plink --vcf ALL.chrX.clean.vcf.gz \
        --threads 4 \
        --double-id \
        --biallelic-only strict \
        --set-missing-var-ids "@:#_\$1_\$2" \
        --make-bed \
        --out 1kg_chrX_final \
        --silent 2>&1 | grep -v "variant with" || true
else
    echo "X chromosome already processed, skipping..."
fi

echo "✓ VCF preprocessing complete!"

#================================================================
# STEP 3: Merge All Chromosomes
#================================================================
echo ""
echo "========================================"
echo "STEP 3: Merging Chromosomes"
echo "========================================"

if [ -f "1kg_all_raw.bed" ]; then
    echo "Merged dataset already exists, skipping..."
else
    # Create merge list (chr2-22 + X)
    rm -f merge_list.txt
    for CHR in {2..22} X; do
        echo "1kg_chr${CHR}_final" >> merge_list.txt
    done

    echo "Merging 23 chromosomes into single dataset..."
    plink --bfile 1kg_chr1_final \
        --merge-list merge_list.txt \
        --make-bed \
        --out 1kg_all_raw \
        --silent 2>&1 | tail -2

    echo "✓ Chromosome merging complete!"
fi

#================================================================
# STEP 4: Verification and Summary
#================================================================
echo ""
echo "========================================"
echo "STEP 4: Verification"
echo "========================================"

# Generate basic statistics
plink --bfile 1kg_all_raw --freq --out 1kg_all_raw --silent 2>&1 | tail -2

# Print summary
echo ""
echo "========================================"
echo "Data Preparation Summary"
echo "========================================"
echo "Total variants: $(wc -l < 1kg_all_raw.bim)"
echo "Total samples: $(wc -l < 1kg_all_raw.fam)"
echo "Output files: 1kg_all_raw.bed/.bim/.fam"
echo ""
echo "✓ Data preparation pipeline complete!"
echo "You are now ready to proceed with Quality Control."
echo "========================================"
```

**Save this script and run it:**

```bash
# Save the script
cat > ~/gwas_tutorial/prepare_data.sh << 'EOF'
"paste the script above"
EOF

# Make it executable
chmod +x ~/gwas_tutorial/prepare_data.sh

# Run the pipeline
cd ~/gwas_tutorial
./prepare_data.sh
```

**Expected Output:**

```
Data Preparation Summary
========================================
Total variants: 84587660
Total samples: 2504
Output files: 1kg_all_raw.bed/.bim/.fam

✓ Data preparation pipeline complete!
```

### Understanding PLINK File Formats

PLINK uses a binary format consisting of three files that work together:

#### 1. BED File (.bed) – Binary Genotype Data

The `.bed` file stores genotype information in compressed binary format. This file is not human-readable but is very efficient for processing millions of variants.

**What it contains:**

- Genotype calls for each individual at each variant

- Coded as: 0/0 (homozygous reference), 0/1 (heterozygous), 1/1 (homozygous alternate), or missing

#### 2. BIM File (.bim) – Variant Information

The `.bim` file is a tab-delimited text file with one line per variant. Here’s an example:

```bash
# Chromosome  Variant_ID         Position(cM)  Position(bp)  Allele1  Allele2
1             1:10177_A_AC       0             10177         A        AC
1             rs367896724        0             10235         A        AC
1             1:10352_T_TA       0             10352         T        TA
11            11:89920705_C_T    0             89920705      C        T
11            11:89921232_C_T    0             89921232      T        C
```

**Column descriptions:**

- **Column 1:** Chromosome (1-22, X, Y, MT)

- **Column 2:** Variant identifier (rsID or position-based ID)

- **Column 3:** Genetic distance in centiMorgans (usually 0 if unknown)

- **Column 4:** Base-pair position

- **Column 5:** Allele 1 (usually reference allele)

- **Column 6:** Allele 2 (usually alternate allele)

#### 3. FAM File (.fam) – Sample Information

The `.fam` file is a tab-delimited text file with one line per individual. Here’s an example:

```bash
# FID     IID       PID  MID  Sex  Phenotype
HG00096   HG00096   0    0    1    -9
HG00097   HG00097   0    0    2    -9
HG00099   HG00099   0    0    2    -9
NA18486   NA18486   0    0    2    -9
NA18488   NA18488   0    0    1    -9
```

**Column descriptions:**

- **Column 1 (FID):** Family ID

- **Column 2 (IID):** Individual ID within family

- **Column 3 (PID):** Paternal ID (0 if unknown/unrelated)

- **Column 4 (MID):** Maternal ID (0 if unknown/unrelated)

- **Column 5 (Sex):** Sex (1=male, 2=female, 0=unknown)

- **Column 6 (Phenotype):** Phenotype value (-9 or 0=missing, 1=control, 2=case for binary traits)

> 

**Note:** For quantitative traits, phenotype values are actual measurements. For case-control studies, use 1 (control) and 2 (case).

### Understanding WGS VCF Characteristics

Before moving to quality control, it’s important to understand that **whole genome sequencing VCF files have inherent characteristics that differ from genotyping array data**. These are normal biological features requiring proper handling:

**Multi-allelic Variants (5-10% of WGS variants):**

- Some genomic positions have 3+ different alleles across human populations

- Unlike genotyping arrays that only test pre-selected bi-allelic SNPs, WGS discovers all variation

- PLINK requires bi-allelic variants, so we split them into multiple records

- This is why preprocessing with `bcftools norm -m -any` is essential

**Missing Variant IDs (30-50% of WGS variants):**

- Many WGS-discovered variants lack rsIDs in dbSNP

- Novel variants, population-specific variants, and private mutations are uncatalogued

- We assign position-based IDs to ensure unique identification

**Duplicate Positions:**

- After splitting multi-allelic variants, some positions may have true duplicates

- Removed with `bcftools norm -d all` to prevent merge conflicts

**These preprocessing steps are universal—not specific to 1000 Genomes.** Every WGS dataset (including your own from the previous tutorial) requires the same handling. These are standard practices in professional GWAS pipelines.

## Quality Control: The Foundation of Reliable GWAS

Quality control is arguably the most critical phase of GWAS analysis. Poor QC leads to spurious associations, missed true signals, and irreproducible results. We’ll implement a rigorous, multi-stage QC workflow.

### Understanding QC Philosophy

For this tutorial using 1000 Genomes data with simulated phenotypes, we’ll implement a **streamlined QC approach** that focuses on data quality without removing samples unnecessarily. In real GWAS with actual case-control data or quantitative traits, you may need additional steps like relatedness filtering when family structure could confound associations. However, for unrelated individuals or simulated phenotypes, sample-level relatedness filtering is typically unnecessary and can remove valuable data.

### Step 1: Sample-Level Quality Control

```bash
cd ~/gwas_tutorial/results

# Remove samples with >5% missing genotype calls
echo "Step 1: Filtering samples by missing genotype rate..."
plink --bfile ../data/1kg_all_raw \
    --missing \
    --out qc_missing

plink --bfile ../data/1kg_all_raw \
    --mind 0.05 \
    --make-bed \
    --out 1kg_qc1

echo "Samples after missingness filter: $(wc -l < 1kg_qc1.fam)"
```

**What This Does:**

- `--missing`: Calculates per-sample missing genotype rates

- `--mind 0.05`: Removes samples with >5% missing genotypes

- Ensures we only analyze samples with high-quality genotype data

### Step 2: Sex Determination from X Chromosome

```bash
# Infer genetic sex from X chromosome homozygosity
echo "Step 2: Inferring genetic sex from X chromosome..."
plink --bfile 1kg_qc1 \
    --check-sex \
    --out sex_check

plink --bfile 1kg_qc1 \
    --impute-sex \
    --make-bed \
    --out 1kg_qc2

echo "Sex distribution after imputation:"
cut -d' ' -f5 1kg_qc2.fam | sort | uniq -c
```

**What This Does:**

- `--check-sex`: Calculates X chromosome homozygosity (F statistic)

  - F < 0.2: Female (XX)

  - F > 0.8: Male (XY)

  - 0.2 < F < 0.8: Ambiguous

- `--impute-sex`: Updates sex information in the dataset based on genetic data

**Expected Output:**

```
Sex distribution after imputation:
      690 0    # Ambiguous (F ≈ 0.3-0.7)
     1237 1    # Males
      577 2    # Females
```

**Note on “Sex Discrepancies”:** If you see samples flagged as “PROBLEM” in the `.sexcheck` file, these are typically samples where original sex was unknown (coded as 0) but genetic sex was successfully inferred. These are NOT true discrepancies and should NOT be removed—the `--impute-sex` command already corrected them.

**Example .sexcheck file:**

```
      FID       IID       PEDSEX       SNPSEX       STATUS            F
  HG00096   HG00096            0            1      PROBLEM       0.9529
  HG00097   HG00097            0            0      PROBLEM       0.2645
  HG00099   HG00099            0            0      PROBLEM       0.2554
  HG00100   HG00100            0            0      PROBLEM        0.286
  HG00101   HG00101            0            1      PROBLEM        0.953
  HG00102   HG00102            0            0      PROBLEM       0.2007
```

**Column 1 (FID): **Family ID**Column 2 (IID): **Individual ID**Column 3 (PEDSEX): **Pedigree/Reported Sex, 1=male, 2=female, 0=unknown**Column 4 (SNPSEX ): **SNP-inferred Sex, 1=male, 2=female, 0=unknown**Column 5 (STATUS): **Comparison between PEDSEX and SNPSEX, OK: PEDSEX matches SNPSEX, PROBLEM: PEDSEX and SNPSEX disagree**Column 6 (F): **F coefficient used to infer sex

### Step 3: Variant-Level Quality Control

Now we apply stringent filters to variants to ensure high-quality association testing:

```bash
# Remove SNPs with >5% missing genotypes
echo "Step 3: Filtering variants by missing genotype rate..."
plink --bfile 1kg_qc2 \
    --geno 0.05 \
    --make-bed \
    --out 1kg_qc3

echo "Variants after missingness filter: $(wc -l < 1kg_qc3.bim)"
# Variants after missingness filter: 84587660

# Remove SNPs with MAF < 0.01 (1%)
echo "Step 4: Filtering variants by minor allele frequency..."
plink --bfile 1kg_qc3 \
    --maf 0.01 \
    --make-bed \
    --out 1kg_qc4

echo "Variants after MAF filter: $(wc -l < 1kg_qc4.bim)"
# Variants after MAF filter: 14054398

# Remove SNPs deviating from Hardy-Weinberg equilibrium
echo "Step 5: Filtering variants by Hardy-Weinberg equilibrium..."
plink --bfile 1kg_qc4 \
    --hwe 1e-6 \
    --make-bed \
    --out 1kg_clean_final

echo "Variants after HWE filter: $(wc -l < 1kg_clean_final.bim)"
# Variants after HWE filter: 10946463
```

**Filter Explanations:**

**Missing Genotype Rate (`--geno 0.05`):**

- Removes variants with >5% missing data across samples

- Missing data can indicate genotyping errors or technical issues

- Prevents spurious associations due to differential missingness between cases/controls

**Minor Allele Frequency (`--maf 0.01`):**

- Removes rare variants with MAF < 1%

- Rare variants have low statistical power and are more susceptible to genotyping errors

- Focus analysis on common variants where GWAS has optimal power

- For rare variant analysis, specialized methods like SKAT or burden tests are more appropriate

**Hardy-Weinberg Equilibrium (`--hwe 1e-6`):**

- Tests whether genotype frequencies match expectations under random mating

- Deviation suggests genotyping errors, population stratification, or selection

- Stringent threshold (p < 10⁻⁶) removes likely technical artifacts

- Applied to control samples or entire dataset if no case-control distinction

### Step 4: Generate QC Report

```bash
# Generate comprehensive QC statistics
echo "Generating final QC statistics..."
plink --bfile 1kg_clean_final \
    --missing \
    --freq \
    --hardy \
    --out final_qc_stats

# Output files
# final_qc_stats.imiss    # Sample (individual) missingness
# final_qc_stats.lmiss    # Variant (locus) missingness
# final_qc_stats.frq      # Allele frequencies
# final_qc_stats.hwe      # Hardy-Weinberg equilibrium tests
# final_qc_stats.log      # Log file with command details

# Create QC summary
echo "=== Quality Control Summary ===" > ../qc/qc_summary.txt
echo "Original samples: $(wc -l < ../data/1kg_all_raw.fam)" >> ../qc/qc_summary.txt
echo "Final samples: $(wc -l < 1kg_clean_final.fam)" >> ../qc/qc_summary.txt
echo "Sample retention: $(awk "BEGIN {printf \"%.1f%%\", $(wc -l < 1kg_clean_final.fam) / $(wc -l < ../data/1kg_all_raw.fam) * 100}")" >> ../qc/qc_summary.txt
echo "" >> ../qc/qc_summary.txt
echo "Original variants: $(wc -l < ../data/1kg_all_raw.bim)" >> ../qc/qc_summary.txt
echo "Final variants: $(wc -l < 1kg_clean_final.bim)" >> ../qc/qc_summary.txt
echo "Variant retention: $(awk "BEGIN {printf \"%.1f%%\", $(wc -l < 1kg_clean_final.bim) / $(wc -l < ../data/1kg_all_raw.bim) * 100}")" >> ../qc/qc_summary.txt

cat ../qc/qc_summary.txt
```

**Example QC Results:**

```
=== Quality Control Summary ===
Original samples: 2504
Final samples: 2504
Sample retention: 100.0%

Original variants: 84587660
Final variants: 10946463
Variant retention: 12.9%
```

**Why 87% Variant Reduction is Excellent:**

- Removes low-quality variants that cause spurious associations

- Removes rare variants with low statistical power (MAF < 1%)

- Removes potential genotyping errors (HWE violations)

- Retains ~11 million high-quality, common variants

- This is the optimal dataset for genome-wide association testing

## Population Stratification: Controlling for Genetic Ancestry

Population stratification—systematic ancestry differences between cases and controls—is a major source of spurious associations in GWAS. We’ll use principal component analysis (PCA) to detect and correct for population structure.

### Understanding Population Stratification

**Why Population Stratification Matters:** Imagine conducting a GWAS for lactose intolerance in a mixed European-Asian cohort. European ancestry correlates with both:

1. Lower lactose intolerance rates (due to evolutionary selection for lactase persistence)

2. European-specific genetic variants

Without correction, European-ancestry variants will spuriously associate with lactose tolerance, even if they’re not causal. PCA identifies these ancestry-informative variants and creates principal components (PCs) that capture population structure, which we then include as covariates in association testing.

**The 1000 Genomes Advantage:** The 1000 Genomes dataset includes samples from five super-populations (African, American, East Asian, European, South Asian), making it ideal for demonstrating population stratification correction. You’ll see clear separation of ancestry groups in PCA plots.

### What is LD Pruning?

Before performing PCA, we need to understand **Linkage Disequilibrium (LD) pruning**:

**What is Linkage Disequilibrium?**

- LD occurs when nearby genetic variants are inherited together more often than expected by chance

- Variants close together on a chromosome tend to be correlated because they’re rarely separated by recombination

- This creates redundant information—many nearby variants carry essentially the same genetic signal

**Why Prune Before PCA?**

- PCA should capture broad population structure (ancestry), not fine-scale LD patterns

- Without pruning, regions with high LD would dominate PCA results

- A single gene with 1,000 correlated variants would have more influence than 1,000 independent variants spread across the genome

- LD pruning ensures each genomic region contributes proportionally to ancestry inference

**How LD Pruning Works:**

- Examines variants in sliding windows across the genome

- Calculates correlation (r²) between variant pairs

- Removes one variant from highly correlated pairs

- Result: A subset of independent variants ideal for population structure analysis

Think of it like sampling: You don’t need to measure temperature every millimeter to understand a weather pattern—measurements every few kilometers give you the same information with less redundancy.

### Performing PCA for Ancestry Analysis

```bash
cd ~/gwas_tutorial/results

# LD-prune SNPs for PCA to avoid redundancy from correlated variants
echo "Pruning variants in linkage disequilibrium for PCA..."
plink --bfile 1kg_clean_final \
    --indep-pairwise 50 5 0.2 \
    --out pca_pruning

echo "LD-pruned variants for PCA: $(wc -l < pca_pruning.prune.in)"
# LD-pruned variants for PCA: 2116161

# Perform PCA using LD-pruned SNPs
echo "Performing principal component analysis..."
plink --bfile 1kg_clean_final \
    --extract pca_pruning.prune.in \
    --pca 20 \
    --out pca_results

echo "PCA complete! Generated 20 principal components."
```

**Parameter Explanation:**

**`--indep-pairwise 50 5 0.2`:**

- **Window size: 50 SNPs** – Examines 50 variants at a time

- **Step size: 5 SNPs** – Shifts the window by 5 variants for the next calculation

- **r² threshold: 0.2** – Removes one SNP from pairs with correlation > 0.2

- **Result:** Reduces ~11 million variants to ~1-2 million independent SNPs, removing redundant information

**`--pca 20`:**

- Calculates first 20 principal components

- PC1-PC2 typically capture continental ancestry (e.g., African vs. European vs. Asian)

- PC3-PC10 capture finer population structure (e.g., Northern vs. Southern European)

- Higher PCs may capture technical artifacts or rare substructure

### Visualizing Population Structure

Create comprehensive PCA plots to visualize genetic ancestry:

```r
# Create PCA visualization script
cat > ../qc/plot_pca.R << 'EOF'
library(ggplot2)
library(dplyr)

setwd("~/gwas_tutorial/results")

# Read PCA results
pca <- read.table("pca_results.eigenvec", header=FALSE)
colnames(pca) <- c("FID", "IID", paste0("PC", 1:20))

# Read population information from 1000 Genomes
pop_info <- read.table("../data/integrated_call_samples_v3.20130502.ALL.panel",
                       header=TRUE, stringsAsFactors=FALSE)

# Merge PCA with population labels
pca_labeled <- merge(pca, pop_info, by.x="IID", by.y="sample", all.x=TRUE)

# Calculate variance explained by each PC
eigenval <- read.table("pca_results.eigenval")
variance_explained <- eigenval$V1 / sum(eigenval$V1) * 100

# Plot PC1 vs PC2 colored by super-population
png("../plots/pca_plot_pc1_pc2.png", width=1000, height=800)
ggplot(pca_labeled, aes(x=PC1, y=PC2, color=super_pop)) +
    geom_point(alpha=0.6, size=2) +
    labs(title="Principal Component Analysis of 1000 Genomes Samples",
         x=paste0("PC1 (", round(variance_explained[1], 2), "% variance)"),
         y=paste0("PC2 (", round(variance_explained[2], 2), "% variance)"),
         color="Super Population") +
    scale_color_manual(values=c("AFR"="red", "AMR"="orange", "EAS"="green", 
                                 "EUR"="blue", "SAS"="purple"),
                       labels=c("AFR"="African", "AMR"="American", "EAS"="East Asian",
                               "EUR"="European", "SAS"="South Asian")) +
    theme_minimal() +
    theme(legend.position="right",
          plot.title = element_text(hjust = 0.5, size=14, face="bold"))
dev.off()

# Create scree plot showing variance explained
png("../plots/pca_scree.png", width=800, height=600)
scree_data <- data.frame(PC=1:20, Variance=variance_explained[1:20])
ggplot(scree_data, aes(x=PC, y=Variance)) +
    geom_bar(stat="identity", fill="steelblue", alpha=0.7) +
    geom_line(color="red", size=1) +
    geom_point(color="red", size=2) +
    labs(title="Scree Plot: Variance Explained by Principal Components",
         x="Principal Component",
         y="Percentage of Variance Explained") +
    theme_minimal() +
    theme(plot.title = element_text(hjust = 0.5, size=14, face="bold"))
dev.off()

# Plot PC3 vs PC4 for finer structure
png("../plots/pca_plot_pc3_pc4.png", width=1000, height=800)
ggplot(pca_labeled, aes(x=PC3, y=PC4, color=super_pop)) +
    geom_point(alpha=0.6, size=2) +
    labs(title="Fine-Scale Population Structure (PC3 vs PC4)",
         x=paste0("PC3 (", round(variance_explained[3], 2), "% variance)"),
         y=paste0("PC4 (", round(variance_explained[4], 2), "% variance)"),
         color="Super Population") +
    scale_color_manual(values=c("AFR"="red", "AMR"="orange", "EAS"="green", 
                                 "EUR"="blue", "SAS"="purple"),
                       labels=c("AFR"="African", "AMR"="American", "EAS"="East Asian",
                               "EUR"="European", "SAS"="South Asian")) +
    theme_minimal() +
    theme(legend.position="right",
          plot.title = element_text(hjust = 0.5, size=14, face="bold"))
dev.off()

# Print summary statistics
cat("\n=== PCA Summary ===\n")
cat(paste("Total samples:", nrow(pca), "\n"))
cat(paste("Variance explained by PC1:", round(variance_explained[1], 2), "%\n"))
cat(paste("Variance explained by PC2:", round(variance_explained[2], 2), "%\n"))
cat(paste("Cumulative variance (PC1-10):", round(sum(variance_explained[1:10]), 2), "%\n"))

cat("\n=== Population Distribution ===\n")
print(table(pca_labeled$super_pop))

cat("\nPCA plots saved to plots/ directory:\n")
cat("  - pca_plot_pc1_pc2.png\n")
cat("  - pca_plot_pc3_pc4.png\n")
cat("  - pca_scree.png\n")
EOF

# Run the PCA visualization script
Rscript ../qc/plot_pca.R
```

**Note on Visualization:** This visualization step is **optional and educational**—it’s designed to help you understand how PCA captures population structure. Because the 1000 Genomes Project provides ancestry labels for each sample (African, European, Asian, etc.), we can color our PCA plots by known ancestry to visually confirm that principal components successfully separate populations. This demonstrates *why* PCA correction works: the principal components we include as covariates in GWAS are capturing these ancestry differences. **In your own research with real patient data**, you typically won’t have ancestry labels to color by, but you’ll still perform PCA and include the principal components as covariates to control for stratification—even without knowing which specific ancestries are present. The visualization here simply makes the invisible (population structure) visible, helping you build intuition for what PCA is actually detecting in your data.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/pca_plot_pc1_pc2.png?resize=1000%2C800&ssl=1)

### Interpreting PCA Results

**PC1 vs PC2 Plot:**

- Should show clear separation of continental ancestry groups

- African samples typically separate on PC1

- East Asian and European samples separate on PC2

- American samples show admixture between populations

- South Asian samples cluster separately

**Scree Plot:**

- Shows variance explained by each PC

- Look for “elbow” where variance drops sharply

- PCs before the elbow capture meaningful population structure

- PCs after the elbow may represent noise or subtle structure

**How Many PCs to Include as Covariates?**

The number of PCs to include requires balancing two considerations:

**Too Few PCs:** Residual population stratification causes false positives **Too Many PCs:** Over-correction reduces power and may remove true signals

**Guidelines:**

- **Visual Inspection:** Include PCs before the scree plot “elbow”

- **Rule of Thumb:** Typically 5-10 PCs for most studies

- **Multi-Ancestry Studies:** More PCs needed (10-20 PCs) like our 1000 Genomes data

- **Homogeneous Populations:** Fewer PCs sufficient (3-5 PCs)

**For this tutorial with 1000 Genomes’ diverse populations, we’ll use the first 10 PCs as covariates.**

## Creating a Simulated Phenotype for GWAS

Since the 1000 Genomes data lacks phenotype information, we’ll simulate a quantitative trait to demonstrate the association testing workflow. This simulated phenotype will incorporate realistic components: population structure effects, sex effects, and environmental variation.

### Understanding Phenotype Simulation

Our simulated phenotype models a quantitative trait (like height or cholesterol) influenced by:

1. **Population Structure:** Captured by principal components (creates stratification to test our correction)

2. **Sex Effects:** Different trait values between males and females

3. **Random Environmental Variation:** Non-genetic factors affecting the trait

This creates a realistic scenario for demonstrating GWAS analysis and population stratification correction.

### Simulating the Phenotype

```r
# Create phenotype simulation script
cat > simulate_phenotype.R << 'EOF'
library(dplyr)

setwd("~/gwas_tutorial/results")

# Read sample information
fam <- read.table("1kg_clean_final.fam", header=FALSE)
colnames(fam) <- c("FID", "IID", "PID", "MID", "SEX", "PHENOTYPE")

# Read PCA results for covariates
pca <- read.table("pca_results.eigenvec", header=FALSE)
colnames(pca) <- c("FID", "IID", paste0("PC", 1:20))

# Merge FAM file with PCA results
samples <- merge(fam[,1:5], pca[,1:12], by=c("FID", "IID"))

set.seed(12345)  # For reproducibility

cat("Creating simulated quantitative trait...\n")

# Simulate quantitative trait with three components
n_samples <- nrow(samples)

# Component 1: Population structure effect (captured by PCs)
# This creates confounding that PCA correction should remove
polygenic_effect <- 0.3 * samples$PC1 + 0.2 * samples$PC2

# Component 2: Sex effect (males slightly higher than females)
sex_effect <- ifelse(samples$SEX == 1, 0, 0.5)

# Component 3: Random environmental noise
environmental_noise <- rnorm(n_samples, mean=0, sd=1)

# Combine effects to create phenotype
phenotype <- 10 + polygenic_effect + sex_effect + environmental_noise

# Standardize phenotype (mean=0, SD=1)
phenotype <- scale(phenotype)[,1]

# Create phenotype file for PLINK (FID, IID, Phenotype)
pheno_file <- data.frame(
    FID = samples$FID,
    IID = samples$IID,
    Phenotype = phenotype
)

write.table(pheno_file, "simulated_phenotype.txt",
            row.names=FALSE, col.names=FALSE, quote=FALSE)

# Create covariate file with sex and first 10 PCs
covar_file <- data.frame(
    FID = samples$FID,
    IID = samples$IID,
    SEX = samples$SEX,
    samples[, paste0("PC", 1:10)]
)

write.table(covar_file, "covariates.txt",
            row.names=FALSE, col.names=TRUE, quote=FALSE, sep="\t")

# Print summary statistics
cat("\n=== Phenotype Simulation Summary ===\n")
cat(paste("Number of samples:", n_samples, "\n"))
cat(paste("Phenotype mean:", round(mean(phenotype), 3), "\n"))
cat(paste("Phenotype SD:", round(sd(phenotype), 3), "\n"))
cat(paste("Phenotype range:", round(min(phenotype), 2), "to", round(max(phenotype), 2), "\n"))

# Sex distribution
sex_table <- table(samples$SEX)
cat("\nSex distribution:\n")
cat(paste("  Males (1):", sex_table["1"], "\n"))
cat(paste("  Females (2):", sex_table["2"], "\n"))
if("0" %in% names(sex_table)) {
    cat(paste("  Ambiguous (0):", sex_table["0"], "\n"))
}

cat("\nFiles created:\n")
cat("  - simulated_phenotype.txt (FID, IID, Phenotype)\n")
cat("  - covariates.txt (FID, IID, SEX, PC1-PC10)\n")

# Save phenotype distribution plot
png("../plots/phenotype_distribution.png", width=800, height=600)
hist(phenotype, breaks=50, col="steelblue", border="white",
     main="Distribution of Simulated Quantitative Trait",
     xlab="Standardized Phenotype Value", ylab="Frequency")
abline(v=0, col="red", lty=2, lwd=2)
text(0, par("usr")[4]*0.9, "Mean = 0", pos=4, col="red")
dev.off()

cat("\nPhenotype distribution plot saved to plots/phenotype_distribution.png\n")
EOF

# Run phenotype simulation
Rscript simulate_phenotype.R
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/phenotype_distribution.png?resize=800%2C600&ssl=1)

**Expected Output:**

```
=== Phenotype Simulation Summary ===
Number of samples: 2504
Phenotype mean: 0
Phenotype SD: 1
Phenotype range: -3.31 to 3.5

Sex distribution:
  Males (1): 1237
  Females (2): 577
  Ambiguous (0): 690

Files created:
  - simulated_phenotype.txt (FID, IID, Phenotype)
  - covariates.txt (FID, IID, SEX, PC1-PC10)
```

**Understanding the Files:**

**simulated_phenotype.txt format:**

```
HG00096  HG00096  0.234
HG00097  HG00097  -1.456
HG00099  HG00099  0.891
```

- Column 1: Family ID

- Column 2: Individual ID

- Column 3: Phenotype value (standardized to mean=0, SD=1)

**covariates.txt format:**

```
FID      IID      SEX  PC1       PC2       PC3       ...  PC10
HG00096  HG00096  1    0.0234    -0.0145   0.0067    ...  0.0012
HG00097  HG00097  2    0.0241    -0.0139   0.0071    ...  0.0009
```

- First row: Header with covariate names

- Subsequent rows: One per individual with their covariate values

- SEX: 0=ambiguous, 1=male, 2=female

- PC1-PC10: Principal component values

> 

**Real GWAS Note:** In actual GWAS, you would use real phenotype data:

- **Case-Control Studies:** Binary phenotypes (1=control, 2=case, -9=missing)

- **Quantitative Traits:** Continuous measurements (height, BMI, cholesterol levels, gene expression, etc.)

- **Covariates:** Real demographic variables (age, sex, batch, study site) plus principal components

- **Phenotype Files:** Must match sample IDs in your PLINK files exactly

## Statistical Association Testing: The Heart of GWAS

Now we perform genome-wide association testing to identify variants associated with our trait. This is where we test millions of genetic variants to find those that show statistical evidence of association with the phenotype.

### Understanding Association Test Statistics

PLINK implements several association tests depending on trait type:

**Linear Regression (Quantitative Traits):**

- Tests whether variant allele dosage predicts trait value

- Model: Phenotype = β₀ + β₁×Genotype + β₂×Sex + β₃×PC1 + … + ε

- Null hypothesis: β₁ = 0 (no genetic effect)

- Reports beta (effect size), standard error, and p-value

**Logistic Regression (Binary Traits – Case/Control):**

- Tests whether variant allele dosage predicts case-control status

- Reports odds ratio (OR), confidence intervals, and p-value

**Genetic Models:**

- **Additive Model:** Each additional copy of effect allele has same impact (most common, most powerful)

- **Dominant Model:** One or two copies have same effect (rare to use)

- **Recessive Model:** Effect only seen with two copies (rare to use)

For our quantitative trait, we’ll use **linear regression with an additive genetic model**, adjusting for sex and 10 principal components.

### Performing Genome-Wide Association Analysis

```bash
cd ~/gwas_tutorial/results

# Perform genome-wide association testing
echo "Performing genome-wide association analysis..."

plink --bfile 1kg_clean_final \
    --linear \
    --pheno simulated_phenotype.txt \
    --covar covariates.txt \
    --covar-name SEX,PC1,PC2,PC3,PC4,PC5,PC6,PC7,PC8,PC9,PC10 \
    --hide-covar \
    --out gwas_results

echo "GWAS analysis complete!"
```

**Command Explanation:**

**`--linear`:** Perform linear regression for quantitative traits

- Alternative: `--logistic` for case-control (binary) traits

**`--pheno simulated_phenotype.txt`:** Specify phenotype file

- Format: FID IID Phenotype (no header)

- Missing phenotypes coded as -9 or NA

**`--covar covariates.txt`:** Specify covariate file

- Must include header line with covariate names

- Tab or space-delimited

**`--covar-name SEX,PC1,PC2,...,PC10`:** Select which covariates to include

- Explicitly lists covariates for transparency

- Sex corrects for sex-specific effects

- PC1-PC10 correct for population stratification

**`--hide-covar`:** Suppress covariate results in output

- Output file only includes genetic variant associations

- Keeps output file manageable

### Understanding the GWAS Output File

**Output Files:**

- `gwas_results.assoc.linear`: Association results for all tested variants

**Example output (gwas_results.assoc.linear):**

```
CHR  SNP              BP        A1  TEST  NMISS  BETA      STAT     P
1    1:10177_A_AC     10177     A   ADD   2504   0.0123    0.456    0.6483
1    rs367896724      10235     A   ADD   2504   -0.0089   -0.321   0.7482
11   11:89920705_C_T  89920705  C   ADD   1814   -0.2079   -5.643   1.941e-08
11   11:89921232_C_T  89921232  T   ADD   1814   -0.2066   -5.544   3.403e-08
22   22:50876234_G_A  50876234  G   ADD   2504   0.0034    0.124    0.9014
```

**Column Descriptions:**

**CHR:** Chromosome number (1-22, X, Y)

- Indicates which chromosome contains the variant

**SNP:** Variant identifier

- rsID (e.g., rs367896724) or position-based ID (e.g., 11:89920705_C_T)

- Position-based format: chromosome:position_reference_alternate

**BP:** Base-pair position

- Physical position on the chromosome

**A1:** Effect allele (tested allele)

- The allele for which the effect (BETA) is calculated

- Usually the minor (less common) allele

**TEST:** Type of association test performed

- **ADD:** Additive model (standard for GWAS)

- Other options: DOM (dominant), REC (recessive)

**NMISS:** Number of non-missing samples

- Number of individuals with both genotype and phenotype data for this test

- Varies by variant if some samples have missing genotypes

**BETA:** Effect size

- **Interpretation:** Change in phenotype per copy of the effect allele (A1)

- **Positive BETA:** Each copy of A1 increases the trait value

- **Negative BETA:** Each copy of A1 decreases the trait value

- **Example:** BETA = -0.2079 means each copy of the C allele at 11:89920705 decreases the phenotype by 0.21 standard deviations

**STAT:** Test statistic (t-statistic for linear regression)

- Larger absolute values indicate stronger evidence

- STAT = BETA / SE (standard error)

- Used to calculate the p-value

**P:** P-value

- Probability of observing this association by chance

- **Lower p-values** = stronger evidence of association

- **Genome-wide significance threshold:** p < 5×10⁻⁸

**Real-World Example Interpretation:**

```
11   11:89920705_C_T  89920705  C   ADD   1814   -0.2079   -5.643   1.941e-08
```

This variant shows:

- Located on chromosome 11 at position 89,920,705

- Effect allele is C (reference allele)

- Tested in 1,814 individuals

- Each copy of C allele decreases phenotype by 0.21 SD

- Test statistic = -5.643 (negative indicates decreased phenotype)

- P-value = 1.941×10⁻⁸ (genome-wide significant!)

### Understanding GWAS Significance Thresholds

GWAS tests millions of variants, requiring stringent significance thresholds to control for multiple testing:

**Genome-Wide Significance: p < 5×10⁻⁸**

- Bonferroni correction for ~1 million independent tests across the genome

- Standard threshold for declaring significant associations

- Conservative but protects against false positives

- Required for publication of novel associations

**Suggestive Significance: p < 1×10⁻⁵**

- Indicates potential association worth investigating

- Not definitive but warrants follow-up

- Used to identify loci for replication studies

- May represent true associations below genome-wide significance threshold

**Why 5×10⁻⁸?** The human genome contains roughly 1 million independent common variants (accounting for linkage disequilibrium). Using Bonferroni correction: 0.05 / 1,000,000 = 5×10⁻⁸.

### Extracting Significant Results

```bash
# Extract genome-wide significant hits (p < 5e-8)
echo "Extracting significant associations..."
awk 'NR==1 || ($9 != "NA" && $9 < 5e-8)' gwas_results.assoc.linear > significant_snps.txt

NSIG=$(awk 'NR>1' significant_snps.txt | wc -l)
echo "Genome-wide significant associations (p < 5e-8): ${NSIG}"

# Extract suggestive hits (p < 1e-5)
awk 'NR==1 || ($9 != "NA" && $9 < 1e-5)' gwas_results.assoc.linear > suggestive_snps.txt

NSUGG=$(awk 'NR>1' suggestive_snps.txt | wc -l)
echo "Suggestive associations (p < 1e-5): ${NSUGG}"

# Identify top 20 associations
echo "Extracting top 20 associations..."
(head -1 gwas_results.assoc.linear; \
 tail -n +2 gwas_results.assoc.linear | \
 awk '$9 != "NA"' | \
 sort -k9,9g | \
 head -20) > top_associations.txt

echo ""
echo "Top 20 associations:"
cat top_associations.txt
```

**Example output:**

```
Genome-wide significant associations (p &lt; 5e-8): 2
Suggestive associations (p &lt; 1e-5): 247

Top 20 associations:
CHR  SNP              BP        A1  TEST  NMISS  BETA      STAT     P
11   11:89920705_C_T  89920705  C   ADD   1814   -0.2079   -5.643   1.941e-08
11   11:89921232_C_T  89921232  T   ADD   1814   -0.2066   -5.544   3.403e-08
```

### Understanding Results for Simulated Data

In our tutorial, we found **2 genome-wide significant associations** on chromosome 11:

**Variant 1: 11:89920705_C_T**

- P-value: 1.941×10⁻⁸ (genome-wide significant)

- Effect: -0.2079 per C allele

- Interpretation: Each copy of the C allele decreases phenotype by ~0.21 SD

**Variant 2: 11:89921232_C_T**

- P-value: 3.403×10⁻⁸ (genome-wide significant)

- Effect: -0.2066 per T allele

- Similar effect size and significance to Variant 1

**Why Did We Find Significant Associations in Simulated Data?**

This might seem surprising since we simulated a phenotype influenced only by population structure, sex, and random noise—we didn’t explicitly program any genetic variants to have real effects. However, finding a few significant hits by chance is actually expected:

**Statistical Explanation:**

- We tested ~11 million variants at a threshold of p < 5×10⁻⁸

- Expected false positives: 11,000,000 × 5×10⁻⁸ = 0.55 variants

- Finding 2 significant variants is within the expected range of random chance

**Why These Two Variants?** The two significant variants are at adjacent positions (527 bp apart: 89920705 and 89921232), suggesting they’re in **linkage disequilibrium**—they’re inherited together. This is a single association signal, not two independent findings. This is common in GWAS: a cluster of significant variants in LD often represents one causal variant, with nearby variants showing association due to correlation.

**Important Lessons:**

1. **Replication is Essential:** These hits would likely not replicate in an independent dataset, demonstrating why replication studies are crucial

2. **Expect Some False Positives:** Even with stringent thresholds, some false discoveries occur

3. **Effect Size Consistency:** Real associations typically show consistent effects across nearby variants in LD

4. **Population Stratification:** Our PCA correction worked—genomic inflation factor (λ) should be ~1.0, indicating proper control

**In Real GWAS:**

- True genetic associations would replicate in independent cohorts

- Effect sizes would be consistent between discovery and replication

- Multiple independent studies would identify the same loci

- Biological validation would confirm the mechanism

This exercise demonstrates that GWAS requires careful interpretation, replication, and biological validation—statistical significance alone is insufficient evidence for true association.

## Visualizing GWAS Results: Manhattan and QQ Plots

Visualization is essential for interpreting GWAS results, identifying potential issues, and communicating findings. The two key plots are Manhattan plots and QQ plots.

### Creating Publication-Quality Visualizations

```r
# Create comprehensive GWAS visualization script
cat > ../plots/plot_gwas_results.R << 'EOF'
library(ggplot2)
library(dplyr)

setwd("~/gwas_tutorial/results")

cat("Reading GWAS results...\n")

# Read GWAS results
gwas <- read.table("gwas_results.assoc.linear", header=TRUE, stringsAsFactors=FALSE)

# Filter to additive test only and remove invalid p-values
gwas <- gwas %>%
    filter(!is.na(P), P > 0, P <= 1, TEST == "ADD") %>%
    mutate(
        log10p = -log10(P),
        is_sig = P < 5e-8,
        is_suggestive = P < 1e-5 & P >= 5e-8
    )

cat(paste("Total variants with valid p-values:", nrow(gwas), "\n"))
cat(paste("Genome-wide significant (p < 5e-8):", sum(gwas$is_sig), "\n"))
cat(paste("Suggestive (p < 1e-5):", sum(gwas$is_suggestive), "\n"))

#================================================================
# Manhattan Plot
#================================================================
cat("\nCreating Manhattan plot...\n")

# Calculate cumulative position for plotting
gwas <- gwas %>%
    arrange(CHR, BP) %>%
    group_by(CHR) %>%
    mutate(chr_len = max(BP)) %>%
    ungroup()

# Calculate chromosome start positions
chr_info <- gwas %>%
    group_by(CHR) %>%
    summarise(chr_len = max(BP)) %>%
    mutate(chr_start = cumsum(lag(chr_len, default=0))) %>%
    select(CHR, chr_start)

gwas <- gwas %>%
    left_join(chr_info, by="CHR") %>%
    mutate(pos_cum = BP + chr_start)

# Calculate chromosome centers for x-axis labels
axis_set <- gwas %>%
    group_by(CHR) %>%
    summarise(center = mean(pos_cum))

# Define colors for alternating chromosomes
chr_colors <- rep(c("steelblue", "darkblue"), length.out=23)

# Create Manhattan plot
png("../plots/manhattan_plot.png", width=1600, height=700, res=100)
ggplot(gwas, aes(x=pos_cum, y=log10p, color=as.factor(CHR))) +
    geom_point(alpha=0.6, size=0.8) +
    geom_hline(yintercept=-log10(5e-8), color="red", linetype="dashed", size=1) +
    geom_hline(yintercept=-log10(1e-5), color="blue", linetype="dashed", size=0.8) +
    scale_color_manual(values=chr_colors) +
    scale_x_continuous(
        breaks=axis_set$center,
        labels=axis_set$CHR,
        expand=c(0.01, 0.01)
    ) +
    scale_y_continuous(expand=c(0.01, 0.01), limits=c(0, max(gwas$log10p)*1.1)) +
    labs(title="Manhattan Plot - Genome-Wide Association Study Results",
         x="Chromosome",
         y="-log₁₀(P-value)",
         caption="Red line: genome-wide significance (p < 5×10⁻⁸)\nBlue line: suggestive (p < 1×10⁻⁵)") +
    theme_minimal() +
    theme(
        legend.position="none",
        panel.grid.major.x = element_blank(),
        panel.grid.minor.x = element_blank(),
        axis.text.x = element_text(size=10),
        plot.title = element_text(hjust = 0.5, size=14, face="bold"),
        axis.title = element_text(size=12)
    )
dev.off()

#================================================================
# QQ Plot
#================================================================
cat("Creating Q-Q plot...\n")

# Calculate expected -log10(p) under null hypothesis
gwas_sorted <- gwas %>%
    arrange(P) %>%
    mutate(
        expected = -log10(ppoints(n())),
        observed = -log10(P)
    )

# Calculate genomic inflation factor (lambda)
chisq <- qchisq(1 - gwas$P, 1)
lambda <- median(chisq, na.rm=TRUE) / qchisq(0.5, 1)

png("../plots/qq_plot.png", width=800, height=800, res=100)
ggplot(gwas_sorted, aes(x=expected, y=observed)) +
    geom_point(alpha=0.5, color="steelblue", size=1.5) +
    geom_abline(intercept=0, slope=1, color="red", linetype="dashed", size=1) +
    labs(title=paste0("Q-Q Plot (λ = ", round(lambda, 3), ")"),
         x="Expected -log₁₀(P-value)",
         y="Observed -log₁₀(P-value)",
         caption="Red line: expected distribution under null hypothesis\nλ (genomic inflation factor) should be ≈1.0") +
    theme_minimal() +
    theme(plot.title = element_text(hjust = 0.5, size=14, face="bold"),
          axis.title = element_text(size=12))
dev.off()

#================================================================
# Summary Statistics
#================================================================
cat("\n=== GWAS Summary Statistics ===\n")
cat(paste("Total variants tested:", nrow(gwas), "\n"))
cat(paste("Genome-wide significant (p < 5e-8):", sum(gwas$is_sig), "\n"))
cat(paste("Suggestive (p < 1e-5):", sum(gwas$is_suggestive), "\n"))
cat(paste("Genomic inflation factor (λ):", round(lambda, 3), "\n"))
cat(paste("Median p-value:", format(median(gwas$P), scientific=TRUE), "\n"))
cat(paste("Minimum p-value:", format(min(gwas$P), scientific=TRUE), "\n"))

# Interpretation of lambda
cat("\nGenomic Inflation Interpretation:\n")
if(lambda < 1.0) {
    cat("  λ < 1.0: Possible over-correction (conservative)\n")
} else if(lambda >= 1.0 && lambda <= 1.05) {
    cat("  λ = 1.0-1.05: Excellent - minimal inflation\n")
} else if(lambda > 1.05 && lambda <= 1.10) {
    cat("  λ = 1.05-1.10: Acceptable - slight inflation\n")
} else {
    cat("  λ > 1.10: Problematic - suggests residual stratification\n")
    cat("  Consider adding more PCs or checking for batch effects\n")
}

# Top associations
if(nrow(gwas) > 0) {
    cat("\n=== Top 10 Associations ===\n")
    top_hits <- gwas %>%
        arrange(P) %>%
        select(SNP, CHR, BP, A1, BETA, P) %>%
        head(10)
    print(top_hits)
}

cat("\n✓ Visualization complete!\n")
cat("Plots saved to plots/ directory:\n")
cat("  - manhattan_plot.png\n")
cat("  - qq_plot.png\n")
EOF

# Run visualization script
Rscript ../plots/plot_gwas_results.R
```

**Example output:**

```bash
=== GWAS Summary Statistics ===
Total variants tested: 10537718
Genome-wide significant (p &lt; 5e-8): 2
Suggestive (p &lt; 1e-5): 138
Genomic inflation factor (λ): 1.001
Median p-value: 4.997e-01
Minimum p-value: 1.941e-08

Genomic Inflation Interpretation:
λ = 1.0-1.05: Excellent - minimal inflation

=== Top 10 Associations ===
# A tibble: 10 × 6
SNP CHR BP A1 BETA P
1 11:89920705_C_T 11 89920705 C -0.208 0.0000000194
2 11:89921232_C_T 11 89921232 T -0.207 0.0000000340
3 15:82677008_C_G 15 82677008 C -0.620 0.000000669
4 15:82484952_A_C 15 82484952 C -0.658 0.000000724
5 15:82485001_A_T 15 82485001 A -0.658 0.000000724
6 8:130925116_C_T 8 130925116 C 0.169 0.00000101
7 8:120437457_C_CA 8 120437457 C -0.176 0.00000102
8 5:148726630_G_GT 5 148726630 G -0.644 0.00000110
9 5:148728798_A_G 5 148728798 G -0.644 0.00000110
10 5:148729662_A_G 5 148729662 A -0.644 0.00000110
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/gwas_manhattan_qq-e1763584477787-1024x338.png?resize=1024%2C338&ssl=1)

### Interpreting Manhattan and QQ Plots

**Manhattan Plot Interpretation:**

The Manhattan plot displays -log₁₀(p-values) across all chromosomes, resembling the Manhattan skyline when significant associations are present.

**Key Features to Look For:**

- **Height:** Taller peaks indicate stronger associations (lower p-values)

- **Genome-Wide Significance Line (red):** Variants above this line (p < 5×10⁻⁸) are significant

- **Clustering:** Multiple significant variants in a region suggest true association signal rather than isolated false positive

- **Isolated Spikes:** Single significant variants without nearby support may be false positives

- **Chromosome Distribution:** True polygenic traits show signals across multiple chromosomes

**In Our Tutorial Results:**

- You should see a small peak on chromosome 11 (our two significant variants)

- Most of the genome shows baseline low signals

- The single peak demonstrates that most associations in simulated data are noise

**QQ Plot Interpretation:**

The QQ (quantile-quantile) plot compares observed p-value distribution to expected distribution under the null hypothesis (no true associations).

**Key Features to Look For:**

- **Diagonal Line:** Expected distribution if all null hypotheses are true

- **Points on Line:** Results match null expectation (no associations)

- **Deviation at Right Tail:** True association signals (expected in GWAS with real effects)

- **Systematic Deviation Throughout:** Suggests systematic problems like population stratification or batch effects

**Genomic Inflation Factor (λ):**

Lambda quantifies overall inflation of test statistics:

- **λ ≈ 1.0:** Ideal—no systematic inflation, proper control of false positives

- **λ = 1.0-1.05:** Excellent—minimal inflation, acceptable for publication

- **λ = 1.05-1.10:** Acceptable—slight inflation, consider sensitivity analyses

- **λ > 1.10:** Problematic—suggests residual population stratification, relatedness, or technical artifacts

**Common Causes of High λ:**

1. Inadequate population stratification correction (add more PCs)

2. Cryptic relatedness not removed

3. Phenotype correlated with batch effects

4. Genotyping quality issues

**λ < 1.0:** May indicate over-correction (too many PCs removed true signals)

**In Our Tutorial:**

- λ should be close to 1.0, demonstrating proper population stratification control

- The QQ plot should follow the diagonal line closely

- This confirms our PCA correction effectively removed confounding

## Replication and Meta-Analysis: Validating GWAS Findings

Single-study GWAS results require independent validation to establish confidence in discoveries. Replication in independent cohorts is a cornerstone of rigorous genetic research.

### Understanding Replication Studies

**Why Replication Matters:**

1. **False Positive Control:** Independent validation dramatically reduces false discovery rate

2. **Effect Size Precision:** Replication provides better estimates of true effect sizes (discovery studies often overestimate due to “winner’s curse”)

3. **Generalizability:** Tests whether findings apply to different populations, environments, or age groups

4. **Publication Standards:** Most high-impact journals require replication for novel genetic associations

**Replication Study Design:**

**Essential Requirements:**

- **Independent Cohort:** Different individuals from discovery sample (no sample overlap)

- **Similar Phenotype:** Matching trait definition and measurement methods

- **Adequate Power:** Sufficient sample size to detect the discovery effect size at α = 0.05

- **Same Ancestry (Usually):** Replication typically in same population, though trans-ethnic replication strengthens findings

**Key Considerations:**

- **Direction of Effect:** Consistent beta sign (or OR > 1 vs < 1) across studies is crucial

- **Effect Size Magnitude:** Should be similar between discovery and replication

- **P-value Threshold:** Nominal significance (p < 0.05) often sufficient for replication of known loci

- **One-Tailed vs Two-Tailed:** Can use one-tailed test in replication if direction is pre-specified

### Meta-Analysis Overview

Meta-analysis combines results from multiple studies to increase statistical power and precision. This is particularly valuable when individual studies are underpowered.

**Meta-Analysis Approaches:**

**Fixed-Effects Meta-Analysis:**

- **Assumption:** All studies estimate the same true effect

- **Method:** Inverse-variance weighted average of effect estimates

- **Best For:** Studies from similar populations and conditions

- **Formula:** β_meta = Σ(w_i × β_i) / Σ(w_i), where w_i = 1/SE_i²

**Random-Effects Meta-Analysis:**

- **Assumption:** True effects vary between studies (heterogeneity)

- **Method:** Accounts for between-study variance

- **Best For:** Diverse populations or heterogeneous results

- **Tests:** Cochran’s Q test and I² statistic quantify heterogeneity

**Key Meta-Analysis Statistics:**

**Heterogeneity Measures:**

- **I² statistic:** Percentage of variation due to heterogeneity rather than chance

  - I² < 25%: Low heterogeneity

  - I² = 25-75%: Moderate heterogeneity

  - I² > 75%: High heterogeneity

- **Cochran’s Q:** Chi-square test for heterogeneity (p < 0.05 suggests heterogeneity)

**When to Use Each Approach:**

- **Low heterogeneity (I² < 25%):** Use fixed-effects

- **High heterogeneity (I² > 50%):** Use random-effects or investigate sources of heterogeneity

- **Very high heterogeneity (I² > 75%):** Consider whether meta-analysis is appropriate; may need stratified analysis

**Preparing Data for Meta-Analysis:**

GWAS summary statistics can be shared and combined with other studies using tools like METAL. The key information needed includes:

- Variant identifier (rsID or position)

- Effect allele and reference allele

- Effect size (beta or odds ratio)

- Standard error

- P-value

- Sample size

Meta-analysis dramatically increases power to detect true associations and provides more accurate effect size estimates by combining evidence across multiple independent studies.

## Best Practices for GWAS Analysis

Adhering to established best practices ensures reproducible, interpretable, and publishable GWAS results.

### Study Design Considerations

**Sample Size Planning:**

**Power Calculations:**

- Use tools like QUANTO or Genetic Power Calculator

- Typical GWAS effect sizes: OR = 1.1-1.3 for common variants in common diseases

- Small effect sizes require large samples for adequate power

**Minimum Recommendations:**

- **Quantitative traits:** N > 5,000 for moderate power to detect common variant effects

- **Case-control studies:** N > 2,000 cases and 2,000 controls as starting point

- **Rare diseases:** Smaller samples acceptable but expect limited power for common variants

- **Biobank-scale:** N > 100,000 enables detection of variants with very small effects

**Effect Size Expectations:**

- Common variants: OR = 1.05-1.3 for disease traits; β = 0.01-0.1 SD for quantitative traits

- Larger effects typically found for: Mendelian disease variants, pharmacogenomic traits, extreme phenotypes

**Phenotype Definition:**

**Precision Matters:**

- Well-defined, objectively measured phenotypes increase power

- Measurement error dilutes effect sizes and reduces power

- Standardized protocols across sites minimize heterogeneity

**Covariate Selection:**

- **Always include:** Age, sex (when appropriate), genetic ancestry PCs

- **Consider including:** Batch effects, study site, assessment center, season

- **Avoid over-adjustment:** Don’t adjust for variables on causal pathway between genotype and phenotype

**Missing Data Handling:**

- **Multiple imputation** better than complete case analysis

- Understand missingness patterns (MCAR, MAR, MNAR)

- Report missingness rates and sensitivity analyses

**Outlier Management:**

- Define outliers based on biological plausibility (e.g., ±5 SD from mean)

- Document outlier removal criteria

- Consider robust regression methods for heavy-tailed phenotypes

### Quality Control Standards

**Sample-Level QC:**

- **Genotyping call rate:** > 95% (remove samples with > 5% missing)

- **Sex check concordance:** Verify reported vs. genetic sex

- **Heterozygosity:** Remove samples with extreme heterozygosity (±3 SD from mean)

- **Relatedness:** Depends on analysis approach

  - Standard methods: Remove related individuals (PI_HAT > 0.185)

  - Mixed models (BOLT-LMM, GCTA): Can include related individuals

- **Ancestry outliers:** Identify using PCA, handle appropriately for study design

**Variant-Level QC:**

- **Genotyping call rate:** > 95% (remove variants with > 5% missing)

- **MAF threshold:** Typically > 0.01 for common variant GWAS

- **Hardy-Weinberg equilibrium:** p > 1×10⁻⁶ in controls or combined sample

- **Differential missingness:** Test for case-control differences in missingness

**Population Stratification:**

- **Genomic inflation:** λ < 1.05 ideal (< 1.10 acceptable)

- **QQ plot inspection:** Systematic deviation indicates issues

- **PC count:** Include sufficient PCs (visual inspection of scree plot + genomic inflation)

### Statistical Analysis Guidelines

**Association Testing:**

**Multiple Testing Correction:**

- **Genome-wide significance:** p < 5×10⁻⁸ for discovery

- **Replication threshold:** p < 0.05 (one-tailed acceptable if direction pre-specified)

- **Don’t adjust p-values post-hoc** (e.g., Bonferroni on suggestive hits)

**Model Selection:**

- **Additive model:** Standard, most powerful in most scenarios

- **Dominant/Recessive:** Only if biological mechanism suggests

- **Test reported:** Always clearly state genetic model used

**Covariate Adjustment:**

- **Minimum:** Sex (if applicable) + ancestry PCs

- **Additional:** Age, batch, study-specific covariates

- **Document:** Report all covariates in methods

**Software Consistency:**

- Use same QC and analysis pipeline for discovery and replication

- Document all parameters and software versions

- Provide analysis scripts for reproducibility

**Result Interpretation:**

**Effect Sizes:**

- **Always report:** Effect size (beta or OR) with 95% confidence intervals

- **Direction:** Clearly specify effect/risk allele

- **Magnitude:** Interpret in context of trait distribution

**Replication Requirements:**

- **Independent sample:** No overlap with discovery cohort

- **Consistent direction:** Beta sign (or OR < 1 vs > 1) must match

- **Nominal significance:** p < 0.05 typically sufficient

- **Meta-analysis:** Combine discovery + replication for final effect estimate

**Functional Follow-up:**

- Prioritize variants for experimental validation

- Integration with multi-omics data (eQTL, pQTL, methylation)

- Pathway and network analyses

**Clinical Utility Assessment:**

- Evaluate predictive value (AUC, R²)

- Consider whether findings inform prevention or treatment

- Assess whether effect sizes are clinically meaningful

## Common Pitfalls and Troubleshooting

### Frequent Issues and Solutions

**Problem 1: High Genomic Inflation (λ > 1.05)**

**Symptoms:**

- QQ plot shows systematic deviation from expected diagonal

- λ > 1.05-1.10

- Many variants reach significance across genome

**Possible Causes:**

1. Inadequate population stratification correction

2. Related individuals not properly handled

3. Phenotype confounded with technical factors (batch, study site)

4. Poor quality control (low-quality variants or samples)

**Solutions:**

```bash
# Solution 1: Add more principal components
plink --bfile 1kg_clean_final \
    --linear \
    --pheno phenotype.txt \
    --covar covariates.txt \
    --covar-name SEX,PC1-PC20 \  # Increase from 10 to 20 PCs
    --hide-covar \
    --out gwas_results_20pcs

# Solution 2: Use stricter relatedness cutoff
plink --bfile data \
    --rel-cutoff 0.125 \  # More stringent than 0.185
    --out related_remove_strict

# Solution 3: Apply more stringent variant QC
plink --bfile data \
    --geno 0.02 \     # Remove variants with > 2% missing (vs 5%)
    --maf 0.05 \      # Increase MAF threshold to 5% (vs 1%)
    --hwe 1e-10 \     # More stringent HWE threshold
    --make-bed \
    --out data_strict_qc
```

**Problem 2: No Genome-Wide Significant Hits**

**Symptoms:**

- Manhattan plot shows no peaks above genome-wide significance line

- Largest -log10(p) < 7.3 (p > 5×10⁻⁸)

**Possible Causes:**

1. Insufficient statistical power (small sample size)

2. Trait not highly heritable

3. Poor phenotype definition or measurement error

4. Rare variant genetic architecture (common variant GWAS underpowered)

5. Over-correction for population structure

**Solutions:**

- Estimate SNP-heritability to assess genetic component (use GCTA)

- Consider gene-based tests (MAGMA or VEGAS)

- Pursue replication and meta-analysis to increase power

- Refine phenotype measurement to reduce error

**Problem 3: Too Many Significant Associations**

**Symptoms:**

- Hundreds or thousands of genome-wide significant variants

- Significant associations spread uniformly across genome

- λ substantially > 1.0

**Possible Causes:**

1. Severe population stratification

2. Batch effects correlated with phenotype

3. Sample contamination or identity errors

4. Phenotype strongly correlated with genetic ancestry

**Solutions:**

- Re-examine PCA and add more PCs

- Stratify analysis by population

- Check for batch effects

- Verify sample identity

**Problem 4: Results Don’t Replicate**

**Symptoms:**

- Discovery hits not significant in replication cohort

- Effect directions inconsistent between studies

- Meta-analysis shows heterogeneity

**Possible Causes:**

1. Discovery finding was false positive (winner’s curse)

2. Different ancestry in replication cohort

3. Different phenotype definitions

4. Insufficient replication sample size

5. True population-specific effects

**Solutions:**

- Calculate required replication sample size

- Ensure phenotype harmonization

- Test for heterogeneity in meta-analysis

- Consider trans-ethnic fine-mapping

- Look at effect direction consistency

### Quality Control Red Flags

**Data Quality Warning Signs:**

- **Sample missingness:** Mean > 5% or max > 10%

- **Variant missingness:** Many variants with > 10% missing

- **HWE violations:** > 1% of variants with p < 1×10⁻⁶

- **Sex check failures:** > 5% of samples

**Analysis Quality Warning Signs:**

- **Genomic inflation:** λ > 1.10

- **QQ plot:** Systematic deviation across all p-values (not just tail)

- **PC-phenotype correlation:** PC1 strongly associated with phenotype (p < 0.001)

- **Uniform Manhattan:** Elevated signals across entire genome

**Result Interpretation Warning Signs:**

- All significant SNPs in single region (possible artifact)

- Top hits clustered near telomeres/centromeres (poor mappability)

- Significant associations only in low-frequency variants

- Effect sizes inconsistent with literature

**When You See Red Flags:**

1. Stop and investigate before proceeding

2. Review QC steps systematically

3. Check for technical artifacts

4. Consult with experienced GWAS analysts

5. Consider sensitivity analyses

## Conclusion: From Analysis to Discovery

Congratulations! You’ve completed a comprehensive genome-wide association study workflow, from downloading and preprocessing WGS data through quality control, population stratification correction, association testing, to visualization and interpretation.

### Next Steps in Your GWAS Journey

**Immediate Actions:**

1. Practice with your own data using the conversion pipeline shown in Option 1

2. Explore functional annotation of significant variants

3. Learn about polygenic risk scores for clinical applications

4. Study gene-based and pathway analyses for deeper insights

**Further Learning:**

- **Advanced Methods:** BOLT-LMM and REGENIE for biobank-scale data

- **Fine-Mapping:** Tools like FINEMAP and SuSiE to identify causal variants

- **Multi-Omics Integration:** Combine GWAS with eQTL, pQTL, and chromatin data

- **Trans-Ethnic Analysis:** Leverage diverse populations for discovery and validation

## References

### Foundational GWAS Papers

1. **Visscher PM, Wray NR, Zhang Q, et al.** 10 Years of GWAS Discovery: Biology, Function, and Translation. *Am J Hum Genet*. 2017;101(1):5-22. doi:10.1016/j.ajhg.2017.06.005

2. **Tam V, Patel N, Turcotte M, Bossé Y, Paré G, Meyre D.** Benefits and limitations of genome-wide association studies. *Nat Rev Genet*. 2019;20(8):467-484. doi:10.1038/s41576-019-0127-1

3. **Bush WS, Moore JH.** Chapter 11: Genome-wide association studies. *PLoS Comput Biol*. 2012;8(12):e1002822. doi:10.1371/journal.pcbi.1002822

4. **Uffelmann E, Huang QQ, Munung NS, et al.** Genome-wide association studies. *Nat Rev Methods Primers*. 2021;1:59. doi:10.1038/s43586-021-00056-9

### PLINK Software

1. **Purcell S, Neale B, Todd-Brown K, et al.** PLINK: a tool set for whole-genome association and population-based linkage analyses. *Am J Hum Genet*. 2007;81(3):559-575. doi:10.1086/519795

2. **Chang CC, Chow CC, Tellier LC, Vattikuti S, Purcell SM, Lee JJ.** Second-generation PLINK: rising to the challenge of larger and richer datasets. *Gigascience*. 2015;4:7. doi:10.1186/s13742-015-0047-8

### 1000 Genomes Project

1. **1000 Genomes Project Consortium, Auton A, Brooks LD, et al.** A global reference for human genetic variation. *Nature*. 2015;526(7571):68-74. doi:10.1038/nature15393

2. **Byrska-Bishop M, Evani US, Zhao X, et al.** High-coverage whole-genome sequencing of the expanded 1000 Genomes Project cohort including 602 trios. *Cell*. 2022;185(18):3426-3440.e19. doi:10.1016/j.cell.2022.08.004

### Population Stratification and Principal Component Analysis

1. **Price AL, Patterson NJ, Plenge RM, Weinblatt ME, Shadick NA, Reich D.** Principal components analysis corrects for stratification in genome-wide association studies. *Nat Genet*. 2006;38(8):904-909. doi:10.1038/ng1847

2. **Patterson N, Price AL, Reich D.** Population structure and eigenanalysis. *PLoS Genet*. 2006;2(12):e190. doi:10.1371/journal.pgen.0020190

3. **Abdellaoui A, Hottenga JJ, de Knijff P, et al.** Population structure, migration, and diversifying selection in the Netherlands. *Eur J Hum Genet*. 2013;21(11):1277-1285. doi:10.1038/ejhg.2013.48

### Quality Control Guidelines

1. **Anderson CA, Pettersson FH, Clarke GM, Cardon LR, Morris AP, Zondervan KT.** Data quality control in genetic case-control association studies. *Nat Protoc*. 2010;5(9):1564-1573. doi:10.1038/nprot.2010.116

2. **Laurie CC, Doheny KF, Mirel DB, et al.** Quality control and quality assurance in genotypic data for genome-wide association studies. *Genet Epidemiol*. 2010;34(6):591-602. doi:10.1002/gepi.20516

3. **Marees AT, de Kluiver H, Stringer S, et al.** A tutorial on conducting genome-wide association studies: Quality control and statistical analysis. *Int J Methods Psychiatr Res*. 2018;27(2):e1608. doi:10.1002/mpr.1608

### Hardy-Weinberg Equilibrium

1. **Wigginton JE, Cutler DJ, Abecasis GR.** A note on exact tests of Hardy-Weinberg equilibrium. *Am J Hum Genet*. 2005;76(5):887-893. doi:10.1086/429864

2. **Graffelman J, Moreno V.** The mid p-value in exact tests for Hardy-Weinberg equilibrium. *Stat Appl Genet Mol Biol*. 2013;12(4):433-448. doi:10.1515/sagmb-2012-0039

### Statistical Methods and Multiple Testing

1. **Dudbridge F, Gusnanto A.** Estimation of significance thresholds for genomewide association scans. *Genet Epidemiol*. 2008;32(3):227-234. doi:10.1002/gepi.20297

2. **Pe’er I, Yelensky R, Altshuler D, Daly MJ.** Estimation of the multiple testing burden for genomewide association studies of nearly all common variants. *Genet Epidemiol*. 2008;32(4):381-385. doi:10.1002/gepi.20303

3. **Benjamini Y, Hochberg Y.** Controlling the false discovery rate: a practical and powerful approach to multiple testing. *J R Stat Soc Series B Stat Methodol*. 1995;57(1):289-300.

### Genomic Inflation and Lambda

1. **Devlin B, Roeder K.** Genomic control for association studies. *Biometrics*. 1999;55(4):997-1004. doi:10.1111/j.0006-341x.1999.00997.x

2. **Yang J, Weedon MN, Purcell S, et al.** Genomic inflation factors under polygenic inheritance. *Eur J Hum Genet*. 2011;19(7):807-812. doi:10.1038/ejhg.2011.39

### Linkage Disequilibrium

1. **Slatkin M.** Linkage disequilibrium–understanding the evolutionary past and mapping the medical future. *Nat Rev Genet*. 2008;9(6):477-485. doi:10.1038/nrg2361

2. **Pritchard JK, Przeworski M.** Linkage disequilibrium in humans: models and data. *Am J Hum Genet*. 2001;69(1):1-14. doi:10.1086/321275

### Meta-Analysis

1. **Willer CJ, Li Y, Abecasis GR.** METAL: fast and efficient meta-analysis of genomewide association scans. *Bioinformatics*. 2010;26(17):2190-2191. doi:10.1093/bioinformatics/btq340

2. **Evangelou E, Ioannidis JP.** Meta-analysis methods for genome-wide association studies and beyond. *Nat Rev Genet*. 2013;14(6):379-389. doi:10.1038/nrg3472

### Replication and Validation

1. **Kraft P, Zeggini E, Ioannidis JP.** Replication in genome-wide association studies. *Stat Sci*. 2009;24(4):561-573. doi:10.1214/09-STS290

2. **Siontis KC, Patsopoulos NA, Ioannidis JP.** Replication of past candidate loci for common diseases and phenotypes in 100 genome-wide association studies. *Eur J Hum Genet*. 2010;18(7):762-767. doi:10.1038/ejhg.2010.26

### Visualization

1. **Turner SD.** qqman: an R package for visualizing GWAS results using Q-Q and manhattan plots. *J Open Source Softw*. 2018;3(25):731. doi:10.21105/joss.00731

### VCF Processing Tools

1. **Danecek P, Bonfield JK, Liddle J, et al.** Twelve years of SAMtools and BCFtools. *Gigascience*. 2021;10(2):giab008. doi:10.1093/gigascience/giab008

2. **Li H.** A statistical framework for SNP calling, mutation discovery, association mapping and population genetical parameter estimation from sequencing data. *Bioinformatics*. 2011;27(21):2987-2993. doi:10.1093/bioinformatics/btr509

### GWAS Catalog and Databases

1. **Buniello A, MacArthur JAL, Cerezo M, et al.** The NHGRI-EBI GWAS Catalog of published genome-wide association studies, targeted arrays and summary statistics 2019. *Nucleic Acids Res*. 2019;47(D1):D1005-D1012. doi:10.1093/nar/gky1120

2. **Sollis E, Mosaku A, Abid A, et al.** The NHGRI-EBI GWAS Catalog: knowledgebase and deposition resource. *Nucleic Acids Res*. 2023;51(D1):D977-D985. doi:10.1093/nar/gkac1010

### Online Resources

1. **PLINK Documentation.** Available at: [https://www.cog-genomics.org/plink/](https://www.cog-genomics.org/plink/). Accessed November 2024.

2. **1000 Genomes Project Data.** Available at: [https://www.internationalgenome.org/](https://www.internationalgenome.org/). Accessed November 2024.

3. **NHGRI-EBI GWAS Catalog.** Available at: [https://www.ebi.ac.uk/gwas/](https://www.ebi.ac.uk/gwas/). Accessed November 2024.

4. **GTEx Portal.** The Genotype-Tissue Expression (GTEx) Project. Available at: [https://gtexportal.org/](https://gtexportal.org/). Accessed November 2024.

---

**This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, **please comment and share** **your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.**
