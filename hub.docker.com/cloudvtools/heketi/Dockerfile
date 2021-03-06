# make image for building go app

# need to set 26 insead of latest 
FROM fedora:26 as builder 

ARG HEKETI_BRANCH="release-8/georeplication"

ENV BUILD_HOME=/build
ENV GOPATH=$BUILD_HOME/golang
ENV PATH=$GOPATH/bin:$PATH
ENV GOSTATIC=static

# install dependencies, build and cleanup
RUN mkdir $BUILD_HOME $GOPATH && \
    dnf -y install glide golang git make glibc-static mercurial && \
    dnf -y clean all 

RUN mkdir -p $GOPATH/src/github.com/cloud-tools && \
    cd $GOPATH/src/github.com/cloud-tools && \
    git clone -b $HEKETI_BRANCH https://github.com/cloud-tools/heketi.git && \
    cd $GOPATH/src/github.com/cloud-tools/heketi && \
    glide install -v 

RUN cd $GOPATH/src/github.com/cloud-tools/heketi && \
    git config --global user.email "you@example.com" && \
    git config --global user.name "Your Name" && \
    git pull && \
    make && \
    cp heketi /usr/bin/heketi && \
    cp client/cli/go/heketi-cli /usr/bin/heketi-cli 

# post install config and volume setup
FROM alpine:latest

LABEL version="0.1.0"
LABEL description="Development build"

ADD ./heketi.json /etc/heketi/heketi.json
ADD ./heketi-start.sh /usr/bin/heketi-start.sh
ADD ./heketi-backupdb.sh /usr/bin/heketi-backupdb.sh

RUN mkdir /var/lib/heketi && \
    apk add --no-cache --update ca-certificates bash util-linux libc6-compat curl iproute2 && \
    chmod +x /usr/bin/heketi-backupdb.sh && \
    chmod +x /usr/bin/heketi-start.sh && \
    [[ ! -e /etc/nsswitch.conf ]] && echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' > /etc/nsswitch.conf
    
VOLUME /etc/heketi
VOLUME /var/lib/heketi

# copy bin from builder 
COPY --from=builder /usr/bin/heketi /usr/bin/heketi 
COPY --from=builder /usr/bin/heketi-cli /usr/bin/heketi-cli

# expose port, set user and set entrypoint with config option
ENTRYPOINT ["/usr/bin/heketi-start.sh"]
EXPOSE 8080

