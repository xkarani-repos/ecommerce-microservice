# Stage 1: Build the Spring Boot application using Maven
FROM maven:3.8.6 AS build-stage

# Set the working directory inside the container
WORKDIR /app

# Copy the pom.xml and download the dependencies
COPY pom.xml ./
RUN mvn dependency:go-offline -B

# Copy the source code to the container
COPY src ./src

# Build the Spring Boot application
RUN mvn clean package -DskipTests

# Stage 2: Create the actual Docker image for the Spring Boot application
FROM openjdk:8-jdk-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the jar file from the previous build stage
COPY --from=build-stage /app/target/*.jar api-gateway.jar

# Expose port 8080 for the application
EXPOSE 8080

# Command to run the Spring Boot application
ENTRYPOINT ["sh", "-c", "echo 'Environment Variables:' && env && echo 'Starting application...' && java -jar api-gateway.jar"]
