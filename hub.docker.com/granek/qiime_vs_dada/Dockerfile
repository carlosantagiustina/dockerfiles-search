## Derived from https://raw.githubusercontent.com/jupyter/docker-stacks/master/base-notebook/Dockerfile

# Distributed under the terms of the Modified BSD License.

FROM rocker/rstudio:3.4.0

MAINTAINER Josh Granek

USER root

RUN echo "deb http://ftp.debian.org/debian jessie-backports main" >  /etc/apt/sources.list.d/backports.list && \
    apt-get update &&  \
    apt-get -t jessie-backports install -y --no-install-recommends \
    libxml2-dev \
    wget \
    bzip2 \
    ca-certificates \
    sudo \
    locales \
    less \    
    make \
    seqtk \
    art-nextgen-simulation-tools \
    art-nextgen-simulation-tools-profiles \
    libsm6 libxrender1 libfontconfig1 \
    texlive-latex-base \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# As per https://github.com/conda/conda/issues/1051: The minimal debian dependencies for anaconda are: apt-get install libsm6 libxrender1 libfontconfig1

# Configure environment
ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH
ENV SHELL /bin/bash
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
ENV RSTUDIO_USER rstudio

# Create jovyan user with UID=1000 and in the 'users' group
RUN mkdir -p $CONDA_DIR && \
    chown $RSTUDIO_USER $CONDA_DIR

RUN Rscript -e "install.packages(pkgs = c('ggplot2','dplyr','argparse','phangorn','tidyr','formatR','fuzzyjoin'), \
    repos='https://cran.revolutionanalytics.com/', \
    dependencies=TRUE)" && \
    Rscript -e "source('https://bioconductor.org/biocLite.R'); \
    biocLite(pkgs=c('dada2','ShortRead','phyloseq','msa','DESeq2','metagenomeSeq'))"

##------------------------------------------------------------
USER $RSTUDIO_USER

# Install conda as ?????
RUN cd /tmp && \
    mkdir -p $CONDA_DIR && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-4.1.11-Linux-x86_64.sh && \
    echo "efd6a9362fc6b4085f599a881d20e57de628da8c1a898c08ec82874f3bad41bf *Miniconda3-4.1.11-Linux-x86_64.sh" | sha256sum -c - && \
    /bin/bash Miniconda3-4.1.11-Linux-x86_64.sh -f -b -p $CONDA_DIR && \
    rm Miniconda3-4.1.11-Linux-x86_64.sh && \
    $CONDA_DIR/bin/conda install --quiet --yes conda==4.1.11 && \
    $CONDA_DIR/bin/conda config --system --add channels conda-forge && \
    $CONDA_DIR/bin/conda config --system --add channels bioconda && \
    $CONDA_DIR/bin/conda config --system --add channels BjornFJohansson && \
    $CONDA_DIR/bin/conda config --system --set auto_update_conda false && \
    $CONDA_DIR/bin/conda install --quiet --yes ea-utils pydna && \
    conda clean -tipsy

# Install qiime1 notebook as 
RUN conda create -n qiime1 python=2.7 qiime matplotlib=1.4.3 mock nose -c bioconda && \
    conda clean -tipsy

##------------------------------------------------------------
USER root

CMD ["/init"]
