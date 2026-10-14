# How to Analyze RNAseq Data for Absolute Beginners Part 15-2: Mastering UMI-Based miRNA-Seq Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/)

- [Understanding UMI-Based miRNA Sequencing](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Understanding_UMI-Based_miRNA_Sequencing)

  - [Why UMIs Matter in miRNA-Seq Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Why_UMIs_Matter_in_miRNA-Seq_Analysis)
  - [Choosing Between Regular and UMI-Based miRNA-Seq](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Choosing_Between_Regular_and_UMI-Based_miRNA-Seq)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Setting_Up_Your_Analysis_Environment)

  - [Required Software Installation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Required_Software_Installation)
  - [Reference File Preparation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Reference_File_Preparation)

- [Analysis Approaches: Two Complementary Methods](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Analysis_Approaches_Two_Complementary_Methods)

  - [Method 1: Streamlined Analysis with UMI-tools](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Method_1_Streamlined_Analysis_with_UMI-tools)
  - [Method 2: Comprehensive Analysis with miRDeep2](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Method_2_Comprehensive_Analysis_with_miRDeep2)

- [Troubleshooting Guide](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Troubleshooting_Guide)
- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-2-mastering-umi-based-mirna-seq-analysis/#Like_this)

## Understanding UMI-Based miRNA Sequencing

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-11-at-2.20.36%E2%80%AFPM.png?resize=1024%2C110&ssl=1)

MicroRNAs (miRNAs) serve as crucial regulators in gene expression, making their accurate quantification essential for understanding disease mechanisms and biological processes. While traditional miRNA sequencing has proven valuable, the integration of Unique Molecular Identifiers (UMIs) represents a significant advancement in achieving precise miRNA measurements. This tutorial will guide you through the complete workflow of UMI-based miRNA-seq analysis, suitable for both beginners and experienced researchers.

### Why UMIs Matter in miRNA-Seq Analysis

Think of UMIs as unique barcodes attached to individual RNA molecules during library preparation. These molecular tags serve a critical purpose: they allow us to distinguish between genuine biological signals and technical artifacts introduced during PCR amplification. Just as a barcode helps track individual items in a store, UMIs help us track individual RNA molecules through the sequencing process.

The key advantages of using UMIs include:

- Elimination of PCR duplicates for more accurate quantification

- Improved detection of low-abundance miRNAs

- Enhanced reliability in differential expression analysis

- Better reproducibility across experiments

### Choosing Between Regular and UMI-Based miRNA-Seq

Your choice between these approaches depends on your research goals:

**Regular miRNA-seq** works well for:

- Initial exploratory studies

- Projects with limited budgets

- Situations requiring broad miRNA detection

- Basic differential expression analysis

**UMI-based miRNA-seq** excels in:

- Biomarker discovery projects

- Clinical sample analysis

- Studies with degraded RNA (e.g., FFPE samples)

- Research requiring absolute quantification

- Low-input RNA experiments

## Setting Up Your Analysis Environment

### Required Software Installation

We’ll build upon the RNA-seq environment from our [previous miRNA-seq tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/).

Let’s begin by setting up our analysis environment with the necessary tools ([UMI-tools](https://github.com/CGATOxford/UMI-tools)):

```bash
# First, activate our RNA-seq environment
conda activate rnaseq_env

# Install UMI-tools and its dependencies
conda install -c bioconda -c conda-forge umi_tools
```

### Reference File Preparation

For this tutorial, we’ll use the same reference files as our previous miRNA-seq analysis. If you haven’t prepared these files yet, please refer to our [basic miRNA-seq tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/) for detailed instructions.

## Analysis Approaches: Two Complementary Methods

We’ll explore two robust approaches for analyzing UMI-based miRNA-seq data, each offering unique advantages. Our example uses data prepared with the QIAseq miRNA Library Kit.

### Method 1: Streamlined Analysis with UMI-tools

This approach offers a quick and efficient way to process your data while maintaining accuracy.

#### Step 1: Quality Control and UMI Extraction

First, let’s create our directory structure and process the raw data:

```bash
# Create directory structure
mkdir -p ~/miRNAseq_UMI/{raw,trimmed,aligned}/Sample1_R1_001

# Extract UMIs and remove QIAseq adapters
umi_tools extract \
    --extract-method=regex \
    --bc-pattern='.+(?P<discard_1>AACTGTAGGCACCATCAAT){s<=2}(?P<umi_1>.{12})(?P<discard_2>.+)' \
    --stdin=~/miRNAseq_UMI/raw/Sample1_R1_001/Sample1_R1_001.fastq.gz \
    --stdout=~/miRNAseq_UMI/trimmed/Sample1_R1_001/Sample1_R1_001-directUMIextracted.fastq \
    --log=~/miRNAseq_UMI/trimmed/Sample1_R1_001/Sample1_R1_001_umi-UMIextraction-fromrawreads.log

# Filter reads by length (18-30 nucleotides)
cutadapt \
    --minimum-length=18 \
    --maximum-length=30 \
    -o ~/miRNAseq_UMI/trimmed/Sample1_R1_001/Sample1_R1_001-directUMIextracted-min18max30L.fastq \
    ~/miRNAseq_UMI/trimmed/Sample1_R1_001/Sample1_R1_001-directUMIextracted.fastq
```

#### Step 2: Reference Alignment

Align the processed reads to your reference miRNA sequences:

```bash
# Align to human miRNA reference using Bowtie
bowtie \
    -n 0 -l 32 --norc --best --strata -m 1 --threads 16 \
    -x ~/Genome_Index/mirbase_bowtie_index/mirbase_hsa \
    ~/miRNAseq_UMI/trimmed/Sample1_R1_001/Sample1_R1_001-directUMIextracted-min18max30L.fastq \
    -S ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-maturemiRNA-aligned-bowtie1-beststratam1.sam
```

#### Step 3: UMI-Based Quantification

Process aligned reads and perform UMI-based counting:

```bash
# Convert and sort alignment file
samtools sort ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-maturemiRNA-aligned-bowtie1-beststratam1.sam > \
    ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-maturemiRNA-aligned-bowtie1-beststratam1.bam

# Index the BAM file
samtools index ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-maturemiRNA-aligned-bowtie1-beststratam1.bam

# Remove PCR duplicates using UMIs
umi_tools dedup \
    --method=unique \
    -I ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-maturemiRNA-aligned-bowtie1-beststratam1.bam \
    -L ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-deduplicate-matureMirna-uniquemethod-beststratam1.log \
    -S ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-deduplicated-matureMirna-uniquemethod-beststratam1.bam

# Generate final counts
umi_tools count \
    --method=unique \
    --per-contig \
    -I ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-deduplicated-matureMirna-uniquemethod-beststratam1.bam \
    -L ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001-counts-uniquemethod-maturemiRNA.log \
    -S ~/miRNAseq_UMI/aligned/Sample1_R1_001/Sample1_R1_001_counts-finaloutput-uniquemethod-maturemiRNA.txt
```

The final count table has the following format: 

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-11-at-3.26.56%E2%80%AFPM.png?resize=404%2C194&ssl=1)

### Method 2: Comprehensive Analysis with miRDeep2

This approach provides deeper insights into miRNA expression and potential novel miRNAs.

#### Step 1: Genome Alignment

```bash
# Create output directory
mkdir -p ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/
cd ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/

# Align to human genome
# The processed FASTQ file is from Method 1
bowtie \
    -n 0 -l 32 --norc --best --strata -m 1 --threads 16 \
    -x ~/Genome_Index/bowtie_index_hg38 \
    ~/miRNAseq_UMI/trimmed/Sample1_R1_001/Sample1_R1_001-directUMIextracted-min18max30L.fastq \
    -S ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1.sam
```

#### Step 2: miRDeep2 Input Preparation

```bash
# Process alignment files
samtools sort ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1.sam > \
    ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1.bam

samtools index ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1.bam

# Remove duplicates
umi_tools dedup \
    --method=unique \
    -I ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1.bam \
    -L ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.log \
    -S ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.bam

# Convert to miRDeep2 format
samtools view -h -o ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.sam \
    ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.bam

bwa_sam_converter.pl \
    -i Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.sam \
    -c \
    -o Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.collapsed \
    -a Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.arf
```

#### Step 3: miRDeep2 Analysis

```
miRDeep2.pl \
    ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.collapsed \
    ~/Genome_Index/Genome/hg38/genome_hg38_renamed.fa \
    ~/miRNAseq_UMI/mirdeep2/output/Sample1_R1_001/Sample1_R1_001-hg38-aligned-bowtie1-beststratam1_dedup.arf \
    ~/Genome_Index/mirbase_bowtie_index/mature_hsa_renamed.fa \
    none \
    ~/Genome_Index/mirbase_bowtie_index/hairpin_hsa_renamed.fa \
    -t hsa
```

After running miRDeep2, you’ll find the output files organized in the same directory structure covered in our previous tutorial on regular miRNA-seq analysis. This includes the result.html file containing detailed miRNA predictions, expression data, and secondary structure information, along with tab-separated files of expression counts and statistics. For a detailed breakdown of these output files and their interpretation, please refer to our [previous miRNA-seq tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/). The only difference is that these results now reflect UMI-based quantification, providing more accurate measurements of miRNA expression levels.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-11-at-2.47.39%E2%80%AFPM.png?resize=478%2C226&ssl=1)

## Troubleshooting Guide

When working with miRDeep2, you’ll need to pay special attention to your Bowtie installation. miRDeep2 comes bundled with an older version of Bowtie, but our analysis requires features only available in the latest version. The older version lacks support for some of the command-line parameters we’re using, particularly the threading options and alignment strategy parameters.

To ensure compatibility, you should explicitly use the latest version of Bowtie from your system installation rather than the version packaged with miRDeep2. You can verify your Bowtie version by running `bowtie --version` – you’ll want version 1.3.0 or higher. If you need to install or update Bowtie, you can do so using conda: 

```
conda install -c bioconda bowtie=1.3.1
```

## Conclusion

UMI-based miRNA-seq analysis provides superior accuracy in miRNA quantification when properly executed. By following this guide and implementing the best practices outlined above, you’ll be well-equipped to generate high-quality miRNA expression data for your research.

For more information on downstream analysis, including differential expression analysis, please refer to our [RNA-seq differential expression tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/).

## References

- Potla P, Ali SA, Kapoor M. A bioinformatics approach to microRNA-sequencing analysis. Osteoarthr Cartil Open. 2020 Dec 19;3(1):100131. doi: 10.1016/j.ocarto.2020.100131. PMID: 36475076; PMCID: PMC9718162.

- Cowan, E., Karagiannopoulos, A., Eliasson, L. (2023). MicroRNAs in Type 2 Diabetes: Focus on MicroRNA Profiling in Islets of Langerhans. In: Moore, A., Wang, P. (eds) Type-1 Diabetes. Methods in Molecular Biology, vol 2592. Humana, New York, NY. https://doi.org/10.1007/978-1-0716-2807-2_8
