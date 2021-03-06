FROM ubuntu:xenial

LABEL maintainer="Arjan van Eersel <arjan@sonemas.com>"

# Install dependencies
RUN apt-get update -y && apt-get install -y \
	git \
	wget \
	python3.5 \
	python3-pip \
	python-setuptools \
	python3-nacl \
	apt-transport-https \
    apt-utils \
	ca-certificates \
    supervisor \ 
    build-essential \
    software-properties-common \
    vim

RUN pip3 install -U \
	pip==9.0.3 \
    setuptools

# Install indy tools
ARG indy_plenum_ver=1.9.0~dev808
ARG indy_node_ver=1.9.0~dev987
ARG python3_indy_crypto_ver=0.4.5
ARG indy_crypto_ver=0.4.5
ARG indy_stream=master
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 68DB5E88
RUN echo "deb https://repo.sovrin.org/sdk/deb xenial $indy_stream" >> /etc/apt/sources.list
RUN echo "deb https://repo.sovrin.org/deb xenial $indy_stream" >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y libindy
RUN apt-get install -y indy-plenum=${indy_plenum_ver} \
    indy-node=${indy_node_ver} \
    python3-indy-crypto=${python3_indy_crypto_ver} \
    libindy-crypto=${indy_crypto_ver}

# Install Go
RUN cd /tmp && \\
wget https://dl.google.com/go/go1.12.6.linux-amd64.tar.gz && \\
tar -C /usr/local -xzf /tmp/go1.12.6.linux-amd64.tar.gz
ENV PATH="$PATH:/usr/local/go/bin"

# Setup Indy pool
RUN echo "[supervisord]\n\
logfile = /tmp/supervisord.log\n\
logfile_maxbytes = 50MB\n\
logfile_backups=10\n\
logLevel = error\n\
pidfile = /tmp/supervisord.pid\n\
nodaemon = true\n\
minfds = 1024\n\
minprocs = 200\n\
umask = 022\n\
user = indy\n\
identifier = supervisor\n\
directory = /tmp\n\
nocleanup = true\n\
childlogdir = /tmp\n\
strip_ansi = false\n\
\n\
[program:node1]\n\
command=start_indy_node Node1 0.0.0.0 9701 0.0.0.0 9702\n\
directory=/home/indy\n\
stdout_logfile=/tmp/node1.log\n\
stderr_logfile=/tmp/node1.log\n\
\n\
[program:node2]\n\
command=start_indy_node Node2 0.0.0.0 9703 0.0.0.0 9704\n\
directory=/home/indy\n\
stdout_logfile=/tmp/node2.log\n\
stderr_logfile=/tmp/node2.log\n\
\n\
[program:node3]\n\
command=start_indy_node Node3 0.0.0.0 9705 0.0.0.0 9706\n\
directory=/home/indy\n\
stdout_logfile=/tmp/node3.log\n\
stderr_logfile=/tmp/node3.log\n\
\n\
[program:node4]\n\
command=start_indy_node Node4 0.0.0.0 9707 0.0.0.0 9708\n\
directory=/home/indy\n\
stdout_logfile=/tmp/node4.log\n\
stderr_logfile=/tmp/node4.log\n"\
>> /etc/supervisord.conf

ARG pool_ip=127.0.0.1

RUN awk '{if (index($1, "NETWORK_NAME") != 0) {print("NETWORK_NAME = \"sandbox\"")} else print($0)}' /etc/indy/indy_config.py> /tmp/indy_config.py
RUN mv /tmp/indy_config.py /etc/indy/indy_config.py
RUN generate_indy_pool_transactions --nodes 4 --clients 5 --nodeNum 1 2 3 4 --ips="$pool_ip,$pool_ip,$pool_ip,$pool_ip"
RUN /usr/bin/supervisord &