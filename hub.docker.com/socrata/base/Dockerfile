FROM ubuntu:trusty
MAINTAINER Socrata <sysadmin@socrata.com>

# Default to basic no_proxy list for things that respect it such as set_ark_*
ENV no_proxy localhost,127.0.0.1,169.254.169.254,jenkins

# Add a user so containers can run things as non root.  Not perfect since it is shared across containers,
# but eventually uid namespacing will hopefully fix that.
RUN groupadd -r socrata && useradd -r -g socrata socrata
RUN groupadd -g 31415 metrics && usermod -a -G metrics socrata

RUN DEBIAN_FRONTEND=noninteractive apt-get -y update && \
  DEBIAN_FRONTEND=noninteractive apt-get -y dist-upgrade && \
  DEBIAN_FRONTEND=noninteractive apt-get -y install \
    curl \
    dnsutils \
    python-jinja2 \
    ruby2.0 \
    zip \
  && rm -rf /var/lib/apt/lists/*

RUN curl -Lo /bin/envconsul https://github.com/hashicorp/envconsul/releases/download/v0.2.0/envconsul_linux_amd64 && \
    chmod +x /bin/envconsul

RUN mkdir /etc/pre-init.d

# Explicitly require safe_yaml avoid "uninitialized constant Gem::SafeYAML" error due to version nightmares
RUN ruby2.0 -r yaml -r rubygems/safe_yaml -S gem2.0 install --no-document aws-sdk-resources --pre

COPY env_parse set_ark_host set_ark_hostname set_metrics_dir set_local_dev_hostname ship /bin/
COPY ship.d /etc/ship.d/

# Credential management bits
COPY clortho-get /etc/pre-init.d/clortho-get

#Create Data Directory
ENV METRICS_ROOT_DIR /data/metrics
ENTRYPOINT ["/bin/ship"]
CMD ["run"]

# Ensure that containers and apps default to UTF-8
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# LABEL must be last for proper base image discoverability
LABEL repository.socrata/base=""
