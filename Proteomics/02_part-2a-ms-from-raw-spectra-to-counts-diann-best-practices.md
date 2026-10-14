# How to Analyze Proteomics Data Part 2a: From Raw Spectra to Protein Quantification Matrix with DIA-NN

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Mass Spectrometry, DIA-NN, Upstream Processing

---

## Table of Contents

- [Introduction](#introduction)
- [How LC-MS/MS Generates Data: A 2-Minute Review](#how-lc-msms-generates-data-a-2-minute-review)
  - [The Three Dimensions of Mass Spectrometry](#the-three-dimensions-of-mass-spectrometry)
  - [DDA vs. DIA: The Paradigm Shift](#dda-vs-dia-the-paradigm-shift)
- [Step 1: Download the Tutorial Dataset](#step-1-download-the-tutorial-dataset)
  - [Dataset Overview: A549 Perturbation Cohort](#dataset-overview-a549-perturbation-cohort)
  - [Download via Command Line](#download-via-command-line)
  - [Laptop Fast-Track (Skip the Compute)](#laptop-fast-track-skip-the-compute)
- [Step 2: Preparing Reference Databases](#step-2-preparing-reference-databases)
  - [Download UniProt Human Reference Proteome](#download-uniprot-human-reference-proteome)
  - [Append the cRAP Contaminant Database](#append-the-crap-contaminant-database)
- [Step 3: Raw File Handling & Conversion](#step-3-raw-file-handling--conversion)
  - [Profile vs. Centroid Mode](#profile-vs-centroid-mode)
  - [Converting .raw to .mzML with ThermoRawFileParser](#converting-raw-to-mzml-with-thermorawfileparser)
- [Step 4: Running DIA-NN (Library-Free Mode)](#step-4-running-dia-nn-library-free-mode)
  - [How DIA-NN Works Under the Hood](#how-dia-nn-works-under-the-hood)
  - [DIA-NN Command-Line Pipeline](#dia-nn-command-line-pipeline)
  - [Key Parameter Breakdown](#key-parameter-breakdown)
- [Step 5: Understanding DIA-NN Output Tables](#step-5-understanding-dia-nn-output-tables)
- [Step 6: Upstream Quality Control Checks](#step-6-upstream-quality-control-checks)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In our [Bulk RNA-seq series](../BulkRNAseq/02_part-2-from-fastq-to-counts-best-practices.md), we took raw sequencing reads (`FASTQ`), trimmed adapters, aligned them against the reference genome, and quantified read counts per gene.

In discovery proteomics, our starting point is not text-based sequence reads, but binary mass spectrometer files (`.raw` from Thermo, `.d` from Bruker, or `.wiff` from Sciex). Our goal is to convert these raw chromatographic ion beams into a clean **protein quantification matrix**.

Over the last five years, **Data-Independent Acquisition (DIA)** coupled with deep-learning search engines like **DIA-NN** (Demichev et al., *Nature Methods* 2020) has superseded older Data-Dependent Acquisition (DDA) pipelines. DIA enables comprehensive proteome coverage (>8,000 proteins in cell lysates), high quantitative reproducibility, and dramatically reduced missing values.

In this guide, we walk through the complete upstream pipeline: from downloading raw instrument files to generating a cross-sample MaxLFQ protein expression matrix.

---

## How LC-MS/MS Generates Data: A 2-Minute Review

Before running algorithms, it is helpful to visualize what the instrument actually measures.

### The Three Dimensions of Mass Spectrometry

Bottom-up proteomics separates digested peptides across three physical dimensions:

```
  Intensity
     ▲
     │       MS1: Intact Precursors
     │        ┌───┐           ┌───┐
     │        │   │           │   │
     │      ┌─┘   └─┐       ┌─┘   └─┐
     └──────┴───────┴───────┴───────┴────────► m/z (Mass-to-charge)
            ▲               ▲
            │               │
     Fragmentation (Collision / HCD)
            │               │
            ▼               ▼
     MS2: Fragment Ions (b- and y-ions)
     │   │   │   │   │   │   │   │   │
     └───┴───┴───┴───┴───┴───┴───┴───┴───────► m/z
     ▲
     │ Retention Time (RT): Minutes eluting from HPLC column
```

1. **Retention Time (RT)**: The time (in minutes) at which a peptide elutes from the reverse-phase C18 liquid chromatography column.
2. **Precursor Mass-to-Charge Ratio ($m/z$) at MS1**: The mass-to-charge ratio of the intact, ionized peptide measured by the mass analyzer.
3. **Fragment Ion $m/z$ at MS2**: The masses of peptide fragments generated after breaking peptide backbone bonds (producing series of $b$-ions and $y$-ions).

### DDA vs. DIA: The Paradigm Shift

* **DDA (Data-Dependent Acquisition)**: The instrument inspects the MS1 survey scan, selects the top 10–20 highest-intensity peaks, isolates each with a very narrow filter ($\sim 1.0\text{ }m/z$), and fragments them one by one.
  * *The Problem*: Peptides elute in complex mixtures. Low-abundance peptides are frequently missed, leading to stochastic "dropouts" between biological replicates.
* **DIA (Data-Independent Acquisition)**: The instrument fragments **everything**. It steps through broad precursor windows (e.g., 32 sequential windows of $25\text{ }m/z$, covering $400\text{--}1200\text{ }m/z$).
  * *The Challenge*: MS2 spectra are multiplexed and chimeric (multiple peptides fragmented simultaneously).
  * *The Solution*: DIA-NN uses artificial neural networks to predict fragmentation patterns and retention times directly from sequence, accurately deconvolving chimeric spectra.

---

## Step 1: Download the Tutorial Dataset

### Dataset Overview: A549 Perturbation Cohort
For this tutorial, we use a benchmark DIA dataset hosted on PRIDE: **[PXD014777](https://www.ebi.ac.uk/pride/archive/projects/PXD014777)**.
* **Organism**: Human (*Homo sapiens*).
* **Cell line**: A549 lung carcinoma.
* **Condition**: Vehicle Control (DMSO) vs. Targeted Kinase Inhibitor treatment ($n = 3$ biological replicates per group; 6 samples total).
* **Instrument**: Thermo Q Exactive HF-X in DIA mode.

### Download via Command Line

Create your project directory and download the raw files:

```bash
# Create working directory structure
mkdir -p ~/Projects/learning_omics/Proteomics_Workspace/data/raw
mkdir -p ~/Projects/learning_omics/Proteomics_Workspace/databases
mkdir -p ~/Projects/learning_omics/Proteomics_Workspace/data/processed
cd ~/Projects/learning_omics/Proteomics_Workspace/data/raw

# Download the 6 raw files from PRIDE FTP (replace with target URLs)
# Sample 1-3: Control replicates
# Sample 4-6: Treated replicates
curl -O ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2020/05/PXD014777/A549_Ctrl_Rep1.raw
curl -O ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2020/05/PXD014777/A549_Ctrl_Rep2.raw
curl -O ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2020/05/PXD014777/A549_Ctrl_Rep3.raw
curl -O ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2020/05/PXD014777/A549_Trt_Rep1.raw
curl -O ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2020/05/PXD014777/A549_Trt_Rep2.raw
curl -O ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2020/05/PXD014777/A549_Trt_Rep3.raw
```

### Laptop Fast-Track (Skip the Compute)

> [!TIP]
> If you are working on a personal laptop or lack the bandwidth/time to download 9 GB of raw files and run 1–2 hours of DIA-NN processing, you can download the precomputed output table (`report.tsv`, ~15 MB):
> ```bash
> mkdir -p ~/Projects/learning_omics/Proteomics_Workspace/data/processed
> cd ~/Projects/learning_omics/Proteomics_Workspace/data/processed
> # Download precomputed DIA-NN output
> curl -L -o report.tsv https://raw.githubusercontent.com/ngs101/proteomics-tutorial-assets/main/data/A549_diann_report.tsv
> curl -L -o report.pg_matrix.tsv https://raw.githubusercontent.com/ngs101/proteomics-tutorial-assets/main/data/A549_diann_pg_matrix.tsv
> ```
> Once downloaded, you can proceed directly to **Part 2b** for downstream analysis in R!

---

## Step 2: Preparing Reference Databases

To identify peptides from mass spectra, search algorithms require a theoretical reference proteome in FASTA format. In addition, you must always append a database of **common laboratory contaminants**.

### Download UniProt Human Reference Proteome

We recommend the canonical, reviewed human proteome (Swiss-Prot, ~20,400 entries):

```bash
cd ~/Projects/learning_omics/Proteomics_Workspace/databases

# Download human canonical proteome (UP000005640) from UniProt
curl -o human_proteome.fasta.gz "https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Eukaryota/UP000005640/UP000005640_9606.fasta.gz"
gunzip human_proteome.fasta.gz
```

### Append the cRAP Contaminant Database

The **cRAP (Common Repository of Adventitious Proteins)** contains ~112 sequences commonly introduced during sample preparation, including:
* Human skin keratins (KRT1, KRT10, etc., shed from hands/air).
* Porcine/bovine trypsin (from enzymatic digestion).
* Bovine serum albumin (BSA, from cell culture media).

If these are not in your reference database, the search engine may incorrectly match contaminant spectra to human proteins, causing false-positive identifications.

```bash
# Download cRAP database
curl -o crap.fasta "https://raw.githubusercontent.com/thelovelab/CRAP/master/crap.fasta"

# Concatenate human reference proteome and cRAP into a single FASTA
cat human_proteome.fasta crap.fasta > human_with_crap.fasta

# Verify total entry count
grep -c "^>" human_with_crap.fasta
```

---

## Step 3: Raw File Handling & Conversion

### Profile vs. Centroid Mode
Mass spectrometers initially record ion signals as continuous waveforms (**profile mode**). To process data efficiently, peak shapes must be reduced to discrete points representing peak center and area (**centroid mode**).

```
   Profile Mode (Continuous)             Centroid Mode (Discrete)
             ▲                                     ▲
           ┌─┴─┐                                   │
          ┌┘   └┐                                  │
         ┌┘     └┐                                 │
        ─┴───────┴─► m/z                          ─┴─► m/z
```

DIA-NN natively centroids raw data on the fly. However, if running on Linux systems without Thermo proprietary libraries, convert `.raw` files to the open, centroided **`.mzML`** format using `ThermoRawFileParser`.

### Converting .raw to .mzML with ThermoRawFileParser

```bash
cd ~/Projects/learning_omics/Proteomics_Workspace/data/raw

# Convert all .raw files in parallel to centroided .mzML
for file in *.raw; do
    echo "Converting $file to mzML..."
    thermorawfileparser -i "$file" -o ../mzml/ -f 2 -m 1
done
```
* `-f 2`: Output format indexed `.mzML`.
* `-m 1`: Centroid both MS1 and MS2 spectra.

---

## Step 4: Running DIA-NN (Library-Free Mode)

### How DIA-NN Works Under the Hood

Older DIA workflows required building a physical "spectral library" by acquiring deep DDA fractionations first. **DIA-NN eliminates this requirement** through *in silico* library generation:
1. DIA-NN performs *in silico* tryptic digestion of all proteins in your FASTA file.
2. A deep neural network predicts the retention time (RT), ion mobility, and MS2 fragment ion intensities for each peptide precursor.
3. DIA-NN extracts ion chromatograms (XICs) from the DIA files for all predicted precursors.
4. An empirical decoy scoring model evaluates false discovery rates (FDR) using target-decoy competition at 1% precursor and 1% protein FDR.

```
 [ UniProt FASTA + cRAP ]
            │
            ▼  In silico tryptic digest
 [ Predicted Library ] ──► (Deep Neural Network: Predicts RT & MS2 intensities)
            │
            ├────────────────────────────────┐
            ▼                                ▼
   [ Sample 1 .mzML ]               [ Sample 2 .mzML ]
            │                                │
            ▼  Extract XICs & Score Matches  ▼
   [ Match Scoring ] ◄── Target-Decoy 1% FDR ─► [ Match Scoring ]
            │                                │
            └────────────────┬───────────────┘
                             ▼
              [ MaxLFQ Cross-Run Alignment ]
                             │
                             ▼
                 [ report.pg_matrix.tsv ]
```

### DIA-NN Command-Line Pipeline

Create an executable script named `run_diann.sh`:

```bash
#!/usr/bin/env bash
set -e

WORKSPACE=~/Projects/learning_omics/Proteomics_Workspace
RAW_DIR=$WORKSPACE/data/raw
OUT_DIR=$WORKSPACE/data/processed
FASTA=$WORKSPACE/databases/human_with_crap.fasta

mkdir -p $OUT_DIR

# Run DIA-NN in library-free mode with MaxLFQ quantification
diann \
  --f $RAW_DIR/A549_Ctrl_Rep1.raw \
  --f $RAW_DIR/A549_Ctrl_Rep2.raw \
  --f $RAW_DIR/A549_Ctrl_Rep3.raw \
  --f $RAW_DIR/A549_Trt_Rep1.raw \
  --f $RAW_DIR/A549_Trt_Rep2.raw \
  --f $RAW_DIR/A549_Trt_Rep3.raw \
  --fasta $FASTA \
  --fasta-search \
  --gen-spec-lib \
  --predictor \
  --out $OUT_DIR/report.tsv \
  --qvalue 0.01 \
  --matrices \
  --max-lfq \
  --relaxed-prot-inf \
  --smart-profiling \
  --threads 8

echo "DIA-NN processing completed successfully!"
```

Execute the script:
```bash
chmod +x run_diann.sh
./run_diann.sh
```

### Key Parameter Breakdown

- `--fasta-search` & `--gen-spec-lib`: Instructs DIA-NN to generate an *in silico* spectral library directly from the provided FASTA.
- `--predictor`: Activates deep neural network models to predict fragmentation spectra and RT.
- `--qvalue 0.01`: Enforces a strict 1% False Discovery Rate (FDR) at the precursor level.
- `--matrices`: Automatically outputs cross-sample summary matrices at the precursor, peptide, and protein group levels.
- `--max-lfq`: Applies the MaxLFQ algorithm to perform robust peptide-to-protein ratio rollups, ensuring that variance between individual peptides doesn't distort protein-level ratios.
- `--relaxed-prot-inf`: Employs parsimonious protein inference to group indistinguishable protein isoforms sharing identical peptides.

---

## Step 5: Understanding DIA-NN Output Tables

After execution, the `data/processed/` directory contains several key output files:

| File Name | Level | Description |
| :--- | :--- | :--- |
| `report.tsv` | Precursor / Peptide | Detailed, long-format table containing all scored precursors, FDR values, RT, and raw intensities. |
| `report.pg_matrix.tsv` | **Protein Group Matrix** | **The primary table for downstream analysis.** A wide matrix of samples (columns) vs. Protein Groups (rows) containing MaxLFQ-normalized intensities. |
| `report.gg_matrix.tsv` | Gene Matrix | Quantification aggregated at the gene symbol level. |
| `report.stats.tsv` | Run-level QC | Summary statistics listing the total number of precursors and protein groups identified in each run. |

Let's inspect the first few lines of `report.pg_matrix.tsv`:

```bash
head -n 5 $OUT_DIR/report.pg_matrix.tsv | cut -f 1-5
```

```text
Protein.Group    Protein.Names   Genes    A549_Ctrl_Rep1   A549_Ctrl_Rep2
P04040           CAT_HUMAN       CAT      1849204.5        1920140.2
P00338           LDHA_HUMAN      LDHA     4829105.0        4791040.0
P60709           ACTB_HUMAN      ACTB     9821034.0        9754120.0
P06733           ENOA_HUMAN      ENO1     3140291.5        3201402.0
```

---

## Step 6: Upstream Quality Control Checks

Before jumping into statistical testing, always evaluate upstream run metrics in `report.stats.tsv`:

1. **Identification Consistency**:
   * Count the number of protein groups per run. In high-quality human cell lysate DIA runs, you should typically see **6,000–8,500 protein groups** per sample.
   * If one replicate identifies 3,000 proteins while the others identify 7,500, check the instrument log (possible autosampler injection failure, column clogging, or electrospray instability).
2. **Mass Accuracy ($ppm$ drift)**:
   * DIA-NN reports the mass accuracy standard deviation across the chromatographic gradient. For modern Orbitrap instruments, this should remain $< 10\text{ ppm}$.
3. **Chromatographic Peak Width (FWHM)**:
   * Full Width at Half Maximum should typically average between **6 and 15 seconds** for standard 60-minute gradients.

---

## Conclusion & What’s Next

Congratulations! You have taken raw binary instrument files, configured a contaminant-aware reference proteome, performed deep-learning DIA deconvolution, and produced a quantified protein matrix (`report.pg_matrix.tsv`).

However, this raw matrix is not ready for differential testing:
- It still contains cRAP contaminant proteins.
- Intensities are on an unlogged, skewed scale.
- Certain proteins have missing values (NA) due to limits of detection.

In **[Part 2b: Data QC, Filtering, and the Missing Value Dilemma](03_part-2b-ms-data-qc-filtering-and-handling-missing-values.md)**, we transition into **R**, load our data into the Bioconductor **`QFeatures`** container, remove contaminants, explore missingness patterns (MNAR vs. MAR), and evaluate imputation strategies.

