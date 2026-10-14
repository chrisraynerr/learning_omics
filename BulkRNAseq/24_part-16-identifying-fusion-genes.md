# How to Analyze RNAseq Data for Absolute Beginners Part 16: A Comprehensive Tutorial on Identifying Fusion Genes

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/)

- [Understanding Fusion Genes: Key Concepts for Cancer Research](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Understanding_Fusion_Genes_Key_Concepts_for_Cancer_Research)

  - [What Are Fusion Genes and Why Do They Matter?](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#What_Are_Fusion_Genes_and_Why_Do_They_Matter)
  - [The Critical Role of Fusion Genes in Cancer Development](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#The_Critical_Role_of_Fusion_Genes_in_Cancer_Development)
  - [Why RNA-seq for Fusion Gene Detection?](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Why_RNA-seq_for_Fusion_Gene_Detection)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Setting_Up_Your_Analysis_Environment)

  - [Required Software Installation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Required_Software_Installation)
  - [Reference File Preparation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Reference_File_Preparation)
  - [Example Dataset Preparation](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Example_Dataset_Preparation)

- [The Analysis Workflow: From Raw Data to Fusion Detection](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#The_Analysis_Workflow_From_Raw_Data_to_Fusion_Detection)

  - [Step 1: Quality Control and Preprocessing](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Step_1_Quality_Control_and_Preprocessing)
  - [Step 2: Alignment with STAR](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Step_2_Alignment_with_STAR)
  - [Step 3: Fusion Gene Detection with STAR-Fusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Step_3_Fusion_Gene_Detection_with_STAR-Fusion)

- [Step 4: Examining Protein Coding Status with FusionInspector](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Step_4_Examining_Protein_Coding_Status_with_FusionInspector)
- [Conclusion: The Future of Fusion Gene Detection](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Conclusion_The_Future_of_Fusion_Gene_Detection)
- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/#Like_this)

## Understanding Fusion Genes: Key Concepts for Cancer Research

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/cells-12-02467-g001.png?resize=1024%2C592&ssl=1)

### What Are Fusion Genes and Why Do They Matter?

Fusion genes represent a fascinating phenomenon in cancer biology where two previously separate genes join together, often creating proteins with altered or entirely new functions. These genetic mergers typically arise through chromosomal rearrangements like translocations, deletions, or inversions. While some fusion genes occur naturally in healthy cells (particularly in germline development), their aberrant formation often signals potential cancer development.

Consider fusion genes as molecular switches gone wrong – when two genes incorrectly fuse, they can create proteins that either lose their normal “off” switch or gain inappropriate new functions. The classic example is the BCR-ABL1 fusion in chronic myeloid leukemia (CML), where the resulting fusion protein acts like a car with a stuck accelerator, driving continuous cell growth.

### The Critical Role of Fusion Genes in Cancer Development

Fusion genes can influence cancer development through multiple mechanisms:

- Creating constitutively active signaling proteins

- Disrupting normal cellular regulation

- Generating novel proteins with cancer-promoting functions

- Serving as diagnostic and prognostic markers

Notable examples include:

- BCR-ABL1 in chronic myeloid leukemia

- TMPRSS2-ERG in prostate cancer

- ETV6-NTRK3 in various pediatric cancers

### Why RNA-seq for Fusion Gene Detection?

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-16-at-9.39.04%E2%80%AFPM.png?resize=1024%2C116&ssl=1)

RNA sequencing has revolutionized how we detect and study fusion genes. Unlike traditional methods that look at DNA, RNA-seq focuses on actively expressed genes, offering several unique advantages:

- Captures actively expressed fusion transcripts

- Provides higher sensitivity than DNA-based methods

- Enables discovery of both known and novel fusions

- Allows quantification of fusion transcript expression levels

## Setting Up Your Analysis Environment

### Required Software Installation

First, we’ll build upon our previous RNA-seq environment (if you haven’t set this up yet, please refer to our [RNA-seq basics tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-1-setting-up-the-environment/)). Here’s how we’ll enhance it for fusion gene detection:

```bash
# Activate the RNA-seq environment
conda activate rnaseq_env

# Install STAR-Fusion and dependencies
conda install -c bioconda star-fusion
```

### Reference File Preparation

Proper reference files are crucial for accurate fusion detection. We’ll use the human reference genome (hg38) and associated annotations.

```bash
# Create analysis directories
mkdir -p ~/Fusion_Detection/{STAR_hg38,raw,trimmed,aligned,star_fusion_outdir}

# Download STAR index files
cd ~/Fusion_Detection/STAR_hg38

# Define base URL for downloads
base_url="http://awspds.refgenie.databio.org/refgenomes.databio.org/2230c535660fb4774114bfa966a62f823fdb6d21acf138d4/star_index__default"

# Download required index files
files=(
    "chrLength.txt"
    "chrName.txt"
    "chrNameLength.txt"
    "chrStart.txt"
    "Genome"
    "genomeParameters.txt"
    "SA"
    "SAindex"
)

for file in "${files[@]}"; do
    wget "$base_url/$file"
done

# Download and prepare annotation files
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_47/gencode.v47.basic.annotation.gtf.gz

# Download CTAT Fusion library
cd ~/Fusion_Detection/
wget https://data.broadinstitute.org/Trinity/CTAT_RESOURCE_LIB/__genome_libs_StarFv1.10/GRCh38_gencode_v37_CTAT_lib_Mar012021.plug-n-play.tar.gz
tar zxvf GRCh38_gencode_v37_CTAT_lib_Mar012021.plug-n-play.tar.gz
```

### Example Dataset Preparation

For this tutorial, we’re using a particularly interesting dataset from a fusion-positive rhabdomyosarcoma (FP-RMS) cell line ( [GSE279335](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE279335)). This cell line contains known fusion genes (PAX3-FOXO1 and MARS-AVIL), making it perfect for learning fusion detection techniques while having built-in positive controls.

```bash
# Download example data
cd ~/Fusion_Detection/raw
fasterq-dump SRR30961741

# Standardize file naming
rename _1.fastq _R1_001.fastq *_1.fastq
rename _2.fastq _R2_001.fastq *_2.fastq

# Compress files
gzip *.fastq
```

## The Analysis Workflow: From Raw Data to Fusion Detection

### Step 1: Quality Control and Preprocessing

First, we’ll ensure our raw data meets quality standards:

```bash
# Create output directory
mkdir ~/Fusion_Detection/trimmed/SRR30961741/

# Trim adapters and low-quality bases
trim_galore --fastqc \
    --paired \
    --cores 8 \
    ~/Fusion_Detection/raw/SRR30961741_R1_001.fastq.gz \
    ~/Fusion_Detection/raw/SRR30961741_R2_001.fastq.gz \
    -o ~/Fusion_Detection/trimmed/SRR30961741/
```

### Step 2: Alignment with STAR

We’ll use STAR aligner with specific parameters optimized for fusion detection:

```bash
# Create alignment output directory
mkdir ~/Fusion_Detection/aligned/SRR30961741/

# Run STAR alignment
STAR --genomeDir ~/Fusion_Detection/STAR_hg38/ \
    --runThreadN 20 \
    --readFilesIn \
        ~/Fusion_Detection/trimmed/SRR30961741/SRR30961741_R1_001_val_1.fq.gz \
        ~/Fusion_Detection/trimmed/SRR30961741/SRR30961741_R2_001_val_2.fq.gz \
    --outSAMtype BAM SortedByCoordinate \
    --outSAMunmapped Within \
    --outSAMattributes Standard \
    --readFilesCommand zcat \
    --outFileNamePrefix ~/Fusion_Detection/aligned/SRR30961741/SRR30961741_trimmed \
    --outReadsUnmapped None \
    --twopassMode Basic \
    --outSAMstrandField intronMotif \
    --chimSegmentMin 12 \
    --chimJunctionOverhangMin 8 \
    --chimOutJunctionFormat 1 \
    --alignSJDBoverhangMin 10 \
    --alignMatesGapMax 100000 \
    --alignIntronMax 100000 \
    --alignSJstitchMismatchNmax 5 -1 5 5 \
    --outSAMattrRGline ID:GRPundef \
    --chimMultimapScoreRange 3 \
    --chimScoreJunctionNonGTAG -4 \
    --chimMultimapNmax 20 \
    --chimNonchimScoreDropMin 10 \
    --peOverlapNbasesMin 12 \
    --peOverlapMMp 0.1 \
    --alignInsertionFlush Right \
    --alignSplicedMateMapLminOverLmate 0 \
    --alignSplicedMateMapLmin 30
```

### Step 3: Fusion Gene Detection with STAR-Fusion

Now let’s identify fusion events. STAR-Fusion analyzes the evidence gathered during alignment to find and characterize fusion events:

```bash
# Create output directory
mkdir ~/Fusion_Detection/star_fusion_outdir/SRR30961741/

# Run STAR-Fusion
STAR-Fusion \
    --genome_lib_dir ~/Fusion_Detection/GRCh38_gencode_v37_CTAT_lib_Mar012021.plug-n-play/ctat_genome_lib_build_dir \
    -J ~/Fusion_Detection/aligned/SRR30961741/SRR30961741_trimmedChimeric.out.junction \
    --output_dir ~/Fusion_Detection/star_fusion_outdir/SRR30961741
```

**Note for Enhanced Sensitivity:** If you want more sensitivity in detecting fusion genes, use the `--max_sensitivity` option when running STAR-Fusion. This will increase the detection of potential fusion events but may also increase false positives, so consider this trade-off based on your research needs.

The STAR-Fusion output “star-fusion.fusion_predictions.tsv” has the following format:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/01/Screenshot-2025-01-16-at-9.29.11%E2%80%AFPM.png?resize=1024%2C178&ssl=1)

As we expected, the top fusion genes detected in our example are MARS1-AVIL and PAX3-FOXO1. The details of the output can be found in the [STAR-Fusion documentation](https://github.com/STAR-Fusion/STAR-Fusion/wiki#output-from-star-fusion).

- **FusionName:** The name of the predicted gene fusion.

- **JunctionReadCount:** The number of sequencing reads that directly span the fusion junction.

- **SpanningFragCount:** The number of read pairs or fragments that span the fusion breakpoint but do not directly align to the junction.

- **SpliceType:** Describes the type of splicing at the fusion junction.

- **LeftGene:** The 5’ gene involved in the fusion, including its transcript annotation.

- **LeftBreakpoint:** The genomic location of the breakpoint in the 5’ gene.

- **RightGene:** The 3’ gene involved in the fusion, including its transcript annotation.

- **RightBreakpoint:** The genomic location of the breakpoint in the 3’ gene.

- **JunctionReads:** A comma-separated list of read names that directly span the fusion junction.

- **SpanningFrags:** A comma-separated list of read names supporting spanning fragments.

- **LargeAnchorSupport:** Indicates the level of anchor support for the fusion junction.

- **FFPM (Fusion Fragments Per Million):** A normalized metric representing the abundance of the fusion event.

- **LeftBreakDinuc:** The two-base (dinucleotide) sequence at the breakpoint of the 5’ gene.

- **LeftBreakEntropy:** Measures the sequence entropy around the 5’ breakpoint. High entropy suggests randomness and biological relevance, while low entropy may indicate artifacts.

- **RightBreakDinuc:** The two-base (dinucleotide) sequence at the breakpoint of the 3’ gene.

- **RightBreakEntropy:** Measures the sequence entropy around the 3’ breakpoint.

- **annots:** Provides annotation information for the fusion, such as whether it is known from databases (e.g., COSMIC) or identified as an oncogenic fusion.

## Step 4: Examining Protein Coding Status with FusionInspector

After detecting fusion genes with STAR-Fusion, it’s crucial to examine the protein coding status of the identified fusions. This step helps determine whether the fusion events are likely to produce functional fusion proteins, which is essential for understanding their biological significance.

We’ll use FusionInspector to perform a more detailed analysis of the fusion predictions, including examination of coding effects:

```bash
# Run FusionInspector to examine protein coding status
FusionInspector \
    --fusions star-fusion.fusion_predictions.abridged.tsv \
    --genome_lib_dir /path/to/GRCh38_gencode_v37_CTAT_lib_build_dir \
    --CPU 16 \
    --left_fq ~/Fusion_Detection/trimmed/SRR30961741/SRR30961741_R1_001_val_1.fq.gz  \
    --right_fq ~/Fusion_Detection/trimmed/SRR30961741/SRR30961741_R2_001_val_2.fq.gz  \
    --output_dir ~/Fusion_Detection/star_fusion_outdir/SRR30961741 \
    --out_prefix finspector \
    --annotate \
    --examine_coding_effect
```

**Key Parameters Explained:**

- `--fusions`: Input file containing the fusion predictions from STAR-Fusion

- `--genome_lib_dir`: Path to the CTAT genome library directory

- `--CPU`: Number of CPU cores to use for processing

- `--left_fq` and `--right_fq`: Your trimmed paired-end FASTQ files

- `--output_dir`: Directory where FusionInspector results will be saved

- `--out_prefix`: Prefix for output files

- `--annotate`: Enables comprehensive annotation of fusion breakpoints

- `--examine_coding_effect`: **Critical parameter** that analyzes whether fusions maintain or disrupt protein coding sequences

**What the Coding Effect Analysis Provides:**

The `--examine_coding_effect` option will generate detailed information about:

- **In-frame vs. out-of-frame fusions**: Whether the fusion maintains the correct reading frame

- **Protein domain preservation**: Which protein domains from each partner gene are retained

- **Coding sequence integrity**: Whether the fusion is predicted to produce a functional protein

- **Breakpoint location relative to coding sequences**: Position of fusion breakpoints within genes

**Output Files to Examine:**

After running FusionInspector, look for these key output files:

- `finspector.fusion_predictions.final.abridged.FFPM`: Final fusion predictions with additional annotations

- `finspector.gmap_trinity_GG.fusions.gff3`: Detailed structural annotations

- `finspector.bed`: BED format file for genome browser visualization

- Coding effect summaries in the main results directory

This analysis is particularly important for identifying fusions that are likely to be oncogenic drivers versus passenger events in cancer samples.

## Conclusion: The Future of Fusion Gene Detection

As we’ve explored in this tutorial, detecting fusion genes from RNA-seq data is a powerful approach in cancer research, but it requires careful attention to detail and a solid understanding of both the biological and computational aspects of the analysis. The field continues to evolve, with new tools and methods emerging regularly.

Remember that fusion gene detection is not just about running a pipeline – it’s about understanding the biology behind these important cancer drivers and using that knowledge to inform your analysis decisions. As you apply these methods to your own research, keep in mind that each dataset may present unique challenges and might require adjustments to the standard workflow.

Looking ahead, the field of fusion gene detection is moving toward even more sophisticated approaches, including machine learning-based methods and long-read sequencing technologies. Stay current with these developments, as they may offer new opportunities for discovering and characterizing fusion genes in cancer.

## References

- Taniue K, Akimitsu N. Fusion Genes and RNAs in Cancer Development. *Non-Coding RNA*. 2021; 7(1):10.

- Heyer EE, et al. Diagnosis of fusion genes using targeted RNA sequencing. *Nat Commun* 10, 1388 (2019).

- Panicker S, Chengizkhan G, Gor R, Ramachandran I, Ramalingam S. Exploring the Relationship between Fusion Genes and MicroRNAs in Cancer. *Cells*. 2023; 12(20):2467. https://doi.org/10.3390/cells12202467
