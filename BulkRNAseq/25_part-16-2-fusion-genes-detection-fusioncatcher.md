# How to Analyze RNA-seq Data for Absolute Beginners Part 16-2: Fusion Gene Detection with FusionCatcher

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Transcriptomics, bulk RNA-seq
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/)

---

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/)

- [Introduction: Advanced Fusion Detection for Cancer Research](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Introduction_Advanced_Fusion_Detection_for_Cancer_Research)

  - [What Makes FusionCatcher Essential for Cancer Research?](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#What_Makes_FusionCatcher_Essential_for_Cancer_Research)
  - [Understanding the Multi-Aligner Strategy](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Understanding_the_Multi-Aligner_Strategy)
  - [Biological Intelligence Built-In](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Biological_Intelligence_Built-In)

- [Setting Up Your FusionCatcher Environment](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Setting_Up_Your_FusionCatcher_Environment)

  - [Installing FusionCatcher](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Installing_FusionCatcher)
  - [Building the Reference Database](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Building_the_Reference_Database)

- [Preparing Your Dataset](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Preparing_Your_Dataset)

  - [Using Consistent Test Data](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Using_Consistent_Test_Data)

- [Running FusionCatcher Analysis](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Running_FusionCatcher_Analysis)

  - [Executing the Fusion Detection Pipeline](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Executing_the_Fusion_Detection_Pipeline)
  - [Understanding the Analysis Process](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Understanding_the_Analysis_Process)

- [Interpreting FusionCatcher Results](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Interpreting_FusionCatcher_Results)

  - [Key Output Files Explained](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Key_Output_Files_Explained)
  - [Understanding the Results Format](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Understanding_the_Results_Format)
  - [Analyzing Expected Results](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Analyzing_Expected_Results)

- [Comparing STAR-Fusion vs FusionCatcher](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Comparing_STAR-Fusion_vs_FusionCatcher)

  - [When to Use Each Tool](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#When_to_Use_Each_Tool)
  - [Complementary Analysis Strategy](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Complementary_Analysis_Strategy)

- [Advanced Analysis and Troubleshooting](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Advanced_Analysis_and_Troubleshooting)

  - [Data Quality Requirements](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Data_Quality_Requirements)
  - [Validation Strategies](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Validation_Strategies)

- [Best Practices for Clinical Applications](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Best_Practices_for_Clinical_Applications)

  - [Quality Assurance Workflow](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Quality_Assurance_Workflow)

- [Conclusion: Mastering Comprehensive Fusion Detection](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Conclusion_Mastering_Comprehensive_Fusion_Detection)

  - [The Power of Multi-Tool Expertise](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#The_Power_of_Multi-Tool_Expertise)
  - [Future Directions](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Future_Directions)
  - [Final Recommendations](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Final_Recommendations)

- [References](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#References)

  - [Essential Resources](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Essential_Resources)
  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-2-fusion-genes-detection-with-fusioncatcher/#Like_this)

## Introduction: Advanced Fusion Detection for Cancer Research

Building on our previous exploration of fusion gene detection with STAR-Fusion ([Part 16](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/)), we now delve into FusionCatcher, a specialized tool that has become the gold standard for detecting somatic fusion genes in cancer samples. While both tools excel at fusion detection, FusionCatcher offers unique capabilities that make it indispensable for oncology research.

### What Makes FusionCatcher Essential for Cancer Research?

FusionCatcher isn’t just another fusion detection tool – it’s a comprehensive system specifically designed for identifying cancer-relevant gene fusions. Here’s what sets it apart:

**Exceptional Validation Rate**: FusionCatcher boasts an excellent RT-PCR validation rate, meaning the fusions it identifies are more likely to be real and biologically relevant.

**Somatic Focus**: Unlike general-purpose fusion detectors, FusionCatcher specifically targets novel and known somatic fusion genes, translocations, and chimeras found in diseased samples.

**Challenging Fusion Detection**: The tool excels at detecting difficult-to-find fusions including IGH, CIC, DUX4, CRLF2, and TCF3 fusions that other tools might miss.

### Understanding the Multi-Aligner Strategy

FusionCatcher’s power comes from its sophisticated multi-aligner approach, which combines three different alignment strategies:

1. **BOWTIE Alignment**: Relies on precise annotation matching, finding fusions when junction points align perfectly with known exon borders

2. **BLAT Alignment**: Detects fusions where junction points fall within exons or introns, even with incomplete annotations

3. **STAR Alignment**: Provides splice-aware sensitivity for complex fusion events

> 

**Think of it this way**: If you’re trying to solve a complex puzzle, using one approach is like trying to complete it with only the corner pieces. FusionCatcher’s multi-aligner strategy is like having corner pieces, edge pieces, and interior pieces all working together – you get a much clearer picture of the complete fusion landscape.

### Biological Intelligence Built-In

What truly distinguishes FusionCatcher is its integration of extensive biological knowledge:

- **False Positive Filtering**: Uses databases of known fusions found in healthy samples to eliminate likely artifacts

- **Pseudogene Recognition**: Automatically filters out gene-pseudogene fusions that often represent technical artifacts

- **Read-through Detection**: Can optionally filter out adjacent gene read-throughs that aren’t true fusions

- **Oncogene Prioritization**: Leverages known oncogene databases to highlight clinically relevant findings

## Setting Up Your FusionCatcher Environment

### Installing FusionCatcher

Let’s create a dedicated environment for FusionCatcher analysis:

```bash
#-----------------------------------------------
# STEP 1: Create FusionCatcher environment
#-----------------------------------------------

# Create a new conda environment for FusionCatcher
conda create -p ~/Env_FusionCatcher -y fusioncatcher

# Activate the environment
conda activate ~/Env_FusionCatcher

# Verify installation
fusioncatcher --help
```

> 

**Installation Note**: The conda installation method is relatively new. If you encounter compatibility issues or missing package errors, refer to the alternative installation methods provided by the authors on their [GitHub repository](https://github.com/ndaniel/fusioncatcher/tree/master).

### Building the Reference Database

FusionCatcher requires species-specific reference databases. Here’s how to set them up:

```bash
#-----------------------------------------------
# STEP 2: Build FusionCatcher reference databases
#-----------------------------------------------

# For human samples (most common in cancer research)
echo "Building human reference database..."
fusioncatcher-build -g homo_sapiens -o ~/fusioncatcher_Index_human

# For mouse samples (common in research models)
echo "Building mouse reference database..."
fusioncatcher-build -g mus_musculus -o ~/fusioncatcher_Index_mouse

# For rat samples (less common but available)
echo "Building rat reference database..."
fusioncatcher-build -g rattus_norvegicus -o ~/fusioncatcher_Index_rat

# Alternative: Download pre-built human database (faster option)
echo "Downloading pre-built human database as alternative..."
mkdir -p ~/fusioncatcher_Index_human_prebuilt
cd ~/fusioncatcher_Index_human_prebuilt

# Download the multi-part compressed database
wget http://sourceforge.net/projects/fusioncatcher/files/data/human_v102.tar.gz.aa
wget http://sourceforge.net/projects/fusioncatcher/files/data/human_v102.tar.gz.ab
wget http://sourceforge.net/projects/fusioncatcher/files/data/human_v102.tar.gz.ac
wget http://sourceforge.net/projects/fusioncatcher/files/data/human_v102.tar.gz.ad

# Extract the complete database
cat human_v102.tar.gz.* | tar xz

# Create a symbolic link for easy access
ln -s human_v102 current

echo "Database setup complete!"
```

> 

**Database Choice**: Building from source gives you the latest annotations, while downloading pre-built databases is much faster. For most applications, the pre-built option is sufficient.

## Preparing Your Dataset

### Using Consistent Test Data

For direct comparison with our STAR-Fusion results from [Part 16](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-16-a-comprehensive-tutorial-on-identifying-fusion-genes/), we’ll analyze the same rhabdomyosarcoma dataset known to contain the PAX3-FOXO1 fusion:

```bash
#-----------------------------------------------
# STEP 3: Prepare analysis dataset
#-----------------------------------------------

# Create organized project directory
mkdir -p ~/FusionCatcher_Analysis/{raw_data,results}
cd ~/FusionCatcher_Analysis

# Navigate to data directory
cd raw_data

# Download the same fusion-positive dataset used in Part 16
echo "Downloading rhabdomyosarcoma dataset with known PAX3-FOXO1 fusion..."
fasterq-dump SRR30961741

# FusionCatcher expects specific naming conventions
echo "Renaming files for FusionCatcher compatibility..."
mv SRR30961741_1.fastq SRR30961741_R1.fastq
mv SRR30961741_2.fastq SRR30961741_R2.fastq

# Compress files to save storage space
echo "Compressing FASTQ files..."
gzip SRR30961741_R1.fastq SRR30961741_R2.fastq

echo "Dataset preparation complete!"
```

> 

**File Organization**: FusionCatcher requires that all FASTQ files belonging to the same sample be placed in a single directory. This organization is crucial for proper processing.

## Running FusionCatcher Analysis

### Executing the Fusion Detection Pipeline

Now let’s run FusionCatcher on our prepared dataset:

```bash
#-----------------------------------------------
# STEP 4: Execute FusionCatcher analysis
#-----------------------------------------------

# Navigate to project root
cd ~/FusionCatcher_Analysis

# Run FusionCatcher with optimized parameters
echo "Starting FusionCatcher analysis..."
echo "This process will take 2-4 hours depending on your system..."

fusioncatcher \
    -d ~/fusioncatcher_Index_human/current/ \     # Reference database path
    -i ~/FusionCatcher_Analysis/raw_data \        # Input directory with FASTQ files
    -o ~/FusionCatcher_Analysis/results           # Output directory for results

# Alternative command if you have matched normal RNA-seq samples:
# fusioncatcher \
#     -d ~/fusioncatcher_Index_human/current/ \
#     -i ~/FusionCatcher_Analysis/raw_data \
#     -I /path/to/matched_normal_sample/ \       # Matched normal sample directory
#     -o ~/FusionCatcher_Analysis/results

echo "FusionCatcher analysis initiated!"
echo "Monitor progress in the log files within the results directory."
```

> 

**Normal Sample Filtering:** By default, FusionCatcher uses a curated background list of fusion genes that have been previously identified in normal healthy samples to filter out likely false positives. If you have matched normal RNA-seq data from the same patient, you can provide it using the -I or –normal option to create a personalized background filter, which can further improve specificity by removing patient-specific germline fusions.

**Important Note**: Do not pre-trim your FASTQ files before running FusionCatcher. The tool performs its own intelligent quality filtering and trimming optimized specifically for fusion detection. Pre-trimming can actually reduce sensitivity by decreasing RNA fragment sizes, which are crucial for accurate fusion gene detection.

**Performance Tip**: FusionCatcher’s performance decreases dramatically when using non-default parameters. The default settings are optimized for the best balance of sensitivity and specificity.

### Understanding the Analysis Process

While FusionCatcher runs, here’s what’s happening behind the scenes:

1. **Automatic Quality Assessment**: Identifies and handles adapter sequences

2. **Smart Trimming**: Preserves RNA fragment length for optimal fusion detection

3. **Multi-Step Alignment**: Uses BOWTIE, BLAT, and STAR in sequence

4. **Junction Validation**: Verifies fusion breakpoints with multiple evidence types

5. **Biological Filtering**: Applies extensive databases to remove false positives

6. **Protein Analysis**: Predicts functional consequences of detected fusions

## Interpreting FusionCatcher Results

### Key Output Files Explained

When analysis completes, you’ll find these essential files in your results directory:

```bash
#-----------------------------------------------
# Primary Results Files
#-----------------------------------------------

# Main fusion gene list (hg38 coordinates)
final-list_candidate_fusion_genes.txt

# Same results with hg19 coordinates for compatibility
final-list_candidate_fusion_genes.hg19.txt

# Executive summary of findings
summary_candidate_fusions.txt

# Detailed interpretation guide
final-list_candidate_fusion_genes.caption.md.txt

#-----------------------------------------------
# Supporting Evidence Files
#-----------------------------------------------

# Supporting reads from BOWTIE alignment
supporting-reads_gene-fusions_BOWTIE.zip

# Supporting reads from BLAT alignment
supporting-reads_gene-fusions_BLAT.zip

# Supporting reads from STAR alignment
supporting-reads_gene-fusions_STAR.zip

#-----------------------------------------------
# Quality Control and Metadata
#-----------------------------------------------

# Pathogen screening results
viruses_bacteria_phages.txt

# Analysis metadata and statistics
info.txt

# Complete analysis log
fusioncatcher.log
```

### Understanding the Results Format

The main results file contains comprehensive information for each detected fusion:

**Gene_1_symbol****Gene_2_symbol****Fusion_description****Counts_of_common_mapping_reads****Spanning_pairs****Spanning_unique_reads****Longest_anchor_found****Fusion_sequence****Predicted_effect**

| Column | Description | Clinical Significance |
| --- | --- | --- |
|  | 5′ partner gene | Often contains regulatory elements |
|  | 3′ partner gene | Usually contributes functional domains |
|  | Chromosomal locations and details | Reveals structural rearrangement |
|  | quality control metric | Expected to be 0 |
|  | Read pairs spanning the junction | Direct evidence of fusion transcript |
|  | Unique spanning reads | Reduces PCR duplication bias |
|  | Maximum anchoring sequence length | Longer anchors indicate higher confidence |
|  | Actual junction sequence | Shows precise breakpoint (marked with *) |
|  | Functional consequence prediction | In-frame fusions often have greater impact |

### Analyzing Expected Results

For our rhabdomyosarcoma dataset, you should observe:

**PAX3–FOXO1**: The diagnostic fusion for alveolar rhabdomyosarcoma

- High supporting read count

- In-frame fusion maintaining functional domains

- Strong clinical significance

**MARS–AVIL**: Secondary fusion event

- Lower support but still significant

- May represent passenger mutation

**Additional Candidates**: Various other potential fusions requiring validation

**final-list_candidate_fusion_genes.txt**:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/fusioncatcher_results.png?resize=1024%2C153&ssl=1)

**summary_candidate_fusions.txt**:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/fusioncatcher_summary.png?resize=1024%2C175&ssl=1)

## Comparing STAR-Fusion vs FusionCatcher

### When to Use Each Tool

Understanding the strengths of each approach helps you choose the right tool for your research:

**Analysis Speed****Sensitivity****False Positive Rate****Automation Level****Protein Analysis****Best Applications****Memory Requirements****Clinical Validation**

| Aspect | STAR-Fusion | FusionCatcher |
| --- | --- | --- |
|  | 30-60 minutes | 2-4 hours |
|  | High for well-annotated fusions | Superior for challenging fusions |
|  | Moderate filtering | Extensive filtering |
|  | Manual parameter tuning | Fully automated optimization |
|  | Requires FusionInspector | Built-in predictions |
|  | General RNA-seq studies | Cancer research focus |
|  | Moderate | Higher |
|  | Good RT-PCR validation | Excellent RT-PCR validation |

### Complementary Analysis Strategy

Rather than choosing one tool over the other, consider this integrated approach:

1. **Primary Screening**: Use FusionCatcher for comprehensive somatic fusion detection

2. **Rapid Validation**: Use STAR-Fusion for quick confirmation of key findings

3. **Cross-Validation**: Compare results between tools for high-confidence calls

4. **Clinical Focus**: Prioritize FusionCatcher results for cancer-relevant fusions

## Advanced Analysis and Troubleshooting

### Data Quality Requirements

**Optimal Input Specifications:**

- **Read Type**: Paired-end RNA-seq preferred for maximum sensitivity

- **Sequencing Depth**: Minimum 20-30 million read pairs for adequate coverage

- **Read Length**: ≥75bp recommended for reliable junction spanning

- **Fragment Size**: Ideally >300bp to maintain detection sensitivity

**Quality Considerations:**FusionCatcher performs integrated quality control and trimming, which is crucial because it preserves RNA fragment length – a critical factor for fusion gene detection.

### Validation Strategies

**RT-PCR Confirmation Design:**

- Use fusion junction sequences from FusionCatcher output

- Design primers spanning the breakpoint

- Include positive and negative controls

- Validate in multiple samples when possible

**Functional Analysis Approaches:**

- Examine protein domain retention in fusion products

- Assess predicted functional consequences

- Correlate with gene expression changes

- Consider therapeutic targeting implications

## Best Practices for Clinical Applications

### Quality Assurance Workflow

1. **Input Validation**: Ensure high-quality RNA-seq data with adequate depth

2. **Reference Standards**: Include known positive and negative controls when possible

3. **Cross-Platform Validation**: Compare results across multiple detection methods

4. **Clinical Correlation**: Connect findings to patient phenotype and treatment response

## Conclusion: Mastering Comprehensive Fusion Detection

### The Power of Multi-Tool Expertise

By combining STAR-Fusion and FusionCatcher in your analytical toolkit, you’ve developed a comprehensive approach to fusion detection that addresses diverse research needs:

- **STAR-Fusion**: Rapid screening and general-purpose fusion detection

- **FusionCatcher**: Deep, cancer-focused analysis with superior validation rates

- **Integrated Approach**: Cross-validation and confidence assessment

### Future Directions

Consider expanding your fusion analysis capabilities with:

1. **Functional Validation**: Learning experimental techniques for fusion confirmation

2. **Protein Structure Analysis**: Understanding fusion protein domains and interactions

3. **Clinical Database Integration**: Connecting findings to treatment response databases

4. **Therapeutic Targeting**: Identifying druggable fusion proteins and pathways

### Final Recommendations

**For Cancer Research**: FusionCatcher’s excellent RT-PCR validation rate and somatic focus make it essential for oncology applications.

**For General RNA-seq**: STAR-Fusion provides efficient screening, while FusionCatcher offers comprehensive validation.

**For Clinical Applications**: The combination of both tools provides the confidence needed for patient care decisions.

Remember that fusion gene detection is both an art and a science. The biological knowledge integrated into FusionCatcher, combined with your growing expertise in result interpretation, positions you to make meaningful contributions to cancer research and precision medicine.

## References

- Nicorici, D., et al. “FusionCatcher – a tool for finding somatic fusion genes in paired-end RNA-sequencing data.” *bioRxiv* (2014). DOI:10.1101/011650

- Haas, B.J., et al. “Accuracy assessment of fusion transcript detection via read-mapping and de novo fusion transcript assembly-based methods.” *Genome Biology* 20, 213 (2019).

- Kumar, S., et al. “Passenger mutations in more than 2,500 cancer genomes: overall molecular functional impact and consequences.” *Cell* 173, 371-385 (2018).

- Bruford, E.A., Antonescu, C.R., Carroll, A.J. *et al.* HUGO Gene Nomenclature Committee (HGNC) recommendations for the designation of gene fusions. *Leukemia* **35**, 3040–3043 (2021).

### Essential Resources

- **[FusionCatcher GitHub Repository](https://github.com/ndaniel/fusioncatcher)**: Complete documentation and updates

- **[FusionCatcher Manual](https://github.com/ndaniel/fusioncatcher/blob/master/doc/manual.md#output-data)**: Detailed usage instructions
