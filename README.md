# High-Performance Computing (HPC) File Parsing Solution - Direct Access to GPU & CPU Resources
### Inherent Differences between HPC Systems and Enterprise Environments
Many solutions are available to work with containers, for example Docker, one of the most popular platforms. However, the enterprise-based container frameworks were motivated to provide micro-services, a solution that fits well in the models of the industry, where system administrators with root privilege install and run applications, each in its own container. This is not so compatible with the workflow in the High-Performance Computing (HPC) and High Throughput Computing (HTC), in which usually complex applications run exhaustively using all the available resources and without any special privilege.

## Targeted Toolsets Implemented
#### Apache Tika™ - by Oracle
#### Apptainer (formerly Singularity) - by Berkeley National Laboratory

## Initial Cause for Solution Development
**Problem:** 7.5 TB of digital forensics data produced/ stored in a variety of non-standard formats (digital forensics tools), which requires directory traversal, decompression, and file parsing. The parsing of all data is necessary to drive subsequent efforts wherein conjectures are made from the subsequent data parsed. 

## Why Apptainer for HPC instead of Virtual Machines or Docker
#### Apptainer/Singularity is a container platform created for the HPC/HTC use case and presents key concepts for the scientific community:
1. It’s designed to execute applications with bare-metal performance while retaining a high level of security, portability, and reproducibility.
2. Containers run rootless to prohibit privilege escalation.
3. Able to Leverages GPUs, FPGAs, high-speed networks, and filesystems.
4. A container platform for building and running Linux containers that packages software, libraries, and runtime compilers in a self-contained enviroment. Application portability (single image file, contain all dependencies) Reproducibility, run cross platform, provide support for legacy OS and apps.
5. Ability to run, and in modern systems also to be installed, without any root daemon or setuid privileges. This makes it safer for large computer centers with shared resources.
6. Preserves the permissions in the environment. The user outside the container can be the same user inside.
7. Apptainer propagates most environment variables set on the host into the container, by default. Docker does not propagate any host environment variables into the container. Environment variables may change the behaviour of software.  
8. Simple integration with resource managers (SLURM in our case) and distributed computing frameworks because it runs as a regular application. 

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/945a382c-3488-4c65-a743-44f0a704c7a5)

_______________________________________________________________________
## DEVELOPMENT STEPS:
_______________________________________________________________________

#### TEST HOST MACHINE (BARE METAL):
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/0c14fa9e-2508-4c80-9883-f016eb70484f)

#### STEP 1: Apptainer - Build from Source/ Install debian packages for dependencies
sudo apt-get install -y \
    build-essential \
    libseccomp-dev \
    pkg-config \
    uidmap \
    squashfs-tools \
    squashfuse \
    fuse2fs \
    fuse-overlayfs \
    fakeroot \
    cryptsetup \
    curl wget git
export GOVERSION=1.20.6 OS=linux ARCH=amd64 \
wget -O /tmp/go${GOVERSION}.${OS}-${ARCH}.tar.gz \
  https://dl.google.com/go/go${GOVERSION}.${OS}-${ARCH}.tar.gz \
sudo tar -C /usr/local -xzf /home/service-typhon/Downloads/go${GOVERSION}.${OS}-${ARCH}.tar.gz \
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc \
curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.51.1 \
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc
source ~/.bashrc \
git clone https://github.com/apptainer/apptainer.git \
cd apptainer \
git checkout v1.2.0 \
./mconfig \
cd ./builddir \
make \
sudo make install

#### STEP 2: Create Sandbox Directory / Pull Ubuntu 22.04 - Jammy Docker Container (Base Ubuntu Build)
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/7aaf3e7e-cb74-40d1-9971-24808b0885f8)

#### STEP 3: Convert to immutable .sif image for future builds
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/224e44f8-f8ef-46e3-b316-25beb3ef058c)

