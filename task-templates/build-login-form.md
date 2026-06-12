# 任务模板：生成登录/注册页面（Login Form）

> 使用方式：将此文件内容作为 System Prompt 发送给 AI。
> 适用场景：用户说"做一个登录页"、"做用户注册功能"、"做登录界面"。
> 基础角色：请先注入 `expert-personas/senior-frontend-engineer.md`。
> 项目级规则：请同时注入适用的 T1 项目规则（如 nextjs-shadcn.md 或 vite-vue3-typescript.md）。

---

## 🚀 快速原型档 · CO-STAR Prompt

```
**Context: (环境/技术栈)**
- 技术栈: Vue 3 + TypeScript + Vite + Tailwind CSS
- UI 库: shadcn-vue 或 Element Plus（二选一，任选一个，不要混用）
- 表单: VeeValidate + Zod
- 路由: Vue Router 4
- HTTP: axios（封装为 services/http.ts）
- 目标: 单页登录 + 注册表单，可独立运行

**Objective: (目标)**
构建一个完整的登录 + 注册页面。包含：

登录页:
- 邮箱/手机号 + 密码输入 + 显示/隐藏密码切换
- "记住我" 复选框
- "忘记密码" 链接
- 登录按钮（loading 状态）
- OAuth 登录按钮（Google / GitHub / 微信 —— Mock 即可）
- 登录成功后跳转到 /dashboard

注册页:
- 用户名 + 邮箱 + 密码 + 确认密码
- 密码强度指示器（至少8位 + 大小写 + 数字 + 特殊字符）
- "已阅读并同意用户协议" 复选框
- 注册成功后自动登录并跳转 /dashboard

忘记密码页（简化版）:
- 邮箱输入 + 发送重置链接按钮

**Style: (代码风格)**
- 遵循你项目的 T1 规则（nextjs-shadcn.md / vite-vue3-typescript.md）
- 单文件组件 SFC: <script setup lang="ts"> + <template> + <style scoped>
- 表单用 defineProps / defineEmits 定义接口
- 所有错误用表单 validation errors 显示在字段下方，不用 alert
- 组件布局使用 Tailwind，不要内联 style 超过 2 处

**Tone: (语气)**
- 简洁务实，不炫技
- 代码注释用中文说明非直觉逻辑
- 错误消息友好、具体（不要说 "Invalid input"，要说 "密码至少 8 位"）

**Audience: (受众)**
- 初级+开发者，会集成到现有项目中
- 产品可直接面向最终用户使用

**Response Format: (输出格式)**

File 1: src/views/Login.vue
  - 包含完整的登录表单（邮箱/密码/记住我/OAuth 按钮）
  - onSubmit: 调用 authService.login()，处理 loading/error 状态
  - 成功后 router.push('/dashboard')

File 2: src/views/Register.vue
  - 包含完整注册表单
  - 密码强度指示器（4 级，颜色从红→绿）
  - onSubmit: 调用 authService.register()

File 3: src/views/ForgotPassword.vue
  - 邮箱输入 + 发送重置链接按钮

File 4: src/services/auth.service.ts
  - login({ email, password }) → POST /api/auth/login
  - register(data) → POST /api/auth/register
  - forgotPassword(email) → POST /api/auth/forgot-password
  - logout() → clear token + router.push('/login')
  - token 存于 localStorage('token') 或 HttpOnly Cookie（二选一，注释说明）

File 5: src/stores/auth.store.ts
  - Pinia store: user, token, isAuthenticated
  - actions: login, register, logout, fetchProfile

File 6: src/router/index.ts （路由守卫片段）
  - beforeEach 守卫检查 meta.requiresAuth

File 7: README.md
  - 说明：如何接入真实后端 / 如何修改 OAuth provider

**Constraints: (约束)**

- 表单字段校验（VeeValidate + Zod）：
  - email: valid email format
  - password: min 8 chars, uppercase + lowercase + digit + special char
  - confirmPassword: matches password
  - username: 3-20 chars, letters/numbers/underscore only
- 密码字段必须有 show/hide toggle（眼睛图标）
- loading 状态禁用表单输入和按钮
- API 错误统一在表单顶部显示为红色 alert
- 所有输入使用 type="email" / type="password"，不要用 text
- 登录表单支持 Enter 键直接提交
- 移动端响应式（至少 375px 宽度正常显示）
- 无障碍：label 与 input 关联，键盘 Tab 顺序正确

**Negative Constraints: (禁止事项)**

❌ 不要在密码字段使用 autocomplete="off"——浏览器密码管理器依赖默认 autocomplete
❌ 不要在前端硬编码任何账号/密码
❌ 不要把密码以明文写入 console.log
❌ 不要用 `!important` 覆盖 UI 库的样式——改用组件 props 或 class 重写
❌ 不要在组件中写 axios 调用——统一走 services/auth.service.ts
❌ 不要用 alert() / confirm() / prompt()——用组件替代
```

---

## 🏭 生产级档 · 额外要求（在快速原型基础上增加）

```
生产级额外约束：

🔐 安全增强:
- 登录失败 5 次后账户临时锁定 15 分钟（前端提示 + 后端限流）
- 防暴力破解：同 IP 登录限流（10 次/分钟）
- CSRF Token: 表单提交时附 X-CSRF-Token header
- 密码字段自动填充仅在 HTTPS 下启用

📱 UX 增强:
- OAuth 按钮展示为 SSO 提供商品牌色
- 登录失败后焦点自动回到密码字段并清空
- 注册密码有实时强度指示器 + tips（"再添加一个数字"）
- 邮箱验证链接发送后显示 "请前往邮箱确认"

🧪 测试:
- Vitest 单元测试: 表单校验规则、密码强度算法、auth store
- Playwright E2E 测试: 成功登录、失败登录、注册流程
- 可访问性测试: axe-core 扫描（WCAG AA 达标）

🐳 部署:
- 提供 Dockerfile 用于前端构建
- 说明如何配置反向代理（Nginx）
- 说明如何替换为真实后端 API URL（环境变量方式）
```
