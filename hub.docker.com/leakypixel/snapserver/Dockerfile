FROM debian:stable

ARG  snapcast_version=0.17.1

RUN  apt-get update \
  && apt-get install -y wget ca-certificates \
  && rm -rf /var/lib/apt/lists/*
RUN  wget https://github.com/badaix/snapcast/releases/download/v${snapcast_version}/snapserver_${snapcast_version}-1_amd64.deb
RUN  dpkg -i snapserver_${snapcast_version}-1_amd64.deb \
  ;  apt-get update \
  && apt-get -f install -y \
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf snapserver_${snapcast_version}-1_amd64.deb
RUN /usr/bin/snapserver -v
EXPOSE 1704 1705
ENTRYPOINT ["/bin/bash","-c","/usr/bin/snapserver"]
CMD  ["-c /etc/snapserver.conf"]
