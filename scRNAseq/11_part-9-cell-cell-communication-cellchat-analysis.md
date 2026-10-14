# How to Analyze Cell-Cell Communication in Single-Cell RNA-seq Data — Complete Beginner's Guide Part 9: CellChat Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/)

- [Introduction: Understanding How Cells Communicate](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Introduction_Understanding_How_Cells_Communicate)

  - [What Is Cell-Cell Communication Analysis?](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#What_Is_Cell-Cell_Communication_Analysis)
  - [What Biological Insights Does Cell-Cell Communication Analysis Generate?](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#What_Biological_Insights_Does_Cell-Cell_Communication_Analysis_Generate)
  - [How Does Cell-Cell Communication Analysis Work?](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#How_Does_Cell-Cell_Communication_Analysis_Work)
  - [What Challenges Exist and How Does CellChat Solve Them?](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#What_Challenges_Exist_and_How_Does_CellChat_Solve_Them)
  - [What Can CellChat Do? A Summary of Its Capabilities](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#What_Can_CellChat_Do_A_Summary_of_Its_Capabilities)
  - [When Should You Use Cell-Cell Communication Analysis?](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#When_Should_You_Use_Cell-Cell_Communication_Analysis)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Setting_Up_Your_Analysis_Environment)

  - [Installing CellChat and Dependencies](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Installing_CellChat_and_Dependencies)

- [Example Dataset: PBMC Periodontitis scRNA-seq](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Example_Dataset_PBMC_Periodontitis_scRNA-seq)
- [Part 1: Cell-Cell Communication Analysis of a Single Group (Healthy)](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Part_1_Cell-Cell_Communication_Analysis_of_a_Single_Group_Healthy)

  - [Step 1.1 — Load Libraries and Data](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_11_%E2%80%94_Load_Libraries_and_Data)
  - [Step 1.2 — Subset to the Healthy Group](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_12_%E2%80%94_Subset_to_the_Healthy_Group)
  - [Step 1.3 — Create the CellChat Object Directly from the Seurat Object](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_13_%E2%80%94_Create_the_CellChat_Object_Directly_from_the_Seurat_Object)
  - [Step 1.4 — Set the Ligand-Receptor Database](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_14_%E2%80%94_Set_the_Ligand-Receptor_Database)
  - [Step 1.5 — Preprocess Expression Data](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_15_%E2%80%94_Preprocess_Expression_Data)
  - [Step 1.6 — Infer the Cell-Cell Communication Network](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_16_%E2%80%94_Infer_the_Cell-Cell_Communication_Network)
  - [Step 1.7 — Aggregate at the Signaling Pathway Level](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_17_%E2%80%94_Aggregate_at_the_Signaling_Pathway_Level)
  - [Step 1.8 — Visualize the Communication Network](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_18_%E2%80%94_Visualize_the_Communication_Network)
  - [Step 1.9 — Systems-Level Analysis: Who Are the Dominant Senders and Receivers?](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_19_%E2%80%94_Systems-Level_Analysis_Who_Are_the_Dominant_Senders_and_Receivers)
  - [Step 1.10 — Identify Global Communication Patterns](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_110_%E2%80%94_Identify_Global_Communication_Patterns)
  - [Step 1.11 — Identify Functionally Similar Signaling Pathways](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_111_%E2%80%94_Identify_Functionally_Similar_Signaling_Pathways)
  - [Step 1.12 — Save the Healthy CellChat Object](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_112_%E2%80%94_Save_the_Healthy_CellChat_Object)

- [Part 2: Cell-Cell Communication Analysis of the Post_Treatment Group](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Part_2_Cell-Cell_Communication_Analysis_of_the_Post_Treatment_Group)
- [Part 3: Comparative Analysis — Healthy vs. Post_Treatment](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Part_3_Comparative_Analysis_%E2%80%94_Healthy_vs_Post_Treatment)

  - [Step 3.1 — Load and Merge the Two CellChat Objects](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_31_%E2%80%94_Load_and_Merge_the_Two_CellChat_Objects)
  - [Step 3.2 — Compare Total Interaction Numbers and Strength](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_32_%E2%80%94_Compare_Total_Interaction_Numbers_and_Strength)
  - [Step 3.3 — Visualize Differential Interactions as Circle Plots](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_33_%E2%80%94_Visualize_Differential_Interactions_as_Circle_Plots)
  - [Step 3.4 — Heatmap of Differential Interactions](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_34_%E2%80%94_Heatmap_of_Differential_Interactions)
  - [Step 3.5 — Compare Pathway-Level Communication](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_35_%E2%80%94_Compare_Pathway-Level_Communication)
  - [Step 3.6 — Identify Altered Signaling in Specific Cell Types](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_36_%E2%80%94_Identify_Altered_Signaling_in_Specific_Cell_Types)
  - [Step 3.7 — Bubble Plot of Specific Cell-Type Pair Interactions](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_37_%E2%80%94_Bubble_Plot_of_Specific_Cell-Type_Pair_Interactions)
  - [Step 3.8 — Joint Manifold Learning: Classify Shared vs. Condition-Specific Pathways](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_38_%E2%80%94_Joint_Manifold_Learning_Classify_Shared_vs_Condition-Specific_Pathways)
  - [Step 3.9 — Identify Condition-Specific Signaling Pathways](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Step_39_%E2%80%94_Identify_Condition-Specific_Signaling_Pathways)

- [Best Practices and Common Pitfalls](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Best_Practices_and_Common_Pitfalls)

  - [✅ Best Practices](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#%E2%9C%85_Best_Practices)
  - [⚠️ Common Pitfalls and How to Avoid Them](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#%E2%9A%A0%EF%B8%8F_Common_Pitfalls_and_How_to_Avoid_Them)

- [Conclusion: From Cell Types to Communication Networks](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Conclusion_From_Cell_Types_to_Communication_Networks)

  - [Key Conceptual Takeaways](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Key_Conceptual_Takeaways)

- [References](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/#Like_this)

## Introduction: Understanding How Cells Communicate

### What Is Cell-Cell Communication Analysis?

If you’ve followed Parts 1–8 of this tutorial series, you’ve already accomplished a great deal:

- Processed raw sequencing reads into count matrices ([Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/))

- Applied quality control and filtered low-quality cells ([Part 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-2-quality-control-and-cell-filtering/))

- Integrated samples and identified cell clusters ([Part 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-3-integration-and-clustering/))

- Assigned biological identities to each cluster ([Part 4](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-4-cell-type-identification/))

- Performed differential expression and functional pathway analysis ([Part 5](https://ngs101.com/single-cell-rna-seq-beginners-guide-part-5-cell-type-specific-differential-expression-proportion-testing-and-functional-pathway-analysis/))

- Mastered Seurat and SingleCellExperiment data structures ([Part 6](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/))

- Explored trajectory and pseudotime analysis ([Part 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/))

- Applied Slingshot for lineage inference ([Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/))

You now know *who* the cells are. You know *what genes* they express and *how they change* between conditions. But there’s a dimension of biology we haven’t explored yet: **how cells talk to each other**.

**Cell-cell communication analysis** is the study of molecular signals that cells use to coordinate behavior. Every cell in your body is constantly receiving and sending signals — growth factors, cytokines, hormones, adhesion molecules — through a language made of ligands and receptors. A ligand is a signaling molecule secreted or displayed by one cell; a receptor is the protein on another cell that detects that signal. When a ligand binds its receptor, it triggers a chain of events inside the receiving cell.

In single-cell RNA-seq, we can infer these conversations by asking: *Which cells express a ligand, and which cells express its matching receptor?* If Cell Type A expresses a ligand and Cell Type B expresses the corresponding receptor, we can predict that A is “talking” to B through that specific signaling pathway.

---

### What Biological Insights Does Cell-Cell Communication Analysis Generate?

Cell-cell communication analysis reveals a layer of biology invisible to other analytical methods. Here are the kinds of questions it can answer:

**1. Who is talking to whom?**

In the periodontitis dataset (GSE174609), immune cell crosstalk is central to disease pathology. Are monocytes sending activation signals to T cells? Are B cells receiving help signals from helper T cells? Communication analysis maps out the full network of these conversations.

**2. Which signaling pathways mediate immune coordination?**

Key pathways like MIF (macrophage migration inhibitory factor), CCL (chemokine ligand-receptor), MHC-II (antigen presentation), and CXCL (chemokine receptor) govern immune cell recruitment and activation. CellChat identifies which of these pathways are active and which cell types drive them.

**3. How does treatment reshape cellular crosstalk?**

This is perhaps the most exciting question for our dataset. Does non-surgical periodontal treatment reduce inflammatory signaling? Do certain cell types become less talkative? Does a new communication hub emerge after treatment? Comparing the Healthy and Post_Treatment groups lets us answer all of these questions.

**4. Which cells are the dominant “senders” and “receivers”?**

In any tissue, some cell types are major orchestrators of signaling while others are primarily responders. Identifying these communication hubs can reveal therapeutic targets — blocking a dominant sender’s signal could dampen an entire inflammatory cascade.

---

### How Does Cell-Cell Communication Analysis Work?

At its core, CellChat uses a deceptively elegant approach:

**Step 1: Identify ligand-receptor pairs**

CellChat uses its curated database, **CellChatDB**, which contains thousands of experimentally validated ligand-receptor interactions for human and mouse, including:

- Secreted signaling (autocrine/paracrine)

- Extracellular matrix (ECM)-receptor interactions

- Cell-cell contact interactions (e.g., gap junctions, Notch signaling)

**Step 2: Calculate average gene expression per cell group**

For each cell type, CellChat calculates the average expression of each signaling gene. It uses a statistically robust method called **trimmed mean** (by default), which reduces the influence of outlier cells.

**Step 3: Model communication probability**

For a given ligand-receptor pair, CellChat computes the communication probability between each pair of cell types using a **law of mass action model**: the more a sender expresses a ligand AND the more a receiver expresses the receptor, the higher the communication probability. Co-factors (activating or inhibiting molecules) are also incorporated.

**Step 4: Statistical testing**

CellChat uses permutation testing to determine whether the observed communication probability is higher than expected by chance, filtering out spurious signals.

**Step 5: Aggregate and visualize**

Individual ligand-receptor signals are aggregated at the pathway level and visualized using network diagrams, heatmaps, bubble plots, and chord diagrams.

---

### What Challenges Exist and How Does CellChat Solve Them?

Cell-cell communication analysis is a computationally challenging problem. Here are the major hurdles and CellChat’s solutions:

**Zero inflation****False positives****Multi-subunit receptors****Co-factors****Comparing conditions****Interpretation overload**

| Challenge | Problem | CellChat’s Solution |
| --- | --- | --- |
|  | scRNA-seq has many “dropouts” (false zeroes) | Trimmed mean expression; optional PPI network smoothing |
|  | Many random L-R co-expressions look significant | Permutation-based statistical testing (shuffles cell labels) |
|  | Many receptors are protein complexes with multiple genes | CellChatDB explicitly models multi-subunit L-R complexes |
|  | Signaling often requires third-party molecules | CellChat incorporates activating and inhibitory cofactors |
|  | Different numbers of cells confound comparisons | Systematic comparative framework with normalized weights |
|  | Hundreds of L-R pairs are overwhelming | Pathway-level aggregation + machine-learning pattern recognition |

---

### What Can CellChat Do? A Summary of Its Capabilities

CellChat provides a complete toolkit for cell-cell communication analysis:

**Inference:**

- Infer communication networks from normalized scRNA-seq expression data

- Model both secreted and contact-based signaling

**Visualization:**

- Circle plots: Who talks to whom, and how strongly?

- Hierarchy plots: Directionality of signaling between defined sender/receiver groups

- Chord diagrams: All interactions in a circular layout

- Bubble plots: Which L-R pairs are active across which cell-type pairs?

- Heatmaps: Signaling pathway activity across cell types

**Systems-level analysis:**

- Identify dominant senders, receivers, mediators, and influencers using network centrality scores

- Discover communication patterns: which cell types co-activate which pathways?

- Group functionally similar signaling pathways using manifold learning

**Comparative analysis:**

- Compare total interaction numbers and strength between conditions

- Identify increased or decreased signaling in specific pathways

- Detect cell-type-specific changes in sending/receiving behavior

---

### When Should You Use Cell-Cell Communication Analysis?

**✅ Use CellChat when:**

1. **Your dataset contains multiple interacting cell types** — CellChat needs diversity. A dataset with only T cells cannot reveal T cell–monocyte crosstalk.

2. **You have a biologically meaningful question about intercellular signaling** — e.g., “How do immune cells coordinate inflammation?” or “How does treatment rewire cellular crosstalk?”

3. **You want to generate hypotheses for wet-lab validation** — predicted interactions can guide antibody blocking experiments or co-culture assays.

4. **You have two or more conditions to compare** — comparative CellChat analysis is where the tool truly shines.

**❌ Avoid CellChat (or interpret with caution) when:**

1. **You’re doing it just because others do** — CellChat inferences are probabilistic predictions, not measurements. Without biological follow-up, the analysis generates lists of interactions, not facts.

2. **Your cell types are poorly annotated** — garbage cell labels in = garbage communication networks out. Always complete rigorous cell type annotation (Part 4) before running CellChat.

3. **Your sample size is very small** — with 1–2 samples per condition, statistical comparisons are unreliable.

4. **Your cell types have very few cells** — cell types with fewer than 50 cells produce unstable mean expression estimates. CellChat will still run, but the results for those small populations are noisy.

5. **You lack domain knowledge to interpret the output** — CellChat generates hundreds of interactions. Without biological context, you cannot distinguish meaningful signals from noise.

> 

**A note of caution**: Cell-cell communication analysis is one of the most popular analyses in scRNA-seq today — but also one of the most over-interpreted. Remember: CellChat predicts *potential* communication based on co-expression. It does not prove that cells are actually signaling. Treat CellChat outputs as **hypotheses to validate**, not conclusions.

---

## Setting Up Your Analysis Environment

> 

**Tested environment:** All code in this tutorial was written and tested on a MacBook with an Apple M-series chip (ARM64), running macOS with R 4.4. If you are on Linux or Windows, the code should run identically — the only platform-specific note is that the `presto` package (an optional speed-up for Wilcoxon tests) frequently fails to compile on macOS, which is why this tutorial uses `do.fast = FALSE` throughout.

### Installing CellChat and Dependencies

CellChat v2 is installed from GitHub and requires a few dependencies that must be set up first. Run these commands **in order** in your R console:

```r
# Step 1: Install BiocManager if needed (for Bioconductor packages)
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")

# Step 2: Install Bioconductor dependencies
BiocManager::install(c("BiocNeighbors", "ComplexHeatmap"))

# Step 3: Install NMF (>= 0.23.0) — required for communication pattern analysis
install.packages("NMF")

# Step 4: Install circlize from CRAN (chord diagrams)
# If the CRAN version is too old, use: devtools::install_github("jokergoo/circlize")
install.packages("circlize")

# Step 5: Install ggalluvial — required for river/Sankey plots (netAnalysis_river)
install.packages("ggalluvial")

# Step 6: Install devtools if not already installed
if (!requireNamespace("devtools", quietly = TRUE))
  install.packages("devtools")

# Step 7: Install CellChat from GitHub
devtools::install_github("jinworks/CellChat")

# Step 8: Install reticulate (Python bridge — required by netEmbedding)
if (!requireNamespace("reticulate", quietly = TRUE))
  install.packages("reticulate")
```

---

## Example Dataset: PBMC Periodontitis scRNA-seq

Throughout this tutorial series, we’ve been working with the GSE174609 dataset, which contains peripheral blood mononuclear cells (PBMCs) from three conditions:

- **Healthy**: Disease-free donors

- **Pre_Treatment**: Patients with active periodontitis

- **Post_Treatment**: Patients after non-surgical periodontal therapy

For this tutorial, we’ll use the **downsampled version of this dataset** that was prepared in [Part 7-2 (Slingshot trajectory analysis)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/). This downsampled object contains annotated cell types from Part 4 (stored in the `final_annotation` metadata column) and condition metadata (`condition` column: `"Healthy"` or `"Post_Treatment"`).

> 

**If you haven’t completed the downsampling step yet**, please refer to [Part 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/) or [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/) for the downsampling code. The downsampled Seurat object is saved as `seurat_subset.rds` in your working directory.

The cell types in our annotated dataset include:

- CD4+ T cells

- CD8+ T cells

- NK cells

- B cells

- Classical Monocytes

- Monocytes

- T cells

> 

**Why use the downsampled dataset?** CellChat’s permutation-based statistical testing is computationally intensive. The full dataset contains tens of thousands of cells, which would take hours to run. The downsampled version (a balanced subset of cells) runs in minutes while preserving the key biological signal.

---

## Part 1: Cell-Cell Communication Analysis of a Single Group (Healthy)

In this section, we’ll analyze cell-cell communication within the **Healthy** group only. This establishes the baseline communication network before disease or treatment.

### Step 1.1 — Load Libraries and Data

```r
library(CellChat)
library(Seurat)
library(patchwork)
library(NMF)        # required by selectK and identifyCommunicationPatterns
library(foreach)    # required by NMF parallel backend
library(ggalluvial) # required by netAnalysis_river (river/Sankey plots)
library(reticulate) # required by netEmbedding (Python UMAP via umap-learn)

# Set up the Python environment for netEmbedding().
# virtualenv_create() is safe to call every run — it does nothing if the env exists.
virtualenv_create("r-reticulate")
virtualenv_install("r-reticulate", packages = c("umap-learn"))
use_virtualenv("r-reticulate", required = TRUE)

options(stringsAsFactors = FALSE)

# Force NMF to run sequentially — prevents "none of the packages are loaded" errors
# that occur when NMF spawns parallel worker processes with empty environments
registerDoSEQ()

# Create output directories for all saved plots
dir.create("plots/healthy",    recursive = TRUE, showWarnings = FALSE)
dir.create("plots/comparison", recursive = TRUE, showWarnings = FALSE)

# Load the downsampled annotated Seurat object from Part 7 / Part 7-2
seurat_obj <- readRDS("seurat_subset.rds")

# Confirm the metadata columns available — we need 'final_annotation' and 'condition'
head(seurat_obj@meta.data)
```

---

### Step 1.2 — Subset to the Healthy Group

For single-group analysis, we subset the Seurat object to only Healthy cells.

```r
# Subset to Healthy condition only
seurat_healthy <- subset(seurat_obj, subset = condition == "Healthy")

# Confirm how many cells and which cell types remain
table(seurat_healthy$final_annotation)
```

---

### Step 1.3 — Create the CellChat Object Directly from the Seurat Object

`createCellChat()` accepts a Seurat object directly — there is no need to manually extract the expression matrix or metadata. You simply tell it which metadata column defines the cell groups via `group.by`, and which assay to use.

CellChat v2 also expects a `samples` column in the metadata to track which biological replicate each cell belongs to. Since our Seurat object stores this information in the `sample_id` column, we map it over before creating the CellChat object.

```r
# Map sample_id to the 'samples' column that CellChat v2 expects
# Must be a factor — CellChat will warn and coerce it if not
seurat_healthy$samples <- factor(seurat_healthy$sample_id)

# Set the cell identities to final_annotation before creating CellChat object
Idents(seurat_healthy) <- "final_annotation"

# Create CellChat object directly from the Seurat object
# CellChat automatically extracts the normalized data from the RNA assay
cellchat_healthy <- createCellChat(
  object   = seurat_healthy,
  group.by = "final_annotation",  # metadata column for cell type grouping
  assay    = "RNA"                 # use the RNA assay (log-normalized data)
)
```

> 

**Why add a `samples` column?** CellChat v2 uses sample-level information to distinguish biological replicates within a condition. Without it, CellChat will assume all cells come from a single sample (`sample1`), which suppresses replicate-aware analyses. Mapping `sample_id` → `samples` before object creation resolves the warning and ensures CellChat correctly assigns each cell to its donor.

---

### Step 1.4 — Set the Ligand-Receptor Database

CellChat has a built-in database (`CellChatDB`) for both human and mouse. Since our dataset is **human** PBMCs, we’ll use the human database.

```r
# Use the human ligand-receptor interaction database
CellChatDB <- CellChatDB.human

# Option A: Use all interaction categories (recommended for most analyses)
cellchat_healthy@DB <- CellChatDB

# Option B: Use only secreted signaling (uncomment if you want a focused analysis)
# cellchat_healthy@DB <- subsetDB(CellChatDB, search = "Secreted Signaling")
```

> 

**Tip:** Using the full database (Option A) captures all interaction types including secreted, ECM-receptor, and cell contact signaling. For a focused analysis of paracrine/autocrine signaling only, use Option B. For immune cell communication in PBMCs, the full database is appropriate.

---

### Step 1.5 — Preprocess Expression Data

CellChat identifies genes in the database that are actually expressed in our data and optionally smooths expression using protein-protein interaction (PPI) networks to reduce dropout effects.

```r
# Step 1: Subset to genes in the database — reduces computation time
cellchat_healthy <- subsetData(cellchat_healthy)

# Step 2: Identify over-expressed signaling genes in each cell group
# do.fast = FALSE uses the standard Wilcoxon test — avoids needing the presto package,
# which frequently fails to compile on macOS. Speed difference is negligible at this dataset size.
cellchat_healthy <- identifyOverExpressedGenes(cellchat_healthy, do.fast = FALSE)

# Step 3: Identify over-expressed ligand-receptor interactions
cellchat_healthy <- identifyOverExpressedInteractions(cellchat_healthy)
```

---

### Step 1.6 — Infer the Cell-Cell Communication Network

This is the core computational step. CellChat calculates communication probabilities for every ligand-receptor pair between every pair of cell types.

```r
# Compute communication probability for each ligand-receptor pair
# nboot: number of permutations for statistical testing (100 is sufficient for exploration)
cellchat_healthy <- computeCommunProb(cellchat_healthy, type = "triMean", nboot = 100)

# Filter out communications with fewer than 10 cells in either the sender or receiver group
cellchat_healthy <- filterCommunication(cellchat_healthy, min.cells = 10)
```

> 

**What is `triMean`?** The trimmed mean method (default) is a robust way to calculate average gene expression that is less sensitive to outliers than arithmetic mean. It requires that at least 25% of cells in a group express the gene — this reduces false-positive interactions driven by a handful of highly expressing cells.

---

### Step 1.7 — Aggregate at the Signaling Pathway Level

CellChat groups ligand-receptor pairs into biologically meaningful signaling pathways (e.g., all CCL family interactions are grouped under the “CCL” pathway).

```r
# Infer communications at the pathway level
cellchat_healthy <- computeCommunProbPathway(cellchat_healthy)

# Calculate aggregated interaction counts and total communication strength
cellchat_healthy <- aggregateNet(cellchat_healthy)
```

---

### Step 1.8 — Visualize the Communication Network

#### 1.9.1 — Overview: Number of Interactions and Interaction Strength

```r
# Set group sizes for visualization (number of cells per cell type)
groupSize <- as.numeric(table(cellchat_healthy@idents))

png("plots/healthy/01a_interaction_count.png", width = 700, height = 700, res = 150)
netVisual_circle(
  cellchat_healthy@net$count,
  vertex.weight = groupSize,       # node size proportional to cell count
  weight.scale  = TRUE,
  label.edge    = FALSE,
  title.name    = "Number of Interactions"
)
dev.off()

png("plots/healthy/01b_interaction_strength.png", width = 700, height = 700, res = 150)
netVisual_circle(
  cellchat_healthy@net$weight,
  vertex.weight = groupSize,
  weight.scale  = TRUE,
  label.edge    = FALSE,
  title.name    = "Interaction Strength"
)
dev.off()
```

**How to read these plots:** Each circle represents a cell type. The size of the circle is proportional to the number of cells. Arrows indicate the direction of signaling (from sender to receiver). The thickness of the arrow indicates the number or strength of interactions. Arrows pointing to the same cell indicate autocrine signaling.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/01ab_interaction_number_strength-e1773191182182-1024x341.png?resize=1024%2C341&ssl=1)

---

#### 1.9.2 — Explore a Specific Signaling Pathway

Let’s look at one biologically important pathway — **MHC-II**, which governs antigen presentation between antigen-presenting cells and T cells.

```r
# Check which signaling pathways are active in this dataset
cellchat_healthy@netP$pathways

# Visualize a specific pathway — MHC-II antigen presentation
pathways.show <- "MHC-II"

# Hierarchy plot: directional view of which senders communicate to a pre-defined
png("plots/healthy/02_MHCII_hierarchy.png", width = 1400, height = 900, res = 150)
netVisual_aggregate(
  cellchat_healthy,
  signaling        = pathways.show,
  vertex.receiver  = c(1, 2),       # B cells (index 1) and CD4+ T cells (index 2)
  layout           = "hierarchy"
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/02_MHCII_hierarchy-e1773191259853-1024x393.png?resize=1024%2C393&ssl=1)

**How to read the hierarchy plot**

The hierarchy plot generates **one sub-panel per cell type specified in `vertex.receiver`**, so `c(1, 2)` produces two side-by-side panels. Each panel answers the question: *which cell types are sending MHC-II signals to this particular target?*

Within each panel, there are three columns with a deliberately counterintuitive layout:

```
Source (pink) ──► Target ◄── Source (green)
```

- **CENTER = Target** — the cell type designated as the receiver for that panel. All arrows point toward it.

- **LEFT column (pink “Source”)** — cell types that are the dominant senders to the target in this pathway.

- **RIGHT column (green “Source”)** — the remaining cell types, which may also contribute but less strongly, arranged on the opposite side to reduce visual crossing.

Note that **both left and right columns are labeled “Source”** — this is intentional and often confusing. It means both sides contain senders; the layout is purely for visual clarity, not to imply different biological roles.

Arrow thickness encodes signaling strength — a thick line means strong communication from that sender to the target.

```bash
# Circle plot of the same pathway
png("plots/healthy/03_MHCII_circle.png", width = 900, height = 900, res = 150)
netVisual_aggregate(
  cellchat_healthy,
  signaling = pathways.show,
  layout    = "circle"
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/03_MHCII_circle-e1773191359247.png?resize=697%2C585&ssl=1)

```bash
# Chord diagram — shows all interactions in the pathway
png("plots/healthy/04_MHCII_chord.png", width = 1800, height = 1800, res = 150)
par(mar = c(0, 0, 0, 0))
netVisual_aggregate(
  cellchat_healthy,
  signaling = pathways.show,
  layout    = "chord"
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/04_MHCII_chord-e1773191416357-1024x998.png?resize=1024%2C998&ssl=1)

> 

**Tip:** Replace `"MHC-II"` with any pathway name from `cellchat_healthy@netP$pathways`. Try `"CCL"`, `"MIF"`, `"CXCL"`, or `"CD45"` — all are relevant for PBMC immune communication. To find which cell types receive MHC-II signals, run `levels(cellchat_healthy@idents)` to see the index order — in this dataset, CD4+ T cells and CD8+ T cells are the expected MHC-II receivers.

---

#### 1.9.3 — Contribution of Individual Ligand-Receptor Pairs

Within a pathway, multiple ligand-receptor pairs may contribute. Let’s see which are most important, then visualize the top one:

```r
# Show the relative contribution of each L-R pair to the MHC-II pathway (ranked by communication probability)
contrib_gg <- netAnalysis_contribution(cellchat_healthy, signaling = "MHC-II")
png("plots/healthy/05_MHCII_LR_contribution.png", width = 900, height = 750, res = 150)
print(contrib_gg)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/05_MHCII_LR_contribution.png?resize=900%2C750&ssl=1)

```r
# Visualize a specific L-R pair (e.g., the top contributing pair, ranked by Expression enrichment score)
pairLR <- extractEnrichedLR(cellchat_healthy, signaling = "MHC-II", geneLR.return = FALSE)

# Visualize the top pair
png("plots/healthy/06_MHCII_top_LR_pair.png", width = 900, height = 900, res = 150)
netVisual_individual(
  cellchat_healthy,
  signaling    = "MHC-II",
  pairLR.use   = pairLR[1, ],    # first (strongest) L-R pair
  layout       = "circle"
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/06_MHCII_top_LR_pair-e1773191499967.png?resize=697%2C657&ssl=1)

---

#### 1.9.4 — Bubble Plot: All Active Interactions at Once

A bubble plot summarizes all significant interactions across all cell-type pairs simultaneously, making it easy to spot dominant communication axes.

```bash
# Bubble plot: all significant L-R pairs
# Each bubble = one L-R interaction; x-axis = cell-type pair; y-axis = L-R pair
# Color = communication probability; Size = p-value significance
png("plots/healthy/07_bubble_all_interactions.png", width = 1650, height = 1350, res = 150)
netVisual_bubble(
  cellchat_healthy,
  remove.isolate = FALSE,    # include cell types with no interactions
  angle.x        = 45        # rotate x-axis labels for readability
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/07_bubble_all_interactions.png?resize=1024%2C838&ssl=1)

---

#### 1.9.5 — Gene Expression Distribution of Signaling Genes

Verify that the signaling genes are actually expressed in the expected cell types using a violin plot.

```r
# Extract the ligand and receptor genes for the MHC-II pathway
# subsetCommunication() returns a data frame with 'ligand' and 'receptor' columns
mhcii_lr    <- subsetCommunication(cellchat_healthy, signaling = "MHC-II")
mhcii_genes <- unique(c(mhcii_lr$ligand, mhcii_lr$receptor))

# Height scales with number of genes: 750 px per row of 3
plot_height <- 750 * ceiling(length(mhcii_genes) / 3)

# VlnPlot on seurat_healthy uses properly joined, log-normalised data
# print() is required to render the patchwork object inside png()/dev.off()
png("plots/healthy/08_MHCII_gene_expression.png", width = 1500, height = plot_height, res = 150)
print(
  VlnPlot(
    seurat_healthy,
    features = mhcii_genes,
    group.by = "final_annotation",
    pt.size  = 0,                      # hide individual dots for clarity
    ncol     = 3
  ) & theme(axis.text.x = element_text(angle = 45, hjust = 1))
)
dev.off()
```

> 

**Alternative method: `plotGeneExpression(cellchat_healthy, ...)`** – Potential compatibility issues with Seurat 5

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/08_MHCII_gene_expression-scaled-e1773191566579-1024x512.png?resize=1024%2C512&ssl=1)

---

### Step 1.9 — Systems-Level Analysis: Who Are the Dominant Senders and Receivers?

CellChat can calculate network centrality scores to identify which cell types play the most important roles in the communication network.

```r
# Calculate network centrality scores
# This assigns each cell type a role: sender, receiver, mediator, or influencer
cellchat_healthy <- netAnalysis_computeCentrality(cellchat_healthy, slot.name = "netP")

# Visualize signaling roles as a heatmap
# Rows = signaling pathways; Columns = cell types; Color = centrality score
png("plots/healthy/09_signaling_role_heatmap_outgoing.png", width = 1200, height = 1000, res = 150)
netAnalysis_signalingRole_heatmap(
  cellchat_healthy,
  pattern  = "outgoing",      # "outgoing" = sending cells; "incoming" = receiving cells
  height   = 12
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/09_signaling_role_heatmap_outgoing.png?resize=1024%2C853&ssl=1)

```bash
# 2D scatter plot: dominant senders vs receivers
# Cells in the upper right are both strong senders AND receivers
png("plots/healthy/10_signaling_role_scatter.png", width = 900, height = 750, res = 150)
netAnalysis_signalingRole_scatter(cellchat_healthy)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/10_signaling_role_scatter.png?resize=900%2C750&ssl=1)

**How to interpret:** Cells positioned high on the y-axis (incoming) are major receivers of signals. Cells positioned far right on the x-axis (outgoing) are major senders. A cell in the upper right quadrant is a communication hub. In PBMC data, Classical Monocytes are typically strong outgoing signalers (they send cytokines/chemokines), while T and NK cells are often major responders.

---

### Step 1.10 — Identify Global Communication Patterns

CellChat uses non-negative matrix factorization (NMF) to identify coordinated communication patterns — groups of cell types that co-activate specific sets of pathways. Before running NMF, you need to choose **K**, the number of patterns. `selectK()` helps you pick the right value.

```bash
# Identify outgoing communication patterns (how senders coordinate)

# registerDoSEQ() forces NMF to run sequentially instead of spawning parallel worker to avoid errors
registerDoSEQ()

png("plots/healthy/11_selectK_outgoing.png", width = 900, height = 700, res = 150)
selectK(cellchat_healthy, pattern = "outgoing")
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/11_selectK_outgoing.png?resize=900%2C700&ssl=1)

**How to read the selectK plot**

`selectK()` runs NMF repeatedly for K = 2, 3, 4, … and displays two model quality metrics side by side:

- **Left panel — Cophenetic correlation coefficient:** Measures how *reproducibly* cells are assigned to the same pattern across runs with different random seeds. Ranges from 0 to 1; higher is more stable. This is the primary metric to read.

- **Right panel — Silhouette score:** Measures how well each cell “belongs” to its assigned pattern versus the next best alternative. Also 0–1; higher means tighter, more distinct patterns.

**The decision rule: pick the last K before either curve drops sharply.**

If the two metrics disagree, give more weight to the Silhouette panel — it is more sensitive to whether patterns are actually distinct from one another, whereas Cophenetic can stay artificially high in small datasets. If there is no obvious elbow, default to the smaller K — fewer, interpretable patterns are preferable over many noisy ones.

> 

**What K means biologically:** Each pattern groups cell types (for outgoing) or signaling pathways (visible later in the river/dot plots) that are co-activated together. K=5 outgoing patterns means the Healthy PBMC immune cells organize their sending behavior into five distinct coordination programs. K is not a hypothesis — it is a resolution knob. Choose the K that produces patterns you can actually name and interpret biologically.

```r
# K=5 selected for outgoing
nPatterns <- 5

# registerDoSEQ() must be set again for the same reason
registerDoSEQ()

cellchat_healthy <- identifyCommunicationPatterns(
  cellchat_healthy,
  pattern   = "outgoing",
  k         = nPatterns,
  height    = 18
)
```

```
png("plots/healthy/12_pattern_river_outgoing.png", width = 1200, height = 900, res = 150)
netAnalysis_river(cellchat_healthy, pattern = "outgoing")
dev.off()

png("plots/healthy/13_pattern_dot_outgoing.png", width = 1200, height = 1050, res = 150)
netAnalysis_dot(cellchat_healthy, pattern = "outgoing")
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/pattern_outgoing-e1773192339178-1024x430.png?resize=1024%2C430&ssl=1)

**How to interpret plot 12 — outgoing river (Sankey) diagram:**Each pattern represents a coordinated sending program: the cell types and pathways linked into the same pattern are co-activated together. Read each pattern as a unit — “Pattern 1 is the monocyte innate signaling program, co-activating MHC-II, MIF, and CCL pathways.”

**How to interpret plot 13 — outgoing dot plot:**The dot plot shows the same information as the river diagram in a grid format that is easier to read for individual values. A large dark dot indicates a strong, specific association — this pathway or cell type is a core member of that pattern. Use the dot plot to rank contributions within a pattern and to identify which cell types or pathways are exclusively assigned to one pattern versus spread across several.

**Incoming patterns** show how receiver cell types coordinate their responses — which cell types respond to the same sets of signals together. Run `selectK()` again separately for incoming because receiver coordination often has a different optimal K than sender coordination.

```bash
# Repeat the same workflow for incoming patterns (receiver coordination)
registerDoSEQ()

png("plots/healthy/14_selectK_incoming.png", width = 900, height = 700, res = 150)
selectK(cellchat_healthy, pattern = "incoming")
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/14_selectK_incoming.png?resize=900%2C700&ssl=1)

```r
# Review plots/healthy/14_selectK_incoming.png before setting nPatterns.
nPatterns <- 6

registerDoSEQ()

cellchat_healthy <- identifyCommunicationPatterns(
  cellchat_healthy,
  pattern   = "incoming",
  k         = nPatterns,
  height    = 18
)
```

```
png("plots/healthy/15_pattern_river_incoming.png", width = 1200, height = 900, res = 150)
netAnalysis_river(cellchat_healthy, pattern = "incoming")
dev.off()

png("plots/healthy/16_pattern_dot_incoming.png", width = 1200, height = 1050, res = 150)
netAnalysis_dot(cellchat_healthy, pattern = "incoming")
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/pattern_incoming-e1773192350578-1024x427.png?resize=1024%2C427&ssl=1)

---

### Step 1.11 — Identify Functionally Similar Signaling Pathways

CellChat groups pathways with similar communication patterns using manifold learning:

```r
# Group pathways by functional similarity (similar sender-receiver structures)
cellchat_healthy <- computeNetSimilarity(cellchat_healthy, type = "functional")
cellchat_healthy <- netEmbedding(cellchat_healthy, type = "functional")

options(future.rng.onMisuse = "ignore")
cellchat_healthy <- netClustering(cellchat_healthy, type = "functional")

# Visualize pathway groups in 2D embedding
png("plots/healthy/17_pathway_functional_embedding.png", width = 1050, height = 900, res = 150)
netVisual_embedding(cellchat_healthy, type = "functional", label.size = 3.5)
dev.off()

png("plots/healthy/18_pathway_functional_embedding_zoom.png", width = 1050, height = 900, res = 150)
netVisual_embeddingZoomIn(cellchat_healthy, type = "functional", nCol = 2)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/pathway_functional_embedding-e1773192523225-1024x446.png?resize=1024%2C446&ssl=1)

**How to interpret plot 17 — pathway functional embedding:**Each dot is a signaling pathway, projected into 2D so that pathways with similar sender-receiver communication structures land close together. “Functional similarity” here means the pathways engage the same cell-type pairs in the same directions — not that they share genes or belong to the same biological category. This plot answers the question: *are there groups of pathways that are always co-active because they work through the same cellular wiring?* Those groups are your functional modules.

**How to interpret plot 18 — zoomed embedding (cluster panels):**Plot 18 breaks the full embedding into one panel per cluster, zooming in so individual pathway labels are readable. This is how you actually *name* the clusters you identified in plot 17. Scan each panel and ask: what do these pathways have in common biologically? The zoom plot is most useful when plot 17 is too crowded to read individual labels.

---

### Step 1.12 — Save the Healthy CellChat Object

```bash
# Save for use in comparative analysis (Part 2)
saveRDS(cellchat_healthy, file = "cellchat_Healthy.rds")
```

---

## Part 2: Cell-Cell Communication Analysis of the Post_Treatment Group

Now we repeat the **exact same workflow** on the Post_Treatment group. Running both groups through identical steps is essential so that the comparison in Part 3 is valid.

```r
# Subset to Post_Treatment condition
seurat_post <- subset(seurat_obj, subset = condition == "Post_Treatment")

# Map sample_id to the 'samples' column that CellChat v2 expects
# Must be a factor — CellChat will warn and coerce it if not
seurat_post$samples <- factor(seurat_post$sample_id)

# Set the cell identities before creating the CellChat object
Idents(seurat_post) <- "final_annotation"

# Create CellChat object directly from the Seurat object
cellchat_post <- createCellChat(
  object   = seurat_post,
  group.by = "final_annotation",
  assay    = "RNA"
)

# Set database
cellchat_post@DB <- CellChatDB.human

# Preprocess
cellchat_post <- subsetData(cellchat_post)
cellchat_post <- identifyOverExpressedGenes(cellchat_post, do.fast = FALSE)
cellchat_post <- identifyOverExpressedInteractions(cellchat_post)

# Infer communication
cellchat_post <- computeCommunProb(cellchat_post, type = "triMean", nboot = 100)
cellchat_post <- filterCommunication(cellchat_post, min.cells = 10)
cellchat_post <- computeCommunProbPathway(cellchat_post)
cellchat_post <- aggregateNet(cellchat_post)

# Systems analysis
cellchat_post <- netAnalysis_computeCentrality(cellchat_post, slot.name = "netP")

# Save
saveRDS(cellchat_post, file = "cellchat_Post_Treatment.rds")
```

> 

**Remember:** The code above is identical to Part 1 — we simply swap out the condition label from `"Healthy"` to `"Post_Treatment"` in the `subset()` call. Everything else, including the direct Seurat-to-CellChat conversion and all downstream steps, remains exactly the same. This methodological consistency is essential for a valid comparison in Part 3.

---

## Part 3: Comparative Analysis — Healthy vs. Post_Treatment

This is where the most powerful insights come from. By merging the two CellChat objects and comparing them systematically, we can identify exactly how periodontal treatment reshapes the immune cell communication network.

### Step 3.1 — Load and Merge the Two CellChat Objects

```r
# Load saved objects (if starting fresh)
cellchat_healthy <- readRDS("cellchat_Healthy.rds")
cellchat_post    <- readRDS("cellchat_Post_Treatment.rds")

# Create a named list and merge
object.list <- list(
  Healthy       = cellchat_healthy,
  Post_Treatment = cellchat_post
)

# Merge into a single CellChat object for comparative analysis
cellchat_merged <- mergeCellChat(
  object.list,
  add.names = names(object.list)
)
```

> 

**Important:** Both CellChat objects must have been created with **identical cell type labels**. If your cell type names differ between conditions (e.g., one uses “CD4 T cells” and the other uses “CD4+ T cells”), the merge will fail or produce incorrect results. Double-check with `levels(cellchat_healthy@idents)` and `levels(cellchat_post@idents)`.

> 

**📌 Understanding the comparison direction in Part 3**

When you built `object.list`, you placed Healthy at position 1 and Post*Treatment at position 2. Every differential function in Part 3 uses that order to decide which condition is the _reference* and which is the *query*. The internal calculation is always:

**query (index 2) − reference (index 1) = Post_Treatment − Healthy**

In practice, this means:

- **Red arrows / red cells / positive bars** = that interaction or pathway is *stronger or more frequent* in Post_Treatment than in Healthy — it was gained or upregulated after treatment.

- **Blue arrows / blue cells / negative bars** = that interaction or pathway is *weaker or less frequent* in Post_Treatment than in Healthy — it was lost or downregulated after treatment.

This convention is consistent across `netVisual_diffInteraction()`, `netVisual_heatmap()`, `rankNet(mode = "comparison")`, and `netVisual_bubble()`. The one exception is `compareInteractions()`, which simply plots absolute bar heights for each condition without computing any difference — its output has no directionality and is unaffected by list order.

**If you ever swap the list order** (e.g., `list(Post_Treatment = ..., Healthy = ...)`), every signed plot will flip: red and blue will reverse, and all biological interpretations will be inverted. Always double-check your list order before interpreting any differential result.

---

### Step 3.2 — Compare Total Interaction Numbers and Strength

The first question: does treatment change the overall volume of immune cell communication?

```r
gg1 <- compareInteractions(cellchat_merged, show.legend = FALSE, group = c(1, 2))
gg2 <- compareInteractions(cellchat_merged, show.legend = FALSE, group = c(1, 2), measure = "weight")
png("plots/comparison/01_interaction_count_vs_strength.png", width = 1200, height = 750, res = 150)
print(gg1 + gg2)  # print() required to render patchwork inside png()/dev.off()
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/01_interaction_count_vs_strength.png?resize=1024%2C640&ssl=1)

**How to interpret:** `compareInteractions()` produces a simple grouped bar chart — one bar per condition showing the absolute total number of interactions (left plot) or total communication weight (right plot). There is no directionality or subtraction here; you are reading two independent values side by side. A taller Post_Treatment bar means treatment increased the overall volume of communication; a shorter bar means it decreased. Interaction weight (right plot) captures intensity rather than just presence — two datasets can have the same number of interactions but very different strengths if the communication probabilities differ.

---

### Step 3.3 — Visualize Differential Interactions as Circle Plots

Identify which *specific* cell-type pairs show increased or decreased communication after treatment:

```r
for (i in 1:length(object.list)) {
  fname <- paste0("plots/comparison/02_circle_", tolower(names(object.list)[i]), ".png")
  png(fname, width = 700, height = 700, res = 150)
  netVisual_circle(
    object.list[[i]]@net$count,
    weight.scale = TRUE, label.edge = FALSE,
    title.name   = paste("Number of Interactions -", names(object.list)[i])
  )
  dev.off()
}
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/02_circle_healthy_treatment-e1773193013490-1024x389.png?resize=1024%2C389&ssl=1)

The loop generates two circle plots — one for Healthy and one for Post_Treatment — each showing the **number of interactions between every cell-type pair** in that condition. Each node is a cell type, node size is proportional to cell count, and arrow thickness reflects how many ligand-receptor pairs are active between that sender→receiver pair.

```bash
# Red = increased in Post_Treatment; blue = decreased
png("plots/comparison/03a_diff_interactions_count.png", width = 700, height = 700, res = 150)
netVisual_diffInteraction(cellchat_merged, weight.scale = TRUE)
dev.off()

png("plots/comparison/03b_diff_interactions_strength.png", width = 700, height = 700, res = 150)
netVisual_diffInteraction(cellchat_merged, weight.scale = TRUE, measure = "weight")
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/03_diff_interactions_count_strength-e1773192996670-1024x394.png?resize=1024%2C394&ssl=1)

**How to read:** Red arrows point from senders that *gained* interactions to their receivers after treatment. Blue arrows show *lost* interactions. This immediately shows which communication axes are being turned on or off by treatment.

---

### Step 3.4 — Heatmap of Differential Interactions

For a more systematic view across all cell-type pairs:

```r
# Differential interaction heatmap — interaction count
gg1 <- netVisual_heatmap(cellchat_merged)

# Differential interaction heatmap — interaction strength
gg2 <- netVisual_heatmap(cellchat_merged, measure = "weight")

png("plots/comparison/04_differential_heatmaps.png", width = 1800, height = 750, res = 150)
print(gg1 + gg2)  # print() required to render patchwork inside png()/dev.off()
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/04_differential_heatmaps.png?resize=1024%2C427&ssl=1)

**How to read:** Each row is a sender cell type; each column is a receiver. Red squares indicate more interactions in Post_Treatment; blue indicates fewer. The bar plots on the top and right summarize total incoming and outgoing changes per cell type.

---

### Step 3.5 — Compare Pathway-Level Communication

Which specific signaling pathways change most dramatically between conditions?

```bash
# Information flow comparison — how does total signaling change per pathway?
# Bars show the total communication probability summed across all cell-type pairs
png("plots/comparison/05_rankNet_stacked.png", width = 1050, height = 1200, res = 150)
rankNet(cellchat_merged, mode = "comparison", stacked = TRUE, do.stat = TRUE)
dev.off()
```

```bash
# Alternatively, view only pathways that change between conditions
png("plots/comparison/06_rankNet_sidebyside.png", width = 1050, height = 1200, res = 150)
rankNet(cellchat_merged, mode = "comparison", stacked = FALSE)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/05_06_rankNet-e1773193641238-1024x654.png?resize=1024%2C654&ssl=1)

**Plot 05 (stacked) — relative information flow:** Each pathway has a single bar normalized to 1.0, split proportionally between conditions. The dashed line at 0.5 is the midpoint. This plot answers: *of this pathway’s total activity across both conditions, what fraction belongs to each?* It is good for spotting which condition dominates each pathway, regardless of how active the pathway actually is.

**Plot 06 (side-by-side) — absolute information flow:** Each pathway has two separate bars on a shared absolute axis. This answers: *how large is each condition’s information flow in real terms?* Use it alongside plot 05 — a pathway can look evenly split in plot 05 but have negligible absolute values in plot 06, or appear strongly dominant in one condition in plot 05 while the actual difference in plot 06 is modest. Pathways with a bar in only one condition are condition-specific.

---

### Step 3.6 — Identify Altered Signaling in Specific Cell Types

Now drill down into which cell types change their sending and receiving behavior:

```bash
# Outgoing signaling heatmap: which pathways does each cell type send MORE or LESS after treatment?
png("plots/comparison/07_outgoing_heatmap_Healthy.png", width = 1200, height = 1000, res = 150)
netAnalysis_signalingRole_heatmap(
  object.list[[1]],        # Healthy
  pattern  = "outgoing",
  title    = "Healthy — Outgoing Signaling",
  height   = 12,
  color.heatmap = "GnBu"
)
dev.off()

png("plots/comparison/08_outgoing_heatmap_Post_Treatment.png", width = 1200, height = 1000, res = 150)
netAnalysis_signalingRole_heatmap(
  object.list[[2]],        # Post_Treatment
  pattern  = "outgoing",
  title    = "Post_Treatment — Outgoing Signaling",
  height   = 12,
  color.heatmap = "GnBu"
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/07_08_outgoing_heatmap-e1773193685611-1024x621.png?resize=1024%2C621&ssl=1)

```bash
# Incoming signaling heatmap for both conditions
png("plots/comparison/09_incoming_heatmap_Healthy.png", width = 1200, height = 1000, res = 150)
netAnalysis_signalingRole_heatmap(
  object.list[[1]],
  pattern  = "incoming",
  title    = "Healthy — Incoming Signaling",
  height   = 12,
  color.heatmap = "OrRd"
)
dev.off()

png("plots/comparison/10_incoming_heatmap_Post_Treatment.png", width = 1200, height = 1000, res = 150)
netAnalysis_signalingRole_heatmap(
  object.list[[2]],
  pattern  = "incoming",
  title    = "Post_Treatment — Incoming Signaling",
  height   = 12,
  color.heatmap = "OrRd"
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/09_10_incoming_heatmap-e1773193706907-1024x625.png?resize=1024%2C625&ssl=1)

**How to read the heatmaps (plots 07–10):**

Each heatmap has **signaling pathways as rows** and **cell types as columns**. The color intensity represents the centrality score for that cell type in that pathway — darker = stronger role. The row bar on the right and the column bar on the top sum up each pathway’s total activity and each cell type’s total signaling contribution, respectively.

Compare the Healthy and Post_Treatment heatmaps side by side for the same `pattern` direction:

- **A pathway that is dark in Post_Treatment but pale in Healthy** = that pathway was upregulated after treatment in those specific cell types.

- **A cell type column that is uniformly darker or lighter** = that cell type broadly gained or lost signaling activity across many pathways.

- **A pathway row that disappears entirely** (present in one heatmap, absent in the other) = that pathway is condition-specific and was either newly activated or completely silenced.

The outgoing (GnBu, green-blue) and incoming (OrRd, orange-red) heatmaps answer complementary questions: outgoing tells you *who changed their sending*, incoming tells you *who changed their receiving*. A cell type that gains outgoing signal in one pathway and incoming signal in a different pathway is rewiring its communication role, not just scaling everything up or down uniformly.

---

### Step 3.7 — Bubble Plot of Specific Cell-Type Pair Interactions

To identify which specific ligand-receptor pairs change between conditions for a cell-type pair of interest (e.g., monocytes sending signals to T cells):

```bash
# Compare L-R interactions between Classical Monocytes (sender) and T cells (receiver)
png("plots/comparison/11_bubble_monocyte_to_Tcells.png", width = 1500, height = 1200, res = 150)
netVisual_bubble(
  cellchat_merged,
  sources.use = c("Classical Monocytes"),
  targets.use = c("CD4+ T cells", "CD8+ T cells"),
  comparison  = c(1, 2),
  angle.x     = 45
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/11_bubble_monocyte_to_Tcells.png?resize=1024%2C819&ssl=1)

**How to read the bubble plot (plot 11):**

Each row is a ligand-receptor pair; each column is a sender→receiver cell-type combination. Bubble **color** encodes communication probability (darker = stronger signal) and bubble **size** encodes statistical significance (larger = more significant). 

The most informative comparisons are:

- **A large dark bubble in Post_Treatment with a small pale bubble in Healthy** for the same L-R pair = that interaction was strongly induced by treatment.

- **An L-R pair that is present in only one condition** = a condition-exclusive interaction — highest priority for biological follow-up.

- **Rows where both conditions show similar bubble size and color** = constitutive, treatment-independent interactions that form the stable communication backbone between these cell types.

```bash
# Chord diagram: L-R pairs from Classical Monocytes to CD4+ T cells — per condition
png("plots/comparison/12_chord_monocyte_CD4T_Healthy.png", width = 1800, height = 1800, res = 150)
par(mar = c(0, 0, 0, 0))
netVisual_chord_gene(
  object.list[[1]],
  sources.use  = c("Classical Monocytes"),
  targets.use  = c("CD4+ T cells"),
  lab.cex      = 0.5,
  title.name   = paste0("Signaling from Classical Monocytes -- Healthy")
)
dev.off()

png("plots/comparison/13_chord_monocyte_CD4T_Post_Treatment.png", width = 1800, height = 1800, res = 150)
par(mar = c(0, 0, 0, 0))
netVisual_chord_gene(
  object.list[[2]],
  sources.use  = c("Classical Monocytes"),
  targets.use  = c("CD4+ T cells"),
  lab.cex      = 0.5,
  title.name   = paste0("Signaling from Classical Monocytes -- Post_Treatment")
)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/12_13_chord_monocyte_CD4T-e1773193770971-1024x578.png?resize=1024%2C578&ssl=1)

**How to read the chord diagrams (plots 12–13):**

Each arc on the outer ring is either a ligand (on the sender side) or a receptor (on the receiver side). The ribbons connecting them represent individual L-R interactions — ribbon width is proportional to communication probability. The sender cell type (Classical Monocytes) occupies one arc segment and the receiver (CD4+ T cells) occupies another; all ribbons flow between these two segments.

Compare plots 12 and 13 directly: ribbons that appear only in the Post_Treatment chord (plot 13) are treatment-induced interactions; ribbons that shrink or disappear represent lost interactions. Pay attention to which specific ligands gain or lose thick ribbons — these are the molecular effectors of the communication change, and the most actionable leads for validation experiments.

---

### Step 3.8 — Joint Manifold Learning: Classify Shared vs. Condition-Specific Pathways

CellChat can identify which signaling pathways are conserved across conditions and which are unique to each condition using joint manifold learning.

```r
# Compute functional similarity between pathways across conditions
cellchat_merged <- computeNetSimilarityPairwise(cellchat_merged, type = "functional")
cellchat_merged <- netEmbedding(cellchat_merged, type = "functional")
options(future.rng.onMisuse = "ignore")
cellchat_merged <- netClustering(cellchat_merged, type = "functional")

# Visualize: pathways close together = functionally similar
# Pathways separated by condition = condition-specific
png("plots/comparison/14_pairwise_embedding.png", width = 1200, height = 1050, res = 150)
netVisual_embeddingPairwise(cellchat_merged, type = "functional", label.size = 3.5)
dev.off()

png("plots/comparison/15_pairwise_embedding_zoom.png", width = 1200, height = 1050, res = 150)
netVisual_embeddingPairwiseZoomIn(cellchat_merged, type = "functional", nCol = 2)
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/14_15_pairwise_embedding-e1773193801909-1024x452.png?resize=1024%2C452&ssl=1)

**How to read the embedding plots (plots 14–15):**

Each dot is a signaling pathway, plotted in 2D space where distance encodes functional similarity — pathways that engage the same sender-receiver cell-type combinations land close together. Unlike the single-condition embedding in Step 1.11, this pairwise embedding places pathways from **both conditions in the same coordinate space**, so you can directly compare their positions.

**The key visual pattern to look for is whether a pathway appears as two overlapping dots (one per condition) or as two separated dots:**

- **Two dots close together** (one Healthy, one Post_Treatment) = the pathway operates through the same cell-type wiring in both conditions — a conserved communication axis.

- **Two dots far apart** = the pathway recruits different cell-type combinations in each condition — the communication topology of this pathway changed with treatment, which is biologically significant.

- **A dot with no counterpart nearby** = the pathway is active in only one condition, or the two conditions use it so differently that they land in separate regions of the embedding.

---

### Step 3.9 — Identify Condition-Specific Signaling Pathways

```bash
# Compare pathway activation — which pathways are Healthy-specific vs Post-Treatment-specific?
png("plots/comparison/16_rankSimilarity.png", width = 900, height = 1050, res = 150)
rankSimilarity(cellchat_merged, type = "functional")
dev.off()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/03/16_rankSimilarity-e1773193867501-878x1024.png?resize=878%2C1024&ssl=1)

**How to interpret:** Pathways at the top (highest dissimilarity scores) are the most condition-specific — they behave very differently between Healthy and Post_Treatment. These are your most biologically interesting findings and the best candidates for follow-up validation.

---

## Best Practices and Common Pitfalls

### ✅ Best Practices

**1. Always run the same pipeline on all groups before comparing.**

Using different parameters (e.g., different `nboot`, different database subsets) between groups will produce incomparable results. The comparative analysis assumes methodological consistency.

**2. Verify your cell type annotations before running CellChat.**

CellChat predictions are only as good as your cell type labels. A misannotated cluster will generate spurious interactions. Double-check Part 4 annotations using canonical markers before proceeding.

**3. Use `filterCommunication(min.cells = 10)` to remove noisy signals.**

Cell types with very few cells produce unstable average expression estimates, leading to unreliable interaction predictions. The default filter of 10 cells is a reasonable minimum.

**4. Interpret at the pathway level, not just the L-R pair level.**

Individual ligand-receptor pairs can be noisy. Pathway-level aggregations are more robust and biologically interpretable.

**5. Validate key predictions with orthogonal data.**

Use published literature, protein data (e.g., CyTOF), or functional assays to confirm your top predicted interactions. CellChat is a hypothesis generator, not a proof of mechanism.

**6. Report your parameters transparently.**

When publishing, always include: CellChat version, database version, `nboot` value, `min.cells` threshold, expression calculation method (`triMean`), and the number of cells per cell type per condition.

---

### ⚠️ Common Pitfalls and How to Avoid Them

**Pitfall 1: Mismatched cell type names across conditions**

*Symptom:* Error during `mergeCellChat` or unexpected results in comparison.

*Solution:* Before creating CellChat objects, standardize cell type names across conditions:

```bash
# Check that cell type names match between conditions
levels(cellchat_healthy@idents)
levels(cellchat_post@idents)
# They must be identical — both come from seurat_obj$final_annotation
```

**Pitfall 2: Using the wrong data slot**

*Symptom:* Very low communication probabilities or no significant interactions.

*Solution:* CellChat requires log-normalized data, NOT raw counts or scaled data. When passing a Seurat object directly, `createCellChat()` automatically reads from the `data` layer (log-normalized). This is handled correctly as long as you have run `NormalizeData()` on your object beforehand — which is standard in the Part 2–3 workflow. If you ever need to verify:

```bash
# Confirm the data layer contains log-normalized values (non-integer, non-zero mean)
summary(GetAssayData(seurat_healthy, assay = "RNA", layer = "data")@x)[3]  # median should be > 0
```

**Pitfall 3: Too few cells per group**

*Symptom:* Warning messages about cell groups with too few cells; unstable results.

*Solution:* Filter out cell types with fewer than 10 cells before creating the CellChat object:

```r
# Check cell counts per cell type
table(seurat_healthy$final_annotation)

# Remove cell types with fewer than 10 cells before creating the CellChat object
keep_types <- names(which(table(seurat_healthy$final_annotation) >= 10))
seurat_healthy <- subset(seurat_healthy, subset = final_annotation %in% keep_types)
```

**Pitfall 4: Over-interpreting autocrine signaling**

*Symptom:* Thick arrows pointing from a cell type back to itself (autocrine loops).

*Caution:* CellChat predicts autocrine signaling based on co-expression within a cell type. In bulk deconvolved data or integrated datasets, this can be inflated. Be cautious about interpreting autocrine loops as definitive biology without experimental validation.

**Pitfall 5: Ignoring the direction of communication**

*Symptom:* Describing interactions without specifying sender and receiver.

*Solution:* Always note the arrow direction. In CellChat visualizations, the arrow points **from sender to receiver**. A pathway being active in a cell type does not tell you whether it’s sending or receiving.

**Pitfall 6: Running CellChat on data with strong batch effects**

*Symptom:* Communication differences between conditions reflect technical variation rather than biology.

*Solution:* Always run batch correction (as in Part 3 with Harmony/RPCA) before creating condition-specific subsets for CellChat. Use the integrated embedding for clustering, but the condition-specific normalized expression for CellChat input.

---

## Conclusion: From Cell Types to Communication Networks

Congratulations — you’ve completed a full CellChat analysis workflow, from single-condition communication inference to systematic comparative analysis between Healthy and Post_Treatment groups.

### Key Conceptual Takeaways

**1. Cell-cell communication analysis adds a new dimension to scRNA-seq interpretation**

Knowing that Cell Type A expresses different genes in two conditions (Parts 4–5) is valuable. But knowing that Cell Type A has also *changed who it’s talking to* — and what it’s saying — is an entirely different level of biological insight.

**2. The value of comparison over single-condition analysis**

Single-condition analysis (Part 1) establishes the baseline communication architecture. Comparative analysis (Part 3) reveals what changes. Both are necessary: without the baseline, you can’t interpret what changed; without the comparison, you can’t answer biologically meaningful questions.

**3. CellChat inferences are probabilistic, not definitive**

Every interaction CellChat predicts is a statistical inference based on co-expression patterns. The database interactions are literature-supported, but actual protein levels, post-translational modifications, and spatial context are not captured. Always validate your top findings.

**4. Cell identity quality determines communication quality**

The entire analysis rests on the accuracy of your cell type annotations from Part 4. Spend time getting those labels right — it pays dividends in every downstream analysis, including CellChat.

**5. Communication analysis complements, not replaces, differential expression**

Part 5 told you what changed within each cell type. CellChat tells you how those changes ripple through the tissue ecosystem via signaling. Together, they provide a complete picture of the biological response to treatment.

---

## References

1. Jin S, Guerrero-Juarez CF, Zhang L, et al. Inference and analysis of cell-cell communication using CellChat. *Nature Communications*. 2021;12(1):1088. doi:10.1038/s41467-021-21246-9 [CellChat v1]

2. Jin S, Plikus MV, Nie Q. CellChat for systematic analysis of cell-cell communication from single-cell transcriptomics. *Nature Protocols*. 2025;20(1):180-219. doi:10.1038/s41596-024-01045-4 [CellChat v2 — cite this when using CellChat ≥ v1.5]

3. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. *Journal of Translational Medicine*. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source — GSE174609]

4. Hao Y, Stuart T, Kowalski MH, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nature Biotechnology*. 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

5. Armingol E, Baghdassarian HM, Lewis NE. The diversification of methods for studying cell-cell interactions and communication. *Nature Reviews Genetics*. 2024;25:381-400. doi:10.1038/s41576-023-00685-8 [Review of CCC methods]

6. Efremova M, Vento-Tormo M, Teichmann SA, Vento-Tormo R. CellPhoneDB: inferring cell-cell communication from combined expression of multi-subunit ligand-receptor complexes. *Nature Protocols*. 2020;15(4):1484-1506. doi:10.1038/s41596-020-0292-x [Alternative method for comparison]

7. Dimitrov D, Türei D, Garrido-Rodriguez M, et al. Comparison of methods and resources for cell-cell communication inference from single-cell RNA-seq data. *Nature Communications*. 2022;13(1):3224. doi:10.1038/s41467-022-30755-0 [Benchmark of CCC methods]

8. Street K, Risso D, Fletcher RB, et al. Slingshot: cell lineage and pseudotime inference for single-cell transcriptomics. *BMC Genomics*. 2018;19(1):477. doi:10.1186/s12864-018-4772-0 [Part 7-2 reference]

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
