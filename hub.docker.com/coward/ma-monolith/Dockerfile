# --- Base ---
FROM ubuntu:16.04 as openalpr

RUN apt-get update && \
    apt-get install -y software-properties-common

RUN apt-add-repository -y ppa:jonathonf/ffmpeg-3 && \
        apt-get update && \
        apt-get install -y \
        build-essential \
        cmake \
        curl \
        git \
        wget \
        unzip \
        yasm \
        libgtk2.0-dev \
        pkg-config \
        libavcodec-dev \
        libswscale-dev \
        libtbb2 \
        libtbb-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libjasper-dev \
        libavformat-dev \
        libpq-dev \
        libcurl3-dev \
        libleptonica-dev \
        liblog4cplus-dev \
        libtesseract-dev \
        ocl-icd-libopencl1 \
        opencl-headers \
        ocl-icd-opencl-dev \
        beignet \
        ffmpeg \
        libavresample-dev

WORKDIR /
RUN wget https://github.com/opencv/opencv/archive/3.3.0.zip \
    && unzip 3.3.0.zip \
    && mkdir /opencv-3.3.0/cmake_binary \
    && cd /opencv-3.3.0/cmake_binary \
    && cmake -DBUILD_TIFF=ON \
      -DBUILD_opencv_java=OFF \
      -DWITH_CUDA=OFF \
      -DENABLE_AVX=ON \
      -DWITH_OPENGL=ON \
      -DWITH_OPENCL=ON \
      -DWITH_IPP=ON \
      -DWITH_TBB=ON \
      -DWITH_EIGEN=ON \
      -DWITH_V4L=ON \
      -DBUILD_TESTS=OFF \
      -DBUILD_PERF_TESTS=OFF \
      -DCMAKE_BUILD_TYPE=RELEASE \
      -DCMAKE_INSTALL_PREFIX=/usr .. \
    && make -j4 \
    && make install \
    && rm /3.3.0.zip \
    && rm -r /opencv-3.3.0

RUN git clone https://github.com/openalpr/openalpr.git /srv/openalpr \
    && mkdir /srv/openalpr/src/build

WORKDIR /srv/openalpr/src/build

# Setup the compile environment
RUN cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_INSTALL_SYSCONFDIR:PATH=/etc .. && \
    make -j2 && \
    make install && rm -r /srv

#
#
# --- Builder ---
FROM openalpr as builder

RUN apt-get update && \
    apt-get install -y curl git build-essential && \
    curl -O https://storage.googleapis.com/golang/go1.9.linux-amd64.tar.gz && \
    tar -xvf go1.9.linux-amd64.tar.gz && \
    mv go /usr/local && \
    export PATH="/usr/local/go/bin:$PATH"

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

COPY ./ /go/src/github.com/dgmann/ma-monolith

WORKDIR /go/src/github.com/dgmann/ma-monolith

RUN go get -d -v ./... && CGO_ENABLED=1 GOOS=linux go build -a -installsuffix cgo -o /monolith .

#
#
# --- Release ---
FROM openalpr as release

WORKDIR /
COPY --from=builder /monolith /monolith
COPY ./openalpr.conf /

ENV RUNTIME_DATA_DIR="/usr/share/openalpr/runtime_data" REDIS_HOST=redis
VOLUME /stats

ENTRYPOINT ["/monolith"]
