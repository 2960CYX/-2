# 使用官方的 Maven 镜像作为构建环境，包含 JDK 8
FROM docker.m.daocloud.io/library/maven:3.8.3-openjdk-11-slim AS builder
WORKDIR /app
COPY . .
# 构建 ruoyi-admin 模块并跳过测试
RUN mvn -B -DskipTests -pl ruoyi-admin -am package

# --- 第二阶段：创建最终的运行镜像 ---

# 使用轻量级的 OpenJDK 8 镜像作为最终的运行环境
FROM docker.m.daocloud.io/eclipse-temurin:8-jre
WORKDIR /app
COPY --from=builder /app/ruoyi-admin/target/ruoyi-admin.jar .
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "ruoyi-admin.jar"]