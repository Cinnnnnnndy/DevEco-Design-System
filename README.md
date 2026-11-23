# DevEco Design System

基于本地 Pixso 矢量 AST（133,622 节点）提取构建的 **DevEco 双设计系统**：
- **鸿蒙原生端（HarmonyOS NEXT HDesign 2.0）**：`data-system="harmony"`（`dark` / `light`）
- **PC / Mac 当代桌面端（IntelliJ NewUI）**：`data-system="pc-mac"`（`dark` / `light` / `light-contrast`）

## 核心资产目录
- **在线总览页**：`design-system/preview/index.html`（开启 GitHub Pages 后访问根路径自动跳转）
- **双系统 Token 与组件样式**：`design-system/tokens/index.css`（`foundation.css` / `semantic.css` / `components.css` / `ai.css`）
- **Pixso 原生图标库（995 个零重绘 SVG + Token）**：`design-system/icons/sprite.svg` & `design-system/icons/manifest.json`
- **12 套官方页面脚手架（整窗 / 分屏 / 设置弹窗 / 抽屉 / 命令面板）**：`design-system/scaffolds/scaffolds.json`
- **23 个核心组件集（含开源 IntelliJ ExpUI & `openharmony-sig/deveco-code` AI Agent IDE 组件）**：`design-system/docs/`
- **跨模型高还原度 Skill 与 14 项自动化门禁**：
  - 规范文档：`design-system/SKILL.md`
  - 一键校验：`node design-system/scripts/verify-fidelity.js`
