FROM ubuntu:14.04  
MAINTAINER Steven Heyninck, "steven.heyninck@foresee.be"  
RUN echo deb http://archive.ubuntu.com/ubuntu trusty universe >>
/etc/apt/sources.list  
RUN apt-get update && apt-get clean  
RUN apt-get install -q -y openjdk-7-jre-headless && apt-get clean  
ADD http://mirrors.jenkins-ci.org/war/latest/jenkins.war /opt/jenkins.war  
RUN ln -sf /jenkins /root/.jenkins  
  
ENTRYPOINT ["java", "-jar", "/opt/jenkins.war"]  
EXPOSE 8080  
VOLUME ["/jenkins"]  
CMD [""]  

