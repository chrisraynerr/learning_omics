# How to Analyze RNAseq Data for Absolute Beginners Part 15: A Complete Guide to miRNA-seq Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/)

- [Understanding the World of microRNAs](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Understanding_the_World_of_microRNAs)
- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Setting_Up_Your_Analysis_Environment)

  - [Preparing Your Reference Files](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Preparing_Your_Reference_Files)

- [Two Paths to miRNA Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Two_Paths_to_miRNA_Analysis)

  - [Method 1: The Quick and Efficient Approach](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Method_1_The_Quick_and_Efficient_Approach)
  - [Method 2: The Comprehensive Approach with miRDeep2](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Method_2_The_Comprehensive_Approach_with_miRDeep2)

- [Choosing Between Methods](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Choosing_Between_Methods)
- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-15-a-complete-guide-to-mirna-seq-analysis/#Like_this)

## Understanding the World of microRNAs

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-05-at-10.43.17%E2%80%AFAM.png?resize=1024%2C379&ssl=1)

The fascinating world of microRNAs (miRNAs) represents one of molecular biology’s most elegant regulatory systems. These tiny RNA molecules, spanning just 20-24 nucleotides, function as precise genetic regulators by binding to messenger RNAs (mRNAs) and fine-tuning their expression. Since their serendipitous discovery in the early 1990s, miRNAs have revolutionized our understanding of gene regulation, emerging as crucial players in nearly every biological process we’ve studied.

The journey of a miRNA from its birth to its regulatory function is a carefully orchestrated process. Initially transcribed as primary miRNAs (pri-miRNAs), these molecules undergo precise processing steps guided by the enzymes Drosha and Dicer. The final product joins forces with the RNA-induced silencing complex (RISC), creating a sophisticated molecular machine that can target specific mRNAs with remarkable precision. In animals, this targeting typically occurs in the 3′ untranslated region (3′ UTR) of mRNAs, leading to either translational repression or mRNA degradation, depending on the degree of sequence complementarity.

What makes miRNAs particularly fascinating is their regulatory versatility. A single miRNA can target multiple mRNAs, creating intricate regulatory networks that help maintain cellular homeostasis. However, this power comes with responsibility – when miRNAs malfunction, the consequences can be severe. Their dysregulation has been implicated in various diseases, from cancer to cardiovascular disorders and neurological conditions.

The advent of miRNA sequencing (miRNA-seq) technology has transformed our ability to study these molecules. This powerful tool enables comprehensive profiling of miRNA expression, helps identify novel miRNAs, and provides insights into miRNA-mediated regulatory networks. In this tutorial, we’ll explore how to harness this technology effectively.

## Setting Up Your Analysis Environment

Before diving into analysis, we need to prepare our computational workspace. We’ll build upon the RNA-seq environment from our previous tutorial on [small RNA analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-14-mastering-small-rna-seq-analysis/). This ensures we have a consistent and reliable setup for our analysis pipeline.

First, let’s set up our environment and install the necessary tools:

```bash
# Activate our RNA-seq environment
conda activate rnaseq_env

# Install miRDeep2 - a comprehensive tool for miRNA analysis
mamba install mirdeep2
mamba update mirdeep2
```

### Preparing Your Reference Files

A crucial step in miRNA analysis is preparing appropriate reference files. Unlike whole-genome sequencing, we can work with more compact and manageable references from [miRBase](https://www.mirbase.org/download/), the authoritative database for miRNA sequences.

```bash
# Create a directory structure for reference files
mkdir -p ~/Genome_Index/mirbase_bowtie_index/
cd ~/Genome_Index/mirbase_bowtie_index/

# Download reference files from miRBase
wget https://mirbase.org/download/mature.fa
wget https://mirbase.org/download/hairpin.fa

# Download the human genome for comprehensive analysis
wget https://ftp.ensembl.org/pub/release-113/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
```

The mature.fa file has the following structure:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-08-at-10.44.11%E2%80%AFAM.png?resize=870%2C288&ssl=1)

Now, let’s process these references to make them suitable for our analysis:

```bash
# Extract human-specific entries (those starting with "hsa")
awk '/^>/ {p = ($0 ~ /hsa/)} p' mature.fa > mature_hsa.fa
awk '/^>/ {p = ($0 ~ /hsa/)} p' hairpin.fa > hairpin_hsa.fa

# Clean up reference files by removing unwanted spaces
remove_white_space_in_id.pl mature_hsa.fa > mature_hsa_renamed.fa
remove_white_space_in_id.pl hairpin_hsa.fa > hairpin_hsa_renamed.fa
remove_white_space_in_id.pl Homo_sapiens.GRCh38.dna.primary_assembly.fa > genome_hg38_renamed.fa

# Convert RNA sequences to DNA alphabet
sed 's/U/T/g' mature_hsa_renamed.fa > mature_hsa_renamed_dna.fa

# Build Bowtie index for alignment
bowtie-build mature_hsa_renamed_dna.fa mirbase_hsa

# Build Bowtie index for the whole genome if you haven't done so
bowtie-build Homo_sapiens.GRCh38.dna.primary_assembly.fa \
    ~/Genome_Index/bowtie_index_hg38
```

## Two Paths to miRNA Analysis

In the world of miRNA-seq analysis, we often face a choice between quick results and comprehensive insights. I’ll present two complementary approaches, each with its own strengths and use cases. miRNA-seq is typically performed using single-end sequencing due to their small sizes. Here we use one of the samples in dataset [GSE64977](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE64977) as our example.

### Method 1: The Quick and Efficient Approach

This streamlined method is perfect when you need to quickly quantify known miRNAs. It’s particularly useful for initial exploration or when working with well-characterized miRNAs.

#### Step 1: Quality Control and Adapter Trimming

The short length of miRNAs makes proper adapter trimming crucial:

```bash
# Create output directory for trimmed files
mkdir -p ~/miRNA/trimmed/

# Trim adapters and filter for quality (use your own adapter sequence)
trim_galore --fastqc \
    --quality 20 \
    --adapter TGGAATTCTCGGGTGCCAAGG \
    --length 18 \
    --max_length 30 \
    --cores 8 \
    ~/miRNA/trimmed/SRR1759248/SRR1759248.fastq.gz \
    -o ~/miRNA/trimmed/SRR1759248/
```

#### Step 2: Reference Alignment

After trimming, we need to align our processed reads to the reference miRNA sequences. The alignment step is crucial for accurately identifying which miRNAs are present in our samples:

```bash
# Create a directory for aligned results
mkdir -p ~/miRNA/aligned/

# Align reads to the reference using Bowtie
# Parameters explained:
# -v 1: Allow up to 1 mismatch
# -m 1: Report only uniquely mapping reads
# --best --strata: Get best possible alignments
# --norc: Don't align to reverse complement (miRNAs are strand-specific)
bowtie -v 1 -m 1 --best --strata --norc -l 20 \
    -x ~/Genome_Index/mirbase_bowtie_index/mirbase_hsa \
    -p 16 \
    -q ~/miRNA/trimmed/SRR1759248/SRR1759248_trimmed.fq.gz \
    -S ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed.sam
```

#### Step 3: Quantification and Analysis

Now comes the exciting part – determining how many reads map to each miRNA. This gives us insight into miRNA expression levels:

```bash
# Convert SAM to BAM format and sort
samtools view -bS ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed.sam \
    > ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed.bam

samtools sort ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed.bam \
    -o ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed_sorted.bam

# Index the sorted BAM file for quick access
samtools index ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed_sorted.bam

# Generate alignment statistics for quality control
samtools flagstat ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed_sorted.bam \
    > ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed_sorted.bam.flagstat

# Count reads mapping to each miRNA
samtools idxstat ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed_sorted.bam \
    > ~/miRNA/aligned/SRR1759248/SRR1759248_trimmed_sorted_count.txt
```

#### Step 4: Differential Expression Analysis

The differential expression analysis of miRNA sequencing data builds upon the fundamental principles we covered in our [previous RNA-seq tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/).

### Method 2: The Comprehensive Approach with miRDeep2

While our first method is quick and efficient, sometimes we need more in-depth analysis. This is where [miRDeep2](https://github.com/rajewsky-lab/mirdeep2) shines. It’s a powerful tool designed specifically for miRNA analysis, capable of detecting both known and novel miRNAs while providing detailed information about miRNA precursors. Check the [miRDeep2 Documentation](https://www.mdc-berlin.de/content/mirdeep2-documentation?mdcbl%5B0%5D=/n-rajewsky%23t-data%2Csoftware%26resources&mdctl=0&mdcou=20738&mdcot=6&mdcbv=crsjgo3KpH2eVDwEmJ_-5lh5FYkn8dZh4PNU6NsBrTE) for the details of the parameters.

First, let’s align our reads to the whole genome:

```bash
# Create output directory
mkdir -p ~/miRNA/mirdeep2/output/SRR1759248/

# Perform genome alignment with mapper.pl
# Parameters explained:
# -g hsa: Specify human genome
# -l 18: Minimum read length
# -n -h -e -i -j -m: Various mapping options for optimal miRNA detection
mapper.pl \
    ~/miRNA/raw/SRR1759248.fastq \
    -g hsa \
    -l 18 \
    -n -h -e -i -j -m \
    -k TGGAATTCTCGGGTGCCAAGG \
    -s ~/miRNA/mirdeep2/output/SRR1759248/SRR1759248.fastq.collapsed \
    -p ~/Genome_Index/bowtie_index_hg38 \
    -t ~/miRNA/mirdeep2/output/SRR1759248/SRR1759248_vs_genome_h38.arf
```

Next, we’ll run the full miRDeep2 analysis:

```bash
# Run miRDeep2 analysis
# This step integrates alignment data with known miRNA information
# and searches for novel miRNAs based on sequence and structure
miRDeep2.pl \
    ~/miRNA/mirdeep2/output/SRR1759248/SRR1759248.fastq.collapsed \
    ~/Genome_Index/Genome/hg38/genome_hg38_renamed.fa \
    ~/miRNA/mirdeep2/output/SRR1759248/SRR1759248_vs_genome_h38.arf \
    ~/Genome_Index/mirbase_bowtie_index/mature_hsa_renamed.fa \
    none \
    ~/Genome_Index/mirbase_bowtie_index/hairpin_hsa_renamed.fa \
    -t hsa
```

The output directory has the following structure:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-05-at-8.49.17%E2%80%AFAM.png?resize=1024%2C59&ssl=1)

- The HTML files serve as your primary interface for exploring results. These interactive documents present comprehensive information about all detected miRNAs, including known miRNAs from your reference database and potentially novel miRNAs discovered in your samples. For each miRNA, you’ll find detailed expression data, sequence information, and structural predictions of miRNA precursors. These files are particularly valuable for visualizing and sharing your findings with colleagues.

- For computational analysis and data processing, miRDeep2 provides tabulated data in both TSV (tab-separated values) and CSV (comma-separated values) formats. These files contain the same information as the HTML files but in a format that’s easily imported into analysis software like R or Python. This makes them invaluable for downstream statistical analysis and visualization.

- The BED files provide crucial genomic context by storing the precise chromosomal coordinates of each detected miRNA. These files follow the standard BED format, making them compatible with genome browsers and other genomic analysis tools. You can use them to examine how your miRNAs relate to other genomic features or to validate their locations against known miRNA annotations.

- To help with troubleshooting and documentation, miRDeep2 generates detailed log files that record every command executed during the analysis. These files are essential for reproducing your analysis or identifying the source of any issues that might arise during processing.

- You’ll also notice various temporary files in the output directory. While these files help miRDeep2 during processing, they’re not typically needed for analysis and can be safely ignored once you’ve verified your results are complete.

## Choosing Between Methods

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-02-at-11.23.26%E2%80%AFPM.png?resize=350%2C293&ssl=1)

While both analysis methods provide comparable quantification results for mature miRNAs, miRDeep2 offers significant advantages beyond simple counting. Its sophisticated algorithms improve quantification accuracy by considering the structural features of miRNA precursors and their genomic context. This approach helps distinguish genuine miRNAs from other small RNA species that might contaminate your samples.

Moreover, miRDeep2 expands your analytical capabilities in several important ways. It can identify novel miRNAs by analyzing sequencing reads that don’t match known references, evaluate the secondary structure of potential precursor molecules, and assess the confidence of each prediction based on multiple criteria. These features make it particularly valuable for discovery-oriented research or when working with non-model organisms where many miRNAs remain uncharacterized.

Understanding these differences is crucial for choosing the right approach for your specific research needs. While the quick method might suffice for expression profiling of well-characterized miRNAs, miRDeep2’s comprehensive analysis provides the depth needed for more exploratory investigations.

## Conclusion

The field of miRNA analysis continues to evolve, offering increasingly sophisticated tools for understanding these crucial regulatory molecules. Whether you choose the quick method for rapid expression profiling or the comprehensive miRDeep2 approach for in-depth analysis, success lies in careful attention to detail and understanding your research needs.

Remember that miRNA analysis is both an art and a science. While the technical steps are important, equally crucial is understanding the biological context of your results. Always validate key findings using complementary methods, and stay current with the latest developments in the field.

## References

- Peng, Y., Croce, C. (2016). The role of MicroRNAs in human cancer. *Sig Transduct Target Ther*, 1, 15004. https://doi.org/10.1038/sigtrans.2015.4

- Ryan B, Joilin G and Williams JM (2015) Plasticity-related microRNA and their potential contribution to the maintenance of long-term potentiation. *Front. Mol. Neurosci.* **8**:4. [doi: 10.3389/fnmol.2015.00004](http://dx.doi.org/10.3389/fnmol.2015.00004)
