# HarmonyOS 6.0 沉浸式导航准则 (Immersive Navigation Rules)

## 1. 全屏沉浸实现 (Full-screen Immersive)
- **方案 A (窗口级)**: 调用 `setWindowLayoutFullScreen(true)`。
- **方案 B (组件级)**: 属性 `expandSafeArea([SafeAreaType.SYSTEM], [SafeAreaEdge.TOP, SafeAreaEdge.BOTTOM])`。
- **状态栏适配**: 
    - 使用 `systemBarStyle` 或 `setWindowSystemBarProperties` 控制配色（背景、文字颜色）。
    - 文字必须具备高对比度。

## 2. 避让区处理 (Avoid Area)
- **核心原则**: 获取状态栏 (`TYPE_SYSTEM`) 和导航条 (`TYPE_NAVIGATION_INDICATOR`) 高度，通过监听 `on('avoidAreaChange')` 动态调整 UI 间距 (Padding)。
- **操作**: 确保可点击组件不与避让区重叠。

## 3. 标题栏动态显隐 (Dynamic Title Bar)
- **显隐模式**: 
    - `dynamicHideTitleBar`: 使用 `SCROLL_UP` (滚动隐藏) 或 `SCROLL_UP_TO` (至固定位置隐藏)。
    - 绑定: 必须与滚动容器 (`List`, `Scroll`) 通过 `bindToScrollable` 绑定。
- **高斯模糊 (Blur Effect)**:
    - **COMMON_BLUR**: 通用高斯模糊，强调层级。
    - **GRADUAL_BLUR**: 线性渐变模糊，适用于沉浸状态切换。
    - **推荐**: 阅读详情页导航栏使用 `COMMON_BLUR`。
