FROM debian:sid

# Add global dependencies
RUN apt-get update \
    && apt-get install -y curl apt-transport-https ca-certificates libfontconfig apt-utils

RUN apt-get update \
    && apt-get install -y openssh-client

RUN \
    curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && \
    apt-get install -y  nodejs && \
	apt-get install -y --no-install-recommends  yarn
