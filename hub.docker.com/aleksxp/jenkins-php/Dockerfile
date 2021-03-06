FROM jenkins/jenkins:lts
MAINTAINER Alexander Pankov <ap@wdevs.ru>

USER root

COPY ./tools/composer.sh /tools/composer.sh

RUN apt-get update -qq && apt-get upgrade -y && \
    apt-get install -y software-properties-common curl mc nano ant \
    php7.0-cli php7.0-intl php7.0-xsl php7.0-dom php7.0-zip php7.0-mbstring php7.0-mysql php7.0-gd php7.0-mongodb php-pear php7.0-xdebug && \
    chmod a+x /tools/composer.sh

RUN /tools/composer.sh && rm -rf tools

RUN mkdir -p /var/www && chown -R jenkins:jenkins /var/www

RUN mkdir -p /var/composer && chown -R jenkins /var/composer

USER jenkins

COPY ./tools/executors.groovy /usr/share/jenkins/ref/init.groovy.d/executors.groovy
COPY ./tools/jenkins_plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN xargs /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

RUN composer global config minimum-stability dev && composer global config prefer-stable true && \
    composer global config vendor-dir /var/composer/vendor && composer global config cache-dir /var/composer/cache && \
    composer global config data-dir /var/composer && composer global config data-dir /var/composer && \
    composer global require phpunit/phpunit squizlabs/php_codesniffer \
    phploc/phploc pdepend/pdepend phpmd/phpmd sebastian/phpcpd \
    mayflower/php-codebrowser theseer/phpdox:dev-master

RUN cd /var/www && git clone https://github.com/alekspankov/php-jenkins.git && \
    mv /var/www/php-jenkins /var/www/default

ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/var/composer/vendor/bin