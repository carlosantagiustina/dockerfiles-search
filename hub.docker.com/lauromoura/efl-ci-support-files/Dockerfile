FROM stefanschmidt1/ci-support-files:Ubuntu1910

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get install -y wget
RUN wget -q https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
RUN dpkg -i packages-microsoft-prod.deb
RUN apt-get update
RUN apt-get install -y apt-transport-https
RUN apt-get update
RUN apt-get install -y dotnet-sdk-3.0
