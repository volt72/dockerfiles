# Spotify Cassandra 2.0 Base Image  
#  
# VERSION 0.1  
#  
# Installs Cassandra 2.0 package. Does only basic configuration.  
# Tokens and seed nodes should be configured by child images.  
FROM java:8  
ENV DEBIAN_FRONTEND noninteractive  
  
RUN apt-get update  
  
# Add DataStax sources  
ADD datastax_key /tmp/datastax_key  
RUN apt-key add /tmp/datastax_key  
RUN echo "deb http://www.apache.org/dist/cassandra/debian 30x main" >
/etc/apt/sources.list.d/datastax.list  
  
# Workaround for https://github.com/docker/docker/issues/6345  
RUN ln -s -f /bin/true /usr/bin/chfn  
  
# Install Cassandra 3.0.0~beta2  
RUN apt-get update && \  
apt-get -y --force-yes install python-pip && \  
apt-get update && \  
apt-get install -y --force-yes cassandra=3.0.0~beta2 && \  
rm -rf /var/lib/apt/lists/*  
  
ENV CASSANDRA_CONFIG /etc/cassandra  
  
# Run base config script  
ADD scripts/config-cassandra-base.sh /usr/local/bin/config-cassandra-base  
RUN /usr/local/bin/config-cassandra-base  
  
# Necessary since cassandra is trying to override the system limitations  
# See https://groups.google.com/forum/#!msg/docker-
dev/8TM_jLGpRKU/dewIQhcs7oAJ  
RUN rm -f /etc/security/limits.d/cassandra.conf  
  
EXPOSE 7199 7000 7001 9160 9042 22 8012 61621  
RUN apt-get update  
  
CMD [""]  

