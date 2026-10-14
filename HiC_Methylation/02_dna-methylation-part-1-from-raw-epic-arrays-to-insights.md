# How To Analyze DNA Methylation Data For Absolute Beginners Part 1: From Raw EPIC Arrays To Biological Insights

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** January 5, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/)

---

*A comprehensive step-by-step tutorial for analyzing DNA methylation data using Illumina Infinium MethylationEPIC BeadChip v2.0*

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/)

- [Understanding DNA Methylation and Its Biological Significance](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Understanding_DNA_Methylation_and_Its_Biological_Significance)

  - [What is DNA Methylation and Why Does It Matter?](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#What_is_DNA_Methylation_and_Why_Does_It_Matter)
  - [Biological Insights from DNA Methylation Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Biological_Insights_from_DNA_Methylation_Analysis)
  - [Understanding the Illumina Infinium MethylationEPIC BeadChip v2.0 Technology](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Understanding_the_Illumina_Infinium_MethylationEPIC_BeadChip_v20_Technology)
  - [Comprehensive DNA Methylation Analysis Workflow](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Comprehensive_DNA_Methylation_Analysis_Workflow)

- [Setting Up Your R Environment for DNA Methylation Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Setting_Up_Your_R_Environment_for_DNA_Methylation_Analysis)
- [Understanding EPIC v2.0 Data Structure and Import Procedures](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Understanding_EPIC_v20_Data_Structure_and_Import_Procedures)

  - [Raw Data Format and File Structure](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Raw_Data_Format_and_File_Structure)
  - [Importing Raw Methylation Data](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Importing_Raw_Methylation_Data)

- [Comprehensive Data Preprocessing and Normalization Pipeline](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Comprehensive_Data_Preprocessing_and_Normalization_Pipeline)

  - [Data Import and Initial Assessment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Data_Import_and_Initial_Assessment)
  - [Understanding Preprocessing and Normalization](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Understanding_Preprocessing_and_Normalization)

- [Comprehensive Quality Control and Sample Filtering](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Comprehensive_Quality_Control_and_Sample_Filtering)

  - [Detection P-value Filtering and Quality Assessment](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Detection_P-value_Filtering_and_Quality_Assessment)
  - [Methylation Quantification and Normalization](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Methylation_Quantification_and_Normalization)
  - [Understanding Beta-values and M-values](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Understanding_Beta-values_and_M-values)
  - [Additional Probe Filtering](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Additional_Probe_Filtering)
  - [Sample Outlier Detection](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Sample_Outlier_Detection)

- [Differential Methylation Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Differential_Methylation_Analysis)

  - [Individual CpG Site Analysis (Differentially Methylated Positions – DMPs)](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Individual_CpG_Site_Analysis_Differentially_Methylated_Positions_%E2%80%93_DMPs)
  - [Understanding Differential Methylated Regions (DMR) Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Understanding_Differential_Methylated_Regions_DMR_Analysis)
  - [Functional Enrichment Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Functional_Enrichment_Analysis)

- [Best Practices and Troubleshooting Guide](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Best_Practices_and_Troubleshooting_Guide)

  - [Essential Best Practices for DNA Methylation Analysis](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Essential_Best_Practices_for_DNA_Methylation_Analysis)
  - [Common Issues and Troubleshooting Solutions](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Common_Issues_and_Troubleshooting_Solutions)
  - [Advanced Considerations and Special Scenarios](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Advanced_Considerations_and_Special_Scenarios)

- [Visualization and Results Interpretation](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Visualization_and_Results_Interpretation)
- [Conclusion and Next Steps](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Conclusion_and_Next_Steps)

  - [Key Takeaways](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Key_Takeaways)

- [References](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-dna-methylation-data-for-absolute-beginners-from-raw-epic-arrays-to-biological-insights/#Like_this)

## Understanding DNA Methylation and Its Biological Significance

DNA methylation represents one of the most important epigenetic modifications in mammalian genomes, acting as a molecular switch that regulates gene expression without altering the underlying DNA sequence. This chemical modification primarily occurs at cytosine bases that precede guanine bases (CpG dinucleotides), creating 5-methylcytosine through the addition of a methyl group.

### What is DNA Methylation and Why Does It Matter?

DNA methylation serves as a stable, heritable mark that cells use to remember their identity and function. Unlike genetic mutations that permanently alter DNA sequence, methylation changes are potentially reversible, making them particularly important for understanding disease mechanisms and therapeutic targets.

The biological significance of DNA methylation extends across multiple cellular processes:

**Gene Expression Regulation:** Methylation at gene promoters typically silences gene expression, while methylation in gene bodies can enhance transcription. This mechanism allows cells to fine-tune gene expression levels in response to developmental cues, environmental stimuli, or disease states.

**Genomic Imprinting:** Some genes are expressed exclusively from either the maternal or paternal chromosome, with DNA methylation serving as the molecular memory that maintains this parent-of-origin expression pattern throughout life.

**X-Chromosome Inactivation:** In female mammals, DNA methylation helps maintain the silencing of one X chromosome, ensuring proper gene dosage compensation between males and females.

**Tissue Development and Differentiation:** During embryonic development, specific methylation patterns are established that help maintain cell type identity. Stem cells progressively acquire methylation marks as they differentiate into specialized cell types.

**Disease Pathogenesis:** Aberrant DNA methylation patterns are hallmarks of many diseases, particularly cancer, where tumor suppressor genes may become hypermethylated and silenced, contributing to malignant transformation.

### Biological Insights from DNA Methylation Analysis

Modern DNA methylation analysis provides researchers with powerful tools to address fundamental biological questions and clinical challenges:

**Age-Related Changes:** DNA methylation patterns change predictably with age, leading to the development of “epigenetic clocks” that can estimate biological age and health span. These methylation-based biomarkers are being investigated for their potential to predict disease risk and monitor interventional therapies.

**Environmental Response:** Methylation patterns can reflect environmental exposures, including diet, toxins, stress, and lifestyle factors. This makes methylation analysis valuable for understanding how external factors influence health and disease risk.

**Cell Type Heterogeneity:** Different cell types within the same tissue exhibit distinct methylation signatures. By analyzing these patterns, researchers can estimate the cellular composition of complex tissue samples and understand how cell type proportions change in disease states.

**Therapeutic Monitoring:** DNA methylation patterns can serve as biomarkers for treatment response, particularly in cancer therapy where DNA methyltransferase inhibitors are used to reactivate silenced tumor suppressor genes.

> 

**Research Impact:** DNA methylation studies have contributed to major breakthroughs in understanding autism spectrum disorders, Alzheimer’s disease, cancer subtypes, and developmental disorders, demonstrating the technique’s broad applicability across biomedical research.

### Understanding the Illumina Infinium MethylationEPIC BeadChip v2.0 Technology

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/MethylationEPIC-BeadChip.png?resize=1024%2C358&ssl=1)

The Illumina Infinium MethylationEPIC BeadChip v2.0 represents the latest evolution in array-based methylation analysis, interrogating approximately 935,000 CpG sites across the human genome. Understanding the underlying technology is crucial for proper data analysis and interpretation.

#### The Chemistry Behind the Array

The EPIC array technology is based on a sophisticated chemical process that begins with **bisulfite conversion** of DNA. This critical step chemically converts unmethylated cytosines to uracils while leaving methylated cytosines unchanged. This conversion creates a “chemical signature” that allows the array to distinguish between methylated and unmethylated states.

#### Red and Green Signal Detection

The EPIC array uses a two-color detection system with distinct fluorescent channels:

**Red Channel (Cy5):** Detects signals from one allele state (typically methylated or unmethylated, depending on probe design)

**Green Channel (Cy3):** Detects signals from the alternative allele state

The ratio of red to green signals at each CpG site determines the methylation level. When a CpG site is:

- **Fully methylated:** One channel shows high intensity, the other shows low intensity

- **Unmethylated:** The opposite pattern occurs

- **Partially methylated:** Both channels show intermediate intensities

This dual-channel approach provides built-in quality control and allows for precise quantification of methylation levels ranging from 0% (completely unmethylated) to 100% (fully methylated).

#### Two Types of Infinium Probes

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/Infinium-probe-type.png?resize=1024%2C853&ssl=1)

The EPIC array employs two different probe chemistries, each with distinct characteristics:

**Infinium I Probes:**

- Use separate beads for methylated and unmethylated states

- Both red and green channels detect the same target

- Generally provide higher precision and dynamic range

- Account for approximately 20% of probes on the array

**Infinium II Probes:**

- Use a single bead type per CpG site

- Red channel detects methylated state, green channel detects unmethylated state

- Allow higher probe density on the array

- Account for approximately 80% of probes on the array

- May exhibit slightly higher technical variation

Understanding these probe types is important because they require different normalization approaches and may show different performance characteristics in your analysis.

### Comprehensive DNA Methylation Analysis Workflow

The analysis of EPIC BeadChip data follows a structured workflow designed to transform raw fluorescent signals into biologically meaningful methylation patterns:

1. **Raw Data Import:** Loading .idat files containing fluorescent intensities from both red and green channels

2. **Quality Control:** Assessing sample and probe quality using detection p-values and signal distributions

3. **Preprocessing:** Background correction, normalization to account for probe type differences, and probe filtering

4. **Methylation Quantification:** Converting red/green signal ratios to beta-values and M-values

5. **Batch Effect Correction:** Removing technical variation while preserving biological signals

6. **Statistical Analysis:** Identifying differentially methylated positions (DMPs) and regions (DMRs)

7. **Biological Interpretation:** Annotating results and performing pathway enrichment analysis

## Setting Up Your R Environment for DNA Methylation Analysis

Before diving into methylation analysis, we need to establish a robust computational environment with all necessary R packages and dependencies. This step ensures we have all the tools required for comprehensive methylation analysis.

```r
# Install essential Bioconductor packages
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install(version = "3.21")

# Core methylation analysis packages
essential_packages <- c(
    "minfi",              # Core package for Illumina methylation arrays
    "missMethyl",         # Specialized functions for methylation analysis
    "limma",              # Linear models for differential analysis
    "DMRcate",            # Differential methylated regions detection
    "sva",                # Surrogate variable analysis for batch effects
    "IlluminaHumanMethylationEPICv2anno.20a1.hg38", # EPIC v2.0 annotations
    "IlluminaHumanMethylationEPICv2manifest",        # EPIC v2.0 manifest
    "TxDb.Hsapiens.UCSC.hg38.knownGene",             # Gene annotations
    "org.Hs.eg.db",                                  # Gene symbol mappings
    "ggplot2",            # General plotting
    "pheatmap",           # Heatmap visualization
    "RColorBrewer",       # Color palettes
    "dplyr",              # Data manipulation
    "reshape2"            # Data reshaping
)

BiocManager::install(essential_packages, update = TRUE, ask = FALSE)

# Load essential libraries
library(minfi)
library(limma)
library(DMRcate)
library(sva)
library(dplyr)
library(ggplot2)
library(pheatmap)
library(RColorBrewer)

# Set up working directory structure
project_dir <- "~/methylation_analysis"
dir.create(project_dir, recursive = TRUE, showWarnings = FALSE)
setwd(project_dir)

# Create organized directory structure for analysis
directories <- c(
    "data/raw",           # Raw .idat files
    "data/processed",     # Processed data objects
    "results/qc",         # Quality control outputs
    "results/differential", # Differential methylation results
    "results/annotation", # Annotation results
    "plots",              # All visualization outputs
    "reports"             # Analysis reports
)

sapply(directories, function(x) dir.create(x, recursive = TRUE, showWarnings = FALSE))

# Configure R session options for optimal performance
options(
    stringsAsFactors = FALSE,
    scipen = 999,
    max.print = 100,
    width = 120
)

set.seed(12345)  # Ensure reproducible results
```

This setup creates a well-organized project structure and installs all necessary packages. The directory structure helps maintain organized analysis files and makes it easy to locate results later.

## Understanding EPIC v2.0 Data Structure and Import Procedures

### Raw Data Format and File Structure

The Illumina EPIC v2.0 scanner generates several important file types that we need to understand:

**IDAT Files (Intensity Data Files):** These binary files contain the raw fluorescent intensities for each probe on the array. The EPIC technology measures both red and green fluorescent signals, so each sample generates two .idat files:

- `*_Red.idat`: Contains intensities for the red fluorescent channel (Cy5)

- `*_Grn.idat`: Contains intensities for the green fluorescent channel (Cy3)

**Sample Sheet:** A CSV file containing sample metadata, including sample identifiers, biological groupings, technical variables, and any experimental conditions. This file links the raw data files to meaningful sample information.

### Importing Raw Methylation Data

For this tutorial, we’ll use dataset **[GSE240469](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE240469)** as our example, which contains EPIC v2.0 data from cancer cell lines treated with different conditions.

```r
# Extract sample information from filenames
# The file naming convention contains important sample metadata
idat_files <- list.files("data/raw", pattern = "*.idat", full.names = TRUE)
red_files <- idat_files[grepl("Red.idat", idat_files)]
basenames <- gsub("_Red.idat", "", red_files)

# Create sample information dataframe
sample_info <- data.frame(
    Basename = basenames,
    Sample_Name = basename(basenames),
    stringsAsFactors = FALSE
)

# Parse sample types from filenames (extract biological information)
sample_info$Sample_Type <- sapply(sample_info$Sample_Name, function(x) {
    if(grepl("PREC", x)) return("PREC")      # Prostate cancer cell line
    if(grepl("LNCAP", x)) return("LNCAP")    # Prostate cancer cell line
    if(grepl("SYN", x)) return("SYN")        # Synthetic control
    if(grepl("MCF7", x)) return("MCF7")      # Breast cancer cell line
    if(grepl("TAMR", x)) return("TAMR")      # Tamoxifen-resistant cell line
    if(grepl("FD", x)) return("FD")          # Control condition
    return("Unknown")
})

# Extract treatment information from filenames
sample_info$Treatment <- sapply(sample_info$Sample_Name, function(x) {
    if(grepl("500", x)) return("500")        # High concentration treatment
    if(grepl("250", x)) return("250")        # Medium concentration treatment
    if(grepl("125", x)) return("125")        # Low concentration treatment
    if(grepl("Aza", x)) return("Aza")        # 5-azacytidine treatment
    if(grepl("NoAza", x)) return("NoAza")    # No treatment control
    return("Control")
})

# Create simplified grouping for statistical analysis
sample_info$Group <- paste(sample_info$Sample_Type, sample_info$Treatment, sep = "_")
sample_info$Group[sample_info$Group == "FD_Control"] <- "FD"
sample_info$Group[sample_info$Group == "MCF7_Control"] <- "MCF7"

# Add technical variables that may affect data quality
sample_info$Slide <- "GSE240469"           # Experimental batch identifier
sample_info$Array <- "EPICv2"              # Array type

# Save sample sheet for future reference
write.csv(sample_info, "data/raw/sample_sheet.csv", row.names = FALSE)
```

This step creates a comprehensive sample information table that links each raw data file to its biological and technical characteristics. Proper sample annotation is crucial for downstream analysis and interpretation.

## Comprehensive Data Preprocessing and Normalization Pipeline

### Data Import and Initial Assessment

The first step in analysis involves importing the raw fluorescent intensity data and performing initial quality assessments.

```r
# Import methylation data using the sample information
# This function reads both red and green .idat files for each sample
rgSet <- read.metharray.exp(targets = sample_info, recursive = TRUE, extended = TRUE, force = TRUE)

# Get probe information to understand array content
probe_info <- getProbeInfo(rgSet)
```

The `rgSet` object contains the raw red and green channel intensities for all probes and samples. This is our starting point for all subsequent processing steps.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/probe_info.png?resize=1024%2C158&ssl=1)

### Understanding Preprocessing and Normalization

Preprocessing and normalization are critical steps that address several technical issues inherent in array-based methylation data:

#### Why Preprocessing is Necessary

**Background Signal Correction:** The array scanner picks up background fluorescence that must be subtracted from the true signal. Without correction, this background noise can obscure real biological differences.

**Probe Type Differences:** As mentioned earlier, Infinium I and II probes have different chemical properties and dynamic ranges. Normalization ensures these probe types are comparable.

**Dye Bias Correction:** The red and green dyes may have different labeling efficiencies or detection sensitivities, creating systematic bias that needs correction.

**Sample-to-Sample Variation:** Technical factors during sample processing can create batch effects that mask true biological differences.

#### Normalization Methods Explained

Several normalization methods are available, each with different strengths:

**Functional Normalization (FunNorm):** This method is specifically designed for EPIC arrays and uses control probes to model and remove technical variation while preserving biological signal. It’s particularly effective at removing batch effects.

**Subset-quantile Within Array Normalization (SWAN):** This method separately normalizes Infinium I and II probes, making them more comparable.

**Quantile Normalization:** A general method that makes the distribution of intensities identical across samples.

For this tutorial, we’ll use Functional Normalization (FunNorm) as it’s considered the gold standard for EPIC array data.

## Comprehensive Quality Control and Sample Filtering

### Detection P-value Filtering and Quality Assessment

Quality control is essential to ensure reliable results. We use several metrics to assess data quality:

**Detection P-values:** These represent the probability that a probe’s signal is distinguishable from background noise. Low p-values (< 0.01) indicate reliable signal detection.

```r
# Calculate detection p-values for all probes and samples
# These p-values indicate whether each probe's signal is above background
detP <- detectionP(rgSet)

# Set quality control thresholds
detP_threshold <- 0.01          # Probes with p > 0.01 are considered "failed"
sample_failure_rate <- 0.05     # Remove samples with >5% failed probes
probe_failure_rate <- 0.10      # Remove probes that fail in >10% of samples

# Identify failed probes and samples using these criteria
failed_probes <- rowMeans(detP > detP_threshold, na.rm = TRUE) > probe_failure_rate
failed_samples <- colMeans(detP > detP_threshold, na.rm = TRUE) > sample_failure_rate

# Create quality control plots to visualize data quality
pdf("plots/qc_summary.pdf", width = 12, height = 8)
par(mfrow = c(2, 2))

# Plot sample failure rates
sample_failure_rates <- colMeans(detP > detP_threshold, na.rm = TRUE)
barplot(sample_failure_rates, main = "Sample Quality Assessment", 
        ylab = "Proportion of Failed Probes", las = 2, cex.names = 0.7,
        col = ifelse(sample_failure_rates > sample_failure_rate, "red", "lightblue"))
abline(h = sample_failure_rate, col = "red", lwd = 2, lty = 2)

# Plot probe failure distribution
probe_failure_rates <- rowMeans(detP > detP_threshold, na.rm = TRUE)
hist(probe_failure_rates, main = "Probe Quality Distribution", 
     xlab = "Proportion of Failed Samples", breaks = 50, col = "lightblue")
abline(v = probe_failure_rate, col = "red", lwd = 2, lty = 2)

dev.off()

# Generate comprehensive quality control report
qcReport(rgSet)

# Remove poor quality samples if any are detected
if(any(failed_samples)) {
    rgSet <- rgSet[, !failed_samples]
    sample_info <- sample_info[!failed_samples, ]
    cat("Removed", sum(failed_samples), "poor quality samples\n")
} else {
    cat("All samples passed quality control\n")
}
```

This quality control step is crucial because poor quality probes or samples can introduce noise and bias into downstream analyses.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/epic-qc-1.png?resize=1024%2C443&ssl=1)

### Methylation Quantification and Normalization

Now we perform the actual normalization and convert the raw signals into methylation values:

```r
# Apply functional normalization to correct for technical variation
mSet_norm <- preprocessFunnorm(rgSet, nPCs = 2, sex = NULL, bgCorr = TRUE, dyeCorr = TRUE)

# Remove failed probes after normalization
if(any(failed_probes)) {
    common_probes <- intersect(rownames(mSet_norm), names(failed_probes))
    failed_probes_subset <- failed_probes[common_probes]
    mSet_norm <- mSet_norm[!failed_probes_subset, ]
    cat("Removed", sum(failed_probes_subset), "failed probes\n")
} else {
    cat("All probes passed quality control\n")
}

# Extract beta-values and M-values (explained in detail below)
beta_values <- getBeta(mSet_norm)
m_values <- getM(mSet_norm)

# Save processed data for future use
saveRDS(list(
    mSet_norm = mSet_norm,
    beta_values = beta_values,
    m_values = m_values,
    sample_info = sample_info
), "data/processed/normalized_data.rds")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/epic-beta-value.png?resize=1024%2C123&ssl=1)

### Understanding Beta-values and M-values

The EPIC array produces raw fluorescent intensities, but for biological interpretation, we need to convert these to methylation measurements. Two main metrics are used:

#### Beta-values

Beta-values represent the proportion of methylation at each CpG site, ranging from 0 to 1:

- **Beta = 0:** Completely unmethylated (0% methylation)

- **Beta = 0.5:** Half-methylated (50% methylation)

- **Beta = 1:** Fully methylated (100% methylation)

**Calculation:** Beta = Methylated signal / (Methylated signal + Unmethylated signal)

**When to use Beta-values:**

- Biological interpretation and visualization

- Reporting methylation percentages

- Clinical applications where percentages are meaningful

- Heat maps and scatter plots

#### M-values

M-values represent the log2 ratio of methylated to unmethylated signals:

**Calculation:** M = log2(Methylated signal / Unmethylated signal)

**Properties:**

- Range from -∞ to +∞

- M = 0 corresponds to 50% methylation

- Positive M-values indicate hypermethylation

- Negative M-values indicate hypomethylation

**When to use M-values:**

- Statistical analysis and differential methylation testing

- Linear modeling and regression analysis

- When statistical assumptions require normally distributed data

- Better statistical properties for small effect sizes

> 

**Key Point:** Use M-values for statistical calculations and beta-values for biological interpretation and visualization.

### Additional Probe Filtering

Beyond quality-based filtering, we also remove probes that may give unreliable results due to genetic variation or cross-reactivity:

```r
# Get comprehensive annotation data for all probes
annotation <- getAnnotation(mSet_norm)

# Identify problematic probes that should be removed:

# 1. Cross-reactive probes (may bind to multiple genomic locations)
# 2. Probes affected by SNPs (genetic variants can affect binding)
cross_reactive <- rownames(annotation)[!is.na(annotation$Probe_rs) | 
                                      !is.na(annotation$CpG_rs) | 
                                      !is.na(annotation$SBE_rs)]

# 3. Sex chromosome probes (remove to avoid sex-related confounding)
sex_chr_probes <- rownames(annotation)[annotation$chr %in% c("chrX", "chrY")]

# Combine all problematic probes for removal
problematic_probes <- unique(c(cross_reactive, sex_chr_probes))
keep_probes <- !rownames(mSet_norm) %in% problematic_probes

# Apply filtering
mSet_filtered <- mSet_norm[keep_probes, ]
beta_filtered <- getBeta(mSet_filtered)
m_filtered <- getM(mSet_filtered)

# Ensure we only keep probes with complete data across all samples
complete_probes <- complete.cases(m_filtered) & 
                   apply(m_filtered, 1, function(x) all(is.finite(x)))

m_complete <- m_filtered[complete_probes, ]
beta_complete <- beta_filtered[complete_probes, ]
```

This filtering step removes probes that could provide misleading results, ensuring our downstream analysis focuses on reliable methylation measurements.

### Sample Outlier Detection

Before proceeding to differential analysis, we check for sample outliers that might skew our results:

```r
# Perform Principal Component Analysis (PCA) to identify outliers
# PCA reduces the high-dimensional methylation data to key patterns
pca_result <- prcomp(t(m_complete), center = TRUE, scale. = FALSE)
var_explained <- summary(pca_result)$importance[2, 1:10]

# Create visualization of sample relationships
pdf("plots/sample_qc_pca.pdf", width = 12, height = 8)
par(mfrow = c(2, 2))

# Plot samples in PC space, colored by sample type
plot(pca_result$x[,1], pca_result$x[,2],
     main = "PCA: Sample Clustering by Methylation Patterns",
     xlab = paste0("PC1 (", round(var_explained[1] * 100, 1), "%)"),
     ylab = paste0("PC2 (", round(var_explained[2] * 100, 1), "%)"),
     col = as.factor(sample_info$Sample_Type),
     pch = 16, cex = 1.2)
legend("topright", legend = unique(sample_info$Sample_Type), 
       col = 1:length(unique(sample_info$Sample_Type)), pch = 16, cex = 0.8)

# Create sample correlation heatmap
sample_cors <- cor(beta_filtered, use = "complete.obs")
pheatmap(sample_cors, 
         annotation_col = data.frame(Type = sample_info$Sample_Type, 
                                   row.names = colnames(sample_cors)),
         show_rownames = FALSE, show_colnames = FALSE,
         main = "Sample-to-Sample Correlations")

dev.off()

# Statistical outlier detection using Mahalanobis distance
pc_scores <- pca_result$x[, 1:5]
mahal_dist <- mahalanobis(pc_scores, colMeans(pc_scores), cov(pc_scores))
outlier_threshold <- qchisq(0.975, df = 5)
outliers <- names(mahal_dist)[mahal_dist > outlier_threshold]

if(length(outliers) > 0) {
    cat("Outlier samples detected:", paste(outliers, collapse = ", "), "\n")
    cat("Consider investigating these samples for technical issues\n")
} else {
    cat("No outlier samples detected - data quality looks good\n")
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/epic-pca.png?resize=1024%2C664&ssl=1)

This step helps identify samples that behave very differently from others, which could indicate technical problems or interesting biological variation that needs special consideration.

## Differential Methylation Analysis

Differential methylation analysis identifies CpG sites and regions where methylation levels differ significantly between experimental groups. This is the core statistical analysis that reveals biologically relevant methylation changes.

### Individual CpG Site Analysis (Differentially Methylated Positions – DMPs)

DMPs are individual CpG sites that show statistically significant differences in methylation between groups:

```r
# Define comparison groups for analysis
# For this example, we'll compare LNCAP vs PREC cell lines
sample_info$Comparison_Group <- ifelse(sample_info$Sample_Type %in% c("LNCAP", "PREC"), 
                                      sample_info$Sample_Type, "Other")

# Filter to only include samples in our comparison
comparison_samples <- sample_info$Comparison_Group %in% c("LNCAP", "PREC")
m_comparison <- m_filtered[, comparison_samples]
sample_info_comparison <- sample_info[comparison_samples, ]

# Create design matrix for statistical modeling
# This matrix tells the statistical model which samples belong to which groups
design <- model.matrix(~ 0 + Comparison_Group, data = sample_info_comparison)
colnames(design) <- gsub("Comparison_Group", "", colnames(design))

# Define the specific contrast we want to test
# This specifies exactly which comparison we're making
contrast_matrix <- makeContrasts(
    Group_Diff = paste0(colnames(design)[1], "-", colnames(design)[2]), 
    levels = design
)

# Fit linear models to identify differentially methylated positions
# We use M-values here because they have better statistical properties
fit <- lmFit(m_comparison, design)
fit_contrasts <- contrasts.fit(fit, contrast_matrix)
fit_eb <- eBayes(fit_contrasts)

# Extract results with statistical significance testing
results_all <- topTable(fit_eb, coef = colnames(contrast_matrix), 
                       n = Inf, p.value = 1, lfc = 0, sort.by = "p")

# Add genomic annotation to understand where differences occur
annotation_subset <- annotation[rownames(results_all), ]
results_annotated <- cbind(results_all, annotation_subset)

# Save results for further analysis
write.csv(results_annotated, "results/differential/dmp_results_annotated.csv")
```

This analysis identifies individual CpG sites where methylation differs between your experimental groups. The results include statistical significance, effect sizes, and genomic annotations.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/differential-cpg.png?resize=1024%2C131&ssl=1)

### Understanding Differential Methylated Regions (DMR) Analysis

While DMPs identify individual CpG sites, **Differentially Methylated Regions (DMRs)** identify larger genomic regions where multiple nearby CpG sites show coordinated methylation changes. This approach is often more biologically meaningful because:

#### Why DMR Analysis Matters

**Biological Relevance:** Gene regulation often involves coordinated methylation changes across entire promoter regions, CpG islands, or enhancers rather than single CpG sites.

**Statistical Power:** By combining evidence from multiple nearby CpGs, DMR analysis can detect subtle but consistent changes that might be missed when looking at individual sites.

**Functional Significance:** DMRs are more likely to have functional consequences for gene expression than isolated methylation changes.

**Reduced Multiple Testing Burden:** By testing regions instead of individual sites, we reduce the number of statistical tests and improve our ability to detect true differences.

#### How DMR Analysis Works

DMR analysis typically involves several steps:

1. **Smoothing:** Methylation values at nearby CpG sites are smoothed to identify continuous regions of change

2. **Region Definition:** Genomic regions containing multiple differentially methylated CpGs are identified

3. **Statistical Testing:** Each region is tested for overall significance

4. **Effect Size Calculation:** The magnitude of methylation difference across the region is calculated

```r
# DMRcate analysis for identifying differentially methylated regions
# First, annotate CpGs with statistical information
dmr_annotation <- cpg.annotate(
    object = m_comparison,                    # Use M-values for statistical testing
    datatype = "array",                       # Specify this is array data
    what = "M",                               # Use M-values
    arraytype = "EPICv2",                     # Specify EPIC v2.0 array
    analysis.type = "differential",           # Differential analysis
    design = design,                          # Design matrix
    contrasts = TRUE,                         # Use contrasts
    cont.matrix = contrast_matrix,            # Contrast matrix
    coef = colnames(contrast_matrix)[1],      # Which contrast to test
    fdr = 0.05                                # FDR threshold for significance
)

# Find DMRs using spatial correlation of nearby CpGs
# lambda controls the smoothing bandwidth
# C controls the minimum number of CpGs required per region
dmrs <- dmrcate(dmr_annotation, lambda = 1000, C = 2, pcutoff = 0.05)

# Extract genomic ranges for the identified DMRs
dmr_results <- extractRanges(dmrs, genome = "hg38")

# Convert to a data frame for easier analysis
dmr_df <- as.data.frame(dmr_results)
write.csv(dmr_df, "results/differential/dmr_results.csv")
```

DMR analysis provides a more comprehensive view of methylation changes by identifying coherent regions of differential methylation rather than just individual CpG sites.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/dmr.png?resize=1024%2C161&ssl=1)

### Functional Enrichment Analysis

After identifying differentially methylated CpGs and regions, we want to understand their biological significance through pathway enrichment analysis:

```r
# Gene Ontology (GO) enrichment analysis
# This tests whether genes near differentially methylated CpGs are enriched
# for specific biological processes, molecular functions, or cellular components
go_results <- gometh(
    sig.cpg = rownames(results_all[results_all$adj.P.Val < 0.05, ]),  # Significant CpGs
    all.cpg = rownames(results_all),                                   # Background CpGs
    collection = "GO",                                                 # Gene Ontology
    array.type = "EPIC_V2"                                            # Array type
)

write.csv(go_results, "results/annotation/go_enrichment_results.csv")

# KEGG pathway enrichment analysis
# This tests for enrichment in specific metabolic and signaling pathways
kegg_results <- gometh(
    sig.cpg = rownames(results_all[results_all$adj.P.Val < 0.05, ]),
    all.cpg = rownames(results_all),
    collection = "KEGG",                                              # KEGG pathways
    array.type = "EPIC_V2"
)

write.csv(kegg_results, "results/annotation/kegg_enrichment_results.csv")
```

Functional enrichment analysis helps interpret the biological meaning of your methylation results by identifying which cellular processes and pathways are affected by the observed methylation changes.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/differential-cpg-kegg.png?resize=1024%2C180&ssl=1)

**Pathway Enrichment for Differentially Methylated Regions (DMRs):**

While `gometh()` is specifically designed for individual CpG sites and accounts for the varying number of probes per gene on methylation arrays, analyzing pathway enrichment for DMRs requires a different approach. DMRs often span regulatory regions and gene bodies, and the genes overlapping or near these regions can be extracted for standard pathway enrichment analysis.

To perform pathway enrichment on your DMR results, first extract the genes associated with significant DMRs:

```r
# Extract unique genes overlapping with DMRs
# The overlapping.genes column contains gene symbols for each DMR
dmr_genes <- unique(unlist(strsplit(dmr_df$overlapping.genes, ", ")))

# Remove any NA or empty entries
dmr_genes <- dmr_genes[!is.na(dmr_genes) & dmr_genes != ""]
```

Once you have your list of DMR-associated genes, you can perform Over-Representation Analysis (ORA) to identify enriched pathways. For detailed step-by-step instructions on conducting ORA including GO enrichment, KEGG pathway analysis, and creating publication-ready visualizations, please refer to the **“3. Over-Representation Analysis (ORA)”** section in our comprehensive pathway analysis tutorial: [How to Analyze RNA-seq Data for Absolute Beginners Part 5: From DEGs to Pathways](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/).

The ORA workflow is identical whether you’re analyzing differentially expressed genes from RNA-seq or genes associated with differentially methylated regions—simply provide your DMR gene list as input. This approach will help you understand which biological pathways and processes are affected by the coordinated methylation changes identified in your DMR analysis.

## Best Practices and Troubleshooting Guide

### Essential Best Practices for DNA Methylation Analysis

#### Experimental Design Considerations

**Sample Size Planning:**

- Conduct power analysis before data collection (minimum 6-8 samples per group recommended)

- Consider effect sizes typical for your research area

- Account for multiple testing correction in power calculations

**Batch Effect Prevention:**

- Randomize samples across processing batches and array positions

- Process all samples using identical protocols and reagents

- Record all technical variables (processing date, operator, reagent lots)

- Include appropriate biological and technical controls

**Sample Matching:**

- Match samples on relevant demographic variables (age, sex, ethnicity)

- Control for known confounding factors in your study design

- Consider tissue-specific factors (cell type composition, collection method)

#### Data Quality Standards

**Stringent Quality Control:**

- Use detection p-value thresholds < 0.01 for reliable signal detection

- Remove probes with >10% missing data across samples

- Remove samples with >5% failed probes

- Systematically assess and correct for batch effects using appropriate methods

**Preprocessing Best Practices:**

- Always apply background correction and normalization

- Use functional normalization (FunNorm) for EPIC arrays as the gold standard

- Remove cross-reactive probes and SNP-affected probes

- Filter sex chromosome probes unless sex differences are of interest

#### Statistical Analysis Guidelines

**Appropriate Statistical Methods:**

- Use M-values for statistical testing and beta-values for interpretation

- Apply appropriate multiple testing correction (FDR < 0.05 recommended)

- Include relevant covariates in statistical models (age, sex, cell type proportions)

- Report both statistical significance and biological effect sizes

**Model Selection:**

- Use linear mixed-effects models when dealing with related samples

- Include batch variables as random effects when appropriate

- Consider surrogate variable analysis (SVA) for unknown confounders

- Validate key findings when possible using independent methods

### Common Issues and Troubleshooting Solutions

#### Low Signal Quality Issues

**Problem:** Poor detection p-values or low signal intensities across many probes

**Possible Causes:**

- Poor DNA quality or degradation

- Incomplete bisulfite conversion

- Suboptimal hybridization conditions

- Array storage or handling issues

**Solutions:**

- Check DNA quality metrics (A260/A280 ratio, gel electrophoresis)

- Verify bisulfite conversion efficiency using control probes

- Review sample processing protocols for consistency

- Consider re-processing poor quality samples if DNA is available

- Adjust background correction parameters if technical issues are systematic

#### Batch Effects and Technical Variation

**Problem:** Strong clustering by processing batch rather than biological groups

**Identification Methods:**

- Principal component analysis showing batch-related clustering

- High correlation within batches regardless of biological group

- Surrogate variable analysis detecting batch-related factors

**Solutions:**

- Apply ComBat batch correction for known batch variables

- Use Surrogate Variable Analysis (SVA) for unknown confounders

- Include batch variables as covariates in statistical models

- Re-randomize sample processing if batch effects are severe

- Use functional normalization which inherently corrects many batch effects

- Check my tutorial on [batch effect adjustment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-21-a-comprehensive-guide-to-batch-effects-covariates-adjustment/)

```r
# Example batch effect correction using ComBat
library(sva)

# Identify batch effects
batch_variable <- sample_info$Processing_Date  # or other batch identifier
mod <- model.matrix(~ Sample_Type, data = sample_info)

# Apply ComBat correction
m_combat <- ComBat(dat = m_values, batch = batch_variable, mod = mod)
```

#### Poor Sample Clustering or Unexpected Results

**Problem:** Samples don’t cluster according to expected biological groups

**Diagnostic Steps:**

- Verify sample identities and group assignments using SNP probes

- Check for sample swaps or mislabeling

- Assess tissue heterogeneity and cell type composition

- Review processing protocols for systematic errors

**Solutions:**

- Confirm sample identities using genetic fingerprinting

- Check for contaminated or degraded samples

- Consider cell type deconvolution methods

- Re-examine experimental design and biological hypotheses

- Compare results with published studies from similar systems

#### Statistical Issues and Interpretation Challenges

**Problem:** Very few or too many differentially methylated sites

**Too Few Significant Results:**

- Increase sample size if power is insufficient

- Check for appropriate effect sizes in your system

- Consider less stringent significance thresholds (with appropriate justification)

- Focus on DMR analysis which may have more power than individual CpG analysis

**Too Many Significant Results:**

- Check for batch effects or technical artifacts

- Verify multiple testing correction is appropriate

- Consider more stringent significance thresholds

- Focus on effect sizes, not just p-values

### Advanced Considerations and Special Scenarios

#### Cell Type Heterogeneity

**Challenge:** Complex tissues contain multiple cell types with distinct methylation patterns

**Solutions:**

- Use reference-based deconvolution methods to estimate cell type proportions

- Include estimated cell proportions as covariates in statistical models

- Consider cell-type-specific analyses if pure populations are available

- Interpret results in the context of cellular composition changes

```r
# Example cell type deconvolution
library(FlowSorted.Blood.EPIC)

# Estimate blood cell type proportions
cell_props <- estimateCellCounts(rgSet, compositeCellType = "Blood",
                                processMethod = "preprocessFunnorm")

# Include in statistical model
design_with_cells <- model.matrix(~ Group + CD8T + CD4T + NK + Bcell + Mono + Gran,
                                 data = cbind(sample_info, cell_props))
```

#### Genetic Variation Effects

**Challenge:** Genetic variants can affect probe binding and create false methylation differences

**Prevention Strategies:**

- Filter probes with common SNPs (MAF > 0.01)

- Use population-matched reference data when available

- Consider population stratification in study design

- Account for genetic ancestry in statistical models

**Interpretation Considerations:**

- Consult methylation quantitative trait loci (mQTL) databases

- Distinguish between genetic and epigenetic effects

- Consider gene-environment interactions

#### Age and Sex Effects

**Challenge:** Age and sex have strong effects on DNA methylation patterns

**Best Practices:**

- Always include age as a covariate in statistical models

- Consider sex-stratified analyses when appropriate

- Account for hormonal status in relevant studies

- Use epigenetic age clocks to assess biological vs. chronological age

```r
# Example model including age and sex
design_age_sex <- model.matrix(~ Group + Age + Sex, data = sample_info)
```

## Visualization and Results Interpretation

While this tutorial focuses on the core analytical pipeline, effective visualization is crucial for interpreting methylation results. Key visualization approaches include:

**Volcano Plots:** Display statistical significance vs. effect size for all tested CpGs**Heatmaps:** Visualize methylation patterns across samples and top differentially methylated sites**Manhattan Plots:** Show genome-wide methylation differences across chromosomes**Regional Plots:** Show methylation patterns across specific genomic regions of interest**Pathway Networks:** Visualize functional enrichment results and pathway relationships

## Conclusion and Next Steps

This comprehensive tutorial has provided a complete workflow for analyzing DNA methylation data from the Illumina EPIC v2.0 BeadChip. The methods covered include:

- **Data Import and Quality Control:** Ensuring high-quality, reliable methylation measurements

- **Preprocessing and Normalization:** Correcting technical biases while preserving biological signals

- **Statistical Analysis:** Identifying both individual differentially methylated positions (DMPs) and broader differentially methylated regions (DMRs)

- **Functional Interpretation:** Connecting methylation changes to biological pathways and processes

### Key Takeaways

1. **Quality Control is Critical:** Rigorous filtering of poor-quality probes and samples is essential for reliable results

2. **Choose Appropriate Metrics:** Use M-values for statistical testing and beta-values for biological interpretation

3. **Consider Batch Effects:** Technical variation can obscure biological signals if not properly addressed

4. **Think Regionally:** DMR analysis often provides more biologically meaningful results than individual CpG analysis

5. **Interpret in Context:** Consider cell type composition, genetic variation, and known confounders

## References

1. **Aryee, M.J., et al.** (2014). Minfi: a flexible and comprehensive Bioconductor package for the analysis of Infinium DNA methylation microarrays. *Bioinformatics* 30(10): 1363-1369.

2. **Fortin, J.P., et al.** (2014). Functional normalization of 450k methylation array data improves replication in large cancer studies. *Genome Biology* 15: 503.

3. **Peters, T.J., et al.** (2015). De novo identification of differentially methylated regions in the human genome. *Epigenetics & Chromatin* 8: 6.

4. **Phipson, B., et al.** (2016). missMethyl: an R package for analyzing data from Illumina’s HumanMethylation450 platform. *Bioinformatics* 32(2): 286-288.

5. **Ritchie, M.E., et al.** (2015). limma powers differential expression analyses for RNA-sequencing and microarray studies. *Nucleic Acids Research* 43(7): e47.

6. **Leek, J.T., et al.** (2012). The sva package for removing batch effects and other unwanted variation in high-throughput experiments. *Bioinformatics* 28(6): 882-883.

7. **Salas, L.A., et al.** (2022). An optimized library for reference-based deconvolution of whole-blood biospecimens assayed using the Illumina HumanMethylationEPIC BeadArray. *Genome Biology* 23: 204.

8. **Teschendorff, A.E., et al.** (2013). A beta-mixture quantile normalization method for correcting probe design bias in Illumina Infinium 450 k DNA methylation data. *Bioinformatics* 29(2): 189-196.

9. **Dedeurwaerder S., et al.** Evaluation of the Infinium Methylation 450K technology. Epigenomics. 2011 Dec;3(6):771-84.

---

*This tutorial represents current best practices for DNA methylation analysis using the EPIC v2.0 platform. The field continues to evolve, and practitioners should stay informed about new developments in methodology and interpretation. For questions or updates, consult the latest Bioconductor documentation and methylation analysis literature.*
