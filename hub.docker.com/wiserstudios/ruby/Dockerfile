FROM ubuntu:14.10

RUN apt-get update && apt-get install -y \
  build-essential \
  git-core \
  libcurl4-openssl-dev \
  libffi-dev \
  libpq-dev \
  libreadline-dev \
  libssl-dev \
  libxml2-dev \
  libxslt1-dev \
  libyaml-dev \
  wget \
  zlib1g-dev

WORKDIR /tmp
RUN wget http://ftp.ruby-lang.org/pub/ruby/2.1/ruby-2.1.5.tar.gz && \
  tar -xzvf ruby-2.1.5.tar.gz

WORKDIR /tmp/ruby-2.1.5
RUN ./configure && make && make install

RUN gem install rubygems-update --no-ri --no-rdoc && \
  update_rubygems && \
  gem install bundler --no-ri --no-rdoc

RUN apt-get clean autoclean && \
  apt-get autoremove -y && \
  rm -rf /var/lib/{apt,dpkg,cache,log}/
