FROM phpdockerio/php56-fpm:latest
WORKDIR "/application"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y install \
          git \
          php5-dev \
          libpcre3-dev \
          gcc \
          make \
    && apt-get -y --no-install-recommends install \
          mysql-common \
          php5-memcached \
          php5-mongo \
          php5-mysql \
          php5-redis \
          php5-xdebug \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
    && cd /tmp && git clone git://github.com/phalcon/cphalcon.git \
    && cd cphalcon && git checkout tags/phalcon-v1.3.4 \
    && cd build/ && ./install \
    && cd /tmp && rm -rf /tmp/cphalcon \
    && apt-get -y purge git php5-dev libpcre3-dev gcc make \
    && apt-get -y autoremove \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* \
    && echo 'extension=phalcon.so' > /etc/php5/mods-available/phalcon.ini \
    && /usr/sbin/php5enmod phalcon
