# docker images/containers name/tag
IMG_NAME = img_socks5
CONT_NAME = cont_socks5

default: 
        make docker-socks5-build
        make docker-socks5-run

# build a socks5 docker image
docker-socks5-build:
        docker build --rm -t $(IMG_NAME) .

# run the build docker image
docker-socks5-run:
        docker run --name $(CONT_NAME) \
                -d -it \
                -p 1080:1080 \
                $(IMG_NAME)