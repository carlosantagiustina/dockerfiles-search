#
# Image for Open DevOps Pipeline
#
#VERSION : 0.1
FROM docker.io/capitalone/hygieia-api

#Maintainer
MAINTAINER Open DevOps Team <open.devops@gmail.com>

#Environment variables
#ENV SPRING_DATA_MONGODB_HOST=localhost
#ENV SPRING_DATA_MONGODB_PORT=27017

#Install Mongo
RUN \
  apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 && \
  echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' > /etc/apt/sources.list.d/mongodb.list && \
  apt-get update && \
  apt-get install mongodb-org-shell

COPY init_hygiea_db.sh /hygieia/
CMD  sh init_hygiea_db.sh && \ 
   ./properties-builder.sh &&\
  java -Djava.security.egd=file:/dev/./urandom -jar api.jar --spring.config.location=/hygieia/dashboard.properties
