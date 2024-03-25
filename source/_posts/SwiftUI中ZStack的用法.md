---
title: SwiftUI中ZStack的用法
date: 2023-11-07 21:57:02
categories:
- SwiftUI
tags:
---
`ZStack` 在 SwiftUI 中是用来重叠视图的，它按照代码中的顺序堆叠视图，最先声明的视图会出现在底部，随后声明的视图则依次叠加在上面。这对于创建覆盖效果或者需要视图叠加的界面非常有用。

以下是 `ZStack` 的一些基本用法：

### 基本用法

```swift
ZStack {
    Text("Underneath")
    Text("On top")
}
```

第二个 `Text` 视图将会覆盖在第一个 `Text` 视图上面。

### 对齐

`ZStack` 默认会将所有子视图居中对齐，但可以指定一个不同的对齐方式。

```swift
ZStack(alignment: .topLeading) {
    Text("Aligned to the top leading corner")
    Text("Second view")
}
```

上面的代码会使所有子视图在 ZStack 的顶部左侧对齐。

### 使用背景和覆盖

`ZStack` 常用来给视图添加背景或者覆盖。

```swift
ZStack {
    Image("photo")
        .resizable()
        .aspectRatio(contentMode: .fit)
    Text("Over the Image")
        .font(.caption)
        .foregroundColor(.white)
        .padding(5)
        .background(Color.black.opacity(0.7))
        .cornerRadius(10)
        .padding(10)
}
```

文本会作为覆盖层显示在图片上，并且带有半透明的黑色背景以确保文字的可读性。

### 结合 Frame 和 Alignment 使用

可以通过 `frame` 和 `alignment` 属性来控制子视图的大小和位置。

```swift
ZStack {
    Color.blue
    VStack {
        Text("Hello")
        Text("World")
    }
    .alignmentGuide(.top) { d in d[.top] }
    .frame(width: 200, height: 200, alignment: .top)
}
```

在这个例子中，`VStack` 被设定在蓝色背景的上方，并且它的大小被限制为 200x200。

### 在实际应用中

`ZStack` 可以创建自定义的复杂视图，如下面的代码片段中，一个定制按钮的视觉效果就是通过 `ZStack` 实现的：

```swift
ZStack {
    Circle()
        .fill(Color.blue)
        .frame(width: 100, height: 100)
    Text("Click Me")
        .font(.title)
        .foregroundColor(.white)
}
```

这会创建一个带有文本的圆形按钮视图。

`ZStack` 是一个非常强大的工具，允许开发者在 SwiftUI 中以创造性的方式构建复杂且富有层次感的用户界面。通过合理地堆叠和对齐视图，可以实现各种自定义设计。