FROM ruby:2.4

MAINTAINER thinkbot@outlook.de

ENV VERSION=0.1.8

RUN gem install bib-vagrant --version ${VERSION} --no-format-exec

WORKDIR /tmp

ENTRYPOINT ["bib-vagrant"]
CMD ["--help"]
