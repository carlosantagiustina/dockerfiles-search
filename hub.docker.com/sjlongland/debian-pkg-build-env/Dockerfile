FROM debian:buster-slim
# Force non-interactive behaviour out of debconf:
ENV DEBIAN_FRONTEND noninteractive

# Install everything
RUN	apt-get update \
	&& apt-get install -y gnupg1 gnupg2 lsb-release \
	&& printf 'deb [arch=amd64,i386,ppc64el] http://mirror.aarnet.edu.au/pub/MariaDB/repo/10.4/%s %s main\n' \
		$( lsb_release -si | tr A-Z a-z ) \
		$( lsb_release -sc | tr A-Z a-z ) \
		> /etc/apt/sources.list.d/mariadb.list \
	&& apt-key adv --recv-keys --keyserver \
		keyserver.ubuntu.com 0xF1656F24C74CD1D8 \
	&& apt-get update \
	&& apt-get install -y \
		pkg-config libssl-dev libmariadb-dev-compat libmysqlclient18 \
		debhelper python3 dh-virtualenv python3-setuptools \
		python3-pip python3-dev libffi-dev argon2 fakeroot dpkg-dev \
		lsb-release python3-pymysql python3-tz python3-yaml \
		python3-passlib python3-lxml python3-iso8601 gosu gnupg2 \
		python3-virtualenv mysql-client virtualenv wget curl git \
		build-essential \
	&& curl -sL https://deb.nodesource.com/setup_8.x | bash - \
	&& apt-get update \
	&& apt-get install -y nodejs \
	&& apt-get clean \
	&& rm -fr /var/lib/apt/lists/*
