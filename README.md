<div align="center">

# 🚀 Prompt Craft v3

**三层 Prompt 架构的 AI 网页生成系统**

> 用户一句需求 → 设计方向 → 技术栈 → 可运行代码

[![Version](https://img.shields.io/badge/version-3.0.0-blue)](https://github.com/xcsweb/prompt-craft)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Made with AI](https://img.shields.io/badge/made%20with-AI%20%2B%20TRAE-purple)](#)

**[English Version](README.en.md) · 英文版在此**

</div>

---

## 📖 什么是 Prompt Craft？

Prompt Craft 是一个**结构化的 Prompt 工程系统**，它把用户模糊的需求转化为一致、高质量的网页应用。与依赖"一句神奇 Prompt"的方式不同，它采用**三层架构**（T1 项目规则 → T2 专家角色 → T3 任务模板）来产生可复用、可预期的输出。

### 它解决的问题

- ❌ AI 每次生成的风格都不一样，无法对齐团队审美
- ❌ 输出缺少设计系统（字体层级、颜色约束、排版节奏）
- ❌ 没有无障碍 / 移动端降级 / 性能预算的标准化流程
- ❌ 想"换个方向"时，所有 Prompt 要重写

### 怎么解决

```
                    用户说 "做一个官网"
                             │
    ┌────────────────────────┼────────────────────────┐
    │                        │                        │
    ▼                        ▼                        ▼
T1 · 项目规则           T2 · 专家角色           T3 · 任务模板
（硬约束：技术栈契约）  （AI 的思维人格）      （可复用的 Prompt 蓝图）
                         ┌───────────────────────┐
                         │   组合后的完整 Prompt    │
                         │  = T1 + T2 + T3 + 用户需求 │
                         └───────────┬─────────────┘
                                     ▼
                              可运行代码
                                     ▼
                            15 项自检清单
                                     ▼
                                 交付给用户
```

---

## 🎯 三层架构详解

### T1 · 项目规则（Project Rules）

每一次生成都必须遵守的"法律法规"，保证输出的一致性和质量下限。

| 类别 | 文件 | 作用 |
|------|------|------|
| 艺术化官网技术栈 | `project-rules/artistic-stack.md` | GSAP + Lenis + 字体系统 + 颜色预算 |
| Next.js + shadcn | `project-rules/nextjs-shadcn.md` | 生产级全栈脚手架 |
| Vue 3 + Vite + TS | `project-rules/vite-vue3-typescript.md` | 模块化 SPA 架构 |
| Node.js 后端 | `project-rules/node-express-prisma.md` | REST/GraphQL API 脚手架 |
| 通用安全规则 | `project-rules/shared-security.md` | OWASP 最佳实践 |

### T2 · 专家角色（Expert Personas）

代码背后的"思维"。决定 AI 的语气、优先级和质量标准。

| 角色 | 文件 | 适用场景 |
|------|------|----------|
| Creative Technologist（创意技术工程师） | `expert-personas/creative-technologist.md` | 艺术化 / Awwwards 级官网 |
| Senior Frontend Engineer（高级前端） | `expert-personas/senior-frontend-engineer.md` | 标准 Web 应用 |
| Senior Backend Engineer（资深后端） | `expert-personas/senior-backend-engineer.md` | API、数据库、基础设施 |
| Senior Fullstack Engineer（高级全栈） | `expert-personas/senior-fullstack-engineer.md` | 端到端功能开发 |
| Senior AI Agent Developer（AI 代理开发） | `expert-personas/senior-ai-agent-developer.md` | Agent 框架 / RAG / LLM 应用 |

### T3 · 任务模板（Task Templates）

可直接套用的 Prompt 蓝图，每个模板聚焦一类具体任务。

| 模板 | 文件 | 用途 |
|------|------|------|
| Build Artistic Website（生成艺术化官网） | `task-templates/build-artistic-website.md` | 品牌站 / 作品集 / 落地页 |
| Build Dashboard（生成数据大屏） | `task-templates/build-dashboard.md` | 数据可视化 / 管理后台 |
| Build Login Form（登录注册页） | `task-templates/build-login-form.md` | 鉴权 UI 流程 |
| Build CRUD API（生成 CRUD API） | `task-templates/build-crud-api.md` | REST/GraphQL 接口开发 |
| Debug Errors（调试错误） | `task-templates/debug-error.md` | 问题排查流程 |
| Write Tests（生成测试） | `task-templates/write-tests.md` | 测试代码生成 |
| Code Review（代码审查） | `task-templates/code-review.md` | PR Review Prompt |

### 参考资料库（Reference Library）

系统的"知识心脏"——让输出保持一致性的设计模式和交互签名。

| 参考 | 文件 | 内容 |
|------|------|------|
| **艺术化官网 Design DNA** | `references/artistic-website-dna.md` | ⭐ **3 大设计方向 × 15 个交互签名** |
| Awwwards 级官网指南 | `references/frontend-awwwards-website.md` | 高端站点的完整设计 → 代码指南 |
| 前端 Dashboard v2 | `references/frontend-dashboard-v2.md` | 管理后台 / 大屏模式 |
| 全栈 SaaS | `references/fullstack-saas.md` | SaaS 产品架构 |
| 实时数据可视化 | `references/dataviz-realtime.md` | 动态数据展示模式 |
| AI Agent / RAG / 聊天机器人 | `references/ai-agent.md` · `ai-rag.md` · `ai-chatbot.md` | AI 应用蓝图 |
| 后端：API / Auth / DB / MQ / Scheduler | 多个文件 | 后端模式库 |
| 系统架构指南 | `SYSTEM_PROMPT_ARCHITECTURE.md` | Prompt 系统设计文档 |

---

## 🎨 Design DNA 库（设计基因库）

### 3 大设计方向（每个方向 = 一套完整设计语言）

| 方向 | 颜色 | 字体 | 布局 | 适用行业 |
|------|------|------|------|----------|
| **A · 杂志博物馆风（Editorial）** | 奶油纸色 `#F5F3EF` + 近墨黑 + 赭石强调色（3 色预算） | Serif 大标题（Newsreader/Playfair）+ Sans 正文（Inter） · `clamp(3rem, 10vw, 9rem)` | 不对称双栏 · 巨量留白 14-20vh · 巨型编号分隔章节 | 设计工作室 / 文化机构 / 高端品牌 |
| **B · 赛博朋克沉浸风（Cyberpunk）** | 深黑 `#0A0A0F` + 近白文本 + 洋红 `#FF2E97` + 青 `#00F0FF` | Condensed 窄体几何 sans · 全大写宽字距标签 | Canvas 优先 · WebGL 铺满屏幕 · HUD 风格 UI 悬浮其上 | 游戏工作室 / 互动媒体 / 音乐厂牌 / 科幻发布 |
| **C · 趣味探索风（Playful）** | 天空/草地/橙色 + 签名红 | 圆润几何 sans · 等宽数字 · "钉在 3D 世界上"的文字 | 没有传统布局 —— 页面是一张用户驾驶探索的 3D 地图 | 个人作品集 / 教育 / 创意工具 |

### 15 个交互签名（每次选 3–5 个）

这是系统的"点菜菜单"——让输出真正"看起来像专业作品"的元素。

| 编号 | 签名 | 效果 |
|------|------|------|
| IS-01 | **Mask Reveal 遮罩入场** | 文字被遮罩裁剪，从下到上"翻开" |
| IS-02 | **SplitText Stagger 逐字入场** | 标题拆成单字，错峰出现 |
| IS-03 | **Scroll-driven Line Grow 滚动下划线生长** | 章节下划线随滚动进度生长 |
| IS-04 | **Custom Cursor + Magnetic 光标磁吸** | 隐藏原生光标，用圆环跟随；hover 按钮时圆环"吸"过去 |
| IS-05 | **Hover Invert hover 反色** | 卡片从白底翻成黑底，文字变白 |
| IS-06 | **Scroll-driven Shader 滚动驱动着色器** | Hero 的 WebGL shader 随滚动进度演化 |
| IS-07 | **Horizontal Scroll 横向滚动** | 纵向滚动触发 pin + 内容横向滑动 |
| IS-08 | **Sticky Reveal Narrative 粘性叙事** | 章节标题粘在顶部，滚动时内部内容切换 |
| IS-09 | **Shader Orb 发光球体** | 3D fresnel 光晕球体，响应鼠标 |
| IS-10 | **Canvas 2D Particles 粒子场** | 100–300 粒子悬浮，hover 时聚拢 |
| IS-11 | **SVG Noise Paper 纸张噪点** | 2–4% 透明度的 `feTurbulence` 模拟印刷质感 |
| IS-12 | **Tilt 3D Card 3D 倾斜卡片** | hover 时卡片做透视倾斜 |
| IS-13 | **Page Wipe 页面擦除** | 路由切换时全屏色块从右向左擦过 |
| IS-14 | **Long-form Editor Scroll 长文编辑滚动** | 优化的阅读体验 + 段落动效 |
| IS-15 | **Fixed Background Progress 固定背景随动** | 静态 Hero 随滚动进度微妙变形 |

> **完整技术细节**（GSAP 时间线、CSS 数值、缓动曲线）见 [`references/artistic-website-dna.md`](references/artistic-website-dna.md)

---

## 🖼️ 示例画廊

以下 6 个真实可运行的网站均由本系统生成。每个文件都是**单文件 HTML**——双击即可在浏览器打开，零构建步骤。

| 编号 | 名称 | 风格 | 行数 | 预览 | 源码 |
|------|------|------|------|------|------|
| 01 | Basic Official Site（基础官方站） | 简洁企业风 | ~700 | 打开 `index.html` | [`examples/01-basic-official-site/`](examples/01-basic-official-site/) |
| 02 | Standard Official Site v2（标准官方站） | 精致企业风 | ~570 | 打开 `index.html` | [`examples/02-standard-official-site-v2/`](examples/02-standard-official-site-v2/) |
| 03 | Advanced Animation Site（动画进阶站） | 重交互 / GSAP 动效 | ~570 | 打开 `index.html` | [`examples/03-advanced-animation-site/`](examples/03-advanced-animation-site/) |
| 04 | **HELIOS** | 杂志博物馆风（Editorial） | ~900 | 打开 `index.html` | [`examples/04-editorial-helios/`](examples/04-editorial-helios/) |
| 05 | **BRUTALIST** | Neo-Swiss 野兽派 / 黑底亮色 | ~1270 | 打开 `index.html` | [`examples/05-brutalist-swiss/`](examples/05-brutalist-swiss/) |
| 06 | Dashboard Data Viz（数据大屏） | KPI / 图表 / 深色主题 | ~510 | 打开 `index.html` | [`examples/06-dashboard-data-viz/`](examples/06-dashboard-data-viz/) |

每个示例都是不同"方向 × 签名"的组合。对比 HELIOS（方向 A · mask-reveal + magnetic-cursor + hover-invert）和 BRUTALIST（方向 A 变体 · canvas-spotlight + split-text + horizontal-scroll + block-hover-reveal），可以看到基因库如何带来多样化产出。

---

## 🚦 快速开始

### 方式 1 · 在 TRAE / Claude Code 中使用

1. 克隆或复制 `prompt-craft/` 到你的工作区
2. 以 `SKILL.md` 作为 Skill 入口文件
3. 对 AI 说：**"帮我做一个设计工作室官网"** —— 系统会自动组合 T1 + T2 + T3
4. 输出到指定目录。默认输出单文件 `index.html`（无需构建）

### 方式 2 · 作为 Prompt 脚手架使用（任何 LLM 均可）

1. 打开 [`SKILL.md`](SKILL.md) —— 380 行完整系统摘要
2. 以"生成艺术化官网"为例，组合以下文件：
   - `expert-personas/creative-technologist.md`（思维人格）
   - `project-rules/artistic-stack.md`（技术约束）
   - `task-templates/build-artistic-website.md`（Prompt 模板）
   - `references/artistic-website-dna.md`（设计菜单 —— 从 3 个方向和 15 个签名里选 3–5 个）
3. 将这 4 个文件一起喂给 LLM。架构保证了一致、高质量的输出。

### 方式 3 · 浏览 6 个示例站点

直接打开 `examples/*/index.html` 中的任意一个——无需安装。

---

## 📁 项目结构

```
prompt-craft/
├── SKILL.md                              ← ⭐ 系统入口（先读这个）
├── README.md                             ← 你正在看的文件
├── README.en.md                          ← 👋 English version
├── SYSTEM_PROMPT_ARCHITECTURE.md         ← 架构设计文档
├── .preheat                              ← （TRAE 内部文件）
│
├── project-rules/                        ← T1 · 项目级硬约束
│   ├── artistic-stack.md
│   ├── nextjs-shadcn.md
│   ├── vite-vue3-typescript.md
│   ├── node-express-prisma.md
│   └── shared-security.md
│
├── expert-personas/                      ← T2 · 专家角色 / AI 思维人格
│   ├── creative-technologist.md
│   ├── senior-frontend-engineer.md
│   ├── senior-backend-engineer.md
│   ├── senior-fullstack-engineer.md
│   └── senior-ai-agent-developer.md
│
├── task-templates/                       ← T3 · 任务模板 / Prompt 蓝图
│   ├── build-artistic-website.md
│   ├── build-dashboard.md
│   ├── build-login-form.md
│   ├── build-crud-api.md
│   ├── debug-error.md
│   ├── write-tests.md
│   └── code-review.md
│
├── plugins/                              ← 可选功能插件
│   ├── plugin-auth.md
│   ├── plugin-docker.md
│   └── plugin-i18n.md
│
├── references/                           ← 设计 DNA & 模式库 / 参考资料
│   ├── artistic-website-dna.md           ← ⭐ 核心：3 方向 × 15 签名
│   ├── frontend-awwwards-website.md
│   ├── frontend-dashboard-v2.md
│   ├── fullstack-saas.md
│   ├── ai-agent.md, ai-rag.md, ai-chatbot.md
│   ├── backend-api.md, backend-auth.md, backend-database.md
│   ├── backend-microservices.md, backend-mq.md, backend-scheduler.md
│   └── ...
│
└── examples/                             ← 6 个可运行的示例网站
    ├── 01-basic-official-site/
    ├── 02-standard-official-site-v2/
    ├── 03-advanced-animation-site/
    ├── 04-editorial-helios/
    ├── 05-brutalist-swiss/
    └── 06-dashboard-data-viz/
```

---

## ✅ 15 项 QA 自检清单

每次生成的网站都自动核对：

- [ ] 颜色预算 ≤ 4 色
- [ ] 字体使用 `clamp()` 含视口单位
- [ ] GSAP ScrollTrigger + Lenis 安装并激活
- [ ] 已实现 `prefers-reduced-motion` 降级
- [ ] ≤ 768px 移动端断点 + 简化动画
- [ ] ≥ 3 个来自 DNA 库的交互签名
- [ ] WebGL 有 Canvas 2D / 静态 SVG 回退方案
- [ ] 字体加载完成后（`document.fonts.ready`）才触发动画（避免 FOUT 跳帧）
- [ ] 无用户请求时不注入外部 cookie / 统计
- [ ] `:root` 中定义 CSS 设计 token（`--bg`, `--text`, `--accent`, `--line`）
- [ ] 缓动：`power3.out` / `power4.out`（禁用 `linear` / `ease-in-out`）
- [ ] 保留键盘焦点环
- [ ] 代码完整，无 `// TODO` / `// ...` 占位符
- [ ] 单文件原型：单个 `index.html` / 无构建步骤
- [ ] 离线可用（资源通过 CDN 或内联 —— 无私有域名依赖）

---

## 📜 License

MIT —— 自由使用、修改、发布。如果帮你做出了有趣的项目，欢迎给个 ⭐ Star。

---

<div align="center">

**Prompt Craft 的目标很简单：**

> 从「希望这次 AI 能搞对」，变成「我知道它会产出什么——而且能讲清楚为什么」。

</div>
