# 插件模块：🐳 Docker 部署 v1.0

> 适用：所有后端服务、全栈应用、需要容器化部署的前端项目
> 使用方式：在生成代码时，将本插件内容附加到提示词中

---

## 📦 需要添加的文件

### 1. Dockerfile（多阶段构建）

要求：
- 使用多阶段构建减少最终镜像大小
- Builder 阶段：安装依赖、构建
- Runner 阶段：仅复制必要文件，使用轻量基础镜像
- 目标镜像大小 < 200MB（Node.js 项目）

Node.js 项目示例结构：
```dockerfile
# Builder 阶段
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production || npm install --only=production
COPY . .
RUN npm run build

# Runner 阶段
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package.json ./
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

### 2. .dockerignore

必须包含：
```
node_modules
.git
.gitignore
.vscode
.idea
dist
build
coverage
npm-debug.log
yarn-error.log
*.md
!README.md
.env
.env.local
.env.*.local
```

### 3. docker-compose.yml

要求：
- 包含 app 服务 + 依赖服务（如 PostgreSQL、Redis）
- 有健康检查配置（healthcheck）
- 数据持久化使用 volumes
- 使用 environment 或 env_file 传递配置
- 合理的 restart 策略（unless-stopped）

示例结构：
```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: production
      DATABASE_URL: postgresql://user:pass@postgres:5432/app
    depends_on:
      postgres:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    restart: unless-stopped

  postgres:
    image: postgres:15-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: app
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

### 4. .env.example

提供环境变量模板，不要包含真实密钥：
```env
# 服务器配置
NODE_ENV=production
PORT=3000

# 数据库
DATABASE_URL=postgresql://user:password@localhost:5432/app

# Redis（如使用）
REDIS_URL=redis://localhost:6379

# 认证
JWT_SECRET=change-me-in-production

# 第三方服务（如需要）
# API_KEY=your-api-key-here
```

---

## ✅ Docker 最佳实践检查清单

- [ ] 使用多阶段构建（Multi-stage builds）
- [ ] 使用 alpine 或 slim 版本的基础镜像
- [ ] 精确指定版本号（node:20-alpine 而非 node:alpine）
- [ ] 合理的 .dockerignore，避免复制 node_modules 和 .git
- [ ] 先复制 package.json 再复制源码（利用缓存层）
- [ ] 使用非 root 用户运行应用（生产级）
- [ ] HEALTHCHECK 指令（或 docker-compose healthcheck）
- [ ] EXPOSE 端口声明
- [ ] 环境变量而非硬编码配置
- [ ] 日志输出到 stdout/stderr（不写入文件，便于 Docker 日志管理）

---

## 🚫 Docker 反模式

- ❌ 不要在镜像中包含开发依赖（devDependencies）
- ❌ 不要将 .env 文件打包进镜像
- ❌ 不要在 Dockerfile 中硬编码密钥或 API Key
- ❌ 不要使用 latest 标签的基础镜像（不可复现）
- ❌ 不要在一个容器中运行多个服务（如同时跑 Node 和 Nginx）

---

## 📝 额外建议

- 如项目包含前端静态文件，可考虑 Nginx 作为第二阶段
- 生产环境可添加容器资源限制（cpus, mem_limit）
- 如需要热重载开发模式，可额外提供 docker-compose.dev.yml
