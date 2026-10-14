# How to Analyze RNAseq Data for Absolute Beginners Part 13: Circular RNAseq Analysis

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/)

- [Understanding the Biology of Circular RNAs](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Understanding_the_Biology_of_Circular_RNAs)

  - [The Nature of Circular RNAs](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#The_Nature_of_Circular_RNAs)
  - [The Biological Significance of CircRNAs](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#The_Biological_Significance_of_CircRNAs)
  - [CircRNAs in Disease](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#CircRNAs_in_Disease)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Setting_Up_Your_Analysis_Environment)
- [Preparing Annotation Files and Genome Index](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Preparing_Annotation_Files_and_Genome_Index)
- [Circular RNA Analysis with CIRCexplorer2](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Circular_RNA_Analysis_with_CIRCexplorer2)

  - [Step-by-Step Analysis Protocol](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Step-by-Step_Analysis_Protocol)

- [Understanding Your Analysis Options: CIRCexplorer2 and CIRCexplorer3](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Understanding_Your_Analysis_Options_CIRCexplorer2_and_CIRCexplorer3)

  - [CIRCexplorer2’s Flexible Approach](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#CIRCexplorer2s_Flexible_Approach)
  - [CIRCexplorer3’s Specialized Features](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#CIRCexplorer3s_Specialized_Features)

- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Conclusion)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-13-circular-rnaseq-analysis/#Like_this)

## Understanding the Biology of Circular RNAs

### The Nature of Circular RNAs

Circular RNAs (circRNAs) represent one of molecular biology’s most fascinating discoveries. Unlike the linear RNA molecules that dominated our understanding of gene expression for decades, circRNAs form continuous loops through a unique process called back-splicing. In this process, a downstream 5′ splice site connects to an upstream 3′ splice site, creating a covalently closed circle that defies our traditional understanding of RNA processing.

This unusual structure gives circRNAs remarkable properties. Without free ends, they’re resistant to the exonucleases that typically degrade linear RNAs. This stability isn’t just a curious feature – it’s a fundamental property that cells have evolved to exploit for long-term gene regulation and cellular memory.

### The Biological Significance of CircRNAs

The discovery of widespread circRNA expression has revolutionized our understanding of gene regulation. These molecules serve multiple functions that we’re only beginning to understand:

1. **MicroRNA Regulation**: Many circRNAs act as molecular sponges, binding and sequestering microRNAs to fine-tune gene expression. This mechanism allows cells to create complex regulatory networks where circRNAs compete with messenger RNAs for microRNA binding.

2. **Protein Interactions**: Some circRNAs serve as scaffolds, bringing proteins together into functional complexes. This role is particularly important in cellular signaling and transcriptional regulation.

3. **Protein Coding**: Breaking with traditional views of non-coding RNAs, some circRNAs can actually be translated into proteins. These peptides often have unique functions distinct from those produced by canonical linear mRNAs.

### CircRNAs in Disease

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/1-s2.0-S1043661821003509-ga1_lrg.jpg?resize=1024%2C713&ssl=1)

The stability and tissue-specific expression of circRNAs make them particularly relevant to disease processes:

**Cancer Biology**:In cancer, circRNAs often show dramatic changes in expression. Some act as oncogenes by sponging tumor-suppressor microRNAs, while others function as tumor suppressors. Their stable presence in blood makes them promising biomarkers for cancer diagnosis and monitoring.

**Neurological Disorders**:The brain expresses an exceptionally diverse array of circRNAs. In conditions like Alzheimer’s disease, specific circRNAs show altered expression patterns that may contribute to neurodegeneration. Understanding these changes could lead to new therapeutic strategies.

**Cardiovascular Disease**:CircRNAs play crucial roles in heart development and function. During cardiac stress or injury, certain circRNAs change their expression patterns, suggesting potential therapeutic targets for heart disease.

## Setting Up Your Analysis Environment

Building upon our experience from previous RNA-seq analysis tutorials in this series, we’ll expand our bioinformatics environment to include specialized tools for circular RNA analysis. If you haven’t yet set up a basic RNA-seq environment, you may want to review our [earlier tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/) on RNA-seq basics before proceeding. 

```bash
# Activate our RNA-seq environment
conda activate rnaseq_env

# Install the core analysis toolkit
conda install -c bioconda circexplorer2 -y
```

## Preparing Annotation Files and Genome Index

We’ll need gene annotation files and genome index for our analysis. 

```bash
# Download human reference files (mm10 for mouse)
fetch_ucsc.py hg38 ref hg38_ref.txt    # RefSeq annotations
fetch_ucsc.py hg38 kg hg38_kg.txt      # KnownGenes annotations
fetch_ucsc.py hg38 fa hg38.fa          # Reference genome

# Download STAR Index components for human genome from refgenie
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/chrLength.txt && \
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/chrName.txt && \
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/chrNameLength.txt && \
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/chrStart.txt && \
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/Genome && \
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/genomeParameters.txt && \
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/SA && \
wget http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default/SAindex
```

## Circular RNA Analysis with [CIRCexplorer2](https://github.com/YangLab/CIRCexplorer2)

### Step-by-Step Analysis Protocol

1. **Quality Control and Adapter Trimming**

```
trim_galore --fastqc --paired --cores 20 \
    ~/raw/Sample1_L001_R1_001.fastq.gz \
    ~/raw/Sample1_L001_R2_001.fastq.gz \
    -o ~/Trimmed/Sample1/
```

1. **Genome Alignment with STAR**

```
STAR --genomeDir ~/Genome_Index/STAR_GRCH38/ \
    --runThreadN 20 \
    --readFilesIn ~/Trimmed/Sample1/Sample1_L001_R1_001_val_1.fq.gz \
                  ~/Trimmed/Sample1/Sample1_L001_R1_001_val_2.fq.gz \
    --chimSegmentMin 10 \
    --readFilesCommand zcat \
    --outFileNamePrefix ~/aligned/Sample1/Sample1_L001_R1_001_trimmed
```

1. **Circular RNA Detection and Annotation**

```
fast_circ.py parse \
    -r ~/ref/hg38/hg38_kg.txt \
    -g ~/ref/hg38/hg38.fa \
    -t STAR \
    -o ~/aligned/Sample1/fast_circ_parse \
    ~/aligned/Sample1/Sample1_L001_R1_001_trimmedChimeric.out.junction
```

Make sure to repeat the process for all your samples.

The resulted circularRNA_known.txt file contains the following information:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-21-at-9.43.35%E2%80%AFPM.png?resize=1024%2C142&ssl=1)

The annotation of each columns are shown below:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-21-at-9.49.42%E2%80%AFPM.png?resize=1024%2C468&ssl=1)

## Understanding Your Analysis Options: [CIRCexplorer2](https://github.com/YangLab/CIRCexplorer2?tab=readme-ov-file) and [CIRCexplorer3](https://github.com/YangLab/CLEAR)

While both CIRCexplorer2 and CIRCexplorer3 are powerful tools for circular RNA analysis, each offers distinct advantages and challenges that are worth understanding before you begin your analysis journey.

### CIRCexplorer2’s Flexible Approach

CIRCexplorer2 provides two paths for analysis: a streamlined one-command process and our recommended three-step protocol. While the one-command option might seem appealing at first glance, it comes with several important considerations. This approach requires additional computational resources and setup time, as it needs both HISAT2 and Bowtie aligners along with their corresponding genome indices. More significantly, the processing time for this method can be substantially longer than the three-step approach in this tutorial.

Our recommended three-step protocol offers a more efficient and manageable workflow. By breaking the analysis into distinct stages – quality control, alignment, and circular RNA detection – you gain better control over each step and can more easily troubleshoot if issues arise. This approach also builds upon the STAR aligner that many researchers already use for standard RNA-seq analysis, making it a natural extension of existing workflows.

### CIRCexplorer3’s Specialized Features

CIRCexplorer3, through its CLEAR pipeline, introduces an innovative approach by enabling direct comparisons between circular and linear RNA expression. This capability makes it particularly valuable for studies focusing on the relationship between circular RNAs and their linear counterparts. However, it’s important to note that setting up CIRCexplorer3 can be challenging due to its dependence on legacy bioinformatics tools.

When should you choose CIRCexplorer3? If your research specifically requires comprehensive analysis of the interplay between circular and linear RNA expression, the additional setup complexity may be worthwhile. The tool excels in situations where you need to:

- Compare expression patterns between circular and linear RNA forms

- Investigate the regulation of back-splicing versus canonical splicing

- Study the competition between these two RNA processing paths

For most standard circRNA studies, CIRCexplorer2 provides all the necessary functionality with a more straightforward setup process. Its robust detection algorithms and well-documented workflow make it an excellent choice for researchers beginning their journey into circular RNA analysis.

## Conclusion

The analysis of circular RNAs represents a perfect example of how biological discovery drives technological innovation, which in turn enables deeper biological insights. The methods and tools we’ve covered in this tutorial provide a solid foundation for your own explorations into this fascinating aspect of RNA biology. Whether you’re studying disease mechanisms, developing biomarkers, or exploring fundamental biology, understanding circRNA analysis is increasingly essential for modern molecular biology research.

Remember that while the technical aspects of circRNA analysis are important, the ultimate goal is to contribute to our understanding of biological systems and human health. As you apply these methods to your own research questions, stay curious and be ready to adapt as new tools and insights emerge in this rapidly evolving field.

## References

Misir, S., Wu, N. & Yang, B.B. (2022). Specific expression and functions of circular RNAs. Cell Death Differ 29, 481–491.Alyaa Dawoud, Zeina Ihab Zakaria, Hannah Hisham Rashwan, Maria Braoudaki, Rana A. Youness,Circular RNAs: New layer of complexity evading breast cancer heterogeneity, Non-coding RNA Research, Volume 8, Issue 1, 2023, Pages 60-74, ISSN 2468-0540,https://doi.org/10.1016/j.ncrna.2022.09.011.Amit Kumar Rai, Brooke Lee, Carleigh Hebbard, Shizuka Uchida, Venkata Naga Srikanth Garikipati, Decoding the complexity of circular RNAs in cardiovascular disease, Pharmacological Research, Volume 171, 2021, 105766, ISSN 1043-6618, https://doi.org/10.1016/j.phrs.2021.105766.Zhang XO, et al. (2016). Diverse alternative back-splicing and alternative splicing landscape of circular RNAs. Genome Res, 26:1277-1287.Ma XK, et al. (2019). A CLEAR pipeline for direct comparison of circular and linear RNA expression. bioRxiv doi: 10.1101/668657
