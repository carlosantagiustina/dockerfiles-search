FROM alpine:3.7

LABEL maintainer="DJ Enriquez <denrie.enriquezjr@gmail.com> (@djenriquez)"

RUN addgroup nomad && \
    adduser -S -G nomad nomad

ENV GLIBC_VERSION "2.25-r0"
ENV GOSU_VERSION 1.10
ENV DUMB_INIT_VERSION 1.2.0

RUN set -x && \
    apk --update add --no-cache --virtual .gosu-deps dpkg curl gnupg && \
    curl -L -o /tmp/glibc-${GLIBC_VERSION}.apk https://github.com/andyshinn/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk && \
    apk add --allow-untrusted /tmp/glibc-${GLIBC_VERSION}.apk && \
    rm -rf /tmp/glibc-${GLIBC_VERSION}.apk /var/cache/apk/* && \
    curl -L -o /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v${DUMB_INIT_VERSION}/dumb-init_${DUMB_INIT_VERSION}_amd64 && \
    chmod +x /usr/local/bin/dumb-init && \
    dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" && \
    curl -L -o /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch" && \
    curl -L -o /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc" && \
    export GNUPGHOME="$(mktemp -d)" && \
    for server in $(shuf -e ha.pool.sks-keyservers.net \
                            hkp://p80.pool.sks-keyservers.net:80 \
                            keyserver.ubuntu.com \
                            hkp://keyserver.ubuntu.com:80 \
                            pgp.mit.edu) ; do \
        gpg --keyserver "$server" --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 && break || : ; \
    done && \
    gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu && \
    rm -rf "$GNUPGHOME" /usr/local/bin/gosu.asc && \
    chmod +x /usr/local/bin/gosu && \
    gosu nobody true && \
    apk del .gosu-deps

ENV NOMAD_VERSION 0.7.1

RUN set -x \
  && apk --update add --no-cache --virtual .nomad-deps gnupg curl \
  && cd /tmp \
  && curl -L -o nomad_${NOMAD_VERSION}_linux_amd64.zip https://releases.hashicorp.com/nomad/${NOMAD_VERSION}/nomad_${NOMAD_VERSION}_linux_amd64.zip \
  && curl -L -o nomad_${NOMAD_VERSION}_SHA256SUMS      https://releases.hashicorp.com/nomad/${NOMAD_VERSION}/nomad_${NOMAD_VERSION}_SHA256SUMS \
  && curl -L -o nomad_${NOMAD_VERSION}_SHA256SUMS.sig  https://releases.hashicorp.com/nomad/${NOMAD_VERSION}/nomad_${NOMAD_VERSION}_SHA256SUMS.sig \
  && export GNUPGHOME="$(mktemp -d)" \
  && for server in $(shuf -e ha.pool.sks-keyservers.net \
                            hkp://p80.pool.sks-keyservers.net:80 \
                            keyserver.ubuntu.com \
                            hkp://keyserver.ubuntu.com:80 \
                            pgp.mit.edu) ; do \
        gpg --keyserver "$server" --recv-keys 91A6E7F85D05C65630BEF18951852D87348FFC4C && break || : ; \
     done \
  && gpg --batch --verify nomad_${NOMAD_VERSION}_SHA256SUMS.sig nomad_${NOMAD_VERSION}_SHA256SUMS \
  && grep nomad_${NOMAD_VERSION}_linux_amd64.zip nomad_${NOMAD_VERSION}_SHA256SUMS | sha256sum -c \
  && unzip -d /bin nomad_${NOMAD_VERSION}_linux_amd64.zip \
  && chmod +x /bin/nomad \
  && rm -rf "$GNUPGHOME" nomad_${NOMAD_VERSION}_linux_amd64.zip nomad_${NOMAD_VERSION}_SHA256SUMS nomad_${NOMAD_VERSION}_SHA256SUMS.sig \
  && apk del .nomad-deps

RUN mkdir -p /nomad/data && \
    mkdir -p /etc/nomad && \
    chown -R nomad:nomad /nomad

RUN apk --update add ca-certificates && rm -rf /var/cache/apk/*

EXPOSE 4646 4647 4648 4648/udp

ADD start.sh /usr/local/bin/start.sh

ENTRYPOINT ["/usr/local/bin/start.sh"]
