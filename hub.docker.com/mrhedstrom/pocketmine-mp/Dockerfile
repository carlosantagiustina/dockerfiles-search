FROM ubuntu:14.04

MAINTAINER mrhedstrom https://github.com/mrhedstrom

RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y \
        wget \
        perl gcc g++ make automake libtool autoconf m4 \
        gcc-multilib \
        bison

RUN adduser --gecos 'PocketMine-MP' --disabled-password --home /pocketmine pocketmine

WORKDIR /pocketmine
RUN mkdir /pocketmine/PocketMine-MP
RUN chown -R pocketmine:pocketmine /pocketmine

COPY assets/server.properties /pocketmine/server.properties.original
COPY assets/entrypoint.sh /pocketmine/entrypoint.sh

RUN chmod 755 /pocketmine/entrypoint.sh

USER pocketmine

ENV GNUPGHOME /pocketmine

ENV PHP_BINARY /pocketmine/PocketMine-MP/bin/php7/bin/php

RUN cd PocketMine-MP && wget -q -O - https://get.pmmp.io | bash -s -

EXPOSE 19132
EXPOSE 19132/udp

ENTRYPOINT ["./entrypoint.sh"]
