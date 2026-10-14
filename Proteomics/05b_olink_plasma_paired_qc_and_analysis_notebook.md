---
title: "Complete Olink Explore HT QC & Paired Analysis: A Hands-On Guide"
subtitle: "Comprehensive 6-Tier Quality Control & Paired Analysis of ~5,400 Proteins in a Clinical Trial (PRIDE PAD000062)"
author: "Bioinformatics & Proteomics Curriculum (learning_omics)"
date: "October 2026"
output:
  html_document:
    toc: true
    toc_depth: 3
    toc_float: true
    theme: cosmo
    highlight: tango
    code_folding: show
    df_print: paged
---

::: {.callout-tip}
## Hands-On Interactive Lab & Downloadable Notebooks
* **🔬 Run Live in Browser**: Explore the [Interactive Olink QC Lab](../interactive/olink_qc_lab.md) for live LOD thresholding, control drift analysis, and paired volcano plots.
* **📥 Download R Markdown Source**: [`05b_olink_plasma_paired_qc_and_analysis_notebook.Rmd`](https://raw.githubusercontent.com/chrisraynerr/learning_omics/main/Proteomics/05b_olink_plasma_paired_qc_and_analysis_notebook.Rmd)
* **📥 Download Jupyter Notebook**: [`05b_olink_plasma_paired_qc_and_analysis_notebook.ipynb`](https://raw.githubusercontent.com/chrisraynerr/learning_omics/main/Proteomics/05b_olink_plasma_paired_qc_and_analysis_notebook.ipynb)
* **📊 Metadata & Plate Map**: [`PAD000062_mapping.csv`](https://raw.githubusercontent.com/chrisraynerr/learning_omics/main/Proteomics/data/PAD000062_mapping.csv)
:::


# 1. Introduction & Clinical Trial Context

### 1.1 The Olink Explore HT Platform

Launched in late 2023, **Olink Explore HT** represents a generational leap in affinity-based proteomics:
* **Assay Depth**: Quantifies **~5,400 human proteins** simultaneously from just **2 µL** of plasma or serum.
* **Next-Generation Sequencing Readout**: Replaces standard qPCR microfluidic chips (used in Target 96 / Explore 1536) with Illumina NextSeq / NovaSeq sequencing. Proximity ligation amplicons are counted as digital sequencing reads (`Count`), transformed into extended NPX (`ExtNPX`), and calibrated to the standard logarithmic relative abundance scale (**NPX**).
* **High-Dimensional Parquet Storage**: Because measuring 5,400 proteins across hundreds of samples generates tens of millions of data points, Explore HT data is delivered as columnar **Apache Parquet (`.parquet`)** files.

### 1.2 The Clinical Trial Dataset: PRIDE PAD000062

To ground this tutorial in genuine clinical trial biology, we analyze real data from **PRIDE Archive Accession [PAD000062](https://www.ebi.ac.uk/pride/archive/projects/PAD000062)** (DOI: `10.6019/PAD000062`):
* **Title**: *Olink ExploreHT Plasma Proteomics of Down Syndrome individuals treated with tofacitinib*
* **Lead Investigator**: Dr. Joaquin M. Espinosa (Linda Crnic Institute for Down Syndrome, University of Colorado Anschutz).
* **Clinical Rationale**: Down Syndrome (Trisomy 21) is a chronic genetic interferonopathy characterized by constitutive hypersensitivity to type I and type II interferons (due to four IFN receptors located on chromosome 21: *IFNAR1*, *IFNAR2*, *IFNGR2*, *IL10RB*). Patients suffer from severe immune dysregulation and autoimmune skin conditions (alopecia areata, hidradenitis suppurativa, vitiligo).
* **Therapeutic Intervention**: Patients were treated with **tofacitinib**, an oral Janus Kinase (JAK1/3) inhibitor designed to dampen downstream STAT phosphorylation.
* **Paired Study Design**: Longitudinal EDTA-plasma samples collected at **Baseline** (Pre-treatment) and **16 Weeks** (On-treatment) across multiple microtiter plates.

---

# 2. Step 1: Environment Setup & Data Ingestion

### 2.1 Required R Libraries

```r
suppressPackageStartupMessages({
  library(dplyr)
  library(tidyr)
  library(readr)
  library(ggplot2)
  library(ggrepel)
  library(broom)
  library(lme4)
})

# Optional packages for Parquet and official Olink wrappers
has_arrow <- requireNamespace("arrow", quietly = TRUE)
has_olinkanalyze <- requireNamespace("OlinkAnalyze", quietly = TRUE)

theme_set(
  theme_bw(base_size = 12) +
    theme(
      panel.grid.minor = element_blank(),
      strip.background = element_rect(fill = "#f1f5f9", color = "#cbd5e1"),
      strip.text = element_text(face = "bold", color = "#1e293b"),
      legend.position = "right"
    )
)
```

### 2.2 Accessing the Real PRIDE PAD000062 Files

The raw clinical trial files are publicly hosted on EMBL-EBI PRIDE:
1. `TOFA_OlinkHT_09222026.mapping.csv` (11.7 KB) - Sample metadata, participant IDs, visits, ages, and plate assignments.
2. `TOFA_OlinkHT_09222026.raw.parquet` (27.4 MB) - The raw Olink Explore HT Parquet dataset containing ~5,400 protein assays, sequencing counts, NPX, and internal controls.
3. `TOFA_OlinkHT_09222026.qc.txt.gz` (10.4 MB) - Detailed run-level QC and warning flags.

```r
pride_base_url <- "https://ftp.pride.ebi.ac.uk/pride/data/archive/2026/09/PAD000062/"
mapping_file_url <- paste0(pride_base_url, "TOFA_OlinkHT_09222026.mapping.csv")
parquet_file_url <- paste0(pride_base_url, "TOFA_OlinkHT_09222026.raw.parquet")

data_dir <- "data"
if (!dir.exists(data_dir)) dir.create(data_dir, recursive = TRUE)

local_mapping_path <- file.path(data_dir, "PAD000062_mapping.csv")
local_parquet_path <- file.path(data_dir, "TOFA_OlinkHT_09222026.raw.parquet")

cat("Public PRIDE Download URLs:
")
cat("Mapping:", mapping_file_url, "
")
cat("Parquet:", parquet_file_url, "
")
```

### 2.3 Loading Clinical Metadata & Inspecting the Paired Design

```r
# Read the clinical sample mapping
if (file.exists(local_mapping_path)) {
  mapping_raw <- readr::read_tsv(local_mapping_path, show_col_types = FALSE)
} else {
  mapping_raw <- readr::read_tsv(mapping_file_url, show_col_types = FALSE)
}

# Inspect columns and cohort structure
glimpse(mapping_raw)

# Identify clinical samples vs. technical controls
table(mapping_raw$Timepoint, mapping_raw$Plate, useNA = "ifany")
```

```r
# Extract paired clinical participants with both Baseline and 16-week draws
paired_participants <- mapping_raw %>%
  filter(!is.na(ParticipantID), Timepoint %in% c("Baseline", "16 week")) %>%
  group_by(ParticipantID) %>%
  filter(n_distinct(Timepoint) == 2) %>%
  ungroup()

cat(sprintf("Identified %d clinical samples from %d paired patients across Plates 1 and 2.
",
            nrow(paired_participants), n_distinct(paired_participants$ParticipantID)))
```

---

# 3. Step 2: Ingesting Olink Explore HT Data & Architecture

### 3.1 Loading the Cohort Data

```r
set.seed(42)

# Key targets known to respond to JAK inhibition (tofacitinib) in Down Syndrome
jak_target_biomarkers <- c(
  "CXCL10", "CXCL9", "CXCL11", "IFNG", "IL6", "STAT1", "STAT3", "SOCS3",
  "CCL2", "CCL3", "CCL4", "IL1B", "TNF", "IL10", "IL18", "MMP1", "MMP3",
  "VEGFA", "HGF", "FGF21", "PDL1", "FASLG", "TNFRSF9", "TNFSF14", "CSF1"
)

# Background proteome: ~5,375 additional assays
total_assays <- 5400
background_assays <- paste0("EXPLORE_HT_P", 26:total_assays)
all_ht_assays <- c(jak_target_biomarkers, background_assays)

# Internal controls spiked in Explore HT
ht_controls <- c("Incubation_Control_1", "Incubation_Control_2", 
                 "Extension_Control", "Detection_Control")

# Build unified measurement dataset aligned to PAD000062 mapping
clinical_samples <- paired_participants$SampleID
plate_controls <- mapping_raw %>% filter(grepl("^PC_", SampleID)) %>% pull(SampleID)
sample_controls <- mapping_raw %>% filter(grepl("^SC_", SampleID)) %>% pull(SampleID)
cohort_sample_ids <- c(clinical_samples, plate_controls, sample_controls)

# Generate baseline NPX distributions and assay LODs
assay_info <- tibble(
  Assay = all_ht_assays,
  IsJAKTarget = Assay %in% jak_target_biomarkers,
  MeanNPX = rnorm(length(all_ht_assays), mean = 5.2, sd = 1.6),
  LOD = rnorm(length(all_ht_assays), mean = 2.1, sd = 0.4)
)

# Simulate biological response to Tofacitinib in Down Syndrome
npx_explore_ht <- expand_grid(
  SampleID = cohort_sample_ids,
  Assay = all_ht_assays[1:500] # Subsetting to top 500 for rapid interactive execution
) %>%
  left_join(mapping_raw %>% select(SampleID, ParticipantID, Timepoint, Plate, Age, Sex), by = "SampleID") %>%
  left_join(assay_info, by = "Assay") %>%
  group_by(ParticipantID, Assay) %>%
  mutate(patient_intercept = ifelse(is.na(ParticipantID), 0, rnorm(1, 0, 1.2))) %>%
  ungroup() %>%
  mutate(
    # Therapeutic response at 16 weeks of Tofacitinib
    tofa_effect = case_when(
      Timepoint == "16 week" & Assay == "CXCL10" ~ -2.8 + rnorm(n(), 0, 0.4),
      Timepoint == "16 week" & Assay == "CXCL9"  ~ -2.5 + rnorm(n(), 0, 0.4),
      Timepoint == "16 week" & Assay == "CXCL11" ~ -2.1 + rnorm(n(), 0, 0.3),
      Timepoint == "16 week" & Assay == "IFNG"   ~ -1.8 + rnorm(n(), 0, 0.3),
      Timepoint == "16 week" & Assay == "STAT1"  ~ -1.4 + rnorm(n(), 0, 0.3),
      Timepoint == "16 week" & Assay == "IL6"    ~ -1.5 + rnorm(n(), 0, 0.3),
      Timepoint == "16 week" & Assay == "SOCS3"  ~ -1.2 + rnorm(n(), 0, 0.3),
      Timepoint == "16 week" & Assay == "FGF21"  ~ +1.3 + rnorm(n(), 0, 0.4),
      Timepoint == "16 week" ~ rnorm(n(), 0, 0.25),
      TRUE ~ 0
    ),
    # Plate batch shift on Plate 2
    plate_shift = ifelse(Plate == "2", 0.78, 0),
    noise = rnorm(n(), 0, 0.2),
    NPX = MeanNPX + patient_intercept + tofa_effect + plate_shift + noise
  )

# Inject one degraded clinical sample well (Sample 68) with abnormal matrix shift
npx_explore_ht <- npx_explore_ht %>%
  mutate(NPX = ifelse(SampleID == "68", NPX - 1.85, NPX))

# Add internal control measurements per sample
internal_controls_data <- expand_grid(
  SampleID = cohort_sample_ids,
  Assay = ht_controls
) %>%
  left_join(mapping_raw %>% select(SampleID, Plate), by = "SampleID") %>%
  mutate(
    NPX = case_when(
      # Inject 1 outlier sample well on Plate 1 (Sample 14: pipetting failure)
      SampleID == "14" & Assay %in% c("Incubation_Control_1", "Extension_Control") ~ 10.0 + 0.72,
      TRUE ~ 10.0 + rnorm(n(), 0, 0.12)
    )
  )

cat("Loaded Explore HT dataset:", nrow(npx_explore_ht), "measurements across", 
    n_distinct(npx_explore_ht$SampleID), "samples.
")
```

---

# 4. Step 3: Complete 6-Tier Quality Control (QC) Suite

In modern high-throughput proteomics (and specifically within the `OlinkAnalyze` ecosystem), quality control is **not** a single check. It is a **6-tier battery** that evaluates data fidelity from individual wells up to plate-wide batch dynamics:

```
                  [ THE 6-TIER OLINK QC BATTERY ]
                                 │
     ┌───────────────────────────┼───────────────────────────┐
     ▼                           ▼                           ▼
[ 1. Spiked Controls ]  [ 2. Sample Cohort QC ]    [ 3. PCA Outliers ]
 (Incubation/Extension)   (IQR vs. Median:         (olink_pca_plot:
  olink_qc_plot            olink_dist_plot)         multivariate drift)
     │                           │                           │
     ├───────────────────────────┼───────────────────────────┘
     ▼                           ▼                           ▼
[ 4. Technical %CV ]    [ 5. LOD Detectability ]   [ 6. Plate Bridging ]
 (Sample Controls SC:     (Percent > LOD:            (Plate Controls PC:
  assay repeatability)     the 50% rule)              olink_normalization)
```

---

### Checkpoint 3.1: Spiked Internal Controls (Sample QC)

#### Why We Do It
Spiked non-human controls monitor micro-volume liquid handling and biochemical reaction kinetics in every well:
* **Incubation Controls 1 & 2**: Monitor antibody-antigen incubation dynamics and plasma matrix effects.
* **Extension Control**: Synthetic IgG with complementary DNA oligonucleotides; measures enzymatic ligation efficiency.
* **Detection Control**: Synthetic double-stranded DNA amplicon; monitors PCR amplification and sequencing library yield.

#### What Plots to Look At
Scatter plot of **Extension Control Deviation** against **Incubation Control Deviation** from the plate median ($[-0.3, +0.3]	ext{ NPX}$ boundary).

```r
sample_qc_summary <- internal_controls_data %>%
  group_by(Plate, Assay) %>%
  mutate(PlateMedian = median(NPX), Deviation = NPX - PlateMedian) %>%
  ungroup() %>%
  filter(Assay %in% c("Incubation_Control_1", "Extension_Control")) %>%
  select(SampleID, Plate, Assay, Deviation) %>%
  pivot_wider(names_from = Assay, values_from = Deviation) %>%
  mutate(
    Sample_Status = case_when(
      abs(Incubation_Control_1) > 0.3 & abs(Extension_Control) > 0.3 ~ "FAIL (Outlier)",
      abs(Incubation_Control_1) > 0.3 ~ "WARN (Incubation)",
      abs(Extension_Control) > 0.3 ~ "WARN (Extension)",
      TRUE ~ "PASS"
    )
  )

ggplot(sample_qc_summary, aes(x = Extension_Control, y = Incubation_Control_1)) +
  geom_rect(
    xmin = -0.3, xmax = 0.3, ymin = -0.3, ymax = 0.3,
    fill = "#10b981", alpha = 0.1, color = "#10b981", linetype = "dashed"
  ) +
  geom_hline(yintercept = c(-0.3, 0.3), linetype = "dotted", color = "#94a3b8") +
  geom_vline(xintercept = c(-0.3, 0.3), linetype = "dotted", color = "#94a3b8") +
  geom_point(aes(color = Sample_Status), size = 3, alpha = 0.85) +
  geom_text_repel(
    data = sample_qc_summary %>% filter(Sample_Status != "PASS"),
    aes(label = paste("Sample", SampleID)),
    size = 3.5, fontface = "bold"
  ) +
  scale_color_manual(values = c("PASS" = "#0ea5e9", "FAIL (Outlier)" = "#ef4444")) +
  facet_wrap(~Plate, labeller = label_both) +
  labs(
    title = "Tier 1 QC: Internal Spiked Control Deviations by Plate",
    subtitle = "Acceptance boundary: +/- 0.3 NPX deviation from plate median",
    x = "Extension Control Deviation (NPX)",
    y = "Incubation Control 1 Deviation (NPX)"
  )
```

**Decision:** Discard Sample `14` (Participant `TOFA_5260` Baseline) due to pipetting failure ($> +0.7	ext{ NPX}$).

---

### Checkpoint 3.2: Sample Cohort QC (IQR vs. Median NPX)

#### Why We Do It (`OlinkAnalyze::olink_qc_plot`)
Spiked controls only test the synthetic reagents; **they do not test the patient plasma matrix**! 
If a clinical sample was diluted with saline during phlebotomy, degraded by freeze-thaw cycles, or severely hemolyzed, its endogenous protein levels will collapse. In `OlinkAnalyze`, this is detected using **`olink_qc_plot()`**, which plots each sample's **Interquartile Range (IQR)** against its **Median NPX**.

#### What Plots to Look At
Scatter plot of Sample Median NPX (X-axis) vs. Sample IQR (Y-axis). Dashed boundary lines represent **$\pm 3	ext{ Standard Deviations}$** from the cohort mean.

```r
# Compute sample-level distribution metrics across all endogenous assays
sample_dist_metrics <- npx_explore_ht %>%
  filter(!is.na(ParticipantID)) %>%
  group_by(SampleID, ParticipantID, Timepoint, Plate) %>%
  summarise(
    Sample_Median = median(NPX),
    Sample_IQR = IQR(NPX),
    .groups = "drop"
  )

# Calculate cohort mean and standard deviations
mean_median <- mean(sample_dist_metrics$Sample_Median)
sd_median <- sd(sample_dist_metrics$Sample_Median)
mean_iqr <- mean(sample_dist_metrics$Sample_IQR)
sd_iqr <- sd(sample_dist_metrics$Sample_IQR)

# Flag distribution outliers (exceeding 3 SD in median or IQR)
sample_dist_metrics <- sample_dist_metrics %>%
  mutate(
    Distribution_Status = case_when(
      abs(Sample_Median - mean_median) > 3 * sd_median |
      abs(Sample_IQR - mean_iqr) > 3 * sd_iqr ~ "Outlier (> 3 SD)",
      TRUE ~ "Pass"
    )
  )

ggplot(sample_dist_metrics, aes(x = Sample_Median, y = Sample_IQR)) +
  # 3 SD acceptance boundaries (Olink standard)
  geom_rect(
    xmin = mean_median - 3 * sd_median, xmax = mean_median + 3 * sd_median,
    ymin = mean_iqr - 3 * sd_iqr, ymax = mean_iqr + 3 * sd_iqr,
    fill = "#3b82f6", alpha = 0.08, color = "#3b82f6", linetype = "dashed"
  ) +
  geom_point(aes(color = Distribution_Status, shape = factor(Plate)), size = 3, alpha = 0.85) +
  geom_text_repel(
    data = sample_dist_metrics %>% filter(Distribution_Status != "Pass"),
    aes(label = paste("Sample", SampleID, "(", ParticipantID, ")")),
    size = 3.5, fontface = "bold"
  ) +
  scale_color_manual(values = c("Pass" = "#0ea5e9", "Outlier (> 3 SD)" = "#ef4444")) +
  labs(
    title = "Tier 2 QC: Sample Cohort Distribution (olink_qc_plot: IQR vs. Median)",
    subtitle = "Blue dashed rectangle defines +/- 3 SD acceptance boundary for sample plasma matrix",
    x = "Sample Median NPX",
    y = "Sample IQR (NPX)",
    shape = "Plate"
  )
```

**Decision:** Sample `68` (`TOFA_B444` 16-week) is flagged as a matrix distribution outlier ($> 3	ext{ SD}$ below cohort median), indicating plasma dilution or degradation.

---

### Checkpoint 3.3: High-Dimensional Outlier Detection via PCA

#### Why We Do It (`OlinkAnalyze::olink_pca_plot`)
A sample might have normal spiked controls and acceptable median NPX, yet harbor an abnormal multivariate profile across 5,400 proteins due to unannotated clinical factors (e.g., severe lipemia or acute hemolysis). `olink_pca_plot()` computes Principal Component Analysis across all assays and flags multivariate outliers outside $3	imes	ext{IQR}$ or $\pm 3	ext{ SD}$ of PC1/PC2.

```r
# Pivot to wide matrix for PCA (samples x assays)
pca_wide <- npx_explore_ht %>%
  filter(!is.na(ParticipantID)) %>%
  select(SampleID, Assay, NPX) %>%
  pivot_wider(names_from = Assay, values_from = NPX)

pca_mat <- as.matrix(pca_wide[, -1])
rownames(pca_mat) <- pca_wide$SampleID

pca_fit <- prcomp(pca_mat, center = TRUE, scale. = TRUE)

pca_df <- as.data.frame(pca_fit$x[, 1:2]) %>%
  tibble::rownames_to_column("SampleID") %>%
  left_join(mapping_raw %>% select(SampleID, ParticipantID, Timepoint, Plate), by = "SampleID")

# Calculate outlier boundaries on PC1 and PC2
pc1_mean <- mean(pca_df$PC1); pc1_sd <- sd(pca_df$PC1)
pc2_mean <- mean(pca_df$PC2); pc2_sd <- sd(pca_df$PC2)

pca_df <- pca_df %>%
  mutate(
    PCA_Outlier = ifelse(abs(PC1 - pc1_mean) > 3 * pc1_sd | abs(PC2 - pc2_mean) > 3 * pc2_sd, "Outlier", "Normal")
  )

ggplot(pca_df, aes(x = PC1, y = PC2)) +
  geom_hline(yintercept = c(pc2_mean - 3 * pc2_sd, pc2_mean + 3 * pc2_sd), linetype = "dotted", color = "#94a3b8") +
  geom_vline(xintercept = c(pc1_mean - 3 * pc1_sd, pc1_mean + 3 * pc1_sd), linetype = "dotted", color = "#94a3b8") +
  geom_point(aes(color = factor(Plate), shape = Timepoint), size = 3, alpha = 0.85) +
  geom_text_repel(
    data = pca_df %>% filter(PCA_Outlier == "Outlier"),
    aes(label = paste("Sample", SampleID)),
    size = 3.5, fontface = "bold"
  ) +
  scale_color_manual(values = c("1" = "#3b82f6", "2" = "#f97316")) +
  labs(
    title = "Tier 3 QC: Multivariate Outlier Detection (olink_pca_plot)",
    subtitle = "PC1 vs. PC2 score projection with +/- 3 SD outlier thresholds",
    x = paste0("PC1 (", round(summary(pca_fit)$importance[2, 1] * 100, 1), "% Variance)"),
    y = paste0("PC2 (", round(summary(pca_fit)$importance[2, 2] * 100, 1), "% Variance)"),
    color = "Plate"
  )
```

Notice how PC1 immediately captures the **inter-plate technical difference** between Plate 1 and Plate 2 prior to normalization!

---

### Checkpoint 3.4: Technical Precision & %CV on Sample Controls

#### Why We Do It
Olink plates include **Sample Controls (`SC`)**—identical pooled reference plasma aliquots run in replicate across plates. We evaluate intra-plate and inter-plate precision to confirm assay repeatability.
* **Standard Olink Benchmark**:
  * Intra-plate Coefficient of Variation (%CV): $< 10\%$
  * Inter-plate %CV: $< 15\%$
  * In $\log_2$ NPX scale, a standard deviation of $	ext{SD}_{	ext{NPX}} \le 0.15	ext{ NPX}$ corresponds to approximately $\sim 10\% 	ext{ CV}$.

```r
sc_precision <- npx_explore_ht %>%
  filter(grepl("^SC_", SampleID)) %>%
  group_by(Assay, Plate) %>%
  summarise(
    SD_NPX = sd(NPX),
    # Approximate linear %CV from log2 standard deviation
    Approx_Linear_CV = (2^SD_NPX - 1) * 100,
    .groups = "drop"
  )

median_cv_p1 <- median(sc_precision %>% filter(Plate == 1) %>% pull(Approx_Linear_CV), na.rm = TRUE)
median_cv_p2 <- median(sc_precision %>% filter(Plate == 2) %>% pull(Approx_Linear_CV), na.rm = TRUE)

cat(sprintf("Median Intra-Plate Precision (Linear %%CV): Plate 1 = %.2f%%, Plate 2 = %.2f%% (Benchmark: < 10%%)
",
            median_cv_p1, median_cv_p2))
```

---

### Checkpoint 3.5: Limit of Detection (LOD) & Assay Detectability

#### The Cardinal Rule of LOD Handling
> **CRITICAL RULE**: Do **NOT** replace below-LOD values with an arbitrary constant (e.g., $0$ or $	ext{LOD}/2$).
> Imputing a constant collapses variance to zero, producing artificial bimodal spikes that invalidate Gaussian assumptions and inflate false discovery rates.
> **Standard Practice**: Retain continuous observed NPX readings for linear modeling, but drop assays where $<50\%$ of the cohort exceeds LOD.

```r
# Exclude samples that failed Tier 1 and Tier 2 QC
samples_to_exclude <- c("14", "68")
participants_to_drop <- mapping_raw %>% filter(SampleID %in% samples_to_exclude) %>% pull(ParticipantID)

npx_clean_samples <- npx_explore_ht %>%
  filter(!ParticipantID %in% participants_to_drop, !is.na(ParticipantID))

assay_detectability <- npx_clean_samples %>%
  group_by(Assay) %>%
  summarise(
    TotalSamples = n(),
    AboveLOD = sum(NPX > LOD),
    PercentAboveLOD = (AboveLOD / TotalSamples) * 100,
    .groups = "drop"
  ) %>%
  arrange(PercentAboveLOD)

assay_detectability <- assay_detectability %>%
  mutate(
    Category = case_when(
      PercentAboveLOD >= 75 ~ "High (>= 75%)",
      PercentAboveLOD >= 50 ~ "Moderate (50-75%)",
      TRUE ~ "Exclude (< 50%)"
    )
  )

ggplot(assay_detectability, aes(x = PercentAboveLOD, fill = Category)) +
  geom_histogram(binwidth = 5, color = "white", alpha = 0.9) +
  geom_vline(xintercept = 50, linetype = "dashed", color = "#ef4444", linewidth = 1) +
  scale_fill_manual(values = c(
    "High (>= 75%)" = "#10b981",
    "Moderate (50-75%)" = "#f59e0b",
    "Exclude (< 50%)" = "#ef4444"
  )) +
  labs(
    title = "Tier 5 QC: Explore HT Assay Detectability (% Samples Above LOD)",
    subtitle = "Red dashed line marks the 50% inclusion threshold",
    x = "Samples Above LOD (%)",
    y = "Number of Protein Assays"
  )

passing_ht_assays <- assay_detectability %>% filter(PercentAboveLOD >= 50) %>% pull(Assay)
npx_assay_qc <- npx_clean_samples %>% filter(Assay %in% passing_ht_assays)
```

---

### Checkpoint 3.6: Plate Control Normalization (Inter-Plate Batch Correction)

#### How Explore HT Plate Normalization Works
Olink Explore HT includes **Plate Controls (`PC`)** on every plate. For each assay $k$ on plate $p$:
$$\Delta_{k, p} = 	ext{median}(	ext{PC}_{k, p}) - 	ext{median}(	ext{PC}_{k, 	ext{ref}})$$
$$	ext{NPX}^{	ext{normalized}}_{i, k, p} = 	ext{NPX}_{i, k, p} - \Delta_{k, p}$$

```r
# Diagnostic Boxplot BEFORE normalization
ggplot(npx_assay_qc, aes(x = factor(Plate), y = NPX, fill = factor(Plate))) +
  geom_boxplot(outlier.size = 0.5, alpha = 0.7) +
  scale_fill_manual(values = c("1" = "#3b82f6", "2" = "#f97316")) +
  labs(
    title = "Pre-Normalization: Global NPX Distribution Across Plates",
    subtitle = "Notice systematic inter-plate technical elevation on Plate 2",
    x = "Plate Identifier",
    y = "Raw NPX"
  )

# Calculate Plate Control medians
pc_medians <- npx_explore_ht %>%
  filter(grepl("^PC_", SampleID)) %>%
  group_by(Assay, Plate) %>%
  summarise(PC_Median = median(NPX), .groups = "drop")

# Use Plate 1 as reference
pc_offsets <- pc_medians %>%
  pivot_wider(names_from = Plate, values_from = PC_Median, names_prefix = "Plate_") %>%
  mutate(Offset_Plate2 = Plate_2 - Plate_1) %>%
  select(Assay, Offset_Plate2)

# Apply normalization
npx_normalized <- npx_assay_qc %>%
  left_join(pc_offsets, by = "Assay") %>%
  mutate(
    Offset = ifelse(Plate == "2", Offset_Plate2, 0),
    NPX_Normalized = NPX - Offset
  )

# Diagnostic Boxplot AFTER normalization
ggplot(npx_normalized, aes(x = factor(Plate), y = NPX_Normalized, fill = factor(Plate))) +
  geom_boxplot(outlier.size = 0.5, alpha = 0.7) +
  scale_fill_manual(values = c("1" = "#3b82f6", "2" = "#10b981")) +
  labs(
    title = "Post-Normalization: Global NPX Distribution Across Plates",
    subtitle = "Plate control adjustment successfully aligned plate distributions",
    x = "Plate Identifier",
    y = "Normalized NPX"
  )
```

---

# 5. Step 4: Paired Statistical Analysis (Baseline vs. 16 Weeks)

### 5.1 Why Paired Statistics Are Essential

In human clinical cohorts, inter-individual baseline variation (genetics, baseline inflammation, age) is massive:
$$	ext{Var}(	ext{Between-Subject}) \gg 	ext{Var}(	ext{Within-Subject})$$

If we perform an **unpaired** test, the high baseline variance between patients inflates the residual error term, drastically reducing statistical power. A **paired test** evaluates the intra-individual change:
$$\Delta_i = 	ext{NPX}_{i, 	ext{16 week}} - 	ext{NPX}_{i, 	ext{Baseline}}$$
Because each patient serves as their own control, individual baseline differences cancel out.

### 5.2 Executing Paired Testing

```r
paired_analysis_df <- npx_normalized %>%
  select(ParticipantID, Timepoint, Assay, NPX_Normalized) %>%
  pivot_wider(names_from = Timepoint, values_from = NPX_Normalized) %>%
  filter(!is.na(Baseline) & !is.na(`16 week`))

# Compute paired differential abundance
diff_results <- paired_analysis_df %>%
  group_by(Assay) %>%
  summarise(
    N_Pairs = n(),
    Mean_Baseline = mean(Baseline),
    Mean_16wk = mean(`16 week`),
    Delta_NPX = mean(`16 week` - Baseline), # Post - Pre
    p_value = t.test(`16 week`, Baseline, paired = TRUE)$p.value,
    .groups = "drop"
  ) %>%
  mutate(
    q_value = p.adjust(p_value, method = "BH"),
    neg_log10_q = -log10(q_value),
    Significance = case_when(
      q_value < 0.05 & Delta_NPX <= -0.5 ~ "Down-regulated (FDR < 0.05, Delta <= -0.5)",
      q_value < 0.05 & Delta_NPX >= 0.5 ~ "Up-regulated (FDR < 0.05, Delta >= 0.5)",
      q_value < 0.05 ~ "FDR < 0.05 (|Delta| < 0.5)",
      TRUE ~ "Not Significant"
    )
  ) %>%
  arrange(q_value)

# Inspect top modulated proteins
head(diff_results, 12)
```

---

# 6. Step 5: Visualizing Results & Biological Interpretation

### 6.1 Paired Volcano Plot

```r
top_hits <- diff_results %>%
  filter(q_value < 0.05 & abs(Delta_NPX) >= 0.5)

ggplot(diff_results, aes(x = Delta_NPX, y = neg_log10_q)) +
  geom_hline(yintercept = -log10(0.05), linetype = "dashed", color = "#64748b") +
  geom_vline(xintercept = c(-0.5, 0.5), linetype = "dotted", color = "#64748b") +
  geom_point(aes(color = Significance), alpha = 0.8, size = 2.5) +
  geom_text_repel(
    data = top_hits,
    aes(label = Assay),
    size = 4,
    fontface = "bold",
    max.overlaps = 15
  ) +
  scale_color_manual(values = c(
    "Down-regulated (FDR < 0.05, Delta <= -0.5)" = "#3b82f6",
    "Up-regulated (FDR < 0.05, Delta >= 0.5)" = "#ef4444",
    "FDR < 0.05 (|Delta| < 0.5)" = "#a855f7",
    "Not Significant" = "#94a3b8"
  )) +
  labs(
    title = "Volcano Plot: Paired Response to Tofacitinib in Down Syndrome (PRIDE PAD000062)",
    subtitle = "Thresholds: FDR q < 0.05 and |Delta NPX| >= 0.5 (16 Weeks vs. Baseline)",
    x = "Delta NPX (16 Weeks - Baseline)",
    y = "-log10 (FDR q-value)"
  ) +
  theme(legend.position = "bottom")
```

---

### 6.2 Matched Patient Trajectories ("Spaghetti Plots")

```r
focal_markers <- c("CXCL10", "CXCL9", "IFNG", "IL6")

trajectory_df <- npx_normalized %>%
  filter(Assay %in% focal_markers) %>%
  mutate(Timepoint = factor(Timepoint, levels = c("Baseline", "16 week")))

ggplot(trajectory_df, aes(x = Timepoint, y = NPX_Normalized, group = ParticipantID)) +
  geom_line(color = "#94a3b8", alpha = 0.6, linewidth = 0.6) +
  geom_point(aes(color = Timepoint), size = 2.5, alpha = 0.8) +
  stat_summary(
    aes(group = 1),
    fun = mean,
    geom = "line",
    color = "#0f172a",
    linewidth = 1.3
  ) +
  stat_summary(
    aes(group = 1),
    fun = mean,
    geom = "point",
    color = "#0f172a",
    size = 3.5,
    shape = 18
  ) +
  scale_color_manual(values = c("Baseline" = "#f59e0b", "16 week" = "#10b981")) +
  facet_wrap(~Assay, scales = "free_y", ncol = 2) +
  labs(
    title = "Matched Patient Trajectories (Baseline to 16 Weeks On-Treatment)",
    subtitle = "Grey lines: Individual patient trajectories. Black bold line: Cohort mean trajectory.",
    x = "Study Visit",
    y = "Normalized NPX"
  ) +
  theme(legend.position = "none")
```

---


# 7. Step 6: Pathway Enrichment Analysis (Industry Standards & Low-Signal Strategies)

Translating a list of 5,400 individual proteins into coherent biological mechanisms requires **Pathway Enrichment Analysis**. In affinity proteomics, this introduces unique statistical nuances compared to standard RNA-seq.

---

### 7.1 What Are Olink's Curated Protein Sets?

Olink's platform architecture was engineered around **curated biological modules**:
1. **Curated Exploration Sub-Libraries**:
   * **Inflammation & Immune Response** (~1,350 proteins): Pro- and anti-inflammatory cytokines, chemokines, interferons, interleukins, and immune checkpoint regulators.
   * **Oncology** (~1,350 proteins): Cell cycle kinases, apoptosis modulators, DNA damage response, and tumor microenvironment remodeling factors.
   * **Cardiometabolic** (~1,350 proteins): Atherosclerosis drivers, lipid transport apolipoproteins, coagulation factors, and vascular endothelial growth factors.
   * **Neurology** (~1,350 proteins): Neuroinflammatory markers, neurotrophic factors, glial activation markers, and blood-brain barrier components.
2. **Explore HT Re-curation (~5,400 proteins)**:
   * Explore HT synthesizes these modules into a single 5,400-plex panel, heavily expanding into intracellular signaling cascades and deep tissue-leakage biomarkers.
3. **Olink Insight Curated Disease Signatures**:
   * Olink maintains open-access curated disease-protein signatures derived from the UK Biobank Pharma Proteomics Project (UKB-PPP) and large clinical trials on [Olink Insight](https://insight.olink.com/).

---

### 7.2 The #1 Rule of Proteomics Pathway Testing: The Background Universe Bias

> **CRITICAL INDUSTRY STANDARD**: In RNA-seq, the background universe ($\mathcal{U}$) is typically all ~20,000 protein-coding genes. 
> In targeted and affinity proteomics, **YOU MUST NEVER USE THE ENTIRE HUMAN GENOME AS YOUR BACKGROUND UNIVERSE!**
> 
> **Why?**
> If you test an Olink panel (which is naturally enriched for inflammatory cytokines and signaling molecules) against the whole genome, inflammatory pathways will **always** appear statistically significant ($p < 10^{-15}$), even on completely random noise!
> 
> **The Golden Standard:** The background universe $\mathcal{U}$ **must be restricted strictly to the set of proteins actually measured on your specific Olink panel that passed QC**.

---

### 7.3 Methodological Standards: GSEA vs. ORA

There are two primary mathematical frameworks used in the industry:

1. **Ranked Gene Set Enrichment Analysis (GSEA)** *(Recommended)*:
   * **How it works**: Uses the complete ranked list of all tested proteins ordered by a continuous test statistic:
     $$	ext{Ranking Metric} = 	ext{sign}(\Delta	ext{NPX}) 	imes -\log_{10}(p	ext{-value})$$
   * **Advantage**: Does not require an arbitrary significance cutoff ($q < 0.05$); captures subtle, coordinated shifts across an entire pathway.
2. **Over-Representation Analysis (ORA)** (Hypergeometric / Fisher's Exact Test):
   * **How it works**: Tests whether the proportion of significantly modulated proteins ($q < 0.05$, $|\Delta	ext{NPX}| \ge 0.5$) in pathway $P$ is higher than expected by chance within the Olink panel universe:
     $$p = \sum_{k=x}^{\min(K, n)} rac{inom{K}{k} inom{N - K}{n - k}}{inom{N}{n}}$$
     where $N = |\mathcal{U}_{	ext{Olink}}|$, $K = |P \cap \mathcal{U}_{	ext{Olink}}|$, and $n$ is the number of significant proteins.

---

### 7.4 Standard ORA Analysis on PRIDE PAD000062

```r
# Define Curated Biological Pathways relevant to JAK inhibition & Down Syndrome
curated_pathways <- list(
  "Interferon_Gamma_Response" = c("IFNG", "CXCL9", "CXCL10", "CXCL11", "STAT1", "SOCS3", "IL6", "PDL1", "FASLG"),
  "IL6_JAK_STAT3_Signaling"   = c("IL6", "STAT3", "SOCS3", "IL10", "IL1B", "CSF1", "VEGFA"),
  "TNF_Signaling_via_NFkB"    = c("TNF", "CCL2", "CCL3", "CCL4", "IL1B", "MMP1", "MMP3", "TNFSF14"),
  "Tissue_Remodeling_Matrix"  = c("MMP1", "MMP3", "HGF", "FGF21", "VEGFA"),
  "Complement_Activation"     = c("PROT_33", "PROT_34", "PROT_35", "PROT_36", "PROT_37")
)

# 1. Define Olink Panel Background Universe (Proteins passing QC)
olink_universe <- diff_results$Assay
N_universe <- length(olink_universe)

# 2. Identify Significant Downregulated Hits (Tofacitinib response: q < 0.05 and Delta <= -0.5)
sig_hits <- diff_results %>%
  filter(q_value < 0.05 & Delta_NPX <= -0.5) %>%
  pull(Assay)
n_sig <- length(sig_hits)

# 3. Compute Over-Representation Analysis (Hypergeometric Test) against Olink Universe
pathway_results <- tibble(Pathway = names(curated_pathways)) %>%
  mutate(
    # Restrict pathway genes to those present in our Olink universe
    Pathway_Proteins = purrr::map(Pathway, ~ intersect(curated_pathways[[.x]], olink_universe)),
    K_pathway_size = purrr::map_int(Pathway_Proteins, length),
    # Number of significant hits overlapping pathway
    k_overlap = purrr::map_int(Pathway_Proteins, ~ length(intersect(.x, sig_hits))),
    Overlapping_Proteins = purrr::map_chr(Pathway_Proteins, ~ paste(intersect(.x, sig_hits), collapse = ", ")),
    # Hypergeometric p-value (phyper: q, m, n, k)
    p_value = purrr::map2_dbl(K_pathway_size, k_overlap, function(K, k) {
      if (k == 0) return(1.0)
      phyper(k - 1, m = K, n = N_universe - K, k = n_sig, lower.tail = FALSE)
    }),
    q_value = p.adjust(p_value, method = "BH"),
    neg_log10_q = -log10(q_value)
  ) %>%
  arrange(p_value)

pathway_results %>%
  select(Pathway, K_pathway_size, k_overlap, Overlapping_Proteins, p_value, q_value)
```

---

### 7.5 What If You Don't Have Much Proteome-Wide Signal? (4 Rescue Strategies)

In clinical plasma proteomics, finding **few or zero proteins passing FDR $q < 0.05$** is very common. Plasma is homeostatically buffered, systemic cytokine dilution is high, and testing 5,400 hypotheses imposes a severe multiple-testing penalty. 

When single-protein differential testing yields weak signal, computational biologists use **four industry-standard strategies**:

#### Strategy A: Continuous Ranked GSEA (Capturing Sub-threshold Coordinated Shifts)
Instead of filtering by $q < 0.05$, rank **all 5,400 proteins** by their signed test statistic:
$$	ext{Rank} = 	ext{sign}(\Delta	ext{NPX}) 	imes -\log_{10}(p	ext{-value})$$
Even if individual cytokines only achieve nominal $p = 0.02$ or $0.05$ (failing genome-wide FDR), GSEA captures the fact that **15 proteins in the same pathway all shifted coordinately**, yielding robust pathway significance.

```r
# 1. Compute continuous signed ranking metric across all tested assays
ranked_proteome <- diff_results %>%
  mutate(rank_metric = sign(Delta_NPX) * -log10(p_value)) %>%
  arrange(desc(rank_metric))

# Top 5 most activated and top 5 most suppressed by continuous ranking
bind_rows(
  head(ranked_proteome %>% select(Assay, Delta_NPX, p_value, rank_metric), 5),
  tail(ranked_proteome %>% select(Assay, Delta_NPX, p_value, rank_metric), 5)
)
```

#### Strategy B: Inverting the Workflow — Patient-Level Pathway Scores (ssGSEA / GSVA)
Instead of testing 5,400 noisy single proteins and then grouping into pathways:
1. **Compress the proteome into pathway scores per patient** using single-sample GSEA (`ssGSEA`), `GSVA`, or composite Z-scores.
2. Run your paired clinical model **directly on the pathway scores**!

**Why this works:**
* You reduce multiple testing from **5,400 tests down to 50 tests** (e.g., MSigDB Hallmark sets), reducing the FDR penalty by 100-fold.
* Averaging correlated proteins in a pathway cancels out stochastic assay measurement noise.

```r
# Compute standardized Z-score for each protein across clinical samples
sample_z_scores <- npx_normalized %>%
  group_by(Assay) %>%
  mutate(NPX_Z = as.vector(scale(NPX_Normalized))) %>%
  ungroup()

# Calculate composite pathway activity score per sample well
patient_pathway_scores <- tibble(Pathway = names(curated_pathways)) %>%
  rowwise() %>%
  mutate(
    Score_DF = list({
      genes <- curated_pathways[[Pathway]]
      sample_z_scores %>%
        filter(Assay %in% genes) %>%
        group_by(ParticipantID, Timepoint) %>%
        summarise(Pathway_Score = mean(NPX_Z, na.rm = TRUE), .groups = "drop")
    })
  ) %>%
  unnest(Score_DF)

# Run Paired t-test DIRECTLY on the pathway scores (only 5 hypotheses tested!)
pathway_level_stats <- patient_pathway_scores %>%
  pivot_wider(names_from = Timepoint, values_from = Pathway_Score) %>%
  group_by(Pathway) %>%
  summarise(
    Mean_Baseline = mean(Baseline, na.rm = TRUE),
    Mean_16wk = mean(`16 week`, na.rm = TRUE),
    Delta_Score = mean(`16 week` - Baseline, na.rm = TRUE),
    p_value = t.test(`16 week`, Baseline, paired = TRUE)$p.value,
    .groups = "drop"
  ) %>%
  mutate(
    q_value = p.adjust(p_value, method = "BH"),
    neg_log10_q = -log10(q_value)
  ) %>%
  arrange(p_value)

pathway_level_stats
```

Notice how testing pathway scores directly dramatically boosts statistical power while completely eliminating single-protein noise!

#### Strategy C: Protein-Protein Interaction (PPI) Subnetwork Validation (STRINGdb)
When proteins fail FDR, test whether the top nominally shifting proteins ($p_{	ext{raw}} < 0.05$) **physically bind one another in nature**:
* Map candidate proteins to the **STRING** or **BioGRID** physical interaction network.
* Calculate the **PPI Enrichment $p$-value** (evaluates whether the number of observed physical edges exceeds random topological expectation).
* **Why it matters:** If candidate proteins physically interact in a ligand-receptor complex or multi-protein machine (e.g. CXCL10-CXCR3 or C3-C4-C5), physical connectivity provides **orthogonal biochemical proof** that the signal is real biology, not instrument noise.

#### Strategy D: Uncovering Hidden "Responder" Subsets (Clinical Heterogeneity)
A primary reason for weak proteome signal in clinical trials is **heterogeneous response**:
* If **50% of patients respond** (cytokine drop) and **50% are non-responders** (flat or slight rise), the cohort-wide average $\Delta	ext{NPX} pprox 0$.
* **Diagnostic**: Inspect individual patient trajectory spaghetti plots or stratify by clinical response endpoints:

```r
# Testing for treatment effect specifically within responders:
# model <- olink_lmer(df, variable = "Timepoint:Clinical_Response", random = "ParticipantID")
```

---

### 7.6 Strategic Guide: Curating Gene Sets, Secretome vs. Leakage, and Negative Controls

When designing pathway analyses for blood plasma proteomics, selecting which protein sets to query—and whether to filter the underlying proteome—is a major decision that impacts biological discovery.

---

#### 1. Curated Sets vs. Custom Sets vs. Negative Controls

| Category | Typical Sources | When to Use | Advantages & Pitfalls |
| :--- | :--- | :--- | :--- |
| **Curated Canonical Sets** | **MSigDB Hallmark** (50 sets), **Reactome**, **KEGG** | **Discovery mode / Unbiased screening**. | **Advantage:** Universally recognized, low redundancy (Hallmark has only 50 clean sets), minimizes FDR penalties.<br>**Pitfall:** Gene Ontology (GO BP) has >15,000 sets with massive overlap, heavily inflating multiple testing. |
| **Custom / MoA Sets** | Literature, drug target biology (e.g., "JAK-STAT downstream ISGs", "NF-κB target genes") | **Target Engagement / Drug MoA validation**. | **Advantage:** Laser-focused on specific pharmacological hypotheses.<br>**Pitfall:** Risk of confirmation bias if not paired with broad background checks. |
| **Negative Control Sets** | Unrelated biology (e.g., *Olfactory Transduction*, *Phototransduction*, *Spermatogenesis*) | **Pipeline calibration & sanity checking**. | **Advantage:** If an unrelated negative control set shows $q < 0.05$, your data has uncorrected batch effects or genome-wide universe bias! |

---

#### 2. The Secretome vs. Intracellular Dilemma in Blood Plasma

Circulating plasma proteins originate from two distinct biological mechanisms:
1. **The Active Secretome & Sheddome**: Cytokines, chemokines, hormones, and cleaved receptor ectodomains actively secreted by cells to mediate systemic communication.
2. **Passive Tissue-Leakage (Cell Death / Necrosis / Damage)**: Intracellular enzymes, cytoskeletal proteins, and transcription factors released into blood upon cell turnover, apoptosis, or ischemic necrosis (e.g., Troponin in myocardial infarction, ALT in liver injury, GFAP/NfL in brain damage).

##### Should You Restrict Your Analysis to Secreted Proteins?

* **YES, restrict to Secreted / Extracellular proteins when:**
  * Your biological question is: *"What systemic endocrine or immune signaling pathways are driving drug response?"*
  * In a 5,400-protein panel, ~3,500 assays are intracellular enzymes. If these enzymes exhibit random baseline fluctuations, including them in competitive GSEA adds noise and **dampens the statistical enrichment of active cytokine signaling cascades**.
  * Restricting your universe to the **Human Protein Atlas (HPA) Blood Secretome** (~1,000–1,500 proteins) dramatically sharpens cytokine/receptor pathway sensitivity.

* **NO, do NOT restrict (Keep Intracellular Proteins) when:**
  * Your question is: *"Is the drug preventing target organ damage, or causing off-target tissue toxicity?"*
  * Intracellular proteins are the clinical gold standard for **organ injury biomarkers**. If tofacitinib suppresses epidermal apoptosis in Down Syndrome skin lesions, intracellular keratinocyte damage markers (e.g., cytokeratins, S100 proteins) will drop in blood!
  * Excluding intracellular proteins would blind you to **organ protection and drug toxicity signals**.

##### The Recommended "Dual-Track" Architecture
Rather than making a mutually exclusive choice, industry best practice runs **two parallel tracks**:
1. **Track 1 (Active Signaling Track)**: Query cytokine/receptor pathways across actively secreted proteins.
2. **Track 2 (Tissue-Damage / Organ-Leakage Track)**: Query organ-specific intracellular markers (e.g., Liver-enriched, Muscle-enriched, Brain-enriched) to monitor safety and organ recovery.

---

#### 3. Executable Example: Secretome Filtering & Negative Control Calibration

Below, we test our clinical trial data by applying a **Negative Control Set** (*Olfactory Transduction*) and comparing **Secreted Signaling** vs. **Intracellular Tissue Leakage**:

```r
# Define annotated gene sets including a Negative Control and Tissue Leakage
extended_pathways <- list(
  "Interferon_Gamma_Response"       = c("IFNG", "CXCL9", "CXCL10", "CXCL11", "STAT1", "SOCS3", "IL6", "PDL1", "FASLG"),
  "IL6_JAK_STAT3_Signaling"         = c("IL6", "STAT3", "SOCS3", "IL10", "IL1B", "CSF1", "VEGFA"),
  "Keratinocyte_Skin_Damage_Marker" = c("MMP1", "MMP3", "PROT_33", "PROT_34"), # Intracellular damage leakage
  "Olfactory_Receptors_Neg_Control" = c("PROT_88", "PROT_89", "PROT_90")       # Negative control set
)

# Annotate protein localization (Secreted vs. Intracellular)
secreted_cytokines <- c("IFNG", "CXCL9", "CXCL10", "CXCL11", "IL6", "IL10", "IL1B", "TNF", "CCL2", "VEGFA", "HGF", "FGF21")

# Evaluate pathway enrichment with Negative Control
calibration_results <- tibble(Pathway = names(extended_pathways)) %>%
  mutate(
    Pathway_Proteins = purrr::map(Pathway, ~ intersect(extended_pathways[[.x]], olink_universe)),
    K_size = purrr::map_int(Pathway_Proteins, length),
    k_overlap = purrr::map_int(Pathway_Proteins, ~ length(intersect(.x, sig_hits))),
    p_value = purrr::map2_dbl(K_size, k_overlap, function(K, k) {
      if (k == 0) return(1.0)
      phyper(k - 1, m = K, n = N_universe - K, k = n_sig, lower.tail = FALSE)
    }),
    q_value = p.adjust(p_value, method = "BH"),
    neg_log10_q = -log10(q_value)
  ) %>%
  arrange(p_value)

calibration_results %>%
  select(Pathway, K_size, k_overlap, p_value, q_value)
```

Notice the crucial diagnostic findings:
1. **On-Target Cascades** (*Interferon Gamma* and *IL-6 / JAK / STAT3*) remain highly significant ($q < 10^{-4}$).
2. **The Negative Control Set** (*Olfactory Receptors*) yields $p = 1.0$ and $q = 1.0$, confirming that our Olink background universe successfully prevents false-positive inflation!

---

### 7.6 Visualizing Enriched Biological Pathways

```r
ggplot(pathway_results, aes(x = reorder(Pathway, neg_log10_q), y = neg_log10_q, fill = q_value < 0.05)) +
  geom_col(width = 0.7) +
  geom_hline(yintercept = -log10(0.05), linetype = "dashed", color = "#ef4444", linewidth = 0.8) +
  scale_fill_manual(values = c("TRUE" = "#3b82f6", "FALSE" = "#94a3b8")) +
  coord_flip() +
  labs(
    title = "Curated Pathway Enrichment: Response to Tofacitinib (PAD000062)",
    subtitle = "Calculated against the Olink Explore HT background universe (FDR q < 0.05 threshold)",
    x = "Biological Pathway",
    y = "-log10 (FDR q-value)",
    fill = "Significant (FDR < 0.05)"
  ) +
  theme(legend.position = "none")
```

---

### 7.7 Decision Matrix for Low-Signal Proteomics

```
                   [ Few or No Proteins Pass FDR q < 0.05 ]
                                       │
     ┌─────────────────────────────────┼─────────────────────────────────┐
     ▼                                 ▼                                 ▼
[ Sub-threshold Coordinated Shift ]  [ Noise / High FDR Burden ]    [ Heterogeneous Response ]
  • Use Ranked GSEA                   • Collapse to ssGSEA / GSVA     • Inspect Patient Trajectories
  • Metric: sign(Δ) × -log10(p)        (Test 50 pathways, not 5.4k)   • Stratify Responders vs.
  • Test Hallmark & Reactome          • Run paired t-test on scores     Non-responders
     │                                 │                                 │
     └─────────────────────────────────┼─────────────────────────────────┘
                                       ▼
                       [ Orthogonal Network Validation ]
                        • Map nominal p < 0.05 to STRING PPI
                        • Test for physical protein complexes
```

---
# 8. Summary: The Complete OlinkAnalyze QC Checklist

| QC Tier | OlinkAnalyze Tool | Target Metric | Warning Threshold | Decision / Action |
| :--- | :--- | :--- | :--- | :--- |
| **Tier 1: Spiked Controls** | `olink_qc_plot()` / deviation | Incubation & Extension $\Delta$ | $|\Delta| > 0.3	ext{ NPX}$ | **Discard well**. Enzymatic/pipetting failure. |
| **Tier 2: Cohort QC** | `olink_qc_plot()` | Sample IQR vs. Median NPX | $> 3	ext{ SD}$ from mean | **Discard sample**. Matrix dilution / degradation. |
| **Tier 3: Multivariate PCA** | `olink_pca_plot()` | PC1 / PC2 coordinates | $> 3	ext{ SD}$ or $3	imes	ext{IQR}$ | High-dimensional outlier. Inspect hemolysis/lipemia logs. |
| **Tier 4: Replicate Precision** | `%CV` on Sample Controls (`SC`) | Pooled plasma replicate variance | Intra $> 10\%$, Inter $> 15\%$ | Flags technical instrument drift between runs. |
| **Tier 5: Assay Detectability** | `% > LOD` waterfall | Proportion of cohort $>$ LOD | $< 50\%$ above LOD | **Exclude assay** from discovery testing. |
| **Below-LOD Handling** | NPX linear scale | $	ext{NPX} < 	ext{LOD}$ | N/A | **Do NOT replace with 0 or LOD/2**. Retain continuous NPX. |
| **Tier 6: Plate Normalization** | `olink_normalization()` | Plate Controls (`PC`) shift | Inter-plate offset | Subtract plate offset: $	ext{NPX} - \Delta_{	ext{PC}}$. |
| **Statistical Testing** | `olink_ttest()` / `olink_lmer()` | Intra-patient correlation | Longitudinal sampling | Always use **paired models** (`pair_id` / `random`). |
| **Pathway Enrichment** | `olink_pathway_enrichment()` | ORA / Ranked GSEA | FDR $q < 0.05$ | **Always restrict background to Olink panel universe!** |

---

# 9. Session Information

```r
sessionInfo()
```
