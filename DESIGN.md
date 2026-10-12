# DESIGN.md

> 清晰、克制、好读 —— 一页讲清「如何真正吸收一篇长文 / 一段视频」。

**项目**：Absorption Learning Coach — 开源 AI 辅导技能落地页  
**受众**：AI 产品经理 / 自学者 / Agent 构建者，需要一份可复用的吸收教练配方  
**页面**：单页落地 `docs/index.html`（GitHub Pages）

---

## 1. Visual Theme & Atmosphere

**Style**: Bright Calm Educational  
**Keywords**: 明亮、留白、教育感、清晰层级、克制动效、可读、可信、单一强调色  
**Tone**: 清晰、克制、好读 — NOT 炫技霓虹、NOT 营销口号堆砌、NOT 暗黑赛博  
**Feel**: 一间采光充足的自习室：桌面整洁，重点用一支靛蓝马克笔标出，没有多余贴纸

**Interaction Tier**: **L2 流畅交互**  
**Dependencies**: CSS only（无 GSAP / 无外部动画库）

**Signature Moments（本页 3 处爆点）**:
1. **Hero 视觉爆点**：纯 CSS 绘制的五步吸收环（Split → Value → Recall → Transfer → Rebuttal）环形示意图
2. **首屏滚动爆点**：解释层级 + 难度自适应的双组卡片（Tiers）
3. **列表区爆点**：Quiz Rules 勾选清单，滚动时逐项 soft fadeInUp

**Clever Detail**：右侧/底部进度点（progress dots）跟随当前 section，轻量导航

---

## 2. Color Palette & Roles

```css
:root {
  /* Backgrounds */
  --bg: #f7f8fc;                          /* 页面主背景：浅灰蓝白 */
  --surface: #ffffff;                     /* 卡片 / 容器 */
  --surface-alt: #eef1f8;                 /* 交替 section */
  --surface-hover: #e8ecf6;               /* 悬停态表面 */

  /* Borders */
  --border: #d8dde8;                      /* 默认边框 */
  --border-hover: #b8c0d4;                /* 悬停边框 */

  /* Text */
  --text: #1a1d26;                        /* 标题、重要文字 */
  --text-secondary: #4a5160;              /* 正文 */
  --text-tertiary: #7a8294;               /* 标签、辅助 */

  /* Accent — indigo / violet 单一强调 */
  --accent: #5b5bd6;                      /* CTA、链接、活跃态 */
  --accent-hover: #4a4ac0;
  --accent-soft: rgba(91, 91, 214, 0.12);
  --accent-ring: rgba(91, 91, 214, 0.28);

  /* RGB variants */
  --bg-rgb: 247, 248, 252;
  --accent-rgb: 91, 91, 214;
  --text-rgb: 26, 29, 38;

  /* Semantic */
  --success: #2f9e74;
  --error: #d14343;
  --warning: #c97b2a;

  /* Loop diagram step colors (all from accent family) */
  --step-bg: var(--accent-soft);
  --step-border: var(--accent-ring);
}
```

**Color Rules:**
- 所有颜色通过 CSS 变量引用，禁止在组件里硬编码 hex
- 同一 section 内只用一个强调色（`--accent`）
- 大面积背景只用 `--bg` / `--surface` / `--surface-alt`；强调色只出现在 CTA、活跃导航、环图节点、勾选图标
- 文字层级严格：标题 `--text`、正文 `--text-secondary`、辅助 `--text-tertiary`

---

## 3. Typography Rules

**Font Stack:**
```css
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;600;700&display=swap');

--font-sans: 'Noto Sans SC', system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;
--font-mono: ui-monospace, 'SF Mono', Menlo, Consolas, monospace;
```

| Role | Font | Size | Weight | Line Height | Letter Spacing |
|------|------|------|--------|-------------|----------------|
| Hero H1 | Noto Sans SC | `clamp(32px, 5vw, 48px)` | 700 | 1.2 | -0.01em |
| Section H2 | Noto Sans SC | `clamp(24px, 3vw, 32px)` | 700 | 1.25 | -0.01em |
| H3 | Noto Sans SC | 18px | 600 | 1.35 | 0 |
| Body | Noto Sans SC | ≥15px（默认 16px） | 400 | ≥1.7 | 0.01em |
| Label / Eyebrow | Noto Sans SC | 13px | 500 | 1.4 | 0.06em |
| Mono / Code | mono stack | 14px | 500 | 1.5 | 0 |

**Typography Rules:**
- 中文为主；行高 body ≥ 1.7；字号 body ≥ 15px
- Heading weight ≥ 600（Hero / H2 为 700）
- **NEVER use**: Arial, Times New Roman, Comic Sans, 霓虹装饰字体、过度斜体艺术字

**Text Decorations:**
- Hero H1：纯色 `--text`，无渐变、无投影
- Section H2：纯色 `--text`，无装饰
- 关键词可用 `--accent` 着色，一屏不超过一处

---

## 4. Component Stylings

### Buttons

```css
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  min-height: 44px;
  padding: 12px 22px;
  border-radius: 9999px;
  font-family: var(--font-sans);
  font-size: 15px;
  font-weight: 600;
  line-height: 1.4;
  text-decoration: none;
  border: 1.5px solid transparent;
  cursor: pointer;
  transition: background 0.2s ease, color 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease, transform 0.15s ease;
}

.btn-primary {
  background: var(--accent);
  color: #fff;
  box-shadow: 0 1px 2px rgba(var(--accent-rgb), 0.25);
}
.btn-primary:hover {
  background: var(--accent-hover);
  box-shadow: 0 4px 14px rgba(var(--accent-rgb), 0.28);
}
.btn-primary:active { transform: translateY(1px); }
.btn-primary:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px var(--accent-ring);
}

.btn-secondary {
  background: var(--surface);
  color: var(--accent);
  border-color: var(--border);
}
.btn-secondary:hover {
  border-color: var(--accent);
  background: var(--accent-soft);
}
.btn-secondary:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px var(--accent-ring);
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}
```

### Cards

```css
.card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 24px;
  box-shadow: 0 1px 2px rgba(var(--text-rgb), 0.04);
  transition: border-color 0.2s ease, box-shadow 0.2s ease, transform 0.2s ease;
}
.card:hover {
  border-color: var(--border-hover);
  box-shadow: 0 8px 24px rgba(var(--text-rgb), 0.06);
  transform: translateY(-2px);
}
.card:focus-within {
  border-color: var(--accent);
  box-shadow: 0 0 0 3px var(--accent-ring);
}
```

### Navigation

```css
.nav {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(var(--bg-rgb), 0.72);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid transparent;
  transition: background 0.25s ease, border-color 0.25s ease, box-shadow 0.25s ease;
}
.nav.is-scrolled {
  background: rgba(255, 255, 255, 0.92);
  border-bottom-color: var(--border);
  box-shadow: 0 1px 0 rgba(var(--text-rgb), 0.04);
}
.nav a {
  color: var(--text-secondary);
  text-decoration: none;
  font-size: 14px;
  font-weight: 500;
  min-height: 44px;
  display: inline-flex;
  align-items: center;
  padding: 8px 12px;
  border-radius: 8px;
}
.nav a:hover,
.nav a.is-active {
  color: var(--accent);
  background: var(--accent-soft);
}
```

### Links

```css
a.inline-link {
  color: var(--accent);
  text-decoration: underline;
  text-underline-offset: 3px;
  text-decoration-thickness: 1px;
  transition: color 0.15s ease;
}
a.inline-link:hover { color: var(--accent-hover); }
a.inline-link:focus-visible {
  outline: none;
  box-shadow: 0 0 0 3px var(--accent-ring);
  border-radius: 4px;
}
```

### Tags / Badges

```css
.badge {
  display: inline-flex;
  align-items: center;
  padding: 4px 10px;
  border-radius: 9999px;
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.04em;
  background: var(--accent-soft);
  color: var(--accent);
  border: 1px solid var(--step-border);
}
```

### Progress Dots（clever detail）

```css
.progress-dots {
  position: fixed;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 10px;
  z-index: 90;
}
.progress-dots a {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--border);
  border: none;
  min-width: 44px;
  min-height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: transparent;
}
.progress-dots a::before {
  content: '';
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: var(--border);
  transition: background 0.2s ease, transform 0.2s ease;
}
.progress-dots a.is-active::before,
.progress-dots a:hover::before {
  background: var(--accent);
  transform: scale(1.25);
}
@media (max-width: 900px) {
  .progress-dots { display: none; }
}
```

### Quiz Checklist

```css
.checklist li {
  display: flex;
  gap: 12px;
  align-items: flex-start;
  padding: 14px 0;
  border-bottom: 1px solid var(--border);
}
.checklist .mark {
  flex-shrink: 0;
  width: 24px;
  height: 24px;
  border-radius: 6px;
  background: var(--accent-soft);
  color: var(--accent);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  font-weight: 700;
}
```

### Loop Diagram（Hero）

```css
.loop-diagram {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 8px;
  max-width: 560px;
}
.loop-step {
  background: var(--surface);
  border: 1.5px solid var(--step-border);
  border-radius: 12px;
  padding: 12px 8px;
  text-align: center;
  position: relative;
}
.loop-step .num {
  display: inline-flex;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: var(--accent);
  color: #fff;
  font-size: 13px;
  font-weight: 700;
  align-items: center;
  justify-content: center;
  margin-bottom: 8px;
}
.loop-step .label {
  font-size: 12px;
  font-weight: 600;
  color: var(--text);
  line-height: 1.35;
}
```

---

## 5. Layout Principles

**Container:**
- Max width: `1080px`
- Padding: `0 24px`
- Narrow variant (正文密集区): `720px`

**Spacing Scale:**
- Section padding: `clamp(56px, 8vw, 96px) 0`
- Component gap: `16px` / `24px`
- Card internal padding: `24px`（移动端 `20px`）

**Grid:**
```css
.grid-2 {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}
.grid-3 {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}
.grid-tiers {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}
@media (max-width: 800px) {
  .grid-2, .grid-3, .grid-tiers { grid-template-columns: 1fr; }
}
```

**Section Order（单页）:**
1. Nav
2. Hero（名称 + 一句话 + 双 CTA + 五步环图）
3. Problem（冷测 / 答案倾倒为何失败）
4. Method（五步吸收环详解）
5. Tiers（解释层级 + 难度自适应）
6. Rules（Quiz 规则清单）
7. Living Model + Weekly Review
8. Install
9. Footer（MIT）

---

## 6. Depth & Elevation

| Level | Treatment | Use |
|-------|-----------|-----|
| Flat | 无阴影，仅边框 | Section 背景分区、标签 |
| Subtle | `0 1px 2px rgba(text, 0.04)` | 默认卡片、导航 scrolled |
| Elevated | `0 8px 24px rgba(text, 0.06)` | 卡片 hover、Hero 环图容器 |
| Focus ring | `0 0 0 3px var(--accent-ring)` | 键盘焦点 |

不做厚重多层投影；不使用毛玻璃大面积遮挡正文。

---

## 7. Animation & Interaction

**Motion Philosophy**: 只用 opacity + translateY；慢、轻、可忽略 —— 信息优先于动效  
**Tier**: L2（CSS + 少量 IntersectionObserver / scroll 监听）

### Dependencies
无外部动画库。仅 Google Fonts（Noto Sans SC）。

### Entrance Animation
```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(16px); }
  to { opacity: 1; transform: translateY(0); }
}
.reveal {
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.55s ease, transform 0.55s ease;
}
.reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}
.reveal-stagger > *:nth-child(1) { transition-delay: 0.05s; }
.reveal-stagger > *:nth-child(2) { transition-delay: 0.12s; }
.reveal-stagger > *:nth-child(3) { transition-delay: 0.19s; }
.reveal-stagger > *:nth-child(4) { transition-delay: 0.26s; }
.reveal-stagger > *:nth-child(5) { transition-delay: 0.33s; }
```

### Scroll Behavior
```js
/* sticky nav tint */
window.addEventListener('scroll', () => {
  document.querySelector('.nav').classList.toggle('is-scrolled', window.scrollY > 12);
}, { passive: true });

/* IntersectionObserver for .reveal + progress dots */
const io = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (e.isIntersecting) e.target.classList.add('is-visible');
  });
}, { threshold: 0.15, rootMargin: '0px 0px -8% 0px' });
document.querySelectorAll('.reveal').forEach((el) => io.observe(el));

/* section progress dots */
const sections = [...document.querySelectorAll('[data-section]')];
const dots = [...document.querySelectorAll('.progress-dots a')];
const sio = new IntersectionObserver((entries) => {
  entries.forEach((e) => {
    if (!e.isIntersecting) return;
    const id = e.target.getAttribute('data-section');
    dots.forEach((d) => d.classList.toggle('is-active', d.dataset.target === id));
  });
}, { threshold: 0.35 });
sections.forEach((s) => sio.observe(s));
```

### Hover & Focus States
- 卡片：轻微上移 2px + 边框加深
- 按钮：背景变深 / 次要按钮描边变 accent
- 所有可交互元素具备 `:focus-visible` 环

### Special Effects
- Hero 环图：节点编号圆形填充 accent，步骤间用 CSS `→` 或细连接线暗示顺序
- Quiz checklist：逐项 stagger reveal

### Reduced Motion
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
  .reveal {
    opacity: 1;
    transform: none;
  }
}
```

---

## 8. Do's and Don'ts

### Do
- 中文为主文案，术语首次出现可附英文括注
- 所有颜色走 CSS 变量
- 触控目标 ≥ 44px；移动端 ≤600px 无横向溢出
- body 字号 ≥ 15px，行高 ≥ 1.7
- Hero 用 CSS 环图，不用库存照片
- 保留充足留白；一屏一个主信息点
- 尊重 `prefers-reduced-motion`
- 文案原创，克制、具体、可操作

### Don't
- ❌ 霓虹渐变大标题、粒子背景、赛博网格
- ❌ 营销口号堆砌（「颠覆」「革命」「终极」等）
- ❌ GSAP / Three.js / 重型视差（本项目定为 L2 CSS only）
- ❌ 硬编码颜色 hex 散落在组件中
- ❌ 横向滚动条、小于 44px 的可点区域
- ❌ 答案倾倒式长文墙；段落超过 4 行要拆
- ❌ 克隆其他落地页的文案或素材
- ❌ 引入追踪脚本或密钥
- ❌ 暗色主题（本项目明确明亮教育风）
- ❌ 为动效牺牲可读性

---

## 9. Responsive Behavior

**Breakpoints:**
| Name | Width | Key Changes |
|------|-------|-------------|
| Desktop | > 900px | 双栏 Hero；进度点显示；tiers 三列 |
| Tablet | 601–900px | Hero 单栏；进度点隐藏；tiers 可两列或一列 |
| Mobile | ≤ 600px | 单栏；nav 简化；环图 2+2+1 或纵向堆叠；无溢出 |

**Touch Targets:** minimum 44×44px  
**Collapsing Strategy:** 导航链接在窄屏收成「GitHub / 安装」两个主入口；环图改为纵向步骤列表或 2 列网格

```css
@media (max-width: 600px) {
  .loop-diagram {
    grid-template-columns: 1fr 1fr;
  }
  .nav-links { gap: 4px; }
  .nav-links a:not(.nav-priority) { display: none; }
  .hero-actions { flex-direction: column; width: 100%; }
  .hero-actions .btn { width: 100%; }
  body { font-size: 15px; }
  .section { padding: 48px 0; }
}
html { overflow-x: hidden; }
body { overflow-x: hidden; max-width: 100vw; }
```
