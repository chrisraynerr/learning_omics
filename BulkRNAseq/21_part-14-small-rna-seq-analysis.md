# How to Analyze RNAseq Data for Absolute Beginners Part 14: Mastering Small RNA-Seq Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/)

- [The Hidden World of Small RNAs: More Than Just Tiny Molecules](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#The_Hidden_World_of_Small_RNAs_More_Than_Just_Tiny_Molecules)

  - [MicroRNAs: The Master Regulators](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#MicroRNAs_The_Master_Regulators)
  - [siRNAs: Nature’s Defense System](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#siRNAs_Natures_Defense_System)
  - [piRNAs: Guardians of the Genome](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#piRNAs_Guardians_of_the_Genome)
  - [The Supporting Cast: Emerging Small RNA Classes](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#The_Supporting_Cast_Emerging_Small_RNA_Classes)

- [Preparing for Small RNA Analysis: Setting Up Your Toolkit](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Preparing_for_Small_RNA_Analysis_Setting_Up_Your_Toolkit)

  - [Creating Your Analysis Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Creating_Your_Analysis_Environment)
  - [Preparing Your Reference Files](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Preparing_Your_Reference_Files)

- [The Analysis Journey: From Raw Reads to Biological Insights](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#The_Analysis_Journey_From_Raw_Reads_to_Biological_Insights)

  - [Step 1: Quality Control and Adapter Trimming](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Step_1_Quality_Control_and_Adapter_Trimming)
  - [Step 2: Genome Alignment – Finding Where Your Small RNAs Come From](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Step_2_Genome_Alignment_%E2%80%93_Finding_Where_Your_Small_RNAs_Come_From)
  - [Step 3: Quantification – Counting Your Small RNAs](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Step_3_Quantification_%E2%80%93_Counting_Your_Small_RNAs)
  - [Step 4: Data Integration and Analysis in R](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Step_4_Data_Integration_and_Analysis_in_R)
  - [Step 5: Differential Expression Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Step_5_Differential_Expression_Analysis)

- [Moving Forward](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Moving_Forward)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/#Like_this)

## The Hidden World of Small RNAs: More Than Just Tiny Molecules

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/13045_2023_1466_Fig5_HTML.webp?resize=1024%2C1024&ssl=1)

Small RNAs are fascinating molecules that challenge the traditional “DNA to RNA to protein” dogma of molecular biology. Despite being just 20-30 nucleotides in length – barely a fraction of a typical messenger RNA – these molecules orchestrate complex biological processes with remarkable precision. Think of them as the conductors of the cellular orchestra, coordinating when and how genes are expressed.

Let’s dive into the key players in this molecular symphony:

### MicroRNAs: The Master Regulators

MicroRNAs (miRNAs) are perhaps the most well-studied members of the small RNA family. They act like molecular brakes, fine-tuning gene expression through two main mechanisms:

1. Direct mRNA degradation – imagine a paper shredder specifically targeting certain messages

2. Translational repression – like putting a lock on the protein production machinery

These tiny regulators influence everything from embryonic development to immune responses. When you’re analyzing miRNA data, you’re essentially watching these molecular switches in action, observing how they respond to different conditions or diseases.

### siRNAs: Nature’s Defense System

Small interfering RNAs (siRNAs) are part of an ancient defense mechanism that cells use to fight off viral invaders and maintain genomic stability. They work through RNA interference (RNAi), a process so important that its discovery won the 2006 Nobel Prize. Think of siRNAs as part of the cell’s immune system, specifically targeting and neutralizing potential threats.

### piRNAs: Guardians of the Genome

PIWI-interacting RNAs (piRNAs) serve as genomic guardians, primarily in germline cells. Their main job is keeping transposable elements (jumping genes) in check, preventing these genomic parasites from causing chaos in our DNA. Without piRNAs, our genome would be vulnerable to all sorts of disruptive elements.

### The Supporting Cast: Emerging Small RNA Classes

Recent research has unveiled even more players in the small RNA world:

- tRNA-derived small RNAs (tsRNAs) help cells respond to stress

- Fragments from traditional RNA molecules (snRNAs and snoRNAs) may have regulatory roles we’re just beginning to understand

## Preparing for Small RNA Analysis: Setting Up Your Toolkit

Before we dive into the analysis, we need to set up our computational environment. Think of this as preparing your laboratory workspace – everything needs to be in its proper place for efficient analysis.

### Creating Your Analysis Environment

First, let’s build upon our previous RNA-seq environment. If you haven’t set this up yet, check out our [earlier tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/) for the basics.

```bash
# Activate our RNA-seq environment
conda activate rnaseq_env

# Install Bowtie - our specialized tool for small RNA alignment
# While Bowtie is an older tool, it excels at handling short sequences
conda install -c bioconda bowtie
```

### Preparing Your Reference Files

Just as a map helps you navigate unfamiliar territory, reference files guide our analysis through the genome:

```bash
# Navigate to your working directory
cd ~/Downloads

# Download and extract the human genome from Ensembl
wget https://ftp.ensembl.org/pub/release-113/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz

# Get the gene annotations
wget https://ftp.ensembl.org/pub/release-113/gtf/homo_sapiens/Homo_sapiens.GRCh38.113.gtf.gz
gunzip Homo_sapiens.GRCh38.113.gtf.gz

# Create a small RNA-specific annotation file
# This filters for just the RNA types we're interested in
grep -P 'gene_biotype "(miRNA|snRNA|snoRNA|rRNA|tRNA|piRNA)"' \
    Homo_sapiens.GRCh38.113.gtf > small_RNA.gtf

# Build the Bowtie index - think of this as creating a specialized 
# reference book for quick lookups
bowtie-build Homo_sapiens.GRCh38.dna.primary_assembly.fa \
    ~/Genome_Index/bowtie_index_hg38
```

## The Analysis Journey: From Raw Reads to Biological Insights

### Step 1: Quality Control and Adapter Trimming

Small RNA sequencing requires special attention to quality control. The short length of these RNAs means that adapter sequences can make up a significant portion of our reads:

```bash
# Trim adapters and filter for quality
# Note: This adapter sequence is from the NEBNext® Multiplex kit
# Adjust if you're using a different kit
trim_galore --fastqc \
    --quality 20 \
    --phred33 \
    --length 18 \
    --stringency 3 \
    --three_prime_clip_R1 1 \
    --cores 20 \
    --adapter AGATCGGAAGAGCACACGTCTGAACTCCAGTCAC \
    ~/smRNAseq/raw/Sample1_R1_001.fastq.gz \
    -o ~/smRNAseq/trimmed/Sample1/

# Filter for appropriate read lengths
# Small RNAs are short, so we want to keep only reads ≤35 bases
zcat ~/smRNAseq/trimmed/Sample1/Sample1_R1_001_trimmed.fq.gz | \
    awk 'BEGIN {FS = "\n"}
    {
        header = $0
        getline seq
        getline plus
        getline qual
        if (length(seq) <= 35) {
            print header "\n" seq "\n" plus "\n" qual
        }
}' | gzip > ~/smRNAseq/trimmed/Sample1/Sample1_R1_001_trimmed.maxlen35.fq.gz
```

### Step 2: Genome Alignment – Finding Where Your Small RNAs Come From

Aligning small RNA reads requires special consideration due to their short length:

```bash
# Align reads to the genome using Bowtie
# Parameters explained:
# -v 1: Allow 1 mismatch
# -m 1: Report only unique alignments
# --best --strata: Get the best possible alignments
bowtie -v 1 -m 1 --best --strata --norc -l 18 \
    -x ~/Genome_Index/bowtie_index_hg38 \
    -p 20 \
    -q ~/smRNAseq/trimmed/Sample1/Sample1_R1_001_trimmed.maxlen35.fq.gz \
    -S ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35.sam

# Convert, sort, and index your aligned reads
samtools view -bS ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35.sam \
    > ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35.bam

samtools sort ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35.bam \
    -o ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35_sorted.bam

samtools index ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35_sorted.bam

# Generate alignment statistics to check how well things worked
samtools flagstat ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35_sorted.bam \
    > ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35_sorted.bam.flagstat
```

The alignment statistics provided by the flagstat command:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-26-at-10.23.52%E2%80%AFPM.png?resize=898%2C424&ssl=1)

### Step 3: Quantification – Counting Your Small RNAs

Now we’ll count how many reads map to each small RNA:

```bash
# Use featureCounts to quantify small RNAs
# -T 20: Use 20 threads for faster processing
# -s 1: Consider strandedness
featureCounts -T 20 \
    -t gene \
    -g gene_id \
    -s 1 \
    -a ~/Genome_Index/GTF/small_RNA.gtf \
    -o ~/smRNAseq/aligned/Sample1/Sample1_R1_001_featureCounts_gene.txt \
    ~/smRNAseq/aligned/Sample1/Sample1_R1_001_trimmed.maxlen35_sorted.bam
```

Repeat the above steps for all your samples.

### Step 4: Data Integration and Analysis in R

Now comes the exciting part – bringing everything together to make sense of your data:

```r
# Load your essential R packages
library(data.table)
library(edgeR)
library(stringr)

# Find all your count files
file_names <- list.files(count_file_path, 
                        pattern = "featureCounts_gene\\.txt$", 
                        recursive = T, 
                        full.names = T)

# Create a DGElist object - this organizes your count data
dgls <- readDGE(file_names, columns = c(1, 7), skip = 1)
counts_raw <- as.data.frame(dgls$counts)

# Clean up sample names for clarity
sample_name <- gsub("_R1_001_featureCounts_gene", "", 
                   basename(colnames(counts_raw)))
colnames(counts_raw) <- sample_name

# Process gene annotations to make your results interpretable
gtf <- fread("~/Genome_Index/GTF/small_RNA.gtf")
gtf_gene <- gtf[V3 == "gene"]

# Create a mapping between gene IDs and names
id_name_df <- data.table(
    gene_id = gsub("gene_id| |\"", "", 
                   sapply(strsplit(gtf_gene[[9]], ";"), 
                          \(x) {x[str_detect(x, "gene_id")]})),
    gene_name = gsub("gene_name| |\"", "", 
                     sapply(strsplit(gtf_gene[[9]], ";"), 
                            \(x) {x[str_detect(x, "gene_name")]}))
)

# Clean up and combine your data
id_name_df[id_name_df == "character(0)"] <- NA
rownames(id_name_df) <- id_name_df[[1]]
id_name_df <- id_name_df[rownames(counts_raw), ]
counts_raw <- cbind(id_name_df, counts_raw)

# Save your processed count table
fwrite(counts_raw, "counts_raw.tsv", sep = "\t")
```

The final count table has the following structure:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-26-at-10.33.22%E2%80%AFPM.png?resize=1024%2C167&ssl=1)

### Step 5: Differential Expression Analysis

The differential expression analysis of small RNA sequencing data builds upon the fundamental principles we covered in our [previous RNA-seq tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/).

## Moving Forward

Small RNA analysis is a complex but rewarding journey. Each dataset tells a unique story about cellular regulation and disease processes. By following this guide and staying mindful of best practices, you’re well-equipped to unlock the biological insights hidden in your small RNA-seq data.

## References

1. Xiong, Q.; Zhang, Y.; Li, J.; Zhu, Q. Small Non-Coding RNAs in Human Cancer. *Genes* **2022**, *13*, 2072. https://doi.org/10.3390/genes13112072

2. Xiong, Q., Zhang, Y. Small RNA modifications: regulatory molecules and potential applications. J Hematol Oncol 16, 64 (2023). https://doi.org/10.1186/s13045-023-01466-w
