# 项目快速启动（精简版）

提供两种方式：
- 方法一：Docker 一键启动（推荐）
- 方法二：本地分别启动（前端/后端独立调试）

## 方法一：Docker 一键启动（推荐）

一次性启动前端、后端、数据库和 Redis。确保已安装 Docker 与 Docker Compose。

```bash
docker compose up -d --wait
```

启动后访问：
- 管理后台：http://localhost:80
- 博客前台：http://localhost:81
- 后端 API：http://localhost:8080

数据库/中间件（由 Docker 启动）：
- MySQL：localhost:3307（库：`blog_system`，用户：`root`，密码：`root`）
- Redis：localhost:6379

常用命令：
```bash
# 查看运行状态与日志
docker compose ps
docker compose logs -f

# 停止并清理（含卷）
docker compose down -v
```

## 方法二：本地开发分别启动

### 1. 后端 API（RuoYi）
```bash
cd ./api/RuoYi-Vue
mvn -B -DskipTests -pl ruoyi-admin -am package
java -jar ruoyi-admin/target/ruoyi-admin.jar
```
默认端口：http://localhost:8080  
提示：如需使用 Docker 的 MySQL/Redis，请在配置中指向 `localhost:3307` 与 `localhost:6379`。

### 2. 管理后台（RuoYi-Vue3）
```bash
cd ./admin/RuoYi-Vue3
yarn config set registry https://registry.npmmirror.com
yarn install
yarn dev
```
默认端口：http://localhost:5173

### 3. 博客前台（my-vitesse-app）
```bash
cd ./web/my-vitesse-app
pnpm i
pnpm dev
```
默认端口：http://localhost:3333

## 默认账号（仅开发环境）
- 管理后台：`admin` / `admin123`（请在生产环境修改）