FROM node:10-alpine

# labels
LABEL maintainer="nohitme@gmail.com"

# variables
ENV HUGO_VERSION 0.52
# ENV HUGO_EDITION hugo
ENV HUGO_EDITION hugo_extended

# install hugo
RUN set -x && \
  apk add --update --upgrade --no-cache wget ca-certificates libc6-compat && \
  # make sure we have up-to-date certificates
  update-ca-certificates && \
  cd /tmp &&\
  wget https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/${HUGO_EDITION}_${HUGO_VERSION}_Linux-64bit.tar.gz -O hugo.tar.gz && \
  tar xzf hugo.tar.gz && \
  mv hugo /usr/bin/hugo && \
  rm -r * && \
  # don't delete ca-certificates pacakge here since it remove all certs too
  apk del --purge wget && \
  # install firebase-cli
  # use --unsafe-perm to solve the issue: https://github.com/firebase/firebase-tools/issues/372
  npm install -g firebase-tools --unsafe-perm
