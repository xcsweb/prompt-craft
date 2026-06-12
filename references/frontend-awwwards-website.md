# 🚀 Prompt Craft - 高级炫酷官网提示词 v2.1

> 适用场景：用户要做一个 Apple / Linear / Vercel / Framer 级别的炫酷官网。
> 核心架构：React Three Fiber (R3F) + GSAP + Lenis + Tailwind + GLSL Shader。
> 设计哲学：**"克制的炫"—— 每一帧都服务于品牌叙事，不是炫技。**

---

## 快速开始

把下面整段代码复制粘贴给 AI（Claude / GPT / Gemini 都行），描述你的品牌和业务，即可生成。

```
请根据以下提示词生成一个高级炫酷官网。使用 React + TypeScript + Vite。
生成完整的项目代码和文件结构，确保代码可直接运行（npm install && npm run dev）。

【项目信息】
品牌名：[请替换]
业务：[例如：AI SaaS / 3D 游戏引擎 / 设计工具 / 云计算 / 加密钱包 / 硬件产品]
slogan：[例如：用 AI 重塑创意流程]
主色调：[例如：紫蓝渐变 (#6366f1 → #ec4899) / 纯黑科技感 / 白色极简 / 苹果风格的柔和渐变]
风格参考：[Apple / Linear / Vercel / Framer / Luma AI / 你的想法]

【核心页面结构】
首页：Hero + Features + Showcase + Testimonials + CTA + Footer
（如果有更多页面请在这里补充）

===============================
⚠️ 下面是本文件的完整提示词模板，直接整段发给 AI 即可。
AI 会根据你在上面填写的信息定制化生成。
===============================

```

---

## 完整 Prompt 模板（复制下方所有内容给 AI）

```
【角色】
你是一位顶级创意机构的资深前端工程师兼交互设计师，
曾为 Apple / Linear / Vercel 级别的公司打造过获 Awwwards SOTD 的网站。
你精通 WebGL、Three.js、GSAP、GLSL Shader，以及极致的微交互设计。
你的产出标准：**Awwwards 获奖站级别，可直接部署到生产**。

【风格约束】
- 极简但不简陋：大量留白，排版清晰，每一个元素都有存在理由
- 动态但不躁动：动画缓动，时长 400-1500ms，避免短促抖动
- 质感但不花哨：noise 纹理叠加、微妙的渐变光照，而非彩虹轰炸
- 3D 作为内容服务：3D 场景永远不能抢过内容的注意力
- 滚动即叙事：用户滚动时，页面像电影一样有起承转合
- 响应式：所有效果在移动端降级为轻量版（禁用 heavy shader + 3D）

【技术栈】
- Next.js 14 App Router + TypeScript (strict: true)
- Tailwind CSS 3 (utility-first, 不写内联 style)
- three + @react-three/fiber (R3F) + @react-three/drei
- @react-three/postprocessing (Bloom + Vignette + Noise)
- gsap + @gsap/react + @gsap/ScrollTrigger
- @studio-freight/lenis (平滑滚动)
- framer-motion (轻量 React 组件动画)
- 图片：next/image + sharp
- 字体：next/font + Google Fonts (正文 Inter / Neue Haas，标题 Space Grotesk 或同类)

【文件结构】
your-site/
├── app/
│   ├── layout.tsx          ← 全局 meta, Lenis 初始化, font 加载
│   ├── page.tsx            ← 首页
│   └── globals.css         ← CSS Variables + Tailwind base
├── components/
│   ├── layout/
│   │   ├── Navbar.tsx      ← 毛玻璃导航 + scroll progress bar
│   │   └── Footer.tsx
│   ├── sections/
│   │   ├── Hero.tsx        ← 首屏（3D Canvas + 遮罩文字）
│   │   ├── Features.tsx    ← 特性网格（3D tilt cards）
│   │   ├── Showcase.tsx    ← 产品展示（scroll-driven）
│   │   ├── Testimonials.tsx ← 客户评价（marquee）
│   │   └── CTA.tsx         ← 最终行动号召
│   ├── canvas/             ← 所有 R3F 3D 组件
│   │   ├── HeroScene.tsx   ← 3D 主场景
│   │   ├── FloatingLogo.tsx ← 漂浮几何体
│   │   ├── Particles.tsx   ← 环境粒子
│   │   └── DistortText.tsx ← 文字 shader 扭曲
│   ├── effects/            ← 2D 视觉效果组件
│   │   ├── ShaderBackground.tsx ← 全屏 FBM noise gradient
│   │   ├── Cursor.tsx      ← 双层光标（dot + ring）+ hover 变形
│   │   └── NoiseOverlay.tsx ← 高级纸感 grain 叠加
│   └── ui/                 ← 基础 UI 组件
│       ├── MagneticButton.tsx ← 磁吸按钮（mousemove 跟随 + scale）
│       ├── RevealText.tsx  ← 遮罩式文字入场
│       ├── TiltCard.tsx    ← 3D 倾斜卡片
│       └── Marquee.tsx     ← 无限跑马灯
├── hooks/
│   ├── useScrollProgress.ts ← 返回 0-1 滚动进度
│   └── useMousePosition.ts ← 返回 normalized mouse x/y
├── shaders/
│   ├── noise.glsl          ← FBM / Simplex noise 函数
│   ├── hero-fragment.glsl  ← Hero 背景 shader
│   └── particle-vertex.glsl ← 粒子 vertex shader
└── lib/
    └── utils.ts            ← cn(), lerp(), map() 工具函数

【核心交互 — 每个页面都必须实现】

1. 【平滑滚动】 Lenis
   - 全局启用，duration 1.2，自然 easing
   - 与 GSAP ScrollTrigger 完全兼容（lenis.on('scroll', ScrollTrigger.update)）

2. 【自定义光标】 Cursor
   - 内层 6x6 黑色 dot（瞬时跟随鼠标）
   - 外层 36x36 白色描边环（lerp 缓动跟随）
   - hover 到 a / button / [data-cursor="hover"] 时：外层环放大到 80x80 + 混合模式 difference
   - 移动端完全禁用

3. 【Shader 背景】 ShaderBackground
   - 全屏 <canvas>，position fixed，z-index -10
   - FBM 噪声三色混合 + time uniform 缓慢流动
   - 鼠标位置作为 uniform uMouse，轻微扭曲中心
   - Vignette 边缘渐暗，noise 纹理 0.03 透明度叠加
   - 颜色通过 CSS variables 注入，便于主题切换

4. 【Scroll Progress Bar】
   - 页面顶部 2px 高的线，随滚动从左到右填充品牌渐变色
   - 放在 Navbar 内

5. 【Magnetic Button】所有主要 CTA 按钮
   - mousemove 时向鼠标方向位移 15px（使用距离衰减）
   - mouseleave 弹性回位
   - hover 时 scale 1.05，border-radius 稍微变化

【Hero 首屏 — 核心中的核心】

设计要求：
- 左侧/居中：大标题，使用 SplitText 入场（每行 80ms stagger，yPercent 100→0）
- 右侧/背景：全宽半透明 3D 场景，跟随鼠标轻微旋转（lerp 平滑）
- 3D 场景内容：
  - 2-3 个低多边形几何体（Icosahedron / TorusKnot / Box），品牌渐变色材质
  - 使用 drei <Float speed={1.2} rotationIntensity={0.6} floatIntensity={0.5}> 漂浮
  - 1000-3000 个 Points 粒子做背景星尘（GPU Instanced）
  - 光照：<ambientLight intensity={0.4}>, <directionalLight position={[3,5,2]}>, <Environment preset="city"> 制造反射
  - 后期：Bloom({ luminanceThreshold: 0.6, intensity: 0.4 }) + Vignette + Noise
- 滚动时（ScrollTrigger scrub）：
  - 3D 场景缓慢 rotateY: 0 → Math.PI/2
  - 标题字体 opacity/scale 轻微变化
  - 场景 opacity 0.9 → 0.2

代码结构要点：
```tsx
// components/sections/Hero.tsx
export default function Hero() {
  return (
    <section className="relative min-h-screen w-full overflow-hidden">
      <ShaderBackground />
      <Canvas shadows camera={{ position: [0, 0, 5], fov: 50 }}
        gl={{ antialias: true, alpha: true }}>
        <Suspense fallback={null}>
          <HeroScene />
          <Particles count={2000} />
        </Suspense>
      </Canvas>
      <div className="relative z-10 h-screen flex items-center">
        <div className="container mx-auto px-6">
          <RevealText>你的品牌大标题</RevealText>
          <RevealText delay={0.2}>副文案，一句话描述价值</RevealText>
          <MagneticButton>开始免费试用</MagneticButton>
        </div>
      </div>
    </section>
  );
}
```

【Features / 特性网格】
- 3 列 x 2 行 = 6 个特性卡片
- 每张卡片：TiltCard（鼠标 hover 时 rotateX/Y + 轻微 scale）
- 卡片内容：彩色图标 / 标题 / 短描述
- 入场：IntersectionObserver 触发，每张卡片 stagger 0.1s，y: 30 → 0，opacity: 0 → 1
- 背景：每张卡片有独立的径向渐变光晕，hover 时强度增强

【Showcase / 产品展示】
- 单张全屏宽图（next/image）或 3D 产品模型
- ScrollTrigger pin 此 section，滚动时：
  - 左侧文字逐段 fade in/out（scrub true）
  - 右侧产品图片/3D 模型缓慢 rotate、scale、材质透明度变化
  - 至少 3 屏滚动内容，制造电影感

【Testimonials / 客户评价】
- 经典 Marquee（跑马灯），内容为客户 logo 或评价卡片
- 两行反向滚动（一行左，一行右）
- 或使用品牌墙（Brand Wall），hover 时 logo 从灰度变彩色

【CTA / 最终号召】
- 居中大标题（渐变色字，使用 bg-clip-text）
- 下方 MagneticButton 按钮
- 背景：极大字号的半透明品牌字作水印（如 "AI", 12rem, opacity 0.05）

【Navbar / 导航】
- position: fixed，top 0，全宽
- backdrop-blur-xl，10px 下的内容透明度 50%
- scroll > 40px 时加 bg-white/70 (暗色模式下 bg-black/70)
- 左侧 logo，右侧 links + 一个 CTA 按钮
- 顶部 2px scroll-progress bar（从品牌色渐变）

【Footer / 页脚】
- 4 列链接 + 1 列品牌信息
- 底部版权行，背景色与 body 相同但更深一级

【Shader / GLSL 实现要求】

Hero 背景 shader（放在 shaders/hero-fragment.glsl）：
```glsl
uniform float uTime;
uniform vec2  uMouse;
uniform vec3  uColorA;
uniform vec3  uColorB;
uniform vec3  uColorC;

varying vec2 vUv;

// ------ 包含 noise.glsl 的 FBM 噪声函数 ------

void main() {
  vec2 uv = vUv;

  // 随时间流动的 FBM
  float n = fbm(uv * 3.0 + uTime * 0.05);

  // 鼠标位置引起的轻微扰动
  vec2 toMouse = uMouse - uv;
  float mouseDist = length(toMouse);
  float push = 1.0 - smoothstep(0.0, 0.4, mouseDist);
  uv += normalize(toMouse) * push * 0.05;

  // 三色混合
  vec3 col = mix(uColorA, uColorB, n);
  col = mix(col, uColorC, smoothstep(0.4, 0.8, n));

  // Vignette
  float vignette = smoothstep(1.2, 0.2, length(uv - 0.5));
  col *= vignette;

  gl_FragColor = vec4(col, 1.0);
}
```

在 React 组件中通过 R3F 的 ShaderMaterial + uniforms 注入颜色。

【粒子系统 Particles.tsx】
- drei <Points material={shaderMaterial}>
- 2000 个随机分布在一个球体体积内的顶点
- custom shader vertex: 让每个点随 sin(uTime+id) 上下起伏
- fragment: 圆形 soft particle，距离中心 alpha 衰减

【动画时机 / 性能预算】
- 首屏 LCP < 2.5s：LCP 元素是文字/按钮，不是 3D scene
- 3D Canvas lazy load：只在 Hero section 进入 viewport 后才渲染
- 移动端（max-width: 768px）：禁用 Canvas shader 背景，降级为纯 CSS 渐变
- 所有 requestAnimationFrame 使用 useRef + cleanup，避免内存泄漏
- IntersectionObserver 触发入场动画，out of view 时 disable
- GSAP ScrollTrigger 调用 ScrollTrigger.refresh() 在所有图片/字体加载后

【字体与排版】
- H1：clamp(3rem, 8vw, 7rem)，font-weight: 700-900，line-height: 0.95
- H2：clamp(2rem, 5vw, 3.5rem)，font-weight: 600-700
- Body：Inter / system-ui，font-size 15-17px，line-height 1.65-1.8
- 品牌字（如果有）：Space Grotesk / DM Sans / Instrument Serif 作为标题
- 所有大写字母用 letter-spacing 0.05em - 0.1em

【颜色系统 — 放在 globals.css CSS variables】
```css
:root {
  --bg: #ffffff;            ← 主体背景（暗色模式 #0a0a0f）
  --fg: #0f172a;            ← 正文色（暗 #f1f5f9）
  --muted: #64748b;         ← 次要文字
  --brand-a: #6366f1;       ← 品牌色 A（紫蓝）
  --brand-b: #ec4899;       ← 品牌色 B（粉红）
  --brand-c: #06b6d4;       ← 品牌色 C（青）
  --line: rgba(15, 23, 42, 0.08);  ← 分割线
  --card: #ffffff;          ← 卡片背景
  --shadow: 0 1px 2px rgba(0,0,0,.04), 0 8px 24px rgba(0,0,0,.05);
}
```

【输出交付物清单】

请输出完整项目代码，包括：

1. package.json（精确到具体版本号）
2. next.config.js / tailwind.config.js / tsconfig.json
3. app/layout.tsx + globals.css
4. app/page.tsx（首页，import 各 sections）
5. components/canvas/HeroScene.tsx（3D 场景，带 Bloom 后期）
6. components/canvas/Particles.tsx（2000 粒子 shader）
7. components/effects/ShaderBackground.tsx（全屏 FBM noise shader）
8. components/effects/Cursor.tsx（双层自定义光标）
9. components/ui/MagneticButton.tsx（磁吸按钮）
10. components/ui/RevealText.tsx（文字遮罩入场）
11. components/ui/TiltCard.tsx（3D 倾斜卡片）
12. components/sections/Hero.tsx
13. components/sections/Features.tsx
14. components/sections/Showcase.tsx（至少 3 屏 scroll-pin）
15. components/sections/Testimonials.tsx（marquee 品牌墙）
16. components/sections/CTA.tsx
17. components/layout/Navbar.tsx（毛玻璃 + scroll progress bar）
18. components/layout/Footer.tsx
19. hooks/useScrollProgress.ts + hooks/useMousePosition.ts
20. shaders/noise.glsl（包含 snoise / fbm 函数）
21. lib/utils.ts（cn, lerp, map, clamp 等工具函数）
22. README.md（如何运行、部署，以及修改品牌色的说明）

【代码质量要求】

- 所有组件为 TypeScript 函数组件，明确 props type（不用 any）
- 所有 GLSL shader 代码放在独立 .glsl 文件，通过 raw import 加载
- 所有动画 hooks 使用 useEffect + cleanup，无内存泄漏
- 所有动画在 prefers-reduced-motion: reduce 时优雅降级
- 所有组件中使用的 hooks 放在顶层（避免条件 hook）
- 组件默认导出，支持 tree-shaking
- 控制台无任何 warning/error
- 代码格式统一：2-space indent，单引号，尾逗号，prettier 风格

【最重要的一条】
不要把炫酷做成炫技。如果你不确定某个效果是否加，那就不加。
当你犹豫时，参考 Linear.com 的克制程度。

好，现在开始写代码。
```

---

## 轻量版提示词（如果不想要 3D，纯 CSS+GSAP 也能非常酷）

如果用户不想用 R3F / Three.js（太重），但想要 Linear/Vercel 那种"质感足够但不使用 WebGL"的酷感，用以下轻量技术栈：

```
【技术栈（轻量版）】
- Next.js 14 + TypeScript + Tailwind CSS 3
- gsap + @gsap/react + @gsap/ScrollTrigger + @gsap/SplitText
- @studio-freight/lenis (平滑滚动，可替换为 GSAP ScrollSmoother)
- framer-motion (组件级微动画)

【核心效果 —— 全部 CSS + JS，无需 WebGL】

1. 【Gradient Mesh Background】CSS 多层 radial-gradient + backdrop-blur
   - 多个绝对定位的彩色圆形（mix-blend-mode: screen），随时间缓慢位移
   - 参考：https://gradienta.io 或 Linear 官网
   - 实现：3-4 个 div，每个 div 一个 radial-gradient，不同 position，CSS animation 15s ease-in-out infinite alternate

2. 【Text Reveal on Scroll】GSAP ScrollTrigger + SplitText
   - 每个 section 的 h2/h3 使用 yPercent: 100 → 0 + opacity: 0 → 1
   - stagger 0.08 per word
   - enterance 只播放一次（once: true）

3. 【Tilt Card】纯 CSS perspective + JS mousemove
   - transform-style: preserve-3d
   - rotateX = (y - centerY) * 0.05，rotateY = -(x - centerX) * 0.05

4. 【Marquee】CSS + GSAP
   - 内容重复 3 份，x: -33.333% → 0 循环

5. 【Noise Overlay】0.03-0.08 透明度的 grain.png 叠加
   - mix-blend-mode: overlay 或 multiply

6. 【Custom Cursor】同上标准版

7. 【Magnetic Button】同上标准版

8. 【Scroll Progress Bar】同上标准版

9. 【Border Gradient】卡片 hover 时边框显示渐变色
   - 用伪元素 ::before 放置渐变背景 + 1px inset，mask 技术让边框"流动"
   - 静态时半透明灰色线条，hover 时显示品牌渐变

10. 【Sticky Nav + Scroll-driven backdrop-blur】
    - position sticky + top:0
    - className 在 scroll > 50px 时增加 bg-white/70 backdrop-blur-xl

【注意】此轻量版首屏约 80-120KB JS（含 gzip），加载极快
适合品牌官网 / SaaS 官网，性能 100 / 100 / 100 / 100（LCP < 1s）
```

---

## 具体代码片段参考

### Magnetic Button（可直接拷贝）

```tsx
// components/ui/MagneticButton.tsx
'use client';
import React, { useRef, useEffect } from 'react';
import gsap from 'gsap';

interface Props {
  children: React.ReactNode;
  className?: string;
  strength?: number;
  onClick?: () => void;
}

export default function MagneticButton({ children, className = '', strength = 15, onClick }: Props) {
  const btnRef = useRef<HTMLButtonElement>(null);

  useEffect(() => {
    const btn = btnRef.current;
    if (!btn) return;

    const handleMove = (e: MouseEvent) => {
      const rect = btn.getBoundingClientRect();
      const x = e.clientX - rect.left - rect.width / 2;
      const y = e.clientY - rect.top - rect.height / 2;
      gsap.to(btn, { x: x * strength / 100, y: y * strength / 100, duration: 0.4, ease: 'power2.out' });
    };

    const handleLeave = () => {
      gsap.to(btn, { x: 0, y: 0, duration: 0.6, ease: 'elastic.out(1, 0.4)' });
    };

    btn.addEventListener('mousemove', handleMove);
    btn.addEventListener('mouseleave', handleLeave);
    return () => {
      btn.removeEventListener('mousemove', handleMove);
      btn.removeEventListener('mouseleave', handleLeave);
    };
  }, [strength]);

  return (
    <button
      ref={btnRef}
      onClick={onClick}
      className={`relative inline-flex items-center justify-center rounded-full bg-brand text-white font-medium px-8 py-4 transition-shadow hover:shadow-xl hover:shadow-brand/20 ${className}`}
    >
      {children}
    </button>
  );
}
```

### Lenis 平滑滚动初始化

```tsx
// app/layout.tsx 或单独 provider
'use client';
import { useEffect } from 'react';
import Lenis from '@studio-freight/lenis';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  useEffect(() => {
    const lenis = new Lenis({
      duration: 1.2,
      easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
      smoothWheel: true,
    });

    function raf(time: number) {
      lenis.raf(time);
      requestAnimationFrame(raf);
    }
    requestAnimationFrame(raf);

    // 让 GSAP ScrollTrigger 感知 lenis
    const ScrollTrigger = (window as any).ScrollTrigger;
    if (ScrollTrigger) {
      lenis.on('scroll', ScrollTrigger.update);
      gsap.ticker.add((time) => { lenis.raf(time * 1000); });
      gsap.ticker.lagSmoothing(1000 / 60);
    }
    return () => lenis.destroy();
  }, []);

  return <html>{children}</html>;
}
```

### 3D 场景简化版（无 R3F，纯 Three.js 轻量实现）

如果你只想用 Three.js 而不用 R3F（更轻、学习成本更高），创建一个 HeroCanvas 组件：

```tsx
// components/canvas/HeroCanvas.tsx
'use client';
import { useEffect, useRef } from 'react';
import * as THREE from 'three';

export default function HeroCanvas() {
  const containerRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const container = containerRef.current!;
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(50, container.clientWidth / container.clientHeight, 0.1, 100);
    camera.position.z = 5;
    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setSize(container.clientWidth, container.clientHeight);
    container.appendChild(renderer.domElement);

    // Mesh
    const geo = new THREE.IcosahedronGeometry(1.5, 1);
    const mat = new THREE.MeshStandardMaterial({ color: 0x6366f1, metalness: 0.4, roughness: 0.3, wireframe: false });
    const mesh = new THREE.Mesh(geo, mat);
    scene.add(mesh);

    // Light
    scene.add(new THREE.AmbientLight(0xffffff, 0.4));
    const dir = new THREE.DirectionalLight(0xffffff, 1.0);
    dir.position.set(3, 5, 2);
    scene.add(dir);

    // Animation
    const clock = new THREE.Clock();
    let mouseX = 0, mouseY = 0;
    const onMove = (e: MouseEvent) => {
      mouseX = (e.clientX / window.innerWidth - 0.5) * 0.5;
      mouseY = (e.clientY / window.innerHeight - 0.5) * 0.5;
    };
    window.addEventListener('mousemove', onMove);

    function animate() {
      const t = clock.getElapsedTime();
      mesh.rotation.y = t * 0.2 + mouseX;
      mesh.rotation.x = t * 0.15 + mouseY;
      mesh.position.y = Math.sin(t * 0.5) * 0.2;
      renderer.render(scene, camera);
      requestAnimationFrame(animate);
    }
    animate();

    const onResize = () => {
      camera.aspect = container.clientWidth / container.clientHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(container.clientWidth, container.clientHeight);
    };
    window.addEventListener('resize', onResize);

    return () => {
      window.removeEventListener('mousemove', onMove);
      window.removeEventListener('resize', onResize);
      container.removeChild(renderer.domElement);
      geo.dispose();
      mat.dispose();
      renderer.dispose();
    };
  }, []);

  return <div ref={containerRef} className="absolute inset-0" />;
}
```

---

## 你应该如何向用户提问

在开始生成代码前，向用户确认以下 3 个问题（信息越全，生成越精准）：

**1. 你的品牌是做什么的？**（一句话描述）
  → e.g. "我们是一家 AI 视频生成 SaaS"
  → 这个决定了 3D 场景内容（视频产品会放播放中的播放器抽象形状；云计算放服务器集群抽象）

**2. 风格参考？**
  - [ ] Apple 风格（大量留白、电影式 scroll-driven，几乎无 3D）
  - [ ] Linear 风格（极简几何、柔和渐变、克制动画）
  - [ ] Vercel / Framer 风格（Dark，科技感，明显 WebGL 元素）
  - [ ] Awwwards 艺术先锋风格（强视觉、实验性）
  - [ ] 我要最炸的（粒子爆炸、fluid、3D 全上）

**3. 技术复杂度偏好？**
  - [ ] 轻量：纯 CSS + GSAP，无 WebGL（< 150KB JS，秒开）
  - [ ] 标准：R3F + Shader（推荐，平衡效果与性能）
  - [ ] 重型：自定义 WebGL pipeline + 声音响应 + 大量 3D（效果最强）

**最后确认**：你想让我先用哪个方案做出一个完整示例给你看效果？建议从"标准"起步，看看效果再调整。
