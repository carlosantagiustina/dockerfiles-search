FROM immerhungrig/apache:stretch
MAINTAINER immerhungrig <immerhungrig@me.com>

RUN apt-get update && apt-get -y install php php-mysql libapache2-mod-php && apt-get clean && rm -r /var/lib/apt/lists/*
RUN /usr/sbin/a2dismod 'mpm_*' && /usr/sbin/a2enmod mpm_prefork

VOLUME ["/var/www", "/var/log", "/etc/apache2", "/etc/ssl"]

EXPOSE 80
EXPOSE 443

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

