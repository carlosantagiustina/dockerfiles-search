FROM alpine:3.11
LABEL maintainer "DI GREGORIO Nicolas <nicolas.digregorio@gmail.com>"

### Environment variables
ENV LANG='en_US.UTF-8' \
    LANGUAGE='en_US.UTF-8' \
    TERM='xterm'  \
    GIT_BRANCH='master' \
    APPUSER='sickchill' \
    APPUID='10000' \
    APPGID='10000'

# Copy config files
COPY root/ /

### Install Application
RUN set -x && \
    chmod 1777 /tmp && \
    . /usr/local/bin/docker-entrypoint-functions.sh && \
    MYUSER=${APPUSER} && \
    MYUID=${APPUID} && \
    MYGID=${APPGID} && \
    ConfigureUser && \
    apk --no-cache upgrade && \
    apk add --no-cache --virtual=build-deps \
      make \
      gcc \
      g++ \
      python2-dev \
      py2-pip \
      libressl-dev \
      libffi-dev \
      libxml2-dev\
      libxslt-dev\
    && \
    apk add --no-cache --virtual=run-deps \
      bash \
      ca-certificates \
      python2 \ 
      libxml2 \
      libxslt \
      nodejs \
      unrar  \
      su-exec \
      git \
    && \
    pip --no-cache-dir install --upgrade setuptools && \
    pip --no-cache-dir install --upgrade pyopenssl cheetah cfscrape lxml && \
    git clone --depth 1 --branch ${GIT_BRANCH} https://github.com/SickChill/SickChill.git /opt/sickchill && \
    apk del --no-cache --purge \
      build-deps  \
    && \
    mkdir /docker-entrypoint.d && \
    chmod +x /usr/local/bin/docker-entrypoint.sh && \
    ln -snf /usr/local/bin/docker-entrypoint.sh /docker-entrypoint.sh && \
    rm -rf /opt/sickchill/.git* \
           /tmp/* \
           /var/cache/apk/*  \
           /var/tmp/*

### Volume
VOLUME ["/config", "/downloads", "/tv", "/animes"]

### Expose ports
EXPOSE 8081

### Running User: not used, managed by docker-entrypoint.sh
#USER sickchill

### Start Sickrage
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["sickchill"]
