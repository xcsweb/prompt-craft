# T1 项目级规则：艺术化官网技术栈

> 适用于：品牌官网、设计工作室官网、沉浸式落地页、滚动叙事体验。
> 基于 Awwwards 2023-2025 获奖项目的逆向研究（Active Theory / Immersive Garden / Locomotive / Bruno Simon / Meridian / Blue Desert）。

---

## 核心技术栈

```
FRONTEND
├─ 原生 HTML + CSS + ES Modules (快速原型)
│   或 Vue 3 + Vite (生产级项目)
│   或 Next.js 14 App Router (如需 CMS/SEO)
│
├─ 动画与滚动
│   ├─ gsap@3.x                — 所有时间线动画
│   │   └─ ScrollTrigger Plugin — 滚动驱动动画
│   └─ @studio-freight/lenis   — 平滑滚动（替换原生滚动）
│
├─ 视觉（WebGL / Canvas）
│   ├─ three.js                — 3D 几何、场景、复杂着色器
│   ├─ ogl@0.x                 — 更轻的 WebGL 替代品（首选）
│   └─ Canvas 2D API           — 简单粒子、线条、SVG 手绘
│
└─ 字体
    ├─ Google Fonts             — Inter (正文) / Newsreader (展示)
    └─ 或自建 font family       — 永远在 `<head>` 用 `preconnect` + `font-display: swap`

NO BUILD TOOLS 模式（快速原型）
└─ 通过 ESM.sh / unpkg CDN 直接 import { gsap } from 'https://esm.sh/gsap'
```

---

## 颜色系统约定

```
3 色原则（所有艺术官网的通用约束）：

1. --bg:       背景色（非纯黑/纯白，带轻微色偏）
2. --text:     主文本色（与背景 ≥ 7:1 对比度）
3. --accent:   品牌色（占比 5%-15%，在 hover / CTA / 关键装饰中出现）
4. --muted:    次要文本色（第 4 色可加，占比<10%，通常是灰色调）

示例配色板（每种方向一组）：

Editorial 杂志风：
  bg: #F5F3EF (奶油纸色)   text: #1A1A1A (墨黑)   accent: #E0512B (赭石橙)

Cyberpunk 霓虹风：
  bg: #0A0A0F (深夜空)     text: #F5F5F5 (近白)   accent: #FF2E97 (洋红霓虹)
  accent2: #00F0FF (青)

Minimal 极简品牌风：
  bg: #FFFFFF               text: #1A1A1A          accent: #2E2E2E (深灰)
```

---

## 字体系统约定

```
方案 A — Editorial 杂志 / 博物馆：
  Display (标题): serif    → Newsreader / Playfair Display / Cormorant Garamond
  Body (正文):    sans     → Inter / Helvetica Neue

方案 B — Tech 科技 / 金融：
  Display (标题): sans     → Inter Bold / Space Grotesk / Formular
  Body (正文):    sans     → Inter Regular / Suisse Intl

方案 C — Playful 游戏 / 创意工作室：
  Display (标题): display  → Fraunces / Instrument Serif / 自建可变字体
  Body (正文):    sans     → Inter

Sizing（标题使用 vw 相对单位）：
  Hero 标题:     clamp(3rem, 10vw, 9rem)   — 占满视宽度感
  Section 标题:  clamp(2rem, 5vw, 4.5rem)  — 章节分隔
  正文:          clamp(0.95rem, 1.2vw, 1.1rem)
  辅助文字:      0.8rem, letter-spacing 0.15em

Spacing：
  Hero padding:  0 6vw, centered, 100vh
  Section pad:   14-20vh vertical, 6vw horizontal
  段落间距:      1.5em (body), 0.8em (heading → body)
```

---

## GSAP + Lenis 标准启动代码

```js
// js/main.js — 艺术官网标准入口
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import Lenis from '@studio-freight/lenis';

gsap.registerPlugin(ScrollTrigger);

// 平滑滚动
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true
});
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);

// 页面动画入口：各 section 自己在独立文件中
import { initHero } from './animations/hero.js';
import { initSections } from './animations/sections.js';
import { initMagnetic } from './animations/magnetic.js';

// 等待字体加载，避免 CLS 破坏动画节奏
document.fonts.ready.then(() => {
  initHero();
  initSections();
  initMagnetic();
});
```

---

## 性能与可访问性硬性规则

```
[1] Reduced Motion 尊重
    const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
    if (prefersReducedMotion) → 跳过所有 shaders、ScrollTrigger scrub、stagger
    → 改为瞬时 opacity: 0 → 1

[2] WebGL 预算
    ✓ 桌面端最多 2 个 active canvas
    ✓ 移动端降级为静态 SVG 或简化 shader
    ✓ 页面不可见时 (visibilitychange) 暂停所有 RAF 循环
    ✓ shader 中避免 sin/cos 组合噪声——优先使用简单伪噪声

[3] ScrollTrigger 最佳实践
    ✓ 所有 pin 元素明确声明 pinSpacing: true
    ✓ scrub 使用数字（0.6-0.8）而非 true，避免"粘滞感"
    ✓ markers: false（生产环境永远关闭）
    ✓ 用 Refresh() 而不是 kill/recreate 处理窗口尺寸变化

[4] 字体加载
    ✓ `<link rel="preconnect" href="https://fonts.googleapis.com">`
    ✓ `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`
    ✓ `font-display: swap` 避免文字不可见
    ✓ 在 `document.fonts.ready` 后触发入场动画

[5] 颜色对比度
    ✓ 正文 ≥ 7:1 (AAA)
    ✓ 按钮文本 ≥ 4.5:1 (AA)
    ✓ 装饰文本可例外，但保证可读性

[6] 键盘可访问
    ✓ 所有按钮/链接可 Tab 聚焦
    ✓ focus-visible 样式与 hover 风格一致
    ✓ ScrollTrigger pin 的 section 不阻塞键盘导航
```

---

## 文件结构约定（单文件原型 vs 生产级）

```
━━━ 🚀 快速原型（单文件 index.html）━━━

index.html
├── <head>  → CSS 内联 + Google Fonts preconnect
├── <body>
│   ├── <!-- 页面结构：Hero / Manifesto / Features / Showcase / CTA / Footer -->
│   └── <script> ESM import gsap, Lenis, Three.js (CDN)
└── 所有 JS 在 HTML body 末尾 — 单文件可直接双击打开

━━━ 🏭 生产级（模块化）━━━

src/
├── index.html                # 语义化结构 + 最少视觉
├── styles/
│   ├── main.css              # CSS 变量 + 布局 + 字体（无动画）
│   ├── sections.css          # 各 section 的排版样式
│   └── utility.css           # .visually-hidden 等工具类
├── js/
│   ├── main.js               # Lenis + 全局 RAF + 字体加载
│   ├── animations/
│   │   ├── hero.js           # Hero timeline
│   │   ├── sections.js       # Section scroll reveals
│   │   └── magnetic.js       # 磁吸按钮 + 3D tilt 效果
│   └── webgl/
│       ├── orb.js            # 浮动球体 shader
│       └── showcase.js       # 展示区 3D 场景
└── assets/
    └── (图片/字体/模型资源)
```
