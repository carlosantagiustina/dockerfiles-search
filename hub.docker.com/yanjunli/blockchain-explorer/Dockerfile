# Dockerfile for Hyperledger blockchain-explorer image
# Workdir is set to /blockchain-explorer/explorer_1
# env:
# HTTP_PORT=<web server port. Default is 9090 if not set>
# HYP_REST_ENDPOINT=<REST endpoint. Default is http://127.0.0.1:7050 if not set>

FROM node:9
MAINTAINER yanjunli <allovertheword@163.com>

# install maintain tools
#RUN npm install bower grunt-cli graceful-fs@4.1.5 minimatch@3.0.2 -g

# clone latest code from github
RUN git clone https://github.com/hyperledger/blockchain-explorer.git

WORKDIR /blockchain-explorer

#RUN echo '{ "allow_root": true }' > /root/.bowerrc
RUN echo '{ "allow_root": true }' > .bowerrc

# Modify config.json to update the value of channels/mysql/tls
# Or just mount external one inside

RUN npm install

VOLUME /blockchain-explorer

CMD ["bash", "start.sh"]
