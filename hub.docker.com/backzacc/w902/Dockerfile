FROM php:7.0-apache
RUN docker-php-ext-install pdo_mysql 
COPY ./src/ /var/www/html/
EXPOSE 80
