FROM debian:stretch
LABEL maintainer="Lin Song <linsongui@gmail.com>"

ENV REFRESHED_AT 2017-10-26
ENV SWAN_VER 3.22
ENV PROXY_VER 0.8.8

WORKDIR /opt/src

RUN apt-get -yqq update \
    && DEBIAN_FRONTEND=noninteractive \
       apt-get -yqq --no-install-recommends install \
         wget dnsutils openssl ca-certificates kmod \
         iproute gawk grep sed net-tools iptables \
         bsdmainutils libcurl3-nss \
         libnss3-tools libevent-dev libcap-ng0 xl2tpd \
         libnss3-dev libnspr4-dev pkg-config libpam0g-dev \
         libcap-ng-dev libcap-ng-utils libselinux1-dev \
         libcurl4-nss-dev flex bison gcc make vim tcpdump \
    && wget -t 3 -T 30 -nv -O "libreswan.tar.gz" "https://github.com/libreswan/libreswan/archive/v${SWAN_VER}.tar.gz" \
    || wget -t 3 -T 30 -nv -O "libreswan.tar.gz" "https://download.libreswan.org/libreswan-${SWAN_VER}.tar.gz" \
    && tar xzf "libreswan.tar.gz" \
    && rm -f "libreswan.tar.gz" \
    && cd "libreswan-${SWAN_VER}" \
    && printf 'WERROR_CFLAGS =\nUSE_DNSSEC = false\nUSE_SYSTEMD_WATCHDOG = false\n' > Makefile.inc.local \
    && make -s base \
    && make -s install-base \
    && cd /opt/src \
    && rm -rf "/opt/src/libreswan-${SWAN_VER}" \
    && wget https://github.com/z3APA3A/3proxy/archive/3proxy-${PROXY_VER}.tar.gz \
    && tar xzfv 3proxy-${PROXY_VER}.tar.gz -C /tmp \
    && echo '#define ANONYMOUS 1' >> /tmp/3proxy-3proxy-${PROXY_VER}/src/3proxy.h \
    && make -C /tmp/3proxy-3proxy-${PROXY_VER} -f Makefile.Linux \
    && make -C /tmp/3proxy-3proxy-${PROXY_VER} -f Makefile.Linux install \
    && rm 3proxy-${PROXY_VER}.tar.gz \
    && ln -sf /dev/stdout /var/log/3proxy.log \
    && apt-get -yqq remove \
         libnss3-dev libnspr4-dev pkg-config libpam0g-dev \
         libcap-ng-dev libcap-ng-utils libselinux1-dev \
         libcurl4-nss-dev flex bison gcc make \
         perl-modules perl \
    && apt-get -yqq autoremove \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/*

COPY ./run.sh ./xTun ./vpn-up.sh /opt/src/
COPY rootfs /
RUN chmod 755 /opt/src/run.sh

EXPOSE 500/udp 4500/udp 3128/tcp

VOLUME ["/lib/modules"]

CMD ["/opt/src/run.sh"]
