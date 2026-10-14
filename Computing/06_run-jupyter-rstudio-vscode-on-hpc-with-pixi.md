# No More Command-Line Only: Run Jupyter Lab, RStudio, and VS Code Interactively in Your Browser on Any HPC Cluster with Pixi

- **Author:** Thanh-Giang
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** HPC, Scientific Programming
- **Source:** [https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/)

---

**This tutorial is contributed by [Giang Nguyen](https://www.linkedin.com/in/giang-nguyen-761b28190/)**, founder of G Labs, providing consulting, software development, infrastructure engineering, and bioinformatics services to support scalable research and production workflows. He helps teams design, build, and optimize cloud/HPC platforms, develop custom tools and pipelines, and deliver reproducible, production-ready solutions for data-intensive science.

You have your scRNA-seq environment set up with Pixi ([see Part 1](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/)). Now what? Your data lives on an HPC cluster, but your preferred tools — Jupyter Lab, RStudio, VS Code — are graphical, browser-based applications. How do you bridge that gap?

This tutorial solves exactly that problem. You will learn how to install web-based IDEs on a remote machine, keep sessions alive after disconnecting, and access everything securely from your local browser via SSH tunneling.

The tutorial is split into two sections. **Section 1** walks through the complete workflow inside a Docker container that simulates an HPC cluster — a safe sandbox where you can make mistakes without consequences. **Section 2** then deploys the exact same workflow to a real HPC cluster, calling out every difference along the way. By working through Section 1 first, you will already understand the mechanics before applying them to production.

---

Table of Contents

[Toggle](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/)

- [Introduction](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Introduction)

  - [Why Web-Based IDEs Matter for Bioinformatics](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Why_Web-Based_IDEs_Matter_for_Bioinformatics)
  - [The Core Challenge: Ports Are Not Directly Reachable](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#The_Core_Challenge_Ports_Are_Not_Directly_Reachable)
  - [What This Tutorial Covers](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#What_This_Tutorial_Covers)

- [Section 1: Complete Walkthrough in Docker](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Section_1_Complete_Walkthrough_in_Docker)

  - [Step 1.1 — Install Docker](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_11_%E2%80%94_Install_Docker)
  - [Step 1.2 — Launch the SLURM Container](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_12_%E2%80%94_Launch_the_SLURM_Container)
  - [Step 1.3 — Connect via SSH and Set Up Passwordless Auth](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_13_%E2%80%94_Connect_via_SSH_and_Set_Up_Passwordless_Auth)
  - [Step 1.4 — Install Pixi and Global Tools](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_14_%E2%80%94_Install_Pixi_and_Global_Tools)
  - [Step 1.5 — Set Up RStudio Server](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_15_%E2%80%94_Set_Up_RStudio_Server)
  - [Step 1.6 — Open the SSH Tunnel and Access from Your Browser](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_16_%E2%80%94_Open_the_SSH_Tunnel_and_Access_from_Your_Browser)
  - [Step 1.7 — Install R Packages with Pixi](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_17_%E2%80%94_Install_R_Packages_with_Pixi)

- [Section 2: Deploying to a Real HPC Cluster](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Section_2_Deploying_to_a_Real_HPC_Cluster)

  - [What Changes on a Real HPC Cluster](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#What_Changes_on_a_Real_HPC_Cluster)
  - [Step 2.1 — Connect to the HPC Login Node](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_21_%E2%80%94_Connect_to_the_HPC_Login_Node)
  - [Step 2.2 — Install Pixi](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_22_%E2%80%94_Install_Pixi)
  - [Step 2.3 — Install Global Tools](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_23_%E2%80%94_Install_Global_Tools)
  - [Step 2.4 — Set Up RStudio Server](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_24_%E2%80%94_Set_Up_RStudio_Server)
  - [Step 2.5 — Open SSH Tunnels — HPC-Specific Routing](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_25_%E2%80%94_Open_SSH_Tunnels_%E2%80%94_HPC-Specific_Routing)
  - [Step 2.6 — Keeping Sessions Alive with screen](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Step_26_%E2%80%94_Keeping_Sessions_Alive_with_screen)

- [Troubleshooting](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Troubleshooting)

  - [“Too many open files” during singularity exec or singularity pull (Docker only)](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#%E2%80%9CToo_many_open_files%E2%80%9D_during_singularity_exec_or_singularity_pull_Docker_only)
  - [Connection refused when opening http://localhost:PORT](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Connection_refused_when_opening_http_localhost_PORT)
  - [Browser shows “Host Not Resolvable” or “This website is down”](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Browser_shows_%E2%80%9CHost_Not_Resolvable%E2%80%9D_or_%E2%80%9CThis_website_is_down%E2%80%9D)
  - [RStudio — “Could not find details for server user” (HPC only)](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#RStudio_%E2%80%94_%E2%80%9CCould_not_find_details_for_server_user%E2%80%9D_HPC_only)
  - [Jupyter “Token required” and you lost the token](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Jupyter_%E2%80%9CToken_required%E2%80%9D_and_you_lost_the_token)
  - [“libstdc++.so.6: version not found” in RStudio](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#%E2%80%9Clibstdcso6_version_not_found%E2%80%9D_in_RStudio)
  - [RStudio “Unable to connect to service” after login](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#RStudio_%E2%80%9CUnable_to_connect_to_service%E2%80%9D_after_login)
  - [SLURM job killed (OOM)](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#SLURM_job_killed_OOM)
  - [SSH tunnel drops frequently](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#SSH_tunnel_drops_frequently)

- [Recap](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Recap)
- [References](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#References)

  - [Share this:](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Share_this)
  - [Like this:](https://ngs101.com/no-more-command-line-only-run-jupyter-lab-rstudio-and-vs-code-interactively-in-your-browser-on-any-hpc-cluster-with-pixi/#Like_this)

## Introduction

### Why Web-Based IDEs Matter for Bioinformatics

Working with genomics data is inherently interactive. You load data, inspect it, adjust parameters, plot results, and iterate. Command-line workflows are great for production runs, but exploratory analysis — the stage where most scientific decisions are made — benefits enormously from a proper IDE:

**Jupyter Lab** gives you live notebooks where code, plots, and markdown documentation live side by side. Adjusting a UMAP parameter and immediately seeing the result in the same document accelerates understanding.

**RStudio** is the gold standard for R development in bioinformatics. Its integrated data viewer, plot panel, and help browser make working with Seurat objects and DESeq2 results far more productive than the command line.

**Code Server** (VS Code in the browser) is ideal for writing modular scripts, version control with Git, and working across multiple files. Its extension ecosystem brings language intelligence and debugging to remote R and Python workflows.

All three run on a remote machine and are accessed from your local browser — exactly what this tutorial teaches.

### The Core Challenge: Ports Are Not Directly Reachable

When Jupyter Lab starts on a remote machine, it listens on a port (e.g., 8888) of *that machine*. Your laptop cannot reach that port directly because HPC clusters block all inbound traffic except SSH (port 22), and compute nodes sit on a private network.

**SSH tunneling** solves this. It routes browser traffic through the encrypted SSH connection, making a remote web application appear completely local. The tunnel command is:

```
ssh -N -L <local_port>:<target_host>:<remote_port> username@server
```

`-N``-L``local_port``target_host``remote_port`

| Flag | Meaning |
| --- | --- |
|  | Do not open a shell — just hold the tunnel open |
|  | Local port forwarding |
|  | Port on your laptop (e.g., 8888) |
|  | Where to connect on the remote side (covered per-tool below) |
|  | Port the app listens on (e.g., 8888) |

The exact values for `target_host` and the server address differ between the Docker test environment and a real HPC — this is one of the key differences covered in Section 2.

### What This Tutorial Covers

**Prerequisites**:

- Basic familiarity with SSH and the Linux command line

- Docker installed on your local machine (Section 1 — installation covered below)

- An HPC account (Section 2)

- Pixi familiarity helpful but not required — see the [Pixi Part 1 tutorial](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/)

> 

**📚 New to SLURM?** See the [NGS101 SLURM beginner’s guide](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/) for a full introduction to HPC job submission.

---

## Section 1: Complete Walkthrough in Docker

In this section you will run a real SLURM cluster inside a Docker container on your laptop. The container has the SLURM scheduler, a compute daemon, and a pre-created user — everything needed to practice the full workflow before touching a real cluster.

### Step 1.1 — Install Docker

**macOS**:

Download Docker Desktop from [https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/) (choose Apple Silicon or Intel). After launching Docker Desktop:

```bash
docker --version
# Docker version 26.x.x, build ...
```

**Linux (Ubuntu/Debian)**:

```bash
curl -fsSL https://get.docker.com | bash
sudo usermod -aG docker $USER   # run Docker without sudo
# Log out and back in, then verify:
docker --version
```

**Windows**:

Download Docker Desktop from [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/). Enable WSL 2 when prompted, then verify in a terminal:

```bash
docker --version
```

### Step 1.2 — Launch the SLURM Container

```bash
# --privileged is required so Singularity can use Linux user namespaces
# inside the container (not needed on a real HPC where Singularity is
# pre-configured by the sysadmin)
# -p 8081:22 maps your laptop's port 8081 to SSH port 22 inside the container
docker run -it -d --privileged -p 8081:22 nttg8100/river-slurm:1.1.0

# Verify it is running
docker ps
# CONTAINER ID   IMAGE                        STATUS        PORTS
# 2b9964b18c1a   nttg8100/river-slurm:1.1.0   Up 3 seconds  0.0.0.0:8081->22/tcp
```

> 

**ℹ️ Info:** The Docker image is available at: [https://github.com/nttg8100/single-node-slurm-using-docker](https://github.com/nttg8100/single-node-slurm-using-docker)

### Step 1.3 — Connect via SSH and Set Up Passwordless Auth

```bash
# Connect using the pre-created 'river' user (default password: "password")
ssh river@localhost -p 8081

# Verify SLURM is working
srun --mem=1GB --pty bash
squeue
# JOBID PARTITION  NAME   USER  ST  NODELIST
#     2    LocalQ  bash  river   R  localhost
exit   # end the interactive job
```

Set up SSH key authentication so the tunnel command works without a password:

```bash
# Run on your LOCAL machine
ssh-copy-id -p 8081 river@localhost
# Enter "password" once — this is the last time you will need it

# Confirm passwordless login works
ssh river@localhost -p 8081
```

### Step 1.4 — Install Pixi and Global Tools

All remaining steps in this section run **inside the Docker container** (via SSH).

```bash
# Install Pixi — no root required
curl -fsSL https://pixi.sh/install.sh | bash
source ~/.bashrc

which pixi
# /home/river/.pixi/bin/pixi

# Install Jupyter Lab and Code Server globally
# "Global" means available across all Pixi projects, not just one directory
pixi global install --channel conda-forge jupyterlab code-server

which jupyter-lab   # /home/river/.pixi/bin/jupyter-lab
which code-server   # /home/river/.pixi/bin/code-server

# Install screen to keep sessions alive after disconnection
pixi global install screen
```

### Step 1.5 — Set Up RStudio Server

RStudio Server requires deeper OS integration than Jupyter or Code Server — it needs to authenticate users and access shared libraries that vary between Linux distributions. Rather than installing it natively (which requires root), we run the pre-built [Rocker](https://rocker-project.org) RStudio container via **Singularity**.

**What is Singularity?** Singularity (now called Apptainer) is a container runtime designed for HPC. Unlike Docker, it runs containers without root privileges — making it safe and permitted on virtually all shared HPC systems. In this tutorial we install it through Pixi.

**What is Singularity?** Singularity (now called Apptainer) is a container runtime designed for HPC. Unlike Docker, it runs containers without root privileges — making it safe and permitted on virtually all shared HPC systems. In this tutorial we install it through Pixi.

> 

**ℹ️ Note:** If you are new to containers, check out the NGS101 tutorial [Build Once, Run Anywhere: Creating Portable NGS Analysis Environments with Docker](https://ngs101.com/build-once-run-anywhere-creating-portable-ngs-analysis-environments-with-docker/), which covers both Docker and Singularity concepts in detail. Here we use Singularity specifically because it runs without root privileges — making it the only container runtime permitted on most shared HPC clusters.

**Create the RStudio project directory and config files**:

```bash
mkdir -p ~/rstudio-server
cd ~/rstudio-server

# RStudio uses SQLite to store session state (recently opened files, preferences)
cat > database.conf << EOF
provider=sqlite
directory=/var/lib/rstudio-server
EOF

# Session configuration — leave blank to use RStudio defaults
touch rsession.conf

# Runtime directories mounted into the Singularity container
RSTUDIO_WORKSPACE="./workspace"
mkdir -p $RSTUDIO_WORKSPACE/{run,var-lib-rstudio-server,local-share-rstudio}
# run/                    — PID files and sockets for internal RStudio communication
# var-lib-rstudio-server/ — RStudio's internal session database
# local-share-rstudio/    — Per-user preferences and recently opened files
```

**Initialize a Pixi project and install Singularity**:

```bash
pixi init
pixi workspace channel add conda-forge bioconda

# Install Singularity, R, and Python — all managed by Pixi, no root needed
pixi add singularity==3.8.6 r-base python
```

**Verify the Pixi-managed binaries**:

```
R_BIN=$(pixi run which R)
PY_BIN=$(pixi run which python)
echo $R_BIN   # /home/river/rstudio-server/.pixi/envs/default/bin/R
echo $PY_BIN  # /home/river/rstudio-server/.pixi/envs/default/bin/python
```

**Pull the RStudio Server container image**:

```bash
CONTAINER="rstudio:latest"
if [ ! -f $CONTAINER ]; then
    singularity pull $CONTAINER docker://docker.io/rocker/${CONTAINER}
fi
# This downloads the image once and caches it locally as a .sif file
```

**Create the passwd and group files**:

In the Docker container, the user `river` exists in the local `/etc/passwd`, so you can bind it directly. This is the simpler case. The real HPC environment handles this differently — see Section 2.

```bash
# In Docker: the local /etc/passwd already contains 'river', so we can copy it directly
cp /etc/passwd /tmp/custom_passwd
cp /etc/group /tmp/custom_group
```

**Start a screen session and request a compute node**:

In the Docker environment there is only one node, so `srun` keeps you on the same machine — but it still goes through SLURM, which is exactly how a real HPC works. The container has very limited memory, so request only 1 GB here (enough for testing the setup):

```
screen -S rstudio
srun --mem=1GB --pty bash
```

> 

**ℹ️ Note:** On a real HPC cluster, RStudio needs at least 4 GB RAM and 32 GB is recommended for scRNA-seq analysis. The 1 GB used here is only for testing the Docker environment.

**Launch RStudio Server**:

```bash
# Navigate back to the rstudio-server directory and enter the Pixi environment
cd ~/rstudio-server
pixi shell

# Redefine all variables here — they must be set in the current shell session.
# Variables defined before 'pixi shell' are not automatically inherited.
USER=$(whoami)
PORT=8001
RSTUDIO_WORKSPACE="./workspace"
CONTAINER="rstudio:latest"
R_BIN=$(which R)
PY_BIN=$(which python)
# Capture the absolute path of the project directory.
# This is needed so Singularity can resolve the Pixi R binary path,
# which may live outside $HOME (common on HPC systems).
PROJECT_DIR=$(cd ~/rstudio-server && pwd)
export PASSWORD="password"   # change this to something stronger in production

singularity exec \
    # --- Project directory (required when Pixi lives outside $HOME) ---
    --bind $PROJECT_DIR:$PROJECT_DIR \           # makes the Pixi env path visible inside the container
    # --- User identity (bind our resolved passwd/group files) ---
    --bind /tmp/custom_passwd:/etc/passwd:ro \   # user info (read-only)
    --bind /tmp/custom_group:/etc/group:ro \     # group info (read-only)
    # --- RStudio runtime directories ---
    --bind $RSTUDIO_WORKSPACE/run:/run \                                              # PID files and sockets
    --bind $RSTUDIO_WORKSPACE/var-lib-rstudio-server:/var/lib/rstudio-server \        # session database
    --bind /sys/fs/cgroup/:/sys/fs/cgroup/:ro \                                       # required for PAM auth
    # --- Configuration files ---
    --bind ./database.conf:/etc/rstudio/database.conf \   # SQLite database path
    --bind ./rsession.conf:/etc/rstudio/rsession.conf \   # session defaults
    # --- Per-user state ---
    --bind $RSTUDIO_WORKSPACE/local-share-rstudio:/home/rstudio/.local/share/rstudio \  # user preferences
    --bind $HOME:/home/rstudio \                                                          # your home directory
    # --- Environment variables passed into the container ---
    --env RSTUDIO_WHICH_R=$R_BIN \        # use the Pixi-managed R, not the container's R
    --env RETICULATE_PYTHON=$PY_BIN \     # use the Pixi-managed Python for reticulate
    --env USER=$USER \
    --env PASSWORD=$PASSWORD \
    ./$CONTAINER \
    rserver \
    --auth-none=0 \                                              # always require authentication
    --auth-pam-helper-path=/usr/lib/rstudio-server/bin/pam-helper \  # PAM helper inside the container
    --server-user $USER \                                        # run as your user, not root
    --www-address=0.0.0.0 \                                      # listen on all interfaces (required for tunneling)
    --www-port=$PORT \                                           # port your browser will connect to
    --server-working-dir $HOME \                                 # default working directory in RStudio
    --rsession-which-r=$R_BIN \                                  # also set at session level (belt-and-suspenders)
    --rsession-ld-library-path=$CONDA_PREFIX/lib                 # expose Pixi's shared libraries to R
```

When RStudio is running you will see:

```
TTY detected. Printing informational message about logging configuration.
Logging configuration loaded from '/etc/rstudio/logging.conf'. Logging to 'syslog'.
```

**This is normal — not a freeze.** RStudio has started and is silently listening. Leave this terminal open and proceed to the tunnel step.

### Step 1.6 — Open the SSH Tunnel and Access from Your Browser

Open a **new terminal on your local machine** for each tool you want to access. The tunnel stays open as long as the command runs.

**For RStudio** (port 8001):

```bash
# In Docker: the app runs on the same machine you SSH into,
# so the target host is 'localhost' (not a separate compute node)
ssh -N -L 8001:localhost:8001 -p 8081 river@localhost
```

Open your browser and navigate to:

```
http://localhost:8001
```

Log in with:

- **Username**: `river`

- **Password**: the value of `$PASSWORD` (e.g., `password`)

**For Jupyter Lab** (port 8888):

First, inside the container (in a screen session), start Jupyter:

```bash
screen -S jupyter
srun --mem=1GB --pty bash   # use 1GB in Docker (limited resources); use 8GB+ on real HPC

jupyter-lab --no-browser --port=8888
# Prints a URL like: http://localhost:8888/lab?token=abc123...
# Detach: Ctrl+A, then D
```

Then on your local machine:

```
ssh -N -L 8888:localhost:8888 -p 8081 river@localhost
```

Open the URL printed by Jupyter in your browser — but **replace the hostname with `localhost`**:

```bash
# What Jupyter prints (do NOT paste this directly):
http://2b9964b18c1a:8888/lab?token=abc123...

# What to open in your browser (always localhost):
http://localhost:8888/lab?token=abc123...
```

> 

**⚠️ Always use `localhost` in the browser — never the container or compute node hostname.** Your browser does not know what `2b9964b18c1a` or `node042` is — it will try to resolve it as a public website and fail with “Host Not Resolvable.” The SSH tunnel is already routing traffic from your laptop to the remote machine. The hostname belongs only in the tunnel command, never in the browser address bar.

**For Code Server** (VS Code):

```bash
# Inside the container
screen -S vscode
srun --mem=1GB --pty bash   # use 1GB in Docker; use 8GB+ on real HPC
code-server --bind-addr 0.0.0.0:8888
# Detach: Ctrl+A, then D

# On your local machine
ssh -N -L 8888:localhost:8888 -p 8081 river@localhost
```

Open `http://localhost:8888`. Enter the password from `~/.config/code-server/config.yaml`.

### Step 1.7 — Install R Packages with Pixi

```
cd ~/rstudio-server
pixi add bioconductor-deseq2
pixi add r-seurat
pixi add bioconductor-edger
```

Packages are tracked in `pixi.lock` — collaborators who run `pixi install` get the exact same versions. For the full scRNA-seq analysis stack (Harmony, SingleR, etc.) see the [Pixi Part 1 tutorial](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/).

---

## Section 2: Deploying to a Real HPC Cluster

You now understand the full workflow from Section 1. This section deploys the same setup to a real HPC cluster. Most steps are identical — this section focuses on the **differences** and explains why each change is necessary.

### What Changes on a Real HPC Cluster

**SSH port**`-p`**App runs on**`node042`**Tunnel target**`localhost``node042`**User management**`/etc/passwd``getent`**`--privileged`****Pixi cache****Jupyter `--ip`**`--ip=0.0.0.0`

|  | Docker (Section 1) | Real HPC (Section 2) |
| --- | --- | --- |
|  | 8081 | 22 (default, omit ) |
|  | Same machine you SSH into | A separate compute node (e.g., ) |
|  |  | Compute node hostname (e.g., ) |
|  | Local | LDAP/NIS — use |
|  | Required for Docker | Not needed — Singularity pre-configured |
|  | Default location fine | Point to large storage to avoid quota issues |
|  | Not strictly needed | Required () |

### Step 2.1 — Connect to the HPC Login Node

```bash
# Standard SSH — port 22 is the default, no -p needed
ssh jsmith@hpc.university.edu

# You are now on the login node
# Do NOT run analysis here — it is shared by all users
```

Set up SSH key authentication if you have not already:

```bash
# Run on your LOCAL machine
ssh-copy-id jsmith@hpc.university.edu
```

### Step 2.2 — Install Pixi

```bash
curl -fsSL https://pixi.sh/install.sh | bash
source ~/.bashrc

which pixi
# /home/jsmith/.pixi/bin/pixi
```

**Configure the Pixi cache** to avoid filling your home directory quota. Pixi’s cache can grow to 5–20 GB, which will quickly eat into the storage limits most HPC clusters impose on home directories. Point it at a larger storage location — this could be a project directory, lab shared folder, or any path with sufficient space. Check with your HPC documentation or sysadmin if you are unsure where large files should go.

```bash
# Replace /your/large/storage with an appropriate path on your cluster
# e.g., /project/mylab/jsmith or /work/$USER
export PIXI_CACHE_DIR=/your/large/storage/pixi-cache
echo "export PIXI_CACHE_DIR=/your/large/storage/pixi-cache" >> ~/.bashrc
mkdir -p $PIXI_CACHE_DIR
```

### Step 2.3 — Install Global Tools

Identical to Section 1:

```
pixi global install --channel conda-forge jupyterlab code-server
pixi global install screen
```

### Step 2.4 — Set Up RStudio Server

The setup is the same as Section 1 with two important differences: how you request a compute node, and how you create the passwd/group files.

**Request a compute node first** — RStudio needs at least 4 GB RAM; 8 GB is safer:

```bash
# On the login node, start a screen session then request a compute node
screen -S rstudio
srun --mem=8GB --cpus-per-task=4 --pty bash

# Note your compute node hostname — you will need it for the tunnel
hostname
# e.g., node042
```

**Create the RStudio directory and config files** (same as Section 1):

```
mkdir -p ~/rstudio-server && cd ~/rstudio-server

cat > database.conf << EOF
provider=sqlite
directory=/var/lib/rstudio-server
EOF

touch rsession.conf

RSTUDIO_WORKSPACE="./workspace"
mkdir -p $RSTUDIO_WORKSPACE/{run,var-lib-rstudio-server,local-share-rstudio}
```

**Initialize Pixi and install Singularity** (same as Section 1):

```
pixi init
pixi workspace channel add conda-forge bioconda
pixi add singularity==3.8.6 r-base python
```

**Pull the container** (same as Section 1 — do this on the login node which has internet access):

```
CONTAINER="rstudio:latest"
if [ ! -f $CONTAINER ]; then
    singularity pull $CONTAINER docker://docker.io/rocker/${CONTAINER}
fi
```

> 

**💡 Tip:** Pull the container on the login node before your compute job starts. Once downloaded, the `.sif` file is visible on all compute nodes through the shared filesystem.

**Create passwd and group files — HPC-specific step**:

This is the key difference from Docker. Most university HPC clusters manage users through **LDAP or NIS** rather than local files, so your username does not exist in the container’s `/etc/passwd`. Binding it directly fails with:

```
ERROR Could not find details for server user 'jsmith'
ERROR system error 2 (No such file or directory) [description: User not found.]
```

Use `getent` to resolve your user and group information from whatever source (local or LDAP/NIS) your system uses:

```bash
# Run this on the compute node, at the start of each new job
# (/tmp is node-local and does not persist across jobs)
getent passwd $USER > /tmp/custom_passwd
getent group > /tmp/custom_group

# Verify your user was found
cat /tmp/custom_passwd
# jsmith:x:12345:5678:John Smith:/home/jsmith:/bin/bash
```

**Launch RStudio Server** — same `singularity exec` command as Section 1, using the `getent`-resolved files:

```bash
pixi shell   # enter the Pixi environment

# Redefine all variables here — they must be set in the current shell session.
USER=$(whoami)
PORT=8001
RSTUDIO_WORKSPACE="./workspace"
CONTAINER="rstudio:latest"
R_BIN=$(which R)
PY_BIN=$(which python)
# Capture the absolute path of the project directory.
# On HPC, your project may live outside $HOME (e.g., /project/mylab/...).
# Singularity needs this bind so it can resolve the Pixi R binary path.
PROJECT_DIR=$(cd ~/rstudio-server && pwd)
export PASSWORD="your_strong_password_here"

singularity exec \
    --bind $PROJECT_DIR:$PROJECT_DIR \
    --bind /tmp/custom_passwd:/etc/passwd:ro \
    --bind /tmp/custom_group:/etc/group:ro \
    --bind $RSTUDIO_WORKSPACE/run:/run \
    --bind $RSTUDIO_WORKSPACE/var-lib-rstudio-server:/var/lib/rstudio-server \
    --bind /sys/fs/cgroup/:/sys/fs/cgroup/:ro \
    --bind ./database.conf:/etc/rstudio/database.conf \
    --bind ./rsession.conf:/etc/rstudio/rsession.conf \
    --bind $RSTUDIO_WORKSPACE/local-share-rstudio:/home/rstudio/.local/share/rstudio \
    --bind $HOME:/home/rstudio \
    --env RSTUDIO_WHICH_R=$R_BIN \
    --env RETICULATE_PYTHON=$PY_BIN \
    --env USER=$USER \
    --env PASSWORD=$PASSWORD \
    ./$CONTAINER \
    rserver \
    --auth-none=0 \
    --auth-pam-helper-path=/usr/lib/rstudio-server/bin/pam-helper \
    --server-user $USER \
    --www-address=0.0.0.0 \
    --www-port=$PORT \
    --server-working-dir $HOME \
    --rsession-which-r=$R_BIN \
    --rsession-ld-library-path=$CONDA_PREFIX/lib
```

When you see the `syslog` message, detach from screen: `Ctrl+A`, then `D`.

### Step 2.5 — Open SSH Tunnels — HPC-Specific Routing

This is the most important difference from Docker. On a real HPC cluster, your job runs on a **compute node** (e.g., `node042`) — a different machine from the login node you SSH into. The tunnel must route *through* the login node to reach the compute node.

**Find your compute node hostname** before creating the tunnel:

```bash
# Option 1: check the shell prompt after srun
# (base) [jsmith@node042 ~]$  ← the hostname is here

# Option 2: run hostname inside the job
hostname
# node042

# Option 3: check from the login node
squeue -u $USER
# JOBID  PARTITION  NAME  USER    ST  NODELIST
# 12345  compute    bash  jsmith   R  node042
```

**For RStudio** (port 8001, running on `node042`):

```bash
# On your LOCAL machine — route through the login node to the compute node
ssh -N -L 8001:node042:8001 jsmith@hpc.university.edu
```

Open `http://localhost:8001`. Log in with your Linux username and `$PASSWORD`.

**For Jupyter Lab** (port 8888):

```bash
# On the login node: start a screen session and request a compute node
screen -S jupyter
srun --mem=8GB --cpus-per-task=4 --pty bash

hostname   # e.g., node042

# --ip=0.0.0.0 is required on HPC — without it Jupyter only listens on the
# compute node's loopback interface, which the SSH tunnel cannot reach
jupyter-lab --no-browser --ip=0.0.0.0 --port=8888 --ServerApp.allow_remote_access=True
# Prints: http://node042:8888/lab?token=abc123...
# Detach: Ctrl+A, then D
```

```bash
# On your LOCAL machine
ssh -N -L 8888:node042:8888 jsmith@hpc.university.edu
```

Open in your browser — always use `localhost`, never the compute node name:

```
http://localhost:8888/lab?token=abc123...
```

> 

**⚠️ Why `--ip=0.0.0.0` is required on HPC but not in Docker:** In Docker, the container has a single network interface and Jupyter’s default loopback binding happens to be reachable. On a real HPC compute node, the tunnel enters via the node’s network interface — not loopback. Without `--ip=0.0.0.0`, Jupyter refuses these tunnel connections and you get `Connection refused`. Always include it on HPC.

**For Code Server** (port 8888):

```bash
# On the compute node (inside screen + srun)
code-server --bind-addr 0.0.0.0:8888
# Detach: Ctrl+A, then D

# On your LOCAL machine
ssh -N -L 8888:node042:8888 jsmith@hpc.university.edu
```

Open `http://localhost:8888` and enter the password from `~/.config/code-server/config.yaml`.

### Step 2.6 — Keeping Sessions Alive with screen

`screen` is the same on both Docker and HPC. The key commands:

```bash
screen -S name     # create a named session
# Ctrl+A, then D   # detach (leave running)
screen -ls         # list sessions
screen -r name     # reattach
```

On HPC, always start `screen` on the **login node**, then run `srun` inside the screen session. That way if your SSH connection drops, the screen session (and your SLURM job) keeps running:

```bash
# On the login node
screen -S rstudio
# Inside screen:
srun --mem=8GB --cpus-per-task=4 --pty bash
# Now start your tool...
# Ctrl+A, D to detach
```

> 

**💡 Tip:** `tmux` is a modern alternative with split panes and better scrolling. Install with `pixi global install tmux`. Detach with `Ctrl+B, D`, reattach with `tmux attach -t name`.

---

## Troubleshooting

### “Too many open files” during `singularity exec` or `singularity pull` (Docker only)

*Cause*: The Docker container’s default file descriptor limit (usually 1024) is too low for Singularity to extract large images like RStudio. This does not occur on real HPC clusters where sysadmins set high limits system-wide.

*Fix*: Raise the limit and clean up any partial extraction:

```bash
ulimit -n 65536
rm -rf /tmp/singularity-* ~/.singularity/cache
# Then retry the singularity exec command
```

### `Connection refused` when opening `http://localhost:PORT`

The tool is not running, or the tunnel is misconfigured. Check in order:

```bash
# 1. Reattach to screen and confirm the tool is still running
screen -r jupyter   # or rstudio, vscode

# 2. On HPC: confirm Jupyter was started with --ip=0.0.0.0
#    (without it only loopback is bound and the tunnel cannot reach it)
ss -tlnp | grep 8888
# Correct:   0.0.0.0:8888
# Incorrect: 127.0.0.1:8888

# 3. On HPC: confirm your tunnel uses the compute node name, not localhost
# Wrong:  ssh -N -L 8888:localhost:8888 jsmith@hpc.university.edu
# Right:  ssh -N -L 8888:node042:8888 jsmith@hpc.university.edu
```

### Browser shows “Host Not Resolvable” or “This website is down”

You pasted the compute-node URL directly into the browser (e.g., `http://node042:8888/lab?token=...`). Replace the hostname with `localhost`:

```
http://localhost:8888/lab?token=abc123...
```

### RStudio — “Could not find details for server user” (HPC only)

Your cluster uses LDAP/NIS. Use `getent` instead of copying `/etc/passwd` directly:

```bash
getent passwd $USER > /tmp/custom_passwd
getent group > /tmp/custom_group
# Use these with --bind /tmp/custom_passwd:/etc/passwd:ro
#               and --bind /tmp/custom_group:/etc/group:ro
```

### Jupyter “Token required” and you lost the token

```bash
# Reattach to screen to see the original output
screen -r jupyter

# Or restart Jupyter — a new token will be printed
jupyter-lab --no-browser --ip=0.0.0.0 --port=8888 --ServerApp.allow_remote_access=True
```

### “libstdc++.so.6: version not found” in RStudio

```bash
pixi shell
echo $CONDA_PREFIX
ls $CONDA_PREFIX/lib/libstdc++*
# Verify the library exists
# The --rsession-ld-library-path=$CONDA_PREFIX/lib flag should handle this automatically
```

### RStudio “Unable to connect to service” after login

```bash
cd ~/rstudio-server
rm -rf workspace/
mkdir -p workspace/{run,var-lib-rstudio-server,local-share-rstudio}
# Restart RStudio Server
```

### SLURM job killed (OOM)

```bash
sacct -j YOUR_JOB_ID --format=JobID,MaxRSS,Elapsed
# Increase the memory request
srun --mem=64G --cpus-per-task=8 --time=4:00:00 --pty bash
```

### SSH tunnel drops frequently

Add to `~/.ssh/config` on your **local machine**:

```
Host hpc.university.edu
    ServerAliveInterval 60
    ServerAliveCountMax 10
```

---

## Recap

**Section 1 (Docker)** walked through the complete workflow in a safe local environment:

- SLURM container with `--privileged` for Singularity support

- Pixi installed globally for Jupyter Lab, Code Server, and screen

- Singularity installed via Pixi for RStudio Server

- Local `/etc/passwd` copied directly (users are local in Docker)

- Tunnel target is `localhost` (single machine, no separate compute nodes)

- Jupyter works without `--ip=0.0.0.0` in this environment

**Section 2 (HPC)** applied the same workflow to a real cluster, with these key differences:

- No `-p` needed for SSH (port 22 is the default)

- `srun --mem=8GB` required before launching any tool — never run on the login node

- Pixi cache configured to large storage, not home directory

- `getent passwd $USER` and `getent group` replace direct `/etc/passwd` and `/etc/group` binds (LDAP/NIS user management)

- Tunnel target is the compute node name, not `localhost` (e.g., `ssh -N -L 8001:node042:8001 jsmith@hpc.university.edu`)

- Jupyter requires `--ip=0.0.0.0` so the tunnel can reach it across the network

**What to read next**:

- **[Pixi Part 1: Setting up the scRNA-seq environment](https://ngs101.com/setting-up-single-cell-rna-seq-analysis-environment-with-pixi-10x-faster-setup-zero-version-conflicts/)** — the scRNA-seq analysis environments this infrastructure is designed to run

- **[NGS101 SLURM Beginner’s Guide](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/)** — batch job submission and HPC resource management

---

## References

1. **Pixi Documentation**. prefix.dev. [https://pixi.sh](https://pixi.sh) (2026)

2. **Jupyter Lab Documentation**. Project Jupyter. [https://jupyterlab.readthedocs.io](https://jupyterlab.readthedocs.io) (2026)

3. **Code Server Documentation**. cdr. [https://github.com/coder/code-server](https://github.com/coder/code-server) (2026)

4. **Rocker Project: R Containers**. [https://rocker-project.org](https://rocker-project.org) (2026)

5. **Singularity/Apptainer User Guide**. Apptainer. [https://apptainer.org/docs/user/main/](https://apptainer.org/docs/user/main/) (2026)

6. **Docker Documentation**. Docker Inc. [https://docs.docker.com](https://docs.docker.com) (2026)

7. **SLURM Workload Manager Documentation**. SchedMD. [https://slurm.schedmd.com/documentation.html](https://slurm.schedmd.com/documentation.html) (2026)

8. **GNU Screen Manual**. GNU Project. [https://www.gnu.org/software/screen/manual/](https://www.gnu.org/software/screen/manual/) (2026)

9. **tmux Documentation**. tmux. [https://github.com/tmux/tmux/wiki](https://github.com/tmux/tmux/wiki) (2026)

10. **OpenSSH Manual: Port Forwarding**. OpenBSD. [https://man.openbsd.org/ssh](https://man.openbsd.org/ssh) (2026)

11. **RIVER Infrastructure**. Giang Nguyen. [https://github.com/nttg8100/single-node-slurm-using-docker](https://github.com/nttg8100/single-node-slurm-using-docker) (2026)

12. **Hao Y, Stuart T, Kowalski MH, et al.** Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nat Biotechnol.* 2024;42(2):293-304. doi:10.1038/s41587-023-01767-y

13. **NGS101 scRNA-seq Tutorial Series**. [https://ngs101.com](https://ngs101.com) (2026)

14. **Giang Nguyen — RIVER Infrastructure**. [https://gianglabs.github.io/glabs-docs-blogs/docs/resources/administration/ssh-remote-server](https://gianglabs.github.io/glabs-docs-blogs/docs/resources/administration/ssh-remote-server) (2026)

15. **Beaulieu-Jones BK, Greene CS.** Reproducibility of computational workflows is automated using continuous analysis. *Nat Biotechnol.* 2017;35(4):342-346. doi:10.1038/nbt.3780
