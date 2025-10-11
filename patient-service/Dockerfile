# syntax=docker/dockerfile:1.4

# ---------- Stage 1: Build ----------
FROM maven:3.9.6-eclipse-temurin-21 AS builder
WORKDIR /app

# Copy pom.xml first to leverage dependency caching
COPY pom.xml .

# Download dependencies (cached with --mount)
RUN --mount=type=cache,target=/root/.m2 mvn -B dependency:go-offline

# Copy the rest of the project
COPY . .

# Build the project (skip tests, cache .m2)
RUN --mount=type=cache,target=/root/.m2 mvn -B package -DskipTests

# ---------- Stage 2: Run ----------
FROM eclipse-temurin:21-jdk
WORKDIR /app

# Copy built jar from builder stage
COPY --from=builder /app/target/patient-service-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 4000
ENTRYPOINT ["java", "-jar", "app.jar"]
