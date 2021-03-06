ARG PHP_VERSION=7.3.1

FROM php:${PHP_VERSION}-fpm-alpine
# FROM metowolf/dev:php-${PHP_VERSION}-fpm-alpine

LABEL maintainer="metowolf <i@i-meto.com>"

ENV PHPREDIS_VERSION 4.2.0
ENV COMPOSER_VERSION 1.8.0

RUN apk --no-cache update \
      && apk --no-cache upgrade --update \
      && apk add --no-cache --update \
         libzip \
         openssl \
         freetype \
         libpng \
         libjpeg-turbo \
         libintl \
         icu \
         libxslt \
      && apk add --no-cache --virtual .build-deps \
         libzip-dev \
         openssl-dev \
         freetype-dev \
         libpng-dev \
         libjpeg-turbo-dev \
         libxml2-dev \
         gettext-dev \
         icu-dev \
         libxslt-dev \
      && docker-php-source extract && touch /usr/src/php/.docker-delete-me \
      && curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
      && tar xfz /tmp/redis.tar.gz \
      && rm -r /tmp/redis.tar.gz \
      && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
      && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
      && docker-php-ext-install exif gettext iconv intl pcntl shmop soap sysvsem tokenizer xsl zip pdo_mysql mysqli gd opcache redis xmlrpc bcmath sockets \
      && apk del .build-deps \
      && rm -rf /var/cache/apk/* \
      && docker-php-source delete \
      && curl -L -o /usr/local/bin/composer https://getcomposer.org/download/$COMPOSER_VERSION/composer.phar \
      && chmod a+x /usr/local/bin/composer

RUN { \
      echo 'opcache.memory_consumption=128'; \
      echo 'opcache.interned_strings_buffer=8'; \
      echo 'opcache.max_accelerated_files=4000'; \
      echo 'opcache.revalidate_freq=60'; \
      echo 'opcache.fast_shutdown=1'; \
      echo 'opcache.enable_cli=1'; \
    } > /usr/local/etc/php/conf.d/opcache-recommended.ini \
    && { \
      echo 'memory_limit=256M'; \
      echo 'upload_max_filesize=50M'; \
      echo 'post_max_size=100M'; \
      echo 'max_execution_time=600'; \
      echo 'default_socket_timeout=3600'; \
      echo 'request_terminate_timeout=600'; \
    } > /usr/local/etc/php/conf.d/options.ini

COPY entrypoint.sh /

ENTRYPOINT ["sh", "/entrypoint.sh"]
