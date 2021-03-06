FROM quay.io/pypa/manylinux2010_x86_64
MAINTAINER Henri Menke <pairinteraction@henrimenke.de>

# Use Python 3.5
ENV PATH="/opt/python/cp35-cp35m/bin/:$PATH"
ENV PYTHON_INCLUDE_DIR="/opt/python/cp35-cp35m/include/python3.5m/"

# We have to install all kinds of things by hand.
#
# From pip:
#     cmake, numpy, scipy
#
# From the official repos:
#     curl, gsl-devel, pcre-devel
#
#     curl and pcre-devel are required to build other packages from
#     source.
#
# From source:
#     swig, boost
#
#     The sources and build directories are deleted after finishing to
#     save space.
#
RUN pip3 install cmake numpy==1.11.0 scipy==0.17.0 \
  && echo 'echo "pyuic5 $@"' > /usr/local/bin/pyuic5 \
  && chmod +x /usr/local/bin/pyuic5 \
  && yum install -y curl gsl-devel pcre-devel \
  && curl -LO http://prdownloads.sourceforge.net/swig/swig-3.0.12.tar.gz \
  && tar xzvf swig-3.0.12.tar.gz \
  && cd swig-3.0.12 \
  && ./configure \
  && make \
  && make install \
  && cd .. \
  && rm -rf swig-3.0.12* \
  && curl -LO https://dl.bintray.com/boostorg/release/1.69.0/source/boost_1_69_0.tar.gz \
  && tar xzvf boost_1_69_0.tar.gz \
  && cd boost_1_69_0 \
  && ./bootstrap.sh \
  && ./b2 --without-atomic --without-chrono --without-container --without-context \
      --without-contract --without-coroutine --without-date_time --without-exception \
      --without-fiber --without-graph --without-graph_parallel --without-iostreams \
      --without-locale --without-log --without-math --without-mpi --without-python \
      --without-random --without-regex --without-stacktrace --without-thread \
      --without-timer --without-type_erasure --without-wave \
      install \
  && cd .. \
  && rm -rf boost_1_69_0*
