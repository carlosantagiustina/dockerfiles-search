#
# Dockerfile for privoxy
#

FROM alpine:latest
MAINTAINER chanfried.chung <chanfried.chung@gmail.com>

ARG TZ="Asia/Shanghai"

ENV TZ ${TZ}
ENV BROOK_VERSION 20180601
ENV BROOK_DOWNLOAD_URL https://github.com/txthinking/brook/releases/download/v${BROOK_VERSION}/brook

RUN apk upgrade --update \
    && apk add bash tzdata wget \
    && wget --no-check-certificate ${BROOK_DOWNLOAD_URL} -O /usr/local/bin/brook \
    && chmod +x /usr/local/bin/brook \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && apk del wget \
    && rm -rf /var/cache/apk/*

CMD ["brook"]
