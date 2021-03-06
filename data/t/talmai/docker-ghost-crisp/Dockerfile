FROM node:6-alpine  
  
# grab su-exec for easy step-down from root  
RUN apk add --no-cache 'su-exec>=0.2'  
  
RUN apk add --no-cache \  
# add "bash" for "[["  
bash git gawk sed grep  
  
ENV NODE_ENV production  
  
ENV GHOST_CLI_VERSION 1.7.1  
RUN npm install -g "ghost-cli@$GHOST_CLI_VERSION"  
  
ENV GHOST_INSTALL /var/lib/ghost  
ENV GHOST_CONTENT /var/lib/ghost/content  
  
ENV GHOST_VERSION 1.22.4  
RUN set -ex; \  
mkdir -p "$GHOST_INSTALL"; \  
chown node:node "$GHOST_INSTALL"; \  
\  
su-exec node ghost install "$GHOST_VERSION" \--db sqlite3 --no-prompt --no-
stack --no-setup --dir "$GHOST_INSTALL"; \  
\  
# Tell Ghost to listen on all ips and not prompt for additional configuration  
cd "$GHOST_INSTALL"; \  
su-exec node ghost config --ip 0.0.0.0 \--port 2368 \--no-prompt --db sqlite3
--url http://localhost:2368 \--dbpath "$GHOST_CONTENT/data/ghost.db"; \  
su-exec node ghost config paths.contentPath "$GHOST_CONTENT"; \  
\  
# make a config.json symlink for NODE_ENV=development (and sanity check that
it's correct)  
su-exec node ln -s config.production.json
"$GHOST_INSTALL/config.development.json"; \  
readlink -f "$GHOST_INSTALL/config.development.json"; \  
\  
# need to save initial content for pre-seeding empty volumes  
mv "$GHOST_CONTENT" "$GHOST_INSTALL/content.orig"; \  
mkdir -p "$GHOST_CONTENT"; \  
chown node:node "$GHOST_CONTENT"; \  
\  
# sanity check to ensure knex-migrator was installed  
"$GHOST_INSTALL/current/node_modules/knex-migrator/bin/knex-migrator"
\--version  
  
# add knex-migrator bins into PATH  
# we want these from the context of Ghost's "node_modules" directory (instead
of doing "npm install -g knex-migrator") so they can share the DB driver
modules  
ENV PATH $PATH:$GHOST_INSTALL/current/node_modules/knex-migrator/bin  
  
  
# install Crisp and patch configuration  
RUN mkdir -p "$GHOST_CONTENT"/themes/crisp \  
&& git clone https://github.com/kathyqian/crisp "$GHOST_CONTENT"/themes/crisp
\  
# Clean up  
&& rm -rf "$GHOST_CONTENT"/themes/crisp/.git \  
# disable comments  
&& echo -n "" > "$GHOST_CONTENT"/themes/crisp/partials/comments.hbs \  
&& sed -i "41s/.*/<section id='footer-message'>\&copy; {{date format='YYYY'}}
{{@blog.title}}. All rights reserved.<\/section>/"
"$GHOST_CONTENT"/themes/crisp/default.hbs \  
&& sed -i -e '1,111d' "$GHOST_CONTENT"/themes/crisp/partials/share.hbs  
  
COPY docker-entrypoint.sh /usr/local/bin  
RUN chmod 777 /usr/local/bin/docker-entrypoint.sh  
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]  
  
WORKDIR $GHOST_INSTALL  
VOLUME $GHOST_CONTENT  
  
EXPOSE 2368  
CMD ["node", "current/index.js"]  

