FROM tomcat:8.5-jre8  
LABEL maintainer "Johannes Wettinger <jowettinger@gmail.com>, Michael Wurster
<miwurster@gmail.com>"  
  
ARG DOCKERIZE_VERSION=v0.3.0  
ENV TOMCAT_USERNAME admin  
ENV TOMCAT_PASSWORD admin  
  
RUN rm /dev/random && ln -s /dev/urandom /dev/random \  
&& wget
https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-
linux-amd64-$DOCKERIZE_VERSION.tar.gz \  
&& tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
\  
&& rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \  
&& sed -ie
"s/securerandom.source=file:\/dev\/random/securerandom.source=file:\/dev\/.\/urandom/g"
/usr/lib/jvm/java-8-openjdk-amd64/jre/lib/security/java.security  
  
ADD tomcat-users.xml.tpl ${CATALINA_HOME}/conf/tomcat-users.xml.tpl  
ADD manager.xml ${CATALINA_HOME}/conf/Catalina/localhost/manager.xml  
ADD host-manager.xml ${CATALINA_HOME}/conf/Catalina/localhost/host-manager.xml  
ADD server.xml ${CATALINA_HOME}/conf/server.xml  
  
EXPOSE 8080  
CMD dockerize -template ${CATALINA_HOME}/conf/tomcat-
users.xml.tpl:${CATALINA_HOME}/conf/tomcat-users.xml \  
${CATALINA_HOME}/bin/catalina.sh run  

