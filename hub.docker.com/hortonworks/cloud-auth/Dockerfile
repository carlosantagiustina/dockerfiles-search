FROM node:8.4.0-alpine
MAINTAINER SequenceIQ

ENV SL_SERVER_PORT 3001
RUN apk add --no-cache curl bash git
EXPOSE 3001
ADD . /hortonworks-cloud-auth
RUN cd /hortonworks-cloud-auth && npm install
RUN cp -R /hortonworks-cloud-auth/schema /

CMD ["/hortonworks-cloud-auth/start-docker.sh"]
