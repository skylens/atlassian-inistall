FROM openjdk:8-alpine
ENV CONF_HOME /opt/confluence-home
ENV CONF_VERSION 7.4.8

RUN set -ex && apk update
RUN set -ex && apk add --no-cache --virtual .build-deps bash fontconfig
COPY "atlassian-confluence-${CONF_VERSION}.tar.gz" /opt
COPY "atlassian-agent.jar" /opt
RUN mkdir -p ${CONF_HOME} \
    && tar -xvf /opt/atlassian-confluence-${CONF_VERSION}.tar.gz -C /opt \
    && mv /opt/atlassian-confluence-${CONF_VERSION} /opt/atlassian-confluence \
    && echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian-confluence/bin/setenv.sh \
    && source /etc/profile
COPY "mysql-connector-java-5.1.49.jar" /opt/atlassian-confluence/confluence/WEB-INF/lib
RUN sed -i 's/^# confluence.home=\/var\/data\/confluence\//confluence.home=\/opt\/confluence-home\//g' /opt/atlassian-confluence/confluence/WEB-INF/classes/confluence-init.properties
RUN chmod +x /opt/atlassian-confluence/bin/start-confluence.sh
EXPOSE 8000 8090

CMD ["/opt/atlassian-confluence/bin/start-confluence.sh", "-fg"]