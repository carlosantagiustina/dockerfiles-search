FROM danlynn/ember-cli:3.11.0

MAINTAINER Sebastien LANGOUREAUX <linuxworkgroup@hotmail.com>

ARG http_proxy
ARG https_proxy

ADD https://raw.githubusercontent.com/disaster37/che-scripts/master/debian.sh /tmp/debian.sh
RUN sh /tmp/debian.sh

RUN ember init

WORKDIR "/projects"
VOLUME "/home/dev"

CMD ["sleep", "infinity"]


