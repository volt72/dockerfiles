FROM ubuntu:15.10  
ENV DEBIAN_FRONTEND noninteractive  
ENV SCALA_VERSION="2.11.7"  
ENV JAVA_VERSION="8"  
ENV CONFLUENT_MAJOR_VERSION="2.0"  
#Adding the confluent deb repo.  
RUN apt-get update && \  
apt-get install -y \  
wget \  
software-properties-common && \  
wget -qO - http://packages.confluent.io/deb/2.0/archive.key | apt-key add - &&
\  
add-apt-repository "deb http://packages.confluent.io/deb/2.0 stable main"  
  
RUN apt-get update && \  
apt-get install -y \  
build-essential \  
openjdk-${JAVA_VERSION}-jre \  
scala \  
confluent-platform-${SCALA_VERSION} && \  
rm -rf /var/lib/apt/lists/*

