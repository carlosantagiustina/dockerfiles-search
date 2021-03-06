FROM java:8

ENV ANDROID_HOME /usr/local/android-sdk-linux
ENV PATH ${ANDROID_HOME}/tools:$ANDROID_HOME/platform-tools:$PATH

RUN apt-get update \
    && apt-get install -y \
    git \
    locales \
    sudo \
    openssh-client \
    ca-certificates \
    tar \
    gzip \
    parallel \
    net-tools \
    netcat \
    unzip \
    build-essential \
    && apt-get -y autoclean \
    && apt-get -y autoremove


# Install Android SDK
RUN mkdir -p /usr/local/android-sdk-linux \
    && wget https://dl.google.com/android/repository/sdk-tools-linux-3859397.zip -O tools.zip \
    && unzip tools.zip -d /usr/local/android-sdk-linux \
    && rm tools.zip

# Use
RUN mkdir -p /root/.android \
    && touch /root/.android/repositories.cfg \
    && yes | $ANDROID_HOME/tools/bin/sdkmanager --licenses \
    && $ANDROID_HOME/tools/bin/sdkmanager "build-tools;25.0.3" \
    && $ANDROID_HOME/tools/bin/sdkmanager "platforms;android-25" \
    && $ANDROID_HOME/tools/bin/sdkmanager "tools" "platform-tools" \
    && $ANDROID_HOME/tools/bin/sdkmanager "extras;android;m2repository" "extras;google;m2repository"

#react stuff
RUN curl -sL https://deb.nodesource.com/setup_7.x | bash \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update && apt-get install -y yarn \
    && apt-get -y autoclean \
    && apt-get -y autoremove


RUN yarn global add react-native-cli rnpm babel-cli
