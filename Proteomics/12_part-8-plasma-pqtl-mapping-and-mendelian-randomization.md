# How to Analyze Proteomics Data Part 8: Population Plasma pQTL Mapping & Mendelian Randomization

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, pQTL, Genetics, Mendelian Randomization, Drug Target Discovery, UK Biobank

---

## Table of Contents

- [Introduction](#introduction)
- [What is a pQTL?](#what-is-a-pqtl)
  - [The Central Dogma Meets Human Genetics](#the-central-dogma-meets-human-genetics)
  - [Why Pharma Cares: Genetically Validated Drug Targets](#why-pharma-cares-genetically-validated-drug-targets)
- [Cis-pQTL vs. Trans-pQTL: Two Different Genetic Architectures](#cis-pqtl-vs-trans-pqtl-two-different-genetic-architectures)
  - [Cis-pQTLs: Local Regulatory Variants](#cis-pqtls-local-regulatory-variants)
  - [Trans-pQTLs: Distant Network Mediators](#trans-pqtls-distant-network-mediators)
  - [The Cis-Epitope Artifact Check](#the-cis-epitope-artifact-check)
- [Step 1: Running pQTL Association Testing in R](#step-1-running-pqtl-association-testing-in-r)
  - [Linear Regression Model Formulation](#linear-regression-model-formulation)
  - [Testing SNP-Protein Association](#testing-snp-protein-association)
  - [Visualizing pQTLs with Regional Locus Plots](#visualizing-pqtls-with-regional-locus-plots)
- [Step 2: Causal Inference via Mendelian Randomization (MR)](#step-2-causal-inference-via-mendelian-randomization-mr)
  - [Nature’s Randomized Controlled Trial](#natures-randomized-controlled-trial)
  - [The Three Core Instrumental Variable Assumptions](#the-three-core-instrumental-variable-assumptions)
  - [Running Two-Sample MR in R with TwoSampleMR](#running-two-sample-mr-in-r-with-twosamplemr)
- [Step 3: Sensitivity Analyses: Testing for Horizontal Pleiotropy](#step-3-sensitivity-analyses-testing-for-horizontal-pleiotropy)
  - [MR-Egger Intercept Test](#mr-egger-intercept-test)
  - [Weighted Median Estimator](#weighted-median-estimator)
- [Real-World Case Study: PCSK9, LDL-C, and Cardiovascular Disease](#real-world-case-study-pcsk9-ldl-c-and-cardiovascular-disease)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In the history of drug development, **over 90% of clinical-stage therapeutics fail in clinical trials**, primarily due to lack of efficacy. However, retrospective analyses by AstraZeneca and GSK (Nelson et al., *Nature Genetics* 2015; King et al., *PLOS Genetics* 2019) revealed a game-changing insight:

> **Drug targets supported by human genetic evidence are MORE THAN TWICE AS LIKELY to achieve FDA approval.**

To discover genetically validated drug targets, researchers combine genomics (genome-wide association studies, GWAS) with high-throughput proteomics (Olink and SomaScan). 

When a genetic variant (SNP) associates with the abundance of a protein, that locus is called a **Protein Quantitative Trait Locus (pQTL)**.

By leveraging massive population cohorts such as the **UK Biobank Pharma Proteomics Project (UKB-PPP)** (>54,000 participants) and deCODE genetics, we can map pQTLs across the entire human proteome. In this tutorial, we learn how to test for pQTLs, distinguish *cis* from *trans* regulatory effects, and use **Mendelian Randomization (MR)** to determine whether a circulating protein causally drives human disease.

---

## What is a pQTL?

### The Central Dogma Meets Human Genetics

A **pQTL** is a genomic variant (typically a single nucleotide polymorphism, SNP) that is statistically associated with the concentration of a specific protein in a tissue or biofluid.

```
 [ DNA VARIANT ] (SNP: rs11591147)
        │
        ▼  Alters transcription, mRNA stability, or translation
 [ PROTEIN LEVEL ] (Plasma PCSK9 concentration)
        │
        ▼  Causally modulates biomarker / disease pathophysiology
 [ CLINICAL OUTCOME ] (Circulating LDL Cholesterol & Myocardial Infarction)
```

### Why Pharma Cares: Genetically Validated Drug Targets
If naturally occurring human genetic variants that lower protein $X$ also lower disease risk (without causing toxic side effects), an antibody or small molecule that inhibits protein $X$ is overwhelmingly likely to succeed in Phase 3 clinical trials.

---

## Cis-pQTL vs. Trans-pQTL: Two Different Genetic Architectures

Not all genetic associations are created equal. In genome-wide pQTL scans, associations fall into two classes:

```
                            TYPES OF pQTLs
                                  │
       ┌──────────────────────────┴──────────────────────────┐
       ▼                                                     ▼
 [ CIS-pQTL ]                                          [ TRANS-pQTL ]
• Located within ±500 kb to 1 Mb                      • Located on a different chromosome
  of the protein-coding gene                           or >1 Mb away
• Directly regulates target gene                      • Operates via upstream pathways:
  (Promoter, Enhancer, mRNA stability)                  - Transcription factor binding
• Ideal instruments for drug target                     - Clearing receptor in liver
  Mendelian Randomization                               - Degradation machinery
```

### Cis-pQTLs: Local Regulatory Variants
A *cis*-pQTL lies physically close to the gene encoding the protein (conventionally within $\pm 500\text{ kb}$ to $1\text{ Mb}$ of the transcription start site). 
* *Mechanism*: Enhancer or promoter variants altering transcriptional initiation, or 3' UTR variants altering microRNA binding.
* *Drug Target Utility*: *Cis*-pQTLs are considered the **gold standard instrumental variables** for Mendelian Randomization because their effect is localized directly to the target gene, minimizing off-target (pleiotropic) confounding.

### Trans-pQTLs: Distant Network Mediators
A *trans*-pQTL lies far away from the target gene (frequently on another chromosome).
* *Mechanism*: A variant in a cell-surface receptor (e.g., *ASGR1* in the liver) that regulates the clearance and uptake of dozens of different circulating glycoproteins.
* *Drug Target Utility*: Useful for mapping biological networks, but risky for Mendelian Randomization because altering the master regulator affects multiple pathways simultaneously (**horizontal pleiotropy**).

### The Cis-Epitope Artifact Check
As discussed in [Part 7](11_part-7-cross-platform-discordance-ms-vs-olink-vs-somascan.md), always verify whether a strong *cis*-pQTL is a **missense variant** that alters the physical epitope binding pocket rather than true protein abundance.

---

## Step 1: Running pQTL Association Testing in R

### Linear Regression Model Formulation

To test whether a genetic variant associates with protein abundance, we fit a multivariable linear model:
$$\text{Protein NPX}_i = \beta_0 + \beta_{\text{SNP}} \cdot \text{Genotype}_i + \beta_1 \cdot \text{Age}_i + \beta_2 \cdot \text{Sex}_i + \sum_{k=1}^{5} \gamma_k \cdot \text{PC}_{ik} + \epsilon_i$$
Where:
- $\text{Genotype}_i$ is the minor allele dosage ($0, 1, 2$).
- $\text{PC}_{1\dots 5}$ are genomic principal components correcting for population ancestry stratification.

---

### Testing SNP-Protein Association

```r
library(tidyverse)

# Simulate pQTL dataset for 1,000 individuals
set.seed(42)
n <- 1000

# Genotype dosage (0, 1, 2) with Minor Allele Frequency = 0.25
dosage <- rbinom(n, size = 2, prob = 0.25)
age    <- rnorm(n, mean = 58, sd = 8)
sex    <- sample(c(0, 1), n, replace = TRUE) # 0 = Female, 1 = Male

# Simulate protein level influenced by genotype (beta = -0.45 NPX per allele)
pcsk9_npx <- 5.2 - 0.45 * dosage + 0.02 * age + 0.15 * sex + rnorm(n, sd = 0.7)

pqtl_data <- data.frame(PCSK9_NPX = pcsk9_npx, SNP_Dosage = dosage, Age = age, Sex = sex)

# Fit linear regression model
pqtl_model <- lm(PCSK9_NPX ~ SNP_Dosage + Age + Sex, data = pqtl_data)
summary(pqtl_model)
```

```text
Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)  5.18420    0.16240  31.922  < 2e-16 ***
SNP_Dosage  -0.44812    0.03412 -13.134  < 2e-16 ***
Age          0.01984    0.00271   7.321 5.12e-13 ***
Sex          0.16104    0.04392   3.666 0.00026 ** 
```

The SNP demonstrates an exceptionally strong association ($\beta = -0.45\text{ NPX}$ per minor allele, $p < 2 \times 10^{-16}$), establishing a robust *cis*-pQTL.

---

### Visualizing pQTLs with Regional Locus Plots

```r
# Boxplot of Protein NPX by Genotype Dosage
p_pqtl <- ggplot(pqtl_data, aes(x = factor(SNP_Dosage), y = PCSK9_NPX, fill = factor(SNP_Dosage))) +
  geom_boxplot(width = 0.5, alpha = 0.8, outlier.size = 0.5) +
  geom_jitter(width = 0.15, alpha = 0.2, size = 1) +
  scale_fill_brewer(palette = "Blues") +
  theme_bw(base_size = 12) +
  labs(
    title = "cis-pQTL: rs11591147 Decreases Plasma PCSK9",
    subtitle = "Gene dosage effect: Each minor allele reduces PCSK9 by ~0.45 NPX",
    x = "Genotype Dosage (Minor Allele Count: 0, 1, 2)",
    y = "Plasma PCSK9 (Olink NPX)"
  ) +
  theme(legend.position = "none")

ggsave("results/figures/Fig8a_pQTL_Dosage_Boxplot.png", p_pqtl, width = 6, height = 4.5)
```

---

## Step 2: Causal Inference via Mendelian Randomization (MR)

Observational studies often suffer from **reverse causation** and **confounding**. For example, does elevated protein $X$ *cause* heart disease, or does inflammation from heart disease simply increase protein $X$?

**Mendelian Randomization (MR)** resolves this by using genetic variants as natural randomized instruments.

```
                             [ GENETIC VARIANT (SNP) ]
                             (Randomized at conception)
                                   /             \
                   Instrumental   /               \  Violations of MR:
                   Variable      /                 \ Horizontal Pleiotropy
                                ▼                   ▼
                       [ PROTEIN LEVEL ] ──────► [ DISEASE OUTCOME ]
                        (e.g., PCSK9)      Causal  (e.g., Heart Disease)
                                           Effect?
```

### The Three Core Instrumental Variable Assumptions
1. **Relevance**: The genetic variant must be strongly associated with the protein ($F\text{-statistic} > 10$).
2. **Independence**: The variant must not be associated with confounders (e.g., smoking, socioeconomic status).
3. **Exclusion Restriction**: The variant affects disease risk **ONLY** through its effect on the target protein, with no alternate biological pathways.

---

### Running Two-Sample MR in R with TwoSampleMR

The Bioconductor/CRAN package **`TwoSampleMR`** connects pQTL summary statistics (Exposure) with disease GWAS summary statistics (Outcome):

```r
# Install TwoSampleMR if needed
if (!requireNamespace("TwoSampleMR", quietly = TRUE)) {
  remotes::install_github("MRCIEU/TwoSampleMR")
}

library(TwoSampleMR)

# 1. Format exposure data (cis-pQTL for PCSK9)
exposure_dat <- data.frame(
  SNP = "rs11591147",
  beta = -0.45,
  se = 0.034,
  effect_allele = "T",
  other_allele = "G",
  eaf = 0.25,
  Phenotype = "Plasma PCSK9"
)
exposure_dat <- format_data(exposure_dat, type = "exposure")

# 2. Format outcome data (Coronary Artery Disease GWAS)
outcome_dat <- data.frame(
  SNP = "rs11591147",
  beta = -0.18, # Negative log(OR) = reduced risk of heart attack
  se = 0.021,
  effect_allele = "T",
  other_allele = "G",
  eaf = 0.25,
  Phenotype = "Coronary Artery Disease"
)
outcome_dat <- format_data(outcome_dat, type = "outcome")

# 3. Harmonize alleles across studies
harmonized_dat <- harmonise_data(exposure_dat, outcome_dat)

# 4. Perform Wald Ratio Mendelian Randomization
mr_results <- mr(harmonized_dat, method_list = c("mr_wald_ratio"))
print(mr_results)
```

```text
       id.exposure id.outcome       method nsnp     b    se      pval
1 Plasma PCSK9      CAD     Wald ratio    1  0.40 0.055 3.12e-13
```

The causal estimate is positive ($\beta = 0.40, p = 3 \times 10^{-13}$), indicating that higher plasma PCSK9 **causally increases the risk of Coronary Artery Disease**!

---

## Step 3: Sensitivity Analyses: Testing for Horizontal Pleiotropy

When multiple independent *cis*-SNPs are available for a protein, we must test for **horizontal pleiotropy** (a SNP directly affecting the disease outcome independently of the protein):

1. **MR-Egger Regression**: If the MR-Egger intercept deviates significantly from zero ($p < 0.05$), it indicates horizontal pleiotropy, violating the exclusion restriction.
2. **Weighted Median Estimator**: Produces valid causal estimates even if up to 50% of genetic instruments are invalid.

```r
# For multi-SNP instruments:
# mr_pleiotropy_test(harmonized_dat)
# mr(harmonized_dat, method_list = c("mr_ivw", "mr_egger_regression", "mr_weighted_median"))
```

---

## Real-World Case Study: PCSK9, LDL-C, and Cardiovascular Disease

The PCSK9 story is the crown jewel of genetically guided proteomics:
1. Sequencing studies identified rare loss-of-function variants in *PCSK9* that slashed plasma LDL cholesterol by 40% and protected against heart attacks without apparent adverse effects.
2. Mendelian Randomization confirmed that pharmaceutical inhibition of circulating PCSK9 would mimic this lifelong genetic protection.
3. Today, monoclonal antibodies (**Evolocumab, Alirocumab**) and siRNA therapeutics (**Inclisiran**) targeting PCSK9 are FDA-approved blockbuster drugs saving millions of lives worldwide.

---

## Conclusion & What’s Next

In this guide, you learned:
1. What **pQTLs** are and why they predict pharmaceutical clinical trial success.
2. The difference between **local *cis*-pQTLs** and **network *trans*-pQTLs**.
3. How to perform **SNP-protein linear association testing** in R.
4. The core assumptions of **Mendelian Randomization (MR)**.
5. How to use **`TwoSampleMR`** to infer causal relationships between circulating proteins and clinical disease outcomes.

Now, we shift our focus from genetic regulation of protein abundance to enzymatic regulation of protein activity: **Post-Translational Modifications**.

In **[Part 9: Phosphoproteomics & Post-Translational Modification (PTM) Analysis](13_part-9-phosphoproteomics-and-ptm-site-analysis.md)**, we learn how mass spectrometers map phosphorylation sites and run Kinase-Substrate Enrichment Analysis (KSEA)!
