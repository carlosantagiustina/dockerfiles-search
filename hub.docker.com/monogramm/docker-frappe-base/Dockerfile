FROM python:3.8-rc-slim-stretch

LABEL maintainer="Monogramm Maintainers <opensource at monogramm dot io>"

ENV DEBIAN_FRONTEND="noninteractive"
ENV WKHTMLTOX_VERSION 0.12.4
ENV DOCKERIZE_VERSION v0.6.1

RUN set -ex; \
    apt-get update; \
    apt-get install -y software-properties-common gnupg2 curl; \
    apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db; \
    add-apt-repository "deb http://ams2.mirrors.digitalocean.com/mariadb/repo/10.4/debian stretch main"; \
    curl -sL https://deb.nodesource.com/setup_8.x | bash -; \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -; \
    apt-get update; \
    apt-get install -y --allow-unauthenticated \
        sudo \
        wget \
        nodejs \
        python-mysqldb \
        git \
        build-essential \
        python3-setuptools \
        python3-dev \
        libffi-dev \
        libssl-dev  \
        ntp \
        screen \
        mariadb-common \
        libmariadbclient-dev \
        default-mysql-client \
        postgresql-client-common \
        libxslt1.1 \
        libxslt1-dev \
        libcrypto++-dev \
        python3-openssl \
        python3-ldap3 \
        libtiff5-dev \
        libjpeg62-turbo-dev \
        liblcms2-dev \
        libwebp-dev \
        tcl8.6-dev \
        tk8.6-dev \
        python-tk \
        zlib1g-dev \
        libfreetype6-dev \
        fontconfig \
        libxrender1 \
        libxext6 \
        xfonts-75dpi \
        xfonts-base \
    ; \
    npm install -g yarn; \
    rm -rf /var/lib/apt/lists/*; \
    wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/${WKHTMLTOX_VERSION}/wkhtmltox-${WKHTMLTOX_VERSION}_linux-generic-amd64.tar.xz; \
    tar xf wkhtmltox-${WKHTMLTOX_VERSION}_linux-generic-amd64.tar.xz; \
    mv wkhtmltox/bin/* /usr/local/bin/; \
    rm wkhtmltox-${WKHTMLTOX_VERSION}_linux-generic-amd64.tar.xz; \
    wget https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-linux-amd64-${DOCKERIZE_VERSION}.tar.gz; \
    tar -C /usr/local/bin -xzvf dockerize-linux-amd64-${DOCKERIZE_VERSION}.tar.gz; \
    rm dockerize-linux-amd64-${DOCKERIZE_VERSION}.tar.gz; \
    pip install --upgrade setuptools pip pip-tools; \
    pip --version
