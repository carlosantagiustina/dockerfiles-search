FROM alpine:3.10

LABEL maintainer "laogao <endoffight@gmail.com>"
LABEL keyword "alpine python scrapy quick 1.8.0"

ENV VERSION=1.8.0

RUN set -ex && \
        apk add --no-cache --virtual .fetch-deps \
            git \
            && \
        git clone -b $VERSION https://github.com/scrapy/scrapy.git /usr/src/scrapy && \
        apk add --no-cache python3 && \
        apk add --no-cache --virtual .build-deps \
            py3-openssl \
            py3-twisted \
            py3-cffi \
            py3-cryptography \
            py3-lxml && \
        apk del .fetch-deps && \
        pip3 install -r /usr/src/scrapy/requirements-py3.txt && \
        cd /usr/src/scrapy && \
        python3 setup.py install && \
        cd / && \
        rm -rf /usr/src/scrapy

CMD ["ash"]