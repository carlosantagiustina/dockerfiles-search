FROM alpine AS qemu_arm64

# Download QEMU, see https://github.com/docker/hub-feedback/issues/1261
ENV QEMU_URL https://github.com/balena-io/qemu/releases/download/v3.0.0%2Bresin/qemu-3.0.0+resin-aarch64.tar.gz
RUN apk add curl && curl -L ${QEMU_URL} | tar zxvf - -C . --strip-components 1

FROM arm64v8/node:alpine
# Add QEMU
COPY --from=qemu_arm64 qemu-aarch64-static /usr/bin

RUN apk update && apk upgrade && \
    apk add --update bash git npm

ENV NODE_ENV production
WORKDIR /opt/magic_mirror

RUN git clone https://github.com/MichMich/MagicMirror.git .
RUN cp -R modules /opt/default_modules
RUN cp -R config /opt/default_config
RUN npm install --unsafe-perm --silent

COPY server-only-default-config.js /opt/magic_mirror/config/config.js 
COPY server-only-german-sample-config.js /opt/magic_mirror/config/server-only-german-sample-config.js 
COPY server-only-default-config.js docker-entrypoint.sh ./
RUN chmod +x ./docker-entrypoint.sh

VOLUME [ "/opt/magic_mirror/config", "/opt/magic_mirror/modules" ]

EXPOSE 8080
ENTRYPOINT ["./docker-entrypoint.sh"]
CMD ["node", "serveronly"]
