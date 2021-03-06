FROM alpine:3.9

RUN \
    apk update && \
    apk upgrade && \
    apk add --no-cache --update \
            apache2-utils \
            busybox-extras \
            openssl \
            pcre \
            zlib \
            && \
    rm -rf /var/cache/apk/* && \
    openssl version

ARG NGINX_VERSION=1.15.9
ENV USER=nginx
WORKDIR /opt/cache-proxy

# --- build nginx with let and statsd module
RUN \
    addgroup ${USER} 2> /dev/null && \
    adduser -S ${USER} -G ${USER} -s /bin/sh 2> /dev/null && \
    # --- add dev packages
    DEV_PACKAGES='g++ git make openssl-dev pcre-dev wget zlib-dev' && \
    apk add --update --no-cache ${DEV_PACKAGES} && \
    # --- download nginx src
    wget --no-check-certificate -O nginx.tar.gz https://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
    tar zxvf nginx.tar.gz --strip-components 1 && \
    rm nginx.tar.gz && \
    # --- download memc-nginx client
    wget -O memc-nginx.tar.gz https://github.com/openresty/memc-nginx-module/archive/v0.19.tar.gz && \
    mkdir ./memc-nginx && \
    tar zxvf memc-nginx.tar.gz --strip-components 1 -C memc-nginx && \
    rm memc-nginx.tar.gz && \
    # --- download nginx let module
    # git clone --branch=master --single-branch --depth=1 https://github.com/arut/nginx-let-module ./modules/nginx-let && \
    # rm -rf ./modules/nginx-let/.git && \
    # --- download nginx statsd module
    git clone --branch=tags-12 --single-branch --depth=1 https://github.com/robbtraister/nginx-statsd ./modules/nginx-statsd && \
    rm -rf ./modules/nginx-statsd/.git && \
    # --- build nginx
    ./configure \
                --user=${USER} \
                --group=${USER} \
                --with-http_realip_module \
                # --add-module=modules/nginx-let \
                --add-module=modules/nginx-statsd \
                --add-module=./memc-nginx \
                && \
    make -j2 && \
    make install && \
    ln -sf `pwd`/objs/nginx /usr/bin/nginx && \
    rm -rf \
       ./CHANGES* \
       ./auto \
       ./modules \
       ./objs/src \
       ./src \
       && \
    nginx -v && \
    # --- remove dev packages
    apk del ${DEV_PACKAGES} && \
    rm -rf /var/cache/apk/* && \
    chown -R ${USER}:${USER} ./

RUN \
    mkdir -p \
          ./logs \
          ./tmp \
          && \
    ln -sf /dev/stdout ./logs/access.log && \
    ln -sf /dev/stderr ./logs/error.log && \
    chown -R ${USER}:${USER} \
          ./logs \
          ./tmp

COPY ./src ./src
RUN chown -R ${USER}:${USER} ./src

ARG RELEASE=beta
ENV RELEASE=${RELEASE}
RUN echo "${RELEASE}" > ./release.txt

VOLUME /opt/cache-proxy/tmp

# --- Use nginx user
USER ${USER}

CMD ./src/run.sh
