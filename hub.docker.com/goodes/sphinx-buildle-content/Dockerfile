FROM centos:7

# perl-devel, perl-ExtUtils-Embed for --with-http_perl_module
# gd-devel for --with-http_image_filter_module
RUN yum -y install yum-fastestmirror && \
    yum install -y wget gcc-c++ pcre-devel zlib-devel make unzip openssl-devel perl-devel perl-ExtUtils-Embed gd-devel && \
    yum clean all

RUN useradd -r nginx

ENV NPS_VERSION 1.12.34.3-stable
ENV NGINX_VERSION 1.12.2

RUN cd /tmp && \
    wget https://github.com/apache/incubator-pagespeed-ngx/archive/v${NPS_VERSION}.zip && \
    unzip v${NPS_VERSION}.zip && \
    cd incubator-pagespeed-ngx-${NPS_VERSION}/ && \
    psol_url=$(scripts/format_binary_url.sh PSOL_BINARY_URL) && \
    wget ${psol_url} && \
    tar -xzvf $(basename ${psol_url}) && \
    cd /tmp && \
    wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
    tar -xvzf nginx-${NGINX_VERSION}.tar.gz && \
    mkdir -p /var/lib/nginx/tmp/client_body && \
    mkdir -p /var/lib/nginx/tmp/proxy && \
    cd nginx-${NGINX_VERSION}/ && \
    ./configure \
    --prefix=/etc/nginx                   \
    --sbin-path=/usr/sbin/nginx           \
    --conf-path=/etc/nginx/nginx.conf     \
    --http-client-body-temp-path=/var/lib/nginx/tmp/client_body \
    --http-proxy-temp-path=/var/lib/nginx/tmp/proxy \
    --pid-path=/var/run/nginx.pid         \
    --lock-path=/var/run/nginx.lock       \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --add-module=/tmp/incubator-pagespeed-ngx-${NPS_VERSION} \
    --with-http_gzip_static_module        \
    --with-http_stub_status_module        \
    --with-http_ssl_module                \
    --with-pcre                           \
    --with-file-aio                       \
    --with-http_realip_module             \
    --with-http_perl_module               \
    --with-http_image_filter_module       \
    --without-http_scgi_module            \
    --without-http_uwsgi_module           \
    --without-http_fastcgi_module && \
    make && \
    make install && \
    rm -frdv /tmp/*

COPY ./nginx.conf /etc/nginx/nginx.conf

RUN ln -sf /dev/stderr /var/log/nginx/error.log
RUN ln -sf /dev/stdout /var/log/nginx/access.log

CMD ["nginx", "-g", "daemon off;"]
