# High-Performance Computing (HPC) File Parsing Solution - Direct Access to GPU & CPU Resources
Many solutions are available to work with containers, for example Docker, one of the most popular platforms. However, the enterprise-based container frameworks were motivated to provide micro-services, a solution that fits well in the models of the industry, where system administrators with root privilege install and run applications, each in its own container. This is not so compatible with the workflow in the High-Performance Computing (HPC) and High Throughput Computing (HTC), in which usually complex applications run exhaustively using all the available resources and without any special privilege.

**Apptainer/Singularity is a container platform created for the HPC/HTC use case and presents key concepts for the scientific community:**

1. It’s designed to execute applications with bare-metal performance while retaining a high level of security, portability, and reproducibility.
2. Containers run rootless to prohibit privilege escalation.
3. Able to Leverages GPUs, FPGAs, high-speed networks, and filesystems.
4. Single-file based container images, facilitating the distribution, archiving and sharing.
5. Ability to run, and in modern systems also to be installed, without any root daemon or setuid privileges. This makes it safer for large computer centers with shared resources.
6. Preserves the permissions in the environment. The user outside the container can be the same user inside.
7. Simple integration with resource managers (SLURM in our case) and distributed computing frameworks because it runs as a regular application. 

# Overview
**Problem:** 7.5 TB of digital forensics data produced/ stored in a variety of non-standard formats (digital forensics tools), which requires directory traversal, decompression, and file parsing. The parsing of all data is necessary to drive subsequent efforts wherein conjectures are made from the subsequent data parsed. 

# Targeted Toolsets Included in this Solution:
## Apache Tika™ - by Oracle:
1. Java driven toolkit which detects and extracts metadata and text from over a thousand different file types (such as PPT, XLS, and PDF). All of these file types can be parsed through a single interface, making Tika useful for search engine indexing, content analysis, translatio, and much more.

## Apptainer (formerly Singularity) - Berkeley National Laboratory:
1. A container platform for building and running Linux containers that packages software, libraries, and runtime compilers in a self-contained enviroment. Application portability (single image file, contain all dependencies) Reproducibility, run cross platform, provide support for legacy OS and apps.
2. Apptainer propagates most environment variables set on the host into the container, by default. Docker does not propagate any host environment variables into the container. Environment variables may change the behaviour of software.  

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/945a382c-3488-4c65-a743-44f0a704c7a5)


1. The tool is written in the Java language
2. Typical usage includes an Nginx webserver, which listens on a given port specified at initialization.
3. At theneck from the performance perspective.
4. 
5. Instead of using web servers, and pushing files over a network through tcp connections, the jar will be executed as a component in a Unix pipeline.
