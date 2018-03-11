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

## Python version and Conda version to be installed
#ARG PYTHON_VERSION=3.6
#ARG CONDA_PYTHON_VERSION=3
#ARG CONDA_VERSION=4.3.31
#ARG CONDA_DIR=/opt/conda

# user
# 1) if not launched with `make` uncomment the line
ARG USERNAME=docker
# 2) with `make` and arguments passed during the build command
#ARG USERNAME

# obtain your current user id by running in the host:
# $ id
# 1) uncomment if not run with `make`
ARG USERID=1002
# 2) with `make` and arguments passed during the build command
#ARG USERID


# Install some dependencies
# Opencv needs: libsm6, libxext6, libxrender1
RUN apt-get update && \
  apt-get install -y --no-install-recommends \
  libsm6 \
  libxext6 \
  libxrender1 \
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
#ENV PATH $CONDA_DIR/bin:$PATH

# Conda installers https://repo.continuum.io/miniconda/
#RUN apt-get update \
#  && apt-get install -y --no-install-recommends ca-certificates \
#  && wget --quiet https://repo.continuum.io/miniconda/Miniconda$CONDA_PYTHON_VERSION-$CONDA_VERSION-Linux-x86_64.sh -O /tmp/miniconda.sh \ 
#  && echo 'export PATH=$CONDA_DIR/bin:$PATH' > /etc/profile.d/conda.sh \
#  && /bin/bash /tmp/miniconda.sh -b -p $CONDA_DIR  \
#  && rm -rf /tmp/* \
#  && apt-get clean \
#  && rm -rf /var/lib/apt/lists/* 


# user's home dir should be mapped from EFS
#RUN useradd --create-home -s /bin/bash --no-user-group -u $USERID $USERNAME \ 
#  && chown $USERNAME $CONDA_DIR -R \
#  && adduser $USERNAME sudo \
#  && echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Instal pip and Virtualenv
# for Python 2.7
#RUN apt-get update && \
#	apt-get install -y --no-install-recommends python-pip python-dev python-virtualenv
# for Python 3.n
RUN apt-get update \
#	&& add-apt-repository universe \ 
	&& apt-get install -y --install-recommends \
	python3-pip \ 
	python3-dev \
	python-virtualenv \
	&& apt-get clean \
 	&&  rm -rf /var/lib/apt/lists/*

# create a user with same uid as the user who launched the creation of the container
RUN useradd --create-home -s /bin/bash --no-user-group -u $USERID  $USERNAME \
	&& adduser $USERNAME sudo \
	&& echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers


# activate user
USER $USERNAME
#RUN mkdir -p /home/$USERNAME/notebooks
#VOLUME /home/$USERNAME/notebooks
#WORKDIR /home/$USERNAME/notebooks

# set the folder
# 1) uncomment if not launched from `make`
ARG PROJECT_DIR=projects
# 2) with `make` and arguments passed during the build command
#ARG PROJECT_DIR

RUN mkdir -p /home/$USERNAME/$PROJECT_DIR
WORKDIR /home/$USERNAME/$PROJECT_DIR

# be sure to have read/write access to the PROJECT_DIR
#RUN chmod -R 700
# install python & jupyter nitebooks  using Anaconda
#RUN conda install -y --quiet python=$PYTHON_VERSION \
#  && conda install -y --quiet jupyter  notebook 

# upgrade pip
#RUN pip install --upgrade pip


# Create a Virtualenv environment
# for Python 2.7
#RUN virtualenv --system-site-packages $PROJECT_DIR/py2
# for Python 3.n
#RUN virtualenv --system-site-packages -p python3 $PROJECT_DIR/py3

# Activate the Virtualenv environment
# By default py3 environment is activated
#RUN source $PROJECT_DIR/py3/bin/activate

# Ensure pip ≥8.1 is installed:
#RUN easy_install -U pip


# Allow to use the jupyter notebooks without tokens
#COPY jupyter_notebook_config.py .jupyter/


# setup environment variables for cuda
ENV CUDA_HOME=/usr/local/cuda
ENV CUDA_ROOT=$CUDA_HOME
ENV PATH=$PATH:$CUDA_ROOT/bin:$HOME/bin
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_ROOT/lib64

## scripts to run
#ADD run_ipython.sh /home/$USERNAME
#RUN sudo chmod +x /home/$USERNAME/run_ipython.sh
#RUN sudo chown $USERNAME /home/$USERNAME/run_ipython.sh -R

# Jupyter
EXPOSE 8888

# TensorBoard
EXPOSE 6006

#CMD jupyter notebook --ip=0.0.0.0 --port=8888
#ARG JUPYTER_SCRIPT = /home/$USERNAME
#CMD ["/home/docker/run_ipython.sh"]
#CMD ["/bin/bash"]
ENTRYPOINT ["/bin/bash"]
