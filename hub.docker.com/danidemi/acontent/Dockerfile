# To create the image
#
#    docker image build -t acontent .
#
# To run acontent
#
#    docker run -e MYSQL_ROOT_PASSWORD=rootpwd --name mysql -d mysql:5.6
#    docker run -p80:80 --name acontent -d acontent

FROM php:5.6.30-apache

LABEL "maintainer"="Daniele Demichelis <demichelis@danidemi.com>" \
      "version.php"="5.6.30" \
      "version.Atutor"="2.2.2"

EXPOSE 80 

# Install the needed PHP libraries
# ================================
RUN apt-get update; \
    apt-get install -y wget unzip; \
    docker-php-ext-install mysql; \
    apt-get install -y libfreetype6-dev libjpeg62-turbo-dev wget unzip; \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/lib; \
    docker-php-ext-install gd

# Configure PHP writing a custom php .ini
# ================================
RUN touch /var/log/php-errors.log; \
    echo "date.timezone=Europe/Rome" >> /usr/local/etc/php/conf.d/docker-php.ini; \
    echo "display_errors = Off" >> /usr/local/etc/php/conf.d/docker-php.ini; \
    echo "log_errors = On" >> /usr/local/etc/php/conf.d/docker-php.ini; \
    echo "error_log = /dev/stdout" >> /usr/local/etc/php/conf.d/docker-php.ini;
    
# Download and install ATutor
# AContent
# ===========================
RUN wget -O /tmp/acontent.zip --quiet https://github.com/atutor/AContent/archive/acontent_1_3.zip; \
    unzip /tmp/acontent.zip -d /tmp; \
    rm -rf /var/www/html; \
    mv /tmp/AContent-acontent_1_3 /var/www/html/; \
    touch /var/www/html/include/config.inc.php; \
    chmod a+rw -R /var/www/html/; \
    echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php; \
    rm /tmp/acontent.zip 

RUN mkdir /content; \
    chmod a+rw -R /content


# https://github.com/atutor/ATutor/archive/atutor_2_2_2.zip

# RUN mkdir -p /var/content; \chmod 2777 /var/content
