### Dockerfile

FROM reconnix/base:latest
MAINTAINER Russ McKendrick <russ.mckendrick@reconnix.com>
ADD nginx.repo /etc/yum.repos.d/
RUN yum -y install yum-plugin-replace
RUN yum -y install nginx mariadb php56u php56u-common php56u-fpm php56u-process php56u-pecl-jsonc php56u-cli php56u-mysqlnd php56u-pdo php56u-devel php56u-gd php56u-pspell php56u-snmp php56u-xmlrpc php56u-xml php56u-pear php56u-mcrypt php56u-mbstring php56u-opcache python-setuptools postfix gcc make && yum clean all
RUN pecl install channel://pecl.php.net/xhprof-0.9.4 && pecl install xdebug
RUN echo "extension=xhprof.so" >> /etc/php.d/xhprof.ini && echo "zend_extension=/usr/lib64/php/modules/xdebug.so" >> /etc/php.d/xdebug.ini
RUN easy_install pip && pip install "pip>=1.4,<1.5" --upgrade && pip install supervisor
RUN useradd webserver -u 666 && gpasswd -a webserver apache
ADD conf-supervisord.conf /etc/supervisord.conf
ADD nginx.conf /etc/nginx/nginx.conf
ADD default.conf /etc/nginx/conf.d/default.conf
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
ADD run /usr/local/bin/
RUN chmod +x /usr/local/bin/run
RUN yum -y remove gcc cpp glibc-devel glibc-headers  kernel-headers libmpc mpfr
CMD ["/usr/local/bin/run"]