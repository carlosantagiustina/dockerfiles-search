FROM devwithlando/pantheon-appserver

RUN curl -sL https://deb.nodesource.com/setup_6.x | bash - \
    && apt-get update && apt-get install -y nodejs \
    && npm install -g bower grunt-cli gulp-cli \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn apt-key add - \
    && echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends yarn

