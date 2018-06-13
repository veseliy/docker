# image to build from
FROM node:10

MAINTAINER Stas Batururimi <s.batururimi@gmail.com>

# clone the cks5 server repository
ARG rep=/home/socks5

RUN cd /home
RUN git clone https://github.com/brozeph/simple-socks.git $rep
RUN cd $rep

# install the  socks 5 nodejs app
RUN npm install simple-socks

# install pm2
RUN npm install pm2@latest -g

WORKDIR $rep

EXPOSE 1080

# start the simple socks server
#CMD ["bash"]
CMD ["pm2-runtime", "examples/createServer.js"]