FROM jprjr/ubuntu-apache:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN cd /tmp && \
    curl -R -L -O https://download.elasticsearch.org/kibana/kibana/kibana-3.1.0.tar.gz && \
    tar xvf kibana-3.1.0.tar.gz && \
    mv -v kibana-3.1.0/* /var/www/html/ && \
    sed -i 's/9200"/"+window.location.port/' /var/www/html/config.js && \
    sed -i 's/"http:/window.location.protocol+"/' /var/www/html/config.js && \
    rm -rf /tmp/kibana-3.1.0* && \
    cat /var/www/html/config.js

