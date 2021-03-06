FROM php:7.2.23-alpine AS builder

ENV PHP_INI_DIR  "/etc/php"
ENV UNIT_VERSION "1.12.0"

RUN apk add --no-cache --quiet \
            argon2-dev \
            bison \
            bzip2-dev \
            autoconf \
            coreutils \
            curl-dev \
            db-dev \
            dpkg \
            dpkg-dev \
            file \
            freetype-dev \
            g++ \
            gcc \
            gd-dev \
            gdbm-dev \
            gettext-dev \
            git \
            gmp-dev \
            icu-dev \
            imap-dev \
            krb5-dev \
            libc-dev \
            libedit-dev \
            libical-dev \
            libjpeg-turbo-dev \
            libpng-dev \
            libsodium-dev \
            libwebp-dev \
            libxml2-dev \
            libxpm-dev \
            libxslt-dev \
            libzip-dev \
            make \
            musl-dev \
            openldap-dev \
            openssl-dev \
            pcre-dev \
            pkgconf \
            re2c \
            sqlite-dev \
            tidyhtml-dev \
            zlib-dev

WORKDIR /usr/src/php

# https://github.com/docker-library/php/blob/master/7.2/alpine3.10/fpm/Dockerfile
# https://git.alpinelinux.org/aports/tree/community/php7/APKBUILD
RUN set -ex \
  ; export CFLAGS="${PHP_CFLAGS}" \
           CPPFLAGS="${PHP_CPPFLAGS}" \
           LDFLAGS="${PHP_LDFLAGS}" \
  ; docker-php-source extract \
  ; mkdir -p "${PHP_INI_DIR}/conf.d" \
  ; ./configure build_alias=x86_64-linux-musl \
                --quiet \
                --prefix=/usr \
                --datadir=/usr/share/php \
                --sysconfdir=/etc/php \
                --localstatedir=/var \
                --build=x86_64-linux-musl \
                --enable-option-checking=fatal \
                --enable-embed=shared \
                --disable-phpdbg \
                --disable-short-tags \
                --enable-bcmath=shared \
                --enable-calendar=shared \
                --enable-ctype=shared \
                --enable-dom=shared \
                --enable-exif=shared \
                --enable-fileinfo=shared \
                --enable-ftp=shared \
                --enable-intl=shared \
                --enable-json=shared \
                --enable-libxml \
                        --with-libxml-dir=/usr \
                --enable-pdo=shared \
                        --with-pdo-mysql=shared,mysqlnd \
                        --with-pdo-sqlite=shared,/usr \
                --enable-session=shared \
                --enable-shmop=shared \
                --enable-simplexml=shared \
                --enable-sockets=shared \
                --with-ldap=shared \
                        --with-ldap-sasl \
                --enable-mbstring=shared \
                --enable-mysqlnd=shared \
                --enable-opcache=shared \
                --enable-pcntl=shared \
                --enable-tokenizer=shared \
                --enable-xml=shared \
                --enable-xmlreader=shared \
                --enable-xmlwriter=shared \
                --enable-zip=shared \
                        --with-libzip=/usr \
                --with-pcre-regex=/usr \
                        --without-pcre-jit \
                --with-layout=GNU \
                --with-pic \
                --with-bz2=shared \
                --with-gettext=shared \
                --with-gmp=shared \
                --with-iconv=shared \
                --with-imap=shared \
                        --with-imap-ssl \
                --with-icu-dir=/usr \
                --with-password-argon2=shared \
                --with-sodium=shared \
                --with-sqlite3=shared,/usr \
                --with-curl=shared \
                --with-gd=shared,/usr \
                        --disable-gd-jis-conv \
                        --with-freetype-dir=/usr \
                        --with-jpeg-dir=/usr \
                        --with-png-dir=/usr \
                        --with-webp-dir=/usr \
                        --with-xpm-dir=/usr \
                --with-kerberos \
                --with-libedit \
                --with-mysqli=shared,mysqlnd \
                --with-openssl=shared \
                        --with-system-ciphers \
                --with-tidy=shared \
                --with-xmlrpc=shared \
                --with-xsl=shared \
                --with-zlib \
                        --with-zlib-dir=/usr \
                --with-config-file-path="${PHP_INI_DIR}" \
                --with-config-file-scan-dir="${PHP_INI_DIR}/conf.d" \
                --without-readline \
  ; make -j "$(nproc)" \
  ; find -type f -name '*.a' -delete \
  ; make install \
  ; find /usr/bin /usr/sbin -type f -perm +0111 -exec strip --strip-all '{}' + || true \
  ; cp -v php.ini-production "${PHP_INI_DIR}/php.ini"

WORKDIR /usr/src/unit

RUN set -ex \
  ; git clone --quiet --no-checkout --jobs "$(nproc)" https://github.com/nginx/unit . \
  ; git checkout --quiet "${UNIT_VERSION}" \
  ; ./configure \
                --group=app \
                --user=app \
                --openssl \
                --prefix=/usr \
                --modules=/usr/lib/unit/modules \
                --pid=/tmp/unit.pid \
                --state=/var/lib/unit \
                --log=/dev/stderr \
                --control=unix:/var/lib/unit/control.sock \
  ; ./configure php \
                --module=php \
                --config=/usr/bin/php-config \
                --lib-path=/usr/lib \
  ; make \
  ; make install

ENV DESTDIR      "/install"
ENV INSTALL_ROOT "/install"

RUN set -ex \
  ; make install \
  ; cd /usr/src/php \
  ; make install \
  ; find "${INSTALL_ROOT}/usr/bin" "${INSTALL_ROOT}/usr/sbin" -type f -perm +0111 -exec strip --strip-all '{}' + || true \
  ; cp -v -r /etc/php "${INSTALL_ROOT}/etc/" \
#  ; cp -v -r /usr/share/php "${INSTALL_ROOT}/usr/share/" \
  ; rm -f "${INSTALL_ROOT}/.*" \
  ; du -hs "${INSTALL_ROOT}"

COPY docker/extensions.ini "${INSTALL_ROOT}/etc/php/conf.d/extensions.ini"
COPY docker/docker-entrypoint.sh "${INSTALL_ROOT}/usr/bin/docker-entrypoint"

FROM alpine:3.10 AS base

WORKDIR /app

COPY --from=builder /install /

RUN set -ex \
  ; apk add --no-cache --quiet \
            argon2-libs \
            bash \
            c-client \
            ca-certificates \
            curl \
            icu-libs \
            freetype \
            gmp \
            krb5-libs \
            libcrypto1.1 \
            libcurl \
            libedit \
            libbz2 \
            libgd \
            libintl \
            libjpeg-turbo \
            libldap \
            libpng \
            libsodium \
            libssl1.1 \
            libtls-standalone \
            libwebp \
            libxml2 \
            libxpm \
            libxslt \
            libzip \
            ncurses-libs \
            nghttp2-libs \
            openssl \
            pcre \
            pcre2 \
            sqlite-libs \
            tidyhtml-libs \
            zlib \
   ; addgroup -g 1042 app \
   ; adduser -h /app -s /bin/sh -G app -u 1042 -D -H app \
   ; chown app:app /app \
   ; chown app:root /var/lib/unit \
   ; chmod 0700 /var/lib/unit \
   ; echo '{"listeners":{"*:8000":{"pass":"applications/php_app"}},"applications":{"php_app":{"type":"php","root":"/app/"}}}' > /var/lib/unit/conf.json

ENTRYPOINT ["docker-entrypoint"]
CMD ["unitd"]
