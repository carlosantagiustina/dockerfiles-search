FROM vorakl/centos:latest

MAINTAINER "Oleksii Tsvietnov" <vorakl@protonmail.com>

ENV RC_VERBOSE true

RUN yum install -y opensmtpd && \
    yum clean all && \
    rm -rf /var/cache/yum/*

COPY etc/ /etc/

EXPOSE 25 465
