# HarmonyOS 6.0 高级 UI 实现准则 (Advanced UI Rules)

## 1. 材质、层级与阴影 (Material & Elevation)
- **阴影样式 (`ShadowStyle`)**:
    - **基础卡片/列表项**: 使用 `OUTER_DEFAULT_SM` 或 `OUTER_DEFAULT_MD`。
    - **悬浮按钮 (FAB)/弹出菜单**: 使用 `OUTER_FLOATING_SM` 或 `OUTER_FLOATING_MD`。
- **模糊材质 (`BlurStyle`)**:
    - **导航栏/工具栏**: 强制使用 `COMPONENT_ULTRA_THIN`（超轻薄）以保持内容可见度。
    - **对话框/半模态底板**: 使用 `COMPONENT_REGULAR` 或 `COMPONENT_THICK` 增加深层感。

## 2. 深度阅读排版 (Reading Typography)
- **标题优化 (`TextHeightAdaptivePolicy`)**:
    - 订阅列表和文章标题必须设置 `MAX_LINES_FIRST` 策略。
    - 配合 `minFontSize` 和 `maxFontSize`，确保长标题在空间不足时自动缩小字号而非简单截断。
- **正文间距**:
    - 建议使用 `paragraphSpacing` 明确区分段落，避免大块文字堆叠。
    - 开启 `enableAutoSpacing(true)` 优化中西文混排。

## 3. 核心动效参数 (Motion Specs)
- **物理弹簧曲线 (Spring Motion)**:
    - **优先准则**: 严禁在交互动效中硬编码 `duration`，必须优先使用 `curves.springMotion` 或 `curves.responsiveSpringMotion`。
    - **共享元素转场**: 默认播放时长基准为 `1000ms`（若使用插值曲线）。
- **一镜到底 (One-Lens)**:
    - 采用 `geometryTransition` 实现组件内隐式共享元素转场。
    - 结合模态转场接口，确保转场过程中无“白闪”或“断裂感”。

## 4. 响应式与多窗避让 (Multi-window Adaptability)
- **窗口监听**: 必须注册 `on('windowSizeChange')`，并将尺寸持久化到 `AppStorage`，通过 `@StorageLink` 实时驱动布局重绘。
- **智慧多窗避让**:
    - 在分屏/悬浮窗模式下，监听 `on('avoidAreaChange')`。
    - 动态调整页面 `padding`，防止内容被系统控制条（顶部）或导航指示器（底部）遮挡。

## 5. 无障碍与大字体重排 (A11y & Relayout)
- **适老化触发**: 当系统字体比例 `fontSizeScale >= 1.75` 时，触发布局重排。
- **重排策略**: 
    - **X 轴转 Y 轴**: 将左右布局（如：图标+文字）自动转换为上下布局，防止文字截断。
- **长按放大**: 针对由于空间限制无法完全显示的文本，需支持系统级的“长按适老化弹窗”提取展示。
