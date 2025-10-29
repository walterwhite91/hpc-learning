# Introduction to High-Performance Computing: Scheduling Jobs

## Resource: https://epcced.github.io/hpc-intro/13-scheduler/index.html

### Questions

* What is a scheduler and why are they used?
* How do I launch a program to run on any one node in the cluster?
* How do I capture the output of a program that is run on a node in the cluster?

### Objectives

* Run a simple Hello World style program on the cluster.
* Submit a simple Hello World style script to the cluster.
* Use batch system command-line tools to monitor job execution.
* Inspect the output and error files of your jobs.

## Overview

## Job Scheduler

An HPC system might have thousands of nodes and users. The scheduler decides which job runs where and when. It ensures every task gets the required resources.
Schedulers in HPC are like waiters in restaurants—managing queues and assigning resources fairly.

The scheduler used in this lesson is **PBS Pro**. Even though syntax varies across systems, job submission concepts are similar.

## Running a Batch Job

Any non-interactive command or series of commands submitted to the scheduler is a **batch job**.

### Creating a Test Job

Example job script:

```bash
#!/bin/bash
echo 'This script is running on:'
hostname
sleep 60
```

Run directly to see it executes on the login node. To submit it to the scheduler:

```bash
qsub -q R7090776 -A tc011 -l select=1,walltime=0:10:0 example-job.sh
```

### Checking Job Status

Use `qstat` to check the job queue:

```bash
qstat -u yourUsername
```

Use `watch` to auto-refresh job status:

```bash
watch -n 20 qstat -u yourUsername
```

Press **Ctrl + C** to stop watching.

## Customizing a Job

Default scheduler settings might not suit every job. Customize resource and job details using special PBS directives within the script.

### Example Custom Job Script

```bash
#!/bin/bash
#PBS -N new_name
#PBS -A tc011
#PBS -l walltime=00:10:00
#PBS -l select=1

echo 'This script is running on:'
hostname
sleep 60
```

### Checking the Job Name

```bash
qstat -u yourUsername
```

This shows that the job name has been successfully changed.

### Email Notifications

You can configure PBS to email job status updates using appropriate `qsub` options.

## Resource Requests

You must specify resources for every job. This helps the scheduler allocate suitable nodes and time slots.

### Key Resource Options

* `-l select=<nnodes>` → Number of nodes required.
* `-l walltime=<hh:mm:ss>` → Expected runtime.

Requesting more resources doesn’t make the job faster—it just reserves more capacity.

### Example Resource Request Script

```bash
#!/bin/bash
#PBS -A tc011
#PBS -l walltime=00:00:30
#PBS -l select=1

echo 'This script is running on:'
hostname
sleep 60
```

If the job exceeds walltime, it will be killed:

```
PBS: job killed: walltime 50 exceeded limit 30
```

### Why Limits Matter

Strict limits prevent one user from monopolizing cluster resources, ensuring fair access for everyone.

### Cost

Jobs are charged for time used. Request slightly more time than you expect—too long, and it may take longer to schedule.

## Cancelling a Job

Cancel a job with:

```bash
qdel <job_id>
```

Verify cancellation with:

```bash
qstat -u yourUsername
```

## Interactive Jobs

For tasks that require manual execution but need more resources than a login node, use **interactive sessions**:

```bash
qsub -q R7090776 -A tc011 -IVl select=1,walltime=0:10:0
```

You’ll get a prompt on a compute node. Type `exit` to leave.

## Filesystem on ARCHER

ARCHER has two main filesystems:

* `/home/` → Small files, source code (not mounted on compute nodes).
* `/work/` → Large files, datasets, and production runs.

Before running real programs:

```bash
cd /work/tc011/tc011/yourUsername/
```

## Running Parallel Jobs with MPI

HPC power comes from **parallelism**—running processes across many cores or nodes.
Parallel jobs use **MPI (Message Passing Interface)** for inter-process communication.

### MPI Job Requirements

1. Parallel launch command (`mpirun`, `mpiexec`, `aprun`, etc.)
2. Total number of processes (`-n`)
3. Processes per node (`-N`)
4. The program and its arguments

### Example MPI Job Script

```bash
#!/bin/bash
#PBS -N mpisharpen
#PBS -l select=2
#PBS -l walltime=00:10:00
#PBS -A tc011

cd $PBS_O_WORKDIR
module load epcc-training/sharpen
cp $SHARPEN_DATA/fuzzy.pgm .

aprun -n 48 -N 24 sharpen
```

Submit the job:

```bash
qsub -q R7090776 run-sharpen.pbs
```

Check output:

```bash
ls -l *.pgm
```

You should see both `fuzzy.pgm` and `sharpened.pgm`.

### Modifying Parallel Jobs

* Change node count or process distribution using `-N` and `-n`.
* Verify with job output and ensure correct process placement.
---
## Key Points

* The scheduler manages resource sharing.
* Always submit work through the scheduler.
* A job is simply a shell script.
* Request slightly more resources than you expect to need.
