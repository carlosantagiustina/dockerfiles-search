## Dockerfile for building production image
FROM bitnami/express:4.15.3-r3
LABEL maintainer="Vandhuy Martins <vandhuy@gmail.com>"

ENV DISABLE_WELCOME_MESSAGE=1

ENV NODE_ENV=production \
    PORT=3000

# Skip fetching dependencies and database migrations for production image
ENV SKIP_DB_WAIT=0 \
    SKIP_DB_MIGRATION=1 \
    SKIP_NPM_INSTALL=1 \
    SKIP_BOWER_INSTALL=1

COPY . /app
RUN sudo chown -R bitnami: /app

RUN npm install

EXPOSE 3000
CMD ["npm", "start"]
