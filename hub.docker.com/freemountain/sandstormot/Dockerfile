FROM node:10-alpine AS builder

WORKDIR /build
COPY package*.json .babelrc /build/
COPY app /build/app/
RUN npm install && npm run _build

FROM node:10-alpine

ENV HOME=/home/app/app
ENV DATA_DIRECTORY=/var/lib/app/data
WORKDIR ${HOME}

COPY package*.json templates.json ${HOME}/
COPY --from=builder /build/dist ${HOME}/dist
RUN npm install --production && mkdir -p ${DATA_DIRECTORY}

CMD npm run _production:run
