FROM library/debian:stretch
MAINTAINER quimnieto@gmail.com

RUN	apt-get update && \
	apt-get -y install libapache2-mod-php7.0 && \
	rm -rf /var/lib/dpkg /var/lib/apt /var/cache/apt /var/www/html/index.html && \
	ln -sf /dev/stdout /var/log/apache2/access.log && \	
	ln -sf /dev/sterr /var/log/apache2/error.log  

#ADD web.tgz /var/www/html --> el add además descomprime
COPY index.html /var/www/html/

ENTRYPOINT [ "usr/sbin/apache2ctl", "-D", "FOREGROUND" ]


#EXPOSE  80	
