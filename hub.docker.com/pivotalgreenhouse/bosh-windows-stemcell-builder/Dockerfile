#docker pull pivotalgreenhouse/bosh-windows-stemcell-builder
FROM ruby:2.3.3

#ENV PACKER_URL "https://releases.hashicorp.com/packer/0.12.2/packer_0.12.2_linux_amd64.zip"
ENV PACKER_URL "https://s3.amazonaws.com/windows-bosh-temp/packer-2.zip"

RUN apt-get update && apt-get -y install zip unzip wget
RUN wget ${PACKER_URL} -O packer.zip
RUN unzip packer.zip && mv packer /usr/local/bin/packer && rm packer.zip
RUN gem install bundler

# bosh Golang CLI
RUN version_number=$(curl 'https://github.com/cloudfoundry/bosh-cli/releases/latest' 2>&1 | egrep -o '([0-9]+\.[0-9]+\.[0-9]+)') && \
  curl "https://s3.amazonaws.com/bosh-cli-artifacts/bosh-cli-${version_number}-linux-amd64" -o /usr/local/bin/bosh && \
  chmod 755 /usr/local/bin/bosh

# gcloud CLI
RUN apt-get update && apt-get install -y apt-transport-https && \
  echo "deb https://packages.cloud.google.com/apt cloud-sdk-jessie main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
  curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
  apt-get update && \
  apt-get install -y google-cloud-sdk

# azure CLI
# see https://github.com/Azure/azure-cli-docker/blob/master/0.10.0/Dockerfile
ENV AZURE_CLI_VERSION "0.10.0"
ENV NODEJS_APT_ROOT "node_4.x"
ENV NODEJS_VERSION "4.2.4"

RUN apt-get update -qq && \
    apt-get install -qqy --no-install-recommends\
      apt-transport-https \
      build-essential \
      curl \
      ca-certificates \
      git \
      lsb-release \
      python-all \
      rlwrap \
      vim \
      nano \
      jq && \
    rm -rf /var/lib/apt/lists/* && \
    curl https://deb.nodesource.com/${NODEJS_APT_ROOT}/pool/main/n/nodejs/nodejs_${NODEJS_VERSION}-1nodesource1~jessie1_amd64.deb > node.deb && \
      dpkg -i node.deb && \
      rm node.deb && \
      npm install --global azure-cli@${AZURE_CLI_VERSION} && \
      azure --completion >> ~/azure.completion.sh && \
      echo 'source ~/azure.completion.sh' >> ~/.bashrc && \
      azure

RUN azure config mode arm

#install go
# gcc for cgo
RUN apt-get update && apt-get install -y --no-install-recommends \
        g++ \
        gcc \
        libc6-dev \
        make \
        pkg-config \
    && rm -rf /var/lib/apt/lists/*

ENV GOLANG_VERSION 1.7.4
ENV GOLANG_DOWNLOAD_URL https://golang.org/dl/go$GOLANG_VERSION.linux-amd64.tar.gz
ENV GOLANG_DOWNLOAD_SHA256 47fda42e46b4c3ec93fa5d4d4cc6a748aa3f9411a2a2b7e08e3a6d80d753ec8b

RUN curl -fsSL "$GOLANG_DOWNLOAD_URL" -o golang.tar.gz \
    && echo "$GOLANG_DOWNLOAD_SHA256  golang.tar.gz" | sha256sum -c - \
    && tar -C /usr/local -xzf golang.tar.gz \
    && rm golang.tar.gz

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"

