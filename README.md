# 🧬 Learning Omics: Computational Biology & Proteomics Curriculum

[![Website](https://img.shields.io/badge/Website-GitHub%20Pages-blue.svg)](https://chrisraynerr.github.io/learning_omics/)
[![Quarto](https://img.shields.io/badge/Powered%20by-Quarto-447099.svg)](https://quarto.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

An open-source, hands-on curriculum designed for computational biologists, bioinformaticians, and translational data scientists navigating high-throughput sequencing, single-cell dynamics, and multi-thousand-plex plasma proteomics.

---

## 🌟 Key Highlights

* **Multi-Pillar Curriculum**:
  * **Transcriptomics**: Bulk RNA-seq count modeling, DESeq2, and Single-Cell RNA-seq (Seurat, pseudobulk, cellular niches).
  * **Epigenomics**: Single-cell ATAC-seq (TSS enrichment, TF-IDF clustering, transcription factor footprinting) and Hi-C chromatin conformation.
  * **Proteomics**: 15-part comprehensive tutorial series spanning discovery mass spectrometry (DIA-NN, MaxLFQ, MSstats) and targeted affinity platforms (Olink PEA, SomaScan .adat).
* **Real-World Clinical Trial Data**:
  * Features **PRIDE PAD000062** (~5,400 proteins on **Olink Explore HT**): Down Syndrome patients treated with the JAK inhibitor tofacitinib with matched Baseline vs. 16-week draws.
* **Interactive Virtual Laboratory**:
  * Standalone web application with live LOD sliders, internal control scatterplots, plate bridge normalization toggles, and patient trajectory inspectors (`interactive/olink_qc_lab.md`).
* **Curated Literature & Reading Compendiums**:
  * Modules include annotated reading lists integrating landmark methodology papers, textbooks, and Dr. Skanda Rajasundaram's *Genetics in Drug Development: A Curated Anthology*.

---

## 📂 Repository Layout

```
learning_omics/
├── _quarto.yml                     # Quarto website configuration & navigation
├── index.md                        # Course homepage & syllabus roadmap
├── .github/workflows/deploy.yml    # Automated GitHub Actions deployment to GitHub Pages
├── interactive/                    # Virtual lab & interactive dashboards
│   ├── olink_qc_lab.md             # Embedded lab page with guided exercises
│   └── olink_explore_interactive_qc_analysis.html  # Standalone interactive app
├── BulkRNAseq/                     # Bulk transcriptomics & reading list
├── scRNAseq/                       # Single-cell transcriptomics & reading list
├── scATACseq/                      # Single-cell chromatin accessibility & reading list
├── Proteomics/                     # 15-part curriculum & hands-on workbooks
│   ├── README.md                   # Proteomics curriculum index
│   ├── RECOMMENDED_READING.md      # Proteomics reading list & anthology
│   ├── 05b_olink_plasma_paired_qc_and_analysis_notebook.Rmd  # Executable Rmd
│   ├── 05b_olink_plasma_paired_qc_and_analysis_notebook.ipynb# Jupyter notebook
│   └── data/PAD000062_mapping.csv  # Real clinical trial metadata (PRIDE)
└── [Standalone Articles]          # Dozens of focused bioinformatics guides
```

---

## 🚀 How to Publish to Your GitHub Pages

Publishing this repository as a live website takes under 2 minutes:

### Step 1: Initialize Git and Commit
In your terminal:
```bash
cd /path/to/learning_omics

git init
git add .
git commit -m "feat: complete Learning Omics curriculum with interactive lab & website config"
```

### Step 2: Push to GitHub
Create a new public repository named `learning_omics` on GitHub, then push:
```bash
git branch -M main
git remote add origin https://github.com/chrisraynerr/learning_omics.git
git push -u origin main
```

### Step 3: Enable GitHub Pages in Repository Settings
1. Go to your repository on GitHub: `https://github.com/<your-username>/learning_omics/settings/pages`.
2. Under **Build and deployment** > **Source**, select **GitHub Actions** (or Deploy from a branch: `gh-pages` / `/root`).
3. That's it! GitHub Actions will run the workflow in `.github/workflows/deploy.yml` and publish your course website live at:
   ```
   https://<your-username>.github.io/learning_omics/
   ```

---

## 💻 Local Preview (Optional)

If you have Quarto installed locally:
```bash
# Preview the site locally with live reloading:
quarto preview

# Render static website:
quarto render
```

---

## 📄 License & Attribution

All instructional materials and code in this repository are open access under the MIT License. Data from **PRIDE PAD000062** is shared under the Creative Commons Public Domain (CC0) license courtesy of Dr. Joaquin M. Espinosa and the Linda Crnic Institute for Down Syndrome.

