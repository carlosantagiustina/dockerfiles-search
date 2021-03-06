FROM alpine
ARG PANEL_VERSION="v0.7.16"

WORKDIR /var/www/pterodactyl
RUN apk --update add --no-cache bash curl nginx php7 php7-cli php7-gd php7-mysqli php7-pdo php7-mbstring php7-tokenizer php7-bcmath php7-xml php7-fpm php7-curl php7-zip php7-json php7-phar php7-pdo_mysql php7-simplexml php7-fileinfo tar unzip gettext php7-bcmath php7-common php7-dom php7-memcached php7-openssl git php7-session php7-ctype php7-zlib tini && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    curl -Lo /tmp/panel.tar.gz https://github.com/pterodactyl/panel/releases/download/${PANEL_VERSION}/panel.tar.gz && \
    tar --strip-components=1 -xzvf /tmp/panel.tar.gz -C /var/www/pterodactyl && \
    chmod -R 755 storage/* bootstrap/cache/ && \
    find storage -type d > .storage.template && \
    mv .env.example .env && \
    composer install --no-dev --optimize-autoloader && \
    chown -R nginx:nginx * && \
    rm -rf .env ./storage ./bootstrap/cache /root/.npm /root/.node-gyp /var/cache/apk/* /tmp/*

COPY root/ /
EXPOSE 80 443
VOLUME ["/data"]

ENTRYPOINT ["/sbin/tini", "--", "/entrypoint.sh"]
CMD ["config"]
