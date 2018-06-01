# How to use

1. Install (docker)[https://docs.docker.com/docker-for-mac/install/].
2. Build a docker image using the Dockerfile that can be found in this folder. That is run you terminal from this folder with following command:

```
$ docker build -t mpc_image .
```
You can replace `mpc_image` with any wanted name;
3. Run the container:

```
docker run -it -v {{ your CarND-MPC-Project dir }}:/home/ubuntu/work -p 4567:4567 mpc_image
```

Replace "{{ your CarND-MPC-Project dir }}" with the directory of your project's repo.  Here's an example:
```
docker run -it -v /Users/stas/work/carnd/CarND-MPC-Project:/home/ubuntu/work -p 4567:4567 mpc_image bash
```

Once the docker container is running, you should be logged in as the ubuntu user and your current directory will be your project's repo directory.  You should be able to compile the code.  When you run mpc in the container, it will bind to port 4567 on your localhost and be able to connect to the simulator.