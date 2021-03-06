FROM jetbrains/teamcity-minimal-agent

MAINTAINER Joshua Taylor <lordralex@pufferpanel.com>

ENV LANG en_US.UTF-8
ENV DEBIAN_FRONTEND=noninteractive 
ENV TZ US/Eastern

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get clean && apt-get update && apt-get install --no-install-recommends -y git ruby ruby-dev rubygems build-essential rpm openssl openssh-client cpio tzdata gpg-agent
RUN apt-get install --no-install-recommends -y software-properties-common && add-apt-repository ppa:ondrej/php
RUN apt-get update && apt-get install --no-install-recommends -y php-cli php-curl php-mysql
RUN curl -o /tmp/go.tar.gz https://dl.google.com/go/go1.11.1.linux-amd64.tar.gz \
    && tar zxf /tmp/go.tar.gz -C /usr/local \
    && ln -s ../go/bin/go /usr/local/bin/go
RUN export INSTALL_DIRECTORY=/usr/local/go/bin/ \
    && curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
RUN chmod +x /usr/local/go/bin/dep \
    && ln -s ../go/bin/dep /usr/local/bin/dep \
    && ln -s /data/teamcity_agent/conf/.packagecloud ~/.packagecloud
RUN gem install rake \
    && gem install --no-ri --no-rdoc fpm \
    && gem install package_cloud
RUN curl -o /tmp/composer.php https://getcomposer.org/installer
RUN php /tmp/composer.php --filename=composer --install-dir=/usr/local/bin
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get update && apt-get install -y nodejs
RUN go version \
    && git version \
    && dep version \
    && fpm --version \
    && package_cloud version \
    && php -v \
    && composer --version \
    && node -v \
    && npm -v
RUN rm -f /tmp/go.tar.gz /tmp/composer.php
RUN apt-get clean
