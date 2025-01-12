# Build stage
FROM maven:3.8.4-openjdk-11-slim as builder

WORKDIR /app

# Copy only pom.xml first to cache dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code and build
COPY src ./src
RUN mvn clean package -DskipTests

# Run stage - using the smallest possible JRE base image
FROM eclipse-temurin:11.0.21_9-jre-jammy

WORKDIR /app

# Copy jar from builder stage and rename it to reduce path length
COPY --from=builder /app/target/*.jar app.jar

# Configure minimal JVM options
ENV JAVA_OPTS="-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0 -Xss256k -XX:MaxMetaspaceSize=128M"

EXPOSE 8000

# Using a minimal health check to save space
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -q --spider http://localhost:8000/actuator/health || exit 1

ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]