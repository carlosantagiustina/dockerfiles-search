FROM java:openjdk-8

LABEL mantainer "Giuseppe Iannelli a.k.a Barba <giuseppe.iannelli@mosaicoon.com>"

#ENV for Debian
ENV DEBIAN_FRONTEND noninteractive

#SOFTWARE VERSION
ARG SCALA_VERSION=2.12
ARG KAFKA_VERSION=0.10.2.1

#JAVA ENV
ENV _JAVA_OPTIONS="-Djava.net.preferIPv4Stack=true"

# RMI_PORT must be JMX_PORT + 1
# ENV RMI_PORT 10000
# ENV JMX_PORT 9999

#ZOOKEEPER ENV
ENV MAX_CLIENT_CNXNS 60
ENV TICK_TIME 2000
ENV INIT_LIMIT 10
ENV SYNC_LIMIT 5

#set to true for single node or development environment
ENV STANDALONE_ENABLED true

ENV ZOOKEEPER_SERVER_PORT=2181
ENV ZOOKEEPER_SERVER_PORT_LEADER=2888
ENV ZOOKEEPER_SERVER_PORT_LEADER_ELECTION=3888

#if run replicated zookeeper, set ZOOKEEPER_SERVER_IP_LIST as ip1:leader_port:leader_election_port,ip2:leader_port:leader_election_port,....
ENV ZOOKEEPER_SERVER_ID 1
ENV ZOOKEEPER_SERVER_IP_LIST localhost:2888:3888

#INSTALL PACKAGES
RUN apt update -y
RUN apt install wget gnupg gettext-base -y

WORKDIR /tmp

#DOWNLOAD KAFKA
RUN wget http://www-eu.apache.org/dist/kafka/${KAFKA_VERSION}/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz http://www-eu.apache.org/dist/kafka/${KAFKA_VERSION}/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz.asc
#VERIFY KAFKA PACKAGES
RUN wget http://kafka.apache.org/KEYS
RUN gpg --import KEYS
RUN gpg --verify kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz.asc kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz

#EXTRACT KAFKA
RUN mkdir -p /opt /tmp/zookeeper
RUN $(which tar) -xzvf kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz -C /opt

RUN rm -rf KEYS kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz.asc kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz

RUN mv /opt/kafka_${SCALA_VERSION}-${KAFKA_VERSION} /opt/kafka

WORKDIR /opt/kafka

COPY scripts/zookeeper/entrypoint.sh entrypoint.sh

COPY config/zookeeper/zookeeper.properties.template config/zookeeper.properties.template

EXPOSE ${ZOOKEEPER_SERVER_PORT} ${ZOOKEEPER_SERVER_PORT_LEADER} ${ZOOKEEPER_SERVER_PORT_LEADER_ELECTION} ${JMX_PORT} ${RMI_PORT}

ENTRYPOINT ["/bin/sh", "/opt/kafka/entrypoint.sh"]
