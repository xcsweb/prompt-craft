# 插件模块：🌐 i18n 国际化 v1.0

> 适用：需要多语言支持的前端/全栈应用
> 默认语言：中文（zh-CN）+ 英文（en-US）
> 语言文件格式：JSON / YAML

---

## 📦 需要添加的内容

### 1. 目录结构

```
src/
├── locales/
│   ├── zh-CN.json     # 中文
│   ├── en-US.json     # 英文
│   └── index.ts       # 配置文件
├── composables/
│   └── useI18n.ts     # (Vue) 组合式函数
└── components/
    └── LangSwitcher.vue # 语言切换组件
```

### 2. 翻译文件格式（JSON）

```json
// zh-CN.json
{
  "common": {
    "confirm": "确认",
    "cancel": "取消",
    "save": "保存",
    "delete": "删除"
  },
  "dashboard": {
    "title": "数据看板",
    "totalSales": "销售总额",
    "orderCount": "订单数量"
  },
  "errors": {
    "network": "网络连接异常，请重试",
    "notFound": "页面未找到"
  }
}
```

```json
// en-US.json
{
  "common": {
    "confirm": "Confirm",
    "cancel": "Cancel",
    "save": "Save",
    "delete": "Delete"
  },
  "dashboard": {
    "title": "Dashboard",
    "totalSales": "Total Sales",
    "orderCount": "Order Count"
  },
  "errors": {
    "network": "Network error, please retry",
    "notFound": "Page not found"
  }
}
```

### 3. 核心功能

- **语言切换**：用户可选择语言，偏好保存在 localStorage
- **浏览器语言检测**：首次访问根据 navigator.language 自动选择
- **嵌套 key 支持**：`t('dashboard.title')` 支持点号分隔
- **变量插值**：`t('welcome', { name: 'Alice' })` → "欢迎 Alice"
- **复数支持**：`t('items', { count: 5 })` → "5 个项目"

### 4. 推荐库

- **Vue 项目**：vue-i18n（官方方案）
- **React 项目**：react-i18next（成熟生态）
- **轻量方案**：自己实现 ~50 行代码的简易 i18n（无需额外依赖）

---

## ✅ 最佳实践

- [ ] Key 使用点号分层，如 `dashboard.title` 而非 `dashboardTitle`
- [ ] 翻译内容包含完整句子，避免拼接（不同语言语法不同）
- [ ] 日期、数字、货币使用对应语言的格式化库（如 Intl API）
- [ ] 避免在翻译中混合 HTML（除非有 XSS 防护）
- [ ] 提供语言切换 UI，通常放在 Header 右侧
- [ ] 第一次加载只加载当前语言文件，按需加载其他语言

---

## 🚫 反模式

- ❌ 不要在代码中硬编码非翻译语言的字符串
- ❌ 不要把多个句子拼接成一个字符串（不同语言词序不同）
- ❌ 不要用数组索引或位置相关的硬编码翻译
- ❌ 不要把翻译 key 命名为 `text1`, `text2`（无业务含义）
