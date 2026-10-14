# How to Analyze RNAseq Data for Absolute Beginners Part 18: Analyzing Viral Gene Expression in Host RNA-seq Data

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** November 20, 2025
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/)

---

*Understanding viral gene expression patterns during infection is crucial for studying host-pathogen interactions. This comprehensive guide will walk you through the process of accurately quantifying viral transcripts from RNA-seq data of infected host cells, providing you with practical approaches for this challenging analysis.*

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/)

- [The Challenge of Viral RNA-seq Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#The_Challenge_of_Viral_RNA-seq_Analysis)
- [Choosing Your Strategy: Two Paths to Success](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Choosing_Your_Strategy_Two_Paths_to_Success)

  - [The Two-step Strategy: A Methodical Approach](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#The_Two-step_Strategy_A_Methodical_Approach)
  - [The Integrated Strategy: One-Stop Processing](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#The_Integrated_Strategy_One-Stop_Processing)

- [Method 1: The Two-step Alignment Strategy](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Method_1_The_Two-step_Alignment_Strategy)

  - [Creating Your Viral Reference](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Creating_Your_Viral_Reference)
  - [Host Genome Alignment and Filtering](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Host_Genome_Alignment_and_Filtering)
  - [Viral Genome Alignment and Quantification](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Viral_Genome_Alignment_and_Quantification)

- [Method 2: The Integrated Alignment Strategy](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Method_2_The_Integrated_Alignment_Strategy)

  - [Building a Combined Reference](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Building_a_Combined_Reference)
  - [One-Step Alignment and Quantification](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#One-Step_Alignment_and_Quantification)

- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-18-analyzing-viral-gene-expression-in-host-rna-seq-data/#Like_this)

## The Challenge of Viral RNA-seq Analysis

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/mapping.png?resize=782%2C455&ssl=1)

When we sequence RNA from virus-infected cells, we’re faced with a unique analytical challenge. Unlike typical RNA-seq experiments where we’re dealing with transcripts from a single organism, infected samples contain a complex mixture of RNA molecules. Picture your sequencing data as a bustling metropolitan area – the majority of “residents” (reads) belong to the host cell, while viral transcripts are like tourists, sometimes rare and sometimes abundant, mixing with the local population.

This mixed nature of the data creates several challenges:

First, standard RNA-seq pipelines aren’t designed to handle this complexity. They typically assume all transcripts come from a single genome, which can lead to missed viral sequences or misidentified transcripts. Think of it like trying to find specific visitors in a crowded city using a map that only shows local residents.

Second, the proportion of viral reads can vary dramatically – from less than 1% in early infection to a substantial percentage in heavily infected samples. This variability means we need flexible approaches that work across different scenarios.

Lastly, some viral sequences might share similarities with host genes, creating potential confusion in our analysis – like tourists who blend in perfectly with the locals.

## Choosing Your Strategy: Two Paths to Success

To tackle these challenges, we’ve developed two main approaches, each with its own strengths. Let’s understand when to use each method:

### The Two-step Strategy: A Methodical Approach

Think of this method as a careful screening process at immigration control. We first identify all the “locals” (host reads) by mapping to the host genome, then focus on the “visitors” (unmapped reads) that might be viral in origin. This approach is particularly valuable when:

- Your samples have low viral content

- You’re concerned about false positives from host-virus sequence similarity

- You need high confidence in identifying viral transcripts

### The Integrated Strategy: One-Stop Processing

This approach is more like having a universal census that counts everyone at once. We create a combined reference that includes both host and viral genomes, then map all reads in a single step. This works best when:

- You expect substantial viral content

- There’s minimal sequence similarity between host and virus

- You want a streamlined workflow

- Computational resources aren’t a limiting factor

## Method 1: The Two-step Alignment Strategy

Let’s dive into implementing the two-step approach, breaking it down into manageable stages.

### Creating Your Viral Reference

First, we need to create a specialized index for our viral genome. Think of this as creating a detailed map of viral genes that we’ll use to identify viral transcripts later.

```bash
# Activate our RNA-seq environment
conda activate rnaseq_env

# Create a directory for the new genome
mkdir -p ~/Genome_Index/Reovirus_Genome/Reovirus_Star_Index

# Create a customized virus genome index for STAR
# Note: Adjust RAM limit based on your system
STAR --runThreadN 12 \
     --limitGenomeGenerateRAM 200000000000 \
     --genomeSAindexNbases 6 \
     --runMode genomeGenerate \
     --genomeDir ~/Genome_Index/Reovirus_Genome/Reovirus_Star_Index \
     --genomeFastaFiles ~/Genome_Index/Reovirus_Genome/Reovirus_Genome.fa \
     --sjdbGTFfile ~/Genome_Index/Reovirus_Genome/reovirus_genome_annot.gtf
```

When preparing your viral genome files, there are two critical components to understand: the genome sequence itself (FASTA) and its annotation (GTF). In our example, we’ve structured the viral genome as a single continuous sequence in our FASTA file, which we’ve named “chrun”. This simplified structure works well for many viral genomes, which are typically much smaller and less complex than host genomes.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-30-at-2.53.43%E2%80%AFPM.png?resize=1024%2C325&ssl=1)

The GTF file is where we define the functional elements of the viral genome. Think of it as a detailed map that tells our analysis tools where to find important features like genes and regulatory regions. We follow the same standardized GTF format used for host genomes (9 tab delimited columns), which includes:

- Precise coordinates for each gene’s location

- Unique gene identifiers and names

- Sequence feature types (e.g., exons, CDS regions)

- Additional annotations like strand information

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-30-at-2.51.43%E2%80%AFPM.png?resize=1024%2C213&ssl=1)

If you’re working with a more complex viral genome that consists of multiple sequences (for example, a segmented virus like influenza), you’ll need to take extra care with your file preparation. Each sequence in your FASTA file should have a unique, meaningful identifier, and these identifiers must exactly match the sequence names referenced in your GTF file’s coordinates. This consistency is crucial – even a small mismatch in naming can cause your analysis pipeline to fail or produce incorrect results.

### Host Genome Alignment and Filtering

Now we’ll separate host and potential viral reads. This is like identifying all the locals first, so we can focus on the visitors afterward.

```bash
# First, clean up our reads by removing adapters
trim_galore --fastqc --paired --cores 8 \
            ~/my_rnaseq/raw/Sample1_R1_001.fastq.gz \
            ~/my_rnaseq/raw/Sample1_R2_001.fastq.gz \
            -o ~/my_rnaseq/trimmed/Sample1/

# Align to the host genome, keeping track of unmapped reads
STAR --genomeDir ~/Genome_Index/STAR_GRCm38/ \
     --runThreadN 20 \
     --readFilesIn ~/my_rnaseq/trimmed/Sample1/Sample1_R1_001_val_1.fq.gz \
                   ~/my_rnaseq/trimmed/Sample1/Sample1_R2_001_val_2.fq.gz \
     --outSAMtype BAM SortedByCoordinate \
     --outSAMattributes Standard \
     --readFilesCommand zcat \
     --outReadsUnmapped Fastx \
     --outFileNamePrefix ~/my_rnaseq/aligned/Sample1/Sample1_trimmed
```

### Viral Genome Alignment and Quantification

Finally, we’ll examine the unmapped reads to identify viral sequences.

```bash
# Align unmapped reads to the viral genome
STAR --genomeDir ~/Genome_Index/Reovirus_Genome/Reovirus_Star_Index/ \
     --limitBAMsortRAM 200000000000 \
     --runThreadN 20 \
     --readFilesIn ~/Sample1_trimmedUnmapped.out.mate1 \
                   ~/Sample1_trimmedUnmapped.out.mate2 \
     --outSAMtype BAM SortedByCoordinate \
     --outSAMunmapped Within \
     --outSAMattributes Standard \
     --outFileNamePrefix ~/my_rnaseq/aligned/Sample1/Sample1_trimmed_unmapped

# Quantify viral gene expression
featureCounts -T 20 -t exon -g gene_id -s 0 \
              -a ~/Genome_Index/Reovirus_Genome/reovirus_genome_annot.gtf \
              -o ~/my_rnaseq/aligned/Sample1/Sample1_featureCounts_exon.txt \
              ~/my_rnaseq/aligned/Sample1/Sample1_trimmed_unmappedAligned.sortedByCoord.out.bam
```

## Method 2: The Integrated Alignment Strategy

If you prefer a more streamlined approach, the integrated strategy might be your better choice. Here’s how to implement it effectively.

### Building a Combined Reference

First, we’ll create a comprehensive reference that includes both host and viral genomes. This is like creating a single map that includes both residents and visitors. Genomes and GTF files for model species can be downloaded from [refgenie](http://refgenomes.databio.org/index) and  [GENCODE](https://www.gencodegenes.org/mouse/release_M25.html) as shown in my [previous tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/).

```bash
# Combine the genome sequences
cat ~/Genome_Index/Mouse_Genome/mm10.fa \
    ~/Genome_Index/Reovirus_Genome/Reovirus_Genome.fa \
    > ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_combined.fa

# Merge the gene annotations
# Make sure the GTF files for host and virus are in the same format (9 tab delimited columns)
cat ~/Genome_Index/Mouse_Genome/gencode.vM25.annotation.gtf \
    ~/Genome_Index/Reovirus_Genome/reovirus_genome_annot.gtf \
    > ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_combined.gtf

# Create the combined index
mkdir -p ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_Star_Index

STAR --runThreadN 12 \
     --limitGenomeGenerateRAM 200000000000 \
     --genomeSAindexNbases 6 \
     --runMode genomeGenerate \
     --genomeDir ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_Star_Index \
     --genomeFastaFiles ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_combined.fa \
     --sjdbGTFfile ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_combined.gtf
```

### One-Step Alignment and Quantification

Now we can analyze everything in a single pass:

```bash
# Align reads to the combined genome
STAR --genomeDir ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_Star_Index \
     --limitBAMsortRAM 200000000000 \
     --runThreadN 20 \
     --readFilesIn ~/my_rnaseq/trimmed/Sample1/Sample1_R1_001_val_1.fq.gz \
                   ~/my_rnaseq/trimmed/Sample1/Sample1_R2_001_val_2.fq.gz \
     --outSAMtype BAM SortedByCoordinate \
     --outSAMunmapped Within \
     --outSAMattributes Standard

# Quantify both host and viral genes
featureCounts -T 20 -t exon -g gene_id -s 0 \
              -a ~/Genome_Index/Reovirus_Genome/mm10_Reovirus_combined.gtf \
              -o ~/my_rnaseq/aligned/Sample1/Sample1_featureCounts_exon.txt \
              ~/my_rnaseq/aligned/Sample1/Sample1_trimmedAligned.sortedByCoord.out.bam
```

Let’s examine the quantification results from both analysis methods using our example dataset. In our case study, we observed robust levels of reovirus gene expression across the samples, making it an excellent demonstration of viral transcript detection.

When we compared the gene counts obtained from both methods – the two-step alignment strategy and the integrated approach – we found remarkably consistent results. This concordance is particularly reassuring, as it suggests both methods effectively captured the viral transcriptional landscape in our high-viral-load samples.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-30-at-7.47.17%E2%80%AFPM.png?resize=660%2C380&ssl=1)

## Conclusion

Understanding viral gene expression through RNA-seq analysis is a powerful tool in our arsenal for studying host-pathogen interactions. Through this tutorial, we’ve explored two robust approaches for quantifying viral transcripts in host-associated samples, each with its own strengths and ideal use cases. The two-step strategy offers high specificity and confidence in viral transcript identification, while the integrated approach provides a streamlined workflow for samples with substantial viral content.

As you apply these methods to your own research, remember that the choice between approaches often depends on your specific experimental context. Are you studying early infection with minimal viral load? The two-step strategy might be your best bet. Working with samples showing high viral titers? The integrated approach could save you valuable time while maintaining accuracy.

Keep in mind that while this tutorial focuses on technical implementation, the biological interpretation of your results is equally important. Consider integrating your viral expression data with host transcriptional changes, pathway analyses, and other experimental validations to build a comprehensive picture of the host-pathogen interaction.

## References

- Naqvi AAT, Anjum F, Shafie A, Badar S, Elasbali AM, Yadav DK, et al. (2021) Investigating host-virus interaction mechanism and phylogenetic analysis of viral proteins involved in the pathogenesis. PLoS ONE 16(12): e0261497. https://doi.org/10.1371/journal.pone.0261497

- Joachim Wolff, Bérénice Batut, Helena Rasche, Mapping (Galaxy Training Materials). https://training.galaxyproject.org/training-material/topics/sequence-analysis/tutorials/mapping/tutorial.html Online; accessed Thu Jan 30 2025
