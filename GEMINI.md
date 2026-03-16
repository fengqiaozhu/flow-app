# Flow - HarmonyOS RSS Reader

## 1. 项目概览
**Flow** 是一款基于 HarmonyOS NEXT 6.0 (API 21) 的原生 RSS 阅读器。
- **核心定位**: 极简、高效、沉浸式阅读。
- **设计风格**: 瑞士极简编辑风格 (Swiss Minimal Editorial)。
- **技术栈**: ArkTS, ArkUI, RelationalStore (RDB), Preferences, Navigation。

## 2. 核心规范与文档
开发过程中应严格遵守以下设计与架构文档：
- **产品需求文档 (PRD)**: [`PRD.md`](./PRD.md)
- **MVP 设计方案**: [`.docs/2026-03-12-flow-mvp-design.md`](./.docs/2026-03-12-flow-mvp-design.md)
- **UI/UX 详细设计**: [`.docs/2026-03-16-flow-ui-ux-design.md`](./.docs/2026-03-16-flow-ui-ux-design.md)
- **HarmonyOS 6.0 设计准则**:
    - [自适应布局准则 (Adaptive Layout)](./.gemini/rules/adaptive_layout.md)
    - [沉浸式导航准则 (Immersive Navigation)](./.gemini/rules/immersive_navigation.md)
    - [动效准则 (Motion Guidelines)](./.gemini/rules/motion_guidelines.md)
    - [高级 UI 实现准则 (Advanced UI Implementation)](./.gemini/rules/advanced_ui_implementation.md)
- **HarmonyOS 设计规范**: 遵守原生 6.0 的自适应布局、沉浸式导航及动效准则。

## 3. 开发规范 (Development Conventions)
- **UI 开发**: 
    - 优先使用 `Navigation` 组件及其 `Auto` 模式实现响应式布局。
    - 正文排版单位统一使用 `fp` 以支持系统字体缩放。
    - 列表渲染必须使用 `LazyForEach` 保证流畅度。
- **数据管理**:
    - 采用 **MVVM + Repository** 模式。
    - 结构化数据存储于 `RDB`，用户设置存储于 `Preferences`。
- **目录结构**: 遵循 `entry/src/main/ets/` 下的 `common`, `components`, `models`, `services`, `repository`, `viewmodels`, `pages` 分层。

## 4. 关键命令 (Build & Run)
- **编译/构建**: 使用 DevEco Studio 或 `hvigorw` (TODO: 确认 CLI 命令)。
- **包管理**: `ohpm install`
- **单元测试**: `ohpm test` (依赖 `@ohos/hypium`)

## 5. UI/UX Pro Max 技能
本项目已安装 `ui-ux-pro-max` 技能。当进行 UI 设计或代码重构时，请优先使用以下流程：
1. 分析需求（阅读类、极简、沉浸）。
2. 使用 `python3 .shared/ui-ux-pro-max/scripts/search.py` 搜索相关风格。
3. 结合 `Harmony_api` 笔记本确认鸿蒙 6.0 的原生实现 API。

## 6. 开发者须知
- **本地优先**: 始终优先考虑本地存储和离线体验。
- **响应式**: 代码必须兼容手机、折叠屏和平板设备。
- **沉浸感**: 阅读器页面必须开启全屏沉浸模式并处理好状态栏避让。
