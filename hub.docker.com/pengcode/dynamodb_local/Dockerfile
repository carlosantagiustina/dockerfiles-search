FROM openjdk:8-jre

RUN curl -o dynamodb.tar.gz -L http://dynamodb-local.s3-website-us-west-2.amazonaws.com/dynamodb_local_latest.tar.gz && \
    mkdir -p /opt/dynamodb_local && \
    mkdir -p /data && \
    tar -zxf dynamodb.tar.gz --directory /opt/dynamodb_local && \
    rm -rf dynamodb.tar.gz

EXPOSE 8000

CMD ["java", "-Djava.library.path=/opt/dynamodb_local/DynamoDBLocal_lib", \
     "-jar", "/opt/dynamodb_local/DynamoDBLocal.jar", \
     "-dbPath", "/data", \
      "-sharedDb"]
