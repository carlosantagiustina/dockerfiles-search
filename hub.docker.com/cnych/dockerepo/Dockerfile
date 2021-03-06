FROM jenkins/jenkins:lts

USER root
ENV \
    TERM="xterm" \
    LANG=zh_CN.UTF-8 \
    TIMEZONE="Asia/Shanghai"

RUN \
    echo "${TIMEZONE}" > /etc/timezone \
    && apt-get update -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -yq apt-utils dialog locales \
    && ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales

# Install required packages
RUN apt-get dist-upgrade -y

WORKDIR /tmp

# $JENKINS_HOME  Jenkins数据目录

ENV JENKINS_FOLDER /usr/share/jenkins

# Build Args
ARG BLUEOCEAN_VERSION=1.5.0
ARG JENKINS_STAGING=/usr/share/jenkins/ref/
ARG CURL_RETRY=20
ARG CURL_RETRY_MAX_TIME=900

RUN apt-get update && apt-get install -y python zip jq git

# 添加java默认 DNS 查询缓存时间
RUN echo 'networkaddress.cache.ttl=60' >> ${JAVA_HOME}/jre/lib/security/java.security

RUN mkdir -p "$JENKINS_HOME" "${JENKINS_FOLDER}/war"

# jenkins setup
COPY config.xml "${JENKINS_STAGING}/config.xml"

# add plugins
RUN /usr/local/bin/install-plugins.sh       \
  blueocean-bitbucket-pipeline:${BLUEOCEAN_VERSION}    \
  blueocean-commons:${BLUEOCEAN_VERSION}    \
  blueocean-config:${BLUEOCEAN_VERSION}     \
  blueocean-dashboard:${BLUEOCEAN_VERSION}  \
  blueocean-events:${BLUEOCEAN_VERSION}     \
  blueocean-git-pipeline:${BLUEOCEAN_VERSION}          \
  blueocean-github-pipeline:${BLUEOCEAN_VERSION}       \
  blueocean-i18n:${BLUEOCEAN_VERSION}       \
  blueocean-jwt:${BLUEOCEAN_VERSION}        \
  blueocean-jira:${BLUEOCEAN_VERSION}       \
  blueocean-personalization:${BLUEOCEAN_VERSION}        \
  blueocean-pipeline-api-impl:${BLUEOCEAN_VERSION}      \
  blueocean-pipeline-editor:${BLUEOCEAN_VERSION}        \
  blueocean-pipeline-scm-api:${BLUEOCEAN_VERSION}       \
  blueocean-rest-impl:${BLUEOCEAN_VERSION}  \
  blueocean-rest:${BLUEOCEAN_VERSION}       \
  blueocean-web:${BLUEOCEAN_VERSION}        \
  blueocean:${BLUEOCEAN_VERSION}            \
  ace-editor:1.1                 \
  analysis-core:1.93             \
  ansible:0.6.2                  \
  ansicolor:0.5.2                \
  antisamy-markup-formatter:1.5  \
  artifactory:2.14.0             \
  audit-trail:2.2                \
  authentication-tokens:1.3      \
  branch-api:2.0.17              \
  build-failure-analyzer:1.19.1  \
  build-name-setter:1.6.7        \
  build-pipeline-plugin:1.5.8    \
  build-timeout:1.19             \
  build-token-root:1.4           \
  cloudbees-folder:6.3           \
  cloverphp:0.5                  \
  conditional-buildstep:1.3.6    \
  config-file-provider:2.16.4    \
  copy-to-slave:1.4.4            \
  copyartifact:1.39              \
  dashboard-view:2.9.11          \
  delivery-pipeline-plugin:1.1.0 \
  description-setter:1.10        \
  dingding-notifications:1.4     \
  docker-build-publish:1.3.2     \
  docker-workflow:1.14           \
  durable-task:1.17              \
  ec2:1.38                       \
  email-ext:2.61                 \
  embeddable-build-status:1.9    \
  envinject:2.1.5                \
  extended-read-permission:2.0   \
  external-monitor-job:1.7       \
  favorite:2.3.1                 \
  ghprb:1.39.0                   \
  git:3.8.0                      \
  git-client:2.7.1               \
  git-server:1.7                 \
  github:1.29.0                  \
  github-api:1.90                \
  github-branch-source:2.3.3     \
  github-organization-folder:1.6 \
  gitlab-plugin:1.5.5            \
  gradle:1.28                    \
  greenballs:1.15                \
  handlebars:1.1.1               \
  ivy:1.28                       \
  jackson2-api:2.8.10.1          \
  jenkins-multijob-plugin:1.28   \
  job-dsl:1.66                   \
  jobConfigHistory:2.18          \
  jquery:1.12.4-0                \
  kpp-management-plugin:1.0.0    \
  kubernetes:1.1.2               \
  kubernetes-ci:1.3              \
  kubernetes-cd:0.1.4            \
  kubernetes-pipeline-steps:1.5  \
  ldap:1.18                      \
  mapdb-api:1.0.9.0              \
  marathon:1.6.0                 \
  matrix-auth:2.2                \
  matrix-project:1.12            \
  maven-plugin:3.0               \
  mesos:0.15.0                   \
  metrics:3.1.2.10               \
  momentjs:1.1.1                 \
  monitoring:1.70.0              \
  msbuild:1.28                   \
  nant:1.4.3                     \
  node-iterator-api:1.5.0        \
  pam-auth:1.3                   \
  parameterized-trigger:2.35.2   \
  pipeline-build-step:2.6        \
  pipeline-github-lib:1.0        \
  pipeline-input-step:2.8        \
  pipeline-milestone-step:1.3.1  \
  pipeline-model-api:1.2.5       \
  pipeline-model-definition:1.2.5 \
  pipeline-model-extensions:1.2.5 \
  pipeline-rest-api:2.9          \
  pipeline-stage-step:2.3        \
  pipeline-stage-view:2.9        \
  plain-credentials:1.4          \
  postbuildscript:2.2.1          \
  publish-over-cifs:0.5          \
  publish-over-ftp:1.12          \
  publish-over-ssh:1.17          \
  puppet:1.1                     \
  rebuild:1.27                   \
  role-strategy:2.6.1            \
  run-condition:1.0              \
  s3:0.10.12                     \
  saferestart:0.3                \
  saml:1.0.4                     \
  saltstack:3.1.2                \
  scm-api:2.2.6                  \
  ssh-agent:1.15                 \
  ssh-slaves:1.25                \
  slave-setup:1.10               \
  subversion:2.10.2              \
  timestamper:1.8.9              \
  translation:1.15               \
  uno-choice:2.1                 \
  variant:1.1                    \
  view-job-filters:1.27          \
  windows-slaves:1.3.1           \
  workflow-aggregator:2.5        \
  workflow-api:2.24              \
  workflow-basic-steps:2.6       \
  workflow-cps:2.42              \
  workflow-cps-global-lib:2.9    \
  workflow-durable-task-step:2.17 \
  workflow-job:2.16              \
  workflow-multibranch:2.16      \
  workflow-scm-step:2.6          \
  workflow-step-api:2.14         \
  workflow-support:2.16          \
  ws-cleanup:0.34                \
  xcode-plugin:2.0.0

# disable first-run wizard（向导）
RUN echo 2.0 > /usr/share/jenkins/ref/jenkins.install.UpgradeWizard.state

# remove executors in master
COPY master-executors.groovy /usr/share/jenkins/ref/init.groovy.d/

ENV JAVA_OPTS="-XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap -XX:MaxRAMFraction=1 -XshowSettings:vm \
        -Djenkins.install.runSetupWizard=false -Dhudson.udp=-1 -Djava.awt.headless=true -Dhudson.DNSMultiCast.disabled=true"
ENV JENKINS_OPTS="--webroot=${JENKINS_FOLDER}/war --httpListenAddress=0.0.0.0"
