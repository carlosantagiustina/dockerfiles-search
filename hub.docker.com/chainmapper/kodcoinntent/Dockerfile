#################################################################
# Dockerfile
#
# Version:          1.0
# Software:         hisat2
# Software Version: 2.0.5
# Description:      Microbial pan-genome analysis and exploration toolkit
# Code:             https://github.com/neherlab/pan-genome-analysis
# Base Image:       debian:jessie
# Build Cmd:        sudo docker build -t debian_prokka:prokka_latest_docker1.0 .
# Pull Cmd:         docker pull debian_prokka:prokka_latest_docker1.0
# Run Cmd:          sudo docker run -it --rm -v /home:/home --name=prokka debian_prokka:prokka_latest_docker1.0
# File Author / Maintainer: cheng gong <512543469@qq.com>
#################################################################

FROM continuumio/miniconda

RUN conda update --all -y&&\
         conda config --add channels r &&\
         conda config --add channels bioconda &&\
         conda config --set show_channel_urls yes &&\
         conda install htseq=0.7.2  -y

CMD ["/bin/bash"]


