# 3D 产品展示技术栈 / 3D Product Showcase Tech Stack

> 适用：汽车、消费电子、家具、珠宝等需要"实物感"的产品展示页面
> 代表项目：小米 SU7 官网 (gamemcu.com/su7) · Apple Vision Pro 产品页 · 蔚来 ET5 官网

---

## 技术架构

```
FRONTEND
├─ Vue 3 + TypeScript + Vite（推荐）
│   或 React + TypeScript + Vite
│   或 单文件 HTML + ES Modules CDN（快速原型）
│
├─ 3D 渲染核心
│   ├─ three.js@0.163+              — WebGL 渲染引擎
│   ├─ kokomi.js                    — three.js 封装层（可选但推荐，简化场景搭建）
│   └─ @react-three/fiber           — React 用户可选（Three.js 的 React renderer）
│
├─ 3D 加载与压缩
│   ├─ GLTFLoader                   — 加载 GLTF/GLB 模型
│   ├─ DRACOLoader                  — DRACO 压缩模型解码（体积减少 90%+）
│   └─ MeshoptDecoder               — 网格优化解码器
│
├─ 交互控制
│   ├─ OrbitControls                — 轨道相机（旋转/缩放/平移）
│   ├─ Raycaster                    — 鼠标与 3D 对象交点检测（热点标注）
│   └─ GSAP                         — 相机动画、材质过渡、UI 动效
│
├─ 光照与环境
│   ├─ RoomEnvironment              — 室内环境光照（three.js 内置）
│   ├─ PMREMGenerator               — 预过滤环境贴图生成
│   └─ CubeTextureLoader            — HDR/EXR 环境贴图加载
│
├─ 后处理（可选）
│   ├─ postprocessing               — Bloom 辉光、反射地面
│   └─ EffectComposer               — 后处理管线
│
├─ 材质与着色器
│   ├─ MeshStandardMaterial         — PBR 标准材质（金属/粗糙度工作流）
│   ├─ MeshPhysicalMaterial         — 物理材质（清漆、透光、薄膜干涉）
│   └─ ShaderMaterial               — 自定义 GLSL（反射地面、隧道特效）
│
└─ 调试工具
    └─ dat.gui / lil-gui            — 实时参数调试面板（开发阶段）
```

---

## 强制项（Must Have）

| 项目 | 要求 | 原因 |
|------|------|------|
| **模型格式** | GLTF/GLB（首选）或 OBJ+MTL | GLTF 是 Web 3D 标准，支持 PBR 材质、动画、层级 |
| **模型压缩** | DRACO 压缩 + Meshopt | 原始模型 50MB → 压缩后 3-5MB，网页可承受 |
| **PBR 材质** | Metalness/Roughness 工作流 | 真实感车漆、金属、玻璃效果 |
| **环境贴图** | PMREM 预过滤环境贴图 | 产品表面反射真实环境，不是假高光 |
| **响应式** | Canvas 随窗口 resize | 全屏体验，适配各种屏幕 |
| **加载状态** | 进度条 + 骨架屏 | 3D 资源加载需要时间，不能白屏 |
| **降级方案** | WebGL 不支持时显示静态图片 | 覆盖 99.5% 用户，但旧设备需要 fallback |

## 约束项（Must NOT）

| 项目 | 禁止 | 原因 |
|------|------|------|
| **模型面数** | > 100 万面（移动端 > 30 万面） | 帧率低于 30fps 体验崩塌 |
| **纹理尺寸** | > 2048×2048 | 内存占用过大，移动端爆显存 |
| **实时阴影** | 在移动端开启 | 性能杀手，用 baked 阴影贴图替代 |
| **物理引擎** | 除非必要（如碰撞检测） | 增加 bundle 体积和 CPU 负担 |
| **自动旋转** | 速度 > 0.5 rad/s | 用户会感到眩晕，0.1-0.2 rad/s 为宜 |

---

## 颜色系统

```css
:root {
  /* 背景：深黑让产品成为视觉中心 */
  --bg: #0A0A0F;
  --bg-secondary: #141419;

  /* 文本：近白，降低对比度避免刺眼 */
  --text: #E8E8EC;
  --text-muted: #6B6B75;

  /* 强调色：电光蓝（科技感）或霓虹橙（活力感） */
  --accent: #00D4FF;        /* 电光蓝 */
  /* --accent: #FF6B35; */  /* 霓虹橙（备选） */

  /* 产品材质色：由模型本身定义，不硬编码 */
  --product-primary: var(--model-color);
}
```

## 字体系统

| 用途 | 字体 | 字号 | 字重 |
|------|------|------|------|
| 产品名称 | Inter / Roboto | clamp(2rem, 5vw, 4rem) | 300 (极细) |
| 技术参数 | JetBrains Mono / SF Mono | 0.875rem | 400 |
| 按钮/标签 | Inter | 0.75rem | 500 |
| 单位 (km/h, mm) | JetBrains Mono | 与数字同行 | 400 |

> 所有文字使用 sentence case（首字母大写），不用全大写——全大写会让 HUD 看起来像警告牌。

## 性能预算

| 指标 | 目标 | 测量方式 |
|------|------|----------|
| 首屏加载 | < 3s（3G 网络） | Lighthouse |
| 交互响应 | 拖拽旋转 60fps | Chrome DevTools FPS |
| 内存占用 | < 200MB（桌面） | Chrome Task Manager |
| 模型加载 | < 5s（压缩后） | 自定义计时 |
| 总包体积 | < 2MB（不含模型） | webpack-bundle-analyzer |

---

## 关键代码片段

### 1. 基础场景搭建（kokomi.js 风格）

```typescript
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
import { RoomEnvironment } from 'three/examples/jsm/environments/RoomEnvironment'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'
import { DRACOLoader } from 'three/examples/jsm/loaders/DRACOLoader'

// 场景
const scene = new THREE.Scene()
scene.background = new THREE.Color(0x0A0A0F)

// 渲染器
const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true })
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
renderer.setSize(window.innerWidth, window.innerHeight)
renderer.toneMapping = THREE.ACESFilmicToneMapping
renderer.toneMappingExposure = 1.0
renderer.shadowMap.enabled = true
renderer.shadowMap.type = THREE.PCFSoftShadowMap
document.body.appendChild(renderer.domElement)

// 相机
const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 100)
camera.position.set(3, 1.5, 4)

// 环境光照
const pmremGenerator = new THREE.PMREMGenerator(renderer)
scene.environment = pmremGenerator.fromScene(new RoomEnvironment(renderer), 0.04).texture

// 轨道控制
const controls = new OrbitControls(camera, renderer.domElement)
controls.enableDamping = true
dampingFactor = 0.05
controls.minPolarAngle = 0.5
controls.maxPolarAngle = 1.35
controls.enablePan = false
controls.minDistance = 2
controls.maxDistance = 8

// 加载压缩模型
const dracoLoader = new DRACOLoader()
dracoLoader.setDecoderPath('/jsm/draco/')

const gltfLoader = new GLTFLoader()
gltfLoader.setDRACOLoader(dracoLoader)

gltfLoader.load('/models/car.glb', (gltf) => {
  const model = gltf.scene
  model.position.set(0, 0, 0)
  scene.add(model)
})

// 动画循环
function animate() {
  requestAnimationFrame(animate)
  controls.update()
  renderer.render(scene, camera)
}
animate()

// 响应式
window.addEventListener('resize', () => {
  camera.aspect = window.innerWidth / window.innerHeight
  camera.updateProjectionMatrix()
  renderer.setSize(window.innerWidth, window.innerHeight)
})
```

### 2. 材质颜色切换

```typescript
function changeCarColor(color: THREE.Color, roughness: number, metalness: number) {
  carBodyMesh.material.color.lerp(color, 0.1)  // 每帧 lerp 一点
  carBodyMesh.material.roughness = roughness
  carBodyMesh.material.metalness = metalness
}

// GSAP 动画版本
import gsap from 'gsap'

gsap.to(carBodyMesh.material.color, {
  r: targetColor.r,
  g: targetColor.g,
  b: targetColor.b,
  duration: 0.6,
  ease: 'power2.inOut'
})
```

### 3. 热点标注（Raycaster）

```typescript
const raycaster = new THREE.Raycaster()
const mouse = new THREE.Vector2()

window.addEventListener('mousemove', (event) => {
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1

  raycaster.setFromCamera(mouse, camera)
  const intersects = raycaster.intersectObjects(carModel.children, true)

  if (intersects.length > 0) {
    const point = intersects[0].point
    // 在 point 位置显示 HTML 浮层
    showAnnotation(point, intersects[0].object.name)
  }
})
```

### 4. 反射地面（自定义 Shader）

```glsl
// vertex shader
varying vec3 vWorldPosition;
varying vec3 vViewPosition;

void main() {
  vec4 worldPosition = modelMatrix * vec4(position, 1.0);
  vWorldPosition = worldPosition.xyz;
  vViewPosition = -mvPosition.xyz;
  gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}

// fragment shader
uniform sampler2D uReflectTexture;
uniform mat4 uReflectMatrix;
uniform float uRoughness;

varying vec3 vWorldPosition;
varying vec3 vViewPosition;

void main() {
  vec3 viewDir = normalize(vViewPosition);
  vec3 normal = normalize(vec3(0.0, 1.0, 0.0));  // 地面法线

  // 反射向量
  vec3 reflectDir = reflect(-viewDir, normal);

  // Fresnel
  float fresnel = pow(1.0 - max(dot(viewDir, normal), 0.0), 2.0);

  // 反射采样
  vec4 reflectPoint = uReflectMatrix * vec4(vWorldPosition, 1.0);
  reflectPoint /= reflectPoint.w;
  vec3 reflection = texture2D(uReflectTexture, reflectPoint.xy).rgb;

  // 粗糙度模糊（简化版）
  reflection *= (1.0 - uRoughness);

  gl_FragColor = vec4(reflection * fresnel, fresnel * 0.8);
}
```

---

## 模型资源准备指南

### 模型来源

| 来源 | 网址 | 说明 |
|------|------|------|
| Sketchfab | sketchfab.com | 大量免费/付费 3D 模型，支持 GLTF 导出 |
| Poly Haven | polyhaven.com | 免费 HDR 环境贴图 + PBR 材质 |
| 自建 | Blender / C4D / 3ds Max | 导出 GLTF + DRACO 压缩 |

### 导出设置（Blender）

1. **格式**：GLTF 2.0（.glb 二进制格式）
2. **材质**：使用 Principled BSDF，Metalness/Roughness 工作流
3. **压缩**：勾选 "Draco compression"，compression level 6
4. **优化**：
   - 合并相同材质的网格
   - 删除不可见面
   - 纹理尺寸 ≤ 2048×2048
   - 总面数 ≤ 50 万（目标 10-20 万）

---

## 调试清单

- [ ] 模型在 Three.js Editor (threejs.org/editor) 中正常显示
- [ ] DRACO 压缩后的模型体积 < 5MB
- [ ] 桌面端 60fps，移动端 30fps+
- [ ] 拖拽旋转流畅，无卡顿
- [ ] 颜色切换过渡平滑（0.6s）
- [ ] 热点标注位置准确（Raycaster 检测正确）
- [ ] 环境切换时反射自然过渡
- [ ] WebGL 不支持时显示静态图片 fallback
- [ ] 加载进度条显示正常
- [ ] 响应式：手机横屏/竖屏都能正常查看
