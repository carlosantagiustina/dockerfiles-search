FROM node:11.9.0-alpine as base

WORKDIR /tmp
COPY package.json /tmp/package.json
RUN apk add git && yarn && mkdir -p /app/node_modules && cp -a ./node_modules /app/

FROM node:11.9.0-alpine

#Add Tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-static /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]

WORKDIR /app
RUN apk add bash
COPY --from=base /app/node_modules node_modules
COPY . .
ENV API_PORT=80 \
    NODE_ENV=production \
    PORT=465 \
    SERVER=smtp.example.com \
    FROM=me@example.com \
    SECURE=true

EXPOSE 80
CMD ["/bin/bash", "./docker/startup.sh"]

