# How To Analyze Whole Genome Sequencing Data For Absolute Beginners Part 4: Visualizing and Interpreting Somatic Mutations

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/)

- [Introduction: From Multiple VCF Files to Biological Insights](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Introduction_From_Multiple_VCF_Files_to_Biological_Insights)

  - [Understanding Key Concepts in Cancer Mutation Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Understanding_Key_Concepts_in_Cancer_Mutation_Analysis)
  - [Clinical and Research Significance](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Clinical_and_Research_Significance)
  - [What You’ll Learn](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#What_Youll_Learn)
  - [Prerequisites](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Prerequisites)

- [Converting Multiple VCF Files to MAF Format](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Converting_Multiple_VCF_Files_to_MAF_Format)

  - [Setting Up vcf2maf](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Setting_Up_vcf2maf)
  - [Converting VCF to MAF](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Converting_VCF_to_MAF)
  - [Merging MAF Files](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Merging_MAF_Files)

- [Setting Up the R Environment](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Setting_Up_the_R_Environment)

  - [Installing Essential R Packages](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Installing_Essential_R_Packages)
  - [Loading the MAF Data](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Loading_the_MAF_Data)

- [Creating Publication-Ready Oncoplots](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Creating_Publication-Ready_Oncoplots)

  - [Basic Oncoplot](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Basic_Oncoplot)
  - [Advanced Oncoplot with Custom Features](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Advanced_Oncoplot_with_Custom_Features)
  - [Gene-Specific Lollipop Plots](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Gene-Specific_Lollipop_Plots)

- [Mutation Summary and Statistics](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Mutation_Summary_and_Statistics)

  - [Sample-Level Mutation Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Sample-Level_Mutation_Analysis)
  - [Gene-Level Mutation Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Gene-Level_Mutation_Analysis)
  - [Transition/Transversion Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#TransitionTransversion_Analysis)

- [Mutation Signature Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Mutation_Signature_Analysis)

  - [Preparing Data for Signature Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Preparing_Data_for_Signature_Analysis)
  - [Visualizing Mutation Signatures](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Visualizing_Mutation_Signatures)

- [Mutation Hotspot Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Mutation_Hotspot_Analysis)

  - [Analyzing Mutation Hotspots](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Analyzing_Mutation_Hotspots)
  - [Protein Domain Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Protein_Domain_Analysis)
  - [Sample Comparison and Clustering](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Sample_Comparison_and_Clustering)

- [Best Practices for Mutation Visualization](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Best_Practices_for_Mutation_Visualization)

  - [Quality Control Considerations](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Quality_Control_Considerations)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Common_Pitfalls_to_Avoid)
  - [Extending the Analysis](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Extending_the_Analysis)

- [Conclusion](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Conclusion)

  - [Next Steps](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Next_Steps)

- [References](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-4-visualizing-and-interpreting-somatic-mutations/#Like_this)

## Introduction: From Multiple VCF Files to Biological Insights

This tutorial builds upon our previous whole genome sequencing analysis pipeline, specifically the mutation calling results from [Part 2A: Matched Tumor-Normal Mutation Calling with Mutect2](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/). You should now have multiple high-confidence VCF files from different tumor-normal pairs that need to be converted to MAF (Mutation Annotation Format) and visualized for biological interpretation.

### Understanding Key Concepts in Cancer Mutation Analysis

Before diving into the technical implementation, it’s essential to understand the core analytical concepts we’ll be working with:

**Oncoplots (Mutation Landscapes):** These are matrix-style heatmaps that display mutation status across multiple samples and genes simultaneously. Each row represents a gene, each column represents a sample, and colored cells indicate the presence and type of mutations. Oncoplots are the gold standard for visualizing mutation patterns in cancer genomics because they allow researchers to quickly identify frequently mutated genes, sample-specific mutation profiles, and potential mutation co-occurrence or mutual exclusivity patterns. They’re particularly valuable for identifying driver genes and understanding tumor heterogeneity across patient cohorts.

**Mutation Burden Analysis:** This refers to the quantitative assessment of the total number of mutations per sample, often normalized by genome size (mutations per megabase). Mutation burden is a critical biomarker in cancer research because it can predict treatment response, particularly to immunotherapy. Tumors with high mutation burden often have more neoantigens that can be recognized by the immune system, making them more susceptible to checkpoint inhibitor therapies. Additionally, mutation burden can indicate underlying DNA repair defects or exposure to mutagens.

**Transition/Transversion (Ti/Tv) Analysis:** This quality control assessment examines the types of single nucleotide substitutions in mutation data by categorizing them as transitions (mutations between chemically similar bases: A↔G or C↔T) or transversions (mutations between chemically different base types: purines to pyrimidines or vice versa). The Ti/Tv ratio serves as both a data quality indicator and a biological signature – normal human samples typically show ratios of 2.0-3.0, with deviations suggesting either technical artifacts (ratios <1.5 or >4.0) or specific mutational processes like APOBEC enzyme activity, UV exposure, or alkylating agent damage. In cancer genomics, Ti/Tv analysis helps validate mutation calls, identify underlying carcinogenic exposures, and characterize the mutational landscape that shaped each tumor. This analysis is particularly valuable because different cancer types and treatment exposures create characteristic Ti/Tv patterns, making it both a quality control checkpoint and a window into the biological processes driving mutagenesis.

**Mutation Signatures:** These are characteristic patterns of mutations that reflect the underlying mutational processes that have been active in cancer cells. Each signature represents a specific combination of mutation types in their trinucleotide context (the mutated base plus its immediate neighbors). For example, UV exposure creates a distinctive pattern of C>T mutations at dipyrimidine sites, while defective mismatch repair creates signatures characterized by small insertions and deletions at microsatellites. Identifying mutation signatures can reveal the etiology of cancer, predict treatment responses, and guide therapeutic strategies.

**Mutation Hotspots:** These are specific genomic positions or protein domains that are recurrently mutated across multiple cancer samples. Hotspots often indicate functionally important sites where mutations provide a growth advantage to cancer cells. Identifying hotspots helps distinguish between driver mutations (those that contribute to cancer development) and passenger mutations (those that occur randomly). Therapeutic targeting of mutation hotspots has led to successful precision medicine approaches, such as targeting EGFR mutations in lung cancer or BRAF mutations in melanoma.

### Clinical and Research Significance

These analytical approaches provide crucial insights for both research and clinical applications:

- **Treatment Selection:** Mutation burden and specific mutation signatures can guide immunotherapy decisions

- **Drug Development:** Hotspot analysis identifies potential therapeutic targets for precision medicine

- **Prognosis:** Mutation patterns can predict patient outcomes and disease progression

- **Resistance Mechanisms:** Temporal analysis of mutations can reveal how tumors evolve and develop resistance

- **Population Studies:** Large-scale mutation analysis reveals cancer subtypes and population-specific variants

### What You’ll Learn

By the end of this tutorial, you’ll be able to:

1. **Convert multiple VCF files to a single MAF file** using vcf2maf with proper annotation

2. **Create publication-ready oncoplots** showing mutation landscapes across samples

3. **Analyze mutation burden and patterns** with statistical summaries

4. **Identify mutation signatures** that reveal underlying mutational processes

5. **Discover mutation hotspots** and recurrent variants across your samples

### Prerequisites

- Completed mutation calling tutorial with multiple tumor-normal pairs

- Basic familiarity with R and command line

- High-confidence VCF files from Mutect2 analysis

- Access to the conda environment from the [Part 3: Annotating SNVs and Mutations with Multiple Tools](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/)

## Converting Multiple VCF Files to MAF Format

Before we can visualize our mutations in R, we need to convert the VCF files to the MAF format that maftools requires.

### Setting Up vcf2maf

First, let’s activate our conda environment from the annotation tutorial and set up the vcf2maf tool:

```bash
# Activate the conda environment from the WGS tutorial part 3
conda activate wgs_analysis

# Create and navigate to the visualization directory
mkdir -p ~/wgs/mutation_analysis
cd ~/wgs/mutation_analysis

# Clone and set up vcf2maf
git clone https://github.com/mskcc/vcf2maf.git
cd vcf2maf
chmod +x vcf2maf.pl maf2vcf.pl
cd ../
```

### Converting VCF to MAF

Now let’s convert each high-confidence VCF file to MAF format. This step adds comprehensive functional annotations using VEP (Variant Effect Predictor):

```bash
# Convert tumor1_vs_normal1 VCF to MAF
~/wgs/mutation_analysis/vcf2maf/vcf2maf.pl \
  --input-vcf tumor1_vs_normal1_high_confidence.vcf \
  --output-maf tumor1_vs_normal1_high_confidence.maf \
  --ncbi-build GRCh38 \
  --vep-path ~/wgs_analysis/bin/ \
  --vep-data ~/wgs_tutorial/annotation/vep_cache \
  --ref-fasta ~/wgs_tutorial/reference/Homo_sapiens_assembly38.fasta
```

**Repeat this process for each additional sample**, changing the input and output filenames accordingly:

- `tumor2_vs_normal2_high_confidence.vcf` → `tumor2_vs_normal2_high_confidence.maf`

- `tumor3_vs_normal3_high_confidence.vcf` → `tumor3_vs_normal3_high_confidence.maf`

- And so on for all your samples…

### Merging MAF Files

Once all individual MAF files are created, we’ll merge them using R and the maftools package (see the following section for package installation), which provides a more robust approach than command-line concatenation:

```r
# Start R and load maftools
library(maftools)

# Create a list of MAF files to merge
maf_files <- c(
    "tumor1_vs_normal1_high_confidence.maf",
    "tumor2_vs_normal2_high_confidence.maf",
    "tumor3_vs_normal3_high_confidence.maf"
    # Add additional MAF files as needed
)

# Merge all MAF files into a single MAF object
# The merge_mafs function properly handles different MAF formats and validates data integrity
merged_maf <- merge_mafs(
    mafs = maf_files,                    # List of MAF files to merge
    verbose = TRUE                       # Show progress and summary information
)

# Save the merged MAF object as a file for future use
write.table(
    merged_maf@data,                     # Extract the data component
    file = "merged_mutations.maf",       # Output filename
    sep = "\t",                          # Tab-separated format
    quote = FALSE,                       # Don't quote strings
    row.names = FALSE                    # Don't include row names
)
```

The `merge_mafs` function offers several advantages over simple file concatenation:

- **Data validation**: Ensures consistent column formats across files

- **Duplicate handling**: Identifies and manages potential duplicate entries

- **Error checking**: Validates MAF format compliance

- **Summary statistics**: Provides information about the merging process

Now you have a properly merged MAF file ready for comprehensive analysis.

## Setting Up the R Environment

### Installing Essential R Packages

Let’s install the packages we need for this visualization and analysis tutorial:

```r
#-----------------------------------------------
# Install essential R packages for mutation visualization
#-----------------------------------------------

# Set up CRAN mirror for package installation
options(repos = c(CRAN = "https://cloud.r-project.org/"))

# Install Bioconductor if not already installed
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# List of essential packages for this tutorial
essential_packages <- c(
    # Data manipulation and visualization
    "tidyverse",           # For data wrangling and ggplot2
    "data.table",          # Fast data manipulation

    # Mutation analysis packages
    "maftools",            # Main package for mutation visualization and analysis
    "MutationalPatterns",  # For mutation signature analysis
    "BSgenome.Hsapiens.UCSC.hg38",  # Human genome reference

    # Visualization enhancements
    "RColorBrewer",        # Professional color palettes
    "pheatmap"             # Pretty heatmaps for additional visualizations
)

# Install packages
for (pkg in essential_packages) {
    if (!requireNamespace(pkg, quietly = TRUE)) {
        if (pkg %in% c("tidyverse", "data.table", "RColorBrewer", "pheatmap")) {
            install.packages(pkg)
        } else {
            BiocManager::install(pkg, update = FALSE)
        }
    }
}

# Load essential packages
library(tidyverse)
library(maftools)
library(RColorBrewer)

# Set up plotting theme for consistent visualization
theme_set(theme_minimal())
```

### Loading the MAF Data

For this tutorial, we’ll use the example dataset from maftools to demonstrate the visualization techniques. You can then apply these same methods to your merged MAF file:

```r
#-----------------------------------------------
# Load mutation data for analysis
#-----------------------------------------------

# Load the example TCGA LAML (Acute Myeloid Leukemia) dataset from maftools
# This dataset contains mutations from 200 samples and serves as an excellent example
laml.maf <- system.file('extdata', 'tcga_laml.maf.gz', package = 'maftools')

# Read the MAF file into a maftools object
# This creates a structured object optimized for mutation analysis
maf <- read.maf(maf = laml.maf, verbose = TRUE)

# To use your own merged MAF file instead, uncomment the following line:
# maf <- read.maf(maf = "merged_mutations.maf", verbose = TRUE)

# The MAF object contains:
# - Sample summary: mutation counts per sample
# - Gene summary: mutation frequencies per gene  
# - Variant classifications: types of mutations found
# - Clinical data: if available in the MAF file
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/maf_format_example.png?resize=1024%2C160&ssl=1)

## Creating Publication-Ready Oncoplots

Oncoplots are the signature visualization in cancer genomics, displaying mutation patterns across samples and genes in a matrix format.

### Basic Oncoplot

The most fundamental visualization shows mutation patterns across your top mutated genes. This creates a matrix where rows are genes, columns are samples, and colors indicate mutation types.

```bash
#-----------------------------------------------
# Create basic oncoplot showing top mutated genes
#-----------------------------------------------

# Create a basic oncoplot showing the top 20 most frequently mutated genes
# Parameters:
# - maf: the MAF object containing our mutation data
# - top: number of top mutated genes to display
# - removeNonMutated: exclude samples with no mutations in displayed genes
oncoplot(
    maf = maf, 
    top = 20,                    # Show top 20 mutated genes
    removeNonMutated = FALSE     # Keep all samples for comparison
)

# Save the plot as a high-quality PDF for publication
pdf("basic_oncoplot.pdf", width = 12, height = 8)
oncoplot(maf = maf, top = 20, removeNonMutated = FALSE)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/oncoplot.png?resize=1024%2C614&ssl=1)

### Advanced Oncoplot with Custom Features

Now we’ll create a more sophisticated version with custom colors, better formatting, and clearer annotations for publication-quality output.

```r
#-----------------------------------------------
# Create advanced oncoplot with customizations
#-----------------------------------------------

# Define custom colors for different mutation types
# This helps distinguish between different types of genetic alterations
mutation_colors <- c(
    'Frame_Shift_Del' = '#A41E22',     # Red for deletions
    'Frame_Shift_Ins' = '#A41E22',     # Red for insertions  
    'Missense_Mutation' = '#1F78B4',   # Blue for missense mutations
    'Nonsense_Mutation' = '#33A02C',   # Green for nonsense mutations
    'Splice_Site' = '#6A3D9A',         # Purple for splice site mutations
    'In_Frame_Del' = '#FB9A99',        # Light red for in-frame deletions
    'In_Frame_Ins' = '#FB9A99',        # Light red for in-frame insertions
    'Silent' = '#BEBEBE'               # Gray for silent mutations
)

# Create an advanced oncoplot with customizations
oncoplot(
    maf = maf,
    top = 15,                          # Focus on top 15 genes for clarity
    colors = mutation_colors,          # Apply custom color scheme
    showTumorSampleBarcodes = TRUE,    # Display sample names
    fontSize = 0.8,                    # Adjust text size for readability
    titleText = "Mutation Landscape - Top 15 Genes",  # Add descriptive title
    legendFontSize = 0.8,              # Adjust legend text size
    annotationFontSize = 0.8           # Adjust annotation text size
)

# Save as high-resolution PDF for publication
pdf("advanced_oncoplot.pdf", width = 14, height = 10)
oncoplot(
    maf = maf,
    top = 15,
    colors = mutation_colors,
    showTumorSampleBarcodes = TRUE,
    fontSize = 0.8,
    titleText = "Mutation Landscape - Top 15 Genes",
    legendFontSize = 0.8,
    annotationFontSize = 0.8
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/advanced_oncoplot.png?resize=1024%2C731&ssl=1)

### Gene-Specific Lollipop Plots

For detailed analysis of individual genes, lollipop plots show exactly where mutations occur within protein sequences, revealing potential functional domains and hotspots.

```r
#-----------------------------------------------
# Create lollipop plots for specific genes of interest
#-----------------------------------------------

# Get the most frequently mutated genes from our dataset
gene_summary <- getGeneSummary(maf)
top_genes <- gene_summary$Hugo_Symbol[1:5]  # Get top 5 genes

# Create lollipop plots for each top mutated gene
# Lollipop plots show the distribution of mutations along the protein sequence
pdf("lollipop_plots.pdf", width = 12, height = 8)

for (gene in top_genes) {
    # Create lollipop plot for each gene
    # This shows where mutations occur within the protein structure
    lollipopPlot(
        maf = maf,
        gene = gene,                    # Gene to visualize
        showMutationRate = TRUE,        # Display mutation frequency
        labelPos = "all"                # Label all mutation positions
    )
}

dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/lollipop_mutation.png?resize=1024%2C620&ssl=1)

## Mutation Summary and Statistics

Understanding the overall mutation landscape requires comprehensive statistical analysis of mutation patterns and burden.

### Sample-Level Mutation Analysis

This analysis calculates key statistics for each sample, including total mutation burden, mutation rates, and the distribution of different mutation types across your cohort.

```r
#-----------------------------------------------
# Analyze mutation burden and patterns across samples
#-----------------------------------------------

# Get detailed sample summary statistics
sample_summary <- getSampleSummary(maf)

# Calculate additional mutation statistics
mutation_stats <- sample_summary %>%
    mutate(
        # Calculate mutation rate per megabase (assuming ~30 Mb exome)
        mutation_rate_per_mb = total / 30,

        # Calculate percentage of each mutation type
        pct_missense = Missense_Mutation / total * 100,
        pct_nonsense = Nonsense_Mutation / total * 100,
        pct_frameshift = (Frame_Shift_Del + Frame_Shift_Ins) / total * 100,
        pct_splice = Splice_Site / total * 100
    )

# Display basic statistics
head(mutation_stats)

# Create mutation burden visualization
ggplot(mutation_stats, aes(x = reorder(Tumor_Sample_Barcode, -mutation_rate_per_mb), y = mutation_rate_per_mb)) +
    geom_bar(stat = "identity", fill = "#1F78B4", alpha = 0.7) +
    labs(
        title = "Mutation Burden Across Samples",
        x = "Sample ID", 
        y = "Mutations per Megabase",
        subtitle = paste("Mean:", round(mean(mutation_stats$mutation_rate_per_mb), 1), "mutations/Mb")
    ) +
    theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
    geom_hline(yintercept = mean(mutation_stats$mutation_rate_per_mb), 
               linetype = "dashed", color = "red", alpha = 0.6)

# Save the mutation burden plot
ggsave("mutation_burden_analysis.pdf", width = 12, height = 6)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/sample_summary.png?resize=1024%2C130&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/mutation_burden.png?resize=1024%2C420&ssl=1)

### Gene-Level Mutation Analysis

Here we identify which genes are most frequently mutated across the cohort and calculate mutation frequencies as percentages of total samples.

```r
#-----------------------------------------------
# Analyze mutation patterns at the gene level
#-----------------------------------------------

# Get gene-level summary statistics
gene_summary <- getGeneSummary(maf)

# Analyze the most frequently mutated genes
top_mutated_genes <- gene_summary %>%
    head(20) %>%
    mutate(
        # Calculate mutation frequency as percentage of samples
        mutation_frequency = MutatedSamples / getSampleSummary(maf) %>% nrow() * 100
    )

# Visualize top mutated genes
ggplot(top_mutated_genes, aes(x = reorder(Hugo_Symbol, MutatedSamples), 
                              y = MutatedSamples)) +
    geom_bar(stat = "identity", fill = "#33A02C", alpha = 0.7) +
    coord_flip() +
    labs(
        title = "Most Frequently Mutated Genes",
        x = "Gene Symbol",
        y = "Number of Mutated Samples",
        subtitle = "Top 20 genes ranked by mutation frequency"
    ) +
    theme_minimal()

# Save the gene frequency plot
ggsave("top_mutated_genes.pdf", width = 10, height = 8)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/gene_summary.png?resize=1024%2C161&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/top_mutated_genes.png?resize=1024%2C823&ssl=1)

### Transition/Transversion Analysis

This quality control analysis examines the chemical nature of base substitutions, providing both validation of data quality and insights into underlying mutational processes.

```r
#-----------------------------------------------
# Analyze transition/transversion (Ti/Tv) ratios
#-----------------------------------------------

# Calculate Ti/Tv ratios for quality assessment
# Normal Ti/Tv ratios are typically 2-3 for most cancer types
titv_analysis <- titv(maf = maf, plot = FALSE, useSyn = TRUE)

# Save Ti/Tv analysis
pdf("titv_analysis.pdf", width = 10, height = 6)
plotTiTv(res = titv_analysis)
dev.off()

# Samples with very low (<1.5) or very high (>4) Ti/Tv ratios may indicate
# technical issues or specific mutational processes
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/titv_analysis.png?resize=1024%2C569&ssl=1)

## Mutation Signature Analysis

Mutation signatures reveal the underlying mutational processes that shaped the tumor genome, such as DNA repair deficiencies, environmental exposures, or treatment effects.

### Preparing Data for Signature Analysis

```r
#-----------------------------------------------
# Extract and analyze mutation signatures
#-----------------------------------------------

# Load required packages for signature analysis
library(MutationalPatterns)
library(BSgenome.Hsapiens.UCSC.hg19)

# Extract trinucleotide context for signature analysis
# This creates a 96-dimensional mutational profile for each sample
trinuc_matrix <- trinucleotideMatrix(
    maf = maf, 
    prefix = 'chr',                    # Chromosome prefix format
    add = TRUE,                        # Add to existing MAF object
    ref_genome = "BSgenome.Hsapiens.UCSC.hg19"  # Reference genome
)

# The trinucleotide matrix shows the count of each possible 
# base substitution in its trinucleotide context (e.g., A[C>T]G)
```

### Visualizing Mutation Signatures

With the trinucleotide matrix created, we can now analyze APOBEC signature enrichment, which reveals whether specific mutational processes have been active in these samples.

```bash
#-----------------------------------------------
# Create mutation signature visualizations
#-----------------------------------------------

# Plot the 96-trinucleotide profile for the first few samples
# This shows the characteristic "signature" of mutational processes
plotApobecDiff(
    tnm = trinuc_matrix,               # Trinucleotide matrix
    maf = maf,                         # MAF object
    pVal = 0.2                         # P-value threshold for significance
)

# Save signature analysis plot
pdf("mutation_signatures.pdf", width = 12, height = 8)
plotApobecDiff(tnm = trinuc_matrix, maf = maf, pVal = 0.2)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/mutation_signature.png?resize=1024%2C693&ssl=1)

## Mutation Hotspot Analysis

Identifying recurrent mutations and hotspots helps prioritize functionally important variants and potential therapeutic targets.

### Analyzing Mutation Hotspots

This statistical analysis identifies genes that have significantly more mutations than expected by chance, helping distinguish potential driver genes from passenger mutations.

```r
#-----------------------------------------------
# Detect and visualize mutation hotspots
#-----------------------------------------------

# Find genes with significantly more mutations than expected
# This helps identify potential driver genes vs passenger mutations
oncotable <- oncodrive(maf = maf, AACol = 'Protein_Change', minMut = 5)

# Display top oncogenic genes
head(oncotable)

# Create visualization of oncogenes
ggplot(head(oncotable, 15), aes(x = reorder(Hugo_Symbol, -zscore), y = zscore)) +
    geom_bar(stat = "identity", fill = "#E31A1C", alpha = 0.7) +
    geom_hline(yintercept = 2, linetype = "dashed", color = "blue") +
    coord_flip() +
    labs(
        title = "Potential Driver Genes",
        x = "Gene Symbol",
        y = "Z-score (mutation frequency)",
        subtitle = "Genes with significantly elevated mutation rates",
        caption = "Dashed line indicates significance threshold (z-score > 2)"
    ) +
    theme_minimal()

# Save oncogene analysis
ggsave("driver_genes_analysis.pdf", width = 10, height = 8)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/oncotable.png?resize=1024%2C63&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/driver_genes.png?resize=1024%2C818&ssl=1)

### Protein Domain Analysis

This analysis examines whether mutations cluster within specific functional protein domains, indicating regions under positive selection or structural constraints.

```bash
#-----------------------------------------------
# Analyze mutations within protein domains
#-----------------------------------------------

# Identify protein domains enriched for mutations and these domains
pdf("protein_domain_analysis.pdf", width = 12, height = 8)
pfamDomains(maf = maf, top = 10)
dev.off()

# Protein domains with high mutation density may indicate:
# 1. Functionally important regions under positive selection
# 2. Structural constraints that make mutations impactful
# 3. Therapeutic targets for precision medicine approaches
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/protein_domain_analysis.png?resize=1024%2C669&ssl=1)

### Sample Comparison and Clustering

This analysis identifies patterns of mutual exclusivity and co-occurrence among mutations, revealing potential pathway relationships and therapeutic targets.

```bash
#-----------------------------------------------
# Compare mutation patterns between samples
#-----------------------------------------------

# Create a heatmap showing mutation patterns across samples and genes
# This helps identify sample clusters and gene co-mutation patterns
somaticInteractions(
    maf = maf,                         # MAF object
    top = 25,                          # Top genes to include
    pvalue = c(0.05, 0.1)             # P-value thresholds for significance
)

# Save interaction analysis
pdf("somatic_interactions.pdf", width = 12, height = 10)
somaticInteractions(maf = maf, top = 25, pvalue = c(0.05, 0.1))
dev.off()

# Mutual exclusivity and co-occurrence patterns can reveal:
# 1. Pathways that are disrupted by alternative mechanisms
# 2. Gene pairs that work together in cancer development  
# 3. Potential synthetic lethal relationships for therapy
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/somatic_interactions.png?resize=1024%2C895&ssl=1)

## Best Practices for Mutation Visualization

### Quality Control Considerations

**Data Quality Checks:**

- Verify Ti/Tv ratios are within expected ranges (2-3 for most cancers)

- Check mutation burden against cancer type expectations

- Ensure adequate sample size for statistical analyses (minimum 10-20 samples)

- Validate key findings with independent datasets or literature

**Visualization Standards:**

- Use consistent color schemes across all plots

- Include appropriate statistical annotations and p-values

- Provide clear legends and axis labels

- Save plots in both vector (PDF) and raster (PNG) formats

**Interpretation Guidelines:**

- Focus on recurrent mutations across multiple samples

- Prioritize mutations in known cancer genes and pathways

- Consider functional impact predictions from annotation tools

- Integrate with clinical data when available

### Common Pitfalls to Avoid

**Technical Issues:**

- Not adjusting for multiple testing in statistical analyses

- Ignoring batch effects between sequencing runs

- Using inappropriate reference genomes or annotation versions

- Insufficient filtering of low-quality variants

**Biological Interpretation:**

- Over-interpreting single-sample findings

- Ignoring passenger mutations in highly mutated tumors

- Not considering tumor heterogeneity and clonal evolution

- Failing to validate computational predictions experimentally

### Extending the Analysis

**Advanced Visualizations:**

- Integrate copy number and structural variant data

- Create interactive plots using plotly or shiny

- Generate patient-specific mutation reports

- Compare with public datasets (TCGA, COSMIC)

**Clinical Applications:**

- Identify actionable mutations for targeted therapy

- Assess mutation signatures for treatment selection

- Monitor mutation evolution in longitudinal samples

- Develop prognostic or predictive biomarkers

## Conclusion

You now have a comprehensive toolkit for visualizing and interpreting somatic mutations from whole genome sequencing data. The combination of vcf2maf for data preparation and maftools for analysis provides a powerful framework for:

- **Professional Visualization:** Publication-ready oncoplots and mutation landscapes

- **Statistical Analysis:** Rigorous assessment of mutation patterns and significance

- **Biological Insight:** Understanding mutational processes and driver genes

- **Clinical Translation:** Identifying actionable findings for precision medicine

### Next Steps

- **Validation:** Confirm key mutations using orthogonal sequencing methods

- **Integration:** Combine with RNA-seq, copy number, and clinical data

- **Functional Studies:** Design experiments to test mutation impact

- **Clinical Application:** Translate findings into diagnostic or therapeutic strategies

This tutorial provides the foundation for advanced mutation analysis. As you apply these methods to your own data, remember that the goal is not just to catalog mutations, but to understand their biological significance and clinical relevance.

## References

1. **Mayakonda, A., Lin, D. C., Assenov, Y., Plass, C., & Koeffler, H. P. (2018).** Maftools: efficient and comprehensive analysis of somatic variants in cancer. *Genome Research*, 28(11), 1747-1756.

2. **Alexandrov, L. B., Kim, J., Haradhvala, N. J., et al. (2020).** The repertoire of mutational signatures in human cancer. *Nature*, 578(7793), 94-101.

3. **Blokzijl, F., Janssen, R., van Boxtel, R., & Cuppen, E. (2018).** MutationalPatterns: comprehensive genome-wide analysis of mutational processes. *Genome Medicine*, 10(1), 33.

4. **Chakravarty, D., Gao, J., Phillips, S. M., et al. (2017).** OncoKB: A Precision Oncology Knowledge Base. *JCO Precision Oncology*, 1, 1-16.

5. **Tamborero, D., Gonzalez-Perez, A., & Lopez-Bigas, N. (2013).** OncodriveCLUST: exploiting the positional clustering of somatic mutations to identify cancer genes. *Bioinformatics*, 29(18), 2238-2244.

6. **Cancer Genome Atlas Research Network. (2013).** Genomic and epigenomic landscapes of adult de novo acute myeloid leukemia. *New England Journal of Medicine*, 368(22), 2059-2074.

7. **Martincorena, I., & Campbell, P. J. (2015).** Somatic mutation in cancer and normal cells. *Science*, 349(6255), 1483-1489.

8. **Rheinbay, E., Nielsen, M. M., Abascal, F., et al. (2020).** Analyses of non-coding somatic drivers in 2,658 cancer whole genomes. *Nature*, 578(7793), 102-111.

---

***This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, please **comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.***
