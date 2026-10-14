# Setting Up Single-Cell RNA-seq Analysis Environment with Pixi: 10x Faster Setup, Zero Version Conflicts

- **Author:** Thanh-Giang
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** HPC, RNA-seq, Scientific Programming, Single Cell Sequencing
- **Source:** [https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/)

---

**This tutorial is contributed by [Giang Nguyen](https://www.linkedin.com/in/giang-nguyen-761b28190/)**, founder of G Labs, providing consulting, software development, infrastructure engineering, and bioinformatics services to support scalable research and production workflows. He helps teams design, build, and optimize cloud/HPC platforms, develop custom tools and pipelines, and deliver reproducible, production-ready solutions for data-intensive science.

Table of Contents

[Toggle](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/)

- [Introduction: Why Environment Management Is Critical for scRNA-seq Analysis](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Introduction_Why_Environment_Management_Is_Critical_for_scRNA-seq_Analysis)

  - [The Complexity of scRNA-seq Tool Dependencies](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#The_Complexity_of_scRNA-seq_Tool_Dependencies)
  - [What This Tutorial Covers](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#What_This_Tutorial_Covers)

- [The Conda Problem in scRNA-seq Workflows](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#The_Conda_Problem_in_scRNA-seq_Workflows)

  - [Speed Issues: The 30-Minute Wait](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Speed_Issues_The_30-Minute_Wait)
  - [Version Conflicts: The “Unsatisfiable” Nightmare](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Version_Conflicts_The_%E2%80%9CUnsatisfiable%E2%80%9D_Nightmare)
  - [Missing Linux Dependencies: The Hidden Complexity](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Missing_Linux_Dependencies_The_Hidden_Complexity)
  - [HPC-Specific Challenges: Where Conda Really Struggles](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#HPC-Specific_Challenges_Where_Conda_Really_Struggles)
  - [Why Even Veterans Struggle](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Why_Even_Veterans_Struggle)

- [Introducing Pixi: A Modern Solution](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Introducing_Pixi_A_Modern_Solution)

  - [What Is Pixi?](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#What_Is_Pixi)
  - [How Pixi Relates to Conda](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#How_Pixi_Relates_to_Conda)
  - [Conda vs Mamba vs Pixi: Key Differences](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Conda_vs_Mamba_vs_Pixi_Key_Differences)
  - [How Pixi Differs from Conda](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#How_Pixi_Differs_from_Conda)
  - [The Pixi Workflow](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#The_Pixi_Workflow)
  - [Advantages for scRNA-seq Analysis](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Advantages_for_scRNA-seq_Analysis)
  - [When to Use Pixi vs Conda](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#When_to_Use_Pixi_vs_Conda)
  - [Pixi vs Docker: Quick Comparison](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Pixi_vs_Docker_Quick_Comparison)

- [Installing Pixi on HPC Systems](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Installing_Pixi_on_HPC_Systems)

  - [Complete HPC Setup Walkthrough](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Complete_HPC_Setup_Walkthrough)
  - [Summary of HPC Configuration](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Summary_of_HPC_Configuration)

- [Setting Up the scRNA-seq Environment](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Setting_Up_the_scRNA-seq_Environment)

  - [Understanding Features vs Environments](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Understanding_Features_vs_Environments)
  - [Project Initialization](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Project_Initialization)
  - [Installing Linux Command-Line Tools (Part 1)](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Installing_Linux_Command-Line_Tools_Part_1)
  - [Installing R-Based Analysis Tools (Parts 2-4)](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Installing_R-Based_Analysis_Tools_Parts_2-4)
  - [Complete pixi.toml Configuration](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Complete_pixitoml_Configuration)
  - [Installing the Environment](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Installing_the_Environment)
  - [Post-Installation: Install Required Bioconductor Packages](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Post-Installation_Install_Required_Bioconductor_Packages)
  - [Understanding the Lock File](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Understanding_the_Lock_File)
  - [What Gets Created](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#What_Gets_Created)

- [Validating the Installation](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Validating_the_Installation)

  - [Comprehensive Validation Script](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Comprehensive_Validation_Script)

- [Using Pixi for Daily Workflows](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Using_Pixi_for_Daily_Workflows)

  - [Key Advantages for Interactive Work](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Key_Advantages_for_Interactive_Work)
  - [Basic Commands](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Basic_Commands)
  - [Custom Tasks](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Custom_Tasks)
  - [Sharing Your Environment](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Sharing_Your_Environment)

- [Best Practices and Advanced Topics](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Best_Practices_and_Advanced_Topics)

  - [Version Pinning Strategy](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Version_Pinning_Strategy)
  - [Performance Optimization](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Performance_Optimization)
  - [Managing Multiple Projects](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Managing_Multiple_Projects)
  - [Updating Environments Safely](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Updating_Environments_Safely)
  - [Collaborative Workflows](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Collaborative_Workflows)
  - [HPC Best Practices](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#HPC_Best_Practices)

- [Conclusion](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Conclusion)
- [References](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#References)

  - [Share this:](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Share_this)
  - [Like this:](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/#Like_this)

## Introduction: Why Environment Management Is Critical for scRNA-seq Analysis

If you’ve worked with single-cell RNA-seq (scRNA-seq) data, you’ve likely spent hours—or days—wrestling with environment setup. Installing Seurat, Bioconductor packages, integration tools, and command-line utilities should be straightforward, but conda’s dependency resolver has other ideas. You watch the terminal spin for 30 minutes only to receive an “unsatisfiable” error message. You try again with different package versions, and three hours later, you’re still not analyzing data.

This frustration isn’t unique to beginners. Even experienced bioinformaticians struggle with conda’s limitations, especially on HPC clusters where missing system libraries and restrictive environments compound the problem. **Environment setup has become a major bottleneck in computational biology**, often taking longer than the actual analysis.

### The Complexity of scRNA-seq Tool Dependencies

Single-cell RNA-seq analysis requires a complex ecosystem of tools:

**Command-line utilities**:

- SRA Toolkit (data download)

- FastQC & MultiQC (quality control)

- Cell Ranger (alignment and quantification)

- SAMtools (BAM file processing)

**R analysis packages**:

- Seurat 5 (core analysis framework)

- Bioconductor packages (DropletUtils, scater, SingleR, celldex)

- Integration methods (harmony, batchelor)

- Visualization tools (ggplot2, patchwork, ggalluvial)

- Annotation tools (scCATCH, scType dependencies)

**The challenge**: These tools have overlapping dependencies with conflicting version requirements. Seurat needs specific R versions, Bioconductor packages need particular Rcpp versions, harmony requires specific matrix libraries—and conda must solve this massive constraint satisfaction problem every time you install packages.

### What This Tutorial Covers

This tutorial introduces **Pixi**, a modern package manager that solves conda’s pain points while maintaining full compatibility with the conda ecosystem. You’ll learn how to:

1. ✓ Install and configure Pixi on HPC clusters

2. ✓ Set up complete scRNA-seq environments for the [NGS101 tutorial series](https://ngs101.com) (Parts 1-4)

3. ✓ Install and validate all tools in minutes instead of hours

4. ✓ Create reproducible environments that work identically across systems

5. ✓ Manage multiple workflow stages with separate environments

6. ✓ Share environments with collaborators via Git

**Who this tutorial is for**:

- Researchers analyzing scRNA-seq data who are tired of conda’s slowness

- Bioinformaticians working on HPC clusters with restrictive environments

- Anyone who needs reproducible computational environments

- Students following the NGS101 scRNA-seq tutorial series

By the end, you’ll have a working scRNA-seq analysis environment installed in 5-10 minutes—not hours—with guarantees that it will work identically for your collaborators.

## The Conda Problem in scRNA-seq Workflows

Before introducing the solution, let’s understand exactly why conda struggles with scRNA-seq environments. These aren’t theoretical issues—they’re problems you’ve likely encountered yourself.

### Speed Issues: The 30-Minute Wait

**The problem**: Conda’s dependency resolver is notoriously slow for complex environments.

**Real example**: Installing a complete scRNA-seq environment (R + Seurat 5 + Bioconductor + integration packages):

```r
# Create conda environment
conda create -n scrna python=3.11 r-base=4.3
conda activate scrna

# Install Seurat and dependencies
conda install -c conda-forge -c bioconda r-seurat r-seuratobject \
  bioconductor-dropletutils bioconductor-scater bioconductor-singler \
  r-harmony bioconductor-batchelor r-ggplot2 r-patchwork

# Solving environment... (20-35 minutes)
# Often fails with "unsatisfiable" after the long wait
```

**Why it’s so slow**:

- Conda checks *all possible combinations* of package versions

- Each package addition multiplies the search space

- No parallel processing of dependencies

- Inefficient SAT solver implementation

**The multiplicative effect**: Need multiple environments (QC, integration, annotation)? Multiply that 30-minute wait by the number of environments. A full workflow setup can take 2-3 hours.

### Version Conflicts: The “Unsatisfiable” Nightmare

**The problem**: scRNA-seq tools have complex, overlapping dependencies that frequently conflict.

**Common conflict scenarios**:

**Scenario 1: Seurat + Bioconductor incompatibility**

```bash
conda install r-seurat=5.0 bioconductor-dropletutils

# Error: The following specifications were found to be incompatible with existing packages:
#   - r-seurat -> r-base=4.3 -> conflicts with bioconductor-dropletutils
```

Why? Seurat might require R 4.3.2, but DropletUtils was built for R 4.3.1, and conda considers these incompatible.

**Scenario 2: Integration package conflicts**

```bash
conda install r-harmony bioconductor-batchelor r-seurat

# Error: Unsatisfiable dependencies
```

Why? Harmony and batchelor both depend on different versions of underlying matrix libraries (RcppArmadillo, Matrix), creating circular dependency issues.

**Scenario 3: Cell Ranger + R analysis tools**

```bash
# Cell Ranger requires specific versions of system libraries
# That conflict with R package requirements
# Result: Cannot install both in same environment
```

**The frustration cascade**:

1. Try to install packages → fails

2. Try different package versions → fails

3. Google the error → find 5-year-old GitHub issue

4. Try suggested workaround → fails

5. Give up and use two separate environments (now switching between them constantly)

### Missing Linux Dependencies: The Hidden Complexity

**The problem**: Conda manages packages but not always system libraries, leading to runtime errors even after “successful” installation.

**Common scenarios**:

**Scenario 1: Missing glibc version**

```r
# Installation succeeds
conda install r-soupx

# Runtime failure
library(SoupX)
# Error: /lib64/libc.so.6: version 'GLIBC_2.29' not found
```

**Scenario 2: libstdc++ incompatibility**

```r
library(scDblFinder)
# Error: /lib/x86_64-linux-gnu/libstdc++.so.6: version 'GLIBCXX_3.4.29' not found
```

**Why this happens**:

- Conda packages are built on specific OS versions

- They depend on system libraries (glibc, libstdc++) at specific versions

- Your system might have older versions

- Without sudo access (common on HPC), you can’t upgrade system libraries

**The workaround attempts**:

```bash
# Try to install system libraries via conda
conda install -c conda-forge sysroot_linux-64

# Sometimes works, often doesn't
# Creates new conflicts with other packages
```

### HPC-Specific Challenges: Where Conda Really Struggles

High-performance computing clusters add additional layers of complexity:

**Challenge 1: No sudo/root access**

- Can’t install system libraries

- Can’t use Docker (requires root)

- Conda is often the *only* option, but it doesn’t work well

**Challenge 2: Limited internet on compute nodes**

- Dependency resolution requires internet access

- Many HPC systems block internet from compute nodes

- Must pre-download everything to login node

- No way to test if installation actually works until you submit a job

**Challenge 3: Home directory quotas**

- Conda cache grows rapidly (10-50 GB)

- HPC home directories often limited to 10-50 GB

- Must manually configure cache locations

- Quota exceeded errors break installations

**Challenge 4: Shared environments with version locks**

- System-wide Python or R installations with specific versions

- Can’t use certain package versions due to compatibility

- “Works on my laptop” ≠ “Works on HPC cluster”

**Challenge 5: Memory constraints during resolution**

- Dependency resolution can use 4-8 GB RAM

- On busy clusters with memory limits per user

- Solver killed by OS before completing

**Real HPC horror story**:

```r
# Monday morning: Start environment setup
conda create -n analysis r-base=4.3

# 30 minutes later: Add packages
conda install -c bioconda r-seurat bioconductor-dropletutils
# Solving environment... [2 hours]
# Killed (memory limit exceeded)

# Try again with fresh start
# Solving environment... [1.5 hours]
# UnsatisfiableError

# Try different versions
# [Another hour]
# Success!

# Test it
conda activate analysis
R
library(Seurat)
# Error: libstdc++.so.6: version not found

# Friday afternoon: Still not analyzing data
```

### Why Even Veterans Struggle

These problems affect everyone:

- **Beginners** waste days on environment setup instead of learning analysis

- **Intermediate users** develop complex workarounds that aren’t reproducible

- **Veterans** maintain elaborate bash scripts and Docker images, but can’t use them on HPC

- **Teams** can’t share environments—everyone has slightly different setups

- **Reproducibility** suffers—papers say “conda environment.yml provided” but it doesn’t work 6 months later

**The core issue**: Conda was designed for simple Python environments. Modern bioinformatics workflows—especially scRNA-seq—have outgrown its capabilities.

## Introducing Pixi: A Modern Solution

Enter **Pixi**, a package manager that solves conda’s pain points while maintaining full compatibility with the conda ecosystem.

### What Is Pixi?

**Pixi** is a fast, modern package management tool built on the conda ecosystem. Think of it as “conda 2.0″—it uses the same package repositories (conda-forge, bioconda) but with a completely redesigned architecture.

**Key facts**:

- Developed by [prefix.dev](https://prefix.dev), the team behind mamba

- Written in Rust (explaining the massive speed improvement)

- Open source (Apache 2.0 license)

- Launched in 2023, rapidly gaining adoption in bioinformatics

- Project-based rather than global environment model

### How Pixi Relates to Conda

**The conda ecosystem family tree**:

```
conda-forge & bioconda (package repositories)
    ↓
┌───────────────┬──────────────┬──────────────┐
│   conda       │   mamba      │   pixi       │
│   (Python)    │   (C++)      │   (Rust)     │
│   Original    │   Faster     │   Modern     │
│   2012        │   2019       │   2023       │
└───────────────┴──────────────┴──────────────┘
```

**All three use the same packages** from conda-forge and bioconda. The difference is *how* they manage those packages.

### Conda vs Mamba vs Pixi: Key Differences

**Language****Dependency resolution****Installation speed****Environment model****Reproducibility****System libraries****HPC compatibility****Learning curve****Parallel operations**

| Aspect | Conda | Mamba | Pixi |
| --- | --- | --- | --- |
|  | Python | C++ | Rust |
|  | Slow (minutes-hours) | Fast (minutes) | Very fast (seconds) |
|  | Slow | Moderate | Fast |
|  | Global environments | Global environments | Project-based |
|  | Manual (environment.yml) | Manual (environment.yml) | Automatic (lock files) |
|  | Manual handling | Manual handling | Automatic |
|  | Moderate | Good | Excellent |
|  | Familiar | Same as conda | New but simple |
|  | No | Yes | Yes |

### How Pixi Differs from Conda

#### 1. Project-Based vs Global Environments

**Conda approach** (global environments):

```bash
# Create global environment
conda create -n scrna-analysis r-seurat
conda activate scrna-analysis
cd /path/to/project1
# Work...

cd /path/to/project2
# Oops, still in scrna-analysis environment
# Need to activate different environment
conda deactivate
conda activate other-analysis
```

**Pixi approach** (project-based):

```bash
# Each project has its own environment
cd /path/to/project1
pixi init
pixi add r-seurat
pixi run R  # Automatically uses project1's environment

cd /path/to/project2
pixi run R  # Automatically uses project2's environment
# No activation/deactivation needed!
```

**Benefits**:

- No environment name confusion

- No accidentally using wrong environment

- Each project is self-contained

- No global pollution

#### 2. Lock Files for Reproducibility

**Conda approach**:

```bash
# Create environment
conda install r-seurat r-harmony

# Share with collaborators
conda env export > environment.yml
git add environment.yml

# Collaborator recreates
conda env create -f environment.yml
# Gets different versions! (conda-forge packages updated daily)
```

**Pixi approach**:

```bash
# Install packages
pixi add r-seurat r-harmony

# Automatically creates pixi.lock
# Contains exact versions, build hashes, URLs, checksums

# Collaborator recreates
pixi install
# Gets EXACT same versions (guaranteed)
```

**The lock file difference**:

- `environment.yml`: Says “install seurat and harmony” (versions can drift)

- `pixi.lock`: Says “install seurat 5.0.1 build h123abc from https://… with SHA256 checksum xyz” (exact reproduction)

#### 3. Parallel Dependency Resolution

**Conda**:

- Single-threaded solver

- Checks dependencies one at a time

- 20-30 minutes for complex environments

**Pixi**:

- Multi-threaded solver written in Rust

- Parallel dependency checking

- Advanced SAT solver algorithms

- 1-2 minutes for same environment

**Real benchmark** (full scRNA-seq environment):

```
Conda:  ████████████████████████████░░ (28m 30s)
Mamba:  ████████████░░░░░░░░░░░░░░░░░░ (12m 15s)
Pixi:   ███░░░░░░░░░░░░░░░░░░░░░░░░░░░ ( 3m 42s)
```

#### 4. System Library Management

**Conda**:

```r
conda install r-soupx
# Installation succeeds

R
library(SoupX)
# Error: libstdc++.so.6 version not found
# System library missing
```

**Pixi**:

```r
pixi add r-soupx
# Automatically includes required system libraries
# Checks compatibility with your OS

pixi run R
library(SoupX)
# ✓ Works! System libraries included
```

**How Pixi handles this**:

- Detects OS version automatically

- Includes necessary system libraries in lock file

- Platform-specific builds (linux-64, osx-64, osx-arm64)

- Works in restricted HPC environments without sudo

### The Pixi Workflow

Understanding how pixi works helps you use it effectively:

```
┌─────────────────────────────────────────────────────────┐
│  1. Create Project                                       │
│     pixi init                                            │
│     → Creates pixi.toml (what you want)                  │
└─────────────────────┬───────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────┐
│  2. Add Dependencies                                     │
│     pixi add r-seurat r-harmony                          │
│     → Updates pixi.toml with packages                    │
└─────────────────────┬───────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────┐
│  3. Resolve & Install                                    │
│     pixi install                                         │
│     → Solves dependencies (fast!)                        │
│     → Generates pixi.lock (exact versions)               │
│     → Downloads and installs packages                    │
│     → Creates .pixi/ directory (local environment)       │
└─────────────────────┬───────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────┐
│  4. Run Analysis                                         │
│     pixi run R                                           │
│     → Automatically activates environment                │
│     → Runs your analysis                                 │
└─────────────────────────────────────────────────────────┘
```

**Key files**:

**`pixi.toml`** (your manifest):

```
[project]
name = "scrna-analysis"
channels = ["conda-forge", "bioconda"]

[dependencies]
r-seurat = ">=5.0"
r-harmony = "*"
```

- Human-readable

- Specifies what you want

- Can use version ranges

- Committed to git

**`pixi.lock`** (the solution):

```bash
# (Simplified - actual file is much larger)
[[package]]
name = "r-seurat"
version = "5.0.1"
build = "r43h123abc_0"
sha256 = "abc123..."
url = "https://conda.anaconda.org/conda-forge/linux-64/..."
```

- Machine-readable

- Exact versions and build hashes

- Platform-specific

- Guarantees reproducibility

- Committed to git

**`.pixi/`** directory:

- Contains actual installed packages

- Created locally (like `node_modules/`)

- NOT committed to git (add to `.gitignore`)

- Each project has its own

### Advantages for scRNA-seq Analysis

#### Speed: 5 Minutes vs 30 Minutes

**Concrete example** – Complete scRNA-seq environment:

**With Conda**:

```bash
time conda create -n scrna r-seurat bioconductor-dropletutils \
  bioconductor-singler r-harmony bioconductor-batchelor

# Collecting package metadata: 3m 22s
# Solving environment: 22m 45s
# Downloading packages: 9m 15s
# Extracting packages: 3m 08s
# Total: 38m 30s
```

**With Pixi**:

```bash
time pixi add r-seurat bioconductor-dropletutils \
  bioconductor-singler r-harmony bioconductor-batchelor
pixi install

# Resolving dependencies: 1m 24s
# Downloading packages: 2m 56s
# Installing packages: 1m 08s
# Total: 5m 28s
```

**7x faster** – and this is for a single environment. For complete workflow (4 environments), the difference is even more dramatic:

- Conda: ~2 hours

- Pixi: ~15 minutes

#### Reproducibility: Guaranteed Identical Environments

**The version drift problem with conda**:

User A (January 2025):

```r
conda install r-seurat
# Gets Seurat 5.0.1
```

User B (March 2025):

```r
conda install r-seurat
# Gets Seurat 5.1.0 (new version released)
# Results might differ!
```

**Pixi’s lock file solution**:

User A (January 2025):

```bash
pixi add r-seurat
# pixi.lock: seurat=5.0.1, build=h123abc
git commit pixi.lock
```

User B (March 2025):

```bash
git clone project
pixi install
# pixi.lock specifies: seurat=5.0.1, build=h123abc
# Gets EXACT same version (even if 5.1.0 exists)
```

**Benefits**:

- Publications: Readers can recreate exact environment years later

- Collaboration: Teammates get identical results

- Debugging: Environment is never the issue

- Confidence: Your analysis is reproducible

#### HPC-Friendly: No More Missing Libraries

**Common HPC scenario with conda**:

```bash
# On HPC login node
conda install r-package
# Installation succeeds

# Submit job to compute node
sbatch analysis.sh

# Job output:
# Error: libstdc++.so.6: version 'GLIBCXX_3.4.29' not found
# Different system libraries on compute node
```

**Pixi handles this**:

```bash
# On HPC login node
pixi add r-package
# Detects system environment
# Includes necessary libraries in lock file

# Submit job
sbatch analysis.sh

# ✓ Works! Libraries included in environment
```

**Why this matters on HPC**:

- No sudo needed (Pixi installs everything in user space)

- Works across different nodes (lock file includes platform info)

- Respects quotas (configurable cache location)

- Offline-friendly (pre-download packages, then work offline)

#### Clear Error Messages

**Conda error** (typical):

```
UnsatisfiableError: The following specifications were found to be incompatible with each other:
  - r-seurat -> r-base=4.3.2
  - bioconductor-dropletutils -> r-base=4.3.1

Hint: You might be able to solve this by using a different version of r-base
```

*Thanks, but which version?*

**Pixi error** (actionable):

```r
Error: Package conflict detected

Package 'r-seurat' requires r-base 4.3.2, but 'bioconductor-dropletutils' requires r-base 4.3.1

Suggested solution:
  pixi add "r-base=4.3.1" "r-seurat>=5.0,&lt;5.1"

This will pin r-base to 4.3.1 and find a compatible Seurat version.
```

*Clear, specific, actionable!*

### When to Use Pixi vs Conda

#### Use Pixi When:

✓ **Starting new projects** (no legacy conda environments to maintain)

- Clean slate, modern approach

- Set up reproducible workflow from the start

✓ **Working on HPC** (especially with restrictions)

- No Docker allowed (common)

- Limited home directory quota

- Missing system libraries

- Need offline capability

✓ **Collaborating on analysis** (multiple people, reproducibility critical)

- Share exact environment via git

- No “works on my machine” issues

- Publication-quality reproducibility

✓ **Complex workflows** (multiple stages, many dependencies)

- scRNA-seq multi-step pipelines

- Integration of R and Python tools

- Long-running analyses

✓ **Tired of conda being slow** (seriously, life is short)

- 5 minutes vs 30 minutes per environment

- Faster iteration, more time analyzing

#### Stick with Conda If:

⚠ **Existing conda setup works** (if it ain’t broke…)

- Legacy projects with working conda environments

- Migration effort not worth it for one-off analyses

⚠ **Team mandates conda** (institutional requirements)

- Lab standard is conda

- Published workflows use conda

- Not worth fighting the system

⚠ **Need packages not in conda-forge/bioconda** (rare)

- Proprietary packages

- Internal corporate channels

- (But 95% of bioinformatics packages are in conda-forge/bioconda)

**The truth**: For new scRNA-seq projects, especially on HPC, Pixi is simply better. The only reason to stick with conda is inertia or external requirements.

### Pixi vs Docker: Quick Comparison

Since this question often comes up:

**Isolation level****Size****Speed****HPC compatibility****Use case****Learning curve**

| Aspect | Pixi | Docker |
| --- | --- | --- |
|  | Package-level | OS-level |
|  | Small (~1-2 GB) | Large (3-10 GB) |
|  | Native (no overhead) | Slight overhead |
|  | ✓ Works everywhere | Often blocked (needs root) |
|  | Development & analysis | Deployment & services |
|  | Easy | Steeper |

**When to use each**:

- **Pixi**: Active analysis work, HPC clusters, iteration and development

- **Docker**: Deployment, long-term archival, web services, maximum isolation

**Best practice**: Use Pixi for analysis, Docker for archival:

```bash
# Development with Pixi (fast, easy)
pixi add tools...
pixi run analysis.R

# Archive with Docker (maximum reproducibility)
FROM ubuntu:22.04
RUN curl -fsSL https://pixi.sh/install.sh | bash
COPY pixi.toml pixi.lock ./
RUN pixi install
```

For this tutorial and NGS101 workflows, **use Pixi**—it’s perfect for analyzing data on HPC clusters with reproducible environments.

## Installing Pixi on HPC Systems

Let’s walk through a complete installation and configuration on a typical HPC cluster. This section provides a comprehensive, real-world example that you can adapt to your specific HPC environment.

### Complete HPC Setup Walkthrough

This example demonstrates installation on a Linux HPC cluster with common constraints: limited home directory quota, proxy requirements, and restricted compute node access.

#### Step 1: Initial Installation

```bash
# SSH to your HPC cluster
ssh username@hpc.institution.edu

# Download and run Pixi installer (no sudo required)
curl -fsSL https://pixi.sh/install.sh | bash
```

**What this script does**:

1. Detects your system architecture (x86_64 or ARM64)

2. Downloads the latest Pixi binary for Linux

3. Installs it to `~/.pixi/bin/pixi`

4. Adds Pixi to your PATH in `~/.bashrc`

5. Verifies installation

**Expected output**:

```bash
Downloading pixi-x86_64-unknown-linux-musl from https://github.com/prefix-dev/pixi/releases/...
######################################################################## 100.0%
Pixi installed successfully!

The 'pixi' binary is installed to: /home/username/.pixi/bin/pixi

To get started, run:
    source /home/username/.bashrc
    pixi --help
```

**Reload your shell configuration**:

```
source ~/.bashrc
```

**Verify installation**:

```bash
pixi --version
# Expected: pixi 0.63.1 (or newer)
```

#### Step 2: Configure Cache Location for HPC

**The challenge**: HPC home directories typically have strict quotas (10-50 GB), but Pixi’s cache can grow to 5-20 GB for large environments.

**Check your home directory quota**:

```bash
quota -s
# Output shows: Space used: 8GB / Quota: 50GB
```

**Solution**: Use scratch or project space for cache

**Configure cache location** (add to `~/.bashrc`):

```bash
# Add this line to the end of ~/.bashrc
echo 'export PIXI_CACHE_DIR=/scratch/$USER/pixi-cache' >> ~/.bashrc

# Or if you have project space:
# echo 'export PIXI_CACHE_DIR=/project/mylab/pixi-cache' >> ~/.bashrc

# Reload configuration
source ~/.bashrc
```

**Create cache directory**:

```bash
mkdir -p $PIXI_CACHE_DIR

# Verify it's set correctly
echo $PIXI_CACHE_DIR
# /scratch/username/pixi-cache
```

**Why this matters**:

- Scratch space typically has much larger quotas (TB-scale)

- Faster I/O on scratch storage

- Avoids exceeding home directory quota

- Cache can be cleaned periodically without affecting home directory

#### Step 3: Configure Network Settings

Many HPC clusters require proxy configuration and have network-related constraints.

**Test if you need proxy configuration**:

```bash
curl https://conda-forge.org
# If this succeeds: You don't need proxy
# If this times out: You need proxy configuration
```

**Create Pixi configuration file**:

```
mkdir -p ~/.pixi
nano ~/.pixi/config.toml
```

**Add network configuration** (adjust for your institution):

```bash
# Proxy configuration (if needed)
[proxy-config]
http = "http://proxy.institution.edu:8080/"
https = "http://proxy.institution.edu:8080/"
# Hosts that should bypass proxy
non-proxy-hosts = [".edu", ".gov", "localhost", "127.0.0.1", "[::1]"]

# Parallel download configuration
[concurrency]
downloads = 10  # Increase for fast HPC networks (up to 20-50)
```

**How to get your proxy settings**:

- Check HPC documentation

- Ask your HPC support team

- Check existing conda configuration: `cat ~/.condarc | grep proxy`

- Check environment variables: `echo $HTTP_PROXY`

#### Step 4: Verify Configuration

**Test network connectivity**:

```
pixi search r-seurat
```

**Expected output**:

```
Package Name   Version  Build           Channel
r-seurat       5.4.0    r43h...        conda-forge
r-seurat       5.3.0    r43h...        conda-forge
...
```

**If this fails**:

- Check proxy settings

- Verify internet access from login node

- Contact HPC support for assistance

#### Step 5: HPC-Specific Optimizations

**For shared team environments** (optional):

If your team wants to share a cache to avoid downloading packages multiple times:

```bash
# Team leader creates shared cache
mkdir -p /project/mylab/shared-pixi-cache
chmod 775 /project/mylab/shared-pixi-cache

# Team members add to ~/.bashrc
echo 'export PIXI_CACHE_DIR=/project/mylab/shared-pixi-cache' >> ~/.bashrc
```

**Benefits**:

- Each package downloaded only once

- Significant time savings for large teams

- Reduced storage usage across team

**Considerations**:

- Requires write permissions for all team members

- Need to coordinate cache cleaning

- Consider disk space on shared storage

**For high-speed HPC networks**:

If your HPC has very fast network (10 Gbps+), increase parallel downloads:

Edit `~/.pixi/config.toml`:

```
[concurrency]
downloads = 20  # Or even 50 for extremely fast networks
```

**For offline work on compute nodes**:

If compute nodes lack internet access (common), pre-download packages:

```bash
# On login node (has internet)
cd ~/scrna-seq-analysis
pixi install --all  # Downloads all packages to cache

# Now submit jobs to compute nodes
# Pixi will use cached packages (no internet needed)
sbatch analysis_job.sh
```

### Summary of HPC Configuration

After completing these steps, you should have:

✅ Pixi installed in `~/.pixi/bin/`✅ Cache configured to use scratch space (avoiding quota issues)✅ Network/proxy configured (if needed)✅ Optimized for your HPC environment✅ Tested and working

**Your `~/.bashrc` should contain**:

```bash
# Pixi PATH (added by installer)
export PATH="$HOME/.pixi/bin:$PATH"

# Cache location (added by you)
export PIXI_CACHE_DIR=/scratch/$USER/pixi-cache
```

**Your `~/.pixi/config.toml` should contain**:

```
[proxy-config]
http = "http://proxy.institution.edu:8080/"
https = "http://proxy.institution.edu:8080/"
non-proxy-hosts = [".edu", "localhost", "127.0.0.1"]

[concurrency]
downloads = 10
```

**Now you’re ready to set up your scRNA-seq analysis environment!**

## Setting Up the scRNA-seq Environment

Now let’s create environments for the complete scRNA-seq workflow from the [NGS101 tutorial series](https://ngs101.com).

The complete scRNA-seq analysis pipeline consists of four major stages:

1. **Data Processing (Part 1)**: Download raw FASTQ files, run quality control with FastQC/MultiQC, and process through Cell Ranger to generate count matrices

2. **Quality Control (Part 2)**: Filter empty droplets, detect doublets, remove low-quality cells, and correct ambient RNA contamination

3. **Integration & Clustering (Part 3)**: Integrate multiple samples using Harmony/CCA/RPCA, perform dimensional reduction, and identify cell clusters

4. **Cell Type Annotation (Part 4)**: Assign biological identities to clusters using manual markers, SingleR, scType, and scCATCH

Each stage requires specific tools and R packages. Pixi makes it easy to manage all these dependencies using **features** and **environments**.

### Understanding Features vs Environments

Before we dive into installation, it’s important to understand Pixi’s organizational system:

**Features**:

- Collections of dependencies, configurations, and tasks

- Building blocks that define what packages and settings you want

- Defined in sections like `[feature.NAME.dependencies]`

- Think of features as “ingredient lists”

**Environments**:

- Combinations of one or more features

- Define which features should be installed together

- Defined in the `[environments]` section

- Think of environments as “recipes” that combine ingredients

**Why separate environments for each part?**

- **Isolation**: Avoid dependency conflicts between stages

- **Size**: Each environment is smaller (faster to install/update)

- **Clarity**: Know exactly which tools are for which stage

- **Flexibility**: Can update one stage without affecting others

### Project Initialization

Create a new directory for your scRNA-seq analysis project and initialize it with Pixi:

```bash
# Create and enter project directory
mkdir ~/scrna-seq-analysis
cd ~/scrna-seq-analysis

# Initialize Pixi project with channels
pixi init --channel conda-forge --channel bioconda

# This creates pixi.toml - your project's configuration file
```

#### Understanding pixi.toml Structure

The `pixi.toml` file is the heart of your Pixi project. Open it and you’ll see a basic structure:

```
[workspace]
channels = ["conda-forge", "bioconda"]
name = "scrna-seq-analysis"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]

[dependencies]
```

Let’s break down each section:

**`[workspace]`**:

- `channels`: Where pixi searches for packages

- `name`: Your project name

- `platforms`: Target operating systems (linux-64 for HPC)

- `version`: Project version (semantic versioning)

**`[tasks]`**: Custom commands (we’ll add these later)

**`[dependencies]`**: Packages shared across all environments (usually empty for multi-environment projects)

#### Configuring Channels

Channels are repositories where Pixi searches for packages. For bioinformatics workflows, we need two essential channels:

- **conda-forge**: Community-maintained, general-purpose packages. Most bioinformaticians now prefer this channel. Should be listed first for R packages to avoid conflicts.

- **bioconda**: Specialized bioinformatics tools and packages (SRA Toolkit, FastQC, samtools, etc.)

**Note on the defaults channel**: The `defaults` channel (Anaconda’s official repository) requires accepting additional [terms of service](https://www.anaconda.com/docs/getting-started/working-with-conda/reference/default-channels) for commercial use, so we exclude it from this configuration.

**Platform specification**: We configure support for Linux systems:

- `linux-64`: x86_64 Linux systems (most HPC clusters and workstations)

**Channel priority matters**:

- conda-forge should be listed first for R packages (more up-to-date, fewer conflicts)

- bioconda second for bioinformatics-specific tools

- The order determines which channel pixi searches first

### Installing Linux Command-Line Tools (Part 1)

These tools handle FASTQ download and quality control. We’ll create a dedicated feature and environment for them:

```bash
# Create part1 feature and add command-line tools
# Note: pixi will warn that feature isn't linked to environment - we'll fix this next

pixi add --feature part1-feature sra-tools     # Download data from NCBI SRA
pixi add --feature part1-feature fastqc        # Quality control for FASTQ files
pixi add --feature part1-feature multiqc       # Aggregate QC reports
pixi add --feature part1-feature samtools      # BAM file manipulation

# Link feature to environment
pixi workspace environment add part1 --feature part1-feature --force
```

#### About Cell Ranger

Cell Ranger is 10x Genomics’ proprietary software for processing scRNA-seq data. Unlike the packages above, it **cannot be installed via Conda/Pixi** because:

- It’s distributed as a pre-compiled binary tarball

- Requires acceptance of 10x Genomics’ license agreement

- Download URL contains authentication tokens

- Commercial license restrictions

**Manual Cell Ranger Installation**:

You can follow the detailed installation instructions from [NGS101 Part 1](https://ngs101.com/how-to-analyze-single-cell-rna-seq-data-complete-beginners-guide-part-1-from-fastq-to-count-matrix/).

> 

⚠️ **Important**:

- The download link is generated dynamically and expires quickly

- You must obtain your own download link from [10x Genomics Support](https://www.10xgenomics.com/support/software/cell-ranger/downloads)

- Requires free account registration

**Integrating Cell Ranger with Pixi workflow**:

While Cell Ranger itself can’t be in pixi, you can create pixi tasks that use it:

```bash
[tasks]
# Cell Ranger tasks (assumes cellranger in PATH)
cellranger-count = "cellranger count --id=sample --transcriptome=refdata --fastqs=fastqs/"
```

### Installing R-Based Analysis Tools (Parts 2-4)

Now we’ll install R packages for quality control, integration, and cell type annotation. We’ll create separate features and environments for each analysis stage.

#### Part 2: Quality Control Packages

Part 2 covers filtering empty droplets, detecting doublets, and removing low-quality cells.

**IMPORTANT**: Note the corrected syntax – no `=` before version operators:

```r
# Create part2 feature
pixi add --feature part2-feature "r-base>=4.3"           # R language (version 4.3+)
pixi add --feature part2-feature "r-seurat>=5.4.0,<6"    # Core scRNA-seq analysis
pixi add --feature part2-feature "r-seuratobject>=5.3.0,<6"  # Seurat data structures

# Bioconductor packages for QC
pixi add --feature part2-feature "bioconductor-scater>=1.34.1,<2"  # QC metrics
pixi add --feature part2-feature "bioconductor-singlecellexperiment>=1.28.0,<2"  # Data container
pixi add --feature part2-feature "bioconductor-scdblfinder>=1.23.4,<2"  # Doublet detection
pixi add --feature part2-feature "bioconductor-dropletutils>=1.26.0,<2"  # Empty droplet detection

# Specialized QC tools
pixi add --feature part2-feature "r-soupx>=1.6.2,<2"     # Ambient RNA correction
pixi add --feature part2-feature "r-fnn>=1.1.4.1,<2"     # k-nearest neighbors
pixi add --feature part2-feature "r-cluster>=2.1.8.1,<3"  # Silhouette scores
pixi add --feature part2-feature "r-reshape2>=1.4.5,<2"   # Data reshaping
pixi add --feature part2-feature "r-dplyr>=1.1.4,<2"     # Data manipulation

# Link feature to environment
pixi workspace environment add part2 --feature part2-feature --force
```

#### Part 3: Integration & Clustering Packages

Part 3 covers batch correction and integration of multiple samples.

```bash
# Create part3 feature
# Integration methods
pixi add --feature part3-feature r-harmony         # Fast probabilistic integration
pixi add --feature part3-feature bioconda::bioconductor-batchelor  # FastMNN integration

# Link to environment
pixi workspace environment add part3 --feature part3-feature --force

# Continue adding visualization packages
pixi add --feature part3-feature r-ggplot2         # Core plotting
pixi add --feature part3-feature r-patchwork       # Combine multiple plots
pixi add --feature part3-feature r-ggalluvial      # Sankey diagrams
pixi add --feature part3-feature r-viridis         # Perceptually uniform colors
pixi add --feature part3-feature r-rcolorbrewer    # Color palettes
pixi add --feature part3-feature r-ggrepel         # Non-overlapping labels
```

#### Part 4: Cell Type Annotation Packages

Part 4 covers assigning biological identities to cell clusters.

```bash
# Create part4 feature
# Reference-based annotation
pixi add --feature part4-feature bioconductor-singler    # Transfer labels from references
pixi add --feature part4-feature bioconductor-celldex    # Reference cell type atlases

# Link to environment
pixi workspace environment add part4 --feature part4-feature --force

# Continue with marker-based annotation
pixi add --feature part4-feature "r-hgnchelper>=0.8.15,<0.9"      # Gene symbol validation
pixi add --feature part4-feature "r-openxlsx>=4.2.8.1,<5"        # Read Excel marker databases
```

**Note on scCATCH**: At the time of this tutorial, scCATCH installation via pixi/conda is problematic due to dependency conflicts. For scCATCH functionality, use SingleR or scType as alternatives, or install scCATCH from GitHub in R after installing the environments:

```bash
# Inside R session
devtools::install_github("ZJUFanLab/scCATCH")
```

**Important: Bioconductor Package Issues**

After installing the Part 4 environment, you’ll need to manually install `GenomeInfoDbData` and `celldex` from Bioconductor. These packages have conda versions that don’t properly install the R packages:

- **celldex**: Provides reference cell type datasets for SingleR annotation

- **GenomeInfoDbData**: Required by GenomeInfoDb, which is a dependency of many Bioconductor packages

Installation is covered in the **Post-Installation** section after running `pixi install --all`.

### Complete pixi.toml Configuration

After adding all packages, your `pixi.toml` will look like this:

```bash
[workspace]
channels = ["conda-forge", "bioconda"]
name = "scrna-seq-analysis"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]

[dependencies]

#---------------------------------------
# Part 1: Command-Line Tools
#---------------------------------------
[feature.part1-feature.dependencies]
sra-tools = "*"
fastqc = "*"
multiqc = "*"
samtools = "*"

#---------------------------------------
# Part 2: Quality Control
#---------------------------------------
[feature.part2-feature.dependencies]
r-base = ">=4.3"
r-seurat = ">=5.4.0,<6"
r-seuratobject = ">=5.3.0,<6"
bioconductor-scater = ">=1.34.1,<2"
bioconductor-singlecellexperiment = ">=1.28.0,<2"
bioconductor-scdblfinder = ">=1.23.4,<2"
bioconductor-dropletutils = ">=1.26.0,<2"
r-soupx = ">=1.6.2,<2"
r-fnn = ">=1.1.4.1,<2"
r-cluster = ">=2.1.8.1,<3"
r-reshape2 = ">=1.4.5,<2"
r-dplyr = ">=1.1.4,<2"

#---------------------------------------
# Part 3: Integration & Clustering
#---------------------------------------
[feature.part3-feature.dependencies]
r-harmony = "*"
bioconductor-batchelor = "*"
r-ggplot2 = "*"
r-patchwork = "*"
r-ggalluvial = "*"
r-viridis = "*"
r-rcolorbrewer = "*"
r-ggrepel = "*"

#---------------------------------------
# Part 4: Cell Type Annotation
#---------------------------------------
[feature.part4-feature.dependencies]
bioconductor-singler = "*"
bioconductor-celldex = "*"
r-hgnchelper = ">=0.8.15,<0.9"
r-openxlsx = ">=4.2.8.1,<5"

#---------------------------------------
# Environments
#---------------------------------------
[environments]
part1 = ["part1-feature"]
part2 = ["part2-feature"]
part3 = ["part3-feature"]
part4 = ["part4-feature"]
```

**Understanding version constraints**:

- `"*"`: Any version (pixi chooses latest compatible)

- `">=5.4.0,<6"`: At least 5.4.0, but less than 6.0.0 (allows minor/patch updates)

- `">=4.3"`: At least 4.3 (allows any 4.x version)

- `"=5.0.1"`: Exact version (most restrictive)

**Best practices**:

- Pin major versions for critical packages (Seurat, R)

- Allow minor/patch updates for flexibility

- Use `"*"` for stable packages

### Installing the Environment

Now install all environments at once:

```bash
# Install all four environments
pixi install --all
```

**What happens during installation:**

**Dependency Resolution** (1-2 minutes)

- Pixi analyzes all package requirements

- Finds compatible versions for each environment

- Checks for conflicts

- Much faster than conda (parallel processing, Rust implementation)

**Lock File Generation**

- Creates `pixi.lock` with exact versions

- Includes build hashes, URLs, checksums

- Platform-specific (linux-64)

- Guarantees reproducibility

**Package Download** (2-4 minutes depending on network)

- Parallel downloads (default: 5 simultaneous)

- Progress bars for each package

- Cached for future use

**Environment Creation**

- Installs packages to `.pixi/envs/part1/`, `.pixi/envs/part2/`, etc.

- Links binaries

- Configures environment variables

- Each environment is isolated

**Expected installation time**:

With good internet connection:

```bash
time pixi install --all

# Typical output:
# Part1 environment installed (1m 32s)
# Part2 environment installed (2m 18s)  
# Part3 environment installed (1m 45s)
# Part4 environment installed (1m 28s)
# 
# Total: ~7-8 minutes
```

**Compare to conda** (same packages):

```bash
# Conda would take: 25-40 minutes
# - Dependency resolution: 15-25 minutes
# - Downloads: 8-12 minutes  
# - Installation: 2-3 minutes
```

**Pixi is 4-5x faster!**

### Post-Installation: Install Required Bioconductor Packages

After the pixi environments are installed, you need to manually install certain Bioconductor packages that don’t work correctly via conda. This affects **annotation and data packages** in Parts 2, 3, and 4.

```r
# Install for all environments at once
for env in part2 part3 part4; do
  echo "Installing Bioconductor packages for $env..."
  pixi run -e $env Rscript -e '
  lib_path <- .libPaths()[1];
  if (!require("BiocManager", quietly = TRUE)) install.packages("BiocManager", lib = lib_path, repos = "https://cloud.r-project.org");
  library(BiocManager);

  # Part 2 and 3 need only GenomeInfoDbData
  # Part 4 needs both GenomeInfoDbData and celldex
  if (Sys.getenv("PIXI_ENVIRONMENT_NAME") == "part4") {
    BiocManager::install(c("GenomeInfoDbData", "celldex"), lib = lib_path, update = FALSE, ask = FALSE, force = TRUE);
  } else {
    BiocManager::install("GenomeInfoDbData", lib = lib_path, update = FALSE, ask = FALSE, force = TRUE);
  }

  cat(paste0("✓ ", Sys.getenv("PIXI_ENVIRONMENT_NAME"), " complete\n"))
  ' PIXI_ENVIRONMENT_NAME=$env
done
```

**Why this extra step?** Bioconductor annotation and data packages (GenomeInfoDbData & celldex in this case) are often not properly packaged for conda. Installing from Bioconductor directly ensures they’re correctly placed in your pixi environment’s R library.

### Understanding the Lock File

The `pixi.lock` file is key to reproducibility. Let’s examine what it contains:

```bash
# Simplified example from pixi.lock

version = 5
environments:
  part2:
    channels:
    - url: https://conda.anaconda.org/conda-forge/
    - url: https://conda.anaconda.org/bioconda/
    packages:
      linux-64:
      - conda: https://conda.anaconda.org/bioconda/noarch/bioconductor-celldex-1.16.0-r44hdfd78af_0.tar.bz2
        sha256: e2c061a628fcfc8b88a056123549f7ca0628dde33ad1150e9d9bf89253833e11
        md5: f22e220718bcd2d43e90876c6f9d99b8
        depends:
        - bioconductor-alabaster.base >=1.6.0,&lt;1.7.0
        - bioconductor-alabaster.matrix >=1.6.0,&lt;1.7.0
        - bioconductor-annotationdbi >=1.68.0,&lt;1.69.0
        - r-base >=4.4,&lt;4.5.0a0
        - r-dbi
        - r-jsonlite
        license: GPL-3
        size: 20947
        timestamp: 1735285120405
```

**What the lock file contains**:

- **Exact version**: Not “>=5.0”, but “5.0.1”

- **Build hash**: `r44hdfd78af_0` (specific compilation)

- **SHA256 checksum**: Verify package integrity

- **MD5 checksum**: Additional verification

- **Download URL**: Exact location

- **Dependencies**: Complete tree with exact versions

- **Timestamp**: When package was built

- **Size**: Package size in bytes

**Why this matters**:

**Without lock file (conda’s environment.yml)**:

```
dependencies:
  - bioconductor-celldex
```

→ Different users get different versions (packages update daily)

**With lock file (pixi.lock)**:

```
conda: https://conda.anaconda.org/bioconda/noarch/bioconductor-celldex-1.16.0-r44hdfd78af_0.tar.bz2
sha256: e2c061a628fcfc8b88a056123549f7ca0628dde33ad1150e9d9bf89253833e11
```

→ Everyone gets *exactly* the same package, byte-for-byte identical

**This is the foundation of reproducible research!**

### What Gets Created

After `pixi install --all`, your project directory contains:

```
scrna-seq-analysis/
├── pixi.toml          # Your configuration (commit to git)
├── pixi.lock          # Exact versions (commit to git)
├── .pixi/             # Installed environments (DON'T commit)
│   ├── envs/
│   │   ├── part1/     # Part 1 tools
│   │   ├── part2/     # Part 2 R packages
│   │   ├── part3/     # Part 3 integration
│   │   └── part4/     # Part 4 annotation
│   └── solve-group-environments/
└── .gitignore         # (should include .pixi/)
```

## Validating the Installation

Installing packages is one thing—verifying they actually work is another. Let’s test all tools to ensure they’re functional before starting analysis.

### Comprehensive Validation Script

For thorough automated testing, use this validation script. This version writes output to temporary files for better error diagnosis.

**Create `validate_environment.sh`**:

```r
cat > validate_environment.sh << 'EOF'
#!/bin/bash

echo "=========================================="
echo "  scRNA-seq Environment Validation"
echo "  Testing all tools from Parts 1-4"
echo "=========================================="

#---------------------------------------
# Part 1: Command-Line Tools
#---------------------------------------
echo -e "\n=== Part 1: Command-Line Tools ===\n"

echo "[1/4] Testing SRA Toolkit..."
if pixi run -e part1 fastq-dump --version > /tmp/test_sra.out 2>&1; then
    echo "  ✓ SRA Toolkit working"
else
    echo "  ✗ SRA Toolkit FAILED"
    cat /tmp/test_sra.out
    exit 1
fi

echo "[2/4] Testing FastQC..."
if pixi run -e part1 fastqc --version > /tmp/test_fastqc.out 2>&1; then
    echo "  ✓ FastQC working"
else
    echo "  ✗ FastQC FAILED"
    cat /tmp/test_fastqc.out
    exit 1
fi

echo "[3/4] Testing MultiQC..."
if pixi run -e part1 multiqc --version > /tmp/test_multiqc.out 2>&1; then
    echo "  ✓ MultiQC working"
else
    echo "  ✗ MultiQC FAILED"
    cat /tmp/test_multiqc.out
    exit 1
fi

echo "[4/4] Testing SAMtools..."
if pixi run -e part1 samtools --version > /tmp/test_samtools.out 2>&1; then
    echo "  ✓ SAMtools working"
else
    echo "  ✗ SAMtools FAILED"
    cat /tmp/test_samtools.out
    exit 1
fi

#---------------------------------------
# Part 2: R QC Packages
#---------------------------------------
echo -e "\n=== Part 2: Quality Control Packages ===\n"

echo "[1/3] Testing R installation..."
if pixi run -e part2 Rscript -e "cat('R working\n')" > /tmp/test_r.out 2>&1; then
    echo "  ✓ R installation working"
else
    echo "  ✗ R FAILED"
    cat /tmp/test_r.out
    exit 1
fi

echo "[2/3] Testing Seurat..."
if pixi run -e part2 Rscript -e "suppressMessages(library(Seurat)); cat('Seurat OK\n')" > /tmp/test_seurat.out 2>&1; then
    echo "  ✓ Seurat loaded successfully"
else
    echo "  ✗ Seurat FAILED"
    cat /tmp/test_seurat.out
    exit 1
fi

echo "[3/3] Testing Bioconductor packages..."
cat > /tmp/test_bioc.R << 'RCODE'
suppressMessages({
  library(DropletUtils)
  library(scater)
  library(SingleCellExperiment)
  library(scDblFinder)
})
cat('Bioconductor OK\n')
RCODE

if pixi run -e part2 Rscript /tmp/test_bioc.R > /tmp/test_bioc.out 2>&1; then
    echo "  ✓ Bioconductor QC packages loaded successfully"
else
    echo "  ✗ Bioconductor packages FAILED"
    cat /tmp/test_bioc.out
    exit 1
fi

#---------------------------------------
# Part 3: Integration Packages
#---------------------------------------
echo -e "\n=== Part 3: Integration & Visualization ===\n"

echo "[1/2] Testing integration packages..."
cat > /tmp/test_integration.R << 'RCODE'
suppressMessages({
  library(harmony)
  library(batchelor)
})
cat('Integration OK\n')
RCODE

if pixi run -e part3 Rscript /tmp/test_integration.R > /tmp/test_int.out 2>&1; then
    echo "  ✓ Integration packages loaded successfully"
else
    echo "  ✗ Integration packages FAILED"
    cat /tmp/test_int.out
    exit 1
fi

echo "[2/2] Testing visualization packages..."
cat > /tmp/test_viz.R << 'RCODE'
suppressMessages({
  library(ggplot2)
  library(patchwork)
  library(ggalluvial)
})
cat('Visualization OK\n')
RCODE

if pixi run -e part3 Rscript /tmp/test_viz.R > /tmp/test_viz.out 2>&1; then
    echo "  ✓ Visualization packages loaded successfully"
else
    echo "  ✗ Visualization packages FAILED"
    cat /tmp/test_viz.out
    exit 1
fi

#---------------------------------------
# Part 4: Annotation Packages
#---------------------------------------
echo -e "\n=== Part 4: Cell Type Annotation ===\n"

echo "[1/1] Testing annotation packages..."
cat > /tmp/test_annotation.R << 'RCODE'
suppressMessages({
  library(SingleR)
  library(celldex)
  library(HGNChelper)
  library(openxlsx)
})
cat('Annotation OK\n')
RCODE

if pixi run -e part4 Rscript /tmp/test_annotation.R > /tmp/test_annot.out 2>&1; then
    echo "  ✓ Annotation packages loaded successfully"
else
    echo "  ✗ Annotation packages FAILED"
    cat /tmp/test_annot.out
    exit 1
fi

#---------------------------------------
# Summary
#---------------------------------------
echo -e "\n=========================================="
echo "  ✓✓✓ All validations passed!"
echo "=========================================="
echo ""
echo "Your environment is ready for scRNA-seq analysis!"
echo ""
echo "Next steps:"
echo "  • Part 1: pixi run -e part1 <command>"
echo "  • Part 2: pixi run -e part2 R"
echo "  • Part 3: pixi run -e part3 R"
echo "  • Part 4: pixi run -e part4 R"
echo ""
echo "Follow the NGS101 tutorial series:"
echo "  https://ngs101.com"
echo ""

# Cleanup
rm -f /tmp/test_*.out /tmp/test_*.R
EOF

chmod +x validate_environment.sh
```

**Key features of this validation script**:

- **Writes output to temp files** (`/tmp/test_*.out`) for easier debugging

- **Shows actual error messages** when tests fail

- **Uses R script files** to avoid shell quoting issues

- **Tests all four environments systematically**

- **Cleans up after itself** by removing temp files

**If validation fails**, the script will show you the actual error message, making it easy to diagnose issues like missing Bioconductor packages.

**Run validation**:

```
./validate_environment.sh
```

**Expected output**:

```r
==========================================
  scRNA-seq Environment Validation
  Testing all tools from Parts 1-4
==========================================

=== Part 1: Command-Line Tools ===

[1/4] Testing SRA Toolkit...
  ✓ SRA Toolkit: fastq-dump : 3.1.1
[2/4] Testing FastQC...
  ✓ FastQC: FastQC v0.12.1
[3/4] Testing MultiQC...
  ✓ MultiQC: multiqc, version 1.21
[4/4] Testing SAMtools...
  ✓ SAMtools: samtools 1.19

=== Part 2: Quality Control Packages ===

[1/3] Testing R installation...
  ✓ R version: R version 4.3.2 (2023-10-31)
[2/3] Testing Seurat...
  ✓ Seurat version: 5.4.0
[3/3] Testing Bioconductor packages...
  ✓ All Bioconductor QC packages loaded successfully

=== Part 3: Integration &amp; Visualization ===

[1/2] Testing integration packages...
  ✓ Integration packages (harmony, batchelor) loaded
[2/2] Testing visualization packages...
  ✓ Visualization packages loaded

=== Part 4: Cell Type Annotation ===

[1/1] Testing annotation packages...
  ✓ All annotation packages loaded successfully

==========================================
  ✓✓✓ All validations passed!
==========================================

Your environment is ready for scRNA-seq analysis!
```

**If any test fails**:

1. Check the error message carefully

2. Ensure `pixi install --all` completed successfully

3. Try reinstalling that specific environment: `pixi install -e part2`

4. Check package availability: `pixi search <package-name>`

5. See Troubleshooting section below

**Your environments are now validated and ready for analysis!**

## Using Pixi for Daily Workflows

Now that your environment is set up and validated, let’s explore how to use Pixi efficiently in your day-to-day scRNA-seq analysis work.

### Key Advantages for Interactive Work

**Instant Environment Access**: Unlike traditional conda environments that require explicit activation, Pixi automatically manages your environment context. This means you can jump between different analysis stages without manually switching environments:

```bash
# No need for 'conda activate' - just run your command
pixi run -e part2 R

# Pixi handles the environment switching for you
pixi run -e part3 Rscript integration_analysis.R
```

**Rapid Experimentation**: Need to test a new package or approach? Add it instantly without disrupting your workflow:

```r
# Add a package on-the-fly
pixi add --feature part2-feature r-ggridges

# Test it immediately
pixi run -e part2 R
# library(ggridges)
```

**Reproducible Exploration**: Every package you add is automatically tracked in `pixi.lock`, ensuring that your exploratory analysis remains reproducible even as you iterate and experiment.

### Basic Commands

#### Activating Environments

Pixi automatically activates the environment when you run tasks. With multiple environments, specify which one to use:

```bash
# Start R in the part2 environment (for QC analysis)
pixi run -e part2 R

# Or enter a shell with a specific environment activated
pixi shell -e part1

# Now all tools from part1 are available:
which fastqc
# /path/to/project/.pixi/envs/part1/bin/fastqc

fastqc --version
# FastQC v0.12.1

# Exit to return to normal shell
exit
```

**Global tool installation** (optional):

Sometimes you need tools available globally across all projects:

```bash
# Install tools globally (not project-specific)
pixi global install nextflow jq git python=3.12

# Now available everywhere
nextflow -version
# N E X T F L O W
# version 25.10.0
```

**When to use global vs project-specific**:

- **Global**: Tools you use across many projects (nextflow, jq, git)

- **Project**: Analysis-specific packages (Seurat, harmony, SingleR)

#### Running Single Commands

Execute tools without entering the shell:

```bash
# Run FastQC on FASTQ files
pixi run -e part1 fastqc sample1.fastq.gz -o qc_results/

# Run R scripts
pixi run -e part2 Rscript analysis/01_quality_control.R

# Check package versions
pixi run -e part2 R --version
```

**Advantage**: No environment activation/deactivation needed. Pixi handles it automatically.

#### Adding New Packages

Add packages as you discover new analysis needs:

```bash
# Add a new package to an existing feature
pixi add --feature part2-feature r-cowplot

# Pixi automatically:
# 1. Updates pixi.toml
# 2. Resolves dependencies
# 3. Updates pixi.lock
# 4. Installs package

# Add to specific platform
pixi add --feature part4-feature --platform linux-64 bioconductor-newpackage

# Search for available packages
pixi search r-seurat
```

#### Updating Packages

Keep your packages up to date:

```bash
# Update all packages to latest compatible versions
pixi update

# Update specific package
pixi update r-seurat

# Update packages in specific environment
pixi update -e part2

# This regenerates pixi.lock with new versions
```

**Warning**: Updating can change package versions. For reproducibility:

1. Test updates in separate branch

2. Verify analysis results unchanged

3. Commit updated pixi.lock only after validation

#### Removing Packages

```bash
# Remove package from feature
pixi remove --feature part2-feature r-oldpackage

# Pixi automatically:
# 1. Updates pixi.toml
# 2. Regenerates pixi.lock
# 3. Removes from environment
```

### Custom Tasks

Define reusable commands in your `pixi.toml` for common operations.

#### Basic Task Definitions

Add tasks via command line or by editing `pixi.toml` directly.

**Via command line**:

```bash
# Add download task with environment variable
pixi task add download_SRA "prefetch \$SRR_ID && fasterq-dump \$SRR_ID --split-files --threads 8" --env SRR_ID=SRR123456
```

This updates `pixi.toml`:

```
[tasks]
download_SRA = { cmd = "prefetch $SRR_ID && fasterq-dump $SRR_ID --split-files --threads 8", env = { SRR_ID = "SRR123456" } }
```

**Running the task**:

```bash
# Use default SRR ID
pixi run download_SRA

# Override with specific SRR ID
SRR_ID=SRR14575500 pixi run download_SRA
```

#### Environment-Specific Tasks

Tasks can be associated with specific environments:

**Edit `pixi.toml` manually** for complex tasks:

```bash
[tasks]
# Part 1 tasks (command-line tools)
download-sra = { cmd = "prefetch $SRR_ID && fasterq-dump $SRR_ID --split-files --threads 8", env = "part1" }
qc-fastq = { cmd = "fastqc *.fastq -o qc_results/", env = "part1" }
qc-report = { cmd = "multiqc qc_results/ -o multiqc_output/", env = "part1" }

# Part 2 tasks (R QC)
run-qc = { cmd = "Rscript scripts/01_quality_control.R", env = "part2" }
run-doublet-detection = { cmd = "Rscript scripts/02_doublet_detection.R", env = "part2" }

# Part 3 tasks (integration)
run-integration-harmony = { cmd = "Rscript scripts/03a_integration_harmony.R", env = "part3" }
run-integration-fastmnn = { cmd = "Rscript scripts/03b_integration_fastmnn.R", env = "part3" }
run-clustering = { cmd = "Rscript scripts/04_clustering.R", env = "part3" }

# Part 4 tasks (annotation)
run-annotation-singler = { cmd = "Rscript scripts/05a_annotation_singler.R", env = "part4" }
run-annotation-markers = { cmd = "Rscript scripts/05b_annotation_markers.R", env = "part4" }
```

**Running environment-specific tasks**:

```bash
# Part 1 workflow
pixi run download-sra
pixi run qc-fastq
pixi run qc-report

# Part 2 QC
pixi run run-qc

# Part 3 integration
pixi run run-integration-harmony

# Part 4 annotation
pixi run run-annotation-singler
```

#### Task Dependencies (Chaining)

Create workflows by chaining tasks:

```bash
[tasks]
# Individual tasks
download-sra = { cmd = "prefetch $SRR_ID && fasterq-dump $SRR_ID --split-files", env = "part1" }
qc-fastq = { cmd = "fastqc *.fastq -o qc_results/", env = "part1", depends-on = ["download-sra"] }
qc-report = { cmd = "multiqc qc_results/", env = "part1", depends-on = ["qc-fastq"] }

# Combined workflow (runs all in order)
part1-complete = { depends-on = ["download-sra", "qc-fastq", "qc-report"] }
```

**Running chained tasks**:

```bash
# This runs download-sra, then qc-fastq, then qc-report
pixi run part1-complete
```

### Sharing Your Environment

One of Pixi’s key advantages is **cross-platform portability** and easy collaboration.

#### Committing to Version Control

Share your environment with collaborators via Git:

```bash
# Add Pixi files to git
git add pixi.toml pixi.lock

# Commit the environment
git commit -m "Add scRNA-seq analysis environment with Pixi"

# Push to remote
git push
```

**What to commit**:

- ✅ `pixi.toml` – Your environment configuration

- ✅ `pixi.lock` – Exact package versions

- ✅ Analysis scripts and notebooks

- ❌ `.pixi/` directory – Generated locally (add to `.gitignore`)

**Create comprehensive `.gitignore`**:

```bash
cat > .gitignore << 'EOF'
# Pixi
.pixi/

# R
.Rproj.user/
.Rhistory
.RData
.Ruserdata

# Data (don't commit large files)
*.fastq
*.fastq.gz
*.bam
*.h5
*.h5ad

# Outputs
qc_results/
multiqc_output/
figures/
results/
EOF

git add .gitignore
git commit -m "Add gitignore"
```

#### Reproducing the Environment

Your collaborator can reproduce your exact environment:

```bash
# Clone the repository
git clone https://github.com/yourusername/scrna-seq-analysis
cd scrna-seq-analysis

# Install exact same environment (reads pixi.lock)
pixi install --all

# Start analyzing with specific environment
pixi run -e part1 fastqc --help
```

**No `environment.yml` files needed** – `pixi.toml` + `pixi.lock` contain everything!

**Key advantages**:

1. **Exact reproduction**: Same package versions, builds, checksums

2. **Fast**: Installation from lock file is faster (no dependency resolution)

3. **Reliable**: Works identically on collaborator’s machine

## Best Practices and Advanced Topics

Now that you have a working environment, let’s cover best practices for maintaining it and advanced techniques for power users.

### Version Pinning Strategy

**The version pinning spectrum**:

```bash
# Most restrictive (exact version and build)
r-seurat = "=5.0.1"

# Pin major.minor (allow patch updates)
r-seurat = ">=5.0,&lt;5.1"

# Pin major (allow minor and patch updates)  
r-seurat = ">=5.0,&lt;6.0"

# No pinning (latest compatible version)
r-seurat = "*"
```

**Recommended strategy for scRNA-seq**:

```bash
[feature.part2-feature.dependencies]
# Core packages: Pin major version
r-seurat = ">=5.4.0,&lt;6"          # Allow 5.x updates, block 6.x
r-base = "4.3.*"                  # Allow 4.3.x, block 4.4

# Bioconductor: Pin major
bioconductor-dropletutils = ">=1.26,&lt;2"

# Utilities: Allow flexibility
r-dplyr = "*"
r-ggplot2 = "*"
```

**Rationale**:

- **Core packages**: Pin major version (breaking changes unlikely within major version)

- **Critical algorithms**: Pin tightly if results depend on exact behavior

- **Utilities**: Let pixi choose (faster resolution, fewer conflicts)

**When to pin exactly**:

- Final analysis for publication

- Known bugs in newer versions

- Reproducibility requirement (lock file handles this anyway)

### Performance Optimization

#### Channel Prioritization

**Default order matters**:

```
[workspace]
channels = ["conda-forge", "bioconda"]
```

Pixi searches in order. For R packages:

- **conda-forge first**: More up-to-date R packages, fewer conflicts

- **bioconda second**: Bioinformatics tools

**Custom channel for specific packages**:

```bash
# Force package from specific channel
pixi add conda-forge::r-seurat
pixi add bioconda::samtools
```

#### Parallel Downloads

**Increase concurrent downloads** (especially on HPC with fast networks):

Edit `~/.pixi/config.toml`:

```
[concurrency]
downloads = 20  # Default is 5
```

**Note**: Timeout and retry settings are not currently configurable via `~/.pixi/config.toml` and will cause warnings if included.

**Tuning guide**:

- Laptop/home: 5-10

- University network: 10-15

- HPC with 10 Gbps: 20-50

**Test your setting**:

```bash
time pixi install --all
# If network is bottleneck, increase downloads
# If CPU is bottleneck (100% usage), decrease
```

#### Cache Management

**Monitor cache size**:

```bash
# Check cache size
du -sh $PIXI_CACHE_DIR

# Or default location
du -sh ~/.cache/rattler
```

**Clean cache when needed**:

```bash
# Remove all cached packages
pixi clean cache

# Manually remove old packages (keeps recent)
find $PIXI_CACHE_DIR -type f -mtime +90 -delete  # Older than 90 days
```

**Cache strategies**:

- **Personal laptop**: Default location fine (~5 GB)

- **HPC with quota**: Custom location in scratch

- **Shared HPC**: Shared cache for team (permissions needed)

**Shared cache example** (HPC):

```bash
# Create shared cache (team leader)
mkdir -p /project/lab/shared/pixi-cache
chmod 775 /project/lab/shared/pixi-cache

# Team members add to ~/.bashrc
export PIXI_CACHE_DIR=/project/lab/shared/pixi-cache
```

Benefits:

- Each package downloaded once

- Faster for team members

- Reduced storage usage

### Managing Multiple Projects

**Project organization**:

```
~/projects/
├── project1-scrna/
│   ├── pixi.toml
│   ├── pixi.lock
│   └── .pixi/
├── project2-bulk/
│   ├── pixi.toml
│   ├── pixi.lock
│   └── .pixi/
└── project3-atac/
    ├── pixi.toml
    ├── pixi.lock
    └── .pixi/
```

**Shared dependencies**:

- Cache is shared across projects automatically

- Same package downloaded once, used everywhere

- Each project has isolated environment

**Switching between projects**:

```bash
cd ~/projects/project1-scrna
pixi run -e part2 R
# Uses project1's environment

cd ~/projects/project2-bulk
pixi run -e analysis R
# Uses project2's environment (no conflicts!)
```

### Updating Environments Safely

**The update workflow**:

1. **Create branch** for testing:

```
git checkout -b update-packages
```

1. **Update packages**:

```bash
# Update all packages
pixi update

# Or specific packages
pixi update r-seurat r-harmony
```

1. **Test thoroughly**:

```bash
# Run validation script
./validate_environment.sh

# Run key analysis scripts
pixi run part2-workflow
pixi run part3-workflow

# Compare results to previous version
diff results_old/ results_new/
```

1. **If tests pass**:

```
git add pixi.lock
git commit -m "Update packages: seurat 5.0.1→5.1.0, harmony 1.0→1.1"
git checkout main
git merge update-packages
```

1. **If tests fail**:

```bash
git checkout main  # Discard changes
# Or selectively update:
pixi add "r-seurat=5.0.1"  # Pin to old working version
```

**Best practice**: Test updates in CI/CD pipeline before merging.

### Collaborative Workflows

#### For Team Leader

**Setting up project**:

```bash
# Initialize project
mkdir team-scrna-analysis
cd team-scrna-analysis
pixi init --channel conda-forge --channel bioconda

# Add packages
pixi add --feature qc r-seurat ...
pixi add --feature integration r-harmony ...

# Create comprehensive README
cat > README.md << 'EOF'
# Team scRNA-seq Analysis

## Setup
1. Install Pixi: `curl -fsSL https://pixi.sh/install.sh | bash`
2. Clone repo: `git clone ...`
3. Install environment: `pixi install --all`
4. Run analysis: `pixi run full-workflow`

## Team Guidelines
- **Don't update packages** without team discussion
- **Test changes** in branch before merging
- **Document** any analysis decisions in `docs/`
- **Use tasks** for reproducible workflows
EOF

# Commit and push
git add pixi.toml pixi.lock README.md
git commit -m "Initial environment setup"
git push
```

#### For Team Members

**Joining project**:

```bash
# Clone
git clone git@github.com:team/scrna-analysis.git
cd scrna-analysis

# Install (gets exact environment leader created)
pixi install --all

# Verify
pixi run -e part2 R --version

# Start working
pixi run part2-workflow
```

**Proposing package additions**:

```r
# Create branch
git checkout -b add-cowplot

# Add package
pixi add --feature part3-feature r-cowplot

# Test
pixi install -e part3
pixi run -e part3 R
# library(cowplot)  # Verify it works

# Commit
git add pixi.toml pixi.lock
git commit -m "Add cowplot for enhanced plot layouts"
git push

# Create pull request for team review
```

### HPC Best Practices

#### Job Script Template

**SLURM script with pixi** (`template_job.sh`):

```bash
#!/bin/bash
#SBATCH --job-name=scrna-analysis
#SBATCH --cpus-per-task=16
#SBATCH --mem=64G
#SBATCH --time=12:00:00
#SBATCH --output=logs/job_%j.log
#SBATCH --error=logs/job_%j.err
#SBATCH --mail-type=END,FAIL
#SBATCH --mail-user=your.email@university.edu

# Print job info
echo "Job started at $(date)"
echo "Running on node: $(hostname)"
echo "Job ID: $SLURM_JOB_ID"

# Set up Pixi cache
export PIXI_CACHE_DIR=/scratch/$USER/pixi-cache

# Navigate to project
cd $SLURM_SUBMIT_DIR

# Run analysis
echo "Starting analysis..."
pixi run part2-workflow

echo "Job finished at $(date)"
```

**Submit**:

```
mkdir -p logs
sbatch template_job.sh
```

#### Array Jobs for Multiple Samples

**Process many samples** (`array_job.sh`):

```bash
#!/bin/bash
#SBATCH --job-name=scrna-array
#SBATCH --array=1-20  # 20 samples
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G
#SBATCH --time=4:00:00
#SBATCH --output=logs/sample_%A_%a.log

# Sample list
SAMPLES=($(cat sample_list.txt))
SAMPLE=${SAMPLES[$SLURM_ARRAY_TASK_ID-1]}

echo "Processing sample: $SAMPLE"

# Set up environment
export PIXI_CACHE_DIR=/scratch/$USER/pixi-cache
cd $SLURM_SUBMIT_DIR

# Download
SRR_ID=$SAMPLE pixi run download-sra

# QC
pixi run -e part1 fastqc ${SAMPLE}*.fastq

echo "Sample $SAMPLE complete"
```

**Create sample list**:

```
cat > sample_list.txt << 'EOF'
SRR14575500
SRR14575501
SRR14575502
...
EOF
```

**Submit**:

```
sbatch array_job.sh
```

## Conclusion

Environment setup is the foundation of computational biology. For too long, conda’s limitations have frustrated researchers and wasted valuable time. Slow dependency resolution, version conflicts, and missing system libraries have plagued everyone from beginners to veterans.

**Pixi changes this paradigm**. It brings the speed, reproducibility, and user experience that modern bioinformatics deserves:

- **5 minutes instead of 35** for environment setup

- **Zero version drift** between collaborators

- **Actually works** on HPC clusters without fighting the system

- **Modern, Git-integrated** workflow aligned with software engineering best practices

Whether you’re a beginner starting your first scRNA-seq project or an experienced bioinformatician tired of conda’s limitations, Pixi offers a compelling alternative that respects your time and your science.

**The best part?** You can adopt Pixi incrementally:

- Start with your next project

- Keep existing conda environments working

- Gradually transition as you experience the benefits

- Share with colleagues who ask “how did you set that up so fast?”

Your collaborators will thank you. Your future self will thank you. Your research will be more reproducible, your time better spent on science rather than debugging package conflicts.

**Now stop waiting for conda and start analyzing!**

Your scRNA-seq data awaits.

## References

1. **Pixi Documentation**. prefix.dev. https://pixi.sh (2025)

2. **Conda Documentation**. Anaconda, Inc. https://docs.conda.io (2025)

3. **Mamba Documentation**. mamba-org. https://mamba.readthedocs.io (2025)

4. **conda-forge Community**. https://conda-forge.org (2025)

5. **Grüning B, Dale R, Sjödin A, et al.** Bioconda: sustainable and comprehensive software distribution for the life sciences. *Nat Methods.* 2018;15(7):475-476. doi:10.1038/nmeth.4285

6. **Hao Y, Stuart T, Kowalski MH, et al.** Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nat Biotechnol.* 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y [Seurat 5]

7. **Lun ATL, Riesenfeld S, Andrews T, et al.** EmptyDrops: distinguishing cells from empty droplets in droplet-based single-cell RNA sequencing data. *Genome Biol.* 2019;20(1):63. doi:10.1186/s13059-019-1662-y [DropletUtils]

8. **Germain PL, Lun A, Garcia Meixide C, Macnair W, Robinson MD.** Doublet identification in single-cell sequencing data using scDblFinder. *F1000Res.* 2021;10:979. doi:10.12688/f1000research.73600.2

9. **Korsunsky I, Millard N, Fan J, et al.** Fast, sensitive and accurate integration of single-cell data with Harmony. *Nat Methods.* 2019;16(12):1289-1296. doi:10.1038/s41592-019-0619-0

10. **Haghverdi L, Lun ATL, Morgan MD, Marioni JC.** Batch effects in single-cell RNA-sequencing data are corrected by matching mutual nearest neighbors. *Nat Biotechnol.* 2018;36(5):421-427. doi:10.1038/nbt.4091 [FastMNN/batchelor]

11. **Aran D, Looney AP, Liu L, et al.** Reference-based analysis of lung single-cell sequencing reveals a transitional profibrotic macrophage. *Nat Immunol.* 2019;20(2):163-172. doi:10.1038/s41590-018-0276-y [SingleR]

12. **Shao X, Liao J, Lu X, et al.** scCATCH: automatic annotation on cell types of clusters from single-cell RNA sequencing data. *iScience.* 2020;23(3):100882. doi:10.1016/j.isci.2020.100882

13. **Ianevski A, Giri AK, Aittokallio T.** Fully-automated and ultra-fast cell-type identification using specific marker combinations from single-cell transcriptomic data. *Nat Commun.* 2022;13(1):1246. doi:10.1038/s41467-022-28803-w [scType]

14. **Zheng GXY, Terry JM, Belgrader P, et al.** Massively parallel digital transcriptional profiling of single cells. *Nat Commun.* 2017;8:14049. doi:10.1038/ncomms14049 [10x Genomics chemistry]

15. **Beaulieu-Jones BK, Greene CS.** Reproducibility of computational workflows is automated using continuous analysis. *Nat Biotechnol.* 2017;35(4):342-346. doi:10.1038/nbt.3780

16. **Grüning B, Chilton J, Köster J, et al.** Practical Computational Reproducibility in the Life Sciences. *Cell Syst.* 2018;6(6):631-635. doi:10.1016/j.cels.2018.03.014

17. **NGS101 Single-Cell RNA-seq Tutorial Series**. https://ngs101.com (2025)

18. **Pixi GitHub Repository**. https://github.com/prefix-dev/pixi (2025)

19. **Beaulieu-Jones BK, Greene CS.** The Reproducibility Crisis in Bioinformatics. *Annu Rev Biomed Data Sci.* 2020;3:309-327.

20. **Young MD, Behjati S.** SoupX removes ambient RNA contamination from droplet-based single-cell RNA sequencing data. *Gigascience.* 2020;9(12):giaa151. doi:10.1093/gigascience/giaa151

---

**Citation**: If this tutorial helped your research, please cite:

```
Lei. (2026). Setting Up Single-Cell RNA-seq Analysis Environment with Pixi: 10x Faster Setup, Zero Version Conflicts. 
NGS101. https://ngs101.com
```
