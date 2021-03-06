FROM ubuntu:14.04

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

RUN sudo apt-get update && apt-get install -y \
  wget \
  curl \
  libcurl4-openssl-dev \
  unzip

RUN sudo apt-get install -y build-essential python-dev libffi-dev python-pip

RUN sudo apt-get update && apt-get install -y gcc

ENV VERSION 8
ENV UPDATE 45
ENV BUILD 14

ENV JAVA_HOME /usr/lib/jvm/java-${VERSION}-oracle

RUN apt-get update && apt-get install ca-certificates curl -y && \
    curl --silent --location --retry 3 --cacert /etc/ssl/certs/GeoTrust_Global_CA.pem \
    --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
    http://download.oracle.com/otn-pub/java/jdk/"${VERSION}"u"${UPDATE}"-b"${BUILD}"/jdk-"${VERSION}"u"${UPDATE}"-linux-x64.tar.gz \
    | tar xz -C /tmp && \
    mkdir -p /usr/lib/jvm && mv /tmp/jdk1.${VERSION}.0_${UPDATE} "${JAVA_HOME}" && \
    apt-get autoclean && apt-get --purge -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN update-alternatives --install "/usr/bin/java" "java" "${JAVA_HOME}/bin/java" 1
RUN update-alternatives --install "/usr/bin/javaws" "javaws" "${JAVA_HOME}/bin/javaws" 1
RUN update-alternatives --install "/usr/bin/javac" "javac" "${JAVA_HOME}/bin/javac" 1
RUN update-alternatives --set java "${JAVA_HOME}/bin/java"
RUN update-alternatives --set javaws "${JAVA_HOME}/bin/javaws"
RUN update-alternatives --set javac "${JAVA_HOME}/bin/javac"
