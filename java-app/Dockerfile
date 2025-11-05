# Stage 1: Build the JAR file
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /java-app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn package -DskipTests

# Stage 2: Run the JAR file
FROM openjdk:17-jdk-slim
WORKDIR /java-app
COPY --from=builder /java-app/target/demo-1.0.0.jar java-app.jar

EXPOSE 8080
ENTRYPOINT ["java","-jar","java-app.jar"]