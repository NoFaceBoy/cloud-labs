
#Stage 1
# initialize build and set base image for first stage
FROM maven:3.9.5-eclipse-temurin-17 as stage1
# speed up Maven JVM a bit
ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"
# set working directory
WORKDIR ./
# copy just pom.xml
COPY pom.xml .
# go-offline using the pom.xml
RUN mvn dependency:go-offline
# copy your other files
COPY src ./src
# compile the source code and package it in a jar file
RUN mvn clean install -D maven.test.skip=true
#Stage 2
# set base image for second stage
FROM eclipse-temurin:17.0.8.1_1-jre
# set deployment directory
EXPOSE 8080

WORKDIR ./
# copy over the built artifact from the maven image
COPY --from=stage1 target/imdb.jar ./imdb.jar

CMD java -jar ./imdb.jar
