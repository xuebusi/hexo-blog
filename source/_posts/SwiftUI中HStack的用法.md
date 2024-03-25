---
title: SwiftUI中HStack的用法
date: 2023-11-07 21:52:20
categories:
- SwiftUI
tags:
---
`HStack` 是 SwiftUI 中一个非常基础和常用的布局工具，用于在水平方向上堆叠视图。以下是 `HStack` 的一些基本用法：

### 创建一个基础的 HStack

```swift
HStack {
    Text("Left")
    Text("Center")
    Text("Right")
}
```
这会将三个文本视图水平排列在一行内。

### 调整间距

```swift
HStack(spacing: 20) {
    Text("Left")
    Text("Center")
    Text("Right")
}
```
这将会在子视图之间设置为20点的间距。

### 对齐

`HStack` 默认在垂直方向上是居中对齐的，但你可以改变这个对齐方式：

```swift
HStack(alignment: .top) {
    Text("Top")
    Text("Aligned")
    Text("Text")
}
```
这会在垂直方向上将文本对齐到顶部。

### 使用 Spacer

```swift
HStack {
    Text("Left")
    Spacer() // 使用 Spacer 会推动两侧的文本到左右边缘
    Text("Right")
}
```
`Spacer` 占据所有可用的空间，这里将会使两个文本视图分别对齐到左右边缘。

### 嵌套 HStack

`HStack` 可以嵌套使用，以创建更复杂的布局：

```swift
HStack {
    VStack {
        Text("Top Left")
        Text("Bottom Left")
    }
    VStack {
        Text("Top Right")
        Text("Bottom Right")
    }
}
```
这将在水平方向上创建两个垂直堆叠，每个堆叠包含两个文本视图。

### 结合使用 Frame 和 Alignment

```swift
HStack(alignment: .bottom) {
    Text("Bottom")
        .frame(width: 100)
    Text("Aligned")
        .frame(width: 100, alignment: .trailing)
}
```
这将设置每个文本视图的宽度，并在底部对齐它们。

### 在实际应用中

`HStack` 通常与其它布局视图（如 `VStack`, `ZStack`, `ScrollView`）一起使用，以实现复杂的用户界面布局。下面是一个常见的例子，结合 `HStack` 和 `VStack`：

```swift
VStack {
    HStack {
        Image(systemName: "star.fill")
            .foregroundColor(.yellow)
        Text("Favorite")
            .font(.headline)
        Spacer()
    }
    .padding()
    
    Divider()
    
    // 其他内容
}
```
这会创建一个包含星形图标和标题的水平布局，并将其包裹在垂直布局中，通常这会作为列表的一部分。使用 `Divider` 来分隔列表项是常见的做法。