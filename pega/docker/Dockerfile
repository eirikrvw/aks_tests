
FROM websphere-liberty:kernel

ARG REPOSITORIES_PROPERTIES=""

RUN if [ ! -z $REPOSITORIES_PROPERTIES ]; then mkdir /opt/wlp/etc/ \
  && echo $REPOSITORIES_PROPERTIES > /opt/wlp/etc/repositories.properties; fi \
  && installUtility install --acceptLicense \
    appSecurity-3.0 ldapRegistry-3.0 samlWeb-2.0 \
    localConnector-1.0 monitor-1.0 requestTiming-1.0 restConnector-2.0 sessionCache-1.0 \
    sessionDatabase-1.0 ssl-1.0 transportSecurity-1.0 webCache-1.0 webProfile-8.0 \
    appSecurityClient-1.0 javaee-8.0 javaeeClient-8.0 openidConnectClient-1.0 monitor-1.0 microProfile-2.0 \
    jdbc-4.2 jndi-1.0 servlet-4.0 ejb-3.2 jms-2.0 mdb-3.2 \
  && if [ ! -z $REPOSITORIES_PROPERTIES ]; then rm /opt/wlp/etc/repositories.properties; fi \
  && rm -rf /output/workarea /output/logs

COPY server.xml /config/

RUN server start && server stop && rm -rf /output/resources/security/ /output/messaging /logs/*
EXPOSE
CMD ["server start"]

