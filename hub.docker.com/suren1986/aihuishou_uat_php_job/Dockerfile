FROM php:7.1-cli

RUN apt-get update
RUN apt-get install -y nodejs
RUN apt-get install -y supervisor
RUN apt-get install -y mysql-client
RUN apt-get install -y python
RUN apt-get install -y swftools
RUN apt-get install -y netcat-openbsd
RUN apt-get install -y libpng-dev
RUN apt-get install -y libjpeg-dev
RUN apt-get install -y libgmp-dev
RUN apt-get install -y zlib1g-dev
RUN apt-get install -y zip unzip

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install bcmath
RUN docker-php-ext-configure gd --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install gd
RUN docker-php-ext-install gmp
RUN docker-php-ext-install zip

RUN ln -s /usr/bin/nodejs /usr/bin/node
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN mv composer.phar /usr/local/bin/composer
RUN php -r "unlink('composer-setup.php');"
RUN mkdir /app

CMD supervisord -n -c /etc/supervisor/supervisord.conf
