FROM plugins/docker as plugin
FROM dronelab/dronelab
COPY --from=plugin / /
ENV DRONELAB_UPSTREAM_IMAGE="plugins/docker"
ENV DRONELAB_UPSTREAM_AUTHOR="drone-plugins"
ENV DRONELAB_UPSTREAM_REPO="https://github.com/drone-plugins/drone-docker"
ENV DRONELAB="/usr/local/bin/dockerd-entrypoint.sh /bin/drone-docker"
