FROM debian:wheezy

RUN \
  apt-get update && \
  apt-get upgrade -y

ADD https://www.dropbox.com/download?plat=lnx.x86_64 /dropbox.tar.gz
RUN tar xfvz /dropbox.tar.gz && rm /dropbox.tar.gz

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

VOLUME ["/root/Dropbox","/root/.dropbox"]

CMD /.dropbox-dist/dropboxd
