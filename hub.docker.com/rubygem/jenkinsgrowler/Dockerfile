FROM ruby:2.4

MAINTAINER thinkbot@outlook.de

ENV VERSION=0.0.6

RUN gem install jenkinsgrowler --version ${VERSION} --no-format-exec

WORKDIR /tmp

ENTRYPOINT ["jenkinsgrowler"]
CMD ["--help"]
