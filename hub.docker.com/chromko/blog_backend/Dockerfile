FROM express42/ruby:2.3

HEALTHCHECK CMD curl --fail http://localhost:9292/health || exit 1

RUN echo "Wow!"
CMD ["/usr/local/bundle/bin/puma"]

