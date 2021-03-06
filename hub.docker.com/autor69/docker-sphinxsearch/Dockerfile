# Dockerfile for Sphinx SE
FROM debian:stretch
ENV SPHINX_VERSION 3.0.3-facc3fb

# install dependencies
RUN apt-get update && apt-get install -y \
        default-libmysqlclient-dev \
        libpq-dev \
        wget

# set timezone
# @see http://unix.stackexchange.com/a/76711
RUN cp /usr/share/zoneinfo/CET /etc/localtime && dpkg-reconfigure --frontend noninteractive tzdata

# set up and expose directories
RUN mkdir -pv /opt/sphinx/log /opt/sphinx/index
VOLUME /opt/sphinx/index

# http://sphinxsearch.com/files/sphinx-3.0.3-facc3fb-linux-amd64.tar.gz
RUN wget http://sphinxsearch.com/files/sphinx-${SPHINX_VERSION}-linux-amd64.tar.gz -O /tmp/sphinxsearch.tar.gz
RUN cd /opt/sphinx && tar -xf /tmp/sphinxsearch.tar.gz
RUN rm /tmp/sphinxsearch.tar.gz

# point to sphinx binaries
ENV PATH "${PATH}:/opt/sphinx/sphinx-3.0.3/bin"
ADD sphinx.conf /opt/sphinx/conf/sphinx.conf
ADD indexsearch.sh /opt/sphinx/sphinx-3.0.3/bin/indexsearch
RUN chmod a+x /opt/sphinx/sphinx-3.0.3/bin/indexsearch
RUN indexer -v --config /opt/sphinx/conf/sphinx.conf

# redirect logs to stdout
RUN ln -sv /dev/stdout /opt/sphinx/log/query.log
RUN ln -sv /dev/stdout /opt/sphinx/log/searchd.log

# expose TCP ports
EXPOSE 9312
EXPOSE 9306

WORKDIR /opt/sphinx
CMD indexsearch