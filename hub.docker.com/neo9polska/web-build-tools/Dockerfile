# nodejs with bower and gulp installed
FROM neo9polska/nodejs:latest

# maintaner of the repo
MAINTAINER Neo9 Polska <labs@neo9.pl>

# last update, to invalidate cache if needed
ENV LAST_UPDATE 2015-05-07

ENV DEBIAN_FRONTEND noninteractive

# node-gyp needs python 2 to install & compile some dependencies
ENV PYTHON python2

# create app folder and switch to it
RUN mkdir /app

# install git for bower dependencies and libfontconfig as phantomjs requirement
RUN apt-get -yq update && apt-get -yq install git libfontconfig ruby-full

#install bower, gulp and grunt
RUN npm install -g bower gulp grunt-cli

# install compass using ruby gems
RUN gem install compass

# default dir for app
WORKDIR /app/

# default command to run gulp
CMD ['gulp']
