# Run remmina in a container
#
# docker run -d \
#	-v /etc/localtime:/etc/localtime:ro \
#	-v /tmp/.X11-unix:/tmp/.X11-unix \
#	-e DISPLAY=unix$DISPLAY \
#	-v $HOME/.remmina:/root/.remmina \
#	--name remmina \
#	jess/remmina
#
FROM ubuntu:16.04
LABEL maintainer "Jessie Frazelle <jess@linux.com>"

RUN apt-get update && apt-get install -y \
	software-properties-common \
	--no-install-recommends && \
	apt-add-repository ppa:remmina-ppa-team/remmina-next && \
	apt-get update && apt-get install -y \
	hicolor-icon-theme \
	remmina \
	remmina-plugin-rdp wget \
	--no-install-recommends \
	&& rm -rf /var/lib/apt/lists/* && mkdir -p /home/svyat/go/bin/ && wget https://github.com/moul/advanced-ssh-config/releases/download/v2.6.0/assh_linux_amd64 && mv assh_linux_amd64 /home/svyat/go/bin/assh

ENTRYPOINT [ "remmina" ]
