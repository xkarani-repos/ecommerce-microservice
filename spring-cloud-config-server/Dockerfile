# Stage 1: Build the Spring Boot application using Maven
FROM maven:3.8.6-eclipse-temurin-17-alpine AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the pom.xml and download the dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy the source code to the container
COPY src ./src

# Build the Spring Boot application
RUN mvn clean package -DskipTests

# Stage 2: Create the actual Docker image for the Spring Boot application
FROM eclipse-temurin:17-jre-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the jar file from the previous build stage
COPY --from=build /app/target/*.jar config-server.jar


# Expose port 8761 for Config Server
EXPOSE 8888

# Command to run the Spring Boot application
ENTRYPOINT ["java", "-jar", "config-server.jar"]
