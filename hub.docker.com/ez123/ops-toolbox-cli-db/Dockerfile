# adapted from [ williamyeh/ansible:alpine3-onbuild ]
# "ONBUILD version" because:
# - updates some SSL stuff everytime is used in another image

FROM alpine:3.8

MAINTAINER HighSkillz <webdev@highskillz.com>

# To preserve the behaviour of pip==pip2 we need to pay attention to
# the order in which things are installed.
# This is the reason for us to
#       install pip3
#       remove $(which pip), and then
#       reinstall pip2
# Nevertheless, we use pip2 on the rest of the script "just in case"

RUN \
    echo "===> Updating TLS certificates..."         && \
    apk --update --no-cache add \
            ca-certificates \
            openssl \
    && \
    \
    echo "===> Adding Python runtime..."  && \
    apk --update --no-cache add \
        #     python py-pip \
            python3 \
        && \
    echo "===> Adding usefull packages for devops shell-works..."  && \
    apk --update --no-cache add \
            coreutils \
            openssh-client \
            rsync \
            tar \
            make \
            \
            tini \
            \
            file \
            bash \
            bash-completion \
            less \
            nano \
            \
            curl \
            wget \
            \
            zip \
            xz \
            unrar \
            \
            git \
            \
            mc \
            mosh \
            screen \
            \
            lsscsi \
            \
            htop \
            atop \
            sysstat \
            iftop \
            lsof \
            util-linux \
            mtr \
            \
            postgresql-client \
            mysql-client \
            mongodb-tools \
            mongodb \
            \
        && \
    \
    echo "===> Configuring Python and PIP..."  && \
    pip3 install --upgrade pip setuptools wheel && \
#     pip2 install --upgrade pip setuptools wheel && \
#     rm $(which pip) && \
#     ln -s $(which pip2) /usr/local/bin/pip && \
#     \
#     echo Making sure pip is set to python2 && \
#     pip --version | grep "python 2\."  && \
    \
    echo "===> Installing NodeJS..."  && \
    apk --update --no-cache add \
            nodejs \
    && \
    \
    echo "===> Adding Ruby..."  && \
    apk --update --no-cache add \
            ruby \
    && \
    \
    echo "===> Cleaning up ..."  && \
    rm -rf \
      "${HOME}/.cache" \
       /var/cache/apk/* \
       /tmp/*               \
       /var/tmp/*           \
    && \
    \
    echo "OK!"

SHELL ["/bin/bash", "-c"]

ONBUILD  RUN  \
              echo "===> Updating TLS certificates..."         && \
              apk --update --no-cache add \
              ca-certificates \
              openssl \
              ;

# default command: display some versions
CMD \
#     python  --version          | head -n 1 && \
    python3 --version          | head -n 1 && \
#     pip  --version             | head -n 1 && \
    pip3 --version             | head -n 1 && \
    node --version             | head -n 1 && \
    ruby --version             | head -n 1 && \
    git  --version             | head -n 1 && \
    openssl version            | head -n 1 && \
    psql --version             | head -n 1 && \
    mysql --version            | head -n 1 && \
    mongo --version            | head -n 1 && \
    echo ...
