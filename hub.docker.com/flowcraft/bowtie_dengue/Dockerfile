FROM ubuntu:16.04
MAINTAINER Inês Mendes, cimendes@medicina.ulisboa.pt

RUN apt-get update && apt-get -y install \
	bc \
	bzip2 \
	gcc \
	gzip \
	make \
	wget  \
	unzip \
	python \
	python3 \
	parallel \
	zlib1g-dev

WORKDIR /NGStools

RUN wget https://sourceforge.net/projects/samtools/files/samtools/1.4.1/samtools-1.4.1.tar.bz2
RUN mkdir samtools && tar jxf samtools-1.4.1.tar.bz2 -C samtools --strip-components=1

WORKDIR /NGStools/samtools/

RUN ./configure --without-curses --disable-bz2 --disable-lzma
RUN make
ENV PATH="/NGStools/samtools:$PATH"

WORKDIR /NGStools/

RUN rm -rf samtools-1.4.1.tar.bz2

RUN wget https://sourceforge.net/projects/bowtie-bio/files/bowtie2/2.2.9/bowtie2-2.2.9-linux-x86_64.zip && unzip bowtie2-2.2.9-linux-x86_64.zip
ENV PATH="/NGStools/bowtie2-2.2.9:$PATH"

WORKDIR /ref/
RUN wget https://github.com/assemblerflow/DEN-IM/raw/master/ref/1_GenotypesDENV_14-05-18.fasta

WORKDIR /NGStools/
