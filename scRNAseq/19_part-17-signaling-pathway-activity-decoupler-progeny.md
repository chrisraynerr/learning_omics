# How to Analyze Single-Cell RNA-seq Data &#8212; Complete Beginner's Guide Part 17: Infer Signaling Pathway Activity with decoupleR and PROGENy

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** RNA-seq, Single Cell Sequencing
- **Source:** [https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/)

---

*You already know the decoupleR engine from Part 16. Swap the network, swap one method, and you go from “which transcription factors are active” to “which signaling pathways are active” — in about half the code.*

---

In [Part 16](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/) you inferred per-cell transcription factor activity with decoupleR and CollecTRI, and the analysis landed on a concrete finding: inflammatory transcription factors (JUN, STAT3, IRF1, NFKB1) were most active in Classical Monocytes and dropped after periodontal treatment. A natural question follows. Transcription factors are the last step of a signaling cascade — so are the upstream *pathways* (TNFa, NF-kB, JAK-STAT) telling the same story?

That is exactly what PROGENy answers. This is a deliberately short follow-up: the decoupleR machinery, the dataset, and the plotting are all identical to Part 16, so we will move quickly through the shared parts and spend our time on the one thing that genuinely changes — the inference method.

> 

**Prerequisites:** Finish [Part 16](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/) first. This tutorial reuses its R environment (decoupleR, Seurat, and the plotting packages) and the same downsampled object, `seurat_subset.rds`, from [Part 7-2](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-7-2-trajectory-analysis-using-slingshot/#Strategic_Downsampling_Maintaining_Biological_Diversity). No new packages and no new data are required.

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/)

- [What PROGENy Adds to the Footprint Toolkit](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#What_PROGENy_Adds_to_the_Footprint_Toolkit)

  - [The One Real Difference: MLM Instead of ULM](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#The_One_Real_Difference_MLM_Instead_of_ULM)

- [Load the Environment and Data](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Load_the_Environment_and_Data)
- [Step 1: Get the PROGENy Model](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Step_1_Get_the_PROGENy_Model)
- [Step 2: Infer Pathway Activity with the Multivariate Linear Model](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Step_2_Infer_Pathway_Activity_with_the_Multivariate_Linear_Model)
- [Step 3: Store Pathway Scores as a Seurat Assay](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Step_3_Store_Pathway_Scores_as_a_Seurat_Assay)
- [Step 4: Which Pathways Define Each Cell Type?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Step_4_Which_Pathways_Define_Each_Cell_Type)
- [Step 5: Project a Pathway onto the UMAP](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Step_5_Project_a_Pathway_onto_the_UMAP)
- [Step 6: Do the Pathways Track the Treatment Effect?](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Step_6_Do_the_Pathways_Track_the_Treatment_Effect)
- [Best Practices for PROGENy Pathway Inference](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Best_Practices_for_PROGENy_Pathway_Inference)
- [Conclusion: What You Built in Part 17](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Conclusion_What_You_Built_in_Part_17)
- [Beyond decoupleR: Explore the Saez-Lab Ecosystem](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Beyond_decoupleR_Explore_the_Saez-Lab_Ecosystem)
- [References](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-17-infer-signaling-pathway-activity-with-decoupler-and-progeny/#Like_this)

## What PROGENy Adds to the Footprint Toolkit

PROGENy (Pathway RESPONSIVE GENes) is the pathway-level counterpart to CollecTRI. Instead of asking “which genes does each transcription factor control,” it asks “which genes change when each signaling pathway is perturbed.” Those perturbation-responsive genes are the pathway’s footprint. If a pathway’s responsive genes move in the expected direction in a cell, the pathway is likely active in that cell — the same footprint logic as Part 16, one level up the signaling cascade.

PROGENy covers 14 pathways: Androgen, EGFR, Estrogen, Hypoxia, JAK-STAT, MAPK, NFkB, p53, PI3K, TGFb, TNFa, Trail, VEGF, and WNT. For an inflammatory condition like periodontitis, TNFa, NFkB, and JAK-STAT are the ones to watch.

### The One Real Difference: MLM Instead of ULM

In Part 16 we used `run_ulm`, the univariate linear model — one independent regression per transcription factor. For PROGENy we use `run_mlm`, the multivariate linear model, and the switch is not arbitrary.

CollecTRI regulons barely overlap: most genes are targets of one transcription factor or none, so fitting each TF independently is fine. PROGENy pathways are different — TNFa, NFkB, and JAK-STAT share many responsive genes, because inflammatory signaling converges. If you scored each pathway independently, several pathways would all “claim credit” for the same upregulated inflammatory genes, inflating and duplicating their scores. MLM fits all 14 pathways together as predictors in a single regression per cell, so each pathway is assigned only the variation it uniquely explains. As in ULM, the score is the t-value of each pathway’s slope: positive means active, negative means inactive. (If the univariate-versus-multivariate distinction is fuzzy, the ULM walkthrough in [Part 16](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/) is the place to build that intuition first.)

Everything else — extracting the expression matrix, storing scores as a Seurat assay, scaling, and plotting — is exactly what you did in Part 16.

---

## Load the Environment and Data

Same libraries and the same object as Part 16. If you are starting a fresh R session, load the packages and the downsampled object.

```r
#-----------------------------------------------
# STEP 0: Libraries and the downsampled object (same as Part 16)
#-----------------------------------------------

library(decoupleR)
library(Seurat)
library(ggplot2)
library(patchwork)
library(dplyr)
library(tibble)
library(tidyr)
library(pheatmap)
library(RColorBrewer)

dir.create("plots/progeny", recursive = TRUE, showWarnings = FALSE)

seurat_obj <- readRDS("seurat_subset.rds")
seurat_obj <- JoinLayers(seurat_obj)
Idents(seurat_obj) <- "final_annotation"

# Detect whichever integrated UMAP you built in Part 3
reductions_present <- names(seurat_obj@reductions)
umap_reduction <- dplyr::case_when(
  "umap.harmony" %in% reductions_present ~ "umap.harmony",
  "umap.cca"     %in% reductions_present ~ "umap.cca",
  "umap.rpca"    %in% reductions_present ~ "umap.rpca",
  "umap.mnn"     %in% reductions_present ~ "umap.mnn",
  TRUE                                   ~ "umap"
)
```

---

## Step 1: Get the PROGENy Model

We download the human PROGENy weights, keeping the top 500 responsive genes per pathway (ranked by p-value). The `top = 500` value is the recommended default for single-cell data; bulk RNA-seq often uses a smaller, higher-confidence set such as `top = 100`.

```r
#-----------------------------------------------
# STEP 1: Download the PROGENy pathway-gene network
#-----------------------------------------------

# source = pathway, target = responsive gene,
# weight = signed responsiveness coefficient (continuous, not +1/-1)
net <- decoupleR::get_progeny(organism = "human", top = 500)

head(net)
```

**Expected output (first rows):**

```bash
# A tibble: 7,000 x 4
   source   target  weight  p_value
 1 Androgen TMPRSS2  11.5  2.38e-47
 2 Androgen NKX3-1   10.6  2.21e-44
 3 Androgen MBOAT2   10.5  4.63e-44
 4 Androgen KLK2     10.2  1.94e-40
 5 Androgen SARG     11.4  2.79e-40
 6 Androgen SLC38A4   7.36 1.25e-39
```

The structure mirrors CollecTRI, with one important difference: `weight` is a **continuous** responsiveness coefficient (a gene can be strongly or weakly responsive, positively or negatively), not the simple +1/-1 mode of regulation you saw in CollecTRI. That richer weighting is part of why PROGENy pairs naturally with the multivariate model.

> 

**Same download fragility as Part 16.** `get_progeny()` uses OmniPath, so it can hit the same errors documented in [Part 16, Step 3](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/). The same fix applies: obtain the network once on any working machine and read it from a CSV. To create it, run `write.csv(net, "progeny_human.csv", row.names = FALSE)` after the line above (or `dc.get_progeny(organism='human', top=500).to_csv('progeny_human.csv', index=False)` in Python), then in your analysis session simply:

```r
net <- read.csv("progeny_human.csv", stringsAsFactors = FALSE)
```

The columns are identical (`source`, `target`, `weight`, `p_value`), so everything below is unchanged. Mouse and rat weights are available too — pass `organism = "mouse"` or `"rat"`.

---

## Step 2: Infer Pathway Activity with the Multivariate Linear Model

Extract the normalized expression matrix exactly as in Part 16, then run `run_mlm`. Note the single meaningful code change from Part 16: the method is `run_mlm` and `.mor = "weight"` (PROGENy’s column) instead of `run_ulm` and `.mor = "mor"`.

```r
#-----------------------------------------------
# STEP 2: Run MLM to score pathway activity per cell
#-----------------------------------------------

# Seurat 5: LayerData() is the version-safe accessor for the normalized data
mat <- as.matrix(LayerData(seurat_obj, assay = "RNA", layer = "data"))

# Multivariate: all 14 pathways are fit jointly per cell
acts <- decoupleR::run_mlm(
  mat     = mat,
  net     = net,
  .source = "source",
  .target = "target",
  .mor    = "weight",
  minsize = 5
)

head(acts)
```

**Expected output (first rows):**

```bash
# A tibble: ... x 5
  statistic source   condition         score  p_value
1 mlm       Androgen AAACCC...-1       0.56  0.576
2 mlm       EGFR     AAACCC...-1       3.63  0.000290
3 mlm       Estrogen AAACCC...-1      -0.89  0.375
4 mlm       Hypoxia  AAACCC...-1       1.22  0.224
5 mlm       JAK-STAT AAACCC...-1      -1.02  0.308
6 mlm       MAPK     AAACCC...-1      -2.74  0.00619
```

As in Part 16, `condition` here is decoupleR’s generic name for a column of the matrix — a cell barcode, not the Healthy/Post-Treatment label. With only 14 pathways and ~7,262 cells, this runs in seconds on the downsampled object.

---

## Step 3: Store Pathway Scores as a Seurat Assay

Identical pattern to Part 16, so we move through it quickly: reshape the long results into a pathway-by-cell matrix, add it as a new assay (`pathwaysmlm`), switch to it, scale, and use the scaled values for plotting.

```r
#-----------------------------------------------
# STEP 3: Store activities in a 'pathwaysmlm' assay
#-----------------------------------------------

pathway_assay <- acts %>%
  tidyr::pivot_wider(id_cols = "source", names_from = "condition",
                     values_from = "score") %>%
  tibble::column_to_rownames("source") %>%
  as.matrix()

seurat_obj[["pathwaysmlm"]] <- CreateAssayObject(data = pathway_assay)
DefaultAssay(seurat_obj) <- "pathwaysmlm"

seurat_obj <- ScaleData(seurat_obj)
LayerData(seurat_obj, assay = "pathwaysmlm", layer = "data") <-
  LayerData(seurat_obj, assay = "pathwaysmlm", layer = "scale.data")
```

Your object now carries three assays: `RNA` (gene expression), `tfsulm` (transcription factor activity, from Part 16, if you saved it), and `pathwaysmlm` (pathway activity). Switching `DefaultAssay` lets you plot any of the three with the same Seurat functions.

---

## Step 4: Which Pathways Define Each Cell Type?

With only 14 pathways there is no need to rank by variability as we did for hundreds of transcription factors — we simply show all of them. The mean-activity heatmap is built exactly as in Part 16.

```r
#-----------------------------------------------
# STEP 4: Mean pathway activity per cell type
#-----------------------------------------------

pathway_long <- t(as.matrix(LayerData(seurat_obj, assay = "pathwaysmlm", layer = "data"))) %>%
  as.data.frame() %>%
  dplyr::mutate(cell_type = Idents(seurat_obj)) %>%
  tidyr::pivot_longer(cols = -cell_type, names_to = "source", values_to = "score") %>%
  dplyr::group_by(cell_type, source) %>%
  dplyr::summarise(mean_activity = mean(score), .groups = "drop")

heatmap_mat <- pathway_long %>%
  tidyr::pivot_wider(id_cols = "cell_type", names_from = "source",
                     values_from = "mean_activity") %>%
  tibble::column_to_rownames("cell_type") %>%
  as.matrix()

palette_cols <- grDevices::colorRampPalette(
  rev(RColorBrewer::brewer.pal(n = 11, name = "RdBu"))
)(100)

pheatmap::pheatmap(
  mat          = heatmap_mat,
  color        = palette_cols,
  border_color = "white",
  cellwidth    = 20,
  cellheight   = 20,
  treeheight_row = 20,
  treeheight_col = 20,
  main         = "Mean PROGENy pathway activity by cell type",
  filename     = "plots/progeny/01_pathway_activity_by_celltype.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/01_pathway_activity_by_celltype.png?resize=1024%2C544&ssl=1)

> 

**Reading the heatmap (actual result).** Color here is *scaled* (per-pathway z-scored) mean activity, so the heatmap shows cell-type **specificity** — which cell type is relatively most active for each pathway — rather than absolute level. **Classical Monocytes dominate**: their row is the reddest for most pathways, led by **NFkB** (by far the strongest single cell), then TGFb, Hypoxia, WNT, and EGFR. That is the expected myeloid inflammatory signature, and NFkB standing out is the headline. **B cells** peak for **Trail** (the apoptosis pathway), consistent with the decoupleR vignette’s own observation, while the T and NK rows stay pale — their pathway activity is not strongly differentiated by this resting-state signature.

Two honest subtleties are worth flagging, because they teach how to read PROGENy correctly. First, **TNFa looks muted in this scaled view even though it is one of the most active pathways overall.** Scaling centers each pathway, so a pathway that is uniformly high across *all* cells (as TNFa is here) shows little cell-type contrast. To see absolute level, look at the raw `run_mlm` scores: averaging `acts$score` per pathway shows JAK-STAT, TNFa, and Hypoxia as the most active pathways across the dataset, and p53 as strongly inactive (a large negative score in nearly every cell — expected, since p53 is a stress/damage pathway with little baseline activity in resting circulating immune cells). The scaled heatmap answers “where is each pathway *most* active,” not “how active is it.” Keep both views in mind.

---

## Step 5: Project a Pathway onto the UMAP

Switch nothing but the assay and the feature name. Here we visualize TNFa, one of the inflammatory pathways central to this dataset (the callout below shows why NFkB turns out to be the more cell-type-specific example).

```r
#-----------------------------------------------
# STEP 5: Project one pathway's activity onto the UMAP
#-----------------------------------------------

diverging_cols <- rev(RColorBrewer::brewer.pal(n = 11, name = "RdBu"))[c(2, 10)]

p_celltypes <- DimPlot(
  seurat_obj, reduction = umap_reduction,
  label = TRUE, repel = TRUE, pt.size = 0.1
) + NoLegend() + ggtitle("Cell types")

p_tnfa <- FeaturePlot(
  seurat_obj, features = "TNFa", reduction = umap_reduction
) +
  scale_colour_gradient2(low = diverging_cols[1], mid = "white",
                         high = diverging_cols[2]) +
  ggtitle("TNFa pathway activity")

p_pathway <- p_celltypes | p_tnfa
ggsave("plots/progeny/02_tnfa_activity_umap.png", p_pathway,
       width = 12, height = 6, dpi = 300)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/02_tnfa_activity_umap.png?resize=1024%2C512&ssl=1)

> 

**Reading the UMAP (actual result).** TNFa activity is **broadly distributed** across the map rather than concentrated in any one cluster — scattered red and blue throughout the T, NK, and monocyte regions, with B cells (lower left) leaning slightly low. This is the spatial version of the point from Step 4: TNFa is highly active almost everywhere, so its scaled signal looks diffuse rather than cell-type-specific. If you want a pathway that lights up one population cleanly, **NFkB** is the better example here — swap `"TNFa"` for `"NFkB"` and the Classical Monocytes cluster turns sharply red, matching the heatmap. Pathway names are case-sensitive (`"TNFa"`, `"NFkB"`, `"JAK-STAT"`); confirm with `rownames(seurat_obj[["pathwaysmlm"]])`.

---

## Step 6: Do the Pathways Track the Treatment Effect?

This is the payoff that connects back to Part 16. There, inflammatory transcription factor activity in Classical Monocytes fell after periodontal therapy. If that reflects genuinely reduced inflammatory signaling, the upstream inflammatory *pathways* should move the same way. We compute mean pathway activity per cell type and condition, focusing on the inflammatory set.

```r
#-----------------------------------------------
# STEP 6: Pathway activity by cell type x condition
#-----------------------------------------------

inflammatory <- c("TNFa", "NFkB", "JAK-STAT")
inflammatory <- inflammatory[inflammatory %in% rownames(seurat_obj[["pathwaysmlm"]])]

condition_long <- t(as.matrix(LayerData(seurat_obj, assay = "pathwaysmlm", layer = "data"))) %>%
  as.data.frame() %>%
  dplyr::mutate(
    cell_type = Idents(seurat_obj),
    condition = seurat_obj$condition
  ) %>%
  tidyr::pivot_longer(cols = c(-cell_type, -condition),
                      names_to = "source", values_to = "score") %>%
  dplyr::filter(source %in% inflammatory) %>%
  dplyr::group_by(cell_type, condition, source) %>%
  dplyr::summarise(mean_activity = mean(score), .groups = "drop")

condition_mat <- condition_long %>%
  dplyr::mutate(group = paste(cell_type, condition, sep = " :: ")) %>%
  tidyr::pivot_wider(id_cols = "group", names_from = "source",
                     values_from = "mean_activity") %>%
  tibble::column_to_rownames("group") %>%
  as.matrix()

pheatmap::pheatmap(
  mat          = condition_mat,
  color        = palette_cols,
  border_color = "white",
  cluster_rows = FALSE,
  cellwidth    = 22,
  cellheight   = 16,
  main         = "Inflammatory pathway activity: cell type x condition",
  filename     = "plots/progeny/03_pathway_activity_by_condition.png"
)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2026/06/03_pathway_activity_by_condition.png?resize=851%2C1024&ssl=1)

> 

**Reading the comparison.** This is the payoff, and it largely confirms Part 16. Look at the two Classical Monocytes rows: **NFkB pathway activity is highest in Healthy and clearly drops after treatment** (the darkest red cell on the map is `Classical Monocytes :: Healthy` at NFkB, fading to a lighter red in Post_Treatment). That is exactly the direction the inflammatory *transcription factors* moved in Part 16 — NFKB1 among them — so two independent footprint analyses, regulators and pathways, now point to the same conclusion in the same cell type: reduced monocyte NF-kB signaling after periodontal therapy. Every other cell type stays low and flat for these pathways, so the effect is monocyte-specific.

There is also an honest nuance worth keeping. **JAK-STAT moves the opposite way** — it is slightly *higher* in Classical Monocytes after treatment than before. Rather than contradicting the story, this fits the original study’s framing of treatment “rebalancing” the immune response: NF-kB-driven inflammation cools while cytokine (JAK-STAT) signaling shifts. And consistent with Step 5, TNFa stays low and undifferentiated in the scaled view. Because this is the downsampled object and these are group means, treat the NFkB result as a strong, well-corroborated hypothesis and confirm it with a pseudobulk differential test on the full dataset; report the JAK-STAT direction honestly rather than smoothing it over.

You can also export the results for reuse, exactly as in Part 16:

```
write.csv(acts, "plots/progeny/pathway_activity_per_cell.csv", row.names = FALSE)
saveRDS(seurat_obj, "plots/progeny/seurat_with_pathway_activity.rds")
```

---

## Best Practices for PROGENy Pathway Inference

**1. Always pair PROGENy with MLM, not ULM.** This is the central rule. The 14 pathways share responsive genes, so the multivariate model is required to avoid redundant, inflated scores. Reserve `run_ulm` for non-overlapping networks like CollecTRI.

**2. Use top = 500 for single-cell, fewer for bulk.** More responsive genes per pathway give the per-cell models enough signal to fit against sparse single-cell data. For bulk RNA-seq, a tighter `top = 100` of the highest-confidence genes is common.

**3. Read pathway activity alongside, not instead of, TF activity.** Pathways (Part 17) and transcription factors (Part 16) are complementary layers of the same signaling story. The strongest conclusions come from where they agree — as with the monocyte inflammatory program in this dataset.

**4. Respect the continuous weights.** Unlike CollecTRI’s +1/-1, PROGENy weights are graded responsiveness coefficients. Do not binarize them; pass the `weight` column directly to `.mor`.

**5. Reuse Part 16’s offline and memory practices.** The same OmniPath download fragility, the same benefit of the downsampled object, and the same “never subset genes” rule all carry over — see [Part 16’s Best Practices](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-16-build-gene-regulatory-networks-with-decoupler-and-collectri/).

---

## Conclusion: What You Built in Part 17

In a fraction of the code of Part 16, you added a complementary layer of biological insight. You:

1. **Learned the pathway-footprint idea** — inferring signaling pathway activity from perturbation-responsive genes, the upstream counterpart to TF activity.

2. **Understood why PROGENy needs the multivariate model** — overlapping responsive genes require fitting all 14 pathways jointly, which is what distinguishes `run_mlm` from `run_ulm`.

3. **Computed per-cell activity for 14 pathways** on the same downsampled dataset, and stored it as a third Seurat assay.

4. **Profiled pathway activity across cell types** and projected a pathway onto the UMAP.

5. **Found that the pathways corroborate the Part 16 treatment effect** — NF-kB pathway activity in Classical Monocytes drops after treatment, echoing the inflammatory transcription factors from Part 16, while JAK-STAT shifts the other way, hinting at immune rebalancing rather than blanket suppression.

## Beyond decoupleR: Explore the Saez-Lab Ecosystem

One of the quiet advantages of decoupleR is that it is not a standalone tool — it is the inference engine at the center of a larger, interoperable toolkit from the [Saez-Rodriguez lab](https://github.com/saezlab). Every tool below draws on the same OmniPath prior-knowledge backbone and the same tidy “source/target/weight” conventions you have now used twice, so the skills from Parts 16 and 17 transfer directly. If footprint analysis has been useful, these are well worth exploring:

- **DoRothEA** — the original signed TF regulon resource, accessible with `get_dorothea()` as a drop-in alternative to CollecTRI in the Part 16 workflow (it adds confidence-level tiers). A one-line swap to compare regulon resources.

- **OmniPath / OmnipathR** — the meta-database (180+ resources) that powers CollecTRI, PROGENy, DoRothEA, and the kinase-substrate networks. Worth understanding as the common foundation under all of these analyses.

- **Kinase activity** — if you also have phosphoproteomics, `get_ksn_omnipath()` gives a kinase-substrate network you can run through the very same `run_ulm`/`run_mlm` to infer kinase activities — footprint analysis one more layer up.

- **LIANA / LIANA+** — a consensus framework for ligand-receptor and cell-cell communication inference (the [liana](https://github.com/saezlab/liana) R package and [liana-py / LIANA+](https://github.com/saezlab/liana-py) in Python). It complements the CellChat analysis from [Part 9](https://ngs101.com/how-to-analyze-cell-cell-communication-in-single-cell-rna-seq-data-complete-beginners-guide-part-9-cellchat-analysis/) and connects naturally to pathway activity — did a sender cell’s ligand actually activate the receiver’s pathway?

- **CARNIVAL** — takes the TF activities (Part 16) and pathway scores (Part 17) you just computed and contextualizes them on a signed protein-protein network, reconstructing the signaling route that links a perturbation to the observed regulators. A natural “next level up” that consumes decoupleR’s outputs directly.

- **COSMOS** — causal reasoning across multi-omics (transcriptomics, phosphoproteomics, metabolomics), again using decoupleR-inferred activities as anchors. For groups generating more than one data type.

- **mistyR (MISTy)** — for spatial transcriptomics, models how cell-type composition relates to PROGENy pathway activities across a tissue slide — the spatial extension of exactly what you did in Step 4.

You do not need all of these, and you certainly do not need them at once. The point is that the footprint mindset you have built — prior knowledge plus a simple model equals interpretable activity — scales from transcription factors to pathways to kinases to cell communication to spatial context. Browse [github.com/saezlab](https://github.com/saezlab), pick the one closest to your next question, and you will find the conventions already familiar.

## References

1. Schubert M, Klinger B, Klunemann M, et al. Perturbation-response genes reveal signaling footprints in cancer gene expression. *Nature Communications*. 2018;9(1):20. doi:10.1038/s41467-017-02391-6 [PROGENy]

2. Holland CH, Tanevski J, Perales-Paton J, et al. Robustness and applicability of transcription factor and pathway analysis tools on single-cell RNA-seq data. *Genome Biology*. 2020;21(1):36. doi:10.1186/s13059-020-1949-z [PROGENy and TF tools in single cell]

3. Badia-i-Mompel P, Velez Santiago J, Braunger J, et al. decoupleR: ensemble of computational methods to infer biological activities from omics data. *Bioinformatics Advances*. 2022;2(1):vbac016. doi:10.1093/bioadv/vbac016 [decoupleR]

4. Turei D, Valdeolivas A, Gul L, et al. Integrated intra- and intercellular signaling knowledge for multicellular omics analysis. *Molecular Systems Biology*. 2021;17(3):e9923. doi:10.15252/msb.20209923 [OmniPath]

5. Lee H, Joo JY, Sohn DH, et al. Single-cell RNA sequencing reveals rebalancing of immunological response in patients with periodontitis after non-surgical periodontal therapy. *Journal of Translational Medicine*. 2022;20(1):504. doi:10.1186/s12967-022-03686-8 [Dataset source — GSE174609]

---

*This tutorial is part of the comprehensive NGS101.com single-cell RNA-seq analysis series for beginners.*
