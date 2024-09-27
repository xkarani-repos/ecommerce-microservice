# Stage 1: Build the Spring Boot application using Maven
FROM maven:3.8.6 AS build

# Download and install OpenJDK 13 manually
RUN apt-get update && \
    apt-get install -y wget && \
    wget https://github.com/AdoptOpenJDK/openjdk13-binaries/releases/download/jdk-13.0.2%2B8/OpenJDK13U-jdk_x64_linux_hotspot_13.0.2_8.tar.gz && \
    tar -xvf OpenJDK13U-jdk_x64_linux_hotspot_13.0.2_8.tar.gz && \
    mv jdk-13.0.2+8 /opt/openjdk-13 && \
    rm OpenJDK13U-jdk_x64_linux_hotspot_13.0.2_8.tar.gz

# Set JAVA_HOME for Java 13
ENV JAVA_HOME=/opt/openjdk-13
ENV PATH="$JAVA_HOME/bin:$PATH"

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
FROM openjdk:13-jdk-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the jar file from the previous build stage
COPY --from=build /app/target/*.jar product-catalog-service.jar

# Expose port 8810 for product catalog
EXPOSE 8810

# Command to run the Spring Boot application
ENTRYPOINT ["sh", "-c", "java -jar product-catalog-service.jar"]
