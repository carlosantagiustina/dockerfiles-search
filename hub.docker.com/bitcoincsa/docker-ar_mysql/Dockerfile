FROM mysql:5.5

LABEL version="1.0"

# Use Custom custom.cnf configuration
COPY config/my.cnf /etc/mysql/conf.d/ 
RUN chmod 664 /etc/mysql/conf.d/my.cnf
