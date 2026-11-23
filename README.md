# DevEco Design System

基于本地 Pixso 矢量 AST（133,622 矢量节点）全量提取构建的 **DevEco 双设计系统**，并吸收开源 JetBrains IntelliJ NewUI（ExpUI）与 `openharmony-sig/deveco-code` AI Agent IDE 组件。

## 在线预览（GitHub Pages）

- **设计系统总览页**：https://cinnnnnnndy.github.io/DevEco-Design-System/
- **鸿蒙 HDesign 2.0 全量色卡**：https://cinnnnnnndy.github.io/DevEco-Design-System/#tokens
- **PC/Mac NewUI 全量色阶与字阶**：https://cinnnnnnndy.github.io/DevEco-Design-System/#official-names
- **Pixso 原生图标库（995 个零重绘 SVG + Token）**：https://cinnnnnnndy.github.io/DevEco-Design-System/#icons
- **12 套官方页面脚手架（整窗 / 分屏 / 弹窗 / 抽屉）**：https://cinnnnnnndy.github.io/DevEco-Design-System/#scaffolds
- **23 个核心组件文档与实时工作台**：https://cinnnnnnndy.github.io/DevEco-Design-System/#components
- **跨模型 Skill 与 14 项自动化门禁**：https://cinnnnnnndy.github.io/DevEco-Design-System/#skill-guide

## 双设计系统支持

| 系统 | 根节点声明 | 主题 | 核心特征 |
| :--- | :--- | :--- | :--- |
| **鸿蒙原生端（HarmonyOS NEXT HDesign 2.0）** | `data-system="harmony"` | `dark` / `light` | 正文 16vp，主色 `#0A59F7` / `#317AF7`，顶栏 44px，状态栏 32px，34vp 按钮，28vp 胶囊页签，毛玻璃侧栏 |
| **PC / Mac 当代桌面端（IntelliJ NewUI）** | `data-system="pc-mac"` | `dark` / `light` / `light-contrast` | 正文 13px，8 大官方色系（186 色），顶栏 40px，状态栏 28–29px，28px 按钮，40px 底条页签 |

## 目录结构

```text
.
├── index.html                           # GitHub Pages 根入口（设计系统总览页）
└── design-system/
    ├── SKILL.md                         # 跨模型高还原度 Skill 规范（Claude / Codex / Cursor / Gemini）
    ├── DESIGN.md                        # Pixso 矢量 AST 提取的完整规格表
    ├── tokens/                          # foundation / semantic / components / ai / index.css
    ├── icons/                           # 995 个 icon.pix 原生 SVG 雪碧图 (sprite.svg) 与检索清单 (manifest.json)
    ├── scaffolds/scaffolds.json         # 12 套 Pixso 实测校准页面脚手架 (S1–S12)
    ├── components/                      # React / TSX 组件源码
    ├── docs/                            # 23 个核心组件文档
    ├── compare/                         # 6 组 Pixso 1:1 原稿对照
    ├── examples/                        # 整窗 Pattern 与实战诊断工作台 Demo
    └── scripts/
        ├── verify-fidelity.js           # 14 项自动化还原度硬门禁
        └── extract-pixso-icons.js       # Pixso 矢量 AST 无损图标提取脚本
```

## 快速接入与还原度校验

1. **引入样式并在容器根节点声明系统与主题**：
   ```html
   <link rel="stylesheet" href="design-system/tokens/index.css">
   <div data-system="harmony" data-theme="dark">...</div>
   ```
2. **使用 AI 模型构建页面时**：让模型先阅读 `design-system/SKILL.md`，从 `design-system/scaffolds/scaffolds.json` 选用标准脚手架、从 `design-system/icons/sprite.svg` 引用原生图标（严禁手绘 `<svg>` 或使用 Emoji）。
3. **运行 14 项自动化还原度门禁**：
   ```bash
   node design-system/scripts/verify-fidelity.js
   ```
