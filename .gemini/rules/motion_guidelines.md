# HarmonyOS 6.0 动效准则 (Motion Guidelines)

## 1. 核心交互理念 (Core Concepts)
- **弹性 (Elasticity)**:
    - 滚动容器必须设置 `EdgeEffect.Spring`。
    - 交互控件点击使用 `ClickEffectLevel.LIGHT` (轻盈) 或 `MEDIUM` (稳定)。
- **跟手性 (Hand-tracking)**:
    - 动画起始必须继承手势末速度 (Handover Speed)。
    - 滑动、拖拽、半模态面板必须实时响应手指位置。

## 2. 转场动效 (Transitions)
- **共享元素 (Shared Element)**:
    - 列表到详情页强制使用共享元素转场。
    - 图片、标题等关键元素应作为共享节点。
- **自定义路由转场**:
    - 使用 `Navigation` 的 `customNavContentTransition` 定义。
    - 不推荐使用旧版 `PageTransition`。

## 3. 反馈与模糊 (Feedback & Motion Blur)
- **运动模糊 (Motion Blur)**:
    - 快速位移或缩放动画应用 `motionBlur`，半径建议 <= 1.0。
    - 动画结束后务必清除模糊半径。
- **交互回弹 (Click Feedback)**:
    - 按钮、卡片在点击时应有轻微缩放 (`clickEffect`)。
    - 沉浸卡片点击推荐使用 `hdsEffect` 的流光或按压阴影。
- **触感反馈 (Haptics)**:
    - 关键动作（如收藏、刷新、删除）需配合线性马达触感。
