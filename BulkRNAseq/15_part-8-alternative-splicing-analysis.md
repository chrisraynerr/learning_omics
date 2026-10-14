# How to Analyze RNAseq Data for Absolute Beginners Part 8: Alternative Splicing Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** November 20, 2025
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/)

- [Introduction](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Introduction)

  - [Installation and Setup](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Installation_and_Setup)

- [Analysis Workflow](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Analysis_Workflow)

  - [Step 1: Building Splicing Graphs](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Step_1_Building_Splicing_Graphs)
  - [Step 2: Differential Splicing Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Step_2_Differential_Splicing_Analysis)
  - [Step 3: Visualization and Interpretation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Step_3_Visualization_and_Interpretation)

- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-8-alternative-splicing-analysis/#Like_this)

## Introduction

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/ijms-20-02067-g002.png?resize=1024%2C476&ssl=1)

The human genome harbors an elegant solution to the challenge of biological complexity. Through alternative splicing (AS), a single gene can orchestrate the production of multiple protein variants, much like a composer creating different melodies from the same set of musical notes. This remarkable mechanism serves as nature’s way of expanding our proteome’s diversity, allowing cells to dynamically adjust their protein repertoire in response to changing conditions and developmental needs. With the advent of RNA sequencing (RNA-seq) technology, we can now observe these sophisticated molecular choreographies with unprecedented clarity and precision.

The significance of alternative splicing extends far beyond its elegant molecular mechanics. In cancer biology, researchers have discovered that malignant cells often hijack splicing machinery to promote tumor growth, while neurobiologists have revealed how precise splicing patterns shape brain development and function. Cardiovascular researchers have similarly uncovered critical roles for alternative splicing in heart development and disease, demonstrating how this fundamental process touches every corner of human biology.

This tutorial will guide you through the computational analysis of alternative splicing patterns using modern tools and techniques. At its core, alternative splicing manifests through several distinct mechanisms – from the straightforward skipping of exons to the more complex retention of introns and selection of alternative splice sites. Each of these patterns contributes to the remarkable ability of organisms to generate diverse proteins from a relatively modest number of genes. Understanding these patterns and their biological implications forms the foundation for meaningful splicing analysis.

To navigate this complex landscape, we’ll focus on [SplAdder](https://github.com/ratschlab/spladder), a sophisticated computational tool that excels at unraveling splicing patterns from RNA-seq data. SplAdder transforms genetic annotations into comprehensive splicing graphs, augments them with experimental data, and provides robust statistical analysis capabilities. Through this powerful framework, we’ll learn how to detect novel splicing events, quantify their occurrence, and generate compelling visualizations that bring your findings to life.

Whether you’re studying the role of alternative splicing in disease progression, investigating developmental processes, or exploring basic cellular mechanisms, this tutorial will equip you with the knowledge and tools needed to extract meaningful insights from your RNA-seq data. Let’s begin our journey into the fascinating world of alternative splicing analysis.

### Installation and Setup

First, let’s set up our analysis environment on a powerful Linux system as we discussed in our [first tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/):

```bash
# Create a clean conda environment - this isolates our work
conda create -n spladder_env python=3.9

# Activate the new environment
conda activate spladder_env

# Install SplAdder with all dependencies
pip install spladder
```

## Analysis Workflow

### Step 1: Building Splicing Graphs

The first step involves constructing splicing graphs from your RNA-seq data. Specify the paths of your BAM or GTF files if they are not in the current working directory. Here we use the read alignments (BAM files) from our [previous tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/) as the input.

**Note**: BAM files are separated by commas with no spaces between them.

```bash
# Construct splicing graphs from BAM files
spladder build -o spladder_output \
               -b SRR28119110_trimmedAligned.sortedByCoord.out.bam,SRR28119111_trimmedAligned.sortedByCoord.out.bam,SRR28119112_trimmedAligned.sortedByCoord.out.bam,SRR28119113_trimmedAligned.sortedByCoord.out.bam \
               -a gencode.vM25.annotation.gtf \
               --set-mm-tag nM
```

This command performs several crucial steps:

1. Reads your aligned RNA-seq data

2. Constructs initial splicing graphs

3. Augments these graphs with experimental evidence

4. Quantifies detected events

The output directory “spladder_output” contains the following structures. SplAdder identifies 6 types of splicing event:

- exon skips (exon_skip)

- intron retentions (intron_retention)

- alternative 3’ splice sites (alt_3prime)

- alternative 5’ splice sites (alt_5prime)

- mutually exclusive exons (mutex_exons)

- multiple (coordinated) exons skips (mult_exon_skip)

Four types of file were generated for each identified event type: “gff3”, “pickle”, “txt.gz”, and “hdf5”.

- The “gff3” files contain the events that have been detected by SplAdder. Each event is shown as a mini gene consisting of two different isoforms. 

- The “hdf5” event files contain all relevant event information.

- The “txt.gz” files contain essentially the same information as the HDF5 files in a tab delimited column format with one line per event.

- The “pickle” files are intermediate files and can be ignored.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-17-at-6.34.20%E2%80%AFPM.png?resize=1024%2C180&ssl=1)

### Step 2: Differential Splicing Analysis

Once we have our splicing graphs, we can compare conditions:

```bash
# Perform differential analysis
spladder test --conditionA SRR28119110_trimmedAligned.sortedByCoord.out.bam,SRR28119111_trimmedAligned.sortedByCoord.out.bam \
              --conditionB SRR28119112_trimmedAligned.sortedByCoord.out.bam,SRR28119113_trimmedAligned.sortedByCoord.out.bam \
              --outdir spladder_output \
              --labelA KRAS_SPIB \
              --labelB KRAS \
              --plot-format pdf \
              --parallel 8
```

This analysis:

- Compares splicing patterns between conditions

- Calculates statistical significance

- Generates comprehensive reports

- Produces visualizations

The results of this step are stored in the “testing_KRAS_SPIB_vs_KRAS” folder in the “spladder_output” directory.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/Screenshot-2024-11-17-at-6.34.49%E2%80%AFPM.png?resize=1024%2C125&ssl=1)

Each tsv file contains a type of differential splicing event.

- test_results_C3_<event_type>.tsv: These files contain the basic statistical testing results for the given alternative splicing event. They list all the events tested, including duplicates (if an event is associated with multiple genes).

- test_results_C3_<event_type>.gene_unique.tsv: These files contain filtered statistical results, ensuring that each splicing event is linked to a unique gene. They remove duplicates to provide a clearer interpretation of splicing events associated with each gene.

- test_results_extended_C3_<event_type>.tsv: Extended versions of the statistical results files, often including additional information or less stringent filtering criteria. These files may have more splicing events listed compared to the non-extended versions.

- test_setup_C3_<event_type>.pickle: Pickle files that store the experimental setup and parameters used for testing the given alternative splicing event. These files can be used to reload the setup and rerun or extend the analysis.

### Step 3: Visualization and Interpretation

Visualizing differential splicing events helps understand their biological context. You can also choose to visualize the splicing events in specific samples if your dataset doesn’t have comparing groups.

```bash
# Generate comprehensive visualizations
spladder viz --range gene ENSMUSG00000009471.4 \
             --track coverage,segments \
             KRAS_SPIB:SRR28119110_trimmedAligned.sortedByCoord.out.bam,SRR28119111_trimmedAligned.sortedByCoord.out.bam \
            KRAS:SRR28119112_trimmedAligned.sortedByCoord.out.bam,SRR28119113_trimmedAligned.sortedByCoord.out.bam \
             --track event any \
             --track splicegraph \
             -O Myod1_Splicing \
             -o spladder_output

# Generate visualizations for differential analysis
spladder viz --test default any --outbase testing_KRAS_SPIB_vs_KRAS --format pdf --testdir plot
```

This command generates a graph showing all the splicing events (specified by the “–track event any” option) identified in group “KRAS_SPIB” and group “KRAS” for the Myod1 gene. The gene ID for Myod1 gene can be found in the GTF file used in step 1.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/11/image.png?resize=1024%2C803&ssl=1)

## Conclusion

Alternative splicing analysis offers a powerful window into the complexities of gene regulation and its role in health and disease. By unraveling the intricate patterns of exon inclusion, exclusion, and alternative isoform expression, researchers can gain deeper insights into cellular function and identify key mechanisms driving pathological processes.

By carefully applying the methods and best practices outlined in this guide, you can ensure that your analysis yields reliable, reproducible, and biologically meaningful results. As with any bioinformatics workflow, attention to detail in experimental design, data processing, and interpretation will maximize the impact of your findings, paving the way for novel discoveries in transcriptomics and beyond.

## References

1. Bowler E, Oltean S. Alternative Splicing in Angiogenesis. International Journal of Molecular Sciences. 2019; 20(9):2067. https://doi.org/10.3390/ijms20092067

2. Gazmend Temaj, Silvia Chichiarelli, Sarmistha Saha, Pelin Telkoparan-Akillilar, Nexhibe Nuhii, Rifat Hadziselimovic, Luciano Saso. An intricate rewiring of cancer metabolism via alternative splicing. Biochemical Pharmacology. Volume 217, 2023, 115848, ISSN 0006-2952, https://doi.org/10.1016/j.bcp.2023.115848.

3. Rocco Sciarrillo, Anna Wojtuszkiewicz, Yehuda G. Assaraf, Gerrit Jansen, Gertjan J.L. Kaspers, Elisa Giovannetti, Jacqueline Cloos. The role of alternative splicing in cancer: From oncogenesis to drug resistance. Drug Resistance Updates. Volume 53, 2020, 100728, ISSN 1368-7646. https://doi.org/10.1016/j.drup.2020.100728.

4. André Kahles, Cheng Soon Ong, Yi Zhong, Gunnar Rätsch, SplAdder: identification, quantification and testing of alternative splicing events from RNA-Seq data, Bioinformatics, Volume 32, Issue 12, June 2016, Pages 1840–1847, https://doi.org/10.1093/bioinformatics/btw076
