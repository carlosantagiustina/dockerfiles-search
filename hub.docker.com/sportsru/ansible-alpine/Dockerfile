FROM alpine:3.9

# Metadata params
ARG VERSION=2.7.13

# Metadata
LABEL maintainer="Mikhail Konyakhin <m.konyahin@gmail.com>" \
      org.label-schema.url="https://github.com/sportsru/docker-ansible-alpine/blob/master/README.md" \
      org.label-schema.version=$VERSION \
      org.label-schema.vcs-url="https://github.com/sportsru/docker-ansible-alpine.git" \
      org.label-schema.vcs-ref=$VERSION \
      org.label-schema.docker.dockerfile="/Dockerfile" \
      org.label-schema.description="Ansible on alpine docker image" \
      org.label-schema.schema-version="1.0"

RUN apk --update add \
        ca-certificates \
        git \
        openssh-client \
        openssl \
        py-pip \
        python \
        rsync \
        sshpass \
        gcc \
        musl-dev \
        python2-dev \
        openldap-dev \
        libffi-dev
RUN apk --update add --virtual \
        .build-deps \
        python-dev \
        libffi-dev \
        libressl-dev \
        build-base \
 && pip install --upgrade \
        pip \
        cffi \
 && pip install \
        ansible==${VERSION} \
 && apk del \
        .build-deps \
 && rm -rf /var/cache/apk/*

RUN mkdir -p /etc/ansible \
 && echo 'localhost' > /etc/ansible/hosts \
 && echo -e """\
\n\
Host *\n\
    StrictHostKeyChecking no\n\
    UserKnownHostsFile=/dev/null\n\
""" >> /etc/ssh/ssh_config

COPY entrypoint /usr/local/bin/

WORKDIR /ansible

ENTRYPOINT ["entrypoint"]

# default command: display Ansible version
CMD [ "ansible-playbook", "--version" ]
