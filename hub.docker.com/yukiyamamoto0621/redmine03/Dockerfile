FROM redmine:3.2

# Setting Mail of Redmine
COPY configuration.yml /home/LogFiles/redmine/config/
RUN chmod 777 /home/LogFiles/redmine/config/configuration.yml \
  && rm -rf /usr/src/redmine/config/configuration.yml.example \
  && ln -s /home/LogFiles/redmine/config/configuration.yml /usr/src/redmine/config/configuration.yml

# ------------------------
# SSH Server support
# ------------------------
RUN apt-get update \
  && apt-get install -y --no-install-recommends openssh-server \
  && echo "root:Docker!" | chpasswd

COPY sshd_config /etc/ssh/
EXPOSE 2222 587

COPY init_container.sh /bin/
RUN chmod 777 /bin/init_container.sh
CMD ["sh", "/bin/init_container.sh"]
