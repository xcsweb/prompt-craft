# 插件模块：🔐 用户认证 v1.0

> 适用：所有需要登录/权限控制的 Web 应用
> 默认方案：JWT（JSON Web Token）+ HTTP-only Cookie

---

## 📦 需要添加的内容

### 1. 核心功能清单

- 用户注册（邮箱/手机号 + 密码）
- 用户登录 / 登出
- 密码重置（通过邮箱验证码）
- Token 自动刷新（Access Token 15min + Refresh Token 7天）
- 权限检查（路由守卫 + 按钮级权限）
- 当前用户信息获取（从 token 或 API）

### 2. 推荐技术方案

**A方案：前后端分离 + JWT**
- 后端生成 JWT Access Token（短，15min）+ Refresh Token（长，7天）
- Access Token 在内存中（Vuex/Pinia/Zustand store）
- Refresh Token 存储在 HTTP-only Secure Cookie 中
- Access Token 过期时自动用 Refresh Token 换新

**B方案：第三方认证服务**
- 使用 NextAuth.js（Next.js）或 Auth0 / Clerk
- 适合不想自己实现认证的快速项目

### 3. 路由权限模式

```javascript
// 路由配置示例（Vue Router）
const routes = [
  { path: '/login', meta: { requiresAuth: false } },
  { path: '/dashboard', meta: { requiresAuth: true, roles: ['admin', 'user'] } },
  { path: '/admin', meta: { requiresAuth: true, roles: ['admin'] } }
];

// 路由守卫
router.beforeEach((to, from, next) => {
  const isAuth = checkAuth();
  if (to.meta.requiresAuth && !isAuth) return next('/login');
  if (to.meta.roles && !hasRole(to.meta.roles)) return next('/403');
  next();
});
```

### 4. API 请求拦截

```javascript
// axios 拦截器示例
axios.interceptors.request.use(config => {
  const token = getToken();
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

axios.interceptors.response.use(
  res => res,
  async error => {
    if (error.response?.status === 401) {
      // 自动刷新 token 或跳转到登录
      await refreshToken();
      return axios.request(error.config);  // 重试原请求
    }
    return Promise.reject(error);
  }
);
```

---

## ⚠️ 安全要点（必须遵守）

- [ ] 密码使用 bcrypt/argon2 哈希存储（cost >= 12）
- [ ] JWT 使用 RS256（非对称密钥）而非 HS256
- [ ] Token 存储在 HTTP-only + Secure + SameSite Cookie 中
- [ ] 登录失败次数限制（防止暴力破解）
- [ ] 敏感操作（修改密码/绑定手机）需要重新验证身份
- [ ] 支持 Token 吊销（Redis 黑名单机制）
- [ ] 登录页面做速率限制（1分钟最多 5 次失败尝试）

---

## 🚫 反模式

- ❌ 不要在 localStorage 中存储 JWT（XSS 攻击可窃取）
- ❌ 不要在 JWT payload 中存放敏感信息（payload 仅 base64，非加密）
- ❌ 不要在前端硬编码任何密钥或 secret
- ❌ 不要使用 MD5 / SHA1 等弱哈希算法存密码
- ❌ 不要信任前端传来的权限校验结果——后端必须做二次校验
