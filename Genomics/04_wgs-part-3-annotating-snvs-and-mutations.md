# How To Analyze Whole Genome Sequencing Data For Absolute Beginners Part 3: Annotating SNVs and Mutations with Multiple Tools

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Genomics
- **Source:** [https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/)

---

*A comprehensive step-by-step guide to understanding the functional impact of genomic variants using GATK Funcotator, Ensembl VEP, SnpEff, and ANNOVAR*

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/)

- [Introduction: From Variants to Biological Meaning](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Introduction_From_Variants_to_Biological_Meaning)

  - [What is Variant Annotation?](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#What_is_Variant_Annotation)
  - [Why Use Multiple Annotation Tools?](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Why_Use_Multiple_Annotation_Tools)

- [Setting Up the Annotation Environment](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Setting_Up_the_Annotation_Environment)

  - [Installing Required Software](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Installing_Required_Software)
  - [Downloading Annotation Databases](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Downloading_Annotation_Databases)
  - [Setting Up ANNOVAR](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Setting_Up_ANNOVAR)

- [Annotation with GATK Funcotator](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Annotation_with_GATK_Funcotator)

  - [Running Funcotator on Germline Variants](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Running_Funcotator_on_Germline_Variants)
  - [Running Funcotator on Somatic Mutations](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Running_Funcotator_on_Somatic_Mutations)

- [Comprehensive Annotation with Ensembl VEP](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Comprehensive_Annotation_with_Ensembl_VEP)

  - [Running VEP Annotation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Running_VEP_Annotation)

- [Fast Annotation with SnpEff](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Fast_Annotation_with_SnpEff)

  - [Running SnpEff Annotation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Running_SnpEff_Annotation)

- [Population Analysis with ANNOVAR](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Population_Analysis_with_ANNOVAR)

  - [Running ANNOVAR Annotation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Running_ANNOVAR_Annotation)

- [Understanding and Exploring Your Annotation Results](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Understanding_and_Exploring_Your_Annotation_Results)

  - [Understanding Funcotator Output](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Understanding_Funcotator_Output)
  - [Understanding VEP Output](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Understanding_VEP_Output)
  - [Understanding SnpEff Output](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Understanding_SnpEff_Output)
  - [Understanding ANNOVAR Output](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Understanding_ANNOVAR_Output)

- [Conclusion](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Conclusion)

  - [Key Takeaways](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Key_Takeaways)
  - [Next Steps](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Next_Steps)

- [References](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#References)

  - [Tool Documentation](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Tool_Documentation)
  - [Key Databases](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Key_Databases)
  - [Further Learning](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Further_Learning)
  - [Share this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-3-annotating-snvs-and-mutations-with-multiple-tools/#Like_this)

## Introduction: From Variants to Biological Meaning

After successfully identifying genomic variants using GATK (covered in [Part 1](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-1-from-raw-reads-to-high-quality-variants-using-gatk/)) and discovering somatic mutations with Mutect2 (detailed in [Part 2A](https://ngs101.com/how-to-analyze-whole-genome-sequencing-data-for-absolute-beginners-part-2a-matched-tumor-normal-mutation-calling-with-mutect2/)), you now have VCF files containing thousands of genetic variants. However, knowing that position chr17:43,044,295 changed from G to A tells us nothing about its biological significance.

### What is Variant Annotation?

Variant annotation is the process of adding biological context to your genetic variants. It’s like translating genetic coordinates into meaningful biological information. Raw variants are similar to having GPS coordinates without knowing whether they point to a hospital, a park, or an empty field.

For example, a variant at chr17:43,044,295 G>A becomes much more meaningful when annotation reveals it:

- Falls in the BRCA1 gene

- Creates a missense mutation (p.Ala1708Thr)

- Has been classified as “Pathogenic” in ClinVar

- Is extremely rare in the population (AF < 0.0001)

This transformation from coordinates to biological understanding is what makes genomic data clinically actionable.

### Why Use Multiple Annotation Tools?

No single tool provides complete information. Each has unique strengths:

- **GATK Funcotator** – GATK’s native annotator with clinical database focus

- **Ensembl VEP** – Comprehensive consequence prediction with extensive options

- **SnpEff** – Fast annotation with powerful filtering capabilities

- **ANNOVAR** – Excellent for population frequency analysis

By using multiple tools, you gain different perspectives on each variant and build confidence in your interpretations.

## Setting Up the Annotation Environment

Let’s establish a complete annotation environment with all necessary tools and databases.

### Installing Required Software

```bash
#-----------------------------------------------
# STEP 0: Set up annotation environment
#-----------------------------------------------

# Activate WGS analysis environment from part 1
conda activate wgs_analysis

# Install all required tools for variant annotation
# The -y flag automatically answers "yes" to installation prompts
conda install -y \
    gatk4 \                    # GATK toolkit including Funcotator
    ensembl-vep \              # Ensembl Variant Effect Predictor
    snpeff \                   # SnpEff annotation tool
    snpsift \                  # SnpSift filtering companion to SnpEff
    tabix \                    # Tool for indexing VCF files
    bcftools \                 # Tools for manipulating VCF files
    wget \                     # Tool for downloading files from web
    perl                       # Perl interpreter (needed by some tools)
```

### Downloading Annotation Databases

```bash
#-----------------------------------------------
# STEP 1: Download annotation databases
#-----------------------------------------------

# Create directory structure for all annotation databases
# This keeps everything organized in one place
mkdir -p ~/wgs_annotation/annotation_databases
cd ~/wgs_annotation/annotation_databases

#=============================================
# GATK Funcotator databases
#=============================================

# Download the Funcotator data sources using GATK's built-in downloader
# --somatic: Downloads databases relevant for cancer/somatic analysis
# --validate-integrity: Checks that downloaded files aren't corrupted
# --extract-after-download: Automatically extracts compressed files
# --hg38: Downloads databases for human genome build hg38
gatk FuncotatorDataSourceDownloader \
    --somatic \
    --validate-integrity \
    --extract-after-download \
    --hg38

# Set environment variable pointing to the downloaded Funcotator data
# This tells Funcotator where to find its annotation databases
export FUNCOTATOR_DATA_SOURCES_PATH="$(pwd)/funcotator_dataSources.v1.8.hg38.20230908s"

#=============================================
# VEP cache and plugins
#=============================================

# Create directory structure for VEP data and plugins
mkdir -p vep_data/plugins
cd vep_data

# Install VEP cache using the built-in installer
# -a cf: Install cache and FASTA files
# -s homo_sapiens: Install for human species
# -y GRCh38: Use GRCh38 genome assembly
# --CACHE_VERSION 115: Use Ensembl version 115
# --CACHEDIR: Specify where to install the cache
vep_install \
    -a cf \
    -s homo_sapiens \
    -y GRCh38 \
    --CACHE_VERSION 115 \
    --CACHEDIR $(pwd)/vep_cache

# Alternative: Download cache manually if the installer fails
# This is a large file (several GB), so it may take time
wget https://ftp.ensembl.org/pub/release-115/variation/indexed_vep_cache/homo_sapiens_vep_115_GRCh38.tar.gz

# Download useful VEP plugins for enhanced annotation
# These plugins add additional prediction scores and databases
cd plugins

# dbNSFP plugin: Adds multiple pathogenicity prediction scores
wget https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/115/dbNSFP.pm

# CADD plugin: Adds CADD pathogenicity scores
wget https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/115/CADD.pm

# REVEL plugin: Adds REVEL pathogenicity scores
wget https://raw.githubusercontent.com/Ensembl/VEP_plugins/release/115/REVEL.pm

# Return to annotation databases directory
cd ../../

#=============================================
# SnpEff database
#=============================================

# Download the human genome annotation database for SnpEff
# GRCh38.105 refers to genome build GRCh38, Ensembl version 105
snpEff download GRCh38.105
```

### Setting Up ANNOVAR

ANNOVAR requires manual registration and download:

```bash
#-----------------------------------------------
# STEP 2: Setup ANNOVAR
#-----------------------------------------------

# Create directory for ANNOVAR installation
mkdir -p tools/annovar
cd tools/annovar

# ANNOVAR requires manual registration - provide instructions
echo "===== IMPORTANT: ANNOVAR Manual Setup Required ====="
echo "Please download ANNOVAR manually:"
echo "1. Visit: https://www.openbioinformatics.org/annovar/annovar_download_form.php"
echo "2. Register with your academic email address"
echo "3. Download the annovar.latest.tar.gz file"
echo "4. Extract it to this directory: $(pwd)"
echo ""
echo "After downloading and extracting, add to PATH:"
echo "export PATH=$(pwd)/annovar:\$PATH"
echo "========================================================="

# After ANNOVAR is installed, download databases
cd annovar

# Add ANNOVAR to the current PATH for this session
export PATH=$(pwd):$PATH

# Gene annotation database
# -buildver hg38: Use human genome build hg38
# -downdb: Download database
# -webfrom annovar: Download from ANNOVAR website
# refGene: Gene annotation database
perl annotate_variation.pl -buildver hg38 -downdb -webfrom annovar refGene humandb/

# Population frequency databases
# gnomad312_genome: gnomAD genome frequencies (version 3.1.2)
perl annotate_variation.pl -buildver hg38 -downdb -webfrom annovar gnomad312_genome humandb/

# 1000g2015aug: 1000 Genomes Project frequencies (August 2015 release)
perl annotate_variation.pl -buildver hg38 -downdb -webfrom annovar 1000g2015aug humandb/

# Clinical databases
# clinvar_20220320: ClinVar clinical significance database (March 2022)
perl annotate_variation.pl -buildver hg38 -downdb -webfrom annovar clinvar_20220320 humandb/

# Functional prediction databases
# dbnsfp42c: dbNSFP database with multiple prediction scores (version 4.2c)
perl annotate_variation.pl -buildver hg38 -downdb -webfrom annovar dbnsfp42c humandb/

# Return to main annotation directory
cd ../../../
```

## Annotation with GATK Funcotator

GATK Funcotator provides clinically-focused annotation with strong integration to clinical databases.

### Running Funcotator on Germline Variants

```bash
#-----------------------------------------------
# STEP 3: GATK Funcotator annotation
#-----------------------------------------------

# Set up variables for file paths
PROJECT_DIR="~/wgs_annotation"
cd ${PROJECT_DIR}

# Input files from previous tutorials
# The example VCF files are from Part 1 and Part 2A tutorials
INPUT_VCF="normal1_filtered.vcf.gz"  # Germline variants from Part 1
REFERENCE_FASTA="~/wgs_analysis/reference/Homo_sapiens_assembly38.fasta"  # Human reference genome
FUNCOTATOR_DATA="${FUNCOTATOR_DATA_SOURCES_PATH}"  # Path to Funcotator databases

# Create output directory for Funcotator results
OUTPUT_DIR="output/funcotator"
mkdir -p ${OUTPUT_DIR}

# Basic Funcotator annotation - VCF output format
gatk Funcotator \
    --variant ${INPUT_VCF} \                     # Input VCF file with variants
    --reference ${REFERENCE_FASTA} \             # Reference genome file
    --ref-version hg38 \                         # Genome version (must match your data)
    --data-sources-path ${FUNCOTATOR_DATA} \     # Path to annotation databases
    --output ${OUTPUT_DIR}/normal1_filtered_funcotator.vcf \  # Output VCF file
    --output-file-format VCF \                   # Output format (VCF keeps original structure)
    --remove-filtered-variants false             # Keep all variants, even those marked as filtered

# Funcotator annotation - MAF output format
gatk Funcotator \
    --variant ${INPUT_VCF} \                     # Same input file
    --reference ${REFERENCE_FASTA} \             # Same reference genome
    --ref-version hg38 \                         # Same genome version
    --data-sources-path ${FUNCOTATOR_DATA} \     # Same annotation databases
    --output ${OUTPUT_DIR}/normal1_filtered_funcotator.maf \  # Output in MAF format
    --output-file-format MAF \                   # MAF = Mutation Annotation Format (table)
    --remove-filtered-variants false             # Include all variants
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/normal1_vcf_funcotator.png?resize=1024%2C92&ssl=1)

### Running Funcotator on Somatic Mutations

```bash
# Annotate somatic mutations with emphasis on cancer databases
# Input file from Part 2A tutorial (tumor vs normal comparison)
SOMATIC_VCF="tumor1_vs_normal1_high_confidence.vcf.gz"
SOMATIC_OUTPUT_DIR="${PROJECT_DIR}/output/funcotator/somatic"

# Create separate directory for somatic results
mkdir -p ${SOMATIC_OUTPUT_DIR}

gatk Funcotator \
    --variant ${SOMATIC_VCF} \                   # Somatic mutations from Mutect2
    --reference ${REFERENCE_FASTA} \             # Same reference genome
    --ref-version hg38 \                         # Same genome version
    --data-sources-path ${FUNCOTATOR_DATA} \     # Same annotation databases
    --output ${SOMATIC_OUTPUT_DIR}/somatic_mutations_funcotator.maf \  # Output file
    --output-file-format MAF \                   # MAF format for easy analysis
    --remove-filtered-variants false \           # Keep all variants
    --transcript-selection-mode CANONICAL \      # Use canonical (main) transcripts only
    --verbosity INFO                             # Show detailed progress information
```

## Comprehensive Annotation with Ensembl VEP

VEP provides detailed consequence prediction with extensive customization options.

### Running VEP Annotation

```bash
#-----------------------------------------------
# STEP 4: Ensembl VEP annotation
#-----------------------------------------------

# Set up VEP-specific variables
VEP_OUTPUT_DIR="${PROJECT_DIR}/output/vep"           # VEP output directory
VEP_CACHE_DIR="~/wgs_annotation/annotation_databases/vep_data/"  # VEP cache location
VEP_PLUGINS_DIR="~/wgs_annotation/annotation_databases/vep_data/plugins"  # VEP plugins

# Create output directory
mkdir -p ${VEP_OUTPUT_DIR}

# Basic VEP annotation with comprehensive options
vep \
    --input_file ${INPUT_VCF} \                  # Input VCF file
    --output_file ${VEP_OUTPUT_DIR}/normal1_filtered_vep.vcf \  # Output file
    --format vcf \                               # Input format is VCF
    --vcf \                                      # Output format is VCF
    --symbol \                                   # Include gene symbols
    --terms SO \                                 # Use Sequence Ontology terms
    --tsl \                                      # Include transcript support level
    --biotype \                                  # Include transcript biotype
    --hgvs \                                     # Include HGVS nomenclature
    --hgvsg \                                    # Include genomic HGVS
    --canonical \                                # Mark canonical transcripts
    --protein \                                  # Include protein identifiers
    --ccds \                                     # Include CCDS identifiers
    --uniprot \                                  # Include UniProt identifiers
    --domains \                                  # Include protein domain info
    --regulatory \                               # Include regulatory region info
    --numbers \                                  # Include position numbers
    --total_length \                             # Include transcript length
    --allele_number \                            # Include allele numbers
    --no_escape \                                # Don't escape special characters
    --xref_refseq \                              # Include RefSeq cross-references
    --species homo_sapiens \                     # Species (human)
    --assembly GRCh38 \                          # Genome assembly
    --offline \                                  # Use local cache (no internet required)
    --cache \                                    # Use pre-downloaded cache
    --dir_cache ${VEP_CACHE_DIR} \               # Cache directory location
    --fasta ~/references/hg38/hg38.fa \          # Reference genome FASTA
    --force_overwrite \                          # Overwrite existing output files
    --stats_file ${VEP_OUTPUT_DIR}/normal1_filtered_vep_stats.html \  # Generate HTML stats
    --warning_file ${VEP_OUTPUT_DIR}/normal1_filtered_vep_warnings.txt \  # Log warnings
    --fork 8                                     # Use 8 CPU cores for speed

#=============================================
# VEP with population frequencies
#=============================================

vep \
    --input_file ${INPUT_VCF} \                  # Same input file
    --output_file ${VEP_OUTPUT_DIR}/normal1_filtered_vep_with_freq.vcf \  # Different output
    --format vcf \                               # Input format
    --vcf \                                      # Output format
    --symbol \                                   # Gene symbols
    --terms SO \                                 # Sequence Ontology terms
    --canonical \                                # Canonical transcripts
    --hgvs \                                     # HGVS nomenclature
    --species homo_sapiens \                     # Human species
    --assembly GRCh38 \                          # Genome assembly
    --offline \                                  # Use local cache
    --cache \                                    # Use cache
    --dir_cache ${VEP_CACHE_DIR} \               # Cache location
    --fasta ~/references/hg38/hg38.fa \          # Reference FASTA
    --af \                                       # Add allele frequencies
    --af_1kg \                                   # Add 1000 Genomes frequencies
    --af_gnomad \                                # Add gnomAD frequencies
    --max_af \                                   # Add maximum allele frequency
    --force_overwrite \                          # Overwrite existing files
    --fork 8                                     # Use 8 CPU cores

#=============================================
# VEP with advanced plugins (if available)
#=============================================

# Note: This section requires additional database downloads for the plugins
# It may fail if the plugin databases aren't available - this is normal
vep \
    --input_file ${INPUT_VCF} \                  # Input file
    --output_file ${VEP_OUTPUT_DIR}/normal1_filtered_vep_plugins.tsv \  # Tab-separated output
    --format vcf \                               # Input format
    --tab \                                      # Output as tab-separated table
    --symbol \                                   # Gene symbols
    --canonical \                                # Canonical transcripts
    --hgvs \                                     # HGVS nomenclature
    --protein \                                  # Protein information
    --species homo_sapiens \                     # Human species
    --assembly GRCh38 \                          # Genome assembly
    --offline \                                  # Use local cache
    --cache \                                    # Use cache
    --dir_cache ${VEP_CACHE_DIR} \               # Cache location
    --fasta ~/references/hg38/hg38.fa \          # Reference FASTA
    --dir_plugins ${VEP_PLUGINS_DIR} \           # Plugin directory
    --plugin dbNSFP,${VEP_PLUGINS_DIR}/dbNSFP4.4a.txt.gz,SIFT_score,SIFT_pred,Polyphen2_HDIV_score,Polyphen2_HDIV_pred,MutationTaster_score,MutationTaster_pred,CADD_raw,CADD_phred \
    --fields "Uploaded_variation,Location,Allele,Gene,Feature,Feature_type,Consequence,cDNA_position,CDS_position,Protein_position,Amino_acids,Codons,Existing_variation,SYMBOL,CANONICAL,HGVSc,HGVSp,dbNSFP_SIFT_score,dbNSFP_SIFT_pred,dbNSFP_Polyphen2_HDIV_score,dbNSFP_Polyphen2_HDIV_pred,dbNSFP_CADD_phred" \
    --force_overwrite \                          # Overwrite existing files
    --fork 8                                     # Use 8 CPU cores
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/normal1_vcf_vep.png?resize=1024%2C45&ssl=1)

## Fast Annotation with SnpEff

SnpEff provides rapid annotation with integrated filtering through SnpSift.

### Running SnpEff Annotation

```bash
#-----------------------------------------------
# STEP 5: SnpEff annotation
#-----------------------------------------------

# Create output directory for SnpEff results
SNPEFF_OUTPUT_DIR="${PROJECT_DIR}/output/snpeff"
mkdir -p ${SNPEFF_OUTPUT_DIR}

# SnpEff annotation
snpEff ann \
    -v \                                         # Verbose output (show progress)
    -stats ${SNPEFF_OUTPUT_DIR}/normal1_filtered_snpeff_stats.html \  # Generate HTML statistics
    -csvStats ${SNPEFF_OUTPUT_DIR}/normal1_filtered_snpeff_stats.csv \  # Generate CSV statistics
    -canon \                                     # Use canonical transcripts
    -hgvs \                                      # Include HGVS nomenclature
    GRCh38.p13 \                                 # Genome database to use
    ${INPUT_VCF} \                               # Input VCF file
    > ${SNPEFF_OUTPUT_DIR}/normal1_filtered_snpeff.vcf  # Output VCF file
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/SnpEff-Annotation.png?resize=1024%2C168&ssl=1)

## Population Analysis with ANNOVAR

ANNOVAR provides excellent population frequency analysis and flexible database integration.

### Running ANNOVAR Annotation

```bash
#-----------------------------------------------
# STEP 6: ANNOVAR annotation
#-----------------------------------------------

# Set up ANNOVAR-specific variables
OUTPUT_DIR="${PROJECT_DIR}/output/annovar"       # ANNOVAR output directory
ANNOVAR_DIR="~/wgs_annotation/annotation_databases/tools/annovar/annovar"  # ANNOVAR installation

# Create output directory
mkdir -p ${OUTPUT_DIR}

# Add ANNOVAR to PATH for this session
export PATH=${ANNOVAR_DIR}:$PATH

#=============================================
# Convert VCF to ANNOVAR input format
#=============================================

# Convert VCF format to ANNOVAR's input format
# -format vcf4: Input is VCF version 4
perl ${ANNOVAR_DIR}/convert2annovar.pl \
    -format vcf4 \                               # Input format
    ${INPUT_VCF} \                               # Input VCF file
    > ${OUTPUT_DIR}/normal1_filtered.avinput     # Output ANNOVAR input file

#=============================================
# Gene-based annotation
#=============================================

# Run gene-based annotation using RefGene database
perl ${ANNOVAR_DIR}/table_annovar.pl \
    ${OUTPUT_DIR}/normal1_filtered.avinput \     # Input file (ANNOVAR format)
    ${ANNOVAR_DIR}/humandb/ \                    # Database directory
    -buildver hg38 \                             # Genome version
    -out ${OUTPUT_DIR}/normal1_filtered_gene \   # Output prefix
    -remove \                                    # Remove temporary files
    -protocol refGene \                          # Use RefGene annotation
    -operation g \                               # g = gene-based annotation
    -nastring . \                                # Use "." for missing data
    -csvout                                      # Output in CSV format

#=============================================
# Comprehensive annotation with multiple databases
#=============================================

# Run comprehensive annotation using multiple databases
perl ${ANNOVAR_DIR}/table_annovar.pl \
    ${OUTPUT_DIR}/normal1_filtered.avinput \     # Input file
    ${ANNOVAR_DIR}/humandb/ \                    # Database directory
    -buildver hg38 \                             # Genome version
    -out ${OUTPUT_DIR}/normal1_filtered_comprehensive \  # Output prefix
    -remove \                                    # Remove temporary files
    -protocol refGene,gnomad312_genome,clinvar_20220320,dbnsfp42c \  # Multiple databases
    -operation g,f,f,f \                         # g=gene, f=filter (frequency/annotation)
    -nastring . \                                # Use "." for missing data
    -csvout                                      # Output in CSV format
```

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/annovar_gene.png?resize=1024%2C135&ssl=1)

## Understanding and Exploring Your Annotation Results

Now that you have annotations from multiple tools, let’s understand what each output contains and how to explore them effectively.

### Understanding Funcotator Output

Funcotator produces two main output formats:

#### VCF Format Output

The annotated VCF contains all original information plus functional annotations in the INFO field:

```bash
# View the header to understand annotations
bcftools view -h output/funcotator/normal1_filtered_funcotator.vcf | grep "##INFO=<ID=FUNCOTATION"

# Extract first few annotated variants
bcftools view -H output/funcotator/normal1_filtered_funcotator.vcf | head -5
```

The FUNCOTATION field contains pipe-separated annotations with information about:

- Gene symbol and transcript

- Variant classification (Missense_Mutation, Silent, etc.)

- HGVSc and HGVSp notation

- Consequence and impact

- Population frequencies

- Clinical significance

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/vcf_format.png?resize=1024%2C74&ssl=1)

#### MAF Format Output

The MAF (Mutation Annotation Format) file is a tab-delimited table that’s easier to read.

Key MAF columns include:

- **Hugo_Symbol**: Gene name

- **Variant_Classification**: Type of mutation (Missense_Mutation, Nonsense_Mutation, etc.)

- **HGVSp**: Protein-level change description

- **Transcript_ID**: Reference transcript used

- **Genome_Change**: Genomic coordinate change

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/maf_format.png?resize=1024%2C89&ssl=1)

### Understanding VEP Output

VEP provides rich consequence prediction with multiple output formats.

The CSQ field contains consequence annotations with information about:

- **Consequence**: Type of variant effect (missense_variant, synonymous_variant, etc.)

- **SYMBOL**: Gene symbol

- **HGVSc/HGVSp**: HGVS notation for cDNA and protein changes

- **CANONICAL**: Whether this is the canonical transcript

- **BIOTYPE**: Type of transcript (protein_coding, etc.)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/normal1_vcf_vep-1.png?resize=1024%2C45&ssl=1)

### Understanding SnpEff Output

SnpEff adds ANN (Annotation) fields to VCF files.

The ANN field format is: Allele|Annotation|Impact|Gene_Name|Gene_ID|Feature_Type|Feature_ID|Transcript_BioType|Rank|HGVS.c|HGVS.p|cDNA.pos/cDNA.length|CDS.pos/CDS.length|AA.pos/AA.length|Distance|ERRORS

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/08/SnpEff-Annotation.png?resize=1024%2C168&ssl=1)

### Understanding ANNOVAR Output

ANNOVAR produces easy-to-read CSV files.

Key ANNOVAR columns:

- **Func.refGene**: Functional region (exonic, intronic, UTR, etc.)

- **Gene.refGene**: Gene name

- **ExonicFunc.refGene**: Exonic function (missense, nonsense, synonymous, etc.)

- **AAChange.refGene**: Amino acid change

- **gnomad312_genome_AF**: Population frequency from gnomAD

- **clinvar_20220320**: ClinVar clinical significance

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/09/annovar_gene.png?resize=1024%2C135&ssl=1)

## Conclusion

You have successfully completed a comprehensive variant annotation workflow using four powerful tools. This tutorial has guided you through:

- Setting up complete annotation environments with detailed explanations

- Running each annotation tool with appropriate parameters and understanding what each does

- Understanding different output formats and their contents

- Converting complex VCF annotations to readable tables

- Exploring and finding high-impact variants

### Key Takeaways

**Tool Selection Strategy:**

- **Funcotator** excels in GATK integration and provides clinically-focused annotations with cancer databases

- **VEP** offers the most comprehensive consequence prediction with extensive customization options

- **SnpEff** provides rapid annotation with excellent filtering capabilities through SnpSift

- **ANNOVAR** delivers superior population frequency analysis and flexible database integration

**Critical Success Factors:**

- Maintain current and consistent database versions across all tools

- Understand the different output formats each tool provides

- Use multiple tools to gain different perspectives on the same variants

- Convert VCF annotations to tables for easier analysis

**Understanding Your Data:**

- Each tool provides different perspectives on the same variants

- High-impact variants (stop-gain, frameshift) require immediate attention

- Population frequencies help distinguish rare pathogenic variants from common benign ones

- Clinical databases like ClinVar provide expert-curated variant interpretations

### Next Steps

With your annotation expertise, you can now:

- Develop robust clinical diagnostic pipelines for patient samples

- Perform population genomics studies to understand disease associations

- Integrate variant data with gene expression analyses for functional studies

- Contribute to variant interpretation databases to help the community

## References

### Tool Documentation

- **GATK Funcotator**: [https://gatk.broadinstitute.org/hc/en-us/articles/360037224432](https://gatk.broadinstitute.org/hc/en-us/articles/360037224432)

- **Ensembl VEP**: [https://ensembl.org/info/docs/tools/vep/index.html](https://ensembl.org/info/docs/tools/vep/index.html)

- **SnpEff**: [http://pcingola.github.io/SnpEff/](http://pcingola.github.io/SnpEff/)

- **ANNOVAR**: [https://annovar.openbioinformatics.org/](https://annovar.openbioinformatics.org/)

### Key Databases

- **gnomAD**: [https://gnomad.broadinstitute.org/](https://gnomad.broadinstitute.org/) (Population frequencies)

- **ClinVar**: [https://www.ncbi.nlm.nih.gov/clinvar/](https://www.ncbi.nlm.nih.gov/clinvar/) (Clinical significance)

- **1000 Genomes**: [https://www.internationalgenome.org/](https://www.internationalgenome.org/) (Population data)

- **dbNSFP**: [https://sites.google.com/site/jpopgen/dbNSFP](https://sites.google.com/site/jpopgen/dbNSFP) (Pathogenicity predictions)

### Further Learning

- **ACMG Guidelines**: Standards for variant interpretation in clinical genetics

- **ClinGen**: Clinical genome resource for variant curation

- **COSMIC**: Catalogue of somatic mutations in cancer

---

**This tutorial is part of the NGS101.com series on whole genome sequencing analysis. If this tutorial helped advance your research, please **comment and share your experience** to help other researchers! **Subscribe to stay updated** with our latest bioinformatics tutorials and resources.**
