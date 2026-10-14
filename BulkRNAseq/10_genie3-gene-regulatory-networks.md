# How to Build Gene Regulatory Networks from RNA-seq Data Using GENIE3 – Complete Step-by-Step Guide For Absolute Beginners

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/)

- [Introduction: Understanding Gene Regulatory Network Inference](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Introduction_Understanding_Gene_Regulatory_Network_Inference)

  - [What is GENIE3 and What Can It Do?](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#What_is_GENIE3_and_What_Can_It_Do)
  - [How is GENIE3 Different from WGCNA?](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#How_is_GENIE3_Different_from_WGCNA)
  - [What Biological Insights Can We Gain from GENIE3 Networks?](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#What_Biological_Insights_Can_We_Gain_from_GENIE3_Networks)
  - [How Does GENIE3 Reconstruct Gene Regulatory Networks?](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#How_Does_GENIE3_Reconstruct_Gene_Regulatory_Networks)
  - [When Should You Use GENIE3?](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#When_Should_You_Use_GENIE3)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Setting_Up_Your_Analysis_Environment)

  - [Installing Required R Packages](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Installing_Required_R_Packages)

- [Loading the Example Dataset](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Loading_the_Example_Dataset)

  - [Loading Pre-processed Data](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Loading_Pre-processed_Data)

- [Data Preparation for GENIE3](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Data_Preparation_for_GENIE3)

  - [Understanding GENIE3 Input Requirements](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Understanding_GENIE3_Input_Requirements)
  - [Filtering and Formatting the Expression Matrix](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Filtering_and_Formatting_the_Expression_Matrix)
  - [Using GENIE3 with Custom Gene Lists Beyond Transcription Factors](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Using_GENIE3_with_Custom_Gene_Lists_Beyond_Transcription_Factors)
  - [Choosing Between Raw and Normalized Expression Data](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Choosing_Between_Raw_and_Normalized_Expression_Data)

- [Building the Gene Regulatory Network with GENIE3](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Building_the_Gene_Regulatory_Network_with_GENIE3)

  - [Running GENIE3 Network Inference](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Running_GENIE3_Network_Inference)
  - [Extracting and Ranking Regulatory Links](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Extracting_and_Ranking_Regulatory_Links)
  - [Interpreting Network Output](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Interpreting_Network_Output)

- [Downstream Analysis: Identifying Key Regulatory Relationships](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Downstream_Analysis_Identifying_Key_Regulatory_Relationships)

  - [Identifying Top Regulatory Transcription Factors](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Identifying_Top_Regulatory_Transcription_Factors)
  - [Analyzing Targets of Top Transcription Factor](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Analyzing_Targets_of_Top_Transcription_Factor)
  - [Comparing Regulatory Networks Across Conditions](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Comparing_Regulatory_Networks_Across_Conditions)

- [Network Visualization and Interpretation](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Network_Visualization_and_Interpretation)

  - [Creating a Network Graph of Top Predictions](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Creating_a_Network_Graph_of_Top_Predictions)
  - [Analyzing Network Motifs and Structure](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Analyzing_Network_Motifs_and_Structure)
  - [Comparing with Known TF-Target Databases](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Comparing_with_Known_TF-Target_Databases)

- [Interpreting GENIE3 Results: From Predictions to Biology](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Interpreting_GENIE3_Results_From_Predictions_to_Biology)

  - [Understanding Prediction Confidence](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Understanding_Prediction_Confidence)
  - [Biological Interpretation Guidelines](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Biological_Interpretation_Guidelines)

- [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Common_Pitfalls_and_How_to_Avoid_Them)
- [Conclusion: From Network Predictions to Biological Discovery](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Conclusion_From_Network_Predictions_to_Biological_Discovery)
- [References](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#References)

  - [Share this:](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Share_this)
  - [Like this:](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/#Like_this)

## Introduction: Understanding Gene Regulatory Network Inference

In the complex choreography of cellular function, transcription factors (TFs) act as master conductors, orchestrating when and where genes are expressed. Understanding which transcription factors regulate which target genes is fundamental to deciphering how cells respond to stimuli, how developmental programs unfold, and how diseases emerge from regulatory dysfunction. Gene regulatory network (GRN) inference provides a computational framework for uncovering these transcription factor-target gene relationships from gene expression data.

### What is GENIE3 and What Can It Do?

GENIE3 (GEne Network Inference with Ensemble of trees) is a machine learning-based algorithm that infers gene regulatory networks from expression data. Unlike correlation-based methods that simply measure co-expression, GENIE3 attempts to identify **directed regulatory relationships**—that is, which genes (particularly transcription factors) regulate which other genes.

The fundamental question GENIE3 addresses is: “Given the expression patterns of all genes across multiple samples, which transcription factors are likely regulating each target gene?”

GENIE3 answers this question using an elegant approach based on random forest regression. For each gene in your dataset, GENIE3 treats it as a potential target and asks: “Can I predict this gene’s expression from the expression of transcription factors?” Genes whose expression patterns help predict the target gene’s expression are inferred to be its regulators.

> 

**Beginner’s Tip:** Think of GENIE3 as a detective investigating which transcription factors control each gene. If knowing a TF’s expression level helps predict a target gene’s expression level, that TF likely regulates that target. The relationship is directional: TF → target gene, not just “these genes are correlated.”

### How is GENIE3 Different from WGCNA?

If you’ve followed our previous tutorial on [WGCNA](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna/), you might wonder: aren’t we already building gene networks? What makes GENIE3 different?

**WGCNA (Weighted Gene Co-expression Network Analysis):**

- **Approach:** Correlation-based; identifies groups of genes with similar expression patterns

- **Network Type:** Undirected co-expression network (Gene A ↔ Gene B)

- **Output:** Modules of co-expressed genes

- **Strength:** Excellent for identifying functional gene groups and pathway-level insights

- **Limitation:** Cannot determine which genes regulate which—only that they’re co-expressed

- **Best For:** Discovering gene modules associated with conditions or phenotypes

**GENIE3 (Gene Regulatory Network Inference):**

- **Approach:** Machine learning-based (random forests); infers regulatory relationships

- **Network Type:** Directed regulatory network (TF → Target Gene)

- **Output:** Ranked list of TF-target interactions with importance scores

- **Strength:** Identifies which transcription factors likely regulate which genes

- **Limitation:** Requires predefined list of transcription factors; more computationally intensive

- **Best For:** Discovering transcription factor regulatory programs and master regulators

**Visual Comparison:**

```
WGCNA Network:
Gene A ←→ Gene B ←→ Gene C
(All genes are equal; relationships are symmetric)

GENIE3 Network:
TF1 → Gene A
TF1 → Gene B
TF2 → Gene B
TF2 → Gene C
(Transcription factors regulate targets; relationships are directional)
```

**When to Use Each Method:**

- **Use WGCNA** when you want to identify groups of co-expressed genes and associate them with experimental conditions or phenotypes

- **Use GENIE3** when you want to identify which transcription factors regulate which genes, discover master regulators, or understand regulatory cascades

- **Use Both** for comprehensive analysis: WGCNA for module discovery, GENIE3 for regulatory mechanisms within those modules

### What Biological Insights Can We Gain from GENIE3 Networks?

GENIE3 regulatory networks enable several types of biological discovery:

**1. Master Regulator Identification**

By analyzing which transcription factors have the most predicted targets or the strongest regulatory influence, GENIE3 can identify master regulators—TFs that control large regulatory programs. These master regulators are often:

- Key drivers of cell state transitions (e.g., differentiation, activation)

- Central nodes in disease pathways (e.g., oncogenic TFs in cancer)

- Promising therapeutic targets due to their broad influence

Connection to our [Master Regulator Analysis tutorial](https://ngs101.com/how-to-perform-master-regulator-analysis-on-rna-seq-data-using-regenrich-and-rtn-a-complete-beginners-guide/): While that tutorial focused on identifying master regulators from differential expression and known TF-target databases, GENIE3 infers regulatory relationships directly from your data, potentially discovering novel regulatory connections not yet documented in databases.

**2. Regulatory Cascade Discovery**

GENIE3 can reveal regulatory hierarchies where upstream TFs regulate downstream TFs, which in turn regulate effector genes. Understanding these cascades helps explain:

- How signals propagate through gene regulatory networks

- The temporal order of gene regulation during biological processes

- Where interventions might be most effective (targeting upstream vs. downstream regulators)

**3. Condition-Specific Regulatory Mechanisms**

By building separate GENIE3 networks for different experimental conditions (e.g., wild-type vs. mutant, treated vs. control), you can identify:

- TFs whose regulatory activity changes between conditions

- Rewiring of regulatory networks in response to perturbations

- Condition-specific regulatory programs

**4. Transcription Factor Function Prediction**

For poorly characterized transcription factors, GENIE3 predictions can suggest function based on their predicted target genes:

- If a TF regulates cell cycle genes, it likely functions in proliferation

- If a TF regulates metabolic genes, it may control metabolic state

- Target gene enrichment analysis provides functional hypotheses for validation

**5. Integration with Other Data Types**

GENIE3 predictions can be integrated with:

- **ChIP-seq data:** Validate predicted TF-target relationships with direct binding evidence

- **ATAC-seq/DNase-seq:** Check if predicted targets have accessible TF binding sites

- **Perturbation experiments:** Test if TF knockout affects predicted targets

- **Known regulatory databases:** Compare predictions with curated TF-target interactions

### How Does GENIE3 Reconstruct Gene Regulatory Networks?

Understanding GENIE3’s algorithm helps interpret its results and set appropriate parameters. Let’s walk through the methodology step by step.

**Step 1: Problem Formulation**

GENIE3 treats gene regulatory network inference as a supervised machine learning problem. For each gene g in the dataset:

- **Outcome variable (Y):** Expression levels of gene g across all samples

- **Predictor variables (X):** Expression levels of all candidate transcription factors across all samples

- **Goal:** Build a model that predicts Y from X

If the expression of transcription factor TF₁ helps predict the expression of gene g, GENIE3 infers that TF₁ likely regulates g.

**Step 2: Random Forest Regression**

For each target gene, GENIE3 trains a random forest regression model:

**What is Random Forest?**

Random forest is an ensemble learning method that combines many decision trees:

1. **Create multiple decision trees:** Each tree is trained on a random subset of samples (bootstrap sampling)

2. **Random feature selection:** At each split in a tree, only consider a random subset of TFs

3. **Aggregate predictions:** The final prediction is the average of all trees’ predictions

**Why Random Forests for Regulatory Network Inference?**

- **Non-linear relationships:** Can capture complex regulatory logic (e.g., combinatorial regulation)

- **Feature importance:** Automatically ranks which TFs are most important for predicting each target

- **Robust to noise:** Ensemble approach reduces overfitting and handles noisy expression data

- **No assumptions:** Doesn’t require linear relationships or normal distributions

**Step 3: Feature Importance Calculation**

After training the random forest for a target gene, GENIE3 calculates the importance of each transcription factor:

- **Importance Score:** Measures how much a TF contributes to predicting the target gene’s expression

- **Calculation:** Based on how much prediction error increases when that TF’s values are randomly permuted

- **Interpretation:** Higher importance = stronger regulatory influence

**Step 4: Network Construction**

GENIE3 repeats this process for every gene in the dataset:

```
For gene 1: Train random forest → Get TF importance scores → Create ranked list of regulators
For gene 2: Train random forest → Get TF importance scores → Create ranked list of regulators
...
For gene N: Train random forest → Get TF importance scores → Create ranked list of regulators
```

The output is a regulatory network where:

- **Nodes:** Genes (both TFs and targets)

- **Directed edges:** TF → target gene

- **Edge weights:** Importance scores indicating regulatory strength

**Step 5: Ranking and Thresholding**

GENIE3 produces a ranked list of all possible TF-target pairs sorted by importance score. Users can then:

- **Select top N interactions:** Take the top 1,000 or 10,000 highest-confidence predictions

- **Apply threshold:** Keep only interactions with importance score above a cutoff

- **Analyze TF-specific networks:** Extract all predicted targets for specific TFs of interest

**Key Algorithmic Features:**

1. **Independence assumption:** Each target gene is modeled independently, allowing parallelization

2. **Ensemble approach:** Combines multiple weak learners (trees) into a strong predictor

3. **Feature selection:** Automatically identifies which TFs matter for each target

4. **Scalability:** Can handle thousands of genes and samples

5. **No prior knowledge required:** Infers relationships purely from expression data

**Comparison to Correlation-Based Methods:**

```
Correlation-Based (WGCNA):
- Measures: Similarity in expression patterns
- Captures: Co-expression
- Example: If TF1 and Gene A both increase together, they're correlated
- Cannot distinguish: TF1 → Gene A vs. Gene A → TF1 vs. both regulated by third factor

GENIE3 (Machine Learning):
- Measures: Predictive power of TF expression for target expression
- Captures: Directed regulatory relationships
- Example: If knowing TF1 helps predict Gene A, infer TF1 → Gene A
- Provides: Directionality and confidence scores
```

**Limitations to Keep in Mind:**

- **Indirect regulation:** GENIE3 may predict indirect relationships (TF₁ → TF₂ → Gene)

- **Correlation vs. causation:** High importance doesn’t prove direct binding (could be indirect)

- **Static snapshots:** Uses steady-state expression data, may miss dynamic regulation

- **No temporal information:** Cannot determine order of events without time-series data

- **Requires TF list:** You must provide a list of candidate transcription factors

### When Should You Use GENIE3?

GENIE3 is particularly powerful in several scenarios:

**Ideal Use Cases:**

1. **Identifying Transcriptional Regulators:**

- You want to know which TFs drive gene expression changes in your experiment

- You’re studying transcriptional responses to stimuli or perturbations

- You need to identify master regulators of cell state transitions

2. **Hypothesis Generation for Validation:**

- Planning ChIP-seq or CUT&RUN experiments and need candidate TF-target pairs

- Designing perturbation experiments (which TFs to knock down/overexpress)

- Generating hypotheses about poorly characterized transcription factors

3. **Comparative Regulatory Network Analysis:**

- Comparing regulatory networks between conditions (e.g., disease vs. healthy)

- Identifying rewired regulatory relationships in genetic perturbations

- Understanding how regulatory programs differ across cell types or developmental stages

4. **Integration with Other Analyses:**

- Following up WGCNA module analysis to identify regulators within modules

- Complementing differential expression analysis with regulatory mechanism insights

- Combining with ChIP-seq to validate predicted TF-target interactions

**When NOT to Use GENIE3:**

- **Very small sample sizes (<10-15 samples):** Machine learning requires sufficient data

- **Only interested in co-expression patterns:** Use WGCNA instead (faster, simpler)

- **No transcription factors in your gene list:** GENIE3 specifically infers TF regulation

- **Need guaranteed causation:** GENIE3 predicts associations, not proven causal relationships

**Complementary Approach:**

The most powerful strategy combines multiple methods:

1. **Start with WGCNA:** Identify modules of co-expressed genes

2. **Apply GENIE3:** Infer which TFs regulate genes in interesting modules

3. **Validate predictions:** Use ChIP-seq, perturbation experiments, or literature

4. **Integrate with Master Regulator Analysis:** Compare GENIE3 predictions with database-driven approaches

In the following tutorial, we’ll walk through using GENIE3 to infer regulatory networks from RNA-seq data, identify key transcription factors, analyze their predicted targets, and integrate results with other network analysis approaches.

## Setting Up Your Analysis Environment

Before diving into network inference, we need to set up our R environment with the necessary packages. GENIE3 is available through Bioconductor and has relatively few dependencies compared to comprehensive network analysis suites.

### Installing Required R Packages

Let’s install all the packages needed for the complete GENIE3 analysis workflow:

```r
#-----------------------------------------------
# STEP 0: Install all required R packages
#-----------------------------------------------

# Set CRAN mirror to avoid installation prompts
options(repos = c(CRAN = "https://cloud.r-project.org"))

# Install BiocManager for Bioconductor packages
if (!requireNamespace("BiocManager", quietly = TRUE)) {
  install.packages("BiocManager")
}

# Install GENIE3 from Bioconductor
BiocManager::install("GENIE3")

# Install dorothea for TF annotations and validation
BiocManager::install("dorothea")

# Install packages for network analysis and visualization
install.packages(c(
  "igraph",           # Network analysis and visualization
  "ggplot2",          # Advanced plotting
  "ggrepel",          # Non-overlapping labels in plots
  "reshape2",         # Data manipulation
  "pheatmap",         # Heatmap visualization
  "RColorBrewer",     # Color palettes
  "dplyr"             # Data manipulation
))

# Install Bioconductor packages for gene annotation and enrichment
BiocManager::install(c(
  "org.Hs.eg.db",     # Human gene annotations
  "clusterProfiler"   # Functional enrichment analysis
))

# Load all required libraries
library(GENIE3)
library(dorothea)
library(igraph)
library(ggplot2)
library(ggrepel)
library(reshape2)
library(pheatmap)
library(RColorBrewer)
library(dplyr)
library(clusterProfiler)
library(org.Hs.eg.db)

# Set working directory
setwd("~/GSE261875_GENIE3")

# Create directories for outputs
dir.create("plots", showWarnings = FALSE)
dir.create("results", showWarnings = FALSE)
```

## Loading the Example Dataset

For this tutorial, we’ll use the same dataset as our previous [clustering](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/) and [WGCNA](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/) tutorials: **GSE261875**, which examines TDP-43 perturbation in motor neurons with and without Ataxin-2 knockout. This dataset is ideal for regulatory network inference because it contains multiple experimental conditions where transcriptional regulation likely differs.

### Loading Pre-processed Data

Load the normalized expression matrix and sample information generated from our previous [clustering tutorial](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/):

```r
#-----------------------------------------------
# STEP 1: Load pre-processed RNA-seq data
#-----------------------------------------------

# Load normalized expression data (VST or rlog-transformed counts)
expr_data <- read.table("normalized_counts.tsv", 
                        header = TRUE, 
                        row.names = 1)

# Load sample information
pheno_data <- read.table("sample_info.tsv", 
                         header = TRUE, 
                         row.names = 1)

# Load gene annotation (ENSEMBL IDs and gene symbols)
gene_data <- read.table("gene_data.tsv", 
                        header = TRUE)
```

> 

**Data Format Note:** The normalized_counts.tsv file should contain variance-stabilized (VST) or regularized-log (rlog) transformed counts from DESeq2, not raw counts. These transformations produce data on a log-like scale suitable for GENIE3’s regression framework.

## Data Preparation for GENIE3

GENIE3 has specific input requirements and works best with properly filtered and formatted data. Let’s prepare our expression matrix and transcription factor list.

### Understanding GENIE3 Input Requirements

**Expression Matrix Format:**

GENIE3 requires a gene-by-sample matrix where:

- **Rows:** Genes (both transcription factors and potential targets)

- **Columns:** Samples

- **Values:** Normalized expression levels (not raw counts)

- **No missing values:** All entries must be numeric

This is the **opposite** orientation from WGCNA, which uses sample-by-gene matrices.

**Transcription Factor List:**

You must provide a list of candidate transcription factors (TFs). GENIE3 will:

1. Use these TFs as potential regulators (predictors)

2. Model each gene (including TFs themselves) as potential targets

**Where to Get Transcription Factor Lists:**

Several curated databases provide transcription factors:

1. **[DoRothEA](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/saezlab.github.io/dorothea/)**: Curated regulons for human/mouse GRN analysis

2. **[Stein Aerts Lab](https://ngs101.com/how-to-build-gene-regulatory-networks-from-rna-seq-data-using-genie3-complete-step-by-step-guide-for-absolute-beginners/resources.aertslab.org/cistarget/tf_lists/):** Human/mouse/fly TF lists

3. **[University of Toronto](https://humantfs.ccbr.utoronto.ca/download.php):** Human TFs

For this tutorial, we’ll use a curated list of human transcription factors from DoRothEA.

### Filtering and Formatting the Expression Matrix

Our expression data from the clustering tutorial has already been quality controlled and normalized (VST transformation). We just need to apply minimal variance filtering and ensure proper formatting for GENIE3:

```r
#-----------------------------------------------
# STEP 2: Filter and format expression data
#-----------------------------------------------

# Filter genes with very low variance (bottom 25%)
# These genes show minimal variation and provide little regulatory information
gene_variance <- apply(expr_data, 1, var)
variance_threshold <- quantile(gene_variance, 0.25)

expr_filtered <- expr_data[gene_variance > variance_threshold, ]
```

> 

**Variance Filtering Rationale:** We remove only the bottom 25% of genes by variance—those that barely change across samples. These genes are unlikely to reveal meaningful regulatory relationships since effective regulation requires expression changes. Keeping 75% of genes ensures we retain:

- **Moderately variable genes:** May show condition-specific regulation

- **Tissue-specific genes:** Important for cell-type identity

- **Lowly expressed but regulated genes:** Can be biologically critical

For most analyses with 20-50 samples, removing the bottom 25% is a reasonable compromise.

### Using GENIE3 with Custom Gene Lists Beyond Transcription Factors

While GENIE3 is typically used to infer transcription factor regulation, it can actually work with **any set of genes** as potential regulators. This flexibility enables several advanced applications:

**Alternative Regulator Sets:**

1. **Hub genes from WGCNA:** Use highly connected genes from co-expression modules as regulators

2. **Signaling molecules:** Study regulation by kinases, receptors, or secreted factors

3. **microRNA targets:** Investigate post-transcriptional regulation (using miRNA expression)

4. **Pathway genes:** Focus on specific pathways (e.g., immune response, metabolism)

5. **Differentially expressed genes:** Limit to genes changing between conditions

**When to Use Custom Gene Lists:**

1. **Hypothesis-driven analysis:** Test if specific genes regulate a pathway of interest

2. **Integration with other analyses:** Follow up WGCNA or differential expression

3. **Computational efficiency:** Smaller regulator sets = faster computation

4. **Specific biological questions:** “Do interferon-stimulated genes regulate each other?”

**Important Considerations:**

- **Biological plausibility:** Ensure candidate regulators can plausibly regulate other genes

- **Expression variation:** Regulators should vary across samples (constant expression is uninformative)

- **Sample size:** Smaller regulator sets work better with limited samples

- **Interpretation:** Non-TF regulators may indicate indirect relationships or non-transcriptional regulation

> 

**Key Insight:** GENIE3’s random forest approach doesn’t require regulators to be transcription factors. Any gene whose expression pattern helps predict another gene’s expression will be identified as a potential regulator. However, interpreting non-TF regulation requires careful consideration of biological mechanisms—these may represent co-regulation by common upstream factors rather than direct regulation.

```r
#-----------------------------------------------
# STEP 3: Prepare transcription factor list
#-----------------------------------------------

# Get human transcription factors from dorothea database
dorothea_data <- dorothea::dorothea_hs

# Extract unique transcription factor gene symbols
all_tf_symbols <- unique(dorothea_data$tf)

# Find which TFs are present in our expression data
tf_info <- gene_data[gene_data$SYMBOL %in% all_tf_symbols, 
                     c("ENSEMBL", "SYMBOL")]

# Keep only TFs that are in our filtered expression matrix
tf_genes <- tf_info$ENSEMBL
tf_genes <- tf_genes[tf_genes %in% rownames(expr_filtered)]

# Save complete TF list for reference
write.table(tf_info[tf_info$ENSEMBL %in% tf_genes, ],
            "results/transcription_factors_list.tsv",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/tf_info.png?resize=344%2C224&ssl=1)

> 

**TF List Strategy:** We use the `dorothea` package which provides a comprehensive, manually curated collection of human transcription factors with confidence levels based on literature evidence. This database includes ~1,500 human TFs, covering:

- Sequence-specific DNA-binding TFs

- General transcription factors

- Chromatin remodeling factors with regulatory roles

### Choosing Between Raw and Normalized Expression Data

An important question: should we use raw counts or normalized data for GENIE3?

**Answer: GENIE3 can work with both, but normalized data is recommended for RNA-seq**

GENIE3’s random forest approach is quite robust and can handle various data types. However, the choice depends on your data source and goals:

**For RNA-seq Data (our case): Use Normalized Counts**

**Recommended: VST or rlog-transformed counts**

```r
# In your preprocessing (from clustering tutorial):
# dds <- DESeqDataSetFromMatrix(...)
# vsd <- vst(dds)  # or rlog(dds)
# normalized_counts <- assay(vsd)
```

**Why normalization is better for RNA-seq:**

1. **Removes technical variation:** Raw RNA-seq counts vary due to library size differences (sequencing depth), which is technical noise unrelated to biology

2. **Stabilizes variance:** High-count genes have higher variance than low-count genes in raw data. VST/rlog makes variance more uniform across expression levels

3. **Puts genes on comparable scales:** Prevents highly expressed housekeeping genes from dominating the importance calculations

4. **Better for regression:** Random forests work better when features (genes) are on similar scales

**Can you use raw counts?**

Yes, GENIE3 will still work with raw counts, especially if:

- You set `normalize = TRUE` in GENIE3 (performs internal sample-wise normalization)

- You have deep, evenly sequenced samples

- You’re comparing predictions within the same dataset (not across studies)

However, **results will be less reliable** because technical variation from library sizes can create spurious regulatory relationships.

**What NOT to use:**

- **TPM or FPKM:** These length-normalized metrics are better than raw counts but still heteroskedastic (variance depends on expression level)

- **Z-score normalization:** This removes the mean and may eliminate biological signal that GENIE3 needs

- **Quantile normalization:** Too aggressive for GENIE3; assumes all samples have identical distributions

## Building the Gene Regulatory Network with GENIE3

Now we’re ready to run GENIE3 to infer regulatory relationships. This section covers parameter settings, execution, and output interpretation.

### Running GENIE3 Network Inference

Let’s run GENIE3 with our prepared data:

```r
#-----------------------------------------------
# STEP 4: Run GENIE3 network inference
#-----------------------------------------------

# Convert filtered expression matrix to format required by GENIE3
# GENIE3 needs: rows = genes, columns = samples
expr_matrix <- as.matrix(expr_filtered)

# Run GENIE3 network inference
# Set seed for reproducibility
set.seed(123)

# Run GENIE3 with our data
net <- GENIE3(
  expr_matrix,           # Expression matrix (genes x samples)
  regulators = tf_genes, # List of TF genes to use as regulators
  nTrees = 1000,         # Number of trees in random forest (more = more stable)
  K = "sqrt",            # Number of TFs randomly selected at each node split
  nCores = 8,            # Use 8 CPU cores for parallel processing
  verbose = TRUE         # Print progress messages
)

# Save the network matrix to disk
saveRDS(net, "results/genie3_network_matrix.rds")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_net.png?resize=1024%2C171&ssl=1)

**What Happens During Execution:**

1. For each gene (target), GENIE3 trains a random forest model

2. The model predicts target expression from TF expression patterns

3. Feature importance scores measure each TF’s contribution

4. Results are stored in a gene × TF matrix of importance scores

**Output Structure:**

The `net` object is a matrix where:

- **Rows:** All genes (potential targets)

- **Columns:** Transcription factors (regulators)

- **Values:** Importance scores indicating regulatory strength

### Extracting and Ranking Regulatory Links

The raw network matrix contains importance scores for all TF-target pairs. Let’s extract the most confident predictions:

```r
#-----------------------------------------------
# STEP 5: Extract and rank regulatory interactions
#-----------------------------------------------

# Convert network matrix to a ranked list of interactions
link_list <- getLinkList(net)

# The link list has three columns:
# - regulatoryGene: TF ENSEMBL ID
# - targetGene: Target gene ENSEMBL ID  
# - weight: Importance score

# Add gene symbols for interpretability
link_list$regulatorSymbol <- gene_data$SYMBOL[
  match(link_list$regulatoryGene, gene_data$ENSEMBL)
]
link_list$targetSymbol <- gene_data$SYMBOL[
  match(link_list$targetGene, gene_data$ENSEMBL)
]

# Reorder columns for clarity
link_list <- link_list[, c("regulatoryGene", "regulatorSymbol", 
                           "targetGene", "targetSymbol", "weight")]

# Save full link list
write.table(link_list, 
            "results/genie3_all_links.tsv",
            sep = "\t", 
            row.names = FALSE, 
            quote = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_link_list.png?resize=1024%2C214&ssl=1)

> 

**Thresholding Strategy:** There’s no universal threshold for importance scores. Options include:

- **Top N links:** Select top 1,000 or 10,000 interactions

- **Top percentage:** Take top 0.1% or 1% of links

- **Absolute threshold:** Keep links with weight > X (requires calibration)

- **Permutation testing:** Compare to null distribution (computationally intensive)

For exploratory analysis, taking the top 0.1-1% of links is reasonable. For validation experiments, focus on the very top predictions (top 100-500 links).

### Interpreting Network Output

**Understanding Importance Scores:**

The importance score reflects how much a TF’s expression helps predict the target’s expression:

- **High scores (top 0.1%):** Strong evidence for regulatory relationship

- **Medium scores:** Possible regulation but less confident

- **Low scores:** Little evidence for direct regulation

**Important Caveats:**

1. **Correlation ≠ Causation:** High importance suggests regulation but doesn’t prove it

2. **Direct vs. Indirect:** GENIE3 may predict indirect relationships (TF₁ → TF₂ → Target)

3. **False positives:** Not all predictions are real regulatory relationships

4. **False negatives:** GENIE3 may miss true regulations (especially with limited samples)

5. **Context-specific:** Predictions reflect the conditions in your samples

**Validation Strategies:**

To increase confidence in predictions:

- **Check literature:** Are predicted interactions documented?

- **Compare with ChIP-seq:** Does the TF bind near the target gene?

- **Look for TF binding motifs:** Are motifs present in target promoters?

- **Perturbation experiments:** Does TF knockdown affect target expression?

- **Cross-reference databases:** Do curated TF-target databases support predictions?

## Downstream Analysis: Identifying Key Regulatory Relationships

With our inferred network, we can now identify important transcription factors and characterize their regulatory programs.

### Identifying Top Regulatory Transcription Factors

Which transcription factors have the most predicted targets and strongest regulatory influence?

```r
#-----------------------------------------------
# STEP 6: Identify master regulatory transcription factors
#-----------------------------------------------

# Identify high-confidence predictions using an absolute weight threshold
# Take only the top predictions across all TF-target pairs

# Use top 1% of all predictions as high-confidence threshold
weight_threshold <- quantile(link_list$weight, 0.99)

high_confidence_links <- link_list %>%
  filter(weight >= weight_threshold)

# Calculate statistics for each TF based on their high-confidence targets
tf_stats <- high_confidence_links %>%
  group_by(regulatorSymbol) %>%
  summarise(
    n_targets = n(),
    mean_weight = mean(weight),
    median_weight = median(weight),
    max_weight = max(weight),
    sum_weight = sum(weight),
    .groups = 'drop'
  ) %>%
  arrange(desc(sum_weight))

# Save TF statistics
write.table(tf_stats,
            "results/tf_regulatory_statistics.tsv",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_tf_stats.png?resize=1024%2C189&ssl=1)

> 

**Understanding the Results:**

- **Weight threshold:** We use the 99th percentile of all importance scores as a cutoff for “high-confidence” predictions

- **n_targets:** Number of targets above this threshold for each TF

- **sum_weight:** Total regulatory influence (higher = broader regulatory program)

TFs vary in their number of high-confidence targets because:

- Some TFs are master regulators with many strong predictions

- Some TFs are specialized regulators with fewer but high-weight targets

- Some TFs may have weak predictions across the board (no high-confidence targets)

This approach ensures we only count truly high-confidence regulatory relationships rather than all possible TF-target combinations.

**Master Regulator Definition:** Transcription factors with high total regulatory influence (sum of importance scores) are candidate “master regulators”—TFs that control large regulatory programs. These TFs are:

- High-priority candidates for perturbation experiments

- Likely important for the biological processes in your samples

- Potential therapeutic targets in disease contexts

- Central nodes in the regulatory hierarchy

### Analyzing Targets of Top Transcription Factor

Let’s examine the predicted targets of the top master regulator:

```r
#-----------------------------------------------
# STEP 7: Analyze targets of top transcription factor
#-----------------------------------------------

# Select the top master regulator
top_tf <- tf_stats$regulatorSymbol[1]

# Get high-confidence targets of this TF
tf_targets <- high_confidence_links %>%
  filter(regulatorSymbol == top_tf) %>%
  arrange(desc(weight))

# Save targets for this TF
write.table(tf_targets,
            paste0("results/targets_of_", top_tf, ".tsv"),
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)

# Get target gene symbols for enrichment analysis
target_symbols <- tf_targets$targetSymbol[!is.na(tf_targets$targetSymbol)]

# GO enrichment analysis
ego <- enrichGO(
  gene = target_symbols,
  OrgDb = org.Hs.eg.db,
  keyType = "SYMBOL",
  ont = "BP",
  pAdjustMethod = "BH",
  pvalueCutoff = 0.05,
  qvalueCutoff = 0.2
)

# Plot GO enrichment if any terms found
if (!is.null(ego) && nrow(ego) > 0) {

  # Save GO results
  write.table(as.data.frame(ego),
              paste0("results/GO_enrichment_", top_tf, ".tsv"),
              sep = "\t",
              row.names = FALSE,
              quote = FALSE)

  # Create dot plot
  p1 <- dotplot(ego, showCategory = 15) +
    ggtitle(paste("GO Biological Process Enrichment:", top_tf, "Targets"))

  ggsave(paste0("plots/GO_enrichment_", top_tf, ".png"), 
         plot = p1, width = 10, height = 8)

  # Create bar plot
  p2 <- barplot(ego, showCategory = 15) +
    ggtitle(paste("GO Biological Process Enrichment:", top_tf, "Targets"))

  ggsave(paste0("plots/GO_barplot_", top_tf, ".png"), 
         plot = p2, width = 10, height = 8)
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_go_enrich-e1764822557556-1024x408.png?resize=1024%2C408&ssl=1)

> 

**Understanding Enrichment Results:**

If no enriched terms are found, this suggests the TF regulates genes across diverse functions rather than focusing on specific pathways. This is common for master regulators that control broad cellular programs.

### Comparing Regulatory Networks Across Conditions

One powerful application of GENIE3 is comparing regulatory networks between conditions to identify rewired regulatory relationships:

```r
#-----------------------------------------------
# STEP 8: Condition-specific network comparison
#-----------------------------------------------

# Build separate networks for wild-type and AKO conditions
wt_samples <- rownames(pheno_data)[pheno_data$Treatment %in% 
                                     c("EV", "Y", "T", "N")]
ako_samples <- rownames(pheno_data)[pheno_data$Treatment %in% 
                                      c("EV_AKO", "Y_AKO", "T_AKO", "N_AKO")]

# Subset expression matrices
expr_wt <- expr_matrix[, wt_samples]
expr_ako <- expr_matrix[, ako_samples]

# Run GENIE3 for wild-type samples
set.seed(123)
net_wt <- GENIE3(expr_wt, regulators = tf_genes, nCores = 8, verbose = TRUE)

# Run GENIE3 for AKO samples  
set.seed(123)
net_ako <- GENIE3(expr_ako, regulators = tf_genes, nCores = 8, verbose = TRUE)

# Extract link lists
links_wt <- getLinkList(net_wt)
links_ako <- getLinkList(net_ako)

# Add gene symbols
links_wt$regulatorSymbol <- gene_data$SYMBOL[
  match(links_wt$regulatoryGene, gene_data$ENSEMBL)
]
links_wt$targetSymbol <- gene_data$SYMBOL[
  match(links_wt$targetGene, gene_data$ENSEMBL)
]

links_ako$regulatorSymbol <- gene_data$SYMBOL[
  match(links_ako$regulatoryGene, gene_data$ENSEMBL)
]
links_ako$targetSymbol <- gene_data$SYMBOL[
  match(links_ako$targetGene, gene_data$ENSEMBL)
]

# Compare TF rankings between conditions
# Use absolute weight threshold (99th percentile) for each network
weight_threshold_wt <- quantile(links_wt$weight, 0.99)
weight_threshold_ako <- quantile(links_ako$weight, 0.99)

high_conf_wt <- links_wt %>%
  filter(weight >= weight_threshold_wt)

high_conf_ako <- links_ako %>%
  filter(weight >= weight_threshold_ako)

tf_stats_wt <- high_conf_wt %>%
  group_by(regulatorSymbol) %>%
  summarise(
    sum_weight_wt = sum(weight),
    n_targets_wt = n(),
    .groups = 'drop'
  )

tf_stats_ako <- high_conf_ako %>%
  group_by(regulatorSymbol) %>%
  summarise(
    sum_weight_ako = sum(weight),
    n_targets_ako = n(),
    .groups = 'drop'
  )

# Merge statistics
tf_comparison <- merge(tf_stats_wt, tf_stats_ako, 
                       by = "regulatorSymbol", all = TRUE)
tf_comparison[is.na(tf_comparison)] <- 0

# Calculate difference in regulatory influence
tf_comparison$weight_diff <- tf_comparison$sum_weight_ako - 
                             tf_comparison$sum_weight_wt
tf_comparison$weight_fold_change <- log2(
  (tf_comparison$sum_weight_ako + 1) / (tf_comparison$sum_weight_wt + 1)
)

# Plot comparison
ggplot(tf_comparison, aes(x = sum_weight_wt, 
                          y = sum_weight_ako,
                          label = regulatorSymbol)) +
  geom_point(aes(color = abs(weight_fold_change)), size = 3) +
  geom_abline(slope = 1, intercept = 0, linetype = "dashed", color = "gray") +
  geom_text_repel(data = subset(tf_comparison, abs(weight_fold_change) > 1),
                  size = 3) +
  scale_color_gradient2(low = "blue", mid = "white", high = "red",
                        midpoint = 0, name = "|log2 FC|") +
  theme_minimal() +
  labs(
    title = "Transcription Factor Regulatory Influence",
    subtitle = "Comparison between WT and AKO conditions",
    x = "Total Regulatory Weight (WT)",
    y = "Total Regulatory Weight (AKO)"
  )

ggsave("plots/tf_comparison_wt_vs_ako.png", width = 10, height = 8)

# Identify condition-specific master regulators
wt_specific <- tf_comparison %>%
  filter(weight_fold_change < -1) %>%
  arrange(weight_fold_change) %>%
  head(5)

ako_specific <- tf_comparison %>%
  filter(weight_fold_change > 1) %>%
  arrange(desc(weight_fold_change)) %>%
  head(5)

# Save comparison results
write.table(tf_comparison,
            "results/tf_comparison_wt_vs_ako.tsv",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)

# Analyze condition-specific regulatory links
# Create TF-target pairs for each condition
wt_pairs <- paste(high_conf_wt$regulatorSymbol, 
                  high_conf_wt$targetSymbol, 
                  sep = "_")

ako_pairs <- paste(high_conf_ako$regulatorSymbol, 
                   high_conf_ako$targetSymbol, 
                   sep = "_")

# Find shared and condition-specific links
shared_links <- intersect(wt_pairs, ako_pairs)
wt_specific_links <- setdiff(wt_pairs, ako_pairs)
ako_specific_links <- setdiff(ako_pairs, wt_pairs)

# Get details of condition-specific links
wt_only <- high_conf_wt[wt_pairs %in% wt_specific_links, ]
ako_only <- high_conf_ako[ako_pairs %in% ako_specific_links, ]

# Save condition-specific links
write.table(wt_only,
            "results/wt_specific_regulatory_links.tsv",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)

write.table(ako_only,
            "results/ako_specific_regulatory_links.tsv",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)
```

**tf_comparison:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_tf_comparison.png?resize=1024%2C171&ssl=1)

**wt_only:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_wt_only.png?resize=1024%2C217&ssl=1)

> 

**Understanding Condition-Specific Links:**

- **Shared links:** Regulatory relationships present in both conditions (stable core regulation)

- **WT-specific links:** Relationships disrupted or lost in AKO condition

- **AKO-specific links:** New regulatory relationships that emerge in AKO condition (compensatory or rewired regulation)

**Biological Interpretation:**

> 

- TFs with many WT-specific links may depend on the knocked-out gene for their function

- TFs with many AKO-specific links may be activated as compensatory mechanisms

- Condition-specific links reveal how regulatory networks are rewired in response to genetic perturbation

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/04_tf_comparison_wt_vs_ako.png?resize=1024%2C819&ssl=1)

## Network Visualization and Interpretation

Visualizing the regulatory network helps identify network structure, central regulators, and regulatory motifs.

### Creating a Network Graph of Top Predictions

Let’s visualize the top regulatory interactions:

```r
#-----------------------------------------------
# STEP 9: Network visualization
#-----------------------------------------------

# Create network graph from top links
n_top_viz <- 500  # Use top 500 links for visualization

top_links_viz <- link_list[1:n_top_viz, ]

# Create igraph object
g <- graph_from_data_frame(
  top_links_viz[, c("regulatorSymbol", "targetSymbol", "weight")],
  directed = TRUE
)

# Calculate node properties
V(g)$degree <- degree(g, mode = "all")
V(g)$in_degree <- degree(g, mode = "in")
V(g)$out_degree <- degree(g, mode = "out")

# Identify TFs (nodes that regulate others)
is_tf <- V(g)$name %in% unique(top_links_viz$regulatorSymbol)
V(g)$type <- ifelse(is_tf, "TF", "Target")

# Color nodes by type
V(g)$color <- ifelse(V(g)$type == "TF", "coral", "lightblue")

# Size nodes by degree
V(g)$size <- sqrt(V(g)$degree) * 3 + 3

# Edge width by importance
E(g)$width <- (E(g)$weight / max(E(g)$weight)) * 3

# Plot network
png("plots/regulatory_network.png", width = 1200, height = 1200, res = 100)
par(mar = c(1, 1, 3, 1))
set.seed(123)
plot(g,
     vertex.label = ifelse(V(g)$degree > quantile(V(g)$degree, 0.9),
                          V(g)$name, NA),
     vertex.label.cex = 0.7,
     vertex.label.color = "black",
     edge.arrow.size = 0.3,
     edge.color = "gray70",
     layout = layout_with_fr(g),
     main = "Gene Regulatory Network (Top 500 Interactions)")

legend("topright",
       legend = c("Transcription Factor", "Target Gene"),
       col = c("coral", "lightblue"),
       pch = 19,
       cex = 0.8,
       bty = "n")
dev.off()

# Save network statistics
network_stats <- data.frame(
  Metric = c("Nodes", "Edges", "Density", "TFs", "Targets"),
  Value = c(
    vcount(g),
    ecount(g),
    edge_density(g),
    sum(V(g)$type == "TF"),
    sum(V(g)$type == "Target")
  )
)

write.table(network_stats,
            "results/network_stats_viz.tsv",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_network_stats.png?resize=304%2C266&ssl=1)

> 

**Network Interpretation:**

- **Hub TFs (large coral nodes):** Transcription factors regulating many targets

- **Hub targets (large blue nodes):** Genes regulated by multiple TFs (may be key effectors)

- **Network modules:** Clusters of densely connected genes may represent co-regulated pathways

- **Network density:** Sparse networks suggest specific regulation; dense networks may indicate many indirect connections

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/05_regulatory_network.png?resize=1024%2C1024&ssl=1)

### Analyzing Network Motifs and Structure

Regulatory networks often contain recurring patterns called network motifs:

```r
#-----------------------------------------------
# STEP 10: Network motif analysis
#-----------------------------------------------

# Feed-forward loops: TF1 → TF2 → Target, TF1 → Target
# These represent coordinated regulation

# Find all triangles in the network (potential FFLs)
triangles <- count_triangles(g)

# Identify TF-TF edges (regulatory cascades)
tf_tf_edges <- E(g)[V(g)[type == "TF"] %--% V(g)[type == "TF"]]

# Create TF-only subnetwork to visualize regulatory hierarchy
tf_nodes <- V(g)[type == "TF"]$name
tf_subgraph <- induced_subgraph(g, tf_nodes)

if (ecount(tf_subgraph) > 0) {
  png("plots/tf_hierarchy.png", width = 800, height = 800, res = 100)
  par(mar = c(1, 1, 3, 1))
  plot(tf_subgraph,
       vertex.label = V(tf_subgraph)$name,
       vertex.label.cex = 0.8,
       vertex.size = 15,
       vertex.color = "coral",
       edge.arrow.size = 0.5,
       edge.color = "gray40",
       layout = layout_with_sugiyama(tf_subgraph),
       main = "Transcription Factor Regulatory Hierarchy")
  dev.off()
} else {
  cat("No TF-TF regulatory relationships in top predictions\n")
}

# Calculate centrality measures
V(g)$betweenness <- betweenness(g, directed = TRUE)
V(g)$closeness <- closeness(g, mode = "all")

# Identify central nodes
node_centrality <- data.frame(
  gene = V(g)$name,
  type = V(g)$type,
  degree = V(g)$degree,
  in_degree = V(g)$in_degree,
  out_degree = V(g)$out_degree,
  betweenness = V(g)$betweenness,
  closeness = V(g)$closeness
)

# Top central genes
top_central <- node_centrality %>%
  arrange(desc(betweenness)) %>%
  head(10)

write.table(node_centrality,
            "results/network_node_centrality.tsv",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)
```

**node_centrality:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_node_centrality.png?resize=1024%2C206&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/06_tf_hierarchy.png?resize=800%2C800&ssl=1)

> 

**Understanding Network Centrality Measures:**

These metrics identify important genes in the regulatory network:

**1. Degree:**

- **Definition:** Total number of connections (edges) a gene has

- **Calculation:** in_degree + out_degree

- **High degree means:** Gene is highly connected in the network

- **Example:** A TF regulating 50 targets AND being regulated by 10 other TFs has degree = 60

- **Biological meaning:** Hub genes with many connections; often master regulators or key effectors

**2. In-Degree:**

- **Definition:** Number of incoming edges (how many regulators control this gene)

- **High in-degree means:** Gene is regulated by many transcription factors

- **Example:** A target gene regulated by 20 different TFs has in-degree = 20

- **Biological meaning:** Highly controlled genes; often important effectors at pathway convergence points

**3. Out-Degree:**

- **Definition:** Number of outgoing edges (how many targets this gene regulates)

- **High out-degree means:** Gene regulates many other genes

- **Example:** A TF regulating 100 target genes has out-degree = 100

- **Biological meaning:** Master regulators with broad influence; candidates for perturbation experiments

**4. Betweenness:**

- **Definition:** How often a gene lies on the shortest path between other genes

- **Calculation:** Counts paths that pass through this gene

- **High betweenness means:** Gene connects different parts of the network

- **Example:** A TF that bridges stress response and metabolic pathways has high betweenness

- **Biological meaning:** Information bottlenecks; disrupting these genes affects communication between network modules

**5. Closeness:**

- **Definition:** How close a gene is (on average) to all other genes in the network

- **Calculation:** Average shortest path length to all other genes

- **High closeness means:** Gene can quickly influence or be influenced by other genes

- **Example:** A central TF with short paths to most genes has high closeness

- **Biological meaning:** Genes that can rapidly propagate signals through the network

**Which Metric to Use?**

- **Out-degree:** Find master regulators (TFs with many targets)

- **In-degree:** Find highly controlled effector genes

- **Betweenness:** Find genes connecting different pathways or processes

- **Closeness:** Find genes with fast network-wide influence

- **Degree:** Find overall hub genes (highly connected in general)

**Biological Significance of Network Properties:**

- **High degree:** Genes participating in many regulatory relationships (potential master regulators or key effectors)

- **High betweenness:** Genes connecting different network regions (potential information bottlenecks); disrupting these affects multiple pathways

- **High out-degree TFs:** Master regulators with broad influence; top candidates for perturbation experiments

- **High in-degree genes:** Tightly controlled effectors; often functionally important

- **Feed-forward loops (FFLs):** Common motif where TF1 regulates both TF2 and a target, and TF2 also regulates the target. FFLs provide signal processing and noise filtering

- **TF cascades:** Sequential regulation (TF1 → TF2 → TF3) reveals regulatory hierarchies

### Comparing with Known TF-Target Databases

To validate GENIE3 predictions, we can compare them with experimentally validated TF-target relationships from the `dorothea` database:

```r
#-----------------------------------------------
# STEP 11: Validation against known TF-target relationships
#-----------------------------------------------

# Use dorothea database which contains curated TF-target interactions
# with confidence levels (A = highest, E = lowest)

# Filter for high-confidence interactions (levels A, B, and C)
known_interactions <- dorothea_hs %>%
  filter(confidence %in% c("A", "B", "C")) %>%
  dplyr::select(tf, target, confidence)

# Check overlap of TFs between GENIE3 and dorothea
genie3_tfs <- unique(link_list$regulatorSymbol)
dorothea_tfs <- unique(known_interactions$tf)
common_tfs <- intersect(genie3_tfs, dorothea_tfs)

# Check overlap of target genes
genie3_targets <- unique(link_list$targetSymbol)
dorothea_targets <- unique(known_interactions$target)
common_targets <- intersect(genie3_targets, dorothea_targets)

# Filter known interactions to only those with TFs and targets in our dataset
known_interactions_filtered <- known_interactions %>%
  filter(tf %in% genie3_tfs & target %in% genie3_targets)

# For validation, use top N predictions (more stringent than percentile)
# Test different thresholds
n_predictions_to_test <- 5000

top_predictions <- head(link_list, n_predictions_to_test)

# Create TF-target pairs for comparison
genie3_pairs <- paste(top_predictions$regulatorSymbol, 
                      top_predictions$targetSymbol, 
                      sep = "_")

known_pairs <- paste(known_interactions_filtered$tf, 
                     known_interactions_filtered$target, 
                     sep = "_")

# Calculate overlap
overlap_pairs <- intersect(genie3_pairs, known_pairs)
precision <- length(overlap_pairs) / length(genie3_pairs)

cat("Validation Results:\n")
cat("GENIE3 predictions tested:", length(genie3_pairs), "\n")
# GENIE3 predictions tested: 5000 
cat("Relevant known interactions:", length(known_pairs), "\n")
# Relevant known interactions: 13223 
cat("Validated predictions:", length(overlap_pairs), "\n")
# Validated predictions: 14 
cat("Precision:", round(precision * 100, 2), "%\n\n")
# Precision: 0.28 %

# Examine validated interactions
if (length(overlap_pairs) > 0) {
  validated_links <- top_predictions[genie3_pairs %in% overlap_pairs, ]

  # Add confidence level from dorothea
  validated_links$tf_target_pair <- paste(validated_links$regulatorSymbol,
                                           validated_links$targetSymbol, 
                                           sep = "_")

  known_interactions_filtered$tf_target_pair <- paste(
    known_interactions_filtered$tf,
    known_interactions_filtered$target, 
    sep = "_"
  )

  validated_links <- merge(
    validated_links, 
    known_interactions_filtered[, c("tf_target_pair", "confidence")],
    by = "tf_target_pair"
  )

  # Sort by GENIE3 weight
  validated_links <- validated_links %>%
    arrange(desc(weight))

  # Save all validated predictions
  write.table(validated_links,
              "results/validated_tf_target_predictions.tsv",
              sep = "\t",
              row.names = FALSE,
              quote = FALSE)
```

**dorothea_hs:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_dorothea_hs.png?resize=552%2C230&ssl=1)

**validated_links:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/12/genie3_validated_links.png?resize=1024%2C160&ssl=1)

> 

**Interpreting Validation Results:**

**Focus on:**

- The specific TF-target pairs that validated (these are high-confidence)

- TFs with multiple validated targets (likely true regulators)

- Fold enrichment over random expectation

**Don’t worry if:**

- Precision is 1-5% (this is normal and still informative)

- Many predictions aren’t validated (they may be novel or context-specific)

- Different TFs have different validation rates (reflects database bias)

## Interpreting GENIE3 Results: From Predictions to Biology

With our network inferred and analyzed, let’s discuss how to extract meaningful biological insights.

### Understanding Prediction Confidence

**What Makes a High-Quality GENIE3 Prediction?**

Strong evidence for a TF-target relationship includes:

1. **High importance score:** Target is in top 0.1% of predictions

2. **Biological plausibility:** TF and target have related functions

3. **Literature support:** Interaction documented in previous studies

4. **Motif presence:** TF binding motif in target promoter/enhancers

5. **Conservation:** Interaction conserved across species

6. **Multiple samples:** Relationship robust across biological replicates

**Red Flags for False Positives:**

1. **Housekeeping targets:** TF “regulates” genes expressed in all cells

2. **Extreme outliers:** Relationship driven by 1-2 aberrant samples

3. **Indirect relationships:** True regulator is upstream of predicted TF

4. **Technical artifacts:** Batch effects create spurious associations

5. **Insufficient samples:** Predictions unstable with <15 samples

### Biological Interpretation Guidelines

**For Individual TF-Target Predictions:**

Ask these questions about top predictions:

1. **Is there biological precedent?**

- Are TF and target co-localized (same cell type/compartment)?

- Do they participate in related pathways?

- Is the TF expressed when the target is regulated?

2. **Does the mechanism make sense?**

- If TF is activating, does target increase with TF?

- If TF is repressive, does target decrease with TF?

- Are there cofactors or chromatin modifiers involved?

3. **What is the functional consequence?**

- How does this regulation affect cell phenotype?

- Is this relationship important for disease or development?

- Could perturbing this relationship have therapeutic value?

## Common Pitfalls and How to Avoid Them

**Pitfall 1: Over-interpreting Low-Confidence Predictions**

- **Problem:** Testing predictions with low importance scores

- **Solution:** Focus on top 0.1-1% of predictions for validation

**Pitfall 2: Ignoring Biological Context**

- **Problem:** Predicting TF regulates target despite incompatible expression patterns

- **Solution:** Visualize TF and target expression across samples; check co-expression

**Pitfall 3: Assuming Direct Regulation**

- **Problem:** GENIE3 predicts indirect relationships (TF₁ → TF₂ → Target)

- **Solution:** Use ChIP-seq or motif analysis to confirm direct binding

**Pitfall 4: Insufficient Sample Size**

- **Problem:** Running GENIE3 on <15 samples produces unstable predictions

- **Solution:** Increase sample size or focus only on very top predictions

**Pitfall 5: Not Comparing with Known Biology**

- **Problem:** Pursuing predictions contradicted by extensive literature

- **Solution:** Literature review before expensive validation experiments

**Pitfall 6: Batch Effects Creating False Predictions**

- **Problem:** Technical artifacts drive spurious TF-target associations

- **Solution:** Remove batch effects before GENIE3; check if predictions align with known batch structure

## Conclusion: From Network Predictions to Biological Discovery

Gene regulatory network inference represents a shift from studying individual genes to understanding them within their regulatory context. GENIE3’s machine learning approach provides a powerful tool for this systems-level analysis, enabling discovery of transcription factor programs that control cellular behavior.

Remember that GENIE3 is most powerful when:

- Combined with complementary network methods (WGCNA, master regulator analysis)

- Validated through multiple lines of evidence (literature, motifs, ChIP-seq)

- Followed up with targeted experimental studies

- Interpreted in the context of biological knowledge

The regulatory networks you’ve inferred are hypotheses waiting to be tested. The most exciting discoveries often come from unexpected predictions that, upon validation, reveal new biology. By integrating GENIE3 with other bioinformatics approaches and experimental validation, you can move from expression data to mechanistic understanding of gene regulation.

## References

1. Huynh-Thu VA, Irrthum A, Wehenkel L, Geurts P. Inferring regulatory networks from expression data using tree-based methods. PLoS One. 2010;5(9):e12776.

2. Marbach D, Costello JC, Küffner R, et al. Wisdom of crowds for robust gene network inference. Nat Methods. 2012;9(8):796-804.

3. Aibar S, González-Blas CB, Moerman T, et al. SCENIC: single-cell regulatory network inference and clustering. Nat Methods. 2017;14(11):1083-1086.

4. Moerman T, Aibar S, González-Blas CB, et al. GRNBoost2 and Arboreto: efficient and scalable inference of gene regulatory networks. Bioinformatics. 2019;35(12):2159-2161.

5. Margolin AA, Nemenman I, Basso K, et al. ARACNE: an algorithm for the reconstruction of gene regulatory networks in a mammalian cellular context. BMC Bioinformatics. 2006;7 Suppl 1:S7.

6. Faith JJ, Hayete B, Thaden JT, et al. Large-scale mapping and validation of Escherichia coli transcriptional regulation from a compendium of expression profiles. PLoS Biol. 2007;5(1):e8.

7. Han H, Cho JW, Lee S, et al. TRRUST v2: an expanded reference database of human and mouse transcriptional regulatory interactions. Nucleic Acids Res. 2018;46(D1):D380-D386.

8. ENCODE Project Consortium. An integrated encyclopedia of DNA elements in the human genome. Nature. 2012;489(7414):57-74.

9. Castro-Mondragon JA, Riudavets-Puig R, Rauluseviciute I, et al. JASPAR 2022: the 9th release of the open-access database of transcription factor binding profiles. Nucleic Acids Res. 2022;50(D1):D165-D173.

10. Basso K, Margolin AA, Stolovitzky G, Klein U, Dalla-Favera R, Califano A. Reverse engineering of regulatory networks in human B cells. Nat Genet. 2005;37(4):382-390.

11. Kamal, A., Arnold, C., Claringbould, A., Moussa, R., Servaas, N.H., Kholmatov, M., Daga, N., Nogina, D., Mueller‐Dott, S., Reyes‐Palomares, A. and Palla, G., 2023. GRaNIE and GRaNPA: inference and evaluation of enhancer‐mediated gene regulatory networks. Molecular Systems Biology, p.e11627.; doi: [https://doi.org/10.15252/msb.202311627](https://doi.org/10.15252/msb.202311627)

---

*This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, **please comment and share** **your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.*
