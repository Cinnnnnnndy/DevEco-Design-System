---
name: deveco-design-system
description: 使用 DevEco 双设计系统（鸿蒙原生端 HarmonyOS NEXT HDesign 2.0 与 PC/Mac 当代桌面端 IntelliJ NewUI）构建或重构 IDE 页面与组件。当需要搭建 DevEco Studio、DevEco Code、鸿蒙原生 IDE 界面、工具窗口、代码编辑器、弹窗或使用 design-system/ 时触发此 skill。
---

# DevEco 设计系统 Skill（v2 · Pixso 矢量 AST 全量对齐版）

人和任意 AI 模型（Claude / Codex / Cursor / Gemini）共用同一份规范。构建或重构 DevEco Studio / DevEco Code / 鸿蒙原生 IDE 界面时必须遵循本 Skill，确保跨模型搭建依然保持 Pixso 原稿级高还原度。

## 必读顺序（AI 建模前必须先读）

1. **本文件 (`design-system/SKILL.md`)**：两套系统归属、铁律、标准页面脚手架与自动化门禁脚本。
2. **[scaffolds/scaffolds.json](scaffolds/scaffolds.json)**：12 套源自 Pixso 原稿的页面级脚手架（整窗：`S1` IDE 主工作台 / `S2` AI Agent 会话工作台 / `S5` 欢迎页；分屏：`S10` 代码+预览左右分屏 / `S11` Diff 对比分屏 / `S12` 上下诊断分屏；弹窗：`S3` 双栏设置弹窗 / `S6` 标准表单弹窗 / `S7` 危险告警弹窗；抽屉与面板：`S4` 无顶栏工具窗 / `S8` 右侧滑出抽屉 / `S9` 全局命令面板）——**写任何页面、分屏或弹层前必须先选脚手架，严禁从零手拼布局**。
3. **[icons/manifest.json](icons/manifest.json)** & **[icons/sprite.svg](icons/sprite.svg)**：995 个 `icon.pix` 原生 SVG 图标（零重绘 + CSS Token 绑定）。
4. **[references/quick-reference.md](references/quick-reference.md)**：`data-system`、`data-theme`、22 个核心组件 `.de-*` class 与 Token 速查。
5. **[preview/index.html](preview/index.html)**：一页总览（含全量色卡、995 图标库 `#icons`、5 套页面脚手架 `#scaffolds`、双系统工作台 `#specimens`、组件文档 `#components` 与 6 组 Pixso 原稿对照 `#compare`）。

---

## 一、两套设计系统选择规则（MUST）

写任何页面或组件前，**必须在容器根节点显式声明 `data-system` 与 `data-theme`**：

| 系统名 | 根节点属性 | Pixso 真相源 | 几何尺寸与视觉特征（严禁混用） |
| :--- | :--- | :--- | :--- |
| **鸿蒙原生端 (HDesign 2.0)** | `data-system="harmony"`<br>`data-theme="dark" \| "light"` | `鸿蒙IDE_v1.1.0_PC端组件库.pix`<br>`👉🔥【2.0】基础范式🔥👈.pix`<br>`DevEco Studio NEXT-AI IDE.pix` | • 正文 `16vp`（行高 `22px`），补充 `14vp`，标题 `20vp`<br>• 主色浅 `#0A59F7` / 深 `#317AF7`；面板底浅 `#F1F3F5` / 深 `#202224`；代码编辑区浅 `#FFFFFF` / 深 `#131416`<br>• 顶栏高 `44px`，底栏高 `32px`，侧边图标条宽 `44px`<br>• 按钮高 `34px` `r=8px`（次按钮 `10%` 叠加底配蓝字，告警按钮 `10%` 底配红字）<br>• 输入框 / 搜索 / 下拉 / 树行 / 菜单项高 `28px` `r=4px`<br>• 代码页签高 `28px` 胶囊形 `r=6px`（选中 `10%` 叠加底）<br>• 弹窗 `r=16px` + `Elevation 55` 阴影 |
| **PC / Mac 当代桌面端 (NewUI)** | `data-system="pc-mac"`<br>`data-theme="dark" \| "light" \| "light-contrast"` | `【NewUI】鸿蒙IDE-当代.pix`<br>`icon.pix`<br>`DevEco Studio（当代）chat.pix`<br>+ 只读输入 `deveco-intui-kit/` | • 正文 `13px`（行高 `16px/18px`），小字 `12px`<br>• 官方色阶 `Light/Dark Gray 1–14`；主色 `#3574F0`；深色面板 `#2B2D30`、编辑器 `#1E1F22`、列表选中 `#2E436E`<br>• 顶栏高 `40px`，底栏高 `24px`，侧边图标条宽 `40px`<br>• 按钮 / 输入框高 `28px`（小号 `24px`）`r=4px`，次按钮带 `1px` 描边<br>• 树行高 `24px` `r=4px`；菜单项高 `26px`<br>• 代码页签高 `40px` 矩形，选中态底部 `3px` 蓝色指示条 |

---

## 二、保证高还原度的五条铁律（MUST & MUST NOT）

### 1. 严禁手写私有组件 CSS，100% 走 `tokens/index.css` 与 `.de-*` 类名
- 页面头部必须引入 `design-system/tokens/index.css`。
- 若页面是分析报告或规范卡片页，外框统一引入 `deveco-intui-kit/src/tokens.css` + `site/launcher.css`（复用 `.page`、`.mast`、`.sidebar .cat`、`.card` 岛屿卡片），内部样张舞台挂载 `<div data-system="..." data-theme="...">`。
- **禁止**在页面的 `<style>` 或内联 `style` 中硬编码 `#xxx` 十六进制颜色、私有圆角或私有阴影。所有表面色走 `var(--surface-*)`，文字走 `var(--text-*)`，描边走 `var(--line)` / `var(--border-control)`。

### 2. 直接复制标准页面脚手架（12 套 Pixso 原稿核准骨架，严禁从零手拼）
其他模型搭建任何页面、分屏、弹窗或抽屉前，**必须先从 `design-system/scaffolds/scaffolds.json` 中选取对应骨架直接复制**，并严格遵守下表经 Pixso 矢量 AST 实测核准的像素基准：

| 分类 | 脚手架 ID | Pixso 原稿实测像素基准（不得随意缩放） |
| :--- | :--- | :--- |
| **整窗工作台** | `S1 · ide-workbench`<br>`S2 · ai-chat-workbench`<br>`S5 · welcome-screen` | • `Main Window (1536×960)`：窗口 `r=10px`，NewUI 顶栏 `40px` / 状态栏 `28–29px` / 左右 Stripe `40–41px`（鸿蒙顶栏 `44px` / 状态栏 `32px` / Stripe `44px`）<br>• `TUI / Chat (1440×800)`：顶部 `40px` + 侧栏日期分组 `.de-nav__label` + 单行截断 `.de-nav__item`，**严禁叠加 `.de-studio`**<br>• `Welcome Screen (797×649)`：左侧导航宽 `220px`（原稿 `221px`） |
| **分屏场景** | `S10 · editor-preview-split`<br>`S11 · diff-review-split`<br>`S12 · top-bottom-split` | • 左右分屏 `.de-split-pane` + 垂直分割柄 `.de-splitter (4px)`；上下分屏 `.de-split-pane--vertical` + 横向分割柄 `.de-splitter--h (4px)`<br>• Diff 对比严格走 Pixso `replaceRed` (`.de-diff-line--del`) 与 `replaceGreen` (`.de-diff-line--add`) |
| **设置与弹窗** | `S3 · split-settings-dialog`<br>`S6 · standard-form-dialog`<br>`S7 · alert-confirm-dialog` | • `Settings (883×606)`：左侧树含搜索框、宽 `240–283px`，有纵向分栏时**必须保留**顶栏 (`36px`) 与底栏 (`54px`) 1px 分割线<br>• `Modal Form (507×259)` / `鸿蒙 6.1.1`：无纵向分栏时**不加**顶/底分割线；NewUI `r=8px`、鸿蒙 `r=16px` `pad=16/20px`、按钮 `gap=12px`<br>• `Alert (420×172)`：内边距 `20px`，左图标 `28×28` + `12px` 间距，**「不再提示」复选框在正文下方**（不与底栏按钮同行） |
| **抽屉与面板** | `S4 · studio-tool-panel`<br>`S8 · side-drawer`<br>`S9 · command-palette` | • `S4` 是**唯一**合法使用 `.de-window.de-studio` 的无顶栏纯工具窗（严禁放 `.de-titlebar`）<br>• `Side Drawer`：宽 `340px`，固定头/滚动体/固定底 + 左 1px 描边<br>• `Search Everywhere (650×320)`：宽 `650px`，Tab 栏 `40px`，搜索栏 `37px`，**结果行高 `24px`**，底栏 `34–36px` |

#### 骨架 A：鸿蒙原生 IDE 整窗 (`HarmonyIdeWindow`)
```html
<section class="de-window" data-system="harmony" data-theme="dark" style="min-height:480px">
  <!-- 1. 44px 顶栏 -->
  <header class="de-titlebar">
    <span class="de-titlebar__mark">A</span>
    <div class="de-titlebar__group">
      <button type="button" class="de-titlebar__pill">Healthy_life ▾</button>
      <button type="button" class="de-titlebar__pill">⑂ Master ▾</button>
    </div>
    <span class="de-titlebar__spacer"></span>
    <div class="de-titlebar__group">
      <button type="button" class="de-titlebar__pill">💻 MyPC (Designed for phone) ▾</button>
      <button type="button" class="de-titlebar__pill">⊞ Entry ▾</button>
    </div>
    <span class="de-titlebar__spacer"></span>
    <div class="de-window__controls" aria-hidden="true">
      <button type="button" tabindex="-1">🔨</button>
      <button type="button" tabindex="-1" style="color:var(--color-success)">▶</button>
      <button type="button" tabindex="-1" style="color:var(--color-success)">🐞</button>
      <button type="button" tabindex="-1">–</button>
      <button type="button" tabindex="-1" data-close>×</button>
    </div>
  </header>

  <!-- 2. 主体区：左侧图标条 + Project 树 + 右侧代码主区 -->
  <div class="de-window__body">
    <nav class="de-stripe" aria-label="侧边工具条">
      <button type="button" class="de-icon-button de-icon-button--m" aria-label="Project" aria-pressed="true">
        <svg viewBox="0 0 16 16" width="16" height="16" aria-hidden="true"><path d="M1.8 3.5A1.3 1.3 0 0 1 3.1 2.2h3.1c.4 0 .78.18 1.02.5l.86 1.1h4.82A1.3 1.3 0 0 1 14.2 5.1v7.4a1.3 1.3 0 0 1-1.3 1.3H3.1a1.3 1.3 0 0 1-1.3-1.3V3.5Z" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linejoin="round"/></svg>
      </button>
      <button type="button" class="de-icon-button de-icon-button--m" aria-label="Search">
        <svg viewBox="0 0 16 16" width="16" height="16" aria-hidden="true"><circle cx="7" cy="7" r="4.2" fill="none" stroke="currentColor" stroke-width="1.3"/><path d="M10.2 10.2 13.5 13.5" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round"/></svg>
      </button>
    </nav>

    <!-- 目录节点自动渲染矢量文件夹图标；文件节点使用 .de-tree__badge -->
    <aside class="de-nav" aria-label="Project" style="width:220px">
      <div class="de-toolbar" style="padding:0 6px;min-height:36px;background:transparent">
        <span class="de-toolbar__title">Project ▾</span>
      </div>
      <ul class="de-tree">
        <li>
          <details class="de-tree__node" open>
            <summary class="de-tree__row">
              <span class="de-tree__chevron">›</span>
              <span class="de-tree__name"><strong>MyApplication</strong></span>
            </summary>
            <ul class="de-tree__children">
              <li><div class="de-tree__row" data-selected="true"><span class="de-tree__badge">A</span><span class="de-tree__name">EntryAbility.ets</span></div></li>
              <li><div class="de-tree__row"><span class="de-tree__badge" data-ext="cpp">C</span><span class="de-tree__name">BadgeCardComponent.cpp</span></div></li>
              <li><div class="de-tree__row"><span class="de-tree__badge" data-ext="json">&#123;&#125;</span><span class="de-tree__name">module.json5</span></div></li>
            </ul>
          </details>
        </li>
      </ul>
    </aside>

    <div class="de-main">
      <div class="de-tabs" role="tablist" data-tabs>
        <div class="de-tab" data-selected="true">
          <button type="button" role="tab" class="de-tab__main" aria-selected="true"><span class="de-tree__badge">A</span> EntryAbility.ets</button>
          <button type="button" class="de-tab__close" aria-label="关闭">×</button>
        </div>
      </div>
      <div class="de-code-editor">
        <div class="de-code-alert-card"><span style="color:var(--color-success)">✓</span> 0 告警 · ArkTS Linter</div>
        <div class="de-code-line"><span class="de-code-gutter">1</span><span class="de-code-content"><span class="kw">import</span> UIAbility <span class="kw">from</span> <span class="str">'@ohos.app.ability.UIAbility'</span>;</span></div>
        <div class="de-code-line" data-current="true"><span class="de-code-gutter">2</span><span class="de-code-content"><span class="kw">export default class</span> <span class="fn">EntryAbility</span> <span class="kw">extends</span> UIAbility &#123;&#125;</span></div>
      </div>
      <footer class="de-statusbar">
        <span class="de-crumb">Healthy_life › entry › src › main › ets › <strong>EntryAbility.ets</strong></span>
        <span class="de-statusbar__group">
          <span class="de-lsp-pill"><span class="de-lsp-pill__dot">2</span> LSP Service</span>
          <span>134/2,275</span>
          <span>UTF-8</span>
        </span>
      </footer>
    </div>
  </div>
</section>
```

#### 骨架 B：模态弹窗与表单控件 (`Dialog` + `Select` + `Switch` + `Button`)
```html
<div class="de-dialog">
  <header class="de-dialog__header">
    <h3 class="de-dialog__title">新建鸿蒙模块</h3>
  </header>
  <div class="de-dialog__body">
    <div class="de-field">
      <span class="de-field__label">Module Name</span>
      <label class="de-input" data-state="error">
        <input type="text" value="entry_invalid!" aria-label="Module Name">
      </label>
      <div class="de-error-tip">模块名称仅支持字母、数字与下划线（4.6报错tips）</div>
    </div>
    <div class="de-field">
      <span class="de-field__label">Device Type</span>
      <span class="de-select">
        <select aria-label="Device Type"><option>Phone / Tablet / 2in1</option></select>
      </span>
    </div>
    <div class="de-row">
      <label class="de-choice"><input type="checkbox" class="de-checkbox" checked> <span>启用 Stage 模型</span></label>
      <label class="de-choice"><input type="checkbox" role="switch" class="de-switch" checked> <span>自动生成 Native C++</span></label>
    </div>
  </div>
  <footer class="de-dialog__footer">
    <button type="button" class="de-button de-button--secondary de-button--m">Cancel</button>
    <button type="button" class="de-button de-button--primary de-button--m">Confirm</button>
  </footer>
</div>
```

### 3. 图标库规范（MUST USE Pixso SVG + Token，严禁重新绘制）
- 所有 IDE 图标已从本地 `icon.pix` 矢量 AST 无损提取为 995 个官方 SVG（354 个鸿蒙 NEXT + 641 个 PC/Mac NewUI），并自动将单色/语义色绑定至 `currentColor`（`var(--icon)`）与 `var(--color-info / success / warning / error)`：
  - 雪碧图：`design-system/icons/sprite.svg`
  - 检索清单：`design-system/icons/manifest.json`
- **禁止模型手写或重绘 `<svg><path .../></svg>`**；必须通过 `<svg class="de-svg-icon"><use href="../icons/sprite.svg#<id>"/></svg>` 引用，或直接拷贝 `manifest.json` 中对应图标的 Token 化 `<svg>`。

### 4. 保护区、样式隔离与高频防坑硬规则（MUST FOLLOW）
- **严禁修改 `deveco-intui-kit/` 目录下的任何文件**。
- **严禁再用操作系统自带的「文件管理器」「备忘录」「记事本」充当 DevEco IDE 窗口**。
- **严禁使用 `Inter` 或 `Helvetica` 字体**；UI 字体必须走 `--font-ui`（HarmonyOS Sans SC），代码字体走 `--font-mono`（JetBrains Mono）。
- **严禁把鸿蒙 AARRGGBB（如 `#E5000000`）当成 CSS `#RRGGBBAA` 直接写进样式**。
- **宿主样式穿透隔离**：任何文档页/外层壳禁止写裸标签样式（如 `footer`、`p`、`code`、`hr`）去影响 `[data-system]` 内部；组件预览画布（`.stage` / `.comp-stage` / `.workbench-stage`）必须始终使用自身 `[data-theme]` 对应的 `var(--surface-window)`，绝不继承外层页面的浅/深背景，杜绝反色（白底白字或黑底黑字）。
- **小尺寸徽标与图标居中**：任何高度 ≤ 24px 的徽标/图标按钮（如 `.de-tree__badge`、`.de-titlebar__mark`、`.de-menu__icon`）必须使用 `display: inline-flex; align-items: center; justify-content: center; line-height: 1;`，严禁省略 `line-height: 1` 导致继承行高下溢贴底。
- **严禁 Emoji / Unicode 伪图标与手绘 `<svg>`**：不得使用 `🔨 ▶ 🐞 💻 ⊞ ⑂ ✦ ⚠ 🔖 ⌕ ■` 等符号替代图标，也不得手写 `<svg><path .../></svg>`；一律引用 `design-system/icons/sprite.svg#<pixso-id>`。
- **交互切换控件顺序固定**：所有双系统切换栏统一为 **左 `harmony`（鸿蒙 2.0）、右 `pc-mac`（PC/Mac NewUI）**，主题顺序统一为 **深色 → 浅色 → 高对比**，禁止随默认卡片翻转位置。
- **色卡与 Token 必须全量直出**：展示色板时直接从源 JSON（`pc_lib-styles.json`、`tokens.json`）全量生成，避免只做局部摘要导致缺失色阶。
- **`.de-window` 与 `.de-studio` 严格互斥**：只要窗口包含顶栏 `.de-titlebar`，就**只能使用 `.de-window` + `.de-window__body`**（顶栏在上，下方图标条/侧栏/主区横向排列），**严禁再叠加 `.de-studio`**；`.de-studio` 仅用于无顶栏的纯横向工具窗（`StudioToolWindow`）。
- **侧栏分组标题与会话条目严格区分**：日期/分类分组标题（如 `Today`、`2026/8/30`）用 `<div class="de-nav__label">`，可点击会话/导航条目必须用 `<button class="de-nav__item"><span class="de-nav__text">...</span></button>`（自动单行省略号截断），**严禁把 `.de-nav__label` 用在或嵌套在可点击条目里**。
- **图标颜色继承铁律**：`.de-svg-icon` 必须保持 `color: inherit`（禁止在图标类本身写死 `color: var(--icon)`），保证蓝色激活态、主按钮、深色顶栏内图标自动反白。
- **AI Agent IDE 实战组件与工程细节（吸收自 `gitcode.com/openharmony-sig/deveco-code`）**：
  - 工具调用折叠卡用 `.de-tool-call`（`<details>` + `.de-tool-call__trigger`），工具报错重试用 `.de-tool-error`，Diff 增删统计用 `.de-diff-changes`（配 5 格红绿条），快捷键键帽统一用 `<kbd class="de-kbd">`，会话 Token 占用用 `.de-context-pill`，AI 思考流光用 `.de-shimmer`，轻提示用 `.de-toast-inline`。
  - **Agent 四层 GUI 对话元件体系（共 38 类，参考 DevKit TUI 四层架构并针对 GUI IDE 深化）**：
    - `01 对话流基元（16 类）`：`.de-unit-user`（不折）、`.de-chat-bubble`、`.de-unit-thinking`（默认单行）、`ContextToolGroup`、`.de-unit-action`（固定 24px 三段式）、`.de-unit-result`（封顶 200px 内滚）、行内 Diff 预览卡、`.de-unit-plan`、`.de-unit-approval`（永不折）、`.de-tool-error`（永不折）、`.de-ai-status`、`.de-progress`、`.de-compaction`、`.de-unit-footer`、GUI 多媒体/真机预览卡、门禁验证证据卡。
    - `02 编排类元件（8 类）`：角色归属徽标、`.de-subagent` 子 Agent 任务卡、并行泳道摘要、后台任务胶囊、跨 Agent 交接分隔线、审批队列条、`.de-phase-bar` 阶段时间线、中断恢复锚点。
    - `03 输入区单元（6 类）`：`.de-composer` 多行富输入栏、上下文引用芯片栏、Agent/Plan/Ask 模式与模型选择器、`.de-queued` 队列预备条、`.de-context-pill` 环形上下文占用、斜杠命令补全条。
    - `04 页面级元件（8 类）`：会话头部状态栏、会话侧栏运行蓝点/待审琥珀点（`.de-dot-status`）、折叠时机偏好、权限策略开关、MCP/技能连接开关、变更文件审查头、快捷键速查条、底栏全局状态锚点。
  - 所有耗时、Token 计数、Diff 行数必须声明 `font-variant-numeric: tabular-nums` 防止数字跳动；动效必须提供 `@media (prefers-reduced-motion: reduce)` 降级。

---

## 三、跨模型高还原度验证闭环（Automated Fidelity Gate）

任何模型完成页面编写或修改后，**必须按顺序执行以下两步验证才允许汇报完成**：

### 第一步：运行自动化门禁脚本（0.2 秒 CLI 静态与 Token 检查）
```bash
node design-system/scripts/verify-fidelity.js <你的页面路径.html>
```
该脚本会自动拦截（14 项硬门禁）：
1. 未引入 `tokens/index.css` 或缺失 `data-system` / `data-theme` 声明；
2. 误用 `Inter` 字体或引用了不存在的 `--*` CSS 变量；
3. `.de-*` 组件上硬编码了私有 HEX 颜色，或误把鸿蒙 8 位 `AARRGGBB` 当 CSS 颜色；
4. 页面存在断链或缺失图片/静态资源；
5. 手绘 `<svg>` 或使用 Emoji/Unicode 伪图标（`🔨 ▶ 🐞 💻 ⊞ ⑂ ✦ ⚠ 🔖 ⌕ ■`）；
6. 编造不存在的 `sprite.svg#<id>` 图标 ID（实时核对 `icons/manifest.json` 995 个真实 ID）；
7. 系统切换按钮顺序颠倒（必须左鸿蒙、右 PC/Mac）；
8. `<style>` 中出现未限域的裸 `footer`/`header`/`hr` 穿透规则，或 `inline-grid` 缺少 `line-height: 1` 导致小徽标下溢错位；
9. `.de-svg-icon` 写死 `color: var(--icon)` 导致反色按钮内图标变灰；
10. 带 `.de-titlebar` 的标准窗口误加 `.de-studio` 导致顶栏被挤到左侧，或将 `.de-nav__label` 错用于会话条目内。

### 第二步：浏览器实测与深浅双主题走查
1. 在浏览器中打开页面，分别切换 `data-theme="dark"` 与 `data-theme="light"`（以及 `data-system="harmony"` 与 `data-system="pc-mac"`）。
2. 对照 `design-system/preview/index.html#compare` 中的 Pixso 原稿帧，确认：
   - 顶栏、状态栏、页签栏、横幅均保持单行不折行（`white-space: nowrap`）；
   - 深色模式下弹窗标题（`.de-dialog__title`）、次级说明文字（`--text-secondary` >= 4.5:1）、菜单快捷键对比度清晰可读；
   - 工程树目录节点带有矢量文件夹图标，文件节点带有彩色扩展名徽标（`.de-tree__badge`）。


---

## 九、Agent 对话流与 AI 体验融合硬规范（源自 `Code&Studio体验融合.pix`）

构建 DevEco Code / Studio AI 对话、Subagent 编排、组合输入区与 Session 管理时，必须严格遵守以下从 Pixso 官方稿件抽取的视觉与状态机铁律：

1. **AI 正文回答严禁套卡片（`.de-unit-prose`）**：无底色、无边框、无圆角，直接裸排在对话流画布上；仅用户提问（`.de-unit-user`）使用 `r=8` 无描边浅底色块作为每轮起点锚点，且最后一条提问支持点击原地编辑（与底部输入框共存）。
2. **Deep Thinking 严禁使用螺旋 Logo（`.de-unit-thinking`）**：仅保留 `16×16` 折叠箭头（`sprite.svg#hm-ic-ide-arrow`）+ `Deep Thinking` + 耗时 `(已思考 5分30秒)`；思考中内容区高度上限 **`160px`**，手动展开无上限，思考结束后自动收起。
3. **工具调用无卡片化（`.de-unit-toolcall`）与代码块折叠规则**：工具调用/检索过程（如 `正在搜索... 代码库 已搜索 120 个文件`）**严禁套外框卡片与底色**，采用「`20px` 标题行 + 左侧 `1px` 竖线 + 最多 **5 行** `20px` 文件行（`max-height: 100px` 贴底滚动）」，调用结束自动折叠，多次调用统一收纳折叠；代码块（`r=8` 细边框卡）默认 **5 行**，展开上限 **10 行**；手动滚动离开底部时出现 `32×32` 圆形去底部按钮（`.de-scroll-bottom`）。
4. **Task / Plan 待办与变更文件联动**：待办清单严格三态（规划态点击无效 / 执行态绿完成可定位、白黑进行可跟随、灰未执行无反应 / 完成态全绿）；出现修改文件列表（`680×28 r=4` + `全部拒绝/全部接纳`）后底部待办清单**自动折叠**；停止生成后保留进度并给「继续生成」入口。
5. **互动式意图确认三类分级**：① **强交互**（阻塞、不可跳过，全步骤有效后 `Submit` 激活）；② **弱交互**（默认项 + 倒计时自动执行，可取消）；③ **通知类**（不阻塞）。
6. **Subagent 并行协作卡片流**：顶部折叠条 `并行协作 · 当前对话 · N 运行中 · M 已完成 · K 失败`，**全部结束后自动折叠**；单卡含状态点 + 名称 + 模型标签 + **严格单行简介**；滚出视口时输入框上方提示，主结束子仍跑时提供「一键结束全部」。
7. **输入区与 Token 三段阈值**：模式分段 `Goal / Plan / Build` + 模型下拉 + `默认审批` + 4 类附件芯片 + 底部 `10px` 免责行；Token 圆环 `1%~75%` 亮灰 → `≥75%` 告警色（70%~80% 静默压缩）→ `100%` 输入框置灰禁用（悬浮支持编辑上限，最小 `50K`）。
8. **Session 侧栏（最小宽 `300px`）与 AI 设置**：会话 5 态（未开始 / 运行中 / 已暂停待确认 / 已结束 / 异常未读）+ 4 鼠标态（失焦 / 悬浮出停止或删除 / 聚焦 / 锁定失焦），收藏区按收藏瞬时时间排序，普通区按天切分且运行中置顶。
9. **补齐的 10 类 `NEW` 基本单元（共 4 层 48 类）**：对话流基元补齐检查点回滚（Checkpoint）、引用来源证据链（Citations）、末条提问原地编辑态、结构化产物卡（Artifact Card）；编排类补齐上下文压缩/记忆写入撤销条、并发审批队列批量决策条；输入区补齐生成中排队消息条（`.de-queued`）、`/` 斜杠命令补全浮层；页面级补齐模型供应商校验行、规则/记忆/归档管理行（均在预览页标注 `.de-tag--new` `NEW` 徽标）。
10. **AI 输出内容的结构性处理与渲染优化（`NEW`）**：
    - **固定四段节奏**：`结论先行（1–2 句）→ 结构化依据（文件行号芯片 / 紧致清单 / 指标表 / 受限代码块）→ 风险 Callout（左 3px 竖线轻量提示）→ 1–3 枚可点击后续动作芯片（.de-next-actions）`。
    - **标题与排版克制**：AI 正文内标题封顶 `H3/H4`（`13.5px–14px` 粗体），禁止出现大号 `H1/H2` 抢占视觉层级。
    - **实体自动链接化**：文件路径与行号（如 `EntryAbility.ets:13`）自动渲染为可点击 `.de-file-link` 芯片直跳编辑器，与普通行内 `<code>` 视觉分离。
    - **代码块与表格治理**：代码块默认 **5 行**、展开上限 **10 行**，超长整文件输出自动转为 **Artifact 产物卡**；表格（`.de-prose-table`）支持横向滚动、数值列 `tabular-nums` 右对齐。
    - **流式输出防抖**：未闭合代码块提前占位外壳、未闭合表格稳定行高；用户手动上滚时锁定视口不再抢焦点，底部浮现 `32×32` 去底部按钮。


---

## 十、Pixso 全 12 画布全量复查补漏规则（源自 `Code&Studio体验融合.pix`）

针对 `视觉飞机稿`、`飞机稿`、`Chat对话`、`Session管理`、`AI设置`、`登录+安装&引流`、`930版本`、`典型页面` 全 12 个画布复查补齐的精确交互边界：

1. **对话区宽度与工具输出上限**：对话内容区最大宽约 `600px`（约占 2/3）；工具运行中单行三段式为「加载态图标 + 固定工具名 + 单行动态步骤」；文件检索展开上限 **5 行**（超 5 项且悬停才出滚动条，新文件贴底），结果输出上限 **7 行**；无参工具仅留名称痕不展开，云端知识库可展开看引用链接。
2. **Diff 确认权限边界**：仅**最新一轮**修改文件支持文件级「接纳/拒绝」（悬浮显单文件操作，异常文件常驻），历史轮次未确认文件仅可打开浏览；单个 Diff 块不允许单独拒绝，仅支持单独接纳或批量拒绝/接纳。
3. **意图确认弹窗量化规范**：与输入框等宽且吸附其上方，弹出期间禁用输入框的添加文件/输入/切 Agent；单步选项 **1–9 个**（视口最多显 5 项，6–9 项滚动），总步骤 **≤5 步最佳（上限 9 步）**；按钮 **2–3 个**（确定居右、撤销居左；多步骤不提供跳过机制）。
4. **排队、异常与通知中心**：排队数字动态递减，轮到或手动/异常停止即消失（不驻留对话流）；InlineChat 排队不显具体数字；网络/设备/权限等不可修复异常仅提示原因不给修复按钮；聚合任务进度用「通知与决策中心」（收起 `2 task` / 展开 `task + 2 todo`）。
5. **Session 唯一性与筛选器**：全列表仅允许存在 **1 个**未开始的空对话且永远置顶；类型筛选器支持 `全部 / 待办 / 运行中 / 已完成`（筛选态隐藏按天时间印记）；卡片悬浮外露「停止生成」，`...` 菜单收「重命名 / 取消收藏 / 归档 / 停止并删除」。
6. **模型下拉、提示词库、迁移与登录空态**：模型下拉分「搜索(吸顶) / 列表(最多 5 条滚动) / 添加(吸顶)」，收藏模型后收藏在上；提示词库最多收藏 **3 个**（满额置灰提示）；未登录时 AI 设置子项可点但仅显登录链接；首次登录需勾选隐私条款，930 集成版安装结果弹窗 **5s 自动消失**；AI 卡片响应式最小宽：单行 `1062px` / 双行 `564px` / 三行 `496px`。
