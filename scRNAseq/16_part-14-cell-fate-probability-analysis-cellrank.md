# How to Analyze Single-Cell RNA-seq Data — Complete Beginner's Guide Part 14: Cell Fate Probability Analysis with CellRank

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/)

---

*From velocity arrows to fate decisions — discover the probability that each pancreatic progenitor cell will become a specific hormone-producing endocrine cell type*

---

In [Part 13](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/) you used scVelo to attach a directional velocity arrow to every cell — a vector pointing toward where each cell is heading transcriptionally. Velocity arrows tell you the *direction* of motion at this instant. But arrows have a fundamental limitation: they cannot tell you the *destination*.

Consider a progenitor cell in the developing pancreas. Its velocity arrow points downstream toward the endocrine compartment, but the endocrine compartment splits into four distinct mature cell types: alpha cells, beta cells, delta cells, and epsilon cells. Which one will this progenitor become? The velocity arrow alone cannot say — it just shows the cell is moving forward.

**CellRank** answers this question. Given a velocity-derived transition matrix, CellRank computes a *fate probability* for every cell — the probability that, if you let the cell continue evolving according to its transition dynamics, it will eventually reach each possible terminal state. The output is not a single predicted trajectory but a probability distribution across all possible fates, which captures the biological reality that some cells are firmly committed while others are at decision points.

This tutorial walks through the complete CellRank 2 workflow on the canonical mouse pancreatic endocrinogenesis dataset (Bastidas-Ponce et al., 2019). You will learn how to load a pre-computed velocity kernel, identify macrostates, designate terminal and initial states, compute fate probabilities, validate them biologically, and identify the genes that drive each lineage.

> 

**Prerequisites:** The `velocity` Conda environment from Part 13 is required. No checkpoint file from Part 13 is needed — this tutorial downloads its own dataset automatically. Familiarity with the AnnData object structure (covered in [Part 6](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-6-understanding-seurat-and-singlecellexperiment-objects/)) is helpful but not required.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/)

- [Why Not the GSE174609 PBMC Dataset?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Why_Not_the_GSE174609_PBMC_Dataset)
- [About the Example Dataset](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#About_the_Example_Dataset)

  - [The biology in plain terms](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#The_biology_in_plain_terms)
  - [How a progenitor becomes an endocrine cell](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#How_a_progenitor_becomes_an_endocrine_cell)
  - [Why this dataset is the canonical CellRank example](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Why_this_dataset_is_the_canonical_CellRank_example)

- [Background: Five Concepts You Need](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Background_Five_Concepts_You_Need)

  - [1. The transition matrix](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#1_The_transition_matrix)
  - [2. Markov chains and macrostates](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#2_Markov_chains_and_macrostates)
  - [3. Terminal and initial states](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#3_Terminal_and_initial_states)
  - [4. Fate probabilities (absorption probabilities)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#4_Fate_probabilities_absorption_probabilities)
  - [5. Lineage driver genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#5_Lineage_driver_genes)

- [Environment Setup](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Environment_Setup)

  - [Install CellRank 2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Install_CellRank_2)
  - [Required Python imports](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Required_Python_imports)

- [Step 1: Load the Dataset](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_1_Load_the_Dataset)
- [Step 2: Visualize the RNA Velocity Field](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_2_Visualize_the_RNA_Velocity_Field)

  - [2.1 — Compute the velocity graph](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#21_%E2%80%94_Compute_the_velocity_graph)
  - [2.2 — Plot the velocity stream](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#22_%E2%80%94_Plot_the_velocity_stream)
  - [2.3 — Plot velocity as arrows on a grid](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#23_%E2%80%94_Plot_velocity_as_arrows_on_a_grid)
  - [2.4 — Check velocity confidence](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#24_%E2%80%94_Check_velocity_confidence)

- [Step 3: Build the Combined Transition Matrix](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_3_Build_the_Combined_Transition_Matrix)

  - [3.1 — Reconstruct the VelocityKernel from the saved transition matrix](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#31_%E2%80%94_Reconstruct_the_VelocityKernel_from_the_saved_transition_matrix)
  - [3.2 — Build the ConnectivityKernel](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#32_%E2%80%94_Build_the_ConnectivityKernel)
  - [3.3 — Combine the two kernels](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#33_%E2%80%94_Combine_the_two_kernels)

- [Step 4: Compute Macrostates](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_4_Compute_Macrostates)

  - [4.1 — Initialise the GPCCA estimator](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#41_%E2%80%94_Initialise_the_GPCCA_estimator)
  - [4.2 — Compute the Schur decomposition](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#42_%E2%80%94_Compute_the_Schur_decomposition)
  - [4.3 — Choose n_states and compute macrostates](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#43_%E2%80%94_Choose_n_states_and_compute_macrostates)
  - [4.4 — Check the coarse-grained transition matrix](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#44_%E2%80%94_Check_the_coarse-grained_transition_matrix)
  - [4.5 — Verify the macrostate-to-cluster mapping](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#45_%E2%80%94_Verify_the_macrostate-to-cluster_mapping)

- [Step 5: Identify Terminal and Initial States](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_5_Identify_Terminal_and_Initial_States)

  - [5.1 — Try the automatic stability filter first](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#51_%E2%80%94_Try_the_automatic_stability_filter_first)
  - [5.2 — Use the semi-automatic route to recover Delta](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#52_%E2%80%94_Use_the_semi-automatic_route_to_recover_Delta)
  - [5.3 — Validate biologically with a crosstab](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#53_%E2%80%94_Validate_biologically_with_a_crosstab)
  - [5.4 — Visualise the terminal states on the UMAP](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#54_%E2%80%94_Visualise_the_terminal_states_on_the_UMAP)
  - [5.5 — Identify the initial state](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#55_%E2%80%94_Identify_the_initial_state)
  - [5.6 — Visualise initial and terminal states together](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#56_%E2%80%94_Visualise_initial_and_terminal_states_together)

- [Step 6: Compute Fate Probabilities](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_6_Compute_Fate_Probabilities)
- [Step 7: Visualise and Interpret Fate Probabilities](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_7_Visualise_and_Interpret_Fate_Probabilities)

  - [7.1 — One UMAP per terminal fate](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#71_%E2%80%94_One_UMAP_per_terminal_fate)
  - [7.2 — Aggregate fate probabilities by cell type (the validation heatmap)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#72_%E2%80%94_Aggregate_fate_probabilities_by_cell_type_the_validation_heatmap)
  - [7.3 — Per-cluster fate probability distributions](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#73_%E2%80%94_Per-cluster_fate_probability_distributions)
  - [7.4 — Progenitor commitment: Ngn3 low vs Ngn3 high](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#74_%E2%80%94_Progenitor_commitment_Ngn3_low_vs_Ngn3_high)
  - [7.5 — Combined four-panel UMAP using scanpy](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#75_%E2%80%94_Combined_four-panel_UMAP_using_scanpy)

- [Step 8: Identify Lineage Driver Genes](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_8_Identify_Lineage_Driver_Genes)

  - [8.1 — Compute lineage drivers](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#81_%E2%80%94_Compute_lineage_drivers)
  - [8.2 — Top driver genes per fate](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#82_%E2%80%94_Top_driver_genes_per_fate)
  - [8.3 — Visualise driver genes on the UMAP](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#83_%E2%80%94_Visualise_driver_genes_on_the_UMAP)
  - [8.4 — Driver expression vs fate probability scatter plots](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#84_%E2%80%94_Driver_expression_vs_fate_probability_scatter_plots)

- [Step 9: Gene Expression Trends and Cross-Validation](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Step_9_Gene_Expression_Trends_and_Cross-Validation)

  - [9.1 — Dot plot of driver gene expression by cell type](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#91_%E2%80%94_Dot_plot_of_driver_gene_expression_by_cell_type)
  - [9.2 — Fate probability vs Palantir pseudotime](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#92_%E2%80%94_Fate_probability_vs_Palantir_pseudotime)

- [Best Practices and Common Pitfalls](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Best_Practices_and_Common_Pitfalls)

  - [Best practices](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Best_practices)
  - [Common pitfalls](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Common_pitfalls)
  - [Defensive helper for suffixed macrostate names](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Defensive_helper_for_suffixed_macrostate_names)

- [Why Not the GSE174609 PBMC Dataset? — The Full Diagnostic Story](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Why_Not_the_GSE174609_PBMC_Dataset_%E2%80%94_The_Full_Diagnostic_Story)

  - [What we observed](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#What_we_observed)
  - [The root cause](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#The_root_cause)
  - [When CellRank works (and when to choose a different tool)](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#When_CellRank_works_and_when_to_choose_a_different_tool)

- [Key takeaways](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Key_takeaways)
- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-14-cell-fate-probability-analysis-with-cellrank/#Like_this)

## Why Not the GSE174609 PBMC Dataset?

Parts 1 through 13 of this series used the GSE174609 periodontitis PBMC dataset. Part 14 deliberately switches to a different dataset, and the reason is biological, not technical.

CellRank computes fate probabilities by modeling cells as a Markov chain on a transition matrix derived from RNA velocity. For the output to be biologically meaningful, the velocity arrows must encode genuine *directional* flow from one cell state toward another — the kind of flow that happens in developing tissues, where progenitors actively differentiate into mature cell types.

Peripheral blood mononuclear cells (PBMCs) are already differentiated. A mature NK cell is not becoming a Classical Monocyte. The unspliced-to-spliced ratios that scVelo detects in PBMCs reflect cell cycle activity, activation responses, and stress — all real biology, but none of it encodes a developmental trajectory between cell types. When CellRank was applied to GSE174609 during the development of this tutorial, it assigned 85–92% fate probability to B cells for every cell type, including Classical Monocytes and NK cells. The result reflected kNN graph topology, not biology.

The full diagnostic story appears at the end of this tutorial. For now: CellRank is the right tool for *“where are these cells going developmentally?”* It is not the right tool for *“how does a treatment alter these already-differentiated cells?”* For that question, you want the methods covered earlier in this series — pseudobulk DESeq2, CellChat ([Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/)), or differential abundance testing.

## About the Example Dataset

The dataset comes from Bastidas-Ponce et al. (2019), who used single-cell RNA sequencing to map mouse pancreatic development at embryonic day 15.5 (E15.5). In mouse gestation, which lasts roughly 20 days, E15.5 falls in the middle of endocrine pancreas formation — a window when progenitor cells are actively differentiating into the four hormone-producing endocrine cell types.

### The biology in plain terms

The adult pancreas has two functional compartments:

- The **exocrine pancreas** secretes digestive enzymes into the gut. We are not studying this part.

- The **endocrine pancreas** consists of small clusters of hormone-producing cells called the islets of Langerhans. The cells in these islets release hormones directly into the bloodstream to regulate blood sugar. This is what the dataset captures.

Each islet contains four major endocrine cell types, and each makes a different hormone:

| Cell type | Hormone | Function |
| --- | --- | --- |
| Alpha | Glucagon | Raises blood glucose when it falls too low |
| Beta | Insulin | Lowers blood glucose after a meal. Beta cells are the ones destroyed in type 1 diabetes |
| Delta | Somatostatin | Inhibits both alpha and beta cells, fine-tunes the system |
| Epsilon | Ghrelin | “Hunger hormone”; also active in the stomach |

These four mature cell types are the **terminal states** of pancreatic endocrine development. CellRank’s job in this tutorial is to figure out, for each cell in the dataset, which of these four fates it is heading toward.

### How a progenitor becomes an endocrine cell

The four mature endocrine types do not appear out of nowhere. They develop from a stepwise hierarchy of progenitor cells that progressively narrow their options:

1. **Ngn3 low EP** cells (endocrine progenitors) start expressing the transcription factor *Neurogenin 3* at low levels. *Ngn3* is the master switch that commits a cell to the endocrine lineage; without it, no endocrine cells form.

2. **Ngn3 high EP** cells have upregulated *Ngn3* further and are firmly committed to becoming endocrine but have not yet picked which of the four fates.

3. **Fev+ cells** — a transitional pre-endocrine population marked by expression of the transcription factor *Fev* (also called *Pet1*). These cells have begun expressing fate-specific transcription factors but have not yet matured into a hormone-producing cell.

4. **Mature endocrine cells** (alpha, beta, delta, epsilon) — the terminal states.

This stepwise hierarchy is exactly the structure CellRank was designed for: a clear progenitor population, defined terminal states, and a population of cells captured mid-transit between them.

> 

**Note on the cluster name “Fev+”:** In the original Bastidas-Ponce 2019 paper, the pre-endocrine population is annotated by its expression of the transcription factor *Fev* (also known as *Pet1*). The “+” indicates Fev-positive cells. Other CellRank tutorials sometimes call this population “Pre-endocrine” — it is the same biology, just a different label.

### Why this dataset is the canonical CellRank example

The Bastidas-Ponce data is the canonical example dataset across the CellRank documentation. Three properties make it ideal:

1. **RNA velocity works cleanly.** The dataset was specifically prepared for velocity analysis. The velocity arrows have clear directional flow from progenitors into mature endocrine cells.

2. **Biological ground truth exists.** The hormones produced by each cell type are textbook biology — insulin for beta cells, glucagon for alpha cells — so we can validate CellRank’s results against known marker genes.

3. **The system has rare populations.** Delta cells make up only 70 of 2,531 cells. This stress-tests CellRank’s ability to identify rare terminal states, which is a frequent challenge in real-world datasets.

---

## Background: Five Concepts You Need

Before writing any code, this section builds the conceptual foundation. If you are new to CellRank, read this section carefully — the workflow below makes much more sense once these five ideas are in place.

### 1. The transition matrix

CellRank builds a cell-by-cell **transition matrix** `T` in which `T[i, j]` is the probability that cell `i` transitions to cell `j` in one time step. The probability is computed from two ingredients: how well cell `i`‘s velocity arrow points toward cell `j`, and how similar `i` and `j` are in gene expression space (the kNN graph from Scanpy). A cell whose velocity arrow points directly at a neighbor gets a high transition probability to that neighbor; a cell whose arrow points away gets a low one.

For the pancreas dataset (2,531 cells), `T` is a 2,531 x 2,531 matrix. Each row sums to 1.0 because the transitions out of any one cell must add up to a complete probability distribution. Most entries are zero because only kNN neighbors are connected, so the matrix is sparse.

### 2. Markov chains and macrostates

A **Markov chain** on the transition matrix is a random walk: start at any cell, step to a neighbor according to the transition probabilities, step again, and so on. If you simulate many such walks, you find that the walk gets stuck in certain regions of state space for long stretches before escaping. These sticky regions are **macrostates** — metastable clusters where cells tend to remain for many consecutive transitions before moving on.

CellRank uses an algorithm called **GPCCA** (Generalized Perron Cluster Cluster Analysis) to identify macrostates from the spectrum of the transition matrix. You choose how many macrostates to compute by setting a parameter called `n_states`. For the pancreas dataset we will use `n_states=11`, which is enough to separate all four terminal endocrine fates plus the progenitor populations.

### 3. Terminal and initial states

Among the macrostates, some are **terminal**: cells flow into them and rarely leave. These correspond to mature, terminally differentiated cell types. Mathematically, terminal states have high self-transition probability in the *coarse-grained* transition matrix (`coarse_T`) — the macrostate-level summary of the full cell-cell matrix.

Other macrostates are **initial** (sources from which cells originate) or **transient** (cells pass through them on the way between sources and sinks).

In the pancreas dataset, we expect the four mature endocrine types (alpha, beta, delta, epsilon) to be terminal states, the Ngn3 low EP cluster to be the initial state (it’s the earliest progenitor population in this dataset), and the other Ngn3+ and Fev+ populations to be transient.

### 4. Fate probabilities (absorption probabilities)

Once we have designated terminal states, CellRank treats them as *absorbing* boundaries of the Markov chain and solves the **absorbing Markov chain equation**: for every non-terminal cell `i` and every terminal state `k`, what fraction of random walks starting at `i` end at `k`?

That fraction is the **fate probability** `P(i, k)`. A cell already committed to the beta cell lineage will have `P(i, beta)` close to 1.0; an early progenitor will have distributed probabilities; a cell at a decision point between two fates will have roughly equal probabilities for those two.

Fate probabilities are not a hard classification. They quantify how committed a cell is to each possible fate, capturing the stochastic nature of biological differentiation.

### 5. Lineage driver genes

Once we have fate probabilities, we can ask: which genes are most predictive of a cell ending up in each fate? CellRank computes the correlation between each gene’s expression and the fate probability vector for each terminal state. Genes with high correlation to the alpha-cell fate probability are **alpha-cell lineage drivers** — they go up in cells heading toward the alpha cell fate, even before those cells have arrived. This is a different question from “what are the markers of mature alpha cells?” — lineage drivers identify the molecular programs of commitment, not just identity.

---

## Environment Setup

### Install CellRank 2

```python
conda activate velocity

# Install CellRank 2 (skip if already installed)
pip install cellrank

# PETSc and SLEPc are scientific computing backends that make the Schur
# decomposition (used internally by GPCCA) numerically stable on Linux/WSL2
conda install -c conda-forge petsc petsc4py slepc slepc4py --yes

# Verify the installation
python -c "import cellrank; print('CellRank version:', cellrank.__version__)"
```

> 

**Expected output:** `CellRank version: 2.x.x` (any version 2.0 or later). If you see version 1.x, run `pip install --upgrade cellrank` — CellRank 1 has a different API and this tutorial will not work.

> 

**Platform compatibility — read this before you start.** This tutorial requires **Linux or WSL2 on Windows**. It does **not** work on macOS with Apple Silicon. The recommended setup for Mac users is to run the workflow inside a Linux container (Docker or Lima) or on a Linux HPC node via SSH — for example, a SLURM environment set up following the Pixi/HPC tutorial on NGS101.com.

### Required Python imports

```python
import os
import warnings
import logging

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

import scanpy as sc
import scvelo as scv
import cellrank as cr

# Reduce log spam so output stays focused
sc.settings.verbosity = 1
scv.settings.verbosity = 1
logging.getLogger("cellrank").setLevel(logging.ERROR)

# Suppress harmless scvelo / matplotlib warnings
warnings.filterwarnings("ignore", message="No data for colormapping")
warnings.filterwarnings("ignore", category=FutureWarning)

# Figure output settings
sc.settings.set_figure_params(dpi=100, frameon=False)
scv.settings.figdir = "figures/"
os.makedirs("figures/", exist_ok=True)
```

---

## Step 1: Load the Dataset

The Bastidas-Ponce pancreas dataset is distributed by the CellRank team as a preprocessed AnnData file on figshare. It includes the full RNA velocity transition matrix already computed and stored inside the AnnData object, so we do not need to re-run velocity estimation. This file is the direct equivalent of the `adata_after_velocity_graph.h5ad` checkpoint from [Part 13](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/), just for a different dataset.

Download the file once from the terminal:

```
mkdir -p datasets
curl -L --output datasets/endocrinogenesis_day15.5_preprocessed-kernel.h5ad \
     https://ndownloader.figshare.com/files/41325411
```

Once the download has finished, load it in Python:

```python
adata = sc.read("datasets/endocrinogenesis_day15.5_preprocessed-kernel.h5ad")
print(adata)
```

**Output:**

```
AnnData object with n_obs x n_vars = 2531 x 5974
    obs: 'day', 'proliferation', 'G2M_score', 'S_score', 'phase', 'clusters_coarse',
         'clusters', 'clusters_fine', 'louvain_Alpha', 'louvain_Beta',
         'palantir_pseudotime', 'initial_size_unspliced', 'initial_size_spliced',
         'initial_size', 'n_counts'
    var: 'highly_variable_genes', 'gene_count_corr', 'means', 'dispersions',
         'dispersions_norm', 'highly_variable', 'fit_r2', 'fit_alpha', 'fit_beta',
         'fit_gamma', 'fit_t_', 'fit_scaling', 'fit_std_u', 'fit_std_s',
         'fit_likelihood', 'fit_u0', 'fit_s0', 'fit_pval_steady', 'fit_steady_u',
         'fit_steady_s', 'fit_variance', 'fit_alignment_scaling', 'velocity_genes'
    uns: 'T_fwd_params', 'clusters_colors', 'clusters_fine_colors',
         'louvain_Alpha_colors', 'louvain_Beta_colors', 'neighbors', 'pca',
         'recover_dynamics', 'velocity_params'
    obsm: 'T_fwd_umap', 'X_pca', 'X_umap'
    varm: 'PCs', 'loss'
    layers: 'Ms', 'Mu', 'fit_t', 'fit_tau', 'fit_tau_', 'spliced', 'unspliced',
            'velocity', 'velocity_u'
    obsp: 'T_fwd', 'connectivities', 'distances'
```

**What you are seeing:** This is the standard AnnData summary. The key things to notice:

- `n_obs x n_vars = 2531 x 5974` — 2,531 cells and 5,974 genes after preprocessing

- `obs: ... 'clusters' ... 'palantir_pseudotime' ...` — the AnnData includes cell-type annotations under `clusters` and Palantir pseudotime values we will use later for cross-validation

- `obsm: 'X_pca', 'X_umap'` — PCA and UMAP coordinates are already computed

- `layers: ... 'spliced', 'unspliced', 'velocity' ...` — the spliced/unspliced count matrices and the RNA velocity vectors are already there

- `obsp: 'T_fwd', 'connectivities', 'distances'` — the velocity-derived transition matrix `T_fwd` is precomputed and ready to use

Now confirm that the three components we will rely on are actually present, and look at the cell-type distribution:

```python
print("Velocity kernel present:", "T_fwd" in adata.obsp)
print("Cell type annotations present:", "clusters" in adata.obs.columns)
print("UMAP coordinates present:", "X_umap" in adata.obsm)

print("\nCell type counts:")
print(adata.obs["clusters"].value_counts())
```

**Output:**

```
Velocity kernel present: True
Cell type annotations present: True
UMAP coordinates present: True

Cell type counts:
clusters
Beta            591
Fev+            587
Ngn3 high EP    529
Alpha           477
Epsilon         139
Ngn3 low EP     138
Delta            70
Name: count, dtype: int64
```

**What you are seeing:** Seven cell types, totaling 2,531 cells. Beta is the largest population (591 cells), reflecting the beta-biased nature of pancreas development at E15.5 — this is when beta cell production peaks. Delta is the rarest (70 cells), which is biologically expected and will matter when we choose `n_states` in Step 4. CellRank needs enough macrostates to give Delta a chance to be resolved as its own state rather than being folded into a larger neighbour.

---

## Step 2: Visualize the RNA Velocity Field

Before handing the data to CellRank, it is worth looking directly at the RNA velocity field. In [Part 13](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/) you computed RNA velocity with scVelo; this dataset arrives with that velocity already computed and stored in `adata.layers["velocity"]`. CellRank will turn that velocity into fate probabilities — but it can only produce meaningful results if the velocity itself encodes clean, directional developmental flow. This step lets you see that flow with your own eyes and confirm the dataset is suitable before investing in the full CellRank workflow.

### 2.1 — Compute the velocity graph

The dataset contains the per-cell velocity vectors but not the *velocity graph* — the cell-to-cell similarity structure that scVelo’s plotting functions need in order to project velocity onto the UMAP. We compute it now:

```bash
# The velocity vectors are already in adata.layers["velocity"];
# this builds the cell-cell velocity graph needed for the embedding plots
scv.tl.velocity_graph(adata)
```

> 

**What is the velocity graph, and how is it different from CellRank’s transition matrix?** For every pair of neighbouring cells, scVelo asks: does cell A’s velocity vector point toward cell B’s current position? The velocity graph stores these directional similarities. It is conceptually similar to the CellRank transition matrix we will build in Step 3, but scVelo uses it specifically to draw velocity arrows on a 2D embedding. We compute it here only for visualization — CellRank builds its own transition matrix independently in the next step, so this graph and the CellRank kernel do not interfere with each other.

### 2.2 — Plot the velocity stream

The stream plot is the most intuitive way to view a velocity field. It renders velocity as smooth flow lines, like a weather map showing wind direction:

```
scv.pl.velocity_embedding_stream(
    adata,
    basis="umap",
    color="clusters",
    title="RNA Velocity Stream -- Pancreatic Endocrinogenesis",
    legend_loc="right margin",
    save="figures/velocity_stream.pdf"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/velocity_stream.png?resize=1024%2C610&ssl=1)

> 

**How to read this plot and why it matters for CellRank.** The streamlines trace the developmental trajectory. They start at the progenitor pool and end at the mature endocrine fates, which means the velocity field encodes a genuine, directional differentiation process. This is exactly the structure CellRank needs. Contrast this with the PBMC dataset discussed at the end of this tutorial: in PBMCs the streamlines form disorganised swirls with no clear source or sink, because mature immune cells are not differentiating into one another. If your own dataset’s stream plot looks like a directionless swirl, that is an early warning that CellRank fate probabilities will not be biologically meaningful — and a much cheaper warning than discovering it after the full analysis.

### 2.3 — Plot velocity as arrows on a grid

The grid plot shows the same information as discrete arrows rather than continuous streamlines. Some readers find the arrow view easier to interpret locally:

```
scv.pl.velocity_embedding_grid(
    adata,
    basis="umap",
    color="clusters",
    title="RNA Velocity Grid -- Pancreatic Endocrinogenesis",
    legend_loc="right margin",
    arrow_length=2,
    arrow_size=1.5,
    save="figures/velocity_grid.pdf"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/velocity_grid.png?resize=1024%2C617&ssl=1)

### 2.4 — Check velocity confidence

Finally, we quantify how trustworthy the velocity field is. The `velocity_confidence` score measures, for each cell, how well its velocity vector agrees with those of its neighbours — coherent local flow indicates reliable velocity:

```
scv.tl.velocity_confidence(adata)

scv.pl.scatter(
    adata,
    c=["velocity_length", "velocity_confidence"],
    cmap="coolwarm",
    perc=[5, 95],
    basis="umap",
    save="figures/velocity_confidence.pdf"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/velocity_confidence.png?resize=1024%2C495&ssl=1)

**How to read the UMAPs** `velocity_length` is the magnitude of each cell’s velocity vector — how fast its transcriptome is changing overall. There is no rule that progenitors change faster than mature cells; velocity magnitude is large wherever many genes are being induced or repressed strongly. In this dataset the **Beta cells show the highest `velocity_length`**, because newly-born beta cells at E15.5 are rapidly inducing the insulin program — *Ins1* and *Ins2* are among the most highly transcribed genes in the body, and inducing them produces a large velocity vector. A high `velocity_length` in a terminal cell type is not a contradiction: magnitude measures the *speed* of transcriptional change, while the velocity *direction* — which is what feeds the transition matrix and determines fate — points “deeper into beta identity”, a self-transition. `velocity_confidence` shows how coherent each cell’s velocity is with its neighbours, on a 0–1 scale; most cells should score above 0.8 (warm colours).

> 

**Why this matters.** High velocity confidence across the dataset means the velocity field is smooth and self-consistent — neighbouring cells agree on which way development is flowing. This is the quantitative version of the visual check from the stream plot. Scattered low-confidence cells are normal; low confidence across whole regions would be a warning sign. For this dataset, confidence is high throughout the differentiation trajectory, which is one of the reasons it is the canonical CellRank example.

With the velocity field confirmed to be clean and directional, we can now hand the data to CellRank.

---

## Step 3: Build the Combined Transition Matrix

The transition matrix encodes how cells move through state space. CellRank lets you combine signals from multiple sources by adding their transition matrices with weights. In this tutorial we combine two:

- **VelocityKernel** (80% weight): directional flow from RNA velocity

- **ConnectivityKernel** (20% weight): smoothing based on gene expression similarity in kNN space

The 20% ConnectivityKernel acts as a stabilizer. RNA velocity arrows can be noisy for individual cells, especially when only a few genes contribute to the dynamics. Mixing in some similarity-based smoothing prevents noisy individual velocity estimates from dominating the Markov chain.

### 3.1 — Reconstruct the VelocityKernel from the saved transition matrix

The dataset already contains the velocity-derived transition matrix in `adata.obsp["T_fwd"]`, so we reconstruct the kernel rather than recompute it:

```python
from cellrank.kernels import VelocityKernel, ConnectivityKernel

vk = VelocityKernel.from_adata(adata, key="T_fwd")
print("VelocityKernel transition matrix shape:", vk.transition_matrix.shape)
```

**Output:**

```
VelocityKernel transition matrix shape: (2531, 2531)
```

**What you are seeing:** A 2,531 x 2,531 transition matrix — one row and one column per cell. Each row stores the probabilities of that cell moving to every other cell in the next time step.

### 3.2 — Build the ConnectivityKernel

```python
ck = ConnectivityKernel(adata)
ck.compute_transition_matrix()
print("ConnectivityKernel transition matrix shape:", ck.transition_matrix.shape)
```

**Output:**

```
ConnectivityKernel transition matrix shape: (2531, 2531)
```

**What you are seeing:** The ConnectivityKernel produces a second 2,531 x 2,531 matrix, this one based purely on gene expression similarity rather than velocity direction.

### 3.3 — Combine the two kernels

```python
combined_kernel = 0.8 * vk + 0.2 * ck

print("Combined kernel shape:", combined_kernel.transition_matrix.shape)
print("Row sum check (should be 1.0):",
      combined_kernel.transition_matrix.sum(axis=1).min(),
      "to",
      combined_kernel.transition_matrix.sum(axis=1).max())
```

**Output:**

```
Combined kernel shape: (2531, 2531)
Row sum check (should be 1.0): 0.9999999999999996 to 1.0000000000000004
```

**What you are seeing:** Every row of the combined transition matrix sums to 1.0 (the tiny deviations are floating-point noise — 0.9999999999999996 differs from 1.0 by less than one part in a quadrillion). This confirms that for each cell, the transition probabilities form a complete probability distribution over its possible next cells. The kernel is ready for the macrostate analysis.

> 

**Note on the kernel mix.** The 80:20 split used here gives velocity the leading role while letting connectivity smooth out per-cell noise. This is not a critical choice on this dataset: the canonical CellRank “Estimating Fate Probabilities and Driver Genes” tutorial uses a *pure* VelocityKernel (no ConnectivityKernel at all) on this same file and reaches the same conclusions. Anything from pure velocity to an 80:20 mix works here. The ConnectivityKernel earns its weight on noisier datasets, where individual-cell velocity estimates are less reliable and the expression-similarity smoothing prevents isolated noisy cells from distorting the Markov chain.

---

## Step 4: Compute Macrostates

The GPCCA estimator is CellRank’s tool for finding macrostates from the transition matrix. The workflow has three sub-steps: initialise the estimator, compute the Schur decomposition, then call `compute_macrostates` with the chosen number of states.

### 4.1 — Initialise the GPCCA estimator

```python
from cellrank.estimators import GPCCA

g = GPCCA(combined_kernel)
print(f"GPCCA estimator initialised. Cells: {g.adata.n_obs}")
```

**Output:**

```
GPCCA estimator initialised. Cells: 2531
```

**What you are seeing:** A GPCCA object `g` has been created, wrapping the combined kernel. Everything we do from now on — macrostates, terminal states, fate probabilities, lineage drivers — happens through this object.

### 4.2 — Compute the Schur decomposition

The Schur decomposition is a mathematical transformation of the transition matrix that exposes its dominant structure. GPCCA uses the top Schur vectors to identify macrostates. We compute up to 20 Schur components, which is plenty more than we will actually use (we will pick 11 macrostates in the next step).

```python
g.compute_schur(n_components=20)
print("Schur decomposition complete.")
```

**Output:**

```
WARNING: Using `20` components would split a block of complex conjugate eigenvalues. Using `n_components=21`
Schur decomposition complete.
```

**What you are seeing — and why the warning is harmless:** The Schur decomposition keeps pairs of complex conjugate eigenvalues together as 2×2 blocks. If your requested `n_components` happens to land in the middle of such a pair, CellRank automatically bumps it up by one to keep the pair intact. So instead of 20 components, we get 21. This makes no difference to anything downstream because we will only use the first 11 components in the next step, well below either 20 or 21. No action needed — the warning is just CellRank being transparent about what it did.

### 4.3 — Choose n_states and compute macrostates

The choice of `n_states` is the most consequential parameter in the entire workflow. It controls how finely GPCCA partitions the cells.

- If `n_states` is too small, rare populations like Delta (70 cells) get folded into more abundant neighbours and disappear from the macrostate list.

- If `n_states` is too large, the Schur decomposition becomes numerically unstable, which shows up as negative values in the off-diagonal of the coarse-grained transition matrix.

For this dataset, the canonical CellRank tutorial uses `n_states=11`. The CellRank team explicitly comments that they “compute a few more states than the algorithm suggests to have a chance at identifying the Delta cell population.” We follow that recommendation:

```bash
g.compute_macrostates(n_states=11, cluster_key="clusters")

# Plot the macrostates on the UMAP for a visual sanity check
g.plot_macrostates(which="all", basis="umap", legend_loc="right margin",
                   title="Macrostates (k=11)", save="figures/macrostates_k11.pdf")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/macrostates_k11.png?resize=1024%2C702&ssl=1)

**Output:** A UMAP plot is saved to `figures/macrostates_k11.pdf`. Each macrostate appears as a small coloured island on the embedding. You should see eleven distinct macrostates, with the four endocrine fates (Alpha, Beta, Delta, Epsilon) appearing at the tips of the differentiation branches and the Ngn3+ progenitor populations forming a connected core in the middle of the UMAP.

> 

**Why 11 and not the algorithm’s suggested value?** CellRank’s `plot_spectrum` function suggests a number of macrostates based on the largest gap in the eigenvalue spectrum. That heuristic identifies the most prominent terminal fates but tends to miss rare lineages. For datasets where the biology demands a specific number of terminal states (here, four), it is reasonable to override the heuristic. The cost is one or two extra progenitor subdivisions in the macrostate list, which is harmless — we just pick the terminal states we care about from the larger pool.

### 4.4 — Check the coarse-grained transition matrix

After computing macrostates, always inspect the coarse-grained transition matrix `g.coarse_T`. This matrix summarises how cells move between macrostates rather than between individual cells, and its diagonal tells us how stable each macrostate is.

```python
# Self-transition probability for each macrostate (sorted)
stability = pd.Series(
    np.diag(g.coarse_T.values),
    index=g.coarse_T.index
).sort_values(ascending=False)

print("Macrostate stability (diagonal of coarse_T):")
print(stability.round(3))

# Largest negative off-diagonal entry -- a numerical stability check
off_diag = g.coarse_T.values - np.diag(np.diag(g.coarse_T.values))
print(f"\nLargest negative off-diagonal: {off_diag.min():.4f}")

print("\nFull coarse transition matrix:")
print(g.coarse_T.round(3))
```

**Output (abbreviated — the full coarse_T is an 11×11 matrix):**

```
Macrostate stability (diagonal of coarse_T):
Beta              0.992
Epsilon           0.986
Alpha             0.971
Fev+              0.930
Ngn3 high EP_4    0.912
Ngn3 high EP_3    0.877
Ngn3 high EP_2    0.865
Ngn3 low EP_1     0.834
Delta             0.822
Ngn3 high EP_1    0.791
Ngn3 low EP_2     0.656
dtype: float64

Largest negative off-diagonal: -0.1315
```

**What you are seeing:**

The eleven macrostates are sorted by their self-transition probability (the diagonal of `coarse_T`). This number tells you how “sticky” each macrostate is — the probability that a cell currently in the macrostate stays there in the next step.

- The four endocrine fates (**Beta 0.992, Epsilon 0.986, Alpha 0.971, Delta 0.822**) all sit toward the top. Beta is essentially absorbing — once cells reach the beta macrostate they almost never leave. Alpha and Epsilon are nearly absorbing. Delta is less sticky (0.822) because some Delta cells transition into Beta (we will see this in the full matrix and in the heatmap later); this is a real biological observation, not a bug.

- **Fev+ at 0.930** is moderately stable — it sits between transient and terminal because Fev+ cells are *transitioning* toward the mature fates but a meaningful fraction get caught in the macrostate during a random walk.

- The **Ngn3 high EP subdivisions** (0.912, 0.877, 0.865, 0.791) and **Ngn3 low EP subdivisions** (0.834, 0.656) have lower stability because cells actively flow through them on the way to commitment. This is what progenitor populations should look like.

> 

**Why does Ngn3 high EP split into four subdivisions and Ngn3 low EP into two?** GPCCA divides each annotated cluster only when there is internal structure to find. The Ngn3 high EP cluster is heterogeneous: it contains cells in different cell cycle phases, cells tilting toward different endocrine fates, and cells at different stages of Ngn3 upregulation. At `n_states=11`, GPCCA naturally subdivides this heterogeneity into four sub-populations. The numeric suffixes (_1, _2, _3, _4) are just CellRank’s way of disambiguating macrostates that share the same dominant annotated cell type. We will not use any of these as terminal states.

**About the negative off-diagonal of -0.1315:** the off-diagonal entries of `coarse_T` are transition probabilities between macrostates, and in a true probability matrix they should all be non-negative. Small negative values reflect numerical imprecision from the Schur decomposition. In our run, the -0.1315 occurs between two of the Ngn3 high EP subdivisions, not between any of the endocrine fates. Look at the rows for Alpha, Beta, Delta, and Epsilon in the full `coarse_T` printout — those rows are clean. This is what matters: the *endocrine* macrostates are well-resolved, and the downstream fate probability computation operates on the full cell-cell matrix (not this coarse summary), so the imprecision in progenitor-to-progenitor transitions does not corrupt the terminal-state analysis.

> 

**General guidance:** if you see large negative off-diagonals between *endocrine* macrostate rows (the rows you intend to use as terminal states), or if the downstream fate probability heatmap in Step 7.2 does not show a clean diagonal, that is when to worry and try a different `n_states`. Negatives confined to progenitor-to-progenitor transitions are not a problem.

### 4.5 — Verify the macrostate-to-cluster mapping

GPCCA names each macrostate by its dominant annotated cell type. Sanity check that the names actually correspond to the biology:

```python
macrostate_df = adata.obs[["clusters", "macrostates_fwd"]].dropna()
print(pd.crosstab(macrostate_df["macrostates_fwd"], macrostate_df["clusters"]))
```

**Output:**

```
clusters         Ngn3 low EP  Ngn3 high EP  Fev+  Beta  Alpha  Delta  Epsilon
macrostates_fwd
Ngn3 low EP_1             30             0     0     0      0      0        0
Ngn3 low EP_2             30             0     0     0      0      0        0
Delta                      0             0     0     0      0     30        0
Ngn3 high EP_1             8            22     0     0      0      0        0
Ngn3 high EP_2             0            30     0     0      0      0        0
Ngn3 high EP_3             0            30     0     0      0      0        0
Epsilon                    0             0     0     0      0      0       30
Alpha                      0             0     0     0     30      0        0
Ngn3 high EP_4             0            30     0     0      0      0        0
Fev+                       0             0    20     0     10      0        0
Beta                       0             0     0    30      0      0        0
```

**What you are seeing:** Each macrostate’s 30 boundary cells (the cells most strongly assigned to that macrostate) come from the annotated cluster the macrostate is named after. This is the validation we want — the four endocrine macrostates (Alpha, Beta, Delta, Epsilon) draw their boundary cells exclusively from the matching annotated cluster.

Two interesting wrinkles:

- **Ngn3 high EP_1** has 8 cells from Ngn3 low EP mixed in with 22 from Ngn3 high EP. This makes sense biologically: Ngn3 high EP_1 sits on the boundary between low and high Ngn3 expression, so some cells annotated as Ngn3 low are starting to look like Ngn3 high.

- **Fev+** has 10 cells from the Alpha cluster mixed in with 20 from Fev+. This reflects the fact that Fev+ is a transitional state — some cells that have just begun their alpha commitment still look transcriptionally similar to the pre-endocrine pool.

Both wrinkles are biological signal, not noise.

---

## Step 5: Identify Terminal and Initial States

This is the step where we tell CellRank which macrostates correspond to mature endpoints of differentiation. CellRank offers two routes for this, and the canonical workflow uses both in sequence.

- **Automatic route:** `predict_terminal_states()` — applies a stability filter to keep only the macrostates that exceed a self-transition threshold (default 0.96).

- **Semi-automatic route:** `set_terminal_states(states=[...])` — you name the macrostates by hand, and CellRank assigns the boundary cells automatically.

The canonical CellRank pancreas tutorial demonstrates both, in order. The automatic route is the first thing to try because it requires no biological prior. But on this dataset, the automatic route misses Delta — and that miss is itself an informative result, not a bug.

### 5.1 — Try the automatic stability filter first

```python
g.predict_terminal_states()

if g.terminal_states is None:
    print("No macrostates cleared the default stability threshold.")
else:
    print("Macrostates picked by the automatic stability filter:")
    print(g.terminal_states.value_counts())
```

**Output:**

```
Macrostates picked by the automatic stability filter:
Epsilon    30
Alpha      30
Beta       30
Name: count, dtype: int64
```

**What you are seeing:** The automatic filter picked three macrostates — Epsilon, Alpha, and Beta — as terminal states. Delta is missing.

This is not a bug. Look back at the stability scores from Step 4.4: Delta had a self-transition probability of 0.822, well below the default stability threshold of 0.96. The automatic filter rejected Delta because the Markov chain shows some cells flowing from Delta into Beta, which lowers Delta’s stickiness. The CellRank docs use this dataset specifically to illustrate this case: *“we did not catch the Delta cells!”*

> 

**Why does the automatic filter miss Delta?** Delta cells share part of their developmental programme with Beta cells, and the Markov chain reflects this: a random walk that lands in the Delta macrostate has a non-trivial probability of escaping into the Beta macrostate. This drops Delta’s self-transition probability below 0.96. The threshold is conservative on purpose — it is biased toward the most strongly absorbing macrostates and will routinely miss biologically real terminal fates that exhibit cross-talk with other lineages.

You could lower the threshold (`g.predict_terminal_states(stability_threshold=0.80)`) to recover Delta, but that also risks pulling in transitional macrostates like Fev+ (stability 0.930). The cleaner fix is the semi-automatic route below.

### 5.2 — Use the semi-automatic route to recover Delta

We override the automatic selection by naming the four endocrine fates explicitly. CellRank still does the work of choosing which cells in each macrostate count as boundary cells; we only supply the macrostate names.

```python
g.set_terminal_states(states=["Alpha", "Beta", "Epsilon", "Delta"])

print("Terminal states after semi-automatic designation:")
print(g.terminal_states.value_counts())
```

**Output:**

```
Terminal states after semi-automatic designation:
Alpha      30
Beta       30
Epsilon    30
Delta      30
Name: count, dtype: int64
```

**What you are seeing:** All four endocrine fates are now designated, each with 30 boundary cells. Even Delta, which the automatic filter rejected, is included because we named it explicitly. The 30 cells per fate are the cells most strongly assigned to each macrostate — they will be the “absorbing” cells in the Markov chain.

> 

**What if you get a KeyError because of suffixes?** GPCCA appends `_1` / `_2` suffixes only when multiple macrostates share the same dominant annotated cell type. At `n_states=11` on this dataset, suffixes typically appear only on the Ngn3+ progenitor populations, not on the four endocrine fates. If your run does suffix one of the endocrine names, print `adata.obs["macrostates_fwd"].cat.categories.tolist()` and replace the offending string in `states=[...]` with the exact name from your list. See the troubleshooting table at the end of this tutorial for a defensive helper that automates this lookup.

### 5.3 — Validate biologically with a crosstab

A terminal state is only useful if its boundary cells actually come from the annotated cell type it claims to represent. Confirm this:

```python
terminal_df = adata.obs[["clusters", "term_states_fwd"]].dropna()
print("Terminal state -- annotated cluster mapping:")
print(pd.crosstab(terminal_df["term_states_fwd"], terminal_df["clusters"]))
```

**Output:**

```
clusters         Beta  Alpha  Delta  Epsilon
term_states_fwd
Alpha               0     30      0        0
Beta               30      0      0        0
Epsilon             0      0      0       30
Delta               0      0     30        0
```

**What you are seeing:** A perfect diagonal. Every Alpha terminal-state boundary cell comes from the annotated Alpha cluster; every Beta boundary cell comes from the annotated Beta cluster; and so on for Delta and Epsilon. This is the cleanest possible validation that the terminal state designations match the biological cell type identities.

### 5.4 — Visualise the terminal states on the UMAP

```
adata.obs["terminal_states_plot"] = g.terminal_states

scv.pl.scatter(
    adata,
    color="terminal_states_plot",
    basis="umap",
    legend_loc="right margin",
    title="Terminal States",
    save="figures/terminal_states.pdf"
)
```

**Output:** A UMAP plot saved to `figures/terminal_states.pdf`. You will see four small coloured groups — one per terminal fate — each at the tip of a distinct branch in the endocrine compartment of the UMAP. The rest of the cells (the majority) appear as grey background; they are not boundary cells of any terminal state.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/terminal_states.png?resize=1024%2C816&ssl=1)

### 5.5 — Identify the initial state

The initial state is the source from which cells flow outward. CellRank finds it the same way it finds terminal states, but in the time-reversed Markov chain — the macrostate that loses cells most quickly in the forward chain becomes the most stable state in the reversed chain.

```python
g.predict_initial_states(n_states=1, allow_overlap=True)

print("Initial state identified:")
print(g.initial_states.value_counts())
```

**Output:**

```
Initial state identified:
Ngn3 low EP_1    30
Name: count, dtype: int64
```

**What you are seeing:** CellRank picked the `Ngn3 low EP_1` macrostate as the initial state. This is biologically correct: Ngn3 low EP is the earliest cell type in this dataset (the dataset starts after the ductal-to-progenitor transition, so the Ngn3 low EP progenitors are effectively the “source” of the differentiation trajectory). The `_1` suffix simply means GPCCA split Ngn3 low EP into two subdivisions, and `_1` is the one with the highest outflow.

> 

**Why `allow_overlap=True`?** The candidate boundary cells for the initial state can spatially overlap with macrostates that were already designated as terminal. Without this flag, CellRank would refuse to proceed if any overlap existed. Setting it to `True` tells CellRank to proceed regardless — the overlapping cells just get treated as boundary cells of whichever role was designated first (the terminal states, in our case).

### 5.6 — Visualise initial and terminal states together

```python
adata.obs["terminal_states_plot"] = g.terminal_states
adata.obs["initial_states_plot"]  = g.initial_states

fig, axes = plt.subplots(1, 2, figsize=(16, 6))

scv.pl.scatter(adata, color="terminal_states_plot", basis="umap", ax=axes[0],
               legend_loc="right margin", title="Terminal States (Sinks)", show=False)
scv.pl.scatter(adata, color="initial_states_plot",  basis="umap", ax=axes[1],
               legend_loc="right margin", title="Initial States (Sources)", show=False)

plt.tight_layout()
plt.savefig("figures/initial_terminal_states.pdf")
plt.show()
```

**Output:** A side-by-side figure with two UMAPs. On the left, the four terminal states sit at the tips of the endocrine branches. On the right, the single initial state (Ngn3 low EP_1) appears at the root of the differentiation trajectory. The flow from initial state to terminal states is now visually obvious: cells originate at the Ngn3 low EP root and migrate outward to the four endocrine fates.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/initial_terminal_states.png?resize=1024%2C379&ssl=1)

---

## Step 6: Compute Fate Probabilities

With terminal states designated, we can finally compute the central output of CellRank: for every cell, the probability of reaching each of the four endocrine fates.

```python
g.compute_fate_probabilities()

print("Fate probabilities computed.")
print("Fate names:", g.fate_probabilities.names.tolist())
print("Fate probability matrix shape:", g.fate_probabilities.X.shape)
```

**Output:**

```
Fate probabilities computed.
Fate names: ['Alpha', 'Beta', 'Epsilon', 'Delta']
Fate probability matrix shape: (2531, 4)
```

**What you are seeing:** A 2,531 x 4 matrix. Each row corresponds to a cell, each column to a terminal fate, and each value is the probability that a random walk starting at that cell ends at that fate. Every row sums to 1.0. The fate names come out in the order we passed them to `set_terminal_states`, which is why Delta is listed last.

```bash
# Save the checkpoint so you can pick up here later without re-running Steps 1-6
adata.write("adata_cellrank_pancreas.h5ad")
# To reload: adata = sc.read("adata_cellrank_pancreas.h5ad")
```

---

## Step 7: Visualise and Interpret Fate Probabilities

We now have the central CellRank output. The next five sub-steps visualize it from different angles — per-fate UMAPs, an aggregate heatmap, per-cluster distributions, a comparison of progenitor commitment, and a combined four-panel view.

### 7.1 — One UMAP per terminal fate

```python
# Extract fate probabilities to adata.obs for plotting.
# np.array() strips the CellRank Lineage metadata to avoid index mismatches in scvelo.
fate_names = list(g.fate_probabilities.names)
fate_probs_array = np.array(g.fate_probabilities)

for i, fate in enumerate(fate_names):
    adata.obs[f"to_{fate}"] = fate_probs_array[:, i]

fig, axes = plt.subplots(1, len(fate_names), figsize=(6 * len(fate_names), 5))
for ax, fate in zip(axes, fate_names):
    scv.pl.scatter(
        adata,
        color=f"to_{fate}",
        basis="umap",
        ax=ax,
        color_map="RdYlBu_r",
        vmin=0,
        vmax=1,
        title=f"Fate: {fate}",
        show=False
    )
plt.tight_layout()
plt.savefig("figures/fate_probabilities_umap.pdf")
plt.show()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/fate_probabilities_umap.png?resize=1024%2C207&ssl=1)

**Output:** Four UMAP panels in a row, each coloured by the probability of one specific fate. Red means high probability of that fate, blue means low.

**What to look for in each panel:**

- **Fate: Alpha** — the mature Alpha cluster should be deep red. Most progenitors should be light blue (low Alpha probability).

- **Fate: Beta** — the mature Beta cluster is deep red. Notice that *most progenitors are not blue here* — they appear yellow or orange, indicating substantial Beta probability across the progenitor pool. This reflects the strong beta-biased development at E15.5.

- **Fate: Epsilon** — the small Epsilon cluster is deep red. Progenitors are mostly blue.

- **Fate: Delta** — the small Delta cluster is red. Notice that Delta probabilities outside the Delta cluster itself are near zero (most cells are dark blue) — Delta-bound cells form a much narrower channel through the UMAP than Beta-bound cells.

### 7.2 — Aggregate fate probabilities by cell type (the validation heatmap)

The single most important plot in any CellRank analysis is the mean fate probability heatmap. If CellRank is working correctly, this heatmap should show a clear pattern: each mature cell type should have the highest mean fate probability for its own lineage.

```python
# Aggregate fate probabilities by annotated cell type
fate_df = pd.DataFrame(
    g.fate_probabilities.X,
    columns=g.fate_probabilities.names,
    index=adata.obs_names
)
fate_df["cell_type"] = adata.obs["clusters"].values

fate_summary = fate_df.groupby("cell_type").mean()
print("Mean fate probabilities by cell type:")
print(fate_summary.round(3))

fig, ax = plt.subplots(figsize=(8, 7))
sns.heatmap(
    fate_summary,
    annot=True,
    fmt=".2f",
    cmap="YlOrRd",
    linewidths=0.5,
    ax=ax,
    cbar_kws={"label": "Mean Fate Probability"}
)
ax.set_title("Mean Fate Probability by Cell Type")
ax.set_xlabel("Terminal State")
ax.set_ylabel("Cell Type")
plt.tight_layout()
plt.savefig("figures/fate_probability_heatmap.pdf")
plt.show()
```

**Output:**

```
Mean fate probabilities by cell type:
              Alpha   Beta  Epsilon  Delta
cell_type
Ngn3 low EP   0.138  0.733    0.125  0.005
Ngn3 high EP  0.138  0.732    0.126  0.005
Fev+          0.133  0.719    0.140  0.008
Beta          0.029  0.950    0.020  0.002
Alpha         0.515  0.309    0.174  0.002
Delta         0.021  0.341    0.027  0.612
Epsilon       0.059  0.211    0.723  0.006
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/fate_probability_heatmap.png?resize=1024%2C885&ssl=1)

**This is the most important output of the entire tutorial. Let us read it carefully.**

**Mature cells preserve their identity:**

- **Beta** cells have 0.95 Beta fate probability — almost perfect. Beta cells in the dataset are firmly committed to the beta cell fate.

- **Epsilon** cells have 0.72 Epsilon fate probability. The remaining ~0.28 is spread mostly across Beta (0.21) and Alpha (0.06), reflecting that the Epsilon cluster is small (139 cells) and sits transcriptionally close to the other endocrine populations, so some random walks reach Beta or Alpha before reaching Epsilon.

- **Delta** cells have 0.61 Delta fate probability, with 0.34 going to Beta. This is biologically informative: Delta cells share a developmental programme with Beta cells, and the Markov chain captures that some Delta-annotated cells could plausibly transition into Beta. This is the same observation that caused `predict_terminal_states` to miss Delta in Step 5.1 — it is a feature of the biology.

- **Alpha** cells have 0.52 Alpha fate probability, with 0.31 going to Beta. Alpha commitment at E15.5 is genuinely less crisp than Beta commitment; this is a well-documented developmental observation.

**Progenitors are heavily Beta-biased:**

- **Ngn3 low EP**, **Ngn3 high EP**, and **Fev+** all show ~0.73 Beta fate probability and only ~0.13 Alpha, ~0.13 Epsilon, and <0.01 Delta. This is not a CellRank artifact — it reflects the real biology of the E15.5 pancreas, which is the peak window for beta cell production. The Markov chain on this dataset honestly captures the fact that most progenitors are heading toward Beta. The canonical CellRank “Estimating Fate Probabilities and Driver Genes” tutorial uses this identical dataset and an identical workflow (with a pure VelocityKernel) and reports the same result — “most cells appear to be fate-biased towards Beta cells, in agreement with known biology for mouse pancreas development at E15.5.” If you ran the same analysis on E12.5 data, when alpha cell production dominates, you would see the bias flip.

**How to use this heatmap:** the validation criterion is whether each mature cluster has its own fate as the dominant column. Beta: yes (0.95 is dominant). Alpha: yes (0.52 is dominant). Delta: yes (0.61 is dominant). Epsilon: yes (0.72 is dominant). All four pass. The strong Beta bias in progenitors is a biological signal, not a software problem.

> 

**What if you do not see this pattern?** If a mature cluster has its highest probability for the *wrong* fate, that is a red flag. The most common cause is Schur instability at the chosen `n_states` — print the full `g.coarse_T` and look for large negative values in the rows of the endocrine macrostates (not just any row). If those rows have problems, try a different `n_states`.

**Where do the rare fates come from? — coarse vs. fine cluster annotation**

The heatmap shows the Fev+ progenitors with only ~0.005 Delta fate probability. Taken at face value this looks alarming: if essentially no progenitor is Delta-bound, where do the 70 mature Delta cells come from? The answer is that the coarse `clusters` annotation hides the structure. The Delta-committed progenitors are a small *sub-population* inside the Fev+ pool, and averaging fate probability across *all* Fev+ cells dilutes their signal almost to zero.

This dataset includes a finer annotation, `clusters_fine`, which splits the Fev+ pre-endocrine pool into five fate-specific sub-populations: `Fev+ Beta`, `Fev+ Alpha`, `Fev+ Pyy`, `Fev+ Delta`, and `Fev+ Epsilon`. CellRank’s own fate-probability tutorial uses this finer annotation to reveal the Delta-committed precursors, and we can do the same:

```bash
# Delta commitment is concentrated in the Fev+ Delta sub-population.
# It is averaged away in the coarse heatmap but clear with the fine annotation.
fev_fine = ["Fev+ Beta", "Fev+ Alpha", "Fev+ Pyy", "Fev+ Delta", "Fev+ Epsilon"]

cr.pl.aggregate_fate_probabilities(
    adata,
    mode="violin",
    lineages=["Delta"],
    cluster_key="clusters_fine",
    clusters=fev_fine,
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/FevDelta.png?resize=1024%2C840&ssl=1)

**Output:** A violin plot with five violins, one per Fev+ sub-population, each showing the distribution of Delta fate probability within that sub-population. The `Fev+ Delta` violin should sit clearly higher than the others — those cells are genuinely Delta-committed — while `Fev+ Beta`, `Fev+ Alpha`, and the rest stay near zero.

> 

**The lesson.** Rare-fate commitment is real but concentrated. When you aggregate fate probabilities over a coarse cluster annotation, a small committed sub-population gets averaged away, and a perfectly healthy rare lineage can look like it has near-zero probability everywhere. If a fate you expect looks suspiciously close to zero in a per-cluster heatmap, re-aggregate over a finer annotation before concluding the signal is absent. The mean fate probability heatmap is the right tool for the *dominant* developmental tendency; sub-population analysis is the right tool for *rare* lineages.

### 7.3 — Per-cluster fate probability distributions

The heatmap shows means, which can hide variability. Violin plots show the full distribution within each cluster.

```python
fig, axes = plt.subplots(
    len(g.fate_probabilities.names), 1,
    figsize=(14, 4 * len(g.fate_probabilities.names))
)

for ax, fate_name in zip(axes, g.fate_probabilities.names):
    sns.violinplot(
        data=fate_df,
        x="cell_type",
        y=fate_name,
        palette="Set2",
        inner="box",
        ax=ax
    )
    ax.set_title(f"Fate Probability Distribution: {fate_name}")
    ax.set_xlabel("Cell Type")
    ax.set_ylabel("Fate Probability")
    ax.set_xticklabels(ax.get_xticklabels(), rotation=30, ha="right")

plt.tight_layout()
plt.savefig("figures/fate_probability_by_cluster.pdf")
plt.show()
```

**Output:** Four stacked violin plots, one per fate. Each violin shows the distribution of fate probability values within one annotated cluster.

**What to look for:** mature clusters should show a violin that peaks at the high end of their own-fate panel (close to 1.0) and at the low end of the other-fate panels. Progenitor clusters should show wider distributions, reflecting that progenitor cells are at different stages of commitment. The Beta-fate panel will show the most striking pattern — progenitor violins concentrate around 0.7, not 0.25, confirming the Beta bias we saw in the heatmap.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/fate_probability_by_cluster.png?resize=902%2C1024&ssl=1)

### 7.4 — Progenitor commitment: Ngn3 low vs Ngn3 high

A central biological question in pancreatic development is whether fate commitment changes as progenitors progress from the Ngn3 low EP stage (early) to the Ngn3 high EP stage (later). We can answer this directly with a Mann-Whitney U test on the fate probability distributions:

```python
from scipy.stats import mannwhitneyu

progenitor_df = fate_df[
    fate_df["cell_type"].isin(["Ngn3 low EP", "Ngn3 high EP"])
].copy()

results = []
for fate_name in g.fate_probabilities.names:
    low_vals  = progenitor_df.loc[progenitor_df["cell_type"] == "Ngn3 low EP",  fate_name]
    high_vals = progenitor_df.loc[progenitor_df["cell_type"] == "Ngn3 high EP", fate_name]

    stat, pval = mannwhitneyu(low_vals, high_vals, alternative="two-sided")
    results.append({
        "terminal_state":  fate_name,
        "n_Ngn3_low_EP":   len(low_vals),
        "n_Ngn3_high_EP":  len(high_vals),
        "median_low_EP":   round(low_vals.median(), 3),
        "median_high_EP": round(high_vals.median(), 3),
        "p_value":         pval
    })

results_df = pd.DataFrame(results).sort_values("p_value")
results_df["significant"] = results_df["p_value"] < 0.05

print("Fate commitment shift: Ngn3 low EP vs Ngn3 high EP")
print(results_df[["terminal_state", "median_low_EP", "median_high_EP",
                  "p_value", "significant"]].to_string(index=False))
```

**Output:**

```
Fate commitment shift: Ngn3 low EP vs Ngn3 high EP
terminal_state  median_low_EP  median_high_EP      p_value  significant
          Beta          0.733           0.733 5.251340e-11         True
       Epsilon          0.125           0.124 6.735781e-11         True
         Delta          0.005           0.004 1.105621e-09         True
         Alpha          0.138           0.138 3.776101e-07         True
```

**What you are seeing:** All four fates show statistically significant differences between the two progenitor stages (`p_value` well below 0.05). But notice that the *median values barely change* — Beta is 0.733 vs 0.733, Alpha is 0.138 vs 0.138, Epsilon is 0.125 vs 0.124, Delta is 0.005 vs 0.004.

**How to interpret:** the very small p-values combined with nearly identical medians are a textbook example of the difference between **statistical significance** and **biological significance**. With 138 Ngn3 low EP cells and 529 Ngn3 high EP cells, the test has enough power to detect tiny differences in the *distributions* (perhaps a slightly different spread or skew), even when the central tendencies are essentially the same. The biological signal here is weak: the Ngn3 low to Ngn3 high transition does *not* substantially shift the fate probability distribution at E15.5. Progenitors at both stages are similarly biased toward Beta, with similar small Alpha and Epsilon tails.

This is actually a meaningful biological finding — it tells you that the Ngn3 low -> high transition is *not* the major commitment decision in this dataset. The decisive commitment likely happens later, between Fev+ and the mature endocrine fates.

> 

**Always read p-values together with effect sizes.** A small p-value tells you the effect is *not zero*. It does not tell you the effect is large. The medians here are nearly identical, so the practical effect is small even though the p-value is tiny.

### 7.5 — Combined four-panel UMAP using scanpy

```python
# Also store fate probabilities with clean column names
for i, fate_name in enumerate(g.fate_probabilities.names):
    clean_name = fate_name.replace(" ", "_")
    adata.obs[f"fate_{clean_name}"] = g.fate_probabilities.X[:, i]

fig, axes = plt.subplots(1, len(fate_names), figsize=(6 * len(fate_names), 5))
fate_cols = [f"fate_{n.replace(' ', '_')}" for n in g.fate_probabilities.names]

for ax, col, name in zip(axes, fate_cols, g.fate_probabilities.names):
    sc.pl.umap(adata, color=col, ax=ax, show=False, title=f"Fate: {name}",
               color_map="viridis", vmin=0, vmax=1)

plt.tight_layout()
plt.savefig("figures/fate_probabilities_combined_umap.pdf")
plt.show()
```

**Output:** The same four UMAPs as in Step 7.1, but rendered with scanpy’s `viridis` colormap instead of scvelo’s `RdYlBu_r`. The viridis version may be easier to read for some readers — yellow indicates high probability, dark purple indicates low. Both views show the same underlying data; pick whichever you find more readable.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/fate_probabilities_combined_umap.png?resize=1024%2C208&ssl=1)

---

## Step 8: Identify Lineage Driver Genes

A lineage driver is a gene whose expression correlates with the fate probability for a specific terminal state. A high-correlation gene goes up in cells heading toward that fate — even in early progenitors that have not yet matured. This is biologically important: it tells us about the molecular programmes that *commit* a cell, not just the markers that identify it once it has arrived.

### 8.1 — Compute lineage drivers

```python
drivers = g.compute_lineage_drivers(
    cluster_key="clusters",
    return_drivers=True
)

print("Lineage driver computation complete.")
print("Driver table shape:", drivers.shape)
print("\nColumn names:")
print(drivers.columns.tolist())
print("\nFirst rows:")
print(drivers.head())
```

**Output:**

```
Lineage driver computation complete.
Driver table shape: (5974, 20)

Column names:
['Alpha_corr', 'Alpha_pval', 'Alpha_qval', 'Alpha_ci_low', 'Alpha_ci_high',
 'Beta_corr', 'Beta_pval', 'Beta_qval', 'Beta_ci_low', 'Beta_ci_high',
 'Epsilon_corr', 'Epsilon_pval', 'Epsilon_qval', 'Epsilon_ci_low', 'Epsilon_ci_high',
 'Delta_corr', 'Delta_pval', 'Delta_qval', 'Delta_ci_low', 'Delta_ci_high']
```

**What you are seeing:** A 5,974 x 20 driver table — one row per gene, five metrics for each of the four fates (correlation `_corr`, p-value `_pval`, q-value `_qval`, and two confidence-interval bounds). The correlation values are what we will sort by.

> 

**Important: the column names use underscores, not spaces.** Older CellRank documentation and older blog posts sometimes refer to columns like `"Alpha corr"` (with a space). Recent CellRank versions use `"Alpha_corr"` (with an underscore). If you see code that does `corr_col = f"{fate_name} corr"` and produces no error but no output either, this is the cause — the column name does not match and the code silently skips. The Step 8.2 code below uses keyword search rather than literal name matching, which works regardless of the spacing convention.

### 8.2 — Top driver genes per fate

```python
# Search by fate name and metric keyword to handle column naming variations
for fate_name in g.fate_probabilities.names:
    corr_cols = [c for c in drivers.columns if fate_name in c and "corr" in c.lower()]
    qval_cols = [c for c in drivers.columns if fate_name in c and
                 ("qval" in c.lower() or "pval" in c.lower())]

    if not corr_cols:
        print(f"\nNo correlation column found for '{fate_name}'")
        continue

    corr_col = corr_cols[0]
    display_cols = [corr_col] + (qval_cols[:1] if qval_cols else [])

    top = (drivers[display_cols]
           .dropna()
           .sort_values(corr_col, ascending=False)
           .head(20))

    print(f"\nTop 20 lineage drivers for: {fate_name}")
    print(top.to_string())
```

**Output (showing top 5 per fate; your run will print 20 each):**

```
Top 20 lineage drivers for: Alpha
         Alpha_corr     Alpha_pval
index
Gcg        0.865351   0.000000e+00
Irx2       0.516397  1.599299e-181
Peg10      0.514038  1.640793e-179
Wnk3       0.481947  7.542082e-154
Tmsb15l    0.455927  3.483483e-135

Top 20 lineage drivers for: Beta
          Beta_corr      Beta_pval
index
Nkx6-1     0.511005  5.962195e-177
Pdx1       0.472944  3.473355e-147
Nnat       0.459283  1.717748e-137
Gng12      0.453715  1.114821e-133
Ins2       0.424551  6.020294e-115

Top 20 lineage drivers for: Epsilon
           Epsilon_corr   Epsilon_pval
index
Ghrl           0.820704   0.000000e+00
Anpep          0.486874  1.364318e-157
Gm11837        0.475151  8.468793e-149
Maged2         0.402662  3.661838e-102
Mboat4         0.396718   7.215533e-99

Top 20 lineage drivers for: Delta
               Delta_corr     Delta_pval
index
Sst              0.806064   0.000000e+00
Hhex             0.524735  8.864939e-189
Mest             0.401430  1.788586e-101
Arg1             0.398802  5.148729e-100
Igfbp5           0.361831   5.928258e-81
```

**This is the second most important output of the tutorial. The biology validates everything we have done.**

**Alpha drivers:** ***Gcg*** is the top driver (correlation 0.87) — this gene encodes glucagon, the defining secretory product of alpha cells. ***Irx2*** (correlation 0.52) is a homeodomain transcription factor expressed in committed alpha progenitors. ***Peg10*** is a paternally expressed gene known to be alpha-enriched. The fact that *Gcg* dominates with correlation 0.87 — much higher than any other gene — tells you the alpha lineage commitment is very tightly tracked by glucagon expression in this dataset.

**Beta drivers:** ***Nkx6-1*** (0.51) is the master transcription factor for beta cell identity. ***Pdx1*** (0.47) is the pancreatic master regulator; without Pdx1, no pancreas forms at all. ***Ins2*** (0.42) is one of the two insulin genes in mouse. ***Ins1*** appears further down the list (0.35). Notice that *Nkx6-1* and *Pdx1* score higher than *Ins1* and *Ins2* — this is exactly what lineage driver analysis is designed to find. Master transcription factors are upregulated *earlier* in commitment than the hormone genes they eventually turn on. If we just looked at marker genes of mature beta cells, we would expect insulin to be at the top. But fate probability correlation picks up commitment signal, which is captured better by upstream regulators. This is a textbook validation of how lineage drivers differ from cell-type markers.

**Delta drivers:** ***Sst*** (0.81) is the somatostatin gene — the defining product of delta cells. ***Hhex*** (0.52) is the master transcription factor for delta cell identity. ***Igfbp5*** is a known delta-enriched gene. The pattern is the same as alpha: hormone gene dominates, transcription factor second, other lineage-enriched genes following.

**Epsilon drivers:** ***Ghrl*** (0.82) is the ghrelin gene, the defining product of epsilon cells. ***Anpep*** is a known epsilon-enriched aminopeptidase. ***Mboat4*** is the ghrelin acyl-transferase — the enzyme that processes ghrelin into its active form. Finding the gene that *modifies* the defining hormone right next to the hormone gene itself is a strong sign the analysis is biologically coherent.

**This concordance with established pancreatic development biology validates the entire CellRank analysis.** The method has independently recovered the correct lineage-specific gene programmes — hormone genes, master transcription factors, processing enzymes — without using any prior knowledge of cell identity.

### 8.3 — Visualise driver genes on the UMAP

```
for fate_name in g.fate_probabilities.names:
    # Use underscore, not space -- this is the actual column name in CellRank 2
    corr_col = f"{fate_name}_corr"

    if corr_col not in drivers.columns:
        continue

    top4 = (drivers[corr_col]
            .dropna()
            .sort_values(ascending=False)
            .head(4)
            .index.tolist())

    sc.pl.umap(
        adata,
        color=top4,
        ncols=4,
        cmap="RdYlBu_r",
        title=[f"{gene} ({fate_name[:5]})" for gene in top4],
        show=False,
        save=f"_lineage_drivers_{fate_name.replace(' ', '_')}.png"
    )
```

**Output:** Four PNG files saved to `figures/`, one per fate, each showing the expression of the top 4 driver genes for that fate on the UMAP.

**What to look for:** the top drivers for each fate should be highly expressed in the corresponding mature cluster and progressively less expressed as you move toward the progenitor pool. For Alpha drivers, you should see the Alpha cluster light up in red. For Beta drivers, the Beta cluster should dominate, but you should also see some expression in Fev+ cells that are starting to commit to the beta lineage — this is what makes them drivers rather than just markers.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/umap_lineage_drivers-e1779408332514-1024x935.png?resize=1024%2C935&ssl=1)

### 8.4 — Driver expression vs fate probability scatter plots

A scatter plot of driver gene expression against the corresponding fate probability is one of the most direct ways to see commitment in action: cells in the upper right have both high driver expression and high fate probability, while early progenitors cluster near the bottom left.

```python
fig, axes = plt.subplots(1, len(g.fate_probabilities.names),
                          figsize=(6 * len(g.fate_probabilities.names), 5))

top_drivers = {
    "Alpha":   "Gcg",
    "Beta":    "Ins2",
    "Delta":   "Sst",
    "Epsilon": "Ghrl",
}

for ax, fate_name in zip(axes, g.fate_probabilities.names):
    gene = top_drivers.get(fate_name)
    if gene is None or gene not in adata.var_names:
        ax.set_visible(False)
        continue

    fate_col = f"fate_{fate_name.replace(' ', '_')}"
    expr = adata[:, gene].X

    if hasattr(expr, "toarray"):
        expr = expr.toarray().flatten()
    else:
        expr = np.array(expr).flatten()

    ax.scatter(
        expr,
        adata.obs[fate_col],
        alpha=0.3,
        s=5,
        c=adata.obs[fate_col],
        cmap="viridis"
    )
    ax.set_xlabel(f"{gene} expression")
    ax.set_ylabel(f"{fate_name} fate probability")
    ax.set_title(f"{gene} vs {fate_name} fate")

plt.tight_layout()
plt.savefig("figures/driver_gene_fate_scatter.pdf")
plt.show()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/driver_gene_fate_scatter.png?resize=1024%2C208&ssl=1)

**Output:** Four scatter plots, one per fate. Each dot is a cell.

**What to look for:** the plots should show a clear positive trend — cells with high driver expression are also the cells with high fate probability. The Beta panel (Ins2 vs Beta fate) typically shows the broadest distribution because beta commitment is the dominant fate in the dataset. The Epsilon panel (Ghrl vs Epsilon fate) should show a tight L-shape: most cells have near-zero Ghrl and near-zero Epsilon probability, while a small group of cells in the upper right have high values of both. Progenitor cells cluster near the bottom left across all four panels.

---

## Step 9: Gene Expression Trends and Cross-Validation

### 9.1 — Dot plot of driver gene expression by cell type

```
for fate_name in g.fate_probabilities.names:
    # Use underscore, not space -- this is the actual column name in CellRank 2
    corr_col = f"{fate_name}_corr"
    qval_col = f"{fate_name}_qval"

    if corr_col not in drivers.columns:
        continue

    sig = drivers[drivers[qval_col] < 0.05].copy()
    top_pos = sig[corr_col].sort_values(ascending=False).head(15).index.tolist()
    top_neg = sig[corr_col].sort_values(ascending=True).head(5).index.tolist()
    genes_to_plot = top_pos + top_neg

    sc.pl.dotplot(
        adata,
        var_names=genes_to_plot,
        groupby="clusters",
        colorbar_title="Mean expr.",
        title=f"Top lineage drivers: {fate_name}",
        show=False,
        save=f"_dotplot_drivers_{fate_name.replace(' ', '_')}.pdf"
    )
```

**Output:** Four dot plots saved to `figures/`, one per fate. Each row of dots represents one annotated cell cluster; each column is a gene (15 positive drivers, then 5 negative drivers for contrast). Dot size shows the fraction of cells in that cluster expressing the gene; dot colour shows the mean expression level.

**What to look for:** for each fate, the top positive drivers should show large, dark dots specifically in the matching cluster — e.g., the Beta drivers (Nkx6-1, Pdx1, Ins1, Ins2, …) should be most strongly expressed in the Beta cluster, weaker in Fev+ (the immediate precursor), and weakest in Ngn3 low EP and the off-target endocrine clusters. The negative drivers should show the opposite pattern — strong expression in clusters *other* than the target fate.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/dotplot_drivers.png?resize=1024%2C678&ssl=1)

### 9.2 — Fate probability vs Palantir pseudotime

The Bastidas-Ponce dataset includes Palantir pseudotime in `adata.obs["palantir_pseudotime"]`. Palantir is an independent algorithm that estimates pseudotime from diffusion maps, without using RNA velocity. Comparing fate probabilities against an independent pseudotime measure is a strong cross-validation: if both methods capture the same biology, they should agree on the directionality of differentiation.

```python
fig, axes = plt.subplots(1, len(g.fate_probabilities.names),
                          figsize=(6 * len(g.fate_probabilities.names), 5))

for ax, fate_name in zip(axes, g.fate_probabilities.names):
    fate_col = f"fate_{fate_name.replace(' ', '_')}"

    sc.pl.scatter(
        adata,
        x="palantir_pseudotime",
        y=fate_col,
        color="clusters",
        ax=ax,
        show=False,
        title=f"Pseudotime vs {fate_name} Fate"
    )
    ax.set_xlabel("Palantir Pseudotime")
    ax.set_ylabel(f"{fate_name} Fate Probability")

plt.tight_layout()
plt.savefig("figures/pseudotime_vs_fate.pdf")
plt.show()
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/05/pseudotime_vs_fate.png?resize=1024%2C209&ssl=1)

**Output:** Four scatter plots, one per fate. The x-axis is Palantir pseudotime (low = early progenitor, high = mature); the y-axis is the CellRank fate probability for the corresponding fate. Each dot is a cell, coloured by its cluster annotation.

**What to look for:** for each fate, the cells of the matching mature cluster should appear at high pseudotime AND high fate probability — the upper right of the plot. Progenitor cells (Ngn3 low EP, Ngn3 high EP, Fev+) should appear at low to mid pseudotime, with the fate probability rising as pseudotime advances. This monotonic relationship between pseudotime and fate probability for the matching lineage is what makes the cross-validation convincing — two independent methods (Palantir diffusion-map pseudotime and CellRank velocity-based fate probabilities) agree on the same developmental ordering.

---

## Best Practices and Common Pitfalls

### Best practices

**1. Always inspect the coarse_T before designating terminal states.** Print `g.coarse_T` after computing macrostates and check both the diagonal (self-transition probabilities, which indicate stability) and the off-diagonal values. The coarse_T is the most useful diagnostic in the entire workflow.

**2. Verify the biological identity of each macrostate.** Use `pd.crosstab()` on macrostate assignments and cell type annotations to confirm that each macrostate corresponds to a meaningful biological population before calling it terminal or initial.

**3. Cross-validate fate probabilities against an independent pseudotime estimate.** If the dataset has pseudotime from Palantir, Monocle 3 (covered in [Part 7](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-trajectory-and-pseudotime-analysis-using-monocle-3/)), or scVelo latent time (from [Part 13](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-13-rna-velocity-analysis-with-scvelo/)), plot fate probability against pseudotime. Concordance between independent methods substantially increases confidence in the result.

**4. The validation heatmap (Step 7.2) is the single most important plot.** A clean diagonal in the mature-cell rows is what tells you the analysis worked. Progenitor rows may show strong biases toward one fate (here, Beta) — this is biology, not error, as long as the mature rows look right.

**5. Read p-values together with effect sizes.** The Mann-Whitney test in Step 7.4 returned p-values around 1e-11 but medians differed by less than 0.001. Both pieces of information matter. A small p-value tells you a difference exists; the effect size tells you whether it matters.

**6. Use lineage drivers for hypothesis generation, not causal claims.** The driver genes returned by `compute_lineage_drivers()` are correlates, not causal factors. Use them to prioritise candidates for experimental follow-up. Do not interpret correlation as mechanism.

**7. Demonstrate both the automatic and semi-automatic terminal state routes.** `predict_terminal_states()` is the first thing to try because it requires no biological prior. When a biologically expected terminal state is missing — as Delta is here — `set_terminal_states(states=[...])` is the canonical fallback, not an ad hoc workaround. This is the pattern the CellRank team themselves use for this dataset.

**8. Verify your dataset has a genuine developmental hierarchy before running CellRank.** Fate probabilities are only biologically interpretable when RNA velocity reflects directional state transitions between cell types. The PBMC section at the end of this tutorial explains the failure mode that arises when this assumption is violated.

### Common pitfalls

*mature*`n_states``g.coarse_T``n_states`*mature*`predict_terminal_states()``g.set_terminal_states(states=["Alpha", "Beta", "Epsilon", "Delta"])``KeyError``set_terminal_states``Delta_1``n_states``adata.obs["macrostates_fwd"].cat.categories.tolist()``n_states``n_states``WARNING: Using N components would split a block of complex conjugate eigenvalues``compute_schur``n_components``n_components``f"{fate_name} corr"``Alpha_corr``Alpha corr``if corr_col not in drivers.columns: continue``f"{fate_name}_corr"``ValueError: Sparse implementation is only available for method='krylov'``conda install -c conda-forge petsc petsc4py slepc slepc4py --yes`**Not supported on macOS Apple Silicon (M1/M2/M3/M4)**`AttributeError: 'GPCCA' object has no attribute 'compute_terminal_states'``g.set_terminal_states(states=[...])``g.predict_terminal_states()``AttributeError: 'GPCCA' object has no attribute 'plot_terminal_states'``adata.obs["ts"] = g.terminal_states``scv.pl.scatter(adata, color="ts")``ValueError: Found N overlapping cells between initial and terminal states``predict_initial_states``allow_overlap=True``predict_initial_states()``ValueError: Length of values does not match length of index``Lineage[:, i].squeeze()``np.array(g.fate_probabilities)[:, i]`

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| One fate absorbs >85% probability across all  cell types | too high causing coarse_T instability between endocrine rows, OR dataset lacks genuine developmental velocity | Print the full  — large negatives in the endocrine rows (Alpha, Beta, Delta, Epsilon) confirm instability; reduce . If coarse_T endocrine rows are clean but dominance persists, the dataset may not be appropriate for CellRank |
| Progenitor cells all biased toward one fate | Biology, not a bug — one fate is the dominant developmental outcome at the dataset’s time point (here, Beta at E15.5) | No fix needed. Confirm by checking that  cells of each type still have their own fate dominant in the heatmap |
| returns 3 of 4 endocrine fates (Delta is missing) | Default stability threshold (0.96) is higher than Delta’s self-transition probability because Delta cells transition into Beta cells with non-trivial probability | Use the semi-automatic route: . This is the canonical CellRank pattern for this dataset |
| from  because GPCCA produced a suffixed name (e.g., ) | At the current , multiple macrostates share the same dominant cell type and GPCCA disambiguated with suffixes | Print  and replace the offending name with the exact string from your list; or use the defensive helper below |
| Crosstab in Step 5.3 shows a terminal state with cells from multiple unrelated annotated clusters | Macrostate at current  is mixed — not a clean separation of the biology | Bump  higher and re-check stability and crosstab. If the issue persists across reasonable k values, the macrostate may not actually be biologically terminal |
| from | Requested  falls inside a 2×2 complex conjugate eigenvalue block | Benign. CellRank auto-bumps  by 1 to keep the conjugate pair intact. Downstream is unaffected |
| Driver code with  (space) produces no output and no error | Recent CellRank versions use underscores () not spaces () in driver column names; the  line silently skips | Change the format string to use an underscore: . Or use the keyword-search approach shown in Step 8.2 |
|  | petsc4py / slepc4py not installed | (Linux / WSL2 only).  — run the workflow inside a Linux container or on a Linux HPC node instead |
|  | Renamed in CellRank 2 | Use  or |
|  | Plot methods were removed from GPCCA in CellRank 2 | Assign , then plot with |
|  | finds candidate boundary cells that overlap with already-designated terminal states | Add  to |
| when plotting fate probabilities with scvelo | returns a pandas-indexed object with misaligned index | Use  to strip the Lineage metadata |
| Fate probabilities are uniform at ~0.25 for all cells | Transition matrix is near-random; velocity does not encode directional flow | The dataset may lack a genuine developmental hierarchy. CellRank is not appropriate for terminally differentiated cell populations like PBMCs |

### Defensive helper for suffixed macrostate names

If you want code that works regardless of whether GPCCA produces `Delta` or `Delta_1` — useful when iterating across `n_states` values — this helper resolves the four endocrine targets defensively:

```python
macrostate_names = adata.obs["macrostates_fwd"].cat.categories.tolist()
targets = ["Alpha", "Beta", "Delta", "Epsilon"]

resolved, missing = [], []
for t in targets:
    match = next((s for s in macrostate_names if s.startswith(t)), None)
    if match is None:
        missing.append(t)
    else:
        resolved.append(match)

if missing:
    raise ValueError(
        f"No macrostate starts with: {missing}. "
        f"Available macrostates: {macrostate_names}. "
        f"Increase n_states in g.compute_macrostates() to resolve rare lineages."
    )

g.set_terminal_states(states=resolved)
print(g.terminal_states.value_counts())
```

This is a drop-in replacement for the `set_terminal_states(states=["Alpha", "Beta", "Epsilon", "Delta"])` call in Step 5.2 with one extra safety net: if a lineage is genuinely missing from the macrostate list, you get a clear `ValueError` naming what is absent rather than a cryptic `KeyError` mid-call.

---

## Why Not the GSE174609 PBMC Dataset? — The Full Diagnostic Story

This section documents what happened when we tried to apply CellRank to the GSE174609 PBMC data from Parts 1–13 of this series, and explains why the result was uninterpretable.

### What we observed

The fate probability heatmap looked like this for every `n_states` we tested (4, 6, 7):

```
                     Classical Monocytes  NK cells  B cells
B cells                            0.002     0.074    0.924
CD4+ T cells                       0.003     0.106    0.891
Classical Monocytes                0.071     0.078    0.851
NK cells                           0.003     0.144    0.853
T cells                            0.003     0.112    0.885
```

Every cell type, including Classical Monocytes and NK cells, showed 85–92% fate probability for B cells. We investigated systematically — the coarse transition matrix was numerically clean, stability scores were all above 0.93, terminal states were correctly designated, and varying `n_states` or `n_cells` did not change the pattern. The problem was not a software bug or a parameter choice. It was the data.

### The root cause

RNA velocity measures the ratio of unspliced to spliced RNA. When this ratio is higher than the steady-state value for a gene, the gene is being upregulated — the cell is transcriptionally “moving” toward higher expression of that gene. In developing tissues like the pancreas dataset above, these spliced/unspliced imbalances reflect real-time fate commitment: progenitor genes turn off, lineage-specific genes turn on, and the velocity arrow captures the direction of that switching.

In peripheral blood mononuclear cells, no such switching is happening. A mature NK cell is not becoming a Classical Monocyte. The spliced/unspliced imbalances that scVelo detects in PBMCs reflect cell cycle activity, activation states, and stress responses — all real biological dynamics, but none of them encode directional flow from one mature immune cell type toward another.

When the velocity arrows do not point meaningfully between cell types, the CellRank transition matrix reflects the topology of the kNN graph rather than biology. The Markov chain converges on whichever absorbing state is most accessible from most starting points in the kNN structure — which happened to be B cells in the GSE174609 dataset.

### When CellRank works (and when to choose a different tool)

CellRank produces biologically interpretable results when the dataset captures an ongoing developmental or differentiation process: embryonic development (the pancreas dataset here, brain, heart), bone marrow hematopoiesis, in vitro differentiation time courses, reprogramming experiments, organoid differentiation.

For the research question that motivated Parts 1–13 of this series — how does periodontal disease and its treatment alter the PBMC transcriptome — the appropriate tools are:

- **Pseudobulk differential expression with DESeq2** — most appropriate for condition comparison; covered in the bulk RNA-seq series on NGS101.com

- **Gene regulatory network inference (WGCNA, MEGENA)** — identify co-expression modules altered by treatment

- **Differential cell type abundance (Milo)** — test for abundance differences in cellular neighbourhoods between conditions

- **Cell communication (CellChat)** — analyse condition-dependent shifts in signalling pathways; covered in [Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/)

The right question for CellRank is “*where are these cells going developmentally?*” — not “*how does a treatment change these already-differentiated cells?*“

---

## Key takeaways

- CellRank requires velocity data that encodes genuine developmental dynamics; it is not appropriate for already-differentiated cell populations

- The coarse_T diagonal (stability) is the most useful diagnostic; small negative off-diagonals between *progenitor* macrostates are not a problem as long as the *endocrine* macrostate rows are clean

- The canonical terminal state workflow is *try automatic first, override semi-automatically* — not name-matching from the start

- Fate probabilities are absorption probabilities of an absorbing Markov chain, a precise mathematical concept, not an arbitrary score

- Progenitor fate biases (here, toward Beta) often reflect the developmental window of the sample, not algorithmic error

- Lineage drivers are correlation-based: they predict commitment trajectory but do not establish mechanism. The fact that *Nkx6-1* and *Pdx1* score higher than *Ins1*/*Ins2* shows that lineage driver analysis prioritises upstream transcription factors over downstream marker genes

- The diagonal structure of the mean fate probability heatmap (for mature cells) is the single most important validation of a successful CellRank run

- Aggregating fate probabilities over a coarse cluster annotation averages away small committed sub-populations; a rare fate that looks near-zero in the heatmap (here, Delta) may simply need a finer annotation (`clusters_fine`) to become visible. Use the heatmap for dominant tendencies and sub-population analysis for rare lineages

---

## References

1. Lange M, Bergen V, Klein M, et al. CellRank for directed single-cell fate mapping. *Nature Methods*. 2022;19(2):159-170. doi:10.1038/s41592-021-01346-6

2. Weiler P, Lange M, Klein M, Pe’er D, Theis FJ. CellRank 2: unified fate mapping in multiview single-cell data. *Nature Methods*. 2024;21(7):1196-1205. doi:10.1038/s41592-024-02303-9

3. Bastidas-Ponce A, Tritschler S, Dony L, et al. Comprehensive single cell mRNA profiling reveals a detailed roadmap for pancreatic endocrinogenesis. *Development*. 2019;146(12):dev173849. doi:10.1242/dev.173849 [Dataset used in this tutorial]

4. Bergen V, Lange M, Peidli S, Wolf FA, Theis FJ. Generalizing RNA velocity to transient cell states through dynamical modeling. *Nature Biotechnology*. 2020;38(12):1408-1414. doi:10.1038/s41587-020-0591-3

5. La Manno G, Soldatov R, Zeisel A, et al. RNA velocity of single cells. *Nature*. 2018;560(7719):494-498. doi:10.1038/s41586-018-0414-6

6. Wolf FA, Angerer P, Theis FJ. SCANPY: large-scale single-cell gene expression data analysis. *Genome Biology*. 2018;19(1):15. doi:10.1186/s13059-017-1382-0

7. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. *Journal of Translational Medicine*. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [GSE174609 — Parts 1-13 of this series]

8. Setty M, Kiseliovas V, Levine J, Gayoso A, Mazutis L, Pe’er D. Characterization of cell fate probabilities in single-cell data with Palantir. *Nature Biotechnology*. 2019;37(4):451-460. doi:10.1038/s41587-019-0068-4 [Palantir pseudotime used in Step 9]

9. Reuter B, Fackeldey K, Weber M. Generalized Markov modeling of nonreversible molecular kinetics. *Journal of Chemical Physics*. 2019;150(17):174103. doi:10.1063/1.5064530 [GPCCA algorithm]

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
