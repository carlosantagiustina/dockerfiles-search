FROM ubuntu

MAINTAINER bin liu "liubin0329@gmail.com"

# Install packages for building ruby
RUN apt-get update
RUN apt-get install -y --force-yes build-essential curl git
RUN apt-get install -y --force-yes zlib1g-dev libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt-dev
RUN apt-get clean

# Install rbenv and ruby-build
RUN git clone https://github.com/sstephenson/rbenv.git /.rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git /.rbenv/plugins/ruby-build
RUN /.rbenv/plugins/ruby-build/install.sh

# RUN echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh

ENV PATH /.rbenv/bin:/.rbenv/shims:$PATH
#ENV PATH /.rbenv/bin:$PATH

# Install multiple versions of ruby
ENV CONFIGURE_OPTS --disable-install-doc

RUN rbenv install 2.1.0-rc1

# make it as default Ruby
RUN rbenv global 2.1.0-rc1

# Install Bundler for each version of ruby
RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc

RUN gem install bundler && rbenv rehash
