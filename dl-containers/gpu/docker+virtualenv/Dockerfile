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

# for Python 3.n
RUN apt-get update \
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

# set the folder
# 1) uncomment if not launched from `make`
ARG PROJECT_DIR=projects
# 2) with `make` and arguments passed during the build command
#ARG PROJECT_DIR

RUN mkdir -p /home/$USERNAME/$PROJECT_DIR
WORKDIR /home/$USERNAME/$PROJECT_DIR

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
