FROM node:16-alpine as builder
LABEL maintainer="Julio Cesar <julio@blackdevs.com.br>"

WORKDIR /app

RUN apk add --update --no-cache gettext
COPY package.json yarn.lock nginx.conf ./

RUN yarn install --production=true
COPY . .
RUN yarn run build --mode production

FROM nginx:1.22 as runtime
EXPOSE 8080

COPY docker-entrypoint.sh /opt/docker-entrypoint.sh
COPY --from=builder --chown=nginx:nginx /app/nginx.conf /etc/nginx/nginx.conf
COPY --from=builder --chown=nginx:nginx /app/dist /usr/share/nginx/html

ENTRYPOINT ["/opt/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
