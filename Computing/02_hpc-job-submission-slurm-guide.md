# High Performance Computing (HPC) Job Submission Systems: A Beginner's Guide to Slurm

- **Author:** Lei
- **Publication:** [NGS101](https://ngs101.com)
- **Date:** July 26, 2026
- **Categories:** HPC, Scientific Programming
- **Source:** [https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/)

---

Table of Contents

[Toggle](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/)

- [Introduction: Understanding High Performance Computing](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Introduction_Understanding_High_Performance_Computing)

  - [What is HPC and Why Do We Need It?](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#What_is_HPC_and_Why_Do_We_Need_It)

- [Job Submission Systems: The Traffic Controllers of HPC](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Job_Submission_Systems_The_Traffic_Controllers_of_HPC)

  - [What Are Job Submission Systems?](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#What_Are_Job_Submission_Systems)
  - [Common Terminology](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Common_Terminology)
  - [Popular Job Submission Systems in the US](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Popular_Job_Submission_Systems_in_the_US)
  - [How Job Submission Systems Work](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#How_Job_Submission_Systems_Work)
  - [Why We Need Job Submission Systems](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Why_We_Need_Job_Submission_Systems)

- [Slurm: A Beginner-Friendly Guide to a Popular HPC Job Scheduler](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Slurm_A_Beginner-Friendly_Guide_to_a_Popular_HPC_Job_Scheduler)

  - [What is Slurm?](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#What_is_Slurm)
  - [Slurm Architecture: The Basic Components](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Slurm_Architecture_The_Basic_Components)
  - [Basic Slurm Workflow](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Basic_Slurm_Workflow)

- [Common Slurm Commands with Examples and Outputs](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Common_Slurm_Commands_with_Examples_and_Outputs)

  - [Job Submission and Management](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Job_Submission_and_Management)
  - [Job Monitoring and Information](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Job_Monitoring_and_Information)

- [Writing Your First Slurm Job Script](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Writing_Your_First_Slurm_Job_Script)

  - [Submitting Your Job](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Submitting_Your_Job)
  - [Checking Job Status](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Checking_Job_Status)
  - [Viewing Results](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Viewing_Results)

- [Real-World Example: Running a RNA-seq quantification with Conda](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Real-World_Example_Running_a_RNA-seq_quantification_with_Conda)
- [Advanced Slurm Features Explained Simply](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Advanced_Slurm_Features_Explained_Simply)

  - [Job Arrays: Running Many Similar Jobs at Once](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Job_Arrays_Running_Many_Similar_Jobs_at_Once)
  - [Job Dependencies: Making Jobs Wait for Other Jobs](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Job_Dependencies_Making_Jobs_Wait_for_Other_Jobs)
  - [Using Email Notifications](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Using_Email_Notifications)

- [Best Practices and Tips for Beginners](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Best_Practices_and_Tips_for_Beginners)

  - [Estimating Resource Needs](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Estimating_Resource_Needs)
  - [Common Mistakes to Avoid](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Common_Mistakes_to_Avoid)
  - [Practical Tips for Success](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Practical_Tips_for_Success)

- [Example Workflow: From Login to Results](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Example_Workflow_From_Login_to_Results)

  - [1. Log in to the HPC system](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#1_Log_in_to_the_HPC_system)
  - [2. Create a directory for your project](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#2_Create_a_directory_for_your_project)
  - [3. Transfer your data and scripts](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#3_Transfer_your_data_and_scripts)
  - [4. Create a job script](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#4_Create_a_job_script)
  - [5. Submit the job](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#5_Submit_the_job)
  - [6. Monitor the job](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#6_Monitor_the_job)
  - [7. Check the results when the job completes](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#7_Check_the_results_when_the_job_completes)

- [Troubleshooting Common Issues](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Troubleshooting_Common_Issues)

  - [“Job Failed” or “Out of Memory”](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#%E2%80%9CJob_Failed%E2%80%9D_or_%E2%80%9COut_of_Memory%E2%80%9D)
  - [“Time Limit Exceeded”](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#%E2%80%9CTime_Limit_Exceeded%E2%80%9D)
  - [“Command Not Found”](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#%E2%80%9CCommand_Not_Found%E2%80%9D)
  - [Job Stuck in Pending Status](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Job_Stuck_in_Pending_Status)

- [Job Efficiency and Optimization](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Job_Efficiency_and_Optimization)

  - [Understanding CPU and Memory Efficiency](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Understanding_CPU_and_Memory_Efficiency)
  - [Running Multiple Tasks Within a Job](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Running_Multiple_Tasks_Within_a_Job)

- [Using Slurm with Common Software and Tools](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Using_Slurm_with_Common_Software_and_Tools)

  - [Running R Scripts with Slurm](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Running_R_Scripts_with_Slurm)
  - [Running MATLAB Scripts with Slurm](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Running_MATLAB_Scripts_with_Slurm)

- [Example of a Complete Workflow with Advanced Features](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Example_of_a_Complete_Workflow_with_Advanced_Features)

  - [1. Preparing the data (first job)](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#1_Preparing_the_data_first_job)
  - [2. Running the analysis (second job, depends on first)](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#2_Running_the_analysis_second_job_depends_on_first)
  - [3. Submitting the workflow with dependencies](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#3_Submitting_the_workflow_with_dependencies)

- [Common HPC Terms and Concepts Explained](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Common_HPC_Terms_and_Concepts_Explained)

  - [Parallel Computing Basics](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Parallel_Computing_Basics)
  - [Storage and File Systems](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Storage_and_File_Systems)

- [Environment Modules vs. Conda](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Environment_Modules_vs_Conda)

  - [Environment Modules](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Environment_Modules)
  - [Conda Environments](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Conda_Environments)

- [Requesting Special Hardware](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Requesting_Special_Hardware)

  - [Requesting GPUs](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Requesting_GPUs)
  - [Requesting High-Memory Nodes](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Requesting_High-Memory_Nodes)

- [Learning from the HPC Community](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Learning_from_the_HPC_Community)

  - [HPC Documentation](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#HPC_Documentation)
  - [Asking for Help](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Asking_for_Help)

- [Conclusion: Your HPC Journey](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Conclusion_Your_HPC_Journey)
- [Additional Resources](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Additional_Resources)

  - [Share this:](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Share_this)
  - [Like this:](https://ngs101.com/high-performance-computing-hpc-job-submission-systems-a-beginners-guide-to-slurm/#Like_this)

## Introduction: Understanding High Performance Computing

Have you ever tried to run a program on your computer, only to find that it takes hours or even days to complete? Or perhaps you needed to analyze a huge dataset that wouldn’t even fit in your computer’s memory? These are the kinds of problems that High Performance Computing (HPC) can solve.

### What is HPC and Why Do We Need It?

High Performance Computing (HPC) is like having a supercharged computer – or more accurately, many computers working together as a team. Imagine asking 100 people to each solve one math problem versus asking one person to solve 100 math problems. The team approach will finish much faster! This is the basic idea behind HPC.

An HPC system (often called a “cluster” or “supercomputer”) is made up of multiple computers (called “nodes”) connected together with special high-speed networks. Each node typically has multiple processors (CPUs), memory (RAM), and storage, allowing them to work together on complex problems.

We need HPC when:

- **Our regular computer is too slow**: When calculations would take weeks or months on a regular laptop or desktop

- **We have massive amounts of data**: Like analyzing satellite images, genetic sequences, or climate data

- **We need answers quickly**: For time-sensitive problems like weather forecasting

- **We’re solving complex problems**: Such as analyzing large NGS datasets

In simple terms, HPC lets us solve bigger problems faster than we could with a regular computer.

## Job Submission Systems: The Traffic Controllers of HPC

### What Are Job Submission Systems?

Imagine a busy airport where many planes want to take off and land. Air traffic controllers make sure each plane gets the runway when it needs it, and that everything happens safely and efficiently. In the world of HPC, job submission systems (also called workload managers or job schedulers) are like those air traffic controllers.

A job submission system is a specialized software that manages who gets to use the HPC resources, when, and how much of it they get. Without these systems, it would be chaos – like an airport without air traffic control!

These systems perform several important functions:

1. **Resource allocation**: Deciding which users get which compute nodes and when

2. **Job scheduling**: Determining the order in which jobs will run

3. **Queue management**: Organizing waiting jobs based on priority and requirements

4. **Monitoring**: Keeping track of what’s running and how resources are being used

5. **Accounting**: Recording who used what resources for how long

### Common Terminology

Before we dive deeper, let’s understand some key terms that you’ll encounter when working with HPC systems:

- **Job**: A computational task that you want to run on the HPC system. This could be a simulation, data analysis, or any other type of computation.

- **Node**: A single computer in the HPC cluster, typically with multiple CPUs.

- **Core**: An individual processing unit (part of a CPU). Modern CPUs often have multiple cores (4, 8, 16, or more).

- **Task**: A process that runs on one or more cores.

- **Partition/Queue**: A group of nodes with similar characteristics, often set up for specific types of jobs.

- **Walltime**: The maximum amount of time your job is allowed to run.

- **Memory**: The RAM (Random Access Memory) available for your job.

- **Job Script**: A text file containing instructions for the job scheduler about how to run your program.

### Popular Job Submission Systems in the US

Several job submission systems are widely used across universities, research centers, and companies in the United States:

1. **Slurm (Simple Linux Utility for Resource Management)**: One of the most widely used systems today, Slurm is free, open-source software that powers many of the world’s supercomputers. We’ll focus on Slurm in this tutorial.

2. **PBS (Portable Batch System)** and its variants: These older but reliable systems are still common in many institutions.

3. **LSF (Load Sharing Facility)**: A commercial product often used in business and industry.

4. **SGE (Sun Grid Engine)**: Another older system still found in some computing centers.

5. **Cobalt**: Used primarily at some national laboratories.

### How Job Submission Systems Work

Let’s break down how these systems work in simple terms:

1. **You write a job script**: This is a text file where you tell the system what program you want to run and what resources you need (like how many computers, how much memory, and how much time).

2. **You submit your job**: Using a special command, you send your job script to the job submission system.

3. **Your job waits in a queue**: If the resources you requested aren’t immediately available, your job waits in line.

4. **Your job runs when resources are available**: The system finds suitable computers for your job and runs your program.

5. **Your job completes**: When your program finishes (or runs out of time), the system frees up the resources for someone else to use.

6. **You collect your results**: You can then look at the output files your program created.

### Why We Need Job Submission Systems

You might wonder: “Why can’t I just log into the supercomputer and run my programs directly?” There are several important reasons why job submission systems are necessary:

1. **Fair sharing**: Most HPC systems are shared by dozens, hundreds, or even thousands of users. Without a job system, a few users might use all the resources, leaving none for others.

2. **Efficient use of resources**: These systems keep the expensive hardware busy by filling in gaps and running jobs in the most efficient order.

3. **Convenience**: You don’t have to wait around for your job to run. You can submit it and come back later when it’s done.

4. **Handling complex requirements**: Some jobs need specific types of hardware (like GPUs or large memory nodes). The job system matches jobs to the right hardware.

5. **Automated workflow**: You can set up chains of jobs that run one after another automatically.

## Slurm: A Beginner-Friendly Guide to a Popular HPC Job Scheduler

### What is Slurm?

![](https://i0.wp.com/ngs101.com/wp-content/uploads/2025/03/arch.gif?resize=811%2C681&ssl=1)

Slurm (Simple Linux Utility for Resource Management) is one of the most widely used job scheduling systems for HPC clusters. Developed at Lawrence Livermore National Laboratory, it’s now used on many of the fastest supercomputers in the world.

Think of Slurm as an intelligent assistant that:

- Takes your job requests

- Finds suitable computers to run them on

- Makes sure your programs get the resources they need

- Keeps track of everything that’s running

- Makes efficient use of the entire system

### Slurm Architecture: The Basic Components

Slurm has several components that work together:

1. **slurmctld** (the central controller): This is the “brain” of Slurm that makes all the scheduling decisions. It’s like the air traffic control tower.

2. **slurmd** (the compute node daemon): This runs on each compute node and carries out the instructions from slurmctld. Think of it like the pilot who follows the air traffic controller’s directions.

3. **slurmdbd** (the database daemon): This optional component keeps records of jobs and usage for accounting purposes.

4. **Client commands**: These are the tools you’ll use to interact with Slurm, like `sbatch`, `squeue`, and `scancel`.

Don’t worry about remembering all these details – just understanding that Slurm has a central controller that talks to programs running on each computer in the cluster is enough to get started.

### Basic Slurm Workflow

The typical workflow when using Slurm consists of:

1. Creating a job script (a simple text file with instructions)

2. Submitting the job to Slurm

3. Monitoring the status of your job

4. Reviewing the results when your job completes

Let’s look at each of these steps in more detail.

## Common Slurm Commands with Examples and Outputs

### Job Submission and Management

#### 1. `sbatch`: Submit a batch job script

This command sends your job script to Slurm for processing.

**Usage:**

```
sbatch myjob.sh
```

**Example output:**

```
Submitted batch job 12345
```

This output means Slurm has accepted your job and assigned it the ID number 12345. You’ll use this ID to check on your job or cancel it if needed.

#### 2. `srun`: Run a program directly

While `sbatch` is used for submitting job scripts, `srun` can be used to run programs directly, either within a job script or for interactive work.

**Usage within a job script:**

```
srun ./my_program
```

**Usage for interactive jobs:**

```
srun --nodes=1 --ntasks=4 --time=01:00:00 --pty bash
```

**Example output for interactive use:**

```
srun: job 12346 queued and waiting for resources
srun: job 12346 has been allocated resources
[user@node005 ~]$
```

The above output shows that Slurm found a computer for you to use, and now you have a terminal (command prompt) on that computer. The `--pty bash` part means you want an interactive shell session.

#### 3. `scancel`: Cancel a running or pending job

If you need to stop a job before it completes, use this command.

**Usage:**

```
scancel JOBID
```

**Example output:**There’s usually no output unless there’s an error, which means the command succeeded.

To cancel all of your jobs:

```
scancel -u yourusername
```

#### 4. `salloc`: Allocate resources for interactive use

Similar to `srun` with the `--pty` option, this reserves resources for interactive use.

**Usage:**

```
salloc --nodes=1 --ntasks=4 --time=01:00:00
```

**Example output:**

```
salloc: Granted job allocation 12347
```

After seeing this message, you can use `srun` to run programs on the allocated resources.

### Job Monitoring and Information

#### 1. `squeue`: View information about jobs in the queue

This shows the status of jobs that are running or waiting to run.

**Usage:**

```
squeue
```

**Example output:**

```
  JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
  12345    normal  test_job  user123  R       0:45      1 node005
  12348    normal  analysis  user456 PD       0:00      2 (Resources)
  12350     debug     bash   user789  R       2:10      1 node008
```

In this output:

- `JOBID`: The unique job identifier

- `PARTITION`: Which group of nodes the job is running on (similar to a queue)

- `NAME`: The name of the job (from the job script)

- `USER`: Who submitted the job

- `ST`: Status (R = running, PD = pending, and others)

- `TIME`: How long the job has been running

- `NODES`: How many nodes the job is using

- `NODELIST(REASON)`: Which specific nodes, or if pending, why it’s waiting

To see only your own jobs:

```
squeue -u yourusername
```

**Example output:**

```
  JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
  12345    normal  test_job  user123  R       0:45      1 node005
```

#### 2. `sacct`: Display accounting information about jobs

This shows more detailed information about your jobs, including completed ones.

**Usage:**

```
sacct -j 12345
```

**Example output:**

```
       JobID    JobName  Partition    Account  AllocCPUS      State ExitCode 
------------ ---------- ---------- ---------- ---------- ---------- -------- 
12345          test_job      normal    project         16  COMPLETED      0:0 
12345.batch       batch                project         16  COMPLETED      0:0 
12345.0            srun                project         16  COMPLETED      0:0 
```

This shows that job 12345 ran successfully (`COMPLETED` with exit code `0:0`).

#### 3. `sinfo`: View information about Slurm nodes and partitions

This shows the status of the compute nodes and partitions (groups of nodes).

**Usage:**

```
sinfo
```

**Example output:**

```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
debug        up    2:00:00      4   idle node[001-004]
normal       up 1-00:00:00     20    mix node[005-024]
gpu          up 3-00:00:00      8   idle node[025-032]
```

This output shows:

- Three partitions: debug, normal, and gpu

- Their availability (up means available)

- Time limits (maximum job duration)

- Number of nodes in each partition

- Current state (idle = not being used, mix = some CPUs in use)

- Names of the nodes

#### 4. `scontrol`: View detailed information

This command lets you see detailed information about jobs, nodes, and other Slurm components.

**Usage:**

```
scontrol show job 12345
```

**Example output:**

```
JobId=12345 JobName=test_job
   UserId=user123(1001) GroupId=users(1001) MCS_label=N/A
   Priority=4294901244 Nice=0 Account=project QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:00:45 TimeLimit=01:00:00 TimeMin=N/A
   SubmitTime=2023-06-01T10:00:00 EligibleTime=2023-06-01T10:00:00
   StartTime=2023-06-01T10:01:00 EndTime=2023-06-01T11:01:00 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2023-06-01T10:00:00
   Partition=normal AllocNode:Sid=login01:12345
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=node005
   BatchHost=node005
   NumNodes=1 NumCPUs=16 NumTasks=4 CPUs/Task=4 ReqB:S:C:T=0:0:*:*
   TRES=cpu=16,mem=64G,node=1,billing=16
   Socks/Node=* NtasksPerN:B:S:C=4:0:*:* CoreSpec=*
   MinCPUsNode=4 MinMemoryNode=64G MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
```

This provides very detailed information about the job, including who submitted it, when it started, what resources it’s using, and much more.

## Writing Your First Slurm Job Script

A Slurm job script is a text file that tells Slurm what resources you need and what commands to run. Let’s break down a simple example:

```bash
#!/bin/bash
#SBATCH --job-name=hello_world    # A name for your job
#SBATCH --output=hello_%j.out     # Output file (%j expands to job ID)
#SBATCH --error=hello_%j.err      # Error file
#SBATCH --nodes=1                 # Request 1 node
#SBATCH --ntasks=1                # Run a single task
#SBATCH --mem=1G                  # Request 1 GB of memory
#SBATCH --time=00:05:00           # Set a 5-minute time limit
#SBATCH --partition=debug         # Use the debug partition

# Print some information about the job
echo "Running on host: $(hostname)"
echo "Starting at: $(date)"
echo "Directory: $(pwd)"

# Run the program
echo "Hello, World!"

# Print the end time
echo "Finished at: $(date)"
```

Let’s explain this line by line:

- `#!/bin/bash`: This tells the system to use the bash shell to interpret this script.

- `#SBATCH --job-name=hello_world`: This sets the name of your job to “hello_world”. This name will appear in the output of commands like `squeue`.

- `#SBATCH --output=hello_%j.out`: This tells Slurm to save the standard output (what would normally be printed to the screen) to a file named “hello_[job_id].out”. The `%j` is replaced with your actual job ID.

- `#SBATCH --error=hello_%j.err`: Similar to above, but for error messages.

- `#SBATCH --nodes=1`: This requests one compute node.

- `#SBATCH --ntasks=1`: This specifies that you want to run one task.

- `#SBATCH --mem=1G`: This requests 1 gigabyte of memory (RAM).

- `#SBATCH --time=00:05:00`: This sets a time limit of 5 minutes for your job. If it runs longer, it will be automatically terminated.

- `#SBATCH --partition=debug`: This specifies that you want to use the “debug” partition, which is often configured for short, quick jobs.

After the `#SBATCH` directives, the script includes commands that will be executed when the job runs:

- `echo "Running on host: $(hostname)"`: Prints the name of the compute node running your job.

- `echo "Starting at: $(date)"`: Prints the date and time when your job starts.

- `echo "Directory: $(pwd)"`: Prints the current working directory.

- `echo "Hello, World!"`: A simple command that prints “Hello, World!” (this would be replaced with your actual program).

- `echo "Finished at: $(date)"`: Prints the date and time when your job finishes.

### Submitting Your Job

To submit this job script to Slurm, save it to a file (e.g., `hello.sh`) and use the `sbatch` command:

```
sbatch hello.sh
```

You should see output like:

```
Submitted batch job 12345
```

### Checking Job Status

To check the status of your job:

```
squeue -u yourusername
```

### Viewing Results

After your job completes, you can look at the output:

```
cat hello_12345.out
```

Example output:

```
Running on host: node005
Starting at: Thu Jun 1 10:05:00 PDT 2023
Directory: /home/yourusername
Hello, World!
Finished at: Thu Jun 1 10:05:03 PDT 2023
```

## Real-World Example: Running a RNA-seq quantification with Conda

Here’s a more realistic example of a Slurm script that runs the RNA-seq quantification pipeline from our previous [FASTQ to Counts tutorial](https://ngs101.com/how-to-analyze-rnaseq-data-for-absolute-beginners-part-2-from-fastq-to-counts-best-practices/) using a conda environment:

```bash
#!/bin/bash
#SBATCH --job-name=python_analysis   # Name of the job
#SBATCH --output=analysis_%j.out     # Name of output file
#SBATCH --error=analysis_%j.err      # Name of error file
#SBATCH --nodes=1                    # Request 1 node
#SBATCH --ntasks=1                   # Run a single task
#SBATCH --cpus-per-task=8            # Request 8 CPU cores
#SBATCH --mem=64G                    # Request 64 GB of memory
#SBATCH --time=04:00:00              # Set a 4-hour time limit
#SBATCH --partition=normal           # Use the normal partition

# Print job information
echo "Job ID: $SLURM_JOB_ID"
echo "Running on host: $(hostname)"
echo "Starting at: $(date)"
echo "Current working directory: $(pwd)"

# Load Miniconda/Miniforge module if it's installed on your HPC system
# If your HPC system has module support, you might need something like:
# module load miniforge3

# If not using modules, specify the path to your Miniforge installation
# Adjust this path to match your Miniforge installation location
MINIFORGE_DIR=$HOME/miniforge3

# Initialize conda for bash
# This sets up the conda command for use in this script
source $MINIFORGE_DIR/etc/profile.d/conda.sh

# Activate your conda environment
# Replace "rnaseq_env" with the name of your environment
conda activate rnaseq_env

# Trim the adaptors
trim_galore --fastqc --paired --cores 8 \
  ~/Tutorials/RNAseq/GSE259357/raw/SRR28119110_R1_001.fastq.gz \
  ~/Tutorials/RNAseq/GSE259357/raw/SRR28119110_R2_001.fastq.gz \
  -o ~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110

# Align to the Reference Genome
STAR --genomeDir ~/Tutorials/RNAseq/star_index_mm10 \
  --runThreadN 20 --readFilesIn \ 
~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110/SRR28119110_R1_001_val_1.fq.gz \ 
~/Tutorials/RNAseq/GSE259357/trimmed/SRR28119110/SRR28119110_R2_001_val_2.fq.gz \
  --outSAMtype BAM SortedByCoordinate \
  --outSAMunmapped Within \
  --outSAMattributes Standard \
  --readFilesCommand zcat \
  --outFileNamePrefix ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110

# Quantify gene expression
featureCounts -T 8 -t exon -g gene_name -s 0 \
  -a ~/Tutorials/RNAseq/GTF/gencode.vM25.annotation.gtf \
  -o ~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110/SRR28119110_featureCounts_gene.txt \ 
~/Tutorials/RNAseq/GSE259357/aligned/SRR28119110/SRR28119110_trimmedAligned.sortedByCoord.out.bam

# Print completion time
echo "Job completed at: $(date)"
```

Let’s break down the key parts related to conda:

1. **Path to Miniforge**: `MINIFORGE_DIR=$HOME/miniforge3` – This sets the location of your Miniforge installation. Adjust this to match where Miniforge is installed on your system.

2. **Source conda.sh**: `source $MINIFORGE_DIR/etc/profile.d/conda.sh` – This initializes conda for use in your script.

3. **Activate environment**: `conda activate rnaseq_env` – This activates your specific conda environment (replace “rnaseq_env” with your environment’s name).

4. **Run the RNA-seq quantification pipeline**: The script includes commands to quantify gene expression from FASTQ files.

## Advanced Slurm Features Explained Simply

### Job Arrays: Running Many Similar Jobs at Once

Job arrays let you run the same script many times with different parameters. It’s like photocopying your job script, but each copy has a different ID number that you can use to process different inputs.

**Example:**

```bash
#!/bin/bash
#SBATCH --job-name=array_job
#SBATCH --output=array_%A_%a.out   # %A is the job ID, %a is the array index
#SBATCH --error=array_%A_%a.err
#SBATCH --array=1-10               # Run 10 jobs with indices 1 through 10
#SBATCH --time=00:30:00
#SBATCH --mem=2G

echo "This is array task $SLURM_ARRAY_TASK_ID"

# Process a different input file based on the array index
python process_file.py input_${SLURM_ARRAY_TASK_ID}.txt output_${SLURM_ARRAY_TASK_ID}.txt
```

In this example, Slurm will create 10 separate jobs. Each job will have a different value for the `$SLURM_ARRAY_TASK_ID` variable (from 1 to 10). This lets you process 10 different input files with a single job submission.

**Example output** (for array task 3):

```
This is array task 3
[Output from processing input_3.txt would appear here]
```

### Job Dependencies: Making Jobs Wait for Other Jobs

Sometimes you need to run jobs in a specific order – for example, you might need to prepare data before analyzing it. Job dependencies let you tell Slurm “only start this job after that job finishes.”

**Example:**

```bash
# Submit the first job and capture its ID
first_job=$(sbatch --parsable prepare_data.sh)
echo "Submitted preparation job with ID: $first_job"

# Submit the second job that depends on the first one
sbatch --dependency=afterok:$first_job analyze_data.sh
```

The `--dependency=afterok:$first_job` tells Slurm to only run the analysis job if the preparation job completes successfully.

Types of dependencies:

- `after`: Run after the specified jobs start

- `afterany`: Run after the specified jobs end (regardless of success or failure)

- `afterok`: Run only if the specified jobs succeed

- `afternotok`: Run only if the specified jobs fail

### Using Email Notifications

It’s often useful to get an email when your job starts, ends, or fails, so you don’t have to keep checking its status.

**Example:**

```bash
#!/bin/bash
#SBATCH --job-name=email_test
#SBATCH --output=email_test_%j.out
#SBATCH --nodes=1
#SBATCH --time=01:00:00
#SBATCH --mail-type=BEGIN,END,FAIL     # When to send email
#SBATCH --mail-user=your.email@example.com  # Where to send it

echo "This job will trigger email notifications"
sleep 300  # Just sleep for 5 minutes as a demo
echo "Job completed"
```

## Best Practices and Tips for Beginners

### Estimating Resource Needs

One of the biggest challenges when starting with HPC is knowing how much to request:

1. **Start small and test**: Begin with small test runs to see how much memory and time your program needs.

2. **Use monitoring tools**: After running jobs, check how much memory and CPU they actually used using `sacct`:

```
   sacct -j 12345 --format=JobID,JobName,MaxRSS,Elapsed
```

`MaxRSS` shows the maximum memory used, and `Elapsed` shows how long the job ran.

1. **Add a buffer, but don’t exaggerate**: Request about 20-50% more memory and time than you think you’ll need, but don’t ask for 10x more – it makes your job wait longer and wastes resources.

### Common Mistakes to Avoid

1. **Running heavy processes on login nodes**: Login nodes are shared by everyone and should only be used for light tasks like editing files and submitting jobs. Use Slurm to run your actual analysis.

2. **Hardcoding paths**: Use environment variables like `$HOME` or `$SLURM_SUBMIT_DIR` instead of typing out the full path to your home directory.

3. **Ignoring error messages**: Always check the error file if your job fails. The error message often tells you exactly what went wrong.

4. **Not saving your work**: Use the output and error files to save your program’s output. If you don’t specify these, Slurm will create files like `slurm-12345.out` by default.

5. **Requesting too many resources**: This makes your job wait longer than necessary.

### Practical Tips for Success

1. **Script organization**: Keep your Slurm directives at the top of the script, followed by environment setup, and then your actual commands.

2. **Use job names**: The `--job-name` option makes it easier to identify your job in the queue.

3. **Check queue status before submitting**: Use `sinfo` to see how busy the system is.

4. **Test interactively first**: Before submitting a batch job, test your commands using an interactive session with `srun --pty bash`.

5. **Learn from examples**: Most HPC centers provide example scripts – use them as templates.

6. **Start with short time limits**: If your job has a short time limit, it might start sooner. You can always increase the time for later runs.

7. **Use the right partition**: Different partitions have different purposes. Don’t use a debug partition for long-running jobs.

8. **Clean up your files**: HPC systems often have limited storage. Delete or compress files you don’t need anymore.

## Example Workflow: From Login to Results

Let’s walk through a complete example workflow, from logging in to getting results:

### 1. Log in to the HPC system

```
ssh yourusername@hpc.example.edu
```

### 2. Create a directory for your project

```
mkdir -p ~/projects/data_analysis
cd ~/projects/data_analysis
```

### 3. Transfer your data and scripts

You can use `scp` or another tool to copy files to the HPC system:

```bash
# From your local machine, not on the HPC system
scp data.csv analyze_data.py yourusername@hpc.example.edu:~/projects/data_analysis/
```

### 4. Create a job script

Create a file named `run_analysis.sh` with your favorite text editor (like nano, vim, or emacs):

```
nano run_analysis.sh
```

Add the content (based on our conda example above):

```bash
#!/bin/bash
#SBATCH --job-name=data_analysis
#SBATCH --output=analysis_%j.out
#SBATCH --error=analysis_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=01:00:00

echo "Job started at $(date)"

# Activate conda environment
source ~/miniforge3/etc/profile.d/conda.sh
conda activate myenv

# Run the analysis
python analyze_data.py --input data.csv --output results.csv

echo "Job completed at $(date)"
```

### 5. Submit the job

```
sbatch run_analysis.sh
```

Example output:

```
Submitted batch job 12345
```

### 6. Monitor the job

```
squeue -u yourusername
```

Example output:

```
  JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
  12345    normal data_ana yourusern  R       0:15      1 node007
```

### 7. Check the results when the job completes

```bash
# Check if the job has finished
sacct -j 12345 --format=JobID,JobName,State,Elapsed

# Look at the output file
cat analysis_12345.out

# Examine the results
head results.csv
```

## Troubleshooting Common Issues

### “Job Failed” or “Out of Memory”

If your job fails with an “out of memory” error:

1. Check how much memory it was using:

```
   sacct -j 12345 --format=JobID,JobName,MaxRSS,State
```

1. Increase the memory request in your job script:

```bash
   #SBATCH --mem=16G  # Increase from 8G to 16G
```

### “Time Limit Exceeded”

If your job is terminated because it reached the time limit:

1. Check how long it ran:

```
   sacct -j 12345 --format=JobID,JobName,Elapsed,Timelimit
```

1. Increase the time limit in your job script:

```bash
   #SBATCH --time=04:00:00  # Increase from 1 hour to 4 hours
```

### “Command Not Found”

If your job fails with “command not found”:

1. Make sure you’re activating the correct conda environment.

2. Check your PATH variable.

3. Use absolute paths to your executables if necessary.

### Job Stuck in Pending Status

If your job stays in pending status for a long time:

1. Check the reason:

```
   squeue -j 12345 -l
```

1. Common reasons include:

- `Resources`: Not enough resources available (nodes, CPUs, memory)

- `Priority`: Other jobs have higher priority

- `QOSMaxJobsPerUserLimit`: You’ve reached the maximum number of concurrent jobs

- `DependencyNeverSatisfied`: Your job is waiting for another job that failed

1. Solutions to try:

- Request fewer resources (fewer nodes/CPUs/memory)

- Use a different partition with more available resources

- Wait for other jobs to complete

- Talk to your HPC administrators if the wait seems excessive

## Job Efficiency and Optimization

### Understanding CPU and Memory Efficiency

Running your jobs efficiently not only gets you results faster but also helps everyone use the HPC system better. Here are some simple ways to improve efficiency:

1. **Match tasks to cores**: If your program only uses 1 core, don’t request 16 cores. For example:

```bash
   # For a single-core program
   #SBATCH --ntasks=1
   #SBATCH --cpus-per-task=1
```

1. **Use the right memory**: Request enough memory, but not too much. If your job uses 4GB of memory, requesting 32GB will make it wait longer.

2. **Set realistic time limits**: If your job completes in 2 hours, setting a 24-hour limit might make it wait longer in the queue.

### Running Multiple Tasks Within a Job

Sometimes you have many small tasks to run. Instead of submitting them as separate jobs, you can run them within a single job:

```bash
#!/bin/bash
#SBATCH --job-name=multiple_tasks
#SBATCH --output=tasks_%j.out
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH --time=02:00:00

# Run four different tasks, one on each CPU
srun --ntasks=1 --exclusive python task1.py &
srun --ntasks=1 --exclusive python task2.py &
srun --ntasks=1 --exclusive python task3.py &
srun --ntasks=1 --exclusive python task4.py &

# Wait for all background tasks to complete
wait
```

In this example, the `&` at the end of each line makes the task run in the background, and the `wait` command makes the script wait until all the background tasks are finished.

## Using Slurm with Common Software and Tools

### Running R Scripts with Slurm

Here’s an example of a Slurm script for running an R script:

```bash
#!/bin/bash
#SBATCH --job-name=r_analysis
#SBATCH --output=r_analysis_%j.out
#SBATCH --error=r_analysis_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=2
#SBATCH --mem=4G
#SBATCH --time=01:00:00

echo "Starting R analysis at $(date)"

# Load R module (if your HPC uses modules)
# module load r/4.1.0

# Or use conda
source ~/miniforge3/etc/profile.d/conda.sh
conda activate r_env  # An environment with R installed

# Run the R script
Rscript analyze_data.R

echo "R analysis completed at $(date)"
```

### Running MATLAB Scripts with Slurm

For MATLAB users, here’s a Slurm script example:

```bash
#!/bin/bash
#SBATCH --job-name=matlab_analysis
#SBATCH --output=matlab_%j.out
#SBATCH --error=matlab_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
#SBATCH --time=02:00:00

echo "Starting MATLAB analysis at $(date)"

# Load MATLAB module (if your HPC uses modules)
# module load matlab/R2022a

# Run the MATLAB script
matlab -nodisplay -nosplash -nodesktop -r "run('analyze_data.m'); exit;"

echo "MATLAB analysis completed at $(date)"
```

The `-nodisplay -nosplash -nodesktop` options tell MATLAB to run without a graphical interface, which is necessary on most HPC systems.

## Example of a Complete Workflow with Advanced Features

Let’s walk through a more advanced example that combines several Slurm features:

### 1. Preparing the data (first job)

```bash
#!/bin/bash
#SBATCH --job-name=data_prep
#SBATCH --output=prep_%j.out
#SBATCH --error=prep_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --mem=4G
#SBATCH --time=01:00:00

echo "Starting data preparation at $(date)"

# Set up conda
source ~/miniforge3/etc/profile.d/conda.sh
conda activate myenv

# Run the data preparation script
python prepare_data.py --input raw_data.csv --output prepared_data.csv

echo "Data preparation completed at $(date)"
```

### 2. Running the analysis (second job, depends on first)

```bash
#!/bin/bash
#SBATCH --job-name=analysis
#SBATCH --output=analysis_%j.out
#SBATCH --error=analysis_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH --mem=16G
#SBATCH --time=04:00:00

echo "Starting analysis at $(date)"

# Set up conda
source ~/miniforge3/etc/profile.d/conda.sh
conda activate myenv

# Run the analysis script
python analyze_data.py --input prepared_data.csv --output results.csv

echo "Analysis completed at $(date)"
```

### 3. Submitting the workflow with dependencies

```bash
#!/bin/bash

# Submit the data preparation job
prep_job=$(sbatch --parsable prepare.sh)
echo "Submitted data preparation job with ID: $prep_job"

# Submit the analysis job with a dependency
analysis_job=$(sbatch --parsable --dependency=afterok:$prep_job analyze.sh)
echo "Submitted analysis job with ID: $analysis_job"

# Submit a notification job that sends an email when the whole workflow completes
sbatch --dependency=afterok:$analysis_job --mail-type=END --mail-user=your.email@example.com --wrap="echo Workflow completed at \$(date)"

echo "Workflow submitted. You will receive an email when it completes."
```

This script submits three jobs:

1. A data preparation job

2. An analysis job that only runs if the preparation job succeeds

3. A simple notification job that sends an email when everything is done

## Common HPC Terms and Concepts Explained

### Parallel Computing Basics

Understanding these concepts will help you make better use of HPC resources:

- **Serial processing**: Running a program that uses only one CPU core at a time (one task after another).

- **Parallel processing**: Running multiple parts of a program simultaneously on different CPU cores.

- **MPI (Message Passing Interface)**: A standard for distributed memory parallel computing. Programs using MPI can run across multiple compute nodes.

- **OpenMP**: A standard for shared memory parallel computing. Programs using OpenMP run on a single node but can use multiple CPU cores.

- **Hybrid parallelism**: Using both MPI and OpenMP together – MPI for communication between nodes and OpenMP for parallelism within each node.

### Storage and File Systems

Most HPC systems have different types of storage:

- **Home directory**: Usually has limited space but may be backed up. Good for important scripts and documents.

- **Scratch space**: Temporary, high-performance storage for job data. May be automatically purged after a certain period.

- **Project space**: Shared storage for research groups or projects, often with larger quotas than home directories.

- **Parallel file systems**: Special file systems designed for high-performance data access from many compute nodes simultaneously.

## Environment Modules vs. Conda

Many HPC systems provide software through “environment modules” or allow users to manage their own software with tools like Conda:

### Environment Modules

```bash
# List available modules
module avail

# Load a specific module
module load python/3.9.5

# List loaded modules
module list

# Unload a module
module unload python/3.9.5
```

### Conda Environments

```bash
# Create a new environment
conda create -n myenv python=3.9 numpy pandas

# Activate an environment
conda activate myenv

# Install additional packages
conda install -c conda-forge matplotlib

# Deactivate the environment
conda deactivate
```

Your HPC system may use one or both of these approaches. Environment modules are centrally managed by the HPC administrators, while Conda environments are managed by individual users.

## Requesting Special Hardware

Some HPC systems have specialized hardware like GPUs (Graphics Processing Units) or high-memory nodes. Here’s how to request these resources:

### Requesting GPUs

```bash
#!/bin/bash
#SBATCH --job-name=gpu_job
#SBATCH --output=gpu_%j.out
#SBATCH --error=gpu_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --gres=gpu:1      # Request 1 GPU
#SBATCH --partition=gpu   # Use the GPU partition
#SBATCH --time=02:00:00

echo "GPU job starting at $(date)"

# Load CUDA module (if needed)
# module load cuda/11.4

# Run your GPU program
python train_neural_network.py

echo "GPU job completed at $(date)"
```

### Requesting High-Memory Nodes

```bash
#!/bin/bash
#SBATCH --job-name=bigmem_job
#SBATCH --output=bigmem_%j.out
#SBATCH --error=bigmem_%j.err
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --mem=128G         # Request 128 GB of memory
#SBATCH --partition=bigmem # Use high-memory partition
#SBATCH --time=04:00:00

echo "High-memory job starting at $(date)"

# Run your memory-intensive program
python process_large_dataset.py

echo "High-memory job completed at $(date)"
```

## Learning from the HPC Community

The HPC community is generally very helpful, and there are many resources available:

### HPC Documentation

Most HPC centers provide documentation specific to their systems. This is often the best place to start, as it will cover the specific configurations and policies of your HPC system.

### Asking for Help

Don’t hesitate to ask for help from:

- **HPC support staff**: Most HPC centers have dedicated support staff who can assist with problems.

- **Colleagues**: Others in your department or research group who use the HPC system.

- **Online forums**: Websites like Stack Overflow or the Slurm mailing list.

When asking for help, be sure to include:

- The error message you’re seeing

- Your job script

- What you’ve already tried

- The job ID if applicable

## Conclusion: Your HPC Journey

High Performance Computing might seem intimidating at first, but with practice, it becomes a powerful tool for your research or data analysis. Remember:

1. **Start small**: Begin with simple jobs and gradually tackle more complex workflows.

2. **Learn from examples**: Use the examples in this tutorial and from your HPC center as templates.

3. **Be patient**: HPC systems are shared resources, and sometimes you’ll need to wait for your jobs to run.

4. **Be efficient**: Request only the resources you need to make the best use of the system.

5. **Keep learning**: HPC is a field that’s constantly evolving, so stay curious and keep exploring new ways to use these powerful resources.

With the knowledge and examples from this tutorial, you’re now equipped to begin using Slurm to harness the power of High Performance Computing for your work. Happy computing!

## Additional Resources

- [Official Slurm Documentation](https://slurm.schedmd.com/)

- [Slurm Quick Start Tutorial](https://slurm.schedmd.com/quickstart.html)

- [Princeton HPC Slurm Guide](https://researchcomputing.princeton.edu/support/knowledge-base/slurm)

- [NERSC Slurm User Guide](https://docs.nersc.gov/jobs/examples/)

- [Cornell CAC Slurm Guide](https://cvw.cac.cornell.edu/slurm/)
