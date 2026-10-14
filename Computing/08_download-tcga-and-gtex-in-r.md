# How To Download Gene Expression Data From TCGA and GTEx Using R: A Complete Guide for Cancer Genomics Research

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** August 8, 2026
- **Categories:** Database
- **Source:** [https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/)

---

*A comprehensive step-by-step tutorial for accessing and analyzing large-scale genomic datasets*

Table of Contents

[Toggle](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/)

- [Introduction: Unlocking the Power of Public Cancer Genomics Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Introduction_Unlocking_the_Power_of_Public_Cancer_Genomics_Data)

  - [What Are TCGA and GTEx Databases?](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#What_Are_TCGA_and_GTEx_Databases)
  - [Types of Data Hosted by These Databases](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Types_of_Data_Hosted_by_These_Databases)
  - [Sample Types and Tissue Coverage](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Sample_Types_and_Tissue_Coverage)
  - [Research Applications and Analysis Opportunities](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Research_Applications_and_Analysis_Opportunities)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Setting_Up_Your_Analysis_Environment)

  - [Required Package Installation](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Required_Package_Installation)
  - [Setup Working Directory](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Setup_Working_Directory)

- [Downloading Gene Expression Data from TCGA](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Downloading_Gene_Expression_Data_from_TCGA)

  - [Exploring Available TCGA Datasets](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Exploring_Available_TCGA_Datasets)
  - [Understanding Sample Types](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Understanding_Sample_Types)
  - [Downloading TCGA Gene Expression Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Downloading_TCGA_Gene_Expression_Data)
  - [Examine and Clean TCGA Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Examine_and_Clean_TCGA_Data)
  - [Saving TCGA Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Saving_TCGA_Data)

- [Downloading Gene Expression Data from GTEx](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Downloading_Gene_Expression_Data_from_GTEx)

  - [Exploring Available GTEx Datasets](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Exploring_Available_GTEx_Datasets)
  - [Download GTEx Sample Information](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Download_GTEx_Sample_Information)
  - [Download GTEx Gene Expression Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Download_GTEx_Gene_Expression_Data)
  - [Process and Clean GTEx Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Process_and_Clean_GTEx_Data)
  - [Save GTEx Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Save_GTEx_Data)

- [Performing Differential Expression Analysis Between Tumor and Normal Tissues](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Performing_Differential_Expression_Analysis_Between_Tumor_and_Normal_Tissues)

  - [Prepare Data for Analysis](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Prepare_Data_for_Analysis)
  - [Combine Datasets and Handle Batch Effects](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Combine_Datasets_and_Handle_Batch_Effects)
  - [Perform Differential Expression Analysis](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Perform_Differential_Expression_Analysis)
  - [Create Visualizations](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Create_Visualizations)

- [Understanding Batch Effects and Study Design Caveats](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Understanding_Batch_Effects_and_Study_Design_Caveats)

  - [The Challenge of Cross-Study Analysis](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#The_Challenge_of_Cross-Study_Analysis)
  - [Caveats and Limitations](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Caveats_and_Limitations)
  - [Suggestions for Handling Batch Effects](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Suggestions_for_Handling_Batch_Effects)

- [Alternative Approach: Pre-Processed Unified TCGA and GTEx Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Alternative_Approach_Pre-Processed_Unified_TCGA_and_GTEx_Data)

  - [Why Consider Unified Data?](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Why_Consider_Unified_Data)
  - [The Unified Dataset](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#The_Unified_Dataset)
  - [Downloading the Unified Data](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Downloading_the_Unified_Data)

- [Conclusion: Best Practices for TCGA and GTEx Analysis](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Conclusion_Best_Practices_for_TCGA_and_GTEx_Analysis)

  - [What We’ve Accomplished](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#What_Weve_Accomplished)
  - [Key Best Practices](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Key_Best_Practices)
  - [Extending This Analysis](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Extending_This_Analysis)
  - [Final Recommendations](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Final_Recommendations)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Troubleshooting_Common_Issues)

  - [Download Problems](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Download_Problems)
  - [Memory Issues](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Memory_Issues)
  - [Analysis Issues](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Analysis_Issues)

- [References and Further Reading](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#References_and_Further_Reading)

  - [Share this:](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Share_this)
  - [Like this:](https://ngs101.com/how-to-download-gene-expression-data-from-tcga-and-gtex-using-r-a-complete-guide-for-cancer-genomics-research/#Like_this)

## Introduction: Unlocking the Power of Public Cancer Genomics Data

Cancer research has been revolutionized by the availability of large-scale, high-quality genomic datasets. Two of the most valuable resources for researchers worldwide are The Cancer Genome Atlas (TCGA) and the Genotype-Tissue Expression (GTEx) project. This tutorial will guide you through the process of downloading and analyzing gene expression data from these databases using R, opening doors to powerful comparative analyses between cancer and normal tissues.

### What Are TCGA and GTEx Databases?

**The Cancer Genome Atlas (TCGA)** represents one of the most ambitious cancer genomics projects ever undertaken. Launched by the National Cancer Institute and the National Human Genome Research Institute, TCGA has systematically characterized the molecular alterations across more than 30 different cancer types. The project has generated comprehensive genomic profiles from over 11,000 tumor samples, creating an unprecedented resource for understanding cancer biology.

**The Genotype-Tissue Expression (GTEx) project** complements TCGA by providing a comprehensive atlas of gene expression and regulation across diverse human tissues and individuals. GTEx has collected samples from 54 non-diseased tissue sites across nearly 1,000 individuals, establishing the largest resource to date for studying human gene expression variation across tissues and individuals.

> 

**Why These Databases Matter:** Together, TCGA and GTEx provide researchers with the ability to compare cancer samples against appropriate normal tissue controls, enabling powerful differential expression analyses that can reveal cancer-specific molecular signatures.

### Types of Data Hosted by These Databases

Both databases host multiple types of molecular data:

**TCGA Data Types:**

- **RNA Sequencing (RNA-seq):** Gene expression quantification and transcript-level data

- **Whole Exome/Genome Sequencing:** Mutation profiles and structural variations

- **DNA Methylation:** Epigenetic modifications across the genome

- **Copy Number Variation:** Chromosomal gains and losses

- **MicroRNA Sequencing:** Small RNA expression profiles

- **Clinical Data:** Patient demographics, treatment history, and outcomes

**GTEx Data Types:**

- **RNA Sequencing:** Gene and transcript expression across tissues

- **Whole Genome Sequencing:** Genetic variants and their associations with expression

- **Histology Images:** Tissue morphology for quality assessment

- **Quantitative Trait Loci (eQTL) Data:** Genetic variants affecting gene expression

### Sample Types and Tissue Coverage

**TCGA Sample Categories:**

- **Primary Tumor:** Fresh tumor tissue from the primary cancer site

- **Solid Tissue Normal:** Adjacent normal tissue from the same patient

- **Blood Derived Normal:** Normal blood samples for germline comparisons

- **Recurrent Tumor:** Tumor samples collected after initial treatment

- **Metastatic:** Secondary tumor sites in advanced disease cases

**GTEx Tissue Types:**The GTEx project encompasses 54 tissue types across major organ systems:

- **Brain tissues:** 13 different brain regions

- **Cardiovascular:** Heart, arteries, and related tissues

- **Digestive system:** Colon, stomach, liver, pancreas, and others

- **Endocrine:** Thyroid, adrenal gland, pituitary

- **Reproductive:** Ovary, testis, prostate, breast, uterus

- **Muscle and connective tissue:** Skeletal muscle, skin, fat

- **Blood and immune:** Whole blood, spleen, lymph nodes

### Research Applications and Analysis Opportunities

The combination of TCGA and GTEx data enables several powerful research approaches:

**Differential Expression Analysis:**

- Compare tumor vs. normal tissue expression patterns

- Identify cancer-specific biomarkers and therapeutic targets

- Understand tissue-specific expression differences

**Pathway and Functional Analysis:**

- Discover disrupted biological pathways in cancer

- Identify co-expressed gene modules and regulatory networks

- Characterize immune infiltration patterns in tumors

**Survival and Clinical Association Studies:**

- Correlate gene expression with patient outcomes

- Identify prognostic signatures for risk stratification

- Discover predictive biomarkers for treatment response

**Multi-omics Integration:**

- Combine expression data with mutation, methylation, and copy number data

- Understand the molecular mechanisms driving cancer development

- Identify druggable targets and resistance mechanisms

**Pan-cancer Analyses:**

- Compare molecular patterns across different cancer types

- Identify common and unique features of various cancers

- Discover universal cancer signatures and tissue-specific alterations

## Setting Up Your Analysis Environment

Before diving into data download and analysis, we need to set up our R environment with the necessary packages. If you’re new to R environment setup, I recommend checking out my [previous tutorial on RNA-seq analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/) which covers the basics in detail.

### Required Package Installation

We’ll install packages from both CRAN and Bioconductor. For beginners, here’s what each package does:

```r
#-----------------------------------------------
# STEP 1: Install required packages for TCGA/GTEx analysis
#-----------------------------------------------

# Install BiocManager (manages bioinformatics packages)
if (!require("BiocManager", quietly = TRUE)) {
    install.packages("BiocManager")
}

# Separate packages by source for proper installation
cran_packages <- c(
    "data.table",          # Fast data manipulation
    "ggplot2"              # Making plots
)

bioconductor_packages <- c(
    # Data access
    "TCGAbiolinks",        # Downloads TCGA data
    "recount3",            # Downloads GTEx data  
    "SummarizedExperiment", # Handles genomic data format

    # Analysis packages
    "limma",               # Differential expression analysis
    "edgeR",               # RNA-seq analysis tools
    "sva",                 # Batch effect correction

    # Visualization
    "pheatmap",            # Heatmaps
    "EnhancedVolcano",     # Volcano plots

    # Gene annotation
    "org.Hs.eg.db"        # Human gene information
)

# Install CRAN packages first
install.packages(cran_packages)

# Install Bioconductor packages
BiocManager::install(bioconductor_packages, ask = FALSE, update = FALSE)

# Load the packages we'll use
library(TCGAbiolinks)
library(recount3)
library(SummarizedExperiment)
library(data.table)
library(limma)
library(edgeR)
```

### Setup Working Directory

Let’s create a clean workspace for our analysis:

```r
#-----------------------------------------------
# STEP 2: Create project directory structure
#-----------------------------------------------

# Create main project folder
project_dir <- "~/TCGA_GTEx_Analysis"
dir.create(project_dir, recursive = TRUE, showWarnings = FALSE)

# Create subfolders
dir.create(file.path(project_dir, "data"), showWarnings = FALSE)
dir.create(file.path(project_dir, "results"), showWarnings = FALSE)
dir.create(file.path(project_dir, "plots"), showWarnings = FALSE)

# Set working directory
setwd(project_dir)
```

## Downloading Gene Expression Data from TCGA

The Cancer Genome Atlas provides a wealth of cancer genomics data. We’ll use the `TCGAbiolinks` package to access and download TCGA data systematically.

### Exploring Available TCGA Datasets

Let’s see what cancer types are available in TCGA:

```r
#-----------------------------------------------
# STEP 3: Explore available TCGA datasets
#-----------------------------------------------

# Get all TCGA cancer projects
all_projects <- getGDCprojects()

# Filter for TCGA projects (they start with "TCGA-")
tcga_projects <- all_projects[grepl("^TCGA-", all_projects$project_id), ]

# Show available cancer types
print(tcga_projects[, c("project_id", "name")])

# For this tutorial, we'll use Breast Cancer (TCGA-BRCA)

#    project_id                                                             name
# 5   TCGA-BRCA                                        Breast Invasive Carcinoma
# 6   TCGA-UCEC                             Uterine Corpus Endometrial Carcinoma
# 42  TCGA-LAML                                           Acute Myeloid Leukemia
# 44  TCGA-CHOL                                               Cholangiocarcinoma
# 50   TCGA-ACC                                         Adrenocortical Carcinoma
# 51  TCGA-DLBC                  Lymphoid Neoplasm Diffuse Large B-cell Lymphoma
# 52  TCGA-CESC Cervical Squamous Cell Carcinoma and Endocervical Adenocarcinoma
# 53  TCGA-ESCA                                             Esophageal Carcinoma
# 58  TCGA-KICH                                               Kidney Chromophobe
# 60  TCGA-HNSC                            Head and Neck Squamous Cell Carcinoma
# 61  TCGA-COAD                                             Colon Adenocarcinoma
# 62  TCGA-BLCA                                     Bladder Urothelial Carcinoma
# 64  TCGA-KIRP                            Kidney Renal Papillary Cell Carcinoma
# 65   TCGA-GBM                                          Glioblastoma Multiforme
# 66  TCGA-MESO                                                     Mesothelioma
# 67  TCGA-SARC                                                          Sarcoma
# 68  TCGA-PCPG                               Pheochromocytoma and Paraganglioma
# 69  TCGA-READ                                            Rectum Adenocarcinoma
# 70  TCGA-LUAD                                              Lung Adenocarcinoma
# 71  TCGA-LUSC                                     Lung Squamous Cell Carcinoma
# 72  TCGA-PAAD                                        Pancreatic Adenocarcinoma
# 73  TCGA-LIHC                                   Liver Hepatocellular Carcinoma
# 74  TCGA-KIRC                                Kidney Renal Clear Cell Carcinoma
# 75   TCGA-LGG                                         Brain Lower Grade Glioma
# 76  TCGA-SKCM                                          Skin Cutaneous Melanoma
# 77  TCGA-PRAD                                          Prostate Adenocarcinoma
# 78  TCGA-STAD                                           Stomach Adenocarcinoma
# 79    TCGA-OV                                Ovarian Serous Cystadenocarcinoma
# 80  TCGA-THYM                                                          Thymoma
# 81   TCGA-UCS                                           Uterine Carcinosarcoma
# 82   TCGA-UVM                                                   Uveal Melanoma
# 83  TCGA-TGCT                                      Testicular Germ Cell Tumors
# 85  TCGA-THCA                                                Thyroid Carcinoma
```

### Understanding Sample Types

Before downloading, let’s see what sample types are available:

```r
#-----------------------------------------------
# STEP 4: Check available samples for TCGA-BRCA
#-----------------------------------------------

# Query what data is available for breast cancer
brca_query <- GDCquery(
    project = "TCGA-BRCA",                    # Breast cancer project
    data.category = "Transcriptome Profiling", # RNA-seq data
    data.type = "Gene Expression Quantification",
    workflow.type = "STAR - Counts"           # Count data (not FPKM)
)

# Get sample information
sample_info <- getResults(brca_query)

# Get cancer subtypes
subtype_info <- TCGAquery_subtype(tumor = "BRCA")

table(subtype_info$BRCA_Subtype_PAM50)

 # Basal   Her2   LumA   LumB     NA Normal 
 #   192     82    562    209      2     40 

# Available subtype information for cancers
# "ACC", "BRCA", "BLCA", "CESC", "CHOL", "COAD", "ESCA", "GBM"
# "HNSC", "KICH", "KIRC", "KIRP", "LGG", "LIHC", "LUAD", "LUSC"
# "PAAD", "PCPG", "PRAD", "READ", "SKCM", "SARC", "STAD", "THCA"
# "UCEC", "UCS", "UVM"

# Check sample types available
sample_types <- table(sample_info$sample_type)
print(sample_types)

#          Metastatic    7
#       Primary Tumor 1111
# Solid Tissue Normal  113
```

### Downloading TCGA Gene Expression Data

Now let’s download the actual data. We’ll focus on tumor and normal samples:

```r
#-----------------------------------------------
# STEP 5: Download TCGA breast cancer data
#-----------------------------------------------

# We want both tumor and normal samples
target_samples <- c("Primary Tumor", "Solid Tissue Normal")

# Create focused query
brca_query_filtered <- GDCquery(
    project = "TCGA-BRCA",
    data.category = "Transcriptome Profiling",
    data.type = "Gene Expression Quantification", 
    workflow.type = "STAR - Counts",
    sample.type = target_samples
)

# Check how many samples we'll download
filtered_samples <- getResults(brca_query_filtered)
print(table(filtered_samples$sample_type))

# Download the data (this takes several minutes)
GDCdownload(brca_query_filtered, directory = "data/TCGA_BRCA")

# Process the downloaded data into R format
tcga_data <- GDCprepare(
    query = brca_query_filtered,
    directory = "data/TCGA_BRCA"
)

cat(paste("Downloaded", ncol(tcga_data), "samples with", nrow(tcga_data), "genes\n"))

# Downloaded 1224 samples with 60660 genes
```

### Examine and Clean TCGA Data

Let’s look at our data structure and clean it up:

```r
#-----------------------------------------------
# STEP 6: Clean and examine TCGA data
#-----------------------------------------------

# Get the expression matrix (genes x samples)
tcga_expression <- assay(tcga_data, "unstranded")  # Raw counts
tcga_samples <- colData(tcga_data)                 # Sample information
tcga_genes <- rowData(tcga_data)                   # Gene information

# Check sample distribution
sample_dist <- table(tcga_samples$sample_type)
print(sample_dist)

#       Primary Tumor 1111
# Solid Tissue Normal  113

# Keep only protein-coding genes (reduces noise)
if ("gene_type" %in% colnames(tcga_genes)) {
    protein_coding <- tcga_genes$gene_type == "protein_coding"
    tcga_data_clean <- tcga_data[protein_coding, ]
} else {
    tcga_data_clean <- tcga_data
}

# Save processed data
saveRDS(tcga_data_clean, "data/tcga_brca_processed.rds")
```

### Saving TCGA Data

Let’s save our processed data for future use:

```r
#-----------------------------------------------
# STEP 7: Save TCGA data in multiple formats
#-----------------------------------------------

# Also save as text files for use in other programs
tcga_expression_clean <- assay(tcga_data_clean, "unstranded")
tcga_samples_clean <- as.data.frame(colData(tcga_data_clean))
tcga_genes_clean <- as.data.frame(rowData(tcga_data_clean))

# Save expression matrix with gene names
expression_table <- data.table(
    gene_id = rownames(tcga_expression_clean),
    gene_name = tcga_genes_clean$gene_name,
    tcga_expression_clean
)
fwrite(expression_table, "data/tcga_brca_expression.tsv", sep = "\t")

# Save sample information
tcga_samples_clean$sample_id <- rownames(tcga_samples_clean)

# Convert list columns to character (more concise approach)
tcga_samples_clean[, sapply(tcga_samples_clean, is.list)] <- lapply(tcga_samples_clean[, sapply(tcga_samples_clean, is.list)], as.character)

fwrite(tcga_samples_clean, "data/tcga_brca_samples.tsv", sep = "\t")
```

## Downloading Gene Expression Data from GTEx

The GTEx project provides normal tissue gene expression data that serves as an excellent control for cancer studies. We’ll use the `recount3` package to access GTEx data.

### Exploring Available GTEx Datasets

GTEx provides normal tissue expression data. Let’s see what’s available:

```r
#-----------------------------------------------
# STEP 8: Explore available GTEx datasets
#-----------------------------------------------

# Check what projects are available in recount3
available_data <- available_projects()

# Look for GTEx data
gtex_data <- available_data[available_data$file_source == "gtex", ]
print(gtex_data[, c("project", "organism", "file_source")])

#         project organism file_source
#  ADIPOSE_TISSUE    human        gtex
#          MUSCLE    human        gtex
#    BLOOD_VESSEL    human        gtex
#           HEART    human        gtex
#           OVARY    human        gtex
#          UTERUS    human        gtex
#          VAGINA    human        gtex
#          BREAST    human        gtex
```

### Download GTEx Sample Information

Let’s first get the sample information to see what tissues are available:

```r
#-----------------------------------------------
# STEP 9: Get GTEx sample information
#-----------------------------------------------

# Create GTEx data object (this is lightweight - just gets metadata)
gtex_rse <- create_rse_manual(
    project = "BREAST",
    project_home = "data_sources/gtex", 
    organism = "human",
    annotation = "gencode_v26",
    type = "gene"
)

# Get sample information
gtex_samples <- colData(gtex_rse)

# See what tissue types are available
table(gtex_samples$gtex.smtsd)

# Breast - Mammary Tissue   482
```

### Download GTEx Gene Expression Data

Now let’s download the actual expression data:

```r
#-----------------------------------------------
# STEP 10: Download GTEx expression data
#-----------------------------------------------

# Get the actual expression data (this triggers download)
gtex_expression <- assay(gtex_rse, "raw_counts")
gtex_samples_filtered <- colData(gtex_rse)
gtex_genes <- rowData(gtex_rse)
```

### Process and Clean GTEx Data

Let’s clean the GTEx data to match our TCGA processing:

```r
#-----------------------------------------------
# STEP 11: Process GTEx data
#-----------------------------------------------

# Filter for protein-coding genes (to match TCGA)
gtex_gene_df <- as.data.frame(gtex_genes)
if ("gene_type" %in% colnames(gtex_gene_df)) {
    protein_coding <- gtex_gene_df$gene_type == "protein_coding"
    gtex_clean <- gtex_rse[protein_coding, ]
    cat(paste("Kept", sum(protein_coding), "protein-coding genes\n"))
} else {
    gtex_clean <- gtex_rse
}

# Focus on breast tissue samples for comparison with TCGA
breast_mask <- gtex_clean$gtex.smtsd == "Breast - Mammary Tissue"
gtex_breast <- gtex_clean[, breast_mask]

# Save processed GTEx data
saveRDS(gtex_clean, "data/gtex_processed.rds")
saveRDS(gtex_breast, "data/gtex_breast.rds")
```

### Save GTEx Data

Let’s save the processed GTEx data:

```r
#-----------------------------------------------
# STEP 12: Save GTEx data
#-----------------------------------------------

# Get clean data for saving
gtex_expression_clean <- assay(gtex_clean, "raw_counts")  # Use raw_counts assay
gtex_samples_clean <- as.data.frame(colData(gtex_clean))
gtex_genes_clean <- as.data.frame(rowData(gtex_clean))

# Save expression matrix with gene information
gtex_table <- data.table(
    gene_id = rownames(gtex_expression_clean),
    gene_name = gtex_genes_clean$gene_name,
    gtex_expression_clean
)
fwrite(gtex_table, "data/gtex_breast_expression.tsv", sep = "\t")

# Save sample information (handle list columns)
gtex_samples_clean$sample_id <- rownames(gtex_samples_clean)
gtex_samples_clean[, sapply(gtex_samples_clean, is.list)] <- 
    lapply(gtex_samples_clean[, sapply(gtex_samples_clean, is.list)], as.character)
fwrite(gtex_samples_clean, "data/gtex_breast_samples.tsv", sep = "\t")
```

## Performing Differential Expression Analysis Between Tumor and Normal Tissues

Now we’ll combine TCGA tumor data with GTEx normal tissue data to find genes that are different between cancer and normal breast tissue.

### Prepare Data for Analysis

First, let’s load and prepare our data:

```r
#-----------------------------------------------
# STEP 13: Prepare data for differential expression analysis
#-----------------------------------------------

# Load our processed data
tcga_data <- readRDS("data/tcga_brca_processed.rds")
gtex_breast <- readRDS("data/gtex_breast.rds")

# Get expression matrices
tcga_expression <- assay(tcga_data, "unstranded")  # TCGA counts
gtex_expression <- assay(gtex_breast, "raw_counts")    # GTEx counts

# Remove genes that are not expressed in both datasets
tcga_expression <- tcga_expression[rowSums(tcga_expression > 0) >= ncol(tcga_expression), ]

gtex_expression <- gtex_expression[rowSums(gtex_expression > 0) >= ncol(gtex_expression), ]

# Get sample information
tcga_samples <- colData(tcga_data)
gtex_samples <- colData(gtex_breast)

# Get gene information
tcga_gene <- rowData(tcga_data)[rownames(tcga_expression), ]
gtex_gene <- rowData(gtex_breast)[rownames(gtex_expression), ]

rownames(tcga_gene) <- gsub("\\.\\d+$", "", rownames(tcga_gene))
rownames(gtex_gene) <- gsub("\\.\\d+$", "", rownames(gtex_gene))

# Find common genes between datasets
common_genes <- intersect(rownames(tcga_gene), rownames(gtex_gene))

tcga_gene_common <- tcga_gene[common_genes, ]
gtex_gene_common <- gtex_gene[common_genes, ]

# Keep only common genes
rownames(tcga_expression) <- gsub("\\.\\d+$", "", rownames(tcga_expression))
rownames(gtex_expression) <- gsub("\\.\\d+$", "", rownames(gtex_expression))

tcga_expression <- tcga_expression[rownames(tcga_gene_common), ]
gtex_expression <- gtex_expression[rownames(gtex_gene_common), ]

# Focus on TCGA tumor samples only
tumor_samples <- tcga_samples$sample_type == "Primary Tumor"
tcga_tumor_expression <- tcga_expression[, tumor_samples]
```

### Combine Datasets and Handle Batch Effects

This is the tricky part – combining data from different studies:

```r
#-----------------------------------------------
# STEP 14: Combine datasets and address batch effects
#-----------------------------------------------

# Combine expression matrices
combined_expression <- cbind(tcga_tumor_expression, gtex_expression)

# Create combined sample information
# Use data.table for efficient data manipulation
tcga_info <- data.table(
    sample_id = colnames(tcga_tumor_expression),
    dataset = "TCGA",
    condition = "Tumor"
)

gtex_info <- data.table(
    sample_id = colnames(gtex_expression),
    dataset = "GTEx", 
    condition = "Normal"
)

combined_samples <- rbind(tcga_info, gtex_info)

print(table(combined_samples$dataset, combined_samples$condition))

#       Normal Tumor
#  GTEx    482     0
#  TCGA      0  1111

# Filter out very low expressed genes
# Keep genes with at least 10 counts in at least 10% of samples
min_samples <- round(0.1 * ncol(combined_expression))
keep_genes <- rowSums(combined_expression > 10) >= min_samples
filtered_expression <- combined_expression[keep_genes, ]
```

> 

**Important Note About Batch Effects:** When combining TCGA and GTEx data, we face a major challenge called “confounding.” All tumor samples come from TCGA and all normal samples come from GTEx. This means we can’t tell if differences are due to cancer or just differences between the two studies. We’ll proceed with caution and focus on large effect sizes.

### Perform Differential Expression Analysis

Now let’s find genes that are different between tumor and normal:

```r
#-----------------------------------------------
# STEP 15: Differential expression analysis with limma
#-----------------------------------------------

# Load required packages
library(limma)
library(edgeR)

# Create DGEList object for normalization
dge <- DGEList(counts = filtered_expression)

# Add gene symbols to the DGEList object
dge$genes <- data.frame(
    gene_id = rownames(filtered_expression),
    gene_name = tcga_gene_common$gene_name,
    gene_type = tcga_gene_common$gene_type
)

# Add sample group information
dge$samples$condition <- factor(combined_samples$condition)
dge$samples$dataset <- factor(combined_samples$dataset)

# Calculate normalization factors (accounts for library size differences)
dge <- calcNormFactors(dge, method = "TMM")

# Create design matrix
# Include both condition (tumor vs normal) and dataset (TCGA vs GTEx)
design <- model.matrix(~ condition, data = combined_samples)
colnames(design) <- c("Intercept", "Tumor_vs_Normal")

# Apply voom transformation (converts counts to log2-cpm with weights)
v <- voom(dge, design, plot = FALSE)

# Fit linear model
fit <- lmFit(v, design)
fit <- eBayes(fit)  # Apply empirical Bayes smoothing

# Extract results for tumor vs normal comparison
results <- topTable(
    fit,
    coef = "Tumor_vs_Normal",  # This coefficient represents tumor vs normal
    number = Inf,              # Get all genes
    adjust.method = "BH"       # Multiple testing correction
)

fwrite(results, "DEGs_Tumor_vs_Normal.tsv", sep = "\t")

#         gene_id       gene_name      gene_type     logFC P.Value adj.P.Val
# ENSG00000254996 ANKHD1-EIF4EBP3 protein_coding -7.739631       0         0
# ENSG00000166337           TAF10 protein_coding -6.742676       0         0
# ENSG00000255508      AP002990.1 protein_coding -7.434083       0         0
# ENSG00000167995           BEST1 protein_coding -8.792405       0         0
# ENSG00000131503          ANKHD1 protein_coding -6.586050       0         0
# ENSG00000243725            TTC4 protein_coding -5.127006       0         0
```

### Create Visualizations

Let’s make some plots to visualize our results:

```r
#-----------------------------------------------
# STEP 16: Create visualization plots
#-----------------------------------------------

# Load plotting library
library(ggplot2)

# 1. Volcano plot
volcano_data <- data.table(
    gene_name = results$gene_name,
    logFC = results$logFC,
    neg_log10_pval = -log10(results$adj.P.Val),
    significant = results$adj.P.Val < 0.05 & abs(results$logFC) > 1
)

volcano_plot <- ggplot(volcano_data, aes(x = logFC, y = neg_log10_pval)) +
    geom_point(aes(color = significant), alpha = 0.6, size = 1) +
    scale_color_manual(values = c("FALSE" = "gray", "TRUE" = "red")) +
    geom_vline(xintercept = c(-1, 1), linetype = "dashed", alpha = 0.5) +
    geom_hline(yintercept = -log10(0.05), linetype = "dashed", alpha = 0.5) +
    labs(
        title = "Volcano Plot: Tumor vs Normal Breast Tissue",
        x = "Log2 Fold Change (Tumor vs Normal)",
        y = "-Log10(Adjusted P-value)",
        color = "Significant"
    ) +
    theme_minimal()

ggsave("plots/volcano_plot.png", volcano_plot, width = 8, height = 6, dpi = 300)

# 2. MA plot (shows relationship between expression level and fold change)
ma_plot <- ggplot(results, aes(x = AveExpr, y = logFC)) +
    geom_point(aes(color = adj.P.Val < 0.05), alpha = 0.6, size = 0.8) +
    scale_color_manual(values = c("FALSE" = "gray", "TRUE" = "red")) +
    geom_hline(yintercept = c(-1, 0, 1), linetype = c("dashed", "solid", "dashed")) +
    labs(
        title = "MA Plot: Average Expression vs Fold Change",
        x = "Average Expression (log2 CPM)",
        y = "Log2 Fold Change",
        color = "FDR < 0.05"
    ) +
    theme_minimal()

ggsave("plots/ma_plot.png", ma_plot, width = 8, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/06/Volcano-MA-Plot-e1749820789589-1024x391.png?resize=1024%2C391&ssl=1)

## Understanding Batch Effects and Study Design Caveats

### The Challenge of Cross-Study Analysis

When we combine TCGA and GTEx data, we face a serious challenge called **confounding**:

- **All tumor samples** come from TCGA

- **All normal samples** come from GTEx

- We can’t separate true biological differences from technical differences between studies

This means some of our “cancer genes” might actually be “TCGA vs GTEx” genes!

### Caveats and Limitations

```r
#-----------------------------------------------
# STEP 17: Assess potential batch effects
#-----------------------------------------------

# Let's check some "housekeeping" genes that shouldn't change much
housekeeping_genes <- c("ACTB", "GAPDH", "RPL13A", "SDHA")

# Find these genes in our results
hk_results <- results[results$gene_name %in% housekeeping_genes, ]

cat("Housekeeping gene results (should have small changes):\n")
print(hk_results[, c("gene_name", "logFC", "adj.P.Val")])

# If housekeeping genes show large changes, this suggests batch effects
large_hk_changes <- abs(hk_results$logFC) > 0.5
if (any(large_hk_changes)) {
    cat("⚠️  Warning: Some housekeeping genes show large changes\n")
    cat("This suggests possible batch effects between TCGA and GTEx\n")
} else {
    cat("✓ Housekeeping genes look stable\n")
}

#                 gene_name      logFC     adj.P.Val
# ENSG00000073578      SDHA -4.0925807  0.000000e+00
# ENSG00000142541    RPL13A  1.3622276 1.155829e-152
# ENSG00000075624      ACTB  0.4329943  8.901307e-35
# ENSG00000111640     GAPDH  0.4409455  3.127211e-18

# ⚠️  Warning: Some housekeeping genes show large changes
# This suggests possible batch effects between TCGA and GTEx
```

### Suggestions for Handling Batch Effects

**1. Use Conservative Thresholds:**

```r
# Focus on genes with very large effect sizes and high significance
high_confidence <- results[
    abs(results$logFC) > 2 &           # Large effect size
    results$adj.P.Val < 0.001 &       # Very significant
    results$AveExpr > 5,              # Well-expressed genes
]

cat(paste("High-confidence DE genes:", nrow(high_confidence), "\n"))

# High-confidence DE genes: 334 
```

**2. Within-Study Analysis (When Possible):**

```r
# Check if TCGA has normal samples for within-study comparison
tcga_normal_count <- sum(tcga_samples$sample_type == "Solid Tissue Normal")
cat(paste("TCGA normal samples available:", tcga_normal_count, "\n"))

if (tcga_normal_count > 10) {
    cat("Recommendation: Use TCGA tumor vs TCGA normal for more reliable results\n")
}

# TCGA normal samples available: 113 
# Recommendation: Use TCGA tumor vs TCGA normal for more reliable results
```

**3. Literature Validation:**

```r
# Check if our top genes are known cancer genes
known_cancer_genes <- c("MYC", "TP53", "BRCA1", "ERBB2", "ESR1")
cancer_gene_hits <- high_confidence$gene_name %in% known_cancer_genes

cat("Known cancer genes in high-confidence results:\n")
print(high_confidence[cancer_gene_hits, c("gene_name", "logFC", "adj.P.Val")])

#                 gene_name    logFC     adj.P.Val
# ENSG00000141736     ERBB2 2.086971 6.695777e-110
# ENSG00000091831      ESR1 2.238715  1.308510e-42
```

## Alternative Approach: Pre-Processed Unified TCGA and GTEx Data

### Why Consider Unified Data?

While downloading raw TCGA and GTEx data provides maximum flexibility, direct comparison between these datasets poses significant challenges due to differences in sample processing, sequencing platforms, and analysis pipelines. The RNA-seq expression levels typically range from 4-10 (log2 of normalized_count) for TCGA and 0-4 (log2 of RPKM) for GTEx, making direct comparisons problematic.

Wang et al. (2018) developed a comprehensive pipeline that processes and unifies RNA-seq data from TCGA and GTEx studies, including uniform realignment, gene expression quantification, and batch effect removal. Their analysis demonstrated that uniform alignment and quantification alone is insufficient – explicit batch effect correction is essential to facilitate meaningful data comparison between cancer and normal samples.

### The Unified Dataset

The researchers processed data from 10,366 samples, including 2,790 from GTEx and 7,576 from TCGA, resulting in 9,109 high-quality samples after quality control and degradation filtering. This unified dataset covers tissues studied by both projects and provides three different data formats:

### Downloading the Unified Data

The processed data is available on Figshare in three formats:

- Option 1: [Expected Count Data](https://figshare.com/articles/dataset/Data_record_1/5330539) (Recommended for Differential Expression)

- Option 2: [Unnormalized FPKM Data](https://figshare.com/articles/dataset/Data_record_2/5330575)

- Option 3: [Batch-Corrected Normalized Data](https://figshare.com/articles/dataset/Data_record_3/5330593) (Recommended for Cross-Study Comparisons)

## Conclusion: Best Practices for TCGA and GTEx Analysis

This tutorial has shown you how to download and analyze gene expression data from TCGA and GTEx databases. Here are the key takeaways:

### What We’ve Accomplished

1. **Downloaded real cancer genomics data** from public databases

2. **Processed and cleaned** the data for analysis

3. **Identified genes** that are different between cancer and normal tissue

4. **Created visualizations** to understand the results

5. **Addressed important limitations** of cross-study analysis

### Key Best Practices

**Data Quality:**

- Always filter low-expressed genes

- Use appropriate normalization methods

- Check for batch effects

**Statistical Analysis:**

- Use proper multiple testing correction

- Focus on large effect sizes for cross-study comparisons

- Validate results when possible

**Interpretation:**

- Be cautious with cross-study comparisons

- Focus on biological relevance

- Consider known cancer biology

### Extending This Analysis

You can extend this analysis in several ways:

```bash
# Example extensions:
# 1. Analyze multiple cancer types
# 2. Include clinical data for survival analysis  
# 3. Perform pathway enrichment analysis
# 4. Compare with published gene signatures
```

### Final Recommendations

1. **Start with well-characterized datasets** like breast cancer

2. **Validate findings** with literature and other datasets

3. **Be transparent** about limitations in your analysis

4. **Consider experimental validation** for important findings

The power of public genomics databases like TCGA and GTEx lies in their ability to generate hypotheses for further testing. By following the principles in this tutorial, you’ll be well-equipped to explore cancer genomics data responsibly and effectively.

For more detailed information about RNA-seq analysis best practices, check out my [previous tutorial on differential expression analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/).

## Troubleshooting Common Issues

### Download Problems

- **Slow downloads:** Try different times of day when servers are less busy

- **Failed downloads:** Check internet connection and try restarting R

- **Package errors:** Update R and Bioconductor packages

### Memory Issues

```bash
# If R runs out of memory:
# 1. Close other programs
# 2. Increase memory limit (Windows)
memory.limit(size = 8000)  # 8GB

# 3. Work with smaller subsets
# subset_data &lt;- data[1:1000, ]  # First 1000 genes only
```

### Analysis Issues

- **No significant genes:** Try less stringent p-value cutoffs

- **Too many significant genes:** Check for batch effects

- **Weird results:** Verify sample labels and data processing steps

Remember: Real data analysis often involves troubleshooting. Don’t get discouraged if things don’t work perfectly the first time!

## References and Further Reading

1. **The Cancer Genome Atlas Research Network.** The Cancer Genome Atlas Pan-Cancer analysis project. *Nature Genetics*, 2013.

2. **GTEx Consortium.** The GTEx Consortium atlas of genetic regulatory effects across human tissues. *Science*, 2020.

3. **Colaprico, A., et al.** TCGAbiolinks: an R/Bioconductor package for integrative analysis of TCGA data. *Nucleic Acids Research*, 2016.

4. **GTEx Consortium.** Genetic effects on gene expression across human tissues. Nature 550, 204–213 (2017). https://doi.org/10.1038/nature24277

5. Wang, Q., Armenia, J., Zhang, C. *et al.* Unifying cancer and normal RNA sequencing data from different sources. *Sci Data* **5**, 180061 (2018). https://doi.org/10.1038/sdata.2018.61

---

*This tutorial is part of the NGS101.com beginner’s guide to genomics data analysis. For questions or suggestions, leave a comment below.*
