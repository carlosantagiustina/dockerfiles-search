FROM python:2.7.15-alpine

WORKDIR /cronjob

COPY /root /
RUN apk add --no-cache tzdata \
  && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
  && pip install --no-cache-dir -r requirements.txt \
  && rm -rf /var/cache/apk/*
CMD ["/usr/sbin/crond","-c","/etc/crontabs","-f"]
