FROM ubuntu:16.04
LABEL maintainer=chanfr

RUN apt-get update && apt-get upgrade -y

RUN apt-get install -y openssh-server sudo nano iputils-ping

RUN apt-get update && \
    apt-get install -y software-properties-common && \
    apt-add-repository -y ppa:ansible/ansible && \
    apt-get install -y ansible 


RUN useradd -ms /bin/bash ansible && echo ansible:ansible | chpasswd && echo "ansible ALL=(ALL) ALL" >> /etc/sudoers

USER ansible
WORKDIR /home/ansible/
RUN ssh-keygen -f $HOME/.ssh/id_rsa -t rsa -N '' 
