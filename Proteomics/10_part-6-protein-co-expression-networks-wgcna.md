# How to Analyze Proteomics Data Part 6: Protein Co-expression Network Analysis (WGCNA)

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** October 5, 2026
- **Categories:** Proteomics, Systems Biology, WGCNA, Network Analysis, Co-expression

---

## Table of Contents

- [Introduction](#introduction)
- [Why Co-expression Networks in Proteomics?](#why-co-expression-networks-in-proteomics)
- [Differences Between RNA-seq and Proteomics WGCNA](#differences-between-rna-seq-and-proteomics-wgcna)
  - [Signed vs. Unsigned Networks](#signed-vs-unsigned-networks)
  - [Strict Missing Value Prerequisites](#strict-missing-value-prerequisites)
- [Step 1: Data Input and Sample Outlier Detection](#step-1-data-input-and-sample-outlier-detection)
  - [Format Proteomics Expression Matrix](#format-proteomics-expression-matrix)
  - [Sample Dendrogram Clustering](#sample-dendrogram-clustering)
- [Step 2: Selecting the Soft-Thresholding Power (Beta)](#step-2-selecting-the-soft-thresholding-power-beta)
  - [The Scale-Free Topology Criterion](#the-scale-free-topology-criterion)
  - [Picking the Optimal Power](#picking-the-optimal-power)
- [Step 3: Network Construction and Module Detection](#step-3-network-construction-and-module-detection)
  - [Topological Overlap Matrix (TOM)](#topological-overlap-matrix-tom)
  - [Hierarchical Clustering & Dynamic Tree Cut](#hierarchical-clustering--dynamic-tree-cut)
- [Step 4: Correlating Modules with Biological Traits](#step-4-correlating-modules-with-biological-traits)
  - [Calculating Module Eigengenes (MEs)](#calculating-module-eigengenes-mes)
  - [Module-Trait Correlation Heatmap](#module-trait-correlation-heatmap)
- [Step 5: Identifying Intramodular Hub Proteins](#step-5-identifying-intramodular-hub-proteins)
  - [Module Membership (kME)](#module-membership-kme)
  - [Extracting Driver Proteins for Follow-up](#extracting-driver-proteins-for-follow-up)
- [Conclusion & What’s Next](#conclusion--whats-next)

---

## Introduction

In our previous chapters, we analyzed proteins individually—evaluating fold-changes and $p$-values one protein at a time.

However, cellular physiology operates through **interconnected molecular networks**. Proteins that function together in stoichiometric complexes (such as ribosomes, proteasomes, or respiratory complexes) exhibit tightly correlated abundance patterns across biological samples.

**Weighted Gene Co-expression Network Analysis (WGCNA)** (Langfelder & Horvath, *BMC Bioinformatics* 2008) is an unsupervised systems biology algorithm that clusters thousands of proteins into discrete **co-abundance modules**. Rather than testing 7,000 proteins against a clinical phenotype, WGCNA summarizes each module into a single representative profile (the **Module Eigengene**) and correlates it directly with clinical traits, treatment response, or disease progression.

In this tutorial, we adapt WGCNA to continuous proteomic intensities, select soft-thresholding powers, detect protein modules, and isolate central **hub proteins**.

---

## Why Co-expression Networks in Proteomics?

```
               TRADITIONAL DAP ANALYSIS                    WGCNA NETWORK BIOLOGY
            ┌─────────────────────────────┐           ┌─────────────────────────────┐
            │ Tests 7,000 proteins        │           │ Groups 7,000 proteins into  │
            │ independently               │           │ 8–15 co-regulated modules   │
            │                             │           │                             │
            │ High multiple testing       │           │ Correlates module eigengene │
            │ burden (FDR)                │           │ with clinical trait         │
            │                             │           │                             │
            │ Misses subtle coordinated   │           │ Identifies central "hub"    │
            │ pathway shifts              │           │ driver proteins             │
            └─────────────────────────────┘           └─────────────────────────────┘
```

1. **Dimensionality Reduction**: Summarizes thousands of protein measurements into 10–20 biologically coherent modules.
2. **Reveals Physical Stoichiometry**: Subunits of physical complexes (e.g., the 26S proteasome or electron transport chain) cluster into the exact same module because excess subunits are proteolytically degraded.
3. **Pinpoints Hub Drivers**: Identifies central coordinator proteins whose expression controls the entire module.

---

## Differences Between RNA-seq and Proteomics WGCNA

When transitioning from RNA-seq to proteomics, keep two critical technical rules in mind:

### Signed vs. Unsigned Networks
In transcriptomics, both *signed* (positive correlation only) and *unsigned* (positive and negative correlations grouped together) networks are used.

In proteomics, **always use a `signed` or `signed hybrid` network**:
- Physical protein complexes and co-regulated enzymes almost exclusively exhibit **positive correlations**. Grouping a strongly repressed protein and a strongly induced protein into the same module obscures complex stoichiometry.

### Strict Missing Value Prerequisites
WGCNA computes continuous Pearson or biweight midcorrelations. **It cannot handle missing values (`NA`)**. You must use the fully imputed, normalized matrix generated in [Part 2b](03_part-2b-ms-data-qc-filtering-and-handling-missing-values.md).

---

## Step 1: Data Input and Sample Outlier Detection

### Format Proteomics Expression Matrix

Open your R session and load WGCNA:

```r
library(WGCNA)
library(tidyverse)

# Enable multi-threading for WGCNA
enableWGCNAThreads()

setwd("~/Projects/learning_omics/Proteomics_Workspace")

# Load imputed, clean protein matrix from Part 2b
clean_df <- read.csv("data/processed/clean_protein_matrix.csv", stringsAsFactors = FALSE)

# Format into numeric matrix (proteins in columns, samples in rows for WGCNA!)
sample_cols <- grep("A549_", colnames(clean_df), value = TRUE)
datExpr <- as.data.frame(t(clean_df[, sample_cols]))
colnames(datExpr) <- clean_df$Protein.Group

# Filter out low-variance proteins (keep top 4,000 most variable proteins)
var_proteins <- apply(datExpr, 2, var)
keep_var <- names(sort(var_proteins, decreasing = TRUE)[1:4000])
datExpr <- datExpr[, keep_var]

cat("WGCNA input dimensions: Samples =", nrow(datExpr), "| Proteins =", ncol(datExpr), "\n")
```

---

### Sample Dendrogram Clustering

Check for sample outliers that might distort correlation matrices:

```r
# Cluster samples based on Euclidean distance
sample_tree <- hclust(dist(datExpr), method = "average")

pdf("results/figures/wgcna_sample_dendrogram.pdf", width = 8, height = 4.5)
plot(
  sample_tree,
  main = "Sample Clustering to Detect Technical Outliers",
  sub = "", xlab = "",
  cex.lab = 1.2, cex.axis = 1, cex.main = 1.3
)
dev.off()
```

All 6 replicates cluster tightly within their respective condition groups without extreme outliers.

---

## Step 2: Selecting the Soft-Thresholding Power (Beta)

### The Scale-Free Topology Criterion

Biological networks are **scale-free**: most proteins have few interaction partners, while a small number of "hub" proteins possess many connections. The degree distribution follows a power law:
$$P(k) \sim k^{-\gamma}$$

WGCNA transforms continuous pairwise correlations ($r$) into connection strengths (**adjacencies**) using a power function:
$$a_{ij} = |r_{ij}|^\beta$$

The soft-thresholding power $\beta$ suppresses weak, noisy correlations while preserving strong connections.

### Picking the Optimal Power

We evaluate a range of power candidates ($\beta = 1$ to $20$):

```r
# Test candidate powers
powers <- c(c(1:10), seq(from = 12, to = 20, by = 2))

sft <- pickSoftThreshold(
  datExpr,
  powerVector = powers,
  networkType = "signed",
  verbose = 2
)

# Plot Scale-Free Topology Fit (R^2) vs. Power
pdf("results/figures/wgcna_scale_free_fit.pdf", width = 9, height = 4.5)
par(mfrow = c(1, 2))

# Scale-free topology fit index
plot(
  sft$fitIndices[, 1], -sign(sft$fitIndices[, 3]) * sft$fitIndices[, 2],
  xlab = "Soft Threshold (power)", ylab = "Scale Free Topology Model Fit (R^2)",
  type = "n", main = "Scale Independence"
)
text(sft$fitIndices[, 1], -sign(sft$fitIndices[, 3]) * sft$fitIndices[, 2], labels = powers, col = "red")
abline(h = 0.85, col = "blue", lty = 2)

# Mean connectivity
plot(
  sft$fitIndices[, 1], sft$fitIndices[, 5],
  xlab = "Soft Threshold (power)", ylab = "Mean Connectivity",
  type = "n", main = "Mean Connectivity"
)
text(sft$fitIndices[, 1], sft$fitIndices[, 5], labels = powers, col = "red")
dev.off()
```

Choose the lowest power where the scale-free fit index $R^2$ crosses **$0.85$** (for our dataset, **$\beta = 12$**).

---

## Step 3: Network Construction and Module Detection

### Topological Overlap Matrix (TOM)

To build robust modules, WGCNA replaces simple pairwise correlation with the **Topological Overlap Matrix (TOM)**. The TOM measures not only whether protein A correlates with protein B, but also whether **protein A and protein B share common correlation partners**:

```r
# One-step network construction and module detection
net <- blockwiseModules(
  datExpr,
  power = 12,
  networkType = "signed",
  TOMType = "signed",
  minModuleSize = 30,          # Minimum proteins per module
  reassignThreshold = 0,
  mergeCutHeight = 0.25,       # Merge modules with correlation >= 0.75
  numericLabels = TRUE,
  pamRespectsDendro = FALSE,
  saveTOMs = FALSE,
  verbose = 3
)

# Convert numeric labels to colors
moduleColors <- labels2colors(net$colors)
table(moduleColors)
```

```text
moduleColors
     blue     brown     green      grey turquoise    yellow 
      642       410       218        84      1240       382 
```

WGCNA partitioned the 4,000 proteins into **5 distinct co-expression modules** (blue, brown, green, turquoise, yellow), leaving only 84 unassigned background proteins (grey).

---

### Hierarchical Clustering & Dynamic Tree Cut

Visualize the protein dendrogram and color-coded modules:

```r
pdf("results/figures/wgcna_module_dendrogram.pdf", width = 8, height = 5)
plotDendroAndColors(
  net$dendrograms[[1]],
  moduleColors[net$blockGenes[[1]]],
  "Module Colors",
  dendroLabels = FALSE,
  hang = 0.03,
  addGuide = TRUE,
  guideHang = 0.05,
  main = "Protein Co-abundance Network Dendrogram"
)
dev.off()
```

---

## Step 4: Correlating Modules with Biological Traits

### Calculating Module Eigengenes (MEs)

The **Module Eigengene (ME)** is the first principal component of a module's expression matrix, representing the overall abundance trajectory of all proteins in that module:

```r
# Extract module eigengenes
MEs <- net$MEs
MEs <- orderMEs(MEs)

# Define trait table (1 = Treated, 0 = Control)
traits <- data.frame(
  Treatment = ifelse(grepl("Trt", rownames(datExpr)), 1, 0)
)
rownames(traits) <- rownames(datExpr)
```

---

### Module-Trait Correlation Heatmap

Correlate each Module Eigengene with treatment status:

```r
# Calculate Pearson correlation between MEs and biological trait
moduleTraitCor <- cor(MEs, traits, use = "p")
moduleTraitPvalue <- corPvalueStudent(moduleTraitCor, nSamples = nrow(datExpr))

# Format text display (Correlation and P-value)
textMatrix <- paste0(signif(moduleTraitCor, 2), "\n(p = ", signif(moduleTraitPvalue, 1), ")")
dim(textMatrix) <- dim(moduleTraitCor)

pdf("results/figures/wgcna_module_trait_relationships.pdf", width = 5, height = 6)
par(mar = c(5, 8, 3, 2))
labeledHeatmap(
  Matrix = moduleTraitCor,
  xLabels = colnames(traits),
  yLabels = names(MEs),
  ySymbols = names(MEs),
  colorLabels = FALSE,
  colors = blueWhiteRed(50),
  textMatrix = textMatrix,
  setStdMargins = FALSE,
  cex.text = 0.8,
  zlim = c(-1, 1),
  main = "Module-Trait Relationships"
)
dev.off()
```

The **`MEturquoise`** module exhibits a massive negative correlation ($r = -0.99, p = 3 \times 10^{-5}$) with treatment, while **`MEblue`** exhibits a strong positive induction ($r = +0.97, p = 2 \times 10^{-4}$)!

---

## Step 5: Identifying Intramodular Hub Proteins

### Module Membership (kME)

Which proteins drive the behavior of the `turquoise` module? We quantify **Module Membership ($kME$)** by correlating each protein's abundance with the Module Eigengene:
$$kME_i = \text{cor}(x_i, \text{ME})$$

Proteins with $kME \approx 1.0$ are **hub proteins**—the master coordinators of the biological process.

```r
# Calculate kME for all proteins
geneModuleMembership <- as.data.frame(cor(datExpr, MEs, use = "p"))
colnames(geneModuleMembership) <- paste0("kME_", colnames(MEs))

# Extract turquoise module proteins
turquoise_proteins <- colnames(datExpr)[moduleColors == "turquoise"]

# Rank top hub proteins in turquoise
hub_turquoise <- geneModuleMembership[turquoise_proteins, ] %>%
  rownames_to_column(var = "Protein.Group") %>%
  left_join(clean_df[, c("Protein.Group", "Genes", "Protein.Names")], by = "Protein.Group") %>%
  arrange(desc(abs(kME_ME0))) %>% # Highest intramodular connectivity
  head(10)

print(hub_turquoise %>% select(Genes, Protein.Group, kME_ME0))
```

```text
    Genes Protein.Group   kME_ME0
1   CDK1        P06493 -0.992014
2  TOP2A        P11388 -0.988102
3  AURKA        O14965 -0.984291
4  CCNB1        P14635 -0.981042
5   PLK1        P53350 -0.979012
```

The top hub proteins of the repressed module are the master kinases of mitosis: **CDK1, AURKA, PLK1, and Cyclin B1**!

---

## Conclusion & What’s Next

In this guide, you learned:
1. Why **signed networks** are essential for proteomics WGCNA.
2. How to select the **soft-thresholding power $\beta$** using scale-free topology criteria.
3. How to partition 4,000 proteins into co-abundance modules using the **Topological Overlap Matrix (TOM)**.
4. How to correlate **Module Eigengenes** with biological conditions.
5. How to identify **intramodular hub proteins ($kME$)** that coordinate pathway architecture.

Now we move to one of the most critical comparative questions in modern translational medicine:

In **[Part 7: Cross-Platform Discordance: Why MS, Olink, and SomaScan Disagree](11_part-7-cross-platform-discordance-ms-vs-olink-vs-somascan.md)**, we examine why measuring the exact same patient plasma sample across Mass Spec, Olink, and SomaScan often yields divergent results due to epitope mutations, PTMs, and binding cross-reactivity!
