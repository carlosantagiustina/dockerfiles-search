FROM rethink/catalogue-baseline
MAINTAINER marc.emmelmann@fokus.fraunhofer.de

#
# Attention:  run with
#
#		docker  run  -it  --net=host 
#


# get the source code from the github repository.
# clone the source code into the "catalogue" directory
RUN cd /opt/reTHINK && git clone https://github.com/reTHINK-project/dev-catalogue.git

RUN cd /opt/reTHINK && ln -s dev-catalogue catalogue

# build the files
RUN cd /opt/reTHINK/catalogue && mvn install

RUN ln -s /opt/reTHINK/catalogue/catalogue_database/target/rethink-catalogue-database-*-jar-with-dependencies.jar /opt/reTHINK/catalogue/catalogue_database/target/rethink-catalogue-database-jar-with-dependencies.jar

# ENTRYPOINT is the command that is always run.
ENTRYPOINT ["java", "-jar", "/opt/reTHINK/catalogue/catalogue_database/target/rethink-catalogue-database-jar-with-dependencies.jar", "-o", "/opt/reTHINK/catalogue/catalogue_objects"]
