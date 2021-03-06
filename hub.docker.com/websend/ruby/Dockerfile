# NAME: Ruby 2.3.3
#
# DESCRIPTION: Base for other images using Ruby.
#
# NEEDED ENVS:
#
# USAGE EXAMPLE: docker run websend/ruby
# BUILD: docker build -t websend/ruby:2.3.3-1 .

FROM websend/debian:8.4
MAINTAINER Ilja Krijger <i.krijger@websend.nl>

RUN \
  apt-get update && \
  apt-get install -yq zlib1g-dev && \
  apt-get install -yq libssl-dev && \
  apt-get install -yq libreadline6-dev && \
  apt-get install -yq libyaml-dev && \
  apt-get install -yq libffi-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD http://cache.ruby-lang.org/pub/ruby/2.3/ruby-2.3.3.tar.gz /tmp/

WORKDIR /tmp

RUN \
  tar -xzf ruby-2.3.3.tar.gz && \
  cd ruby-2.3.3 && \
  ./configure --with-readline-dir=/usr/lib/x86_64-linux-gnu && \
  make && \
  make install && \
  cd .. && \
  rm -rf ruby-2.3.3 && \
  rm -f ruby-2.3.3.tar.gz

RUN \
  gem install rubygems-update --no-ri --no-rdoc && \
  update_rubygems && \
  gem install bundler --no-ri --no-rdoc
