FROM nanoninja/php-fpm:7.3.6
LABEL maintainer="Assis Ngolo <figassis@gmail.com>"

ENV OS_LOCALE=en_US.UTF-8 \
  LANG=${OS_LOCALE} \
  LANGUAGE=${OS_LOCALE} \
  LC_ALL=${OS_LOCALE} \
  DEBIAN_FRONTEND=noninteractive \
  NGINX_CONF_DIR=/etc/nginx

# Install useful packages
RUN apt-get update && apt-get -y install --no-install-recommends -y locales curl wget vim git unzip mysql-client nginx sudo \
  openssl ca-certificates software-properties-common wget gnupg supervisor psmisc nfs-common \
  automake autotools-dev fuse g++ gcc libcurl4-gnutls-dev libfuse-dev libssl-dev libxml2-dev make pkg-config && \
  locale-gen ${OS_LOCALE} && dpkg-reconfigure locales && \
  apt-get purge -y --auto-remove $BUILD_DEPS && \
  apt-get autoremove -y && apt-get clean && \
  rm -rf /var/lib/apt/lists/*

RUN	rm -rf ${NGINX_CONF_DIR}/sites-enabled/* ${NGINX_CONF_DIR}/sites-available/* /var/www/* \
  && mkdir -p /var/www/html /var/log/nginx /var/log/php /var/log/supervisor /var/www/.wp-cli/cache \
  && touch /var/www/html/index.html \
  && chown -R www-data:www-data /var/www/html -Rf \
  && ln -sf /dev/stdout /var/log/nginx/access.log \
  && ln -sf /dev/stderr /var/log/nginx/blocked.log \
  && ln -sf /dev/stderr /var/log/nginx/error.log \
  && ln -sf /dev/stdout /var/log/php/access.log \
  && ln -sf /dev/stderr /var/log/php/error.log \
  && ln -sf /dev/stderr /usr/local/var/log/php-fpm.log

# Install composer, wp-cli and wordpress
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php && \
  php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
  composer global require hirak/prestissimo && chown -R www-data:www-data /var/www/*

# Place configuration files
COPY ./conf/nginx.conf ${NGINX_CONF_DIR}/nginx.conf
COPY ./conf/wprocket.conf ${NGINX_CONF_DIR}/wprocket.conf
COPY ./conf/nginx-simple.conf ${NGINX_CONF_DIR}/sites-available/default.conf
COPY ./conf/supervisord.conf /etc/supervisord.conf
COPY ./conf/php.ini /usr/local/etc/php/php.ini
COPY ./conf/fpm.conf /usr/local/etc/php-fpm.d/www.conf
COPY ./errors/ /var/www/errors

RUN ln -s ${NGINX_CONF_DIR}/sites-available/default.conf ${NGINX_CONF_DIR}/sites-enabled/default.conf

# Add Scripts
ADD init.sh /start.sh
RUN chmod 755 /start.sh && chown -R www-data:www-data /var/run /var/lib/nginx /var/www

EXPOSE 80
VOLUME ["/var/www/html"]
WORKDIR /var/www/html
CMD ["/start.sh"]
