FROM oraclelinux:6
MAINTAINER diodonfrost <diodon.frost@diodonfrost.me>

RUN rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
RUN rpm -Uvh https://yum.puppet.com/puppet5/puppet5-release-el-6.noarch.rpm
RUN yum -y install git puppet-agent sudo which curl openssh-server openssh-clients

RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

VOLUME ["/sys/fs/cgroup"]

CMD ["/sbin/init"]
