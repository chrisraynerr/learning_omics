# How to Choose the Best Genome Aligner for a Specific NGS Dataset &#8212; A Beginner's Guide to Read Mapping Tools

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/)

---

*From short-read DNA to single-cell and long-read RNA — match your sequencing experiment to the right aligner, and learn exactly how to install and run it*

---

Every next-generation sequencing (NGS) analysis begins with the same deceptively simple question: where did each of my millions of short DNA or RNA reads come from in the genome? Answering that question is the job of an **aligner** (also called a **mapper**), and choosing the wrong one is one of the most common — and most costly — mistakes a beginner can make. Align spliced RNA reads with a DNA aligner and you will lose every read that crosses an exon boundary. Feed noisy long reads to a short-read tool and almost nothing will map at all.

The good news is that there is a well-established aligner for every major experiment type, and picking the right one is mostly a matter of answering a few questions about your data: Is it DNA or RNA? Short reads or long reads? Are you counting genes or calling variants? Is it single-cell?

This tutorial walks you through the full landscape of modern aligners, organized by the kind of experiment you are running. For each tool you will learn what it is good at, **which scenario it is the right choice for**, and exactly how to install and run it with fully commented commands. No prior alignment experience is assumed — if you are a wet-lab scientist or a beginner moving into computational biology, this guide is written for you.

> 

**Prerequisites:** Comfort on the Linux command line and a working [conda/mamba installation](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html). All installation commands below use the Bioconda channel, the standard source for bioinformatics software. If you have never set up conda, install [Miniforge](https://github.com/conda-forge/miniforge) first, which ships with the fast `mamba` solver.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/)

- [What Is a Genome Aligner and Why Do You Need One?](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#What_Is_a_Genome_Aligner_and_Why_Do_You_Need_One)

  - [The Core Problem: Millions of Reads, One Genome](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#The_Core_Problem_Millions_of_Reads_One_Genome)
  - [Why Mapping Reads Is Harder Than It Looks](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Why_Mapping_Reads_Is_Harder_Than_It_Looks)
  - [The Two Big Ideas Behind Every Aligner](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#The_Two_Big_Ideas_Behind_Every_Aligner)
  - [Why One Aligner Cannot Do Everything](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Why_One_Aligner_Cannot_Do_Everything)

- [Category 1: DNA-Seq Aligners for Short Reads (Unspliced Alignment)](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Category_1_DNA-Seq_Aligners_for_Short_Reads_Unspliced_Alignment)

  - [BWA (Burrows-Wheeler Aligner): The Industry Standard for DNA](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#BWA_Burrows-Wheeler_Aligner_The_Industry_Standard_for_DNA)
  - [Bowtie2: The Default for ChIP-Seq and ATAC-Seq](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Bowtie2_The_Default_for_ChIP-Seq_and_ATAC-Seq)
  - [Minimap2 (Short-Read Mode): The Speed Option](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Minimap2_Short-Read_Mode_The_Speed_Option)
  - [SOAP2 and Novoalign: Specialist and Legacy Options](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#SOAP2_and_Novoalign_Specialist_and_Legacy_Options)

- [Category 2: Bulk RNA-Seq Aligners (Splice-Aware Genomic Mapping)](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Category_2_Bulk_RNA-Seq_Aligners_Splice-Aware_Genomic_Mapping)

  - [STAR: The Gold Standard for Bulk RNA-Seq](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#STAR_The_Gold_Standard_for_Bulk_RNA-Seq)
  - [HISAT2: The Memory-Efficient Alternative](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#HISAT2_The_Memory-Efficient_Alternative)
  - [GSNAP / GMAP: Maximum Sensitivity for Complex Transcripts](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#GSNAP_GMAP_Maximum_Sensitivity_for_Complex_Transcripts)
  - [TopHat2: Legacy — Do Not Use for New Projects](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#TopHat2_Legacy_%E2%80%94_Do_Not_Use_for_New_Projects)

- [Category 3: Transcript Quantification Tools (Alignment-Free / Pseudo-Aligners)](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Category_3_Transcript_Quantification_Tools_Alignment-Free_Pseudo-Aligners)

  - [Salmon: Fast, Bias-Aware Quantification](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Salmon_Fast_Bias-Aware_Quantification)
  - [Kallisto: The Lightweight Speed Champion](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Kallisto_The_Lightweight_Speed_Champion)
  - [Sailfish: The Predecessor](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Sailfish_The_Predecessor)

- [Category 4: Single-Cell RNA-Seq (scRNA-Seq) Aligners](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Category_4_Single-Cell_RNA-Seq_scRNA-Seq_Aligners)

  - [Cell Ranger: The Official 10x Pipeline](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Cell_Ranger_The_Official_10x_Pipeline)
  - [STARsolo: The Fast, Free Cell Ranger Alternative](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#STARsolo_The_Fast_Free_Cell_Ranger_Alternative)
  - [Alevin / Alevin-fry: Fast and Low-Memory (Salmon Family)](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Alevin_Alevin-fry_Fast_and_Low-Memory_Salmon_Family)
  - [kallisto | bustools (kb-python): The Pseudo-Alignment Pipeline](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#kallisto_bustools_kb-python_The_Pseudo-Alignment_Pipeline)

- [Category 5: Third-Generation Long-Read Aligners (DNA and RNA)](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Category_5_Third-Generation_Long-Read_Aligners_DNA_and_RNA)

  - [Minimap2: The Universal Long-Read Benchmark](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Minimap2_The_Universal_Long-Read_Benchmark)
  - [NGMLR: Structural-Variant-Aware Alignment](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#NGMLR_Structural-Variant-Aware_Alignment)
  - [BLAT: Legacy Alignment of Full-Length cDNAs and ESTs](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#BLAT_Legacy_Alignment_of_Full-Length_cDNAs_and_ESTs)

- [Quick Decision Guide: Which Aligner Should I Use?](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Quick_Decision_Guide_Which_Aligner_Should_I_Use)
- [Best Practices for Read Alignment](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Best_Practices_for_Read_Alignment)
- [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Common_Pitfalls_and_How_to_Avoid_Them)
- [Troubleshooting Common Alignment Errors](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Troubleshooting_Common_Alignment_Errors)
- [Conclusion: Matching the Aligner to the Experiment](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Conclusion_Matching_the_Aligner_to_the_Experiment)
- [References](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#References)

  - [Share this:](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Share_this)
  - [Like this:](https://ngs101.com/how-to-choose-the-best-genome-aligner-for-a-specific-ngs-dataset-a-beginners-guide-to-read-mapping-tools/#Like_this)

## What Is a Genome Aligner and Why Do You Need One?

### The Core Problem: Millions of Reads, One Genome

A sequencing machine does not read a chromosome end to end. Instead, it shreds the DNA or RNA into millions or billions of short fragments and reads each fragment independently, producing short strings of letters called **reads** (typically 50-150 bases for Illumina short-read sequencing). A read on its own is meaningless — a 100-letter fragment of `ACGT...` tells you nothing until you know **where in the genome it originated**.

An aligner solves exactly this problem. Given a **reference genome** (a known, assembled sequence for your species) and a file of reads (usually FASTQ format), the aligner finds the location — the chromosome and coordinate — where each read best matches the reference. The output is a **SAM/BAM file**: a table that records, for every read, where it mapped, how well it matched, and the exact differences (mismatches, insertions, deletions) between the read and the reference.

That mapped position is the foundation for nearly everything downstream: calling genetic variants, counting how many reads landed on each gene, finding peaks in ChIP-seq, or measuring chromatin accessibility in ATAC-seq.

### Why Mapping Reads Is Harder Than It Looks

If genomes were short and reads were perfect, alignment would be a trivial text search. Three realities make it a genuine computational challenge:

**1. Scale.** A human genome is about 3.1 billion bases, and a single experiment produces tens to hundreds of millions of reads. Naively comparing every read against every position in the genome would take years of compute time. Aligners solve this by first building an **index** of the reference — a compressed, searchable data structure that lets the tool jump straight to candidate locations instead of scanning the whole genome. You build this index once per genome and reuse it for every sample.

**2. Sequencing errors and biological variation.** Reads are never a perfect copy of the reference. The sequencer makes occasional mistakes, and your sample genuinely differs from the reference at millions of positions (SNPs, insertions, deletions). A useful aligner must therefore allow **approximate matching** — finding the best location even when the read does not match perfectly — while still being able to tell a real match from a coincidental one.

**3. Repetitive and ambiguous regions.** Large stretches of any genome are repetitive, so some reads match equally well in several places. Aligners handle this by reporting a **mapping quality (MAPQ)** score that reflects how confident they are about the assigned position, letting you filter out ambiguously placed reads later.

### The Two Big Ideas Behind Every Aligner

Almost every aligner in this tutorial is built on one of two indexing strategies. Understanding them in plain terms makes the tool names far less intimidating.

The first is the **Burrows-Wheeler Transform (BWT) with an FM-index**. This is a clever, highly compressed reversible transformation of the genome that lets the aligner search for a pattern extremely quickly while using very little memory. Tools such as BWA, Bowtie2, and HISAT2 are built on this idea. Its strength is a tiny memory footprint; its classic limitation is handling large gaps, which matters for RNA (more on that below).

The second is the **hash table / k-mer or seed approach**. Here the aligner chops reads (and/or the genome) into short subsequences — **seeds** or **k-mers** — and looks them up in a hash table to find candidate locations, then extends and scores the full alignment around each seed. STAR and Minimap2 use sophisticated versions of this “seed-and-extend” strategy. These tools are typically the fastest and best at spanning large gaps, at the cost of higher memory use.

Most aligners follow the same two-phase logic regardless of index type: a fast **seeding** step to find promising candidate locations, followed by a slower, more careful **extension** step that scores the full alignment (often using classic dynamic-programming algorithms like Smith-Waterman or Needleman-Wunsch).

### Why One Aligner Cannot Do Everything

If every aligner shares the same basic goal, why are there dozens of them? Because different experiments break different assumptions:

- **DNA reads** map more or less continuously to the genome. A DNA aligner assumes a read corresponds to one uninterrupted stretch of reference.

- **RNA reads** from a eukaryote do not. Because introns are spliced out of mature mRNA, a single read can straddle two exons that sit thousands of bases apart in the genome. A DNA aligner would fail on such a read; you need a **splice-aware** aligner that can jump across introns.

- **Long reads** (PacBio, Oxford Nanopore) are thousands of bases long with high indel error rates, which demands entirely different scoring than short, low-error Illumina reads.

- **Single-cell reads** carry extra cell barcodes and molecular tags that must be parsed alongside the genomic alignment.

- **Gene-expression quantification** often does not need base-precise coordinates at all — only which transcript a read came from — which opens the door to ultra-fast **alignment-free** methods.

The rest of this tutorial is organized around exactly these scenarios. Find the section that matches your experiment, and you will find the recommended tool and how to run it.

---

## Category 1: DNA-Seq Aligners for Short Reads (Unspliced Alignment)

These aligners are built for DNA experiments — **whole-genome sequencing (WGS)**, **whole-exome sequencing (WES)**, **ChIP-seq**, and **ATAC-seq**. They assume a read matches one continuous stretch of the reference and do not try to jump across large intronic gaps. Most are built on a BWT/FM-index or a spaced-seed hash table, which is what makes them both fast and memory-light.

### BWA (Burrows-Wheeler Aligner): The Industry Standard for DNA

BWA is the default choice for short-read DNA mapping and the assumed input for the GATK variant-calling ecosystem. It ships three algorithms; the one you want is almost always **BWA-MEM**.

- **BWA-MEM** — the modern workhorse. Use it for any Illumina reads longer than ~70 bp. It is accurate for SNPs, indels, and split-read structural variants, and it handles paired-end reads natively.

- **BWA-backtrack** (`bwa aln`) — an older algorithm tuned for very short reads (< 70 bp), such as legacy 36-50 bp runs.

- **BWA-SW** — a legacy mode for longer, error-prone reads, now superseded by Minimap2.

**Recommended scenario:** WGS and WES for germline or somatic variant calling; any short-read DNA project feeding into GATK, bcftools, or DeepVariant. If you are doing human variant calling, BWA-MEM is the safe, community-standard answer.

```bash
# --- Install BWA and samtools from Bioconda ---
# samtools is needed to convert, sort, and index the output.
conda create -n dna_align -c bioconda -c conda-forge bwa samtools -y
conda activate dna_align

# --- Step 1: Build the BWA index (done ONCE per reference genome) ---
# This creates several index files next to your FASTA (.amb, .ann, .bwt, .pac, .sa).
bwa index reference.fa

# --- Step 2: Align paired-end reads with BWA-MEM ---
#   -t 8                : use 8 CPU threads
#   -R "@RG..."         : add a read group; GATK REQUIRES this tag
# The output is piped straight into samtools to make a sorted, indexed BAM
# without ever writing a huge intermediate SAM file to disk.
bwa mem -t 8 \
    -R "@RG\tID:sample1\tSM:sample1\tPL:ILLUMINA\tLB:lib1" \
    reference.fa \
    sample_R1.fastq.gz sample_R2.fastq.gz \
  | samtools sort -@ 8 -o sample1.sorted.bam -

# --- Step 3: Index the sorted BAM so downstream tools can random-access it ---
samtools index sample1.sorted.bam
```

> 

**Beginner tip:** The read group (`-R`) looks like boilerplate, but GATK will refuse to run without it. Set `SM` (sample name) correctly — downstream tools use it to label each sample.

### Bowtie2: The Default for ChIP-Seq and ATAC-Seq

Bowtie2 is an extremely fast, memory-efficient FM-index aligner. Its key advantage over the original Bowtie is support for **gapped and local alignment**, which makes it the community default for **ChIP-seq** and **ATAC-seq** peak-calling pipelines (for example, ENCODE and nf-core/atacseq both build on it). The original **Bowtie (Bowtie1)** is ungapped and now used mainly for very short reads such as small-RNA/miRNA.

**Recommended scenario:** ChIP-seq and ATAC-seq read mapping; any short-read DNA alignment where you want minimal memory use and maximum speed and do not need BWA-MEM’s split-read structural-variant sensitivity.

```bash
# --- Install ---
conda create -n chip_align -c bioconda -c conda-forge bowtie2 samtools -y
conda activate chip_align

# --- Step 1: Build the Bowtie2 index (ONCE per genome) ---
# "hg38_index" is the shared prefix for the generated .bt2 files.
bowtie2-build reference.fa hg38_index

# --- Step 2: Align paired-end ATAC-seq / ChIP-seq reads ---
#   -x           : the index prefix from Step 1
#   -1 / -2      : paired FASTQ files
#   --very-sensitive : higher sensitivity, good for ATAC/ChIP
#   -X 2000      : allow fragments up to 2 kb (ATAC-seq has large fragments)
#   -p 8         : 8 threads
bowtie2 -x hg38_index \
    -1 sample_R1.fastq.gz -2 sample_R2.fastq.gz \
    --very-sensitive -X 2000 -p 8 \
  | samtools sort -@ 8 -o sample.sorted.bam -

samtools index sample.sorted.bam
```

### Minimap2 (Short-Read Mode): The Speed Option

Minimap2 is best known as a long-read aligner (see Category 5), but its short-read preset `-ax sr` maps Illumina DNA reads exceptionally fast using a minimizer sketch. It is a strong choice when throughput matters and you do not require BWA-MEM’s exact behavior for a GATK pipeline.

**Recommended scenario:** Very large short-read DNA datasets where speed is the priority, or workflows that want a single tool for both short and long reads.

```bash
conda install -n dna_align -c bioconda -c conda-forge minimap2 -y

# -ax sr : short-read genomic preset (paired-end aware)
# -t 8   : threads
minimap2 -ax sr -t 8 reference.fa sample_R1.fastq.gz sample_R2.fastq.gz \
  | samtools sort -@ 8 -o sample.sorted.bam -
samtools index sample.sorted.bam
```

### SOAP2 and Novoalign: Specialist and Legacy Options

**SOAP2** is a BWT-index aligner heavily optimized for high-throughput short reads. It is fast but largely superseded by BWA-MEM and Bowtie2 in modern pipelines; you will mainly meet it in older publications.

**Novoalign** is a highly accurate hybrid (commercial with a free academic tier) that uses a Needleman-Wunsch variant. It excels at resolving complex indels, at the cost of speed and a license. Reach for it only when maximal indel accuracy justifies a slower, licensed tool.

**Recommended scenario:** Novoalign — projects where indel-calling accuracy is paramount and runtime is secondary; SOAP2 — reproducing or maintaining legacy pipelines.

> 

**When aligning to an isolated transcriptome FASTA** (a file of spliced transcript sequences with introns already removed), you do **not** need a splice-aware tool. Because the reference has no introns, a standard DNA aligner such as Bowtie2 or BWA-MEM is the correct choice. Splice-awareness only matters when the reference is a genome that still contains introns.

---

## Category 2: Bulk RNA-Seq Aligners (Splice-Aware Genomic Mapping)

Here is the key difference from Category 1: because eukaryotic mRNA has its introns spliced out, an RNA read aligned back to the **genome** frequently spans two or more exons separated by thousands of bases of intron. A DNA aligner treats that intronic gap as a catastrophic mismatch and throws the read away. **Splice-aware** aligners are designed to recognize these gaps as introns and map cleanly across them — and to discover **novel splice junctions** you did not know about in advance.

Use a splice-aware aligner whenever you map RNA-seq reads to a genome and you care about coordinates — for example, differential exon usage, novel isoform or junction discovery, fusion-gene detection, or generating BAM files for a genome browser. (If you only need gene-level counts, jump to Category 3, which is faster.)

### STAR: The Gold Standard for Bulk RNA-Seq

STAR (Spliced Transcripts Alignment to a Reference) is the current gold standard for bulk RNA-seq genome alignment. It uses a “Maximal Mappable Prefix” seed search that makes it both extremely fast and exceptionally accurate at detecting novel splice junctions and chimeric (fusion) transcripts. The one real cost is memory: indexing and aligning the human genome typically needs about **30-40 GB of RAM**, so STAR is usually run on an HPC node or a large workstation, not a laptop.

**Recommended scenario:** The default for most bulk RNA-seq on a machine with sufficient RAM — especially when you need coordinate-accurate BAMs, novel junction discovery, or a starting point for fusion detection (STAR-Fusion, Arriba). It is the aligner used inside nf-core/rnaseq.

```bash
# --- Install ---
conda create -n rnaseq_align -c bioconda -c conda-forge star samtools -y
conda activate rnaseq_align

# --- Step 1: Build the STAR genome index (ONCE per genome + annotation) ---
#   --runMode genomeGenerate  : index-building mode
#   --sjdbGTFfile             : gene annotation (GTF) enables splice-junction awareness
#   --sjdbOverhang            : set to (read length - 1); 100 works well for 101 bp reads
#   --genomeDir               : output folder for the index (needs ~30-40 GB RAM to build for human)
STAR --runMode genomeGenerate \
     --genomeDir star_index/ \
     --genomeFastaFiles reference.fa \
     --sjdbGTFfile annotation.gtf \
     --sjdbOverhang 100 \
     --runThreadN 8

# --- Step 2: Align paired-end RNA-seq reads ---
#   --readFilesCommand zcat        : reads are gzip-compressed
#   --outSAMtype BAM SortedByCoordinate : write a sorted BAM directly
#   --quantMode GeneCounts         : also output a per-gene count table (ReadsPerGene.out.tab)
STAR --genomeDir star_index/ \
     --readFilesIn sample_R1.fastq.gz sample_R2.fastq.gz \
     --readFilesCommand zcat \
     --outSAMtype BAM SortedByCoordinate \
     --quantMode GeneCounts \
     --runThreadN 8 \
     --outFileNamePrefix sample1_

samtools index sample1_Aligned.sortedByCoord.out.bam
```

> 

**Beginner tip:** `--sjdbOverhang` should be your read length minus one. The default of 100 is fine for typical 100-150 bp reads; only change it for unusually short reads.

### HISAT2: The Memory-Efficient Alternative

HISAT2 is the successor to TopHat2 and the best choice when RAM is limited. It uses a Hierarchical Graph FM-index (HGFM) that represents the genome plus known splice sites and variants, giving splice-aware alignment at a fraction of STAR’s memory — the human index runs comfortably in about **4-8 GB of RAM**, so it works on a modest workstation or even a laptop. Speed is comparable to STAR.

**Recommended scenario:** Bulk RNA-seq when you do not have 30+ GB of RAM, or when running many samples where a small memory footprint lets you pack more jobs onto a node. Also common in HISAT2 + StringTie isoform-quantification workflows.

```bash
conda install -n rnaseq_align -c bioconda -c conda-forge hisat2 -y

# --- Step 1: Build the HISAT2 index (ONCE) ---
# "grch38" is the shared prefix for the .ht2 index files.
hisat2-build -p 8 reference.fa grch38

# --- Step 2: Align paired-end reads ---
#   -x            : index prefix
#   --dta         : "downstream transcriptome assembly" -- tailors output for StringTie
#   -p 8          : threads
hisat2 -x grch38 \
    -1 sample_R1.fastq.gz -2 sample_R2.fastq.gz \
    --dta -p 8 \
  | samtools sort -@ 8 -o sample.sorted.bam -
samtools index sample.sorted.bam
```

### GSNAP / GMAP: Maximum Sensitivity for Complex Transcripts

GMAP (for long cDNA/mRNA sequences) and its short-read sibling GSNAP are highly sensitive genomic mappers. They tolerate SNPs, handle multiple splice-site types, and cope with complex variants, which makes them useful for difficult transcripts and SNP-aware alignment. They are slower than STAR and HISAT2, so they are a specialist rather than a default choice.

**Recommended scenario:** Alignment of full-length cDNAs or when maximum sensitivity to variants and unusual splice sites outweighs speed.

```bash
conda create -n gsnap_align -c bioconda -c conda-forge gmap samtools -y
conda activate gsnap_align

# Build a GMAP/GSNAP database (ONCE)
gmap_build -D gmap_db -d mygenome reference.fa

# Align short RNA-seq reads with GSNAP
#   -N 1 : look for novel splice junctions
gsnap -D gmap_db -d mygenome -N 1 -t 8 -A sam \
    sample_R1.fastq.gz sample_R2.fastq.gz \
  | samtools sort -@ 8 -o sample.sorted.bam -
samtools index sample.sorted.bam
```

### TopHat2: Legacy — Do Not Use for New Projects

TopHat2 was the original splice-aware backbone of the classic “Tuxedo” suite (Bowtie + TopHat + Cufflinks). It maps reads by finding exons first and then stitching junctions. It has been **fully superseded by STAR and HISAT2** — its own authors now recommend HISAT2 instead. Mentioned here only so you recognize it in older papers; do not start a new analysis with it.

**Recommended scenario:** None for new work. Reproducing a published pre-2016 Tuxedo pipeline only.

---

## Category 3: Transcript Quantification Tools (Alignment-Free / Pseudo-Aligners)

For the most common RNA-seq goal — **differential gene expression** — you rarely need base-by-base genomic coordinates. You only need to know how many reads came from each transcript. Producing full SAM/BAM alignments just to throw the coordinates away is wasted computation. Alignment-free tools skip traditional alignment entirely: they match reads to transcripts using lightweight k-mer hashing and output a transcript-level count table directly from raw FASTQ files. The result is a 10-20x speedup and a memory footprint small enough for a laptop.

The important trade-off: these tools align to a **transcriptome** (a FASTA of known transcript sequences), not a genome. They give you expression estimates, **not** genomic coordinates, so they cannot discover novel junctions or produce browser-viewable BAMs. For standard gene-expression studies with a good reference annotation, that is exactly the right trade.

### Salmon: Fast, Bias-Aware Quantification

Salmon uses “selective alignment” (an evolution of quasi-mapping) together with a streaming inference algorithm to estimate transcript abundance. Its standout feature is a set of **bias-correction models** — GC content, sequence-specific, and positional bias — that noticeably improve accuracy on real data. Its output pairs naturally with `tximport` and DESeq2 for downstream differential expression.

**Recommended scenario:** The recommended default for differential gene-expression studies where a reference transcriptome exists and you do not need genomic coordinates. Bias correction makes it the preferred choice on data with known GC or positional artifacts.

> 

**“Reference-based” vs. “reference-free” Salmon:** People use these terms two different ways, so it is worth separating them. (1) **Decoy-aware vs. non-decoy indexing** — adding the genome FASTA as a “decoy” alongside the transcriptome (shown below) prevents reads from an unannotated genomic region being falsely assigned to a similar-looking transcript. This is the distinction that matters for most model-organism work, and decoy-aware is the safer default whenever the genome is available. (2) **Quantifying against a known reference transcriptome vs. a *de novo* assembly** — for species without a good reference genome, you instead assemble a transcriptome directly from your RNA-seq reads (e.g., with Trinity) and quantify against that assembly with Salmon. This second workflow is what people usually mean by “reference-free” Salmon; it is a different pipeline entirely and is out of scope here, but worth knowing the term exists.

```bash
# --- Install ---
conda create -n quant -c bioconda -c conda-forge salmon -y
conda activate quant

# --- Step 1: Build the transcriptome index (ONCE) ---
# Input is a TRANSCRIPTOME FASTA (e.g., Ensembl cDNA), NOT the genome.
# A "decoy-aware" index that also includes the genome improves accuracy;
# see the Salmon docs for building the decoy file.
salmon index -t transcripts.fa -i salmon_index -k 31 -p 8

# --- Step 2: Quantify directly from FASTQ (no BAM needed) ---
#   -l A            : automatically detect library type / strandedness
#   -1 / -2         : paired FASTQ files
#   --validateMappings : enables selective alignment (more accurate)
#   --gcBias        : turn on GC bias correction
#   -o              : output folder (contains quant.sf with per-transcript counts + TPM)
salmon quant -i salmon_index -l A \
    -1 sample_R1.fastq.gz -2 sample_R2.fastq.gz \
    --validateMappings --gcBias \
    -p 8 -o sample1_quant
```

> 

**Beginner tip:** Salmon reports **estimated** counts, which are not whole numbers. When importing into DESeq2, use `tximport` (which handles the non-integer counts and transcript-length offsets correctly) rather than rounding the numbers yourself.

### Kallisto: The Lightweight Speed Champion

Kallisto pioneered “pseudo-alignment”: it finds the compatible set of transcripts for each read by walking paths through a transcriptome-derived de Bruijn graph, without ever computing a base-level alignment. It is remarkably fast and light.

Kallisto is not simply a lesser version of Salmon — it is a different design tradeoff, not a worse implementation. On typical, well-behaved libraries the two tools usually produce highly concordant results. The practical difference shows up specifically on libraries with known GC or positional bias (common with certain library-prep kits or degraded RNA), where Salmon’s explicit bias-correction models tend to give measurably more accurate abundance estimates. If you are unsure whether your data has such bias, Salmon is the safer default; if speed and simplicity matter more and you have no reason to suspect bias, Kallisto is a perfectly solid choice.

**Recommended scenario:** Differential expression when you want the fastest, lightest possible workflow and do not have known GC/positional bias concerns. Excellent on a laptop.

> 

**A note on `sleuth`:** Kallisto has historically paired with the `sleuth` R package for downstream statistics using its bootstrap replicates. Sleuth has seen little active development in recent years, and the community has largely shifted to importing Kallisto’s output via `tximport` into DESeq2, edgeR, or limma-voom instead — the same downstream path already used for Salmon in this tutorial. Unless you specifically need sleuth’s bootstrap-based uncertainty modeling, `tximport` + DESeq2 is the more actively maintained route.

```bash
conda create -n kallisto_env -c bioconda -c conda-forge kallisto -y
conda activate kallisto_env

# --- Step 1: Build the index from a transcriptome FASTA (ONCE) ---
kallisto index -i transcripts.idx transcripts.fa

# --- Step 2: Quantify ---
#   -i        : index
#   -o        : output folder (contains abundance.tsv)
#   -t 8      : threads
#   -b 100    : 100 bootstrap samples for downstream uncertainty estimates (sleuth)
kallisto quant -i transcripts.idx -o sample1_quant -b 100 -t 8 \
    sample_R1.fastq.gz sample_R2.fastq.gz
```

### Sailfish: The Predecessor

Sailfish introduced the original k-mer-based quantification idea that Salmon later refined. It is effectively obsolete — if you are reaching for Sailfish, use Salmon instead, which is faster and more accurate. Included only for historical context.

**Recommended scenario:** None for new work; use Salmon.

---

## Category 4: Single-Cell RNA-Seq (scRNA-Seq) Aligners

Single-cell data adds a twist that no bulk aligner handles on its own. Each read carries two extra pieces of information in addition to the cDNA sequence: a **cell barcode (CB)** that identifies which cell the read came from, and a **Unique Molecular Identifier (UMI)** that labels the individual mRNA molecule so PCR duplicates can be collapsed. A single-cell aligner must therefore map the read to the transcriptome **and** parse, correct, and count barcodes and UMIs — producing a **cell-by-gene count matrix** rather than a BAM.

The right tool depends mainly on your chemistry (almost always 10x Genomics) and on your speed/memory constraints.

### Cell Ranger: The Official 10x Pipeline

Cell Ranger is the official pipeline from 10x Genomics. It wraps a customized STAR aligner and produces the feature-barcode matrices that essentially every downstream tool (Seurat, Scanpy) expects. It is the path of least resistance and the results reviewers expect, but it is heavy, opinionated, and slow, with little parameter flexibility.

**Recommended scenario:** Standard 10x Genomics experiments where you want the official, reviewer-familiar output and are not constrained by runtime.

```bash
# Cell Ranger is distributed by 10x Genomics (free account + download),
# NOT via Bioconda. After downloading and adding it to your PATH:
cellranger count \
    --id=sample1 \                     # output folder name
    --transcriptome=/refs/refdata-gex-GRCh38-2024-A \  # prebuilt 10x reference
    --fastqs=/data/fastqs \            # folder of FASTQ files
    --sample=sample1 \                 # sample prefix in the FASTQ names
    --localcores=8 --localmem=64       # CPU / RAM limits
```

### STARsolo: The Fast, Free Cell Ranger Alternative

STARsolo is built directly into the STAR aligner and reproduces the Cell Ranger logic (barcode correction, UMI collapsing, matrix generation) in a single command — typically several times faster, fully open-source, and far more configurable. Its output matrices are essentially a drop-in replacement for Cell Ranger’s.

**Recommended scenario:** The recommended default for most 10x scRNA-seq — especially large studies or when you want speed, control, and reproducibility without the Cell Ranger overhead. (Note: STARsolo cell barcodes lack the `-1` suffix that Cell Ranger adds, so when loading into Seurat you may need to append it to match expected formats.)

```bash
conda create -n sc_align -c bioconda -c conda-forge star -y
conda activate sc_align

# Uses the SAME STAR genome index built in Category 2.
#   --soloType CB_UMI_Simple           : standard 10x droplet chemistry
#   R2 comes FIRST (cDNA), R1 SECOND (barcode+UMI) -- this order matters!
#   --soloCBwhitelist                  : the 10x barcode whitelist for your chemistry
#   --soloCBstart/CBlen/UMIstart/UMIlen: barcode/UMI positions (10x v3 = 16 bp CB, 12 bp UMI)
STAR --genomeDir star_index/ \
     --readFilesIn sample_R2.fastq.gz sample_R1.fastq.gz \
     --readFilesCommand zcat \
     --soloType CB_UMI_Simple \
     --soloCBwhitelist 3M-february-2018.txt \
     --soloCBstart 1 --soloCBlen 16 --soloUMIstart 17 --soloUMIlen 12 \
     --runThreadN 8 \
     --outFileNamePrefix sample1_solo_
```

> 

**RNA velocity users:** STARsolo is also one of the most common entry points for RNA velocity analysis, since it can emit spliced and unspliced count matrices directly by adding `--soloFeatures Gene Velocyto` to the command above — no separate `velocyto` run required. See the [RNA Velocity with scVelo](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/) and [CellRank](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/) tutorials in the scRNA-seq series for what to do with those matrices afterward.

### Alevin / Alevin-fry: Fast and Low-Memory (Salmon Family)

Alevin and its faster successor **Alevin-fry** are the single-cell modules of the Salmon ecosystem. Alevin-fry is a highly optimized, low-memory suite for single-cell and single-nucleus quantification using selective alignment. It is the most resource-frugal option and scales well to very large datasets.

**Recommended scenario:** Large single-cell/single-nucleus studies where memory and speed are the binding constraints, or when you already use the Salmon/tximeta ecosystem.

```bash
conda create -n alevin_env -c bioconda -c conda-forge salmon alevin-fry pyroe -y
conda activate alevin_env

# Step 1: map reads with salmon alevin (chromium = 10x chemistry), producing a RAD file
salmon alevin -i salmon_index -l ISR \
    -1 sample_R1.fastq.gz -2 sample_R2.fastq.gz \
    --chromiumV3 --sketch -p 8 -o alevin_map

# Step 2: generate permit list, collate, and quantify with alevin-fry
alevin-fry generate-permit-list -d fw -i alevin_map -o af_quant --unfiltered-pl whitelist.txt
alevin-fry collate -i af_quant -r alevin_map -t 8
alevin-fry quant -i af_quant -m t2g.tsv -r cr-like -o af_counts -t 8 --use-mtx
```

### kallisto | bustools (kb-python): The Pseudo-Alignment Pipeline

kb-python wraps Kallisto and BUStools into a single-command single-cell workflow. Kallisto pseudo-aligns the reads and emits a BUS (Barcode, UMI, Set) file that BUStools sorts and counts into a matrix. It is fast, light, and reproducible.

**Recommended scenario:** Fast, lightweight single-cell quantification (including RNA velocity prep), especially when you already favor the Kallisto ecosystem.

```bash
pip install kb-python

# Step 1: build a combined kallisto index + transcript-to-gene map (ONCE)
kb ref -i index.idx -g t2g.txt -f1 cdna.fa reference.fa annotation.gtf

# Step 2: count 10x v3 reads into a matrix
#   -x 10xv3 : chemistry; --h5ad writes an AnnData file ready for Scanpy
kb count -i index.idx -g t2g.txt -x 10xv3 -o sample1_out --h5ad -t 8 \
    sample_R1.fastq.gz sample_R2.fastq.gz
```

---

## Category 5: Third-Generation Long-Read Aligners (DNA and RNA)

Long-read platforms — **PacBio** and **Oxford Nanopore (ONT)** — produce reads that are thousands to tens of thousands of bases long, but with much higher insertion/deletion error rates than Illumina. This means their dominant error mode is spurious extra or missing bases (an **insertion** or **deletion**, together called an **indel**) rather than the base-substitution errors that dominate Illumina data — for example, a basecaller miscounting how many identical bases passed through an ONT pore during a homopolymer run. Short-read aligners fail on this data because their scoring assumes short, low-error reads with rare, isolated substitutions. Long-read aligners use different seeding and gap-scoring strategies designed for these error profiles.

They also handle **structural variants (SVs)** naturally — large-scale differences from the reference genome (deletions, duplications, insertions, inversions, and translocations, conventionally defined as ≥50 bp), as opposed to the small SNPs and short indels that standard variant callers like GATK focus on. A single long read can span an entire SV breakpoint directly, whereas short-read callers must infer the same variant indirectly from split reads or discordant pairs — which is why long-read platforms are especially valuable for SV detection.

### Minimap2: The Universal Long-Read Benchmark

Minimap2 is the de facto standard for long-read alignment and one of the most versatile tools in all of bioinformatics. It uses a **minimizer** seed-and-extend approach and switches behavior through presets: genomic DNA, spliced cDNA, or direct RNA, for both PacBio and ONT. A single tool covers long DNA, long RNA, and (via `-ax sr`) short reads too.

**Recommended scenario:** Essentially all long-read mapping — long-read WGS and structural-variant calling, PacBio Iso-Seq or ONT cDNA/direct-RNA transcriptomics. The default first choice for any third-generation data.

```bash
conda create -n longread -c bioconda -c conda-forge minimap2 samtools -y
conda activate longread

# Choose the preset that matches your data:
#   map-pb    : PacBio genomic DNA
#   map-ont   : Oxford Nanopore genomic DNA
#   splice    : long spliced RNA/cDNA (adds intron-aware gap scoring)

# --- Example A: ONT genomic DNA (for structural-variant calling) ---
minimap2 -ax map-ont -t 8 reference.fa ont_reads.fastq.gz \
  | samtools sort -@ 8 -o ont.sorted.bam -
samtools index ont.sorted.bam

# --- Example B: PacBio Iso-Seq / long RNA (splice-aware) ---
#   -uf : consider forward transcript strand (typical for Iso-Seq/direct-RNA)
minimap2 -ax splice -uf -t 8 reference.fa isoseq_reads.fastq.gz \
  | samtools sort -@ 8 -o isoseq.sorted.bam -
samtools index isoseq.sorted.bam
```

> 

**Beginner tip:** The single most common long-read mistake is using the wrong preset. Match `-ax` to your platform and molecule type: `map-ont`/`map-pb` for DNA, `splice` for RNA/cDNA.

### NGMLR: Structural-Variant-Aware Alignment

NGMLR is a long-read aligner built specifically for accurate **structural-variant** detection. Its convex gap-cost scoring model is tuned to the characteristic indel errors of PacBio and ONT, which helps callers like Sniffles resolve breakpoints precisely. It is slower than Minimap2, so it is used when SV precision is the priority.

**Recommended scenario:** Long-read structural-variant calling where breakpoint accuracy matters most, typically paired with the Sniffles SV caller.

```bash
conda create -n ngmlr_env -c bioconda -c conda-forge ngmlr samtools -y
conda activate ngmlr_env

# -x ont (or pacbio) selects the platform error model; -t sets threads
ngmlr -t 8 -x ont \
    -r reference.fa \
    -q ont_reads.fastq.gz \
  | samtools sort -@ 8 -o ngmlr.sorted.bam -
samtools index ngmlr.sorted.bam
```

### BLAT: Legacy Alignment of Full-Length cDNAs and ESTs

BLAT is a fast tool for aligning highly similar sequences — full-length cDNAs, ESTs, or protein queries — against a genome. It is worth being precise about its relationship to BLAST: the two are separate, parallel tools rather than one being a version of the other. BLAST remains the standard for general sequence-similarity search against large public databases (protein or nucleotide, NCBI-scale); BLAT was built specifically for fast, near-exact alignment of a query against a single reference genome, and it is still actively used today, most visibly as the engine behind the UCSC Genome Browser’s BLAT search feature. It is not a general NGS aligner and should not be used for mapping millions of reads.

**Recommended scenario:** Aligning a small number of full-length cDNA/EST or highly homologous sequences to a genome; quick lookups, not high-throughput mapping.

```bash
conda create -n blat_env -c bioconda blat -y
conda activate blat_env

# blat <database> <query> <output.psl>
blat reference.fa query_cdnas.fa output.psl
```

---

## Quick Decision Guide: Which Aligner Should I Use?

When you are staring at a folder of FASTQ files, work through these questions in order:

1. **Is it long-read (PacBio / Nanopore)?** -> **Minimap2** (add NGMLR if structural-variant breakpoint precision is critical).

2. **Is it single-cell (10x)?** -> **STARsolo** (fast, free default), **Cell Ranger** (official output), or **Alevin-fry / kb-python** (lowest memory).

3. **Is it RNA and you only need gene counts for differential expression?** -> **Salmon** or **Kallisto** (alignment-free, fast, laptop-friendly).

4. **Is it RNA and you need genomic coordinates, novel junctions, or fusions?** -> **STAR** (if you have 30+ GB RAM) or **HISAT2** (if memory is tight).

5. **Is it short-read DNA for variant calling (WGS/WES)?** -> **BWA-MEM**.

6. **Is it short-read DNA for ChIP-seq / ATAC-seq?** -> **Bowtie2**.

The summary table below captures the same logic at a glance.

**BWA-MEM****Bowtie2****Minimap2 `-ax sr`****Novoalign****STAR****HISAT2****Salmon****Kallisto****GSNAP / GMAP****STARsolo****Cell Ranger****Alevin-fry****kb-python****Minimap2****NGMLR****BLAT**

| Scenario | Data type | Recommended aligner | Why | Index type |
| --- | --- | --- | --- | --- |
| WGS / WES variant calling | Short-read DNA |  | Industry standard; GATK-compatible | BWT / FM-index |
| ChIP-seq / ATAC-seq | Short-read DNA |  | Fast, low memory, gapped/local | FM-index |
| Very large short-read DNA (speed) | Short-read DNA |  | Fastest short-read mapping | Minimizer hash |
| Maximum indel accuracy | Short-read DNA |  | Needleman-Wunsch; best complex indels | Hash |
| Bulk RNA-seq, coordinates/junctions | Short-read RNA |  | Gold standard; novel junctions, fusions | Hash (suffix array) |
| Bulk RNA-seq, limited RAM | Short-read RNA |  | Splice-aware in ~4-8 GB RAM | Graph FM-index |
| Differential gene expression only | Short-read RNA | / | Alignment-free, very fast, bias-aware | k-mer hash / de Bruijn |
| Full-length / SNP-tolerant cDNA | Short/long RNA |  | Maximum sensitivity | Hash |
| 10x scRNA-seq (default) | Single-cell RNA |  | Cell Ranger logic, much faster, free | Hash |
| 10x scRNA-seq (official) | Single-cell RNA |  | Standard, reviewer-familiar output | Hash (STAR) |
| scRNA/snRNA, low memory | Single-cell RNA | / | Fast, lightweight matrices | k-mer hash |
| Long-read DNA or RNA | PacBio / ONT |  | Universal long-read benchmark | Minimizer |
| Long-read structural variants | PacBio / ONT |  | Convex gap cost for SV breakpoints | Hash |
| A few cDNAs / ESTs to a genome | Any |  | Fast lookup of homologous sequences | k-mer index |

---

## Best Practices for Read Alignment

**1. Build the index once, reuse it forever.** Index generation is the slow, memory-hungry step. Build it a single time per genome-plus-annotation version, store it centrally, and point every sample at the same index. Never rebuild per sample.

**2. Keep your genome FASTA and annotation GTF from the same source and release.** A genome from one Ensembl release paired with a GTF from another causes silent chromosome-name mismatches and missing genes that are painful to diagnose later. Match the release exactly.

**3. Match the tool to the molecule, not to habit.** The single most consequential decision is DNA aligner vs. splice-aware RNA aligner vs. alignment-free quantifier. Using a DNA aligner on genome-mapped RNA silently discards every junction-spanning read.

**4. Always add read groups for variant-calling pipelines.** BWA-MEM’s `-R` read-group tag is mandatory for GATK. Set the sample name (`SM`) correctly so multi-sample tools can tell your samples apart.

**5. Sort and index your BAM immediately.** Nearly every downstream tool needs a coordinate-sorted, indexed BAM. Pipe the aligner output straight into `samtools sort` to avoid writing a giant intermediate SAM.

**6. Check your alignment rate before doing anything else.** A mapping rate far below expectation (well under ~70-80% for a matched-species short-read run) usually signals the wrong reference, adapter contamination, or a species mix-up. Run `samtools flagstat` on every BAM.

**7. Right-size your compute.** STAR needs ~30-40 GB RAM for the human genome; HISAT2, Salmon, and Kallisto run in a few GB. Pick a tool that fits your hardware rather than fighting out-of-memory crashes.

**8. Verify strandedness for RNA quantification.** Let Salmon (`-l A`) or STAR infer library type, and confirm it — incorrect strandedness does not error, it just silently produces wrong counts.

---

## Common Pitfalls and How to Avoid Them

The mistakes below trip up almost every beginner at least once. Knowing them in advance saves hours of confused debugging.

- **Using a DNA aligner on RNA reads mapped to a genome.** Reads spanning exon-exon junctions are dropped, so gene counts are artificially low and no novel junctions are found. Use STAR or HISAT2 for genome alignment; only use BWA/Bowtie2 when the reference is a transcriptome.

- **Feeding short-read tools long reads (or vice versa).** BWA-MEM on Nanopore data maps almost nothing. Use Minimap2 for long reads and match its `-ax` preset to the platform.

- **Wrong Minimap2 preset.** `map-ont` vs `map-pb` vs `splice` produce very different results. DNA uses `map-*`; RNA/cDNA uses `splice`.

- **Swapping single-cell read order.** In STARsolo the cDNA read (R2) comes first and the barcode read (R1) second. Reversing them yields an empty or garbage matrix.

- **Rounding Salmon/Kallisto estimated counts by hand.** These are non-integer estimates; import them with `tximport` into DESeq2 rather than rounding, so transcript-length offsets are handled correctly.

- **Mismatched genome and annotation versions.** Causes chromosome-name errors (“chr1” vs “1”) and missing genes. Keep FASTA and GTF from the same release.

- **Under-provisioning memory for STAR.** A human STAR index needs ~30-40 GB RAM; less causes cryptic crashes. Use HISAT2 if you cannot supply the RAM.

- **Forgetting the read group for GATK.** BWA-MEM without `-R` produces BAMs that GATK rejects outright.

---

## Troubleshooting Common Alignment Errors

`samtools flagstat``[E::...]``map-ont``map-pb``splice``-R``@RG``tximport``-l A``samtools sort``samtools sort`

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Very low overall mapping rate | Wrong reference genome/species, or heavy adapter contamination | Confirm the correct reference; trim adapters (fastp/Trim Galore); run |
| RNA reads map but gene counts too low | DNA aligner used on genome-mapped RNA; junction reads lost | Switch to STAR or HISAT2 (splice-aware) |
| STAR crashes during index build or alignment | Insufficient RAM (< ~30 GB for human) | Use a larger node, or switch to HISAT2 |
| chromosome not found / missing genes | Genome FASTA and GTF from different releases | Rebuild index with matched-release FASTA + GTF |
| Almost nothing maps for long reads | Short-read aligner used, or wrong Minimap2 preset | Use Minimap2 with // as appropriate |
| GATK errors about missing read group | BWA-MEM run without | Re-align with a proper  tag |
| Empty or nonsensical single-cell matrix | R1/R2 order swapped or wrong barcode whitelist/chemistry | Put cDNA (R2) first, barcode (R1) second; verify whitelist and CB/UMI lengths |
| Salmon/Kallisto counts look wrong in DESeq2 | Non-integer estimates rounded or imported incorrectly | Import with ; set library type with |
| runs out of disk/memory | Large unsorted SAM written to disk | Pipe aligner output directly into |

---

## Conclusion: Matching the Aligner to the Experiment

Choosing an aligner is far less about finding the single “best” tool and more about matching the tool to the biology and format of your data. The decision tree is short and reliable:

1. Short-read DNA for variants -> **BWA-MEM**; for ChIP/ATAC -> **Bowtie2**.

2. Bulk RNA-seq needing coordinates -> **STAR** (or **HISAT2** when RAM is limited).

3. Bulk RNA-seq needing only gene counts -> **Salmon** or **Kallisto**.

4. Single-cell 10x data -> **STARsolo** (or **Cell Ranger**, or **Alevin-fry / kb-python** for low memory).

5. Long reads (PacBio / ONT) -> **Minimap2** (add **NGMLR** for structural variants).

Underlying all of them are just two big ideas — the memory-efficient BWT/FM-index and the fast, gap-friendly seed-and-extend hash — adapted to the demands of each experiment. Once you can name your data (DNA or RNA, short or long, coordinates or counts, bulk or single-cell), the right aligner chooses itself. Build the index once, keep your genome and annotation versions in lockstep, check your mapping rate, and you will have a clean, trustworthy foundation for everything downstream.

**Related NGS101 tutorials:** For a fully automated bulk RNA-seq run that wires STAR and Salmon together on an HPC cluster, see the [Nextflow / nf-core/rnaseq HPC guide](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/). For what to do with 10x matrices after STARsolo, see the [Single-Cell RNA-seq Velocity Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/) tutorial.

---

## References

1. Li H, Durbin R. Fast and accurate short read alignment with Burrows-Wheeler transform. *Bioinformatics*. 2009;25(14):1754-1760. doi:10.1093/bioinformatics/btp324

2. Li H. Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM. *arXiv*. 2013;1303.3997.

3. Langmead B, Salzberg SL. Fast gapped-read alignment with Bowtie 2. *Nature Methods*. 2012;9(4):357-359. doi:10.1038/nmeth.1923

4. Li R, Yu C, Li Y, et al. SOAP2: an improved ultrafast tool for short read alignment. *Bioinformatics*. 2009;25(15):1966-1967. doi:10.1093/bioinformatics/btp336

5. Dobin A, Davis CA, Schlesinger F, et al. STAR: ultrafast universal RNA-seq aligner. *Bioinformatics*. 2013;29(1):15-21. doi:10.1093/bioinformatics/bts635

6. Kim D, Paggi JM, Park C, Bennett C, Salzberg SL. Graph-based genome alignment and genotyping with HISAT2 and HISAT-genotype. *Nature Biotechnology*. 2019;37(8):907-915. doi:10.1038/s41587-019-0201-4

7. Kim D, Pertea G, Trapnell C, Pimentel H, Kelley R, Salzberg SL. TopHat2: accurate alignment of transcriptomes in the presence of insertions, deletions and gene fusions. *Genome Biology*. 2013;14(4):R36. doi:10.1186/gb-2013-14-4-r36

8. Wu TD, Reid J, Watanabe C, et al. GMAP and GSNAP for genomic sequence alignment: enhancements to speed, accuracy, and functionality. *Methods in Molecular Biology*. 2016;1418:283-334. doi:10.1007/978-1-4939-3578-9_15

9. Patro R, Duggal G, Love MI, Irizarry RA, Kingsford C. Salmon provides fast and bias-aware quantification of transcript expression. *Nature Methods*. 2017;14(4):417-419. doi:10.1038/nmeth.4197

10. Bray NL, Pimentel H, Melsted P, Pachter L. Near-optimal probabilistic RNA-seq quantification. *Nature Biotechnology*. 2016;34(5):525-527. doi:10.1038/nbt.3519

11. Patro R, Mount SM, Kingsford C. Sailfish enables alignment-free isoform quantification from RNA-seq reads using lightweight algorithms. *Nature Biotechnology*. 2014;32(5):462-464. doi:10.1038/nbt.2862

12. Zheng GXY, Terry JM, Belgrader P, et al. Massively parallel digital transcriptional profiling of single cells. *Nature Communications*. 2017;8:14049. doi:10.1038/ncomms14049

13. Kaminow B, Yunusov D, Dobin A. STARsolo: accurate, fast and versatile mapping/quantification of single-cell and single-nucleus RNA-seq data. *bioRxiv*. 2021. doi:10.1101/2021.05.05.442755

14. He D, Zakeri M, Sarkar H, Soneson C, Srivastava A, Patro R. Alevin-fry unlocks rapid, accurate and memory-frugal quantification of single-cell RNA-seq data. *Nature Methods*. 2022;19(3):316-322. doi:10.1038/s41592-022-01408-3

15. Melsted P, Booeshaghi AS, Liu L, et al. Modular, efficient and constant-memory single-cell RNA-seq preprocessing. *Nature Biotechnology*. 2021;39(7):813-818. doi:10.1038/s41587-021-00870-2

16. Li H. Minimap2: pairwise alignment for nucleotide sequences. *Bioinformatics*. 2018;34(18):3094-3100. doi:10.1093/bioinformatics/bty191

17. Sedlazeck FJ, Rescheneder P, Smolka M, et al. Accurate detection of complex structural variations using single-molecule sequencing. *Nature Methods*. 2018;15(6):461-468. doi:10.1038/s41592-018-0001-7

18. Kent WJ. BLAT — the BLAST-like alignment tool. *Genome Research*. 2002;12(4):656-664. doi:10.1101/gr.229202

19. Li H, Handsaker B, Wysoker A, et al. The Sequence Alignment/Map format and SAMtools. *Bioinformatics*. 2009;25(16):2078-2079. doi:10.1093/bioinformatics/btp352

---

*This tutorial is part of the NGS101.com Genomics series for beginners.*
