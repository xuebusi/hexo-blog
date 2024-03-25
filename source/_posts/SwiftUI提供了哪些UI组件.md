---
title: SwiftUI提供了哪些UI组件?
date: 2023-11-07 15:44:38
categories:
- SwiftUI
tags:
---
SwiftUI 提供了一系列的 UI 组件，它们在 SwiftUI 中被称为 Views。以下是一些基础的组件及其用途：

### Textual Components
- **Text**: 显示一行或多行只读文本。
- **TextField**: 允许用户输入文本。
- **SecureField**: 用于密码输入，输入内容会被遮蔽。
- **Label**: 显示文本和图标旁边的文本，适用于快速创建简单的标签。

### Image and Graphics
- **Image**: 显示图片。
- **Icon**: 利用 SF Symbols 或自定义图像显示图标。
- **Shape**: 绘制基本图形，如 Rectangle、Circle、Ellipse、Capsule 等。

### Buttons and Toggles
- **Button**: 可触发动作的标准按钮。
- **Toggle**: 开关按钮，可用于切换状态。
- **Picker**: 选择器，允许从多个选项中选择一个。
- **Slider**: 滑块，用于从范围中选择一个值。
- **Stepper**: 步进器，用于增加或减少一个值。

### Containers and Layout
- **HStack**: 水平堆叠的容器，用于横向排列子视图。
- **VStack**: 垂直堆叠的容器，用于纵向排列子视图。
- **ZStack**: 重叠容器，用于层叠视图。
- **List**: 用于显示一系列数据的滚动列表。
- **ScrollView**: 提供可滚动视图的容器。
- **Grids (LazyHGrid/LazyVGrid)**: 创建水平或垂直的网格布局。
- **Form**: 用于创建表单布局，常用于设置页面。
- **NavigationView**: 创建带有导航能力的视图结构。

### Presentation
- **Alert**: 显示警告消息。
- **Sheet**: 从底部滑出的覆盖层，用于显示次级视图。
- **Popover**: 弹出视图，用于 iPad 上的视图展示。
- **Modal**: 用于展示临时内容，例如详情页或选择器。

### Controls and Indicators
- **ProgressView**: 显示进度指示器。
- **ActivityIndicator**: 显示一个旋转的加载提示符（iOS 14 以前称为 ActivityIndicator）。
- **Link**: 提供一个可导航至外部链接或其他视图的按钮。

### Specialized Views
- **Map**: 集成地图显示。
- **WebView**: 显示一个网页的视图（通过 `WKWebView`）。

### Animation and Transition
- **Animation**: 将动画应用于视图状态的改变。
- **Transition**: 定义视图添加或移除时的动画效果。

### Environment and Preferences
- **Environment**: 用于从父视图传递数据到子视图的对象。

这些组件可以结合起来，形成复杂的 UI 布局，并且可以通过各种修饰符（Modifiers）进行扩展和自定义，以实现具体的设计要求和动态效果。修饰符可以更改视图的显示方式，例如字体、颜色、对齐、填充、边框等属性，也可以添加如阴影、圆角等视觉效果。此外，SwiftUI 还提供了强大的数据绑定和状态管理机制，使得用户界面的开发更为动态和交互式。