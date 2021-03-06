# Dockerfile fork from https://github.com/blocknetdx/dockerimages.git branch digibyte-v6.16.5.1
# Build via docker:
# docker build --build-arg cores=8 -t blocknetdx/dgb:latest .

FROM ubuntu:xenial

ARG cores=4
ENV ecores=$cores
ENV DGB_VER=v7.17.2

RUN apt update \
  && apt install -y --no-install-recommends \
     software-properties-common \
     ca-certificates \
     wget curl git python vim \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN add-apt-repository ppa:bitcoin/bitcoin \
  && apt update \
  && apt install -y --no-install-recommends \
     curl build-essential libtool autotools-dev automake \
     python3 bsdmainutils cmake libevent-dev autoconf automake \
     pkg-config libssl-dev libboost-system-dev libboost-filesystem-dev \
     libboost-chrono-dev libboost-program-options-dev libboost-test-dev \
     libboost-thread-dev libdb4.8-dev libdb4.8++-dev libgmp-dev \
     libminiupnpc-dev libzmq3-dev \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Build source
RUN mkdir -p /opt/digibyte \
  && mkdir -p /opt/blockchain/config \
  && mkdir -p /opt/blockchain/data \
  && ln -s /opt/blockchain/config /root/.digibyte \
  && cd /opt/digibyte \
  && mkdir digibyte \
  && git clone --depth 1 --branch $DGB_VER https://github.com/digibyte/digibyte digibyte \
  && cd /opt/digibyte/digibyte/ \
  && cd depends \
  && make -j$ecores NO_QT=1 \
  && cd .. \
  && chmod +x ./autogen.sh \
  && ./autogen.sh \
  && ./configure --with-gui=no --enable-hardening --prefix=`pwd`/depends/x86_64-pc-linux-gnu \
  && make -j$ecores \
  && cp /opt/digibyte/digibyte/src/digibyted /opt/blockchain/digibyted \
  && cp /opt/digibyte/digibyte/src/digibyte-cli /opt/blockchain/digibyte-cli \
  && rm -rf /opt/digibyte/

# Write default digibyte.conf (can be overridden on commandline)
RUN echo "datadir=/opt/blockchain/data  \n\
                                        \n\
dbcache=256                             \n\
maxmempool=512                          \n\
port=12024                               \n\
rpcport=14022                          \n\
listen=1                                \n\
server=1                                \n\
logtimestamps=1                         \n\
logips=1                                \n\
rpcthreads=8                            \n\
rpcallowip=127.0.0.1                    \n\
rpctimeout=15                           \n\
rpcclienttimeout=15                     \n" > /opt/blockchain/config/digibyte.conf

WORKDIR /opt/blockchain/
VOLUME ["/opt/blockchain/config", "/opt/blockchain/data"]

# Port, RPC, Test Port, Test RPC
EXPOSE 12024 14022  18332  19332

CMD ["/opt/blockchain/digibyted", "-daemon=0"]