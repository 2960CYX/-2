# --- 第一阶段：构建前端静态文件 ---
FROM node:20-alpine AS builder

# 设置工作目录
WORKDIR /app

# 仅复制依赖声明文件，充分利用 Docker 构建缓存
COPY package.json ./

# 安装依赖，使用淘宝镜像源加速
RUN yarn install --non-interactive --registry=https://registry.npmmirror.com

# 复制所有源代码
COPY . .

# 执行构建
RUN yarn build:prod

# --- 第二阶段：创建 Nginx 运行镜像 ---
FROM nginx:1.25-alpine

# 移除默认的 Nginx 欢迎页面
RUN rm /etc/nginx/conf.d/default.conf

# 复制我们自定义的 Nginx 配置文件
COPY nginx.conf /etc/nginx/conf.d/

# 从构建阶段复制打包好的静态文件到 Nginx 的 web 根目录
COPY --from=builder /app/dist /usr/share/nginx/html

# 暴露 Nginx 监听的端口
EXPOSE 80

# 容器启动时执行的命令
ENTRYPOINT ["nginx", "-g", "daemon off;"]
