# How to Analyze Proteomics Data Part 10: Integrated Proteogenomics: Bulk RNA-seq vs. Proteomics

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Transcriptomics, Proteogenomics, Multi-Omics, RNA-Protein Correlation

---

## Table of Contents

- [Introduction](#introduction)
- [Why mRNA and Protein Abundance Diverge](#why-mrna-and-protein-abundance-diverge)
  - [The Genome-Wide Correlation Reality (r ~ 0.45)](#the-genome-wide-correlation-reality-r--045)
  - [Which Genes Correlate Well, and Which Do Not?](#which-genes-correlate-well-and-which-do-not)
- [Step 1: Loading and Aligning Matched Multi-Omics Datasets](#step-1-loading-and-aligning-matched-multi-omics-datasets)
  - [Harmonizing RNA-seq and Proteomics Gene Identifiers](#harmonizing-rna-seq-and-proteomics-gene-identifiers)
  - [Matching Biological Samples](#matching-biological-samples)
- [Step 2: Gene-by-Gene mRNA-Protein Correlation Analysis](#step-2-gene-by-gene-mrna-protein-correlation-analysis)
  - [Computing Spearman Correlations Across Cohort Samples](#computing-spearman-correlations-across-cohort-samples)
  - [Plotting the Genome-Wide Correlation Distribution](#plotting-the-genome-wide-correlation-distribution)
- [Step 3: Quadrant Analysis of Differential Modulation](#step-3-quadrant-analysis-of-differential-modulation)
  - [The 9-Sector Multi-Omics Matrix](#the-9-sector-multi-omics-matrix)
  - [Constructing the Log2FC(RNA) vs. Log2FC(Protein) Scatter Plot](#constructing-the-log2fcrna-vs-log2fcprotein-scatter-plot)
  - [Identifying Post-Transcriptionally Buffered Genes](#identifying-post-transcriptionally-buffered-genes)
- [Biological Insights from Proteogenomic Discordance](#biological-insights-from-proteogenomic-discordance)
  - [Case A: Transcriptional Overdrive with Proteasomal Buffering](#case-a-transcriptional-overdrive-with-proteasomal-buffering)
  - [Case B: Post-Translational Degradation (E3 Ligase Targets)](#case-b-post-translational-degradation-e3-ligase-targets)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

Throughout this repository, we built comprehensive workflows for both **[Bulk RNA-seq](../BulkRNAseq/01_part-1-setting-up-the-environment.md)** and **Proteomics**.

In isolation:
- RNA-seq reveals transcriptional programs and genomic copy number effects.
- Proteomics reveals the actual functional machinery executing cellular phenotypes.

When we integrate the two into **Proteogenomics**, a fascinating new layer of biology emerges: **post-transcriptional and translational regulation**.

In this tutorial, we bridge our RNA-seq and proteomics knowledge:
1. Harmonizing RNA-seq count matrices and protein intensity matrices for the exact same patient samples (CPTAC / DepMap).
2. Calculating **gene-by-gene mRNA-to-protein correlations** across cohorts.
3. Constructing **quadrant scatter plots ($\log_2\text{FC}_{\text{RNA}}$ vs. $\log_2\text{FC}_{\text{Protein}}$)** to identify post-transcriptionally buffered proteins and translationally regulated genes.

---

## Why mRNA and Protein Abundance Diverge

### The Genome-Wide Correlation Reality (r ~ 0.45)

Across large-scale proteogenomic benchmark studies (such as the NCI **CPTAC** consortia across thousands of human tumors), the genome-wide median Spearman correlation between a gene's transcript level and its protein level is approximately **$r \approx 0.45$**:

```
        Distribution of mRNA-to-Protein Spearman Correlations (CPTAC Cohorts)
   Frequency
      ▲
      │                         ┌───┐
      │                         │   │
      │                ┌───┐    │   │    ┌───┐
      │       ┌───┐    │   │    │   │    │   │    ┌───┐
      │  ┌───┐│   │    │   │    │   │    │   │    │   │
      └──┴───┴┴───┴────┴───┴────┴───┴────┴───┴────┴───┴───► Spearman r
        -0.2  0.0     0.2      0.45     0.7      0.9
              ◄───────────────►         ◄───────────────►
               Stoichiometric            Secreted /
               Buffering                 Metabolic Enzymes
```

### Which Genes Correlate Well, and Which Do Not?

- **High Correlation ($r > 0.70$)**:
  - Secreted factors, cell-surface markers, and metabolic enzymes (e.g., LDHA, GAPDH).
  - These proteins respond dynamically to transcriptional demand without complex multi-protein stoichiometry.
- **Low or Zero Correlation ($r < 0.20$)**:
  - Subunits of large macromolecular machines: **ribosomes (RPL/RPS), proteasomes (PSMA/PSMB), and spliceosomes**.
  - *Why?* If the cell transcribes $3\times$ more mRNA for RPL11, but the other 79 ribosomal subunits are not increased, the orphaned RPL11 protein cannot assemble into a ribosome and is instantly degraded by ubiquitin-mediated proteolysis (**stoichiometric buffering**).

---

## Step 1: Loading and Aligning Matched Multi-Omics Datasets

```r
library(tidyverse)
library(ggrepel)

setwd("~/Projects/learning_omics/Proteomics_Workspace")

# Load differential expression results from Bulk RNA-seq (DESeq2 results)
rna_de <- read.csv("../BulkRNAseq/results/DEG_results.csv", stringsAsFactors = FALSE)

# Load differential abundance results from Proteomics (limma results from Part 2c)
prot_de <- read.csv("results/tables/DAP_results_limma.csv", stringsAsFactors = FALSE)
```

### Harmonizing RNA-seq and Proteomics Gene Identifiers

RNA-seq data typically uses **Ensembl IDs or Gene Symbols**, while proteomics uses **UniProt Accessions**. We harmonize both datasets by joining on standardized **HUGO Gene Symbols**:

```r
# Join RNA-seq and Proteomics tables on Gene Symbol
merged_de <- inner_join(
  rna_de %>% select(Gene = GeneSymbol, log2FC_RNA = log2FoldChange, padj_RNA = padj),
  prot_de %>% select(Gene = Genes, log2FC_Prot = logFC, padj_Prot = adj.P.Val, UniquePeptides),
  by = "Gene"
) %>%
  filter(!is.na(log2FC_RNA) & !is.na(log2FC_Prot))

cat("Matched genes quantified in BOTH RNA-seq and Proteomics:", nrow(merged_de), "\n")
```

```text
Matched genes quantified in BOTH RNA-seq and Proteomics: 5840 
```

---

## Step 2: Gene-by-Gene mRNA-Protein Correlation Analysis

### Computing Spearman Correlations Across Cohort Samples

```r
# Calculate overall concordance across log2 fold-changes
global_r <- cor(merged_de$log2FC_RNA, merged_de$log2FC_Prot, method = "spearman")
cat("Global Spearman correlation of differential response: r =", round(global_r, 3), "\n")
```

```text
Global Spearman correlation of differential response: r = 0.512 
```

There is positive correlation ($r = 0.51$), but significant variance remains unexplained by transcript abundance alone!

---

## Step 3: Quadrant Analysis of Differential Modulation

To dissect regulatory mechanisms, we classify genes into a **9-sector multi-omics matrix**:

```
                       Log2FC (Protein)
                              ▲
                 Q2           │           Q1
           Discordant         │      Concordant UP
           (RNA flat /        │      (Transcription &
            Protein UP)       │       Translation UP)
                              │
          ────────────────────┼────────────────────► Log2FC (RNA)
                              │
                 Q3           │           Q4
           Concordant DOWN    │      Discordant
           (Transcription &   │      (RNA UP /
            Protein DOWN)     │       Protein flat: BUFFERED)
                              ▼
```

### Classifying Genes into Regulatory Quadrants

```r
# Define significance thresholds (|log2FC| >= 1.0, padj < 0.05)
fc_cut <- 1.0
p_cut  <- 0.05

classified_df <- merged_de %>%
  mutate(
    Reg_RNA  = ifelse(padj_RNA < p_cut & abs(log2FC_RNA) >= fc_cut, sign(log2FC_RNA), 0),
    Reg_Prot = ifelse(padj_Prot < p_cut & abs(log2FC_Prot) >= fc_cut, sign(log2FC_Prot), 0),
    Category = case_when(
      Reg_RNA ==  1 & Reg_Prot ==  1 ~ "Concordant UP (Transcriptional Activation)",
      Reg_RNA == -1 & Reg_Prot == -1 ~ "Concordant DOWN (Transcriptional Repression)",
      Reg_RNA ==  1 & Reg_Prot ==  0 ~ "Post-Transcriptionally Buffered (RNA UP / Prot Flat)",
      Reg_RNA ==  0 & Reg_Prot == -1 ~ "Post-Translational Degradation (Prot DOWN / RNA Flat)",
      Reg_RNA ==  0 & Reg_Prot ==  1 ~ "Translational Activation (Prot UP / RNA Flat)",
      TRUE                           ~ "Unchanged / Minor Shift"
    )
  )

table(classified_df$Category)
```

```text
Concordant DOWN (Transcriptional Repression)               214 
Concordant UP (Transcriptional Activation)                 268 
Post-Transcriptionally Buffered (RNA UP / Prot Flat)       142 
Post-Translational Degradation (Prot DOWN / RNA Flat)       86 
Translational Activation (Prot UP / RNA Flat)               52 
Unchanged / Minor Shift                                   5078 
```

---

### Constructing the Log2FC(RNA) vs. Log2FC(Protein) Scatter Plot

```r
# Highlight key hallmark genes
key_genes <- c("DUSP4", "SPRY2", "CCND1", "EGR1", "MYC", "RPL11", "PSMD1", "CDKN1A")

p_quad <- ggplot(classified_df, aes(x = log2FC_RNA, y = log2FC_Prot)) +
  # Background points
  geom_point(aes(color = Category), alpha = 0.6, size = 1.5) +
  # Threshold dashed lines
  geom_vline(xintercept = c(-1.0, 1.0), linetype = "dashed", color = "grey40") +
  geom_hline(yintercept = c(-1.0, 1.0), linetype = "dashed", color = "grey40") +
  # Linear regression trendline
  geom_smooth(method = "lm", color = "black", linetype = "dotted", se = FALSE) +
  # Colors
  scale_color_manual(values = c(
    "Concordant UP (Transcriptional Activation)"   = "#D95F02",
    "Concordant DOWN (Transcriptional Repression)" = "#2B5C8F",
    "Post-Transcriptionally Buffered (RNA UP / Prot Flat)" = "#7570B3",
    "Post-Translational Degradation (Prot DOWN / RNA Flat)" = "#E7298A",
    "Translational Activation (Prot UP / RNA Flat)"         = "#1B9E77",
    "Unchanged / Minor Shift"                     = "grey80"
  )) +
  # Repel labels for key genes
  geom_text_repel(
    data = classified_df %>% filter(Gene %in% key_genes),
    aes(label = Gene),
    size = 4, fontface = "bold",
    box.padding = 0.6, max.overlaps = 15
  ) +
  theme_classic(base_size = 12) +
  labs(
    title = "Integrated Proteogenomics: RNA-seq vs. Proteomics",
    subtitle = "Quadrant analysis reveals post-transcriptional buffering and targeted degradation",
    x = expression(Log[2]~Fold~Change~(Bulk~RNA-seq)),
    y = expression(Log[2]~Fold~Change~(Proteomics))
  ) +
  theme(
    legend.position = "bottom",
    legend.title = element_blank(),
    plot.title = element_text(face = "bold", size = 14)
  )

ggsave("results/figures/Fig10_Proteogenomic_Quadrant.pdf", p_quad, width = 8, height = 7)
ggsave("results/figures/Fig10_Proteogenomic_Quadrant.png", p_quad, width = 8, height = 7, dpi = 300)
```

---

## Biological Insights from Proteogenomic Discordance

### Case A: Transcriptional Overdrive with Proteasomal Buffering
In the plot above, notice ribosomal genes like **`RPL11`** located in the purple sector (**RNA UP, Protein Flat**):
- RNA-seq reports a $3\times$ increase in transcript ($\log_2\text{FC}_{\text{RNA}} = +1.6, p_{\text{adj}} < 10^{-6}$).
- Proteomics reveals that the protein did not change at all ($\log_2\text{FC}_{\text{Prot}} = +0.05$).
- *Interpretation*: The cell up-regulated transcription, but protein abundance was strictly buffered at the post-translational level.

### Case B: Post-Translational Degradation (E3 Ligase Targets)
Notice genes in the pink sector (**Protein DOWN, RNA Flat**):
- Transcript levels remain identical, but protein levels plummet by $> 70\%$.
- *Interpretation*: Activation of an E3 ubiquitin ligase or autophagic degradation targeted the protein directly, an effect completely invisible to RNA sequencing!

---

## Conclusion & What’s Next

In this guide, you learned:
1. Why **genome-wide mRNA-protein correlation averages $r \approx 0.45$**.
2. How to harmonize RNA-seq count tables and mass spectrometry protein tables in R.
3. How to perform **9-sector quadrant analysis** to classify genes into transcriptional vs. post-transcriptional regimes.
4. How to identify **stoichiometrically buffered complexes** and **targeted protein degradation**.

In our final chapter, we discuss open science, data integrity, and reproducibility:

In **[Part 11: Depositing and Reanalyzing Data in PRIDE & ProteomeXchange](15_part-11-submitting-data-to-pride-and-proteomexchange.md)**, we walk through preparing raw spectra, metadata, and search outputs for deposition into the public **PRIDE** database using the ProteomeXchange submission tool!
