# -------- Build stage --------
FROM maven:3.9.6-eclipse-temurin-17-alpine AS build

WORKDIR /app
COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

# -------- Runtime stage --------
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app
COPY --from=build /app/target/*.jar app.jar

RUN apk add --no-cache bash dos2unix curl

HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:8090/actuator/health || exit 1

ENTRYPOINT ["java","-jar","app.jar"]
