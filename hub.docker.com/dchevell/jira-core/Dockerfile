FROM adoptopenjdk/openjdk8:slim

ENV RUN_USER            					daemon
ENV RUN_GROUP           					daemon

# https://confluence.atlassian.com/display/JSERVERM/Important+directories+and+files
ENV JIRA_HOME          						/var/atlassian/application-data/jira
ENV JIRA_INSTALL_DIR   						/opt/atlassian/jira

VOLUME ["${JIRA_HOME}"]
WORKDIR $JIRA_HOME

# Expose HTTP port
EXPOSE 8080

CMD ["/entrypoint.sh", "-fg"]
ENTRYPOINT ["/tini", "--"]

RUN apt-get update \
	&& apt-get install -y --no-install-recommends fontconfig \
	&& apt-get clean autoclean && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

ARG TINI_VERSION=v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

COPY entrypoint.sh              			/entrypoint.sh

ARG JIRA_VERSION=8.0.2
ARG DOWNLOAD_URL=https://product-downloads.atlassian.com/software/jira/downloads/atlassian-jira-core-${JIRA_VERSION}.tar.gz

RUN mkdir -p                             ${JIRA_INSTALL_DIR} \
    && curl -L --silent                  ${DOWNLOAD_URL} | tar -xz --strip-components=1 -C "${JIRA_INSTALL_DIR}" \
    && chown -R ${RUN_USER}:${RUN_GROUP} ${JIRA_INSTALL_DIR}/ \
    && sed -i -e 's/^JVM_SUPPORT_RECOMMENDED_ARGS=""$/: \${JVM_SUPPORT_RECOMMENDED_ARGS:=""}/g' ${JIRA_INSTALL_DIR}/bin/setenv.sh \
    && sed -i -e 's/^JVM_\(.*\)_MEMORY="\(.*\)"$/: \${JVM_\1_MEMORY:=\2}/g' ${JIRA_INSTALL_DIR}/bin/setenv.sh \
    && sed -i -e 's/port="8080"/port="8080" secure="${catalinaConnectorSecure}" scheme="${catalinaConnectorScheme}" proxyName="${catalinaConnectorProxyName}" proxyPort="${catalinaConnectorProxyPort}"/' ${JIRA_INSTALL_DIR}/conf/server.xml \
    && sed -i -e 's/Context path=""/Context path="${catalinaContextPath}"/' ${JIRA_INSTALL_DIR}/conf/server.xml
