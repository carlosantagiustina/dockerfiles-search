FROM mesaguy/debian-8-kitchen-base-x86_64:latest

# Update repo definitions
RUN apt update && \
# Install python for ansible
apt install -y apt-utils python python-apt && \
# Clean (purge) the repo cache
apt-get clean && \
rm -rf /var/lib/apt/lists/* && \
# Purge logs
rm -rf /var/log/*

# Image metadata
LABEL org.label-schema.vendor="Mesaguy" \
      org.label-schema.name="Debian Linux 8 (Jessie) kitchen-ansible" \
      org.label-schema.description="Image boots Debian Linux 8 (Jessie) via systemd configured for kitchen ansible testing" \
      com.mesaguy.operatingsystem.eol="false"
