# How To Analyze ChIP-seq Data For Absolute Beginners Part 4: From FASTQ To Peaks With MACS2

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/)

- [Introduction: Expanding Your ChIP-seq Analysis Toolkit](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Introduction_Expanding_Your_ChIP-seq_Analysis_Toolkit)

  - [Why Learn Multiple Peak Calling Methods?](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Why_Learn_Multiple_Peak_Calling_Methods)
  - [The Evolution of MACS: From Original to MACS3](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#The_Evolution_of_MACS_From_Original_to_MACS3)

- [Understanding Peak Calling Algorithms](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Understanding_Peak_Calling_Algorithms)

  - [The Challenge of Identifying True Binding Sites](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#The_Challenge_of_Identifying_True_Binding_Sites)
  - [How MACS2 Works: Core Algorithm Concepts](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#How_MACS2_Works_Core_Algorithm_Concepts)
  - [MACS vs. HOMER: Choosing the Right Tool](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#MACS_vs_HOMER_Choosing_the_Right_Tool)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Setting_Up_Your_Analysis_Environment)

  - [Required Software Installation](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Required_Software_Installation)

- [The MACS2 Analysis Pipeline](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#The_MACS2_Analysis_Pipeline)

  - [Step 1: Basic Peak Calling with MACS2](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Step_1_Basic_Peak_Calling_with_MACS2)
  - [Step 2: Advanced Peak Calling Options](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Step_2_Advanced_Peak_Calling_Options)
  - [Step 3: Understanding MACS2 Output Files](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Step_3_Understanding_MACS2_Output_Files)
  - [Step 4: Peak Annotation with HOMER](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Step_4_Peak_Annotation_with_HOMER)
  - [Step 5: Alternative Peak Annotation with Bedtools](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Step_5_Alternative_Peak_Annotation_with_Bedtools)
  - [Step 6: Creating Browser Tracks from MACS2 Output](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Step_6_Creating_Browser_Tracks_from_MACS2_Output)

- [Advanced MACS2 Applications](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Advanced_MACS2_Applications)

  - [Calling Broad Peaks for Histone Modifications](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Calling_Broad_Peaks_for_Histone_Modifications)
  - [Differential Binding Analysis with MACS2 and bedtools](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Differential_Binding_Analysis_with_MACS2_and_bedtools)

- [Best Practices for MACS2 Analysis](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Best_Practices_for_MACS2_Analysis)

  - [Parameter Optimization](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Parameter_Optimization)
  - [Quality Control Checks](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Quality_Control_Checks)
  - [Computational Resources](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Computational_Resources)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Troubleshooting_Common_Issues)

  - [Problem: Few or No Peaks Detected](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Problem_Few_or_No_Peaks_Detected)
  - [Problem: Too Many Peaks](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Problem_Too_Many_Peaks)
  - [Problem: Peaks Don’t Match Expected Biology](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Problem_Peaks_Dont_Match_Expected_Biology)
  - [Problem: Error Messages During Peak Calling](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Problem_Error_Messages_During_Peak_Calling)

- [Conclusion: Next Steps in Your ChIP-seq Analysis](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Conclusion_Next_Steps_in_Your_ChIP-seq_Analysis)
- [References and Further Reading](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#References_and_Further_Reading)

  - [Share this:](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-4-from-fastq-to-peaks-with-macs2/#Like_this)

## Introduction: Expanding Your ChIP-seq Analysis Toolkit

Chromatin immunoprecipitation followed by sequencing (ChIP-seq) has revolutionized our understanding of protein-DNA interactions across the genome. In this fourth installment of our ChIP-seq tutorial series, we explore an alternative approach to peak calling using MACS2 (Model-based Analysis of ChIP-Seq), a widely used and statistically robust tool that complements the HOMER workflow covered in [Part 1](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/).

### Why Learn Multiple Peak Calling Methods?

Different experimental designs and biological questions may benefit from specific peak calling algorithms. By mastering both HOMER and MACS2, you’ll be able to:

- Choose the most appropriate tool for your specific research question

- Compare results from different algorithms for higher confidence findings

- Leverage the unique strengths of each approach

- Follow field-specific best practices that might favor one tool over another

### The Evolution of MACS: From Original to MACS3

The MACS tool has evolved substantially since its initial release in 2008, with each version offering significant improvements:

| Version | Key Features | Best Use Cases |
| --- | --- | --- |
| MACS (Original) | Dynamic Poisson model for peak calling | No longer recommended (deprecated) |
| MACS2 | Python implementation with improved statistics, support for broad peaks | Standard analysis, well-validated workflows |
| MACS3 | C++ core for speed, GPU support, multi-sample analysis | Very large datasets, computational efficiency |

In this tutorial, we’ll focus on MACS2, which represents the best balance of stability, features, and performance for most ChIP-seq applications. It’s also directly compatible with many downstream analysis tools, making it an excellent choice for beginners and experts alike.

## Understanding Peak Calling Algorithms

Before diving into the practical steps, let’s understand the conceptual foundations of peak calling in ChIP-seq data analysis.

### The Challenge of Identifying True Binding Sites

At its core, peak calling involves identifying genomic regions where ChIP-seq reads accumulate significantly above background levels, indicating protein binding sites. While conceptually straightforward, this process faces several technical challenges:

1. **Variable signal width:** Different proteins create different “footprints” on the genome

2. **Background noise variation:** Sequencing biases, chromatin accessibility, and mappability issues create uneven background

3. **Fragment complexity:** The sonication/fragmentation process during library preparation creates complex patterns

### How MACS2 Works: Core Algorithm Concepts

MACS2 employs several sophisticated strategies to address these challenges:

1. **Dynamic fragment size estimation:** Rather than relying on a fixed fragment size, MACS2 empirically models the fragment size distribution from your data

2. **Local bias correction:** Uses a dynamic Poisson distribution to account for local variations in background signal

3. **Peak shape analysis:** Identifies the summit (point of maximum binding affinity) within broader enriched regions

4. **Control sample normalization:** Intelligently scales control sample data to properly account for differences in sequencing depth

These features make MACS2 particularly effective for a wide range of ChIP-seq experiments, from sharp transcription factor binding sites to broad histone modification domains.

### MACS vs. HOMER: Choosing the Right Tool

Both MACS and HOMER are widely used peak calling tools, but they employ different algorithms that may be more suitable for specific experimental designs.

**Statistical model****Strengths****Best for**

| Feature | MACS2 | HOMER |
| --- | --- | --- |
|  | Dynamic Poisson/negative binomial | Binomial distribution |
|  | Robust background modeling, precise summit detection | Integrated workflow, excellent annotation |
|  | Complex genomes, histone modifications, precise binding sites | Transcription factors, histone modifications, projects needing integrated analysis |

**When to use MACS2:**

- You need a statistically robust peak caller with good sensitivity

- You’re working with complex genomes with variable background

- You need precise peak summit identification

- Downstream tools require MACS2 output format

**When to use HOMER:**

- You want an integrated analysis pipeline from peak calling to motif discovery

- You need extensive annotation capabilities

- You’re interested in comprehensive downstream analysis within the same framework

## Setting Up Your Analysis Environment

We’ll continue using the Conda environment created in our previous tutorial. If you haven’t completed [Part 1](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/), you may want to start there to ensure your computational environment is properly configured.

### Required Software Installation

```bash
#-----------------------------------------------
# STEP 0: Install MACS2 and supporting tools
#-----------------------------------------------

# Activate the ChIP-seq environment created in the previous tutorial
conda activate ~/Env_Homer

# Install MACS2 and additional tools
conda install -c bioconda macs2 gffread -y

# Verify installation
macs2 --version
```

> 

**Troubleshooting Tip:** If you encounter Python-related errors during MACS2 installation, try creating a clean environment with Python 3.7-3.9, as newer Python versions may have compatibility issues with some bioinformatics tools.

## The MACS2 Analysis Pipeline

Now let’s proceed with the analysis, picking up from where we left off in [Part 1](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/) after generating aligned, filtered BAM files.

### Step 1: Basic Peak Calling with MACS2

The most basic MACS2 command requires the treatment sample (ChIP), control sample (Input), and a few parameters to identify enriched regions:

```bash
#-----------------------------------------------
# STEP 1: Basic peak calling with MACS2
#-----------------------------------------------

# Create output directory
mkdir -p ~/GSE104247/macs2/SRR6117703_USF2

# Call peaks with MACS2
# Note: -B flag is used to generate bedGraph files for visualization
macs2 callpeak \
    -t ~/GSE104247/bam/SRR6117703_USF2_sorted_dedup_filtered.bam \  # ChIP/treatment file
    -c ~/GSE104247/bam/SRR6117732_USF2_Input_sorted_dedup_filtered.bam \  # Input/control file
    -f BAM \                       # Input file format
    -g hs \                        # Genome size ('hs' = human)
    -n SRR6117703_USF2 \           # Prefix for output files
    --outdir ~/GSE104247/macs2/SRR6117703_USF2 \  # Output directory
    -B                             # Generate bedGraph files
```

> 

**What’s Happening Here:** MACS2 analyzes the read distributions in both the ChIP and Input samples, models the background, and identifies regions with statistically significant enrichment of ChIP reads relative to Input.

**MACS2 Parameter Explanation:**

- `-t`: Treatment/ChIP BAM file(s)

- `-c`: Control/Input BAM file(s)

- `-f`: Format of input files (BAM, SAM, BED, etc.)

- `-g`: Effective genome size (‘hs’ for human, ‘mm’ for mouse, or specific number)

- `-n`: Prefix for output file names

- `--outdir`: Directory for saving results

### Step 2: Advanced Peak Calling Options

For more control over the peak calling process, MACS2 offers additional parameters for fine-tuning:

```bash
#-----------------------------------------------
# STEP 2: Advanced peak calling with optimal parameters
#-----------------------------------------------

# Call peaks with advanced parameters
macs2 callpeak \
    -t ~/GSE104247/bam/SRR6117703_USF2_sorted_dedup_filtered.bam \  # ChIP file
    -c ~/GSE104247/bam/SRR6117732_USF2_Input_sorted_dedup_filtered.bam \  # Input file
    -f BAM \                       # Input file format
    -g hs \                        # Genome size ('hs' = human)
    -n SRR6117703_USF2_advanced \  # Prefix for output files
    --outdir ~/GSE104247/macs2/SRR6117703_USF2 \  # Output directory
    -q 0.01 \                      # q-value (FDR) cutoff
    --keep-dup auto \              # Auto determine how to handle duplicates
    --nomodel \                    # Skip model building (for TF with well-defined peaks)
    --extsize 200 \                # Set extension size to 200bp
    --shift -100 \                 # Shift read 5' end by this distance
    --call-summits \               # Detect peak summits for each peak
    -B                             # Generate bedGraph files (required for browser tracks)
```

**Advanced Parameter Explanation:**

- `-q`: q-value (minimum FDR) cutoff, more stringent than p-value

- `--keep-dup`: How to handle duplicate reads (‘auto’, ‘all’, or integer)

- `--nomodel`: Skip model building (useful when you know fragment size)

- `--extsize`: Set extension size when –nomodel is used

- `--shift`: Shift reads by this distance

- `--call-summits`: Detect summits for each peak (precise binding sites)

- `--broad`: For broad peaks like histone modifications (not used here since USF2 is a TF)

- `--broad-cutoff`: Cutoff for broad region (used with –broad)

### Step 3: Understanding MACS2 Output Files

MACS2 generates several output files with complementary information:

```bash
#-----------------------------------------------
# STEP 3: Examining MACS2 output files
#-----------------------------------------------

# List the output files
ls -lh ~/GSE104247/macs2/SRR6117703_USF2/

# Output explanation:
# SRR6117703_USF2_peaks.narrowPeak - Peak coordinates (BED format with extra columns)
# SRR6117703_USF2_peaks.xls - Peak info in tabular format (readable in Excel)
# SRR6117703_USF2_summits.bed - Summit positions (precise binding locations)
# SRR6117703_USF2_model.r - R script for model visualization (if model was built)
# SRR6117703_USF2_control_lambda.bdg - Control lambda background
# SRR6117703_USF2_treat_pileup.bdg - Treatment pileup signal

# Examine the first few peaks
head -n 5 ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks.narrowPeak
```

**Understanding the narrowPeak Format:**The `narrowPeak` file is a BED format with additional columns:

1. Chromosome

2. Start position

3. End position

4. Name

5. Score (integer score for display)

6. Strand

7. Signal value (statistical enrichment)

8. p-value (-log10)

9. q-value (FDR, -log10)

10. Summit position relative to peak start

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/04/Screenshot-2025-04-08-at-7.36.27%E2%80%AFPM.png?resize=1024%2C253&ssl=1)

### Step 4: Peak Annotation with HOMER

After calling peaks with MACS2, we can use HOMER’s annotation capabilities to identify nearby genes and genomic features:

```bash
#-----------------------------------------------
# STEP 4: Annotating peaks with HOMER
#-----------------------------------------------

# Annotate MACS2 peaks using HOMER
annotatePeaks.pl \
    ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks.narrowPeak \  # Input peaks
    hg38 \                                   # Reference genome
    -gtf ~/homer/data/genomes/hg38/hg38.gtf \  # Gene annotations (if available)
    -go ~/GSE104247/macs2/SRR6117703_USF2/GO \  # Gene Ontology output directory
    -genomeOntology ~/GSE104247/macs2/SRR6117703_USF2/genomeOntology \  # Genome feature output
    > ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks_annotated.tsv  # Output file
```

> 

**Note:** This step shows the flexibility of bioinformatics workflows – we’re combining the statistical power of MACS2 for peak calling with HOMER’s excellent annotation capabilities.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/04/Screenshot-2025-04-08-at-7.38.13%E2%80%AFPM.png?resize=1024%2C163&ssl=1)

### Step 5: Alternative Peak Annotation with Bedtools

For those who prefer to use bedtools for annotation, we can create a more customized annotation workflow:

```bash
#-----------------------------------------------
# STEP 5: Alternative peak annotation with bedtools
#-----------------------------------------------

# 5.1: Prepare gene annotation file from GTF
#-------------------------------------------

# First, download the Gencode annotation if you don't have it
# Create a gene annotation BED file from GTF
mkdir -p ~/GSE104247/annotations
cd ~/GSE104247/annotations

# Download GENCODE annotation (if needed)
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_38/gencode.v38.annotation.gtf.gz
gunzip gencode.v38.annotation.gtf.gz

# Extract gene information to create a simplified BED file
awk '$3 == "gene"' ~/GSE104247/annotations/gencode.v38.annotation.gtf | \
awk 'BEGIN { OFS = "\t" }
{
    gene_id = $0;
    gene_name = $0;

    gsub(/.*gene_id "/, "", gene_id);
    gsub(/".*/, "", gene_id);

    gsub(/.*gene_name "/, "", gene_name);
    gsub(/".*/, "", gene_name);

    print $1, $4 - 1, $5, gene_id "|" gene_name, ".", $7;
}' > ~/GSE104247/annotations/genes_hg38.bed

# 5.2: Sort the BED files for bedtools operations
#-----------------------------------------------

# Sort gene annotation BED file
bedtools sort -i ~/GSE104247/annotations/genes_hg38.bed > \
  ~/GSE104247/annotations/genes_hg38.sorted.bed

# Sort peaks file
bedtools sort -i ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks.narrowPeak > \
  ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks.sorted.narrowPeak

# 5.3: Find nearest genes to peaks
#---------------------------------

# Find the closest gene to each peak
bedtools closest \
  -a ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks.sorted.narrowPeak \
  -b ~/GSE104247/annotations/genes_hg38.sorted.bed \
  -d \  # Include distance to closest feature
  > ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks_closest_gene.tsv

# 5.4: Quantify read coverage in peaks
#------------------------------------

# Count reads in each peak
bedtools multicov \
  -bams ~/GSE104247/bam/SRR6117703_USF2_sorted_dedup_filtered.bam \
         ~/GSE104247/bam/SRR6117732_USF2_Input_sorted_dedup_filtered.bam \
  -bed ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks.sorted.narrowPeak \
  > ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_peaks_read_counts.tsv
```

> 

**Bedtools vs. HOMER Annotation:** HOMER provides a more integrated and biologically interpreted annotation, while bedtools gives you more flexibility for custom analyses. For beginners, HOMER’s annotation is often more immediately useful, but learning the bedtools approach provides valuable skills for customized analyses.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/04/Screenshot-2025-04-08-at-7.40.57%E2%80%AFPM.png?resize=1024%2C128&ssl=1)

### Step 6: Creating Browser Tracks from MACS2 Output

MACS2 generates bedGraph files that can be converted to bigWig format for visualization in genome browsers:

```bash
#-----------------------------------------------
# STEP 6: Creating genome browser tracks
#-----------------------------------------------

# 6.1: Get chromosome sizes for the reference genome
#--------------------------------------------------

# Fetch chromosome sizes for hg38
fetchChromSizes hg38 > ~/GSE104247/annotations/hg38.chrom.sizes

# 6.2: Convert bedGraph files to bigWig format
#--------------------------------------------

# Convert treatment pileup to bigWig
bedGraphToBigWig \
  ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_treat_pileup.bdg \
  ~/GSE104247/annotations/hg38.chrom.sizes \
  ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_treat_pileup.bw

# Convert control lambda to bigWig
bedGraphToBigWig \
  ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_control_lambda.bdg \
  ~/GSE104247/annotations/hg38.chrom.sizes \
  ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_control_lambda.bw

# 6.3: Create normalized fold-enrichment track (optional)
#------------------------------------------------------

# First, create a fold-enrichment bedGraph
macs2 bdgcmp \
  -t ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_treat_pileup.bdg \
  -c ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_control_lambda.bdg \
  -o ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_FE.bdg \
  -m FE  # Fold Enrichment method

# Then convert to bigWig
bedGraphToBigWig \
  ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_FE.bdg \
  ~/GSE104247/annotations/hg38.chrom.sizes \
  ~/GSE104247/macs2/SRR6117703_USF2/SRR6117703_USF2_FE.bw
```

> 

**Visualization Tip:** The fold-enrichment track (FE.bw) is often the most informative for visualizing ChIP-seq data, as it normalizes for background signal and highlights truly enriched regions.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/04/Screenshot-2025-04-08-at-7.45.20%E2%80%AFPM.png?resize=1024%2C356&ssl=1)

## Advanced MACS2 Applications

### Calling Broad Peaks for Histone Modifications

While our USF2 example focuses on a transcription factor with narrow peaks, histone modifications often create broad domains of enrichment. MACS2 can be adapted for these scenarios:

```bash
# Example command for calling broad peaks (e.g., for H3K27me3)
macs2 callpeak \
    -t H3K27me3_ChIP.bam \          # ChIP sample
    -c H3K27me3_Input.bam \         # Input control
    -f BAM \                        # Input format
    -g hs \                         # Genome size
    -n H3K27me3 \                   # Output name prefix
    --broad \                       # Call broad peaks
    --broad-cutoff 0.1 \            # q-value cutoff for broad regions
    --nomodel \                     # Skip model building
    --extsize 300                   # Set extension size
```

### Differential Binding Analysis with MACS2 and bedtools

For comparing binding between two conditions (e.g., treatment vs. control), we can combine MACS2 with bedtools:

```bash
# Example workflow for differential binding analysis
# 1. Call peaks in both conditions
macs2 callpeak -t condition1.bam -c input1.bam -n condition1 -g hs -B
macs2 callpeak -t condition2.bam -c input2.bam -n condition2 -g hs -B

# 2. Find overlapping peaks
bedtools intersect \
  -a condition1_peaks.narrowPeak \
  -b condition2_peaks.narrowPeak \
  -wa -wb > overlapping_peaks.bed

# 3. Find unique peaks in condition 1
bedtools intersect \
  -a condition1_peaks.narrowPeak \
  -b condition2_peaks.narrowPeak \
  -v > condition1_unique_peaks.bed

# 4. Find unique peaks in condition 2
bedtools intersect \
  -a condition2_peaks.narrowPeak \
  -b condition1_peaks.narrowPeak \
  -v > condition2_unique_peaks.bed
```

> 

**Note:** For more sophisticated differential binding analysis, consider specialized tools like DiffBind, limma, edgeR, DESeq2 (R/Bioconductor) as covered in Part 3 of our tutorial series.

## Best Practices for MACS2 Analysis

### Parameter Optimization

- **Genome size (-g):** Always use the appropriate genome size parameter (‘hs’ for human, ‘mm’ for mouse, or specific number for other organisms)

- **q-value threshold:** Start with the default (-q 0.05) and adjust based on your confidence requirements (more stringent for greater confidence)

- **Fragment size:** For TFs with well-defined binding sites, using –nomodel with –extsize can improve specificity

- **Duplicate handling:** For high-quality libraries, ‘–keep-dup auto’ is recommended

### Quality Control Checks

- **Number of peaks:** An unusually high (>100,000) or low (<1,000) number of peaks for a TF may indicate issues

- **Peak distribution:** Check genomic distribution (promoters, enhancers, etc.) for expected patterns

- **Signal-to-noise ratio:** Compare enrichment at peaks vs. random genomic regions

- **Motif enrichment:** Top peaks should be enriched for the expected binding motif

- **Replicate concordance:** High overlap between biological replicates indicates reliability

### Computational Resources

- **Memory usage:** MACS2 is relatively memory-efficient but allocate at least 8GB RAM for human genome analysis

- **Disk space:** Keep at least 10GB free space for temporary files and results

- **Processing time:** Expect 20-60 minutes for a typical human ChIP-seq sample on a modern workstation

## Troubleshooting Common Issues

### Problem: Few or No Peaks Detected

**Potential Causes:**

- Poor antibody specificity or efficiency

- Low sequencing depth

- Inefficient immunoprecipitation

- Overly stringent peak calling parameters

**Solutions:**

- Try adjusting q-value threshold (-q 0.1 instead of 0.01)

- Use less stringent parameters for peak calling

- Examine browser tracks to see if any enrichment is visible but below threshold

- Check IP efficiency metrics from your experimental protocol

### Problem: Too Many Peaks

**Potential Causes:**

- Poor quality control/high background

- Incorrect parameters for your experiment type

- Artifacts from DNA shearing or library preparation

**Solutions:**

- Use more stringent q-value threshold (e.g., -q 0.001)

- Check input normalization (–ratio parameter)

- Filter out blacklisted regions known to cause artifacts

- Examine genomic distribution of peaks for unusual patterns

### Problem: Peaks Don’t Match Expected Biology

**Potential Causes:**

- Wrong antibody or target

- Sample mix-up or contamination

- Unexpected biological phenomenon

**Solutions:**

- Perform motif analysis to confirm binding specificity

- Check genomic distribution of peaks

- Compare with published datasets for the same factor

- Validate key findings with orthogonal methods (e.g., ChIP-qPCR)

### Problem: Error Messages During Peak Calling

**Common Error:** “Error: Cannot open BAM file”

**Solutions:**

- Check file paths and permissions

- Verify BAM files are properly formatted and indexed

- Run “samtools quickcheck” on your BAM files

**Common Error:** “ValueError: cannot use broad and call-summits together”

**Solution:** Choose either –broad OR –call-summits based on your experiment type

## Conclusion: Next Steps in Your ChIP-seq Analysis

Congratulations! You’ve successfully called peaks using MACS2, a powerful and flexible peak calling algorithm widely used in the field. From here, you can continue your analysis journey:

1. **Peak Visualization:** Use the bigWig files you generated to visualize your data in genome browsers like IGV or UCSC (as covered in [Part 2](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-2-visualizing-chip-seq-data/))

2. **Motif Analysis:** Discover DNA sequence motifs within your peaks using tools like HOMER or MEME ([Part 3](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-3-differential-binding-analysis-and-motif-discovery/))

3. **Differential Binding Analysis:** Compare binding patterns between different conditions ([Part 3](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-3-differential-binding-analysis-and-motif-discovery/))

4. **Integration with Other Data Types:** Correlate binding with gene expression, chromatin accessibility, or other epigenetic marks

Remember that peak calling is just one step in understanding the biological significance of protein-DNA interactions. The true insights come from integrating these findings with other genomic and functional data to build a comprehensive picture of gene regulation.

## References and Further Reading

- Zhang Y, Liu T, Meyer CA, et al. Model-based Analysis of ChIP-Seq (MACS). Genome Biology. 2008;9(9):R137.

- Feng J, Liu T, Qin B, Zhang Y, Liu XS. Identifying ChIP-seq enrichment using MACS. Nature Protocols. 2012;7(9):1728-1740.

- Bailey T, Krajewski P, Ladunga I, et al. Practical Guidelines for the Comprehensive Analysis of ChIP-seq Data. PLoS Computational Biology. 2013;9(11):e1003326.

- ENCODE Consortium. ENCODE Guidelines for Transcription Factor ChIP-seq. [link](https://www.encodeproject.org/chip-seq/transcription_factor/)

- Wilbanks EG, Facciotti MT. Evaluation of algorithm performance in ChIP-seq peak detection. PLoS One. 2010 Jul 8;5(7):e11471. doi: 10.1371/journal.pone.0011471. PMID: 20628599; PMCID: PMC2900203.

---

*This tutorial is part of the NGS101.com beginner’s guide to next-generation sequencing analysis. For more advanced ChIP-seq analysis techniques, including differential binding analysis and motif discovery, please refer to [Part 3](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-3-differential-binding-analysis-and-motif-discovery/) of this series.*

**Keywords:** ChIP-seq, MACS2, peak calling, protein-DNA interactions, transcription factors, histone modifications, next-generation sequencing, bioinformatics, genomics, epigenetics
