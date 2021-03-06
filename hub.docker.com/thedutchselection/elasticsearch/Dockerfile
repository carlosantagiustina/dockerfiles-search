# NAME: Elasticsearch 5.6.8
#
# DESCRIPTION: Elasticsearch image.
#
# REQUIRED ENVS:
# CLUSTER_NAME (ie. "tds_nl")
# EXPECTED_NUMBER_OF_NODES (ie. "6")
# HOST (ie. "10.0.4.3")
# NODE_DATA (true or false)
# NODE_INGEST (true or false)
# NODE_NAME (ie. "doa3wrkprd004_master")
# NODE_MASTER (true or false)
# MAX_LOCAL_STORAGE_NODES (ie. "1")
# MINIMUM_MASTER_NODES (ie. "2")
# MINIMUM_NUMBER_OF_NODES (ie. "4")
# PATH_DATA (ie. "/home/elastic/data/data")
# PATH_LOGS (ie. "/home/elastic/data/logs")
# PATH_REPO (ie. "/home/elastic/data/backups")
# PUBLISH_HOST (ie. "10.0.4.3")
# TRANSPORT_PORT (ie. "9300")
# HTTP_PORT  (ie. "9200")
#
# OPTIONAL ENVS:
# ELASTICSEARCH_MASTER_[APP_ID]_HOST_PRIVATE_IP (ie. "10.0.4.3")
# ELASTICSEARCH_MASTER_[APP_ID]_HOST_PUBLIC_IP (ie. "10.0.4.3")
# ELASTICSEARCH_MASTER_[APP_ID]_HOST_PORT_PEER (ie. "9300")
# ES_JAVA_OPTS (ie. "-Xms2g -Xmx2g", to set heap size to 2GB, max heap size on a node is roughly 50% of all memory, but never larger than 30.5g)
#
# OTHER:
# To upgrade nodes, first disable shard reallocation:
# curl -XPUT localhost:9200/_cluster/settings -d '{"transient":{"cluster.routing.allocation.enable":"none"}}'
# Then shutdown node, upgrade, confirm it joins the cluster, reenable shard reallocation:
# curl -XPUT localhost:9200/_cluster/settings -d '{"transient":{"cluster.routing.allocation.enable":"all"}}'
# Observe that all shards are properly allocated on all nodes. Balancing may take some time. Repeat this process for all
# remaining nodes.

FROM thedutchselection/java:8.stretch
MAINTAINER Gerard Meijer <g.meijer@thedutchselection.com>

ADD https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.8.tar.gz /tmp/

WORKDIR /tmp

RUN \
  apt-get update && \
  apt-get install -yq curl && \
  tar xvzf elasticsearch-5.6.8.tar.gz && \
  mv /tmp/elasticsearch-5.6.8 /usr/local/bin/elasticsearch && \
  useradd -m -u 9200 elastic

# plugins
RUN \
  /usr/local/bin/elasticsearch/bin/elasticsearch-plugin install repository-s3

ADD files/elasticsearch.yml /usr/local/bin/elasticsearch/config/
ADD files/jvm.options /usr/local/bin/elasticsearch/config/
ADD files/log4j2.properties /usr/local/bin/elasticsearch/config/
ADD files/scripts /usr/local/bin

RUN \
  chmod 640 /usr/local/bin/elasticsearch/config/elasticsearch.yml && \
  chmod 640 /usr/local/bin/elasticsearch/config/jvm.options && \
  chmod 640 /usr/local/bin/elasticsearch/config/log4j2.properties && \
  chmod +x /usr/local/bin/run.sh && \
  chown -R elastic:elastic /usr/local/bin/elasticsearch && \
  chown elastic:elastic /usr/local/bin/*

USER elastic

ENTRYPOINT ["/bin/bash", "/usr/local/bin/run.sh"]
