FROM alexsuch/angular-cli:1.6

WORKDIR /project

ADD . .

RUN npm i
RUN ng build --prod

FROM jonnybgod/alpine-nginx:master

WORKDIR /src

COPY --from=0 /project/dist/ .
ADD ./nginx.conf /etc/nginx/nginx.conf

EXPOSE 80 443

ENTRYPOINT ["nginx"]