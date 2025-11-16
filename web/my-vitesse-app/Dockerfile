# --- 第一阶段：构建前端静态文件 ---
FROM node:20-alpine AS builder

# 启用 corepack 并激活 pnpm，确保与项目 packageManager 保持一致
RUN corepack enable && corepack prepare pnpm@10.7.0 --activate

# 设置工作目录
WORKDIR /app

# 复制依赖声明文件
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./

# 安装依赖，使用淘宝镜像源加速
RUN pnpm install --frozen-lockfile --prod=false --registry=https://registry.npmmirror.com

# 复制所有源代码
COPY . .

# 执行构建
RUN pnpm run build

# --- 第二阶段：创建 Nginx 运行镜像 ---
FROM nginx:1.25-alpine

# 移除默认的 Nginx 欢迎页面
RUN rm /etc/nginx/conf.d/default.conf

# 复制我们自定义的 Nginx 配置文件
COPY nginx.conf /etc/nginx/conf.d/

# 从构建阶段复制打包好的静态文件到 Nginx 的 web 根目录
# Vitesse build 默认输出到 dist 目录
COPY --from=builder /app/dist /usr/share/nginx/html

# 暴露 Nginx 监听的端口
EXPOSE 80

# 容器启动时执行的命令
ENTRYPOINT ["nginx", "-g", "daemon off;"]
