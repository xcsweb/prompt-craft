# 任务模板：生成艺术化官网（build-artistic-website）

> 适用场景：用户说"做一个很酷的官网" / "像 Apple 那种感觉" / "Awwwards 级别" /
>           "沉浸式品牌站" / "官网但要有设计感"。
>
> **使用前必须先读**：`references/artistic-website-dna.md`（选方向 + 选 3-5 个交互签名）+ `references/cinematic-camera-transitions.md`（运镜/转场专项）。

---

## 🧭 第一步：用户输入确认（在生成前向用户确认）

```
我将为你生成一个艺术化官网。为确保符合你的预期，请先确认：

【1】设计方向（7 选 1）
   A. Editorial 杂志博物馆风（奶油纸色 + Serif 大标题 + 巨量留白）
      适用：设计工作室 / 文化机构 / 高端品牌 / 创意咨询
   B. Cyberpunk 黑暗沉浸式（深黑底色 + 霓虹发光 + 全屏 WebGL）
      适用：游戏工作室 / 音乐厂牌 / 互动媒体 / 科技产品发布
   C. Playful 游戏探索风（3D 世界 + 用户驾驶/探索）
      适用：个人作品集 / 儿童教育 / 创意工具
   D. 3D Product Showcase（产品 360° 展示 + HUD 规格）
      适用：汽车 / 消费电子 / 家具 / 珠宝
   E. Neo-Swiss 新瑞士粗野主义（12 列网格 + 编号系统 + news ticker）
      适用：设计工作室 / 建筑事务所 / 法律金融 / 学术机构
   F. Dark Luxury 深色奢华风（暗色空间 + 金色渐变 + 产品 HUD）
      适用：高端手表 / 珠宝 / 香水 / 奢侈品 / 私人银行
   G. Cinematic Narrative 电影运镜叙事风（一镜到底 / 场景切换 / 滚动同步 camera）
      适用：高端品牌发布 / 电影/游戏/音乐宣传 / 科技产品故事页 / 文化展览

【2】品牌信息
   - 品牌名称：___
   - 业务/行业：___
   - 品牌 slogan（1 句话）：___
   - 强调色（如没有则按方向提供默认色）：___

【3】档位（二选一）
   🚀 快速原型：单文件 index.html + CDN，双击直接打开
   🏭 生产级：Vite + Vue 3 + TypeScript 模块化架构

【4】你想要的 3-5 个交互签名（从 artistic-website-dna.md / cinematic-camera-transitions.md 选）
   文本类
   ___ Mask Reveal 遮罩文字入场
   ___ SplitText Stagger 字符逐字入场
   ___ Scroll-driven Line Grow 滚动下划线生长
   光标/按钮类
   ___ Custom Cursor + Magnetic Button 自定义光标 + 磁吸按钮
   ___ Hover Invert Cards hover 反色卡片
   ___ Tilt 3D Card 3D 倾斜卡片
   滚动叙事/视觉类
   ___ Scroll-driven Shader 滚动驱动 shader
   ___ Canvas 2D Particle Field 粒子场
   ___ Sticky Reveal Narrative 粘性章节叙事
   ___ Horizontal Scroll 横向滚动章节
   ___ Long-form Editor Scroll 长文编辑滚动
   ___ Fixed Background Progress 固定背景随滚动变形
   ___ Paper Noise Overlay 纸张噪点纹理
   ___ Shader Orb shader 发光球体
   3D 产品类
   ___ Orbit 360° Viewer 轨道 360° 查看器
   ___ Material Color Switcher 材质颜色切换
   ___ Hotspot Annotation 热点标注
   运镜与转场类
   ___ Scroll-Synced Camera 滚动同步相机
   ___ Scene Wipe 场景擦除转场
   ___ View Transition Page Morph 页面转场形变
   ___ One-Shot Long Take 一镜到底
   ___ Physics-Driven Transition 物理驱动转场
   ___ Page Wipe Transition 页面擦除切换

（默认：如果用户没有指定，使用方向 A + 前 5 个交互签名 + 🚀 快速原型）
```

---

## 🚀 快速原型档 · 完整 Prompt

> 当用户选择 **方向 A + 🚀 快速原型 + 前 5 个交互签名** 时，以下是发给 AI 的完整 Prompt。
> （可直接复制使用）

```
**Context: (环境与技术栈)**
- 技术模式：单文件 HTML + ES Modules CDN import，浏览器直接双击 index.html 运行
- 动画引擎（三选一，根据设计方向匹配）：
  • 🏅 **GSAP 3.x + ScrollTrigger**：Awwwards 级滚动叙事、复杂时间线
    `import { gsap } from 'https://esm.sh/gsap'; import { ScrollTrigger } from 'https://esm.sh/gsap/ScrollTrigger';`
  • 🟢 **Motion.dev / Motion One**：现代 SaaS 产品站，硬件加速，最小体积
    `import { animate, timeline, stagger, spring, inView } from 'https://esm.sh/motion';`
  • 🟣 **anime.js v4**：SVG 重度页面、数据可视化、轻量微交互
    `import anime from 'https://esm.sh/animejs';`
- 平滑滚动：@studio-freight/lenis — 与上述所有引擎兼容
  `import Lenis from 'https://esm.sh/@studio-freight/lenis';`
- 3D / WebGL：Three.js r128+ (import via esm.sh) — 仅在需要产品展示 / shader orb 时使用
- 字体：Google Fonts CDN — Newsreader (display serif) + Inter (body sans)
- 开发模式：无需构建工具，零 npm install

**动画引擎选择规则**：
  - Editorial / 杂志风 → GSAP（精细时间线）或 Motion.dev
  - Modern SaaS / 产品站 → Motion.dev（硬件加速，响应式）
  - Cyberpunk / 沉浸式黑暗风 → GSAP + Canvas 2D
  - Swiss / 粗野主义 → Motion.dev 或 GSAP（marquee 动画）
  - 3D Product Showcase → GSAP（切换动画）+ Three.js（模型）
  - Cinematic Narrative / 电影运镜 → GSAP + ScrollSmoother + Observer + Three.js（camera path）
  - SVG-heavy / 数据可视化 → anime.js

**Objective: (设计目标)**
为品牌【品牌名】生成一个 editorially-designed 官网，方向为【设计方向】。
核心 slogan：【slogan】。

页面结构（共 6 个 story beat，一个 beat = 一个滚动章节）：
  Beat 1: HERO — 大标题 + 标签行 + 品牌色装饰元素（或球体）
  Beat 2: MANIFESTO — 2-3 行居中宣言文字（巨号 serif）
  Beat 3: FEATURES — 4-6 张特性卡片，hover 反色
  Beat 4: SHOWCASE — 作品/产品展示（图+文交替布局）
  Beat 5: CTA — 简单大字 + 1 个行动按钮
  Beat 6: FOOTER — 极简导航 + 版权行

交互签名（本页面必须实现这 5 个）：
  ① Mask Reveal（Hero 标题从 y:100% 遮罩入场）
  ② Custom Cursor + Magnetic Button（自定义光标 + CTA 按钮磁吸）
  ③ Hover Invert Cards（feature 卡片 hover 时反色）
  ④ Shader Orb / SVG Orb（Hero 装饰球体，WebGL 不可用时 SVG 降级）
  ⑤ Scroll-driven animation（滚动驱动 section 标题下划线生长）

**Style: (设计语言)**
排版规则（严格遵守）：
  - Display (Hero 标题)：Newsreader / Playfair Display,
    clamp(3rem, 10vw, 9rem), line-height 1.1, letter-spacing -0.02em, weight 300
  - Section 标题：同一 display font, clamp(2rem, 5vw, 4.5rem), weight 400
  - 正文：Inter, 1rem, line-height 1.65
  - Small caps label：Inter, 0.75rem, uppercase, letter-spacing 0.2em

颜色（严格 3 色 + 1 个 muted）：
  - Background：#F5F3EF（奶油纸色 — off-white with warm tint）
  - Text：#1A1A1A（近墨黑）
  - Accent：#E0512B（赭石橙 — 仅在 hover/CTA/装饰中出现）
  - Muted：#8A8A8A（用于日期、标签等次要信息）

间距（留白即设计）：
  - Section 上下边距：18vh + 2rem min / max
  - Hero：100vh
  - 正文区块：2rem 段间距
  - 水平 padding：6vw 左右

材质与纹理：
  - 全站 SVG noise 2-4% opacity（纸张感）
  - Feature card hover：背景 #1A1A1A，文字 #F5F3EF
  - Shader orb（如使用）：低多边 + fresnel rim light + 缓慢旋转

**Tone: (实现基调)**
- 克制、优雅、有节奏感 — 不要炫技
- 动画时间全部明确写出：duration 1.0-1.2s for hero；0.6s for hover transitions；stagger 0.08
- Ease 选择：content 入场用 power2.out, hero 用 power3.out, hover 用 power1
- 页面开场动画：等待 `document.fonts.ready` 后触发，避免字体加载导致动画跳帧

**Audience: (受众)**
- 目标用户：品牌决策者、设计意识较强的用户
- 技术受众：前端工程师/设计师会查看源码 → 代码需要结构清晰
- 必须考虑移动端：在 `max-width: 768px` 时：关闭 magnetic button、关闭 shader orb（降级为 SVG 静态）、
  关闭 heavy scrub 动画（改为简单 fade-in）、关闭自定义光标

**Response Format: (输出格式)**

你必须按以下结构输出单文件 HTML。代码必须完整无省略，可直接保存为 index.html 双击运行。

┌─ 单文件结构 ─────────────────────────────────────────────────────┐
│                                                                   │
│  1. <!DOCTYPE html><html lang="zh-CN"><head>                      │
│     <meta charset="UTF-8">                                        │
│     <meta name="viewport" content="width=device-width, initial-scale=1.0"> │
│     <title>品牌名</title>                                         │
│     ── Google Fonts preconnect (2 条) + stylesheet 引入            │
│     ── 内联 <style>：                                             │
│        · CSS Variables (颜色、字体大小)                            │
│        · Reset (box-sizing border-box, margin/padding reset)       │
│        · 排版（h1-h6, p, small-caps label）                         │
│        · 布局（section padding, container max-width 1400px, grid） │
│        · Cursor 样式（固定定位，mix-blend-mode difference）        │
│        · Magnetic button hover 样式                                │
│        · Feature card 反色样式                                    │
│        · @media (max-width: 768px)：降级规则                       │
│     ── NO @keyframes 动画（所有动画走 GSAP）                        │
│                                                                   │
│  2. <body>                                                        │
│     <!-- SVG paper noise overlay -->                              │
│     <!-- Custom cursor dot + ring -->                             │
│     <!-- Nav bar -->                                              │
│     <!-- Section 1: HERO -->                                      │
│     <!-- Section 2: MANIFESTO -->                                 │
│     <!-- Section 3: FEATURES -->                                  │
│     <!-- Section 4: SHOWCASE -->                                  │
│     <!-- Section 5: CTA -->                                       │
│     <!-- Section 6: FOOTER -->                                    │
│                                                                   │
│  3. <script type="module">（所有 JS 内联在 HTML 末尾）              │
│     ── import { gsap } from 'https://esm.sh/gsap@3.12.5'          │
│     ── import { ScrollTrigger } from 'https://esm.sh/gsap@3.12.5/ScrollTrigger' │
│     ── import Lenis from 'https://esm.sh/@studio-freight/lenis@1.0.42' │
│     ── 如有 3D： import * as THREE from 'https://esm.sh/three@0.160.0' │
│     ── gsap.registerPlugin(ScrollTrigger)                          │
│     ── Lenis init → lenis.on('scroll', ScrollTrigger.update)      │
│     ── Custom cursor RAF loop                                    │
│     ── document.fonts.ready.then(() => {                          │
│          ── Hero timeline: 标题行 yPercent: 100 → 0 staggered    │
│          ── Hero meta: fade-in + y translation                    │
│          ── Magnetic button: mousemove listener                  │
│          ── ScrollTriggers: sections fade-in reveal, underline grow │
│          ── Feature cards: mousemove 改变 transform (3D tilt)     │
│          ── 如有 Orb: Three.js scene init with resize handling   │
│        });                                                        │
│                                                                   │
│  4. 结尾说明（在代码之后）：                                      │
│     • 一句话描述本页面的"叙事弧"                                    │
│     • 本页用到了哪些交互签名 + 每个的参数要点                       │
│     • 性能与降级策略说明                                           │
│     • 如何修改为真实品牌内容（位置标记）                            │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘

**Constraints: (硬性约束)**
- ❌ 不要使用 CSS @keyframes — 所有动画用 GSAP（性能更好 + 与 Lenis 同步）
- ❌ 不要使用 `font-size: 48px` 这类纯 px — 标题全部使用 clamp() 含 vw
- ❌ 不要超过 3 色（背景 + 文本 + 强调色），muted 灰色算第 4 色（可接受）
- ❌ 不要把 Shader Orb 放在 Hero 文字后面遮挡文本
- ✅ 必须等待 `document.fonts.ready` 后才入场动画（CLS 问题）
- ✅ `prefers-reduced-motion` 查询 → 关闭所有 scrub + stagger + shader，改为瞬时淡入
- ✅ 移动端降级方案必须存在（见 Audience 节）
- ✅ 页面必须可在 `file://` 协议下直接打开运行（不依赖服务器）
- ✅ 所有 GSAP 动画要有明确的 duration + ease（不要依赖默认值）

**Negative Constraints: (禁止事项)**
❌ 不要输出"好的！/ Sure！"等客套话，直接输出文件
❌ 不要在 HTML 中写"// 这里放你的内容"占位——使用真实、合理的示例文案（
   如品牌名为 HELIOS、业务为 AI 创意工具、文案围绕"让创造变得像呼吸一样"展开）
❌ 不要嵌套 CSS 选择器超过 3 层
❌ 不要使用 !important（覆盖第三方库样式除外，需加注释）
❌ 不要使用 jQuery、Bootstrap、Tailwind 等 heavy 库
❌ 不要添加任何分析脚本、广告脚本、第三方 cookies
```

---

## 🏭 生产级档 · Prompt 骨架

生产级 Prompt 基于快速原型，额外增加：

```
【架构】：Vite + Vue 3 + TypeScript + SCSS（CSS Modules）
【文件结构】：
  src/
    ├── App.vue
    ├── main.ts
    ├── styles/
    │   ├── _variables.scss      (颜色 / 字体 CSS variables)
    │   ├── _reset.scss
    │   ├── _typography.scss
    │   └── main.scss
    ├── composables/
    │   ├── useSmoothScroll.ts   (Lenis + ScrollTrigger)
    │   ├── useCursor.ts         (自定义光标)
    │   └── useReveal.ts         (Section reveal)
    ├── sections/
    │   ├── HeroSection.vue      (GSAP timeline in onMounted)
    │   ├── ManifestoSection.vue
    │   ├── FeaturesSection.vue
    │   ├── ShowcaseSection.vue
    │   └── CtaSection.vue
    └── components/
        ├── MagneticButton.vue
        ├── RevealText.vue       (可复用 mask-reveal 标题组件)
        ├── FeatureCard.vue      (hover invert + tilt)
        └── ShaderOrb.vue        (Three.js orb — 带 WebGL fail fallback)

【CMS】：可接入 Storyblok / Contentful 作为 headless CMS
【部署】：Vercel / Netlify — SSR 或静态生成
【测试】：Lighthouse 分数 ≥ 95（性能、可访问性、SEO）
```

---

## 💡 使用提示

1. **第一次使用**：先用快速原型档让用户"看到感觉"，再决定是否升级为生产级
2. **颜色替换**：在 `<style>` 顶部的 `:root` CSS Variables 中改 3 个值即可换肤
3. **内容替换**：每段文字都用中文占位（品牌故事、功能描述），便于用户识别修改
4. **交互签名选择策略**：Editorial 方向选 IS-01, IS-04, IS-05, IS-11, IS-03
   Cyberpunk 方向选 IS-04, IS-09, IS-10, IS-06, IS-13
