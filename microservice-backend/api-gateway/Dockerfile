# ===== BUILD STAGE =====
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /app

# Cache dependencies first (better layer caching)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Package application (skip tests to speed up build)
RUN mvn clean package spring-boot:repackage -DskipTests


# ===== RUNTIME STAGE =====
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Copy only the built jar
COPY --from=build /app/target/*.jar app.jar

# Use non-root user for security
RUN addgroup -S spring && adduser -S spring -G spring
USER spring

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]

