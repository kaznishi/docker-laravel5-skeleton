FROM node:8 AS node-builder

COPY . /home/node/html
WORKDIR /home/node/html

RUN yarn install
RUN npm run production

#######################################

FROM nginx:1-alpine

ENV ENTRYKIT_VERSION 0.4.0

WORKDIR /

# Install Entrykit
RUN apk update && apk add openssl \
  && rm -rf /var/cache/apk/* \
  && wget https://github.com/progrium/entrykit/releases/download/v${ENTRYKIT_VERSION}/entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
  && tar -xvzf entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
  && rm entrykit_${ENTRYKIT_VERSION}_Linux_x86_64.tgz \
  && mv entrykit /bin/entrykit \
  && chmod +x /bin/entrykit \
  && entrykit --symlink

RUN rm /etc/nginx/conf.d/*
COPY docker/nginx/etc/nginx/nginx.conf.tmpl /etc/nginx/
COPY docker/nginx/etc/nginx/conf.d/ /etc/nginx/conf.d/

COPY --from=node-builder /home/node/html/public /var/www/html/public

ENTRYPOINT [ \
  "render", \
      "/etc/nginx/nginx.conf", \
      "--", \
  "render", \
      "/etc/nginx/conf.d/public.conf", \
      "--" \
]

CMD nginx -g "daemon off;"
