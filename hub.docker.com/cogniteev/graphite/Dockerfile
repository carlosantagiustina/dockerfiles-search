# DOCKER-VERSION 0.4.0
from	ubuntu:14.04

env REFRESHED_AT 2014_04_22

run	apt-get -qqy update

# Install required packages
run	apt-get -qqy install  \
    gunicorn              \
    nginx-light           \
    python-cairo          \
    python-django         \
    python-django-tagging \
    python-ldap           \
    python-memcache       \
    python-pip            \
    python-pysqlite2      \
    python-simplejson     \
    python-support        \
    python-twisted        \
    python-tz             \
    supervisor
run	pip install whisper
run	pip install --install-option="--prefix=/var/lib/graphite" --install-option="--install-lib=/var/lib/graphite/lib" carbon
run	pip install --install-option="--prefix=/var/lib/graphite" --install-option="--install-lib=/var/lib/graphite/webapp" graphite-web

# Add system service config
add	./nginx.conf /etc/nginx/nginx.conf
add	./supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Add graphite config
add	./initial_data.json /var/lib/graphite/webapp/graphite/initial_data.json
add	./local_settings.py /var/lib/graphite/webapp/graphite/local_settings.py
add	./carbon.conf /var/lib/graphite/conf/carbon.conf
add	./storage-schemas.conf /var/lib/graphite/conf/storage-schemas.conf
run	mkdir -p /var/lib/graphite/storage/whisper
run	touch /var/lib/graphite/storage/graphite.db /var/lib/graphite/storage/index
run	chown -R www-data /var/lib/graphite/storage
run	chmod 0775 /var/lib/graphite/storage /var/lib/graphite/storage/whisper
run	chmod 0664 /var/lib/graphite/storage/graphite.db
run	cd /var/lib/graphite/webapp/graphite && python manage.py syncdb --noinput

# Nginx
expose	:80
# Carbon line receiver port
expose	:2003
# Carbon udp receiver port
expose	:2003/udp
# Carbon pickle receiver port
expose	:2004
# Carbon cache query port
expose	:7002

cmd	["/usr/bin/supervisord"]

# vim:ts=8:noet:
