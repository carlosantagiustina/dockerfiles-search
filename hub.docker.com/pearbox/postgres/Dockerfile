FROM postgres:10
LABEL MAINTAINER="Artyom Nosov <chip@unixstyle.ru>"

ENV POSTGIS_MAJOR 2.4
ENV POSTGIS_VERSION 2.4.4+dfsg-4.pgdg90+1

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR=$POSTGIS_VERSION \
      postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR-scripts=$POSTGIS_VERSION \
      wget \
 && rm -rf /var/lib/apt/lists/*
