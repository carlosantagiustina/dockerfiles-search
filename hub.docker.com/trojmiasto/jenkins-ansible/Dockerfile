FROM jenkins/jenkins:lts

USER root
RUN apt-get update && apt-get install -y gettext-base \
    curl
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt

RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

USER jenkins
