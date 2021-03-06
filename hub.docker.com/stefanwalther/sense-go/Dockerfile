# -------------------------------------------------------------------
#                               BASE NODE
# -------------------------------------------------------------------
# We need full node as we need git to download from some GitHub repos.
# -------------------------------------------------------------------
FROM node:8.11.3@sha256:04986974434fc565529feaac1d62cce4f9fe99ba4906f076ce498000120a45d4 as BASE
MAINTAINER Stefan Walther <swr-nixda@gmail.com>

COPY package.json .
RUN node -p -e "require('./package.json').devDependencies['sense-go']" > /root/sense-go-version
RUN SENSE_GO_VERSION=$(cat /root/sense-go-version)

RUN echo "Installing sense-go version $SENSE_GO_VERSION"

WORKDIR /opt/sense-go

RUN npm install --quiet sense-go@$SENSE_GO_VERSION -g

# -------------------------------------------------------------------
#                                RELEASE
# -------------------------------------------------------------------
FROM node:8.11.3-alpine@sha256:13f928a8b00ed6f10c1e3964da555e7324d327e2ec0c2202be8b72206625573c as RELEASE

RUN apk update
RUN apk add bash

WORKDIR /opt/sense-go

# Enables colored output
ENV FORCE_COLOR=true

# OK, here we have to copy the symbolic link
# use npm config get prefix to get the node.js prefix https://stackoverflow.com/questions/18383476/how-to-get-the-npm-global-path-prefix

# Create the symbolic link
RUN ln -s /usr/local/lib/node_modules/sense-go/bin/cli.js /usr/local/bin/sense-go

# Copy the global packages previously being installed
COPY --from=BASE /usr/local/lib/node_modules /usr/local/lib/node_modules

COPY ./docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]
