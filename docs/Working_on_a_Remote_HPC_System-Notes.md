# Working on a Remote HPC System — Summary Notes

## Resource: https://epcced.github.io/hpc-intro/12-cluster/index.html

## Objectives:

* Connect to a remote HPC system.
* Understand the general HPC system architecture.

---

## Overview

High-Performance Computing (HPC) systems are powerful networks of computers (called clusters) used to perform large or complex computations faster than a single machine.


## What is an HPC System?

* **Cloud:** General remote computing resources (e.g., servers, web services, storage).
* **Cluster / HPC system:** A network of computers (nodes) working together on large computational tasks.
* Used for computations too big for one computer.


## Logging In

Use SSH (Secure Shell) to connect:

```bash
ssh -XY yourUsername@login.archer.ac.uk
```

* `-XY`: Enables Unix GUI and graphics forwarding.
* **Linux/macOS:** SSH is preinstalled (`ssh --help` to check).
* **Windows:** Use tools like MobaXterm, PuTTY, or Bitvise.
* **MobaXterm** allows GUI access and drag-and-drop file transfers.


## Where Are We?

* Logging in connects you to a **login/head node**, not the full cluster.
* Check current host:

```bash
hostname
```

* Clusters have multiple login nodes to balance user load.


## Cluster Structure

* **Nodes:** Individual computers in the cluster.

 * **Login (Head) Node:** Access point for setup, file transfer, small tests.
 * **Worker (Compute) Nodes:** Perform computation jobs.

* **Scheduler:** Manages and assigns jobs to worker nodes.

  ```bash
  pbsnodes -a

  ```
* Files saved on one node are **available across the entire cluster**.


## Node Anatomy

Each node has:

* **CPU / Cores:** Runs programs and calculations.
* **Memory (RAM):** Holds data for current tasks.
* **Disk Storage:** Stores data permanently.
* Nodes share a **central storage system** (accessible to all).


## Exploring Resources

Useful commands:

```bash
nproc --all       # Number of processors
free -m           # Memory info (in MB)
lscpu             # Detailed CPU info
cat /proc/meminfo # Detailed memory info
pbsnodes node_ID  # Info about a specific worker node
```

Compare:

* Your laptop vs. Head Node vs. Worker Node
  Note differences in CPU, RAM, and performance.


## Units and Memory

* **1 Byte = 8 bits**
* **SI prefixes:** 1 kB = 1000 B, 1 MB = 1000 kB, etc.
* **Binary prefixes:** 1 KiB = 1024 B, 1 MiB = 1024 KiB.
* Computers use base 2, humans use base 10, causing common confusion.


## Node Differences

* HPC clusters may include:

  * High-memory nodes
  * GPU nodes (for parallel computation)
  * Specialized architectures for different workloads

---

## Key Points

* HPC = networked system of many machines.
* Includes **login nodes** (for access) and **worker nodes** (for computation).
* Node resources (CPU, RAM, disk) vary.
* Files are shared cluster-wide.
