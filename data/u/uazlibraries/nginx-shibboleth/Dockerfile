FROM nginx:stable  
  
# Make apt-get commands temporarily non-interactive  
# Solution from https://github.com/phusion/baseimage-docker/issues/58  
# Update apt cache to use fastest local mirror  
RUN \  
export DEBIAN_FRONTEND=noninteractive && \  
export DEBCONF_NONINTERACTIVE_SEEN=true && \  
apt-get update && \  
apt-get install -y nginx-dbg man netcat wget curl openssl nano less sudo
procps git  
  
# Build nginx shibboleth modules, taken from  
# https://github.com/ConsortiumGARR/idem-tutorials/blob/master/idem-
community/HOWTO-Shibboleth/Service-
Provider/Debian/HOW%20TO%20SETUP%20A%20SHIBBOLETH%20SP%20WITH%20NGINX.md  
RUN mkdir -p /root/nginx-modules/deb  
WORKDIR /root/nginx-modules  
ADD http://hg.nginx.org/pkg-oss/archive/tip.tar.gz tip.tar.gz  
RUN \  
tar -xvf tip.tar.gz && mv pkg-oss-* pkg-oss/ && \  
git clone https://github.com/openresty/headers-more-nginx-module.git && \  
git clone https://github.com/nginx-shib/nginx-http-shibboleth.git && \  
pkg-oss/build_module.sh -y -o /root/nginx-modules/deb/ -n shibboleth -v `nginx
-v 2>&1 | sed -n -e 's|^.*/||p' | tr -d '\n'` nginx-http-shibboleth/ && \  
pkg-oss/build_module.sh -y -o /root/nginx-modules/deb/ -n headers-more -v
`nginx -v 2>&1 | sed -n -e 's|^.*/||p' | tr -d '\n'` headers-more-nginx-module
&& \  
cp -vf nginx-http-shibboleth/includes/* /etc/nginx && \  
rm -f deb/*-dbg_*.deb && dpkg -i deb/*.deb && \  
rm -fr /root/debuild && rm -f tip.tar.gz && \  
apt-get purge -y build-essential gcc make unzip wget mercurial git
libpcre3-dev zlib1g-dev libssl-dev devscripts debhelper dpkg-dev quilt lsb-
release && \  
apt-get autoremove -y && \  
apt-get clean -y  
  
WORKDIR /  
  
# Create _shibd uid.gid so we can control the numeric value of uid:gid  
RUN \  
groupadd _shibd --gid 799 && \  
useradd _shibd --uid 799 --gid 799 \  
\--home /var/log/shibboleth --shell /bin/false && \  
adduser nginx _shibd && \  
adduser nginx www-data  
  
# Enable directory colors:  
RUN \  
sed -i "s/^# export LS/export LS/g" /root/.bashrc && \  
sed -i "s/^# eval/eval/g" /root/.bashrc && \  
sed -i "s/^# alias l/alias l/g" /root/.bashrc  
  
# Fix bash error with enabling colors:  
ENV SHELL=/bin/bash  
  
COPY nginx.conf /etc/nginx/nginx.conf  
COPY port443.conf /etc/nginx/conf.d/port443.conf  
RUN rm /etc/nginx/conf.d/default.conf  

