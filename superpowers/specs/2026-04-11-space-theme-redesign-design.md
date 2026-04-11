# Space Theme Redesign — Design Spec

**Date:** 2026-04-11
**Project:** allenge 的小站 (MkDocs Material)
**Approach:** Hybrid — NASA 工业风底子 + 电影感点缀

---

## 1. 设计理念

内容页干净克制（NASA 航天工业风），首页和导航区域有电影感光效。两者通过统一的配色系统串联。深色模式为主推体验，浅色模式同样精心设计。

用户可提供地球/星空/Artemis II 任务图片用于首页 Hero。

---

## 2. 配色体系

### 2.1 深色模式（默认/主推）

| 用途 | 色值 | CSS 变量 | 说明 |
|------|------|----------|------|
| 页面背景 | `#0B0B10` | `--md-default-bg-color` | 深空黑，接近纯黑但带微蓝底 |
| 次级背景 | `#12131A` | `--md-default-bg-color--light` | 卡片、侧边栏底色 |
| 提升背景 | `#1A1B26` | `--md-default-bg-color--lighter` | 代码块、admonition 底色 |
| 最亮背景 | `#222436` | `--md-default-bg-color--lightest` | hover 状态背景 |
| 主文字 | `rgba(248,250,252,0.87)` | `--md-default-fg-color` | 星白 |
| 次级文字 | `rgba(148,163,184,0.54)` | `--md-default-fg-color--light` | 灰蓝辅助 |
| 弱文字 | `rgba(148,163,184,0.32)` | `--md-default-fg-color--lighter` | 更弱的辅助 |
| 最弱文字 | `rgba(148,163,184,0.12)` | `--md-default-fg-color--lightest` | 极弱 |
| 主色 | `#3B82F6` | `--md-primary-fg-color` | 发射蓝 |
| 主色亮 | `#60A5FA` | `--md-primary-fg-color--light` | 发射蓝亮 |
| 主色暗 | `#2563EB` | `--md-primary-fg-color--dark` | 发射蓝暗 |
| 主色背景 | `#E0EAFF` | `--md-primary-bg-color` | |
| 强调色 | `#22D3EE` | `--md-accent-fg-color` | 极光青 |
| 强调色透明 | `rgba(34,211,238,0.1)` | `--md-accent-fg-color--transparent` | |
| 代码背景 | `#0D0E18` | `--md-code-bg-color` | |
| 代码文字 | `#E2E8F0` | `--md-code-fg-color` | |
| 代码高亮 | `rgba(59,130,246,0.15)` | `--md-code-hl-color` | |
| 链接色 | `#22D3EE` | `--md-typeset-a-color` | 极光青 |
| 边框 | `#1E293B` | 自定义 | |
| Footer 背景 | `rgba(6,8,14,0.85)` | `--md-footer-bg-color` | |
| Admonition 背景 | `rgba(13,14,24,0.7)` | `--md-admonition-bg-color` | |

### 2.2 浅色模式

| 用途 | 色值 | CSS 变量 | 说明 |
|------|------|----------|------|
| 页面背景 | `#F1F5F9` | `--md-default-bg-color` | 月面灰白 |
| 次级背景 | `#E2E8F0` | `--md-default-bg-color--light` | |
| 主文字 | `#0F172A` | 默认 | 深空蓝黑 |
| 次级文字 | `#475569` | | 中灰蓝 |
| 主色 | `#2563EB` | `--md-primary-fg-color` | 深发射蓝 |
| 主色亮 | `#3B82F6` | `--md-primary-fg-color--light` | |
| 主色暗 | `#1D4ED8` | `--md-primary-fg-color--dark` | |
| 主色背景 | `#FFFFFF` | `--md-primary-bg-color` | |
| 强调色 | `#0891B2` | `--md-accent-fg-color` | 深青 |
| 链接色 | `#2563EB` | `--md-typeset-a-color` | |
| 边框 | `#CBD5E1` | 自定义 | |

### 2.3 对比度验证

- 深色主文字 `#F8FAFC` on `#0B0B10` → ~17:1 (AAA)
- 深色次级文字 `#94A3B8` on `#0B0B10` → ~7:1 (AAA)
- 深色链接 `#22D3EE` on `#0B0B10` → ~10:1 (AAA)
- 浅色主文字 `#0F172A` on `#F1F5F9` → ~15:1 (AAA)
- 浅色链接 `#2563EB` on `#F1F5F9` → ~5.5:1 (AA)

---

## 3. 字体系统

### 3.1 字体选择

| 角色 | 字体 | 字重 | 用途 |
|------|------|------|------|
| 英文标题 | Orbitron | 500-700 | h1、h2、首页 Hero 标题 |
| 正文/中文 | Noto Sans SC | 400-700 | 所有正文、中文标题、UI 文字 |
| 代码 | JetBrains Mono | 400-500 | 代码块、行内代码 |

### 3.2 字号层级

| 元素 | 大小 | 字重 | 字体 |
|------|------|------|------|
| h1 | 2rem | 700 | Orbitron / fallback Noto Sans SC |
| h2 | 1.5rem | 600 | Orbitron / fallback Noto Sans SC |
| h3 | 1.25rem | 600 | Noto Sans SC |
| h4 | 1.1rem | 600 | Noto Sans SC |
| 正文 | 1rem (16px) | 400 | Noto Sans SC |
| 辅助文字 | 0.875rem | 400 | Noto Sans SC |
| 代码 | 0.875rem | 400 | JetBrains Mono |

### 3.3 行高

- 正文：1.7（中文阅读舒适度）
- 标题：1.3
- 代码：1.6

### 3.4 加载方式

通过 Google Fonts 引入：
```
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@500;600;700&family=Noto+Sans+SC:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap');
```

---

## 4. 首页 Hero 设计

提供两种布局供用户本地对比后选择。

### 4.1 布局 A — 全屏 Hero

- 太空图（用户提供）占满 `100vh`
- `object-fit: cover` 确保不同尺寸图片自适应
- 叠加深色渐变遮罩：底部 `rgba(11,11,16,0.85)` → 顶部 `rgba(11,11,16,0.2)`
- 站名居中偏下，Orbitron 大字（3rem+），`text-shadow: 0 0 30px rgba(59,130,246,0.4)`
- Tagline 在站名下方，Noto Sans SC，次级文字色
- 底部向下滚动指示箭头，带 bounce 动画
- 浅色模式下遮罩调整为白色渐变

### 4.2 布局 C — 分割布局

- 左侧 55% 放太空图，`object-fit: cover`
- 右侧 45% 纯深空色 `#0B0B10`
- 右侧内容：站名（Orbitron）、tagline、3-4 个快速导航入口（博客/笔记/项目/关于）
- 导航入口用卡片样式，hover 时发射蓝边框 + 微光
- 整体高度 `100vh`
- 浅色模式：右侧用 `#F1F5F9`

### 4.3 实现方式

通过 MkDocs Material 的 `custom_dir: docs/overrides` 机制实现：

1. 新增 `overrides/main.html`，继承 `base.html`，判断当前页面是否为首页（`page.is_homepage`）
2. 若为首页，在 `content` block 之前插入 Hero 区域的 HTML
3. Hero 区域的样式写在 `cosmic.css` 中（与主题统一管理）
4. 两种布局通过 CSS class 切换（`.hero--fullscreen` / `.hero--split`），用户在 `index.md` 的 meta 中选择

图片路径：用户提供图片后放入 `docs/assets/hero/` 目录。在 Hero HTML 中通过相对路径引用。

---

## 5. Header / 导航栏

### 深色模式
- 背景：`rgba(11,11,16,0.75)` + `backdrop-filter: blur(16px)`
- 底部边框：`1px solid rgba(59,130,246,0.15)`
- 阴影：`0 2px 20px rgba(0,0,0,0.3)`

### 浅色模式
- 背景：`rgba(241,245,249,0.8)` + `backdrop-filter: blur(16px)`
- 底部边框：`1px solid rgba(37,99,235,0.1)`
- 阴影：`0 2px 12px rgba(0,0,0,0.06)`

### Tabs 栏
- 同样毛玻璃处理
- 活跃 Tab：极光青（深色）/ 深发射蓝（浅色）
- hover：颜色过渡 0.2s

---

## 6. 内容页面

### 背景
- 深色：纯深空色 `#0B0B10`，CSS 星点背景（优化分布，更自然）
- 浅色：纯色 `#F1F5F9`，无星点
- 内容区 `md-content`：微弱半透明底色 `rgba(11,11,16,0.4)` 深色 / `rgba(255,255,255,0.6)` 浅色

### 标题
- h1：深色下 `text-shadow: 0 0 20px rgba(59,130,246,0.3)`；浅色下无 shadow
- h2：下方渐变分隔线 `border-image: linear-gradient(to right, #3B82F6, transparent) 1`

### 链接
- 深色：`#22D3EE`，hover 时 `text-shadow: 0 0 8px rgba(34,211,238,0.4)`
- 浅色：`#2563EB`，hover 时颜色加深

### 代码块
- 深色：底色 `#0D0E18`，边框 `1px solid rgba(59,130,246,0.12)`，圆角 8px，阴影
- 浅色：底色 `#E8ECF2`，边框 `1px solid rgba(37,99,235,0.1)`，圆角 8px
- 行内代码同理适配

### Admonition
- 深色：`rgba(13,14,24,0.6)` + `backdrop-filter: blur(8px)` + 蓝色系边框
- 浅色：`rgba(255,255,255,0.8)` + 蓝色系边框
- 标题栏：`rgba(59,130,246,0.08)` 深色 / `rgba(37,99,235,0.06)` 浅色

### 表格
- th：发射蓝低透明度底色
- tr hover：微弱蓝色底色

---

## 7. 星空背景（深色模式 only）

- 保留 CSS radial-gradient 星点，优化为更自然的分布
- 闪烁动画保留但降低幅度（opacity 0.5 ↔ 0.8，比当前 0.3 ↔ 1 更克制）
- 底层渐变：`linear-gradient(160deg, #0B0B10 0%, #0F1225 40%, #0B1020 70%, #0B0B10 100%)`
- 添加少量彩色星点：发射蓝 `rgba(59,130,246,0.6)` 和极光青 `rgba(34,211,238,0.5)`
- 浅色模式下完全不显示

---

## 8. 其他组件

### Footer
- 深色：毛玻璃 + 顶部 `1px solid rgba(59,130,246,0.1)`
- 浅色：淡灰底 `#E2E8F0` + 顶部 `1px solid rgba(37,99,235,0.08)`

### 搜索框
- 深色：`rgba(13,14,24,0.8)` 底 + 蓝色边框，focus 时极光青边框
- 浅色：白底 + 蓝灰边框，focus 时发射蓝边框

### 滚动条
- 6px 宽，深色下蓝色系 thumb，浅色下灰蓝 thumb

### 回到顶部按钮
- 毛玻璃 + 蓝色边框
- hover 时极光青光晕 `box-shadow: 0 0 12px rgba(34,211,238,0.3)`

### Content Tabs
- 活跃 label：极光青（深色）/ 发射蓝（浅色）

---

## 9. 文件变更清单

| 文件 | 操作 | 说明 |
|------|------|------|
| `docs/stylesheets/cosmic.css` | 重写 | 全新配色 + 效果 |
| `mkdocs.yml` | 修改 | 更新 palette、extra_css（加字体）、可能调整 primary/accent |
| `docs/overrides/home.html` | 新增 | 首页 Hero 模板（两种布局） |
| `docs/overrides/main.html` | 新增 | 首页专用 main 模板，判断首页时加载 Hero |
| `docs/index.md` | 修改 | 添加模板标记，配合 Hero 布局 |
| `docs/assets/hero/` | 新增目录 | 放置用户提供的太空图片 |
| `docs/stylesheets/fonts.css` | 新增 | 字体引入和基础字体设置 |

---

## 10. 不在范围内

- 内容修改（文章正文不动）
- 新增页面或栏目
- JavaScript 交互（除首页滚动箭头外）
- 移动端专门的响应式大改（MkDocs Material 自身已处理）
