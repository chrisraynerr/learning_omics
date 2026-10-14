# How to Set Up a Bulk RNA-seq Pipeline on an HPC Cluster &#8212; A Complete Beginner's Guide to Nextflow and nf-core/rnaseq

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** HPC, Scientific Programming
- **Source:** [https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/)

---

*From raw FASTQ files to a gene count matrix — let Nextflow orchestrate hundreds of SLURM jobs for you, with full reproducibility and zero manual bookkeeping*

---

If you have RNA-seq FASTQ files sitting on your HPC cluster, you face a choice. You can write a separate SLURM submission script for every sample and every processing step — FastQC, trimming, alignment, quantification — then manually track which jobs finished, which failed, and restart from the beginning whenever something breaks. Or you can describe your pipeline once and let a workflow manager handle all of it: parallelizing across samples, submitting jobs to SLURM, monitoring them, recovering from failures, and producing a publication-ready quality report at the end.

This tutorial teaches the second approach using **Nextflow**, the dominant workflow management system in modern bioinformatics, together with **nf-core/rnaseq** (v3.26.0), the community-standard bulk RNA-seq pipeline. By the end you will understand what Nextflow is and how it works, and you will have deployed a complete RNA-seq analysis on a SLURM cluster — from FASTQ files to a gene count matrix ready for differential expression analysis.

No prior Nextflow experience is assumed. This guide is written for wet-lab scientists and researchers who are comfortable on the Linux command line but new to pipeline automation. Throughout, we follow a single concrete worked example — a real four-sample mouse RNA-seq run on a generic SLURM cluster — so every path, module, and config value ties together into a pipeline that runs end to end. The cluster names, paths, and module versions shown are placeholders; substitute your own where indicated.

> 

**Prerequisites:** Access to an HPC cluster running SLURM, RNA-seq FASTQ files available on that cluster, and Singularity (or Apptainer) installed. New to SLURM itself? Read the [NGS101 SLURM Beginner’s Guide](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/) first.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/)

- [Introduction: What Is Nextflow and Why Does It Matter?](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Introduction_What_Is_Nextflow_and_Why_Does_It_Matter)

  - [The Problem with Manual Pipeline Management](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#The_Problem_with_Manual_Pipeline_Management)
  - [What Nextflow Actually Is](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#What_Nextflow_Actually_Is)
  - [How Nextflow Compares to Writing Bash Scripts](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#How_Nextflow_Compares_to_Writing_Bash_Scripts)

- [Background: The Three Core Concepts](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Background_The_Three_Core_Concepts)

  - [1. Processes — The Units of Work](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#1_Processes_%E2%80%94_The_Units_of_Work)
  - [2. Channels — The Data Highways](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#2_Channels_%E2%80%94_The_Data_Highways)
  - [3. Workflows — The Orchestration Layer](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#3_Workflows_%E2%80%94_The_Orchestration_Layer)
  - [DSL2 — The Modern Syntax](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#DSL2_%E2%80%94_The_Modern_Syntax)

- [Background: How Nextflow Runs on a SLURM Cluster](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Background_How_Nextflow_Runs_on_a_SLURM_Cluster)

  - [The Configuration System](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#The_Configuration_System)

- [What Is nf-core/rnaseq?](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#What_Is_nf-corernaseq)

  - [nf-core — The Community Pipeline Library](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#nf-core_%E2%80%94_The_Community_Pipeline_Library)
  - [What the Pipeline Does](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#What_the_Pipeline_Does)
  - [Aligner Options](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Aligner_Options)

- [Step 1: Load Java and Nextflow](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_1_Load_Java_and_Nextflow)
- [Step 2: Verify Singularity](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_2_Verify_Singularity)
- [Step 3: Set Up the Singularity Cache Directory](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_3_Set_Up_the_Singularity_Cache_Directory)
- [Step 4: Create the Project Directory Structure](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_4_Create_the_Project_Directory_Structure)
- [Step 5: Download the Pipeline](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_5_Download_the_Pipeline)
- [Step 6: Run the Test Profile First](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_6_Run_the_Test_Profile_First)
- [Step 7: Download the Reference Genome and Annotation](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_7_Download_the_Reference_Genome_and_Annotation)
- [Step 8: Build Your Samplesheet](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_8_Build_Your_Samplesheet)

  - [Obtaining example data (optional)](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Obtaining_example_data_optional)
  - [Writing the samplesheet](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Writing_the_samplesheet)

- [Step 9: Write the nextflow.config](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_9_Write_the_nextflowconfig)
- [Step 10: Write the SLURM Submission Script](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_10_Write_the_SLURM_Submission_Script)
- [Step 11: Run the Real Pipeline](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_11_Run_the_Real_Pipeline)
- [Step 12: Monitor, Resume, and Interpret the Output](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_12_Monitor_Resume_and_Interpret_the_Output)

  - [Monitoring a Running Pipeline](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Monitoring_a_Running_Pipeline)
  - [Resuming After a Failure](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Resuming_After_a_Failure)
  - [Understanding the Output Directory](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Understanding_the_Output_Directory)
  - [Which files you will actually use](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Which_files_you_will_actually_use)
  - [Choosing the right count file for differential expression](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Choosing_the_right_count_file_for_differential_expression)
  - [Do the DE tools not require integer counts?](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Do_the_DE_tools_not_require_integer_counts)
  - [Route 1 (recommended): the SummarizedExperiment / tximport route](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Route_1_recommended_the_SummarizedExperiment_tximport_route)
  - [Route 2 (convenient alternative): the length-scaled matrix](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Route_2_convenient_alternative_the_length-scaled_matrix)
  - [Route 3 (discouraged): the plain count matrix alone](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Route_3_discouraged_the_plain_count_matrix_alone)
  - [A note on the featurecounts/ folder](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#A_note_on_the_featurecounts_folder)
  - [Execution reports](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Execution_reports)
  - [Quality Metrics to Check in MultiQC](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Quality_Metrics_to_Check_in_MultiQC)

- [Step 13: Clean Up the Work Directory](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Step_13_Clean_Up_the_Work_Directory)
- [Useful Optional Parameters](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Useful_Optional_Parameters)
- [Best Practices and Common Pitfalls](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Best_Practices_and_Common_Pitfalls)

  - [Best Practices](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Best_Practices)

- [Key takeaways:](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Key_takeaways)
- [References](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#References)

  - [Share this:](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Share_this)
  - [Like this:](https://ngs101.com/how-to-set-up-a-bulk-rna-seq-pipeline-on-an-hpc-cluster-a-complete-beginners-guide-to-nextflow-and-nf-core-rnaseq/#Like_this)

## Introduction: What Is Nextflow and Why Does It Matter?

### The Problem with Manual Pipeline Management

A traditional RNA-seq workflow on an HPC quickly becomes an exercise in bookkeeping. Suppose you have 12 samples and an 8-step analysis. Without a workflow manager, you write submission scripts for each step, submit them by hand, and wait. When a single job fails silently overnight, you may not notice until the next morning. When you want to rerun one step with a tweaked parameter, you risk re-running everything. With every additional sample, the manual overhead grows, and so does the chance of a subtle error — a swapped file, a forgotten sample, an inconsistent parameter.

Nextflow eliminates this entire class of problems. You describe the pipeline once — which tools run in what order, what each takes as input, and what each produces — and Nextflow handles the logistics: it parallelizes across samples automatically, submits and monitors SLURM jobs, retries failures, and resumes from where it stopped. You focus on the science; Nextflow handles the orchestration.

### What Nextflow Actually Is

Nextflow is a **workflow management system** — a program designed to run other programs in an organized, automated, and reproducible way. It was built specifically for scientific data pipelines and has become the standard for bioinformatics workflows alongside Snakemake.

Four features make Nextflow especially well-suited to HPC work. First, it natively speaks SLURM: it submits each task as a separate `sbatch` job automatically, so you never write batch scripts by hand. Second, it uses containers for software, meaning every tool runs inside a Singularity container with its dependencies pre-installed — you do not install or manage STAR, Salmon, or FastQC yourself. Third, it has a smart caching system: if your pipeline fails at step 8 of 10, you fix the problem and rerun with a single flag, and Nextflow skips the seven completed steps and resumes from where it stopped. Fourth, it is platform-agnostic: the same pipeline runs unchanged on your laptop, your HPC, or the cloud.

### How Nextflow Compares to Writing Bash Scripts

| Aspect | Manual Bash + SLURM | Nextflow |
| --- | --- | --- |
| Parallelization | Submit one script per sample by hand | Automatic across all samples |
| Failure recovery | Restart from scratch | Resume from last successful step |
| Software management | Install and version every tool yourself | Containerized, version-pinned automatically |
| Reproducibility | Depends on your discipline | Built in via version pinning and containers |
| Portability | Rewrite for each cluster | Same code; only config changes |

---

## Background: The Three Core Concepts

Before deploying anything, it helps to understand the three building blocks every Nextflow pipeline is made from. Understanding these makes every configuration option and every error message far easier to interpret.

### 1. Processes — The Units of Work

A **process** is a single computational step. It wraps one tool or command and declares exactly what inputs it needs, what outputs it produces, and what shell command to run. Each process instance runs in its own isolated temporary directory, so parallel jobs never interfere with one another.

Here is a simplified FastQC process written in Nextflow’s modern DSL2 syntax:

```
process FASTQC {
    container 'biocontainers/fastqc:v0.12.1'   // which container holds this tool
    cpus   2                                    // resource requirements
    memory '4 GB'
    time   '1 h'

    input:                                      // what comes in
    tuple val(sample_id), path(reads)

    output:                                     // what comes out
    tuple val(sample_id), path("*.html"), emit: html
    tuple val(sample_id), path("*.zip"),  emit: zip

    script:                                     // the actual command
    """
    fastqc --threads ${task.cpus} ${reads}
    """
}
```

You will rarely write processes from scratch — nf-core provides hundreds of pre-written, tested, containerized modules. But reading one clarifies what is happening when the pipeline runs.

### 2. Channels — The Data Highways

A **channel** carries data between processes, like a conveyor belt: items are placed on one end and consumed by the next process at the other. The defining feature is that each item in a channel spawns its own independent job. If a channel contains 12 pairs of FASTQ files, Nextflow automatically launches 12 parallel FastQC jobs — one per item. You never write a loop; the parallelization is implicit in the channel structure.

```
// Build a channel of paired FASTQ files -- one item per sample
reads_ch = Channel.fromFilePairs("data/*_R{1,2}.fastq.gz")

// Each item spawns one FastQC job; 12 samples means 12 parallel jobs
FASTQC(reads_ch)
```

This is the key idea behind Nextflow’s power: you do not loop over samples, you place them in a channel and let Nextflow parallelize automatically.

### 3. Workflows — The Orchestration Layer

The **workflow** block connects processes together by wiring the output channel of one process into the input channel of the next. It defines the execution graph — the order and dependency structure of the analysis.

```
workflow {
    reads_ch = Channel.fromFilePairs(params.reads)

    FASTQC(reads_ch)                                  // QC on raw reads
    TRIMGALORE(reads_ch)                              // trim adapters (also uses raw reads)
    STAR_ALIGN(TRIMGALORE.out.reads, params.index)    // align trimmed reads
    SALMON_QUANT(STAR_ALIGN.out.bam, params.tx)       // quantify aligned reads

    MULTIQC(                                          // collect QC from all steps
        FASTQC.out.zip.collect(),
        TRIMGALORE.out.log.collect(),
        STAR_ALIGN.out.log.collect()
    )
}
```

Nextflow infers which steps can run in parallel (FastQC and trimming both consume raw reads, so they run simultaneously) and which must wait (STAR cannot start until trimming finishes). You never specify this explicitly — it is derived from the channel dependencies.

### DSL2 — The Modern Syntax

You will see “DSL2” throughout Nextflow documentation. DSL stands for Domain Specific Language, and DSL2 is the current version. Its key innovation was **modularity**: processes live in separate files and are imported where needed, rather than crowding into one giant script. This modularity is exactly what makes nf-core possible — every tool exists as a standalone, versioned, tested module that any pipeline can import. You do not need to write DSL2 to run prebuilt pipelines, but knowing the term helps when reading documentation.

---

## Background: How Nextflow Runs on a SLURM Cluster

This two-layer architecture confuses most beginners, so it is worth making explicit. When you run Nextflow on a SLURM cluster, there are two distinct layers operating at once.

```r
Your terminal / login node
        |
        | (you run this)
        v
  nextflow run ...          <-- The Nextflow ORCHESTRATOR process
        |                       lightweight: 1-2 CPUs, 4-8 GB RAM, runs for hours
        |                       submits jobs, watches status, manages the cache
        |
        | sbatch job1   sbatch job2   sbatch job3  ...
        v
  SLURM Scheduler           <-- receives hundreds of individual job submissions
        |
        v
  Compute Nodes             <-- the actual tools run here (STAR, Salmon, FastQC)
                                each in its own isolated task directory,
                                each inside a Singularity container
```

The Nextflow process itself is the orchestrator. It does not run STAR or Salmon — it submits those as separate SLURM jobs and tracks their results. It needs very little compute (a couple of CPUs and a few GB of RAM) but it must stay alive for the entire duration of the pipeline, which can be many hours. This is why you run it inside `screen`, `tmux`, or a small dedicated SLURM job — so the orchestrator survives even after you log out.

### The Configuration System

Nextflow enforces a strict separation between **what the pipeline does** (the `.nf` code) and **how it runs** (the configuration). You never modify the pipeline code to adapt it to your cluster. Instead you supply a configuration file — `nextflow.config` — describing your execution environment. Configuration layers load in order, with later layers overriding earlier ones:

```
Pipeline defaults  -->  ~/.nextflow/config  -->  nextflow.config  -->  command-line flags
(lowest priority)                                                       (highest priority)
```

A typical HPC `nextflow.config` tells Nextflow four things: use SLURM as the executor, use Singularity for containers, where to cache container images, and what the resource ceilings are for a single node. The `-profile` flag is a shortcut for a named block of these settings — `singularity` enables Singularity, `slurm` enables the SLURM executor, and you can combine them as `-profile slurm,singularity`.

---

## What Is nf-core/rnaseq?

### nf-core — The Community Pipeline Library

nf-core is an open-source community that builds, peer-reviews, and maintains production-quality Nextflow pipelines for common bioinformatics workflows. Every nf-core pipeline must pass strict code review, follow standardized conventions, be fully containerized, ship with test datasets, and carry comprehensive documentation. More than 100 nf-core pipelines exist, covering bulk RNA-seq, scRNA-seq, ChIP-seq, ATAC-seq, whole-genome sequencing, metagenomics, and more.

For bulk RNA-seq, **nf-core/rnaseq** is the community standard. Rather than writing and validating your own alignment pipeline, you use one that hundreds of research groups have tested in production.

### What the Pipeline Does

nf-core/rnaseq analyses RNA sequencing data from organisms with a reference genome and annotation. It takes a samplesheet of FASTQ files (or pre-aligned BAM files) as input, performs quality control, trimming, and (pseudo-)alignment, and produces a gene expression matrix together with an extensive QC report. The pipeline runs the following steps:

| Stage | Tool(s) | Purpose |
| --- | --- | --- |
| Merge FASTQs | cat | Concatenates multi-lane files from the same sample |
| FASTQ validation | fq lint | Validates FASTQ integrity before processing starts |
| Read QC | FastQC | Per-base quality, adapter content, GC bias |
| Strandedness detection | Salmon | Automatically infers library strandedness |
| Adapter trimming | TrimGalore or fastp | Removes adapters and low-quality bases |
| rRNA removal (optional) | SortMeRNA | Removes ribosomal RNA reads |
| Alignment + quantification | STAR + Salmon | Aligns to genome, quantifies with Salmon |
| Duplicate marking | Picard MarkDuplicates | Flags PCR duplicates for QC |
| Alignment QC | RSeQC, Qualimap, dupRadar | Read distribution, junction saturation, duplication |
| Transcript assembly | StringTie | Assembles transcripts and estimates expression |
| Summary report | MultiQC | Aggregates every QC metric into one HTML report |

The primary output for downstream work is the **gene count matrix** produced by Salmon — a table of read counts per gene per sample that feeds directly into DESeq2, edgeR, or limma-voom.

### Aligner Options

The pipeline supports several alignment strategies. For most standard bulk mRNA-seq, the default STAR + Salmon combination is the right choice.

`--aligner star_salmon``--aligner star_rsem``--aligner hisat2``--pseudo_aligner salmon`

| Strategy | Flag | Best for |
| --- | --- | --- |
| STAR + Salmon (default) |  | Standard mRNA-seq, most species |
| STAR + RSEM |  | When isoform-level quantification is the priority |
| HISAT2 |  | Splice-aware alignment without Salmon |
| Salmon pseudoalignment |  | Fast quantification when speed outweighs alignment accuracy |

This tutorial uses the default `star_salmon` aligner throughout.

---

## Step 1: Load Java and Nextflow

Nextflow runs on the Java Virtual Machine and requires Java 17 or higher (Java 21 LTS is an excellent, well-supported choice). On a managed HPC, the simplest path is to load the cluster’s pre-built modules rather than installing anything yourself. Start an interactive working session and load the relevant modules (module names and versions vary by cluster — use `module avail` to find yours):

```bash
# Start a persistent terminal session so long downloads survive a disconnect
tmux new -s nextflow_setup

# Load the toolchain (module names are cluster-specific -- adjust to yours)
module load conda
module load sratoolkit          # only needed if downloading example data from SRA
module load java/21       # Java 21 LTS -- well within Nextflow's supported range
module load nextflow          # the Nextflow orchestrator
module load singularity       # container engine
module load tree                # handy for inspecting the output directory later

# Verify the core tools
java -version
nextflow -version
```

If your cluster does not provide a Nextflow module, you can install it yourself in a writable directory:

```
mkdir -p ~/bin
curl -s https://get.nextflow.io | bash
mv nextflow ~/bin/
export PATH="$HOME/bin:$PATH"
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
nextflow -version
```

We also create a small conda environment to hold the `nf-core` helper tools (used later to download the pipeline) without touching the system Python:

```
conda create -p /projects/mylab/shared/envs/nf_tools python=3.10
conda activate /projects/mylab/shared/envs/nf_tools
```

> 

**HPC tip:** Loading modules and configuring your environment on the login node is fine — the orchestrator is lightweight. The heavy work runs on compute nodes as SLURM jobs.

---

## Step 2: Verify Singularity

Most HPC clusters use Singularity for containers, because Docker requires root privileges unavailable on shared systems. You may also see **Apptainer** installed instead: when the open-source Singularity project joined the Linux Foundation in 2021, it was renamed Apptainer, while a separately maintained fork called SingularityCE continues under the original name. The two are actively developed projects from a common origin, but for the purposes of this tutorial they behave the same way.

```bash
# Check availability (loaded as a module in Step 1)
singularity --version
# or, on clusters that use Apptainer
apptainer --version
```

If neither is available, contact your HPC support team — it is a standard request, and almost all modern research clusters provide one of them.

> 

**Does Apptainer change any of the commands in this tutorial?** No. Apptainer ships a `singularity` command alias and honors the older `SINGULARITY_*` environment variables, so everything here works unchanged on an Apptainer cluster as long as you keep the `singularity` naming — the `-profile singularity` flag, the `singularity { }` config scope (Step 9), and the `NXF_SINGULARITY_CACHEDIR` variable (Step 3). Nextflow also provides a native `apptainer` config scope if you prefer to be explicit: in that case, rename the `singularity { }` block to `apptainer { }` in your `nextflow.config` and use `NXF_APPTAINER_CACHEDIR` in place of `NXF_SINGULARITY_CACHEDIR`. Both routes are fully supported; the `singularity` route is the simplest and requires no changes.

---

## Step 3: Set Up the Singularity Cache Directory

This step is critical and frequently overlooked. nf-core pipelines pull container images from the internet on first use. These images run 1–4 GB each, and nf-core/rnaseq uses many of them. Without a dedicated cache directory, Nextflow re-downloads the same images on every run, wasting both time and storage.

Point the cache at **persistent project or group storage**, not your home directory (which usually has a tight quota) and not scratch. This tutorial uses `/projects/mylab/shared` as a placeholder persistent path — substitute whatever persistent, non-purged space your cluster provides (common names include `/projects/`, `/work/`, `/group/`, or a lab-specific allocation; ask your HPC admin if unsure).

```bash
# Create a cache directory in PERSISTENT project space
mkdir -p /projects/mylab/shared/singularity_cache

# Set the environment variable, and persist it in ~/.bashrc
export NXF_SINGULARITY_CACHEDIR="/projects/mylab/shared/singularity_cache"
echo 'export NXF_SINGULARITY_CACHEDIR="/projects/mylab/shared/singularity_cache"' >> ~/.bashrc
source ~/.bashrc
```

> 

**Why this matters:** The first run may spend 30–60 minutes downloading containers, depending on your cluster’s connection. Every subsequent run that uses the same containers starts almost immediately because Nextflow finds them already cached.

> 

**Do not put the cache on scratch.** Scratch filesystems are typically purged automatically — files untouched for 30 to 90 days are deleted without warning. If your container images live on purged scratch, they will silently disappear between runs, forcing a slow re-download (or an outright failure if the compute nodes have no internet at that moment). The container images are expensive to obtain and meant to persist, so they belong on storage that is never purged.

---

## Step 4: Create the Project Directory Structure

Nextflow generates many files, so a clean directory layout prevents confusion later. We separate four kinds of data by where they belong: project files and references on persistent project storage, and the transient work directory on fast scratch.

```bash
# Define the persistent project base once
BASE=/projects/mylab/shared

# Create the project layout, the reference store, and the scratch work directory
mkdir -p $BASE/project_test/{scripts,data/{meta,raw,processed},results} \
         $BASE/reference/{hg38,GRCm39,GRCr8} \
         /scratch/$USER/work
```

The finished layout looks like this:

```
/projects/mylab/shared/
|-- project_test/
|   |-- data/
|   |   |-- meta/         # samplesheet.csv lives here
|   |   |-- raw/          # raw FASTQ files
|   |   |-- processed/    # any pre-processed inputs (optional)
|   |-- results/          # populated by the pipeline
|   |-- scripts/          # nextflow.config, run_nextflow.sh, logs, reports
|-- reference/
|   |-- hg38/             # human GRCh38 FASTA + GTF
|   |-- GRCm39/           # mouse GRCm39 FASTA + GTF
|   |-- GRCr8/            # rat GRCr8 FASTA + GTF
|-- singularity_cache/    # container images (Step 3)

/scratch/$USER/
|-- work/                 # Nextflow task scratch (transient -- on fast scratch)
```

Notice the deliberate split: the project, references, and container cache sit on **persistent** project storage, while the `work/` directory sits on **fast scratch** (`/scratch`). This is exactly the right division — the work directory is large and regenerable, so scratch is the correct home for it, whereas anything costly to recreate stays on persistent storage.

---

## Step 5: Download the Pipeline

You do not download the pipeline every time you run it. Nextflow fetches the pipeline code once, stores it on disk, and reuses it on every subsequent run — it only re-downloads if you request a different version with `-r` or explicitly clear the cache. The question is simply *where* that copy lives, and you have two approaches. On clusters where compute nodes have slow or restricted internet access, downloading in advance to a known location is the more reliable choice.

**Option A — Automatic pull at runtime (simpler; needs internet on the login node):**

```bash
# Nextflow downloads the pipeline the first time you run it, then reuses it
nextflow run nf-core/rnaseq -r 3.26.0 -profile test,singularity --outdir results_test
```

With this approach, Nextflow stores the pipeline code in its **assets cache**, which lives inside Nextflow’s home directory:

```bash
$NXF_HOME/assets/nf-core/rnaseq/
```

`NXF_HOME` defaults to `~/.nextflow`, so without any customization the pipeline lands at `~/.nextflow/assets/nf-core/rnaseq/`. The path mirrors the GitHub `organization/repository` structure because Nextflow is effectively cloning the pipeline’s git repository; different revisions are stored as git tags within that same directory, and Nextflow checks out the one you specify with `-r` at run time. You can confirm what is cached and where with `nextflow list` and `nextflow info nf-core/rnaseq`.

Note that `NXF_HOME` is more than just a pipeline store — it is Nextflow’s general home directory, holding the assets (pipeline code), downloaded plugins, the framework runtime, cached Java dependencies, and a run-history log:

```
~/.nextflow/
|-- assets/      # downloaded pipeline code (e.g. assets/nf-core/rnaseq/)
|-- plugins/     # downloaded Nextflow plugins (nf-validation, nf-schema, etc.)
|-- framework/   # the Nextflow runtime, per version
|-- capsule/     # cached Java dependencies (JARs) Nextflow needs to run
|-- tmp/         # temporary working files
|-- history      # a log of past runs
```

On HPC clusters where the home directory has a tight quota — or where Nextflow is a shared module installed in a read-only system path — relocate this entire directory to **persistent project or group storage** before your first run:

```bash
# Redirect Nextflow's home to PERSISTENT project space (do this before downloading)
export NXF_HOME=/projects/mylab/shared/.nextflow
echo 'export NXF_HOME=/projects/mylab/shared/.nextflow' >> ~/.bashrc
source ~/.bashrc
```

> 

**Do not point `NXF_HOME` at scratch.** Like the container cache, `NXF_HOME` holds files meant to persist and be reused across runs — downloaded pipeline code, plugins, and the framework runtime. Scratch is purged periodically, so a scratch-based `NXF_HOME` will silently lose your cached pipelines between projects. Use persistent project storage, or leave it at the default `~/.nextflow` if your home quota allows.

**Option B — Explicit download to a fixed location (recommended for HPC):**

If you want full control over where the pipeline lives — for example, a shared `pipelines` directory you reuse across projects — download it explicitly with the nf-core helper tools. This needs no internet at run time and keeps the location entirely in your hands.

```bash
# Install the nf-core helper tools (into the conda env created in Step 1)
pip install nf-core

# Download the pipeline code AND all its Singularity containers, once
mkdir -p /projects/mylab/shared/pipelines
nf-core pipelines download nf-core/rnaseq \
    --revision 3.26.0 \
    --container-system singularity \
    --container-cache-utilisation amend \
    --outdir /projects/mylab/shared/pipelines/nf-core-rnaseq-3.26.0
```

When prompted to choose a compression type, select **none** — compression only helps if you intend to copy the whole bundle to a different machine, and it adds a slow packaging step that saves little space on already-compressed container images. The download itself can take 30–90 minutes; the `tmux` session from Step 1 keeps it alive if you disconnect.

This creates the following structure (note that dots in the version become underscores in the code folder name):

```
/projects/mylab/shared/pipelines/nf-core-rnaseq-3.26.0/
|-- 3_26_0/      # the actual pipeline code
|-- configs/     # bundled institutional config profiles
```

Here `--outdir` means “where to put the downloaded pipeline” — a different meaning from the `--outdir` on `nextflow run`, which sets where *results* go. The flag’s meaning depends on which command it belongs to.

The containers are downloaded at the same time. With `--container-cache-utilisation amend`, the images go into your `NXF_SINGULARITY_CACHEDIR` (Step 3), so make sure that variable is set before downloading and set to the same value when you run. This keeps a single shared image cache rather than bundling copies inside the pipeline folder.

To run from the downloaded copy, point `nextflow run` at the local code directory (`.../nf-core-rnaseq-3.26.0/3_26_0`) and drop the `-r` flag, since revision selection only applies when pulling from a remote repository. Alternatively — and this is what the run script in Step 10 does — you can keep using the convenient `nf-core/rnaseq -r 3.26.0` shorthand; once `NXF_HOME` points at writable storage, Nextflow reuses the cached copy either way.

> 

**Keep your storage locations straight.** Four independent locations are in play, and on HPC they often sit on different filesystems. The pipeline code lives in `$NXF_HOME/assets/` (Option A) or your explicit download folder (Option B); the container images live in `NXF_SINGULARITY_CACHEDIR` (Step 3); the results go to `--outdir`; and the per-task scratch goes to `-work-dir`. The deciding factor for each is persistence: the pipeline code and container images are costly to obtain and must be reused, so they belong on **persistent, non-purged** project storage; the `work/` directory is large, transient, and deleted after every run, so it belongs on **fast scratch** (`/scratch/$USER/work` in this tutorial), which is exactly what scratch is designed for.

---

## Step 6: Run the Test Profile First

Before committing your real data to a run that may take hours, always validate the setup with the nf-core test profile. It runs the full pipeline on a tiny public dataset designed to finish in minutes and confirm that every piece — Nextflow, Singularity, the container pulls, and the SLURM executor — is wired together correctly. Doing this immediately after the pipeline is installed (and before you invest time in references, samplesheets, and the full config) means you discover environment problems in minutes rather than after a long real run fails.

The test profile bundles its own tiny dataset and small resource requests, so you only need to tell Nextflow to route jobs through SLURM with Singularity. Create a minimal config for this purpose:

```
nano /projects/mylab/shared/project_test/scripts/test.config
```

```bash
// minimal config -- just enough to route the test profile through SLURM
singularity {
    enabled    = true
    autoMounts = true
}

process {
    executor = 'slurm'
    queue    = 'compute'    // your SLURM partition -- check with: sinfo

    // resourceLimits caps any single job; safe ceilings for an compute node
    resourceLimits = [ cpus: 128, memory: 248.GB, time: 96.h ]
}
```

Then launch the test run:

```
cd /projects/mylab/shared/project_test/scripts

nextflow run nf-core/rnaseq \
    -r 3.26.0 \
    -profile test,singularity \
    -c test.config \
    --outdir results_test \
    -work-dir /scratch/$USER/work_test \
    -ansi-log false
```

The test run pulls any containers not yet cached, fetches a small test dataset automatically, and runs the full pipeline on roughly 100,000 reads per sample. It typically completes in 20–60 minutes. A successful run ends with a clear confirmation:

```
executor >  slurm (47)
[83/4adf12] NFCORE_RNASEQ:...:FASTQC                  [100%] 2 of 2 OK
[a1/8bc923] NFCORE_RNASEQ:...:STAR_ALIGN              [100%] 2 of 2 OK
[ff/2de109] NFCORE_RNASEQ:...:SALMON_QUANT            [100%] 2 of 2 OK
[55/9afc11] NFCORE_RNASEQ:...:MULTIQC                 [100%] 1 of 1 OK

Pipeline completed successfully!
```

If the test fails, the error message usually points to one of a handful of setup problems: a wrong partition name (run `sinfo`), Java or Nextflow not loaded (re-check the `module load` lines), Singularity unavailable (`singularity --version`), or `NXF_HOME` pointing at a read-only path (see Troubleshooting). Once the test profile succeeds, you can trust that your environment is ready for real data.

---

## Step 7: Download the Reference Genome and Annotation

nf-core/rnaseq needs two reference files: the genome sequence (FASTA) and the gene annotation (GTF). The pipeline builds a STAR index from these automatically the first time it runs, so you do not need to build the index yourself — but the FASTA and GTF must be present and must come from the same Ensembl release.

This example downloads three species into the reference store created in Step 4 (human, mouse, and rat from Ensembl release 116), because the project may analyze more than one organism. The actual run in this tutorial uses the mouse (GRCm39) reference. Download only the species you need:

```bash
# ---- Human (GRCh38) ----
cd /projects/mylab/shared/reference/hg38
wget https://ftp.ensembl.org/pub/release-116/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
wget https://ftp.ensembl.org/pub/release-116/gtf/homo_sapiens/Homo_sapiens.GRCh38.116.gtf.gz
gunzip *

# ---- Mouse (GRCm39) -- used in this tutorial ----
cd /projects/mylab/shared/reference/GRCm39
wget https://ftp.ensembl.org/pub/release-116/fasta/mus_musculus/dna/Mus_musculus.GRCm39.dna.primary_assembly.fa.gz
wget https://ftp.ensembl.org/pub/release-116/gtf/mus_musculus/Mus_musculus.GRCm39.116.gtf.gz
gunzip *

# ---- Rat (GRCr8) ----
cd /projects/mylab/shared/reference/GRCr8
wget https://ftp.ensembl.org/pub/release-116/fasta/rattus_norvegicus/dna/Rattus_norvegicus.GRCr8.dna.toplevel.fa.gz
wget https://ftp.ensembl.org/pub/release-116/gtf/rattus_norvegicus/Rattus_norvegicus.GRCr8.116.gtf.gz
gunzip *
```

> 

**The genome and GTF version must match.** Pairing a release 116 genome with a release 114 GTF causes chromosome-name and gene-ID mismatches that silently corrupt quantification. Always download both from the same release — here, release 116 for both files.

> 

**Primary assembly vs toplevel.** For human and mouse, use the `primary_assembly` FASTA, which excludes the redundant alternative-haplotype contigs and is the recommended input for RNA-seq alignment. Some species (such as rat GRCr8) are only distributed as `toplevel`, which is fine to use when no primary assembly is published.

> 

**Check for a shared reference first.** Many clusters maintain a shared reference directory. Ask your HPC admin before downloading — you may save significant storage and time.

---

## Step 8: Build Your Samplesheet

The samplesheet is a CSV file telling nf-core/rnaseq which FASTQ files belong to which samples. It must be comma-separated with four required columns: `sample`, `fastq_1`, `fastq_2`, and `strandedness`.

### Obtaining example data (optional)

If you are following along rather than using your own data, this tutorial uses four public mouse RNA-seq samples (a KRAS vs KRAS+SPIB comparison, two replicates each) from the dataset featured in the [NGS101 RNA-seq Part 2 tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/). Download them from SRA with the SRA Toolkit loaded in Step 1:

```bash
cd /projects/mylab/shared/project_test/data/raw

# Download the four runs
fasterq-dump SRR28119110
fasterq-dump SRR28119111
fasterq-dump SRR28119112
fasterq-dump SRR28119113

# Compress
gzip *.fastq

# Rename to the _R1_001 / _R2_001 convention many tools expect
rename '_1.fastq.gz' '_R1_001.fastq.gz' *_1.fastq.gz
rename '_2.fastq.gz' '_R2_001.fastq.gz' *_2.fastq.gz
```

### Writing the samplesheet

```
nano /projects/mylab/shared/project_test/data/meta/samplesheet.csv
```

For this paired-end mouse dataset, the samplesheet maps each sample name to its R1/R2 files using absolute paths:

```
sample,fastq_1,fastq_2,strandedness
KRAS_SPIB_1,/projects/mylab/shared/project_test/data/raw/SRR28119110_R1_001.fastq.gz,/projects/mylab/shared/project_test/data/raw/SRR28119110_R2_001.fastq.gz,auto
KRAS_SPIB_2,/projects/mylab/shared/project_test/data/raw/SRR28119111_R1_001.fastq.gz,/projects/mylab/shared/project_test/data/raw/SRR28119111_R2_001.fastq.gz,auto
KRAS_1,/projects/mylab/shared/project_test/data/raw/SRR28119112_R1_001.fastq.gz,/projects/mylab/shared/project_test/data/raw/SRR28119112_R2_001.fastq.gz,auto
KRAS_2,/projects/mylab/shared/project_test/data/raw/SRR28119113_R1_001.fastq.gz,/projects/mylab/shared/project_test/data/raw/SRR28119113_R2_001.fastq.gz,auto
```

For single-end data, leave the `fastq_2` column empty. The strandedness column accepts four values:

`auto`**Recommended for beginners.**`forward``reverse``unstranded`

| Value | When to use |
| --- | --- |
|  | The pipeline subsamples 1 million reads and uses Salmon to detect strandedness automatically. |
|  | Use when you know the library is forward-stranded (e.g. some Lexogen kits). |
|  | Use when you know the library is reverse-stranded (most TruSeq-based kits). |
|  | Use for unstranded libraries (uncommon in modern protocols). |

Using `auto` is the safest choice when unsure: the pipeline sub-samples each library to 1 million reads, uses Salmon to infer strandedness, and propagates that information through the rest of the run.

If the same sample was sequenced across multiple lanes, give each lane the same `sample` identifier — the pipeline concatenates the reads before downstream analysis.

> 

**Always use absolute paths.** Relative paths fail because each Nextflow task runs in its own subdirectory of `work/`, where a relative path cannot resolve.

Verify the samplesheet before continuing:

```bash
# Inspect the file
cat /projects/mylab/shared/project_test/data/meta/samplesheet.csv

# Confirm every FASTQ path actually exists (prints nothing when everything is good)
while IFS=',' read -r sample fq1 fq2 strand; do
    [ "$sample" != "sample" ] && [ -n "$fq1" ] && [ ! -f "$fq1" ] && echo "MISSING: $fq1"
done < /projects/mylab/shared/project_test/data/meta/samplesheet.csv
```

---

## Step 9: Write the nextflow.config

This is the most important configuration step. Create `nextflow.config` in your scripts directory and adjust the highlighted sections to match your cluster.

```
nano /projects/mylab/shared/project_test/scripts/nextflow.config
```

```bash
// ============================================================
// nextflow.config -- nf-core/rnaseq HPC configuration
// ============================================================

// Singularity container settings
singularity {
    enabled     = true
    autoMounts  = true   // auto-mount host filesystem paths into containers
    cacheDir    = "/projects/mylab/shared/singularity_cache"
    pullTimeout = '60 min'   // raise the default 20 min timeout for slow connections
}

// SLURM executor settings and resource ceilings
process {
    executor = 'slurm'
    queue    = 'compute'    // your SLURM partition name -- check with: sinfo

    // resourceLimits caps the resources any single job may request.
    // This REPLACES the old --max_cpus / --max_memory / --max_time params.
    // Set these to the largest single-node specs your partition offers
    // (check with: sinfo -o "%P %c %m %l"). The values below are an example.
    resourceLimits = [
        cpus:   128,         // max cores available on one node
        memory: 248.GB,      // max RAM available on one node
        time:   96.h         // your partition's hard walltime limit
    ]

    maxRetries    = 3        // retry a failed job up to 3 times
    errorStrategy = { task.attempt <= 3 ? 'retry' : 'finish' }
    cache         = 'lenient'  // avoids false cache misses from timestamp changes
}

// How Nextflow itself submits jobs to the scheduler
executor {
    queueSize       = 100      // max simultaneously queued SLURM jobs
    submitRateLimit = '10 sec' // throttle submission so the scheduler is not overwhelmed
}

// Execution reports written into the scripts directory
timeline { enabled = true; file = "/projects/mylab/shared/project_test/scripts/timeline.html" }
report   { enabled = true; file = "/projects/mylab/shared/project_test/scripts/report.html"   }
trace    { enabled = true; file = "/projects/mylab/shared/project_test/scripts/trace.txt"     }
```

Find your SLURM partition name with `sinfo` — the name appears in the first column. This tutorial uses a generic partition called `compute`; common real names include `normal`, `general`, `batch`, or `standard`. Use whatever your cluster reports in the `queue` field.

---

## Step 10: Write the SLURM Submission Script

The orchestrator must stay running for the entire pipeline — potentially many hours. The most robust approach is to submit Nextflow itself as a small SLURM job whose only purpose is to manage the other jobs.

```
nano /projects/mylab/shared/project_test/scripts/run_nextflow.sh
```

```bash
#!/bin/bash
#SBATCH --job-name=rnaseq_nextflow
#SBATCH --output=/projects/mylab/shared/project_test/scripts/nextflow_%j.out
#SBATCH --error=/projects/mylab/shared/project_test/scripts/nextflow_%j.err
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=2
#SBATCH --mem=8G
#SBATCH --time=96:00:00           # matches compute hard limit and config time limit
#SBATCH --partition=compute
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=your_username@university.edu   # substitute your address

# ---- Environment setup (must match the modules loaded interactively) ----
module load java/21
module load nextflow
module load singularity

export NXF_HOME="/projects/mylab/shared/.nextflow"
export NXF_SINGULARITY_CACHEDIR="/projects/mylab/shared/singularity_cache"
export NXF_JVM_ARGS="-Xms2g -Xmx6g"   # cap the orchestrator's Java heap

# ---- Run the pipeline ----
cd /projects/mylab/shared/project_test/scripts

nextflow run nf-core/rnaseq \
    -r 3.26.0 \
    -profile singularity \
    -c /projects/mylab/shared/project_test/scripts/nextflow.config \
    --input /projects/mylab/shared/project_test/data/meta/samplesheet.csv \
    --outdir /projects/mylab/shared/project_test/results \
    --fasta /projects/mylab/shared/reference/GRCm39/Mus_musculus.GRCm39.dna.primary_assembly.fa \
    --gtf /projects/mylab/shared/reference/GRCm39/Mus_musculus.GRCm39.116.gtf \
    --aligner star_salmon \
    -work-dir /scratch/$USER/work \
    -resume \
    -ansi-log false
```

Each flag does the following:

`-r 3.26.0``-profile singularity``-c .../nextflow.config``--input .../samplesheet.csv``--outdir .../results``--fasta``--gtf``--aligner star_salmon``-work-dir /scratch/$USER/work``-resume``-ansi-log false`

| Flag | Meaning |
| --- | --- |
|  | Pin to a specific pipeline version for reproducibility |
|  | Use Singularity containers |
|  | Load your cluster configuration |
|  | Your sample information |
|  | Where final outputs go |
|  | Reference genome FASTA (mouse GRCm39 here) |
|  | Gene annotation GTF (must match the FASTA release) |
|  | STAR for alignment, Salmon for quantification |
|  | Task scratch on fast, purgeable scratch storage |
|  | Resume from cache if this is a restart |
|  | Plain-text logs (ANSI color codes corrupt SLURM log files) |

The `--mail-type` and `--mail-user` SBATCH lines are optional but convenient — SLURM will email you when the orchestrator job begins, ends, or fails, so you do not have to poll the queue manually.

---

## Step 11: Run the Real Pipeline

Submit the orchestrator job:

```bash
cd /projects/mylab/shared/project_test/scripts
sbatch run_nextflow.sh
# Submitted batch job 12013146
```

SLURM returns a job ID (here `12013146`), which appears in the log filenames (`nextflow_12013146.out` and `nextflow_12013146.err`) so you can match logs to runs. From here the orchestrator submits the individual pipeline tasks as their own SLURM jobs and manages them to completion.

Alternatively, for an interactive run where you watch live output, launch inside the `tmux` session from Step 1 instead of via `sbatch`, then detach with `Ctrl+B` then `D` and reattach later with `tmux attach -t nextflow_setup`.

---

## Step 12: Monitor, Resume, and Interpret the Output

### Monitoring a Running Pipeline

While the pipeline runs, monitor it through the log file and SLURM queue:

```bash
# Follow the Nextflow log live
tail -f /projects/mylab/shared/project_test/scripts/nextflow_12013146.out

# See which SLURM jobs are currently queued or running
squeue -u $USER

# Inspect per-task resource usage
tail -f /projects/mylab/shared/project_test/scripts/trace.txt
```

The alphanumeric codes in the progress output (e.g. `a1/4bc923`) are task hashes pointing to specific subdirectories inside the work directory. When a task fails, that directory contains everything you need to diagnose it:

```
cd /scratch/$USER/work/a1/4bc923*/

cat .command.sh    # the exact command that ran
cat .command.err   # the error output
cat .command.out   # the standard output
```

### Resuming After a Failure

If the pipeline fails partway through, do not start over. Fix the cause — a wrong partition name, insufficient memory, a missing module — and resubmit the same script. The `-resume` flag is already in it:

```
sbatch run_nextflow.sh
```

Nextflow compares each completed task’s inputs and parameters against its cache, reuses any step whose inputs are unchanged, and runs only the failed or new tasks. Completed work is skipped instantly.

### Understanding the Output Directory

When the pipeline completes, the `results/` directory is densely populated. Here is the actual structure produced by the four-sample mouse run in this tutorial, annotated:

```
results/
|-- fastqc/
|   |-- raw/          # FastQC reports on the RAW reads (per sample)
|   |-- trim/         # FastQC reports AFTER adapter trimming
|-- fq_lint/
|   |-- raw/          # FASTQ integrity checks before trimming
|   |-- trimmed/      # FASTQ integrity checks after trimming
|-- trimgalore/       # adapter/quality trimming reports (per sample, per read)
|-- star_salmon/      # the main alignment + quantification outputs (see below)
|-- multiqc/
|   |-- star_salmon/  # *** the aggregated MultiQC HTML report ***
|-- pipeline_info/
    |-- nf_core_rnaseq_software_mqc_versions.yml   # exact version of every tool used
    |-- params_2026-06-26_21-57-45.json            # every parameter the run used
    |-- pipeline_dag_2026-06-26_21-57-10.html      # the workflow dependency graph
```

The `star_salmon/` subdirectory is where the science lives:

```
star_salmon/
|-- KRAS_1.markdup.sorted.bam(.bai)        # aligned, duplicate-marked, sorted BAM + index
|-- KRAS_2.markdup.sorted.bam(.bai)        # (one BAM per sample, four samples total)
|-- KRAS_SPIB_1.markdup.sorted.bam(.bai)
|-- KRAS_SPIB_2.markdup.sorted.bam(.bai)
|-- KRAS_1/  KRAS_2/  KRAS_SPIB_1/  KRAS_SPIB_2/   # per-sample Salmon quant dirs:
|     |-- quant.sf            #   transcript-level estimates (the raw Salmon output)
|     |-- quant.genes.sf      #   gene-level estimates
|     |-- aux_info/           #   bias models, fragment-length distribution, mapping info
|     |-- libParams/, logs/, cmd_info.json   #   library type, run log, exact command
|-- salmon.merged.gene_counts.tsv          # *** merged gene count matrix (non-integer) ***
|-- salmon.merged.gene_counts_scaled.tsv   # library-size-scaled counts
|-- salmon.merged.gene_counts_length_scaled.tsv  # length + library-size scaled counts
|-- salmon.merged.gene_tpm.tsv             # gene TPM (for visualization, NOT DE testing)
|-- salmon.merged.gene_lengths.tsv         # average gene lengths used in scaling
|-- salmon.merged.gene.SummarizedExperiment.rds  # ready-to-load R object (recommended for DE)
|-- salmon.merged.transcript_*.tsv / .rds  # the same set, at transcript level
|-- salmon.merged.tx2gene.tsv              # transcript-to-gene mapping
|-- bigwig/            # stranded coverage tracks (.bigWig, .forward/.reverse) for IGV/UCSC
|-- deseq2_qc/         # PCA + sample-distance plots, size factors, saved dds.RData
|-- dupradar/          # duplication-vs-expression diagnostics (box/scatter/histogram PDFs)
|-- featurecounts/     # reads per gene biotype, plus rRNA biotype breakdown (QC)
|-- qualimap/          # per-sample HTML report: gene-body coverage, read genomic origin
|-- rseqc/             # bam_stat, infer_experiment (strandedness), read_distribution, etc.
|-- picard_metrics/    # MarkDuplicates duplicate metrics per sample
|-- samtools_stats/    # flagstat / idxstats / stats for sorted and markdup BAMs
|-- stringtie/         # reference-guided transcript assembly (.gtf, gene abundance, ballgown)
|-- log/               # STAR per-sample logs (Log.final.out = the mapping-rate summary)
```

A few of these are worth knowing by name. Inside each per-sample Salmon directory, **`quant.sf`** holds the transcript-level estimates and **`quant.genes.sf`** the gene-level estimates — these are the raw per-sample outputs that get merged into the `salmon.merged.*` matrices at the top level. In `rseqc/`, **`infer_experiment.txt`** is where Salmon’s auto-detected strandedness is independently confirmed from the alignments — worth checking if anything downstream looks off. In `log/`, each **`Log.final.out`** is the STAR mapping-rate summary for that sample (uniquely mapped %, multi-mapping %, unmapped %), which is the first number to inspect when a sample looks suspicious. And `stringtie/` provides reference-guided transcript assemblies (including ballgown tables) for anyone wanting to look at novel isoforms beyond the reference annotation.

### Which files you will actually use

Three outputs matter most for a typical analysis:

**`star_salmon/salmon.merged.gene_counts.tsv`** is your gene count matrix — rows are genes, columns are the four samples (`KRAS_1`, `KRAS_2`, `KRAS_SPIB_1`, `KRAS_SPIB_2`). One thing surprises people the first time they open it: **the counts are not whole numbers.** Salmon does not count reads by integer assignment the way featureCounts or HTSeq do; it estimates expression probabilistically and apportions multi-mapping reads fractionally across the transcripts they could have come from. Those fractional transcript estimates are then summed to the gene level, so a gene’s count comes out as something like `1843.27` rather than `1843`. This is expected and correct — it is not a bug or a rounding error. The same is true of the scaled variants (`gene_counts_scaled.tsv` and `gene_counts_length_scaled.tsv`): scaling changes the magnitude of the values, not their non-integer nature. None of the Salmon count files contain integers. For a detailed walkthrough of differential expression analysis using these count matrices, see our previous tutorial, [Comparing limma, DESeq2, and edgeR in Differential Expression Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-20-comparing-limma-deseq2-and-edger-in-differential-expression-analysis/).

**`multiqc/star_salmon/multiqc_report.html`** aggregates every QC metric — FastQC, trimming, STAR mapping rates, Salmon strandedness, duplication, gene-body coverage — into one interactive HTML report. Open it in a browser and review it before any downstream analysis. This is where you catch a failed sample, a strandedness mismatch, or contamination before it costs you a day of confused differential-expression results.

**`star_salmon/deseq2_qc/`** contains a PCA plot and a sample-to-sample distance heatmap (`deseq2.plots.pdf`), the underlying values (`deseq2.pca.vals.txt`, `deseq2.sample.dists.txt`), the size factors used for normalization, and a saved `deseq2.dds.RData` object — all generated automatically by the pipeline. For this KRAS vs KRAS+SPIB design, this is the fastest first look at whether the two conditions separate and whether replicates cluster together — a sanity check before you run your own DE analysis.

### Choosing the right count file for differential expression

The multiple `salmon.merged.gene_*` files exist for different purposes, and mixing them up is one of the most common mistakes in downstream analysis. The table below summarizes them; the rest of this section explains how to actually use each one.

`gene.SummarizedExperiment.rds`**Recommended**`gene_counts.tsv``gene_lengths.tsv``gene_counts_length_scaled.tsv``.rds``gene_counts_scaled.tsv``gene_tpm.tsv`

| File | What it contains | Use it for |
| --- | --- | --- |
|  | Estimated counts + gene lengths + TPM in one R object | — load into DESeq2 via tximport offsets |
|  | Raw estimated counts, length-uncorrected (non-integer) | Only with  to build an offset; not alone |
|  | Counts corrected for transcript length, scaled to library size (non-integer) | A plain count matrix when you cannot use the  route |
|  | Counts scaled to library size only (non-integer) | Specialized workflows expecting scaledTPM input |
|  | Transcripts Per Million | Visualization and cross-gene comparison only — never DE testing |

### Do the DE tools not require integer counts?

This is the question that confuses most newcomers, because the usual advice (“DE tools need integer counts”) is true only in a narrow sense. The integer requirement comes from the *statistical model*, and it applies to specific entry functions, not to DE analysis in general:

- **DESeq2** models counts with a negative binomial distribution, which is defined only on integers — but only its `DESeqDataSetFromMatrix()` function enforces this and throws the `"some values in assay are not integers"` error. Its `DESeqDataSetFromTximport()` function does **not** require integers: it is built specifically to accept Salmon’s fractional estimates together with the gene-length offsets.

- **edgeR** is the strict one: its classic `DGEList()` path expects integer-like counts and will round or warn otherwise.

- **limma-voom** does **not** require integers at all — it models the log of normalized counts with linear models, so fractional counts are fine as-is.

So “DE tools require integers” really means “the negative-binomial matrix-entry functions of DESeq2 and edgeR expect integers.” That leaves several valid non-integer routes, in order of preference.

### Route 1 (recommended): the SummarizedExperiment / tximport route

This is the route nf-core intends, and it sidesteps the integer question entirely because DESeq2 ingests the estimated counts and the per-gene length offsets together. Load the `.rds` object and build the DESeq2 dataset from it:

```r
library(SummarizedExperiment)
library(DESeq2)

# Load the ready-made object the pipeline produced
se <- readRDS("salmon.merged.gene.SummarizedExperiment.rds")

# Attach your experimental design (column order matches the assay columns)
se$condition <- factor(c("KRAS_SPIB", "KRAS_SPIB", "KRAS", "KRAS"))

# Build the DESeq2 dataset directly from the SummarizedExperiment.
# The length information travels with the object and is used as an offset,
# so the non-integer estimated counts are handled correctly -- no rounding.
dds <- DESeqDataSet(se, design = ~ condition)
dds <- DESeq(dds)
res <- results(dds, contrast = c("condition", "KRAS_SPIB", "KRAS"))
```

### Route 2 (convenient alternative): the length-scaled matrix

If you would rather work from a flat table than an R object, use `gene_counts_length_scaled.tsv`. The key property of this file is that tximport’s `lengthScaledTPM` scaling makes each sample’s column sum back to that sample’s original library size, so the values live on a genuine count scale (millions per sample) — unlike TPM, where every column sums to exactly 1,000,000 and the library-size information needed by DESeq2 is destroyed. Crucially, the transcript-length correction is already baked into these values *before* you touch them.

For **limma-voom**, you can use the matrix directly with no rounding:

```r
library(edgeR); library(limma)
counts <- read.delim("salmon.merged.gene_counts_length_scaled.tsv",
                     row.names = 1, check.names = FALSE)
counts <- counts[, -1]                       # drop the gene_name column if present
dge <- DGEList(counts = as.matrix(counts))   # fractional values are fine for voom
dge <- calcNormFactors(dge)
v   <- voom(dge, model.matrix(~ condition))
```

For **DESeq2** via `DESeqDataSetFromMatrix()`, “use it directly” really means “load it and round it” — the model needs integers:

```r
library(DESeq2)
counts <- read.delim("salmon.merged.gene_counts_length_scaled.tsv",
                     row.names = 1, check.names = FALSE)
counts <- round(as.matrix(counts[, -1]))     # round AFTER length scaling
coldata <- data.frame(row.names = colnames(counts),
                      condition = c("KRAS_SPIB", "KRAS_SPIB", "KRAS", "KRAS"))
dds <- DESeqDataSetFromMatrix(counts, coldata, design = ~ condition)
```

Rounding here is acceptable because the length correction has already been applied; the leftover rounding error is a fraction of one read per gene. This is why rounding the **length-scaled** file is fine, but rounding the plain `gene_counts.tsv` is not — the latter would round away nothing useful and still carry no length correction.

### Route 3 (discouraged): the plain count matrix alone

Using `gene_counts.tsv` by itself is the route the nf-core maintainers explicitly advise against, because it carries no length correction and therefore does not account for differential isoform usage between conditions. If you do use it, pair it with `gene_lengths.tsv` to construct the offset — which is exactly what the official `nf-core/differentialabundance` pipeline does internally. The one situation where the plain counts are appropriate on their own is 3′-tagged RNA-seq, where there is no transcript-length bias to correct. For standard full-length mRNA-seq, prefer Route 1 or Route 2.

### A note on the `featurecounts/` folder

It is tempting to reach into `star_salmon/featurecounts/` expecting true integer gene counts, since featureCounts is a classic integer counter. **Do not use it as your DE input.** Since v3.0 of the pipeline, featureCounts is no longer used for quantification — it is run only to generate biotype QC. The pipeline invokes it grouped by the `gene_biotype` attribute, so each `*.featureCounts.tsv` is a per-sample table of read counts per biotype class (protein_coding, lncRNA, rRNA, and so on) — a few dozen rows — not a gene-by-sample matrix of every gene. Those files, together with the `*.biotype_counts_mqc.tsv` files, feed the “where are my reads landing” biotype plot in MultiQC and the rRNA-contamination check; they are QC, not expression quantification. If you genuinely need integer per-gene counts, run featureCounts yourself with `-g gene_id` on the pipeline’s output BAMs (`star_salmon/*.markdup.sorted.bam`) — but for standard DE, the Salmon routes above are the recommended and more accurate choice.

The single most important rule across all of this: **never run differential expression on the TPM file.** TPM is normalized for gene length and sequencing depth, which is what makes it readable for comparing genes within a sample — and exactly what makes it invalid for DESeq2 or edgeR, which need library-size information preserved so they can model and normalize it themselves.

### Execution reports

The `scripts/` directory also holds three run-level reports generated by the config: `report.html` (resource usage and run summary), `timeline.html` (a visual timeline of when each task ran and for how long), and `trace.txt` (a per-task table of CPU, memory, and runtime). These are invaluable for tuning — if a particular process is consistently slow or memory-hungry, the trace file tells you which one and by how much.

### Quality Metrics to Check in MultiQC

| Metric | What to look for | Warning threshold |
| --- | --- | --- |
| % Reads mapped (STAR) | Fraction aligning to the genome | < 70% warrants investigation |
| % Duplicate reads | Fraction of PCR duplicates | > 50% suggests over-amplification |
| % rRNA reads | Fraction mapping to rRNA genes | > 10% suggests poor rRNA depletion |
| Inferred strandedness | Salmon’s auto call vs your expectation | Mismatch suggests a protocol or samplesheet error |
| Gene-body coverage | Even coverage 5′ to 3′ | Strong 3′ bias suggests RNA degradation |

---

## Step 13: Clean Up the Work Directory

The work directory holds intermediate files for every task — input symlinks, outputs, logs, container binds. For even a modest run it grows large, and on scratch it will eventually be purged anyway. Once the run has completed successfully and you have confirmed the outputs in `results/`, delete it explicitly:

```bash
# Confirm the run finished before deleting anything
grep "Pipeline completed" /projects/mylab/shared/project_test/scripts/nextflow_12013146.out

# Delete the work directory; keep results/ -- that is your permanent output
rm -rf /scratch/$USER/work
```

> 

**Do not delete the work directory prematurely.** Removing it while the pipeline is still running, or before using `-resume` to recover a partial run, forces a full restart from scratch.

---

## Useful Optional Parameters

Once the basic run is comfortable, these parameters are worth knowing:

```
--remove_ribo_rna                 # remove rRNA reads with SortMeRNA
--save_align_intermeds            # save intermediate BAMs (needed for reprocessing)
--pseudo_aligner salmon --skip_alignment   # fast Salmon-only quantification
--stranded_threshold 0.8          # tune auto strandedness detection
--unstranded_threshold 0.1
--save_reference                  # save the generated STAR index to results/genome/
--star_index /path/to/star_index/ # reuse a pre-built STAR index (saves 30-60 min)
--gencode                         # use this flag if your GTF is from GENCODE, not Ensembl
```

By default the STAR index is built inside the `work/` directory, used for alignment, and then lost when you clean up `work/` (Step 13) — so the next run rebuilds it from scratch, costing 30–60 minutes for a mammalian genome. The `--save_reference` flag is the other half of the `--star_index` pair: it copies the generated index into your output directory at `results/genome/index/star/`, so you can keep it. The save-once-then-reuse workflow is: run once with `--save_reference`; move the resulting `star/` folder into your persistent reference area (e.g. `/projects/<your_group>/shared/reference/GRCm39/star/`); and on every later run, skip the rebuild by passing `--star_index /projects/<your_group>/shared/reference/GRCm39/star/`. Note that a saved index is tied to the exact FASTA + GTF combination used to build it, so you need a fresh build whenever you change species or Ensembl release.

For reprocessing without re-aligning, nf-core/rnaseq supports a two-step workflow. Run first with `--save_align_intermeds`, which saves the BAMs and generates a `samplesheet_with_bams.csv`. Then rerun with `--input samplesheets/samplesheet_with_bams.csv --skip_alignment` to quantify from the existing BAMs in minutes rather than hours — ideal for tweaking downstream settings without repeating the expensive alignment step.

---

## Best Practices and Common Pitfalls

### Best Practices

**1. Always pin the pipeline version with `-r 3.26.0`.** This guarantees that the analysis is exactly reproducible months later, even after the pipeline has been updated. An unpinned run silently pulls whatever version is newest.

**2. Run the test profile immediately after installing the pipeline.** Validating the environment on the tiny test dataset (Step 6) catches configuration errors in minutes — before you invest time in references, samplesheets, and a long real run.

**3. Use `auto` strandedness unless you are certain of the protocol.** Incorrect strandedness silently destroys quantification accuracy — it does not error, it just produces wrong counts. Let Salmon infer it and confirm in the MultiQC report.

**4. Put persistent data on project storage and transient data on scratch.** The container cache, pipeline code, and references belong on non-purged project storage; the `work/` directory belongs on fast scratch (`/scratch`). Getting this split right avoids both quota errors and silent data loss.

**5. Use absolute paths everywhere in the samplesheet.** Each task runs in an isolated work subdirectory where relative paths cannot resolve. This is the single most common beginner error.

**6. Keep the genome and GTF from the same Ensembl release.** Mismatched versions cause chromosome-name errors and missing genes that are hard to diagnose after the fact — here, release 116 for both.

**7. Submit the orchestrator as its own SLURM job.** It can run for many hours; relying on an open SSH session means a dropped connection kills the entire pipeline. A `tmux` session is the interactive alternative.

**8. Examine the MultiQC report before any downstream analysis.** Catching a sample swap, contamination, or protocol mismatch here saves far more time than discovering it after differential expression analysis.

---

## Key takeaways:

- Nextflow is an orchestrator, not a tool: it coordinates SLURM submission and parallelization but never runs STAR or Salmon itself

- Configuration is strictly separate from pipeline code — adapt your `nextflow.config`, never the pipeline files

- Persistent data (cache, code, references) belongs on project storage; the transient work directory belongs on scratch

- The `-resume` flag makes failures cheap: completed steps are skipped, and only failed or new tasks run

- The Salmon counts are non-integer estimates: prefer the `.rds` SummarizedExperiment (tximport handles the non-integers and length offsets), or the length-scaled matrix as an alternative — never the plain counts alone, and never the TPM file

---

## References

1. Di Tommaso P, Chatzou M, Floden EW, Barja PP, Palumbo E, Notredame C. Nextflow enables reproducible computational workflows. *Nature Biotechnology*. 2017;35(4):316-319. doi:10.1038/nbt.3820

2. Ewels PA, Peltzer A, Fillinger S, et al. The nf-core framework for community-curated bioinformatics pipelines. *Nature Biotechnology*. 2020;38(3):276-278. doi:10.1038/s41587-020-0439-x

3. Dobin A, Davis CA, Schlesinger F, et al. STAR: ultrafast universal RNA-seq aligner. *Bioinformatics*. 2013;29(1):15-21. doi:10.1093/bioinformatics/bts635

4. Patro R, Duggal G, Love MI, Irizarry RA, Kingsford C. Salmon provides fast and bias-aware quantification of transcript expression. *Nature Methods*. 2017;14(4):417-419. doi:10.1038/nmeth.4197

5. Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology*. 2014;15(12):550. doi:10.1186/s13059-014-0550-8

6. Soneson C, Love MI, Robinson MD. Differential analyses for RNA-seq: transcript-level estimates improve gene-level inferences. *F1000Research*. 2015;4:1521. doi:10.12688/f1000research.7563.2

7. Ewels P, Magnusson M, Lundin S, Kaller M. MultiQC: summarize analysis results for multiple tools and samples in a single report. *Bioinformatics*. 2016;32(19):3047-3048. doi:10.1093/bioinformatics/btw354

8. Kurtzer GM, Sochat V, Bauer MW. Singularity: scientific containers for mobility of compute. *PLoS ONE*. 2017;12(5):e0177459. doi:10.1371/journal.pone.0177459

9. nf-core/rnaseq v3.26.0 documentation. [https://nf-co.re/rnaseq/3.26.0](https://nf-co.re/rnaseq/3.26.0) (2026)

10. Nextflow documentation. [https://nextflow.io/docs/latest/](https://nextflow.io/docs/latest/) (2026)

---

*This tutorial is part of the NGS101.com Scientific Programming series for beginners.*
