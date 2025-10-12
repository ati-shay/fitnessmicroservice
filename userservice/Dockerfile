# Stage 1: Build the project
FROM maven:3.9.3-eclipse-temurin-17 AS build

# Set working directory
WORKDIR /app

# Copy pom.xml and download dependencies first (caching benefit)
COPY pom.xml .
COPY .mvn/ .mvn
COPY mvnw .
RUN chmod +x mvnw
RUN ./mvnw dependency:go-offline

# Copy the rest of the source code
COPY src/ ./src

# Build the jar
RUN ./mvnw clean package -DskipTests

# Stage 2: Run the application
FROM eclipse-temurin:17-jdk-jammy

WORKDIR /app

# Copy the jar from build stage
COPY --from=build /app/target/userservice-0.0.1-SNAPSHOT.jar app.jar

# Expose the port your service runs on
EXPOSE 8081

# Command to run the jar
ENTRYPOINT ["java","-jar","app.jar"]
