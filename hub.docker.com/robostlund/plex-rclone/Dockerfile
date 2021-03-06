FROM ubuntu:16.04

ARG S6_OVERLAY_VERSION=v1.17.2.0
ARG DEBIAN_FRONTEND="noninteractive"
ENV TERM="xterm" LANG="C.UTF-8" LC_ALL="C.UTF-8"
ENV RCLONE_VERSION="1.44"

ENTRYPOINT ["/init"]

RUN \
# Update and get dependencies
    apt-get update && \
    apt-get install -y \
      tzdata \
      curl \
      xmlstarlet \
      uuid-runtime \
      unzip \
      wget \
      fuse \
      libfuse-dev \
    && \

# Fetch and extract S6 overlay
    curl -J -L -o /tmp/s6-overlay-amd64.tar.gz https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz && \
    tar xzf /tmp/s6-overlay-amd64.tar.gz -C / && \

# Fetch and extract rclone
    wget https://github.com/ncw/rclone/releases/download/v${RCLONE_VERSION}/rclone-v${RCLONE_VERSION}-linux-amd64.zip -O /tmp/rclone-v${RCLONE_VERSION}-linux-amd64.zip && \
    unzip /tmp/rclone-v${RCLONE_VERSION}-linux-amd64.zip -d /tmp/ && \
    mv /tmp/rclone-v${RCLONE_VERSION}-linux-amd64/rclone /usr/sbin/rclone && \
    chmod 755 /usr/sbin/rclone && \
    chown root:root /usr/sbin/rclone && \
    rm -rf /tmp/rclone-v${RCLONE_VERSION}-linux-amd64 && \
    rm -f /tmp/rclone-v${RCLONE_VERSION}-linux-amd64.zip && \

# Add user and groups
    groupadd fuse -g 107 && \
    useradd -U -d /config -s /bin/false plex && \
    usermod -G users plex && \
    usermod -aG fuse plex && \

# Setup directories
    mkdir -p \
      /config \
      /transcode \
      /data \
    && \
# Allow others to fuse
  sed -i 's/#user_allow_other/user_allow_other/' /etc/fuse.conf && \

# Cleanup
    apt-get -y autoremove && \
    apt-get -y clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/* && \
    rm -rf /var/tmp/*

EXPOSE 32400/tcp 3005/tcp 8324/tcp 32469/tcp 1900/udp 32410/udp 32412/udp 32413/udp 32414/udp
VOLUME /config /transcode

ENV CHANGE_CONFIG_DIR_OWNERSHIP="true" \
    HOME="/config"

ARG TAG=beta
ARG URL=

COPY root/ /

RUN \
# Save version and install
    /installBinary.sh

HEALTHCHECK --interval=200s --timeout=100s CMD /healthcheck.sh || exit 1
