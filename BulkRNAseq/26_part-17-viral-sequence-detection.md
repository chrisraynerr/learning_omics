# How to Analyze RNAseq Data for Absolute Beginners Part 17: Viral Sequence Detection

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Metagenomics, Transcriptomics
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/)

- [Introduction to Viral Sequence Detection](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Introduction_to_Viral_Sequence_Detection)

  - [The Dynamic World of Viral Detection](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#The_Dynamic_World_of_Viral_Detection)
  - [Meeting the Computational Challenge](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Meeting_the_Computational_Challenge)

- [Choosing Your Analysis Approach](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Choosing_Your_Analysis_Approach)

  - [Method 1: EsViritu Pipeline Implementation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Method_1_EsViritu_Pipeline_Implementation)
  - [Method 2: VIRTUS2 Implementation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Method_2_VIRTUS2_Implementation)

- [Conclusion: Mastering Viral Sequence Detection](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Conclusion_Mastering_Viral_Sequence_Detection)

  - [Key Takeaways from This Tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Key_Takeaways_from_This_Tutorial)

- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-17-viral-sequence-detection/#Like_this)

## Introduction to Viral Sequence Detection

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-22-at-11.14.26%E2%80%AFAM.png?resize=1024%2C215&ssl=1)

The intersection of high-throughput sequencing and viral genomics has transformed our understanding of viral biology and disease. Through RNA-seq and whole-genome sequencing (WGS), researchers can now peer into the complex relationship between viruses and their hosts with unprecedented clarity. This technological breakthrough has revolutionized both biomedical research and clinical diagnostics, enabling real-time tracking of disease outbreaks and deep analysis of host-pathogen interactions at the molecular level.

### The Dynamic World of Viral Detection

Viruses represent nature’s ultimate paradox – they are simultaneously potential threats and invaluable agents of biological innovation. This duality becomes apparent when we examine landmark discoveries in viral research. The study of human papillomavirus (HPV) opened new frontiers in cancer biology, revealing how viral proteins manipulate cellular pathways and contribute to carcinogenesis. Similarly, research into viral genome integration has illuminated complex mechanisms of host genome modification and cellular transformation. More recently, the global SARS-CoV-2 pandemic has provided unprecedented insights into viral evolution and adaptation, demonstrating how quickly viruses can evolve and challenging our traditional models of host-pathogen dynamics.

### Meeting the Computational Challenge

The detection of viral sequences in sequencing data presents a fascinating computational puzzle. Modern sequencing technologies generate millions of DNA or RNA fragments, creating a complex mixture of genetic material from both host and potential viral sources. Researchers must navigate this sea of data with sophisticated computational tools that can efficiently process vast quantities of sequencing reads while maintaining accuracy and sensitivity. The challenge lies not only in identifying viral sequences but also in distinguishing them from host genetic material and potential environmental contamination.

Beyond mere detection, modern viral sequence analysis requires precise quantification of viral abundance and careful consideration of technical artifacts. This process demands a delicate balance between sensitivity and specificity, as false positives can lead to misidentification while false negatives might miss crucial viral signatures. The computational approaches we’ll explore in this tutorial have been carefully developed to address these challenges, providing reliable methods for viral sequence detection and characterization.

## Choosing Your Analysis Approach

This guide covers two complementary methods for viral sequence detection, each with distinct advantages for different research scenarios. We’ll explore:

1. [EsViritu](https://github.com/cmmr/EsViritu?tab=readme-ov-file): Optimal for broad virus diversity analysis in transcriptomic and metagenomic data

2. [VIRTUS2](https://github.com/yyoshiaki/VIRTUS2): Specialized for viral transcript detection in human RNA-seq data

### Method 1: EsViritu Pipeline Implementation

EsViritu excels at detecting and measuring human and animal virus pathogens in metagenomic data. Let’s walk through the setup and analysis process.

#### Environment Setup

First, create a dedicated conda environment on your Linux system:

```bash
# Create and activate EsViritu environment
conda create -n Env_EsViritu -c conda-forge -c bioconda esviritu biopython
conda activate Env_EsViritu
```

#### Database Preparation

EsViritu requires a comprehensive virus database (current version: v2.0.2):

```bash
# Set up database directory
mkdir -p ~/Genome_Index/EsViritu_DB/
cd ~/Genome_Index/EsViritu_DB/

# Download and extract database
wget https://zenodo.org/records/7876309/files/DB_v2.0.2.tar.gz
tar -xvf DB_v2.0.2.tar.gz
rm DB_v2.0.2.tar.gz

# Configure database path
conda env config vars set ESVIRITU_DB=~/Genome_Index/EsViritu_DB/DBs/v2.0.2
```

#### Running the Analysis

Execute virus detection on your sequencing data:

```bash
# Create output directory
mkdir -p ~/EsViritu_Output/Sample1

# Run EsViritu analysis
EsViritu -r ~/raw/Sample1_R1.fastq.gz \
         ~/raw/Sample1_R2.fastq.gz \
         -s Sample1 \
         -o ~/EsViritu_Output/Sample1 \
         -t 16 -p paired -q True -f True
```

**Key Output Files:**

- `Sample1_EsViritu_reactable.html`: Interactive coverage reports

- `Sample1.detected_virus.info.tsv`: Detailed detection results

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-21-at-10.43.40%E2%80%AFPM.png?resize=1024%2C129&ssl=1)

### Method 2: VIRTUS2 Implementation

VIRTUS2 specializes in viral transcript detection, considering splicing events in both bulk and single-cell RNA-seq data. It currently supports 762 viruses, including SARS-CoV-2.

#### Environment Configuration

Set up your VIRTUS2 environment:

```bash
# Create Python environment
conda create -n Env_VIRTUS2 python=3.9
conda activate Env_VIRTUS2

# Install dependencies
conda install conda-forge::singularity
pip install cwltool numpy pandas scipy statsmodels seaborn

# Get VIRTUS2 source
git clone https://github.com/yyoshiaki/VIRTUS2
```

*Note: For HPC systems, modify the CWL files to use Singularity instead of Docker:*

- Edit `~/VIRTUS2/bin/createindex.cwl`

- Edit `~/VIRTUS2/bin/VIRTUS.PE.cwl`

- Replace first line with: `#!~/Env_VIRTUS2/bin/cwltool --singularity`

#### Reference Preparation

Set up the required reference files:

```bash
# Create reference directory
mkdir -p ~/Genome_Index/VIRTUS2_DB/
cd ~/Genome_Index/VIRTUS2_DB/

# Download and index references
~/VIRTUS2/bin/createindex.cwl \
  --url_virus https://raw.githubusercontent.com/yyoshiaki/VIRTUS2/master/data/viruses.fasta \
  --output_name_virus OUTPUT_VIRUS \
  --runThreadN 16 \
  --dir_name_STAR_virus STAR_VIRUS \
  --url_genomefasta_human ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_33/GRCh38.p13.genome.fa.gz \
  --output_name_genomefasta_human OUTPUT_hg38_GENOMEFASTA_HUMAN \
  --dir_name_STAR_human STAR_HUMAN_hg38
```

#### Analysis Execution

Run the viral detection pipeline:

```bash
# For paired-end data
~/VIRTUS2/bin/VIRTUS.PE.cwl \
  --fastq1 Sample1_R1.fastq.gz \
  --fastq2 Sample1_R2.fastq.gz \
  --genomeDir_human ~/Genome_Index/VIRTUS2_DB/STAR_HUMAN_hg38 \
  --genomeDir_virus ~/Genome_Index/VIRTUS2_DB/STAR_VIRUS \
  --outFileNamePrefix_human Sample1 \
  --nthreads 16
```

**Key Output Files:**

- `VIRTUS.output.tsv`: Detailed detection results

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-21-at-10.51.03%E2%80%AFPM-1.png?resize=1024%2C97&ssl=1)

## Conclusion: Mastering Viral Sequence Detection

The ability to detect and analyze viral sequences in RNA-seq data opens up powerful possibilities for both research and clinical applications. Through this tutorial, we’ve explored two robust approaches – EsViritu for broad virus diversity analysis and VIRTUS2 for specialized viral transcript detection. Each method offers unique advantages that can be leveraged depending on your specific research needs.

### Key Takeaways from This Tutorial

Understanding viral sequences in RNA-seq data requires a careful balance of computational precision and biological insight. We’ve seen how proper environment setup, database preparation, and analysis execution form the foundation of reliable results. The choice between EsViritu and VIRTUS2 depends largely on your research questions – whether you’re investigating virus diversity in metagenomic samples or focusing on viral transcript expression in human samples.

## References

- Tisza, M., et al. (2023). Wastewater sequencing reveals community and variant dynamics of the collective human virome. Nature Communications, 14, 6878.

- Yoshiaki Yasumizu, et al. (2021). VIRTUS: a pipeline for comprehensive virus analysis from conventional RNA-seq data. Bioinformatics, 37(10), 1465-1467.
