FROM gliderlabs/alpine:3.8

WORKDIR /tmp
# install packages and symlink libs
RUN \
 echo "**** install build packages ****" && \
 apk add --no-cache --virtual=build-dependencies \
	autoconf \
	automake \
	freetype-dev \
	g++ \
	gcc \
	jpeg-dev \
	lcms2-dev \
	libffi-dev \
	libpng-dev \
	libwebp-dev \
	linux-headers \
	make \
	openjpeg-dev \
	openssl-dev \
	python3-dev \
	tiff-dev \
	zlib-dev && \
 echo "**** install runtime packages ****" && \
 apk add --no-cache \
	curl \
	python3 \
        unzip \
	nano \
	wget \
   tzdata \
	htop \
	mc \
	zlib && \
 echo "**** install pip packages ****" && \
 pip3 install --no-cache-dir -U \
	pip && \
 pip3 install --no-cache-dir -U \
	psutil \
	gevent && \
 echo "**** clean up ****" && \
 apk del --purge \
	build-dependencies \
	zlib \ 
	curl && \
 mkdir -p \
	/opt/tv && \
# install aceproxy
 wget -o - https://github.com/pepsik-kiev/HTTPAceProxy/archive/master.zip -O aceproxy.zip && \
 unzip aceproxy.zip -d /opt/tv && \
#acestream
 mkdir -p /opt/acestream.engine/ && \
 wget -o - https://sybdata.de/files/public-docs/acestream_3.1.33_x86_webUI.tar.gz && \
 tar -zxvf acestream_3.1.33_x86_webUI.tar.gz && \
 mv acestream.engine/ /opt/ && \
 find /opt/acestream.engine/androidfs/system -type d -exec chmod 755 {} \; && \
 find /opt/acestream.engine/androidfs/system -type f -exec chmod 644 {} \; && \
 chmod 755 /opt/acestream.engine/androidfs/system/bin/* /opt/acestream.engine/androidfs/acestream.engine/python/bin/python && \
# cleanup
 rm -rf \
	/tmp/*  
 # add services
ADD acestream.start /opt/acestream.engine/acestream.start
ADD acestream.stop /opt/acestream.engine/acestream.stop
ADD aceconfig.py /opt/tv/HTTPAceProxy-master/aceconfig.py
ADD acestream.conf /opt/acestream.engine/androidfs/acestream.engine/acestream.conf
ADD start.sh /start.sh
RUN chmod +x /start.sh
RUN chmod +x /opt/acestream.engine/acestream.start
RUN chmod +x /opt/acestream.engine/acestream.stop

EXPOSE 8621 9955 62062 6878 8000

WORKDIR /
