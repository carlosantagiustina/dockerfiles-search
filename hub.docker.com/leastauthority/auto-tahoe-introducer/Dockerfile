FROM leastauthority/tahoe-base

RUN /app/env/bin/tahoe create-introducer /var/run/introducer

CMD \
    /app/env/bin/python /app/configure-tahoe /var/run/introducer < /app/config/introducer.json \
    && exec /app/env/bin/tahoe run /var/run/introducer
