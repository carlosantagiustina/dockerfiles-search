FROM ubuntu

RUN apt-get update && apt-get install -y software-properties-common && \
    apt-add-repository ppa:ansible/ansible && \
    apt-get update && apt-get install -y \
    ansible \
    openssh-server \
    curl \
    lsb-release \
    fonts-lato \
    javascript-common libgdbm3 libgmp-dev libgmpxx4ldbl libjs-jquery \
    libruby2.3 rake ruby-did-you-mean ruby-minitest ruby-net-telnet \
    ruby-power-assert ruby-test-unit ruby2.3 ruby2.3-dev rubygems-integration \
    unzip zip

