FROM ruby:2.4

MAINTAINER thinkbot@outlook.de

ENV VERSION=1.5.1

RUN gem install ar_mailer --version ${VERSION} --no-format-exec

WORKDIR /tmp

ENTRYPOINT ["ar_sendmail"]
CMD ["--help"]
