FROM alpine:3.9
ENV PYTHONUNBUFFERED 1
RUN apk update && apk upgrade && apk --update add \
    linux-headers musl-dev gcc zlib-dev jpeg-dev \
    python3 python3-dev postgresql-dev \
    nodejs nodejs-npm  libsass sassc
RUN pip3 install uwsgi
RUN npm install -g yuglify
RUN mkdir /website
WORKDIR /website
ADD requirements.txt /website/
RUN pip3 install -r requirements.txt
ADD package.json /website/
ADD package-lock.json /website/
RUN npm install
ADD . /website/
ENV DJANGO_ENV=prod
EXPOSE 8000
COPY ./docker-entrypoint.sh /
RUN chmod +x /docker-entrypoint.sh
ENTRYPOINT /docker-entrypoint.sh
