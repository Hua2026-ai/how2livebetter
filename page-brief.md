# how2livebetter.net 首页克隆 · 页面简报

> 采集时间：2026-10-12 18:14–18:41 (GMT+8)
> 源站：https://how2livebetter.net/ （Astro 静态站，Cloudflare，lang=zh-CN）
> 证据文件：`source-home.html`、`source.css`、`source-rules.css`、`computed-styles.json`、`responsive-sweep.json`、`docs/design-references/source-*.png`

## 1. 路由映射（本次范围：仅首页）

| 源站 URL | 本地路由 | 文件归属 | 状态 |
|---|---|---|---|
| https://how2livebetter.net/ | `/` | `src/app/page.tsx` + `src/components/sites/how2livebetter/*` | 克隆 |
| /stages/、/stages/*、/moments/*、/chapters/*、/tools/*、/q/*、/about/*、/privacy/ 等 | — | — | **不在范围**，站内链接保留指向源站（同窗口跳转），已在交付说明中披露 |

首个根路由克隆，替换模板占位首页（SKILL 第 1 条允许）。

## 2. 技术栈与字体

- 源站：Astro 编译产物，无框架运行时；交互=3 个原生 module script（主题切换 / consent / q-read 已读标记）
- 字体（Google Fonts，与源站完全一致，改用 next/font 自托管）：
  - `--font-display`: Newsreader 400/500/600 + italic 400（衬线，标题）+ Noto Serif SC 500/600（中文衬线回退）
  - `--font-body`: Plus Jakarta Sans 400/500/600/700（正文）
  - `--font-mono`: JetBrains Mono 400/500/600（等宽，eyebrow/pill/数字）

## 3. 设计 Token（`:root`，暗色 `html[data-theme=dark]` 全套见 how2livebetter.css）

| Token | 亮色 | 暗色 |
|---|---|---|
| --bg-page | #faf9f6 | #141210 |
| --bg-surface | #ffffff | #1d1b18 |
| --bg-recess | #f7f6f2 | #262320 |
| --border-line | #e7e5df | #34302a |
| --text-main | #1c1917 | #ece7de |
| --text-body | #57534e | #b0a99c |
| --text-muted | #78716c | #8b8477 |
| --text-faint | #a8a29e | #6b6459 |
| --sage | #15803d | #4ade80 |
| --sage-deep | #166534 | #22c55e |
| --content-max | 1040px | — |
| --radius | 12px | — |

主题机制：`localStorage.theme` 优先，否则跟随 `prefers-color-scheme` 且实时联动；`meta[name=theme-color]` 同步切换（#faf9f6 / #141210）。**无 FOUC**：源站在 `<head>` 内联同步脚本先行设置 `documentElement.dataset.theme`，克隆用 `beforeInteractive` 脚本复刻。

## 4. 区块顺序与测量（桌面 1440px，实测 computed-styles.json）

1. **site-header**（sticky, h=64, bg=--bg-float 95% + blur(8px), border-bottom）：brand-mark（SVG logo 27px + 18px 衬线字 + 阶段版 mono 徽章）+ site-nav（阶段手册▾ details 下拉 / 场景指南 / 完整手册 / 下载 / 打卡与待办 / 我的清单 / ☰ burger / 40×40 theme-toggle / sage 实心 nav-cta「处境体检 →」）
2. **hero-split**（grid 1.15fr .85fr, gap 48, padding-bottom 48, border-bottom）：左=eyebrow（mono 12px sage 大写）+ hero-title（44px/500/1.18/-0.02em）+ hero-lede（16.5px/--text-body）+ trust-meta（mono 12px，3 项 sage 圆点）+ hero-actions（btn-primary 实心 sage / btn-secondary 白底描边）；右=**paper-slip**（白卡 12px 圆角 + shadow-slip，padding 20/22）：slip-header（mono 11.5 定位 + pill-green「A 级证据」）+ h3 18px + pill-row（4 枚灰 pill）+ slip-plain 14.5 + slip-footer（dashed 上边框，✓ sage mono 印章 + 右侧链接）
3. **怎么用**（section-block, content-flow gap 56）：section-lead（26px 衬线标题 + 14px muted 副题右对齐）+ stepper-rail-4（4 列 gap 28，顶部实线）：step-digit 32px 斜体 sage 数字 + h3 18px + p 14px + step-link
4. **五个阶段**：strip（白卡圆角容器 overflow hidden，5 行 strip-item padding 16/22，行间 border-line）：strip-rank 斜体 sage + h3 16.5（内嵌 mono 12px 年龄）+ p 13px muted + strip-meta（灰 pill「N 条相关」+ sage「查看手册 →」）
5. **懵住的时刻**：pillars-unified（2 列网格，白卡圆角）：8 个 pillar-cell（padding 24/28，右/下 border-line，2n 去右边框、末两行去下边框）：pillar-tag mono 11px 上行 + pill「N 步」+ h3 19px + p 13.5 + pillar-link sage
6. **为什么是工具, 不是又一个阅读器**：editorial-contrast（2 列 gap 48，上下实线 padding 32/0）：passive 侧（h3 muted，− 灰 bullet 3 条）vs active 侧（h3 sage，+ sage bullet 3 条）
7. **关于《高性价比人生指南》**：prose 白卡（padding 30/32），14.5px/1.85 正文 + 3 个链接
8. **或按主题翻完整手册**：section-lead 右侧 strip-action「全部 34 章 →」+ chips（34 枚 sage 浅底 chip，mono 11.5）
9. **colophon-banner**（白卡 padding 34/38）：h2 24px + p 14px + btn-primary「直接开始处境体检 →」
10. **ad-slot**（dashed 边框占位，当前 `display:none`——源站未接入广告时隐藏）
11. **site-foot**（白底 border-top）：foot-grid 5 列（mono 11px 组标题 + 13px muted 链接）+ 3 段 12px faint 说明（署名/免责/版权，外链带 ↗）
12. **consent-banner**（fixed 底部，白底 border-top + 上投影）：13px muted 说明 + 「仅必要」描边 / 「全部接受」text-main 实心；无 localStorage 选择时显示

## 5. 交互清单（trigger → result，均已实测）

| 触发 | 结果 |
|---|---|
| 点 theme-toggle | `dataset.theme` 反转，全部 CSS 变量换套，meta theme-color 同步，写入 localStorage.theme |
| 系统深浅色变化（未手动选过） | 实时跟随 |
| 点「阶段手册 ▾」/「☰」 | details[open] → menu-panel display:block（绝对定位 + shadow-menu） |
| hover nav-link / btn-primary / btn-secondary / strip-item / pillar-cell / menu-panel a | 颜色/背景过渡（.15s），见 CSS |
| consent 按钮 | 写 localStorage.consent-choice，banner 隐藏；footer「Cookie 设置」清除选择并重新弹出 |
| /q/ 链接（纸卡片底部 1 个） | q-read 脚本：localStorage 记录已读，pageshow 时给已读链接的 h3 追加「已读」read-tag |
| 滚动 | html scroll-behavior:smooth；无滚动动画 |

## 6. 响应式（实测 responsive-sweep.json）

- ≤900px：header padding 18、nav gap 12、**burger 出现**（`.site-nav>.nav-link` 与桌面下拉隐藏）
- ≤860px：hero 单列、stepper 单列、pillars 单列、contrast 单列
- ≤720px：strip-item 纵排、foot-grid 2 列
- ≤640px：hero-title 28px、hero-actions 按钮通栏、content-flow padding 32/18/72、section-title 22px、colophon 内边距收紧
- ≤560px：stepper-rail-4 单列
- ≤480px：brand-badge 隐藏、brand 15px、header padding 14
- 320–1920px 全程无横向溢出（docScrollW == clientW）

## 7. 资产映射

| 源 URL | 本地路径 | 备注 |
|---|---|---|
| /_astro/data.Dd7S3L8f.css | 提取首页相关 152 条规则 → `public/sites/how2livebetter/how2livebetter.css` | 已去 Astro scoped 属性；其余页面规则按范围剔除 |
| /favicon.svg | `public/sites/how2livebetter/favicon.svg` | 686B SVG |
| /apple-touch-icon.png | `public/sites/how2livebetter/apple-touch-icon.png` | 180×180 |
| /og-image.png | `public/sites/how2livebetter/og-image.png` | 1200×630 |
| Google Fonts 4 族 | next/font 自托管（同字体同字重） | 消除外部请求 |
| 页面内联 SVG（logo/月/太阳） | 组件内原样保留（currentColor + CSS 变量着色） | 无位图资产 |

源站无 `<img>`、无视频、无 WebGL；视觉全部由 CSS + 内联 SVG 构成。

## 8. 明确不做的部分

- Google Analytics / AdSense / Consent Mode v2 脚本：克隆不含第三方追踪脚本；consent 横幅文字与交互保留，按钮只操作 localStorage
- /q/ 详情页、tools 交互应用（打卡/清单/体检）、其余 34 章与阶段页：不在本次范围，站内链接指向源站
- ICP 备案号（源站 TODO 未填）：保持同样留空
