FROM ubuntu:18.04

MAINTAINER Felipe Marques <180039822@aluno.unb.br>

WORKDIR /work

RUN apt-get update

RUN apt-get upgrade -y

RUN apt-get install -y wget make cmake

# Install KOfam and its dependencies

RUN mkdir -p /work/kofamscan/db ; cd /work/kofamscan/db ; wget ftp://ftp.genome.jp/pub/db/kofam/ko_list.gz ; wget ftp://ftp.genome.jp/pub/db/kofam/profiles.tar.gz ; gunzip ko_list.gz ; tar xvzf profiles.tar.gz
RUN mkdir -p /work/kofamscan/bin ; cd /work/kofamscan/bin ; wget ftp://ftp.genome.jp/pub/tools/kofamscan/kofamscan-1.1.0.tar.gz ; tar xvzf kofamscan-1.1.0.tar.gz ; cp -r kofamscan-1.1.0/* . 
RUN cd /work/kofamscan ; mkdir hmmer src ; cd src ; wget http://eddylab.org/software/hmmer/hmmer.tar.gz ; tar xvzf hmmer.tar.gz ; cd hmmer-3.3 ; ./configure --prefix=/work/kofamscan/hmmer ; make ; make install
RUN mkdir /work/kofamscan/ruby ; cd /work/kofamscan/src ; wget https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.3.tar.gz ; tar xvzf ruby-2.6.3.tar.gz ; cd ruby-2.6.3 ; ./configure --prefix=/work/kofamscan/ruby ; make ; make install 
RUN mkdir /work/kofamscan/parallel ; cd /work/kofamscan/src ; wget https://ftp.gnu.org/gnu/parallel/parallel-latest.tar.bz2 ; tar xvjf parallel-latest.tar.bz2 ; cd parallel-20191022 ; ./configure --prefix=/work/kofamscan/parallel ; make ; make install 

RUN apt-get install -y ncbi-tools-bin

ENV PATH=/work/kofamscan/ruby/bin:$PATH

RUN ln -rs /work/kofamscan/bin/exec_annotation /usr/local/bin/kofamscan

# Edit config.yml
RUN mv /work/kofamscan/bin/config-template.yml /work/kofamscan/bin/config.yml ; echo "profile: /work/kofamscan/db/profiles" >> /work/kofamscan/bin/config.yml ; echo "ko_list: /work/kofamscan/db/ko_list" >> /work/kofamscan/bin/config.yml ; echo "hmmsearch: /work/kofamscan/hmmer/bin/hmmsearch" >> /work/kofamscan/bin/config.yml ; echo "parallel: /work/kofamscan/parallel/bin/parallel" >> /work/kofamscan/bin/config.yml 

WORKDIR /work
