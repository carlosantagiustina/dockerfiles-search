FROM centos:7
MAINTAINER "Tom" <tom_hejiaxi@163.com>

RUN yum install -y git \
        gcc \
        gcc-c++ \
        gd-devel \
        gettext \
        GeoIP-devel \
        libxslt-devel \
        make \
        perl \
        perl-ExtUtils-Embed \
        readline-devel \
        unzip \
        zlib-devel \
        patch

RUN curl -sL https://rpm.nodesource.com/setup_10.x | bash - \
    && yum install -y nodejs

WORKDIR /app

COPY app /app

RUN cd /app; \
    npm install; \
    npm audit fix

COPY entrypoint.sh /entrypoint.sh

RUN mkdir /app/dist

VOLUME ["/webpack/src", "/app/dist"]
ENTRYPOINT ["/entrypoint.sh"]