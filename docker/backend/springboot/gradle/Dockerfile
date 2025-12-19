# ========== STAGE 1: BUILDER ==========
# Use Gradle with JDK to build the application
# According to java version used (match your gradle-wrapper)
FROM gradle:8.6-jdk21-alpine AS builder

WORKDIR /app

# Copy Gradle wrapper and configuration files first
COPY gradle ./gradle
COPY gradlew .
COPY gradlew.bat .
COPY build.gradle .
COPY settings.gradle .
COPY src ./src

# Make gradlew executable
RUN chmod +x gradlew

# Download dependencies (cached layer)
RUN ./gradlew dependencies --no-daemon

# Build the application (creates JAR in build/libs/)
RUN ./gradlew bootJar --no-daemon

# ========== STAGE 2: PRODUCTION ==========
# Use minimal JRE to run the application
# Note: JDK version should match builder stage!
FROM eclipse-temurin:21-jre-alpine AS production  

WORKDIR /app

# Create non-root user for security
RUN addgroup -g 1001 -S springgroup && \
    adduser -S springuser -u 1001 -G springgroup

# Copy the built JAR from builder stage
COPY --from=builder --chown=springuser:springgroup /app/build/libs/*.jar app.jar

# Switch to non-root user
USER springuser

# Expose port (Spring Boot default is 8080)
EXPOSE 8080

# Run the JAR file
ENTRYPOINT ["java", "-jar", "app.jar"]