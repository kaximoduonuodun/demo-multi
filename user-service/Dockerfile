# ========== Stage 1: Build ==========
FROM maven:3.9.6-eclipse-temurin-21-alpine AS builder

WORKDIR /build

# 先拷贝 pom，利用 Docker 缓存层加速
COPY pom.xml .
COPY common/pom.xml ./common/
COPY user-service/pom.xml ./user-service/
COPY order-service/pom.xml ./order-service/

# 下载依赖（这一层会被缓存）
RUN mvn -B -f pom.xml dependency:go-offline

# 拷贝源码
COPY common ./common
COPY user-service ./user-service
COPY order-service ./order-service

# 只构建 user-service（跳过测试加速）
RUN mvn -B -f user-service/pom.xml clean package -DskipTests

# ========== Stage 2: Runtime ==========
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# 从构建阶段拷贝 jar
COPY --from=builder /build/user-service/target/*.jar app.jar

# 创建非 root 用户
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
