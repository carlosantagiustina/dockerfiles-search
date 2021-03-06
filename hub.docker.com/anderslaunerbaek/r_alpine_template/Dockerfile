FROM alpine:latest

# inspired by: https://raw.githubusercontent.com/artemklevtsov/r-alpine/master/release/Dockerfile
MAINTAINER Anders Launer Baek anderslaunerbaek@gmail.com

ARG R_VERSION
ENV R_VERSION ${R_VERSION:-3.6.1}
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

# R runtime dependencies
RUN apk --no-cache add \
        gcc \
        gfortran \
        g++ \
        make \
        readline-dev \
        icu-dev \
        bzip2-dev \
        xz-dev \
        pcre-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        tiff-dev  \
        curl-dev \
        zip \
        file \
        coreutils \
        libxml2-dev \
        libressl-dev \
        sqlite-dev \
        mariadb-dev \
        postgresql-dev \
        bash && \
# R build dependencies
    apk --no-cache add --virtual build-deps \
        curl \
        perl \
        openjdk8-jre-base \
        pango-dev \
        cairo-dev \
        tcl-dev \
        tk-dev && \
    cd /tmp && \
# Download source code
    curl -O https://cran.r-project.org/src/base/R-${R_VERSION:0:1}/R-${R_VERSION}.tar.gz && \
# Extract source code
    tar -xf R-${R_VERSION}.tar.gz && \
    cd R-${R_VERSION} && \
# Sect compiler flags
    CFLAGS="-g -O2 -fstack-protector-strong -D_DEFAULT_SOURCE -D__USE_MISC" \
    CXXFLAGS="-g -O2 -fstack-protector-strong -D_FORTIFY_SOURCE=2 -D__MUSL__" \
# configure script options
    ./configure --prefix=/usr \
                --sysconfdir=/etc/R \
                --localstatedir=/var \
                rdocdir=/usr/share/doc/R \
                rincludedir=/usr/include/R \
                rsharedir=/usr/share/R \
                --enable-memory-profiling \
                --enable-R-shlib \
                --disable-nls \
                --without-x \
                --without-recommended-packages && \
# Build and install R
    make && \
    make install && \
    cd src/nmath/standalone && \
    make && \
    make install && \
# Remove build dependencies
    apk del --purge --rdepends build-deps && \
    rm -f /usr/lib/R/bin/R && \
    ln -s /usr/bin/R /usr/lib/R/bin/R && \
# Fis library path
    echo "R_LIBS_SITE=\${R_LIBS_SITE-'/usr/local/lib/R/site-library:/usr/lib/R/library'}" >> /usr/lib/R/etc/Renviron && \
# Add default CRAN mirror
    echo 'options(repos = c(CRAN = "https://cloud.r-project.org/"))' >> /usr/lib/R/etc/Rprofile.site && \
# Add symlinks for the config ifile in /etc/R
    mkdir -p /etc/R && \
    ln -s /usr/lib/R/etc/* /etc/R/ && \
# Add library directory
    mkdir -p /usr/local/lib/R/site-library && \
    chgrp users /usr/local/lib/R/site-library && \
# Clean up
    rm -f /R-${R_VERSION}.tar.gz

ENV RPKGS dplyr
RUN Rscript -e 'install.packages(commandArgs(TRUE))' ${RPKGS} && \
# Clean up
    rm -rf /usr/lib/R/library/translations && \
    rm -rf /tmp/*


#CMD ["R"]
