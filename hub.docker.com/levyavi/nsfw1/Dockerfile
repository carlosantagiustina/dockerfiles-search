FROM nvidia/cuda:7.5-cudnn5-devel-ubuntu14.04
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        git \
        wget \
        libatlas-base-dev \
        libboost-all-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        python-dev \
        python-numpy \
        python-pip \
        python-scipy \
	python-flask && \
    rm -rf /var/lib/apt/lists/*

ENV CAFFE_ROOT=/opt/caffe
WORKDIR $CAFFE_ROOT

# FIXME: clone a specific git tag and use ARG instead of ENV once DockerHub supports this.
ENV CLONE_TAG=master

RUN git clone -b ${CLONE_TAG} --depth 1 https://github.com/BVLC/caffe.git . && \
    for req in $(cat python/requirements.txt) pydot; do pip install $req; done && \
    mkdir build && cd build && \
    cmake -DUSE_CUDNN=1 .. && \
    make -j"$(nproc)"

RUN pip install flask

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

#ENV OPEN_NSFW_REPO "https://github.com/yahoo/open_nsfw.git" 
#ENV OPEN_NSFW_REV "59bca2eea844d804c0ed5d981867dc431f0ab096" 

RUN mkdir /workspace
VOLUME /workspace

#RUN set -x \
#  && mkdir -p /usr/src \
#  && git clone "${OPEN_NSFW_REPO}" /workspace/open_nsfw \
#  && cd /usr/src/open_nsfw \
#  && git checkout "${OPEN_NSFW_REV}"

EXPOSE 5000
WORKDIR /workspace
CMD python /workspace/nsfw/nsfwas.py

