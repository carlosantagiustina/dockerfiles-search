# Builder
FROM node:10 as builder
WORKDIR /app
COPY ./ /app
RUN npm --unsafe-perm --production install \
  && npm --production run-script build

# Service
FROM nginx:1.13
COPY ./etc/nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/html /usr/share/nginx/html
