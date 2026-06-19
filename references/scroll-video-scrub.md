# 滚动控制视频 / Kinetic Scroll 体验设计指南

> 针对"滚动时人物/物体向前运动，反向滚动时向后回退（快退感），配合光影变化"的高级动态背景网站。
> 源自 Nike、Apple、vivo iQOO、DJI、Polestar 等品牌案例与 GSAP ScrollTrigger 视频 scrub 最佳实践的逆向拆解。

---

## 目录

1. [模式定义](#模式定义)
2. [三种实现方案](#三种实现方案)
3. [光影效果配方](#光影效果配方)
4. [性能与可访问性](#性能与可访问性)
5. [常见陷阱](#常见陷阱)
6. [代码配方](#代码配方)

---

## 模式定义

**Kinetic Scroll / Scroll Video Scrub** 指：用户的滚动输入被映射为时间轴控制信号，驱动视频、图像序列或 3D 场景中角色/物体的运动。

核心特征：

- **双向控制**：向下滚动 = 正向播放；向上滚动 = 反向播放（快退/回放）
- **帧精确**：画面与滚动位置严格绑定，而非自由播放的视频
- **光影同步**：阴影方向、环境光强度、镜头光晕随运动方向/速度变化
- **沉浸背景**：通常作为全屏背景或巨幅视觉焦点

---

## 三种实现方案

### 方案 A：HTML5 Video Scrub（最简单）

| 优点 | 缺点 | 适用 |
|------|------|------|
| 实现简单，直接使用 `<video>` | 反向 seek 不流畅；移动设备兼容性差 | 短片段、低帧率、概念验证 |
| 文件体积小（单个 MP4） | 大分辨率视频解码压力大 | 桌面端为主 |

**技术要点**：
- 视频必须 `preload="auto"` 且关键帧间隔短（keyframe every frame 最佳）
- 使用 GSAP ScrollTrigger 更新 `video.currentTime`
- 监听 `loadedmetadata` 后再启动滚动绑定

### 方案 B：图像序列帧动画（最稳定）

| 优点 | 缺点 | 适用 |
|------|------|------|
| 任意方向精确 scrub，无解码卡顿 | 文件总体积大 | 运动类品牌站、产品发布 |
| 可逐帧控制，适合运动员动作 | 需要预加载管理 | 高帧率、高质量场景 |

**技术要点**：
- 预加载所有帧（显示进度条）
- 用 `<canvas>` 绘制当前帧
- 图片格式：WebP > JPEG > PNG
- 帧率建议：24-30fps；时长 3-8 秒 → 72-240 帧

### 方案 C：WebGL / Three.js 角色动画（最灵活）

| 优点 | 缺点 | 适用 |
|------|------|------|
| 无限时长、无限方向、光影可控 | 制作成本高 | 高端产品站、游戏、虚拟人 |
| 可实时合成背景、粒子、阴影 | 需要 3D 资产 | 预算充足的品牌项目 |

**技术要点**：
- 骨骼动画（glTF）+ `animationMixer.setTime(scrollProgress * duration)`
- 角色在 3D 场景中前后移动，相机跟随
- 动态灯光、阴影贴图、后处理 bloom

---

## 光影效果配方

### 1. 速度感径向模糊（Radial Motion Blur）

- 滚动速度越快，画面边缘径向模糊越强
- 实现：Canvas 2D 或 WebGL shader，根据 `scrollVelocity` 调整 blur radius

### 2. 方向性阴影（Directional Shadow）

- 角色/物体下方有一个动态阴影
- 向前运动时阴影拉长并偏移到后方
- 向后运动时阴影反向偏移
- 实现：CSS `box-shadow` / `drop-shadow` 插值，或 Three.js shadow map

### 3. 速度线 / 粒子拖尾（Speed Lines & Trail）

- 快速滚动时在背景生成水平速度线
- 或让粒子向运动反方向飞逝
- 实现：Canvas 2D 粒子系统，根据速度改变粒子 `vx`

### 4. 镜头光晕 / Bloom（Lens Flare & Bloom）

- 高亮区域（如太阳、灯光）随运动产生拖尾光晕
- 实现：WebGL post-processing UnrealBloomPass，或 CSS `box-shadow` 模拟

### 5. 暗角加深（Vignette Intensity）

- 高速运动时暗角加深，聚焦中心主体
- 实现：CSS `radial-gradient` opacity 随速度变化

---

## 性能与可访问性

### 必须做

1. **预加载**：图像序列必须全部加载完成后再启动 scrub，否则画面撕裂
2. **`prefers-reduced-motion`**：降级为静态海报图 + 简单淡入文字
3. **移动端降级**：
   - 视频 scrub 在 iOS Safari 上性能极差，建议移动端切换为自动播放静音视频或静态帧
   - 图像序列在移动端减少分辨率或帧数
4. **帧率预算**：桌面端保持 60fps；复杂 WebGL 场景移动端 30fps
5. **LCP 警觉**：首屏不要放超大视频/序列，先用 poster 占位

### 不要做

- 不要把整个长视频（>10MB）用于 scrub
- 不要在没有预加载的情况下直接滚动
- 不要忽略触控设备的惯性滚动

---

## 常见陷阱

| 陷阱 | 表现 | 解决 |
|------|------|------|
| **视频 seek 卡顿** | 滚动时画面不连贯，掉帧 | 改用图像序列；或确保视频每帧都是关键帧 |
| **方向感知错误** | 向上滚动时人物继续向前 | 正确计算滚动增量方向并反转播放 |
| **移动端崩溃** | 大序列图导致内存不足 | 移动端降低分辨率、减少帧数、使用视频替代 |
| **光影不同步** | 阴影/光晕与运动方向相反 | 把光照参数也绑定到滚动进度 |
| **缺少 fallback** | 低端设备白屏或长时间黑屏 | 预加载进度条 + 静态 poster 兜底 |

---

## 代码配方

### 配方 1：HTML5 Video Scrub

```html
<video id="scrub-video" preload="auto" muted playsinline style="position:fixed; inset:0; width:100%; height:100%; object-fit:cover; z-index:-1">
  <source src="athlete.mp4" type="video/mp4">
</video>
```

```javascript
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger)

const video = document.getElementById('scrub-video')

video.addEventListener('loadedmetadata', () => {
  gsap.to(video, {
    currentTime: video.duration,
    ease: 'none',
    scrollTrigger: {
      trigger: '.scroll-track',
      start: 'top top',
      end: 'bottom bottom',
      scrub: 0.5
    }
  })
})
```

### 配方 2：Canvas 图像序列 Scrub

```javascript
const canvas = document.getElementById('scrub-canvas')
const ctx = canvas.getContext('2d')
const totalFrames = 120
const frames = []
let loaded = 0

// 预加载
for (let i = 0; i < totalFrames; i++) {
  const img = new Image()
  img.src = `frames/athlete_${String(i).padStart(4, '0')}.webp`
  img.onload = () => {
    loaded++
    if (loaded === totalFrames) initScrub()
  }
  frames.push(img)
}

function initScrub() {
  ScrollTrigger.create({
    trigger: '.scroll-track',
    start: 'top top',
    end: 'bottom bottom',
    scrub: 0.3,
    onUpdate: (self) => {
      const frameIndex = Math.min(totalFrames - 1, Math.floor(self.progress * totalFrames))
      ctx.clearRect(0, 0, canvas.width, canvas.height)
      ctx.drawImage(frames[frameIndex], 0, 0, canvas.width, canvas.height)
    }
  })
}
```

### 配方 3：动态光影（速度 + 方向）

```javascript
let lastProgress = 0
let velocity = 0

ScrollTrigger.create({
  trigger: '.scroll-track',
  start: 'top top',
  end: 'bottom bottom',
  scrub: 0.3,
  onUpdate: (self) => {
    const delta = self.progress - lastProgress
    velocity = delta * 1000 // 滚动速度
    lastProgress = self.progress

    // 方向：1 = 向前，-1 = 向后
    const direction = Math.sign(delta)

    // 阴影偏移
    const shadowOffset = direction * Math.min(Math.abs(velocity) * 2, 40)
    document.documentElement.style.setProperty('--shadow-x', `${shadowOffset}px`)

    // 暗角强度
    const vignetteOpacity = Math.min(Math.abs(velocity) * 0.5, 0.7)
    document.documentElement.style.setProperty('--vignette-opacity', vignetteOpacity)
  }
})
```

```css
.athlete-shadow {
  filter: drop-shadow(var(--shadow-x, 0) 20px 30px rgba(0,0,0,0.6));
}
#vignette {
  background: radial-gradient(circle at center, transparent 0%, rgba(0,0,0,var(--vignette-opacity, 0.3)) 100%);
}
```

### 配方 4：Three.js 角色动画 Scrub

```javascript
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'

const mixer = new THREE.AnimationMixer(character)
const action = mixer.clipAction(gltf.animations[0])
action.play()

ScrollTrigger.create({
  trigger: '.scroll-track',
  start: 'top top',
  end: 'bottom bottom',
  scrub: 0.5,
  onUpdate: (self) => {
    const duration = action.getClip().duration
    mixer.setTime(self.progress * duration)

    // 角色在场景中前后移动
    character.position.z = self.progress * 20

    // 阴影方向
    const dir = Math.sign(self.getVelocity())
    shadowLight.position.x = -dir * 5
  }
})
```

---

## Sources

[cite:1] [GSAP ScrollTrigger Scrub Video](https://gsap.com/docs/v3/Plugins/ScrollTrigger/?tab=demo-scrub-video) — GSAP 官方视频 scrub 示例

[cite:2] [How to Create a Scroll-based Video Scrubbing Effect](https://www.creativebloq.com/features/scroll-based-video-scrubbing-effect) — Creative Bloq 实现指南

[cite:3] [Scroll-Triggered Video Playback Techniques](https://www.smashingmagazine.com/2025/03/scroll-triggered-video-playback-techniques/) — Smashing Magazine 2025 年滚动视频技术对比

[cite:4] [DJI Avata 2 官网 scroll-driven experience](https://www.dji.com/global/avata-2) — 产品发布页滚动驱动视觉

[cite:5] [vivo iQOO 12 产品页 scroll video](https://www.vivo.com.cn/vivo/iqoo12) — 手机产品页滚动视频 scrub 案例

[cite:6] [Nike Air Max Day 2025 Scroll Experience](https://www.nike.com/airmax) — 运动员/产品滚动驱动案例

[cite:7] [Apple AirPods Pro 页面滚动叙事](https://www.apple.com/airpods-pro/) — 产品故事滚动驱动
