FROM evarga/jenkins-slave

MAINTAINER Benjamin Cousin "b.cousin@code-troopers.com"
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get -y install curl git && curl -sL https://deb.nodesource.com/setup_5.x | sudo bash - && apt-get -y install nodejs


