FROM ubuntu:16.04
LABEL maintainer='Travis Plunk <github@ez13.net>'
LABEL readme.md="https://github.com/TravisEz13/docker/blob/master/opensuse/42.1/git/readme.md"
LABEL description="This Dockerfile for Ubuntu 16.04 with git."

# Install dependencies and clean up
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        git \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*
