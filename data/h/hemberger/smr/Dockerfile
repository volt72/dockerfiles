FROM node:alpine as builder  
  
WORKDIR /smr/  
  
# See https://github.com/hollandben/grunt-cache-bust/issues/236  
RUN npm i --save grunt grunt-contrib-uglify grunt-contrib-cssmin grunt-cache-
bust@1.4.1  
  
# Copy the SMR source code  
COPY . .  
  
# Perform CSS/JS minification and cache busting  
RUN npx grunt  
  
# Remove local grunt install so it is not copied to the next build stage  
RUN rm -rf node_modules  
  
#---------------------------  
FROM php:7.2-apache  
RUN apt-get update \  
&& apt-get install -y zip unzip \  
&& rm -rf /var/lib/apt/lists/* \  
&& docker-php-ext-install mysqli  
  
# Use the production php.ini unless PHP_DEBUG=1 (defaults to 0)  
ARG PHP_DEBUG=0  
RUN [ "$PHP_DEBUG" = "1" ] && echo "Using development php.ini" || \  
{ echo "Using production php.ini" \  
&& mkdir /usr/src/php \  
&& tar --file /usr/src/php.tar.xz --extract --strip-components=1 --directory
/usr/src/php \  
&& cp /usr/src/php/php.ini-production /usr/local/etc/php/php.ini; \  
}  
  
# Disable apache .htaccess files (suggested optimization)  
RUN sed -i 's/AllowOverride All/AllowOverride None/g' /etc/apache2/conf-
enabled/docker-php.conf  
  
WORKDIR /smr/  
  
RUN curl -sS https://getcomposer.org/installer | php -- --install-
dir=/usr/local/bin --filename=composer  
  
COPY composer.json .  
RUN composer install --no-interaction  
  
COPY \--from=builder /smr .  
RUN rm -rf /var/www/html/ && ln -s "$(pwd)/htdocs" /var/www/html  
  
# Make the upload directory writable by the apache user  
RUN chown www-data ./htdocs/upload  
  
# Add the option to set the git commit as metadata  
# (This arg is set automatically in the Docker Cloud builds)  
ARG SOURCE_COMMIT=unknown  
LABEL git-commit=$SOURCE_COMMIT  

