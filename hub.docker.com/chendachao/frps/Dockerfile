FROM alpine:3.10

ENV FRP_VERSION 0.28.2

VOLUME [ "/sys/fs/cgroup" ]

RUN apk add --no-cache openrc \
    && cd /root \
    &&  wget -c https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_amd64.tar.gz \
    &&  tar zxvf frp_${FRP_VERSION}_linux_amd64.tar.gz  \
    &&  cd frp_${FRP_VERSION}_linux_amd64/ \
    &&  cp frps /usr/bin/ \
    &&  mkdir -p /etc/frp \
    &&  cp frps.ini /etc/frp \
    &&  cd /root \
    &&  rm frp_${FRP_VERSION}_linux_amd64.tar.gz \
    &&  rm -rf frp_${FRP_VERSION}_linux_amd64/ 

ENTRYPOINT /usr/bin/frps -c /etc/frp/frps.ini