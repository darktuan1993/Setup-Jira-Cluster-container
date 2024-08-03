FROM openjdk:11-bullseye

ARG JIRA_VERSION=9.17.1
# Production: jira-software jira-core
ARG JIRA_PRODUCT=jira-software
ARG AGENT_VERSION=1.3.3
ARG MYSQL_DRIVER_VERSION=8.0.22

ENV JIRA_USER=jira \
    JIRA_GROUP=jira \
    JIRA_HOME=/var/atlassian/application-data/jira \
    JIRA_INSTALL=/opt/atlassian/jira \
    JVM_MINIMUM_MEMORY=2g \
    JVM_MAXIMUM_MEMORY=16g \
    JVM_CODE_CACHE_ARGS='-XX:InitialCodeCacheSize=2g -XX:ReservedCodeCacheSize=4g' \
    AGENT_PATH=/var/agent

RUN mkdir -p ${JIRA_INSTALL} ${JIRA_HOME} ${AGENT_PATH} \
    && curl -o /tmp/atlassian.tar.gz https://product-downloads.atlassian.com/software/jira/downloads/atlassian-${JIRA_PRODUCT}-${JIRA_VERSION}.tar.gz -L \
    && tar xzf /tmp/atlassian.tar.gz -C ${JIRA_INSTALL}/ --strip-components 1 \
    && rm -f /tmp/atlassian.tar.gz \
    && echo "jira.home = ${JIRA_HOME}" > ${JIRA_INSTALL}/atlassian-jira/WEB-INF/classes/jira-application.properties

RUN groupadd -r $JIRA_GROUP && useradd -r -g $JIRA_GROUP $JIRA_USER \
    && chown -R $JIRA_USER:$JIRA_GROUP ${JIRA_INSTALL} ${JIRA_HOME} ${AGENT_PATH}

VOLUME $JIRA_HOME
USER $JIRA_USER
WORKDIR $JIRA_INSTALL
EXPOSE 8080

ENTRYPOINT ["/opt/atlassian/jira/bin/start-jira.sh", "-fg"]