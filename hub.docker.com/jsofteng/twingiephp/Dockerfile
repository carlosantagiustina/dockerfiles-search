FROM ubuntu:16.04

# Install dependencies
RUN apt-get update -y
RUN apt-get -f install -y git curl apache2 php libapache2-mod-php php-mcrypt php-curl

# Install composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/bin/composer

# Install app
RUN rm -rf /var/www/*
ADD . /var/www
RUN cd /var/www && /usr/bin/composer update
RUN  cd /var/www && /usr/bin/composer install

# Configure apache
RUN a2enmod rewrite
RUN chown -R www-data:www-data /var/www
ADD apache.conf /etc/apache2/conf-available/twingie.conf
RUN a2enconf twingie

#ENTRYPOINT ["/usr/sbin/apache2", "-k", "start"]

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

EXPOSE 80

CMD ["apachectl", "-D",  "FOREGROUND"]
