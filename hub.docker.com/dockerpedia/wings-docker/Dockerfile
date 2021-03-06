FROM tomcat:8
RUN sed -i 's/debian testing main/debian testing main contrib non-free/' /etc/apt/sources.list

# Install general tools
RUN apt-get update && apt-get -y install --no-install-recommends \
        graphviz \
        libcurl4-openssl-dev \
        libxml2-dev \
        python-pip \
        git \
        cgroupfs-mount \
        maven \
        tcl \
        tk \
        apt-transport-https \
        software-properties-common \
        gnupg2 \
        python-dev \
        python-setuptools \
        python-tk

# Install Docker
RUN curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add -
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable"
RUN apt-get update && apt-get -y install docker-ce

# Configure tomcat
# Add the tomcat users file
COPY ./config/tomcat/tomcat-users.xml /usr/local/tomcat/conf/

# Add the tomcat server configuration file
COPY ./config/tomcat/server.xml /usr/local/tomcat/conf/

# Add wings environment variable
ENV WINGS_MODE='dind'

# Install wings
COPY ./config/pom.xml /wings-src/pom.xml
RUN mkdir -p /wings-src
WORKDIR /wings-src
RUN mvn package

# Add wings properties
RUN mkdir -p /opt/wings/storage/default
COPY ./config/default/portal.properties /opt/wings/storage/default/portal.properties

# Add Wings build to tomcat (Deploy)
RUN cp -R /wings-src/target/wings-portal-4.1-SNAPSHOT /usr/local/tomcat/webapps/wings-portal

# Add wings context file to the wings portal webapp
COPY ./config/default/wings-portal.xml /usr/local/tomcat/webapps/wings-portal/META-INF/context.xml

# Install pip requirements
COPY ./config/requirements.txt /wings-src/
RUN for p in $(cat /wings-src/requirements.txt); do pip install $p; done

RUN echo "backend : Agg" ~/.config/matplotlib/matplotlibrc

WORKDIR /usr/local/tomcat

