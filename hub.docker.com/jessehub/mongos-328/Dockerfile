# Dockerizing MongoDB: Dockerfile for building MongoDB images
# installs MongoDB following the instructions from:
# http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/

FROM ubuntu:14.04

ARG TERM=linux
ARG DEBIAN_FRONTEND=noninteractive

# Basic security patch 
RUN apt-get update && apt-get install -yq apt-transport-https && apt-get dist-upgrade --force-yes -fuy -o Dpkg::Options::="--force-confnew --force-confdef"

# Import MongoDB public GPG key AND create a MongoDB list file
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
RUN echo "deb https://repo.mongodb.org/apt/ubuntu $(cat /etc/lsb-release | grep DISTRIB_CODENAME | cut -d= -f2)/mongodb-org/3.2 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.2.list

# Update apt-get sources AND install MongoDB
RUN apt-get update && \
    apt-get install -y mongodb-org-mongos=3.2.8

# Create the MongoDB data directory
RUN mkdir -p /data/db

# Expose port #27017 from the container to the host
EXPOSE 27017

# Set /usr/bin/mongod as the dockerized entry-point application
ENTRYPOINT ["/usr/bin/mongos"]
