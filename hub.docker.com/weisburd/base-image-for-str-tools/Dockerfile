# This base image contains dependencies used by 2 or more STR caller containers.
# It uses bitnami/minideb:stretch as its base image so it'll have CLI utils expected by Cromwell.

FROM bitnami/minideb:stretch

MAINTAINER Ben Weisburd

ENV PYTHON_VERSION="3.7.1"
ENV GATK_VERSION="4.1.0.0"
ENV PICARD_VERSION="2.18.21"
ENV SAMTOOLS_VERSION="1.9"
ENV VCF_TOOLS_VERSION="0.1.16"
ENV BED_TOOLS_VERSION="2.25.0"
ENV GCLOUD_SDK_VERISON="237.0.0"

RUN install_packages ca-certificates wget pkg-config gcc g++ make bzip2 zlib1g-dev dpkg-dev build-essential libcurl4-openssl-dev libbz2-dev liblzma-dev

# python2 utils
RUN install_packages python-dev python-setuptools python-pip \
	&& pip install --upgrade wheel

# install python3.7
RUN apt-get update \
	&& apt-get dist-upgrade -y \
	&& apt-get install -y python-smbus libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev libssl-dev openssl libffi-dev

RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tar.xz \
	&& tar xvf Python-${PYTHON_VERSION}.tar.xz \
	&& rm Python-${PYTHON_VERSION}.tar.xz \
	&& cd Python-${PYTHON_VERSION} \
	&& ./configure --enable-optimizations \
	&& make install

# install java-8
RUN apt install -y default-jdk

# install picard
RUN wget https://github.com/broadinstitute/picard/releases/download/${PICARD_VERSION}/picard.jar -O /picard.jar

# install GATK
RUN wget https://github.com/broadinstitute/gatk/releases/download/${GATK_VERSION}/gatk-${GATK_VERSION}.zip \
	&& apt install -y unzip \
	&& unzip gatk-${GATK_VERSION}.zip \
	&& rm gatk-${GATK_VERSION}.zip \
	&& ln -s gatk-${GATK_VERSION}/gatk-package-${GATK_VERSION}-local.jar /gatk.jar


# install samtools
RUN wget https://github.com/samtools/samtools/releases/download/${SAMTOOLS_VERSION}/samtools-${SAMTOOLS_VERSION}.tar.bz2 \
	&& tar xjf samtools-${SAMTOOLS_VERSION}.tar.bz2 \
	&& rm samtools-${SAMTOOLS_VERSION}.tar.bz2 \
	&& cd samtools-${SAMTOOLS_VERSION} \
	&& ./configure \
	&& make \
	&& make install

# install htslib
RUN wget https://github.com/samtools/htslib/releases/download/${SAMTOOLS_VERSION}/htslib-${SAMTOOLS_VERSION}.tar.bz2 \
	&& tar xjf htslib-${SAMTOOLS_VERSION}.tar.bz2 \
	&& rm htslib-${SAMTOOLS_VERSION}.tar.bz2 \
	&& cd htslib-${SAMTOOLS_VERSION} \
	&& ./configure \
	&& make \
	&& make install

# install bcftools for post-processing and merging scattered output vcfs
# https://github.com/samtools/bcftools/archive/develop.zip -O bcftools-develop.zip
RUN apt install -y autoconf unzip \
	&& wget https://github.com/samtools/bcftools/archive/95898767c48457d8bee4a0fb3a813316cc65042f.zip -O bcftools-develop.zip \
	&& unzip bcftools-develop.zip \
	&& rm bcftools-develop.zip \
	&& cd bcftools-* \
	&& autoconf \
	&& autoheader \
	&& ./configure \
	&& make \
	&& make install

# install vcf-tools
RUN wget https://github.com/vcftools/vcftools/releases/download/v${VCF_TOOLS_VERSION}/vcftools-${VCF_TOOLS_VERSION}.tar.gz \
	&& tar xzf vcftools-${VCF_TOOLS_VERSION}.tar.gz \
	&& rm vcftools-${VCF_TOOLS_VERSION}.tar.gz \
	&& cd vcftools-${VCF_TOOLS_VERSION} \
	&& ./configure \
    && make \
    && make install

# install bedtools
RUN wget https://github.com/arq5x/bedtools2/releases/download/v${BED_TOOLS_VERSION}/bedtools-${BED_TOOLS_VERSION}.tar.gz \
	&& tar xzf bedtools-${BED_TOOLS_VERSION}.tar.gz \
	&& rm bedtools-${BED_TOOLS_VERSION}.tar.gz \
	&& cd bedtools2 \
	&& make \
	&& make install

# install verticalize (https://github.com/lindenb/verticalize)
RUN wget https://github.com/lindenb/verticalize/archive/v1.0.1.tar.gz -O verticalize-1.0.1.tar.gz \
	&& tar xzf verticalize-1.0.1.tar.gz \
	&& cd verticalize-1.0.1 \
	&& make \
	&& mv verticalize /usr/local/bin \
	&& cd .. \
	&& rm -rf verticalize-1.0.1*

# install vt (https://github.com/atks/vt)
RUN wget https://github.com/atks/vt/archive/0.57721.tar.gz -O vt-0.57721.tar.gz \
	&& tar xzf vt-0.57721.tar.gz \
	&& cd vt-0.57721 \
	&& make \
	&& mv vt /usr/local/bin \
	&& cd .. \
	&& rm -rf vt-0.57721*

# igvtools
RUN wget http://data.broadinstitute.org/igv/projects/downloads/2.4/igvtools_2.4.17.zip \
	&& unzip igvtools_2.4.17

# nice-to-have linux utilities
RUN install_packages emacs vim less curl htop xterm git

# install gcloud
RUN wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${GCLOUD_SDK_VERISON}-linux-x86_64.tar.gz \
    && tar xzf google-cloud-sdk-${GCLOUD_SDK_VERISON}-linux-x86_64.tar.gz \
    && /google-cloud-sdk/install.sh -q \
    && /google-cloud-sdk/bin/gcloud components update --quiet

ENV PATH=/google-cloud-sdk/bin:$PATH

# tandem repeat finder
RUN wget http://tandem.bu.edu/trf/downloads/trf409.linux64 \
	&& chmod 777 /trf409.linux64

# python3 packages
RUN python3 -m pip install --upgrade pip \
	&& python3 -m pip install --upgrade tqdm pyvcf pyfaidx cyvcf2 peewee scipy


COPY bashrc /root/.bashrc




