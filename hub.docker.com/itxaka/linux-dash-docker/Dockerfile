################################
# Dockerfile to run linux-dash #
# Based on Ubuntu	       #
################################

# Ubuntu base
FROM ubuntu

MAINTAINER Itxaka Serrano

# add universe repository
RUN sed -i "s/main/main universe/g" /etc/apt/sources.list

#update
RUN apt-get update

# install nginx and php5-fpm, supervisor to control them
RUN apt-get install nginx php5-fpm supervisor git -y

# create the run dir for supervisor
RUN mkdir -p /var/log/supervisor

# add our supervisord file
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# add our default file with php5-fpm configured
ADD default /etc/nginx/sites-available/

# set daemon to off
RUN echo "daemon off;" >> /etc/nginx/nginx.conf

# clone the latest linux-dash version
RUN git clone https://github.com/afaqurk/linux-dash.git /var/www

# open port 80
EXPOSE 80

# run supervisor so it launches our process
ENTRYPOINT /usr/bin/supervisord
