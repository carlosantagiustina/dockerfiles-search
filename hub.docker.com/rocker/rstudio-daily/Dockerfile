FROM rocker/rstudio


RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    libxml2-dev libssl-dev procps rrdtool libclang-dev \
  && install2.r xml2 httr \
  && wget --no-check-certificate \
    https://raw.githubusercontent.com/rocker-org/rstudio-daily/master/latest.R \
  && Rscript latest.R && rm latest.R

RUN dpkg -i rstudio-server-daily-amd64.deb \
  && rm rstudio-server-*-amd64.deb \
  && ln -s -f /usr/lib/rstudio-server/bin/pandoc/pandoc /usr/local/bin \
  && ln -s -f /usr/lib/rstudio-server/bin/pandoc/pandoc-citeproc /usr/local/bin \
  && apt-get clean

