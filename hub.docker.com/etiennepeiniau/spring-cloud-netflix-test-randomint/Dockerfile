FROM maven:3.5.4-jdk-8 as builder
WORKDIR /usr/src/app/
COPY . /usr/src/app/
RUN mvn package

FROM openjdk:8-jdk-alpine
WORKDIR /usr/build/app/
COPY --from=builder /usr/src/app/target/*.jar /usr/build/app/app.jar
ENTRYPOINT exec java -Djava.security.egd=file:/dev/./urandom $JAVA_OPTS -jar /usr/build/app/app.jar