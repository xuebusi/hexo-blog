---
title: SwiftUI中VStack的用法
date: 2023-11-07 21:54:54
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`VStack` 是一个容器视图，它在垂直方向上排列其子视图。这是组织多个子视图，使其在单列中自上而下排列的一种简单有效的方式。

以下是 `VStack` 的基本和高级用法：

### 基本用法

```swift
VStack {
    Text("First Line")
    Text("Second Line")
    Text("Third Line")
}
```

这将会把三行文本垂直排列。

### 调整间距

通过间距参数可以调整子视图之间的距离。

```swift
VStack(spacing: 10) {
    Text("First Line")
    Text("Second Line")
    Text("Third Line")
}
```

这样设置会在每行文本之间添加10点的空间。

### 对齐

`VStack` 默认在水平方向上是居中对齐的，但可以改变这个对齐方式：

```swift
VStack(alignment: .leading) {
    Text("Aligned to")
    Text("the leading edge")
}
```

这样会将文本对齐到 VStack 的前缘（左边）。

### 使用 Spacer

`Spacer` 可以用来推动子视图，让它们靠近容器的边缘或者分散对齐。

```swift
VStack {
    Text("Top")
    Spacer() // 推动所有内容到顶部和底部
    Text("Bottom")
}
```

这里的 `Spacer` 会尝试占据所有可用的垂直空间，推动“Top”到顶部，将“Bottom”推到底部。

### 嵌套使用

可以在 `VStack` 中嵌套其他 `HStack` 或 `VStack` 来创建复杂的布局结构。

```swift
VStack {
    HStack {
        Text("Top Left")
        Text("Top Right")
    }
    HStack {
        Text("Bottom Left")
        Text("Bottom Right")
    }
}
```

这将创建一个两行的布局，每行都有左右两个文本视图。

### 结合 Frame 和 Alignment 使用

你可以为 `VStack` 或其子视图指定大小和对齐方式：

```swift
VStack(alignment: .trailing) {
    Text("Line 1")
        .frame(width: 200)
    Text("Line 2 is longer")
        .frame(width: 150, alignment: .trailing)
}
```

这样会使 `VStack` 的文本对齐到右边，尽管子视图的宽度不同。

### 在实际应用中

`VStack` 经常被用来组织表单、列表的单个项、或者任何需要垂直排列的内容。

```swift
VStack {
    Image(systemName: "photo")
    Text("Welcome to SwiftUI")
        .font(.title)
    Text("Let's build some UIs!")
        .font(.subheadline)
}
.padding()
```

在这个例子中，图像位于顶部，紧接着是标题和副标题，所有内容都在 `VStack` 中垂直排列，并且使用 `.padding` 修饰符在周围添加空间。

`VStack` 是实现 SwiftUI 应用程序中垂直布局的基石，与 `HStack`、`ZStack` 和其他容器视图一起，可以构建出几乎任何布局结构。