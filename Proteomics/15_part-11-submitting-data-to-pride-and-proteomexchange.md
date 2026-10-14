# How to Analyze Proteomics Data Part 11: Depositing and Reanalyzing Data in PRIDE & ProteomeXchange

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Open Science, PRIDE, ProteomeXchange, Data Deposition, rpx

---

## Table of Contents

- [Introduction](#introduction)
- [The ProteomeXchange (PX) Ecosystem](#the-proteomexchange-px-ecosystem)
  - [What is PRIDE?](#what-is-pride)
  - [Complete vs. Partial Submissions](#complete-vs-partial-submissions)
- [Step 1: Preparing the Required Submission Files](#step-1-preparing-the-required-submission-files)
  - [The Four Essential File Types](#the-four-essential-file-types)
  - [Ontology Standards (PSI-MS & Unimod)](#ontology-standards-psi-ms--unimod)
- [Step 2: Using the ProteomeXchange (PX) Submission Tool](#step-2-using-the-proteomexchange-px-submission-tool)
  - [Step-by-Step Submission Wizard](#step-by-step-submission-wizard)
  - [High-Speed Aspera Upload](#high-speed-aspera-upload)
- [Step 3: Managing Peer Review & Public Release](#step-3-managing-peer-review--public-release)
  - [Securing a PXD Accession](#securing-a-pxd-accession)
  - [Generating Private Reviewer Credentials](#generating-private-reviewer-credentials)
- [Step 4: Programmatic Re-analysis of Public PRIDE Datasets with rpx](#step-4-programmatic-re-analysis-of-public-pride-datasets-with-rpx)
  - [Querying PRIDE from R](#querying-pride-from-r)
  - [Downloading Raw Files and Reports Programmatically](#downloading-raw-files-and-reports-programmatically)
- [Series Conclusion: The Modern Proteomics Mindset](#series-conclusion-the-modern-proteomics-mindset)

---

## Introduction

In our [Bulk RNA-seq series](../BulkRNAseq/29_part-12-submitting-data-to-ncbi-geo.md), we deposited raw sequencing reads and count tables into NCBI GEO.

In mass spectrometry, the counterpart to GEO is the **PRIDE (PRoteomics IDEntifications database)** at the European Bioinformatics Institute (EMBL-EBI), operating under the international **ProteomeXchange (PX)** consortium.

Nearly all peer-reviewed journals (*Nature*, *Cell*, *Molecular & Cellular Proteomics*, *PNAS*) mandate that:
> **All raw mass spectrometry files, reference databases, and identification output tables must be deposited in a ProteomeXchange repository before manuscript publication.**

In this final chapter, we complete our journey by mastering data deposition:
1. Understanding the difference between **Complete** and **Partial** submissions.
2. Preparing files and ontologies for the **PX Submission Tool**.
3. Obtaining a permanent **PXD accession** and setting up private reviewer access.
4. Programmatically querying and downloading public PRIDE datasets directly in R using **`rpx`**.

---

## The ProteomeXchange (PX) Ecosystem

### What is PRIDE?
Founded in 2004 at EMBL-EBI, PRIDE is the world’s largest public repository for mass spectrometry-based proteomics. Together with **MassIVE** (UC San Diego), **jPOST** (Japan), and **iProX** (China), it forms the **ProteomeXchange Consortium**.

Every submitted dataset receives a unique identifier formatted as:
$$\mathbf{PXD}\#\#\#\#\#\# \quad (\text{e.g., PXD014777})$$

```
                            PROTEOMEXCHANGE (PX)
                                     │
       ┌─────────────────────────────┼─────────────────────────────┐
       ▼                             ▼                             ▼
 [ EMBL-EBI: PRIDE ]          [ UCSD: MassIVE ]            [ JAPAN: jPOST ]
 (Primary Global Hub)        (Interactive Re-analysis)     (Asian Data Hub)
```

### Complete vs. Partial Submissions

When depositing data, ProteomeXchange distinguishes between two categories:

| Feature | Complete Submission | Partial Submission |
| :--- | :--- | :--- |
| **Search Engine Files** | Must be in an officially supported format (e.g., `mzIdentML`, PRIDE XML, MaxQuant) | Uses custom, non-standard, or proprietary search outputs |
| **PRIDE Inspector** | Viewable directly in the web browser (spectra matched to peptide sequences) | Cannot be visually inspected online; files downloaded as raw archives |
| **Re-use Value** | Maximum transparency and FAIR compliance | High (raw data accessible, but manual re-processing required) |

---

## Step 1: Preparing the Required Submission Files

### The Four Essential File Types

Before launching the submission tool, gather your files into a dedicated folder:

1. **RAW Files (`.raw`, `.d`, `.wiff`)**: The unprocessed binary data from the mass spectrometer.
2. **SEARCH Files**: The output of your search engine (e.g., DIA-NN `report.tsv`, or MaxQuant `evidence.txt` and `proteinGroups.txt`).
3. **PEAK Files (`.mzML`, `.mgf`)**: Centroided peak lists used during database searching.
4. **FASTA Database**: The exact sequence database used, including the appended **cRAP** contaminant database (`human_with_crap.fasta`).

---

### Ontology Standards (PSI-MS & Unimod)

To ensure machine readability, metadata must adhere to standardized controlled vocabularies:

- **Species Ontology (NEWT / NCBI Taxon)**: `Homo sapiens (9606)`.
- **Tissue Ontology (BTO / BRENDA)**: `Lung carcinoma (BTO:0000763)`.
- **Instrument Ontology (PSI-MS)**: `Q Exactive HF-X (MS:1002877)`.
- **PTM Ontology (Unimod)**:
  - Fixed modification: `Carbamidomethyl of C (UNIMOD:4)`.
  - Variable modification: `Oxidation of M (UNIMOD:35)`.
  - Variable modification: `Phosphorylation of S/T/Y (UNIMOD:21)`.

---

## Step 2: Using the ProteomeXchange (PX) Submission Tool

The ProteomeXchange submission tool is a cross-platform Java desktop application provided by EMBL-EBI.

```bash
# 1. Download the PX Submission Tool from EMBL-EBI
curl -O http://ftp.pride.ebi.ac.uk/pride/resources/tools/submission-tool/latest/desktop/px-submission-tool.jar

# 2. Launch the tool (requires Java 11 or higher)
java -jar px-submission-tool.jar
```

---

### Step-by-Step Submission Wizard

```
[ Step 1: Login ] ──► [ Step 2: Metadata ] ──► [ Step 3: Files ] ──► [ Step 4: Upload ]
  PRIDE Account         Title, Abstract,         Assign RAW,          High-speed
  Credentials           Keywords, Ontologies     SEARCH, FASTA        Aspera / FTP
```

1. **Step 1: Credentials**: Create a free account at [ebi.ac.uk/pride](https://www.ebi.ac.uk/pride) and log in.
2. **Step 2: Project Metadata**:
   * **Title**: e.g., *"Targeted Kinase Inhibition in A549 Lung Carcinoma (DIA-NN Proteomics)"*.
   * **Abstract**: Paste your manuscript abstract.
   * **Keywords**: `A549`, `DIA-NN`, `Kinase Inhibitor`, `Phosphoproteomics`.
   * **Experiment Type**: Select `Data-Independent Acquisition (DIA)`.
3. **Step 3: File Assignment**:
   * Drag and drop your project directory.
   * Assign file types: mark `.raw` files as **RAW**, `report.tsv` as **SEARCH**, and `human_with_crap.fasta` as **FASTA**.
4. **Step 4: Sample-to-File Mapping**:
   * Link each biological sample (`Control_Rep1`, `Treated_Rep1`) to its corresponding `.raw` file.

---

### High-Speed Aspera Upload

Uploading 20–50 GB of mass spectrometry data via standard browser HTTP often fails due to network timeouts. The PX Submission Tool integrates **IBM Aspera**:
- Uses UDP protocol to saturate your available bandwidth.
- Transfers 10 GB in under 5 minutes on standard academic fiber networks.
- Supports automatic resume if your connection drops.

---

## Step 3: Managing Peer Review & Public Release

### Securing a PXD Accession

Once the upload completes, the PRIDE team automatically generates your official accession number:
$$\mathbf{PXD014777}$$
You cite this accession in the **"Data Availability"** section of your manuscript:

> *"The mass spectrometry proteomics data have been deposited to the ProteomeXchange Consortium via the PRIDE partner repository with the dataset identifier PXD014777."*

### Generating Private Reviewer Credentials

Your dataset remains **private and confidential** during peer review. 

The PX submission dashboard provides a dedicated reviewer username and password (e.g., `reviewer_pxd014777`). Peer reviewers can inspect your raw spectra without making the data public to the world.

Once your manuscript is officially accepted for publication, simply click **"Make Public"** in the PRIDE web dashboard.

---

## Step 4: Programmatic Re-analysis of Public PRIDE Datasets with rpx

The Bioconductor package **`rpx`** (Laurent Gatto) allows bioinformaticians to query, inspect, and download public PRIDE datasets directly inside R scripts.

### Querying PRIDE from R

```r
library(rpx)

# Connect to our tutorial dataset on PRIDE
px <- PXDataset("PXD014777")

# Inspect dataset metadata
px
```

```text
Project PXD014777 with 12 files
 Title: Deep DIA-NN profiling of A549 lung cancer cells
 Instrument: Q Exactive HF-X
 Species: Homo sapiens
 Tag: ProteomeXchange
```

---

### Downloading Raw Files and Reports Programmatically

You can list all files associated with the project and download specific quantification matrices without opening a web browser:

```r
# List all files in the repository
files <- pxfiles(px)
print(head(files, 5))

# Download only the DIA-NN report table
downloaded_file <- pxget(px, "report.tsv")
cat("Downloaded file saved to:", downloaded_file, "\n")
```

This functionality makes your downstream analyses **100% reproducible**: another researcher can clone your R script, and it will automatically pull the source mass spectrometry data directly from PRIDE!

---

## Series Conclusion: The Modern Proteomics Mindset

Congratulations! You have completed the **Proteomics Data Analysis Tutorial Series**.

Across this 11-part curriculum, you mastered:
1. **The Proteomics Landscape**: Understanding why proteins differ from mRNA, and how Mass Spectrometry, Olink, and SomaScan operate.
2. **Discovery Mass Spectrometry**: Running **DIA-NN**, controlling 1% FDR, and understanding chromatographic MS1/MS2 spectra.
3. **Data QC & Missing Values**: Managing the **QFeatures** container, filtering cRAP contaminants, and applying left-censored Gaussian downshift imputation for limit-of-detection dropouts.
4. **Differential Abundance Testing**: Applying empirical Bayes linear models with **`limma`** and peptide-level mixed models with **`MSstats`**.
5. **Targeted Affinity Proteomics**: Analyzing **Olink PEA (NPX)** and **SomaScan SOMAmers (RFU)**, handling Limits of Detection (LOD), and adjusting for clinical confounders.
6. **Publication Visualizations**: Designing high-resolution PCA, enhanced Volcano plots, and multi-track **ComplexHeatmap** figures.
7. **Systems Biology**: Performing custom-universe pathway enrichment with **`clusterProfiler`** and physical complex reconstruction with **`STRINGdb`**.
8. **Network Biology & pQTLs**: Building **WGCNA** co-abundance networks and applying **Mendelian Randomization** for causal drug target validation.
9. **Phosphoproteomics & Multi-Omics**: Mapping phosphorylation sites, computing **KSEA** kinase activities, and integrating RNA-seq with proteomics in **Proteogenomics**.
10. **Open Science**: Depositing and retrieving public datasets in **PRIDE** and **ProteomeXchange**.

You now possess the complete computational and statistical toolkit to analyze any proteomics dataset—from bench experiments to clinical biobanks. Happy analyzing!
