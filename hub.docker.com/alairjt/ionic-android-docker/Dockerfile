FROM ubuntu:16.04
MAINTAINER Alair J. Tavares <alairjt@gmail.com>

# Install apt packages
RUN apt-get update && apt-get install -y git  lib32ncurses5 lib32gomp1 lib32z1-dev lib32stdc++6 lib32z1 npm nodejs nodejs-legacy s3cmd build-essential curl openjdk-8-jdk-headless sendemail libio-socket-ssl-perl libnet-ssleay-perl && apt-get clean && rm -rf /var/lib/apt/lists/*

# Install android SDK, tools and platforms 
RUN cd /opt && curl https://dl.google.com/android/android-sdk_r24.4.1-linux.tgz -o android-sdk.tgz && tar xzf android-sdk.tgz && rm android-sdk.tgz
ENV ANDROID_HOME /opt/android-sdk-linux
RUN echo 'y' | /opt/android-sdk-linux/tools/android update sdk --no-ui
# Install npm packages
RUN npm i -g cordova ionic gulp bower grunt phonegap && npm cache clean
RUN npm i -g protractor
RUN webdriver-manager update

# Create dummy app to build and preload gradle and maven dependencies
#RUN cd / && echo 'n' | ionic start app && cd /app && ionic platform add android && ionic build android && rm -rf * .??* && rm /root/.android/debug.keystore

WORKDIR /app

