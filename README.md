# demo-multi

Maven 多模块 Spring Boot 项目，用于学习容器化 + CI/CD。

## 结构

- `common`：公共模块（被 user-service / order-service 依赖）
- `user-service`：用户服务，端口 8080
- `order-service`：订单服务，端口 8081

## 本地编译打包

```bash
mvn clean package -DskipTests
```

## Docker 构建 & 运行

### user-service

```bash
docker build -t user-service:1.0 -f user-service/Dockerfile .
docker run -d -p 8080:8080 --name user-svc user-service:1.0
curl http://localhost:8080/api/users/hello
```

### order-service

```bash
docker build -t order-service:1.0 -f order-service/Dockerfile .
docker run -d -p 8081:8081 --name order-svc order-service:1.0
curl http://localhost:8081/api/orders/hello
```

## 清理

```bash
docker rm -f user-svc order-svc
docker rmi user-service:1.0 order-service:1.0
```
