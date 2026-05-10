# Heartbeat Link - 孕期进度条母亲节礼物

## 1. Concept & Vision

**Heartbeat Link** (心跳连线) 是一个充满温情的技术创意——作为在外地的程序员，用代码为准妈妈打造一个可交互的孕期陪伴页面。这不仅是冰冷的倒计时，而是一个能感受宝宝胎动、听心跳声的温暖礼物。整体风格柔和、浪漫，带有淡淡的羊水流动感和宝宝踢腿的互动反馈，让妈妈感受到科技背后的爱意。

## 2. Design Language

### Aesthetic Direction
采用**温暖梦幻风**——柔和的粉紫色渐变背景，如同子宫内的安全感。结合羊水粒子流动效果，营造温暖、宁静、被包裹的氛围。

### Color Palette
- Primary: `#E8B4C8` (玫瑰粉 - 代表妈妈的爱)
- Secondary: `#B4D4E8` (天空蓝 - 宝宝的天空)
- Accent: `#F6E6F0` (浅薰衣草 - 梦幻感)
- Background: `#FDF5F7` → `#EBF4FC` (淡粉到淡蓝渐变)
- Text Primary: `#5D4E60` (深紫灰 - 柔和易读)
- Text Secondary: `#8B7B8E` (中紫灰)
- Heartbeat: `#FF6B8A` (心跳红)

### Typography
- 主标题: **ZCOOL XiaoWei** (站酷小薇体) - 中文手写感，温暖
- 正文: **Noto Sans SC** - 清晰易读的无衬线中文
- 数字: **Quicksand** - 圆润可爱的阿拉伯数字
- Fallback: system-ui, sans-serif

### Spatial System
- 基础单位: 8px
- 内边距: 24px (移动端) / 48px (桌面端)
- 卡片圆角: 24px
- 组件间距: 32px

### Motion Philosophy
- **羊水粒子**: 缓慢、柔和的浮动 (2-4s周期)，模拟漂浮感
- **宝宝动作**: 轻微的上下浮动 + 呼吸感，踢腿时产生涟漪
- **心跳**: 120-160次/分，视觉波纹扩散
- **UI过渡**: 300-500ms ease-out，避免突兀

### Visual Assets
- 图标: Lucide Icons (圆润线条)
- 水果插图: 半透明剪影风格
- 装饰: 柔和光晕、心形粒子、渐变圆形

## 3. Layout & Structure

### Page Structure
```
┌─────────────────────────────────────┐
│  [Canvas 全屏背景层 - 羊水粒子效果]    │
│  ┌─────────────────────────────────┐ │
│  │     Header: "Heartbeat Link"   │ │
│  │     副标题: 程序员远程开发 ❤️    │ │
│  └─────────────────────────────────┘ │
│                                     │
│  ┌─────────────────────────────────┐ │
│  │   主卡片: 孕期倒计时             │ │
│  │   "距离卸货还有 XX天 XX小时"     │ │
│  │   "第 XX周 + XX天"              │ │
│  └─────────────────────────────────┘ │
│                                     │
│  ┌─────────────────────────────────┐ │
│  │   宝宝大小展示                  │ │
│  │   [宝宝轮廓] ←→ [水果轮廓]      │ │
│  │   "像一颗 [水果] 那么大"        │ │
│  │   尺寸: XXcm | 体重: XXg        │ │
│  └─────────────────────────────────┘ │
│                                     │
│  ┌─────────────────────────────────┐ │
│  │   每日贴士卡片                  │ │
│  │   "妈妈辛苦了，这周宝宝..."     │ │
│  └─────────────────────────────────┘ │
│                                     │
│  ┌───────┐                          │
│  │ ❤️心跳│  角落胎心模拟器          │
│  └───────┘                          │
│                                     │
│  ┌─────────────────────────────────┐ │
│  │   底部: 爸爸的远程留言          │ │
│  │   "—— 爱你的程序员"             │ │
│  └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### Responsive Strategy
- 移动优先设计 (375px 基准)
- 桌面端最大宽度 480px (手机模拟器效果)
- 所有元素居中，单列布局

## 4. Features & Interactions

### Core Features

#### F1: 精准倒计时
- 实时计算距离预产期的天数、小时数
- 显示当前孕周和天数 (如: 第34周 + 3天)
- 预产期可通过 URL 参数或 localStorage 配置

#### F2: 水果/物体具象化
- 根据孕周匹配水果数据库
- 显示双轮廓对比图 (宝宝 vs 水果)
- 标注长度和重量

#### F3: Canvas 羊水背景
- 柔和的粉紫色粒子流动
- 粒子大小: 2-8px 随机
- 透明度: 0.3-0.7
- 缓慢的随机运动轨迹

#### F4: 宝宝动画
- 轻微的上下浮动 (模拟呼吸)
- 腹部区域可点击触发"踢腿"效果
- 踢腿时: 粒子加速 + 涟漪扩散 + 提示文字

#### F5: 胎心模拟
- 左下角跳动的爱心
- 频率: 120-160 次/分 (随机模拟)
- 点击可听到心跳音效 (Web Audio API)
- 视觉波纹扩散效果

#### F6: 每日贴士
- 根据孕周显示妈妈的身体状态
- 温馨的关怀文案
- 每日自动更新

### Interaction Details

| 交互 | 触发 | 反馈 |
|-----|-----|-----|
| 点击腹部区域 | click/touch | 涟漪扩散 + "宝宝踢了一下!" 提示 |
| 点击心跳 | click | 波纹扩散 + 心跳音效 |
| 页面滚动 | scroll | 卡片淡入动画 |
| 首次访问 | load | 光晕呼吸效果 (2s) |

### Edge Cases
- 预产期未设置: 显示引导页"请输入预产期"
- 已超过预产期: 显示"宝宝已经准备好见面啦!"
- 孕周 < 4周: 显示"宝宝还在路上..."

## 5. Component Inventory

### Component: PregnancyCountdown
- 主卡片，显示倒计时
- States: loading | normal | overdue | ready
- 数字使用 Quicksand 字体，带呼吸动画

### Component: FruitComparison
- 宝宝和水果的对比图
- 双轮廓 + 虚线连接 + 尺寸标注
- States: loading | loaded | error

### Component: DailyTip
- 每日贴士卡片
- 淡紫色背景 + 圆角
- States: normal | highlight (新的一天)

### Component: HeartbeatSimulator
- 角落跳动的爱心
- States: idle | playing (带音效)
- 波纹动画 on click

### Component: AmnioticCanvas
- 全屏 Canvas 背景
- 粒子系统管理
- 响应 resize

### Component: KickFeedback
- 踢腿反馈 Toast
- 3s 自动消失
- 淡入淡出动画

## 6. Technical Approach

### Frontend Stack
- **Framework**: Vue 3 (Composition API) + Vite
- **Styling**: Scoped CSS + CSS Variables
- **Animation**: Canvas API + CSS Transitions
- **Audio**: Web Audio API (心跳音效)

### Project Structure
```
heartbeat-link/
├── public/
│   └── sounds/
│       └── heartbeat.mp3  (心跳音效)
├── src/
│   ├── assets/
│   │   └── styles/
│   │       └── variables.css
│   ├── components/
│   │   ├── AmnioticCanvas.vue    (羊水背景)
│   │   ├── PregnancyCountdown.vue
│   │   ├── FruitComparison.vue
│   │   ├── DailyTip.vue
│   │   ├── HeartbeatSimulator.vue
│   │   └── KickFeedback.vue
│   ├── composables/
│   │   ├── usePregnancyData.ts   (孕周计算逻辑)
│   │   ├── useCanvasParticles.ts (粒子系统)
│   │   └── useHeartbeat.ts       (心跳音效)
│   ├── data/
│   │   └── fruit_data.ts         (40周水果数据库)
│   ├── App.vue
│   └── main.ts
├── .github/
│   └── workflows/
│       └── deploy.yml             (GitHub Actions)
├── index.html
├── package.json
├── vite.config.ts
└── SPEC.md
```

### Data Model
```typescript
interface BabyGrowth {
  week: number;
  fruit: string;
  fruitEmoji: string;
  weight: string;
  length: string;
  tip: string;
}

interface PregnancyStatus {
  daysLeft: number;
  hoursLeft: number;
  currentWeek: number;
  daysIntoWeek: number;
  fruit: string;
  weight: string;
  length: string;
  tip: string;
}
```

### Deployment
- **托管**: GitHub Pages / Vercel / Netlify
- **CI/CD**: GitHub Actions
- **配置**: 通过环境变量 VITE_DUE_DATE 设置预产期

## 7. 40周水果数据参考

| 周数 | 水果 | 长度 | 体重 | 贴士 |
|-----|-----|-----|-----|-----|
| 8周 | 覆盆子 | 1.6cm | 1g | 宝宝开始做鬼脸啦，妈妈要注意补钙哦！ |
| 12周 | 枇杷 | 5-6cm | 17-29g | 宝宝已经成型，开始活动啦！ |
| 16周 | 牛油果 | 12cm | 100g | 宝宝能听到声音了，多和TA说话吧！ |
| 20周 | 香蕉 | 25cm | 300g | 胎动越来越明显，宝宝在打嗝呢！ |
| 24周 | 芒果 | 30cm | 600g | 宝宝有了睡眠周期，妈妈要好好休息！ |
| 28周 | 葡萄柚 | 35cm | 1000g | 宝宝在攒力气，妈妈多吃高蛋白食物！ |
| 32周 | 大白菜 | 40cm | 1700g | 宝宝越来越强壮，胎动有力气啦！ |
| 36周 | 木瓜 | 45cm | 2500g | 宝宝位置下降，很快就能见面啦！ |
| 40周 | 西瓜 | 50cm | 3500g | 随时准备见面！准备好待产包了吗？ |
