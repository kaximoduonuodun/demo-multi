# ========== Stage 1: Build ==========
FROM maven:3.9.6-eclipse-temurin-21-alpine AS builder

WORKDIR /build

COPY pom.xml .
COPY common/pom.xml ./common/
COPY user-service/pom.xml ./user-service/
COPY order-service/pom.xml ./order-service/

RUN mvn -B -f pom.xml dependency:go-offline

COPY common ./common
COPY user-service ./user-service
COPY order-service ./order-service

RUN mvn -B -f order-service/pom.xml clean package -DskipTests

# ========== Stage 2: Runtime ==========
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

COPY --from=builder /build/order-service/target/*.jar app.jar

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]
