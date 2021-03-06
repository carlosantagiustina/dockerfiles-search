FROM ubuntu:18.04
LABEL maintainer "ripples <w@q2zy.com>"

# sources
ADD https://mirrors.ustc.edu.cn/repogen/conf/ubuntu-http-4-bionic /etc/apt/sources.list

RUN dpkg --add-architecture i386 \
    && apt-get update && apt-get -y upgrade && apt-get install -y \
        ack-grep \
        autojump \
        bc \
        binutils-arm-linux-gnueabi \
        bison \
        build-essential \
        cmake \
        ctags \
        curl \
        debian-keyring \
        dnsutils \
        g++ \
        gawk \
        gcc \
        gdb-multiarch \
        gdbserver \
        git \
        htop \
        httpie \
        inetutils-traceroute \
        iputils-ping \
        libc6-dbg \
        libc6-i386 \
        libc6-dbg:i386 \
        libc6-dev-i386 \
        libevent-dev \
        libffi-dev \
        libgmp-dev \
        libncurses5:i386 \
        libssh-dev \
        libstdc++6:i386 \
        locales \
        locate \
        ltrace \
        make \
        man \
        mtr \
        nasm \
        net-tools \
        netcat \
        nmap \
        openssh-server \
        p7zip-full \
        patchelf \
        python \
        python-dev \
        python-pip \
        rsync \
        ruby \
        silversearcher-ag \
        socat \
        software-properties-common \
        strace \
        sudo \
        tmux \
        trash-cli \
        wget \
        vim \
        xsel \
        zsh \
    && rm -rf /var/lib/apt/lists/*

# concurrency jobs for make, empty for unlimited
ARG CONCURRENCY=1

# keystone-engine
RUN cd ~ && git clone --depth 1 --branch=0.9.1 https://github.com/keystone-engine/keystone.git \
    && cd keystone && mkdir build && cd build \
    && sed -i "s/make -j8/make -j${CONCURRENCY}/" ../make-share.sh \
    && bash ../make-share.sh && make install -j${CONCURRENCY} && ldconfig \
    && cd ../bindings/python \
    && sed -i "s/^PKG_NAME = 'keystone'/PKG_NAME = 'keystone-engine'/" setup.py \
    && sed -i "s/^VERSION = '0.9'/VERSION = '9.9'/" setup.py \
    && make install -j${CONCURRENCY}

# glibc with full debug info
RUN cd ~ && apt-get update && apt-get build-dep -y glibc && apt-get source glibc \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf ./glibc_* \
    && export CFLAGS="-g -g3 -ggdb3 -gdwarf-4 -Og" \
    && export CXXFLAGS="${CFLAGS}" \
    && cd glibc-* && make -f ./debian/rules && mkdir -p build && cd build \
    && ../configure --prefix=/dbg64 --with-headers=/usr/include/ --enable-add-ons=libidn --enable-obsolete-rpc --disable-werror \
    && make -j${CONCURRENCY} && make install -j${CONCURRENCY} \
    && cd .. && rm -rf build && mkdir -p build && cd build \
    && CC="gcc -m32" CXX="g++ -m32" ../configure --prefix=/dbg32 --with-headers=/usr/include/ --enable-add-ons=libidn --enable-obsolete-rpc --disable-werror --host=i686-pc-linux-gnu \
    && make -j${CONCURRENCY} && make install -j${CONCURRENCY} \
    && cd .. && rm -rf build sysdeps iconvdata .pc \
    && find . -type f ! -name '*.[c,cc,h,S]' | xargs rm \
    && find /dbg32 /dbg64 -name '*.a' | xargs rm

# python libs
RUN pip install -U pip pyopenssl && hash -r
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple \
    && pip install -U binch pwntools virtualenv virtualenvwrapper \
    && pip install --isolated angr ipython \
    && pip install -I --no-binary :all: capstone
ENV WORKON_HOME /root/.virtualenvs

# one gadget
RUN gem install one_gadget

# gdb
COPY patch /root/patch
RUN cd ~ && git clone --depth 1 https://github.com/scwuaptx/Pwngdb.git Pwngdb \
    && git clone --depth 1 https://github.com/pwndbg/pwndbg.git pwndbg \
    && git config --global user.email "ctf-box@q2zy.com" \
    && git config --global user.name "ctf-box" \
    && cd pwndbg && git am -3 ../patch/pwndbg/*.patch && ./setup.sh
COPY .gdbinit /root/.gdbinit
RUN echo directory $(find /root/glibc-* -type d | paste -sd:) >> /root/.gdbinit

# how2heap
RUN cd ~ && git clone --depth 1 https://github.com/shellphish/how2heap how2heap

# fix locale
RUN locale-gen en_US.UTF-8 && localedef -i en_US -f UTF-8 en_US.UTF-8

# personal configuration
RUN cd ~ && git clone --recursive --depth 1 https://github.com/ripples-alive/dotfiles.git .dotfiles \
    && sed -i 's/<\/dev\/tty/<\/dev\/zero/' .dotfiles/install.conf.yaml \
    && rm -f .bashrc \
    && .dotfiles/install \
    && chsh -s /bin/zsh
RUN cd ~ && git clone --depth 1 https://github.com/ripples-alive/CTF-Scripts.git ctf-scripts
ENV PYTHONPATH /root/ctf-scripts/utils
COPY authorized_keys /root/.ssh/authorized_keys
RUN chmod -R 700 /root/.ssh && chmod 600 /root/.ssh/authorized_keys

# ssh port
EXPOSE 22

COPY my_init /sbin/
RUN chown root:root /sbin/my_init && chmod 755 /sbin/my_init
ENV WORKDIR /root/workspace
WORKDIR $WORKDIR

COPY extra.zsh /tmp/.extra
COPY Dockerfile-18.04 /tmp/Dockerfile
RUN grep '^ENV ' /tmp/Dockerfile | awk '{printf "export %s=%s\n",$2,$3}' \
    | cat /tmp/.extra - | tee /root/.extra \
    && rm -rf /tmp/*

CMD ["/sbin/my_init"]
