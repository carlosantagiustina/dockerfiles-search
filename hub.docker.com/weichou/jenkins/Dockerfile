FROM jenkins/jenkins

USER root
RUN apt-get -qq update \
   && apt-get -qq -y install curl


RUN curl -sSL https://get.docker.com/ | sh

RUN usermod -a -G docker jenkins

RUN apt-get -qq update \
   && apt-get -qq -y install net-tools

USER jenkins
