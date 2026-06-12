# Vite · Vue 3 · TypeScript — 项目规则

> 适用场景：现代 SPA 前端项目（非 SSR）。放置在 `.cursor/rules/01-vite-vue3.mdc` 或 `.github/copilot-instructions.md`

---

## 🔧 技术栈（Stack）

- Build: Vite 5
- Framework: Vue 3.4+ with Composition API + `<script setup>`
- Language: TypeScript 5.5+ with strict mode
- State: Pinia for stores (NOT Vuex)
- Router: Vue Router 4
- UI Library: Element Plus or Ant Design Vue (pick ONE, do NOT mix)
- Forms: VeeValidate + zod OR vee-validate / vuelidate
- Icons: @element-plus/icons-vue or lucide-vue-next
- HTTP: axios with interceptors for auth + error handling
- Testing: Vitest for unit tests, Playwright for E2E

## 🏗️ 目录结构（Directory Structure）

```
src/
├── assets/              # Images, fonts, global styles
├── components/          # Reusable UI components (PascalCase filenames)
│   ├── common/          # Cross-feature reusable components
│   └── ui/              # Base/wrapper components
├── views/               # Route-level pages (matched by router)
├── layouts/             # Layout wrappers (DefaultLayout, AuthLayout, etc.)
├── stores/              # Pinia stores (one per domain)
├── composables/         # Vue composables (useAuth, useTable, usePagination, etc.)
├── router/              # Vue Router definition + route guards
├── services/            # API service layer (axios instances, endpoints)
├── api/                 # API endpoint definitions (optional — alternative to services/)
├── utils/               # Pure utility functions (formatDate, debounce, etc.)
├── types/               # Shared TypeScript interfaces/enums
├── directives/          # Custom Vue directives (v-click-outside, etc.)
└── main.ts              # App bootstrap

tests/                   # Vitest test files (parallel to src structure)
```

### 组件示例结构（Correct SFC structure）

```vue
<script setup lang="ts">
// 1. Imports
import { ref, computed, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import { Button, Form, Input } from 'element-plus';

// 2. Props & Emits (defineProps / defineEmits with TypeScript types)
interface Props {
  title: string;
  initialCount?: number;
}
const props = withDefaults(defineProps<Props>(), { initialCount: 0 });

const emit = defineEmits<{
  (e: 'update', value: number): void;
  (e: 'save'): void;
}>();

// 3. Reactive state (ref for primitives, reactive for objects)
const count = ref(props.initialCount);
const loading = ref(false);
const router = useRouter();

// 4. Computed
const doubled = computed(() => count.value * 2);

// 5. Methods
function increment() {
  count.value++;
  emit('update', count.value);
}
async function save() {
  loading.value = true;
  try {
    // await api call
    emit('save');
  } finally {
    loading.value = false;
  }
}

// 6. Lifecycle
onMounted(() => {
  console.log('Component mounted');
});
</script>

<template>
  <div class="p-4">
    <h2 class="text-xl font-bold">{{ props.title }}</h2>
    <p>Count: {{ count }} (doubled: {{ doubled }})</p>
    <el-button @click="increment" :loading="loading">Increment</el-button>
  </div>
</template>

<style scoped lang="scss">
/* scoped styles only. Use Tailwind utilities first. */
</style>
```

## 📝 关键代码规范（Essential Conventions）

- **`<script setup>` 是默认方式**。不要使用 `setup()` 函数语法。
- **单文件中 section 顺序**：`<script setup>` → `<template>` → `<style scoped>`。
- **Props 使用 TypeScript 泛型定义**：`defineProps<Props>()`，不要用运行时对象。
- **Emits 使用 TypeScript 泛型定义**：`defineEmits<{ (e: 'name', payload: Type): void }>()`。
- **ref vs reactive**：简单值用 `ref`，对象用 `reactive` 或直接 `ref<ObjType>()`。
- **`computed` 永远用于派生状态**，不要在模板中做复杂计算。
- **命名：** 组件文件 PascalCase（`UserProfile.vue`），composables 和 utils 用 camelCase（`useAuth.ts`）。

## 🔌 API 服务层（API Service Layer）

```typescript
// src/services/http.ts
import axios, { AxiosInstance, AxiosError } from 'axios';

const http: AxiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
  timeout: 15000,
  headers: { 'Content-Type': 'application/json' },
});

// Request interceptor — attach auth token
http.interceptors.request.use((config) => {
  const token = localStorage.getItem('token');
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Response interceptor — global error handling
http.interceptors.response.use(
  (response) => response,
  (error: AxiosError) => {
    if (error.response?.status === 401) {
      localStorage.removeItem('token');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  },
);

export default http;

// src/services/user.api.ts
import http from './http';

export interface User { id: string; email: string; name: string; }

export const userApi = {
  getProfile: () => http.get<User>('/api/users/me').then(r => r.data),
  updateProfile: (data: Partial<User>) => http.put<User>('/api/users/me', data).then(r => r.data),
};
```

## 🍍 Pinia Store 规范

```typescript
// stores/user.ts
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';
import { userApi, type User } from '@/services/user.api';

export const useUserStore = defineStore('user', () => {
  // State — use refs, NOT reactive objects (harder to type-safely update)
  const user = ref<User | null>(null);
  const loading = ref(false);

  // Getters as computed
  const isLoggedIn = computed(() => !!user.value);
  const displayName = computed(() => user.value?.name ?? 'Guest');

  // Actions
  async function fetchProfile() {
    loading.value = true;
    try {
      user.value = await userApi.getProfile();
    } finally {
      loading.value = false;
    }
  }
  function logout() {
    user.value = null;
    localStorage.removeItem('token');
  }

  return { user, loading, isLoggedIn, displayName, fetchProfile, logout };
});
```

## 🔐 认证与路由守卫（Auth & Route Guards）

```typescript
// router/index.ts
const routes = [
  { path: '/login', component: () => import('@/views/Login.vue'), meta: { requiresAuth: false } },
  { path: '/dashboard', component: () => import('@/views/Dashboard.vue'), meta: { requiresAuth: true } },
];

router.beforeEach((to) => {
  const isAuth = !!localStorage.getItem('token');
  if (to.meta.requiresAuth && !isAuth) return { path: '/login', query: { redirect: to.fullPath } };
  if (to.path === '/login' && isAuth) return '/dashboard';
});
```

## 💅 样式约定（Styling）

- Tailwind CSS 优先。`<style scoped>` 仅用于 Tailwind 无法表达的复杂选择器或动画。
- 不要使用全局 `<style>`（无 scoped），除非是应用根组件且明确需要全局样式。
- Element Plus 自定义主题：通过 `element-plus/theme-chalk/src/mixins/config.scss` 或 CSS 变量。

## 🛡️ 共享安全规则

请参考 `shared-security.md`。**前端特定补充：**

- `v-html` 必须配合 DOMPurify 使用，绝不能直接渲染用户输入。
- `:href` 用户提供的 URL 需要白名单校验（只允许 http/https/mailto，禁止 `javascript:`）。
- 不要在 `localStorage` 存储敏感数据。Auth token 是可接受的最低限度。
- `import.meta.env` 中只有 `VITE_*` 前缀变量会暴露给客户端。绝不要把后端密钥放进去。

---

## 文件放置建议

**Cursor 用户：**
```
.project/.cursor/rules/01-vite-vue3.mdc
或
.cursor/rules/01-vite-vue3.mdc
```

**GitHub Copilot 用户：**
```
.github/copilot-instructions.md
```
