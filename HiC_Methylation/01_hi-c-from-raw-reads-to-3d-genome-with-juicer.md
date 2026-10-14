# How To Analyze Hi-C Data For Absolute Beginners: From Raw Reads To 3D Genome Organization With Juicer

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Epigenetics, chromatin accessibility
- **Source:** [https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/)

---

*A comprehensive step-by-step guide to uncover three-dimensional chromosome structure using Juicer*

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/)

- [Introduction: Understanding Hi-C Technology](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Introduction_Understanding_Hi-C_Technology)

  - [What is Hi-C?](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#What_is_Hi-C)
  - [What Biological Insights Can We Get From Hi-C Data?](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#What_Biological_Insights_Can_We_Get_From_Hi-C_Data)
  - [What Type of Data Do We Need for Hi-C Analysis?](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#What_Type_of_Data_Do_We_Need_for_Hi-C_Analysis)
  - [What’s the Workflow for Analyzing Hi-C Data?](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Whats_the_Workflow_for_Analyzing_Hi-C_Data)

- [Setting Up Your Analysis Environment](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Setting_Up_Your_Analysis_Environment)

  - [Creating a Conda Environment for Hi-C Analysis](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Creating_a_Conda_Environment_for_Hi-C_Analysis)
  - [Installing Juicer and Dependencies](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Installing_Juicer_and_Dependencies)
  - [Preparing Reference Genome and Restriction Sites](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Preparing_Reference_Genome_and_Restriction_Sites)

- [Data Preparation and Quality Control](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Data_Preparation_and_Quality_Control)

  - [Understanding Hi-C Data Structure](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Understanding_Hi-C_Data_Structure)
  - [Quality Control and Preprocessing](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Quality_Control_and_Preprocessing)
  - [Preparing Input Files for Juicer](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Preparing_Input_Files_for_Juicer)

- [Running Juicer: The Complete Hi-C Analysis Pipeline](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Running_Juicer_The_Complete_Hi-C_Analysis_Pipeline)

  - [Understanding Juicer’s Analysis Principles](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Understanding_Juicers_Analysis_Principles)
  - [Running the Juicer Pipeline](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Running_the_Juicer_Pipeline)

- [Advanced Hi-C Analysis and Visualization](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Advanced_Hi-C_Analysis_and_Visualization)

  - [Topologically Associating Domains (TAD) Detection](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Topologically_Associating_Domains_TAD_Detection)
  - [Contact Matrix Extraction and Compartment Analysis](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Contact_Matrix_Extraction_and_Compartment_Analysis)

- [Visualizing Hi-C Results with Juicebox](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Visualizing_Hi-C_Results_with_Juicebox)

  - [Installing and Setting Up Juicebox](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Installing_and_Setting_Up_Juicebox)
  - [Essential Juicebox Features and Navigation](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Essential_Juicebox_Features_and_Navigation)
  - [Best Practices for Juicebox Visualization](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Best_Practices_for_Juicebox_Visualization)

- [Conclusion: Interpreting Your Hi-C Results](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Conclusion_Interpreting_Your_Hi-C_Results)

  - [Key Outputs and Their Biological Significance](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Key_Outputs_and_Their_Biological_Significance)
  - [Data Quality Benchmarks](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Data_Quality_Benchmarks)
  - [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Troubleshooting_Common_Issues)

- [Best Practices for Hi-C Analysis](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Best_Practices_for_Hi-C_Analysis)

  - [Experimental Design Considerations](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Experimental_Design_Considerations)
  - [Computational Best Practices](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Computational_Best_Practices)
  - [Statistical Considerations](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Statistical_Considerations)
  - [Common Pitfalls to Avoid](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Common_Pitfalls_to_Avoid)

- [References](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#References)

  - [Share this:](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-hi-c-data-for-absolute-beginners-from-raw-reads-to-3d-genome-organization-with-juicer/#Like_this)

## Introduction: Understanding Hi-C Technology

The genome isn’t just a linear string of DNA—it exists as a complex three-dimensional structure within the cell nucleus. Understanding how chromosomes fold and interact in space is crucial for comprehending gene regulation, DNA repair mechanisms, and disease processes. Hi-C (High-throughput Chromosome Conformation Capture) technology has revolutionized our ability to map these three-dimensional genome interactions genome-wide.

### What is Hi-C?

Hi-C is a powerful molecular technique that captures and sequences DNA fragments that are physically close to each other in the three-dimensional space of the cell nucleus, even if they are far apart on the linear genome. This proximity-based approach allows researchers to create comprehensive maps of chromosome interactions, revealing the spatial organization of the genome.

The experimental process begins with living cells where DNA is cross-linked with formaldehyde, preserving the three-dimensional structure by creating covalent bonds between proteins and DNA that are in close proximity. The chromatin is then digested with restriction enzymes, creating DNA fragments while maintaining the cross-links. These fragments are biotinylated at their ends and religated under dilute conditions, preferentially joining fragments that were originally close in space. After reversing the cross-links and purifying the DNA, the resulting ligation products are sequenced using paired-end sequencing.

> 

**Beginner’s Tip:** Think of Hi-C as taking a “snapshot” of how chromosomes are folded inside the cell nucleus. Just like how a photographer captures people standing close together at a party, Hi-C captures DNA segments that are physically near each other in the cell, even if they’re far apart when you read the genome sequence.

### What Biological Insights Can We Get From Hi-C Data?

Hi-C data provides unprecedented insights into genome organization and function:

**Chromosome Territories:** Each chromosome occupies a distinct region within the nucleus. Hi-C reveals how chromosomes interact with each other and how this organization changes during development or disease.

**A/B Compartments:** The genome is organized into active (A) and inactive (B) compartments. A compartments contain actively transcribed genes and open chromatin, while B compartments are enriched for heterochromatin and silenced genes. Changes in compartmentalization can indicate disease states.

**Topologically Associating Domains (TADs):** These are self-interacting genomic regions that serve as regulatory units. Genes within the same TAD often share similar expression patterns and regulatory elements. Understanding TAD boundaries helps predict which enhancers regulate which genes.

**Chromatin Loops:** Hi-C identifies specific long-range interactions between regulatory elements like enhancers and promoters. These loops bring distant regulatory sequences into physical proximity, enabling gene regulation across large genomic distances.

**Structural Variations:** Large-scale genomic rearrangements, duplications, and deletions can be detected through disrupted interaction patterns, making Hi-C valuable for medical genetics and cancer research.

### What Type of Data Do We Need for Hi-C Analysis?

Successful Hi-C analysis requires several key data components:

**Paired-End Sequencing Reads:** Hi-C experiments generate paired-end sequencing data where each read pair represents two DNA fragments that were originally in close spatial proximity. The sequencing depth requirements are substantial—typically 100-500 million read pairs for mammalian genomes to achieve sufficient resolution for detailed analysis.

**Reference Genome:** A high-quality reference genome assembly is essential for mapping Hi-C reads. The quality of your reference genome directly impacts the accuracy of your interaction maps.

**Restriction Enzyme Information:** The specific restriction enzyme used during library preparation must be known, as this affects how the data is processed. Common enzymes include MboI (4-base cutter) for high-resolution analysis and HindIII (6-base cutter) for broader coverage.

**Sample Metadata:** Information about cell type, experimental conditions, and biological replicates is crucial for proper interpretation and comparison of results.

> 

**Quality Considerations:** Higher sequencing depth provides better resolution for detecting fine-scale structures and weak interactions. For initial exploratory analysis, 50-100 million read pairs may suffice, but comprehensive studies benefit from deeper sequencing.

### What’s the Workflow for Analyzing Hi-C Data?

The Hi-C analysis pipeline consists of several interconnected steps, each building upon the previous one:

1. **Quality Control and Preprocessing:** Assessing read quality, trimming adapters, and filtering low-quality sequences to ensure reliable downstream analysis.

2. **Read Alignment:** Mapping paired-end reads to the reference genome while accounting for the unique properties of Hi-C data, including chimeric reads created during the ligation process.

3. **Contact Matrix Generation:** Converting aligned read pairs into interaction frequencies between genomic regions, creating the foundational data structure for all subsequent analyses.

4. **Normalization:** Correcting for experimental biases such as restriction enzyme cutting efficiency, GC content, and mappability to ensure accurate comparison of interaction frequencies.

5. **Feature Detection:** Identifying biological structures including TADs, compartments, and chromatin loops using specialized algorithms designed for Hi-C data.

6. **Visualization and Interpretation:** Creating heatmaps, contact matrices, and other visualizations to explore and communicate findings.

7. **Comparative Analysis:** Comparing interaction patterns between different conditions, time points, or cell types to understand dynamic changes in genome organization.

Each step requires careful parameter selection and quality assessment to ensure meaningful biological conclusions.

## Setting Up Your Analysis Environment

Before diving into Hi-C analysis, we need to establish a robust computational environment with all necessary tools and dependencies.

### Creating a Conda Environment for Hi-C Analysis

Let’s create a dedicated environment for Hi-C analysis that includes Juicer and all its dependencies:

```bash
#-----------------------------------------------
# STEP 1: Setup conda environment for Hi-C analysis
#-----------------------------------------------

# Create a dedicated conda environment
conda create -n hic_analysis python=3.9

# Activate the newly created environment
conda activate hic_analysis

# Configure conda channels in order of priority
conda config --add channels defaults       # Standard packages
conda config --add channels bioconda       # Bioinformatics packages
conda config --add channels conda-forge    # Community-maintained packages
conda config --set channel_priority strict # Prevent package conflicts

# Install essential tools for Hi-C analysis
conda install -y \
    wget \                          # For downloading files and datasets
    git \                           # Version control for Juicer installation
    samtools \                      # For manipulating SAM/BAM files
    sra-tools \                     # For downloading sequencing data
    fastqc \                        # For quality control of sequencing reads
    trim-galore \                   # For adapter trimming and quality filtering
    bwa \                           # For read alignment
    java-jdk \                      # Required for Juicer (Java-based tools)
    gcc \                           # C compiler for building Juicer
    make \                          # Build system
    gawk \                          # Text processing (required by Juicer scripts)
    parallel                        # For parallel processing
```

### Installing Juicer and Dependencies

Juicer is a comprehensive platform for analyzing Hi-C data. Let’s install it along with its specific dependencies:

```bash
#-----------------------------------------------
# STEP 2: Install Juicer and its dependencies
#-----------------------------------------------

# Create a directory for all Hi-C analysis tools
mkdir -p ~/hic_tools
cd ~/hic_tools

# Clone the Juicer repository
git clone https://github.com/aidenlab/juicer.git
cd juicer

# Create necessary directory structure for Juicer 2.0
mkdir -p scripts/common
cp CPU/*.* scripts/common
cp CPU/common/* scripts/common

# Download Juicer Tools JAR file
wget https://github.com/aidenlab/Juicebox/releases/download/v2.17.00/juicer_tools_2.17.00.jar

# Create symbolic link with the expected name
mv juicer_tools_2.17.00.jar scripts/common/juicer_tools.jar
```

> 

**Installation Tips:**

- Ensure you have at least 32GB of RAM available for Hi-C analysis of mammalian genomes

- The Java heap size can be adjusted in the juicer_tools alias if you have different memory requirements

- Some cluster environments may require loading specific modules instead of using conda

### Preparing Reference Genome and Restriction Sites

Hi-C analysis requires a reference genome and information about restriction enzyme cutting sites:

```python
#-----------------------------------------------
# STEP 3: Prepare reference genome and restriction sites
#-----------------------------------------------

# Create directories for reference data
mkdir -p ~/hic_references/hg38
cd ~/hic_references/hg38

#=============================================
# 3.1: Download and prepare reference genome
#=============================================

# Download the reference genome
wget https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz
gunzip hg38.fa.gz

# Create BWA index for the reference genome
bwa index hg38.fa

#=============================================
# 3.2: Generate restriction enzyme sites
#=============================================

# Create a comprehensive script to generate all common Hi-C restriction enzyme sites
cat > generate_restriction_sites.py << 'EOF'
#!/usr/bin/env python3
"""
Generate restriction enzyme cut sites for all commonly used Hi-C enzymes
"""

import re
import os

# Common restriction enzymes used in Hi-C experiments
HIC_ENZYMES = {
    'MboI': 'GATC',           # 4-base cutter, most common for high-resolution Hi-C
    'DpnII': 'GATC',          # 4-base cutter, same recognition as MboI
    'HindIII': 'AAGCTT',      # 6-base cutter, good for genome-wide overview
    'NcoI': 'CCATGG',         # 6-base cutter
    'BglII': 'AGATCT',        # 6-base cutter
    'EcoRI': 'GAATTC',        # 6-base cutter
    'BamHI': 'GGATCC',        # 6-base cutter
    'XhoI': 'CTCGAG',         # 6-base cutter
    'SacI': 'GAGCTC',         # 6-base cutter
    'KpnI': 'GGTACC',         # 6-base cutter
    'SalI': 'GTCGAC',         # 6-base cutter
    'SpeI': 'ACTAGT',         # 6-base cutter
    'XbaI': 'TCTAGA',         # 6-base cutter
    'NheI': 'GCTAGC',         # 6-base cutter
    'AluI': 'AGCT',           # 4-base cutter
    'Sau3AI': 'GATC',         # 4-base cutter, same as MboI/DpnII
    'TaqI': 'TCGA',           # 4-base cutter
    'MseI': 'TTAA',           # 4-base cutter
    'CviQI': 'GTAC',          # 4-base cutter
    'HaeIII': 'GGCC'          # 4-base cutter
}

def find_restriction_sites(fasta_file, enzyme_name, enzyme_seq, output_file):
    """
    Find all restriction enzyme cut sites in the genome
    """
    sites = []
    current_chr = None
    current_pos = 0
    sequence_buffer = ""

    with open(fasta_file, 'r') as f:
        for line_num, line in enumerate(f, 1):
            line = line.strip()
            if line.startswith('>'):
                # Process any remaining sequence in buffer
                if sequence_buffer and current_chr:
                    for match in re.finditer(enzyme_seq, sequence_buffer.upper()):
                        pos = current_pos + match.start() + 1  # 1-based coordinate
                        sites.append(f"{current_chr}\t{pos}")

                # New chromosome
                current_chr = line[1:].split()[0]  # Take only chromosome name
                current_pos = 0
                sequence_buffer = ""
            else:
                # DNA sequence - add to buffer
                sequence_buffer += line.upper()

                # Process buffer when it gets large enough
                if len(sequence_buffer) > 1000000:  # Process in 1MB chunks
                    # Find sites in current buffer
                    for match in re.finditer(enzyme_seq, sequence_buffer):
                        pos = current_pos + match.start() + 1  # 1-based coordinate
                        sites.append(f"{current_chr}\t{pos}")

                    # Keep overlap region for sites spanning chunks
                    overlap = len(enzyme_seq) - 1
                    current_pos += len(sequence_buffer) - overlap
                    sequence_buffer = sequence_buffer[-overlap:]

    # Process final buffer
    if sequence_buffer and current_chr:
        for match in re.finditer(enzyme_seq, sequence_buffer):
            pos = current_pos + match.start() + 1  # 1-based coordinate
            sites.append(f"{current_chr}\t{pos}")

    # Write restriction sites to file
    with open(output_file, 'w') as f:
        f.write(f"# Restriction sites for {enzyme_name} ({enzyme_seq})\n")
        f.write(f"# Format: chromosome<tab>position\n")
        f.write(f"# Total sites found: {len(sites)}\n")
        for site in sites:
            f.write(site + '\n')

    return len(sites)

def generate_all_restriction_sites(fasta_file):
    """Generate restriction sites for all Hi-C enzymes"""

    if not os.path.exists(fasta_file):
        print(f"Error: Reference genome file not found: {fasta_file}")
        return

    total_enzymes = len(HIC_ENZYMES)
    results = {}

    for i, (enzyme_name, enzyme_seq) in enumerate(HIC_ENZYMES.items(), 1):
        output_file = f"hg38_{enzyme_name}.txt"
        site_count = find_restriction_sites(fasta_file, enzyme_name, enzyme_seq, output_file)
        results[enzyme_name] = site_count

if __name__ == "__main__":
    # Generate sites for the reference genome
    fasta_file = 'hg38.fa'
    generate_all_restriction_sites(fasta_file)
EOF

# Make the script executable
chmod +x generate_restriction_sites.py

# Run the script to generate all restriction sites
python3 generate_restriction_sites.py

#=============================================
# 3.3: Create chromosome sizes file
#=============================================

# Extract chromosome sizes from the FASTA file
samtools faidx hg38.fa
cut -f1,2 hg38.fa.fai > hg38.chrom.sizes
```

> 

**Restriction Enzyme Choice:**

- **MboI (GATC)**: 4-base cutter, creates ~1 million fragments in human genome, ideal for high-resolution analysis

- **HindIII (AAGCTT)**: 6-base cutter, creates ~300,000 fragments, better for genome-wide overview

- Choose based on your research questions and computational resources

## Data Preparation and Quality Control

Before running the main Juicer pipeline, we need to prepare our Hi-C sequencing data and perform quality control checks.

### Understanding Hi-C Data Structure

Hi-C generates paired-end sequencing data with unique characteristics that distinguish it from other NGS approaches:

```bash
#-----------------------------------------------
# STEP 4: Download and examine example Hi-C data
#-----------------------------------------------

# Create project directory structure
mkdir -p ~/hic_project/{raw_data,trimmed_data,fastq}

# Download example Hi-C dataset (GSE63525)
cd ~/hic_project/raw_data
fasterq-dump SRR1658570
gzip *.fastq
```

### Quality Control and Preprocessing

Quality control is crucial for Hi-C data because poor-quality reads can lead to incorrect interaction calls:

```bash
#-----------------------------------------------
# STEP 5: Quality control and preprocessing
#-----------------------------------------------

#=============================================
# 5.1: Initial quality assessment
#=============================================

# Run FastQC on the raw data
mkdir -p ~/hic_project/qc_reports/raw
fastqc ~/hic_project/raw_data/*.fastq.gz -o ~/hic_project/qc_reports/raw -t 8

#=============================================
# 5.2: Adapter trimming and quality filtering
#=============================================

# Trim adapters and low-quality bases
trim_galore \
    --paired \                      # Paired-end mode
    --quality 20 \                  # Trim bases with quality < 20
    --stringency 3 \                # Adapter overlap requirement
    --length 20 \                   # Minimum read length after trimming
    --fastqc \                      # Run FastQC on trimmed reads
    --cores 8 \                     # Use multiple cores
    --output_dir ~/hic_project/trimmed_data/ \
    ~/hic_project/raw_data/SRR1658570_1.fastq.gz \
    ~/hic_project/raw_data/SRR1658570_2.fastq.gz
```

> 

**Hi-C Quality Indicators:**

- **GC Content:** Should match the organism (~41% for human)

- **Read Length:** Longer reads (>75bp) provide better mapping accuracy

- **Duplication Rate:** Higher than typical sequencing due to ligation artifacts (this is normal)

- **Insert Size Distribution:** Hi-C shows a characteristic distribution different from genomic DNA

### Preparing Input Files for Juicer

Juicer requires specific input file formats and directory structures:

```bash
#-----------------------------------------------
# STEP 6: Prepare input files for Juicer
#-----------------------------------------------

# Uncompress and rename files to Juicer's expected format
gunzip -c ~/hic_project/trimmed_data/SRR1658570_1_val_1.fq.gz > ~/hic_project/fastq/SRR1658570_R1.fastq
gunzip -c ~/hic_project/trimmed_data/SRR1658570_2_val_2.fq.gz > ~/hic_project/fastq/SRR1658570_R2.fastq
```

> 

**File Format Requirements:**

- FASTQ files can be compressed or uncompressed

- Restriction sites must list chromosome numbers (not names) and positions

- Directory structure must match Juicer’s expectations

- File permissions should allow read/write access

## Running Juicer: The Complete Hi-C Analysis Pipeline

Now we’ll run the complete Juicer pipeline, which handles alignment, filtering, and contact matrix generation in an integrated workflow.

### Understanding Juicer’s Analysis Principles

Before running Juicer, it’s important to understand how it processes Hi-C data:

**Two-Step Alignment Strategy:** Juicer uses a sophisticated approach where reads are first aligned normally, then chimeric reads (spanning ligation junctions) are split and re-aligned. This captures the maximum number of valid Hi-C contacts.

**Quality Filtering:** The pipeline removes PCR duplicates, low-quality alignments, and artifacts while preserving legitimate long-range interactions.

**Contact Matrix Generation:** Aligned read pairs are converted into contact frequencies between genomic bins, creating the fundamental data structure for all downstream analyses.

**Normalization:** Multiple normalization methods correct for biases in Hi-C data, including restriction enzyme cutting efficiency, GC content, and mappability.

### Running the Juicer Pipeline

```bash
#-----------------------------------------------
# STEP 7: Run Juicer pipeline
#-----------------------------------------------

# Set key variables for the analysis
GENOME="hg38"
REFERENCE_DIR="~/hic_references/hg38"
RESTRICTION_SITE="MboI"
RESTRICTION_FILE="~/hic_references/hg38/hg38_MboI.txt"
CHROM_SIZES="~/hic_references/hg38/hg38.chrom.sizes"
THREADS=16

# Run the complete Juicer pipeline
bash ~/hic_tools/juicer/scripts/common/juicer.sh \
    -D ~/hic_tools/juicer \             # Juicer installation directory
    -d ~/hic_project/ \                 # Project directory containing fastq folder
    -g ${GENOME} \                      # Genome assembly name
    -s ${RESTRICTION_SITE} \            # Restriction enzyme used
    -p ${CHROM_SIZES} \                 # Chromosome sizes file
    -y ${RESTRICTION_FILE} \            # Restriction enzyme cut sites
    -z ${REFERENCE_DIR}/hg38.fa \       # Reference genome FASTA file
    -t ${THREADS}                       # Number of threads to use

# Validate the generated Hi-C file
java -Xmx32g -jar ~/hic_tools/juicer/scripts/common/juicer_tools.jar validate \
    ~/hic_project/aligned/inter_30.hic
```

> 

**Runtime Expectations:**

- Small datasets (10M reads): 1-3 hours

- Medium datasets (100M reads): 6-12 hours

- Large datasets (500M+ reads): 24-48 hours

- Memory usage can peak at 20-50GB for mammalian genomes

## Advanced Hi-C Analysis and Visualization

After generating contact matrices, we can perform sophisticated analyses to extract biological insights from the data.

### Topologically Associating Domains (TAD) Detection

TADs are fundamental units of chromosome organization. Let’s identify them using Juicer Tools:

```bash
#-----------------------------------------------
# STEP 8: TAD detection using Arrowhead algorithm
#-----------------------------------------------

# Run Arrowhead algorithm to detect TADs genome-wide
java -Xmx32g -jar ~/hic_tools/juicer/scripts/common/juicer_tools.jar arrowhead \
    -c chr1,chr2,chr3,chr4,chr5,chr6,chr7,chr8,chr9,chr10,chr11,chr12,chr13,chr14,chr15,chr16,chr17,chr18,chr19,chr20,chr21,chr22,chrX \
    -m 5000 \                           # Minimum resolution for TAD detection
    -r 25000 \                          # Resolution for TAD boundary detection
    -k KR \                             # Normalization method (Knight-Ruiz)
    ~/hic_project/aligned/inter_30.hic \
    ~/hic_project/aligned/tads_genome_wide

# Convert BEDPE TAD output to BED format for Juicebox visualization
awk 'NR>1 {print $1"\t"$2"\t"$6"\tTAD_"NR-1"\t1000\t."}' \
    ~/hic_project/aligned/tads_genome_wide/25000_blocks.bedpe | tail -n +2 > \
    ~/hic_project/aligned/tad_domains_25kb.bed
```

### Contact Matrix Extraction and Compartment Analysis

```bash
#-----------------------------------------------
# STEP 9: Extract contact matrices and perform compartment analysis
#-----------------------------------------------

#=============================================
# Extract Contact Matrices for Detailed Analysis
#=============================================

# Extract intra-chromosomal contact matrix for chromosome 1 at 25kb resolution
java -Xmx32g -jar ~/hic_tools/juicer/scripts/common/juicer_tools.jar dump \
    observed KR \                            # Use KR normalization (recommended)
    ~/hic_project/aligned/inter_30.hic \     # Input Hi-C file
    chr1 chr1 \                              # Extract chr1 vs chr1 interactions
    BP 25000 \                               # Resolution: 25kb bins
    ~/hic_project/aligned/chr1_contacts_25kb.txt  # Output contact matrix

# Extract chromosome 1 contact matrix (alternative format for full chromosome)
java -Xmx32g -jar ~/hic_tools/juicer/scripts/common/juicer_tools.jar dump \
    observed KR \                            # Use KR normalization
    ~/hic_project/aligned/inter_30.hic \     # Input Hi-C file
    chr1 \                                   # Extract all chr1 interactions
    BP 25000 \                               # Resolution: 25kb bins
    ~/hic_project/aligned/chr1_25kb.txt      # Output contact matrix

#=============================================
# A/B Compartment Analysis Using Eigenvector Decomposition
#=============================================

# Perform eigenvector analysis to identify A/B compartments
java -Xmx32g -jar ~/hic_tools/juicer/scripts/common/juicer_tools.jar eigenvector \
    KR \                                     # Normalization method
    ~/hic_project/aligned/inter_30.hic \     # Input Hi-C file
    chr1 \                                   # Target chromosome
    BP 100000 \                              # Resolution: 100kb bins (optimal for compartments)
    ~/hic_project/aligned/chr1_eigenvector_100kb.txt  # Output eigenvector values

# Note: Positive eigenvector values = A compartment (active chromatin)
#       Negative eigenvector values = B compartment (inactive chromatin)

#=============================================
# Pearson Correlation Analysis for Compartment Visualization
#=============================================

# Calculate Pearson correlation matrix for compartment analysis
java -Xmx32g -jar ~/hic_tools/juicer/scripts/common/juicer_tools.jar pearsons \
    KR \                                     # Normalization method
    ~/hic_project/aligned/inter_30.hic \     # Input Hi-C file
    chr1 \                                   # Target chromosome
    BP 100000 \                              # Resolution: 100kb bins
    ~/hic_project/aligned/chr1_pearsons_100kb.txt  # Output correlation matrix

# Note: Pearson correlation reveals compartmental organization
#       - Strong positive correlations indicate same compartment type
#       - Negative correlations indicate different compartment types
#       - Creates characteristic plaid pattern in heatmaps

#=============================================
# Prepare Files for Juicebox Visualization
#=============================================

# Detect chromatin loops using HiCCUPS algorithm
java -Xmx32g -jar ~/hic_tools/juicer/scripts/common/juicer_tools.jar hiccups \
    -m 512 \                                # Memory allocation
    -r 5000,10000 \                         # Resolutions for loop detection
    -f 0.1,0.1 \                           # FDR thresholds
    -p 4,2 \                               # Peak calling parameters
    -i 7,5 \                               # Iteration parameters
    -d 20000,20000 \                       # Distance thresholds
    ~/hic_project/aligned/inter_30.hic \
    ~/hic_project/aligned/loops
```

## Visualizing Hi-C Results with Juicebox

Juicebox is the essential companion tool for exploring and visualizing Hi-C data interactively. It provides an intuitive interface for examining contact matrices, identifying structural features, and generating publication-quality figures.

### Installing and Setting Up Juicebox

Juicebox is available both as a desktop application and a web-based tool. We recommend the desktop version for comprehensive analysis:

```bash
#-----------------------------------------------
# STEP 10: Download and setup Juicebox
#-----------------------------------------------

# Download Juicebox for your system
# https://github.com/aidenlab/Juicebox/wiki/Download

# Or use the Juicebox online version
# https://aidenlab.org/juicebox/
```

### Essential Juicebox Features and Navigation

#### Basic Navigation and Interface

**Opening Hi-C Maps:**

- File → Open → Select your .hic file (inter_30.hic)

- The main heatmap displays chromosome-chromosome interactions

- Use the chromosome dropdown menus to select specific chromosomes or regions

**Zoom Controls:**

- Mouse wheel: Zoom in/out on the contact matrix

- Click and drag: Pan around the heatmap

- Zoom slider: Fine-tune magnification levels

- Preset resolution buttons: Jump to common resolutions (1MB, 250kb, 25kb, 5kb)

**Color Scale Adjustment:**

- Right panel contains color scale controls

- Adjust minimum and maximum values to optimize contrast

- Use “Observed/Expected” ratios to normalize for distance effects

#### Loading Your Analysis Results

**Loading TAD Annotations:**

```bash
# In Juicebox: Show → Show Annotation Panel → 1D Annotations → Add Local 
# Select: ~/hic_project/aligned/tad_domains_25kb.bed
```

**Loading Loop Annotations:**

```bash
# Show → Annotations → 2D Annotations → Add Local 
# Select: ~/hic_project/aligned/loops/postprocessed_pixels_5000.bedpe
```

**Loading Compartment Data:**

```bash
# Show → Show Annotation Panel → 1D Annotations → Add Local 
# Select: ~/hic_project/aligned/chr1_eigenvector_100kb.txt
# Positive values = A compartment (active)
# Negative values = B compartment (inactive)
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/07/juicebox_chr1_example-e1753053848546-1024x457.png?resize=1024%2C457&ssl=1)

#### Advanced Visualization Features

**Multi-Resolution Analysis:**

- Start at low resolution (1MB) to see large-scale organization

- Zoom to higher resolution (25kb-5kb) to examine detailed structures

- Use the “Show grid” option to visualize bin boundaries

**Comparative Analysis:**

```bash
# To compare two Hi-C datasets side by side:
# File → Open As Control → Select second .hic file
# View → Observed vs Control to see differential interactions
```

**Normalization Options:**

- **Observed:** Raw contact counts

- **Observed/Expected:** Normalized for genomic distance

- **KR (Knight-Ruiz):** Matrix balancing normalization (recommended)

- **VC (Vanilla Coverage):** Coverage normalization

- **SQRT_VC:** Square root of coverage normalization

#### Interactive Analysis Workflow

**Step 1: Genome-Wide Overview**

1. Load your .hic file

2. Start at 1MB resolution

3. Examine inter-chromosomal interactions

4. Identify chromosomes with interesting patterns

**Step 2: Chromosome-Specific Analysis**

1. Select individual chromosomes

2. Zoom to 250kb-100kb resolution

3. Look for TAD structures and compartmentalization

4. Load eigenvector tracks to visualize A/B compartments

**Step 3: High-Resolution Feature Detection**

1. Zoom to 25kb-5kb resolution

2. Search for chromatin loops and local interactions

3. Load loop annotations if available

4. Examine specific loci of biological interest

**Step 4: Comparative Analysis**

1. Load control datasets for comparison

2. Use differential view modes

3. Export regions of interest for further analysis

4. Generate figures for publication

### Best Practices for Juicebox Visualization

**Data Exploration Strategy:**

1. Always start with genome-wide view at low resolution

2. Use appropriate normalization (KR recommended)

3. Examine both intra- and inter-chromosomal interactions

4. Focus on regions with biological relevance to your study

**Figure Generation Guidelines:**

- Use consistent color scales across comparisons

- Include scale bars and resolution information

- Label important genomic features clearly

- Provide adequate figure legends explaining the visualization

**Quality Assessment:**

- Check for diagonal enrichment (indicating successful Hi-C)

- Verify that known structures (centromeres, heterochromatin) appear as expected

- Compare your data with published datasets from similar cell types

**Collaborative Analysis:**

- Save session files to preserve analysis states

- Export contact matrices for computational analysis

- Share .hic files with collaborators for independent exploration

## Conclusion: Interpreting Your Hi-C Results

Congratulations! You’ve successfully completed a comprehensive Hi-C analysis pipeline using Juicer and learned to visualize your results with Juicebox. This analysis has revealed the three-dimensional organization of the genome, including TADs, chromatin loops, and compartmental structure.

### Key Outputs and Their Biological Significance

**Contact Matrices (.hic files):** These contain the fundamental interaction data that can be explored interactively using Juicebox. The contact frequencies reveal which genomic regions are spatially proximate in the nucleus.

**Topologically Associating Domains (TADs):** These self-interacting genomic regions represent fundamental units of chromosome organization. Genes within the same TAD often share similar expression patterns and regulatory elements. TAD boundaries are typically enriched for CTCF binding sites and act as barriers to enhancer-promoter interactions.

**Chromatin Loops:** These represent specific long-range interactions between regulatory elements like enhancers and promoters. Loop identification helps predict which enhancers regulate which genes, providing insights into gene regulatory networks.

**A/B Compartments:** The compartmentalization analysis reveals large-scale organization of active and inactive chromatin. A compartments are enriched for actively transcribed genes, while B compartments contain more heterochromatin and silenced genes.

### Data Quality Benchmarks

Understanding whether your Hi-C experiment was successful requires evaluating several key metrics:

**Inter-chromosomal Interaction Rate:** Healthy Hi-C libraries typically show 5-40% inter-chromosomal interactions. Very high percentages may indicate excessive random ligation, while very low percentages might suggest over-digestion or poor ligation efficiency.

**Interaction Distance Distribution:** Most interactions should occur between nearby genomic regions, with the frequency decreasing as genomic distance increases. This creates the characteristic diagonal pattern in Hi-C heatmaps.

**Total Valid Interactions:** For mammalian genomes, aim for at least 10-50 million valid read pairs for basic analysis, and 100-500 million for high-resolution studies.

**TAD Detection:** Successful experiments typically identify 2,000-5,000 TADs in the human genome, with median sizes around 200kb-1Mb.

### Troubleshooting Common Issues

**Low Contact Frequency:** If your contact matrices appear sparse, check your restriction enzyme cutting efficiency and sequencing depth. Consider using a different restriction enzyme or increasing sequencing depth.

**Poor TAD Detection:** Weak or fragmented TADs may indicate low data quality, insufficient resolution, or biological factors like cell cycle stage. Verify your normalization methods and consider pooling replicates.

**High Background Noise:** Random ligation products create background noise in Hi-C data. Ensure proper filtering of low-quality alignments and consider more stringent duplicate removal.

**Visualization Issues:** If heatmaps appear overloaded or unclear, adjust the color scale, try different normalization methods, or focus on smaller genomic regions for detailed views.

## Best Practices for Hi-C Analysis

### Experimental Design Considerations

**Biological Replicates:** Always include at least two biological replicates to assess reproducibility and enable statistical analysis of structural differences.

**Cell Synchronization:** For studying cell cycle-dependent changes in chromosome structure, consider synchronizing cells before fixation.

**Controls:** Include appropriate controls such as different time points or treatment conditions to validate biological findings.

**Sequencing Strategy:** Balance sequencing depth with cost. Start with moderate depth (50-100M reads) for pilot studies, then increase for publication-quality data.

### Computational Best Practices

**Resource Planning:** Hi-C analysis is computationally intensive. Ensure adequate RAM (32-64GB) and storage space (500GB-1TB for full analysis).

**Quality Control at Every Step:** Monitor alignment rates, duplicate levels, and contact distribution throughout the pipeline. Poor metrics early in the pipeline will affect all downstream analyses.

**Parameter Optimization:** Default parameters work well for most datasets, but consider optimizing resolution, normalization methods, and detection thresholds for your specific research questions.

**Data Backup:** Hi-C datasets are large and time-consuming to regenerate. Implement robust backup strategies for both raw data and key intermediate files.

### Statistical Considerations

**Multiple Testing Correction:** When identifying significant interactions or comparing conditions, apply appropriate corrections for multiple testing (FDR, Bonferroni).

**Reproducibility Assessment:** Use metrics like Pearson correlation between replicates and overlap of detected features to assess data quality.

**Effect Size Reporting:** Report not just statistical significance but also effect sizes when comparing interactions between conditions.

### Common Pitfalls to Avoid

**Insufficient Normalization:** Hi-C data contains numerous technical biases. Always apply appropriate normalization methods (KR, VC, or ICE) before analysis.

**Resolution Mismatch:** Match your analysis resolution to your data quality and research questions. Higher resolution requires more sequencing depth.

**Ignoring Cell Heterogeneity:** Remember that Hi-C captures population averages. Single-cell Hi-C methods are available for studying cell-to-cell variation.

**Over-interpretation:** Not all detected interactions are functionally relevant. Validate key findings with orthogonal methods when possible.

**Batch Effects:** Process samples consistently and be aware of potential batch effects, especially when comparing samples processed at different times.

## References

1. **Lieberman-Aiden, E. et al.** (2009). Comprehensive mapping of long-range interactions reveals folding principles of the human genome. *Science*, 326(5950), 289-293.

2. **Rao, S.S. et al.** (2014). A 3D map of the human genome at kilobase resolution reveals principles of chromatin looping. *Cell*, 159(7), 1665-1680.

3. **Dixon, J.R. et al.** (2012). Topological domains in mammalian genomes identified by analysis of chromatin interactions. *Nature*, 485(7398), 376-380.

4. **Durand, N.C. et al.** (2016). Juicer provides a one-click system for analyzing loop-resolution Hi-C experiments. *Cell Systems*, 3(1), 95-98.

5. **Dekker, J. et al.** (2017). The 4D nucleome project. *Nature*, 549(7671), 219-226.

6. **Kruse, K. et al.** (2016). FAN-C: a feature-rich framework for the analysis and visualisation of chromosome conformation capture data. *Genome Biology*, 21, 303.

7. **Abdennur, N. & Mirny, L.A.** (2020). Cooler: scalable storage for Hi-C data and other genomically labeled arrays. *Bioinformatics*, 36(1), 311-316.

8. **Wolff, J. et al.** (2018). Galaxy HiCExplorer: a web server for reproducible Hi-C data analysis, quality control and visualization. *Nucleic Acids Research*, 46(W1), W11-W16.

9. **Mota-Gómez, I., & Lupiáñez, D. G.** (2019). A (3D-Nuclear) Space Odyssey: Making Sense of Hi-C Maps. Genes, 10(6), 415. https://doi.org/10.3390/genes10060415

10. **Liu R, et al. **Hi-C, a chromatin 3D structure technique advancing the functional genomics of immune cells. Front Genet. 2024 Mar 22;15:1377238. doi: 10.3389/fgene.2024.1377238. PMID: 38586584; PMCID: PMC10995239.

11. **Zouari, Y.B., Molitor, A.M., & Sexton, T. **(2018). Sailing the Hi-C’s: Benefits and Remaining Challenges in Mapping Chromatin Interactions.

12. **Hakim O, Misteli T. **SnapShot: Chromosome confirmation capture. Cell. 2012 Mar 2;148(5):1068.e1-2. doi: 10.1016/j.cell.2012.02.019. PMID: 22385969; PMCID: PMC6374129.

---

*This tutorial provides a comprehensive introduction to Hi-C data analysis using Juicer and visualization with Juicebox. For the most current protocols and method updates, always consult the official Juicer documentation and recent publications in the field. The 3D genomics field evolves rapidly, so staying current with new methods and best practices is essential for successful analysis.*
