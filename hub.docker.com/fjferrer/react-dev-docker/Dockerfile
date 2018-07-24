FROM node:8-alpine
MAINTAINER fjferrer

ENV SERVERLESS_VERSION=1.22.0

RUN npm install -g serverless@${SERVERLESS_VERSION}

RUN npm install -g react-scripts

RUN apk --no-cache update && \
    apk --no-cache add mysql-client python py-pip py-setuptools ca-certificates curl groff less && \
    pip --no-cache-dir install awscli && \
rm -rf /var/cache/apk/*
