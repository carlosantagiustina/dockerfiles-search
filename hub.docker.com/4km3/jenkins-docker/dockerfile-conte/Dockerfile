FROM nginx:stable

# Metadata
LABEL mantainer="Gius. Camerlingo <gcamerli@gmail.com>"
LABEL version="1.0"
LABEL description="Nginx server for coming soon page."

# Docker image name
ENV NAME=nginx-launcher

# Timezone
ENV TZ="Europe/Paris"

# Update system and install packages
RUN apt-get update
RUN apt-get install -y iputils-ping --no-install-recommends

# Clean system
RUN apt-get clean 
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# User
RUN useradd -ms /bin/bash docker

# Home
ENV HOME=/home/docker

# Permissions
RUN chown -R docker:docker $HOME

# Healthcheck
COPY healthcheck /usr/local/bin/
RUN chmod 744 /usr/local/bin/healthcheck

# Change user
USER docker
