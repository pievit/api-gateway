# syntax=docker/dockerfile:1.2
FROM maven:3.8.4-openjdk-17-slim AS build
WORKDIR /app
USER root
COPY pom.xml .
COPY src ./src
RUN --mount=type=cache,target=/root/.m2 mvn -B -Dmaven.test.skip clean package
RUN apt-get -y update
RUN apt-get -y install curl

#
# Package stage
#
FROM openjdk:17-alpine
WORKDIR /app
EXPOSE 8761
COPY --from=build /app/target/*.jar app.jar
ENV JAVA_ARGS=""
ENTRYPOINT exec java $JAVA_OPTS -jar app.jar