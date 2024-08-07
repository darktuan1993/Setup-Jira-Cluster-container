FROM openjdk:17-bullseye
ARG JIRA_VERSION=9.17.1
ARG JIRA_PRODUCT=jira-software

ENV JIRA_USER=jira \
JIRA_GROUP=jira \
JIRA_HOME=/var/jira \
JIRA_SETUP=/opt/jira \ 
JVM_CODE_CACHE_ARGS='-XX:InitialCodeCacheSize=2g -XX:ReservedCodeCacheSize=4g' \
AGENT_PATH=/var/agent \
AGENT_FILENAME=atlassian-agent.jar
ENV JAVA_OPTS="-javaagent:${AGENT_PATH}/${AGENT_FILENAME} ${JAVA_OPTS}"

COPY atlassian-agent.jar ${AGENT_PATH}/${AGENT_FILENAME}

RUN mkdir -p ${JIRA_SETUP} ${JIRA_HOME} ${AGENT_PATH} \
    && curl -o /tmp/atlassian.tar.gz https://product-downloads.atlassian.com/software/jira/downloads/atlassian-${JIRA_PRODUCT}-${JIRA_VERSION}.tar.gz -L \
    && tar xzf /tmp/atlassian.tar.gz -C ${JIRA_SETUP}/ --strip-components 1 \
    && rm -f /tmp/atlassian.tar.gz \
    && echo "jira.home = ${JIRA_HOME}" > ${JIRA_SETUP}/atlassian-jira/WEB-INF/classes/jira-application.properties

RUN export CONTAINER_USER=$JIRA_USER \
    && export CONTAINER_GROUP=$JIRA_GROUP \
    && groupadd -r $JIRA_GROUP && useradd -r -g $JIRA_GROUP $JIRA_USER \
    && chown -R $JIRA_USER:$JIRA_GROUP ${JIRA_SETUP} ${JIRA_HOME}/ ${AGENT_PATH}

VOLUME $JIRA_HOME
USER $JIRA_USER
WORKDIR $JIRA_SETUP
EXPOSE 8080

ENTRYPOINT ["/opt/jira/bin/start-jira.sh", "-fg"]



