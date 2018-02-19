# image to build from
ARG os=ubuntu
ARG version=16.04

FROM ${os}:${version}


MAINTAINER Stas Batururimi <s.batururimi@gmail.com>

# NVIDIA driver: https://www.geforce.com/drivers
# Up to date as February 2018 
ENV DRIVER_VERSION 390.25

# Install some dependencies
RUN apt-get -y update \
    && apt-get -y install wget git bc make dpkg-dev libssl-dev module-init-tools \
    && apt-get autoremove \
    && apt-get clean

#RUN apt-get -y install --reinstall linux-image-`uname -r` 
#RUN depmod
#RUN modprobe configs
# kernel modules
# debug (comment the block when building a production image
#RUN /bin/echo -e "\n------[Debug]\n"
#RUN /bin/bash -c 'uname -r'
#RUN /bin/echo -e "\n\n"
#RUN /bin/bash -c 'uname -r | sed -e "s/-.*//"'
#RUN /bin/echo -e "\n"
#RUN /bin/bash -c  'uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]*$//"'
#RUN /bin/echo -e "\n\n"
#RUN /bin/bash -c 'ls /proc/*' 
#RUN /bin/bash -c "find /proc/ -name '*.gz'" 
#RUN /bin/echo -e "\n------[Debug end]\n"
# debug end


RUN  mkdir -p /usr/src/kernels \
    && cd /usr/src/kernels \
    && git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git --single-branch --depth 1 --branch v`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]*$//"`  linux \
    && cd linux \
    && git checkout -b stable v`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]*$//"` \
   # && modprobe configs \
    #&& zcat /proc/config.gz > .config \
    && cat /boot/config-`uname -r` > .config \
    && make modules_prepare \
    && sed -i -e "s/`uname -r | sed -e "s/-.*//" | sed -e "s/\.[0]??*$//"`/`uname -r`/" include/generated/utsrelease.h # In case a '+' was added

# NVIDIA driver
RUN mkdir -p /opt/nvidia && cd /opt/nvidia/ \
    && wget http://us.download.nvidia.com/XFree86/Linux-x86_64/${DRIVER_VERSION}/NVIDIA-Linux-x86_64-${DRIVER_VERSION}.run -O /opt/nvidia/driver.run \ 
    && chmod +x /opt/nvidia/driver.run \
    && /opt/nvidia/driver.run -a -x --ui=none

ENV NVIDIA_INSTALLER /opt/nvidia/NVIDIA-Linux-x86_64-${DRIVER_VERSION}/nvidia-installer
CMD ${NVIDIA_INSTALLER} -q -a -n -s --kernel-source-path=/usr/src/kernels/linux/ \
    && modprobe nvidia \
    && modprobe nvidia-uvm

# ONBUILD, we install the NVIDIA driver and the cuda libraries
# You can find the wanted version here: https://developer.nvidia.com/cuda-toolkit-archive 
ONBUILD ENV CUDA_VERSION 9.0.176

ONBUILD RUN /opt/nvidia/driver.run --silent --no-kernel-module --no-unified-memory --no-opengl-files
ONBUILD RUN wget --no-check-certificate http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_${CUDA_VERSION}-1_amd64.deb \
    && dpkg -i cuda-repo-ubuntu1604_${CUDA_VERSION}-1_amd64.deb \
    && apt-get -y update \
    && apt-get -y install --no-install-suggests --no-install-recommends \
        cuda-command-line-tools-9.0 \
        cuda-nvgraph-dev-9.0 \
        cuda-cusparse-dev-9.0 \
        cuda-cublas-dev-9.0 \
        cuda-curand-dev-9.0 \
        cuda-cufft-dev-9.0 \
        cuda-cusolver-dev-9.0 \
    && sed -i 's#"$#:/usr/local/cuda-9.0/bin"#' /etc/environment \
    && rm cuda-repo-ubuntu1604_${CUDA_VERSION}-1_amd64.deb \
    && cd /usr/local/cuda-9.0 && ln -s . cuda \
    && wget http://developer.download.nvidia.com/compute/redist/cudnn/v7.0.5/cudnn-9.0-linux-x64-v7.0.5.tgz \
    && tar -xf cudnn-9.0-linux-x64-v7.0.5.tgz \
    && rm nn-9.0-linux-x64-v7.0.5.tgz
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/cuda-9.0/bin
