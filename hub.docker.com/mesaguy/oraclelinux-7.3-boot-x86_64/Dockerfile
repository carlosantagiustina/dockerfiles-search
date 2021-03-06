# Source (EOL)
FROM oraclelinux:7.3

# Clean (purge) the repo cache
RUN yum -d1 -y clean all && rm -rf /var/cache/yum/* && \
# Install docker OverlayFS plugin for yum:
yum -d1 -y install yum-plugin-ovl || true && \
# Re-purge cache
yum -d1 -y clean all && \
# Install systemctl
yum -d1 -y install systemd systemd-libs dbus fsck.ext4 && \
# Strip down systemd
find /lib/systemd/system/sysinit.target.wants -type l ! -name systemd-tmpfiles-setup.service -delete && \
rm -f /lib/systemd/system/multi-user.target.wants/* && \
rm -f /etc/systemd/system/*.wants/* && \
rm -f /lib/systemd/system/local-fs.target.wants/* && \
rm -f /lib/systemd/system/sockets.target.wants/*udev* && \
rm -f /lib/systemd/system/sockets.target.wants/*initctl* && \
rm -f /lib/systemd/system/basic.target.wants/* && \
# Clean (purge) the repo cache
yum -d1 -y clean all && rm -rf /var/cache/yum/* && \
# Purge logs
rm -rf /var/log/* /var/run/log/journal

# Without this, init won't start the enabled services. Starting the services
# fails with one of:
#     Failed to get D-Bus connection: Operation not permitted
#     System has not been booted with systemd as init system (PID 1). Can't operate.
#     Failed to connect to bus: No such file or directory
VOLUME [ "/sys/fs/cgroup", "/run" ]

# Start via systemd
CMD ["/usr/lib/systemd/systemd"]

# Image metadata
LABEL org.label-schema.vendor="Mesaguy" \
      org.label-schema.name="Oracle Linux 7.3 boot" \
      org.label-schema.description="Image boots Oracle Linux 7.3 via systemd" \
      com.mesaguy.operatingsystem.eol="true"
