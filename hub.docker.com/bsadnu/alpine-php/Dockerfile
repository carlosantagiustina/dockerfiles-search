FROM bsadnu/alpine-php:7.2-dev

COPY config/env/symfony /tmp/profile

RUN cat /tmp/profile >> /etc/profile && rm -rf /tmp/profile
