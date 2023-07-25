# High-Performance Computing (HPC) File Parsing Solution - Direct Access to GPU & CPU Resources

This solution provides direct access to GPU and CPU resources for high-performance computing (HPC) and high-throughput computing (HTC) environments. Unlike enterprise-based container frameworks, which are designed for microservices and require root privileges to install and run applications, this solution is optimized for complex applications that require all available resources without special privileges.

## Targeted Toolsets Implemented

This solution uses the following targeted toolsets:

- Apache Tika™ by Oracle
- Apptainer (formerly Singularity) by Berkeley National Laboratory

## Initial Cause for Solution Development

The development of this solution was motivated by the need to parse 7.5 TB of digital forensics data produced and stored in a variety of non-standard formats. The parsing of all data is necessary to drive subsequent efforts wherein conjectures are made from the subsequent data parsed.

## Why Apptainer for HPC instead of Virtual Machines or Docker

Apptainer/Singularity is a container platform created for the HPC/HTC use case and presents key concepts for the scientific community:

1. It’s designed to execute applications with bare-metal performance while retaining a high level of security, portability, and reproducibility.
2. Containers run rootless to prohibit privilege escalation.
3. Able to Leverage GPUs, FPGAs, high-speed networks, and filesystems.
4. A container platform for building and running Linux containers that packages software, libraries, and runtime compilers in a self-contained environment. 
   - Application portability (single image file, contain all dependencies) 
   - Reproducibility, run cross platform, provide support for legacy OS and apps.
5. Ability to run, and in modern systems also to be installed, without any root daemon or setuid privileges. This makes it safer for large computer centers with shared resources.
6. Preserves the permissions in the environment. The user outside the container can be the same user inside.
7. Apptainer propagates most environment variables set on the host into the container, by default. Docker does not propagate any host environment variables into the container. Environment variables may change the behavior of software.  
8. Simple integration with resource managers (SLURM in our case) and distributed computing frameworks because it runs as a regular application. 

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/945a382c-3488-4c65-a743-44f0a704c7a5)

## Development Steps:

### Test Host Machine (Bare Metal):

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/0c14fa9e-2508-4c80-9883-f016eb70484f)

### Step 1: Apptainer - Build from Source/ Install Debian Packages for Dependencies

```bash
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
    curl wget git \
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
```

### Step 2: Create Sandbox Directory / Pull Ubuntu 22.04 - Jammy Docker Container (Base Ubuntu Build)
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/7aaf3e7e-cb74-40d1-9971-24808b0885f8)

### Step 3: Convert to Immutable .sif Image for Future Builds - Demonstrate Shell Access
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/88b8bf10-0e96-4ad3-8d91-6135140e9a00)

### Step 4: Definition File Configuration for Building Dependencies - 1st Build Scuccessful
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/ce94c114-8b0b-44b2-a7c0-33c26e4e36b7)

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/0d3b4389-7329-4a06-b551-392b07586abc)

### Step 5: Now that There is a Base Instance Working, lets create a live sandbox for testing from the image we just created:

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/a45a0aed-0b5b-4cc9-abdb-5178ad5ac648)

#### Note: Initial Containers are limited to 64MB in size. Fix:
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/0f2b5e02-d8a1-42bc-995a-507b802c4c3a)

### Step 6: Create a New File System Overlay/ add as a layer in SIF build:

![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/441d3a4a-18cd-4c74-8cf8-b7ffe18167eb)

### Step 7: Build Tika/ Configure Properly - Completed/ Success:
![image](https://github.com/alexander-labarge/hpc-tika-build/assets/103531175/7c7320ab-999b-45b9-bd3c-beb8a19c1230)

```bash
#!/bin/bash

function check_tika_test {
    echo "======================================"
    echo "Checking Tika test..."
    echo "======================================"
    if grep -q "TIKA PASSED TEST - ALEX" /output-files/tika-test-file.txt.json; then
        echo "================================================================="
        echo "================================================================="
        echo "================================================================="
        echo "Tika test passed. FOUND STRING: TIKA PASSED TEST - ALEX in file."
        echo "================================================================="
        echo "================================================================="
        echo "================================================================="
        echo "============TIKA HPC BUILD COMPLETING FINAL STEPS================"
        echo "================================================================="
        echo "================================================================="
    else
        echo "Tika test failed."
    fi
}

echo "======================================"
echo "Starting Tika... at $(date)"
echo "======================================"
echo "input-files directory contents:"
cp /opt/tika-test-file.txt /input-files
ls -l /input-files/
java -jar /tika-app-2.8.0.jar -i /input-files -o /output-files -J
echo "======================================"
echo "Tika started."
echo "Tika output complete."
echo "======================================"
echo "output-files directory contents:"
ls -l /output-files
echo "======================================"
echo "Completed at: $(date)"
echo "======================================"

echo "======================================"
echo "Extracting text from files..."
echo "======================================"
echo "======================================"
echo "Extracted JSON OUTPUT:"

# Extract text from files & ignore JSON text
extracted_text=$(find /output-files -type f -exec strings {} \; | grep -vE '^{.*}$')

# Print extracted text
echo "$extracted_text"

# Check Tika test
check_tika_test
```
