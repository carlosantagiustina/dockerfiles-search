FROM node:12-slim
RUN  apt-get update &&  apt-get install -y build-essential python
WORKDIR /build
COPY package.json /build
RUN npm install
COPY . /build
RUN npm run build
#VOLUME /build

USER node
# need to support old sites like canada ammo (till chrome drops support)
CMD ["node","--tls-min-v1.0","dist/worker/index.js"]