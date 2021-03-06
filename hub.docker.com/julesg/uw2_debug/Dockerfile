# This image needs to be built from the top level project directory.
# Typically:
#    $  docker build -f docs/development/docker/underworld2/Dockerfile .


FROM underworldcode/base@sha256:12016775a555410515783a49c4fb9b3f4edca1e084a0fba656af7c5e7529539b
MAINTAINER https://github.com/underworldcode/

USER root
# install things
RUN apt-get update -qq && \
    DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
        cgdb         \
        valgrind  && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# set working directory to /opt, and install underworld files there.
WORKDIR /opt
ENV UW2_DIR /opt/underworld2
RUN mkdir $UW2_DIR
ENV PYTHONPATH $PYTHONPATH:$UW2_DIR

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.5 1

# install lavavu
RUN pip3 install --no-cache-dir lavavu ipdb

# COPY UW
#COPY --chown=jovyan:users . $UW2_DIR/   # unfortunately, the version of docker at docker cloud does not support chown yet.
COPY . $UW2_DIR/

# get underworld, compile, delete some unnecessary files, trust notebooks, copy to workspace
RUN cd underworld2/libUnderworld && \
    ./configure.py --with-debugging=1  && \
    ./compile.py                 && \
    rm -fr h5py_ext              && \
    rm .sconsign.dblite          && \
    rm -fr .sconf_temp           && \
    cd build                     && \
    find $UW2_DIR/docs -name \*.ipynb  -print0 | xargs -0 jupyter trust && \
#    cd ../../docs/development/api_doc_generator/                     && \
#    sphinx-build . ../../api_doc                                     && \
    rsync -av $UW2_DIR/docs/. $NB_WORK                               && \
    cd /opt/underworld2                                              && \
    find . -name \*.os |xargs rm -f                                  && \
    cat .git/refs/heads/* > build_commit.txt                         && \
    env > build_environment.txt                                      && \
    rm -fr .git                                                      && \
    chown -R $NB_USER:users $NB_WORK $UW2_DIR /home/$NB_USER

RUN git clone https://github.com/underworldcode/UWGeodynamics.git && \
    pip3 install --no-cache-dir UWGeodynamics/ && \
    mkdir /workspace/UWGeodynamics && \
    mv ./UWGeodynamics/examples /workspace/UWGeodynamics/. && \
    mv ./UWGeodynamics/tutorials /workspace/UWGeodynamics/. && \
    mv ./UWGeodynamics/benchmarks /workspace/UWGeodynamics/. && \
    mv ./UWGeodynamics/docs /workspace/UWGeodynamics/ && \
    rm -rf UWGeodynamics && \
    chown -R $NB_USER:users /workspace/UWGeodynamics

# expose glucifer port
EXPOSE 9999

# environment variable will internally run xvfb when glucifer is imported,
# see /opt/underworld2/glucifer/__init__.py
ENV GLUCIFER_USE_XVFB 1

# LEAVE USER AS ROOT
#USER $NB_USER
WORKDIR $NB_WORK

