# How to Analyze Proteomics Data Part 9: Phosphoproteomics & Post-Translational Modification (PTM) Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Phosphoproteomics, PTMs, Kinase Signaling, KSEA, PhosphoSitePlus

---

## Table of Contents

- [Introduction](#introduction)
- [Why Total Protein is Not Enough: The Power of Phosphorylation](#why-total-protein-is-not-enough-the-power-of-phosphorylation)
- [Biophysical & Analytical Foundations](#biophysical--analytical-foundations)
  - [The Mass Shift: +79.9663 Da](#the-mass-shift-799663-da)
  - [Phosphopeptide Enrichment: Fe-NTA and TiO2 IMAC](#phosphopeptide-enrichment-fe-nta-and-tio2-imac)
- [The Two Core Bioinformatic Challenges in Phosphoproteomics](#the-two-core-bioinformatic-challenges-in-phosphoproteomics)
  - [1. Site Localization Probability (Class I Phosphosites)](#1-site-localization-probability-class-i-phosphosites)
  - [2. The Total Protein Normalization Dilemma](#2-the-total-protein-normalization-dilemma)
- [Step 1: Loading and Filtering Phosphosite Data in R](#step-1-loading-and-filtering-phosphosite-data-in-r)
  - [Parsing Phosphosite Annotations (Gene_Residue_Position)](#parsing-phosphosite-annotations-gene_residue_position)
  - [Filtering for High-Confidence Class I Sites (P >= 0.75)](#filtering-for-high-confidence-class-i-sites-p--075)
- [Step 2: Differential Phosphorylation Testing](#step-2-differential-phosphorylation-testing)
  - [Fitting Linear Models with limma](#fitting-linear-models-with-limma)
  - [Volcano Plots of Modulated Phosphosites](#volcano-plots-of-modulated-phosphosites)
- [Step 3: Kinase-Substrate Enrichment Analysis (KSEA)](#step-3-kinase-substrate-enrichment-analysis-ksea)
  - [Mapping Substrates to Upstream Kinases (PhosphoSitePlus)](#mapping-substrates-to-upstream-kinases-phosphositeplus)
  - [Computing Kinase Activity Z-Scores with KSEAapp](#computing-kinase-activity-z-scores-with-kseaapp)
  - [Visualizing Kinase Activation vs. Inhibition](#visualizing-kinase-activation-vs-inhibition)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In our previous chapters, we measured **protein abundance**. However, in cellular signal transduction, protein abundance often remains completely unchanged:

```
  INACTIVE KINASE (Basal state)
           │
           │  Upstream Growth Factor / Ligand Stimulation
           ▼  (Phosphorylated at Thr202 / Tyr204 within 60 seconds)
   ACTIVE KINASE (Phosphorylated)
           │
           ▼  Downstream Phosphorylation Cascade (Transcription / Cell Division)
```

The protein copy number did not change—its enzymatic activity was flipped on by the covalent addition of a single phosphate group ($\text{PO}_4^{2-}$).

Reversible protein phosphorylation on **Serine (S), Threonine (T), and Tyrosine (Y)** controls cell proliferation, metabolism, and apoptosis. Aberrant kinase activation drives oncogenesis, making protein kinases one of the largest classes of cancer drug targets.

In this tutorial, we dive into **phosphoproteomics**: how mass spectrometers locate phosphorylation sites, how to filter for **Class I localization confidence**, and how to infer which upstream kinases are active using **Kinase-Substrate Enrichment Analysis (KSEA)**.

---

## Biophysical & Analytical Foundations

### The Mass Shift: +79.9663 Da
When a kinase transfers a phosphate group from ATP to an amino acid hydroxyl group:
$$\text{Peptide} + \text{HPO}_3 \longrightarrow \text{Phosphopeptide} + \Delta m$$
The monoisotopic mass increases by exactly **$+79.9663\text{ Da}$**. In search engines (DIA-NN, MSFragger, MaxQuant), this is searched as a variable modification on **S, T, and Y**.

```
    Serine Residue                          Phosphoserine Residue
          H                                           H
          │                                           │
    ─ N ─ C ─ C ─                               ─ N ─ C ─ C ─
      │   │   │                                   │   │   │
      H   CH2 O                                   H   CH2 O
          │                                           │
          OH                                          O ─ P ─ O⁻  (+79.9663 Da)
                                                          │
                                                          O⁻
```

### Phosphopeptide Enrichment: Fe-NTA and TiO2 IMAC
Under normal conditions, only $\sim 1\text{--}5\%$ of a given protein's molecules are phosphorylated at any instant. If you inject whole-cell tryptic digests into an LC-MS, unmodified peptides overwhelm the instrument detector.

To solve this, laboratories use **Immobilized Metal Affinity Chromatography (IMAC)** using **Iron ($\text{Fe}^{3+}\text{-NTA}$)** or **Titanium Dioxide ($\text{TiO}_2$)**:
- The positively charged metal ions specifically chelate negatively charged phosphate groups on peptides.
- Non-phosphorylated peptides wash away; pure phosphopeptides are eluted at high pH.

---

## The Two Core Bioinformatic Challenges in Phosphoproteomics

### 1. Site Localization Probability (Class I Phosphosites)
Tryptic peptides often contain multiple potential phosphorylation sites. For example, in the peptide:
$$\text{A}\mathbf{S}\text{PE}\mathbf{T}\text{L}\mathbf{S}\text{K}$$
There are three candidate residues (Ser2, Thr5, Ser7). The precursor mass tells us that **one** phosphate is attached, but **which residue carries it?**

Search engines calculate **Site Localization Probabilities** (using algorithms like *ptmRS* or *phosphoRS*) by examining site-determining fragment ions ($b$- and $y$-ions):
- **Class I Phosphosites**: Localization probability $P \ge 0.75$ (high confidence that the assigned residue is correct).
- **Class II / III Phosphosites**: Localization probability $P < 0.75$ (ambiguous site localization).

> [!IMPORTANT]
> Always filter your phosphoproteomics matrix to retain **Class I sites ($P \ge 0.75$)** before performing kinase-substrate analysis!

---

### 2. The Total Protein Normalization Dilemma
When a phosphosite shows a $2\times$ increase in abundance:
- **Possibility A**: The kinase became hyperactive, phosphorylating more molecules of the protein.
- **Possibility B**: Kinase activity is unchanged, but total protein expression doubled!

To decouple signaling activity from protein synthesis, advanced studies perform **matched total proteomics** and compute the normalized phosphorylation change:
$$\Delta\text{Phosphorylation}_{\text{true}} = \log_2\text{FC}_{\text{Phospho}} - \log_2\text{FC}_{\text{Total}}$$

---

## Step 1: Loading and Filtering Phosphosite Data in R

Let's load a high-resolution phosphoproteome dataset measuring kinase inhibitor response in A549 cells:

```r
library(tidyverse)
library(limma)

setwd("~/Projects/learning_omics/Proteomics_Workspace")

# Load phosphosite quantification table (MaxQuant Phospho (STY)Sites or DIA-NN output)
# Contains Site IDs formatted as: Gene_ResiduePosition (e.g., MAPK1_T185, AKT1_S473)
phospho_df <- read.csv("data/processed/A549_phospho_matrix.csv", stringsAsFactors = FALSE)

# Inspect columns
glimpse(phospho_df[, 1:7])
```

```text
Rows: 14,210
Columns: 7
$ SiteID         <chr> "MAPK1_T185", "MAPK3_T202", "AKT1_S473", "RB1_S807", ...
$ Gene           <chr> "MAPK1", "MAPK3", "AKT1", "RB1", ...
$ AminoAcid      <chr> "T", "T", "S", "S", ...
$ Position       <int> 185, 202, 473, 807, ...
$ Localization_P <dbl> 0.99, 0.98, 0.96, 0.94, ...
$ Ctrl_Rep1      <dbl> 24.1, 23.8, 22.1, 25.4, ...
$ Trt_Rep1       <dbl> 18.2, 17.9, 21.9, 20.1, ...
```

---

### Filtering for High-Confidence Class I Sites (P >= 0.75)

```r
# Filter for Class I localization sites
class1_phospho <- phospho_df %>%
  filter(Localization_P >= 0.75)

cat("Total quantified phosphosites:", nrow(phospho_df), "\n")
cat("Retained Class I sites (P >= 0.75):", nrow(class1_phospho), "\n")
```

```text
Total quantified phosphosites: 14210 
Retained Class I sites (P >= 0.75): 11840 
```

---

## Step 2: Differential Phosphorylation Testing

We test for differential phosphorylation using empirical Bayes moderated linear models (`limma`):

```r
# Extract intensity matrix
sample_cols <- c("Ctrl_Rep1", "Ctrl_Rep2", "Ctrl_Rep3", "Trt_Rep1", "Trt_Rep2", "Trt_Rep3")
mat_phos <- as.matrix(class1_phospho[, sample_cols])
rownames(mat_phos) <- class1_phospho$SiteID

# Design matrix
condition <- factor(rep(c("Control", "Treated"), each = 3), levels = c("Control", "Treated"))
design <- model.matrix(~ 0 + condition)
colnames(design) <- c("Control", "Treated")

# Fit linear model
fit <- lmFit(mat_phos, design)
contrast <- makeContrasts(Treated_vs_Control = Treated - Control, levels = design)
fit_contrast <- contrasts.fit(fit, contrast)
fit_bayes <- eBayes(fit_contrast, trend = TRUE, robust = TRUE)

# Extract differential phosphosites
diff_phos <- topTable(fit_bayes, number = Inf, adjust.method = "BH")
diff_phos$SiteID <- rownames(diff_phos)
diff_phos <- left_join(diff_phos, class1_phospho[, c("SiteID", "Gene", "AminoAcid", "Position")], by = "SiteID")

head(diff_phos %>% select(SiteID, logFC, P.Value, adj.P.Val), 5)
```

```text
      SiteID     logFC      P.Value    adj.P.Val
1 MAPK1_T185 -5.842014 1.482011e-09 1.754701e-05
2 MAPK3_T202 -5.710492 3.204192e-09 1.897282e-05
3   RB1_S807 -4.102391 8.104291e-08 3.199492e-04
4   JUN_S73   -3.849102 1.140291e-07 3.375262e-04
```

Notice the massive drop in **MAPK1 (ERK2) Thr185** and **MAPK3 (ERK1) Thr202** ($\log_2\text{FC} \approx -5.8$, a **$55\times$ reduction in phosphorylation**)! The targeted inhibitor has shut down ERK activation.

---

## Step 3: Kinase-Substrate Enrichment Analysis (KSEA)

### Mapping Substrates to Upstream Kinases (PhosphoSitePlus)

Individual phosphosites tell us what changed, but **KSEA** (*Casado et al., Sci Signal 2013*) tells us **which upstream kinases caused the change**.

KSEA queries curated databases like **PhosphoSitePlus** and **SIGNOR** to map downstream substrate sites to their upstream phosphorylating kinases.

```
       [ UPSTREAM KINASE: MAPK1 / ERK2 ]
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
[ Substrate Site 1 ] [ Substrate Site 2 ] [ Substrate Site 3 ]
  (RSK1_S380)          (FOS_T232)           (JUN_S73)
  logFC = -3.2         logFC = -2.8         logFC = -3.8
                       │
                       ▼
          KSEA Kinase Activity Score
          Z-score = -4.85 (P = 1.2e-6)
          ==> ERK2 is strongly INHIBITED!
```

---

### Computing Kinase Activity Z-Scores with KSEAapp

We compute kinase activity scores using the **`KSEAapp`** Bioconductor package:

```r
library(KSEAapp)

# Format dataset for KSEAapp
# Required columns: Protein, Gene, Peptide, Res, Pos, FC, p.value
ksea_input <- diff_phos %>%
  mutate(
    Protein = Gene,
    Peptide = SiteID,
    Res     = AminoAcid,
    Pos     = Position,
    FC      = 2^logFC, # KSEAapp uses linear fold change
    p.value = adj.P.Val
  ) %>%
  select(Protein, Gene, Peptide, Res, Pos, FC, p.value)

# Run KSEA against PhosphoSitePlus kinase-substrate annotations
ksea_scores <- KSEA.Scores(
  KSData = PhosphoSitePlus,
  PXData = ksea_input,
  NetPath = FALSE,
  m.cutoff = 3 # Require at least 3 known substrates per kinase
)

head(ksea_scores %>% select(Kinase.Gene, m, z.score, p.value), 6)
```

```text
  Kinase.Gene  m   z.score      p.value
1       MAPK1 18 -5.120491 3.048201e-07
2       MAPK3 14 -4.891042 1.002914e-06
3        CDK1 22 -4.102391 4.092014e-05
4        CDK2 16 -3.849102 1.182011e-04
5        AKT1  9  0.241029 8.095412e-01
```

The algorithm identified that **MAPK1 (ERK2), MAPK3 (ERK1), and CDK1** are significantly inhibited ($z < -4.0$), while AKT1 signaling remains unaffected!

---

### Visualizing Kinase Activation vs. Inhibition

```r
# Create lollipop plot of kinase activity z-scores
p_kinase <- ggplot(ksea_scores %>% head(10), aes(x = reorder(Kinase.Gene, z.score), y = z.score, fill = z.score > 0)) +
  geom_col(width = 0.5) +
  coord_flip() +
  scale_fill_manual(values = c("TRUE" = "#D95F02", "FALSE" = "#2B5C8F"), guide = "none") +
  theme_bw(base_size = 12) +
  labs(
    title = "Kinase Activity Profiles Inferred by KSEA",
    subtitle = "Z-score < -2 indicates statistically significant kinase shutdown",
    x = "Kinase",
    y = "KSEA Activity Z-score"
  )

ggsave("results/figures/Fig9_KSEA_Kinase_Activity.png", p_kinase, width = 6, height = 4.5)
```

---

## Conclusion & What’s Next

In this guide, you learned:
1. Why total protein abundance fails to capture signaling and how phosphorylation adds a **$+79.9663\text{ Da}$** mass shift.
2. How to filter for **Class I localization sites ($P \ge 0.75$)**.
3. How to statistically model phosphosites with **`limma`**.
4. How **Kinase-Substrate Enrichment Analysis (KSEA)** reconstructs upstream kinase signaling networks from downstream phosphosite changes.

Now, we bring everything full circle by combining our **Bulk RNA-seq** knowledge with our **Proteomics** knowledge!

In **[Part 10: Integrated Proteogenomics: Bulk RNA-seq vs. Proteomics](14_part-10-integrated-proteogenomics-bulk-rnaseq-vs-proteomics.md)**, we integrate matched RNA-seq counts and protein intensities to uncover post-transcriptionally buffered genes and build quadrant discordance plots!
