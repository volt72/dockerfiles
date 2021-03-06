#
------------------------------------------------------------------------------  
# Based on a work at https://github.com/docker/docker.  
#
------------------------------------------------------------------------------  
# Pull base image.  
FROM kdelfour/supervisor-docker  
MAINTAINER JureUrsic <ursic.jure@gmail.com>  
  
  
  
#
------------------------------------------------------------------------------  
# Install base  
RUN apt-get update  
RUN apt-get install -y nano build-essential g++ curl libssl-dev apache2-utils
git libxml2-dev sshfs  
  
#
------------------------------------------------------------------------------  
# Install Node.js  
RUN curl -sL https://deb.nodesource.com/setup | bash -  
RUN apt-get install -y nodejs  
  
#
------------------------------------------------------------------------------  
# Install Cloud9  
RUN git clone https://github.com/c9/core.git /cloud9  
WORKDIR /cloud9  
RUN scripts/install-sdk.sh  
  
# Tweak standlone.js conf  
RUN sed -i -e 's_127.0.0.1_0.0.0.0_g' /cloud9/configs/standalone.js  
  
# Add supervisord conf  
ADD conf/cloud9.conf /etc/supervisor/conf.d/  
  
#
------------------------------------------------------------------------------  
# Add volumes  
RUN mkdir /home/remote  
RUN mkdir /home/local  
VOLUME /home/local  
  
#
------------------------------------------------------------------------------  
# Clean up APT when done.  
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*  
  
#
------------------------------------------------------------------------------  
# Expose ports.  
EXPOSE 8181  
EXPOSE 3000  
#
------------------------------------------------------------------------------  
# Start supervisor, define default command.  
CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf"]  

