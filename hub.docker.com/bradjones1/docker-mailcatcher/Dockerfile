FROM alpine:3.2
MAINTAINER Brad Jones <brad@bradjonesllc.com>
RUN apk -U add ruby ruby-bigdecimal ruby-sqlite tzdata libstdc++ \
  && apk add -t build ruby-dev build-base \
  && gem install --no-ri --no-rdoc mailcatcher json \
  && apk del --purge build \
  && rm -rf /var/cache/apk/*
EXPOSE 1080 1025
ENTRYPOINT ["mailcatcher", "--smtp-ip=0.0.0.0", "--http-ip=0.0.0.0", "--foreground"]
