FROM rclone/rclone
MAINTAINER sabrsorensen@gmail.com

ARG BUILD_DATE
ARG VCS_REF

LABEL org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url="https://github.com/sabrsorensen/alpine-cloudplow.git" \
      org.label-schema.build-date=$BUILD_DATE

# linking the base image's rclone binary to the path expected by cloudplow's default config
RUN ln /usr/local/bin/rclone /usr/bin/rclone

WORKDIR /

# configure environment variables to keep the start script clean
ENV CLOUDPLOW_CONFIG=/config/config.json CLOUDPLOW_LOGFILE=/config/cloudplow.log CLOUDPLOW_LOGLEVEL=DEBUG CLOUDPLOW_CACHEFILE=/config/cache.db

# map /config to host directory containing cloudplow config (used to store configuration from app)
VOLUME /config

# map /rclone_config to host directory containing rclone configuration files
VOLUME /rclone_config

# map /service_accounts to host directory containing Google Drive service account .json files
VOLUME /service_accounts

# map /data to media queued for upload
VOLUME /data

# install dependencies for cloudplow and user management, upgrade pip
RUN apk -U add --no-cache \
        coreutils \
        findutils \
        git \
        grep \
        py3-pip \
        python3 \
        shadow \
        tzdata && \
        python3 -m pip install --no-cache-dir --upgrade pip

# install s6-overlay for process management
ADD https://github.com/just-containers/s6-overlay/releases/download/v1.22.1.0/s6-overlay-amd64.tar.gz /tmp/
RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C /

# download cloudplow
#RUN git clone --depth 1 --single-branch --branch master https://github.com/l3uddz/cloudplow /opt/cloudplow
RUN git clone --depth 1 --single-branch --branch bug/serviceFilePath https://github.com/sabrsorensen/cloudplow /opt/cloudplow

# install pip requirements
WORKDIR /opt/cloudplow
RUN python3 -m pip install --no-cache-dir -r requirements.txt

# add s6-overlay scripts and config
ADD root/ /

ENTRYPOINT ["/init"]
