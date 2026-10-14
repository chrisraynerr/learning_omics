# How to Analyze RNAseq Data for Absolute Beginners Part 12: A Step-By-Step Guide for Submitting Your NGS Data to NCBI GEO

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** Database
- **Source:** [https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/)

---

In the world of genomics research, sharing your sequencing data isn’t just a box to check for publication – it’s a fundamental part of advancing scientific knowledge. As researchers, we spend countless hours generating and analyzing Next-Generation Sequencing (NGS) data, and making this data accessible to the broader scientific community ensures our work can have the widest possible impact. This guide will walk you through the process of submitting your NGS data to NCBI’s Gene Expression Omnibus (GEO), with practical insights from years of experience in genomics research.

Table of Contents

[Toggle](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/)

- [Understanding the Importance of Data Sharing](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Understanding_the_Importance_of_Data_Sharing)
- [Getting Started: Setting Up Your Submission](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Getting_Started_Setting_Up_Your_Submission)

  - [Creating Your NCBI Account](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Creating_Your_NCBI_Account)
  - [Preparing Your Metadata: The Heart of Your Submission](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Preparing_Your_Metadata_The_Heart_of_Your_Submission)
  - [Understanding MD5 Checksums: Your Data’s Digital Fingerprint](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Understanding_MD5_Checksums_Your_Datas_Digital_Fingerprint)
  - [The Upload Process: Making It Smooth and Error-Free](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#The_Upload_Process_Making_It_Smooth_and_Error-Free)
  - [Managing Data Release and Access](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Managing_Data_Release_and_Access)

- [Troubleshooting Common Issues](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Troubleshooting_Common_Issues)
- [Best Practices from Experience](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Best_Practices_from_Experience)
- [Conclusion](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Conclusion)

  - [Share this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Share_this)
  - [Like this:](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-12-a-step-by-step-guide-for-submitting-your-ngs-data-to-ncbi-geo/#Like_this)

## Understanding the Importance of Data Sharing

When I first started working with NGS data, I quickly realized that access to other researchers’ datasets was invaluable for both validating my own findings and generating new hypotheses. Data sharing serves multiple crucial functions in our scientific ecosystem. It enables independent validation of research findings, which is the cornerstone of scientific reproducibility. More importantly, it opens doors for innovative reanalysis that might reveal patterns or relationships we never considered in our original study.

While several platforms support NGS data sharing – including the European Nucleotide Archive (ENA) and DNA Data Bank of Japan (DDBJ) – NCBI GEO has emerged as a preferred choice for many researchers. Let me explain why.

First, GEO’s interface strikes an excellent balance between comprehensiveness and usability. The submission portal guides you through each step with clear instructions, making it accessible even if you’re new to data submission. The platform’s emphasis on detailed metadata ensures that your experimental conditions and protocols are thoroughly documented, which is crucial for other researchers who might want to build upon your work.

What really sets GEO apart is its integration with other NCBI resources. Your dataset becomes part of a larger ecosystem, seamlessly connecting with resources like the Sequence Read Archive (SRA), PubMed, and RefSeq. This integration significantly increases the visibility and utility of your data.

| Database Name | Data Security | Accepted Data Types | Pros | Cons |
| --- | --- | --- | --- | --- |
| GEO (Gene Expression Omnibus) | Public (some controlled access) | Processed gene expression data (counts, FPKM, microarray) | Well-known for transcriptomics, integrates with NCBI | No raw NGS reads, primarily for processed data |
| NCBI SRA (Sequence Read Archive) | Controlled & public access | Raw NGS reads (FASTQ, BAM, etc.) | Free, widely used, supports metadata | Public access requires proper consent, submission process can be complex |
| dbGaP (Database of Genotypes and Phenotypes) | Controlled access | Genomic & phenotype data | Secure, supports human data | Requires approval for access, not for general sharing |
| ENA (European Nucleotide Archive) | Controlled & public access | Raw reads, assemblies, annotations | Good integration with other EMBL-EBI databases, supports long-term archiving | Complex submission process |
| EGA (European Genome-Phenome Archive) | Controlled access | Human genomic & phenotype data | Strict security, GDPR-compliant | Submission process is restrictive |
| DDBJ Sequence Read Archive (DRA) | Controlled & public access | Raw reads (FASTQ, BAM) | Asia-focused, integrated with NCBI SRA & ENA | Limited global visibility compared to NCBI/ENA |
| ArrayExpress | Public (some controlled access) | Gene expression data | Supports structured metadata, linked with ENA | No raw reads, focuses on microarray and RNA-seq |
| GISAID | Controlled access | Viral genome sequences | Promotes data sharing for viral outbreaks | Limited access, data cannot be freely redistributed |
| GEMINI (Genetic Epidemiology and Genomic Information Network) | Controlled access | Variant data (VCF, phenotypic data) | Integrates phenotype-genotype data | Access restrictions |
| Zenodo / Figshare | User-defined access (public/private) | Any NGS-related data (FASTQ, VCF, scripts) | DOI assignment, good for supplementary data | Not a dedicated genomic database |

## Getting Started: Setting Up Your Submission

Before we dive into the technical details, let’s organize everything you’ll need for a smooth submission process. Think of this like preparing for a long journey – having everything in order before you start will save you considerable time and frustration later.

### Creating Your NCBI Account

First, you’ll need to establish your presence in the NCBI ecosystem. Navigate to the [NCBI account creation page](https://account.ncbi.nlm.nih.gov/signup/?back_url=https://www.ncbi.nlm.nih.gov/geo/submitter/). You’ll see several options for authentication, including Google, NIH Login, eRA Commons, Login.gov, ORCID, and various institutional logins.

From personal experience, I recommend using a Google account for your submissions, particularly a personal one rather than an institutional email. Here’s why: institutional email access can expire when you change positions, but you’ll want to maintain access to your submissions long-term. I learned this the hard way when changing institutions and had to go through additional verification steps to regain access to my submissions.

### Preparing Your Metadata: The Heart of Your Submission

The metadata spreadsheet is where you tell the story of your experiment. While it might seem tedious, thorough metadata is what makes your data truly valuable to other researchers. Let’s break this down into manageable steps:

1. Visit the [GEO data submission page](https://www.ncbi.nlm.nih.gov/geo/info/submission.html)

2. Navigate to [Submit high-throughput sequencing (HTS)](https://www.ncbi.nlm.nih.gov/geo/info/seq.html)

3. Download the [metadata template](https://www.ncbi.nlm.nih.gov/geo/info/examples/seq_template.xlsx)

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-14-at-8.45.18%E2%80%AFPM.png?resize=1024%2C607&ssl=1)

The spreadsheet contains multiple tabs, but focus first on the “Metadata” tab. The locked example tabs aren’t just for show – they’re invaluable references that show you exactly what information to provide for different types of experiments.

For RNA-seq data, one common point of confusion is the “processed data file” column. This is where you’ll reference your expression matrix – if you’ve been following along with my previous tutorials, this is the consolidated file we created containing gene expression values for all samples.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-14-at-6.02.13%E2%80%AFPM.png?resize=1024%2C577&ssl=1)

### Understanding MD5 Checksums: Your Data’s Digital Fingerprint

Think of MD5 checksums as your data’s fingerprint – they ensure that what you uploaded is exactly what you intended to share. To generate these MD5 checksums, we’ll use command-line tools through your computer’s terminal interface. Don’t worry if you’re not familiar with the command line – I’ll guide you through the process step by step. While the specific commands differ slightly between Mac and Windows operating systems, the underlying principle remains the same: we’re asking our computer to calculate a unique mathematical signature for each file.

For Mac users:

```bash
# First, install Homebrew if you haven't already
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Then install md5sum through Homebrew
brew install coreutils
```

For Windows users (using WSL – Windows Subsystem for Linux):

```bash
# Update your package list and install coreutils
sudo apt update
sudo apt install coreutils
```

Once you have the tools installed, generating checksums is straightforward:

```bash
# Navigate to your data directory that contains FASTQ files and the gene expression matrix
cd ~/GEO_Submission

# Generate MD5 checksums for all files
md5sum * > md5sum.txt

# Verify your checksums (highly recommended)
md5sum -c md5sum.txt
```

The format of the MD5 file:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-14-at-8.40.04%E2%80%AFPM.png?resize=984%2C300&ssl=1)

Verify your checksums:

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-14-at-7.08.47%E2%80%AFPM.png?resize=258%2C173&ssl=1)

After generating your MD5 file, you’ll need to document it in your metadata spreadsheet to ensure GEO can verify your file integrity. Navigate to the “MD5 Checksums” tab in your metadata spreadsheet. Here, you have two options for providing the checksum information: you can either enter the filename “md5sum.txt” if you’ve named your file that way, or you can copy and paste the actual contents of the MD5 file directly into the spreadsheet. Both approaches are equally valid – the key is ensuring that GEO has access to these digital fingerprints to verify your uploaded files.

### The Upload Process: Making It Smooth and Error-Free

The actual upload process requires careful organization and attention to detail. I recommend creating a dedicated directory for your submission files – this helps prevent confusion and makes it easier to track what you’ve uploaded.

Once you have your files prepared, you’ll need to transfer them to GEO using FTP (File Transfer Protocol). Navigate to the [Submit high-throughput sequencing (HTS)](https://www.ncbi.nlm.nih.gov/geo/info/seq.html) page and locate Step 7, which provides detailed FTP transfer instructions. Start by installing FileZilla, which provides a user-friendly interface for file transfers. While there are other FTP clients available, FileZilla’s visual interface makes it easier to verify your uploads and track progress. You’ll need to configure it with your GEO FTP credentials, which you can find in Step 2 (below) of the submission instructions. These credentials are unique to your submission and ensure your files are securely transferred to the correct location on GEO’s servers.

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2024/12/Screenshot-2024-12-14-at-7.29.07%E2%80%AFPM.png?resize=1024%2C870&ssl=1)

While FileZilla provides a graphical interface, there’s an even more efficient way to upload your files using a command-line tool called `lftp`. This method might seem more technical at first, but it often proves faster and more reliable for large datasets.

Let’s walk through the process step by step. First, you’ll need to install `lftp` on your system. The installation commands differ slightly depending on your operating system:

```bash
# For Mac users:
# Install lftp using Homebrew - our package manager for Mac
brew install lftp

# For Windows users (using WSL):
# Update the package list and install lftp
sudo apt update
sudo apt install lftp
```

Once you have `lftp` installed, you can connect to GEO’s servers using your unique credentials (found in Step 2 of the submission instructions shown above):

```bash
# Connect to GEO's FTP server
# Replace 'yourpassword' with your actual password from Step 2
lftp ftp://geoftp:yourpassword@ftp-private.ncbi.nlm.nih.gov

# Navigate to your designated upload folder
# Replace 'yourgeofolder' with your assigned folder name from Step 2
cd uploads/yourgeofolder

# Upload all files from your local GEO_Submission folder
# The -R flag tells mirror to work in reverse, copying from local to remote
mirror -R ~/GEO_Submission
```

The `mirror` command is particularly powerful – it automatically handles all your files in one go, maintaining their organization and checking for any existing files to avoid duplicate uploads. The `-R` flag tells `mirror` to work in reverse mode, copying from your local computer to the remote server rather than the other way around.

One significant advantage of this method is that if your connection drops during the upload, you can simply run the same `mirror` command again, and `lftp` will automatically resume where it left off, uploading only the files that haven’t been completely transferred yet.

Upload your files in this order:

1. FASTQ files first (these are typically your largest files)

2. Your gene expression matrix

3. The MD5 checksum file

4. Finally, submit your metadata spreadsheet through the web interface

### Managing Data Release and Access

One of the most common questions I get is about controlling when data becomes publicly available. GEO gives you flexibility here – you can set a release date that aligns with your publication timeline. Before that date, your data remain private, but you can generate special access tokens (the “Reviewer access” link on the upper right corner of your GEO dataset page) for reviewers during the manuscript review process.

## Troubleshooting Common Issues

Over years of helping researchers with their submissions, I’ve encountered several common issues. Here’s how to handle them:

When uploads fail, first check your internet connection stability. For large files, consider breaking the upload into smaller sessions. If you get MD5 mismatch errors after upload, don’t panic – often, simply re-uploading the affected files resolves the issue.

Metadata validation errors are another common hurdle. The most frequent causes are missing required fields or formatting inconsistencies. Always refer back to the example sheets when in doubt, and don’t hesitate to contact the GEO team – they’re incredibly helpful and usually respond within a few days.

## Best Practices from Experience

After helping numerous researchers through this process, I’ve developed some best practices that can save you time and headaches:

1. Use clear, consistent file naming conventions from the start

2. Keep a local backup of all submitted files

3. Document any file modifications or special processing steps

4. Track versions of your processed data and analysis pipeline

5. Write detailed experimental descriptions – you’ll thank yourself later

## Conclusion

While the GEO submission process might seem daunting at first, it’s a crucial step in making your research accessible and impactful. Remember, every dataset you share could be the key to someone else’s breakthrough. The time you invest in proper documentation and submission is an investment in the broader scientific community.

If you run into any issues during submission, remember that the GEO team is there to help. They have extensive experience in handling submissions and can provide valuable guidance to ensure your data is presented optimally to the scientific community.
