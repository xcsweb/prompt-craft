# Node.js · Express / Fastify · Prisma — 后端项目规则

> 适用场景：独立的 REST API 服务。放置在 `.cursor/rules/01-node-express-prisma.mdc` 或 `.github/copilot-instructions.md`

---

## 🔧 技术栈（Stack）

- Runtime: Node.js 20+ LTS
- Language: TypeScript 5.5+ with strict mode
- Framework: Fastify (preferred) or Express 4
- ORM: Prisma 5 with PostgreSQL (MySQL only if explicitly requested)
- Auth: JWT via `jsonwebtoken` library + `bcrypt` / `argon2` for passwords
- Validation: Zod schemas — used at API boundary AND internally
- Logging: Pino (structured JSON logging). Never `console.log` in production code paths.
- Configuration: `zod-config` or typed env validation — never `process.env.X` directly without validation
- Testing: Vitest for unit tests, Supertest for integration, Testcontainers for real DB in tests
- Container: Dockerfile with multi-stage build. docker-compose.yml for local dev.

## 🏗️ 分层架构（Layered Architecture）

```
src/
├── index.ts                  # App bootstrap: wires everything together, starts HTTP server
├── app.ts                    # Fastify/Express app creation + middleware registration
├── config/                   # Typed configuration (env vars → validated config object)
│   └── index.ts
├── routes/                   # API route definitions — parse request → call service → return response
│   ├── users/
│   │   └── users.route.ts    # Route handlers ONLY. NO business logic.
│   ├── orders/
│   │   └── orders.route.ts
│   └── index.ts              # Route registration
├── services/                 # Business logic — the heart of the application
│   ├── user.service.ts       # Pure business logic. No HTTP awareness.
│   └── order.service.ts
├── repositories/             # Data access layer (Prisma queries centralized here)
│   ├── user.repository.ts
│   └── order.repository.ts
├── middleware/               # Express/Fastify middleware
│   ├── auth.middleware.ts    # JWT verification, role checks
│   └── validation.middleware.ts
├── schemas/                  # Zod validation schemas (shared by routes + types)
│   ├── user.schema.ts
│   └── order.schema.ts
├── types/                    # Shared DTOs / types
│   ├── user.dto.ts
│   └── common.ts
├── errors/                   # Custom error hierarchy
│   ├── app.error.ts          # Base class AppError extends Error
│   └── not-found.error.ts    # class NotFoundError extends AppError
├── utils/                    # Pure utility functions (formatting, date, string utils)
└── logger.ts                 # Pino logger instance — single source

prisma/
└── schema.prisma             # Data model
```

## 🔑 关键分层原则（Layer Discipline）

### Routes 层（"薄"）

```typescript
// ✅ GOOD — route only: parse, validate, call service, shape response
async function createUser(request: FastifyRequest, reply: FastifyReply) {
  const input = UserCreateSchema.parse(request.body);    // validation
  const user = await userService.create(input);           // business logic
  return reply.status(201).send(UserResponseSchema.parse(user)); // response shaping
}

// ❌ BAD — business logic in route
app.post('/users', (req, res) => {
  const user = await prisma.user.create({ data: req.body }); // No validation! Inline Prisma!
  res.json(user); // Returns raw database entity — exposes internals!
});
```

### Services 层（"厚"）

- **纯业务逻辑**。无 HTTP，无 Prisma，无 req/res。
- **可独立单元测试**。依赖通过构造函数注入（Repositories / external service clients）。
- **明确抛出 typed errors**（`NotFoundError`、`ValidationError`、`ForbiddenError`）。上层（route）统一映射到 HTTP 状态码。

```typescript
export class UserService {
  constructor(private readonly users: UserRepository) {}

  async create(input: UserCreateInput): Promise<User> {
    const existing = await this.users.findByEmail(input.email);
    if (existing) throw new ConflictError('Email already registered');

    const passwordHash = await hash(input.password, 12);
    return this.users.create({ ...input, password: passwordHash });
  }
}
```

### Repositories 层（"隔离外部依赖"）

- 只有这一层知道 Prisma / SQL / MongoDB。其他层不知道数据是怎么存的。
- 每个方法都是单一数据操作，不做业务判断。

```typescript
export class PrismaUserRepository {
  constructor(private readonly prisma: PrismaClient) {}
  findByEmail(email: string) { return this.prisma.user.findUnique({ where: { email } }); }
  create(data: UserCreateInput) { return this.prisma.user.create({ data }); }
}
```

## 🚦 API 设计（API Design）

```
POST   /api/v1/users       → create user   → 201 + created resource
GET    /api/v1/users/:id   → get by ID     → 200 or 404
GET    /api/v1/users       → list (pageable) → 200 { data: [], total: N, page: 1, limit: 20 }
PATCH  /api/v1/users/:id   → partial update → 200 (NOT PUT unless full replacement semantic)
DELETE /api/v1/users/:id   → delete        → 204 No Content (NOT 200 with body)

Response envelope for all endpoints:
{
  "data": T,                 // only on success
  "error": null | {          // only on error
    "code": "VALIDATION_FAILED",
    "message": "Invalid input",
    "details": [{ field: "email", message: "Invalid email" }]
  }
}
```

## ✅ Prisma Schema 规范

```prisma
// 每个表至少：id (UUID/cuid)、createdAt、updatedAt
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String
  password  String   // NEVER store plain text — hashed by service layer
  role      Role     @default(USER)
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

enum Role { USER ADMIN }
```

## 🔐 认证实现（Auth Implementation）

```typescript
// Login flow — correct order:
// 1. Find user by email (constant-time compare not needed if using bcrypt for pwd)
// 2. bcrypt.compare(password, user.passwordHash)
// 3. Issue access + refresh tokens
// 4. Access token: short-lived (15m), in Authorization: Bearer header
// 5. Refresh token: longer-lived (7d), in HttpOnly Secure SameSite=strict cookie
// NEVER store JWT in localStorage!
// NEVER put sensitive data in JWT payload (JWT is signed, NOT encrypted)
```

## 🛡️ 安全规则（Security Checklist for every PR）

- [ ] **所有用户输入通过 Zod schema 验证**。req.body / req.query / req.params 都是不可信的。
- [ ] **SQL 注入防护**：全部通过 Prisma。零手写字符串拼接 SQL。
- [ ] **密码**：bcrypt cost=12 或 argon2id。明文密码不出现在日志/错误消息中。
- [ ] **JWT**：`HS256` 单服务可接受，微服务或需要密钥轮换用 `RS256`（非对称）。
- [ ] **速率限制**：`/login` 和 `/register` 做 IP 级别 rate limit（推荐 10/min）。
- [ ] **CORS**：显式白名单 origin，不要用 `*` + allow credentials 的组合。
- [ ] **日志**：Pino 结构化 JSON。不记录密码、token、信用卡号。
- [ ] **环境变量**：启动时用 Zod 验证 `DATABASE_URL`、`JWT_SECRET` 等。缺失则立即报错退出。

## 🐛 统一错误处理（Global Error Handler）

```typescript
app.setErrorHandler((error, request, reply) => {
  if (error instanceof AppError) {
    return reply.status(error.statusCode).send({
      error: { code: error.code, message: error.message, details: error.details }
    });
  }
  logger.error({ err: error }, 'Unhandled error');
  return reply.status(500).send({
    error: { code: 'INTERNAL_ERROR', message: 'Something went wrong' }
  });
});
```

## 🧪 测试提示（Testing Approach）

- **Unit tests**: Services → 注入 mock repository → 测纯业务逻辑
- **Integration tests**: Routes + real DB (Testcontainers / separate test DB)
- **Never test controllers with real Prisma + real auth** — test the layer independently
- Example test file path: `tests/unit/user.service.test.ts`

## 🚢 Dockerfile 规范

```dockerfile
# Builder
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npx prisma generate
RUN npm run build

# Runner
FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV production
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package.json ./
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

---

## 文件放置建议

**Cursor 用户：**
```
.project/.cursor/rules/01-node-express-prisma.mdc
或
.cursor/rules/01-node-express-prisma.mdc
```

**GitHub Copilot 用户：**
```
.github/copilot-instructions.md
```
