From centos:7
RUN rpm -i http://ngtech.co.il/repo/centos/7/squid-repo-1-1.el7.centos.noarch.rpm
RUN yum -y update && yum -y install squid && yum -y install wget && wget https://raw.githubusercontent.com/minepicco/dockerfiles/master/squid/squid.conf
RUN chmod +x squid.conf
RUN mv -f squid.conf /etc/squid/squid.conf
RUN yum -y update 
ENTRYPOINT ["sh", "-c", "/etc/squid/squid start", "BACKGROUND"]

EXPOSE 3128
