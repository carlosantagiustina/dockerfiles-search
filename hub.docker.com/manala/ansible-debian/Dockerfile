FROM debian:wheezy

MAINTAINER Manala <contact@manala.io>

# Versions
ENV ANSIBLE_VERSION      2.2.0.0
ENV ANSIBLE_LINT_VERSION 3.4.11
ENV GOSS_VERSION         0.2.6

WORKDIR /tmp

# Apt
RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get -y --no-install-recommends install \
        unzip \
        wget curl ca-certificates less \
        apt-transport-https \
        make \
        procps \
        vim

COPY scripts scripts

RUN bash scripts/ansible.sh \
    && bash scripts/ansible-lint.sh \
    && bash scripts/goss.sh

# Clean
RUN rm -rf * /var/lib/apt/lists/*

WORKDIR /srv
