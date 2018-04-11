# get current directoy using sheel
PWD = $(shell pwd)

# # get current user id using shell
USERID = $(shell id -u)

PORT_JUPYTER=7777
PORT_TENSORBOAD=3003

# docker images/containers name/tag
IMG_NAME = img_sb/price_clusterization
CONT_NAME = cont_sb_price_clusterization

# parameters inside docker, to be passed
VOLUME_NAME = classification
USERNAME = docker
PROJECT_DIR = projects

default:
	make docker-gpu-build
	make docker-gpu-run

# build a docker gpu image
docker-gpu-build:
#	echo "building docker-gpu with arguments...: USERNAME=$(USERNAME), PROJECT_DIR=$(PROJECT_DIR),  USERID=$(USERID)"
	docker build --rm --build-arg USERNAME=$(USERNAME) --build-arg PROJECT_DIR=$(PROJECT_DIR) --build-arg  USERID=$(USERID) \
		-t $(IMG_NAME) .

# launch  docker gpu container 
docker-gpu-run:
#	echo "run docker gpu...: exterbnal ports: $(PORT_JUPYTER) and $(PORT_TENSORBOAD), volume=$(PWD)"
	docker run --runtime=nvidia -e NVIDIA_VISIBLE_DEVICES=all\
		--name $(CONT_NAME) \
		-d -it \
		-p $(PORT_JUPYTER):8888 \
		-p $(PORT_TENSORBOAD):6006 \
		-v $(PWD):/home/$(USERNAME)/$(PROJECT_DIR)/$(VOLUME_NAME) \
		$(IMG_NAME)
