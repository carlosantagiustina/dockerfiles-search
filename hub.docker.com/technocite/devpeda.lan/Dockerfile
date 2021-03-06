FROM debian:stable

# Redirection de apt sur notre mirroir local
# RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak
# RUN sed -e 's/httpredir.debian.org/10.10.24.251:9999/g' /etc/apt/sources.list.bak > /etc/apt/sources.list

# Pre configuration de postfix
RUN echo "postfix postfix/mailname string dev.pedagogique.lan" | debconf-set-selections
RUN echo "postfix postfix/main_mailer_type string 'Internet Site'" | debconf-set-selections

# Pre configuration de mysql-serveur
RUN echo "mysql-server mysql-server/root_password password tttttt" | debconf-set-selections
RUN echo "mysql-server mysql-server/root_password_again password tttttt" | debconf-set-selections

# Pre configuration de phpmyadmin
#RUN echo "phpmyadmin phpmyadmin/dbconfig-install boolean false" | debconf-set-selections
#RUN echo "phpmyadmin phpmyadmin/app-password-confirm password tttttt" | debconf-set-selections
#RUN echo "phpmyadmin phpmyadmin/mysql/admin-pass password tttttt" | debconf-set-selections
#RUN echo "phpmyadmin phpmyadmin/mysql/app-pass password tttttt" | debconf-set-selections
#RUN echo "phpmyadmin phpmyadmin/reconfigure-webserver multiselect none" | debconf-set-selections

# Installation des paquets necessaires
RUN apt-get -y update && apt-get install -y apt-utils apache2 debconf-utils expect php php-mysql mariadb-server nano postfix samba
RUN service apache2 start
RUN service mysql start
#RUN apt-get -q -y install phpmyadmin

#RUN echo '#!/usr/bin/expect -f' > install-phpmyadmin.sh; \
#        echo "set timeout -1" >> install-phpmyadmin.sh; \
#        echo "spawn apt-get install -y phpmyadmin" >> install-phpmyadmin.sh; \
#        echo "expect \"Configure database for phpmyadmin with dbconfig-common?\"" >> install-phpmyadmin.sh; \
#        echo "send \"n\r\"" >> install-phpmyadmin.sh;

#RUN chmod +x install-phpmyadmin.sh

RUN mysqld & \
        service apache2 start; \
        sleep 5; \
#        ./install-phpmyadmin.sh; \
        sleep 10; \
        mysqladmin -u root shutdown


# Lancement automatique de apache2
RUN echo "/etc/init.d/apache2 restart" >> /etc/bash.bashrc

# Configuration de postfix
RUN postconf -e relayhost=[smtp.technocite.be]
ADD ./etc/aliases /etc/aliases
RUN newaliases

# Ajout des fichiers de configuration de apache
ADD ./etc/apache2/sites-available/dev.pedagogique.lan.conf /etc/apache2/sites-available/dev.pedagogique.lan.conf
ADD ./etc/apache2/sites-available/virtu.conf /etc/apache2/sites-available/virtu.conf

# Activaion des sites dans apache2
RUN a2ensite dev.peda*
RUN a2ensite virtu

# Preparation des dossiers de logs
RUN mkdir /var/log/apache2/virtu

# Activation des modules apache2
RUN a2enmod rewrite vhost_alias
RUN a2dissite 000-default

# Preparation du site par defaut
RUN mkdir /var/www/www
ADD ./var/www/www /var/www/www/
ADD ./var/www/index.php /var/www/index.php
RUN chown -R www-data:www-data /var/www

# configuration de samba
ADD ./etc/samba/smb.conf /etc/samba/smb.conf
RUN service smbd restart
RUN service nmbd restart

# Activation de smba au demarrage
RUN echo "service smbd restart" >> /etc/bash.bashrc
RUN echo "service nmbd restart" >> /etc/bash.bashrc
