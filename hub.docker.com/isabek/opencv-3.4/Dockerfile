FROM ubuntu:16.04

LABEL maintainer="isabek2309@gmail.com"

ENV NUM_CORES 6

RUN apt-get -y update

RUN apt-get -y install build-essential cmake git wget libgtk2.0-dev unzip \
                       pkg-config libavcodec-dev libavformat-dev libswscale-dev \
                       libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

RUN apt-get -y install libboost-dev libboost-all-dev

RUN wget https://github.com/opencv/opencv/archive/3.4.0.zip -O opencv-3.4.0.zip && \
    unzip -q opencv-3.4.0.zip && mv /opencv-3.4.0 /opencv

RUN wget https://github.com/opencv/opencv_contrib/archive/3.4.0.zip -O opencv_contrib-3.4.0.zip && \
    unzip -q opencv_contrib-3.4.0.zip && mv /opencv_contrib-3.4.0 /opencv_contrib

RUN mkdir /opencv/build
WORKDIR /opencv/build

RUN cmake -D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
	-D INSTALL_PYTHON_EXAMPLES=OFF \
	-D OPENCV_EXTRA_MODULES_PATH=/opencv_contrib/modules \
	-D BUILD_TESTS=OFF \
    -D WITH_QT=OFF \
    -D BUILD_PERF_TESTS=OFF \
	-D BUILD_PYTHON_SUPPORT=OFF \
	-D BUILD_EXAMPLES=OFF \
	-D BUILD_NEW_PYTHON_SUPPORT=OFF \
	-D BUILD_DOCS=OFF \
    -D BUILD_opencv_python2=OFF \
    -D BUILD_opencv_python3=OFF \
    -D BUILD_JAVA=OFF \
    -D BUILD_opencv_java=OFF \
	-D WITH_IPP=OFF \
	-D WITH_V4L=ON ..

RUN make -j$NUM_CORES
RUN make install
RUN ldconfig

WORKDIR /
RUN wget https://github.com/edenhill/librdkafka/archive/v0.11.3.zip && \
    unzip -q v0.11.3.zip && mv /librdkafka-0.11.3 /librdkafka

WORKDIR /librdkafka
RUN ./configure
RUN make
RUN make install

WORKDIR /
RUN wget https://github.com/mfontanini/cppkafka/archive/v0.1.zip && \
    unzip -q v0.1.zip && mv /cppkafka-0.1 /cppkafka
RUN mkdir /cppkafka/build
WORKDIR /cppkafka/build
RUN cmake ..
RUN make
RUN make install

WORKDIR /home