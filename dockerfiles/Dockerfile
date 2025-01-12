# Start from an Ubuntu base image
FROM ubuntu:20.04

# Set the maintainer of the Dockerfile
LABEL maintainer="dxie@tenstorrent.com"

# Avoid timezone interactive dialog
ENV DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN echo "Installing required packages..." && \
    apt-get update && apt-get install -y \
    curl \
    git \
    vim \
    wget \
    bzip2 \
    ca-certificates \
    sudo \
    locales \
    fontconfig \
    bash \
    build-essential \
    cpio \
    time \
    libelf1 \
    libxml2 \
    libxss1 \
    libxft2 \
    libxt6 \
    libxmu6 \
    libnuma1 \
    software-properties-common && \
    add-apt-repository -y ppa:linuxuprising/libpng12 && \
    apt-get update && \
    apt-get install -y libpng12-0 && \
    echo "dash dash/sh boolean false" | debconf-set-selections && \
    dpkg-reconfigure -p critical dash

# Configure environment
ENV CONDA_DIR=/opt/conda \
    PATH=/opt/conda/bin:$PATH

# Install Conda
RUN echo "Installing Conda..." && \
    wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p $CONDA_DIR

# Clean Conda installation
RUN echo "Cleaning up Conda..." && \
    rm ~/miniconda.sh && \
    conda clean --all -y

# Update Conda
RUN conda update -n base conda -y

RUN echo "Installing and activating conda-lock..." && \
    conda install -n base conda-lock -y && \
    conda init bash && \
    echo "source /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate /chipyard/.conda-env" >> ~/.bashrc

# Clone Chipyard repository
RUN echo "Cloning Chipyard repository..." && \
    git clone https://github.com/ucb-bar/chipyard.git && \
    cd chipyard && \
    git checkout 1.9.1

# Install Chipyard and its dependencies
RUN echo "Installing Chipyard and its dependencies..." && \
    cd chipyard && \
    ./build-setup.sh riscv-tools -s 4 -s 6 -s 7 -s 8 -s 9

