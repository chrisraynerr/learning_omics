# How to Analyze RNAseq Data for Absolute Beginners Part 2: From Fastq to Counts – Best Practices

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/)

- [Introduction](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Introduction)
- [A Brief Review of RNAseq Technology](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#A_Brief_Review_of_RNAseq_Technology)
- [Step 1: Download the Required Files](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Step_1_Download_the_Required_Files)
- [Step 2: Create Folders for Your Files](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Step_2_Create_Folders_for_Your_Files)
- [Step 3: Trim the Adapters](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Step_3_Trim_the_Adapters)
- [Step 4: Align to the Reference Genome](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Step_4_Align_to_the_Reference_Genome)
- [Step 5: Quantify Gene Expression](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Step_5_Quantify_Gene_Expression)
- [Repeat Steps 3-5 for Other Samples](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Repeat_Steps_3-5_for_Other_Samples)
- [Check Data Quality and Mapping Rates](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Check_Data_Quality_and_Mapping_Rates)
- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Conclusion)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/#Like_this)

## Introduction

The most straightforward way to obtain a count table is to request it directly from your sequencing company or your institution’s sequencing core. This option may involve an additional fee. However, for those eager to learn or save money, let’s walk through the process together.

Before we dive in, a quick reminder: If you haven’t set up your analysis environment yet, be sure to check out our previous post, “[How to Analyze RNAseq Data for Absolute Beginners Part 1: Environment Setup](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/)“. This guide will ensure you have all the necessary tools and software in place before we begin processing our data. Having a properly configured environment is crucial for a smooth analysis workflow, so don’t skip this step if you’re starting from scratch!

## A Brief Review of RNAseq Technology

In simple terms, the RNAseq process involves:

1. Extracting mRNAs from subject tissues

2. Fragmenting the mRNAs into smaller pieces

3. Sequencing these fragments

4. Mapping the sequences back to the reference genome

5. Counting how many of these pieces (reads) correspond to each gene

The number of reads falling within each gene region represents that gene’s expression level.

Sequencing can be either single-end (sequencing from one end of the mRNA pieces) or paired-end (sequencing from both ends). Paired-end sequencing has become more popular due to its numerous advantages, including higher accuracy, improved quantification, and reduced ambiguity. This tutorial will focus on paired-end RNA sequencing data.

## Step 1: Download the Required Files

Before we begin, ensure you have access to a powerful Linux computer, such as your lab server or your institution’s High-Performance Computing (HPC) cluster. A regular laptop won’t suffice for this process.

For this tutorial, we’ll use example data downloaded from NCBI GEO [GSE259357](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=gse259357), derived from mouse normal pancreas and early pancreatic neoplasia samples.

For those who want to download the FASTQ files for practicing, here is how:

- Install the “sra-tools” if you haven’t done so in the [Part 1](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/) tutorial.

```bash
# Activate our RNA-seq environment
conda activate rnaseq_env

# Install sra-tools
mamba install sra-tools
mamba update sra-tools
```

- Click on the [SRA Run Selector](https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA1081358&o=acc_s%3Aa) at the bottom of [GSE259357](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=gse259357) webpage.

- Open your terminal on your Linux system and download all the files in the “Run” column of the table at the bottom of the page using the command line:

```bash
# Change the directory
cd ~/Downloads

# Download the FASTQ files
fasterq-dump SRR28119110
fasterq-dump SRR28119111
fasterq-dump SRR28119112
fasterq-dump SRR28119113

# If a direct fasterq-dump fails, use this two-step "Fetch and Extract" method:
# Step 1: Download the SRA archive
prefetch SRR28119110
# Step 2: Extract the fastq files from the downloaded local file
fasterq-dump SRR28119110
```

- Once all the fastq files have been downloaded. Compress the files using the command:

```bash
# Compress the FASTQ files
gzip *.fastq
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/2024-10-20_15-25.png?resize=914%2C169&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/2024-10-20_15-25_1.png?resize=1024%2C201&ssl=1)

First, download the prebuilt STAR Index for mouse (mm10):

1. Visit [refgenie](http://refgenomes.databio.org/index)

2. Download the mm10 STAR Index

3. Name the folder “star_index_mm10” (avoid spaces in the name)

4. Remember the folder’s location on your computer

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Pasted-image-20241010170826.png?resize=1024%2C885&ssl=1)

Next, download the corresponding GTF file (gene annotation file):

1. Go to [GENCODE](https://www.gencodegenes.org/mouse/release_M25.html)

2. Download the appropriate GTF file

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-12-at-7.28.46-PM.png?resize=1024%2C245&ssl=1)

#### Building A Customized STAR Index

In some cases, you may need to create a custom STAR index using your own genome file. The STAR (Spliced Transcripts Alignment to a Reference) aligner requires this indexed genome for efficient read mapping. Here’s an example command you can use on a Linux system:

```
STAR \
    --runThreadN 12 \                          # Use 12 CPU threads for parallel processing
    --limitGenomeGenerateRAM 200000000000 \    # Set RAM limit to 200GB
    --runMode genomeGenerate \                 # Specify that we're generating a genome index
    --genomeDir ~/Genome_Index/STAR_GRCm38 \   # Directory where the index will be stored
    --genomeFastaFiles ~/Genome_Index/Genome/GRCm38/GRCm38.primary_assembly.genome.fa \ # Input genome FASTA file
    --sjdbGTFfile ~/Genome_Index/GTF/GRCm38/gencode.vM25.annotation.gtf                # Gene annotation file
```

Building a STAR index is a computationally intensive process that requires substantial system resources. The example above needs approximately 200GB of RAM and benefits from multiple CPU cores. For the mouse genome (GRCm38), you can obtain both the genome FASTA file and the corresponding GTF annotation file from the [GENCODE](https://www.gencodegenes.org/mouse/release_M25.html) database as shown above. Before running the command, make sure to modify the file paths to match the actual locations on your system. It’s also advisable to check your system’s available RAM and adjust the –limitGenomeGenerateRAM parameter accordingly to prevent memory-related errors during the indexing process.

## Step 2: Create Folders for Your Files

Use the following commands to create the necessary folders. Replace the paths with your desired locations:

```bash
# Create a folder for the STAR Index and GTF file
mkdir -p ~/Tutorials/RNAseq/star_index_mm10
mkdir -p ~/Tutorials/RNAseq/GTF

# Create a folder for the fastq files
mkdir -p ~/Tutorials/RNAseq/raw

# Create folders for the adapter-trimming results
mkdir -p ~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110

# Create folders for the mapping results
mkdir -p ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110
```

To download files directly using the terminal:

```bash
# Navigate to the STAR index folder
cd ~/Tutorials/RNAseq/star_index_mm10

# Download the index files
wget file_url
```

Replace “`file_url`” with the actual download link (right-click on the file hyperlink and select “copy link”).

To transfer FASTQ files to your new folder:

```
mv ~/Tutorials/RNAseq/old_folder/*.fastq.gz ~/Tutorials/RNAseq/raw
```

This command moves all files ending with `.fastq.gz` from the old folder to the new `raw` folder.

## Step 3: Trim the Adapters

Use the following command to trim adapters from your FASTQ files:

```
trim_galore --fastqc --paired --cores 8 \
  ~/Tutorials/RNAseq/GSE259357/raw/SRR28119110_R1_001.fastq.gz \
  ~/Tutorials/RNAseq/GSE259357/raw/SRR28119110_R2_001.fastq.gz \
  -o ~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110
```

After trimming, you’ll find the following files in the trimmed folder:

```
ls ~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-12-at-5.00.26-PM.png?resize=1024%2C54&ssl=1)

The trimmed FASTQ files (`SRR28119110_R1_001_val_1.fq.gz` and `SRR28119110_R2_001_val_2.fq.gz`) will be used in the next step.

## Step 4: Align to the Reference Genome

Use this command to align your data to the reference genome:

```
STAR --genomeDir ~/Tutorials/RNAseq/star_index_mm10 \
  --runThreadN 20 --readFilesIn \
  ~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110/SRR28119110_R1_001_val_1.fq.gz \
  ~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110/SRR28119110_R2_001_val_2.fq.gz \
  --outSAMtype BAM SortedByCoordinate \
  --outSAMunmapped Within \
  --outSAMattributes Standard \
  --readFilesCommand zcat \
  --outFileNamePrefix ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110
```

After alignment, you’ll see the following files in the aligned folder:

```
ls ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-12-at-7.05.21-PM.png?resize=1024%2C66&ssl=1)

The file `SRR28119110_trimmedAligned.sortedByCoord.out.bam` contains the aligned results needed for the next step.

## Step 5: Quantify Gene Expression

Use this command to quantify gene expression from the aligned results:

```
featureCounts -T 8 -t exon -g gene_name -s 0 \
  -a ~/Tutorials/RNAseq/GTF/gencode.vM25.annotation.gtf \
  -o ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110/SRR28119110_featureCounts_exon.txt \
  ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110/SRR28119110_trimmedAligned.sortedByCoord.out.bam
```

To view the resulting count table:

```
less ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110/SRR28119110_featureCounts_exon.txt
```

Press ‘q’ to exit the preview. You can also open this file using Microsoft Excel.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-12-at-7.37.02-PM.png?resize=418%2C347&ssl=1)

The columns in the count table are: “Gene Name”, “Chromosome”, “Start”, “End”, “Strand”, “Gene Length”, and “Counts”.

## Repeat Steps 3-5 for Other Samples

To process all samples efficiently:

1. Create a text file for each sample (e.g., `~/Tutorials/RNAseq/GSE259375/RNAseq_Quant_SRR28119110.sh`) containing the code from steps 3-5.

2. Execute the file using: bash ~/Tutorials/RNAseq/GSE259375/RNAseq_Quant_SRR28119110.sh

3. Repeat for all samples.

## Check Data Quality and Mapping Rates

After quantifying all samples, run:

```
multiqc ~/Tutorials/RNAseq/GSE259357/
```

This command generates a `multiqc_report.html` file in the `~/Tutorials/RNAseq/GSE259357/` folder, summarizing the quality of FASTQ files and mapping statistics.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-12-at-7.19.54-PM.png?resize=1024%2C642&ssl=1)

As shown in the image, we’ve achieved excellent alignment rates (about 80%) to the mouse genome.

## Conclusion

Congratulations! We’ve successfully quantified gene expression for all samples (4 in this case). With count tables for each sample, we’re now ready to perform differential gene expression analysis. Take a moment to appreciate your progress, and prepare for the next stage of our journey in [How to Analyze RNAseq Data for Absolute Beginners (Part 3: From Count Table to DEGs – Best Practices)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/).

  Want to run this whole pipeline on your own data? [Join the live workshop →](https://ngs101.com/workshop/rna-seq-workshop/)
