
FROM docker

COPY scripts/*.sh /usr/local/bin/
COPY scripts/*.py /usr/local/bin/
COPY scripts/*.R /usr/local/bin/

RUN apk update && \
    apk upgrade && \  
    apk -v --update add \
        bash \
        python \
        py-pip \
        groff \
        less \
        mailcap \
        && \
    pip install --upgrade awscli==1.14.5 s3cmd==2.0.1 python-magic && \
    pip install boto3 && \
    apk -v --purge del py-pip && \
    rm /var/cache/apk/* && \
    chmod ugo+x /usr/local/bin/*.sh

   

