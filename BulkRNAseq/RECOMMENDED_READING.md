# Recommended Reading: Bulk RNA-seq & Transcriptomics

This reading curriculum accompanies the **Bulk RNA-seq Tutorial Series** in this directory. It provides the essential statistical textbooks, foundational methodology papers, and benchmark reviews that explain the mathematical theory, experimental design trade-offs, and biological mechanics behind the code.

---

## Table of Contents

- [Core Textbooks & Living Books](#core-textbooks--living-books)
- [Foundational Methodology Papers](#foundational-methodology-papers)
  - [1. Differential Expression & Count Modeling](#1-differential-expression--count-modeling)
  - [2. Normalization & Batch Correction](#2-normalization--batch-correction)
  - [3. Gene Regulatory & Co-Expression Networks](#3-gene-regulatory--co-expression-networks)
  - [4. Splicing, Isoforms & Structural Variations](#4-splicing-isoforms--structural-variations)
  - [5. Cell Type Deconvolution](#5-cell-type-deconvolution)
- [Best-Practice Reviews & Benchmarks](#best-practice-reviews--benchmarks)
- [Tutorial-to-Reading Mapping Matrix](#tutorial-to-reading-mapping-matrix)
- [Core Statistical Formulas to Know](#core-statistical-formulas-to-know)

---

## Core Textbooks & Living Books

### 1. Modern Statistics for Modern Biology
- **Authors:** Susan Holmes & Wolfgang Huber (Stanford University & EMBL Heidelberg)
- **Format:** Free Online Book & Cambridge University Press
- **Access:** [web.stanford.edu/class/bios221/book/](https://web.stanford.edu/class/bios221/book/)
- **Key Chapters for Bulk RNA-seq:**
  - *Chapter 1 & 2*: Generative models for discrete data (Poisson, Binomial).
  - *Chapter 4*: Mixture models and expectation-maximization.
  - *Chapter 6*: Hypothesis testing, false discovery rate (FDR), and Benjamini-Hochberg moderation.
  - *Chapter 8*: High-throughput count data modeling (Negative Binomial distribution, dispersion shrinkage).
- **Why It’s Essential:** Wolfgang Huber is the co-developer of DESeq2. This book explains why ordinary linear regression fails on sequencing counts and builds the Negative Binomial model from first principles.

### 2. Computational Genomics with R
- **Author:** Altuna Akalin (Max Delbrück Center for Molecular Medicine, Berlin)
- **Format:** Free Online Book & CRC Press
- **Access:** [compgenomr.github.io/book/](https://compgenomr.github.io/book/)
- **Key Chapters:**
  - *Chapter 3*: Introduction to GenomicRanges and interval algebra.
  - *Chapter 4*: RNA-seq quantification and exploratory data analysis (PCA, clustering).
  - *Chapter 5*: Statistical modeling of gene expression.

### 3. Data Analysis for the Life Sciences with R
- **Authors:** Rafael A. Irizarry & Michael I. Love (Harvard University & UNC Chapel Hill)
- **Format:** Free Online Book & CRC Press
- **Access:** [rafalab.dfci.harvard.edu/pages/harvardx.html](https://rafalab.dfci.harvard.edu/pages/harvardx.html)
- **Why It’s Essential:** Michael Love created DESeq2; Rafael Irizarry pioneered microarray and RNA-seq normalization. The chapters on matrix algebra, design matrices (`model.matrix`), and batch effects (SVA/ComBat) are mandatory reading.

---

## Foundational Methodology Papers

### 1. Differential Expression & Count Modeling

#### DESeq2 (Empirical Bayes Shrinkage for RNA-seq)
* **Citation:** Love, M. I., Huber, W., & Anders, S. (2014). Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology*, 15(12), 550.
* **DOI:** [10.1186/s13059-014-0550-8](https://doi.org/10.1186/s13059-014-0550-8)
* **Key Concept:** Explains empirical Bayes shrinkage of gene-wise dispersions toward a trend curve, Cook's distance for outlier filtering, and $\log_2$ fold-change shrinkage (apeglm/ashr) to prevent noisy low-count genes from dominating volcano plots.

#### edgeR Quasi-Likelihood Pipeline
* **Citation:** Chen, Y., Lun, A. T., & Smyth, G. K. (2016). From reads to genes to pathways: differential expression analysis of RNA-Seq experiments using Rsubread and the edgeR quasi-likelihood pipeline. *F1000Research*, 5, 1438.
* **DOI:** [10.12688/f1000research.8987.2](https://doi.org/10.12688/f1000research.8987.2)
* **Key Concept:** Gordon Smyth's definitive workflow guide. Contrasts likelihood ratio tests (LRT) with quasi-likelihood F-tests (QLF), explaining how QLF strictly controls the type I error rate even with small sample sizes.

#### limma-voom (Transforming Counts for Linear Modeling)
* **Citation:** Law, C. W., Chen, Y., Shi, W., & Smyth, G. K. (2014). voom: Precision weights unlock linear model analysis tools for RNA-seq read counts. *Genome Biology*, 15(2), R29.
* **DOI:** [10.1186/gb-2014-15-2-r29](https://doi.org/10.1186/gb-2014-15-2-r29)
* **Key Concept:** Details the mathematical formulation of precision weights. Voom models the mean-variance trend of $\log_2\text{CPM}$ values and assigns observational weights, unlocking all linear modeling tools in `limma`.

---

### 2. Normalization & Batch Correction

#### TMM Normalization
* **Citation:** Robinson, M. D., & Oshlack, A. (2010). A scaling normalization method for differential expression analysis of RNA-seq data. *Genome Biology*, 11(3), R25.
* **DOI:** [10.1186/gb-2010-11-3-r25](https://doi.org/10.1186/gb-2010-11-3-r25)
* **Key Concept:** Explains why library size normalization (e.g., CPM, RPKM) fails when a small fraction of genes are massively up-regulated, and how the Trimmed Mean of M-values (TMM) corrects for composition bias.

#### ComBat & Surrogate Variable Analysis (SVA)
* **Citation:** Leek, J. T., et al. (2012). The sva package for removing batch effects and other unwanted variation in high-throughput experiments. *Bioinformatics*, 28(6), 882–883.
* **DOI:** [10.1093/bioinformatics/bts034](https://doi.org/10.1093/bioinformatics/bts034)
* **Key Concept:** Differentiates between known batch variables (corrected via linear model blocking or ComBat) and latent unmeasured confounders (captured via Surrogate Variable Analysis).

---

### 3. Gene Regulatory & Co-Expression Networks

#### WGCNA
* **Citation:** Langfelder, P., & Horvath, S. (2008). WGCNA: an R package for weighted correlation network analysis. *BMC Bioinformatics*, 9, 559.
* **DOI:** [10.1186/1471-2105-9-559](https://doi.org/10.1186/1471-2105-9-559)
* **Key Concept:** Scale-free topology criteria, soft-thresholding power ($\beta$), Topological Overlap Matrix (TOM), Module Eigengenes (MEs), and intramodular connectivity ($kME$).

#### GENIE3 (Random Forest Regulatory Networks)
* **Citation:** Huynh-Thu, V. A., et al. (2010). Inferring regulatory networks from expression data using tree-based ensemble methods. *PLOS ONE*, 5(9), e12776.
* **DOI:** [10.1371/journal.pone.0012776](https://doi.org/10.1371/journal.pone.0012776)
* **Key Concept:** Winner of the DREAM4 in silico network challenge. Deconstructs network inference into separate regression subproblems, predicting each target gene from candidate transcription factors using Random Forests.

---

### 4. Splicing, Isoforms & Structural Variations

#### rMATS (Alternative Splicing)
* **Citation:** Shen, S., et al. (2014). rMATS: robust and flexible detection of alternative splicing from RNA-Seq data. *PNAS*, 111(51), E5593–E5601.
* **DOI:** [10.1073/pnas.1419161111](https://doi.org/10.1073/pnas.1419161111)
* **Key Concept:** Percent Spliced In ($\Psi$, PSI), junction-spanning reads, exon skipping (SE), mutually exclusive exons (MXE), and hierarchical modeling of splicing across replicates.

#### FusionCatcher
* **Citation:** Nicorici, D., et al. (2014). FusionCatcher – a tool for finding somatic fusion genes in paired-end RNA-sequencing data. *bioRxiv*, 011650.
* **DOI:** [10.1101/011650](https://doi.org/10.1101/011650)
* **Key Concept:** Chimeric junction alignment, spanning reads, paired-end split reads, and filtering false-positive read-through fusions and PCR artifacts.

---

### 5. Cell Type Deconvolution

#### MuSiC (Multi-Subject Single-Cell Deconvolution)
* **Citation:** Wang, X., et al. (2019). Bulk tissue cell type deconvolution with multi-subject single-cell expression reference. *Nature Communications*, 10, 380.
* **DOI:** [10.1038/s41467-018-08023-x](https://doi.org/10.1038/s41467-018-08023-x)
* **Key Concept:** Explains cross-subject heterogeneity in single-cell reference datasets and weights genes based on their cross-subject consistency rather than just mean expression.

---

## Best-Practice Reviews & Benchmarks

1. **A survey of best practices for RNA-seq data analysis**
   * *Citation:* Conesa, A., et al. (2016). *Genome Biology*, 17, 13.
   * *Key Takeaways:* Replicate numbers ($n \ge 3$ minimum; $n \ge 6$ for subtle effects), sequencing depth recommendations (20–30M reads for gene DE; 60–100M for splicing/isoforms), paired-end vs. single-end trade-offs.
2. **Comprehensive evaluation of differential gene expression analysis methods for RNA-seq data**
   * *Citation:* Soneson, C., & Delorenzi, M. (2013). *Briefings in Bioinformatics*, 14(6), 722–734.
   * *Key Takeaways:* Systematic comparison of DESeq2, edgeR, and limma-voom across real and simulated datasets.

---

## Tutorial-to-Reading Mapping Matrix

| Tutorial File | Recommended Reading | Core Concepts |
| :--- | :--- | :--- |
| **01 & 02: Environment & Upstream** | Conesa et al. (2016) *Genome Biol* | FastQC, adapter trimming, STAR alignment vs. pseudoalignment (Salmon) |
| **03: Count Table to DEGs** | Love et al. (2014) *Genome Biol* (DESeq2) | Negative binomial fitting, dispersion trend, Wald test |
| **06: Normalization Deep Dive** | Robinson & Oshlack (2010) (TMM) | Library size vs. composition bias; RPKM vs. TPM vs. TMM |
| **07: Comparing limma, DESeq2, edgeR** | Law et al. (2014) & Chen et al. (2016) | Voom precision weights vs. Quasi-Likelihood F-test |
| **08: Batch Effects & Covariates** | Leek et al. (2012) *Bioinformatics* (SVA) | Design formula blocking (`~ Condition + Batch`) vs. ComBat matrix adjustment |
| **09: WGCNA Co-expression Networks** | Langfelder & Horvath (2008) *BMC Bioinf* | Scale-free $\beta$, Topological Overlap Matrix, Module Eigengenes |
| **10: GENIE3 Regulatory Networks** | Huynh-Thu et al. (2010) *PLOS ONE* | Random Forest regression on transcription factor candidates |
| **12: Master Regulator Analysis** | Alvarez et al. (2016) *Nat Genet* (VIPER) | Regulons, enrichment score, transcriptional master switches |
| **14: Cell Type Deconvolution** | Wang et al. (2019) *Nat Commun* (MuSiC) | scRNA-seq references, cell proportions, marker gene weighting |
| **15 & 18: Alternative Splicing** | Shen et al. (2014) *PNAS* (rMATS) | Percent Spliced In ($\Psi$), junction reads, exon skipping |
| **24 & 25: Fusion Genes** | Nicorici et al. (2014) (FusionCatcher) | Split reads, chimeric paired-ends, chromosomal translocations |
| **28: Nextflow & nf-core/rnaseq** | Ewels et al. (2020) *Nat Biotechnol* | Containers (Docker/Singularity), DAG pipeline execution, HPC reproducibility |

---

## Core Statistical Formulas to Know

### 1. Negative Binomial Mean-Variance Relationship
$$\text{Var}(Y_{gi}) = \mu_{gi} + \alpha_g \mu_{gi}^2$$
Where $\mu_{gi}$ is the mean expression of gene $g$ in sample $i$, and $\alpha_g$ is the **dispersion parameter**. As sequencing depth increases ($\mu \to \infty$), the quadratic term dominates, reflecting biological coefficient of variation (BCV).

### 2. TMM Scaling Factor Formula
$$\log_2(\text{Ratio}_{gk}) = \log_2\left(\frac{Y_{gk} / N_k}{Y_{gr} / N_r}\right)$$
The Trimmed Mean of M-values trims the top/bottom 30% of $M$-values and top/bottom 5% of average intensity values $A$, then calculates a weighted average of remaining genes to estimate the library scale factor.

### 3. Voom Precision Weight
$$w_{gi} = \frac{1}{\hat{\sigma}_{gi}^2}$$
Where $\hat{\sigma}_{gi}^2$ is predicted from a LOWESS curve fitted to the square-root standard deviation of $\log_2\text{CPM}$ as a function of mean count.
