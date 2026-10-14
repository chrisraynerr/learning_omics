# Recommended Reading: Quantitative Proteomics (Mass Spec & Affinity Assays)

This reading curriculum accompanies the **Proteomics Data Analysis Tutorial Series** in this directory. It brings together the core open-access textbooks, landmark methodology papers, and benchmark reviews that explain the biophysics of mass spectrometry, the mathematics of peptide-level linear mixed models, and the population genetics of plasma affinity proteomics.

---

## Table of Contents

- [Core Textbooks & Course Curricula](#core-textbooks--course-curricula)
- [Foundational Methodology Papers](#foundational-methodology-papers)
  - [1. Discovery Mass Spectrometry (DIA, MaxLFQ, Deconvolution)](#1-discovery-mass-spectrometry-dia-maxlfq-deconvolution)
  - [2. Statistical Modeling & The Missing Value Dilemma](#2-statistical-modeling--the-missing-value-dilemma)
  - [3. Targeted Affinity Proteomics (Olink & SomaScan)](#3-targeted-affinity-proteomics-olink--somascan)
  - [4. Population pQTLs & Mendelian Randomization](#4-population-pqtls--mendelian-randomization)
  - [5. Phosphoproteomics & Kinase Signaling (KSEA)](#5-phosphoproteomics--kinase-signaling-ksea)
  - [6. Multi-Omics & Proteogenomics](#6-multi-omics--proteogenomics)
- [Benchmark Reviews & Clinical Guidelines](#benchmark-reviews--clinical-guidelines)
- [Tutorial-to-Reading Mapping Matrix](#tutorial-to-reading-mapping-matrix)
- [Core Mathematical & Analytical Formulations](#core-mathematical--analytical-formulations)

---

## Core Textbooks & Course Curricula

### 1. Proteomics Data Analysis
- **Authors:** Laurent Gatto & Charlotte Soneson (UCLouvain & FMI Basel)
- **Format:** Free Online Course Book
- **Access:** [lgatto.github.io/ProtDataAnalysis/](https://lgatto.github.io/ProtDataAnalysis/)
- **Key Modules to Study:**
  - *Mass Spectrometry Basics*: Raw chromatograms, MS1 precursor surveys, and MS2 fragment ion spectra.
  - *QFeatures Infrastructure*: Managing multi-assay containers from peptide precursors to summarized protein matrices.
  - *Missing Values & Normalization*: Visualizing limit-of-detection dropouts and applying variance stabilizing normalization (VSN).
- **Why It’s Essential:** Laurent Gatto created `MSnbase` and `QFeatures`. This is the premier Bioconductor-native textbook for modern computational mass spectrometry in R.

### 2. The May Institute for Computation and Statistics for Mass Spectrometry
- **Instructors:** Olga Vitek, Meena Choi, et al. (Northeastern University)
- **Format:** Free Annual Workshop Materials, GitHub Repositories & Video Lectures
- **Access:** [computationalproteomics.ws](https://computationalproteomics.ws/)
- **Why It’s Essential:** Hosted by the creators of **`MSstats`**. Contains the definitive training modules for experimental design, linear mixed-effects modeling, and clinical biomarker verification in quantitative proteomics.

### 3. Genetics in Drug Development: A Curated Anthology
- **Author:** Dr. Skanda Rajasundaram (associated with the Burgess Group / Dr. Stephen Burgess, MRC Biostatistics Unit, University of Cambridge)
- **Format & Availability:** Specialist Paperback (Published Dec 2023); serves as the official reference text for the University of Cambridge *Genetics in Drug Development* course. *(Note: There is no authorized public PDF copy online; it is a print publication available through major book retailers).*
- **Open-Access Companion Portal:** The Burgess Group maintains free lecture slides, practical R notebooks, and method guidelines at [mendelianrandomization.com](https://www.mendelianrandomization.com/).
- **Why It’s Essential:** Bridges statistical omics (pQTLs, eQTLs, GWAS) with real-world pharmaceutical R&D. Covers causal target validation via Mendelian Randomization, predicting on-target adverse effects through phenome-wide association studies (PheWAS), indication expansion, and de-risking clinical trials using human genetics.

---

## Foundational Methodology Papers

### 1. Discovery Mass Spectrometry (DIA, MaxLFQ, Deconvolution)

#### DIA-NN (Deep Learning DIA Deconvolution)
* **Citation:** Demichev, V., et al. (2020). DIA-NN: neural networks and new structural and statistical algorithms for data-independent acquisition proteomics. *Nature Methods*, 17(1), 41–44.
* **DOI:** [10.1038/s41592-019-0638-x](https://doi.org/10.1038/s41592-019-0638-x)
* **Key Concept:** Deep neural network models predict peptide retention times and fragmentation spectra directly from FASTA sequences, enabling library-free DIA searching and high-precision quantification.

#### MaxLFQ (Delayed Normalization & Peptide Ratio Extraction)
* **Citation:** Cox, J., et al. (2014). Accurate proteome-wide label-free quantification by delayed normalization and maximal peptide ratio extraction, termed MaxLFQ. *Molecular & Cellular Proteomics*, 13(9), 2513–2526.
* **DOI:** [10.1074/mcp.M113.031591](https://doi.org/10.1074/mcp.M113.031591)
* **Key Concept:** Solves the peptide-to-protein rollup problem by calculating pairwise peptide ratio medians between samples, preventing differences in peptide ionization efficiencies from distorting protein ratios.

---

### 2. Statistical Modeling & The Missing Value Dilemma

#### MSstats (Linear Mixed-Effects Models for Proteomics)
* **Citation:** Choi, M., et al. (2014). MSstats: an R package for statistical analysis of quantitative mass spectrometry-based proteomic experiments. *Bioinformatics*, 30(17), 2524–2526.
* **Protocol Citation:** Choi, M., et al. (2024). Statistical analysis of quantitative mass spectrometry data with MSstats. *Nature Protocols*, 19(5), 1400–1435.
* **Key Concept:** Models peptide precursors directly with linear mixed-effects models, incorporating fixed condition effects and random peptide/sample effects while accounting for censored missing values.

#### proDA (Probabilistic Dropout Analysis)
* **Citation:** Ahlmann-Eltze, C., & Huber, W. (2020). proDA: probabilistic dropout analysis for mass spectrometry-based proteomics. *Bioinformatics*, 36(Suppl_1), i552–i560.
* **DOI:** [10.1093/bioinformatics/btaa461](https://doi.org/10.1093/bioinformatics/btaa461)
* **Key Concept:** Eliminates ad-hoc imputation by modeling the dropout probability directly as a function of mean protein intensity within an empirical Bayes linear model framework.

#### Perseus Platform & Left-Censored Downshift Imputation
* **Citation:** Tyanova, S., et al. (2016). The Perseus computational platform for comprehensive analysis of (prote)omics data. *Nature Methods*, 13(9), 731–740.
* **DOI:** [10.1038/nmeth.3901](https://doi.org/10.1038/nmeth.3901)
* **Key Concept:** Formalized the left-censored Gaussian downshift imputation algorithm (shift = $-1.8\sigma$, width = $0.3\sigma$) to model proteins falling below the analytical Limit of Detection (MNAR).

---

### 3. Targeted Affinity Proteomics (Olink & SomaScan)

#### UK Biobank Pharma Proteomics Project (UKB-PPP)
* **Citation:** Sun, B. B., et al. (2023). Plasma proteomic associations with genetics and health in the UK Biobank. *Nature*, 622(7982), 329–338.
* **DOI:** [10.1038/s41586-023-06592-6](https://doi.org/10.1038/s41586-023-06592-6)
* **Key Concept:** The landmark study profiling >54,000 UK Biobank participants with Olink Explore 3072. Documents plate-bridging normalization, LOD thresholds, and the identification of 14,287 primary genetic pQTLs.

#### Cross-Platform Benchmark: SomaScan vs. Olink
* **Citation:** Pietzner, M., et al. (2021). Mapping the proteomic architecture of human diseases. *Science*, 374(6570), eabj1541.
* **DOI:** [10.1126/science.abj1541](https://doi.org/10.1126/science.abj1541)
* **Key Concept:** Systematic direct comparison of SomaScan and Olink across thousands of individuals. Unpacks the structural and genetic causes of discordance (epitope SNPs, PTM masking, complex formation).

#### The Plasma Proteome Dynamic Range Challenge
* **Citation:** Anderson, N. L., & Anderson, N. G. (2002). The human plasma proteome: history, character, and diagnostic prospects. *Molecular & Cellular Proteomics*, 1(11), 845–867.
* **DOI:** [10.1074/mcp.R200007-MCP200](https://doi.org/10.1074/mcp.R200007-MCP200)
* **Key Concept:** The classic paper demonstrating that circulating plasma proteins span $>10$ orders of magnitude in concentration, explaining why affinity assays are necessary to detect low-abundance cytokines.

---

### 4. Population pQTLs & Mendelian Randomization

#### Genetically Validated Drug Targets
* **Citation:** Nelson, M. R., et al. (2015). The support of human genetic evidence for approved drug indications. *Nature Genetics*, 47(8), 856–860.
* **DOI:** [10.1038/ng.3314](https://doi.org/10.1038/ng.3314)
* **Key Concept:** Demonstrates that clinical drug targets with human genetic validation have more than double the probability of success in Phase 3 trials.

#### The Target Discovery Blueprint
* **Citation:** Plenge, R. M., Scolnick, E. M., & Altshuler, D. (2013). Validating therapeutic targets through human genetics. *Nature Reviews Drug Discovery*, 12(8), 581–594.
* **DOI:** [10.1038/nrd4051](https://doi.org/10.1038/nrd4051)
* **Key Concept:** The classic conceptual roadmap for selecting *cis*-variants, linking genetic perturbation to therapeutic mechanisms, and predicting clinical efficacy.

#### Mendelian Randomization Investigation Guidelines
* **Citation:** Burgess, S., et al. (2019). Guidelines for performing Mendelian randomization investigations: update for summer 2023. *Wellcome Open Research*, 4, 186.
* **DOI:** [10.12688/wellcomeopenres.15555.2](https://doi.org/10.12688/wellcomeopenres.15555.2)
* **Key Concept:** The consensus guidelines authored by the Burgess Group on selecting genetic instruments, evaluating the F-statistic (relevance), and testing for horizontal pleiotropy.

#### Two-Sample Mendelian Randomization
* **Citation:** Hemani, G., et al. (2018). The MR-Base platform supports systematic causal inference across the human phenome. *eLife*, 7, e34408.
* **DOI:** [10.7554/eLife.34408](https://doi.org/10.7554/eLife.34408)
* **Key Concept:** Theoretical framework for using genetic variants as instrumental variables, contrasting Inverse Variance Weighted (IVW) regression with MR-Egger pleiotropy testing.

---

### 5. Phosphoproteomics & Kinase Signaling (KSEA)

#### KSEA (Kinase-Substrate Enrichment Analysis)
* **Citation:** Casado, P., et al. (2013). Kinase-substrate enrichment analysis provides insights into the heterogeneity of signaling networks and drug responses. *Science Signaling*, 6(268), rs6.
* **DOI:** [10.1126/scisignal.2003573](https://doi.org/10.1126/scisignal.2003573)
* **Key Concept:** Infers upstream kinase enzymatic activity by calculating $z$-score deviations across the collective phosphorylation changes of known downstream substrates.

#### PhosphoSitePlus Knowledgebase
* **Citation:** Hornbeck, P. V., et al. (2015). PhosphoSitePlus: a comprehensive resource for investigating the structure and function of post-translational modifications in man and mouse. *Nucleic Acids Research*, 43(D1), D512–D520.
* **DOI:** [10.1093/nar/gku1267](https://doi.org/10.1093/nar/gku1267)
* **Key Concept:** Curated repository of experimentally verified phosphorylation, acetylation, and ubiquitination sites mapped to specific upstream regulatory enzymes.

---

### 6. Multi-Omics & Proteogenomics

#### CPTAC Proteogenomics Consortium
* **Citation:** Clark, D. J., et al. (2019). Integrated Proteogenomic Characterization of Clear Cell Renal Cell Carcinoma. *Cell*, 179(4), 964–983.
* **DOI:** [10.1016/j.cell.2019.10.007](https://doi.org/10.1016/j.cell.2019.10.007)
* **Key Concept:** The gold standard case study integrating WGS, bulk RNA-seq, total proteomics, and phosphoproteomics, revealing post-transcriptional buffering and metabolic reprogramming.

---

## Benchmark Reviews & Clinical Guidelines

1. **A beginner's guide to mass spectrometry-based proteomics**
   * *Citation:* Sinha, A., & Mann, M. (2020). *The Biochemist*, 42(5), 64–69.
   * *Why Read It:* An accessible review of bottom-up workflow instrumentation, ionization, and chromatography.
2. **ProteoBench: Community benchmarks for proteomics data analysis**
   * *Citation:* The ProteoBench Consortium (2024). *bioRxiv*.
   * *Why Read It:* Unbiased empirical comparisons of DIA-NN, Spectronaut, FragPipe, and MaxQuant on standardized benchmark standards.

---

## Tutorial-to-Reading Mapping Matrix

| Tutorial File | Recommended Reading | Core Concepts |
| :--- | :--- | :--- |
| **01: Landscape & Setup** | Anderson & Anderson (2002) *MCP* | Dynamic range, Discovery MS vs. Olink PEA vs. SomaScan SOMAmers |
| **02: Raw to Matrix (DIA-NN)** | Demichev et al. (2020) *Nat Methods* | DIA deconvolution, in silico spectral library, 1% target-decoy FDR |
| **03: QC & Missing Values** | Gatto & Soneson (ProtDataAnalysis) | QFeatures container, MNAR vs. MAR, Perseus Gaussian downshift |
| **04: DAPs (MSstats vs. limma)** | Choi et al. (2024) *Nat Protoc* | Linear mixed-effects modeling, Tukey's median polish, peptide weighting |
| **05: Olink QC & NPX** | Sun et al. (2023) *Nature* (UKB-PPP) | NPX scale, internal incubation controls, plate bridging normalization |
| **06: SomaScan .adat & ANML** | Pietzner et al. (2021) *Science* | Chemically modified SOMAmers, hybridization controls, ANML scaling |
| **07: Affinity DAPs & LOD** | Sun et al. (2023) & Olink White Papers | Keeping continuous observed NPX below LOD, ANCOVA age/sex covariates |
| **08: Publication Figures** | Gu et al. (2016) *Bioinformatics* | Z-score scaling, multi-track sidebars in ComplexHeatmap, Volcano plots |
| **09: Pathways & STRING PPI** | Wu et al. (2021) & Szklarczyk et al. (2023) | Custom proteome background universe, GSEA, physical MCM complexes |
| **10: WGCNA Networks** | Langfelder & Horvath (2008) *BMC Bioinf* | Signed co-abundance networks, Topological Overlap Matrix, hub proteins |
| **11: Platform Discordance** | Pietzner et al. (2021) *Science* | Epitope SNPs, glycan shielding, complex masking, isoform specificity |
| **12: pQTLs & Mendelian Rand.** | Nelson et al. (2015), Hemani et al. (2018), Rajasundaram (*Anthology*) | cis vs. trans pQTLs, instrumental variables, IVW regression, PCSK9 |
| **13: Phosphoproteomics & PTMs** | Casado et al. (2013) *Sci Signal* (KSEA) | +79.9663 Da, Class I localization ($P \ge 0.75$), kinase z-scores |
| **14: Integrated Proteogenomics** | Clark et al. (2019) *Cell* (CPTAC) | mRNA-protein correlation ($r \approx 0.45$), stoichiometric buffering, quadrants |
| **15: PRIDE & ProteomeXchange** | Perez-Riverol et al. (2022) *NAR* (PRIDE) | PX submission wizard, PXD accessions, programmatic query with `rpx` |

---

## Core Mathematical & Analytical Formulations

### 1. MaxLFQ Protein Ratio Consistency
MaxLFQ extracts protein abundance ratios by minimizing the difference across all pairwise peptide ratios:
$$S = \sum_{j < k} \sum_{p \in P_{jk}} \left( \log(r_{jk}) - (\log(I_{jp}) - \log(I_{kp})) \right)^2$$
Where $r_{jk}$ is the unknown protein ratio between sample $j$ and $k$, and $I_{jp}, I_{kp}$ are the observed intensities of shared peptide $p$.

### 2. MSstats Linear Mixed-Effects Model
$$\log_2(\text{Intensity}_{ijk}) = \mu + \text{Condition}_i + \text{Peptide}_j + \text{BioReplicate}_k + \epsilon_{ijk}$$
Where $\text{Condition}_i$ is the fixed biological effect of interest, and $\text{Peptide}_j$ and $\text{BioReplicate}_k$ are random effects capturing technical and sample variation.

### 3. Left-Censored Gaussian Downshift Imputation (Perseus)
Missing values ($y_{\text{miss}}$) are drawn from a normal distribution parameterized by:
$$\mu_{\text{impute}} = \mu_{\text{observed}} - 1.8 \cdot \sigma_{\text{observed}}$$
$$\sigma_{\text{impute}} = 0.3 \cdot \sigma_{\text{observed}}$$
This places imputed values at the lower boundary of detection while maintaining realistic sample variance.

### 4. Wald Ratio in Mendelian Randomization
$$\hat{\beta}_{\text{causal}} = \frac{\hat{\beta}_{\text{SNP-Outcome}}}{\hat{\beta}_{\text{SNP-Exposure}}}$$
$$\text{SE}(\hat{\beta}_{\text{causal}}) = \frac{\text{SE}(\hat{\beta}_{\text{SNP-Outcome}})}{|\hat{\beta}_{\text{SNP-Exposure}}|}$$
Where the exposure is circulating protein concentration (pQTL) and the outcome is clinical disease risk (e.g., CAD odds ratio).
