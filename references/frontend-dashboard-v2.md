---
版本: v2.0
分类: 前端开发
子分类: 数据可视化大屏
推荐技术栈: Vue 3 + TypeScript + ECharts 5
替代技术栈: React 18 + TypeScript + D3.js + Vite
典型应用: 销售监控大屏、运营数据看板、智慧城市大屏、实时监控中心
复杂度: 中高
平均代码量: 800-1500 行
预估开发时间: 快速原型 30min / 生产级 3h
---

# 前端 - 数据可视化大屏提示词模板 v2.0

## 📋 场景概述

数据可视化大屏是将关键业务指标（KPI）以图表、地图、数字等形式在大面积显示屏上集中展示的应用。通常用于指挥中心、监控中心、运营中心等场景，需要在视觉上有科技感，在功能上支持实时数据刷新和多维度展示。

适用项目：
- 销售数据大屏（销售趋势、区域分布、品类分析）
- 运营监控大屏（用户量、服务器状态、业务指标）
- 智慧城市/园区大屏（人流、能耗、基础设施状态）
- 实时数据大屏（物流追踪、生产监控、交易实时）

典型用户：
- 企业管理者、运营团队（快速获取业务全景）
- 指挥中心人员（监控异常、实时决策）

---

## 🎭 角色设定（Persona）

你是一位拥有 8 年经验的前端数据可视化工程师，精通 ECharts、AntV、D3.js 等主流可视化库。

你的核心能力：
- 精通图表设计与数据映射，能将业务数据转化为直观的可视化
- 熟悉大屏适配与布局，擅长处理 1080p/2K/4K 等多种分辨率
- 熟悉实时数据流处理（WebSocket、SSE）与动画性能优化
- 有丰富的科技感/企业级视觉设计经验

你的工作风格：
- 优先顺序：**可读性 > 性能 > 视觉炫酷 > 代码简洁**
- 你重视：数字是大屏的核心价值，图表和样式都是为了让数字更容易被理解
- 你避免：过度使用动画影响性能、颜色过多导致视觉干扰、为了炫酷而牺牲清晰度

---

## 🏗️ 项目架构（Context）

### 技术栈选项

**A方案（推荐，单文件快速运行）**
- Vue 3（CDN 引入，无需构建）
- ECharts 5.x（CDN 引入，图表库）
- 原生 CSS Grid / Flexbox 布局
- 适用场景：快速原型、演示项目、无需后端的独立大屏

**B方案（生产级工程化）**
- Vue 3.4 + TypeScript + Vite
- ECharts 5.x + echarts-for-vue
- Pinia（状态管理）+ Tailwind CSS
- 适用场景：长期维护项目、需要团队协作开发

### 推荐目录结构（A方案 - 单文件）

```
project-root/
├── index.html              # 主入口，包含 HTML 结构
├── css/
│   └── style.css          # 布局和样式
├── js/
│   ├── app.js            # 图表初始化和数据逻辑
│   └── mock-data.js      # 模拟数据生成器
└── README.md             # 项目说明
```

### 推荐目录结构（B方案 - 工程化）

```
project-root/
├── src/
│   ├── components/
│   │   ├── charts/       # 图表组件（LineChart/BarChart/PieChart/MapChart）
│   │   └── layout/       # 布局组件（Header/KpiCard/GridPanel）
│   ├── composables/      # useChart, useWebSocket 等组合式函数
│   ├── stores/           # Pinia 状态管理（全局数据存储）
│   ├── utils/            # 工具函数（数字格式化、颜色、时间处理）
│   ├── types/            # TypeScript 类型定义
│   └── mock/             # Mock 数据生成器
├── tests/                # 测试文件
└── README.md
```

### 代码规范

- 命名规范：组件大驼峰（KpiCard.vue），文件短横线分隔，常量全大写
- TypeScript：strict 模式，禁止 any（除非是第三方库类型问题）
- 格式：ESLint + Prettier，2 空格缩进，单引号，无分号

---

## 🎯 核心功能需求（Task）

### 🔴 Must Have（必须实现）
- [x] **KPI 数字卡片**：展示 4-6 个核心指标，显示数值、单位和同比变化
- [x] **趋势折线图**：展示时间序列数据，支持多条线对比（如本月 vs 上月）
- [x] **分类柱状图/条形图**：展示各区域/各品类的数值对比
- [x] **占比饼图/环形图**：展示各部分占比关系
- [x] **自适应布局**：在 1280x720 到 3840x2160 范围内均可正常显示
- [x] **数据模拟**：提供合理的 Mock 数据，模拟真实业务规律（非纯随机）

### 🟡 Should Have（建议实现）
- [ ] **地图可视化**：中国地图或区域地图热力图，展示各省份/城市数据
- [ ] **实时数据流**：WebSocket 或轮询，数据定时刷新（默认 5 秒）
- [ ] **主题切换**：深色/浅色模式（默认深色科技风）
- [ ] **图表交互**：鼠标悬停 tooltip、点击联动筛选、图例切换显示
- [ ] **加载状态**：图表加载时显示 skeleton/loading 动画

### 🟢 Could Have（可选增强）
- [ ] **时间筛选器**：支持选择"今日/本周/本月/自定义时间范围"
- [ ] **数据导出**：图表数据导出为 CSV/PNG
- [ ] **大屏全屏模式**：一键切换全屏展示
- [ ] **告警系统**：关键指标超过阈值时高亮/闪烁/声音提示
- [ ] **拖拽布局**：用户可自定义各模块位置

---

## 📐 设计约束（Constraints）

### 性能约束（必须量化）
- 首屏 LCP < 2.0s（1920x1080, Chrome, 4G 网络）
- JS 总大小 < 300KB（gzip 后，单文件方案可放宽至 500KB）
- 单个 ECharts 图表初始化 < 100ms（数据量 < 1000 条时）
- 页面滚动/切换图表时保持 60fps
- 内存占用 < 200MB，连续运行 24h 无明显泄漏

### 布局约束
- 主分辨率设计：1920x1080（16:9），可等比缩放到 4K
- 采用 CSS Grid 作为主布局，Flexbox 做子项布局
- 模块间距：统一 15px（大屏上视觉间距约 1.5 度视角）
- KPI 卡片区域高度：120-150px（占屏幕 10-15% 高度）
- 图表区域：保持 1:1 或 4:3 宽高比，避免极端比例

### 视觉与设计系统约束
- 主色调：#00d4ff（科技蓝）
- 背景色：#0a0e27（深空蓝，渐变至 #1a1f4a）
- 文字色：主文字 #ffffff，次要文字 #8b9cbd，辅助文字 #4a5568
- 语义色：
  - 成功/上升：#52c41a
  - 警告/中性：#faad14
  - 危险/下降：#ff4d4f
- 数字字体：`'DIN Alternate', 'Menlo', monospace`（等宽对齐）
- 正文字体：`-apple-system, BlinkMacSystemFont, 'PingFang SC', sans-serif`
- 字号系统：12px / 14px / 16px（正文）/ 24px / 32px / 48px（KPI 大数字）
- 圆角：4px（小）/ 8px（卡片）/ 12px（大容器）
- 边框：rgba(0, 212, 255, 0.2) 1px

### 数据格式约束
- 数字：> 1000 用千分位（如 1,234,567），小数点统一保留 2 位
- 百分比：x.xx%，正负号显示（如 +12.34% / -5.67%）
- 时间：HH:mm:ss 24小时制
- 日期：YYYY-MM-DD

### 动画约束
- 入场动画：图表入场使用 0.5s ease-out 动画
- 数据更新：变化动画 0.3-0.8s，数据大时取低值避免卡顿
- 禁止：
  - 无限循环的装饰动画（可能导致性能问题）
  - 超过 3 种不同的动画 easing
  - 文字动画（影响可读性）

### 兼容性约束
- 浏览器：Chrome 90+ / Firefox 90+ / Safari 14+
- 分辨率：1920x1080（主）/ 2560x1440 / 3840x2160
- 最低兼容：1280x720（可能需要横向滚动，但内容不能丢失）

---

## 📤 输出要求（Output Format）

### 文件结构（A方案 - 单文件可运行）

```
/dashboard-demo/
├── index.html          # 主入口（HTML 结构 + 内联 CSS/JS 引用）
├── css/style.css      # 布局 + 样式（约 200-300 行）
├── js/app.js          # 主逻辑：图表初始化、数据更新、事件绑定
├── js/mock-data.js    # Mock 数据生成函数
└── README.md          # 项目说明（功能、结构、如何修改数据）
```

### 文件结构（B方案 - 工程化）

如用户明确需要工程化项目，请使用此结构，否则默认使用 A方案（单文件）

### 代码风格要求

- [ ] 使用原生 ES6+，需要时引入 CDN 库
- [ ] 每个函数必须有 JSDoc 注释：描述 + @param + @returns
- [ ] 复杂逻辑（如数据转换算法）必须有行内注释解释思路
- [ ] 避免全局变量污染：使用 IIFE 或 ES Modules（如果环境支持）
- [ ] 遵循单一职责原则：一个函数做一件事
- [ ] 配置与逻辑分离：颜色、尺寸、时间间隔等为常量，便于修改

### README 要求

README.md 必须包含：
1. **项目简介**：一句话描述 + 截屏或示意图
2. **功能特性**：列表形式，已实现的功能（勾选已实现的 Must/Should/Could Have）
3. **快速开始**：如何运行（A方案直接双击打开，B方案 npm install + npm run dev）
4. **技术栈**：列出主要依赖和版本
5. **目录结构**：简要说明
6. **如何修改数据**：Mock 数据在哪里修改，如何接入真实 API
7. **已知限制**：当前版本的约束或 TODO

### 单文件可运行要求（A方案默认启用）

- 用户下载后在浏览器中直接打开 index.html 即可运行
- 不依赖任何构建工具（npm/webpack/vite）
- 使用 CDN 引入 Vue 和 ECharts（使用稳定版本号，如 5.4.3）

---

## ✅ 验收标准（Acceptance Criteria）

格式：Given-When-Then

- Given 用户在 1920x1080 的 Chrome 浏览器中打开 index.html
  When 等待页面完全加载（window.onload）
  Then LCP < 2.0s，且所有图表在 3 秒内可见

- Given 页面已加载完成
  When 用户将窗口从 1920x1080 调整为 1280x720
  Then 所有图表和 KPI 卡片自动适配新尺寸，无内容溢出和横向滚动条

- Given 图表正在显示数据
  When 用户将鼠标悬停在某个数据点上
  Then 在 100ms 内显示 tooltip，内容清晰，不被页面边缘截断

- Given 页面显示的是销售趋势折线图（含 30 天数据）
  When 用户查看 X 轴日期标签
  Then 日期标签应完整显示（如 06-01），不重叠，不被裁掉

- Given 页面已经加载完成
  When 等待 30 秒（模拟长时间展示场景）
  Then 页面无 JS 报错，数据至少完成 3 次自动刷新，图表平滑更新

- Given 网络断开
  When 页面无法连接到数据源（如果是真实后端）
  Then 显示友好的"数据连接异常"提示，并尝试自动重连（指数退避）

---

## 🚫 反模式清单（What NOT to Do）

请严格避免以下行为：

- ❌ **不要把图表当作纯装饰**：每个图表必须有明确的业务含义。如果不知道为什么需要这个图表，就不要它
- ❌ **不要使用 3D 图表**（如 3D 饼图、3D 柱状图）：容易误导数据解读且性能差
- ❌ **不要在一个页面放超过 8 个图表**：信息过载反而降低决策效率。除非是专门设计的密集信息大屏
- ❌ **不要使用超过 5 种主色**：颜色应该有语义（成功/警告/危险），而非随意使用彩虹色
- ❌ **不要使用过小的字体**：大屏展示距离远，最小正文 14px，数字不小于 18px
- ❌ **不要忽略移动端适配**：即使主要目标是大屏，也应保证在 1280x720 下基本可用
- ❌ **不要用 setTimeout 同步图表状态**：使用 Promise、事件回调或响应式系统管理数据
- ❌ **不要每个图表都使用动画入场**：仅在首屏需要视觉冲击，后续更新禁用动画或使用极短时长
- ❌ **不要硬编码颜色值到每个图表中**：颜色统一在配置对象中定义为常量
- ❌ **不要使用 eval、innerHTML、document.write**：安全风险且性能差。使用安全的 DOM 操作

---

## ⚠️ 常见陷阱与规避（Common Pitfalls）

### 陷阱 1：大屏等比缩放适配
**问题**：在 1920x1080 设计好后，放到 4K 大屏上要么有白边，要么被拉伸变形

**规避方法**：
```css
/* 坏代码：固定像素，不能缩放 */
.chart { width: 500px; height: 300px; }

/* 好代码 A：百分比 + 最小/最大限制 */
.chart {
  width: 100%;
  min-width: 400px;
  height: 100%;
  min-height: 200px;
}

/* 好代码 B：使用 transform scale 整体缩放（推荐用于大屏） */
.dashboard {
  width: 1920px;    /* 设计稿宽度 */
  height: 1080px;   /* 设计稿高度 */
  transform-origin: top left;
  transform: scale(var(--scale, 1));
}
/* JS 根据实际窗口大小计算 scale = Math.min(windowW/1920, windowH/1080) */
```

### 陷阱 2：ECharts 图表窗口缩放后尺寸错误
**问题**：用户调整窗口后，ECharts 实例不知道容器尺寸变化，继续使用旧尺寸

**规避方法**：
```javascript
// 必须做：监听 resize 并调用 chart.resize()
const chart = echarts.init(dom);
window.addEventListener('resize', () => {
  // 加 debounce，避免频繁 resize
  clearTimeout(chart._resizeTimer);
  chart._resizeTimer = setTimeout(() => chart.resize(), 200);
});

// 必须做：页面隐藏/显示时处理
document.addEventListener('visibilitychange', () => {
  if (!document.hidden) chart.resize();
});

// 必须做：组件卸载时清理（Vue/React）
// beforeUnmount(() => { chart.dispose(); window.removeEventListener(...) })
```

### 陷阱 3：地图数据依赖问题
**问题**：ECharts 5.x 不再内置中国地图。使用 require('echarts/map/js/china.js') 在不同环境中可能失败

**规避方法（3选1）**：
```javascript
// 方案 A（推荐）：使用 DataV.GeoAtlas 的免费地图 JSON
fetch('https://geo.datav.aliyun.com/areas_v3/bound/100000_full.json')
  .then(r => r.json())
  .then(geoJson => {
    echarts.registerMap('china', geoJson);
    // 然后初始化地图图表
  });

// 方案 B：把地图 JSON 保存到本地文件，避免依赖外部 CDN

// 方案 C（最简）：如果地图不是核心功能，用柱状图替代
// 用"省份-数值"的横向柱状图，同样能表达区域差异，且零依赖
```

### 陷阱 4：长时间运行的内存泄漏
**问题**：大屏通常 7x24 小时运行，定时器/事件监听/图表实例不清理会导致内存增长

**规避方法**：
- 所有 setInterval 必须保存 ID，并在需要时 clearInterval
- ECharts 实例必须 dispose()（不要用 init 多个实例覆盖）
- 事件监听器用 addEventListener + removeEventListener 配对
- 用 Chrome DevTools 的 Memory 面板检查 10 分钟内的内存趋势

### 陷阱 5：Mock 数据太假
**问题**：完全随机的数据看起来不真实，影响演示效果（如销售额忽高忽低，没有业务规律）

**规避方法**：
```javascript
// 坏代码：纯随机
const value = Math.random() * 1000000;

// 好代码：模拟真实业务规律
function generateSalesValue(date) {
  const dayOfWeek = date.getDay();
  const isWeekend = dayOfWeek === 0 || dayOfWeek === 6;
  const baseValue = 500000;           // 基准值
  const weekendFactor = isWeekend ? 1.3 : 1.0;  // 周末高30%
  const randomFactor = 0.8 + Math.random() * 0.4;  // ±20% 随机波动
  return Math.round(baseValue * weekendFactor * randomFactor);
}

// 好代码：数据有平滑趋势，不是噪声
function generateTrendData(length) {
  const result = [];
  let value = 100;
  for (let i = 0; i < length; i++) {
    value += (Math.random() - 0.45) * 10;  // 略有上升趋势
    result.push(Math.max(0, value));       // 不低于0
  }
  return result;
}
```

---

## 🧪 测试要求（Testing Requirements）

### 手动测试清单（交付前必须逐项检查）

- [ ] **分辨率测试**：在 1920x1080 / 2560x1440 / 1280x720 三种分辨率检查布局
- [ ] **浏览器兼容**：在 Chrome 和 Firefox 中分别测试
- [ ] **长时间运行**：页面连续运行 10 分钟，检查内存是否持续增长
- [ ] **断网场景**：模拟断网（DevTools > Network > Offline），检查是否有友好提示
- [ ] **慢速网络**：模拟 Slow 3G，检查加载状态和超时处理
- [ ] **图表交互**：悬停 tooltip、点击图例切换、缩放（如果支持）
- [ ] **刷新测试**：连续刷新 5 次，每次都能正常显示
- [ ] **可访问性**：Tab 键可以在模块间焦点切换吗？颜色对比度达标吗？

### 快速自测命令

如果用户有 Node.js 环境：
```bash
# 启动本地服务器（避免 file:// 协议的 CORS 问题）
npx serve .
# 然后访问 http://localhost:3000
```

---

## 💡 最佳实践与优化建议（Best Practices）

按优先级排序：

### P0 - 必须执行（快速原型和生产级都应该做）

1. **图表实例管理**
   - 每个图表容器只 init 一次，后续用 setOption 更新数据
   - 在组件卸载/页面卸载时调用 chart.dispose()
   - 保存图表实例引用到一个集中管理的 Map/数组

2. **合理的 Mock 数据**
   - 数据要有"业务规律感"：工作日 > 周末，白天 > 夜晚，有合理的上下限
   - 数据量级要合理：销售额百万级，订单数千级，百分比 0-100
   - 数字格式统一：千分位、小数点位数、单位

3. **颜色系统统一**
   - 定义一个颜色配置对象，所有图表共享
   ```javascript
   const COLORS = {
     primary: '#00d4ff', success: '#52c41a', warning: '#faad14',
     danger: '#ff4d4f', text: '#ffffff', subText: '#8b9cbd'
   };
   ```

4. **响应式布局正确实现**
   - ECharts 容器必须有明确的 height（不是 0，不是 auto）
   - 使用 CSS Grid 设置布局，图表只需 fill 容器
   - window resize 时统一调用所有图表的 resize()

### P1 - 建议执行（生产级必做，快速原型可选）

5. **动画控制**
   - 首屏入场：有轻微动画增加科技感（0.3-0.8s）
   - 数据刷新：禁用动画或用极短时长（< 100ms），避免每次刷新都在动
   - 在 ECharts option 中设置 `animation: false` 或 `animationDuration: 300`

6. **性能优化**
   - 大数据量（>1000 条）启用 progressive: true
   - 折线图大数据开启 sampling: 'lttb'（降采样）
   - 避免频繁 setOption，每次更新只更新必要的 series 数据
   - WebSocket 收到数据后 buffer 一小段时间再统一更新，不做每秒刷新

7. **错误处理**
   - 图表初始化失败（如容器不存在）要有 try-catch
   - 数据请求失败（如 fetch reject）显示"数据加载失败"占位，不白屏
   - 打印有用的错误消息到 console，便于调试

8. **时间格式化**
   - 使用统一的时间格式化工具，不要到处写 `getMonth() + 1`
   ```javascript
   const fmtTime = d => `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}`;
   ```

### P2 - 可选增强（时间充裕时优化）

9. **导出功能**：提供"导出截图"（ECharts 自带 toolbox）和"导出数据 CSV"
10. **主题切换**：支持深色/浅色模式，默认深色大屏风格
11. **全屏模式**：提供一键全屏按钮（Fullscreen API）
12. **多时间尺度**：今日/本周/本月/自定义时间范围切换
13. **告警系统**：关键指标超过阈值高亮/闪烁，可选声音提示
