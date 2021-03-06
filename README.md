# Introduction

A repository containing some useful Docker files, setups for deep learning, mpc (model predictive controller) and other. We show how to use anaconda/virtualenv inside a docker container. 
There are 2 options to use run each docker container:
* using `make`
* using docker container directly

# Structure

- GPU
    *  (gpu docker file)[https://github.com/sbatururimi/docker/blob/master/dl-containers/gpu/Dockerfile] is a setup file for to run a container useful for Data science and machine learning tasks. It uses Ubuntu as OS system and setup a non-root user.

# Recommendations

Inside your containers, use Virtualenv whenever possible when building your models.

Possible alternative is [Anaconda](https://www.anaconda.com/what-is-anaconda/)

# Usinge  docker directly

1.

```
docker build  --rm -t <image tag> .
```

`--rm` is used to remove any intermediate and none images.

----
Settings in the Dockerfile

Key | Default  Value | Explanation
----|----------------|------------
USERNAME | docker | the user to be used instead of `root`
USERID | 1002 | the user `id` mapped from the host. Can be found with $id -u
PROJECT_DIR | projects | project folder that can be mapped to be constant

2. Run with a GPU

```
docker run --runtime=nvidia -e NVIDIA_VISIBLE_DEVICES=all -d -it  -p 8887:8888 <image tag>
```

3. Access the container:

```
docker exec -it <container ID> /bin/bash
```

# Using Make  

Take a look at the automatization of building and runing a docker container with make-build [here](https://github.com/sbatururimi/docker/blob/master/dl-containers/gpu/docker%2Bvirtualenv/Makefile)
```
make
```

# License

[![License: MIT](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://github.com/sbatururimi/docker/blob/master/LICENSE)
