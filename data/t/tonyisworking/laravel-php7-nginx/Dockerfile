# Dependency for php7: libwebp library doesn't work correctly with alpine:v3.3
so we are using alpine:edge  
FROM alpine:edge  
MAINTAINER tonyisworking  
  
# Install dependencies and small amount of devtools  
RUN apk add --update curl bash git openssh-client nano nginx ca-certificates \  
# Libs for php  
libssh2 libpng freetype libjpeg-turbo libgcc libxml2 libstdc++ icu-libs
libltdl libmcrypt \  
ncurses \  
# For mails  
msmtp \  
# For mysql import/export  
mysql-client \  
# Install gettext  
gettext \  
# Set timezone according your location  
tzdata && \  
# Upgrade musl  
apk add -u musl && \  
##  
# Install php7  
# Let's go with the community package  
##  
apk upgrade -U \  
&& apk add --update-cache
--repository=http://dl-4.alpinelinux.org/alpine/edge/community/ \  
php7-pdo_mysql php7-mysqli php7-mysqlnd php7-mcrypt \  
php7 php7-session php7-fpm php7-json php7-zlib php7-xml php7-pdo \  
php7-gd php7-curl php7-opcache php7-ctype php7-mbstring php7-soap
php7-pdo_sqlite php7-sqlite3 \  
php7-intl php7-bcmath php7-dom php7-xmlreader php7-openssl php7-phar && \  
  
# Small fixes to php & nginx  
ln -s /etc/php7 /etc/php && \  
ln -s /usr/bin/php7 /usr/bin/php && \  
ln -s /usr/sbin/php-fpm7 /usr/bin/php-fpm && \  
ln -s /usr/lib/php7 /usr/lib/php && \  
rm -rf /var/log/php7 && \  
mkdir -p /var/log/php/ && \  
  
# No need for the default configs  
rm -f /etc/php/php-fpm.d/www.conf && \  
  
# Remove nginx user because we will create a user with correct permissions
dynamically  
deluser nginx && \  
mkdir -p /var/log/nginx && \  
mkdir -p /tmp/nginx/body && \  
  
# Remove default localhost folder  
rm -rf /var/www/localhost && \  
  
# Create uploads folder and project folder  
mkdir -p /var/www/project && \  
mkdir -p /var/www/laravel && \  
mkdir -p /var/www/nginx && \  
  
# Remove default crontab  
rm /var/spool/cron/crontabs/root && \  
  
##  
# Add S6-overlay to use S6 process manager  
# source: https://github.com/just-containers/s6-overlay/#the-docker-way  
##  
curl -L https://github.com/just-
containers/s6-overlay/releases/download/v1.17.2.0/s6-overlay-amd64.tar.gz \  
| tar -xvzC / && \  
##  
# Install cronlock for running cron correctly with mulitple container setups  
# https://github.com/kvz/cronlock  
##  
curl -L https://raw.githubusercontent.com/kvz/cronlock/master/cronlock -o
/usr/local/bin/cronlock && \  
chmod +rx /usr/local/bin/cronlock && \  
  
##  
# Install Composer  
##  
curl -L -sS https://getcomposer.org/installer | \  
php -- --install-dir=/usr/local/bin --filename=composer && \  
chmod +rx /usr/local/bin/composer && \  
# composer parallel install plugin  
composer global require hirak/prestissimo && \  
# Remove cache and tmp files  
rm -rf /var/cache/apk/* && \  
rm -rf /tmp/*  
  
##  
# Add Project files like nginx and php-fpm processes and configs  
# Also custom scripts and bashrc  
##  
COPY rootfs/ /  
  
# Update path with composer files + wpcs  
ENV TERM="xterm" \  
DB_HOST="" \  
DB_NAME="" \  
DB_USER=""\  
DB_PASSWORD=""\  
# Set defaults which can be overriden  
DB_PORT="3306" \  
# Use default web port in nginx but allow it to be overridden  
# This also works correctly with flynn:  
# https://github.com/flynn/flynn/issues/3213#issuecomment-237307457  
PORT="80" \  
# Cronlock is used to stop simultaneous cronjobs in clusterised environments  
CRONLOCK_HOST="" \  
# This is for your project root  
PROJECT_ROOT="/var/www/laravel" \  
# This is used by nginx and php-fpm  
WEB_ROOT="/var/www/laravel/public" \  
# Nginx include files  
NGINX_INCLUDE_DIR="/var/www/nginx" \  
# Allow bigger file uploads  
NGINX_MAX_BODY_SIZE="64M" \  
# Have sane fastcgi timeout by default  
NGINX_FASTCGI_TIMEOUT="30" \  
# Default php memory limit  
PHP_MEMORY_LIMIT="256M" \  
# Use host machine as default SMTP_HOST  
SMTP_HOST="172.17.0.1" \  
# This folder is used to mount files into host machine  
# This can be overidden by you, it's just default for us  
TZ="UTC"  
# Set default path to project folder for easier running commands in project  
WORKDIR ${PROJECT_ROOT}  
  
EXPOSE ${PORT}  
  
ENTRYPOINT ["/init"]  

