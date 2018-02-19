# image to build from
ARG repository=nvidia/cuda
ARG CUDA_version=9.0
ARG cudnn_version=cudnn7
ARG flavor=runtime
ARG os=ubuntu
ARG os_version=16.04 


# Example of an image to build from: nvidia/cuda:9.0-cudnn7-runtime-ubuntu16.04
FROM ${repository}:${CUDA_version}-${cudnn_version}-${flavor}-${os}${os_version}

MAINTAINER Stas Batururimi <s.batururimi@gmail.com>

# Python version and Conda version to be installed
ARG PYTHON_VERSION=3.6
ARG CONDA_PYTHON_VERSION=3
ARG CONDA_VERSION=4.3.31
ARG CONDA_DIR=/opt/conda

# user
ARG USERNAME=docker

# obtain your current user id by running in the host:
# $ id
ARG USERID=1002

# Install some dependencies
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
  git \
  wget \
  ffmpeg \
  unzip \
  sudo \
  bzip2 \
  build-essential\
  && apt-get clean \
  &&  rm -rf /var/lib/apt/lists/*

# Conda
ENV PATH $CONDA_DIR/bin:$PATH

# Conda installers https://repo.continuum.io/miniconda/
RUN apt-get update \
  && apt-get install -y --no-install-recommends ca-certificates \
  && wget --quiet https://repo.continuum.io/miniconda/Miniconda$CONDA_PYTHON_VERSION-$CONDA_VERSION-Linux-x86_64.sh -O /tmp/miniconda.sh \ 
  && echo 'export PATH=$CONDA_DIR/bin:$PATH' > /etc/profile.d/conda.sh \
  && /bin/bash /tmp/miniconda.sh -b -p $CONDA_DIR  \
  && rm -rf /tmp/* \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* 


# user's home dir should be mapped from EFS
RUN useradd --create-home -s /bin/bash --no-user-group -u $USERID $USERNAME \ 
  && chown $USERNAME $CONDA_DIR -R \
  && adduser $USERNAME sudo \
  && echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# activate user
USER $USERNAME
#RUN mkdir -p /home/$USERNAME/notebooks
#VOLUME /home/$USERNAME/notebooks
#WORKDIR /home/$USERNAME/notebooks

# set the folder
ARG PROJECT_DIR=projects

RUN mkdir -p /home/$USERNAME/$PROJECT_DIR
WORKDIR /home/$USERNAME/$PROJECT_DIR

# install python & jupyter nitebooks  using Anaconda
RUN conda install -y --quiet python=$PYTHON_VERSION \
  && conda install -y --quiet jupyter  notebook 

# upgrade pip
RUN pip install --upgrade pip

# Allow to use the jupyter notebooks without tokens
COPY jupyter_notebook_config.py .jupyter/


ENV CUDA_HOME=/usr/local/cuda
ENV CUDA_ROOT=$CUDA_HOME
ENV PATH=$PATH:$CUDA_ROOT/bin:$HOME/bin
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_ROOT/lib64

# scripts to run
ADD run_ipython.sh /home/$USERNAME
RUN sudo chmod +x /home/$USERNAME/run_ipython.sh
RUN sudo chown $USERNAME /home/$USERNAME/run_ipython.sh -R

# Jupyter
EXPOSE 8888

# TensorBoard
EXPOSE 6006

#CMD jupyter notebook --ip=0.0.0.0 --port=8888
#ARG JUPYTER_SCRIPT = /home/$USERNAME
CMD ["/home/docker/run_ipython.sh"]
