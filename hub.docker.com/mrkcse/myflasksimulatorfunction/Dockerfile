FROM tiangolo/uwsgi-nginx-flask:python3.6-alpine3.7

COPY ./app /app

ENV LISTEN_PORT 80

EXPOSE 80

ENV NGINX_WORKER_PROCESSES auto

ENV NGINX_WORKER_CONNECTIONS 2048

ENV NGINX_WORKER_OPEN_FILES 2048

ENV NGINX_MAX_UPLOAD 1m
