# docker

A repository containing some useful Docker files, setups for deep learning.
There are 2 options to use run each docker container:
* using `make`
* using docker container directly

# Containers
The (gpu docker file)[https://github.com/sbatururimi/docker/blob/master/dl-containers/gpu/Dockerfile] is a setup file for to run a container useful for Data science and machine learning tasks.
Run in a Ubuntu

## With Virtualenv

We can find it [here](https://github.com/sbatururimi/docker/tree/master/dl-containers/gpu/docker%2Bvirtualenv)

* To build the docker image directly:
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

* With make, we can change all settings in the Makefile:
```
make
```
