FROM ruby:2.2.2

RUN curl -sL https://deb.nodesource.com/setup | bash -
RUN apt-get update -qq
RUN apt-get install -y build-essential libpq-dev nodejs

# throw errors if Gemfile has been modified since Gemfile.lock
RUN bundle config --global frozen 1

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY Gemfile /usr/src/app/
COPY Gemfile.lock /usr/src/app/
RUN bundle install

COPY . /usr/src/app

EXPOSE 5000
CMD ./docker-entrypoint.sh

