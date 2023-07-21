# Stage 1
FROM maven:3-openjdk-11-slim as build

WORKDIR /workspace/app

# Clone Platform project and build it (the client module is a dependency for this project)
RUN apt-get update
RUN apt-get -y install git
RUN git clone https://github.com/opendatamesh-initiative/odm-platform.git

WORKDIR /workspace/app/odm-platform

RUN mvn clean install -DskipTests

WORKDIR /workspace/app

RUN git clone https://github.com/opendatamesh-initiative/odm-platform-up-services-policy-opa.git

WORKDIR /workspace/app/odm-platform-up-services-policy-opa

RUN mvn clean package spring-boot:repackage -DskipTests

# Stage 2
FROM openjdk:11-jre-slim

ARG SPRING_PROFILES_ACTIVE=docker
ARG JAVA_OPTS
ARG DATABASE_URL
ARG DATABASE_USERNAME
ARG DATABASE_PASSWORD
ARG FLYWAY_SCHEMA=ODMPOLICY
ARG FLYWAY_SCRIPTS_DIR=postgresql
ARG OPA_HOSTNAME
ARG OPA_PORT
ARG SPRING_PORT=4242
ENV APP_NAME ${APP_NAME}
ENV SPRING_PROFILES_ACTIVE ${SPRING_PROFILES_ACTIVE}
ENV JAVA_OPTS ${JAVA_OPTS}
ENV DATABASE_URL ${DATABASE_URL}
ENV DATABASE_USERNAME ${DATABASE_USERNAME}
ENV DATABASE_PASSWORD ${DATABASE_PASSWORD}
ENV FLYWAY_SCHEMA ${FLYWAY_SCHEMA}
ENV FLYWAY_SCRIPTS_DIR ${FLYWAY_SCRIPTS_DIR}
ENV OPA_HOSTNAME ${OPA_HOSTNAME}
ENV OPA_LOCAL_PORT ${OPA_PORT}
ENV SPRING_LOCAL_PORT ${SPRING_PORT}

COPY --from=build  /workspace/app/odm-platform-up-services-policy-opa/opa-client/target/odm-platform-up-policy-opa-client-*.jar /app/
COPY --from=build  /workspace/app/odm-platform-up-services-policy-opa/opa-policy-server/target/odm-platform-up-policy-opa-server-*.jar /app/

RUN ln -s -f /usr/share/zoneinfo/Europe/Rome /etc/localtime

CMD java $JAVA_OPTS -jar /app/odm-platform-up-policy-opa-server-*.jar --spring.profiles.active=$SPRING_PROFILES_ACTIVE

EXPOSE $SPRING_PORT