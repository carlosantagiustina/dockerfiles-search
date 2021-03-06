FROM centos:7

# Set our our meta data for this container.
LABEL name="LAP Container for eWAPS"
LABEL maintainer="Michael R. Bagnall <michael.bagnall2@usda.gov>"
LABEL vendor="United States Department of Agriculture"

ENV PATH /usr/local/src/vendor/bin/:/usr/local/rvm/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Set TERM env to avoid mysql client error message "TERM environment variable not set" when running from inside the container
ENV TERM xterm

# Fix command line compile issue with bundler.
ENV LC_ALL en_US.utf8

# Install and enable repositories
RUN yum -y update && \
    yum -y install epel-release && \
    yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    rpm -Uvh https://centos7.iuscommunity.org/ius-release.rpm && \
    yum -y update

RUN yum -y install \
    curl \
    git2u \
    mariadb \
    msmtp \
    net-tools \
    python34 \
    vim \
    wget \
    rsync \
    unzip \
    zip \
    gcc \
    gcc-c++ \
    make \
    mod_ssl.x86_64

# Install PHP modules
RUN yum-config-manager --enable remi-php74 && \
  yum -y install \
    php \
    php-cli \
    php-curl \
    php-gd \
    php-imap \
    php-mbstring \
    php-mysqlnd \
    php-mysql \
    php-odbc \
    php-pear \
    php-pecl-imagick \
    php-pecl-json \
    php-pecl-opcache \
    php-pecl-redis \
    php-pecl-memcached \
    php-bcmath \
    php-xml \
    php-ldap \
    php-devel

RUN pecl install igbinary igbinary-devel redis

# Install misc tools
RUN yum -y install \
    python-setuptools

# Perform yum cleanup
RUN yum -y upgrade && \
    yum clean all

# Install Composer and Drush
RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/local/bin \
    --filename=composer \
    --version=1.8.5 && \
    composer \
    --working-dir=/usr/local/src/ \
    global \
    require \
    drush/drush:9.* && \
    ln -s /usr/local/src/vendor/bin/drush /usr/bin/drush

# Install Node.js and NPM
RUN yum -y install nodejs

# Install GRUNT tool and CLI tool
RUN npm install grunt@1.0.3 --save-dev
RUN npm install -g grunt-cli

# Disable services management by systemd.
RUN systemctl disable httpd.service

# Apache config, and PHP config, test apache config
# See https://github.com/docker/docker/issues/7511 /tmp usage
COPY public/index.php /var/www/public/index.php
COPY centos-7 /tmp/centos-7/

RUN rsync -a /tmp/centos-7/etc/ /etc/ 

EXPOSE 80 443

# Simple startup script to avoid some issues observed with container restart
ADD conf/run-httpd.sh /run-httpd.sh
RUN chmod -v +x /run-httpd.sh

ADD conf/mail.ini /etc/php.d/mail.ini
RUN chmod 644 /etc/php.d/mail.ini

CMD ["/run-httpd.sh"]
