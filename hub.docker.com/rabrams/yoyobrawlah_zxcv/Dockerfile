FROM ubuntu:12.04

RUN apt-get update && apt-get install -y netcat && apt-get clean

COPY test.sh /

RUN bash /test.sh
