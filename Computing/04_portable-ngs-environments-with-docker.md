# Build Once, Run Anywhere: Creating Portable NGS Analysis Environments with Docker

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** HPC, Scientific Programming
- **Source:** [https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/)

---

*Never worry about “it works on my machine” again – create portable, reproducible NGS analysis environments*

Table of Contents

[Toggle](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/)

- [Introduction: The Reproducibility Challenge in NGS Analysis](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Introduction_The_Reproducibility_Challenge_in_NGS_Analysis)
- [Understanding Containers: Your Analysis Environment in a Box](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Understanding_Containers_Your_Analysis_Environment_in_a_Box)

  - [What is a Container?](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#What_is_a_Container)
  - [Why Containers Matter for NGS Analysis](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Why_Containers_Matter_for_NGS_Analysis)
  - [Traditional Environment Migration: The Conda Approach](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Traditional_Environment_Migration_The_Conda_Approach)
  - [Enter Docker: Containerization for NGS](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Enter_Docker_Containerization_for_NGS)

- [Installing Docker on Linux](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Installing_Docker_on_Linux)

  - [Ubuntu/Debian Installation](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#UbuntuDebian_Installation)
  - [CentOS/RHEL/Fedora Installation](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#CentOSRHELFedora_Installation)
  - [Post-Installation Configuration for Linux](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Post-Installation_Configuration_for_Linux)

- [Creating a Docker Image from Your HOMER ChIP-seq Environment](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Creating_a_Docker_Image_from_Your_HOMER_ChIP-seq_Environment)

  - [Step 1: Prepare Your Build Environment](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Step_1_Prepare_Your_Build_Environment)
  - [Step 2: Create the Dockerfile](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Step_2_Create_the_Dockerfile)
  - [Step 3: Build Your Docker Image](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Step_3_Build_Your_Docker_Image)
  - [Step 4: Test Your Docker Image](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Step_4_Test_Your_Docker_Image)
  - [Important Note: Manual Environment Activation](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Important_Note_Manual_Environment_Activation)

- [Working with Your Containerized Environment Interactively](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Working_with_Your_Containerized_Environment_Interactively)

  - [Understanding the Container Directory Structure](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Understanding_the_Container_Directory_Structure)
  - [Interactive Docker Usage](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Interactive_Docker_Usage)
  - [Creating Convenient Docker Wrapper Scripts](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Creating_Convenient_Docker_Wrapper_Scripts)
  - [Working Directory Best Practices for Docker](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Working_Directory_Best_Practices_for_Docker)

- [Using Docker Images on High-Performance Computing Systems](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Using_Docker_Images_on_High-Performance_Computing_Systems)

  - [The HPC Docker Limitation](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#The_HPC_Docker_Limitation)
  - [Enter Singularity: HPC-Friendly Containerization](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Enter_Singularity_HPC-Friendly_Containerization)
  - [Converting Your Docker Image for HPC Use](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Converting_Your_Docker_Image_for_HPC_Use)

- [Interactive Singularity Usage on HPC](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Interactive_Singularity_Usage_on_HPC)

  - [Basic Interactive Sessions](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Basic_Interactive_Sessions)
  - [Interactive Sessions with Bound Directories](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Interactive_Sessions_with_Bound_Directories)
  - [Running Single Commands with Singularity](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Running_Single_Commands_with_Singularity)
  - [Creating Convenient Singularity Wrapper Scripts](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Creating_Convenient_Singularity_Wrapper_Scripts)
  - [Working Directory Best Practices for HPC](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Working_Directory_Best_Practices_for_HPC)
  - [HPC-Specific Troubleshooting](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#HPC-Specific_Troubleshooting)
  - [Best Practices for HPC Container Usage](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Best_Practices_for_HPC_Container_Usage)

- [Advanced Container Management and Best Practices](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Advanced_Container_Management_and_Best_Practices)

  - [Creating Portable Analysis Scripts](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Creating_Portable_Analysis_Scripts)
  - [Version Control for Analysis Environments](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Version_Control_for_Analysis_Environments)
  - [Optimizing Container Size](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Optimizing_Container_Size)

- [Troubleshooting Common Issues](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Troubleshooting_Common_Issues)

  - [Build Failures](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Build_Failures)
  - [Runtime Issues](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Runtime_Issues)
  - [Performance Optimization](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Performance_Optimization)

- [Conclusion: Embracing Reproducible NGS Analysis](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Conclusion_Embracing_Reproducible_NGS_Analysis)

  - [Key Takeaways](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Key_Takeaways)
  - [Looking Forward](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Looking_Forward)

- [Further Resources and Documentation](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Further_Resources_and_Documentation)

  - [Docker Documentation and Resources](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Docker_Documentation_and_Resources)
  - [Singularity Documentation and Resources](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Singularity_Documentation_and_Resources)
  - [NGS-Specific Container Resources](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#NGS-Specific_Container_Resources)
  - [Share this:](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Share_this)
  - [Like this:](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/#Like_this)

## Introduction: The Reproducibility Challenge in NGS Analysis

Picture this scenario: you’ve spent weeks perfecting your ChIP-seq analysis pipeline on your local workstation. The results are beautiful, the workflow is smooth, and everything runs flawlessly. Then comes the moment of truth – you need to run the same analysis on your institution’s High-Performance Computing (HPC) cluster, or worse, collaborate with a colleague who has a completely different computing setup. Suddenly, your perfectly crafted environment becomes a nightmare of dependency conflicts, missing libraries, and cryptic error messages.

This challenge is all too familiar for bioinformaticians and NGS researchers. The complexity of modern NGS analysis requires dozens of specialized tools, each with its own dependencies, version requirements, and configuration quirks. What runs perfectly on Ubuntu 20.04 might fail spectacularly on CentOS 7. A pipeline that works with Python 3.8 might break with Python 3.10. This is where containerization technology, specifically Docker, becomes a game-changer for NGS analysis.

## Understanding Containers: Your Analysis Environment in a Box

### What is a Container?

A container is essentially a lightweight, portable package that includes everything needed to run your application: the code, runtime, system tools, libraries, and settings. Think of it as a standardized shipping container for software – just as physical shipping containers revolutionized global trade by creating a standard way to transport goods regardless of the ship or truck carrying them, software containers create a standard way to package and run applications regardless of the underlying system.

In the context of NGS analysis, a container packages your entire computational environment – your Conda environments, reference genomes, analysis tools like HOMER or BWA, and even your data – into a single, self-contained unit that runs consistently across different computing platforms.

### Why Containers Matter for NGS Analysis

NGS data analysis presents unique challenges that make containers particularly valuable:

**Dependency Complexity**: A typical NGS pipeline might use dozens of tools written in different languages (Python, R, C++, Perl), each requiring specific library versions. Tools like STAR might need one version of GCC, while another tool requires a different version. Containers encapsulate all these dependencies, preventing conflicts.

**Reproducibility Crisis**: Scientific reproducibility is paramount, yet software environments are constantly changing. A analysis that works today might fail in six months due to updated dependencies. Containers freeze your environment in time, ensuring your analysis remains reproducible years later.

**Platform Heterogeneity**: NGS researchers work across diverse computing environments – personal laptops, institutional servers, cloud platforms, and HPC clusters. Each might run different operating systems with different configurations. Containers provide consistency across all these platforms.

**Collaboration Challenges**: Sharing complex NGS pipelines between researchers traditionally required extensive documentation and troubleshooting. With containers, you share the exact environment, not just the code.

### Traditional Environment Migration: The Conda Approach

Before diving into Docker, let’s understand the traditional approach to environment portability using Conda. When you need to recreate your NGS environment on a new system, you might:

```bash
# Export your current environment
conda env export > environment.yml

# On the new system, recreate the environment
conda env create -f environment.yml
```

While this approach works in many cases, it has significant limitations:

- **Platform Dependencies**: Some packages are compiled for specific operating systems or architectures

- **Version Conflicts**: The target system might have incompatible system libraries

- **External Dependencies**: Tools that require specific system configurations or files outside Conda

- **Reference Data**: Large reference genomes and databases aren’t easily portable through Conda

### Enter Docker: Containerization for NGS

Docker addresses these limitations by packaging not just your Python/R packages, but the entire operating system environment. When you run a Docker container, you’re essentially running a lightweight virtual machine that includes:

- The base operating system (usually Linux)

- All system libraries and dependencies

- Your Conda environment with all packages

- Your reference genomes and databases

- Your analysis scripts and tools

- Any custom configurations

**Advantages of Docker for NGS:**

1. **Complete Environment Isolation**: Your container runs independently of the host system’s configuration

2. **True Reproducibility**: Bit-for-bit identical environments across systems

3. **Easy Sharing**: Distribute entire environments through Docker registries

4. **Version Control**: Tag different versions of your environment

5. **HPC Compatibility**: Use containers on HPC systems through Singularity

**Potential Drawbacks:**

1. **Learning Curve**: Requires understanding Docker concepts and commands

2. **Storage Overhead**: Containers can be large (10-50GB for NGS environments)

3. **Performance Impact**: Minimal but measurable overhead compared to native execution

4. **Complexity**: Additional layer of abstraction in your workflow

## Installing Docker on Linux

Before we can create and use Docker containers, we need to install Docker on your local system.  Linux is Docker’s native platform and offers the most straightforward installation experience. The process varies slightly depending on your distribution.

### Ubuntu/Debian Installation

For Ubuntu 20.04, 22.04, 24.04, or Debian-based distributions:

```bash
# Update package index
sudo apt-get update

# Install prerequisite packages
sudo apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Add Docker's official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
    sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up the Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package index again
sudo apt-get update

# Install Docker Engine, containerd, and Docker Compose
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Verify Docker installation
sudo docker run hello-world
```

If you see a “Hello from Docker!” message, the installation was successful.

### CentOS/RHEL/Fedora Installation

For CentOS 7/8, RHEL 7/8, or Fedora-based distributions:

```bash
# Remove old Docker versions if present
sudo yum remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-engine

# Install yum-utils
sudo yum install -y yum-utils

# Add Docker repository
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# Install Docker Engine
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Start Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Verify installation
sudo docker run hello-world
```

### Post-Installation Configuration for Linux

By default, Docker requires root privileges. To run Docker commands without `sudo`, add your user to the `docker` group:

```bash
# Create the docker group (usually already exists)
sudo groupadd docker

# Add your user to the docker group
sudo usermod -aG docker $USER

# Log out and log back in for changes to take effect
# Or activate the changes immediately with:
newgrp docker

# Verify you can run Docker without sudo
docker run hello-world
```

> 

**Important Security Note**: Adding a user to the `docker` group grants privileges equivalent to root access. Only do this on systems where you trust all users in the docker group.

## Creating a Docker Image from Your HOMER ChIP-seq Environment

Let’s walk through creating a Docker image using the ChIP-seq analysis environment from our [previous ChIP-seq tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/). This will serve as a practical example that you can adapt for your own NGS workflows.

### Step 1: Prepare Your Build Environment

First, we need to organize all the components of our analysis environment. This includes the Conda environment, reference files, and any additional tools or data.

```bash
# Activate your existing HOMER environment
conda activate ~/Env_Homer

# Create a dedicated directory for Docker build
mkdir ~/docker_build
cd ~/docker_build

# Export your Conda environment to a file
# This captures all package versions and dependencies
conda env export -p ~/Env_Homer > environment.yml

# Copy essential directories to the build context
# Note: These directories can be quite large (several GB)
cp -r ~/homer ./                    # HOMER installation
cp -r ~/BWA_Index_hg38 ./          # BWA genome index
cp -r ~/references ./              # Reference files (blacklists, etc.)

# Optional: Include example data (increases image size significantly)
cp -r ~/GSE104247 ./               # ChIP-seq example dataset
```

> 

**Storage Consideration:** The BWA index and reference files can be quite large (5-10GB). Consider whether you need these in every image or if they can be mounted separately.

### Step 2: Create the Dockerfile

The Dockerfile is the blueprint for building your container. Create a file named `Dockerfile` (no extension) in your build directory (assume your local system is Ubuntu/Debian):

```bash
# Use Miniconda3 as our base image for easy Conda management
FROM continuumio/miniconda3:latest

# Set metadata for the image
LABEL maintainer="your-email@example.com"
LABEL description="HOMER ChIP-seq analysis environment with complete NGS toolkit"
LABEL version="1.0"
LABEL tutorial="ngs101.com/homer-chipseq-tutorial"

# Configure environment variables for consistent behavior
ENV DEBIAN_FRONTEND=noninteractive
ENV PATH="/opt/conda/envs/Env_Homer/bin:/opt/homer/bin:$PATH"
ENV CONDA_DEFAULT_ENV="Env_Homer"
ENV HOMER_PATH="/opt/homer"

# Install system dependencies that might be needed
# These are tools not available through Conda
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    build-essential \
    perl \
    gzip \
    zip \
    unzip \
    procps \
    && rm -rf /var/lib/apt/lists/*

# Copy the exported Conda environment file
COPY environment.yml /tmp/environment.yml

# Create the Conda environment from our exported file
# This recreates our exact NGS analysis environment
RUN conda env create -f /tmp/environment.yml -p /opt/conda/envs/Env_Homer && \
    conda clean -afy

# Install HOMER from source to avoid compatibility issues
# HOMER sometimes has GLIBC issues when pre-compiled binaries are used
RUN mkdir -p /opt/homer && \
    cd /opt/homer && \
    wget http://homer.ucsd.edu/homer/configureHomer.pl && \
    perl configureHomer.pl -install && \
    perl configureHomer.pl -install hg38 && \
    chmod -R 755 /opt/homer

# Copy reference files to standardized locations
# Using /opt/ instead of /root/ for HPC compatibility
COPY BWA_Index_hg38 /opt/BWA_Index_hg38
COPY references /opt/references

# Optional: Copy example data (comment out to reduce image size)
COPY GSE104247 /opt/GSE104247

# Set proper permissions for all copied files
# This ensures files are accessible in HPC environments
RUN chmod -R 755 /opt/BWA_Index_hg38 /opt/references /opt/GSE104247

# Set the working directory
WORKDIR /opt

# Create environment activation script
# This ensures the Conda environment is properly activated
RUN conda init bash && \
    echo '#!/bin/bash' > /opt/activate_env.sh && \
    echo 'export PATH="/opt/conda/envs/Env_Homer/bin:/opt/homer/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"' >> /opt/activate_env.sh && \
    echo 'source /opt/conda/etc/profile.d/conda.sh' >> /opt/activate_env.sh && \
    echo 'conda activate Env_Homer' >> /opt/activate_env.sh && \
    chmod 755 /opt/activate_env.sh

# Create a startup script for interactive sessions
# This provides a user-friendly interface when starting the container
RUN echo '#!/bin/bash' > /opt/start_homer.sh && \
    echo 'source /opt/activate_env.sh' >> /opt/start_homer.sh && \
    echo 'echo "=== HOMER ChIP-seq Environment Ready! ==="' >> /opt/start_homer.sh && \
    echo 'echo "HOMER path: $HOMER_PATH"' >> /opt/start_homer.sh && \
    echo 'echo "Available tools: homer, bwa, samtools, bedtools, etc."' >> /opt/start_homer.sh && \
    echo 'echo "Example data: /opt/GSE104247"' >> /opt/start_homer.sh && \
    echo 'echo "Reference files: /opt/BWA_Index_hg38"' >> /opt/start_homer.sh && \
    echo 'echo "================================"' >> /opt/start_homer.sh && \
    echo 'if [ $# -eq 0 ]; then' >> /opt/start_homer.sh && \
    echo '    exec /bin/bash --rcfile /opt/activate_env.sh -i' >> /opt/start_homer.sh && \
    echo 'else' >> /opt/start_homer.sh && \
    echo '    exec "$@"' >> /opt/start_homer.sh && \
    echo 'fi' >> /opt/start_homer.sh && \
    chmod 755 /opt/start_homer.sh

# Ensure environment is activated for future bash sessions
RUN echo 'source /opt/activate_env.sh' >> /root/.bashrc

# Set the entry point and default command
ENTRYPOINT ["/bin/bash", "/opt/start_homer.sh"]
CMD ["/bin/bash"]
```

### Step 3: Build Your Docker Image

Now we’ll build the Docker image. This process can take 30-60 minutes depending on your system and the size of your reference files.

```bash
# Navigate to your build directory
cd ~/docker_build

# Build the Docker image with a descriptive tag
# The '.' tells Docker to use the current directory as build context
# Note: Docker requires root privileges, so we use sudo
sudo docker build -t homer-chipseq:latest .

# Alternative: Build with version-specific tag for better organization
sudo docker build -t homer-chipseq:v1.0 .

# For better progress tracking, use BuildKit
sudo DOCKER_BUILDKIT=1 docker build -t homer-chipseq:v1.0 .
```

> 

**Build Tips:**

- Ensure you have at least 30-40GB free disk space

- The build process will download and compile software, which takes time

- If the build fails, check the error messages carefully – often it’s a permission or space issue

### Step 4: Test Your Docker Image

Before deploying your image, thoroughly test it to ensure everything works correctly:

```bash
# Run the container interactively
# Note: Docker requires root privileges
sudo docker run -it homer-chipseq:latest

# Inside the container, test that all tools are accessible:

# Test HOMER installation
which findPeaks
findPeaks  # Should show usage information

# Test other NGS tools
which bwa
which samtools
which bedtools

# Verify reference files are present
ls -la /opt/BWA_Index_hg38/
ls -la /opt/references/

# Check example data (if included)
ls -la /opt/GSE104247/

# Test a simple command to ensure environment is working
samtools --version
```

### Important Note: Manual Environment Activation

If you built this Docker image on WSL (Windows Subsystem for Linux), you might encounter path issues where tools aren’t automatically found. In such cases, you’ll need to manually activate the environment:

```bash
# If tools aren't found automatically, manually activate the environment
sudo docker run -it homer-chipseq:latest

source /opt/activate_env.sh

# Then test again
which findPeaks
```

This is a common issue when building Docker images on WSL, as path handling can be inconsistent between the build environment and runtime environment.

## Working with Your Containerized Environment Interactively

Once your Docker image is built and tested, you’ll want to use it for actual analysis work. Let’s explore how to work interactively with Docker on your local system.

### Understanding the Container Directory Structure

Before diving into interactive usage, it’s helpful to understand how your containerized environment is organized. Let’s explore the directory structure:

```bash
# Start an interactive session to explore the container
sudo docker run -it homer-chipseq:latest

# Once inside the container, explore the directory structure
ls -la /

# Key directories in our container:
# /opt/ - Main location for our installed tools and data
tree /opt/ -L 2
# Output will show:
# /opt/
# ├── BWA_Index_hg38/          # BWA genome index files
# ├── GSE104247/               # Example ChIP-seq data
# │   ├── raw/                 # Raw FASTQ files
# │   ├── trim/                # Trimmed FASTQ files
# │   ├── bam/                 # Alignment files
# │   └── homer/               # HOMER analysis results
# ├── conda/                   # Conda installation
# │   └── envs/
# │       └── Env_Homer/       # Our NGS environment
# ├── homer/                   # HOMER installation
# │   ├── bin/                 # HOMER executables
# │   ├── data/                # HOMER reference data
# │   └── cpp/                 # HOMER C++ source
# ├── references/              # Additional reference files
# │   └── blacklists/          # ENCODE blacklist regions
# ├── activate_env.sh          # Environment activation script
# └── start_homer.sh           # Container startup script

# Check the Conda environment location
ls -la /opt/conda/envs/Env_Homer/bin/ | head -10

# Verify HOMER installation
ls -la /opt/homer/bin/ | head -10

# Check reference genome files
ls -la /opt/BWA_Index_hg38/

# View environment variables
echo "PATH: $PATH"
echo "CONDA_DEFAULT_ENV: $CONDA_DEFAULT_ENV"
echo "HOMER_PATH: $HOMER_PATH"
```

### Interactive Docker Usage

#### Basic Interactive Session

```bash
# Start a basic interactive session
sudo docker run -it homer-chipseq:latest

# The container will start with our custom startup script
# You should see a welcome message:
# === HOMER ChIP-seq Environment Ready! ===
# HOMER path: /opt/homer
# Available tools: homer, bwa, samtools, bedtools, etc.
# Example data: /opt/GSE104247
# Reference files: /opt/BWA_Index_hg38
# ================================
```

#### Interactive Session with Data Mounting

For real analysis work, you’ll want to mount your local data directories into the container:

```bash
# Mount your local data directory into the container
sudo docker run -it --rm \
    -v /path/to/your/data:/workspace \
    -w /workspace \
    homer-chipseq:latest

# Example: Mount current directory as workspace
sudo docker run -it --rm \
    -v $(pwd):/workspace \
    -w /workspace \
    homer-chipseq:latest

# Inside the container, you can now access your local files
ls -la /workspace/
# This shows the contents of your local directory

# Work with your data while having access to all containerized tools
makeTagDirectory /workspace/output_tags \
    /workspace/my_sample.bam \
    -genome hg38
```

#### Running Single Commands

Instead of interactive sessions, you can execute single commands:

```bash
# Run a single command in the container
sudo docker run --rm \
    -v $(pwd):/workspace \
    -w /workspace \
    homer-chipseq:latest \
    bash -c "source /opt/activate_env.sh && findPeaks -h"

# Process a BAM file with HOMER
sudo docker run --rm \
    -v $(pwd):/workspace \
    -w /workspace \
    homer-chipseq:latest \
    bash -c "
        source /opt/activate_env.sh
        makeTagDirectory /workspace/output_tags \
            /workspace/sample.bam \
            -genome hg38
    "
```

### Creating Convenient Docker Wrapper Scripts

To make interactive usage easier, create wrapper scripts for common scenarios:

```bash
# Create docker_homer.sh for convenient Docker usage
cat > docker_homer.sh << 'EOF'
#!/bin/bash

# Docker HOMER Environment Wrapper
# Usage: ./docker_homer.sh [command]

IMAGE="homer-chipseq:latest"
MOUNT_DIR=$(pwd)

if [ $# -eq 0 ]; then
    echo "Starting interactive HOMER Docker environment..."
    echo "Current directory mounted as /workspace"
    sudo docker run -it --rm \
        -v ${MOUNT_DIR}:/workspace \
        -w /workspace \
        ${IMAGE}
else
    echo "Executing command in HOMER Docker environment..."
    sudo docker run --rm \
        -v ${MOUNT_DIR}:/workspace \
        -w /workspace \
        ${IMAGE} \
        bash -c "source /opt/activate_env.sh && $*"
fi
EOF

chmod +x docker_homer.sh

# Usage examples:
# Interactive session
./docker_homer.sh

# Single command
./docker_homer.sh findPeaks -h

# Complex command
./docker_homer.sh "makeTagDirectory tags sample.bam -genome hg38"
```

### Working Directory Best Practices for Docker

```bash
# Always mount your data directory
# Don't rely on copying data into containers
sudo docker run -it --rm \
    -v /path/to/project:/workspace \
    -w /workspace \
    homer-chipseq:latest

# Organize your mounted workspace:
# /workspace/
# ├── data/           # Raw data files
# ├── results/        # Analysis outputs
# ├── scripts/        # Analysis scripts
# └── logs/           # Log files
```

## Using Docker Images on High-Performance Computing Systems

### The HPC Docker Limitation

While Docker works excellently on personal computers and many cloud platforms, most HPC systems don’t allow direct Docker usage. This restriction exists for several important security and administrative reasons:

**Security Concerns**: Docker requires root privileges to run, which poses security risks in shared computing environments. A compromised Docker container could potentially access other users’ data or system resources.

**Resource Management**: HPC systems use sophisticated schedulers (like SLURM or PBS) to manage computational resources. Docker’s resource management can conflict with these systems.

**Network Isolation**: HPC systems often have complex networking configurations that Docker can interfere with.

**Multi-tenancy**: HPC systems serve multiple users simultaneously, and Docker’s privilege model doesn’t align well with this shared environment.

### Enter Singularity: HPC-Friendly Containerization

Singularity was specifically designed to address Docker’s limitations in HPC environments. It provides container functionality while maintaining the security and resource management requirements of shared computing systems.

**Key Singularity Advantages for HPC:**

- **No Root Privileges Required**: Singularity containers run with user-level permissions

- **HPC Scheduler Integration**: Works seamlessly with SLURM, PBS, and other schedulers

- **File System Integration**: Easy access to shared file systems and home directories

- **Docker Compatibility**: Can run Docker images with minimal modification

### Converting Your Docker Image for HPC Use

The beauty of Singularity is that it can directly use Docker images. Here’s how to deploy your HOMER ChIP-seq environment on an HPC system:

#### Step 1: Prepare Your Docker Image for HPC

There are several ways to get your Docker image ready for HPC deployment. Choose the method that works best for your HPC system’s policies and internet access:

**Method A: Convert to Singularity locally, then transfer (RECOMMENDED for restricted HPC)**

```bash
# On your local system with Docker, convert Docker image to Singularity format
# This creates a .sif file that can be easily transferred

# Option 1: Direct conversion (requires Singularity installed locally)
# Install Singularity on your local system if not already installed
# On Ubuntu/Debian:
# sudo apt update && sudo apt install -y singularity-container

# Convert Docker image to Singularity format
sudo singularity build homer-chipseq.sif docker-daemon://homer-chipseq:latest

# Transfer the .sif file to HPC (much easier than docker tar files)
scp homer-chipseq.sif username@hpc-login-node.edu:/home/username/

# Option 2: Use docker2singularity if Singularity isn't installed locally
# This runs a containerized version of the conversion tool
sudo docker run -v /var/run/docker.sock:/var/run/docker.sock \
    -v $(pwd):/output \
    --privileged -t --rm \
    quay.io/singularity/docker2singularity \
    homer-chipseq:latest

# This creates a .sif file in your current directory
# Transfer it to HPC
scp *.sif username@hpc-login-node.edu:/home/username/
```

**Method B: Push to Docker Hub, pull on HPC (RECOMMENDED for connected HPC)**

```bash
# Tag your image for Docker Hub (replace 'yourusername' with your Docker Hub username)
sudo docker tag homer-chipseq:latest yourusername/homer-chipseq:latest

# Push to Docker Hub
sudo docker push yourusername/homer-chipseq:latest

# This eliminates the need to transfer large files and works better with HPC systems
```

**Method C: Save Docker tar and transfer (LEGACY method)**

```bash
# Save the Docker image to a compressed tar file
# Note: This creates very large files and is harder to work with on HPC
sudo docker save homer-chipseq:latest | gzip > homer-chipseq-v1.0.tar.gz

# Transfer to HPC using scp (replace with your HPC details)
scp homer-chipseq-v1.0.tar.gz username@hpc-login-node.edu:/home/username/
```

#### Step 2: Deploy on HPC

The deployment method depends on how you prepared your image:

**For Method A (Singularity .sif file transferred):**

```bash
# Log into your HPC system
ssh username@hpc-login-node.edu

# The .sif file is ready to use immediately - no conversion needed!
# Just verify it works
singularity exec homer-chipseq.sif bash -c "source /opt/activate_env.sh && which findPeaks"
```

**For Method B (Docker Hub approach):**

```bash
# Log into your HPC system
ssh username@hpc-login-node.edu

# Check if Singularity is available as a module
module avail singularity
module load singularity

# If Singularity is not available as a module, install via Conda
if ! command -v singularity &> /dev/null; then
    echo "Singularity not found as module. Installing via Conda..."
    conda create -n singularity-env -c conda-forge singularity
    conda activate singularity-env
    singularity --version
fi

# Pull and convert from Docker Hub
singularity build homer-chipseq.sif docker://yourusername/homer-chipseq:latest
```

**For Method C (Docker tar file):**

```bash
# Log into your HPC system
ssh username@hpc-login-node.edu

# Load Singularity
module load singularity

# Convert from docker archive (newer Singularity versions)
singularity build homer-chipseq.sif docker-archive://homer-chipseq-v1.0.tar.gz

# Note: This method may not work on all HPC systems due to Singularity version differences
```

#### Step 3: Test Your Singularity Container

```bash
# Test the container interactively
singularity shell homer-chipseq.sif

# Inside the container, verify everything works
# Note: You may need to manually activate the environment
source /opt/activate_env.sh

# Test tools
which findPeaks
samtools --version

# Check data accessibility
ls /opt/GSE104247/
```

## Interactive Singularity Usage on HPC

Once your Singularity container is set up, you’ll want to use it for actual analysis work on the HPC system. Let’s explore different ways to work interactively with your containerized environment.

### Basic Interactive Sessions

```bash
# Start an interactive Singularity session
singularity shell homer-chipseq.sif

# Once inside, manually activate the environment
source /opt/activate_env.sh

# Now you have access to all tools
which findPeaks
samtools --version

# Explore the same directory structure as in Docker
ls -la /opt/
```

### Interactive Sessions with Bound Directories

On HPC systems, you’ll typically want to bind external directories to access your data:

```bash
# Bind your home directory and scratch space
singularity shell \
    --bind /home:/home,/scratch:/scratch \
    homer-chipseq.sif

# Inside the container
source /opt/activate_env.sh

# Access your HPC directories
ls /home/username/
ls /scratch/

# Work with your data using containerized tools
cd /scratch/my_project/
makeTagDirectory output_tags \
    sample.bam \
    -genome hg38
```

### Running Single Commands with Singularity

Instead of interactive sessions, you can execute single commands directly:

```bash
# Execute a single command
singularity exec \
    --bind /home:/home,/scratch:/scratch \
    homer-chipseq.sif \
    bash -c "source /opt/activate_env.sh && findPeaks -h"

# Run a complete analysis step
singularity exec \
    --bind /scratch:/scratch \
    homer-chipseq.sif \
    bash -c "
        source /opt/activate_env.sh
        cd /scratch/my_project
        findPeaks output_tags \
            -style factor \
            -o peaks.txt \
            -i control_tags
    "
```

### Creating Convenient Singularity Wrapper Scripts

To make HPC usage easier, create wrapper scripts for common scenarios:

```bash
# Create singularity_homer.sh for HPC usage
cat > singularity_homer.sh << 'EOF'
#!/bin/bash

# Singularity HOMER Environment Wrapper
# Usage: ./singularity_homer.sh [command]

IMAGE="homer-chipseq.sif"
BIND_PATHS="/home:/home,/scratch:/scratch"

# Check if Singularity is available
if ! command -v singularity &> /dev/null; then
    echo "Error: Singularity not found."
    echo "Try: module load singularity"
    exit 1
fi

if [ $# -eq 0 ]; then
    echo "Starting interactive HOMER Singularity environment..."
    echo "Remember to run: source /opt/activate_env.sh"
    singularity shell --bind ${BIND_PATHS} ${IMAGE}
else
    echo "Executing command in HOMER Singularity environment..."
    singularity exec --bind ${BIND_PATHS} ${IMAGE} \
        bash -c "source /opt/activate_env.sh && $*"
fi
EOF

chmod +x singularity_homer.sh

# Usage examples:
# Interactive session
./singularity_homer.sh

# Single command
./singularity_homer.sh findPeaks -h

# Complex analysis
./singularity_homer.sh "cd /scratch/project && makeTagDirectory tags sample.bam -genome hg38"
```

### Working Directory Best Practices for HPC

```bash
# Use HPC scratch space for analysis
# Mount home for scripts and small files
singularity shell \
    --bind /home:/home,/scratch:/scratch \
    homer-chipseq.sif

# Typical HPC workflow:
# 1. Scripts in /home/username/scripts/
# 2. Data in /scratch/username/project/data/
# 3. Results in /scratch/username/project/results/
# 4. Temporary files in /tmp/ (usually local to compute node)
```

### HPC-Specific Troubleshooting

#### Common Issues and Solutions

**Problem**: Commands not found in Singularity interactive session**Solution**:

```bash
# Always source the activation script
source /opt/activate_env.sh

# Check if tools are in PATH
echo $PATH
which findPeaks
```

**Problem**: Can’t access data on HPC file systems**Solution**:

```bash
# Use appropriate bind mounts
singularity exec --bind /scratch:/scratch,/home:/home homer-chipseq.sif your_command

# Check what's mounted inside container
ls /scratch/
ls /home/
```

**Problem**: Permission issues (less common with Singularity)**Solution**:

```bash
# Use --fakeroot if available and needed
singularity exec --fakeroot homer-chipseq.sif your_command

# Check file permissions
ls -la /scratch/your_files/
```

#### Step 4: Running Analysis Jobs

Create a SLURM job script to run your ChIP-seq analysis:

```bash
#!/bin/bash
#SBATCH --job-name=chipseq_analysis
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --time=4:00:00
#SBATCH --mem=32GB
#SBATCH --output=chipseq_%j.out
#SBATCH --error=chipseq_%j.err

# Load Singularity module
module load singularity

# Set up environment variables
export SINGULARITY_BIND="/scratch:/scratch,/home:/home"

# Run your analysis inside the container
singularity exec homer-chipseq.sif bash -c "
    source /opt/activate_env.sh

    # Your ChIP-seq analysis commands here
    makeTagDirectory /scratch/output_tags \
        /home/username/data/sample.bam \
        -genome hg38

    findPeaks /scratch/output_tags \
        -style factor \
        -o /scratch/peaks.txt \
        -i /home/username/data/input_tags
"
```

### Best Practices for HPC Container Usage

**Data Management**: Use the `--bind` option to mount HPC file systems into your container:

```bash
# Mount scratch and home directories
singularity exec --bind /scratch:/scratch,/home:/home homer-chipseq.sif your_command
```

**Resource Allocation**: Remember that containers don’t change resource requirements – allocate CPU and memory based on your analysis needs, not container overhead.

**Module Integration**: Many HPC systems allow loading additional modules alongside Singularity containers:

```bash
module load singularity python/3.9
singularity exec homer-chipseq.sif python my_analysis_script.py
```

**Workflow Integration**: Integrate containers into workflow management systems like Nextflow or Snakemake for complex pipelines.

## Advanced Container Management and Best Practices

### Creating Portable Analysis Scripts

Create wrapper scripts that make your containerized environment easy to use:

```bash
# Create a convenient wrapper script
cat > run_homer_analysis.sh << 'EOF'
#!/bin/bash

# HOMER ChIP-seq Analysis Wrapper Script
# Usage: ./run_homer_analysis.sh [command]

CONTAINER_IMAGE="homer-chipseq.sif"
BIND_PATHS="/scratch:/scratch,/home:/home,/data:/data"

# Check if Singularity is available
if ! command -v singularity &> /dev/null; then
    echo "Error: Singularity not found. Please load the singularity module."
    echo "Try: module load singularity"
    exit 1
fi

# Check if container image exists
if [ ! -f "$CONTAINER_IMAGE" ]; then
    echo "Error: Container image $CONTAINER_IMAGE not found."
    echo "Please ensure the image is in the current directory."
    exit 1
fi

# Run the command in the container with environment activated
if [ $# -eq 0 ]; then
    # Interactive mode
    echo "Starting interactive HOMER environment..."
    singularity shell --bind $BIND_PATHS $CONTAINER_IMAGE
else
    # Execute specific command
    singularity exec --bind $BIND_PATHS $CONTAINER_IMAGE bash -c "
        source /opt/activate_env.sh
        $@
    "
fi
EOF

chmod +x run_homer_analysis.sh

# Usage examples:
# Interactive session
./run_homer_analysis.sh

# Run specific command
./run_homer_analysis.sh findPeaks.pl -h

# Run complete analysis
./run_homer_analysis.sh "makeTagDirectory output sample.bam -genome hg38"
```

### Version Control for Analysis Environments

Tag your Docker images with meaningful versions to track environment changes:

```bash
# Tag different versions of your environment
sudo docker tag homer-chipseq:latest homer-chipseq:v1.0-initial
sudo docker tag homer-chipseq:latest homer-chipseq:v1.1-updated-homer
sudo docker tag homer-chipseq:latest homer-chipseq:v2.0-added-macs2

# Push all versions to preserve them
sudo docker push yourusername/homer-chipseq:v1.0-initial
sudo docker push yourusername/homer-chipseq:v1.1-updated-homer
sudo docker push yourusername/homer-chipseq:v2.0-added-macs2
```

### Optimizing Container Size

Large containers can be problematic for storage and transfer. Here are strategies to minimize size:

```bash
# Multi-stage build to reduce final image size
FROM continuumio/miniconda3:latest AS builder

# Install and configure everything in the builder stage
COPY environment.yml /tmp/environment.yml
RUN conda env create -f /tmp/environment.yml -p /opt/conda/envs/Env_Homer

# Final stage with only necessary components
FROM continuumio/miniconda3:latest
COPY --from=builder /opt/conda/envs/Env_Homer /opt/conda/envs/Env_Homer

# Continue with the rest of your Dockerfile...
```

## Troubleshooting Common Issues

### Build Failures

**Problem**: Docker build fails with “No space left on device”**Solution**:

```bash
# Clean up Docker system (requires sudo)
sudo docker system prune -a
# Or increase disk space allocation for Docker
```

**Problem**: Conda environment export includes platform-specific packages**Solution**:

```bash
# Export with --no-builds flag for cross-platform compatibility
conda env export --no-builds > environment.yml
```

### Runtime Issues

**Problem**: Tools not found in container**Solution**:

```bash
# Always manually activate the environment
source /opt/activate_env.sh
# Or modify your Dockerfile to ensure PATH is set correctly
```

**Problem**: Permission denied on HPC**Solution**:

```bash
# Use --fakeroot if available, or ensure files have correct permissions
singularity exec --fakeroot homer-chipseq.sif your_command
```

**Problem**: Container can’t access data on HPC**Solution**:

```bash
# Use appropriate bind mounts
singularity exec --bind /scratch:/scratch,/home:/home homer-chipseq.sif your_command
```

### Performance Optimization

**Container Overhead**: While containers add minimal overhead, you can optimize performance:

```bash
# Use tmpfs for temporary files (if available)
singularity exec --bind /tmp:/tmp homer-chipseq.sif your_analysis

# Allocate appropriate resources in SLURM
#SBATCH --mem=64GB  # Based on actual analysis needs, not container size
```

## Conclusion: Embracing Reproducible NGS Analysis

Containerization represents a paradigm shift in how we approach NGS data analysis. By packaging your entire computational environment – from the operating system to your specific tool versions to your reference data – you create a time capsule that preserves not just your analysis code, but the complete context needed to reproduce your results.

The journey from a local Conda environment to a portable Docker container might seem complex initially, but the benefits compound over time. Your future self will thank you when you can reproduce results from a year-old analysis in minutes rather than days. Your collaborators will appreciate receiving a complete, working environment rather than a lengthy installation guide. Your publication reviewers will value the ability to verify your computational methods exactly as you performed them.

As NGS technologies continue to evolve and the complexity of analysis pipelines increases, the importance of reproducible, portable computational environments will only grow. Container technology provides a foundation for more reliable, collaborative, and transparent bioinformatics research.

### Key Takeaways

1. **Start Simple**: Begin with containerizing your existing successful analyses before building complex new pipelines

2. **Document Everything**: Include clear README files and metadata in your container images

3. **Version Control**: Use meaningful tags to track different versions of your analysis environments

4. **Test Thoroughly**: Always test your containers on different systems before relying on them for important analyses

5. **Share Responsibly**: Consider data sensitivity and licensing when sharing container images

### Looking Forward

Container technology is just the beginning. Technologies like workflow managers (Nextflow, Snakemake) are increasingly integrating with containers to create even more robust and scalable analysis pipelines. Cloud computing platforms are making it easier than ever to run containerized analyses at scale. The future of NGS analysis is increasingly containerized, reproducible, and collaborative.

By mastering these containerization techniques now, you’re not just solving today’s reproducibility challenges – you’re preparing for the future of computational biology, where complex multi-omics analyses will require the kind of robust, portable environments that only containers can provide.

## Further Resources and Documentation

To deepen your understanding of containerization for NGS analysis and stay current with best practices, here are essential resources:

### Docker Documentation and Resources

**Official Docker Documentation**

- [Docker Documentation](https://docs.docker.com/) – Comprehensive official documentation

- [Docker Get Started Guide](https://docs.docker.com/get-started/) – Beginner-friendly introduction to Docker concepts

- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/) – Complete guide to writing Dockerfiles

**Bioinformatics-Specific Docker Resources**

- [BioContainers](https://biocontainers.pro/) – Community-driven bioinformatics containers

- [Docker for Bioinformatics](https://github.com/BioContainers/containers) – Collection of bioinformatics Docker images

### Singularity Documentation and Resources

**Official Singularity Documentation**

- [Singularity Documentation](https://docs.sylabs.io/guides/latest/user-guide/) – Complete user guide for Singularity

- [Singularity Quick Start](https://docs.sylabs.io/guides/latest/user-guide/quick_start.html) – Getting started with Singularity

- [Building Containers](https://docs.sylabs.io/guides/latest/user-guide/build_a_container.html) – Creating Singularity containers

### NGS-Specific Container Resources

**Pre-built NGS Containers**

- [nf-core Containers](https://nf-co.re/tools/#docker-containers) – Curated containers for common NGS tools

- [GATK Container Images](https://gatk.broadinstitute.org/hc/en-us/articles/360035889991) – Official GATK Docker images

- [Bioconductor Containers](https://www.bioconductor.org/help/docker/) – R/Bioconductor analysis environments

These resources will help you stay current with container technologies and best practices as they continue to evolve. Whether you’re troubleshooting specific issues, learning advanced techniques, or planning large-scale containerized workflows, these references provide authoritative guidance from the broader container and bioinformatics communities.

---

*This tutorial builds upon the ChIP-seq analysis pipeline from our [previous ChIP-seq tutorial](https://ngs101.com/how-to-analyze-chip-seq-data-for-absolute-beginners-part-1-from-fastq-to-peaks-with-homer/). For more NGS analysis tutorials and best practices, visit [NGS101.com](https://ngs101.com).*
