FROM alpine:3

RUN apk add --no-cache \
      bash=5.0.0-r0 \
      git=2.22.2-r0 && \
    echo "nogroup:*:100:nobody" > /etc/group && \
    echo "nobody:*:100:100:::" > /etc/passwd

USER 100:100
ENTRYPOINT ["/usr/bin/git"]
