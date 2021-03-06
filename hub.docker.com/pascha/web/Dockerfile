###################################################
#      NGINX 1.9.7-1
###################################################

FROM debian:jessie

MAINTAINER Pascha Butretell <admin@pascha.domains>

#==================================================
#    ENV VARIABLES / POST INSTALLATION
#==================================================

# Change this value to force a full image rebuild
ENV    IMAGE_VERSION "2015-12-19"

# Skip all post-install configurations steps
ENV    DEBIAN_FRONTEND "noninteractive"

# Set the new user/group with fixed uid/gid
ENV    MASTER_USER "pascha"
ENV    MASTER_GROUP "pascha"
ENV    MASTER_UID "3342"
ENV    MASTER_GID "3342"

# Set the CPU architecture
ENV    CPU_ARCHITECTURE1 "amd64"
ENV    CPU_ARCHITECTURE2 "x86_64"

# Set the system language
ENV    SYS_LANGUAGE "en_US.UTF-8"
RUN    export LANGUAGE=${SYS_LANGUAGE} \
    && export LC_ALL=${SYS_LANGUAGE} \
    && export LANG=${SYS_LANGUAGE}
    
# Set the timezone
ENV    TIMEZONE "Europe/Zurich"

# Define softwares versions
ENV    GOSU_VERSION "1.7"
	
# Set nginx version
ENV    NGINX_VERSION 1.9.7-1~jessie

# Import releases key (run gpg software to import pgp key) 
# Gosu key
RUN    gpg --keyserver pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
# Nginx key
RUN    apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62

#==================================================
#    CREATE USERS / GROUPS
#==================================================

# Create user and group with predefined UID and GID
RUN    groupadd -g ${MASTER_GID} -r ${MASTER_GROUP} \
    && useradd -u ${MASTER_UID} -m -r -g ${MASTER_GID} ${MASTER_USER}
	
#==================================================
#      REPOSITORIES
#==================================================    

# Add default repositories
RUN    echo ' \
             deb http://ftp.ch.debian.org/debian/ jessie main\n \
             deb http://security.debian.org/ jessie/updates main\n \
             deb http://ftp.ch.debian.org/debian/ jessie-updates main\n \
             deb http://http.debian.net/debian jessie-backports main\n \
             ' \ 
       > /etc/apt/sources.list

# Add specific repositories
RUN    echo 'deb http://nginx.org/packages/mainline/debian/ jessie nginx' > "/etc/apt/sources.list.d/nginx.list"

#==================================================
#      INSTALLATIONS / CONFIGURATIONS
#==================================================


#      INSTALL PREREQUISITES
#--------------------------------------------------

ADD https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-${CPU_ARCHITECTURE1} /usr/local/bin/gosu
ADD https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-${CPU_ARCHITECTURE1}.asc /usr/local/bin/gosu.asc

RUN    apt-get update \
    && apt-get install -y \
       --no-install-recommends \
       locales ca-certificates \
    && gpg --verify /usr/local/bin/gosu.asc \
    && echo ${TIMEZONE} > /etc/timezone \
    && dpkg-reconfigure tzdata \
    && locale-gen ${SYS_LANGUAGE} \
    && dpkg-reconfigure locales

	
#      INSTALL TOOLS / LIBS
#--------------------------------------------------

	
#      INSTALL APPLICATIONS
#--------------------------------------------------

RUN    apt-get update \
    && apt-get install -y \
       --no-install-recommends \
       nginx=${NGINX_VERSION}	    

#   CONFIGURE APPLICATIONS
#--------------------------------------------------

RUN    chmod +x /usr/local/bin/gosu
ADD    default.conf /etc/nginx/conf.d/default.conf
ADD    nginx /etc/logrotate.d/nginx
ADD    nginx.conf /etc/nginx/nginx.conf

# Forward request and error logs to docker log collector
# and set some permissions   
RUN    ln -sf /dev/stderr /var/log/nginx/error.log \
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && touch /var/run/nginx.pid \
    && chown ${MASTER_USER}:${MASTER_GROUP} /var/cache/nginx \
    && chown ${MASTER_USER}:${MASTER_GROUP} /var/run/nginx.pid \
	&& chown -R ${MASTER_USER}:${MASTER_GROUP} /etc/nginx/

#==================================================
#    UNINSTALL / CLEANUP
#==================================================

RUN    apt-get autoremove --purge -y \
       ca-certificates \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /tmp/*
	
#==================================================
#    OTHER STUFF
#==================================================

EXPOSE 16000 16001

ENTRYPOINT ["/usr/sbin/nginx"]
CMD ["-g","daemon off;"]