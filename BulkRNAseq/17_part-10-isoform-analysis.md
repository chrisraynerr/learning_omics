# How to Analyze RNAseq Data for Absolute Beginners Part 10: Isoform Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/)

---

If you’ve followed our previous tutorials on RNA-seq analysis, you’re already familiar with gene-level analysis. But genes are far more complex than simple on/off switches – they can produce multiple versions of RNA transcripts through fascinating processes like alternative splicing. These different versions, called isoforms, allow a single gene to create multiple protein products, dramatically expanding the functional diversity of our genome. Today, we’ll dive deep into how to identify and quantify these isoforms from your RNA-seq data.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Alternative_splicing.jpg?resize=1024%2C811&ssl=1)

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/)

- [Why Isoform Analysis Matters](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Why_Isoform_Analysis_Matters)
- [Getting Started: Setting Up Your Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Getting_Started_Setting_Up_Your_Environment)

  - [Installing Salmon: Your New Favorite Tool](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Installing_Salmon_Your_New_Favorite_Tool)
  - [Setting Up Your Reference Data](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Setting_Up_Your_Reference_Data)

- [The Analysis Pipeline: From Raw Reads to Isoform Counts](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#The_Analysis_Pipeline_From_Raw_Reads_to_Isoform_Counts)

  - [Step 1: Quality Control and Trimming](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Step_1_Quality_Control_and_Trimming)
  - [Step 2: Quantifying Isoforms with Salmon](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Step_2_Quantifying_Isoforms_with_Salmon)
  - [Step 3: Making Sense of the Results](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Step_3_Making_Sense_of_the_Results)
  - [Step 4: Differential Isoform Expression Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Step_4_Differential_Isoform_Expression_Analysis)

- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Conclusion)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-10-isoform-analysis/#Like_this)

## Why Isoform Analysis Matters

The importance of isoform analysis extends far beyond academic interest. When cells produce incorrect ratios of isoforms or generate aberrant isoforms, the consequences can be severe. We’ve seen this in numerous diseases:

- Cancer cells often express unique isoform patterns that contribute to their aggressive behavior

- Neurodegenerative disorders frequently involve misregulation of splicing

- Cardiovascular diseases can arise from altered isoform expression

By understanding isoform expression patterns, researchers have identified new disease biomarkers and potential therapeutic targets. This makes isoform analysis an essential skill in modern genomics research.

## Getting Started: Setting Up Your Environment

Before we dive into the analysis, let’s make sure you have everything you need. We’ll build upon the environment we created in our previous tutorials, adding some specialized tools for isoform analysis.

### Installing Salmon: Your New Favorite Tool

For this tutorial, we’ll use Salmon – a tool that has revolutionized transcript quantification. What makes Salmon special? Unlike traditional aligners that meticulously map every read to the genome, Salmon uses a clever “quasi-mapping” strategy that’s both faster and highly accurate. Here’s how to get it running. We’ll install Salmon in our [previously created RNA-seq environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/).

```bash
# First, activate our trusty RNA-seq environment
conda activate rnaseq_env

# Install Salmon and trim-galore for adapter trimming
conda install -c bioconda salmon trim-galore

# Let's make sure it worked
salmon --version
```

### Setting Up Your Reference Data

For Salmon to work its magic, we need a pre-built index. Think of this as a well-organized reference library that Salmon can quickly search through. Save the downloaded Salmon index in a clearly named directory for the quantification step, e.g., ~/Genome_Index/Salmon_mm10/salmon_index/.  You can download pre-built indices from [refgenie](http://refgenomes.databio.org/index):

- For human data: [Human Genome Index](http://refgenomes.databio.org/v3/assets/splash/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/salmon_partial_sa_index?tag=default)

- For mouse data: [Mouse Genome Index](http://refgenomes.databio.org/v3/assets/splash/0f10d83b1050c08dd53189986f60970b92a315aa7a16a6f1/salmon_partial_sa_index?tag=default)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/image-1.png?resize=901%2C1024&ssl=1)

## The Analysis Pipeline: From Raw Reads to Isoform Counts

### Step 1: Quality Control and Trimming

Just like in our gene-level analysis, we start with quality control. Clean data is crucial for accurate isoform quantification:

```bash
# Run trim_galore with FastQC for comprehensive QC
trim_galore --fastqc \
    --paired \
    --cores 8 \
    ~/RNAseq/raw/SRR28119110_R1_001.fastq.gz \
    ~/RNAseq/raw/SRR28119110_R2_001.fastq.gz \
    -o ~/RNAseq/isoform_analysis/trimmed/SRR28119110
```

### Step 2: Quantifying Isoforms with Salmon

Now comes the exciting part – actually measuring isoform expression. Salmon will process your reads and tell you how much of each isoform is present:

```bash
# Run Salmon quantification
salmon quant -i ~/Genome_Index/Salmon_mm10/salmon_index/ \
    -l A \
    -1 ~/RNAseq/isoform_analysis/trimmed/SRR28119110/SRR28119110_R1_001_val_1.fq.gz \
    -2 ~/RNAseq/isoform_analysis/trimmed/SRR28119110/SRR28119110_R2_001_val_2.fq.gz \
    --validateMappings \
    -p 8 \
    -o ~/RNAseq/isoform_analysis/quantification/SRR28119110
```

The output includes a crucial file called `quant.sf`. This contains your isoform expression data in two key metrics:

- TPM (Transcripts Per Million): Perfect for comparing between samples

- Read counts: Essential for differential expression analysis

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-30-at-11.24.05%E2%80%AFAM.png?resize=800%2C190&ssl=1)

### Step 3: Making Sense of the Results

Raw transcript IDs aren’t very informative. Let’s add some meaningful annotations to our results using R:

```r
# Load essential packages
library(data.table)
library(stringr)

# Read in the GTF file - this contains our transcript annotations
gtf_mm10 <- fread("~/Genome_Index/Salmon_mm10/salmon_index/0f10d83b1050c08dd53189986f60970b92a315aa7a16a6f1.gtf", skip = 5)

# Focus on transcript-level information
gtf_mm10 <- gtf_mm10[gtf_mm10[[3]] == "transcript"]

# Create a clean annotation table
transcript_annot <- data.table(
    trascript_id = gsub("transcript_id| |\"", "", 
                        sapply(strsplit(gtf_mm10[[9]], ";"), "[[", 3)),
    trascript_name = gsub("transcript_name| |\"", "", 
                         sapply(strsplit(gtf_mm10[[9]], ";"), "[[", 8))
)

# Add annotations to our quantification results
quant_SRR28119110 <- fread("isoform_analysis/quantification/SRR28119110/quant.sf")

quant_SRR28119110$trascript_name <- gsub("\\..*$", "", quant_SRR28119110$trascript_name)

quant_SRR28119110$trascript_name <- transcript_annot$trascript_name[
    match(quant_SRR28119110$Name, transcript_annot$trascript_id)
]

# Save our annotated results
fwrite(quant_SRR28119110, 
       "~/RNAseq/isoform_analysis/quantification/SRR28119110/quant_annotated.sf")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-30-at-11.55.27%E2%80%AFAM.png?resize=1024%2C234&ssl=1)

### Step 4: Differential Isoform Expression Analysis

Now that we have annotated quantification results for each sample, it’s time to bring everything together into a unified isoform expression matrix. This step is crucial for downstream analysis, particularly for identifying differentially expressed isoforms between conditions.

#### Creating the Isoform Expression Matrix

Let’s combine our individual sample results into a comprehensive matrix:

```r
# Install tximport
if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

BiocManager::install(c("tximport", "rtracklayer"))

library(tximport)
library(rtracklayer)

# Estimate counts of the transcripts
files <- list.files("~/RNAseq/isoform_analysis/quantification", pattern = "quant.sf", full.names = TRUE, recursive = T)
names(files) <- basename(dirname(files))

txi <- tximport(files, type = "salmon", txOut = TRUE, countsFromAbundance = "lengthScaledTPM")
count_tbl <- as.data.frame(txi$counts)

# Remove trascript ID version suffix if necessary
rownames(count_tbl) <- gsub("\\..*$", "", rownames(count_tbl))
count_tbl$transcript_id <- rownames(count_tbl)

# Annotate the trascript ID 
count_tbl$trascript_name <- transcript_annot$trascript_name[
  match(rownames(count_tbl), transcript_annot$trascript_id)]

# Order the columns of the count table
count_tbl <- count_tbl[, c(5:6, 1:4)]

# Save the transcript count table
fwrite(count_tbl, "isoform_expression_counts.tsv", sep = "\t")

# Create a TPM matrix if you prefer TPMs
tpm_tbl <- as.data.frame(txi$abundance)
rownames(tpm_tbl) <- gsub("\\..*$", "", rownames(tpm_tbl))

tpm_tbl$transcript_id <- rownames(tpm_tbl)
tpm_tbl$trascript_name <- transcript_annot$trascript_name[
  match(rownames(tpm_tbl), transcript_annot$trascript_id)]

tpm_tbl <- tpm_tbl[, c(5:6, 1:4)]

fwrite(tpm_tbl, "isoform_expression_tpm.tsv", sep = "\t")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2025-02-01-at-8.27.10%E2%80%AFPM.png?resize=1024%2C202&ssl=1)

#### Creating the Gene-level Expression Matrix

Salmon can estimate gene-level expression in both raw counts and TPM formats.

```r
# Load the GTF file
gtf_annot <- import("~/Genome_Index/Salmon_mm10/salmon_index/0f10d83b1050c08dd53189986f60970b92a315aa7a16a6f1.gtf")

# Create a transcript-gene annotation table
gene_symbols <- na.omit(unique(data.frame(
  gene_id = gtf_annot$transcript_id,
  gene_symbol = gtf_annot$gene_name
)))

# Quantify gene expression
txi_gene <- tximport(files, type = "salmon", tx2gene = gene_symbols, ignoreTxVersion = T)

txi_gene_count <- as.data.frame(txi_gene$counts)
txi_gene_tpm <- as.data.frame(txi_gene$abundance)

fwrite(txi_gene_count, "gene_expression_counts.tsv", sep = "\t", row.names = T)
fwrite(txi_gene_tpm, "gene_expression_tpm.tsv", sep = "\t", row.names = T)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2025-02-02-at-9.49.53%E2%80%AFPM.png?resize=1024%2C416&ssl=1)

#### Differential Analysis

Most modern differential expression analysis tools require raw count data as input. Since Salmon provides transcript-level abundance estimates, we use ‘tximport’ to convert these into count estimates. We can then perform differential analysis following the same workflow as our [previous gene-level analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/). 

## Conclusion

Isoform analysis opens up exciting possibilities for understanding gene regulation and function. While it’s more complex than gene-level analysis, the insights it provides are invaluable for understanding disease mechanisms and identifying therapeutic targets.
