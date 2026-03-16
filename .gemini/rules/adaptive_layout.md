# HarmonyOS 6.0 自适应布局准则 (Adaptive Layout Rules)

## 1. 断点系统 (Breakpoints)
- **基准**: 基于设备窗口水平宽度 (vp) 进行布局切换。
- **阈值**:
    - **sm (手机)**: 320vp <= width < 600vp
    - **md (折叠屏展开)**: 600vp <= width < 840vp
    - **lg (平板/桌面)**: width >= 840vp
- **栅格建议**: `xs: 2, sm: 4, md: 8, lg: 12` 列。

## 2. 原子布局能力 (Atomic Layout Capabilities)
- **拉伸 (Stretch)**: 使用 `Blank` 组件在 `Row/Column` 中填充空白。
- **占比 (Proportion)**: 
    - 使用 `layoutWeight` 分配剩余空间。
    - 使用 `aspectRatio` 固定宽高比。
- **延伸 (Extend)**: 
    - 列表类内容必须包裹在 `List` 或 `Scroll` 容器中。
    - 背景色/图应使用 `expandSafeArea` 延伸至非安全区。

## 3. 分栏模式 (Split Mode)
- **触发条件**: 
    - `NavigationMode.Auto` 下，窗口宽度 >= 600vp 自动进入双栏 (Split)。
    - 最小导航栏宽度 240vp，最小内容区宽度 360vp。
- **布局推荐**:
    - 手机端: Stack (单栏)。
    - 大屏端: 侧边导航 + 列表 + 详情 (多栏)。
