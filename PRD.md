# Flow - HarmonyOS RSS Reader 产品需求文档 (PRD)

## 1. Executive Summary
**Flow** 是一款专为 HarmonyOS NEXT 6.0 (API 21) 打造的原生 RSS 阅读应用。在信息过载的时代，Flow 旨在回归阅读本质，通过极致的**瑞士极简编辑风格**和**原生沉浸式体验**，为用户提供一个安静、高效、离线优先的信息获取空间。

**核心价值主张**:
- **原生精致**: 深度适配 HarmonyOS 6.0 的自适应布局和物理动效。
- **阅读至上**: 瑞士极简排版，剥离干扰，强调文字的呼吸感。
- **隐私与自主**: 数据完全存储于本地，支持标准 RSS 和 RSSHub 扩展。

**MVP 目标**: 交付一个功能完整、性能流畅、视觉达到生产级的本地 RSS 阅读器原型。

---

## 2. Mission
**使命**: 让获取信息变得像阅读精装报刊一样优雅。

**核心原则**:
1. **隐形设计**: UI 应该服务于内容，而非干扰内容。
2. **物理直觉**: 动效应符合真实世界的物理特性（如弹性、跟手性）。
3. **性能极致**: 满帧运行 (120Hz)，即开即用。
4. **自适应**: 在任何尺寸的鸿蒙设备上都能提供最佳布局。

---

## 3. Target Users
- **核心用户**: 深度信息消费者、开发者、设计师。
- **技术成熟度**: 中等至高等，了解 RSS 概念。
- **痛点**: 
    - 现有阅读器 UI 杂乱。
    - 缺乏对 HarmonyOS 新特性的深度适配（如折叠屏布局、状态栏沉浸）。
    - 依赖云端账号，担心隐私。

---

## 4. MVP Scope

### 4.1 ✅ In Scope
**核心功能**:
- [x] RSS 2.0 / Atom / RSSHub 协议解析。
- [x] 订阅源管理（添加、删除、重命名、分类）。
- [x] 文章列表展示（卡片/列表模式切换）。
- [x] 沉浸式文章阅读（支持 WebView 渲染及原生长文阅读优化）。
- [x] 收藏与“稍后阅读”功能。
- [x] OPML 导入/导出（支持与其他阅读器迁移数据）。

**技术与性能**:
- [x] 纯本地存储 (RDB + Preferences)。
- [x] 离线缓存最新 N 篇文章（包含图片）。
- [x] 自适应布局（适配手机、折叠屏、平板）。
- [x] 沉浸式窗口与动态模糊导航栏。

### 4.2 ❌ Out of Scope
- [ ] AI 摘要与自动分类（Phase 2 实现）。
- [ ] 多端云同步（Phase 2 实现）。
- [ ] 付费订阅功能。
- [ ] 社交分享/评论系统。

---

## 5. User Stories
1. **订阅管理**: 作为一名用户，我希望能够直接输入 URL 或搜索关键词添加订阅，以便我能持续关注感兴趣的内容。
2. **高效浏览**: 作为一名用户，我希望在折叠屏展开时能看到左边列表右边正文的双栏布局，以便我能快速扫视并深入阅读。
3. **沉浸阅读**: 作为一名用户，我希望阅读正文时状态栏能自动沉浸且无多余 UI，以便我能专注于文字。
4. **离线使用**: 作为一名用户，我希望在没有网络时（如地铁上）也能阅读已下载的文章，以便我能随时随地获取信息。
5. **数据迁移**: 作为一名用户，我希望能够导入我的 OPML 文件，以便我不必手动重新添加几十个订阅源。

---

## 6. Core Architecture & Patterns
- **架构模式**: MVVM (Model-View-ViewModel) + Repository。
- **层级结构**:
    - `Presentation Layer`: ArkUI 组件与 Pages。
    - `Business Layer`: ViewModel 处理逻辑。
    - `Data Layer`: Repository 协调本地数据库 (RDB) 与网络请求。
- **设计模式**: 
    - **单例模式**: 用于全局的 Service 和 Database 管理。
    - **观察者模式**: `@State`, `@Link`, `@StorageLink` 驱动 UI 自动更新。

---

## 7. Technology Stack
- **平台**: HarmonyOS NEXT 6.0 (API 21)。
- **语言**: ArkTS。
- **UI 框架**: ArkUI。
- **持久化**:
    - `RelationalStore (RDB)`: 存储 Feeds, Entries, Categories。
    - `Preferences`: 存储用户设置 (Theme, Font Size, etc.)。
- **网络**: `@ohos.net.http`。
- **测试**: `@ohos/hypium` (Unit Test), `@ohos/hamock` (Mock)。

---

## 8. API Specification (Internal)
虽然是本地应用，但内部 Service 定义需规范化：
- `FeedRepository.addFeed(url: string)`: 抓取并保存新订阅。
- `EntryRepository.getEntries(feedId?: string)`: 获取文章列表，支持分页。
- `CacheService.syncFeed(feedId: string)`: 执行增量刷新。

---

## 9. Success Criteria
- [x] 应用在冷启动 500ms 内显示首屏。
- [x] 列表滑动帧率保持在 120fps。
- [x] 适配至少三种断点 (sm, md, lg)。
- [x] 离线状态下可完整显示已缓存文章。
- [x] 零隐私泄露（无外部账号体系）。

---

## 10. Implementation Phases

### Phase 1: 基础设施与数据层
- **目标**: 完成数据库搭建与 RSS 解析引擎。
- **交付**: RDB 表结构、RssParser 工具类、FeedRepository。

### Phase 2: 核心 UI 与导航
- **目标**: 实现响应式框架与文章列表。
- **交付**: Navigation 路由框架、EntryListPage (List/Card)、自适应 SideBar。

### Phase 3: 阅读体验与离线化
- **目标**: 极致的沉浸阅读与图片缓存。
- **交付**: EntryDetailPage (沉浸模式)、CacheService (离线图片下载)。

### Phase 4: 完善设置与迁移
- **目标**: OPML 支持与细节打磨。
- **交付**: OPML 导入导出、设置页面、物理动效调优。

---

## 11. Risks & Mitigations
- **风险**: RSS 格式不统一导致解析失败。
  - **对策**: 引入 robust 的容错逻辑，并提供“查看原文” WebView 跳转。
- **风险**: 离线图片占用过多空间。
  - **对策**: 提供 LRU 清理策略及用户可配置的缓存数量。
- **风险**: HarmonyOS 6.0 API 变动。
  - **对策**: 建立在 API 21 稳定版基础上，并在 `rules` 中记录 API 依赖。

---

## 12. Appendix
- **项目目录**: `entry/src/main/ets/` 下的分层结构。
- **参考规范**: [HarmonyOS 6.0 设计准则](./.gemini/rules/adaptive_layout.md)
