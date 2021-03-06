FROM maven:latest as builder

ARG REPO='https://github.com/kubernauts/prometheus_example.git'

WORKDIR /prometheus_example

RUN git clone $REPO . && cd springboot/uninstrumented && mvn package

FROM maven:latest as jmx

ARG REPO='https://github.com/prometheus/jmx_exporter.git'

WORKDIR /jmx_exporter

RUN git clone $REPO . && mvn package

FROM openjdk:8-jdk-slim

ARG BUILD_DATE
ARG VCS_REF

LABEL  org.label-schema.description="A hello world application to demonstrate Prometheus Java Spring Boot instrumentation." \
       org.label-schema.usage="docker run -d -p :8080 -p :8888 kubernautslabs/prometheus_example:jmx" \
       org.label-schema.docker.cmd="docker run -d -p :8080 -p :8888 kubernautslabs/prometheus_example:jmx" \
       org.label-schema.build-date=$BUILD_DATE \
       org.label-schema.name="kubernauts/prometheus_example" \
       org.label-schema.schema-version="1.0.0-rc.1" \
       org.label-schema.url="https://github.com/kubernauts/prometheus_example" \
       org.label-schema.vcs-ref=$VCS_REF \
       org.label-schema.vcs-url="https://github.com/kubernauts/prometheus_example.git" \
       org.label-schema.vendor="kubernauts"

EXPOSE 8080 8888

COPY --from=builder /prometheus_example/springboot/uninstrumented/target/prometheus_example-*-SNAPSHOT.jar /uninstrumented.jar
COPY --from=jmx /jmx_exporter/jmx_prometheus_javaagent/target/jmx_prometheus_javaagent-*-SNAPSHOT.jar /jmx_exporter.jar
COPY --from=jmx /jmx_exporter/example_configs/httpserver_sample_config.yml /config.yml

ENTRYPOINT sed -i 's/5555/8080/g' /config.yml && java -javaagent:/jmx_exporter.jar=8888:/config.yml -jar /uninstrumented.jar
