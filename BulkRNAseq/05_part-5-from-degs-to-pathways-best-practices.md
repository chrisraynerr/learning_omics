# How to Analyze RNAseq Data for Absolute Beginners Part 5: From DEGs to Pathways – Best Practices

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/)

- [Introduction](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Introduction)
- [Pathway Databases: Your Guide to Gene Function](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Pathway_Databases_Your_Guide_to_Gene_Function)

  - [Gene Ontology (GO)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Gene_Ontology_GO)
  - [KEGG (Kyoto Encyclopedia of Genes and Genomes)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#KEGG_Kyoto_Encyclopedia_of_Genes_and_Genomes)
  - [Molecular Signatures Database (MSigDB)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Molecular_Signatures_Database_MSigDB)

- [Statistical Methods for Pathway Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Statistical_Methods_for_Pathway_Analysis)

  - [1. Over-Representation Analysis (ORA)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#1_Over-Representation_Analysis_ORA)
  - [2. Gene Set Enrichment Analysis (GSEA)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#2_Gene_Set_Enrichment_Analysis_GSEA)

- [Practical Implementation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Practical_Implementation)

  - [Prerequisites](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Prerequisites)
  - [1. Data Preparation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#1_Data_Preparation)
  - [2. Gene Set Enrichment Analysis (GSEA)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#2_Gene_Set_Enrichment_Analysis_GSEA-2)
  - [3. Over-Representation Analysis (ORA)](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#3_Over-Representation_Analysis_ORA)
  - [4. MSigDB Enrichment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#4_MSigDB_Enrichment)

- [Results Interpretation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Results_Interpretation)
- [Best Practices](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Best_Practices)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#References)
- [Additional Resources](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Additional_Resources)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-5-from-degs-to-pathways-best-practices/#Like_this)

## Introduction

After completing the data preparation, statistical testing, and visualization steps, we’re finally ready to explore the biological significance of our RNA sequencing data. As biologists, this is the moment we’ve been waiting for – but how do we make sense of the hundreds or thousands of differentially expressed genes (DEGs) we’ve identified?

Living organisms are complex systems where thousands of genes work together in harmony. These genes form functional groups, each responsible for different processes like cell division, protein binding, metabolism, or disease responses. To understand what our DEGs are telling us, we need to identify which functional groups they belong to and what biological processes they influence. This is where functional enrichment analysis, also known as pathway analysis, becomes crucial.

Pathway analysis helps us:

- Connect expression changes to known biological mechanisms

- Provide biological context for our experimental findings

- Generate hypotheses for further investigation

## Pathway Databases: Your Guide to Gene Function

Pathway databases are specialized resources containing curated information about biological pathways and their associated gene sets. During pathway analysis, we compare our DEGs against these databases to identify significantly overlapping gene sets and their corresponding pathways. Here are the most widely used databases for RNA-seq analysis:

### Gene Ontology (GO)

[Gene Ontology (GO)](https://geneontology.org/) is a comprehensive bioinformatics resource that provides a structured vocabulary (ontologies) describing gene functions, processes, and cellular locations. It’s organized into three main categories:

**Molecular Function (MF)**

1. 

- Describes specific activities at the molecular level

- Examples: ATP binding, protein kinase activity

**Biological Process (BP)**

1. 

- Defines broader biological goals involving multiple molecular functions

- Examples: cell cycle regulation, immune response, apoptosis

**Cellular Component (CC)**

1. 

- Indicates where gene products are active within the cell

- Examples: nucleus, cytoplasm, mitochondrial membrane

### KEGG (Kyoto Encyclopedia of Genes and Genomes)

[KEGG](https://www.genome.jp/kegg/) is a comprehensive database integrating genomic, chemical, and functional information. It includes:

- **Pathways**: Detailed maps of biological processes

- **KEGG Orthology (KO)**: Groups of genes with shared functions across species

- **KEGG Disease**: Disease pathways and gene-disease associations

- **KEGG Drug and Enzyme**: Drug interactions and metabolic enzyme information

- **KEGG Compound**: Chemical compound structures and roles

### Molecular Signatures Database (MSigDB)

[MSigDB](https://www.gsea-msigdb.org/gsea/msigdb) is the most comprehensive pathway database, incorporating both GO and KEGG data plus much more. It’s your one-stop shop for pathway analysis, featuring:

- **Hallmark Gene Sets**: Core biological processes and pathways

- **Curated Gene Sets (C2)**: Pathways from KEGG, Reactome, and published studies

- **Motif Gene Sets (C3)**: Gene regulatory motifs

- **Computational Gene Sets (C4)**: Co-expression networks

- **GO Gene Sets (C5)**: Gene Ontology groupings

- **Oncogenic Signatures (C6)**: Cancer-related pathways

- **Immunologic Signatures (C7)**: Immune system patterns

- **Cell Type Signatures (C8)**: Cell-specific expression patterns

## Statistical Methods for Pathway Analysis

### 1. Over-Representation Analysis (ORA)

ORA uses statistical tests (typically Fisher’s exact test or hypergeometric test) to evaluate gene set enrichment.

**Advantages:**

- Simple to understand and interpret

- Straightforward implementation

**Limitations:**

- Requires predefined cutoffs for DEG selection

- Treats all genes as equally important

- Doesn’t consider expression change magnitude

### 2. Gene Set Enrichment Analysis (GSEA)

GSEA uses a running-sum statistic to analyze gene distribution patterns within ranked lists.

**Advantages:**

- No arbitrary cutoffs needed

- Considers expression change magnitude

- Detects subtle expression patterns

**Limitations:**

- More computationally intensive

- Can be more complex to interpret

## Practical Implementation

### Prerequisites

First, install the required R packages:

```r
# Install required packages if needed
install.packages(c("BiocManager"))
BiocManager::install(c("clusterProfiler", "org.Hs.eg.db", "org.Mm.eg.db", "org.Rn.eg.db", "pathview", "enrichplot", "DOSE"))

install.packages(c("data.table", "ggplot2"))
```

### 1. Data Preparation

#### 1.1 Loading Required Libraries

```r
library(data.table)
library(clusterProfiler)
# library(org.Hs.eg.db)
library(org.Mm.eg.db)
# library(org.Rn.eg.db)
library(ggplot2)
library(DOSE)
library(enrichplot)
```

#### 1.2 Input Data Format

First, let’s import the DEG table we created in the [previous tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-3-from-count-table-to-degs-best-practices/). For ORA analysis, we need to establish thresholds for both P-value and fold change to filter our DEGs. Note that these filtering thresholds are specific to ORA – the GSEA method analyzes the entire gene set without preliminary filtering.

```r
deg <- fread("DEG_P1_lfc0.tsv")

p_threshold <- 0.05
fc_threshold <- 2
```

### 2. Gene Set Enrichment Analysis (GSEA)

#### 2.1 GO Enrichment Analysis

```r
# rank the DEGs by the fold change
deg_order_fc <- deg[order(-logFC)] # rank the genes by logFC in descending order
logfc <- deg_order_fc$logFC # get logFC
names(logfc) <- deg_order_fc$V1 # make a named vector of logFC

# GSEA
# Mouse: OrgDb = org.Mm.eg.db; Rat: OrgDb = org.Rn.eg.db; Human: OrgDb = org.Hs.eg.db;
enrich_go_gsea <- gseGO(geneList = logfc, OrgDb = org.Mm.eg.db, ont = "ALL", pvalueCutoff = 0.05, keyType ="SYMBOL", verbose= FALSE)

# save the enrichment table
enrich_go_gsea_df <- enrich_go_gsea@result

fwrite(enrich_go_gsea_df, "enrich_go_gsea_df.tsv", sep = "\t")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-26-at-8.45.03%E2%80%AFAM.png?resize=1024%2C436&ssl=1)

#### 2.2 Visualize The Top Enriched GO terms

The clusterProfiler package offers numerous visualization options for enriched GO terms. We’ll start with the dot plot visualization, which is the most widely used and intuitive representation. For additional visualization methods, including advanced graphics options, refer to the [clusterProfiler documentation](https://yulab-smu.top/biomedical-knowledge-mining-book/enrichplot.html).

```r
dotplot_enrich_go_gsea <- dotplot(enrich_go_gsea, showCategory = 10, orderBy="GeneRatio")

ggsave("dotplot_enrich_go_gsea.png", dotplot_enrich_go_gsea, device = "png", units = "cm", width = 16, height = 18)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/dotplot_enrich_go_gsea.png?resize=910%2C1024&ssl=1)

GSEA results can be visualized at the individual pathway level to examine specific enrichment patterns.

```r
# specify the GO term you want to plot (check your "enrich_go_gsea_df" above)
GOID <- "GO:0010634"

gsea_plot <- gseaplot2(enrich_go_gsea, geneSetID = GOID, title = subset(enrich_go_gsea_df, ID == GOID)$Description)

ggsave(paste0("gsea_plot_", gsub(":", "", GOID), ".png"), gsea_plot, device = "png", units = "cm", width = 18, height = 16)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/gsea_plot_GO0010634.png?resize=1024%2C910&ssl=1)

#### 2.3 KEGG Enrichment Analysis

The process follows the same workflow as above, but uses the ‘gseKEGG’ function specifically for KEGG pathway analysis. Note the “organism” option (“hsa”: human, “mmu”: mouse and “rno”: rat), and the gene list option (Entrez ID instead of Gene Symbols).

```r
# Load required annotation packages
# Choose the organism-specific database that matches your dataset:
#   - org.Mm.eg.db : Mouse
#   - org.Hs.eg.db : Human
#   - org.Rn.eg.db : Rat
library(AnnotationDbi)
library(org.Mm.eg.db)
library(org.Rn.eg.db)
library(org.Hs.eg.db)

# ------------------------------------------------------------
# 1. Convert gene symbols → Entrez IDs using mapIds()
# ------------------------------------------------------------
# 'logfc' should be a named numeric vector where names = gene symbols.
# We convert those gene symbols to Entrez IDs because KEGG requires Entrez IDs.
entrez_ids <- mapIds(
  x = org.Mm.eg.db,        # Change to org.Hs.eg.db (human) or org.Rn.eg.db (rat) as needed
  keys = names(logfc),     # Input gene symbols
  keytype = "SYMBOL",      # Type of input identifiers
  column = "ENTREZID",     # Type of output identifiers
  multiVals = "first"      # If multiple Entrez IDs map to one symbol, keep the first
)

# Replace gene symbol names with Entrez IDs
names(logfc) <- entrez_ids

# Remove genes that failed to map (NA Entrez IDs)
logfc <- logfc[!is.na(names(logfc))]

# ------------------------------------------------------------
# 2. Run KEGG GSEA (gseKEGG) for different organisms
# ------------------------------------------------------------
# KEGG organism codes:
#   - "mmu" : Mouse
#   - "hsa" : Human
#   - "rno" : Rat

# Mouse KEGG GSEA
enrich_kegg_gsea <- gseKEGG(
  geneList = logfc,
  organism = "mmu"
)

# Rat KEGG GSEA
enrich_kegg_gsea <- gseKEGG(
  geneList = logfc,
  organism = "rno"
)

# Human KEGG GSEA
enrich_kegg_gsea <- gseKEGG(
  geneList = logfc,
  organism = "hsa"
)
```

### 3. Over-Representation Analysis (ORA)

#### 3.1 GO Enrichment Analysis

The ORA method requires only a list of differentially expressed genes as input. In this example, we’ll focus on analyzing up-regulated genes, but you can apply the same process to down-regulated genes as well.

```r
# make a gene list
deg_up <- deg[logFC > log2(fc_threshold) & adj.P.Val < p_threshold]$V1
# deg_dn <- deg[logFC < -log2(fc_threshold) & adj.P.Val < p_threshold]$V1

enrich_go_fet_up <- enrichGO(gene = deg_up, OrgDb=org.Mm.eg.db, keyType="SYMBOL", ont="ALL", pvalueCutoff=0.05, pAdjustMethod="BH", qvalueCutoff=0.05)

enrich_go_fet_up_df <- enrich_go_fet_up@result

fwrite(enrich_go_fet_up_df, "enrich_go_fet_up_df.tsv", sep = "\t")
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/Screenshot-2024-10-26-at-8.48.11%E2%80%AFAM.png?resize=1024%2C472&ssl=1)

#### 3.2 Visualize The Top Enriched GO terms

```r
# show the top 10 terms
dotplot_enrich_go_fet_up <- dotplot(enrich_go_fet_up, showCategory = 10, orderBy="GeneRatio")

ggsave("dotplot_enrich_go_fet_up.png", dotplot_enrich_go_fet_up, device = "png", units = "cm", width = 16, height = 18)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/10/dotplot_enrich_go_fet_up.png?resize=910%2C1024&ssl=1)

#### 3.3 KEGG Enrichment Analysis

The methodology mirrors the GO enrichment analysis, with the key difference being the use of the ‘enrichKEGG’ function for KEGG pathway enrichment. Be sure to convert gene symbols to Entrez IDs before running KEGG analysis using the method described above.

```r
enrich_kegg_fet_up <- enrichKEGG(
  gene     = deg_up,     # Vector of Entrez IDs
  organism = "mmu",      # Change to "hsa" (human) or "rno" (rat) as needed
  keyType  = "kegg"      # Required when using Entrez IDs
)
```

### 4. MSigDB Enrichment

Now for the most comprehensive analysis option: MSigDB analysis. This powerful database incorporates GO, KEGG, Hallmark, Reactome, and many other pathway collections, allowing you to explore your DEGs across multiple annotation systems simultaneously.

Before beginning the MSigDB analysis, download the appropriate gene set file from the MSigDB website. Select the correct species-specific file ([mouse](https://www.gsea-msigdb.org/gsea/msigdb/download_file.jsp?filePath=/msigdb/release/2024.1.Mm/msigdb.v2024.1.Mm.symbols.gmt) or [human](https://www.gsea-msigdb.org/gsea/msigdb/human/genesets.jsp?targetSpeciesDB=Human)) and either place it in your R working directory or note its full file path for later use.

The visualization methods for MSigDB enrichment follow the same principles we used for GO and KEGG analyses.

#### 4.1 The GSEA Method

```r
msigdb <- read.gmt("msigdb.v2024.1.Mm.symbols.gmt")

enrich_msigdb_gsea <- GSEA(logfc, TERM2GENE = msigdb)

enrich_msigdb_gsea_df <- enrich_msigdb_gsea@result

fwrite(enrich_msigdb_gsea_df, "enrich_msigdb_gsea_df.tsv", sep = "\t")
```

#### 4.2 The ORA Method

```r
msigdb <- read.gmt("msigdb.v2024.1.Mm.symbols.gmt")

enrich_msigdb_fet_up <- enricher(deg_up, TERM2GENE = msigdb)

enrich_msigdb_fet_up_df <- enrich_msigdb_fet_up@result

fwrite(enrich_msigdb_fet_up_df, "enrich_msigdb_fet_up_df.tsv", sep = "\t")
```

## Results Interpretation

When analyzing your pathway analysis results, consider these key factors:

**Statistical Significance**

- Examine adjusted p-values (FDR)

- Consider gene set sizes

- Look for consistent patterns across methods

**Biological Relevance**

- Focus on pathways related to your experimental context

- Consider pathway relationships and hierarchies

- Look for unexpected but interesting connections

**Technical Considerations**

- Account for gene set size effects

- Verify multiple testing correction methods

- Ensure appropriate background gene selection

## Best Practices

1. Use appropriate statistical cutoffs consistently

2. Consider your experimental context when interpreting results

3. Validate key findings using alternative methods

4. Document all analysis parameters and software versions

5. Cross-reference findings with current literature

6. Consider pathway redundancy and overlap

  Want to run this whole pipeline on your own data? [Join the live workshop →](https://ngs101.com/workshop/rna-seq-workshop/)

## References

1. Yu G, Wang LG, Han Y, He QY (2012). clusterProfiler: an R package for comparing biological themes among gene clusters. OMICS: A Journal of Integrative Biology

2. Love MI, Huber W, Anders S (2014). Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biology

## Additional Resources

- [ClusterProfiler Documentation](https://yulab-smu.top/biomedical-knowledge-mining-book/)

- [GSEA Homepage](https://www.gsea-msigdb.org/gsea/index.jsp)

- [Bioconductor Workflow](https://bioconductor.org/packages/release/workflows/)
