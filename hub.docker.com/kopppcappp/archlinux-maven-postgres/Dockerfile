FROM kopppcappp/archlinux-yaourt
MAINTAINER Mikhail Belyaev

RUN pacman -Syy
RUN pacman --noconfirm --needed -S postgresql
RUN sudo -u postgres initdb --locale en_US.UTF-8 -E UTF8 -D "/var/lib/postgres/data"
RUN sudo systemctl enable postgresql
RUN sudo mkdir /run/postgresql
RUN sudo chown postgres:postgres /run/postgresql
RUN sudo -u postgres /usr/bin/postgresql-check-db-dir "/var/lib/postgres/data"
COPY start_postgresql.sh /usr/bin/start_postgresql.sh
RUN sudo chmod +s /usr/bin/start_postgresql.sh
RUN sudo chmod +x /usr/bin/start_postgresql.sh
CMD /usr/bin/start_postgresql.sh
RUN sudo -u yaourt yaourt --noconfirm --needed -S jdk
RUN pacman --noconfirm --needed -S maven
