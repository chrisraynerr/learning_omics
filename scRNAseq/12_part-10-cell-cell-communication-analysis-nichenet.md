# How to Analyze Single-Cell RNA-seq Data – Complete Beginner's Guide Part 10: Cell-Cell Communication Analysis Using NicheNet

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/)

- [Introduction: Taking Cell-Cell Communication Analysis to the Next Level](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Introduction_Taking_Cell-Cell_Communication_Analysis_to_the_Next_Level)

  - [Picking Up Where Part 9 Left Off](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Picking_Up_Where_Part_9_Left_Off)
  - [What Is NicheNet?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#What_Is_NicheNet)
  - [NicheNet vs. CellChat: When to Use Which Tool](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#NicheNet_vs_CellChat_When_to_Use_Which_Tool)
  - [What You Will Learn in This Tutorial](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#What_You_Will_Learn_in_This_Tutorial)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Setting_Up_Your_Analysis_Environment)

  - [Installing NicheNet and Required Packages](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Installing_NicheNet_and_Required_Packages)
  - [Downloading the NicheNet Prior Model Files](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Downloading_the_NicheNet_Prior_Model_Files)

- [Example Dataset: PBMC Periodontitis scRNA-seq](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Example_Dataset_PBMC_Periodontitis_scRNA-seq)

  - [Our Biological Question](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Our_Biological_Question)

- [Cell-Cell Communication Analysis Using NicheNet](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Cell-Cell_Communication_Analysis_Using_NicheNet)

  - [Step 1: Load Libraries, Prior Models, and Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_1_Load_Libraries_Prior_Models_and_Data)
  - [Step 2: Define Receiver and Sender Cell Types](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_2_Define_Receiver_and_Sender_Cell_Types)
  - [Step 3: Determine Expressed Genes in Receiver and Sender Cells](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_3_Determine_Expressed_Genes_in_Receiver_and_Sender_Cells)
  - [Step 4: Define the Gene Set of Interest](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_4_Define_the_Gene_Set_of_Interest)
  - [Step 5: Define Potential Ligands](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_5_Define_Potential_Ligands)
  - [Step 6: Run Ligand Activity Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_6_Run_Ligand_Activity_Analysis)
  - [Step 7: Select Top Ligands for Downstream Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_7_Select_Top_Ligands_for_Downstream_Analysis)
  - [Step 8: Visualize Ligand Activity Scores](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_8_Visualize_Ligand_Activity_Scores)
  - [Step 9: Predict Target Genes of Top Ligands](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_9_Predict_Target_Genes_of_Top_Ligands)
  - [Step 10: Infer Receptors of Top-Ranked Ligands](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_10_Infer_Receptors_of_Top-Ranked_Ligands)
  - [Step 11: Identify Which Sender Cell Types Express Each Top Ligand](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_11_Identify_Which_Sender_Cell_Types_Express_Each_Top_Ligand)
  - [Step 12 (Optional): Extended Prioritization — Incorporating Expression and DE Information](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_12_Optional_Extended_Prioritization_%E2%80%94_Incorporating_Expression_and_DE_Information)
  - [Step 13: Infer Signaling Paths from Top Ligands to Target Genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_13_Infer_Signaling_Paths_from_Top_Ligands_to_Target_Genes)
  - [Step 14: Assign Top Ligands to Specific Sender Cell Types](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_14_Assign_Top_Ligands_to_Specific_Sender_Cell_Types)
  - [Step 15: Circos Plot Visualization — Which Sender Cell Types Influence Which Target Genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Step_15_Circos_Plot_Visualization_%E2%80%94_Which_Sender_Cell_Types_Influence_Which_Target_Genes)

- [Putting It All Together: Interpreting Your NicheNet Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Putting_It_All_Together_Interpreting_Your_NicheNet_Results)

  - [The NicheNet Interpretation Framework](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#The_NicheNet_Interpretation_Framework)
  - [Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Common_Pitfalls_and_How_to_Avoid_Them)

- [Best Practices for NicheNet Analysis](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Best_Practices_for_NicheNet_Analysis)

  - [Choosing the Right Receiver and Gene Set](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Choosing_the_Right_Receiver_and_Gene_Set)
  - [Integrating with CellChat Results](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Integrating_with_CellChat_Results)
  - [Experimental Validation Strategies](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Experimental_Validation_Strategies)

- [Conclusion: From Communication Networks to Mechanistic Hypotheses](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Conclusion_From_Communication_Networks_to_Mechanistic_Hypotheses)

  - [How NicheNet and CellChat Complement Each Other](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#How_NicheNet_and_CellChat_Complement_Each_Other)

- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-10-cell-cell-communication-analysis-using-nichenet/#Like_this)

## Introduction: Taking Cell-Cell Communication Analysis to the Next Level

### Picking Up Where Part 9 Left Off

In [Part 9 of this series](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/), we used **CellChat** to map the full landscape of immune cell communication in our periodontitis dataset. CellChat answered a sweeping question: *Who is talking to whom, through which signaling pathways, and how does treatment reshape these conversations?* We generated circle plots, chord diagrams, and pathway heatmaps that gave us a bird’s-eye view of the immune communication network.

But now we want to zoom in. Suppose you are interested in understanding what drives the transcriptional state of CD4+ T cells — key orchestrators of adaptive immunity whose ligand inputs from other immune cells are poorly understood. You have a list of genes expressed in CD4+ T cells after treatment. The biological question now becomes:

> 

**Which ligands — secreted by other immune cells — are most likely responsible for inducing this gene expression change in CD4+ T cells?**

This is precisely the question that **NicheNet** was designed to answer.

---

### What Is NicheNet?

**NicheNet** (Niche Network) is a computational framework that predicts *which ligands from sender cells* are most likely to regulate *target gene expression in receiver cells*. Unlike tools that simply score co-expression of ligand–receptor pairs, NicheNet traces the signal all the way from the ligand at the cell surface through intracellular signaling cascades to the transcription factors that ultimately alter gene expression.

NicheNet integrates three types of prior knowledge into a unified model:

1. **Ligand–receptor interactions**: Which ligands bind which receptors? (from curated databases like KEGG, Reactome, OmniPath)

2. **Intracellular signaling networks**: What happens downstream of receptor activation? (protein–protein interactions, phosphorylation cascades)

3. **Gene regulatory networks**: Which transcription factors regulate which target genes?

By combining these layers, NicheNet can answer: *“If ligand X from Cell Type A binds receptor Y on Cell Type B, can it plausibly induce the observed gene expression changes in Cell Type B?”*

NicheNet quantifies this plausibility for each candidate ligand and ranks them by their **ligand activity score** — specifically, the area under the precision–recall curve (AUPR) between the ligand’s predicted downstream targets and the observed gene set of interest.

---

### NicheNet vs. CellChat: When to Use Which Tool

Both NicheNet and CellChat analyze cell-cell communication from scRNA-seq data, but they approach the problem from fundamentally different angles. Understanding these differences will help you choose the right tool — or use both together.

**Core question****Starting point****Key output****Prior knowledge****Gene set required?****Condition comparison****Strongest use case**

| Feature | NicheNet | CellChat |
| --- | --- | --- |
|  | Which upstream ligands drive a gene program in receiver cells? | Which cell types communicate, through which pathways, and how strongly? |
|  | A defined gene set of interest in the receiver (e.g., DE genes) | Cell type labels + expression data |
|  | Ranked ligands by activity score; ligand–target predictions; signaling paths | Communication probability networks; sender/receiver centrality scores; signaling patterns |
|  | Ligand–receptor + signaling + GRN (deep mechanistic model) | Curated ligand–receptor database (CellChatDB) |
|  | Yes — must provide a meaningful receiver gene program | No |
|  | Compare DE gene sets between conditions | Directly compare communication networks between conditions |
|  | Upstream regulatory hypotheses; mechanistic insight | Global communication atlas; condition-specific network changes |

**In practice, CellChat and NicheNet are complementary.** A common workflow is:

1. Use **CellChat** ([Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/)) for a global view of who communicates with whom.

2. Use **NicheNet** (this tutorial) to ask mechanistic questions: *“For the receiver cell type I care about, which specific ligands from sender cells explain its transcriptional response?”*

---

### What You Will Learn in This Tutorial

By the end of this tutorial, you will be able to:

- Set up the NicheNet R environment and load prebuilt prior models

- Define receiver and sender cell populations from a Seurat object

- Generate a gene set of interest from differential expression results

- Run NicheNet ligand activity analysis and rank candidate ligands

- Predict which target genes each top ligand regulates

- Identify the receptors through which top ligands act

- Visualize results using dot plots, heatmaps, and circos plots

- Interpret NicheNet outputs in a biologically meaningful way

---

## Setting Up Your Analysis Environment

### Installing NicheNet and Required Packages

NicheNet is installed from GitHub. It requires several CRAN and Bioconductor dependencies. Run the following in your R console, in order:

```r
# Install devtools if not already installed
if (!requireNamespace("devtools", quietly = TRUE))
  install.packages("devtools")

# Install BiocManager if needed
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

# Install Bioconductor dependencies
BiocManager::install("ComplexHeatmap")

# Install CRAN packages needed alongside nichenetr
install.packages(c("tidyverse", "circlize", "ggrepel", "igraph", "ggraph"))

# Install nichenetr from GitHub (use the saeyslab fork — the actively maintained version)
devtools::install_github("saeyslab/nichenetr")
```

---

### Downloading the NicheNet Prior Model Files

NicheNet’s predictive power comes from three prebuilt networks that encode prior biological knowledge. These files are hosted on Zenodo and must be downloaded before analysis. Because our dataset (GSE174609) uses **human** gene symbols, we download the human versions:

`lr_network_human_21122021.rds``ligand_target_matrix_nsga2r_final.rds``weighted_networks_nsga2r_final.rds`

| File | Description |
| --- | --- |
|  | Curated ligand–receptor interaction network |
|  | Ligand–target regulatory potential matrix |
|  | Weighted signaling + GRN networks for path inference |

You can download them directly in R (they will be re-downloaded each session) or, preferably, save them locally once:

```bash
# Recommended: download once and save locally to avoid repeated downloads
options(timeout = 600)  # increase timeout for large files

download.file(
  url = "https://zenodo.org/record/7074291/files/lr_network_human_21122021.rds",
  destfile = "lr_network_human.rds"
)
download.file(
  url = "https://zenodo.org/record/7074291/files/ligand_target_matrix_nsga2r_final.rds",
  destfile = "ligand_target_matrix_human.rds"
)
download.file(
  url = "https://zenodo.org/record/7074291/files/weighted_networks_nsga2r_final.rds",
  destfile = "weighted_networks_human.rds"
)
# Required for Step 14 (signaling path inference) only
download.file(
  url = "https://zenodo.org/record/7074291/files/ligand_tf_matrix_nsga2r_final.rds",
  destfile = "ligand_tf_matrix_human.rds"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_Prior_Models.png?resize=1024%2C678&ssl=1)

**What do the weights in these files mean?** Each file uses a different weight definition — understanding them helps you interpret NicheNet outputs correctly:

`ligand_target_matrix`**Regulatory potential score**`weighted_networks$lr_sig`**Interaction confidence**`weighted_networks$gr`**Interaction confidence**`lr_sig``ligand_tf_matrix`**Regulatory potential score**`ligand_target_matrix`

| Object | Weight meaning |
| --- | --- |
|  | (0–1) between a ligand and a target gene. Reflects how strongly prior knowledge supports that ligand regulating that gene’s expression, computed by propagating signals through the integrated signaling and gene regulatory networks using Personalized PageRank. Higher = stronger predicted regulatory relationship. |
|  | of each ligand–receptor or protein–protein signaling edge, integrated and normalized across multiple databases (KEGG, Reactome, OmniPath, etc.). Higher = interaction supported by more or higher-quality data sources. |
|  | of each transcription factor → target gene regulatory edge, integrated across ChIP-seq, motif, and co-expression databases. Same scale as . |
|  | between a ligand and a transcription factor — analogous to  but stopping at the TF level. Used in Step 13 to identify which TFs mediate the signaling path from ligand to target gene. |

---

## Example Dataset: PBMC Periodontitis scRNA-seq

Throughout this tutorial series, we have been working with the GSE174609 dataset — peripheral blood mononuclear cells (PBMCs) from three conditions:

- **Healthy**: Disease-free donors

- **Pre_Treatment**: Active periodontitis

- **Post_Treatment**: Post non-surgical periodontal therapy

We use the **same downsampled Seurat object** prepared in [Part 7-2 (Slingshot trajectory analysis)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/). The downsampled object (`seurat_subset.rds`) contains:

- Cell type labels in the `final_annotation` metadata column (from Part 4)

- Condition labels in the `condition` metadata column

- Cell types: CD4+ T cells, CD8+ T cells, NK cells, B cells, Classical Monocytes, Monocytes, T cells

> 

**If you haven’t completed the downsampling step**, please refer to [Part 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/) or [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/) for the downsampling code.

### Our Biological Question

For this tutorial, we will ask:

> 

**Which ligands — expressed by other immune cell types — are most likely responsible for driving gene expression changes in CD4+ T cells after periodontal treatment (Post_Treatment vs. Healthy)?**

We choose CD4+ T cells as the **receiver** because they are central orchestrators of adaptive immunity — coordinating responses from monocytes, B cells, NK cells, and other T cell subsets. CD4+ T cells are an ideal NicheNet receiver because their transcriptional state is heavily shaped by upstream signals from the surrounding immune microenvironment — exactly what NicheNet is designed to decipher. All other cell types (CD8+ T cells, NK cells, B cells, Classical Monocytes, Monocytes, T cells) will serve as **senders** — the potential sources of regulatory ligands.

---

## Cell-Cell Communication Analysis Using NicheNet

The NicheNet workflow has five logical steps:

1. **Load data and prior models** — Set up the Seurat object and NicheNet networks

2. **Define populations and expressed genes** — Which genes are expressed in receiver and sender cells?

3. **Define the gene set of interest** — Which genes changed in the receiver between conditions?

4. **Run ligand activity analysis** — Rank ligands by how well their predicted targets overlap the gene set

5. **Downstream analysis** — Predict target genes, infer receptors, and visualize

Let’s walk through each step.

---

### Step 1: Load Libraries, Prior Models, and Data

```r
library(nichenetr)
library(Seurat)
library(tidyverse)
library(ComplexHeatmap)
library(circlize)
library(igraph)
library(ggraph)
library(patchwork)

# Create output directories
dir.create("plots/nichenet", recursive = TRUE, showWarnings = FALSE)

# Load NicheNet prior model files (downloaded earlier)
lr_network         <- readRDS("lr_network_human.rds")
ligand_target_matrix <- readRDS("ligand_target_matrix_human.rds")
weighted_networks  <- readRDS("weighted_networks_human.rds")

# Keep only unique ligand-receptor pairs
lr_network <- lr_network %>% distinct(from, to)

# Load the downsampled annotated Seurat object
seurat_obj <- readRDS("seurat_subset.rds")

# Set cell type labels as the active identity for NicheNet
Idents(seurat_obj) <- seurat_obj$final_annotation

# Verify cell types and conditions
table(seurat_obj$final_annotation)
table(seurat_obj$condition)
```

> 

**Important:** NicheNet requires gene symbols to match its prior model. Our dataset uses official HGNC human gene symbols (e.g., `CD4`, `TGFB1`, `IFNG`), which matches the human NicheNet model. If your gene symbols differ (e.g., Ensembl IDs, mouse symbols, or outdated aliases), you will need to convert them first using `nichenetr::alias_to_symbol_seurat()` or a mapping table.

---

### Step 2: Define Receiver and Sender Cell Types

NicheNet is a **receiver-centric** analysis. You first define your receiver population and the biological comparison of interest, then identify the senders as the potential sources of regulatory signals.

```r
# Define the receiver cell type
receiver <- "CD4+ T cells"

# Define senders: all other cell types in the dataset
all_cell_types <- unique(Idents(seurat_obj))
sender_celltypes <- all_cell_types[all_cell_types != receiver]
sender_celltypes  # inspect the list

# Define the condition comparison
condition_oi        <- "Post_Treatment"  # condition of interest
condition_reference <- "Healthy"         # reference condition
```

> 

**Design note:** The receiver can only be a single cell type per NicheNet run. If you want to analyze multiple receivers (e.g., both CD4+ and CD8+ T cells), you run the analysis separately for each. This is by design — the gene set of interest is receiver-specific, so pooling receivers would dilute the signal.

---

### Step 3: Determine Expressed Genes in Receiver and Sender Cells

NicheNet filters the universe of possible ligands and target genes to only those actually expressed in your data. A gene is considered “expressed” if it is detected in at least 10% of cells in that population (a standard threshold for 10x Chromium data).

```r
# Extract the count matrix and cell identity vector once for efficiency.
expr_mat    <- GetAssayData(seurat_obj, assay = "RNA", layer = "counts")
cell_idents <- Idents(seurat_obj)

# Identify expressed genes in the receiver population
# A gene is "expressed" if detected in >=10% of receiver cells
expressed_genes_receiver <- get_expressed_genes(
  celltype_oi    = receiver,
  object         = expr_mat,
  celltype_annot = cell_idents,
  pct            = 0.10
)

# Define background genes: expressed receiver genes present in the NicheNet model
background_expressed_genes <- expressed_genes_receiver %>%
  .[. %in% rownames(ligand_target_matrix)]

# Identify expressed genes in each sender cell type (reuse the same matrix/idents)
list_expressed_genes_sender <- sender_celltypes %>%
  lapply(function(ct) get_expressed_genes(
    celltype_oi    = ct,
    object         = expr_mat,
    celltype_annot = cell_idents,
    pct            = 0.10
  ))

expressed_genes_sender <- list_expressed_genes_sender %>%
  unlist() %>%
  unique()
```

---

### Step 4: Define the Gene Set of Interest

The gene set of interest is the set of genes that are **differentially expressed in the receiver population** between your conditions. These are the genes you are trying to “explain” — NicheNet will ask: *Which ligands have downstream targets that overlap well with this gene set?*

We compute DE genes between Post_Treatment and Healthy within CD4+ T cells only:

```r
# Subset to receiver cell type
seurat_receiver <- subset(seurat_obj, idents = receiver)

# Set condition as identity for DE testing within receiver cells
Idents(seurat_receiver) <- seurat_receiver$condition

# Find DE genes: Post_Treatment vs Healthy in CD4+ T cells
DE_table_receiver <- FindMarkers(
  object   = seurat_receiver,
  ident.1  = condition_oi,
  ident.2  = condition_reference,
  min.pct  = 0.10
)

# Filter to significant, biologically meaningful DE genes
# and restrict to genes present in the NicheNet ligand-target model
geneset_oi <- DE_table_receiver %>%
  filter(p_val_adj <= 0.05 & avg_log2FC >= 0.25) %>%  # upregulated in Post_Treatment
  rownames() %>%
  intersect(rownames(ligand_target_matrix))

length(geneset_oi)  # how many genes passed filtering? 26 in our case
```

> 

**Choosing the right DE direction:** Here we focus on genes *upregulated* in Post_Treatment (positive `avg_log2FC`). These represent genes induced in CD4+ T cells after treatment — the most interesting targets for upstream ligand prediction. If you were instead interested in what drives gene suppression, you would filter for `avg_log2FC <= -0.25`. You can also run both analyses and compare.

> 

**What if your gene set is very small or very large?** A gene set of 50–500 genes typically works well. If fewer than 20 genes pass filtering, consider relaxing thresholds (e.g., `p_val_adj <= 0.10`, `avg_log2FC >= 0.10`). If you get thousands of DE genes, consider tightening thresholds or using a pre-defined gene signature (e.g., a published cytokine response module).

---

### Step 5: Define Potential Ligands

Next, NicheNet identifies the set of candidate ligands — molecules that *could* plausibly signal to the receiver. NicheNet v2 recommends running both approaches:

- **Sender-agnostic**: Any ligand in the NicheNet model whose cognate receptor is expressed by the receiver. This is an unbiased approach that does not require knowing which sender expresses the ligand.

- **Sender-focused**: Ligands that are (1) expressed in one of your defined sender cell types AND (2) have their cognate receptor expressed in the receiver. This is more targeted.

```r
# Identify all receptors expressed by the receiver population
expressed_receptors <- intersect(
  lr_network$to,
  expressed_genes_receiver
)

# Format of lr_network
#  from  to    
#  A2M   MMP2  
#  A2M   MMP9  
#  A2M   LRP1  
#  A2M   KLK3  
#  AANAT MTNR1A
#  AANAT MTNR1B

# --- Sender-agnostic approach ---
# Potential ligands: any ligand in the network with an expressed receptor in receiver.
all_genes_detected <- rownames(seurat_obj)

potential_ligands_agnostic <- lr_network %>%
  filter(to %in% expressed_receptors) %>%
  pull(from) %>%
  unique() %>%
  intersect(all_genes_detected)  # keep only ligands present in the assay

# --- Sender-focused approach ---
expressed_ligands_sender <- intersect(
  lr_network$from,
  expressed_genes_sender
)

# Potential ligands: expressed in sender AND have expressed receptor in receiver
potential_ligands_focused <- lr_network %>%
  filter(from %in% expressed_ligands_sender & to %in% expressed_receptors) %>%
  pull(from) %>%
  unique()
```

---

### Step 6: Run Ligand Activity Analysis

This is the core NicheNet step. For each potential ligand, NicheNet calculates how well the ligand’s predicted target genes (from the prior model) overlap with your gene set of interest. The ranking metric is the **corrected AUPR** (area under the precision–recall curve), which measures how enriched the ligand’s predicted targets are in your gene set relative to background.

```r
# Run ligand activity analysis — sender-agnostic
ligand_activities_agnostic <- predict_ligand_activities(
  geneset                  = geneset_oi,
  background_expressed_genes = background_expressed_genes,
  ligand_target_matrix     = ligand_target_matrix,
  potential_ligands        = potential_ligands_agnostic
)

# Run ligand activity analysis — sender-focused
ligand_activities_focused <- predict_ligand_activities(
  geneset                  = geneset_oi,
  background_expressed_genes = background_expressed_genes,
  ligand_target_matrix     = ligand_target_matrix,
  potential_ligands        = potential_ligands_focused
)

# Sort by corrected AUPR and assign rank using row_number() on the sorted result.
ligand_activities_agnostic <- ligand_activities_agnostic %>%
  arrange(desc(aupr_corrected)) %>%
  mutate(rank = row_number())

ligand_activities_focused <- ligand_activities_focused %>%
  arrange(desc(aupr_corrected)) %>%
  mutate(rank = row_number())
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_ligand_activities-e1773895562440-1024x763.png?resize=1024%2C763&ssl=1)

> 

**Understanding the output table:** Each row is a candidate ligand. Key columns are:

- `aupr_corrected`: The primary ranking metric — AUPR minus background AUPR. Higher = better. This is the corrected version introduced in NicheNet v2.

- `auroc`: Area under the ROC curve — supplementary metric.

- `pearson`: Pearson correlation between predicted targets and gene set — supplementary metric.

- `rank`: Final rank by `aupr_corrected`.

---

### Step 7: Select Top Ligands for Downstream Analysis

After ranking, we select the top ligands for follow-up. A typical cutoff is the top 20–30 ligands by AUPR rank.

```r
# Select top 20 ligands from each approach
top_n <- 20

best_upstream_ligands_agnostic <- ligand_activities_agnostic %>%
  top_n(top_n, aupr_corrected) %>%
  pull(test_ligand) %>%
  unique()

best_upstream_ligands_focused <- ligand_activities_focused %>%
  top_n(top_n, aupr_corrected) %>%
  pull(test_ligand) %>%
  unique()

# Combine (union of both approaches for comprehensive follow-up)
best_upstream_ligands <- union(
  best_upstream_ligands_agnostic,
  best_upstream_ligands_focused
)
```

> 

**Biological tip:** Cross-reference your top ligands against the CellChat results from Part 9. If a ligand appears as top-ranked in NicheNet AND its corresponding signaling pathway was identified by CellChat as active in your dataset, that is a convergent signal from two independent methods — a much more confident finding.

---

### Step 8: Visualize Ligand Activity Scores

Before diving into target gene predictions, it is useful to visualize the activity scores of the top ligands from both approaches side by side. This lets you see which ligands are shared between the two approaches and which are unique to each — shared top ligands are the most robustly supported predictions.

```r
# Helper to build a bar plot for one set of ligand activities
plot_ligand_activity <- function(ligand_activities_df, top_ligands, subtitle) {
  ligand_activities_df %>%
    filter(test_ligand %in% top_ligands) %>%
    mutate(test_ligand = factor(test_ligand, levels = rev(top_ligands))) %>%
    ggplot(aes(x = test_ligand, y = aupr_corrected)) +
    geom_bar(stat = "identity", fill = "steelblue") +
    coord_flip() +
    labs(
      title    = "NicheNet Ligand Activity Scores",
      subtitle = subtitle,
      x        = "Ligand",
      y        = "Corrected AUPR"
    ) +
    theme_classic()
}

p_agnostic <- plot_ligand_activity(
  ligand_activities_agnostic,
  best_upstream_ligands_agnostic,
  "Top 20 ligands — sender-agnostic"
)

p_focused <- plot_ligand_activity(
  ligand_activities_focused,
  best_upstream_ligands_focused,
  "Top 20 ligands — sender-focused"
)

ggsave("plots/nichenet/ligand_activity_barplot.pdf",
       plot = p_agnostic + p_focused, width = 12, height = 6)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_ligand_activity_barplot-e1773895543189-1024x505.png?resize=1024%2C505&ssl=1)

> 

**Interpreting the two plots:** Ligands that appear in both the sender-agnostic and sender-focused top lists are the strongest candidates — they rank highly regardless of whether sender expression is required. Ligands unique to the sender-focused list are expressed in your defined senders and have good target overlap; those unique to the sender-agnostic list may come from cell types not included in your sender set. All unique ligands from both lists are carried forward in `best_upstream_ligands` for downstream analysis.

---

### Step 9: Predict Target Genes of Top Ligands

For each top-ranked ligand, NicheNet predicts which receiver genes it is most likely to regulate. These predictions come directly from the `ligand_target_matrix`, which encodes regulatory potential derived from the prior model.

```r
# Define a regulatory potential cutoff
# get_weighted_ligand_target_links extracts the top target genes
# for each ligand above a given threshold
active_ligand_target_links_df <- best_upstream_ligands %>%
  lapply(get_weighted_ligand_target_links,
         geneset              = geneset_oi,
         ligand_target_matrix = ligand_target_matrix,
         n                    = 250) %>%  # consider top 250 targets per ligand
  bind_rows() %>%
  drop_na()

# Convert to a wide matrix for heatmap visualization.
# cutoff = 1/3 retains only weights above the 33rd percentile;
# prepare_ligand_target_visualization handles this filtering internally.
active_ligand_target_links <- prepare_ligand_target_visualization(
  ligand_target_df     = active_ligand_target_links_df,
  ligand_target_matrix = ligand_target_matrix,
  cutoff               = 1/3
)

# Order ligands by activity rank for consistent heatmap layout
order_ligands <- intersect(best_upstream_ligands, colnames(active_ligand_target_links)) %>%
  rev()
order_targets <- active_ligand_target_links_df$target %>%
  unique() %>%
  intersect(rownames(active_ligand_target_links))

# Generate the ligand-target heatmap
vis_ligand_target <- active_ligand_target_links[order_targets, order_ligands]

p_ligand_target_network <- vis_ligand_target %>%
  make_heatmap_ggplot(
    y_name    = "Target genes in receiver (CD4+ T cells)",
    x_name    = "Prioritized ligands",
    color     = "purple",
    legend_title = "Regulatory\npotential"
  ) +
  theme(axis.text.x = element_text(face = "italic"))

ggsave("plots/nichenet/ligand_target_heatmap.pdf",
       plot = p_ligand_target_network, width = 10, height = 8)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_ligand_target_heatmap-e1773895491346-1024x817.png?resize=1024%2C817&ssl=1)

> 

**Interpreting the heatmap:** Each cell represents the regulatory potential of a ligand (column) for a specific target gene (row) in the receiver population. Darker colors indicate higher predicted regulatory potential. A ligand with many dark cells in the column is a strong candidate upstream regulator. Importantly, these are *predictions* from the prior model — validation in the receiver cells’ expression data (next step) is essential.

---

### Step 10: Infer Receptors of Top-Ranked Ligands

To understand *how* the top ligands signal into the receiver cells, we identify the specific receptors through which they act. This adds mechanistic specificity: rather than just knowing that “TGFB1 may regulate CD4+ T cells,” we can say “TGFB1 acts through TGFBR1/TGFBR2 expressed on CD4+ T cells.”

```r
# For each top ligand, identify receptors expressed in the receiver
lr_network_top <- lr_network %>%
  filter(from %in% best_upstream_ligands & to %in% expressed_receptors) %>%
  distinct(from, to)

# Build ligand-receptor score matrix using positional arguments
ligand_receptor_links_df <- get_weighted_ligand_receptor_links(
  best_upstream_ligands,
  expressed_receptors,
  lr_network,
  weighted_networks$lr_sig
)

# Prepare wide matrix for heatmap; order_hclust clusters both rows and columns
vis_ligand_receptor_network <- prepare_ligand_receptor_visualization(
  ligand_receptor_links_df,
  best_upstream_ligands,
  order_hclust = "both"
)

p_ligand_receptor_network <- t(vis_ligand_receptor_network) %>%
  make_heatmap_ggplot(
    y_name       = "Ligands",
    x_name       = "Receptors expressed in receiver (CD4+ T cells)",
    color        = "mediumvioletred",
    legend_title = "Prior interaction\npotential"
  ) +
  theme(axis.text.x = element_text(face = "italic"))

ggsave("plots/nichenet/ligand_receptor_heatmap.pdf",
       plot = p_ligand_receptor_network, width = 10, height = 7)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_ligand_receptor_heatmap-e1773895532922-1024x722.png?resize=1024%2C722&ssl=1)

---

### Step 11: Identify Which Sender Cell Types Express Each Top Ligand

So far, we have ranked ligands and predicted their targets and receptors — but we haven’t yet asked *which sender cell type* is the dominant source of each top ligand. This is the step that closes the loop and tells us which cell type is most likely sending the signal to CD4+ T cells.

```r
# For each sender cell type, check which top ligands it expresses
# get_lfc_condition computes log fold-change between conditions for each cell type
# Here we compute average expression of each ligand per sender cell type

# Get average expression across all sender cells (combined conditions, for simplicity)
seurat_sender <- subset(seurat_obj,
                        idents = sender_celltypes,
                        subset = condition %in% c(condition_oi, condition_reference))

# Assign sender cell type as identity
Idents(seurat_sender) <- seurat_sender$final_annotation

best_upstream_ligands <- intersect(best_upstream_ligands, rownames(seurat_sender))

# Compute mean expression of top ligands per sender cell type
ligand_expression_matrix <- AverageExpression(
  seurat_sender,
  features  = best_upstream_ligands,
  assay     = "RNA",
  slot      = "data"  # log-normalized values
)$RNA

# Scale for visualization
ligand_expression_scaled <- t(scale(t(ligand_expression_matrix)))

# Heatmap: which sender cell type expresses each top ligand?
p_ligand_sender <- pheatmap::pheatmap(
  mat              = ligand_expression_scaled[best_upstream_ligands, ],
  color            = colorRampPalette(c("white", "firebrick3"))(50),
  cluster_cols     = TRUE,
  cluster_rows     = TRUE,
  fontsize_row     = 9,
  fontsize_col     = 9,
  main             = "Ligand Expression Across Sender Cell Types",
  filename         = "plots/nichenet/ligand_sender_heatmap.pdf",
  width            = 10,
  height           = 8
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_ligand_sender_heatmap-e1773895519293-1024x816.png?resize=1024%2C816&ssl=1)

> 

**Interpretation:** Look for cell type columns where specific top-ranked ligands are highly expressed (dark red). If a ligand is both highly ranked by NicheNet activity AND highly expressed in a specific sender cell type, that sender–ligand combination is your primary hypothesis for a functionally important intercellular signal.

---

### Step 12 (Optional): Extended Prioritization — Incorporating Expression and DE Information

> 

**Seurat 5 compatibility warning:** Step 13 uses `process_table_to_ic()` and `generate_prioritization_tables()`, which have known compatibility issues with Seurat 5 due to column-naming changes in `FindAllMarkers()` output (`avg_log2FC` vs the legacy `avg_logFC`). If you encounter errors in this step, skip it — the core NicheNet results from Steps 6–11 are complete and publication-ready without extended prioritization.

NicheNet v2 introduced an extended prioritization framework that goes beyond ligand activity alone. It scores each ligand–receptor pair on multiple criteria simultaneously:

- Upregulation of the ligand in sender vs. other cell types

- Upregulation of the receptor in receiver vs. other cell types

- Average expression of the ligand in the sender

- Average expression of the receptor in the receiver

- Condition-specificity of the ligand across all cell types

This multi-criteria prioritization is especially useful when your gene set of interest is small (fewer than 100 DE genes) or when ligand activity scores are close together at the top.

The workflow has three stages: (1) compute DE and average expression manually using Seurat functions, (2) format each table separately with `process_table_to_ic()` specifying its `table_type`, and (3) pass the three processed tables to `generate_prioritization_tables()`.

> 

**Why not use `generate_info_tables()`?** `generate_info_tables()` is a convenience wrapper that internally calls `calculate_de()`, which in turn calls `FindAllMarkers()` and then tries to rename its `cluster` output column. This rename fails under Seurat 5 due to a column-naming change — a known compatibility bug in nichenetr. The manual approach below uses `FindAllMarkers()` and `get_exprs_avg()` directly, bypassing the broken wrapper entirely.

```r
# Step 12a: Identify ligand and receptor genes present in lr_network
lr_network_filtered <- lr_network %>%
  filter(from %in% potential_ligands_focused | to %in% expressed_receptors)

ligands_in_lr   <- unique(lr_network_filtered$from)
receptors_in_lr <- unique(lr_network_filtered$to)

# Restrict to genes that actually exist in the Seurat object.
all_genes_seurat  <- rownames(seurat_obj)
lr_features <- intersect(unique(c(ligands_in_lr, receptors_in_lr)), all_genes_seurat)

# Step 12b: Compute DE of all cell types vs all others, restricted to
# ligand/receptor genes only (reduces compute time considerably)
# Run on the condition of interest subset
seurat_oi <- subset(seurat_obj, subset = condition == condition_oi)
Idents(seurat_oi) <- seurat_oi$final_annotation

DE_table <- FindAllMarkers(
  seurat_oi,
  features        = lr_features,
  min.pct         = 0,
  logfc.threshold = 0,
  return.thresh   = 1   # keep all genes regardless of p-value
)

# Step 12c: Compute average expression per cell type in condition of interest
expression_info <- get_exprs_avg(
  seurat_obj,
  celltype_colname  = "final_annotation",
  condition_colname = "condition",
  condition_oi      = condition_oi,
  features          = lr_features
)

# Step 12d: Compute condition-level DE (ligand/receptor upregulation by condition)
Idents(seurat_obj) <- seurat_obj$condition
condition_markers <- FindMarkers(
  seurat_obj,
  ident.1         = condition_oi,
  ident.2         = condition_reference,
  features        = lr_features,
  min.pct         = 0,
  logfc.threshold = 0
) %>%
  rownames_to_column("gene")

# Step 12e: Format each table separately using process_table_to_ic().
# The function is called three times — once per table type:
#   "celltype_DE" : cell-type-level DE (FindAllMarkers output)
#   "expression"  : average expression per cell type
#   "group_DE"    : condition-level DE (FindMarkers output)
processed_DE_table <- process_table_to_ic(
  DE_table,
  table_type   = "celltype_DE",
  lr_network_filtered,
  senders_oi   = sender_celltypes,
  receivers_oi = receiver
)

processed_expr_table <- process_table_to_ic(
  expression_info,
  table_type = "expression",
  lr_network_filtered
)

processed_condition_markers <- process_table_to_ic(
  condition_markers,
  table_type = "group_DE",
  lr_network_filtered
)

# Step 12f: Generate the multi-criteria prioritization table.
prioritized_tbl <- generate_prioritization_tables(
  processed_expr_table,        # sender_receiver_info (average expression)
  processed_DE_table,          # sender_receiver_de   (cell-type DE)
  ligand_activities_focused,
  processed_condition_markers, # lr_condition_de      (condition-level DE)
  scenario = "case_control"
)

# Save results
write.csv(prioritized_tbl, "plots/nichenet/prioritized_lr_pairs.csv", row.names = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_prioritized_lr_pairs-e1773895452343-1024x89.png?resize=1024%2C89&ssl=1)

> 

**Understanding the output:** `prioritized_tbl` contains one row per sender–ligand–receptor–receiver combination, with 52 columns covering every prioritization criterion. The most important columns for interpretation are: `sender` and `receiver` (the interacting cell types), `ligand` and `receptor` (the specific L-R pair), `lfc_ligand` and `lfc_receptor` (log fold-change of the ligand in the sender and receptor in the receiver between conditions), `p_adj_ligand` and `p_adj_receptor` (adjusted p-values for those DE tests), `avg_ligand` and `avg_receptor` (average expression levels), `pct_expressed_sender` and `pct_expressed_receiver` (fraction of cells expressing the ligand/receptor), `activity_zscore` and `scaled_activity` (NicheNet ligand activity), and finally `prioritization_score` and `prioritization_rank` (the final aggregated multi-criteria score and its rank). L-R pairs with a low `prioritization_rank` (i.e., rank 1, 2, 3…) are your most confident predictions — they score highly across condition-specificity, cell-type-specificity, expression level, and ligand activity simultaneously.

---

### Step 13: Infer Signaling Paths from Top Ligands to Target Genes

One of NicheNet’s most powerful features is the ability to infer *mechanistic signaling paths* — the chain of molecular events from ligand binding all the way to transcription factor activation and target gene regulation. This is unique to NicheNet and is not available in CellChat.

```r
# Load the ligand-TF matrix — a prebuilt matrix (ligands x TFs) encoding
ligand_tf_matrix <- readRDS("ligand_tf_matrix_human.rds")

# Choose a top ligand to trace the signaling path
top_ligand <- best_upstream_ligands_focused[1]

# Choose the top predicted target gene for that ligand (from Step 9)
top_target <- active_ligand_target_links_df %>%
  filter(ligand == top_ligand) %>%
  arrange(desc(weight)) %>%
  slice(1) %>%
  pull(target)

# Infer the signaling path from top_ligand to top_target
sig_path <- get_ligand_signaling_path(
  ligand_tf_matrix  = ligand_tf_matrix,
  ligands_all       = top_ligand,
  targets_all       = top_target,
  top_n_regulators  = 4,
  weighted_networks = weighted_networks,
  minmax_scaling    = TRUE  # normalise edge weights for cleaner visualisation
)

# Combine both edge tables and build an igraph for visualisation
sig_path_combined <- bind_rows(sig_path$sig, sig_path$gr)

# Build igraph and visualise with ggraph
sig_graph <- graph_from_data_frame(sig_path_combined, directed = TRUE)

graph_plot <- ggraph(sig_graph, layout = "kk") +
  geom_edge_link(
    aes(edge_width = weight, edge_alpha = weight),
    arrow    = arrow(length = unit(3, "mm"), type = "closed"),
    end_cap  = circle(3, "mm"),
    color    = "grey50"
  ) +
  geom_node_point(size = 5, color = "steelblue") +
  geom_node_text(aes(label = name), repel = TRUE, size = 3) +
  scale_edge_width(range = c(0.5, 2)) +
  theme_void() +
  labs(title = paste("Signaling path:", top_ligand, "->", top_target))

ggsave("plots/nichenet/signaling_path.pdf",
       plot = graph_plot, width = 8, height = 6)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_signaling_path-e1773895436977-1024x756.png?resize=1024%2C756&ssl=1)

> 

**Understanding the signaling path output:** The path goes: **Ligand** → **Receptor** → **Signaling intermediaries** (kinases, adaptor proteins) → **Transcription factor** → **Target gene**. Not every step will be present — the algorithm finds the highest-confidence path through the prior network, so some paths skip directly from receptor to TF if the intermediate signaling is not well-represented in the prior model.

---

### Step 14: Assign Top Ligands to Specific Sender Cell Types

A clear and informative final summary is to explicitly link each top ligand to the sender cell type most likely producing it. This converts the analysis from a list of ranked ligands to an actionable biological narrative.

```r
# Assign each top ligand to its most likely sender cell type
# based on highest average expression among all sender cell types

# Use the average expression matrix computed in Step 12
ligand_sender_assignment <- apply(
  ligand_expression_matrix[best_upstream_ligands, ],
  1,
  function(x) names(which.max(x))
) %>%
  enframe(name = "ligand", value = "best_sender_celltype")

# Merge with ligand activity scores
ligand_summary <- ligand_activities_focused %>%
  filter(test_ligand %in% best_upstream_ligands_focused) %>%
  rename(ligand = test_ligand) %>%
  left_join(ligand_sender_assignment, by = "ligand") %>%
  arrange(desc(aupr_corrected)) %>%
  select(ligand, best_sender_celltype, aupr_corrected, auroc, pearson, rank)

# Display the summary
print(ligand_summary)

# Save to file
write.csv(ligand_summary, "plots/nichenet/ligand_sender_summary.csv", row.names = FALSE)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_ligand_sender_summary-e1773895506724-1024x821.png?resize=1024%2C821&ssl=1)

---

### Step 15: Circos Plot Visualization — Which Sender Cell Types Influence Which Target Genes

The circos plot provides a clean visual summary of the NicheNet results: which sender cell types are predicted to influence which target genes in CD4+ T cells. Each chord connects a sender cell type (coloured sector) to a target gene (grey sector). The width of a chord reflects how many top ligands from that sender have that target gene among their predicted downstream targets.

The ligand-level detail is already captured in the heatmaps from Steps 9 and 11. The circos focuses on the higher-level question beginners care most about: *who is talking to whom?*

```r
# Build sender → target edges:
# For each top ligand, take its top 5 target genes and
# assign the ligand's dominant sender cell type (from Step 15)
circos_data <- active_ligand_target_links_df %>%
  group_by(ligand) %>%
  slice_max(weight, n = 5) %>%
  ungroup() %>%
  left_join(ligand_sender_assignment, by = "ligand") %>%
  filter(!is.na(best_sender_celltype)) %>%
  group_by(best_sender_celltype, target) %>%
  summarise(n = n(), .groups = "drop")   # count ligands linking that sender to that target

# Define colors for sender cell types
sender_colors <- c(
  "CD8+ T cells"        = "#E41A1C",
  "NK cells"            = "#377EB8",
  "B cells"             = "#4DAF4A",
  "Classical Monocytes" = "#FF7F00",
  "Monocytes"           = "#F781BF",
  "T cells"             = "#984EA3"
)

# Build color vector: senders get distinct colors, target genes stay grey
all_nodes <- unique(c(circos_data$best_sender_celltype, circos_data$target))
color_vector <- rep("grey80", length(all_nodes))
names(color_vector) <- all_nodes
for (ct in names(sender_colors)) {
  if (ct %in% names(color_vector))
    color_vector[ct] <- sender_colors[ct]
}

# Draw circos plot
pdf("plots/nichenet/nichenet_circos.pdf", width = 10, height = 10)
circos.clear()
chordDiagram(
  x                 = circos_data,
  grid.col          = color_vector,
  transparency      = 0.4,
  annotationTrack   = "grid",
  preAllocateTracks = 1
)
circos.trackPlotRegion(
  track.index = 1,
  panel.fun   = function(x, y) {
    xlim        <- get.cell.meta.data("xlim")
    ylim        <- get.cell.meta.data("ylim")
    sector_name <- get.cell.meta.data("sector.index")
    circos.text(
      mean(xlim), ylim[1] + 0.1,
      sector_name, facing = "clockwise",
      niceFacing = TRUE, adj = c(0, 0.5), cex = 0.65
    )
  },
  bg.border = NA
)
title(main = "NicheNet: Predicted Sender Influences on CD4+ T Cell Target Genes")
dev.off()
circos.clear()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/nichenetr_tutorial_nichenet_circos.png?resize=1024%2C678&ssl=1)

> 

**Interpreting the circos plot:** Coloured sectors on the left are sender cell types; grey sectors on the right are predicted target genes in CD4+ T cells. A chord between a sender and a target means that at least one top-ranked ligand from that sender is predicted to regulate that target gene. Wider chords indicate more ligands linking that sender to that target — a stronger, more redundant signal. To find out *which specific ligands* mediate a connection of interest, refer back to the ligand-target heatmap in Step 9.

---

## Putting It All Together: Interpreting Your NicheNet Results

After completing the analysis, you now have a multi-layer picture of immune cell communication in the context of periodontal treatment. Here is how to interpret and integrate your findings:

### The NicheNet Interpretation Framework

**Layer 1: Ligand Activity (Step 6–7)**

Your top-ranked ligands are the molecules that most plausibly explain the gene expression changes in CD4+ T cells after treatment. High-activity ligands are not just strongly expressed — they are specifically predicted to regulate the *particular* set of genes that changed in your receiver cells.

**Layer 2: Ligand–Target Matrix (Step 9)**

For each top ligand, you have a list of predicted target genes in the receiver. These target genes are the mechanistic link between the signal (ligand) and the response (gene expression change). Cross-reference these with your DE gene list from [Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/) to see which known DE genes are “explained” by your top ligands.

**Layer 3: Receptor Identification (Step 10)**

Top ligands act through specific receptors expressed on CD4+ T cells. These receptor identities are critical for experimental validation — antibody blocking experiments or receptor knockout studies can test whether a specific ligand–receptor pair truly mediates the observed transcriptional response.

**Layer 4: Signaling Path (Step 13)**

For your top ligand–target pairs, the mechanistic signaling path provides a complete molecular hypothesis: ligand → receptor → signaling cascade → TF → target gene. This is the level of mechanistic detail needed for follow-up functional experiments.

**Layer 5: Sender Attribution (Step 11–14)**

By linking each top ligand to its dominant sender cell type, you can form specific, testable cellular hypotheses. Using the actual results from this tutorial as an example: the signaling path inferred in Step 13 shows that **IL23A** signals through the transcription factors **NFKB1**, **STAT3**, and **STAT4** to regulate **SOCS3** in CD4+ T cells. Once Step 11 attributes IL23A to its dominant sender cell type, the complete hypothesis becomes: *“CD8+ T cells secretes IL23A, which activates NFKB1/STAT3/STAT4 signaling in CD4+ T cells, leading to upregulation of SOCS3 — a negative regulator of cytokine signaling — after periodontal treatment.”* This is a precise, mechanism-level hypothesis that can be tested by blocking IL23A or knocking out its receptor in CD4+ T cells and measuring SOCS3 expression.

---

### Common Pitfalls and How to Avoid Them

**Top ligands are all housekeeping genes****Very few genes in gene set**`p_val_adj <= 0.10`**Top ligands are not expressed in any sender**`expressed_genes_sender`**Signaling path is empty or very short****Results don’t overlap with CellChat****`ligand_target_matrix` has wrong gene symbols**

| Pitfall | Why It Happens | Solution |
| --- | --- | --- |
|  | Gene set of interest is too broad or non-specific | Use more stringent DE filtering; ensure your gene set reflects a specific biological program |
|  | Too few DE genes pass filtering | Relax thresholds (e.g., ); consider combining upregulated and downregulated genes |
|  | Using sender-agnostic approach | Switch to sender-focused approach; check  for coverage |
|  | Sparse coverage of that pathway in prior model | Try a different target gene; focus on well-studied pathways |
|  | Different methodologies and databases | Overlap is expected to be partial; treat convergence as strong evidence |
|  | Using wrong species network | Confirm dataset is human and files are the human Zenodo versions |

---

## Best Practices for NicheNet Analysis

### Choosing the Right Receiver and Gene Set

NicheNet’s results are only as good as the gene set of interest you provide. Follow these principles:

1. **Use biologically motivated gene sets.** DE genes between conditions are the most common and well-validated input. Published gene signatures (e.g., from MSigDB, from [Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/)‘s pathway analysis) are also valid.

2. **Keep the gene set focused.** A set of 50–300 upregulated genes in one specific cell type yields the clearest results. Avoid using all DE genes from all cell types combined.

3. **Run multiple receivers if biologically justified.** If two cell types both show strong transcriptional responses, run NicheNet for each separately. The top ligands may overlap (shared upstream regulation) or diverge (cell-type-specific regulation).

4. **For systematic multi-condition comparison across multiple samples**, consider **MultiNicheNet** — an extension of NicheNet built specifically for multi-sample, multi-condition datasets. MultiNicheNet uses pseudobulk-based DE testing for more statistically rigorous comparisons and is available at [https://github.com/saeyslab/multinichenetr](https://github.com/saeyslab/multinichenetr).

### Integrating with CellChat Results

The most powerful workflow combines CellChat (Part 9) and NicheNet (this tutorial):

1. From CellChat: identify which cell type pairs show the strongest communication changes between conditions (e.g., Monocytes → CD4+ T cells signaling increases post-treatment).

2. Use these dominant sender–receiver pairs to guide NicheNet: set the CellChat-identified sender as the NicheNet sender, and the CellChat-identified receiver as the NicheNet receiver.

3. Results from NicheNet that are also supported by CellChat are your strongest hypotheses.

### Experimental Validation Strategies

NicheNet outputs are computational predictions. To move from prediction to biological knowledge, consider:

- **Blocking antibodies**: Use antibodies against top-ranked ligands (e.g., anti-CXCL10) in co-culture experiments and measure whether target gene expression changes.

- **Cytokine perturbation**: Add recombinant top-ranked ligands to receiver cells in culture and check whether the gene set of interest is induced.

- **Receptor knockout**: CRISPR-delete the predicted receptor in receiver cells and measure whether the ligand still induces the target gene program.

- **Spatial transcriptomics**: Use Visium or similar spatial data to confirm that sender and receiver cells are physically proximate in tissue.

---

## Conclusion: From Communication Networks to Mechanistic Hypotheses

In this tutorial, you have learned to use NicheNet to generate mechanistic, testable hypotheses about upstream ligands that drive gene expression changes in a receiver cell population.

### How NicheNet and CellChat Complement Each Other

Together, CellChat (Part 9) and NicheNet (this tutorial) give you a complete picture of cell-cell communication in your dataset:

- **CellChat** tells you *the map*: a global network showing all communication pathways and how the network topology changes between conditions.

- **NicheNet** tells you *the mechanism*: for a specific receiver cell type and gene program, which upstream ligands are the most plausible drivers, and through which receptors and signaling cascades do they act?

These tools are not competitors — they are complementary lenses on the same biological phenomenon.

---

## References

1. Browaeys R, Saelens W, Saeys Y. NicheNet: modeling intercellular communication by linking ligands to target genes. Nat Methods. 2020;17(2):159-162. doi:10.1038/s41592-019-0667-5 [NicheNet original paper]

2. Browaeys R, Seurinck R, Saeys Y. MultiNicheNet: a flexible framework for differential cell-cell communication analysis from multi-sample multi-condition single-cell transcriptomics data. bioRxiv. 2023. doi:10.1101/2023.06.13.544751 [MultiNicheNet]

3. Jin S, Guerrero-Juarez CF, Zhang L, et al. Inference and analysis of cell-cell communication using CellChat. Nat Commun. 2021;12(1):1088. doi:10.1038/s41467-021-21246-9 [CellChat v1]

4. Jin S, Plikus MV, Bhatt DL, et al. CellChat for systematic analysis of cell-cell communication from single-cell transcriptomics. Nat Protoc. 2025. doi:10.1038/s41596-024-01045-4 [CellChat v2]

5. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. Nat Biotechnol. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

6. Puram SV, Tirosh I, Parikh AS, et al. Single-cell transcriptomic analysis of primary and metastatic tumor ecosystems in head and neck cancer. Cell. 2017;171(7):1611-1624.e24. doi:10.1016/j.cell.2017.10.044 [HNSCC dataset used in NicheNet validation]

7. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. J Transl Med. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [GSE174609 dataset source]

8. Luecken MD, Theis FJ. Current best practices in single-cell RNA-seq analysis: a tutorial. Mol Syst Biol. 2019;15(6):e8746. doi:10.15252/msb.20188746 [Best practices]

9. Türei D, Valdeolivas A, Gul L, et al. Integrated intra- and intercellular signaling knowledge for multicellular omics analysis. Mol Syst Biol. 2021;17(3):e9923. doi:10.15252/msb.20209923 [OmniPath — ligand-receptor database used in NicheNet v2 model]

10. Subramanian A, Tamayo P, Mootha VK, et al. Gene set enrichment analysis: a knowledge-based approach for interpreting genome-wide expression profiles. Proc Natl Acad Sci USA. 2005;102(43):15545-15550. doi:10.1073/pnas.0506580102 [GSEA — gene set evaluation methodology]

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
