FROM taskrabbit/elasticsearch-dump:v3.3.1

ADD startup.sh /usr/bin/startup.sh

RUN apk add --update bash curl && \
    rm -rf /var/cache/apk/*

ENV SERVICE_ELASTICSEARCH_HOST elasticsearch
ENV SERVICE_ELASTICSEARCH_PORT 9200
ENV SERVICE_ELASTICSEARCH_USERNAME ""
ENV SERVICE_ELASTICSEARCH_PASSWORD ""
ENV SERVICE_KIBANA_HOST kibana
ENV SERVICE_KIBANA_PORT 5601
ENV ELASTICDUMP_OPTS ""
ENV TARGET_INDEX ".kibana"

ENTRYPOINT ["/usr/bin/startup.sh"]
