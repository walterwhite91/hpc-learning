#Intro to HPC

###Resource: 
https://epcced.github.io/hpc-intro/

##Initial Set-up
My first recommendation would be to have linux installed. I already have UBUNTU 24.02 running on my laptop, so I will now directly move to installing some pre-requisite pacakges.

###Required Tools

Run this code in your terminal
	sudo apt install build-essential gdb valgrind tree htop tmux openmpi-bin libopenmpi-dev
where
- build-essential: installs gcc, g++, make, and related utilities
    required to compile C and C++ programs and to manage larger projects efficiently.
- gdb
    C/C++ debugger
- valgrid
    memory leak checker and run-time analysis
- tree
    visualize directory structure
- htop
    monitor processes, CPU, memory usage
- tmux
    terminal multiplexer (run multiple sessions in one terminal, useful for long-runnig HPC jobs)
- openmpi-bin
    installs executables like mpicc, mpiexec 
    MPI lets you run parallell program across multiple processes, essential for HPC
- libopenmpi-dev
    headers and dev files fro compiling MPI C/C++ programs

###Optional Python Stack (for plots/analysis)
Run this code in your terminal
	sudo apt install ppython 3-venv python3-pip
	python3 -m venv hpc-env
	source hpc-env/bin/activate
	pip install matplotlib numpy pandas

- Only for plotting results, runtime logs, or CSV analysis
- Not required for MPI/OpenMP coding in C/C++, but very useful for benchmarking


