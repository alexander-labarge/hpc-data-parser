# High-Performance Computing (HPC) File Parsing Solution - Direct Access to GPU & CPU Resources
## Inherent Differences between HPC Systems and Enterprise Environments
Many solutions are available to work with containers, for example Docker, one of the most popular platforms. However, the enterprise-based container frameworks were motivated to provide micro-services, a solution that fits well in the models of the industry, where system administrators with root privilege install and run applications, each in its own container. This is not so compatible with the workflow in the High-Performance Computing (HPC) and High Throughput Computing (HTC), in which usually complex applications run exhaustively using all the available resources and without any special privilege.

# Targeted Toolsets Included in this Custom Image:
## Apache Tika™ - by Oracle
## Apptainer (formerly Singularity) - by Berkeley National Laboratory

# Why Apptainer for HPC instead of Virtual Machines or Docker
# Apptainer/Singularity is a container platform created for the HPC/HTC use case and presents key concepts for the scientific community:
1. It’s designed to execute applications with bare-metal performance while retaining a high level of security, portability, and reproducibility.
2. Containers run rootless to prohibit privilege escalation.
3. Able to Leverages GPUs, FPGAs, high-speed networks, and filesystems.
4. A container platform for building and running Linux containers that packages software, libraries, and runtime compilers in a self-contained enviroment. Application portability (single image file, contain all dependencies) Reproducibility, run cross platform, provide support for legacy OS and apps.
5. Ability to run, and in modern systems also to be installed, without any root daemon or setuid privileges. This makes it safer for large computer centers with shared resources.
6. Preserves the permissions in the environment. The user outside the container can be the same user inside.
7. Apptainer propagates most environment variables set on the host into the container, by default. Docker does not propagate any host environment variables into the container. Environment variables may change the behaviour of software.  
8. Simple integration with resource managers (SLURM in our case) and distributed computing frameworks because it runs as a regular application. 

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/945a382c-3488-4c65-a743-44f0a704c7a5)

# Initial Cause for Solution Development
**Problem:** 7.5 TB of digital forensics data produced/ stored in a variety of non-standard formats (digital forensics tools), which requires directory traversal, decompression, and file parsing. The parsing of all data is necessary to drive subsequent efforts wherein conjectures are made from the subsequent data parsed. 


