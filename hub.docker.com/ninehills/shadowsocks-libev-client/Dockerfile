FROM alpine

MAINTAINER ninehills "swulling@gmail.com"

ENV SHADOWSOCKS_LIBEV_VERSION v3.1.0
ENV SIMPLE_OBFS_VERSION v0.0.3

# Build shadowsocks-libev and simple-obfs
RUN set -ex \

    # Install dependencies
    && apk add --no-cache --virtual .build-deps \
               autoconf \
               automake \
               build-base \
               libev-dev \
               libtool \
               linux-headers \
               udns-dev \
               libsodium-dev \
               mbedtls-dev \
               pcre-dev \
               tar \
               udns-dev \
               c-ares-dev \
               git \

    # Build shadowsocks-libev
    && mkdir -p /tmp/build-shadowsocks-libev \
    && cd /tmp/build-shadowsocks-libev \
    && git clone https://github.com/shadowsocks/shadowsocks-libev.git \
    && cd shadowsocks-libev \
    && git checkout "$SHADOWSOCKS_LIBEV_VERSION" \
    && git submodule update --init --recursive \
    && ./autogen.sh \
    && ./configure --disable-documentation \
    && make install \
    && ssRunDeps="$( \
        scanelf --needed --nobanner /usr/local/bin/ss-server \
            | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
            | xargs -r apk info --installed \
            | sort -u \
    )" \
    && apk add --no-cache --virtual .ss-rundeps $ssRunDeps \
    && cd / \
    && rm -rf /tmp/build-shadowsocks-libev \

    # Build simple-obfs
    && mkdir -p /tmp/build-simple-obfs \
    && cd /tmp/build-simple-obfs \
    && git clone https://github.com/shadowsocks/simple-obfs.git \
    && cd simple-obfs \
    && git checkout "$SIMPLE_OBFS_VERSION" \
    && git submodule update --init --recursive \
    && ./autogen.sh \
    && ./configure --disable-documentation \
    && make install \
    && simpleObfsRunDeps="$( \
        scanelf --needed --nobanner /usr/local/bin/obfs-server \
            | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
            | xargs -r apk info --installed \
            | sort -u \
    )" \
    && apk add --no-cache --virtual .simple-obfs-rundeps $simpleObfsRunDeps \
    && cd / \
    && rm -rf /tmp/build-simple-obfs \
    
    # Delete dependencies
    && apk del .build-deps

# Shadowsocks environment variables
ENV SERVER_HOST 127.0.0.1
ENV SERVER_PORT 8388
ENV PASSWORD shadowsocks
ENV LISTEN_PORT 1080
ENV ENCRYPT_METHOD chacha20
ENV TIMEOUT 60
ENV PLUGIN obfs-local
ENV PLUGIN_OPTS "obfs=tls;obfs-host=itunes.apple.com"

EXPOSE $LISTEN_PORT/tcp $LISTEN_PORT/udp

# Start shadowsocks-libev server
ENTRYPOINT ss-local  -s "$SERVER_HOST" \
                     -p "$SERVER_PORT" \
                     -k "$PASSWORD" \
                     -l "$LISTEN_PORT" \
                     -m "$ENCRYPT_METHOD" \
                     -t "$TIMEOUT" \
                     --plugin "$PLUGIN" \
                     --plugin-opts "$PLUGIN_OPTS" \
                     -u \
                     -b 0.0.0.0 \
                     --fast-open
