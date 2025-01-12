# Stage 1: Build Stage
FROM maven:3.8.4-openjdk-11-slim as builder

# Set the working directory
WORKDIR /app

# Copy only the pom.xml and dependencies for caching
COPY pom.xml ./

# Pre-download dependencies to leverage caching
RUN mvn dependency:go-offline -B

# Copy the rest of the application code
COPY src ./src

# Build the application (skip tests for faster builds)
RUN mvn clean package -DskipTests

# Stage 2: Runtime Stage
FROM eclipse-temurin:11.0.21_9-jre-jammy

# Set the working directory
WORKDIR /app

# Copy the jar file from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose the application port
EXPOSE 8761

# Run the jar file
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
