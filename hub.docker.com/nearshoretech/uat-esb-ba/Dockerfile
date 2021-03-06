# ------------------------------------------------------------------------
#
# Copyright 2018 WSO2, Inc. (http://wso2.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License
#
# ------------------------------------------------------------------------

# set to latest Ubuntu LTS
FROM ubuntu:18.04
MAINTAINER WSO2 Docker Maintainers "dev@wso2.org"

# set user configurations
ARG USER=wso2carbon
ARG USER_ID=802
ARG USER_GROUP=wso2
ARG USER_GROUP_ID=802
ARG USER_HOME=/home/${USER}
# set dependant files directory
ARG DOWNLOAD_URL=https://s3-us-west-2.amazonaws.com/nst-itsol/esb-6-4-0
ARG FILES=/home/${USER}/files
# set jdk configurations
ARG JDK=jdk1.8.0*
ARG JAVA_HOME=${USER_HOME}/java
# set wso2 product configurations
ARG WSO2_SERVER=wso2ei
ARG WSO2_SERVER_VERSION=6.4.0
ARG WSO2_SERVER_PACK=${WSO2_SERVER}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${USER_HOME}/${WSO2_SERVER_PACK}
# set WSO2 EULA
ARG MOTD="\n\
Welcome to WSO2 Docker resources.\n\
------------------------------------ \n\
This Docker container comprises of a WSO2 product, running with its latest GA release \n\
which is under the Apache License, Version 2.0. \n\
Read more about Apache License, Version 2.0 here @ http://www.apache.org/licenses/LICENSE-2.0.\n"

# install required packages
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    curl \
    unzip \
    netcat && \
    rm -rf /var/lib/apt/lists/* && \
    echo '[ ! -z "$TERM" -a -r /etc/motd ] && cat /etc/motd' \
    >> /etc/bash.bashrc \
    ; echo "$MOTD" > /etc/motd

# create a user group and a user
RUN groupadd --system -g ${USER_GROUP_ID} ${USER_GROUP} && \
    useradd --system --create-home --home-dir ${USER_HOME} --no-log-init -g ${USER_GROUP_ID} -u ${USER_ID} ${USER} && \
    mkdir ${FILES}

ADD --chown=wso2carbon:wso2 ${DOWNLOAD_URL}/wso2ei-6.4.0.zip ${FILES}
ADD --chown=wso2carbon:wso2 ${DOWNLOAD_URL}/jdk-8u192-linux-x64.tar.gz ${FILES}
ADD --chown=wso2carbon:wso2 ${DOWNLOAD_URL}/mysql-connector-java-5.1.45-bin.jar ${FILES}
ADD --chown=wso2carbon:wso2 ${DOWNLOAD_URL}/mssql-jdbc-6.4.0.jre8.jar ${FILES}

RUN tar -zxvf ${FILES}/jdk-8u192-linux-x64.tar.gz -C ${FILES}
RUN mv ${FILES}/jdk1.8.0_192 ${JAVA_HOME}
RUN unzip ${FILES}/wso2ei-6.4.0.zip -d ${USER_HOME}
RUN mv ${FILES}/mysql-connector-java-5.1.45-bin.jar ${WSO2_SERVER_HOME}/lib
RUN mv ${FILES}/mssql-jdbc-6.4.0.jre8.jar ${WSO2_SERVER_HOME}/lib

RUN chown -R wso2carbon:wso2 ${JAVA_HOME}
RUN chown -R wso2carbon:wso2 ${WSO2_SERVER_HOME}

RUN rm -vR ${FILES}


# copy jdk, wso2 product distribution to user's home directory and copy mysql connector jar to product distribution
#COPY --chown=wso2carbon:wso2 ${FILES}/${JDK} ${JAVA_HOME}
#COPY --chown=wso2carbon:wso2 ${FILES}/${WSO2_SERVER_PACK} ${WSO2_SERVER_HOME}
#COPY --chown=wso2carbon:wso2 ${FILES}/mysql-connector-java-*-bin.jar ${WSO2_SERVER_HOME}/lib
# add libraries for Kubernetes membership scheme based clustering
#ADD --chown=wso2carbon:wso2 https://repo1.maven.org/maven2/dnsjava/dnsjava/2.1.8/dnsjava-2.1.8.jar ${WSO2_SERVER_HOME}/lib
#ADD --chown=wso2carbon:wso2 https://repo1.maven.org/maven2/org/wso2/carbon/kubernetes/artifacts/kubernetes-membership-scheme/1.0.5/kubernetes-membership-scheme-1.0.5.jar ${WSO2_SERVER_HOME}/dropins

# set the user and work directory
USER ${USER}
WORKDIR ${USER_HOME}

# set environment variables
ENV JAVA_HOME=${JAVA_HOME} \
    PATH=$JAVA_HOME/bin:$PATH \
    WSO2_SERVER_HOME=${WSO2_SERVER_HOME} \
    WORKING_DIRECTORY=${USER_HOME}
