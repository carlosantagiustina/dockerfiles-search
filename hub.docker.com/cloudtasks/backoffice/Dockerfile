FROM mhart/alpine-node:latest

RUN apk add --no-cache build-base git perl python

WORKDIR /project

ADD . .

RUN npm install
RUN npm run build

FROM jonnybgod/alpine-nginx:master

WORKDIR /src

COPY --from=0 /project/dist/ .
ADD ./nginx.conf /etc/nginx/nginx.conf

EXPOSE 80 443

ENTRYPOINT ["nginx"]
