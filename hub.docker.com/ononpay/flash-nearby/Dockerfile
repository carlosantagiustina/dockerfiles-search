FROM node:8.4-alpine
MAINTAINER DUONG Dinh Cuong <cuong3ihut@gmail.com>

ENV LOG_LEVEL "info"
ENV GEO_SERVICE_PROVIDER "GEO-NEARBY"

COPY . /opt/location-service-provider
RUN apk --update add curl git ca-certificates python build-base libtool autoconf automake &&\
    cd /opt/location-service-provider && npm install &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -rf /var/cache/apk/*

WORKDIR "/opt/location-service-provider/"

EXPOSE 80

CMD ["npm","start"]