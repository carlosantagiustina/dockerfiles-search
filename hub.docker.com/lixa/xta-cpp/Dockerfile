# A Docker image with
# XTA (XA Transaction API) for C++ language
# based on Debian Linux
FROM	debian:stretch-slim
#
# Build without parameters to build latest
#	docker build -t lixa/xta-cpp .
# Build with 2 parameters to build a specific release
#	docker build --build-arg VERSION_CODE=1.7.8 \
#		--build-arg VERSION_TYPE=testing -t lixa/xta-cpp:1.7.8 .
#   ** Note: version code must be of the right type and must exists in
#            Sourceforge.net
# Inspect the image content with
#	docker run --rm -it lixa/xta-cpp bash
#
# Update Ubuntu Software repository
ARG 	DEBIAN_FRONTEND=noninteractive
# Install generic pre-requisites
RUN	apt-get update && apt-get install -y --no-install-recommends \
	apt-utils \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/log/* 
# Install run-time pre-requisites
RUN	apt-get update && apt-get install -y --no-install-recommends \
	libxml2 libglib2.0 uuid-runtime libpq5 libmariadbclient18 \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/log/* 
# LIXA official package
# VERSION_CODE must be in format V.R.M and it must be available on Sourceforge
# VERSION_TYPE must be "stable" or "testing"
# elsewhere, latest will be used
ARG	VERSION_CODE
ARG	VERSION_TYPE
ENV	VERSION_CODE $VERSION_CODE
ENV	VERSION_TYPE $VERSION_TYPE
ENV	LIXA_VERSION $VERSION
ENV	LIXA_DOWNLOAD_DEF_URL https://sourceforge.net/projects/lixa/files/latest/download
ENV	LIXA_DOWNLOAD_VER_URL https://sourceforge.net/projects/lixa/files/lixa/$VERSION_TYPE/lixa-$VERSION_CODE.tar.gz/download
# build LIXA
RUN	BUILD_DEPS='ca-certificates wget make gcc g++ pkg-config libxml2-dev libglib2.0-dev uuid-dev libpq-dev default-libmysqlclient-dev' \
	&& set -x \
	&& apt-get update && apt-get install -y --no-install-recommends \
		$BUILD_DEPS \
	&& mkdir -p /lixa \
	&& if [ "z$VERSION_CODE" != "z" -a "z$VERSION_TYPE" != "z" ]; \
		then wget -O lixa.tar.gz "$LIXA_DOWNLOAD_VER_URL"; \
		else wget -O lixa.tar.gz "$LIXA_DOWNLOAD_DEF_URL"; \
	   fi \
	&& tar -xzf lixa.tar.gz -C /lixa \
	&& rm lixa.tar.gz \
	&& cd /lixa/lixa-* \
	&& ./configure --disable-syslog --disable-server \
		--disable-xta-java --disable-xta-python \
		--with-mysql --with-postgresql \
	&& make && make install \
	&& rm -rf /opt/lixa/share \
	&& rm -r /lixa \
	&& apt-get purge -y --auto-remove $BUILD_DEPS \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/log/* /opt/lixa/var \
	&& mv -v /opt/lixa/etc/lixac1_conf.xml /opt/lixa/etc/lixac_conf.xml \
	&& rm /opt/lixa/etc/lixad*_conf.xml
