FROM ubuntu:16.04

RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates curl wget && rm -rf /var/lib/apt/lists/*
RUN apt-get update && apt-get install -y --no-install-recommends bzr git mercurial openssh-client subversion procps && rm -rf /var/lib/apt/lists/*
RUN apt-get update && apt-get install -y --no-install-recommends autoconf automake bzip2 file g++ gcc imagemagick libbz2-dev libc6-dev libcurl4-openssl-dev libevent-dev libffi-dev libgeoip-dev libglib2.0-dev libjpeg-dev liblzma-dev libmagickcore-dev libmagickwand-dev libmysqlclient-dev libncurses-dev libpng-dev libpq-dev libreadline-dev libsqlite3-dev libssl-dev libtool libwebp-dev libxml2-dev libxslt-dev libyaml-dev make patch xz-utils zlib1g-dev && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /usr/local/etc  && { echo 'install: --no-document';   echo 'update: --no-document';  } >> /usr/local/etc/gemrc

ENV RUBY_MAJOR=2.2
ENV RUBY_VERSION=2.2.4
ENV RUBY_DOWNLOAD_SHA256=b6eff568b48e0fda76e5a36333175df049b204e91217aa32a65153cc0cdcb761
ENV RUBYGEMS_VERSION=2.6.3

RUN set -ex  && buildDeps='   bison   libgdbm-dev   ruby  '  && apt-get update  && apt-get install -y --no-install-recommends $buildDeps  && rm -rf /var/lib/apt/lists/*  && curl -fSL -o ruby.tar.gz "http://cache.ruby-lang.org/pub/ruby/$RUBY_MAJOR/ruby-$RUBY_VERSION.tar.gz"  && echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.gz" | sha256sum -c -  && mkdir -p /usr/src/ruby  && tar -xzf ruby.tar.gz -C /usr/src/ruby --strip-components=1  && rm ruby.tar.gz  && cd /usr/src/ruby  && { echo '#define ENABLE_PATH_CHECK 0'; echo; cat file.c; } > file.c.new && mv file.c.new file.c  && autoconf  && ./configure --disable-install-doc  && make -j"$(nproc)"  && make install  && apt-get purge -y --auto-remove $buildDeps  && gem update --system $RUBYGEMS_VERSION  && rm -r /usr/src/ruby

ENV BUNDLER_VERSION=1.11.2

RUN gem install bundler --version "$BUNDLER_VERSION"

ENV GEM_HOME=/usr/local/bundle
ENV BUNDLE_PATH=/usr/local/bundle BUNDLE_BIN=/usr/local/bundle/bin BUNDLE_SILENCE_ROOT_WARNING=1 BUNDLE_APP_CONFIG=/usr/local/bundle
ENV PATH=/usr/local/bundle/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN"  && chmod 777 "$GEM_HOME" "$BUNDLE_BIN"

CMD ["irb"]
