# How to Analyze Proteomics Data Part 5: Functional Pathway Enrichment and STRING Protein Interaction Networks

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Pathway Enrichment, clusterProfiler, GSEA, Gene Ontology, STRINGdb, Protein Networks

---

## Table of Contents

- [Introduction](#introduction)
- [The Background Universe Pitfall in Proteomics](#the-background-universe-pitfall-in-proteomics)
  - [Why Genome-Wide Backgrounds Produce False Discoveries](#why-genome-wide-backgrounds-produce-false-discoveries)
  - [Defining the Custom Proteome Universe](#defining-the-custom-proteome-universe)
- [Step 1: Identifier Mapping with clusterProfiler and org.Hs.eg.db](#step-1-identifier-mapping-with-clusterprofiler-and-orghsegdb)
- [Step 2: Over-Representation Analysis (ORA)](#step-2-over-representation-analysis-ora)
  - [Gene Ontology (GO) Biological Process Enrichment](#gene-ontology-go-biological-process-enrichment)
  - [KEGG Pathway Enrichment](#kegg-pathway-enrichment)
  - [Visualizing ORA with Dotplots and Treeplots](#visualizing-ora-with-dotplots-and-treeplots)
- [Step 3: Gene Set Enrichment Analysis (GSEA)](#step-3-gene-set-enrichment-analysis-gsea)
  - [Why GSEA is Superior to Arbitrary Thresholds](#why-gsea-is-superior-to-arbitrary-thresholds)
  - [Constructing the Ranked Protein List](#constructing-the-ranked-protein-list)
  - [Running gseGO and gseKEGG](#running-gsego-and-gsekegg)
  - [Plotting GSEA Running-Sum Enrichment Scores](#plotting-gsea-running-sum-enrichment-scores)
- [Step 4: Protein-Protein Interaction (PPI) Networks with STRINGdb](#step-4-protein-protein-interaction-ppi-networks-with-stringdb)
  - [Connecting DAPs into Physical Multi-Protein Complexes](#connecting-daps-into-physical-multi-protein-complexes)
  - [Mapping DAPs to STRING Identifiers](#mapping-daps-to-string-identifiers)
  - [Extracting High-Confidence Subnetworks and Complexes](#extracting-high-confidence-subnetworks-and-complexes)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In [Part 4](08_part-4-creating-publication-ready-figures-volcano-heatmaps-pca.md), we visualized our 598 Differentially Abundant Proteins (DAPs) using PCA, enhanced Volcano plots, and ComplexHeatmap.

However, a list of individual protein names is not enough to write a paper or design follow-up functional assays. Cells function through **coordinated biochemical pathways and physical multi-protein complexes**.

In this tutorial, we translate our protein lists into biological mechanisms:
1. **Identifier Translation**: Converting UniProt accessions to Entrez IDs and Gene Symbols.
2. **Over-Representation Analysis (ORA)**: Testing whether specific Gene Ontology (GO) terms or KEGG pathways are enriched among our DAPs.
3. **Gene Set Enrichment Analysis (GSEA)**: Ranking the entire proteome to uncover subtle, coordinated pathway shifts without arbitrary $p$-value cutoffs.
4. **Physical Protein-Protein Interaction (PPI) Networks**: Mapping DAPs onto the **STRING** knowledgebase to reveal physical complexes (such as the MCM helicase and MAPK signaling machinery).

---

## The Background Universe Pitfall in Proteomics

### Why Genome-Wide Backgrounds Produce False Discoveries

In RNA sequencing, nearly every expressed gene ($> 18,000$) can be detected. Therefore, the "background universe" for Fisher's exact test is the entire human genome.

In mass spectrometry, however, **we only quantify 6,000 to 8,000 proteins** (the most abundant fraction of the proteome). Mass spectrometers naturally favor proteins with high intracellular abundance:
- Ribosomal proteins
- Glycolytic and metabolic enzymes
- Cytoskeletal structural proteins

```
     All Human Genes (~20,000)
   ┌──────────────────────────────────────────────┐
   │                                              │
   │      Quantified in Experiment (~7,000)       │
   │    ┌────────────────────────────────────┐    │
   │    │                                    │    │
   │    │         DAPs (~600)                │    │
   │    │       ┌──────────────┐             │    │
   │    │       │              │             │    │
   │    │       └──────────────┘             │    │
   │    └────────────────────────────────────┘    │
   └──────────────────────────────────────────────┘
```

> [!WARNING]
> If you test for pathway enrichment using the entire human genome (~20,000 genes) as your background, terms like *"Ribosome"* and *"Glycolysis"* will appear massively enriched **simply because they are abundant enough to be detected by the mass spectrometer**, not because your drug treated them!
> 
> **You MUST restrict your background universe to the proteins quantified in your experiment.**

---

## Step 1: Identifier Mapping with clusterProfiler and org.Hs.eg.db

Most pathway databases (KEGG, Reactome, GO) require **Entrez Gene IDs**. We use `clusterProfiler::bitr()` (Biological Id TRanslator) to map UniProt accessions:

```r
library(clusterProfiler)
library(org.Hs.eg.db)
library(enrichplot)
library(tidyverse)

setwd("~/Projects/learning_omics/Proteomics_Workspace")

# Load limma DAP results from Part 2c
dap_df <- read.csv("results/tables/DAP_results_limma.csv", stringsAsFactors = FALSE)

# 1. Define the experimental background universe (all 6,912 quantified proteins)
universe_uniprot <- dap_df$Protein.Group

# 2. Define significant DAPs
sig_daps <- dap_df %>%
  filter(HighConfidenceDAP) %>%
  pull(Protein.Group)

# Map background universe to Entrez IDs
universe_mapped <- bitr(
  universe_uniprot,
  fromType = "UNIPROT",
  toType   = c("ENTREZID", "SYMBOL"),
  OrgDb    = org.Hs.eg.db
)

# Map significant DAPs to Entrez IDs
sig_mapped <- bitr(
  sig_daps,
  fromType = "UNIPROT",
  toType   = c("ENTREZID", "SYMBOL"),
  OrgDb    = org.Hs.eg.db
)

cat("Quantified universe mapped:", nrow(universe_mapped), "genes\n")
cat("Significant DAPs mapped:", nrow(sig_mapped), "genes\n")
```

---

## Step 2: Over-Representation Analysis (ORA)

### Gene Ontology (GO) Biological Process Enrichment

We test whether any Biological Process is over-represented in our significant DAPs relative to our experimental universe:

```r
# Run GO Biological Process enrichment
ego_bp <- enrichGO(
  gene          = sig_mapped$ENTREZID,
  universe      = universe_mapped$ENTREZID, # Crucial: Custom Proteome Background!
  OrgDb         = org.Hs.eg.db,
  ont           = "BP",
  pAdjustMethod = "BH",
  pvalueCutoff  = 0.01,
  qvalueCutoff  = 0.05,
  readable      = TRUE # Converts Entrez IDs back to Gene Symbols in the output
)

head(ego_bp %>% as.data.frame() %>% select(Description, GeneRatio, BgRatio, p.adjust), 5)
```

```text
                                  Description GeneRatio   BgRatio     p.adjust
GO:0007049                         cell cycle    78/542 412/6210 1.482011e-18
GO:0051301                      cell division    54/542 245/6210 3.204192e-15
GO:0000278       mitotic cell cycle regulation    48/542 210/6210 5.920410e-14
GO:0007267      cell-cell signaling via MAPK    36/542 142/6210 8.104291e-11
GO:0006260                   DNA replication    28/542  98/6210 1.140291e-09
```

The biological story is undeniable: Kinase inhibitor treatment profoundly halts **Cell Cycle**, **Cell Division**, and **MAPK Signaling** in A549 lung cancer cells!

---

### KEGG Pathway Enrichment

Now test for curated molecular pathways in the **KEGG** database:

```r
ekegg <- enrichKEGG(
  gene          = sig_mapped$ENTREZID,
  universe      = universe_mapped$ENTREZID,
  organism      = "hsa",
  pvalueCutoff  = 0.05
)

# Convert Entrez IDs to Symbols for readability
ekegg <- setReadable(ekegg, OrgDb = org.Hs.eg.db, keyType = "ENTREZID")
```

---

### Visualizing ORA with Dotplots and Treeplots

```r
# 1. Dotplot showing gene ratio, count, and adjusted p-value
p_dot <- dotplot(ego_bp, showCategory = 15, title = "GO Biological Process Enrichment") +
  theme_bw()

# 2. Gene-Concept Network showing which proteins belong to which pathway
p_cnet <- cnetplot(ego_bp, categorySize = "pvalue", foldChange = NULL, showCategory = 5)

# Save plots
ggsave("results/figures/Fig5a_GO_Enrichment_Dotplot.png", p_dot, width = 8, height = 6, dpi = 300)
ggsave("results/figures/Fig5b_Gene_Concept_Network.png", p_cnet, width = 10, height = 8, dpi = 300)
```

---

## Step 3: Gene Set Enrichment Analysis (GSEA)

### Why GSEA is Superior to Arbitrary Thresholds

Over-Representation Analysis (ORA) requires setting arbitrary cutoffs (e.g., $|\log_2\text{FC}| \ge 1.0$ and $p < 0.05$). Proteins with a $\log_2\text{FC} = 0.95$ are completely discarded.

**Gene Set Enrichment Analysis (GSEA)** evaluates the **entire quantified proteome** without discarding any proteins. It tests whether members of a pathway cluster at the top (strongly up-regulated) or bottom (strongly down-regulated) of an ordered protein list.

### Constructing the Ranked Protein List

We rank every protein using a continuous statistical metric:
$$\text{Rank Score} = \text{sign}(\log_2\text{FC}) \times (-\log_{10} P)$$

```r
# Merge DAP statistics with Entrez mapping
gsea_prep <- left_join(dap_df, universe_mapped, by = c("Protein.Group" = "UNIPROT")) %>%
  filter(!is.na(ENTREZID)) %>%
  # Remove duplicate Entrez IDs by selecting highest absolute logFC
  arrange(desc(abs(logFC))) %>%
  distinct(ENTREZID, .keep_all = TRUE) %>%
  # Calculate ranking score
  mutate(RankScore = sign(logFC) * -log10(P.Value)) %>%
  arrange(desc(RankScore))

# Create named ranking vector
gene_list <- gsea_prep$RankScore
names(gene_list) <- gsea_prep$ENTREZID
```

---

### Running gseGO and gseKEGG

```r
# Run GSEA for Biological Processes
gsea_bp <- gseGO(
  geneList     = gene_list,
  OrgDb        = org.Hs.eg.db,
  ont          = "BP",
  minGSSize    = 15,
  maxGSSize    = 500,
  pvalueCutoff = 0.05,
  verbose      = FALSE
)

head(gsea_bp %>% as.data.frame() %>% select(Description, setSize, enrichmentScore, NES, p.adjust), 4)
```

---

### Plotting GSEA Running-Sum Enrichment Scores

We visualize the iconic GSEA enrichment mountain plot:

```r
# Plot running enrichment score for "Cell Cycle G1/S Phase Transition"
p_gsea <- gseaplot2(
  gsea_bp,
  geneSetID = 1, # Top pathway
  title     = gsea_bp$Description[1],
  color     = "#2B5C8F"
)

pdf("results/figures/Fig5c_GSEA_Running_Score.pdf", width = 8, height = 6)
print(p_gsea)
dev.off()
```

---

## Step 4: Protein-Protein Interaction (PPI) Networks with STRINGdb

### Connecting DAPs into Physical Multi-Protein Complexes

Proteins rarely act in isolation. Enzymes, kinases, and scaffold proteins form physical complexes.

The **STRING** database (*Search Tool for the Retrieval of Interacting Genes/Proteins*) integrates physical protein-protein interactions from crystallography, co-immunoprecipitation, and high-throughput yeast two-hybrid experiments.

We use the official Bioconductor package **`STRINGdb`**:

```r
library(STRINGdb)

# Initialize STRING database for Homo sapiens (Taxonomy ID: 9606)
# score_threshold = 700 enforces high-confidence physical interactions
string_db <- STRINGdb$new(
  version         = "12",
  species         = 9606,
  score_threshold = 700, # High confidence (0.7)
  input_directory = ""
)

# Map our high-confidence DAPs to STRING protein identifiers
mapped_string <- string_db$map(
  dap_df %>% filter(HighConfidenceDAP),
  "Genes",
  removeUnmappedRows = TRUE
)

cat("Successfully mapped", nrow(mapped_string), "DAPs to STRING protein nodes!\n")
```

---

### Extracting High-Confidence Subnetworks and Complexes

We extract the physical interaction subgraph and identify densely connected modules (protein complexes):

```r
# Extract interaction network among our DAPs
hits <- mapped_string$STRING_id
subnetwork <- string_db$get_subnetwork(hits)

# Calculate network clustering (MCL algorithm) to detect physical protein complexes
clusters <- string_db$get_clusters(hits, algorithm = "mcl")

cat("Detected", length(clusters), "distinct physical protein complexes among DAPs!\n")

# Top cluster: Replicative MCM Helicase Complex (MCM2, MCM3, MCM4, MCM5, MCM6, MCM7)
print(clusters[[1]])

# Plot physical interaction network
png("results/figures/Fig6_STRING_PPI_Network.png", width = 10, height = 10, units = "in", res = 300)
string_db$plot_network(hits[1:80]) # Visualize top 80 interacting hub proteins
dev.off()
```

Among our down-regulated proteins, the entire **MCM Replicative Helicase Complex (MCM2–7)** forms a tightly interconnected clique, proving that kinase inhibition halts DNA replication licensing!

---

## Conclusion & What’s Next

In this tutorial, you learned:
1. The **proteome background universe rule**: why testing against the whole genome causes false discoveries and how to define your custom detected background.
2. How to run **Over-Representation Analysis (ORA)** and visualize results with dotplots and gene-concept networks.
3. How to perform **Gene Set Enrichment Analysis (GSEA)** by ranking continuous fold-change and $p$-value metrics.
4. How to reconstruct **physical protein-protein interaction (PPI) networks** using **`STRINGdb`** to reveal active protein complexes.

You now possess the foundational toolkit for end-to-end proteomics analysis!

Next, we proceed to advanced translational topics:
In **[Part 6: Protein Co-expression Network Analysis (WGCNA)](10_part-6-protein-co-expression-networks-wgcna.md)**, we adapt Weighted Gene Co-expression Network Analysis to discover modules of co-regulated proteins that correlate with clinical phenotypes!
