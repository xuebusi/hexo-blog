---
title: SwiftUI中的布局
date: 2023-11-07 21:47:45
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，布局是通过一系列的容器视图来实现的，这些容器视图可以控制它们子视图的位置和大小。常用的布局容器有 `HStack`、`VStack`、`ZStack`、`Grid`、`List` 和 `ScrollView` 等。

### 基本的布局结构

- **HStack**：水平堆叠子视图。
- **VStack**：垂直堆叠子视图。
- **ZStack**：重叠子视图，即在 Z 轴上（深度方向）堆叠。

```swift
struct ContentView: View {
    var body: some View {
        VStack { // 垂直堆叠
            HStack { // 水平堆叠
                Text("Left")
                Spacer() // 使用 Spacer 来推动两边的文本
                Text("Right")
            }
            ZStack { // Z轴堆叠
                Circle().fill(Color.blue)
                Text("Foreground")
            }
        }
    }
}
```

### 对齐和间距

可以对堆叠容器使用 `alignment` 和 `spacing` 参数来定义子视图的对齐方式和间距。

```swift
HStack(alignment: .top, spacing: 20) {
    Text("Left")
    Text("Right")
}
```

### 帧（Frame）

通过 `.frame` 修饰符可以为视图设定宽度、高度和对齐方式。

```swift
Text("Hello, World!")
    .frame(width: 200, height: 200, alignment: .center)
```

### 弹性空间和填充

- **Spacer**：在视图之间创建一个可扩展的空间，常用于推动视图到屏幕的边缘。
- **Padding**：通过 `.padding` 修饰符来为视图添加内边距。

```swift
VStack {
    Text("First")
    Spacer() // 将占据可用的所有空间
    Text("Last")
}
.padding() // 在 VStack 的所有边缘添加内边距
```

### Divider 和 Separator

可以使用 `Divider` 来在视图中添加一条分割线。

```swift
VStack {
    Text("Above Divider")
    Divider()
    Text("Below Divider")
}
```

### ScrollView

`ScrollView` 允许内容超出屏幕大小时滚动查看。

```swift
ScrollView {
    VStack(spacing: 20) {
        ForEach(0..<100) { index in
            Text("Row \(index)")
        }
    }
}
```

### List

`List` 用于创建一个滚动的列表视图。

```swift
List(0..<100) { index in
    Text("Item \(index)")
}
```

### Grids

在 SwiftUI 中，可以使用 `LazyVGrid` 和 `LazyHGrid` 来创建网格布局。

```swift
LazyVGrid(columns: [GridItem(.adaptive(minimum: 100))]) {
    ForEach(0..<100) { index in
        Text("Item \(index)")
    }
}
```

### Frame Layout

对于复杂的布局，可能需要手动计算视图的尺寸和位置，使用 `.frame` 修饰符来设置大小，并使用 `.offset`、`.position` 等修饰符来调整位置。

### 自定义布局

可以通过创建自定义的 `ViewModifier` 或者自定义的布局容器来创建复杂的布局模式。

SwiftUI 的布局系统是高度可组合的，你可以将多个布局容器和修饰符组合在一起，来创建复杂和响应式的用户界面。重要的是要理解 SwiftUI 的布局原则——尤其是容器如何影响内部子视图的布局，以及修饰符的顺序如何影响结果。