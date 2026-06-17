# 运镜与转场 — 电影化网页运动设计指南

> 源自：Awwwards 获奖项目、Codrops 教程、View Transition API 规范与 2025-2026 行业最佳实践的逆向拆解。
> 本文件是 Prompt Craft 的"电影化运动设计库"——**在生成长镜头、场景切换、一镜到底类网站前，请先从这里选方向、选镜头、选转场。**

---

## 目录

1. [什么是"运镜"与"转场"](#什么是运镜与转场)
2. [七大核心镜头语言](#七大核心镜头语言)
3. [六大转场类型](#六大转场类型)
4. [技术实现栈](#技术实现栈)
5. [案例拆解](#案例拆解)
6. [代码配方](#代码配方)
7. [性能与可访问性约束](#性能与可访问性约束)
8. [常见陷阱](#常见陷阱)

---

## 什么是运镜与转场

在网页语境中：

- **运镜（Camera Motion）**：用户滚动/导航时，视点（viewport 或 3D camera）发生的位置、角度、焦距变化。它把"滚动"变成"导演镜头"。
- **转场（Transition）**：从一个场景/状态/页面到另一个的衔接方式。它把"切换"变成"剪辑"。

两者共同构成 **Cinematic Web Narrative（电影化网页叙事）**。其最高目标是让用户感觉不到"我在看网页"，而是"我在经历一段连续的影像"。

---

## 七大核心镜头语言

| 镜头 | 效果 | 网页实现 | 典型参数 | 情绪 |
|------|------|---------|---------|------|
| **推轨 Dolly In / Out** | 摄像机沿 Z 轴靠近/远离主体 | 3D camera.position.z + ScrollTrigger scrub；或 CSS `scale` + `translateZ` | `scrub: 0.7`, `z: 8 → 2` | 强调、揭示、压迫感 |
| **摇臂 Crane** | 摄像机沿弧线或垂直轨迹运动 | `camera.position.y` + `lookAt` 插值；或 path-based camera rig | `path: bezier3D`, `duration: scroll-sync` | 宏大、仪式感 |
| **平移 Pan / Truck** | 摄像机左右/上下平移，主体保持相对静止 | `camera.position.x/y` 随滚动偏移；或背景层反向 parallax | `x: -20% → 20%`, `scrub: 0.8` | 空间延展、横向叙事 |
| **变焦 Zoom** | 焦距变化，画面快速放大/缩小 | `camera.fov` 变化；或 CSS `scale` 配合 `clip-path` | `fov: 75 → 25`, `power2.inOut` | 紧张、聚焦、戏剧化 |
| **环绕 Orbit** | 摄像机围绕主体 360° 旋转 | `camera.position` 沿圆形/椭圆路径，始终 `lookAt(target)` | `angle: 0 → Math.PI * 2`, `radius: 5` | 产品展示、全方位审视 |
| **手持 Handheld** | 轻微、有机的晃动，增加真实感 | 在 camera position 上叠加低频 Perlin noise 或 sine 扰动 | `noise amplitude: 0.02`, `freq: 0.3` | 纪录片、人文、温暖 |
| **一镜到底 One-Shot** | 整个网站像一条连续镜头，无硬切 | 长滚动 + 3D camera path + 场景淡入淡出/变形衔接 | `scroll height: 400-800vh`, `pin: true` | 沉浸、叙事、高级 |

> **关键原则**：每个镜头都必须有叙事理由。不要为了"炫"而移动 camera。

---

## 六大转场类型

### 1. 同文档状态转场（SPA State Transition）

| 子类型 | 效果 | 技术 |
|--------|------|------|
| **Fade / Cross-Dissolve** | 旧状态淡出，新状态淡入 | CSS `opacity` / View Transition API 默认 |
| **Match-Element Morph** | 同一元素从旧位置/大小变形到新位置/大小 | `view-transition-name` + `match-element` |
| **Slide / Push** | 整屏向某个方向推出，新状态从反方向进入 | CSS transform + View Transition types |
| **Shared Element Hero** | 缩略图放大成完整视图，图片/卡片保持连续性 | `view-transition-name` 指定共享元素 |

### 2. 跨文档页面转场（MPA Page Transition）

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

- 需要同源页面（same-origin）。
- 配合 Speculation Rules 可消除约 70ms 的移动 LCP 开销[cite:1]。
- 浏览器支持约 89% 全球流量（2026 年 4 月）。[cite:1]

### 3. 滚动场景转场（Scroll Scene Transition）

| 子类型 | 效果 | 技术 |
|--------|------|------|
| **Wipe（擦除）** | 一个色块/形状扫过画面切换场景 | `clip-path` / SVG mask / div transform |
| **Morph（变形）** | 当前场景元素变形为下一场景元素 | GSAP MorphSVG / FLIP / View Transition |
| **Portal（portal 缩放）** | 画面聚焦于一个点，穿过该点进入新场景 | `scale` + `border-radius` + 内容替换 |
| **Curtain（幕布）** | 两幅"幕布"从中间拉开或合上 | CSS transform + z-index 管理 |

### 4. 3D 空间转场（3D Space Transition）

| 子类型 | 效果 | 技术 |
|--------|------|------|
| **Room-to-Room** | 像在建筑空间中从一个房间移动到另一个房间 | Three.js camera path + 场景预加载 |
| **Dive / Surface** | 像潜水或浮出水面一样穿越界面层 | `camera.position.y` + `fog` density + 水面 shader |
| **Warp / Jump** | 快速拉近穿越，到达新场景 | `camera.fov` 爆发 + 径向 blur + 瞬间切换 |

### 5. 微转场（Micro-Transition）

- 按钮 hover → 状态变化
- Tab 切换 → 下划线滑动
- 列表重排 → FLIP 动画
- 表单聚焦 → 标签上浮

### 6. 物理转场（Physics Transition）

- 碎裂、聚合、流体、粒子消散
- 代表：Noomo Labs 水母玻璃球碎裂[cite:2]

---

## 技术实现栈

### 核心库

| 库 | 用途 | 选型建议 |
|----|------|---------|
| **GSAP + ScrollTrigger** | 滚动驱动时间线、camera path、pin、scrub | 复杂叙事首选 |
| **GSAP Observer** | 统一 mouse/touch/trackpad/scroll 输入 | 需要"单镜头感"时必用 |
| **GSAP ScrollSmoother** | 惯性平滑滚动 | 长镜头/一镜到底必用，但移动端谨慎 |
| **Three.js / React Three Fiber** | 3D camera、场景、光照 | 3D 运镜必备 |
| **three-story-controls** | Three.js 相机工具包，含 ScrollControls、StoryPoints、PathPoints[cite:3] | 快速实现 camera rig |
| **Lenis** | 轻量平滑滚动（4.7kB） | 与 GSAP/Motion 集成简单 |
| **Motion.dev** | 简洁硬件加速动画、spring | 适合 UI 转场、hero 入场 |
| **View Transition API** | 原生页面/状态转场 | 生产级 MPA/SPA 转场首选 |

### 镜头语言 → 技术映射

| 镜头 | 推荐技术 | 关键 API |
|------|---------|---------|
| Dolly / Crane / Orbit | Three.js + GSAP ScrollTrigger | `camera.position.set(x,y,z)`, `camera.lookAt(target)` |
| 一镜到底 | ScrollSmoother + Three.js + ScrollTrigger pin | `ScrollTrigger.create({ pin: true, scrub: 0.7 })` |
| 页面转场 | View Transition API / GSAP timeline | `document.startViewTransition()`, `@view-transition` |
| 滚动场景 wipe | GSAP + clip-path | `clip-path: inset(0 0 0 0)` → `inset(0 100% 0 0)` |
| 共享元素 morph | View Transition API + `view-transition-name` | `::view-transition-old(root)`, `::view-transition-new(root)` |
| 物理碎裂 | Three.js + Cannon.js / RapierJS | `Cannon-es`, `Rapier` |

---

## 案例拆解

### 1. Joseph Santamaria Portfolio — 一镜到底 3D 世界

- **网址**：https://joseph-san.com/
- **技术**：Three.js + GSAP + ScrollTrigger + Observer + SplitText[cite:4]
- **运镜特点**：
  - 每个 section 是一个 scene
  - 滚动进度映射到 camera position、lighting、object animation
  - 使用 GSAP Observer 统一输入，让整站像"single camera take"
- **可复用模式**：
  - `ScrollTrigger pin + scrub` 控制长镜头
  - 用 `Observer` 替代原生 scroll 事件统一触控/鼠标/触摸板

### 2. Shopify Supply Performance Pack — 滚动同步相机

- **网址**：https://shopify.supply/performance-pack
- **技术**：Hydrogen + Three.js + WebGL[cite:5]
- **运镜特点**：
  - 3D, scroll-synced experience
  - 服装在运动中展示
  - Scroll-synchronized Camera 作为核心 element
- **可复用模式**：
  - 产品 + camera path + scroll = 动态商品展示

### 3. Inkwell — 电影化三幕叙事

- **网址**：https://www.awwwards.com/sites/inkwell
- **技术**：Vue.js + OGL + RapierJS[cite:6]
- **运镜/转场特点**：
  - 先写 synopsis、三幕 arc、storyboard
  - 每个 scroll section 是一个 scene
  - transitions, motion, pacing 都像电影
  - 无传统导航，单次不间断旅程
  - 顶部 pathfinder 只显示位置，不允许跳转
- **可复用模式**：
  - 内容结构按电影三幕设计
  - 用滚动进度替代分页/跳转

### 4. Unseen Studio — 虚拟空间中的房间转场

- **网址**：https://unseen.co/
- **技术**：WebGL + Vue.js + Lenis + clip-path[cite:7]
- **运镜/转场特点**：
  - 把网站设计成"虚拟的家"
  - 页面间转场像在不同房间移动
  - 甚至有一个"潜水"效果到达 projects 页
  - 3D cursor、parallax storytelling
- **可复用模式**：
  - 多页网站可以用 camera movement 替代硬切
  - 场景切换时同步 audio 增强沉浸

### 5. Active Theory — 游戏化 3D 环境

- **网址**：https://activetheory.net
- **技术**：WebGL + 实时渲染[cite:8]
- **运镜特点**：
  - 不是滚动，而是"导航穿越"
  - 交互即导航
  - 元素 hover 发光/位移，训练用户如何操作
- **可复用模式**：
  - 把 cursor 移动当作 camera 引导
  - 用 subtle feedback loops 降低学习成本

### 6. Noomo Labs — 物理驱动转场

- **网址**：https://labs.noomoagency.com/
- **技术**：Three.js + Cannon.js[cite:2]
- **转场特点**：
  - 水母从玻璃球中"诞生"
  - 球体碎裂成碎片跟随用户
  - 每次启动碎片位置不同（真实物理）
- **可复用模式**：
  - 用 physics 引擎让转场有不可预测的生命力

---

## 代码配方

### 配方 1：基础 Scroll-Synced Camera（Three.js + GSAP）

```javascript
import * as THREE from 'three'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const camera = new THREE.PerspectiveCamera(45, w / h, 0.1, 100)
camera.position.set(0, 0, 8)

// 滚动从 0% 到 100% 时，camera 从 z=8 推到 z=2
gsap.to(camera.position, {
  z: 2,
  scrollTrigger: {
    trigger: '.scroll-container',
    start: 'top top',
    end: 'bottom bottom',
    scrub: 0.7
  }
})
```

### 配方 2：Camera Path（沿曲线运动）

```javascript
import { CurvePath, CubicBezierCurve3, Vector3 } from 'three'

const path = new CurvePath()
path.add(new CubicBezierCurve3(
  new Vector3(0, 0, 8),
  new Vector3(2, 1, 5),
  new Vector3(-2, 2, 3),
  new Vector3(0, 0, 1)
))

ScrollTrigger.create({
  trigger: '.scene-wrapper',
  start: 'top top',
  end: '+=500%',
  pin: true,
  scrub: 0.8,
  onUpdate: (self) => {
    const p = path.getPoint(self.progress)
    const look = path.getPoint(Math.min(self.progress + 0.01, 1))
    camera.position.copy(p)
    camera.lookAt(look)
  }
})
```

### 配方 3：View Transition API（跨文档淡入）

```css
/* 两个页面都要加 */
@view-transition {
  navigation: auto;
}

::view-transition-old(root) {
  animation: fade-out 0.3s ease forwards;
}
::view-transition-new(root) {
  animation: fade-in 0.3s ease forwards;
}

@keyframes fade-out { to { opacity: 0; } }
@keyframes fade-in { from { opacity: 0; } }
```

### 配方 4：Shared Element Morph（match-element）

```css
.card {
  view-transition-name: match-element;
}
```

```javascript
document.startViewTransition(() => {
  updateDOM() // 你的状态变更
})
```

### 配方 5：Scroll Scene Wipe（clip-path）

```javascript
gsap.fromTo('.scene-a', 
  { clipPath: 'inset(0 0 0 0)' },
  { 
    clipPath: 'inset(0 100% 0 0)',
    scrollTrigger: {
      trigger: '.scene-a',
      start: 'top top',
      end: '+=100%',
      pin: true,
      scrub: 0.6
    }
  }
)
```

### 配方 6：一镜到底结构模板

```html
<div id="smooth-wrapper">
  <div id="smooth-content">
    <section class="scene" style="height: 100vh">...</section>
    <section class="scene" style="height: 100vh">...</section>
    <section class="scene" style="height: 100vh">...</section>
  </div>
</div>
<canvas id="world" style="position:fixed; inset:0; z-index:0"></canvas>
```

```javascript
const smoother = ScrollSmoother.create({
  wrapper: '#smooth-wrapper',
  content: '#smooth-content',
  smooth: 1.5,
  effects: false
})

// 每个 scene 一个 ScrollTrigger pin + camera timeline
```

---

## 性能与可访问性约束

### 必须做

1. **prefers-reduced-motion**：检测到减少动画偏好时，关闭 camera path、关闭 scrub、关闭转场，直接显示内容。
2. **移动端降级**：
   - 关闭 ScrollSmoother（或设置 `smoothTouch: 0.1`）
   - 关闭 heavy 3D camera path
   - 改用简单 fade-in / CSS parallax
3. **WebGL 预算**：桌面端最多 2 个 active canvas；移动端最多 1 个。
4. **LCP 警觉**：
   - View Transitions 在移动重复访问可能增加约 70ms LCP[cite:1]
   - 配合 Speculation Rules 可抵消
5. **预加载**：大型 3D 场景、HDR、纹理必须预加载并显示进度条。

### 不要做

- 不要为了 camera 运动而运动
- 不要同时使用 ScrollSmoother + Lenis + 原生滚动监听（会冲突）
- 不要让 camera path 在没有 fallback 的情况下主导全部内容

---

## 常见陷阱

| 陷阱 | 表现 | 解决 |
|------|------|------|
| **镜头炫技** | 每个 section 都有复杂 camera 运动，用户眩晕 | 一页最多 2-3 个长镜头，其余用静态或 fade |
| **滚动劫持过强** | 用户无法快速浏览，产生挫败感 | 保留传统导航或章节跳转作为逃生口 |
| **移动端一镜到底** | 复杂 3D camera path 在手机上掉帧 | 移动端降级为 2D parallax + 简单 fade |
| **转场过长** | 页面切换动画超过 500ms，感觉慢 | 转场控制在 200-400ms |
| **没有叙事骨架** | 先写代码再编故事，内容支离破碎 | 先写 synopsis + storyboard + 三幕结构 |
| **忽略焦点管理** | View Transition 后焦点丢失 | transition finished 后把焦点放到新页面 `<main>` |

---

## Sources

[cite:1] [CSS View Transitions API: 3 Production Patterns (And 2 to Skip)](https://solid-web.com/css-view-transitions-api-production-patterns/) — 生产级 View Transition 模式与性能数据（2026-04-19）

[cite:2] [Noomo Labs: a hub for cutting-edge immersive experiences](https://www.awwwards.com/noomo-labs-a-hub-for-cutting-edge-immersive-experiences.html) — Awwwards 案例：物理驱动的 3D 转场

[cite:3] [Three Story Controls](https://nytimes.github.io/three-story-controls/) — NYTimes 开源 Three.js camera 工具包

[cite:4] [More Than a Portfolio: Building a Scroll-Driven 3D World](http://tympanus.net/codrops/2026/04/28/more-than-a-portfolio-building-a-scroll-driven-3d-world-with-something-to-say/) — Codrops 2026-04-28 案例研究

[cite:5] [Shopify Supply | Performance Pack](https://www.awwwards.com/sites/shopify-supply-performance-pack) — Awwwards Site of the Day 2025-12-12

[cite:6] [Inkwell: A scroll-driven narrative for AI's most stealth player](https://www.awwwards.com/inkwell-a-scroll-driven-narrative-for-ais-most-stealth-player.html) — Awwwards 案例：电影化三幕叙事

[cite:7] [Unseen Studio wins SOTM February 2023](https://www.awwwards.com/unseen-studio-by-unseen-studio-wins-sotm-february-2023.html) — Awwwards 月度最佳：虚拟空间转场

[cite:8] [25 Stunning Interactive Website Examples & Design Trends](https://www.thewebfactory.us/blogs/25-stunning-interactive-website-examples-design-trends/) — Active Theory、Locomotive 等案例分析（2025-08-11）
