FROM totalstudio/alpine_php  
MAINTAINER Rahmat Kurniawan <wawan@totalstudio.co>  
  
  
  
# Create a user called "deployer" without a password and belonging  
# to the same group as php-fpm and nginx belong to  
RUN adduser -D deployer -s /bin/bash -G www-data  
  
# Set working directory to wp-content, which is a mounted volume  
VOLUME /var/www/content  
WORKDIR /var/www/content  
  
# Environment variables that make the reuse easier  
ENV WP_ROOT /usr/src/wordpress  
ENV WP_VERSION 4.5.2  
ENV WP_SHA1 bab94003a5d2285f6ae76407e7b1bbb75382c36e  
ENV WP_DOWNLOAD_URL https://wordpress.org/wordpress-$WP_VERSION.tar.gz  
  
# Download WP and extract it to /usr/src/wordpress  
RUN curl -o wordpress.tar.gz -SL $WP_DOWNLOAD_URL \  
&& echo "$WP_SHA1 *wordpress.tar.gz" | sha1sum -c - \  
&& tar -xzf wordpress.tar.gz -C /usr/src/ \  
&& rm wordpress.tar.gz  
  
# Copy our cron configuration and set proper permissions  
COPY cron.conf /etc/crontabs/deployer  
RUN chmod 600 /etc/crontabs/deployer  
  
# Copy over our custom Nginx configuration and log to stderr/stdout  
COPY nginx.conf /etc/nginx/nginx.conf  
COPY virtual-host.conf /etc/nginx/conf.d/  
RUN ln -sf /dev/stdout /var/log/nginx/access.log \  
&& ln -sf /dev/stderr /var/log/nginx/error.log \  
&& chown -R www-data:www-data /var/lib/nginx  
  
# Copy our configuration and set proper permissions  
COPY wp-config.php $WP_ROOT  
RUN chown -R deployer:www-data $WP_ROOT \  
&& chmod 640 $WP_ROOT/wp-config.php  
  
RUN mkdir -p /var/www/content/uploads \  
&& chown -R www-data:www-data /var/www/content/uploads  
  
# Copy supervisor configuration for both processes  
RUN mkdir -p /var/log/supervisor  
COPY supervisord.conf /etc/supervisord.conf  
  
# Copy and prepare the entrypoint  
COPY docker-entrypoint.sh /  
RUN chmod +x /docker-entrypoint.sh  
ENTRYPOINT [ "/docker-entrypoint.sh" ]  
  
CMD [ "/usr/bin/supervisord", "-c", "/etc/supervisord.conf" ]  

