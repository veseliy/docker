# image to build from
ARG repository=nvidia/cuda
ARG CUDA_version=9.0
ARG cudnn_version=cudnn7
ARG flavor=devel
ARG os=ubuntu
ARG os_version=16.04 


# Example of an image to build from: nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04
FROM ${repository}:${CUDA_version}-${cudnn_version}-${flavor}-${os}${os_version}

MAINTAINER Stas Batururimi <s.batururimi@gmail.com>

# Install some dependencies
# Opencv needs: libsm6, libxext6, libxrender1
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential\
    # opencv
    libsm6 \
    libxext6 \
    libxrender1 \
    # general
    curl \
    git \
    wget \
    ffmpeg \
    unzip \
    zip \
    sudo \
    bzip2 \
    libcurl3-dev \
    libfreetype6-dev \
    libpng12-dev \
    libzmq3-dev \
    pkg-config \
    rsync \
    software-properties-common \   
    zlib1g-dev \
    # clean, link and update
    && rm -rf /var/lib/apt/lists/* 

# for Python 3.n
RUN apt-get update \
    && apt-get install -y --install-recommends \
    python3-pip \ 
    python3-dev \
    python-virtualenv \
    && apt-get clean \
    &&  rm -rf /var/lib/apt/lists/*

# working directory
WORKDIR /root

# TensorBoard
EXPOSE 6006

# IPython
EXPOSE 8888

ENTRYPOINT ["/bin/bash"]
