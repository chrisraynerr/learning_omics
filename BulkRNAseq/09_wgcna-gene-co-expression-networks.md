# How to Build Gene Co-expression Networks from RNA-seq Data Using WGCNA – Complete Step-by-Step Guide For Absolute Beginners

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/)

- [Introduction: Understanding Gene Co-expression Networks](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Introduction_Understanding_Gene_Co-expression_Networks)

  - [What is a Gene Co-expression Network?](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#What_is_a_Gene_Co-expression_Network)
  - [What Biological Insights Can We Gain From Co-expression Networks?](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#What_Biological_Insights_Can_We_Gain_From_Co-expression_Networks)
  - [How Does WGCNA Reconstruct Co-expression Networks?](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#How_Does_WGCNA_Reconstruct_Co-expression_Networks)
  - [When Should You Use WGCNA?](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#When_Should_You_Use_WGCNA)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Setting_Up_Your_Analysis_Environment)

  - [Installing Required R Packages](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Installing_Required_R_Packages)

- [Downloading and Preparing Example Data](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Downloading_and_Preparing_Example_Data)

  - [Experimental Groups](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Experimental_Groups)
  - [Loading Pre-processed Data](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Loading_Pre-processed_Data)
  - [Data Quality Control and Filtering](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Data_Quality_Control_and_Filtering)

- [Data Preparation for WGCNA](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Data_Preparation_for_WGCNA)

  - [Creating the Trait Data Matrix](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Creating_the_Trait_Data_Matrix)
  - [Understanding the Data Structure](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Understanding_the_Data_Structure)

- [Building the Gene Co-expression Network](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Building_the_Gene_Co-expression_Network)

  - [Understanding WGCNA Parameters](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Understanding_WGCNA_Parameters)
  - [Step 1: Choose Soft-Thresholding Power](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Step_1_Choose_Soft-Thresholding_Power)
  - [Step 2: Construct Network and Detect Modules](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Step_2_Construct_Network_and_Detect_Modules)
  - [Step 3: Calculate Module Eigengenes](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Step_3_Calculate_Module_Eigengenes)

- [Downstream Analysis: Connecting Modules to Biology](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Downstream_Analysis_Connecting_Modules_to_Biology)

  - [Module-Trait Association Analysis](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Module-Trait_Association_Analysis)
  - [Gene Significance and Module Membership](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Gene_Significance_and_Module_Membership)
  - [Hub Gene Identification](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Hub_Gene_Identification)
  - [Functional Enrichment Analysis](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Functional_Enrichment_Analysis)
  - [Constructing Protein-Protein Interaction Networks](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Constructing_Protein-Protein_Interaction_Networks)

- [Network and Module Visualization](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Network_and_Module_Visualization)

  - [Visualizing the Complete Network](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Visualizing_the_Complete_Network)
  - [Module Eigengene Network](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Module_Eigengene_Network)
  - [Heatmap of Module Gene Expression](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Heatmap_of_Module_Gene_Expression)

- [Differential Module Preservation Analysis](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Differential_Module_Preservation_Analysis)

  - [Building Condition-Specific Networks](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Building_Condition-Specific_Networks)
  - [Module Preservation Statistics](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Module_Preservation_Statistics)
  - [Comparing Module Eigengenes Across Conditions](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Comparing_Module_Eigengenes_Across_Conditions)

- [Interpreting WGCNA Results: From Data to Discovery](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Interpreting_WGCNA_Results_From_Data_to_Discovery)

  - [Understanding Module Structure](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Understanding_Module_Structure)
  - [Interpreting Module-Trait Associations](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Interpreting_Module-Trait_Associations)
  - [Prioritizing Genes for Follow-Up](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Prioritizing_Genes_for_Follow-Up)
  - [Validation Strategies](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Validation_Strategies)

- [Best Practices for WGCNA Analysis](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Best_Practices_for_WGCNA_Analysis)

  - [Sample Size and Experimental Design](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Sample_Size_and_Experimental_Design)
  - [Data Quality Considerations](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Data_Quality_Considerations)
  - [Parameter Selection Wisdom](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Parameter_Selection_Wisdom)
  - [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Common_Pitfalls_and_How_to_Avoid_Them)
  - [Computational Considerations](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Computational_Considerations)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Troubleshooting_Common_Issues)

  - [Issue 1: Cannot Find Appropriate Soft-Thresholding Power](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Issue_1_Cannot_Find_Appropriate_Soft-Thresholding_Power)
  - [Issue 2: Too Many or Too Few Modules](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Issue_2_Too_Many_or_Too_Few_Modules)
  - [Issue 3: Large Gray Module](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Issue_3_Large_Gray_Module)
  - [Issue 4: Module Eigengene Calculation Fails](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Issue_4_Module_Eigengene_Calculation_Fails)
  - [Issue 5: Memory Errors During TOM Calculation](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Issue_5_Memory_Errors_During_TOM_Calculation)
  - [Issue 6: No Significant Module-Trait Associations](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Issue_6_No_Significant_Module-Trait_Associations)
  - [Issue 7: cor() Function Error](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Issue_7_cor_Function_Error)

- [Conclusion: From Networks to Knowledge](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Conclusion_From_Networks_to_Knowledge)
- [References](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#References)

  - [Share this:](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Share_this)
  - [Like this:](https://ngs101.com/how-to-build-gene-co-expression-networks-from-rna-seq-data-using-wgcna-complete-step-by-step-guide-for-absolute-beginners/#Like_this)

## Introduction: Understanding Gene Co-expression Networks

In the intricate machinery of living cells, genes rarely act in isolation. Instead, they work together in coordinated networks, with groups of genes being co-expressed to carry out specific biological functions. Understanding these relationships is fundamental to deciphering how cells respond to stimuli, how diseases develop, and how we might intervene therapeutically. Weighted Gene Co-expression Network Analysis (WGCNA) provides a powerful framework for uncovering these hidden patterns in gene expression data.

### What is a Gene Co-expression Network?

A gene co-expression network is a systems biology approach that represents genes as nodes and the correlation relationships between their expression patterns as edges. When two genes show similar expression patterns across multiple samples—both increasing or decreasing together—they are said to be co-expressed. This co-expression often indicates functional relationships: genes working together in the same pathway, responding to the same regulatory signals, or contributing to the same biological process.

Think of it like observing people’s daily routines in a city. If you notice that certain groups of people consistently arrive at the same places at the same times, you might infer they work together, share similar schedules, or participate in common activities. Similarly, when genes show correlated expression patterns across different conditions or samples, it suggests they may be functionally related.

WGCNA takes this concept further by constructing “weighted” networks, where the strength of the connection between genes reflects how strongly they are co-expressed. Rather than using arbitrary cutoffs to decide if genes are connected, WGCNA preserves the continuous nature of co-expression relationships, making the analysis more biologically meaningful and statistically robust.

> 

**Beginner’s Tip:** Unlike traditional clustering methods that assign each gene to a single group, WGCNA creates modules (groups of highly co-expressed genes) while preserving information about the strength of relationships. This “soft” approach better reflects biological reality, where genes can participate in multiple processes.

### What Biological Insights Can We Gain From Co-expression Networks?

Gene co-expression network analysis opens multiple avenues for biological discovery:

**1. Functional Annotation and Pathway Discovery**

When genes cluster together in a module, they often share biological functions. By examining the known functions of some genes in a module, you can infer the roles of uncharacterized genes. This “guilt by association” principle has successfully predicted gene functions across many organisms.

**2. Treatment Response Mechanisms**

By correlating modules with experimental conditions or treatments, WGCNA can identify groups of genes that are collectively regulated in response to interventions. These treatment-associated modules may reveal novel mechanisms of action or resistance pathways.

**3. Hub Gene Identification**

Within each module, certain genes serve as “hubs”—highly connected nodes that may play central regulatory roles. These hub genes are often master regulators like transcription factors, and perturbations to hub genes can have cascading effects throughout the module. Hub genes frequently make excellent candidates for experimental validation or therapeutic targeting.

**4. Comparative Analysis Across Conditions**

WGCNA enables comparison of network organization across different conditions, genotypes, or time points. Module preservation analysis reveals whether the relationships between genes remain stable or are rewired in different contexts. This can highlight condition-specific regulatory mechanisms.

**5. Integration with Other Data Types**

Co-expression networks can be integrated with genetic variation data (eQTL), protein-protein interaction networks, chromatin accessibility data, and more. This multi-layered approach provides a more complete picture of gene regulation.

### How Does WGCNA Reconstruct Co-expression Networks?

WGCNA follows a systematic workflow that transforms gene expression data into biologically meaningful modules:

**Step 1: Calculating Pairwise Correlations**

The process begins by computing correlation coefficients between every pair of genes across all samples. This creates a similarity matrix where high values indicate strong co-expression.

**Step 2: Soft Thresholding and Adjacency Matrix**

Here’s where WGCNA differs from traditional approaches. Instead of applying a hard cutoff (“genes with correlation >0.7 are connected, others are not”), WGCNA uses a soft-thresholding approach. The correlation values are raised to a power (β), which emphasizes strong correlations while suppressing weak ones. This power is chosen to make the network approximate a scale-free topology—a property observed in many biological networks where most genes have few connections, but some hubs have many.

The transformation looks like this:

- Adjacency = |correlation|^β

This creates a weighted adjacency matrix where connection strengths vary continuously.

**Step 3: Topological Overlap Matrix (TOM)**

WGCNA then calculates the Topological Overlap Matrix, which measures not just direct correlation between two genes, but also how many neighbors they share. Genes with high topological overlap are more likely to be functionally related because they connect to similar sets of other genes. This step makes the network analysis more robust to noise.

**Step 4: Hierarchical Clustering and Module Detection**

Using the TOM dissimilarity (1-TOM) as a distance measure, WGCNA performs hierarchical clustering to group genes into modules. Genes within the same module show high topological overlap, suggesting coordinated function. A dynamic tree-cutting algorithm identifies these modules automatically.

**Step 5: Module Eigengene Calculation**

For each module, WGCNA calculates a “module eigengene” (ME)—essentially the first principal component of the module’s expression matrix. The ME represents the overall expression pattern of the module and can be correlated with external traits to identify biologically relevant modules.

**Step 6: Module-Trait Association**

Finally, WGCNA correlates module eigengenes with experimental conditions or phenotypic traits to identify which gene modules are associated with specific biological contexts.

> 

**Key Concept:** The soft-thresholding parameter (β) is critical. Too low, and the network remains too dense with weak connections. Too high, and you lose potentially meaningful relationships. WGCNA provides diagnostic plots to help you choose the optimal β.

### When Should You Use WGCNA?

WGCNA is particularly powerful in several scenarios:

**Sample Size Requirements**

WGCNA works best with at least 15-20 samples, and preferably more. With too few samples, correlation estimates become unreliable. Ideally, you want 30+ samples with good biological variation to capture meaningful co-expression patterns.

**Multiple Conditions or Treatments**

If you have RNA-seq data from multiple experimental conditions, genotypes, or time points, WGCNA can identify modules that respond differently across these contexts. This is more informative than analyzing each condition separately.

**Complex Traits and Experimental Correlations**

When you have accompanying experimental data (treatment response, genotype effects, time-course dynamics), WGCNA excels at identifying gene modules associated with these factors, potentially revealing mechanistic insights.

**Exploratory Analysis and Hypothesis Generation**

WGCNA is excellent for discovering unexpected gene relationships and generating hypotheses about gene function. It’s particularly useful when studying poorly characterized biological systems.

**When NOT to Use WGCNA**

WGCNA may not be the best choice if you:

- Have very few samples (<15)

- Are primarily interested in differential expression of individual genes (use DESeq2, edgeR instead)

- Want to analyze single-cell RNA-seq data (specialized network methods exist for scRNA-seq: [hdWGCNA](https://smorabit.github.io/hdWGCNA/))

- Need to account for complex experimental designs with multiple confounding factors

## Setting Up Your Analysis Environment

Before we begin constructing gene co-expression networks, we need to set up our R environment with the necessary packages.

### Installing Required R Packages

WGCNA analysis requires several R packages. Let’s install them in a systematic way:

```r
#-----------------------------------------------
# STEP 0: Install required R packages
#-----------------------------------------------

# Set CRAN mirror to avoid prompts during installation
options(repos = c(CRAN = "https://cloud.r-project.org"))

# Install WGCNA and dependencies from CRAN
install.packages("WGCNA", dependencies = TRUE)

# Install other required CRAN packages
install.packages(c(
  "BiocManager",      # For Bioconductor package management
  "ggplot2",          # For advanced visualization
  "reshape2",         # For data manipulation
  "igraph",           # For network analysis and visualization
  "pheatmap",         # For heatmap visualization
  "RColorBrewer",     # For color palettes
  "corrplot",         # For correlation plots
  "ggrepel"           # For non-overlapping labels in plots
))

# Install Bioconductor packages
BiocManager::install(c(
  "limma",            # For normalization and QC
  "DESeq2",           # For data transformation
  "clusterProfiler",  # For functional enrichment analysis
  "org.Hs.eg.db",     # Human gene annotations
  "GO.db",            # Gene Ontology database
  "STRINGdb"          # For protein-protein interaction networks
))

# Load required libraries
library(WGCNA)
library(ggplot2)
library(ggrepel)
library(reshape2)
library(pheatmap)
library(igraph)
library(clusterProfiler)
library(org.Hs.eg.db)
library(STRINGdb)

# Enable multi-threading for WGCNA (adjust based on your system)
enableWGCNAThreads(nThreads = 8)

# Set working directory
setwd("~/GSE261875_WGCNA")
```

> 

**Installation Note:** WGCNA requires R version 3.6 or higher. The installation may take several minutes as it includes many dependencies. If you encounter compilation errors, make sure you have the necessary development tools installed on your system.

## Downloading and Preparing Example Data

For this tutorial, we’ll continue working with dataset **GSE261875** from our [previous clustering tutorial](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/). This dataset examines TDP-43 perturbation in motor neurons with and without Ataxin-2 knockout, making it ideal for demonstrating WGCNA’s ability to identify treatment-responsive gene networks and assess how genetic background affects co-expression patterns.

### Experimental Groups

The dataset contains 32 samples across 8 conditions (4 biological replicates each):

**Wild-type background:**

- **EV:** Empty vector control

- **Y:** YFP expression control

- **T:** TDP-43 overexpression

- **N:** TDP-43ΔNLS (cytoplasmic TDP-43)

**Ataxin-2 knockout background:**

- **EV_AKO, Y_AKO, T_AKO, N_AKO:** Same conditions with ATXN2 knockout

For full dataset details, see the [GEO entry GSE261875](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE261875).

> 

**Building on Previous Work:** If you completed our [clustering tutorial](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/), you already have the preprocessed files. If you’re starting fresh, consult that tutorial first for data downloading and normalization.

### Loading Pre-processed Data

Load the normalized expression data and sample information from the [clustering tutorial](https://ngs101.com/how-to-cluster-rna-seq-data-to-uncover-gene-expression-patterns-hierarchical-and-k-means-methods-for-absolute-beginners/):

```r
#-----------------------------------------------
# STEP 1: Load pre-processed RNA-seq data
#-----------------------------------------------

# Load normalized expression data
# Rows are genes, columns are samples
expr_data <- read.table("normalized_counts.tsv", header = TRUE, row.names = 1)

# Load sample information
# Contains treatment/condition information for each sample
pheno_data <- read.table("sample_info.tsv", header = TRUE, row.names = 1)

# Load gene annotation data
# Contains ENSEMBL IDs and gene symbols
gene_data <- read.table("gene_data.tsv", header = TRUE)
```

> 

**Data Format Note:** The normalized_counts.tsv file should contain variance-stabilized or rlog-transformed counts from DESeq2, not raw counts. WGCNA requires data on a roughly normal scale to calculate meaningful correlations.

### Data Quality Control and Filtering

Before constructing networks, we need to ensure data quality and remove genes with low variance or missing values:

```r
#-----------------------------------------------
# STEP 2: Quality control and data filtering
#-----------------------------------------------

# Check for missing values
missing_count <- sum(is.na(expr_data))
cat("Number of missing values:", missing_count, "\n")
# Number of missing values: 0

# Remove genes with missing values if any
if (missing_count > 0) {
  expr_data <- expr_data[complete.cases(expr_data), ]
  cat("Removed genes with missing values. New dimension:", dim(expr_data), "\n")
}

# Calculate variance for each gene
gene_variance <- apply(expr_data, 1, var)

# Visualize variance distribution
png("plots/01_gene_variance_distribution.png", width = 800, height = 600, res = 100)
hist(log10(gene_variance), 
     breaks = 50,
     main = "Distribution of Gene Variance (log10)",
     xlab = "log10(Variance)",
     col = "lightblue",
     border = "white")
abline(v = log10(quantile(gene_variance, 0.4)), 
       col = "red", lty = 2, lwd = 2)
legend("topright", 
       legend = "40th percentile cutoff",
       col = "red", lty = 2, lwd = 2,
       bty = "n")
dev.off()

# Remove genes with very low variance (bottom 40%)
# These genes provide little information for co-expression analysis
variance_threshold <- quantile(gene_variance, 0.4)
cat("Variance threshold (40th percentile):", variance_threshold, "\n")
# Variance threshold (40th percentile): 0.04054851

expr_data_filtered <- expr_data[gene_variance > variance_threshold, ]
cat("Genes after variance filtering:", nrow(expr_data_filtered), "\n")
# Genes after variance filtering: 10853

# Transpose for WGCNA (samples as rows, genes as columns)
expr_matrix <- t(expr_data_filtered)

# Check sample clustering to identify outliers
sample_tree <- hclust(dist(expr_matrix), method = "average")
png("plots/02_sample_clustering_outliers.png", width = 1200, height = 600, res = 100)
par(mar = c(2, 5, 2, 2))
plot(sample_tree, 
     main = "Sample Clustering to Detect Outliers",
     sub = "Height represents dissimilarity between samples",
     xlab = "", 
     cex = 0.7)
abline(h = 200, col = "red", lty = 2, lwd = 2)
dev.off()
```

> 

**Quality Control Insight:** WGCNA is sensitive to outlier samples that can distort correlation patterns. Always inspect sample clustering before proceeding. Samples that cluster far from others may represent technical failures, mislabeled samples, or genuine biological outliers. For this tutorial, we’ll assume no obvious outliers after visual inspection.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/01_02_variance_outlier-e1764194637727-1024x309.png?resize=1024%2C309&ssl=1)

## Data Preparation for WGCNA

WGCNA has specific requirements for input data format and preprocessing. Let’s prepare our expression matrix and trait data properly.

### Creating the Trait Data Matrix

For WGCNA to correlate modules with experimental conditions, we need to create a trait data matrix where each column represents a condition and values are binary (0 or 1):

```r
#-----------------------------------------------
# STEP 3: Prepare trait data matrix
#-----------------------------------------------

# Create binary trait matrix for each experimental condition
# Each column represents presence (1) or absence (0) of that condition
trait_data <- data.frame(
  EV = as.numeric(pheno_data$Treatment == "EV"),
  Y = as.numeric(pheno_data$Treatment == "Y"),
  T = as.numeric(pheno_data$Treatment == "T"),
  N = as.numeric(pheno_data$Treatment == "N"),
  EV_AKO = as.numeric(pheno_data$Treatment == "EV_AKO"),
  Y_AKO = as.numeric(pheno_data$Treatment == "Y_AKO"),
  T_AKO = as.numeric(pheno_data$Treatment == "T_AKO"),
  N_AKO = as.numeric(pheno_data$Treatment == "N_AKO")
)

# Set row names to match expression matrix
rownames(trait_data) <- rownames(expr_matrix)

# Verify that sample names match between expression and trait data
if (!all(rownames(expr_matrix) == rownames(trait_data))) {
  stop("ERROR: Sample names don't match between expression and trait data!")
}
```

> 

**Trait Data Structure:** Each row in trait_data corresponds to a sample, and each column represents a specific experimental condition. The binary encoding (0/1) allows WGCNA to calculate correlations between module eigengenes and experimental conditions.

**trait_data:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/trait_data.png?resize=1024%2C379&ssl=1)

### Understanding the Data Structure

Before proceeding, let’s verify our data is properly formatted:

```bash
#-----------------------------------------------
# STEP 4: Verify data structure
#-----------------------------------------------

# Visualize expression data distribution
png("plots/03_expression_data_distribution.png", width = 1200, height = 600, res = 100)
par(mfrow = c(1, 2))

# Overall distribution
hist(as.matrix(expr_matrix), 
     breaks = 100, 
     main = "Distribution of Expression Values",
     sub = "Should be roughly normal for VST/rlog data",
     xlab = "Expression Value",
     col = "lightblue",
     border = "white")

# Sample distributions (boxplot)
boxplot(expr_matrix[, sample(1:ncol(expr_matrix), min(20, ncol(expr_matrix)))], 
        main = "Sample Expression Distributions\n(20 random genes)",
        xlab = "Genes",
        ylab = "Expression Value",
        las = 2,
        cex.axis = 0.6,
        col = "lightblue",
        border = "gray40")

par(mfrow = c(1, 1))
dev.off()
```

> 

**Data Format Requirements:** WGCNA requires:

1. Sample-by-gene matrix (samples in rows, genes in columns)

2. Normalized data on a comparable scale

3. No missing or infinite values

4. Appropriate transformation (VST or rlog for RNA-seq count data)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/03_expression_data_distribution.png?resize=1024%2C512&ssl=1)

## Building the Gene Co-expression Network

Now we’re ready to construct the weighted gene co-expression network. This is the core of WGCNA analysis.

### Understanding WGCNA Parameters

Before running the analysis, let’s understand the key parameters:

**Soft-Thresholding Power (β):**

- Determines how strongly high correlations are weighted compared to low correlations

- Higher values create more scale-free networks but may lose weak but meaningful connections

- Typically ranges from 1-20; we’ll use diagnostic plots to choose the optimal value

**Minimum Module Size:**

- Smallest number of genes allowed in a module

- Smaller values detect more fine-grained modules but may include noise

- Typical values: 30-50 genes for large datasets

**Merge Cut Height:**

- Threshold for merging similar modules

- Lower values keep modules separate; higher values merge more aggressively

- Typical value: 0.25 (modules with >75% similar eigengenes are merged)

**Network Type:**

- “unsigned”: absolute value of correlation (genes with opposite expression patterns can be in same module)

- “signed”: preserves direction (positive or negative correlation)

- “signed hybrid”: intermediate approach

- For most biological applications, “signed” is recommended

### Step 1: Choose Soft-Thresholding Power

The first critical step is selecting the appropriate soft-thresholding power (β):

```r
#-----------------------------------------------
# STEP 5: Choose soft-thresholding power
#-----------------------------------------------

# Calculate network topology for various soft-thresholding powers
powers <- c(seq(1, 10, by = 1), seq(12, 20, by = 2))

# This function tests different power values
sft <- pickSoftThreshold(
  expr_matrix,
  powerVector = powers,
  networkType = "signed",
  verbose = 3
)

# Plot scale-free topology fit index as a function of power
par(mfrow = c(1, 2))

# Scale-free topology fit
png("plots/04_soft_thresholding_power_selection.png", width = 1200, height = 600, res = 100)
par(mfrow = c(1, 2))

# Scale-free topology fit
plot(sft$fitIndices[, 1],
     -sign(sft$fitIndices[, 3]) * sft$fitIndices[, 2],
     xlab = "Soft Threshold (power)",
     ylab = "Scale Free Topology Model Fit, signed R^2",
     main = "Scale Independence",
     type = "n")
text(sft$fitIndices[, 1],
     -sign(sft$fitIndices[, 3]) * sft$fitIndices[, 2],
     labels = powers, 
     cex = 0.9, 
     col = "red")
abline(h = 0.85, col = "red", lty = 2)

# Mean connectivity
plot(sft$fitIndices[, 1],
     sft$fitIndices[, 5],
     xlab = "Soft Threshold (power)",
     ylab = "Mean Connectivity",
     main = "Mean Connectivity",
     type = "n")
text(sft$fitIndices[, 1],
     sft$fitIndices[, 5],
     labels = powers, 
     cex = 0.9, 
     col = "red")

par(mfrow = c(1, 1))
dev.off()

# Choose the power: first value that reaches plateau in scale-free topology
# and gives reasonable mean connectivity
soft_power <- sft$powerEstimate
if (is.na(soft_power)) {
  soft_power <- 6  # Use 6 as default if automatic selection fails
  cat("Warning: Automatic power selection failed. Using default power =", soft_power, "\n")
} else {
  cat("Selected soft-thresholding power:", soft_power, "\n")
}
```

> 

**Parameter Selection Guide:** Choose the lowest power for which the scale-free topology fit index (R²) reaches 0.80-0.85. This balances creating a scale-free network topology (characteristic of biological networks) with maintaining sufficient connectivity to detect meaningful relationships. If no power achieves R² > 0.8, your data may not perfectly follow scale-free structure, but you can still proceed with the analysis using a reasonable power value (typically 6-12).

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/04_soft_thresholding_power_selection.png?resize=1024%2C512&ssl=1)

### Step 2: Construct Network and Detect Modules

With the soft-thresholding power selected, we can now build the network. **Important:** Before running the main network construction, we need to handle a bug in WGCNA related to the `cor()` function:

```r
#-----------------------------------------------
# STEP 6: Construct network and identify modules
#-----------------------------------------------

# CRITICAL: Fix for WGCNA cor() function bug
# WGCNA has its own cor() function that conflicts with stats::cor()
cor <- WGCNA::cor

# One-step network construction and module detection
net <- blockwiseModules(
  expr_matrix,
  power = soft_power,
  networkType = "signed",
  TOMType = "signed",
  minModuleSize = 30,
  reassignThreshold = 0,
  mergeCutHeight = 0.25,
  numericLabels = TRUE,
  pamRespectsDendro = FALSE,
  saveTOMs = TRUE,
  saveTOMFileBase = "GSE261875_TOM",
  verbose = 3
)

# Restore the standard cor() function after network construction
cor <- stats::cor

# Convert numeric module labels to colors for visualization
module_colors <- labels2colors(net$colors)

# Count genes in each module
module_table <- table(module_colors)
cat("\nNumber of modules identified:", length(unique(module_colors)) - 1, "\n")
# Number of modules identified: 27 
print(module_table)
# black blue brown cyan
# 560 1694 1060 216 

# Plot the dendrogram with module colors
png("plots/05_gene_dendrogram_modules.png", width = 1200, height = 800, res = 100)
plotDendroAndColors(
  net$dendrograms[[1]],
  module_colors[net$blockGenes[[1]]],
  "Module colors",
  dendroLabels = FALSE,
  hang = 0.03,
  addGuide = TRUE,
  guideHang = 0.05,
  main = "Gene Dendrogram and Module Colors"
)
dev.off()
```

> 

**Understanding the cor() Bug:** WGCNA package includes its own `cor()` function that has additional parameters beyond the standard R `stats::cor()` function. When WGCNA internally calls correlation functions, it may try to use these extra parameters, causing an error if the standard `cor()` is loaded instead. By temporarily setting `cor <- WGCNA::cor` before network construction and restoring `cor <- stats::cor` afterward, we ensure the correct function is used at each step. This is a known issue in WGCNA versions 1.70 and later.

**What Just Happened:** WGCNA calculated correlations between all gene pairs, converted them to adjacencies using soft-thresholding, computed the Topological Overlap Matrix, performed hierarchical clustering, and detected modules using dynamic tree cutting. Each module (represented by a color) contains genes with highly correlated expression patterns. The “gray” module contains genes that didn’t fit well into any module.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/05_gene_dendrogram_modules.png?resize=1024%2C683&ssl=1)

**The structure of the “net” object from “blockwiseModules”:**

```
net (LIST object from blockwiseModules)
│
├─ colors (named numeric vector)
│  │
│  │  WHAT IT IS: Module assignment for each gene
│  │  PURPOSE: Tells you which module each gene belongs to
│  │  WHY NUMERIC: Internal representation (0, 1, 2, 3...)
│  │              0 = unassigned/grey, 1 = largest module, 2 = 2nd largest, etc.
│  │
│  ├─ Length: 10,853 (one value per gene)
│  ├─ Names: ENSG00000000001, ENSG00000000002, ...
│  └─ Values: 1, 2, 3, 0, 1, 2, 3, 4, ...
│     
│     Example:
│     ENSG00000000001 → 1  (this gene is in module 1 = turquoise)
│     ENSG00000000002 → 2  (this gene is in module 2 = blue)
│     ENSG00000000003 → 1  (this gene is in module 1 = turquoise)
│     ENSG00000000004 → 0  (this gene is unassigned = grey)
│
├─ MEs (data frame of Module Eigengenes)
│  │
│  │  WHAT IT IS: Summary expression profile for each module
│  │  PURPOSE: Represents the "average" expression of all genes in each module
│  │  HOW TO USE: Correlate with traits, compare across conditions
│  │  THINK OF IT AS: Each module's "signature" expression pattern
│  │
│  ├─ Dimensions: 32 samples × 19 modules
│  ├─ Rownames: Sample IDs (SRR001, SRR002, ...)
│  └─ Colnames: ME0, ME1, ME2, ME3, ... (ME = Module Eigengene)
│     
│     Example (first 3 samples, 3 modules):
│                 ME0       ME1       ME2
│     SRR001   -0.142    0.251    -0.089  (module 1 is high in this sample)
│     SRR002    0.098   -0.113     0.156  (module 1 is low in this sample)
│     SRR003   -0.056    0.187    -0.234  (module 2 is very low here)
│
│     Interpretation: If ME1 = 0.251, the turquoise module is 
│                     strongly expressed in sample SRR001
│
├─ dendrograms (list of dendrogram objects)
│  │
│  │  WHAT IT IS: Hierarchical clustering tree showing gene relationships
│  │  PURPOSE: Visualizes how genes cluster together based on co-expression
│  │  HOW TO USE: Plot with plotDendroAndColors() to see module structure
│  │  WHY IT MATTERS: Shows which genes are most similar to each other
│  │
│  └─ [[1]] (dendrogram object)
│      │
│      └─ Hierarchical clustering tree of all 10,853 genes
│         Branch height = dissimilarity (1 - topological overlap)
│         Genes that branch together early = highly co-expressed
│         Used for: plotDendroAndColors() visualization
│
├─ blockGenes (list of gene indices per block)
│  │
│  │  WHAT IT IS: Which genes were analyzed in each computational block
│  │  PURPOSE: WGCNA splits large gene sets into blocks for memory efficiency
│  │  WHEN IT MATTERS: If you have >5000 genes, analysis is done in chunks
│  │  FOR THIS DATASET: Only one block because we have 10,853 genes
│  │                    (could be split if you had 50,000+ genes)
│  │
│  └─ [[1]] (numeric vector of gene indices)
│      │
│      └─ 1, 2, 3, 4, 5, ..., 10853
│         Maps to: ENSG00000000001, ENSG00000000002, etc.
│         All genes in block 1 (no splitting needed for this dataset)
│
├─ goodGenes (logical vector)
│  │
│  │  WHAT IT IS: Quality control flag for each gene
│  │  PURPOSE: Indicates which genes passed WGCNA's internal QC checks
│  │  WHY SOME ARE FALSE: Genes with too many missing values or zero variance
│  │  HOW TO USE: Rarely needed - WGCNA automatically excludes bad genes
│  │
│  ├─ Length: 10,853
│  └─ Values: TRUE, TRUE, TRUE, FALSE, TRUE, ...
│     
│     TRUE = gene included in analysis
│     FALSE = gene excluded (missing data or zero variance)
│
├─ TOMFiles (character vector of file paths)
│  │
│  │  WHAT IT IS: Path to saved Topological Overlap Matrix file(s)
│  │  PURPOSE: TOM is HUGE (genes × genes matrix), saved to disk to save RAM
│  │  SIZE: ~1.2 GB for 10,853 genes (grows as N²)
│  │  HOW TO USE: Load with load() if you need to recalculate something
│  │  WHY SAVE IT: Calculating TOM is slow; save it to avoid recomputing
│  │
│  └─ "GSE261875_TOM-block.1.RData"
│     
│     Contains: 10,853 × 10,853 matrix of topological overlap values
│     Range: 0 (no overlap) to 1 (perfect overlap)
│     Memory: Not loaded in RAM unless you explicitly load() it
│
├─ unmergedColors (numeric vector)
│  │
│  │  WHAT IT IS: Initial module assignments BEFORE merging similar modules
│  │  PURPOSE: Shows what modules looked like before mergeCutHeight applied
│  │  HOW TO USE: Compare with final colors to see which modules were merged
│  │  RARELY NEEDED: Useful only for troubleshooting module detection
│  │
│  └─ Same length as colors, but typically has MORE unique values
│     Example: 45 initial modules → 19 final modules after merging
│
├─ MEsOK (logical)
│  │
│  │  WHAT IT IS: Did module eigengene calculation succeed?
│  │  PURPOSE: Internal QC check
│  │  VALUE: TRUE (everything worked) or FALSE (something failed)
│  │  RARELY NEEDED: If FALSE, there's a serious problem with your data
│  │
│  └─ TRUE (for successful analysis)
│
└─ Other less commonly used components
   ├─ blockInfo: Details about block structure
   ├─ dendrogramBranches: Raw dendrogram data
   ├─ mergedColors: Same as colors (kept for compatibility)
   └─ ...
```

### Step 3: Calculate Module Eigengenes

Module eigengenes summarize the expression profile of each module:

```r
#-----------------------------------------------
# STEP 7: Calculate and visualize module eigengenes
#-----------------------------------------------

# Extract module eigengenes
module_eigengenes <- net$MEs

# Reorder modules by hierarchical clustering of eigengenes
module_eigengenes <- orderMEs(module_eigengenes)

# Visualize module eigengenes across samples
# Create a heatmap showing how each module's expression varies across samples
png("plots/06_module_eigengenes_heatmap.png", width = 1200, height = 1000, res = 100)
pheatmap(
  t(module_eigengenes),
  scale = "row",
  clustering_distance_cols = "euclidean",
  clustering_method = "average",
  annotation_col = trait_data,
  show_colnames = FALSE,
  main = "Module Eigengenes Across Samples",
  color = colorRampPalette(c("blue", "white", "red"))(50),
  fontsize = 8
)
dev.off()
```

> 

**Module Eigengene Concept:** The module eigengene (ME) is the first principal component of a module’s expression matrix. It represents the average expression pattern of all genes in that module. Think of it as a “summary expression profile” for the module—if the ME is high in a sample, most genes in that module are highly expressed in that sample.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/06_module_eigengenes_heatmap.png?resize=1024%2C853&ssl=1)

## Downstream Analysis: Connecting Modules to Biology

With modules identified, we can now explore their biological significance through several downstream analyses.

### Module-Trait Association Analysis

One of the most powerful features of WGCNA is correlating modules with experimental conditions:

```r
#-----------------------------------------------
# STEP 8: Module-trait association analysis
#-----------------------------------------------

# Calculate correlations between module eigengenes and traits
module_trait_cor <- cor(module_eigengenes, trait_data, use = "pairwise.complete.obs")

# Calculate p-values for these correlations
nSamples <- nrow(expr_matrix)
module_trait_pvalue <- corPvalueStudent(module_trait_cor, nSamples)

# Create a text matrix for the heatmap
# Format: correlation coefficient (p-value)
textMatrix <- paste(signif(module_trait_cor, 2), "\n(",
                    signif(module_trait_pvalue, 1), ")", sep = "")
dim(textMatrix) <- dim(module_trait_cor)

# Visualize module-trait relationships with a heatmap
png("plots/07_module_trait_relationships.png", width = 1200, height = 1000, res = 100)
par(mar = c(8, 10, 3, 3))
labeledHeatmap(
  Matrix = module_trait_cor,
  xLabels = names(trait_data),
  yLabels = names(module_eigengenes),
  ySymbols = names(module_eigengenes),
  colorLabels = FALSE,
  colors = blueWhiteRed(50),
  textMatrix = textMatrix,
  setStdMargins = FALSE,
  cex.text = 0.5,
  zlim = c(-1, 1),
  main = "Module-Trait Relationships"
)
dev.off()
```

> 

**Biological Interpretation:** Modules with strong positive correlations (red in heatmap) are upregulated in that condition; strong negative correlations (blue) indicate downregulation. The p-values tell us whether these associations are statistically significant. Focus on modules with |correlation| > 0.5 and p < 0.05 for follow-up analysis. For example, if a module shows strong positive correlation with T_AKO, it suggests those genes are collectively upregulated when transcription factor T is overexpressed in the AKO genetic background.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/07_module_trait_relationships.png?resize=1024%2C853&ssl=1)

### Gene Significance and Module Membership

For trait-associated modules, we can quantify each gene’s importance using two metrics: gene significance (GS) and module membership (MM):

```r
#-----------------------------------------------
# STEP 9: Gene significance and module membership
#-----------------------------------------------

# Calculate gene significance (correlation with trait) for T_AKO
gene_trait_cor <- cor(expr_matrix, trait_data$T_AKO, use = "pairwise.complete.obs")
gene_trait_pvalue <- corPvalueStudent(as.numeric(gene_trait_cor), nSamples)

# Add gene names to the p-value vector
names(gene_trait_cor) <- colnames(expr_matrix)
names(gene_trait_pvalue) <- colnames(expr_matrix)

# Get the module most strongly associated with T_AKO
t_ako_correlations <- abs(module_trait_cor[, "T_AKO"])
t_ako_module <- names(module_eigengenes)[which.max(t_ako_correlations)]
cat("\nModule most strongly associated with T_AKO:", t_ako_module, "\n")
# Module most strongly associated with T_AKO: ME8

# Convert numeric module label to color name
numeric_label <- gsub("ME", "", t_ako_module)
module_name <- labels2colors(as.numeric(numeric_label))
cat("Module color name:", module_name, "\n")

# Get genes in this module
module_genes <- names(module_colors)[module_colors == module_name]
cat("Number of genes in", module_name, "module:", length(module_genes), "\n")
# Number of genes in pink module: 362

# Calculate module membership for these genes
# MM measures how correlated each gene is with the module eigengene
gene_module_membership <- cor(expr_matrix[, module_genes],
                               module_eigengenes[, t_ako_module],
                               use = "pairwise.complete.obs")

# Create a data frame with gene information
module_gene_info <- data.frame(
  Gene = module_genes,
  ModuleMembership = as.numeric(gene_module_membership),
  GeneSignificance = gene_trait_cor[module_genes],
  MM_pvalue = corPvalueStudent(as.numeric(gene_module_membership), nSamples),
  GS_pvalue = gene_trait_pvalue[module_genes],
  stringsAsFactors = FALSE
)

# Sort by module membership
module_gene_info <- module_gene_info[order(-abs(module_gene_info$ModuleMembership)), ]

# Plot gene significance vs. module membership
png(paste0("plots/08_MM_vs_GS_", module_name, "_module.png"), 
    width = 800, height = 600, res = 100)
plot(abs(module_gene_info$ModuleMembership), 
     abs(module_gene_info$GeneSignificance),
     xlab = paste("Module Membership in", module_name, "module"),
     ylab = "Gene Significance for T_AKO",
     main = paste("MM vs GS in", module_name, "module"),
     pch = 20, 
     col = module_name,
     cex = 1.2)

# Add regression line
abline(lm(abs(module_gene_info$GeneSignificance) ~ abs(module_gene_info$ModuleMembership)), 
       col = "red", lwd = 2)

# Calculate and display correlation
mm_gs_cor <- cor(abs(module_gene_info$ModuleMembership), 
                 abs(module_gene_info$GeneSignificance),
                 use = "pairwise.complete.obs")
text(x = 0.2, y = max(abs(module_gene_info$GeneSignificance)) * 0.95,
     labels = paste("cor =", round(mm_gs_cor, 3)),
     pos = 4, cex = 1.2)
dev.off()
```

> 

**Key Concept – Module Membership vs Gene Significance:**

- **Module Membership (MM):** Measures how strongly a gene’s expression correlates with the module eigengene. High MM (>0.7) indicates the gene is central to the module’s expression pattern.

- **Gene Significance (GS):** Measures how strongly a gene’s expression correlates with the trait of interest. High GS (>0.5) indicates the gene is individually associated with the trait.

- **High MM + High GS:** These genes are excellent candidates for follow-up—they’re central to a trait-associated module AND individually correlated with the trait. They likely play important roles in the biological response.

- A strong positive correlation between MM and GS indicates the entire module is relevant to the trait, not just a few outlier genes.

**module_gene_info:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/module_gene_info.png?resize=1024%2C249&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/08_MM_vs_GS_pink_module.png?resize=800%2C600&ssl=1)

### Hub Gene Identification

Hub genes are highly connected within modules and often play regulatory roles:

```r
#-----------------------------------------------
# STEP 10: Identify hub genes in T_AKO-associated module
#-----------------------------------------------

# Calculate connectivity (intramodular connectivity)
# First, calculate adjacency matrix for genes in this module
adjacency_matrix <- adjacency(
  expr_matrix[, module_genes],
  power = soft_power,
  type = "signed"
)

# Calculate connectivity: sum of connection weights for each gene
# (subtract 1 to exclude self-connection)
connectivity <- rowSums(adjacency_matrix) - 1

# Combine all metrics for hub gene identification
hub_gene_info <- data.frame(
  Gene = module_genes,
  Connectivity = connectivity,
  ModuleMembership = as.numeric(gene_module_membership),
  GeneSignificance = gene_significance,
  stringsAsFactors = FALSE
)

# Sort by connectivity to identify hubs
hub_gene_info <- hub_gene_info[order(-hub_gene_info$Connectivity), ]

# Map ENSEMBL IDs to gene symbols for better interpretation
# Merge with gene annotation data
hub_gene_info_annotated <- merge(hub_gene_info, gene_data, 
                                  by.x = "Gene", by.y = "ENSEMBL",
                                  all.x = TRUE)

# Sort again by connectivity after merge
hub_gene_info_annotated <- hub_gene_info_annotated[order(-hub_gene_info_annotated$Connectivity), ]

# Visualize hub genes
# Plot connectivity vs module membership
png(paste0("plots/09_hub_genes_", module_name, "_module.png"), 
    width = 800, height = 600, res = 100)
plot(hub_gene_info$ModuleMembership, 
     hub_gene_info$Connectivity,
     xlab = "Module Membership",
     ylab = "Connectivity (Intramodular)",
     main = paste("Hub Gene Identification in", module_name, "Module"),
     pch = 20,
     col = ifelse(hub_gene_info$Connectivity > quantile(hub_gene_info$Connectivity, 0.9),
                  "red", "black"))
legend("topright", 
       legend = c("Top 10% connected", "Other genes"),
       col = c("red", "black"),
       pch = 20)
dev.off()
```

> 

**Hub Gene Biology:** Hub genes with high connectivity are often:

- **Transcription factors** that regulate many other genes in the module

- **Key enzymes** in metabolic or signaling pathways

- **Structural proteins** in large complexes

These genes are prime candidates for experimental validation because perturbations to hub genes often have large, measurable effects on module function and phenotype. In the context of this experiment, hub genes in the T_AKO-associated module might represent key effectors of the transcription factor T’s function, especially in the AKO genetic background.

**hub_gene_info_annotated:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/hub_gene_info_annotated.png?resize=1024%2C207&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/09_hub_genes_pink_module.png?resize=800%2C600&ssl=1)

### Functional Enrichment Analysis

Let’s examine what biological processes are enriched in our trait-associated modules:

```r
#-----------------------------------------------
# STEP 11: Functional enrichment of module genes
#-----------------------------------------------

# Get gene symbols for the module of interest
module_gene_symbols <- hub_gene_info_annotated$SYMBOL[!is.na(hub_gene_info_annotated$SYMBOL)]

# Perform GO enrichment for T_AKO-associated module
go_enrichment <- enrichGO(
  gene = module_gene_symbols,
  OrgDb = org.Hs.eg.db,
  keyType = "SYMBOL",
  ont = "BP",              # Biological Process
  pAdjustMethod = "BH",    # Benjamini-Hochberg correction
  pvalueCutoff = 0.05,
  qvalueCutoff = 0.05,
  readable = TRUE
)

# GO Enrichment Barplot
if (!is.null(go_enrichment) && nrow(go_enrichment) > 0) {
  png(paste0("plots/11_GO_enrichment_barplot_", module_name, ".png"), 
      width = 1000, height = 800, res = 100)
barplot(go_enrichment, 
                showCategory = 15,
                title = paste("GO Enrichment in", module_name, "Module"))
  dev.off()
}

# KEGG Pathway Enrichment
# Convert gene symbols to Entrez IDs (required for KEGG)
gene_entrez <- bitr(module_gene_symbols, 
                    fromType = "SYMBOL",
                    toType = "ENTREZID", 
                    OrgDb = org.Hs.eg.db)

kegg_enrichment <- enrichKEGG(
    gene = gene_entrez$ENTREZID,
    organism = "hsa",
    pAdjustMethod = "BH",
    pvalueCutoff = 0.05,
    qvalueCutoff = 0.05
  )

# KEGG Pathway Enrichment
if (!is.null(kegg_enrichment) && nrow(kegg_enrichment) > 0) {
  png(paste0("plots/12_KEGG_enrichment_", module_name, ".png"), 
      width = 1000, height = 800, res = 100)
dotplot(kegg_enrichment, 
                showCategory = 15,
                title = paste("KEGG Pathway Enrichment in", module_name, "Module"))
  dev.off()
}
```

> 

The enriched terms validate the module’s biological relevance and provide hypotheses about the molecular mechanisms underlying the phenotypic effects of treatment T in the AKO background.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/GO_KEGG-e1764195313852-1024x408.png?resize=1024%2C408&ssl=1)

### Constructing Protein-Protein Interaction Networks

Hub genes often physically interact. Let’s visualize their PPI network using STRING database:

```r
#-----------------------------------------------
# STEP 12: Construct PPI network for hub genes
#-----------------------------------------------

# Get top 20 hub genes for PPI network analysis
top_hub_genes <- head(hub_gene_info_annotated$SYMBOL[!is.na(hub_gene_info_annotated$SYMBOL)], 20)

# Initialize STRING database
# Note: species 9606 is Homo sapiens; use 10090 for Mus musculus
string_db <- STRINGdb$new(version = "11.5", 
                          species = 9606,
                          score_threshold = 400)

# Map gene symbols to STRING IDs
hub_genes_df <- data.frame(gene = top_hub_genes, stringsAsFactors = FALSE)
hub_genes_mapped <- string_db$map(hub_genes_df, "gene", removeUnmappedRows = TRUE)

if (nrow(hub_genes_mapped) > 1) {
  # Get interactions among hub genes
  interactions <- string_db$get_interactions(hub_genes_mapped$STRING_id)

  if (nrow(interactions) > 0) {
    cat("Found", nrow(interactions), "interactions among hub genes\n")

    # Create a mapping from STRING_id to gene symbol
    id_to_gene <- setNames(hub_genes_mapped$gene, hub_genes_mapped$STRING_id)

    # Replace STRING IDs with gene symbols in interactions
    interactions$from_gene <- id_to_gene[interactions$from]
    interactions$to_gene <- id_to_gene[interactions$to]

    # Create igraph network object with gene names
    ppi_graph <- graph_from_data_frame(
      interactions[, c("from_gene", "to_gene")],
      directed = FALSE
    )

    # Calculate network properties
    cat("\nPPI Network properties:\n")
    cat("  Nodes:", vcount(ppi_graph), "\n")
    cat("  Edges:", ecount(ppi_graph), "\n")
    cat("  Network density:", edge_density(ppi_graph), "\n")

    # Plot PPI network
    png(paste0("plots/13_PPI_network_", module_name, ".png"), 
        width = 1000, height = 1000, res = 100)
    par(mar = c(1, 1, 3, 1))
    plot(ppi_graph,
         vertex.size = 12,
         vertex.color = "lightblue",
         vertex.label.cex = 0.7,
         vertex.label.color = "black",
         edge.color = "gray70",
         edge.width = 2,
         layout = layout_with_fr(ppi_graph),
         main = paste("Protein-Protein Interactions Among Hub Genes\n", 
                     module_name, "Module"))
    dev.off()

    # Identify highly connected proteins in PPI network
    ppi_degree <- degree(ppi_graph)
    cat("\nMost connected proteins in PPI network:\n")
    print(sort(ppi_degree, decreasing = TRUE))

  } else {
    cat("No interactions found among the hub genes in STRING database\n")
  }
} else {
  cat("Insufficient genes mapped to STRING database for network analysis\n")
}
```

> 

**Integration Power:** Combining co-expression networks with PPI data strengthens biological interpretation:

- **Co-expressed + Physical interaction:** Strong evidence for functional relationship

- **Co-expressed but no PPI:** May be indirect regulation (e.g., transcription factor → target genes)

- **PPI but not co-expressed:** May interact conditionally or post-transcriptionally

Genes that are both co-expressed and physically interact are the highest-confidence candidates for functional studies. They likely work together directly in cellular processes affected by treatment T in the AKO genetic background.

**interactions:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/Hubgene_sting_interactions.png?resize=1024%2C203&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/13_PPI_network_pink.png?resize=1000%2C1000&ssl=1)

## Network and Module Visualization

Effective visualization helps communicate network structure and module relationships.

### Visualizing the Complete Network

Let’s create a network visualization showing module structure using a subset of genes:

```r
#-----------------------------------------------
# STEP 13: Network visualization
#-----------------------------------------------

# Calculate TOM for a subset of genes
# Using all genes would create a very large matrix
# Select top genes by variance for visualization
top_genes_var <- names(sort(apply(expr_matrix, 2, var), decreasing = TRUE)[1:1000])

cat("Calculating TOM for top 1000 variable genes...\n")

# Calculate adjacency matrix for selected genes
adj_subset <- adjacency(expr_matrix[, top_genes_var], 
                        power = soft_power, 
                        type = "signed")

# Calculate TOM
tom_subset <- TOMsimilarity(adj_subset)
colnames(tom_subset) <- top_genes_var
rownames(tom_subset) <- top_genes_var

# Create network from TOM (keep only strong connections)
# Keep top 5% of connections
tom_threshold <- quantile(tom_subset[upper.tri(tom_subset)], 0.95)
cat("TOM threshold (95th percentile):", tom_threshold, "\n")
# TOM threshold (95th percentile): 0.4136307 

adjacency_threshold <- tom_subset
adjacency_threshold[adjacency_threshold < tom_threshold] <- 0

# Create igraph object
network_graph <- graph_from_adjacency_matrix(
  adjacency_threshold,
  mode = "undirected",
  weighted = TRUE,
  diag = FALSE
)

# Get module colors for these genes
node_colors <- module_colors[match(top_genes_var, names(module_colors))]

# Calculate node sizes based on connectivity
node_sizes <- log10(degree(network_graph) + 1) * 3 + 3

# Plot network
png("plots/14_coexpression_network.png", width = 1200, height = 1200, res = 100)
par(mar = c(1, 1, 3, 1))
set.seed(123)
plot(network_graph,
     vertex.size = node_sizes,
     vertex.label = NA,
     vertex.color = node_colors,
     vertex.frame.color = NA,
     edge.width = 0.5,
     edge.color = "gray80",
     layout = layout_with_fr(network_graph),
     main = "Co-expression Network\n(Top 1000 Variable Genes, Top 5% Connections)")

unique_colors <- unique(node_colors[node_colors != "grey"])
if (length(unique_colors) <= 10) {
  legend("topright",
         legend = unique_colors,
         col = unique_colors,
         pch = 19,
         cex = 0.7,
         title = "Modules",
         bty = "n")
}
dev.off()
```

> 

**Network Interpretation:** In this visualization:

- **Nodes (dots):** Individual genes

- **Edges (lines):** Strong co-expression relationships (top 5% by TOM)

- **Colors:** Module assignments

- **Node size:** Connectivity (larger = more connected)

You should see clear clustering of genes by color, confirming that modules represent densely connected groups of genes. Genes that bridge between modules may play important regulatory roles.

**adjacency_threshold:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/adjacency_threshold.png?resize=1024%2C171&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/14_coexpression_network.png?resize=1024%2C1024&ssl=1)

### Module Eigengene Network

Visualize relationships between modules themselves:

```r
#-----------------------------------------------
# STEP 14: Module eigengene network
#-----------------------------------------------

# Calculate correlation between module eigengenes
me_correlation <- cor(module_eigengenes, use = "pairwise.complete.obs")

# Calculate p-values
me_pvalue <- corPvalueStudent(me_correlation, nSamples)

# Create adjacency matrix: keep correlations > 0.5 and p < 0.05
me_adjacency <- (abs(me_correlation) > 0.5) & (me_pvalue < 0.05)
diag(me_adjacency) <- FALSE  # Remove self-connections

# Check if there are any edges
n_edges <- sum(me_adjacency) / 2
cat("\nNumber of module pairs with |correlation| > 0.5 and p < 0.05:", n_edges, "\n")
# Number of module pairs with |correlation| > 0.5 and p < 0.05: 106 

if (n_edges == 0) {
  cat("\nNo strong correlations between modules at |r| > 0.5.\n")
  cat("Trying |r| > 0.3...\n\n")
  me_adjacency <- (abs(me_correlation) > 0.3) & (me_pvalue < 0.05)
  diag(me_adjacency) <- FALSE
  n_edges <- sum(me_adjacency) / 2
  correlation_threshold <- 0.3
} else {
  correlation_threshold <- 0.5
}

if (n_edges == 0) {
  cat("No significant correlations between modules even at |r| > 0.3.\n")
  cat("Modules are largely independent.\n")
} else {
  # Create network of module relationships
  me_network <- graph_from_adjacency_matrix(
    me_adjacency,
    mode = "undirected"
  )

  # Convert numeric module labels to color names
  module_names_numeric <- gsub("ME", "", names(module_eigengenes))
  module_names_colors <- labels2colors(as.numeric(module_names_numeric))

  cat("Module numeric labels:", head(module_names_numeric), "\n")
  cat("Corresponding colors:", head(module_names_colors), "\n")

  # Set vertex colors using the actual color names
  V(me_network)$color <- module_names_colors

  # Calculate node sizes based on module size
  # Use color names to get counts from module_colors table
  module_sizes_table <- table(module_colors)
  module_sizes_net <- module_sizes_table[module_names_colors]

  # Handle any missing values (modules with no genes)
  module_sizes_net[is.na(module_sizes_net)] <- 10

  node_sizes_me <- log10(as.numeric(module_sizes_net) + 1) * 10 + 10

  # Plot module network
if (n_edges > 0) {
  png("plots/15_module_eigengene_network.png", width = 1000, height = 1000, res = 100)
  par(mar = c(1, 1, 3, 1))
  set.seed(123)
  plot(me_network,
       vertex.size = node_sizes_me,
       vertex.label = module_names_colors,
       vertex.label.cex = 0.7,
       vertex.label.color = "black",
       vertex.color = module_names_colors,
       vertex.frame.color = "black",
       edge.width = 3,
       edge.color = "gray40",
       layout = layout_with_fr(me_network),
       main = paste0("Module Eigengene Network\n(|correlation| > ", 
                     correlation_threshold, ", p < 0.05)"))
  dev.off()
}}

# Also show a correlation heatmap for reference
# Use color names for row/column labels
me_cor_labeled <- me_correlation
rownames(me_cor_labeled) <- paste0(module_names_colors, " (", gsub("ME", "", rownames(me_cor_labeled)), ")")
colnames(me_cor_labeled) <- paste0(module_names_colors, " (", gsub("ME", "", colnames(me_cor_labeled)), ")")

png("plots/16_module_eigengene_correlation_heatmap.png", width = 1200, height = 1200, res = 100)
pheatmap(
  me_cor_labeled,
  color = colorRampPalette(c("blue", "white", "red"))(50),
  breaks = seq(-1, 1, length.out = 51),
  clustering_distance_rows = "correlation",
  clustering_distance_cols = "correlation",
  main = "Module Eigengene Correlation Heatmap",
  fontsize = 7
)
dev.off()
```

> 

**Module Relationships:** The eigengene network shows which modules have correlated expression patterns:

- **Connected modules:** May be co-regulated or work together in related biological processes

- **Isolated modules:** Represent independent regulatory programs

- **Node size:** Reflects the number of genes in each module

Understanding module relationships helps identify higher-order organization in the transcriptome and can reveal coordinated responses across multiple cellular processes.

**me_adjacency:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/me_adjacency.png?resize=886%2C226&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/15_16_network_heatmap-e1764196068668-1024x520.png?resize=1024%2C520&ssl=1)

### Heatmap of Module Gene Expression

Create a heatmap showing expression patterns within the T_AKO-associated module:

```r
#-----------------------------------------------
# STEP 15: Module expression heatmap
#-----------------------------------------------

# Select the T_AKO-associated module
module_numeric <- gsub("ME", "", t_ako_module)
cat("Module numeric label:", module_numeric, "\n")

# Convert numeric label to color name
module_of_interest <- labels2colors(as.numeric(module_numeric))
cat("Module color name:", module_of_interest, "\n")

# Get indices of genes in this module
module_gene_indices <- which(module_colors == module_of_interest)
cat("Number of genes in", module_of_interest, "module:", length(module_gene_indices), "\n")

# Check if we found genes
if (length(module_gene_indices) == 0) {
  cat("\nERROR: No genes found in module", module_of_interest, "\n")
  cat("Available colors in module_colors:", unique(module_colors), "\n")
  stop("Cannot create heatmap - no genes in selected module")
}

# If module is too large, select top genes by module membership
max_genes_for_heatmap <- 100
if (length(module_gene_indices) > max_genes_for_heatmap) {
  # Get MM values for genes in this module
  mm_values <- abs(cor(expr_matrix[, module_gene_indices],
                       module_eigengenes[, t_ako_module],
                       use = "pairwise.complete.obs"))

  # Select top genes by MM
  top_mm_indices <- order(mm_values, decreasing = TRUE)[1:max_genes_for_heatmap]
  selected_genes <- colnames(expr_matrix)[module_gene_indices[top_mm_indices]]
  cat("Selecting top", max_genes_for_heatmap, "genes by module membership\n")
} else {
  selected_genes <- colnames(expr_matrix)[module_gene_indices]
}

# Create annotation for samples
sample_annotation <- trait_data[, c("T_AKO", "EV", "EV_AKO")]
colnames(sample_annotation) <- c("T_AKO", "Control", "AKO_Background")

# Create heatmap
png(paste0("plots/17_expression_heatmap_", module_of_interest, "_module.png"), 
    width = 1200, height = 1000, res = 100)
pheatmap(
  t(expr_matrix[, selected_genes]),
  scale = "row",
  clustering_distance_rows = "correlation",
  clustering_distance_cols = "euclidean",
  annotation_col = sample_annotation,
  show_rownames = FALSE,
  show_colnames = FALSE,
  color = colorRampPalette(c("blue", "white", "red"))(100),
  main = paste("Expression Heatmap:", module_of_interest, "Module\n",
               "(Top", length(selected_genes), "genes by Module Membership)"),
  annotation_colors = list(
    T_AKO = c("0" = "white", "1" = "darkred"),
    Control = c("0" = "white", "1" = "darkgreen"),
    AKO_Background = c("0" = "white", "1" = "darkblue")
  )
)
dev.off()

# Save module gene list with annotations
module_gene_list <- data.frame(
  ENSEMBL = selected_genes,
  Module = module_of_interest,
  ModuleMembership = as.numeric(cor(expr_matrix[, selected_genes],
                                    module_eigengenes[, t_ako_module],
                                    use = "pairwise.complete.obs")),
  stringsAsFactors = FALSE
)

# Add gene symbols
module_gene_list <- merge(module_gene_list, gene_data, 
                          by = "ENSEMBL", all.x = TRUE)
module_gene_list <- module_gene_list[order(-abs(module_gene_list$ModuleMembership)), ]

# Save to file
output_file <- paste0(module_of_interest, "_module_genes.txt")
write.table(module_gene_list,
            file = output_file,
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)
```

> 

**Heatmap Interpretation:** The heatmap reveals:

- **Rows (genes):** Clustered by similarity in expression patterns

- **Columns (samples):** Organized by treatment condition

- **Colors:** Red = high expression, Blue = low expression

- **Column annotations:** Show which samples belong to T_AKO, control, or AKO background

You should see coordinated expression patterns within the module, with genes showing consistent up- or down-regulation in T_AKO samples compared to controls. This visual confirmation validates that the module genes truly co-vary together.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/17_expression_heatmap_pink_module.png?resize=1024%2C853&ssl=1)

## Differential Module Preservation Analysis

A powerful feature of WGCNA is comparing network structure across conditions. Let’s assess how modules differ between wild-type and AKO backgrounds.

### Building Condition-Specific Networks

```r
#-----------------------------------------------
# STEP 16: Build networks for WT and AKO conditions
#-----------------------------------------------

# Separate samples by genetic background
wt_samples <- rownames(trait_data)[trait_data$EV == 1 | 
                                     trait_data$Y == 1 | 
                                     trait_data$T == 1 | 
                                     trait_data$N == 1]
ako_samples <- rownames(trait_data)[trait_data$EV_AKO == 1 | 
                                      trait_data$Y_AKO == 1 | 
                                      trait_data$T_AKO == 1 | 
                                      trait_data$N_AKO == 1]

# Subset expression matrices
expr_wt <- expr_matrix[wt_samples, ]
expr_ako <- expr_matrix[ako_samples, ]

# Build network for WT condition
cor <- WGCNA::cor
net_wt <- blockwiseModules(
  expr_wt, 
  power = soft_power,
  networkType = "signed",
  TOMType = "signed",
  minModuleSize = 30,
  mergeCutHeight = 0.25,
  numericLabels = TRUE,
  verbose = 0
)
cor <- stats::cor

# Build network for AKO condition
cor <- WGCNA::cor
net_ako <- blockwiseModules(
  expr_ako, 
  power = soft_power,
  networkType = "signed",
  TOMType = "signed",
  minModuleSize = 30,
  mergeCutHeight = 0.25,
  numericLabels = TRUE,
  verbose = 0
)
cor <- stats::cor

# Convert to color labels
colors_wt <- labels2colors(net_wt$colors)
colors_ako <- labels2colors(net_ako$colors)

cat("\nWT network: ", length(unique(colors_wt)) - 1, "modules\n")
# WT network:  17 modules
cat("AKO network:", length(unique(colors_ako)) - 1, "modules\n")
# AKO network: 12 modules

# Plot 18a: WT Dendrogram
png("plots/18a_dendrogram_WT.png", width = 1200, height = 600, res = 100)
par(mar = c(2, 5, 3, 2))
plotDendroAndColors(
  net_wt$dendrograms[[1]],
  colors_wt[net_wt$blockGenes[[1]]],
  "WT Modules",
  dendroLabels = FALSE,
  hang = 0.03,
  addGuide = TRUE,
  guideHang = 0.05,
  main = "Gene Dendrogram - Wild-Type Condition"
)
dev.off()

# Plot 18b: AKO Dendrogram
png("plots/18b_dendrogram_AKO.png", width = 1200, height = 600, res = 100)
par(mar = c(2, 5, 3, 2))
plotDendroAndColors(
  net_ako$dendrograms[[1]],
  colors_ako[net_ako$blockGenes[[1]]],
  "AKO Modules",
  dendroLabels = FALSE,
  hang = 0.03,
  addGuide = TRUE,
  guideHang = 0.05,
  main = "Gene Dendrogram - AKO Condition"
)
dev.off()
```

> 

**Network Comparison Context:** Building separate networks for WT and AKO conditions allows us to identify:

- **Preserved modules:** Gene relationships that remain stable across genetic backgrounds

- **Condition-specific modules:** Gene relationships that appear only in one context

- **Rewired modules:** Modules that reorganize in response to the genetic perturbation

This analysis reveals how the AKO mutation affects gene regulatory architecture beyond simple changes in expression levels.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/18_ab_gene_dendrogram.png?resize=1024%2C678&ssl=1)

### Module Preservation Statistics

Assess whether modules identified in the WT network are preserved in the AKO network:

```r
#-----------------------------------------------
# STEP 17: Module preservation analysis
#-----------------------------------------------

# CRITICAL: Check for genes with zero variance in each subset
# This is required before modulePreservation

# Check WT samples
gsg_wt <- goodSamplesGenes(expr_wt, verbose = 0)
cat("WT - Good samples:", sum(gsg_wt$goodSamples), "/", length(gsg_wt$goodSamples), "\n")
# WT - Good samples: 16 / 16
cat("WT - Good genes:", sum(gsg_wt$goodGenes), "/", length(gsg_wt$goodGenes), "\n")
# WT - Good genes: 10847 / 10853 

# Check AKO samples
gsg_ako <- goodSamplesGenes(expr_ako, verbose = 0)
cat("AKO - Good samples:", sum(gsg_ako$goodSamples), "/", length(gsg_ako$goodSamples), "\n")
# AKO - Good samples: 16 / 16 
cat("AKO - Good genes:", sum(gsg_ako$goodGenes), "/", length(gsg_ako$goodGenes), "\n")
# AKO - Good genes: 10853 / 10853 

# Keep only genes that are good in BOTH conditions
genes_to_keep <- gsg_wt$goodGenes & gsg_ako$goodGenes
cat("\nGenes passing QC in both conditions:", sum(genes_to_keep), "/", length(genes_to_keep), "\n")

if (sum(!genes_to_keep) > 0) {
  cat("Removing", sum(!genes_to_keep), "genes with zero variance or missing data\n\n")
  # Genes passing QC in both conditions: 10847 / 10853 

  # Filter expression matrices
  expr_wt_filtered <- expr_wt[, genes_to_keep]
  expr_ako_filtered <- expr_ako[, genes_to_keep]

  # Filter module colors to match
  colors_wt_filtered <- colors_wt[genes_to_keep]
  colors_ako_filtered <- colors_ako[genes_to_keep]

} else {
  expr_wt_filtered <- expr_wt
  expr_ako_filtered <- expr_ako
  colors_wt_filtered <- colors_wt
  colors_ako_filtered <- colors_ako
}

# Verify dimensions match
cat("Filtered WT dimensions:", dim(expr_wt_filtered), "\n")
# Filtered WT dimensions: 16 10847 
cat("Filtered AKO dimensions:", dim(expr_ako_filtered), "\n")
# Filtered AKO dimensions: 16 10847 

# Prepare data for preservation analysis
multiExpr <- list(
  WT = list(data = expr_wt_filtered),
  AKO = list(data = expr_ako_filtered)
)

multiColor <- list(
  WT = colors_wt_filtered
)

# Calculate preservation statistics
# Compare WT modules in AKO network

set.seed(123)  # For reproducibility
preservation <- modulePreservation(
  multiExpr,
  multiColor,
  referenceNetworks = 1,  # WT is reference
  nPermutations = 100,     # Use 200+ for publication-quality results
  verbose = 3
)

# Extract preservation statistics
preservation_stats <- preservation$preservation$Z$ref.WT$inColumnsAlsoPresentIn.AKO

# Add module names
preservation_stats$module <- rownames(preservation_stats)
preservation_stats$color <- rownames(preservation_stats)

# Classify preservation status
# Zsummary > 10: strong preservation
# Zsummary 2-10: moderate preservation
# Zsummary < 2: no preservation
preservation_stats$status <- cut(
  preservation_stats$Zsummary.pres,
  breaks = c(-Inf, 2, 10, Inf),
  labels = c("Not Preserved", "Moderate", "Strong")
)

# Visualize preservation
# Plot 19: Module Preservation
png("plots/19_module_preservation.png", width = 1200, height = 800, res = 100)
ggplot(preservation_stats, 
            aes(x = moduleSize, 
                y = Zsummary.pres,
                color = status,
                label = color)) +
  geom_point(size = 4, alpha = 0.7) +
  geom_hline(yintercept = c(2, 10), 
             linetype = "dashed", 
             color = "gray50",
             linewidth = 0.5) +
  geom_text_repel(size = 3, max.overlaps = 20, 
                  segment.color = "gray70",
                  box.padding = 0.5) +
  scale_color_manual(values = c("Not Preserved" = "#d62728",
                                "Moderate" = "#ff7f0e",
                                "Strong" = "#2ca02c")) +
  theme_minimal(base_size = 12) +
  theme(
    legend.position = "bottom",
    panel.grid.minor = element_blank(),
    plot.title = element_text(face = "bold", size = 14),
    plot.subtitle = element_text(size = 11, color = "gray40")
  ) +
  labs(
    title = "Module Preservation Analysis",
    subtitle = "WT modules tested in AKO network",
    x = "Module Size (number of genes)",
    y = "Preservation Z-summary",
    color = "Preservation Status",
    caption = "Zsummary > 10: Strong | 2-10: Moderate | <2: Not preserved"
  )
dev.off()

# Save preservation results
write.table(preservation_stats,
            file = "module_preservation_results.txt",
            sep = "\t",
            row.names = FALSE,
            quote = FALSE)
```

> 

**Biological Significance of Preservation Analysis:**

- **Strongly preserved modules:** Represent core regulatory programs unaffected by the AKO mutation. These are robust, housekeeping functions.

- **Moderately preserved modules:** Show some reorganization but maintain overall structure. May be indirectly affected by the knockout.

- **Non-preserved modules:** Gene relationships are fundamentally rewired in the AKO background. These are the most interesting for understanding the functional consequences of the knockout.

Non-preserved modules are particularly valuable for identifying:

1. Direct targets of the knocked-out gene

2. Compensatory mechanisms activated in response to the knockout

3. Pathways that depend on the knocked-out gene for proper coordination

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/preservation_stats.png?resize=840%2C228&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/19_module_preservation.png?resize=1024%2C683&ssl=1)

### Comparing Module Eigengenes Across Conditions

Visualize how module expression patterns differ between WT and AKO:

```r
#-----------------------------------------------
# STEP 18: Compare module eigengenes across conditions
#-----------------------------------------------

# We'll compare the T_AKO-associated module between WT and AKO backgrounds
# Using the module assignments from our combined analysis (Step 6)

# Convert numeric module label to color name FIRST
module_numeric <- gsub("ME", "", t_ako_module)
module_color <- labels2colors(as.numeric(module_numeric))
module_to_compare <- paste0("ME", module_color)

# Calculate module eigengenes for WT samples
# Using the same module assignments from the combined analysis
me_wt <- moduleEigengenes(expr_wt, 
                          colors = module_colors[colnames(expr_wt)])$eigengenes

# Calculate module eigengenes for AKO samples  
# Using the same module assignments from the combined analysis
me_ako <- moduleEigengenes(expr_ako, 
                           colors = module_colors[colnames(expr_ako)])$eigengenes

# Create comparison data frame
me_comparison <- data.frame(
  Eigengene = c(me_wt[, module_to_compare], me_ako[, module_to_compare]),
  Background = c(rep("WT", nrow(me_wt)), rep("AKO", nrow(me_ako))),
  Treatment = c(pheno_data[wt_samples, "Treatment"],
                gsub("_AKO", "", pheno_data[ako_samples, "Treatment"]))
)

# Plot 20: Overall comparison (WT vs AKO)
png(paste0("plots/20_ME_comparison_overall_", module_color, ".png"), 
    width = 800, height = 600, res = 100)
ggplot(me_comparison, aes(x = Background, y = Eigengene, fill = Background)) +
  geom_boxplot(outlier.shape = NA, alpha = 0.7, width = 0.5) +
  geom_jitter(width = 0.15, alpha = 0.6, size = 2.5) +
  theme_minimal(base_size = 12) +
  scale_fill_manual(values = c("WT" = "#3498db", "AKO" = "#e74c3c")) +
  labs(
    title = paste("Module Eigengene Comparison:", module_color, "Module"),
    subtitle = "Overall expression pattern between backgrounds",
    y = "Module Eigengene Value",
    x = "Genetic Background"
  ) +
  theme(
    legend.position = "none",
    plot.title = element_text(face = "bold", size = 14),
    plot.subtitle = element_text(size = 10, color = "gray40")
  )
dev.off()

# Plot 21: Detailed comparison by treatment
png(paste0("plots/21_ME_comparison_by_treatment_", module_color, ".png"), 
    width = 1000, height = 600, res = 100)
ggplot(me_comparison, aes(x = Treatment, y = Eigengene, fill = Background)) +
  geom_boxplot(position = position_dodge(width = 0.8), alpha = 0.7) +
  geom_point(position = position_dodge(width = 0.8), alpha = 0.6, size = 2) +
  theme_minimal(base_size = 12) +
  scale_fill_manual(values = c("WT" = "#3498db", "AKO" = "#e74c3c")) +
  labs(
    title = paste("Module Eigengene by Treatment:", module_color, "Module"),
    subtitle = "Expression across treatments and backgrounds",
    y = "Module Eigengene Value",
    x = "Treatment",
    fill = "Background"
  ) +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    plot.title = element_text(face = "bold", size = 14),
    plot.subtitle = element_text(size = 10, color = "gray40"),
    legend.position = "bottom"
  )
dev.off()
```

> 

**Interpretation Guide:**

- **Significant difference between WT and AKO:** The module’s overall expression level is affected by the knockout

- **Different treatment responses:** The module responds differently to treatments depending on genetic background (interaction effect)

- **Preserved treatment pattern:** If treatment effects are similar in both backgrounds, the knockout doesn’t affect this module’s treatment response

This comparison reveals whether the module you identified is:

1. Directly regulated by the knocked-out gene (large baseline differences)

2. Required for treatment response (treatment effects differ between backgrounds)

3. Independent of the knockout (similar patterns in both backgrounds)

**me_comparison:**

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/me_comparison.png?resize=542%2C652&ssl=1)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/11/20_21_eigengene_comparison-e1764196570219-1024x350.png?resize=1024%2C350&ssl=1)

## Interpreting WGCNA Results: From Data to Discovery

Now that we’ve completed the analysis, let’s discuss how to interpret the results and extract biological insights.

### Understanding Module Structure

**What do modules represent?**

Modules are groups of genes with correlated expression patterns. This co-expression suggests:

1. **Shared regulation:** Genes may be controlled by the same transcription factors or signaling pathways

2. **Functional relationships:** Genes may work together in the same biological process or pathway

3. **Physical interactions:** Gene products may form protein complexes

4. **Common response:** Genes may respond similarly to cellular conditions or stimuli

**Module quality indicators:**

- **Size:** Very small modules (<20 genes) may be artifacts; very large modules (>1000 genes) may be too heterogeneous

- **Cohesion:** High average correlation within modules indicates strong co-expression

- **Separation:** Modules should be distinct from each other in eigengene space

- **Reproducibility:** Robust modules appear in independent datasets or subsamples

### Interpreting Module-Trait Associations

**Strong positive correlation (r > 0.5, p < 0.05):**

- Module genes are upregulated in samples with that trait

- For treatment traits (like T_AKO), this suggests genes involved in treatment response mechanisms

- For continuous traits, genes may mediate the phenotypic effect

**Strong negative correlation (r < -0.5, p < 0.05):**

- Module genes are downregulated in samples with that trait

- May represent suppressed pathways or compensatory downregulation

- Could indicate protective factors or feedback inhibition

**No significant correlation:**

- Module is not associated with this particular trait

- May represent housekeeping functions or unrelated processes

- Could be significant for unmeasured traits

**Multiple trait associations:**

- Modules correlated with several related traits may represent core mechanisms

- Modules specific to one trait may reveal unique biological features

### Prioritizing Genes for Follow-Up

Not all genes in a module are equally important. Prioritize based on:

1. **High module membership (MM > 0.7):** Central to module, likely important for module function

2. **High gene significance (GS > 0.5):** Strongly associated with trait of interest

3. **High connectivity:** Hub genes with many connections in the network

4. **Known biology:** Genes with established roles in relevant processes

5. **Druggability:** Genes encoding proteins targetable by small molecules or antibodies

**Red flags for false positives:**

- Genes with high GS but low MM may be outliers, not true module members

- Extremely high connectivity could indicate technical artifacts

- Modules enriched for poorly annotated genes need additional validation

### Validation Strategies

WGCNA generates hypotheses that require experimental validation:

**Computational validation:**

- Test in independent datasets (different cell types, conditions, species)

- Compare with literature and pathway databases

- Check consistency across platforms (microarray vs RNA-seq)

- Validate hub gene predictions with known protein interactions

- Cross-reference with ChIP-seq data for transcription factors

**Experimental validation:**

- RT-qPCR to confirm expression patterns in key samples

- Western blot for protein-level validation

- Perturbation experiments (knockdown/overexpression) for hub genes

- ChIP or CUT&RUN to validate transcription factor binding

- Functional assays to test biological relevance

## Best Practices for WGCNA Analysis

Following these guidelines will help ensure robust and reproducible results.

### Sample Size and Experimental Design

**Minimum requirements:**

- At least 15-20 samples per condition for reliable correlation estimates

- More heterogeneous samples provide better network inference

- Balance between conditions is important for comparative analyses

**Optimal design:**

- 30+ samples with diverse biological variation

- Multiple conditions, time points, or genotypes for comparative analysis

- Biological replicates to distinguish technical from biological variation

- Matched clinical/phenotypic data for trait associations

**Red flags:**

- Very small sample sizes (<15) lead to unreliable correlations

- Homogeneous samples provide limited network information

- Strong batch effects can create artificial modules

- Unbalanced designs complicate condition comparisons

### Data Quality Considerations

**Pre-processing requirements:**

- Remove batch effects before WGCNA (use ComBat or similar)

- Normalize appropriately for your data type

- Filter out very low-expressed genes (they add noise)

- Check for and remove outlier samples

- Use variance-stabilizing transformations for count data (VST or rlog from DESeq2)

**Quality metrics:**

- Scale-free topology fit (R² > 0.8 preferred)

- Mean connectivity not too high (over-connected) or too low (fragmented)

- Reasonable number of modules (typically 10-30 for most datasets)

- Modules should have functional coherence (check GO enrichment)

- Gray module should not be excessively large (>40% of genes suggests poor module detection)

### Parameter Selection Wisdom

**Soft-thresholding power (β):**

- Don’t just use the default; examine the diagnostic plots carefully

- Balance scale-free topology (R² > 0.8) with connectivity

- If no power achieves R² > 0.8, your data may not follow scale-free structure (still usable, note this limitation)

- Typical values: 6-12 for signed networks

**Module detection parameters:**

- Start with conservative parameters (minModuleSize = 30, mergeCutHeight = 0.25)

- Adjust based on your dataset size and biological question

- Smaller modules (minModuleSize = 20) for focused analyses

- Larger modules (40-50) for robust, high-confidence modules

- Lower mergeCutHeight (0.15) keeps modules separate; higher (0.35) merges aggressively

**Network type selection:**

- “Signed” networks are biologically intuitive (genes upregulated together cluster together)

- “Unsigned” may capture regulatory relationships (activator-repressor pairs)

- Start with “signed”; try “unsigned” only if biologically justified

### Common Pitfalls and How to Avoid Them

**Pitfall 1: Treating correlation as causation**

- Co-expression indicates association, not mechanism

- Genes in the same module may not directly interact

- Use other data (ChIP-seq, PPI, perturbation experiments) to infer causality

- Remember: A → B → C creates correlation between A and C even without direct interaction

**Pitfall 2: Over-interpreting small modules or weak correlations**

- Small modules (<20 genes) may be noise; focus on larger, robust modules

- Weak trait correlations (|r| < 0.3) are unlikely to be biologically meaningful

- Multiple testing: with many modules, some associations occur by chance

- Always apply appropriate multiple testing correction

**Pitfall 3: Ignoring batch effects**

- Batch effects create artificial modules unrelated to biology

- Always check for and correct batch effects before WGCNA

- Visualize sample clustering colored by batch to detect batch-driven separation

- Use ComBat, removeBatchEffect, or include batch as covariate in DESeq2

**Pitfall 4: Inadequate data filtering**

- Including all genes creates noise and increases computation

- Filter low-variance genes (bottom 30-50%)

- Remove non-expressed or very lowly expressed genes

- Consider tissue-specific filtering based on biology

**Pitfall 5: Not validating results**

- WGCNA is hypothesis-generating, not hypothesis-testing

- Validate key findings in independent datasets

- Experimentally test hub genes and key modules

- Check literature for biological plausibility

- Cross-reference with other data types (proteomics, ChIP-seq, etc.)

**Pitfall 6: Ignoring the gray module**

- Large gray module (>40% genes) suggests poor module detection or noisy data

- Investigate why genes aren’t clustering (low quality? inappropriate filtering?)

- Gray genes are often housekeeping genes or genes with inconsistent expression

- Don’t completely ignore gray module—may contain biologically interesting outliers

### Computational Considerations

**Memory and time requirements:**

- WGCNA is memory-intensive; large datasets (>50,000 genes) may require high-RAM machines

- TOM calculation scales with O(n²) where n = number of genes

- Use `blockwiseModules()` for large datasets (automatically splits into blocks)

- Enable multi-threading with `enableWGCNAThreads()` for faster computation

- Consider sampling genes for initial exploration, then run full analysis

**Reproducibility best practices:**

- Set random seeds for any stochastic steps (preservation analysis)

- Document all parameter choices in your analysis script

- Save intermediate results (TOM matrices, module assignments)

- Keep analysis scripts under version control (Git)

- Record software versions and environment details (sessionInfo())

- Save workspace: `save.image("WGCNA_analysis.RData")`

## Troubleshooting Common Issues

### Issue 1: Cannot Find Appropriate Soft-Thresholding Power

**Symptoms:**

- Scale-free topology R² never reaches 0.8

- pickSoftThreshold() returns NA

**Solutions:**

- Try a wider range of powers (1-30)

- Check data distribution; extreme outliers affect correlation

- Your network may not be scale-free (still analyzable, note this limitation)

- Try signed vs. unsigned network types

- Consider more stringent gene filtering

- Check if data transformation was appropriate (use VST/rlog for RNA-seq counts)

**What to do if R² < 0.8:**

- Select the power where R² plateaus (even if <0.8)

- Use a reasonable default (6-10 for signed networks)

- Note this limitation in your methods

- The analysis is still valid; scale-free topology is a desirable but not absolutely required property

### Issue 2: Too Many or Too Few Modules

**Too many modules (>50):**

- Increase minimum module size (try 50 or 100)

- Decrease merge cut height to merge similar modules (try 0.30 or 0.35)

- May indicate high biological complexity (not necessarily a problem)

- Check if soft-thresholding power is too low

**Too few modules (<5):**

- Decrease minimum module size (try 20 or 25)

- Increase merge cut height to keep modules separate (try 0.20 or 0.15)

- May indicate low biological variation in samples

- Check if soft-thresholding power is too high (collapses network)

- Verify adequate sample size and variation

### Issue 3: Large Gray Module

**Problem:** >40% of genes assigned to the “gray” module (unassigned genes)

**Causes and solutions:**

- **Poor data quality:** Re-examine QC, remove low-quality samples

- **Insufficient variation:** Genes don’t vary enough for co-expression analysis

- **Overly stringent parameters:** Decrease minModuleSize, adjust mergeCutHeight

- **Biological reality:** Many genes may not participate in coordinated expression programs

- **Low power:** Try increasing soft-thresholding power

**What to do:**

- Investigate gray genes—are they truly unexpressed or low variance?

- Check if gray genes share functional characteristics (e.g., all housekeeping genes)

- Consider analyzing gray genes separately

- Don’t force all genes into modules; large gray module may be biologically appropriate

### Issue 4: Module Eigengene Calculation Fails

**Error:** “Module eigengene calculation failed for module X”

**Solutions:**

- Check for genes with zero variance in that module (remove them)

- Ensure module has sufficient genes (at least 5-10)

- Verify no missing values in expression data for module genes

- Try re-running module detection with different parameters

- Manually calculate eigengene: `moduleEigengenes(expr_matrix, colors)$eigengenes`

### Issue 5: Memory Errors During TOM Calculation

**Error:** “Cannot allocate vector of size X GB”

**Solutions:**

- Use `blockwiseModules()` with maxBlockSize parameter (default 5000)

- Reduce maxBlockSize (try 3000 or 2000)

- Filter more aggressively to reduce gene count

- Use a machine with more RAM

- Calculate TOM in blocks manually: `net <- blockwiseModules(expr_matrix, power = soft_power, maxBlockSize = 3000, # Process 3000 genes at a time ...)`

### Issue 6: No Significant Module-Trait Associations

**Problem:** No modules correlate with traits of interest

**Possible reasons:**

- Trait may not have strong transcriptional signature

- Sample size too small for reliable correlation (need 15-20+ samples per trait)

- Batch effects obscuring biological signal

- Wrong trait coding (check trait data frame carefully)

- Modules reflect other biological axes not measured

- Treatment effects are subtle and require more sensitive analysis

**What to do:**

- Check module functional enrichment (may still be biologically interesting)

- Try alternative trait definitions or transformations

- Look at individual gene-trait correlations

- Increase sample size if possible

- Consider that not all biological conditions have strong transcriptional signatures

- Try correlation with continuous rather than binary traits

### Issue 7: cor() Function Error

**Error:** “unused arguments (weights.x = NULL, weights.y = NULL, cosine = FALSE)”

**Cause:** WGCNA’s `cor()` function conflict with `stats::cor()`

**Solution:**

```bash
# Before blockwiseModules:
cor &lt;- WGCNA::cor

# Run network construction
net &lt;- blockwiseModules(...)

# After network construction:
cor &lt;- stats::cor
```

This is the bug we addressed earlier in the tutorial. Always use this pattern when calling WGCNA network construction functions.

## Conclusion: From Networks to Knowledge

Gene co-expression network analysis represents a powerful paradigm shift in genomics—from studying genes one at a time to understanding them as part of interconnected systems. WGCNA provides an accessible framework for this systems-level analysis, transforming large gene expression datasets into interpretable modules and highlighting genes that may drive biological processes or treatment responses.

As you apply WGCNA to your own data, focus not just on statistical significance but on biological insight. Ask what your modules tell you about the system you’re studying:

- What regulatory programs are active?

- How do they respond to perturbations?

- Which genes are the key players?

- How does network organization change across conditions?

The answers to these questions—informed by WGCNA but ultimately validated through experiment—are where computational analysis transforms into biological understanding.

## References

1. Langfelder P, Horvath S. WGCNA: an R package for weighted correlation network analysis. BMC Bioinformatics. 2008;9:559.

2. Zhang B, Horvath S. A general framework for weighted gene co-expression network analysis. Stat Appl Genet Mol Biol. 2005;4:Article17.

3. Horvath S, Dong J. Geometric interpretation of gene coexpression network analysis. PLoS Comput Biol. 2008;4(8):e1000117.

4. Langfelder P, Luo R, Oldham MC, Horvath S. Is my network module preserved and reproducible? PLoS Comput Biol. 2011;7(1):e1001057.

5. Yip AM, Horvath S. Gene network interconnectedness and the generalized topological overlap measure. BMC Bioinformatics. 2007;8:22.

6. van Dam S, Võsa U, van der Graaf A, Franke L, de Magalhães JP. Gene co-expression analysis for functional classification and gene-disease predictions. Brief Bioinform. 2018;19(4):575-592.

7. Russo PST, Ferreira GR, Cardozo LE, et al. CEMiTool: a Bioconductor package for performing comprehensive modular co-expression analyses. BMC Bioinformatics. 2018;19(1):56.

8. Song L, Langfelder P, Horvath S. Comparison of co-expression measures: mutual information, correlation, and model based indices. BMC Bioinformatics. 2012;13:328.

9. Kogelman LJ, Cirera S, Zhernakova DV, et al. Identification of co-expression gene networks, regulatory genes and pathways for obesity based on adipose tissue RNA Sequencing in a porcine model. BMC Med Genomics. 2014;7:57.

10. Chen L, Liu R, Liu ZP, Li M, Aihara K. Detecting early-warning signals for sudden deterioration of complex diseases by dynamical network biomarkers. Sci Rep. 2012;2:342.

---

***This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, **please comment and share** **your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.***
