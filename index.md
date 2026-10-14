---
title: "Learning Omics"
subtitle: "A Hands-On, Practical Curriculum for High-Throughput Computational Biology, Single-Cell Genomics & Proteomics"
page-layout: full
---

::: {.hero-banner style="background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%); color: #f8fafc; padding: 3rem 2rem; border-radius: 16px; margin-bottom: 2.5rem; border: 1px solid #334155;"}
# 🧬 Learning Omics Curriculum
### From Raw Sequencing Reads to Single-Cell Dynamics & High-Dimensional Plasma Proteomics

Welcome to the **Learning Omics** curriculum—an open-source, hands-on educational platform for computational biologists, bioinformaticians, and translational researchers. Whether you are analyzing bulk RNA sequencing, dissecting cellular heterogeneity with single-cell transcriptomics, profiling chromatin accessibility with scATAC-seq, or navigating 5,400-plex plasma proteomics, this course provides step-by-step code, real-world clinical datasets, and interactive virtual laboratories.

[Explore Proteomics Track](Proteomics/README.md){.btn .btn-primary .btn-lg role="button"}
[Open Interactive Lab](interactive/olink_qc_lab.md){.btn .btn-outline-light .btn-lg role="button"}
[View Reading Lists](Proteomics/RECOMMENDED_READING.md){.btn .btn-outline-secondary .btn-lg role="button"}
:::

---

## 🗺️ Curriculum Architecture

The curriculum is structured into **four interconnected pillars**, guiding you from fundamental biological data structures up to translational multi-omics and causal human genetics:

```
                                [ THE OMICS LANDSCAPE ]
                                           │
         ┌─────────────────────────────────┼─────────────────────────────────┐
         ▼                                 ▼                                 ▼
[ 1. TRANSCRIPTOMICS ]           [ 2. EPIGENOMICS ]              [ 3. PROTEOMICS ]
 • Bulk RNA-seq Counts to DGE     • scATAC-seq TF-IDF Clustering  • Discovery LC-MS/MS (DIA-NN)
 • Normalization (TPM, DESeq2)    • TSS Enrichment & Peak Calling • Olink PEA (Explore HT ~5.4k)
 • Single-Cell RNA-seq (Seurat)   • Motif & Footprint Analysis    • SomaScan Aptamer ANML (.adat)
 • Pseudobulk & Meta-cells        • Hi-C Chromatin Conformation   • Limit of Detection (LOD) Rules
         │                                 │                                 │
         └─────────────────────────────────┼─────────────────────────────────┘
                                           ▼
                       [ 4. DOWNSTREAM & TRANSLATIONAL ]
                         • Cross-Platform Concordance (MS vs. Affinity)
                         • Pathway Enrichment & Curated Gene Sets (GSEA / ORA)
                         • Plasma pQTL Mapping & Mendelian Randomization
                         • Interactive Virtual Labs & Cloud Notebooks
```

---

## 📚 Core Modules & Learning Tracks

::: {.grid}

::: {.g-col-12 .g-col-md-6}
::: {.card .h-100 .shadow-sm}
::: {.card-body}
### 📊 Track 1: Transcriptomics (Bulk & Single-Cell)
From bulk RNA-seq count matrices to single-cell droplet deconvolution.
* **Bulk RNA-seq**: Raw FASTQ preprocessing with Salmon/STAR, count normalization (TPM, DESeq2, edgeR), batch effect correction, and multi-factor linear modeling.
* **Single-Cell RNA-seq**: 10x Genomics Cell Ranger, QC filtering (doublets, mitochondrial percentage), TF-IDF / SCTransform normalization, clustering resolution optimization, pseudobulk aggregation, and cellular niche modeling.
* **Curated Guide**: [Bulk RNA-seq Reading List](BulkRNAseq/RECOMMENDED_READING.md) | [scRNA-seq Reading List](scRNAseq/RECOMMENDED_READING.md)
:::
:::
:::

::: {.g-col-12 .g-col-md-6}
::: {.card .h-100 .shadow-sm}
::: {.card-body}
### 🔬 Track 2: Epigenomics & Chromatin Architecture
Uncovering the regulatory grammar of the non-coding genome.
* **Single-Cell ATAC-seq**: Fragment file processing, TSS enrichment calculation, TF-IDF dimensionality reduction, peak-to-gene linkage, and transcription factor motif discovery.
* **3D Genome & Methylation**: Hi-C topologically associating domains (TADs), loop calling, and bisulfite sequencing (WGBS) differential methylation analysis.
* **Curated Guide**: [scATAC-seq Reading List](scATACseq/RECOMMENDED_READING.md)
:::
:::
:::

::: {.g-col-12 .g-col-md-6}
::: {.card .h-100 .shadow-sm}
::: {.card-body}
### 🧪 Track 3: High-Throughput Proteomics
Continuous relative quantification, mass spec deconvolution, and affinity arrays.
* **Discovery Mass Spectrometry**: DIA-NN library-free searching, MaxLFQ peptide rollup, and missing value strategies (MNAR vs. MAR).
* **Targeted Affinity Proteomics**: Olink Proximity Extension Assay (PEA) NPX scale, SomaScan `.adat` ANML normalization, and strict Limit of Detection (LOD) rules.
* **Real-World Clinical Trial**: Paired pre/post tofacitinib therapy in Down Syndrome from **PRIDE PAD000062** (~5,400 proteins on Olink Explore HT).
* **Curated Guide**: [Proteomics 15-Part Curriculum](Proteomics/README.md) | [Proteomics Reading List](Proteomics/RECOMMENDED_READING.md)
:::
:::
:::

::: {.g-col-12 .g-col-md-6}
::: {.card .h-100 .shadow-sm}
::: {.card-body}
### 💻 Track 4: Interactive Virtual Labs & Notebooks
Run real clinical analyses directly in your browser or through executable notebooks.
* **Virtual Olink QC Lab**: Live sliders, internal control scatterplots, plate bridge normalization toggles, and patient trajectory inspectors.
* **Executable Workbooks**: Download and run our end-to-end R Markdown (`.Rmd`) and Jupyter notebook (`.ipynb`) workflows with zero configuration.
* **Launch**: [Open Interactive Lab](interactive/olink_qc_lab.md) | [Hands-On Explore HT Workbook](Proteomics/05b_olink_plasma_paired_qc_and_analysis_notebook.md)
:::
:::
:::

:::

---

## 🎯 Recommended Reading & Literature Compendiums

Each module in this curriculum is backed by a structured reading list integrating landmark methodology papers, textbooks, and translational anthologies:
* **Transcriptomics**: [Bulk RNA-seq Recommended Reading](BulkRNAseq/RECOMMENDED_READING.md) (Love et al., Robinson et al., Soneson et al.)
* **Single-Cell**: [scRNA-seq Recommended Reading](scRNAseq/RECOMMENDED_READING.md) (Luecken & Theis, Hafemeister & Satija, Stuart et al.)
* **Chromatin**: [scATAC-seq Recommended Reading](scATACseq/RECOMMENDED_READING.md) (Satpathy et al., Granja et al., Buenrostro et al.)
* **Proteomics & Drug Discovery**: [Proteomics Recommended Reading](Proteomics/RECOMMENDED_READING.md), featuring:
  * **DIA-NN, MaxLFQ & MSstats** foundational papers.
  * **Dr. Skanda Rajasundaram's** *Genetics in Drug Development: A Curated Anthology* (integrating human genetics, pQTLs, PheWAS, and causal target validation).
  * **Stephen Burgess & the Cambridge MRC Biostatistics Unit** guides on Mendelian Randomization.

---

## 🚀 Getting Started

1. **Clone or Fork the Repository**:
   ```bash
   git clone https://github.com/chrisraynerr/learning_omics.git
   cd learning_omics
   ```
2. **Explore the Curriculum & Workbooks**:
   * Visit the online interactive guide at [Hands-On Explore HT Workbook](Proteomics/05b_olink_plasma_paired_qc_and_analysis_notebook.md) or download the executable `.Rmd` / `.ipynb` notebooks.
   * Or open `interactive/olink_explore_interactive_qc_analysis.html` in any web browser for instant, offline visual exploration.
3. **Contribute & Feedback**:
   * All chapters are written in GitHub Flavored Markdown and R Markdown. Contributions, bug reports, and dataset suggestions are welcomed via GitHub Pull Requests.

