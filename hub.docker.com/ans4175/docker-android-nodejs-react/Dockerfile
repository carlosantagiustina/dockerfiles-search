FROM nao20010128nao/android-build:latest

MAINTAINER Anshorimuslim <ans4175@gmail.com>

ENV NODEJS_VERSION=6.11.3 \
    ANDROID_NDK_VERSION=10e \
    ANDROID_CMAKE_VERSION=3.6.3155560 \
    ANDROID_NDK=/opt/android-ndk \
    PATH=$PATH:/opt/node/bin:${ANDROID_NDK}:${ANDROID_HOME}/cmake/bin

WORKDIR "/opt/node"

RUN set -x && dpkg --add-architecture i386 && apt-get update && apt-get install -y curl ca-certificates --no-install-recommends && \
    curl -sL https://nodejs.org/dist/v${NODEJS_VERSION}/node-v${NODEJS_VERSION}-linux-x64.tar.gz | tar xz --strip-components=1 && \
    apt-get install -y git build-essential libc6:i386 libncurses5:i386 libstdc++6:i386 lib32stdc++6 lib32z1 && \
    rm -rf /var/lib/apt/lists/* && \
    npm install npm -g && \
    npm install -g react-native-cli && \

    mkdir /opt/android-ndk-tmp && \
    mkdir /opt/android-ndk && \
    cd /opt/android-ndk-tmp && wget -q https://dl.google.com/android/repository/android-ndk-r${ANDROID_NDK_VERSION}-linux-x86_64.zip && \
    unzip -q android-ndk-r${ANDROID_NDK_VERSION}-linux-x86_64.zip && mv ./android-ndk-r${ANDROID_NDK_VERSION}/* ${ANDROID_NDK} && \
    rm -rf /opt/android-ndk-tmp && \

    mkdir /opt/android-cmake-tmp && \
    mkdir ${ANDROID_HOME}/cmake && \
    cd /opt/android-cmake-tmp && wget -q https://dl.google.com/android/repository/cmake-${ANDROID_CMAKE_VERSION}-linux-x86_64.zip -O android-cmake.zip && \
    unzip -q android-cmake.zip -d android-cmake && mv ./android-cmake/* ${ANDROID_HOME}/cmake && \
    rm -rf /opt/android-cmake-tmp \

    apt-get clean
