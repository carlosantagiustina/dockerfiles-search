# NAME: PostgreSQL 9.6.9
#
# DESCRIPTION: PostgreSQL image.
#
# REQUIRED ENVS:
# DATA_DIRECTORY (ie. "/home/postgres/data/")
#
# OPTIONAL ENVS:
# AWS_ACCESS_KEY_ID (ie. "FDEDFD3FDFD3AKHR3", needed if WAL-E is used)
# AWS_REGION (ie. "eu-west-1", region of the backup bucket, needed if WAL-E is used)
# AWS_S3_WALE_BUCKET_NAME (ie. "tds-app-backup", needed if WAL-E is used)
# AWS_S3_WALE_BUCKET_BASE_PATH (ie. "postgresql/" ending with a slash), needed if WAL-E is used
# AWS_SECRET_ACCESS_KEY (ie. "yrthxdt45678oestwgsdf5es4trgd23", needed if WAL-E is used)
# BACKUP_EXECUTION_TIME (ie. "02:00", time is in UTC and for backups to succeed, WALE needs to be setup)
# HOST_IP (ie. "123.123.123.1", ip of the host this is running on, needed if WAL-E is used)
# MASTER_HOST_IP (ie. "123.123.123.1", ip of the master, needed if ROLE=slave)
# MASTER_HOST_PORT (ie. "5432", port of the master, needed if ROLE=slave)
# MAX_CONNECTIONS (ie. "100", defaults to 500)
# MAX_REPLICATION_SLOTS (ie. "5", defaults to 0, needed if you want replication)
# MAX_WAL_SENDERS (ie. "5", defaults to 0, no replication, needed if you want replication)
# REPLICATOR_PASSWORD (ie. "rep", replicator password, needed if ROLE=slave)
# REPLICATOR_USER (ie. "rep", replicator user, needed if ROLE=slave)
# ROLE (ie. "master", master or slave, default is master)
# RUN_AS_DEVELOPMENT (ie. "1", if set the database will be insecure)
# SHARED_BUFFERS (ie. "16", means 16GB, default is 1)
# SUPERUSER_USERNAME (ie. "superuser")
# SUPERUSER_PASSWORD (ie. "abcdabcd1234")
#
# OTHER:
# The WAL-E variables activate archiving to S3 (archiving will otherwise be off)
#
# Promoting a slave to master:
# - Set the failover ip to the server of the slave you want to promote to master
# - Ssh to the slave server you want to promote "docker exec -it postgresql_slave bash"
# - "touch [data-directory]/failover"
# - This slave is now accepting write connections and acting as the master, it will not try to connect to the previous master
#
#
# After promoting a slave and when the previous master is back up:
# - Set the failover ip to the master server
# - Go to the slave server and do "docker exec -it postgresql_slave bash"
# - Run "wal-e --s3-prefix=s3://tds-database-backups/"$HOST_IP" backup-push "$DATA_DIRECTORY""
# - Shutdown the PostgreSQL slave
# - Go to the master server and follow the instructions for recovery using wal-e
# - When everything is up and running, throw away the slave (data) containers and redeploy them
# - Everything is back to normal
#
#
# Instructions for recovery using wal-e:
# - Make sure PostgreSQL container is not running, "sudo systemctl disable postgresql_master", "sudo systemctl stop postgresql_master"
# - Throw away current data container "docker rm -v postgresql_master_data"
# - Create new data container "sudo systemctl enable [path-to]/postgresql_master", "sudo systemctl start postgresql_master"
# - Stop PostgreSQL again "sudo systemctl disable postgresql_master", "sudo systemctl stop postgresql_master"
# - Start a container to execute backup commands:
#     /usr/bin/docker run \
#     -it --rm \
#     -e "AWS_ACCESS_KEY_ID=[access key]" \
#     -e "AWS_REGION=[region]" \
#     -e "AWS_S3_WALE_BUCKET_NAME=[bucket name]" \
#     -e "AWS_S3_WALE_BUCKET_BASE_PATH=[path]" \
#     -e "AWS_SECRET_ACCESS_KEY=[aws secret access key]" \
#     -e "DATA_DIRECTORY=[data-directory]" \
#     -e "HOST_IP=[current master host ip, not this server]" \
#     --volumes-from postgresql_master_data \
#     --entrypoint=bash \
#     thedutchselection/postgresql:9.6.9
# - Run "wal-e --s3-prefix=s3://tds-database-backups/"$HOST_IP" backup-fetch "$DATA_DIRECTORY" LATEST" (you can also change LATEST to a backup name ie. base_0000000200000001000000FF_00000040)
# - Run "echo "restore_command = 'wal-e --s3-prefix=s3://tds-database-backups/"$HOST_IP" wal-fetch %f %p'" > "$DATA_DIRECTORY""recovery.conf""
# - If you want to restore to a specific point in time (else latest will be used) do "echo "recovery_target_time = '2016-02-19 15:50:00 GMT'" >> "$DATA_DIRECTORY""recovery.conf""
# - Exit this container.
# - Remove S3 wal-e data for this PostgreSQL.
# - Start PostgreSQL "sudo systemctl enable [path-to]/postgresql_master", "sudo systemctl start postgresql_master" and everything should run fine.
#
#
# Show status of replication on the slave server:
# - Do "docker exec -it postgresql_slave bash"
# - Do "psql"
# - Execute the query "select now() - pg_last_xact_replay_timestamp() AS replication_delay;"
# This gives the replication delay. If there are no wirtes to the master, this might be bigger without any problems.

FROM thedutchselection/debian:8.7
MAINTAINER Gerard Meijer <g.meijer@thedutchselection.com>

RUN \
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
  echo "deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
  apt-get update && \
  apt-get install -yq python-software-properties && \
  apt-get install -yq software-properties-common && \
  useradd -m -u 5432 postgres && \
  apt-get install -yq postgresql-9.6=9.6.9-2.pgdg80+1 && \
  apt-get install -yq postgresql-client-9.6=9.6.9-2.pgdg80+1 && \
  apt-get install -yq postgresql-contrib-9.6=9.6.9-2.pgdg80+1 && \
  apt-get install -yq postgresql-server-dev-9.6=9.6.9-2.pgdg80+1 && \
  apt-get install -yq curl && \
  apt-get install -yq lzop && \
  apt-get install -yq pv && \
  apt-get install -yq python3-pip && \
  apt-get install -yq inotify-tools && \
  apt-get install -yq psmisc && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN \
  pip3 install gevent==1.2.1 && \
  pip3 install boto==2.46.1 && \
  pip3 install wal-e==1.0.2

ADD files/pg_hba_template.conf /etc/postgresql/
ADD files/pg_ident.conf /etc/postgresql/
ADD files/postgresql_template.conf /etc/postgresql/
ADD files/scripts /usr/local/bin

RUN \
  chmod 640 /etc/postgresql/pg_hba_template.conf && \
  chmod 640 /etc/postgresql/pg_ident.conf && \
  chmod 644 /etc/postgresql/postgresql_template.conf && \
  chmod +x /usr/local/bin/run.sh && \
  chown -R postgres:postgres /etc/postgresql && \
  chown postgres:postgres /usr/local/bin/*

EXPOSE 5432

USER postgres

ENTRYPOINT ["/bin/bash", "/usr/local/bin/run.sh"]
