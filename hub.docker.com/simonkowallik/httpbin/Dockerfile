FROM alpine:3.11

LABEL name="httpbin"
LABEL version="0.9.2"
LABEL description="A simple HTTP service."
LABEL org.kennethreitz.vendor="Kenneth Reitz"

ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

WORKDIR /httpbin
ADD . /httpbin

RUN apk update --no-cache; \
    apk add --no-cache python3 libffi libstdc++ && \
    apk add --no-cache --virtual .build-deps git alpine-sdk python3-dev libffi-dev && \
    pip3 install --no-cache-dir pipenv gunicorn && \
    pipenv lock -r > /httpbin/requirements.txt && \
    pip3 install --no-cache-dir /httpbin && \
    apk del .build-deps

EXPOSE 80

CMD ["gunicorn", "-b", "0.0.0.0:80", "httpbin:app", "-k", "gevent"]
