FROM ubuntu:16.04

LABEL authors="Daniel Kokott <dako@berlingskemedia.dk>"

# Installing wget - needed to download node.js
RUN apt-get update && apt-get install -y wget

# Using latest LTS release.
# Latest LTS v6.10.0 is not supported by our jenkins
ENV NODE_VERSION v8.4.0

# Downloading and installing Node.
RUN wget -O - https://nodejs.org/dist/$NODE_VERSION/node-$NODE_VERSION-linux-x64.tar.gz \
    | tar xzf - --strip-components=1 --exclude="README.md" --exclude="LICENSE" \
    --exclude="ChangeLog" -C "/usr/local"

# Set the working directory.
WORKDIR /bpc

# Copying the code into image. Be aware no config files are including.
COPY ./server /bpc/server
COPY ./package.json /bpc/package.json

# Installing packages
RUN npm install --production


# Exposing our endpoint to Docker.
EXPOSE 8000

# When starting a container with our image, this command will be run.
CMD ["node", "server/index.js"]
