# 艺术化官网 — 设计 DNA 与交互签名库

> 源自：对 7 个 Awwwards 获奖项目的逆向拆解。
> 本文件是 Prompt Craft 的"设计灵感库"——**在生成艺术官网前，请先从这里选 1 个方向和 3-5 个交互签名**。

---

## 三大设计方向

每个方向都有：核心特征、配色模板、字体建议、内容结构、适用行业。你必须选 1 个方向作为项目的"主 DNA"。

### 方向 A：Editorial / 杂志博物馆风

> **代表项目**：Immersive Garden Studio · Musée J. Armand Bombardier · Blue Desert

**核心感觉**：克制、优雅、有节奏感——像翻一本高端杂志或博物馆导览册

**设计语言**
| 维度 | 具体做法 |
|------|---------|
| **颜色** | 3 色：奶油纸色背景 (#F5F3EF) + 近墨黑色文本 (#1A1A1A) + 单一强调色（赭石 / 锈红 / 墨绿） |
| **字体** | Serif 超大标题（Newsreader / Playfair） + Sans 正文（Inter）。标题字号 clamp(3rem, 10vw, 9rem)，行高 1.1 |
| **排版节奏** | Section 间距 14-20vh；正文栏宽 50-65 字符；首字母大写 + 宽字距的 small caps 作为 section label |
| **留白** | 巨量留白是"设计签名"——不是填充不满，而是让内容有呼吸的空间 |
| **材质** | SVG noise 纸张纹理（2-4% opacity）+ 轻微的 3D 浮雕元素（罗马数字、字母） |
| **布局** | 不对称双栏 / 左对齐的编辑排版；section 间用巨型数字或引用分隔 |

**内容结构模板**
```
1. Hero：大标题 + 1 行标签 + 轻微 3D 装饰
2. Manifesto 宣言：居中 2-3 行大文字，传达品牌核心理念
3. Features / Offering：网格卡片 + hover 反色（白底变黑底）
4. Showcase / Narrative：图文交错的长叙事段落
5. Stats 数据：巨型数字 + 简短标签
6. CTA：简单大字 + 1 个按钮
7. Footer：极简导航 + 版权
```

**适用行业**：设计工作室、博物馆、文化机构、高端品牌、创意咨询、出版社、金融科技（需克制版）

---

### 方向 B：Cyberpunk / 沉浸式黑暗风

> **代表项目**：Active Theory v5

**核心感觉**：夜晚、发光、未来主义——用户进入的是一个"空间"而不是"页面"

**设计语言**
| 维度 | 具体做法 |
|------|---------|
| **颜色** | 深黑背景 (#0A0A0F) + 2 个霓虹色（洋红 #FF2E97 + 青 #00F0FF）+ 近白文本 |
| **字体** | Condensed Sans / 窄体几何无衬线；全部大写 + 宽字距；小字号文字；数字使用 mono |
| **排版节奏** | 大量空黑区域——内容在屏幕上只占小条带；WebGL 场景占满屏幕 |
| **材质** | Film grain 胶片噪点、glitch 色差、bloom 发光、CRT 扫描线、霓虹灯管感 |
| **布局** | 画布优先，HTML UI 作为 HUD 叠加在 canvas 上——布局是 HUD 设计而非网页设计 |

**内容结构**
```
1. Hero：全屏 WebGL 场景（可旋转的 3D 城市/管道/粒子云）+ 浮在其上的大标题
2. 章节切换：滚动即"传送"到下一场景——每个 section 是一个独立的 3D 环境
3. 每场景内：浮动 NPC / AI 助手文字气泡 + 项目链接
4. Footer：返回顶部按钮 + 社交链接（全部霓虹风格）
```

**适用行业**：游戏工作室、互动媒体、硬件品牌、音乐厂牌、科幻 IP、科技公司的"产品发布页"

---

### 方向 C：Playful / 游戏探索风

> **代表项目**：Bruno Simon Portfolio 2025

**核心感觉**：好玩、惊喜、发现——用户"开车"或"探索"到达内容，而非滚动

**设计语言**
| 维度 | 具体做法 |
|------|---------|
| **颜色** | 明亮自然色——蓝天、绿草、橙砖、白云；单个签名色（如法拉利红）用于所有交互元素 |
| **字体** | 圆润几何 sans；数字用等宽；所有标签文字写在 3D 世界中的标牌上而非 HTML |
| **材质** | Low-poly 风格 + flat shading；水面 shader + 粒子落叶/飘雪；实体碰撞 |
| **布局** | 没有传统布局——整个页面是一个 3D 世界地图；区域间用物理导航 |

**适用行业**：个人作品集（技术背景）、游戏公司、儿童教育、创意工具发布

---

### 方向 D：3D Product Showcase / 3D 产品展示风

> **代表项目**：小米 SU7 3D 车模展示 (gamemcu.com/su7) · Apple Vision Pro 产品页 · 蔚来 ET5 官网

**核心感觉**：真实、沉浸、可触摸——用户像在展厅里一样 360° 查看产品，而不是"阅读"页面

**设计语言**
| 维度 | 具体做法 |
|------|---------|
| **颜色** | 深黑/纯白背景（让产品成为视觉中心）+ 产品本身的材质色 + 1 个 UI 强调色（电光蓝 / 霓虹橙） |
| **字体** | 极细 Sans（Inter 300 / Roboto 300）用于大标题，等宽字体用于技术参数，全部小写或 sentence case |
| **排版节奏** | 产品占 60-80% 屏幕空间，文字信息以 HUD 浮层形式出现在边缘；参数用等宽数字 + 单位 |
| **材质** | PBR 物理渲染材质（金属/车漆/玻璃/织物），环境贴图反射（RoomEnvironment），实时阴影 |
| **布局** | 画布优先——WebGL Canvas 占满全屏，HTML UI 作为 HUD 叠加在边缘；控制面板在底部或侧边 |

**内容结构**
```
1. Hero：全屏 3D 产品模型（自动缓慢旋转）+ 产品名大字 + 1 行 slogan
2. 360° 查看：鼠标拖拽旋转产品，滚轮缩放，双击重置视角
3. 颜色/材质切换：底部色块按钮，点击切换产品材质（车漆颜色 / 织物纹理）
4. 特性拆解：点击产品某部位（如车灯/轮毂），弹出该部位的技术参数浮层
5. 场景切换：白天/夜晚/展厅 三种环境光照切换
6. CTA：预约试驾 / 立即购买 / 了解更多 —— 大按钮，磁吸效果
7. Footer：极简，品牌 logo + 社交链接
```

**适用行业**：汽车、消费电子、家具、珠宝、工业设备、任何需要"看到实物感"的产品

**技术栈要求**：见 `project-rules/3d-product-showcase.md`

---

## 交互签名库（Interaction Signatures）

从获奖项目中提炼的**可复用交互单元**。每个单元包含：做什么、技术栈、典型参数。

每次生成新网站时，请**从中选 3-5 个**，不要凭空"加一些动画"。

### 文本类 TEXT

#### IS-01：Mask Reveal（遮罩文字入场）
```
效果：文字被 clip-path 遮罩，从左到右/从下到上"打开"出现
适用：Hero 标题、section 大标题
技术：GSAP.fromTo + clipPath: "inset(100% 0 0 0)" → "inset(0% 0 0 0)"
参数：duration 1.0s, power3.out, stagger 0.08 per word
参考：Immersive Garden 所有 section 标题
```

#### IS-02：SplitText Stagger（字符逐字入场）
```
效果：标题被拆为字符/单词，每个独立运动入场
适用：Hero 大标题、manifesto 宣言
技术：gsap SplitText plugin + from y:60 opacity:0
参数：stagger 0.02-0.05s, power2.out, duration 0.8s
参考：Blue Desert 每个 section 的首字
```

#### IS-03：Scroll-driven Line Grow（滚动下划线延伸）
```
效果：section 标题下方的下划线从 0 宽度"生长"到目标宽度
适用：所有 section 的小标题 label
技术：ScrollTrigger scrub on pseudo-element width 0% → 100%
参数：scrub true, trigger element = section top
参考：Locomotive Musée Bombardier
```

### 光标/按钮类 CURSOR & BUTTON

#### IS-04：Custom Cursor + Magnetic Button
```
效果：隐藏原生光标，用大圆环跟随鼠标（lerp 0.2），hover 按钮时圆环变文字框并"吸附"到按钮
适用：全站光标系统 + 所有按钮
技术：1) 固定 position: fixed 的 DOM 元素跟随
      2) 鼠标位置 → lerp → 圆环位置
      3) 按钮 hover: 鼠标位置 vs 按钮中心 → 按钮 x += (mx - bx) * 0.3
参数：cursor lerp 0.15-0.2, magnetic strength 0.3-0.5
参考：Locomotive / Immersive Garden / 几乎所有 Awwwards 站点
```

#### IS-05：Hover Invert（悬停反色）
```
效果：hover feature card 时，整卡从白底变黑底，文字变白，箭头从黑色变橙色并旋转
适用：Features 网格卡片、project cards
技术：hover + CSS transition background-color + color + transform (rotate)
参数：transition 0.4s ease, rotate -45deg, scale 1.1 on arrow
参考：Locomotive 工作室官网项目卡片
```

### 滚动叙事类 SCROLL NARRATIVE

#### IS-06：Scroll-Driven Shader（滚动驱动 shader）
```
效果：hero 的 shader 背景随滚动进度演化——球体变形、色彩偏移
适用：Hero 背景、section 过渡
技术：ScrollTrigger 把 scroll progress 0→1 映射到 shader uniform uProgress
参数：scrub: 0.6 (eased), uniform 范围 v = scrollProgress
参考：Active Theory v5 每章节切换的 shader transition
```

#### IS-07：Horizontal Scroll（横向滚动章节）
```
效果：一个水平长条在纵向滚动中"横向移动"
适用：项目展示、时间轴
技术：ScrollTrigger pin + transform xPercent: -200 + scrub
参数：pin element, start "top top", end "+=200%" (2x viewport scroll for horizontal motion)
参考：Awwwards 2023 多个获奖项目
```

#### IS-08：Sticky Reveal Narrative（粘性章节叙事）
```
效果：某个 section"粘"在视口顶部，滚动时内部内容逐步变化（类似幻灯片但用滚动控制）
适用：故事叙事、产品介绍、历史时间线
技术：ScrollTrigger pin, scrub, 多个 timelines 嵌套
参数：pin: true, pinSpacing: true, markers: false, scrub: 0.7
参考：David Whyte 诗歌站点的每首诗
```

### 视觉/3D 类 VISUAL & 3D

#### IS-09：Shader Orb（Shader 发光球体）
```
效果：Hero 右侧/中心一个缓慢浮动+变形的 3D 球体，受鼠标位置轻微"推挤"
适用：品牌 hero、产品特征展示
技术：Three.js IcosahedronGeometry + custom ShaderMaterial + simplex noise
参数：noise scale 2.0, distortion amplitude 0.15, mouse influence 0.08, rim light 0.3
参考：Meridian hero orb (但 Meridian 是发光玻璃球，此处带噪声变形)
```

#### IS-10：Canvas 2D Particle Field（粒子场）
```
效果：100-300 个粒子在 hero 中缓慢漂浮，hover 时粒子向鼠标聚集
适用：hero 背景氛围
技术：Canvas 2D getContext('2d'), RAF loop, mouse proximity drive
参数：particles 150, radius 1-2px, speed 0.3-0.8 (random per particle)
参考：Active Theory + 几乎每一个创意官网
```

#### IS-11：SVG Noise Paper（纸张噪点）
```
效果：整页 2-4% 透明度的 SVG 噪点，模拟纸张/印刷品
适用：editorial 方向的所有页面
技术：`<svg>` in fixed position, or CSS background-image with turb-filter, or tiny PNG (64x64 noise)
参数：opacity 0.02-0.04, mix-blend-mode multiply
参考：Immersive Garden / 所有"印刷感"站点
```

#### IS-12：Tilt 3D Card（3D 倾斜卡片）
```
效果：鼠标 hover 卡片时，卡片绕 X/Y 轴做 3D perspective 倾斜，内容"浮"起
适用：Feature cards, product cards, case study thumbnails
技术：mousemove → calc dx/dy from center → element.style.transform: perspective(800px) rotateX(-dy) rotateY(dx)
参数：max rotate ±6-12deg, lerp 0.1 for smoothness
参考：Locomotive / Immersive Garden project cards
```

### 页面过渡与高级 ADVANCED

#### IS-13：Page Wipe（页面擦除切换）
```
效果：路由切换时，一整屏色块从右向左"擦"过页面，在下一页面反向退回
适用：多页网站、SPA 路由切换
技术：GSAP timeline: div x: 100% → 0 (0.3s) → content switch → x: 0 → -100% (0.3s)
参数：power4.inOut, duration 0.3+0.3 = 0.6s total
参考：Meridian / 高端 SaaS 站点
```

#### IS-14：Long-form Editor Scroll（长文编辑滚动）
```
效果：大段图文混排，滚动时图像从左/右滑入，文字从下方淡入，感觉像在翻一本杂志
适用：品牌故事、案例研究、产品介绍
技术：ScrollTrigger.batch + transform x: ±100 to 0, y: 30 to 0, stagger 0.1
参数：scrub 0.8 per image element
参考：Blue Desert / museum websites
```

#### IS-15：Fixed Background Progress（固定背景随滚动变化）
```
效果：一个固定在屏幕上的 SVG/Canvas 图形，其参数（大小、颜色、旋转、噪点密度）随滚动进度 0→1 变化
适用：整页背景主题元素、品牌 logo 变形
技术：ScrollTrigger.onUpdate() → write to CSS var or shader uniform
参数：scrub: 0.5, map scrollY: 0→maxScrollY to param: start→end
参考：Active Theory v5 全局 logo 变形
```

### 3D 产品展示类 3D PRODUCT SHOWCASE

#### IS-16：Orbit 360° Viewer（轨道 360° 查看器）
```
效果：产品模型在屏幕中央，用户可拖拽旋转、滚轮缩放、双击重置视角
适用：汽车、手机、家具、珠宝等任何需要"看到实物感"的产品
技术：Three.js OrbitControls + 限制垂直角度(minPolarAngle/maxPolarAngle) + 阻尼效果(enableDamping)
参数：minPolarAngle 0.5, maxPolarAngle 1.35, enableDamping true, zoomSpeed 1, enablePan false
参考：小米 SU7 官网 (gamemcu.com/su7) / Apple Vision Pro 产品页
```

#### IS-17：Material Color Switcher（材质颜色切换）
```
效果：底部一排色块按钮，点击后产品材质平滑过渡到新颜色（车漆/织物/金属）
适用：汽车配色、手机配色、家具面料选择
技术：Three.js MeshStandardMaterial.color.lerp() + GSAP 动画过渡 + 环境贴图同步切换
参数：过渡 duration 0.6s, power2.inOut, 同时切换 3 个材质属性(color/roughness/metalness)
参考：小米 SU7 官网配色切换 / 蔚来 ET5 官网
```

#### IS-18：Hotspot Annotation（热点标注）
```
效果：产品上有几个闪烁的小圆点，hover/点击后弹出该部位的技术参数浮层
适用：产品特性拆解、技术参数展示
技术：Three.js Raycaster 检测鼠标与模型交点 → 在交点位置放置 HTML 浮层(absolute定位) → GSAP 淡入
参数：浮层延迟 0.2s 出现，pointer-events: none 默认，hover 时变为 auto
参考：Apple 产品页特性标注 / 汽车官网配置器
```

#### IS-19：Environment Map Switch（环境光照切换）
```
效果：白天/夜晚/展厅 三种环境光照一键切换，产品反射实时变化
适用：展示产品在不同场景下的外观
技术：Three.js PMREMGenerator + CubeTexture 环境贴图 + FBO 离屏渲染混合两个 cubemap + GSAP 过渡 uWeight
参数：3 套 HDR/EXR 环境贴图，过渡 duration 1.5s, power2.inOut
参考：小米 SU7 官网场景切换 / 3D 产品配置器
```

#### IS-20：Reflective Ground（反射地面）
```
效果：产品下方有一块"镜面"地面，反射出产品的倒影，带轻微模糊和 Fresnel 衰减
适用：高端产品展示、汽车、珠宝
技术：自定义 ShaderMaterial — 反射向量计算 + 法线贴图粗糙度 + Fresnel 混合 + 离屏渲染反射贴图
参数：roughness 0.15, fresnel power 2.0, reflectivity 0.8
参考：小米 SU7 官网地面 / 几乎所有汽车 3D 展示
```

---

## 参考的技术配方表（完整可复用）

| 配方名 | 你需要的代码骨架 |
|--------|----------------|
| Smooth Scroll + GSAP | `new Lenis() + gsap.registerPlugin(ScrollTrigger) + lenis.on('scroll', ScrollTrigger.update)` |
| Hero Mask Reveal | `gsap.fromTo('.hero-line span', {yPercent: 100}, {yPercent: 0, duration: 1.0, stagger: 0.1, ease: 'power3.out'})` |
| Magnetic Button | `btn.parentElement.addEventListener('mousemove', e => { const rect = parent.getBoundingClientRect(); btn.style.transform = translate((mx-cx)*0.3, (my-cy)*0.3); })` |
| Custom Cursor | `const cursor = document.querySelector('.cursor'); let cx=0,cy=0,mx=0,my=0; window.addEventListener('mousemove', e => { mx=e.clientX; my=e.clientY; }); function raf(){ cx+=(mx-cx)*0.2; cy+=(my-cy)*0.2; cursor.style.left = cx+'px'; requestAnimationFrame(raf); } raf();` |
| Shader Orb | `new THREE.Mesh(geometry, new THREE.ShaderMaterial({ uniforms: { uTime, uMouse }, vertexShader: distort with noise, fragmentShader: fresnel + gradient }))` |
| Paper Noise | `<svg style="position:fixed; inset:0; pointer-events:none; opacity:0.03; mix-blend-mode: multiply"><filter><feTurbulence baseFrequency="0.9"/></filter><rect width="100%" height="100%" filter="url(#n)"/></svg>` |

---

## 常见陷阱与避免方式

```
陷阱 1：过度动画
    问题：每一个元素都在动，页面变成"游乐场"，用户无法聚焦
    解决：一页只挑 3 个交互签名，其余元素静态或仅有 fade-in

陷阱 2：滚动过重
    问题：太多 ScrollTrigger，导致滚动时丢帧
    解决：pin 元素不超过 2-3 个；用 batch() 代替独立 ScrollTrigger

陷阱 3：颜色过多
    问题：品牌想要"活跃" → 10 种颜色混战
    解决：严格执行 3 色原则（背景 + 文本 + 强调色），可加 1 个次要区分色

陷阱 4：WebGL 不可用
    问题：WebGL 失败时 hero 变成空白
    解决：准备 SVG/图片 fallback，在 shader orb 初始化失败时显示

陷阱 5：忘记移动端
    问题：桌面端超炫，移动端崩
    解决：在 `window.matchMedia('(max-width: 768px)')` 时：
          - 关闭所有 heavy shader
          - 关闭 magnetic button（手指没有指针）
          - 降级为 CSS transition（无 GSAP scrub）

陷阱 6：字体尺寸超出设计
    问题：标题用 px 导致超大屏看不到
    解决：**所有标题使用 clamp(min, vw, max)** — 永远不用纯 px
```
