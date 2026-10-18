# syntax=docker/dockerfile:1
# =============================================================================
#  Dockerfile - FILE KHUNG (multi-stage build cho Spring Boot 4 / Java 21 / Gradle)
#  Sinh viên hoàn chỉnh các ô ______ theo yêu cầu Phần 2 của đề thi.
# =============================================================================

# ---------- Stage 1: build - biên dịch & đóng gói bằng Gradle + JDK 21 ----------
# Spring Boot 4 yêu cầu Gradle >= 8.14
FROM gradle:8.14-jdk21-alpine  AS build
WORKDIR /workspace

# Copy build.gradle + settings.gradle trước, tải dependency để tận dụng cache layer
COPY build.gradle settings.gradle ./
RUN gradle dependencies --no-daemon -q

# Copy mã nguồn rồi đóng gói file JAR (bỏ qua test)
COPY src ./src
RUN gradle bootJar -x test --no-daemon -q

# ---------- Stage 2: runtime - chỉ chứa JRE 21 + file JAR ----------
FROM eclipse-temurin:21-jre-alpine AS runtime

# Phiên bản ứng dụng truyền vào lúc build (docker compose: build-arg APP_VERSION)
ARG APP_VERSION=dev
ENV APP_VERSION=${APP_VERSION} \
    JAVA_TOOL_OPTIONS="-XX:MaxRAMPercentage=75"

WORKDIR /app

# Chạy ứng dụng bằng user thường, không dùng root
RUN addgroup -S spring && adduser -S spring -G spring
# Chỉ lấy file JAR từ stage build
COPY --from=build --chown=spring:spring /workspace/build/libs/app.jar /app/app.jar
USER spring

EXPOSE 8080

# ENTRYPOINT dạng exec (mảng JSON)
ENTRYPOINT ["java", "-jar", "/app/app.jar"]

Run entrypoint.sh
  Will download drone-ssh-1.8.0-linux-amd64 from https://github.com/appleboy/drone-ssh/releases/download/v1.8.0
  ======= CLI Version =======
  Drone SSH version 1.8.0
  ===========================
  2026/10/16 04:40:48 ssh: handshake failed: ssh: unable to authenticate, attempted methods [none publickey], no supported methods remain
  Error: Process completed with exit code 1. \
