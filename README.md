# High Performance Computing Large Scale File Parsing Solution
Custom Linux Containerized Image Build for Large Scale File Parsing in High Performance Computing Environments

# Overview
**Problem:** 7.5 TB of digital forensics data produced/ stored in a variety of non-standard formats (digital forensics tools), which requires directory traversal, decompression, and file parsing. The parsing of all data is necessary to drive subsequent efforts wherein conjectures are made from the subsequent data parsed. 

# Targeted Toolsets Included
1. **Apache Tika™ - by Oracle** toolkit detects and extracts metadata and text from over a thousand different file types (such as PPT, XLS, and PDF). All of these file types can be parsed through a single interface, making Tika useful for search engine indexing, content analysis, translatio, and much more.
2. **Apptainer (formerly Singularity) - Berkely National Lab** is a container platform for building and running Linux containers that packages software, libraries, and runtime compilers in a self-contained enviroment. Application portability (single image file, contain all dependencies) Reproducibility, run cross platform, provide support for legacy OS and apps.
## Apptainer propagates most environment variables set on the host into the container, by default. Docker does not propagate any host environment variables into the container. Environment variables may change the behaviour of software.

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/945a382c-3488-4c65-a743-44f0a704c7a5)


1. The tool is written in the Java language
2. Typical usage includes an Nginx webserver, which listens on a given port specified at initialization.
3. At theneck from the performance perspective.
4. In this instance, we aim to enable batching of jobs through SLURM in an H onset, spawning new threads for multiple instances of the tika-server were implemented. Parsing of 140,000 files/ 182GB completed in approximately 2.5 hours, but it became clear that Tika became the bottle-PC Cluster with 1 Head Node, and approximately 40 slave nodes w/ Nvidia GPUs on board.
5. Instead of using web servers, and pushing files over a network through tcp connections, the jar will be executed as a component in a Unix pipeline.
