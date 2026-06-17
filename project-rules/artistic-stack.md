# T1 项目级规则：艺术化官网技术栈

> 适用于：品牌官网、设计工作室官网、沉浸式落地页、滚动叙事体验、电影化运镜/转场网站。
> 基于 Awwwards 2023-2026 获奖项目的逆向研究（Active Theory / Immersive Garden / Locomotive / Bruno Simon / Meridian / Blue Desert / Unseen Studio / Joseph Santamaria / Inkwell / Shopify Supply Performance Pack）。

---

## 核心技术栈

```
FRONTEND
├─ 原生 HTML + CSS + ES Modules (快速原型)
│   或 Vue 3 + Vite (生产级项目)
│   或 Next.js 14 App Router (如需 CMS/SEO)
│
├─ 动画引擎（3 个档位，按需求选择）
│   ├─ 🏅 GSAP 3.x + ScrollTrigger   — 复杂时间线 / 滚动叙事 / Awwwards 级（首选）
│   │                                  插件：ScrollTrigger / ScrollSmoother / Observer / Flip / Draggable / MorphSVG
│   │
│   ├─ 🟢 motion@12.x (Motion.dev)   — 轻量 / 硬件加速 / 现代首选
│   │   核心 API：animate() / timeline() / stagger() / spring()
│   │   双引擎：Web Animations API + JS fallback（120fps 丝滑）
│   │   纯 JS 版（原生 HTML 无依赖）：import { animate } from 'https://esm.sh/motion'
│   │
│   └─ 🟣 anime.js v4               — 极简 / SVG 友好 / 体积最小 (~15KB)
│       核心 API：anime.timeline() / targets / keyframes / easing
│
├─ 平滑滚动
│   └─ @studio-freight/lenis         — 惯性滚动 + GSAP/Motion 集成
│
├─ 运镜 / 3D Camera 工具
│   ├─ three.js                      — 3D 几何、场景、复杂着色器
│   ├─ three-story-controls          — NYTimes 开源 camera rig 工具包（ScrollControls / StoryPoints / PathPoints）
│   ├─ ogl@0.x                       — 更轻的 WebGL 替代品
│   └─ Canvas 2D API                 — 简单粒子、线条、SVG 手绘
│
├─ 页面/状态转场
│   ├─ View Transition API           — 原生 SPA/MPA 转场，生产级首选（需处理 prefers-reduced-motion）
│   └─ GSAP Flip / MorphSVG          — 自定义复杂形变 / 共享元素动画
│
└─ 字体
    ├─ Google Fonts                   — Inter (正文) / Newsreader (展示) / Fraunces (Display)
    └─ 永远在 `<head>` 用 `preconnect` + `font-display: swap`

NO BUILD TOOLS 模式（快速原型）
└─ 通过 ESM.sh CDN 直接 import：
   import { gsap } from 'https://esm.sh/gsap'
   import { ScrollTrigger } from 'https://esm.sh/gsap/ScrollTrigger'
   import { animate, timeline, stagger, spring } from 'https://esm.sh/motion'
   import anime from 'https://esm.sh/animejs'
   import Lenis from 'https://esm.sh/@studio-freight/lenis'
```

---

## 动画库对比（选型速查表）

| 维度 | GSAP | Motion.dev | anime.js |
|------|------|-----------|---------|
| **体积** | ~60KB (core) + plugins | ~20KB (core) | ~15KB |
| **性能** | 极强，JS 引擎主循环优化 | 最强，双引擎（WAAPI + JS） | 良好，简单场景够用 |
| **滚动驱动** | ScrollTrigger（行业标准） | 原生 supportScroll + ViewTransitions | 需要手写（无内置） |
| **时间线** | timeline()（专业级） | timeline()（简洁版） | timeline()（基础） |
| **SVG 支持** | 极好（MorphSVG/DrawSVG） | 良好 | 极好（路径动画） |
| **学习曲线** | 中高（概念多） | 极低（一行 API） | 极低 |
| **许可证** | Club GSAP 商业插件需授权 | MIT 免费开源 | MIT 免费开源 |
| **适用场景** | Awwwards 级复杂滚动叙事 | 现代产品站 / 简洁动画 / UI 微交互 | 轻量动效 / SVG / 数据可视化 |

### 推荐搭配策略
```
高端品牌站 / 滚动叙事 / 大奖作品  →  GSAP + Lenis
现代 SaaS / 产品站 / 简洁动效      →  Motion.dev + Lenis
创意微型站 / SVG 动画 / 数据展示    →  anime.js
电影化运镜 / 一镜到底 / 3D 叙事     →  GSAP + ScrollSmoother + Observer + Three.js + three-story-controls
跨文档页面转场 / SPA 状态切换        →  View Transition API（渐进增强）+ GSAP Flip（复杂形变）
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

Line Height（排版安全 — 绝对禁止裁剪文字）：
  Display 标题:  ≥ 1.05（绝对禁止 < 1.0，会导致升部/降部被裁剪）
  大标题:        ≥ 1.1（clamp(2rem+, ...) 级别的标题）
  正文:          ≥ 1.5（西文），≥ 1.6（中文）
  等宽代码/标签:  ≥ 1.4
  ⚠️ 警告: line-height < 1.1 与 overflow: hidden 组合 = 文字裁剪灾难

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

// 等待字体加载，避免 CLS 破坏动画节奏
document.fonts.ready.then(() => {
  // 各 section 动画入口
});
```

---

## Motion.dev + Lenis 标准启动代码（现代首选）

```js
// js/main.js — Motion.dev 简洁版入口
import { animate, timeline, stagger, spring, inView } from 'motion';
import Lenis from '@studio-freight/lenis';

// 平滑滚动（Lenis 与 Motion 完美兼容）
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true
});

function raf(time) {
  lenis.raf(time);
  requestAnimationFrame(raf);
}
requestAnimationFrame(raf);

// Hero 入场动画 —— 声明式一行完成
document.fonts.ready.then(() => {
  // Hero: stagger 文字逐字入场
  timeline([
    ['.hero-title span', { y: ['100%', '0%'], opacity: [0, 1] }, { delay: stagger(0.03), duration: 0.8, easing: [0.22, 0.03, 0.26, 1] }],
    ['.hero-desc', { opacity: [0, 1], y: [20, 0] }, { duration: 0.6, at: '-0.2' }],
    ['.cta-btn', { opacity: [0, 1], y: [20, 0] }, { duration: 0.5, easing: spring() }],
  ]);

  // Section 入场：滚动进入视口自动触发
  inView('.feature-card', (info) => {
    animate(info.target, { opacity: [0, 1], y: [30, 0] }, {
      duration: 0.6, easing: [0.22, 0.03, 0.26, 1]
    });
  }, { amount: 0.3 });
});
```

---

## anime.js 标准启动代码（极简 SVG 场景）

```js
// js/main.js — anime.js 入口（轻量站点 / SVG 场景）
import anime from 'animejs';
import Lenis from '@studio-freight/lenis';

const lenis = new Lenis({ duration: 1.2 });

function raf(time) { lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);

document.fonts.ready.then(() => {

  // 基础入场
  anime({
    targets: ['.hero-title', '.hero-desc'],
    translateY: [30, 0],
    opacity: [0, 1],
    duration: 900,
    delay: anime.stagger(200),
    easing: 'cubicBezier(0.22, 0.03, 0.26, 1)',
  });

  // SVG 路径动画（绘制一条线）
  anime({
    targets: '.svg-path',
    strokeDashoffset: [anime.setDashoffset, 0],
    duration: 1500,
    delay: 500,
    easing: 'easeInOutSine',
  });

  // 悬浮循环（spring 感）
  anime({
    targets: '.floating',
    translateY: [-10, 10],
    rotate: [-3, 3],
    duration: 3000,
    direction: 'alternate',
    loop: true,
    easing: 'easeInOutSine',
  });

});
```

---

## GSAP + ScrollSmoother + Observer + Three.js（电影化运镜/一镜到底）

```js
// js/main.js — 电影化叙事入口
import * as THREE from 'three'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { ScrollSmoother } from 'gsap/ScrollSmoother'
import { Observer } from 'gsap/Observer'

gsap.registerPlugin(ScrollTrigger, ScrollSmoother, Observer)

// 惯性平滑滚动（长镜头必备）
const smoother = ScrollSmoother.create({
  wrapper: '#smooth-wrapper',
  content: '#smooth-content',
  smooth: 1.5,
  smoothTouch: 0.1,
  effects: false
})

// Three.js 基础场景
const scene = new THREE.Scene()
const camera = new THREE.PerspectiveCamera(45, innerWidth / innerHeight, 0.1, 100)
camera.position.set(0, 0, 8)
const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
renderer.setSize(innerWidth, innerHeight)
renderer.setPixelRatio(Math.min(devicePixelRatio, 2))
document.body.appendChild(renderer.domElement)

// 统一输入：mouse / touch / trackpad / wheel
Observer.create({
  target: window,
  type: 'wheel,touch,scroll',
  onChange: (self) => {
    // 将输入与 ScrollTrigger 同步，实现"单镜头感"
  }
})

// 滚动驱动 camera dolly-in
gsap.to(camera.position, {
  z: 2,
  scrollTrigger: {
    trigger: '#smooth-content',
    start: 'top top',
    end: 'bottom bottom',
    scrub: 0.7
  }
})

function animate() {
  requestAnimationFrame(animate)
  renderer.render(scene, camera)
}
animate()
```

---

## View Transition API（页面/状态转场）

### MPA 跨文档转场（同源页面）

```css
@view-transition {
  navigation: auto;
}

@media (prefers-reduced-motion: reduce) {
  @view-transition {
    navigation: none;
  }
}
```

### SPA 同文档状态转场

```js
document.startViewTransition(() => {
  updateDOM() // 你的状态变更
})
```

### 生产级约束

- 必须加 `prefers-reduced-motion: reduce` 降级。
- 跨文档转场在移动端可能增加约 70ms LCP；配合 Speculation Rules 预渲染可抵消。
- 仅用于同源导航；不同源浏览器会回退为普通跳转。
- 复杂形变（多元素编排）成本高于收益，优先使用默认 fade 或简单 slide。

---

## 性能与可访问性硬性规则

```
[1] Reduced Motion 尊重
    const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
    if (prefersReducedMotion) → 跳过所有 shaders、ScrollTrigger scrub、stagger、View Transitions
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
