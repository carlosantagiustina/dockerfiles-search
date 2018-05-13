# Copyright (c) 2016 Kaito Udagawa
# Copyright (c) 2016-2017 3846masa
# Copyright (c) 2017 rn0rno
# Released under the MIT license
# https://opensource.org/licenses/MIT

FROM frolvlad/alpine-glibc

MAINTAINER rn0rno

ENV PATH /usr/local/texlive/2018/bin/x86_64-linuxmusl:$PATH
ENV PATH /usr/local/texlive/2018/texmf-dist/doc/man:$PATH
ENV PATH /usr/local/texlive/2018/texmf-dist/doc/info:$PATH

RUN apk --no-cache add perl wget xz tar fontconfig-dev ghostscript&& \
    mkdir /tmp/install-tl-unx && \
    wget -qO- http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz | \
    tar -xz -C /tmp/install-tl-unx --strip-components=1 && \
    printf "%s\n" \
      "selected_scheme scheme-basic" \
      "option_doc 0" \
      "option_src 0" \
      > /tmp/install-tl-unx/texlive.profile && \
    /tmp/install-tl-unx/install-tl \
      --profile=/tmp/install-tl-unx/texlive.profile && \
    tlmgr install \
      collection-basic collection-latex \
      collection-latexrecommended collection-latexextra \
      collection-fontsrecommended collection-langjapanese latexmk && \
    ( tlmgr install xetex || exit 0 ) && \
    rm -fr /tmp/install-tl-unx && \
    apk --no-cache del xz tar fontconfig-dev

RUN LANG=ja_JP.UTF8 tlmgr postaction install script ptex2pdf

RUN apk --no-cache add bash

COPY .latexmkrc /root/

RUN mkdir /workdir

RUN mkdir /workdir/share

WORKDIR /workdir

VOLUME ["/workdir"]

VOLUME ["/workdir/share"]

CMD ["bash"]
