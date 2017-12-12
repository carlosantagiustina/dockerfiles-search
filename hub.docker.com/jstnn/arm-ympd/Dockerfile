FROM resin/raspberrypi3-debian:jessie
LABEL maintainer="n.justiniano@gmail.com"

RUN [ "cross-build-start" ]

ENV YMPD_VERSION 1.2.3

RUN apt-get -qq -y update \
    && apt-get install -qq -y --no-install-recommends build-essential cmake libmpdclient-dev libssl-dev \ 
    && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN curl -fsSlL https://www.ympd.org/downloads/ympd-$YMPD_VERSION-armhf.tar.bz2 -o ympd.tar.bz2 \
    && tar -xvf ympd.tar.bz2

RUN [ "cross-build-end" ]

CMD ["./ympd", "-h", "mpd", "-p", "6600", "-w", "8080"]
