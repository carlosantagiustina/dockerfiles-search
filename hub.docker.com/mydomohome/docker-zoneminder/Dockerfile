FROM phusion/baseimage:0.9.18

MAINTAINER mydomohome.eu <mydomohome.eu@gmail.com> # source aptalca

VOLUME ["/config"]

EXPOSE 80

RUN export DEBCONF_NONINTERACTIVE_SEEN=true DEBIAN_FRONTEND=noninteractive && \
apt-get update && \
apt-get install -y \
software-properties-common \
python-software-properties && \
add-apt-repository -y ppa:iconnor/zoneminder && \
apt-get update && \
apt-get install -y --force-yes \
wget \
apache2 \
mysql-server \
php5 \
php5-gd \
libapache2-mod-php5 \
usbutils \
vlc \
libvlc-dev \
libvlccore-dev \
ssmtp \
mailutils && \
service apache2 restart && \
service mysql restart && \
apt-get install -y \
zoneminder \
libvlc-dev \
libvlccore-dev vlc && \
mysql -uroot < /usr/share/zoneminder/db/zm_create.sql && \
mysql -uroot -e "grant all on zm.* to 'zmuser'@localhost identified by 'zmpass';" && \
chmod 740 /etc/zm/zm.conf && \
chown root:www-data /etc/zm/zm.conf && \
a2enconf zoneminder && \
a2enmod rewrite && \
a2enmod cgi && \
chown -R www-data:www-data /usr/share/zoneminder/ && \
sed  -i 's/\;date.timezone =/date.timezone = \"America\/New_York\"/' /etc/php5/apache2/php.ini && \
service apache2 restart && \
service mysql restart && \
rm -r /etc/init.d/zoneminder && \
mkdir -p /etc/my_init.d

COPY zoneminder /etc/init.d/zoneminder
COPY firstrun.sh /etc/my_init.d/firstrun.sh
COPY cambozola.jar /usr/share/zoneminder/www/cambozola.jar

RUN chmod +x /etc/init.d/zoneminder && \
chmod +x /etc/my_init.d/firstrun.sh && \
adduser www-data video && \
service apache2 restart && \
update-rc.d -f apache2 remove && \
update-rc.d -f mysql remove && \
update-rc.d -f zoneminder remove
