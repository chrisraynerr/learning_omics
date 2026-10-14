# How to Analyze Single-Cell RNA-seq Data – Complete Beginner’s Guide Part 5: Cell Type-Specific Differential Expression, Proportion Testing, and Functional Pathway Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** September 19, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/)

- [Introduction: From Cell Types to Biological Mechanisms](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Introduction_From_Cell_Types_to_Biological_Mechanisms)

  - [Why Single-Cell RNA-seq Transforms Differential Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Why_Single-Cell_RNA-seq_Transforms_Differential_Analysis)
  - [The Three Complementary Analyses in This Tutorial](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#The_Three_Complementary_Analyses_in_This_Tutorial)
  - [Understanding Differential Expression: Cell-Level vs Pseudobulk](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Understanding_Differential_Expression_Cell-Level_vs_Pseudobulk)
  - [Choosing Between Cell-Level and Pseudobulk Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Choosing_Between_Cell-Level_and_Pseudobulk_Analysis)
  - [Understanding Functional State Scoring](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Understanding_Functional_State_Scoring)
  - [What You’ll Learn in This Tutorial](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#What_Youll_Learn_in_This_Tutorial)
  - [Dataset Reminder: GSE174609 PBMC Study](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Dataset_Reminder_GSE174609_PBMC_Study)

- [Setting Up Your R Environment](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Setting_Up_Your_R_Environment)

  - [Installing Required Packages](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Installing_Required_Packages)
  - [Loading Libraries and Configuration](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Loading_Libraries_and_Configuration)
  - [Loading Annotated Data from Part 4](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Loading_Annotated_Data_from_Part_4)

- [Part 1: Cell Type Proportion Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Part_1_Cell_Type_Proportion_Analysis)

  - [Understanding Proportion Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Understanding_Proportion_Analysis)
  - [Calculating Cell Type Proportions](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Calculating_Cell_Type_Proportions)
  - [Visualizing Proportions](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Visualizing_Proportions)
  - [Statistical Testing with Propeller](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Statistical_Testing_with_Propeller)

- [Part 2: Cell-Level Differential Expression with FindMarkers](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Part_2_Cell-Level_Differential_Expression_with_FindMarkers)

  - [Running FindMarkers for All Cell Types](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Running_FindMarkers_for_All_Cell_Types)
  - [Visualizing Cell-Level DE Results](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Visualizing_Cell-Level_DE_Results)

- [Part 3: Pseudobulk Differential Expression Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Part_3_Pseudobulk_Differential_Expression_Analysis)

  - [Understanding the Pseudobulk Approach](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Understanding_the_Pseudobulk_Approach)
  - [Creating Pseudobulk Count Matrices with Seurat 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Creating_Pseudobulk_Count_Matrices_with_Seurat_5)
  - [Running DESeq2 for Each Cell Type](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Running_DESeq2_for_Each_Cell_Type)

- [Validating Results with limma-voom](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Validating_Results_with_limma-voom)

  - [Running limma-voom Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Running_limma-voom_Analysis)
  - [What to Expect from Each Method](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#What_to_Expect_from_Each_Method)
  - [Understanding the Discrepancy in Our Results](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Understanding_the_Discrepancy_in_Our_Results)
  - [Biological Interpretation](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Biological_Interpretation)
  - [Practical Recommendations](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Practical_Recommendations)

- [Part 4: Functional State Scoring Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Part_4_Functional_State_Scoring_Analysis)

  - [Understanding Pathway Scoring](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Understanding_Pathway_Scoring)
  - [Downloading Gene Sets from MSigDB](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Downloading_Gene_Sets_from_MSigDB)
  - [Scoring Cells for Inflammatory Response](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Scoring_Cells_for_Inflammatory_Response)
  - [Visualizing Pathway Scores by Cell Type and Condition](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Visualizing_Pathway_Scores_by_Cell_Type_and_Condition)
  - [Statistical Testing of Pathway Scores (Pseudobulk Approach)](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Statistical_Testing_of_Pathway_Scores_Pseudobulk_Approach)

- [Best Practices for Single-Cell Differential Analysis](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Best_Practices_for_Single-Cell_Differential_Analysis)

  - [General Principles](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#General_Principles)
  - [Differential Expression Best Practices](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Differential_Expression_Best_Practices)
  - [Proportion Analysis Best Practices](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Proportion_Analysis_Best_Practices)
  - [Functional Scoring Best Practices](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Functional_Scoring_Best_Practices)
  - [Common Pitfalls to Avoid](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Common_Pitfalls_to_Avoid)

- [Conclusion: From Differential Analysis to Biological Discovery](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Conclusion_From_Differential_Analysis_to_Biological_Discovery)

  - [Resources for Continued Learning](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Resources_for_Continued_Learning)

- [References](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#References)

  - [Share this:](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Share_this)
  - [Like this:](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/#Like_this)

## Introduction: From Cell Types to Biological Mechanisms

In Parts 1-4 of this tutorial series, we’ve taken scRNA-seq data from raw sequencing reads through quality control, integration, clustering, and cell type annotation. We now have a beautifully annotated dataset where every cell has a biological identity (CD4+ T cells, monocytes, etc.) and metadata linking it to experimental conditions (Healthy vs Post-Treatment).

**But here’s the critical question**: *What biological changes does periodontal treatment induce in different immune cell populations?*

This is where single-cell analysis truly shines compared to bulk RNA-seq. Instead of averaging across all cells and losing cell-type-specific information, scRNA-seq allows us to ask precise, mechanistic questions:

- **Do treatment effects differ across cell types?** (Maybe monocytes respond strongly while B cells don’t)

- **Which immune populations expand or contract with treatment?** (Cell proportion changes)

- **What molecular programs activate in specific cell types?** (Functional state analysis)

- **Are treatment responses consistent across patients?** (Individual variation)

### Why Single-Cell RNA-seq Transforms Differential Analysis

**The fundamental limitation of bulk RNA-seq:**

Imagine you have a bag of mixed fruit and you blend it all together. Bulk RNA-seq measures the average properties of this smoothie. If apples decrease but oranges increase, you might see no change at all—the signal cancels out.

**How scRNA-seq solves this:**

With scRNA-seq, you can separate the fruit first, then measure each type individually. You discover that apples actually doubled while oranges tripled—critical biological insights that bulk analysis would miss entirely.

**Concrete example from our periodontitis study:**

**Bulk RNA-seq might show**: “Inflammatory genes are slightly upregulated after treatment” (p=0.08, not significant)

**scRNA-seq reveals**:

- **CD14+ monocytes**: Massively downregulate inflammatory genes (p<0.001)

- **CD16+ monocytes**: Dramatically upregulate inflammatory genes (p<0.001)

- **T cells**: No change in inflammatory genes (p=0.95)

The bulk result was averaging opposite effects, obscuring the true biology! Only scRNA-seq reveals that treatment induces an anti-inflammatory shift in classical monocytes while activating non-classical monocytes.

**Additional advantages of scRNA-seq analysis:**

1. **Cell composition changes**: Discover if treatment works by eliminating certain cell types or recruiting new ones

2. **Cell-type-specific responses**: Understand why some cells respond while others don’t

3. **Rare population discovery**: Identify small but critical cell populations driving disease

4. **State transitions**: Track cells moving between functional states

5. **Patient heterogeneity**: Understand individual variation in treatment response

### The Three Complementary Analyses in This Tutorial

To fully understand treatment effects, we perform three interconnected analyses:

#### Analysis 1: Cell Type Proportion Analysis

**Question**: *Does treatment change the abundance of different immune cell types?*

**What we measure**: The percentage of each cell type in each sample

**Example findings**:

- “CD14+ monocytes increase from 15% to 25% post-treatment” (recruitment)

- “B cells decrease from 10% to 5%” (depletion or migration)

- “NK cells remain constant at 8%” (unaffected by treatment)

**Why it matters**: Proportion changes can indicate:

- Which immune cells are recruited to fight infection

- Whether treatment depletes pathogenic populations

- If compensatory mechanisms activate (one type replaces another)

#### Analysis 2: Cell-Type-Specific Differential Expression

**Question**: *Within each cell type, which genes change expression between conditions?*

**What we measure**: Gene expression differences between Healthy and Post-Treatment **within each cell type separately**

**Example findings**:

- “In CD14+ monocytes: IL1B, TNF, and S100A8 are downregulated post-treatment” (reduced inflammation)

- “In CD4+ T cells: IFNG and IL2 are upregulated post-treatment” (T cell activation)

- “In B cells: No significant changes” (not responsive to treatment)

**Two statistical approaches we’ll use:**

**Approach 1: Cell-Level Analysis (FindMarkers)**

- Compares individual cells between conditions

- Fast and straightforward

- Good for exploratory analysis

- **Limitation**: Doesn’t account for patient/sample structure (pseudoreplication)

**Approach 2: Pseudobulk Analysis (DESeq2, limma)**

- Aggregates cells by sample, then compares samples

- Accounts for biological replicates properly

- More statistically rigorous

- **Recommended for publication**

**Why we need both:**

- FindMarkers is fast for exploration and biomarker discovery

- Pseudobulk is rigorous for final statistical claims

- Agreement between methods strengthens conclusions

**Critical difference from bulk RNA-seq:**

**What’s measured****DE testing****Cell type info****Statistical unit****Main advantage**

| Aspect | Bulk RNA-seq | scRNA-seq |
| --- | --- | --- |
|  | Average across all cells | Individual cells |
|  | Sample vs sample | Cell vs cell OR pseudobulk |
|  | Lost (averaged away) | Preserved (analyzed separately) |
|  | Biological sample | Cell (cell-level) OR sample (pseudobulk) |
|  | Deep sequencing depth | Cell-type resolution |

#### Analysis 3: Functional State Scoring

**Question**: *Are specific biological processes (inflammation, cell cycle, stress response) activated or suppressed in certain cell types?*

**What we measure**: Enrichment of predefined gene sets (pathways) in each cell

**Example findings**:

- “CD14+ monocytes show high ‘Inflammatory Response’ scores in pre-treatment but low scores post-treatment”

- “NK cells show elevated ‘Interferon Response’ regardless of treatment”

- “B cells have low scores for all immune activation pathways” (quiescent)

**How it works:**

1. **Define gene sets**: Download curated pathways from MSigDB (e.g., “Hallmark Inflammatory Response”)

2. **Score cells**: Calculate how strongly each cell expresses that gene set

3. **Compare conditions**: Test if scores differ between Healthy vs Post-Treatment

4. **Interpret biology**: High scores = pathway active, low scores = pathway suppressed

**When to use functional state scoring:**

**Use when:**

- You want to test specific biological hypotheses (e.g., “Does treatment reduce inflammation?”)

- You’re interested in biological processes rather than individual genes

- You want interpretable, pathway-level insights

- Multiple genes in a pathway show subtle coordinated changes

**Don’t use when:**

- You’re doing unbiased discovery (use DE analysis instead)

- Gene sets for your process don’t exist

- Individual gene changes are more important than pathways

- You need to identify novel biology (gene sets are predefined)

**Examples of biological questions answered by functional scoring:**

- Is oxidative stress higher in diseased vs healthy cells?

- Do specific T cell subsets show cell cycle activity?

- Are interferon responses activated in viral infection?

- Does treatment suppress inflammatory signaling?

### Understanding Differential Expression: Cell-Level vs Pseudobulk

This is a **critical concept** that confuses many beginners. Let’s explain both approaches clearly.

#### Approach 1: Cell-Level Differential Expression (Seurat’s FindMarkers)

**What it does**: Directly compares gene expression in individual cells between conditions

**Example:**

- Group A: 5,000 CD14+ monocytes from Healthy donors

- Group B: 5,000 CD14+ monocytes from Post-Treatment patients

- Test: Is gene X higher in Group A vs Group B?

**Statistical test**: Wilcoxon rank-sum test (default) or other non-parametric tests

**Advantages:**

- Fast and simple

- No aggregation required

- Good for biomarker discovery

- Captures cell-to-cell variability

**Disadvantages:**

- **Pseudoreplication**: Treats 1,000 cells from one patient as independent samples (they’re not!)

- Inflated false positives (many “significant” genes that aren’t truly different)

- Doesn’t account for patient/batch effects

- Not ideal for publication claims

**When to use:**

- Exploratory analysis

- Quick screening for candidate genes

- When you don’t have biological replicates

- Visualization and interpretation (still useful even if not final statistics)

#### Approach 2: Pseudobulk Differential Expression (DESeq2, limma)

**What it does**: Aggregates cells by sample, then compares samples (like bulk RNA-seq)

**Statistical test**: DESeq2 (negative binomial model accounting for count overdispersion)

**Advantages:**

- Proper biological replicates (samples, not cells)

- Accounts for patient effects

- More conservative (fewer false positives)

- Publication-quality statistics

- Handles count data properly (overdispersion, mean-variance relationship)

**Disadvantages:**

- Loses single-cell resolution

- Requires multiple samples per condition (need biological replicates)

- Slower computationally

- May miss very rare cell-type-specific effects

**When to use:**

- Final statistical claims for publication

- When you have >=3 biological replicates per condition

- Testing treatment effects with patient structure

- Comparing across multiple conditions with complex designs

**Why pseudobulk is necessary: The pseudoreplication problem**

Imagine you have 4 healthy donors and 4 post-treatment patients. Each donor contributes ~1,000 CD14+ monocytes.

**Cell-level analysis says**: “I have 4,000 vs 4,000 cells = 8,000 independent samples!”

**Reality**: You have 4 vs 4 **patients** = 8 independent samples. The 1,000 cells per patient are **not independent**—they come from the same person and share patient-specific effects (genetics, environment, technical batch).

**The consequence**: Cell-level analysis massively inflates your sample size, leading to:

- p-values that are far too small (10^-50 instead of realistic 0.01)

- Hundreds of “significant” genes (most are false positives)

- Claims that don’t replicate in new experiments

**Pseudobulk fixes this** by treating each patient as one sample, giving you the correct statistical power and realistic p-values.

**Analogy:**

- **Cell-level**: Asking 1,000 people from New York their opinion and claiming you surveyed 1,000 independent cities

- **Pseudobulk**: Correctly recognizing you surveyed 1 city (New York) with 1,000 residents

### Choosing Between Cell-Level and Pseudobulk Analysis

**Our recommendation**: **Always do both!**

**Start with FindMarkers (cell-level)** for:

- Fast exploration

- Identifying candidate genes

- Creating visualizations

- Understanding biological patterns

**Validate with Pseudobulk (DESeq2)** for:

- Final statistical claims

- Publication figures

- Identifying robust, reproducible changes

- Accounting for patient effects

**Report genes that pass both**:

- Significant in FindMarkers (biomarker candidates)

- Significant in pseudobulk (statistically robust)

- These are your high-confidence hits

### Understanding Functional State Scoring

Functional state scoring answers the question: **“Is a biological process active in this cell?”**

**The concept:**

Instead of looking at one gene at a time, we look at **sets of genes that work together** in biological pathways. If many genes in the “Inflammatory Response” pathway are highly expressed, that cell is in an inflammatory state.

**Example gene sets from MSigDB Hallmarks:**

- **Inflammatory Response**: IL1B, TNF, IL6, CXCL8, CCL2, etc. (50 genes)

- **Interferon Gamma Response**: STAT1, IRF1, GBP1, CXCL9, etc. (200 genes)

- **Oxidative Phosphorylation**: COX5A, ATP5A1, NDUFB5, etc. (200 genes)

- **Cell Cycle**: CDK1, CCNB1, MKI67, TOP2A, etc. (200 genes)

**How scoring works** (simplified):

For each cell:

1. Look at expression of all genes in the gene set

2. Calculate average expression (with controls for background)

3. Assign a score (high score = many genes expressed strongly)

4. Result: Each cell gets a score for each pathway

**What functional scores tell you:**

- **High score**: Pathway is active (cell is in that functional state)

- **Low score**: Pathway is inactive (cell is not in that state)

- **Comparing conditions**: Do Healthy vs Post-Treatment cells differ in pathway activity?

**When functional scoring is most powerful:**

**Best use cases:**

- Testing specific hypotheses (“Does treatment reduce inflammation?”)

- Identifying cell states (inflammatory vs resting monocytes)

- Comparing activation levels across conditions

- Understanding coordinated gene programs

**Less useful when:**

- Doing unbiased discovery (DE analysis is better)

- Pathways of interest aren’t well-defined

- Individual genes matter more than pathways

**Why we use pseudobulk for statistical comparison:**

Just like with differential expression, comparing functional scores between individual cells has the pseudoreplication problem. We need to:

1. **Calculate scores for each cell** (captures cell-to-cell variation)

2. **Aggregate scores by sample** (get mean score per patient)

3. **Compare samples statistically** (proper biological replicates)

This ensures our conclusions about pathway differences are statistically sound.

### What You’ll Learn in This Tutorial

By completing Part 5, you’ll be able to:

- Quantify cell type proportions and test for composition differences

- Perform cell-level DE analysis with FindMarkers for exploration

- Conduct pseudobulk DE analysis with DESeq2 for publication

- Compare results between cell-level and pseudobulk approaches

- Score functional states using MSigDB gene sets

- Interpret biological meaning of differential expression and proportions

- Create publication-quality visualizations (volcano plots, heatmaps, violin plots)

- Identify robust, reproducible changes across methods

### Dataset Reminder: GSE174609 PBMC Study

We continue with our 8-sample dataset:

- **4 Healthy donors**: Baseline immune profiles

- **4 Post-treatment patients**: After non-surgical periodontal therapy

**Expected biological findings:**

- **Proportion changes**: Treatment may alter immune cell recruitment

- **Gene expression changes**: Anti-inflammatory effects in monocytes/T cells

- **Functional states**: Reduced inflammatory signatures, altered activation

Let’s begin by setting up our R environment and loading the annotated data from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/).

## Setting Up Your R Environment

We’ll build on the packages from Parts 1-4 and add tools specifically for differential expression and functional analysis.

### Installing Required Packages

```r
#-----------------------------------------------
# Install required packages (run once)
#-----------------------------------------------

# Install BiocManager if needed
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# Install CRAN packages
install.packages(c("Seurat", "msigdbr", "ggplot2", "pheatmap", 
                   "ggridges", "ggrepel", "patchwork", "dplyr", "tidyr"))

# Install Bioconductor packages
BiocManager::install(c("DESeq2", "edgeR", "limma", "speckle", "fgsea", "EnhancedVolcano"))

# Faster FindMarkers
# install.packages("devtools")
devtools::install_github("immunogenomics/presto")
```

### Loading Libraries and Configuration

```r
#-----------------------------------------------
# STEP 1: Load required libraries
#-----------------------------------------------

# Core single-cell analysis
library(Seurat)
library(SeuratObject)

# Differential expression
library(DESeq2)
library(edgeR)
library(limma)

# Proportion analysis
library(speckle)

# Functional analysis
library(fgsea)
library(msigdbr)

# Visualization
library(ggplot2)
library(pheatmap)
library(ggridges)
library(ggrepel)
library(EnhancedVolcano)
library(patchwork)
library(dplyr)
library(tidyr)

# Set working directory
setwd("~/GSE174609_scRNA/differential_analysis")

# Create output directories
dir.create("plots", showWarnings = FALSE)
dir.create("plots/proportions", showWarnings = FALSE)
dir.create("plots/DE_celllevel", showWarnings = FALSE)
dir.create("plots/DE_pseudobulk", showWarnings = FALSE)
dir.create("plots/functional_scoring", showWarnings = FALSE)
dir.create("results", showWarnings = FALSE)

# Set random seed for reproducibility
set.seed(42)

# Configure plotting defaults
theme_set(theme_classic(base_size = 12))
```

### Loading Annotated Data from Part 4

```r
#-----------------------------------------------
# STEP 2: Load annotated Seurat object from Part 4
#-----------------------------------------------

# Load the annotated object
seurat_obj <- readRDS("~/GSE174609_scRNA/cell_type_annotation/annotations/integrated_annotated_seurat.rds")
```

> 

**Note**: We’re using `final_annotation` from [Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/), which represents the consensus cell type labels built from multiple annotation methods (Manual, SingleR, scType, scCATCH).

## Part 1: Cell Type Proportion Analysis

Cell type proportions reveal whether treatment affects the composition of the immune system—recruiting specific cell types or depleting others.

### Understanding Proportion Analysis

**What we’re measuring**: The percentage of each cell type in each sample

**Why it matters:**

- Treatment may recruit specific immune cells (proportions increase)

- Disease may deplete certain populations (proportions decrease)

- Compositional changes reveal immune system reorganization

**The compositional challenge:**

Cell type proportions are **compositional data**—they sum to 100% within each sample. This creates statistical challenges:

- If one cell type increases, others must decrease (not independent)

- Standard t-tests are inappropriate for proportion data

- We need methods designed for compositional data

**Our approach**: Use the `propeller` function from the `speckle` package, which:

- Accounts for compositional nature of proportions

- Tests for significant changes between conditions

- Provides FDR-corrected p-values

### Calculating Cell Type Proportions

```r
#-----------------------------------------------
# STEP 3: Calculate cell type proportions per sample
#-----------------------------------------------

# Calculate proportions for each sample
proportion_data <- seurat_obj@meta.data %>%
  group_by(sample_id, condition, final_annotation) %>%
  summarise(count = n(), .groups = "drop") %>%
  group_by(sample_id) %>%
  mutate(
    total_cells = sum(count),
    proportion = count / total_cells,
    percentage = proportion * 100
  ) %>%
  ungroup()

# Save proportion data
write.csv(proportion_data, "results/cell_type_proportions_per_sample.csv", row.names = FALSE)

# Display summary
proportion_summary <- proportion_data %>%
  group_by(condition, final_annotation) %>%
  summarise(
    mean_percentage = mean(percentage),
    sd_percentage = sd(percentage),
    .groups = "drop"
  )
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/proportion_summary.png?resize=1024%2C639&ssl=1)

### Visualizing Proportions

```r
#-----------------------------------------------
# STEP 4: Visualize cell type proportions
#-----------------------------------------------

# Stacked barplot showing composition of each sample
p_stacked <- ggplot(proportion_data, 
                    aes(x = sample_id, y = percentage, fill = final_annotation)) +
  geom_bar(stat = "identity") +
  facet_wrap(~ condition, scales = "free_x") +
  theme_classic(base_size = 12) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        legend.position = "right") +
  labs(title = "Cell Type Composition by Sample",
       x = "Sample", y = "Percentage (%)",
       fill = "Cell Type") +
  scale_fill_brewer(palette = "Set3")

ggsave("plots/proportions/01_stacked_composition.png", p_stacked,
       width = 12, height = 6, dpi = 300)

# Grouped barplot with error bars
p_grouped <- ggplot(proportion_summary, 
                    aes(x = final_annotation, y = mean, fill = condition)) +
  geom_bar(stat = "identity", position = position_dodge(0.9)) +
  geom_errorbar(aes(ymin = mean - sd, ymax = mean + sd),
                position = position_dodge(0.9), width = 0.2) +
  theme_classic(base_size = 12) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  labs(title = "Mean Cell Type Proportions by Condition",
       x = "Cell Type", y = "Mean Percentage (%)") +
  scale_fill_manual(values = c("Healthy" = "#2E86AB", "Post_Treatment" = "#F18F01"))

ggsave("plots/proportions/02_grouped_proportions.png", p_grouped,
       width = 12, height = 6, dpi = 300)
```

> 

**Reading the plots:**

- **Stacked bars**: Show overall composition (all bars = 100%)

- **Grouped bars**: Compare mean proportions with error bars (SD)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/scrna_stack_bar_plots.png?resize=1024%2C678&ssl=1)

### Statistical Testing with Propeller

The `propeller` function performs statistical testing specifically designed for compositional data. It uses **arcsin-square-root transformation** followed by a **linear model** to test for differences in cell type proportions between conditions.

**How propeller works:**

1. **Transformation**: Cell type proportions are transformed using arcsin(sqrt(proportion)) to stabilize variance and make the data more suitable for linear modeling

2. **Linear Model**: Fits a linear model with the transformed proportions as the response variable and experimental conditions as predictors

3. **Moderated t-statistics**: Uses empirical Bayes methods (similar to limma) to borrow information across cell types, improving statistical power

4. **Multiple testing correction**: Applies FDR correction across all cell types tested

This approach properly accounts for the compositional nature of the data while providing robust statistical inference.

```r
#-----------------------------------------------
# STEP 5: Statistical testing of proportion differences
#-----------------------------------------------

# Prepare data for propeller
cluster_ids <- seurat_obj$final_annotation
sample_ids <- seurat_obj$sample_id

# Create group vector (condition per sample)
sample_to_condition <- unique(seurat_obj@meta.data[, c("sample_id", "condition")])
rownames(sample_to_condition) <- sample_to_condition$sample_id
group <- sample_to_condition[sample_ids, "condition"]

# Run propeller test
propeller_results <- propeller(
  clusters = cluster_ids,
  sample = sample_ids,
  group = group
)

# Save results
write.csv(propeller_results, "results/propeller_proportion_test.csv", row.names = FALSE)
```

> 

**Understanding Propeller Results:**

- **PropMean.Healthy/Post_Treatment**: Mean proportion in each condition

- **PropRatio**: Ratio of proportions (Post/Healthy). >1 = increase, <1 = decrease

- **P.Value**: Raw p-value from statistical test

- **FDR**: False discovery rate-adjusted p-value (use this for significance)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/propeller_proportion_test.png?resize=1024%2C228&ssl=1)

## Part 2: Cell-Level Differential Expression with FindMarkers

Now we identify differentially expressed genes within each cell type using Seurat’s `FindMarkers` function. This provides fast exploratory analysis to identify candidate genes.

### Running FindMarkers for All Cell Types

```r
#-----------------------------------------------
# STEP 6: Cell-level DE analysis with FindMarkers
#-----------------------------------------------

# Get unique cell types
cell_types <- unique(seurat_obj$final_annotation)

# Set default assay and identify for comparison
DefaultAssay(seurat_obj) <- "RNA"
Idents(seurat_obj) <- "condition"

# Function to run FindMarkers for one cell type
run_findmarkers <- function(celltype, seurat_obj) {
  cat("  Processing:", celltype, "\n")

  # Subset to this cell type
  seurat_obj_sub <- subset(seurat_obj, subset = final_annotation == celltype)

  # Run FindMarkers comparing conditions
  markers <- FindMarkers(
    seurat_obj_sub,
    ident.1 = "Post_Treatment",
    ident.2 = "Healthy",
    test.use = "wilcox",
    logfc.threshold = 0,
    min.pct = 0.1,
    verbose = FALSE
  )

  # Add gene names and cell type
  markers$gene <- rownames(markers)
  markers$cell_type <- celltype

  return(markers)
}

# Run FindMarkers for all cell types
findmarkers_list <- lapply(cell_types, run_findmarkers, seurat_obj = seurat_obj)
names(findmarkers_list) <- cell_types

# Combine results
findmarkers_all <- do.call(rbind, findmarkers_list)
rownames(findmarkers_all) <- NULL

# Add significance flag
findmarkers_all$significant <- findmarkers_all$p_val_adj < 0.05 & 
                               abs(findmarkers_all$avg_log2FC) > 0.25

# Save results
write.csv(findmarkers_all, "results/findmarkers_celllevel_results.csv", row.names = FALSE)

# Summary of significant genes per cell type
sig_summary <- findmarkers_all %>%
  filter(significant) %>%
  group_by(cell_type) %>%
  summarise(
    total_genes_tested = n(),
    sig_genes = sum(significant),
    sig_up = sum(avg_log2FC > 0.25 & p_val_adj < 0.05),
    sig_down = sum(avg_log2FC < -0.25 & p_val_adj < 0.05),
    .groups = "drop"
  )
```

> 

**FindMarkers Parameters:**

- **ident.1**: Post-Treatment (numerator in fold-change)

- **ident.2**: Healthy (denominator)

- **test.use**: Statistical test (Wilcoxon rank-sum)

- **logfc.threshold**: Minimum |log2FC| to test (0.25 = 1.19-fold)

- **min.pct**: Gene must be detected in >=10% of cells in at least one group

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/findmarkers_summary.png?resize=1024%2C377&ssl=1)

### Visualizing Cell-Level DE Results

```r
#-----------------------------------------------
# STEP 7: Visualize FindMarkers results
#-----------------------------------------------

# Volcano plots for each cell type
for (ct in cell_types) {
  # Get data for this cell type
  ct_data <- findmarkers_all %>% filter(cell_type == ct)

  # Identify top genes to label
  top_genes <- ct_data %>%
    filter(significant) %>%
    arrange(p_val_adj) %>%
    head(10)

  # Create volcano plot
  p_volcano <- ggplot(ct_data, aes(x = avg_log2FC, y = -log10(p_val_adj))) +
    geom_point(aes(color = significant), alpha = 0.5, size = 1) +
    scale_color_manual(values = c("FALSE" = "gray", "TRUE" = "red")) +
    theme_classic(base_size = 12) +
    labs(title = paste("Volcano Plot:", ct),
         x = "log2 Fold Change (Post-Treatment vs Healthy)",
         y = "-log10(FDR)",
         color = "Significant") +
    geom_hline(yintercept = -log10(0.05), linetype = "dashed", color = "blue") +
    geom_vline(xintercept = c(-0.25, 0.25), linetype = "dashed", color = "blue")

  # Add labels for top genes
  if (nrow(top_genes) > 0) {
    p_volcano <- p_volcano +
      geom_text_repel(data = top_genes, aes(label = gene), size = 3, max.overlaps = 10)
  }

  ggsave(paste0("plots/DE_celllevel/volcano_", gsub(" ", "_", ct), ".png"),
         p_volcano, width = 8, height = 7, dpi = 300)
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/volcano_CD4_T_cells.png?resize=1024%2C896&ssl=1)

## Part 3: Pseudobulk Differential Expression Analysis

Now we perform the **statistically rigorous** differential expression analysis by aggregating cells into pseudobulk samples and using DESeq2.

### Understanding the Pseudobulk Approach

As explained in the introduction, cell-level analysis treats cells from the same patient as independent samples (pseudoreplication). Pseudobulk corrects this by:

1. **Aggregating cells** by sample (sum counts from all cells of a type in each sample)

2. **Creating pseudobulk samples** (one per patient, like bulk RNA-seq)

3. **Comparing samples** with proper biological replicates (DESeq2)

4. **Accounting for patient effects** and count overdispersion

**The aggregation process:**

```
Before aggregation (cell-level):
  Healthy_1: 1,200 CD14+ monocytes (each with individual counts)
  Healthy_2: 1,150 CD14+ monocytes
  Healthy_3: 1,100 CD14+ monocytes
  Healthy_4: 1,050 CD14+ monocytes
  Post_1: 1,100 CD14+ monocytes
  Post_2: 1,050 CD14+ monocytes
  Post_3: 1,000 CD14+ monocytes
  Post_4: 950 CD14+ monocytes

After aggregation (pseudobulk):
  Healthy_1: Sum of 1,200 cells -> 1 pseudobulk sample
  Healthy_2: Sum of 1,150 cells -> 1 pseudobulk sample
  ...
  Post_4: Sum of 950 cells -> 1 pseudobulk sample

Result: 4 vs 4 samples (proper biological replicates!)
```

### Creating Pseudobulk Count Matrices with Seurat 5

Seurat 5 provides a dedicated function `PseudobulkExpression()` specifically designed for creating pseudobulk count matrices.

```r
#-----------------------------------------------
# STEP 8: Create pseudobulk count matrices using PseudobulkExpression
#-----------------------------------------------

# Function to create pseudobulk for one cell type
create_pseudobulk_seurat5 <- function(celltype, seurat_obj) {
  # Subset to this cell type
  seurat_subset <- subset(seurat_obj, subset = final_annotation == celltype)

  # Use PseudobulkExpression to aggregate counts by sample
  pseudobulk_result <- PseudobulkExpression(
    seurat_subset,
    assays = "RNA",
    group.by = "sample_id",     # Aggregate by sample
    layer = "counts",            # Use raw counts
    method = "aggregate"         # Sum counts
  )

  # Extract the RNA counts matrix
  pseudobulk_matrix <- pseudobulk_result$RNA

  # Convert column names back to match original sample_id format
  colnames(pseudobulk_matrix) <- gsub("-", "_", colnames(pseudobulk_matrix))

  # Return as regular matrix (required for DESeq2)
  return(as.matrix(pseudobulk_matrix))
}

# Create pseudobulk for all cell types
pseudobulk_list <- lapply(cell_types, function(ct) {
  cat("  Processing:", ct, "\n")
  create_pseudobulk_seurat5(ct, seurat_obj)
})

names(pseudobulk_list) <- cell_types
```

**CD4+ T cells Pseudobulk Count Matrix:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/pseudobulk_cd4.png?resize=1024%2C183&ssl=1)

### Running DESeq2 for Each Cell Type

```r
#-----------------------------------------------
# STEP 9: Run DESeq2 pseudobulk analysis
#-----------------------------------------------

# Create sample metadata
sample_metadata <- unique(seurat_obj@meta.data[, c("sample_id", "condition", "patient_id")])

sample_metadata <- as.data.frame(sample_metadata)
rownames(sample_metadata) <- sample_metadata$sample_id

# Function to run DESeq2 for one cell type
run_deseq2_celltype <- function(celltype, pseudobulk_counts, sample_metadata) {

  # Get sample names from count matrix
  samples_in_counts <- colnames(pseudobulk_counts)

  # Verify that sample_metadata rownames match count matrix colnames
  if (!all(samples_in_counts %in% rownames(sample_metadata))) {
    stop("ERROR: Sample names in count matrix don't match sample_metadata rownames!")
  }

  # Order metadata to match count matrix columns
  sample_metadata_ordered <- sample_metadata[samples_in_counts, , drop = FALSE]

  # Create DESeq2 dataset
  dds <- DESeqDataSetFromMatrix(
    countData = pseudobulk_counts,
    colData = sample_metadata_ordered,
    design = ~ condition
  )

  # Filter low-count genes (improve power and speed)
  keep <- rowSums(counts(dds) >= 10) >= 3
  dds <- dds[keep, ]

  # Run DESeq2 analysis
  dds <- DESeq(dds, quiet = TRUE)

  # Extract results
  res <- results(
    dds,
    contrast = c("condition", "Post_Treatment", "Healthy"),
    alpha = 0.05
  )

  # Convert to data frame
  res_df <- as.data.frame(res)
  res_df$gene <- rownames(res_df)
  res_df$cell_type <- celltype

  # Add significance flag
  res_df$significant <- !is.na(res_df$padj) & 
                        res_df$padj < 0.05 & 
                        abs(res_df$log2FoldChange) > 0.25

  return(res_df)
}

# Run DESeq2 for all cell types
deseq2_list <- lapply(cell_types, function(ct) {
  run_deseq2_celltype(ct, pseudobulk_list[[ct]], sample_metadata)
})

names(deseq2_list) <- cell_types

# Combine results
deseq2_all <- do.call(rbind, deseq2_list)
rownames(deseq2_all) <- NULL

# Save results
write.csv(deseq2_all, "results/deseq2_pseudobulk_results.csv", row.names = FALSE)

# Summary of significant genes per cell type
deseq2_summary <- deseq2_all %>%
  group_by(cell_type) %>%
  summarise(
    total_genes_tested = n(),
    sig_genes = sum(significant, na.rm = TRUE),
    sig_up = sum(significant & log2FoldChange > 0.25, na.rm = TRUE),
    sig_down = sum(significant & log2FoldChange < -0.25, na.rm = TRUE),
    .groups = "drop"
  )
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/deseq2_summary.png?resize=1024%2C375&ssl=1)

## Validating Results with limma-voom

To ensure our differential expression findings are robust, we’ll use **limma-voom** as an independent validation method. Limma uses a different statistical framework than DESeq2, so genes significant in both methods are particularly trustworthy.

### Running limma-voom Analysis

```r
#-----------------------------------------------
# STEP 10: limma-voom pseudobulk analysis
#-----------------------------------------------

# Function to run limma-voom for one cell type
limma_paired_analysis <- function(
    counts,
    meta,
    design_formula = ~0 + condition,
    contrast_str = "Post_Treatment - Healthy",
    min_count = 10,
    voom_with_quality_weights = TRUE
) {

  # Order metadata to match count matrix columns
  meta <- meta[match(colnames(counts), meta$sample_id), ]
  rownames(meta) <- meta$sample_id

  # Convert factors
  meta$condition <- factor(meta$condition, levels = c("Healthy", "Post_Treatment"))

  # Filter low expressed genes
  keep <- rowSums(counts) >= min_count
  counts <- counts[keep, ]

  # Create design matrix
  design <- model.matrix(design_formula, data = meta)
  colnames(design) <- gsub("condition", "", colnames(design))

  # Voom transformation with quality weights
  if (voom_with_quality_weights) {
    v <- voomWithQualityWeights(counts, design, plot = FALSE)
  } else {
    v <- voom(counts, design, plot = FALSE)
  }

  # Fit linear model
  fit <- lmFit(v, design)

  # Define contrast
  contrast.matrix <- makeContrasts(
    contrasts = contrast_str,
    levels = colnames(design)
  )

  # Fit contrasts
  fit2 <- contrasts.fit(fit, contrast.matrix)
  fit2 <- eBayes(fit2, trend = TRUE, robust = TRUE)

  # Get results
  res <- topTable(fit2, number = Inf, sort.by = "none", adjust.method = "BH")

  return(res)
}

# Run limma for all cell types
limma_list <- lapply(cell_types, function(ct) {
  cat("\nProcessing:", ct, "\n")
  limma_paired_analysis(pseudobulk_list[[ct]], sample_metadata)
})

names(limma_list) <- cell_types

# Process limma results
limma_all <- lapply(names(limma_list), function(ct) {
  res <- limma_list[[ct]]
  res$gene <- rownames(res)
  res$cell_type <- ct
  res$significant <- res$adj.P.Val < 0.05 & abs(res$logFC) > 0.25
  return(res)
}) %>% bind_rows()

# Save results
write.csv(limma_all, "results/limma_pseudobulk_results.csv", row.names = FALSE)

# Summary of significant genes per cell type
limma_summary <- limma_all %>%
  group_by(cell_type) %>%
  summarise(
    total_genes_tested = n(),
    sig_genes = sum(significant, na.rm = TRUE),
    sig_up = sum(significant & logFC > 0.25, na.rm = TRUE),
    sig_down = sum(significant & logFC < -0.25, na.rm = TRUE),
    .groups = "drop"
  )
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/limma_summary-.png?resize=1024%2C376&ssl=1)

### What to Expect from Each Method

**FindMarkers (Cell-Level Analysis):**

FindMarkers typically identifies **many more significant genes** because it:

- Treats each cell as an independent observation (thousands of “samples”)

- Tests gene expression differences across all individual cells

- Has high sensitivity for detecting cell-to-cell variation

- Does not account for sample/patient structure

- Is prone to **pseudoreplication** (cells from the same patient are not independent)

**Expected outcome**: Hundreds to thousands of significant genes per cell type, with very small p-values (often p < 10^-50)

**DESeq2 (Pseudobulk Analysis):**

DESeq2 typically identifies **fewer but more robust genes** because it:

- Treats each patient/sample as the statistical unit (proper biological replicates)

- Aggregates cells by sample before testing

- Accounts for between-patient variability

- Is more conservative (fewer false positives)

- Has lower power with small sample sizes (we only have 4 vs 4 samples)

**Expected outcome**: Fewer significant genes (sometimes zero for certain cell types), with realistic p-values reflecting true statistical power

### Understanding the Discrepancy in Our Results

Looking at our specific results:

**FindMarkers Results:**

**DESeq2 Results:**

**What This Tells Us:**

1. **FindMarkers is overly optimistic**: The hundreds of “significant” genes per cell type are likely inflated by pseudoreplication. When we properly account for patient-level variation, most of these differences disappear.

2. **DESeq2 reveals the statistical reality**: With only 4 samples per condition, we have limited statistical power to detect differences. The few genes that remain significant in DESeq2 are likely to be:

- Genes with very large effect sizes

- Genes with consistent changes across all patients

- The most robust, reproducible changes

**The dramatic difference indicates**:

- High between-patient variability in gene expression

- Treatment effects may be subtle or patient-specific

- Most “significant” genes in FindMarkers are false positives due to pseudoreplication

- We would need more samples (>=8 per condition) for adequate power

### Biological Interpretation

**Why are most cell types showing no significant genes in DESeq2?**

This does NOT mean there are no biological effects. It means:

**Small sample size**: With only 4 vs 4 samples, DESeq2 has limited power to detect differences, especially if:

- Effect sizes are moderate (log2FC < 1)

- Patient-to-patient variability is high

- Changes are not consistent across all patients

**Real biology is complex**: Treatment responses may be:

- Patient-specific (some respond strongly, others don’t)

- Cell-state-dependent (only certain subpopulations respond)

- Transient (timing of sampling matters)

**Conservative approach is correct**: DESeq2’s stringent requirements ensure that genes we DO call significant are likely to be reproducible in independent experiments.

### Practical Recommendations

**For Publication:**

- Report DESeq2 results as your primary statistical findings

- Use FindMarkers results for hypothesis generation only

- Clearly state sample size limitations

- Consider the few DESeq2-significant genes as high-confidence hits

**For Follow-up Studies:**

- Validate DESeq2-significant genes with qPCR or protein assays

- Increase sample size (aim for >=8 per condition) for better power

- Consider genes significant in BOTH methods as especially robust

- Look for genes with large effect sizes in FindMarkers that approach significance in DESeq2

**For Interpretation:**

- The discrepancy highlights why pseudobulk analysis is essential

- Cell-level analysis alone would lead to many false positive claims

- Even with “no significant genes,” biological effects may exist but require larger studies to detect

This comparison underscores a fundamental principle: **statistical significance depends on both effect size AND sample size**. Our dataset reveals real biological complexity that would require larger studies to characterize fully.

## Part 4: Functional State Scoring Analysis

Rather than looking at individual genes, functional state scoring evaluates whether entire biological pathways are activated or suppressed in different conditions.

### Understanding Pathway Scoring

**The concept**: Many genes work together in coordinated biological programs. By scoring cells for expression of gene sets (pathways), we can identify functional states.

**Advantages over single-gene analysis:**

- More interpretable (pathways have biological meaning)

- More robust (averages over many genes)

- Can detect subtle coordinated changes

- Answers higher-level questions about cell state

### Downloading Gene Sets from MSigDB

```r
#-----------------------------------------------
# STEP 11: Download gene sets from MSigDB
#-----------------------------------------------

# Get Hallmark gene sets for humans
hallmark_gene_sets <- msigdbr(
  species = "Homo sapiens",
  category = "H"  # Hallmark gene sets
)

# Create named list format for scoring
# We'll use HALLMARK_INFLAMMATORY_RESPONSE as our example
hallmark_list <- hallmark_gene_sets %>%
  filter(gs_name == "HALLMARK_INFLAMMATORY_RESPONSE") %>%
  pull(gene_symbol)
```

> 

**MSigDB Hallmark Gene Sets**: Curated gene sets representing well-defined biological states or processes, derived from multiple sources and refined by experts.

### Scoring Cells for Inflammatory Response

```r
#-----------------------------------------------
# STEP 12: Score cells for inflammatory response pathway
#-----------------------------------------------

# Use Seurat's AddModuleScore to calculate pathway scores
seurat_obj <- AddModuleScore(
  seurat_obj,
  features = list(hallmark_list),
  name = "Inflammatory_Response",
  nbin = 24,
  ctrl = 100
)

# The score is added as a new metadata column
# Rename for clarity (Seurat adds "1" to the name)
colnames(seurat_obj@meta.data)[colnames(seurat_obj@meta.data) == "Inflammatory_Response1"] <- "Inflammatory_Response_Score"
```

> 

**AddModuleScore Parameters:**

- **features**: List of gene sets to score

- **name**: Prefix for score column names

- **nbin**: Number of expression bins for control gene selection

- **ctrl**: Number of control genes per bin

### Visualizing Pathway Scores by Cell Type and Condition

```r
#-----------------------------------------------
# STEP 13: Visualize inflammatory response scores
#-----------------------------------------------

# Ridge plot showing score distributions by cell type and condition
p_ridge <- ggplot(seurat_obj@meta.data, 
                  aes(x = Inflammatory_Response_Score, 
                      y = final_annotation, 
                      fill = condition)) +
  geom_density_ridges(alpha = 0.7) +
  theme_classic(base_size = 12) +
  labs(title = "Inflammatory Response Pathway Scores",
       x = "Pathway Score",
       y = "Cell Type",
       fill = "Condition") +
  scale_fill_manual(values = c("Healthy" = "#2E86AB", "Post_Treatment" = "#F18F01"))

ggsave("plots/functional_scoring/ridge_inflammatory_response.png", p_ridge,
       width = 10, height = 8, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/ridge_inflammatory_response.png?resize=1024%2C819&ssl=1)

### Statistical Testing of Pathway Scores (Pseudobulk Approach)

Just like with differential expression, we need to account for patient-level variation when comparing pathway scores statistically.

```r
#-----------------------------------------------
# STEP 14: Pseudobulk statistical testing of pathway scores
#-----------------------------------------------

# Aggregate scores by sample and cell type
score_aggregated <- seurat_obj@meta.data %>%
  group_by(sample_id, condition, final_annotation) %>%
  summarise(
    mean_score = mean(Inflammatory_Response_Score),
    .groups = "drop"
  )

# Function to test one cell type
test_pathway_celltype <- function(celltype, score_data) {
  # Filter to this cell type
  ct_data <- score_data %>% filter(final_annotation == celltype)

  if (nrow(ct_data) < 6) {
    return(data.frame(
      cell_type = celltype,
      mean_healthy = NA,
      mean_post = NA,
      diff = NA,
      p_value = NA,
      significant = FALSE
    ))
  }

  # Separate by condition
  healthy_scores <- ct_data %>% filter(condition == "Healthy") %>% pull(mean_score)
  post_scores <- ct_data %>% filter(condition == "Post_Treatment") %>% pull(mean_score)

  # Statistical test (t-test)
  test_result <- t.test(post_scores, healthy_scores)

  return(data.frame(
    cell_type = celltype,
    mean_healthy = mean(healthy_scores),
    mean_post = mean(post_scores),
    diff = mean(post_scores) - mean(healthy_scores),
    p_value = test_result$p.value
  ))
}

# Test all cell types
pathway_tests <- lapply(cell_types, test_pathway_celltype, 
                       score_data = score_aggregated)
pathway_results <- do.call(rbind, pathway_tests)

# Add FDR correction
pathway_results$FDR <- p.adjust(pathway_results$p_value, method = "BH")

# Save results
write.csv(pathway_results, "results/inflammatory_response_pathway_test.csv", 
          row.names = FALSE)
```

> 

**Interpretation:**

- **Positive diff**: Pathway score higher in Post-Treatment (pathway activated)

- **Negative diff**: Pathway score lower in Post-Treatment (pathway suppressed)

- **FDR < 0.05**: Statistically significant change in pathway activity

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/01/inflammatory_response_pathway_test.png?resize=742%2C282&ssl=1)

## Best Practices for Single-Cell Differential Analysis

### General Principles

**1. Always Use Multiple Approaches**

- Don’t rely on a single method (cell-level OR pseudobulk)

- Use complementary analyses (proportions + DE + pathways)

- Genes/pathways significant in multiple analyses are most robust

**2. Account for Biological Structure**

- Use pseudobulk for final statistical claims (avoids pseudoreplication)

- Include patient/batch as covariates in complex designs

- Consider paired designs if pre/post samples from same patients

**3. Interpret Within Biological Context**

- Fold-changes must be biologically meaningful (not just statistically significant)

- Small changes in key regulators can be more important than large changes in abundant genes

- Cell type-specific responses often differ (don’t expect all types to respond similarly)

**4. Validate Key Findings**

- Independent validation cohort (if available)

- Orthogonal technologies (qPCR, flow cytometry, immunofluorescence)

- Functional experiments (knockdown, overexpression)

### Differential Expression Best Practices

**When to Use Cell-Level (FindMarkers):**

- Initial exploration and biomarker discovery

- Visualizing gene expression patterns

- Single-sample experiments (no biological replicates)

- Identifying candidate genes for hypothesis generation

**When to Use Pseudobulk (DESeq2):**

- Final statistical claims for publication

- Experiments with >=3 biological replicates per condition

- Comparing across conditions with patient/batch structure

- Testing treatment effects while accounting for individual variation

**Statistical Thresholds:**

- **FDR < 0.05**: Standard threshold for significance

- **|log2FC| > 0.5**: Minimum meaningful fold-change (1.4-fold)

- **For publication**: Consider more stringent (FDR < 0.01, |log2FC| > 1)

**Gene Filtering:**

- Remove genes with very low expression (unreliable)

- DESeq2: >=10 reads in >=3 samples

- FindMarkers: >=10% cells expressing in at least one group

### Proportion Analysis Best Practices

**Sample Size Requirements:**

- **Minimum**: >=3 biological replicates per condition

- **Recommended**: >=5 replicates for robust statistical power

- More samples needed if expecting subtle changes

**Interpretation Guidelines:**

- Proportion ratio >1.5 or <0.67 typically indicates meaningful change

- Consider biological context (is change plausible?)

- Validate with orthogonal methods (flow cytometry, IHC)

### Functional Scoring Best Practices

**Choosing Gene Sets:**

- Use well-curated databases (MSigDB Hallmarks, GO, KEGG)

- Gene sets should have 15-200 genes (too small = noisy, too large = diluted)

- Match gene set species to your data

**Statistical Testing:**

- Always use pseudobulk approach (aggregate scores by sample)

- Use appropriate tests (t-test, paired t-test, linear models)

- Apply multiple testing correction across pathways

**Interpretation:**

- Score differences should be interpreted relative to baseline variation

- Consider both statistical significance and effect size

- Validate pathway-level findings with key genes

### Common Pitfalls to Avoid

**Pitfall 1: Using Cell-Level Statistics for Publication**

- **Problem**: Pseudoreplication inflates significance

- **Solution**: Always use pseudobulk for final claims

**Pitfall 2: Ignoring Multiple Testing Correction**

- **Problem**: Testing thousands of genes without FDR control

- **Solution**: Always report FDR-adjusted p-values

**Pitfall 3: Over-Interpreting Small Fold-Changes**

- **Problem**: p<0.05 but log2FC=0.1 (7% change) may not be meaningful

- **Solution**: Set fold-change thresholds (|log2FC| > 0.25 or 0.5)

**Pitfall 4: Insufficient Sample Size**

- **Problem**: <3 replicates per condition = underpowered

- **Solution**: Design experiments with >=5 biological replicates

**Pitfall 5: Comparing Across Cell Types Instead of Within**

- **Problem**: “Gene X is higher in monocytes vs T cells in treatment” confounds cell identity with condition

- **Solution**: Compare **within** cell types across conditions

**Pitfall 6: Ignoring Batch Effects in Pseudobulk**

- **Problem**: Samples processed on different days may have technical variation

- **Solution**: Include batch as covariate in DESeq2 design

- **Example**: `design = ~ batch + condition`

**Pitfall 7: Not Reporting Method Details**

- **Problem**: Reviewers/readers can’t evaluate or reproduce analysis

- **Solution**: Report all parameters clearly:

- Statistical test used

- Thresholds (FDR, fold-change)

- Number of biological replicates

- Software versions

## Conclusion: From Differential Analysis to Biological Discovery

Congratulations! You’ve completed comprehensive differential analysis of your scRNA-seq data, integrating three complementary approaches:

- **Differential proportions** – Which immune cells change in abundance?

- **Differential expression** – Which genes change within each cell type?

- **Functional state scoring** – Which biological pathways are activated/suppressed?

### Resources for Continued Learning

**Statistical Methods:**

- DESeq2 vignette: [https://bioconductor.org/packages/DESeq2/](https://bioconductor.org/packages/DESeq2/)

- limma user guide: [https://bioconductor.org/packages/limma/](https://bioconductor.org/packages/limma/)

- Propeller documentation: [https://www.bioconductor.org/packages/speckle/](https://www.bioconductor.org/packages/speckle/)

- Pseudobulk best practices: Squair et al., Nat Commun 2021

**Gene Set Analysis:**

- MSigDB database: [https://www.gsea-msigdb.org/](https://www.gsea-msigdb.org/)

- GSEA methodology: Subramanian et al., PNAS 2005

- AUCell for single-cell: Aibar et al., Nat Methods 2017

**Seurat 5 Documentation:**

- PseudobulkExpression function: [https://satijalab.org/seurat/reference/pseudobulkexpression](https://satijalab.org/seurat/reference/pseudobulkexpression)

- Seurat v5 vignettes: [https://satijalab.org/seurat/articles/](https://satijalab.org/seurat/articles/)

**Visualization:**

- EnhancedVolcano: [https://bioconductor.org/packages/EnhancedVolcano/](https://bioconductor.org/packages/EnhancedVolcano/)

- ComplexHeatmap: [https://jokergoo.github.io/ComplexHeatmap-reference/book/](https://jokergoo.github.io/ComplexHeatmap-reference/book/)

**Best Practices:**

- Luecken & Theis (2019) – Current best practices in scRNA-seq

- Hao et al. (2024) – Seurat v5 integration and analysis

- Crowell et al. (2020) – Muscat: Multi-sample multi-group scRNA-seq

## References

1. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. Nat Biotechnol. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

2. Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biol. 2014;15(12):550. doi:10.1186/s13059-014-0550-8 [DESeq2]

3. Ritchie ME, Phipson B, Wu D, et al. limma powers differential expression analyses for RNA-sequencing and microarray studies. Nucleic Acids Res. 2015;43(7):e47. doi:10.1093/nar/gkv007 [limma]

4. Law CW, Chen Y, Shi W, Smyth GK. voom: precision weights unlock linear model analysis tools for RNA-seq read counts. Genome Biol. 2014;15(2):R29. doi:10.1186/gb-2014-15-2-r29 [voom]

5. Squair JW, Gautier M, Kathe C, et al. Confronting false discoveries in single-cell differential expression. Nat Commun. 2021;12(1):5692. doi:10.1038/s41467-021-25960-2 [Pseudobulk best practices]

6. Crowell HL, Soneson C, Germain PL, et al. muscat detects subpopulation-specific state transitions from multi-sample multi-condition single-cell transcriptomics data. Nat Commun. 2020;11(1):6077. doi:10.1038/s41467-020-19894-4 [Muscat – pseudobulk DE]

7. Phipson B, Maksimovic J, Oshlack A. propeller: testing for differences in cell type proportions in single cell data. Bioinformatics. 2022;38(20):4720-4726. doi:10.1093/bioinformatics/btac582 [Propeller]

8. Liberzon A, Birger C, Thorvaldsdóttir H, et al. The Molecular Signatures Database (MSigDB) hallmark gene set collection. Cell Syst. 2015;1(6):417-425. doi:10.1016/j.cels.2015.12.004 [MSigDB Hallmarks]

9. Subramanian A, Tamayo P, Mootha VK, et al. Gene set enrichment analysis: a knowledge-based approach for interpreting genome-wide expression profiles. Proc Natl Acad Sci USA. 2005;102(43):15545-15550. doi:10.1073/pnas.0506580102 [GSEA]

10. Hänzelmann S, Castelo R, Guinney J. GSVA: gene set variation analysis for microarray and RNA-seq data. BMC Bioinformatics. 2013;14:7. doi:10.1186/1471-2105-14-7 [GSVA]

11. Aibar S, González-Blas CB, Moerman T, et al. SCENIC: single-cell regulatory network inference and clustering. Nat Methods. 2017;14(11):1083-1086. doi:10.1038/nmeth.4463 [AUCell scoring]

12. Luecken MD, Theis FJ. Current best practices in single-cell RNA-seq analysis: a tutorial. Mol Syst Biol. 2019;15(6):e8746. doi:10.15252/msb.20188746

13. Zimmerman KD, Espeland MA, Langefeld CD. A practical solution to pseudoreplication bias in single-cell studies. Nat Commun. 2021;12(1):738. doi:10.1038/s41467-021-21038-1

14. Soneson C, Robinson MD. Bias, robustness and scalability in single-cell differential expression analysis. Nat Methods. 2018;15(4):255-261. doi:10.1038/nmeth.4612

15. Finak G, McDavid A, Yajima M, et al. MAST: a flexible statistical framework for assessing transcriptional changes and characterizing heterogeneity in single-cell RNA sequencing data. Genome Biol. 2015;16:278. doi:10.1186/s13059-015-0844-5 [MAST]

16. Van den Berge K, Perraudeau F, Soneson C, et al. Observation weights unlock bulk RNA-seq tools for zero inflation and single-cell applications. Genome Biol. 2018;19(1):24. doi:10.1186/s13059-018-1406-4 [zinbwave]

17. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. J Transl Med. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source]

---

***This tutorial is part of the NGS101.com series on single cell sequencing analysis. If this tutorial helped advance your research, **please comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.***
