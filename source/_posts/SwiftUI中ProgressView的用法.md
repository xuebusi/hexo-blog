---
title: SwiftUI中ProgressView的用法
date: 2023-11-07 21:12:20
categories:
- SwiftUI
tags:
---
`ProgressView` 在 SwiftUI 中被用来表示一个任务的进度。它可以展示为一个旋转的指示器，用来表示正在进行的工作，但具体的进度未知，也可以显示为一个水平的进度条，用来表示可确定进度的任务。

以下是 `ProgressView` 的几种用法：

### 不确定进度的指示器

当你没有具体进度值时，可以使用不带参数的 `ProgressView`，它会显示为一个旋转的活动指示器。

```swift
struct ContentView: View {
    var body: some View {
        ProgressView()
            .progressViewStyle(CircularProgressViewStyle()) // 在iOS上默认样式，可省略
    }
}
```

### 确定进度的进度条

如果你有具体的进度值，你可以通过设置一个 `0.0` 到 `1.0` 之间的值来创建一个进度条。

```swift
struct ContentView: View {
    @State private var progress = 0.5

    var body: some View {
        ProgressView(value: progress, total: 1.0)
    }
}
```

你可以通过定时器或者其他事件更新进度值，进度条会反映这些变化。

### 带有标签和描述的进度条

`ProgressView` 还可以提供更多的上下文信息，比如一个标签或者一个描述性文本。

```swift
struct ContentView: View {
    @State private var progress = 0.5

    var body: some View {
        VStack {
            ProgressView("Downloading...", value: progress, total: 1.0)
            
            // 或者使用标签和系统图标
            ProgressView(value: progress) {
                Label("Downloading", systemImage: "arrow.down.doc")
            }
            // 添加描述性文本
            .progressViewStyle(LinearProgressViewStyle()) // 水平进度条
            .accessibilityValue("\(Int(progress * 100)) percent")
        }
    }
}
```

### 自定义进度条样式

你可以通过 `progressViewStyle` 修饰符来自定义进度条的样式。

```swift
struct ContentView: View {
    @State private var progress = 0.5

    var body: some View {
        ProgressView(value: progress)
            .progressViewStyle(LinearProgressViewStyle(tint: .blue)) // 自定义颜色
    }
}
```

使用 `ProgressView` 的时候要注意，它需要在应用的适当位置显示，以避免阻碍用户操作，尤其是当使用不确定进度的指示器时。确保当任务完成或不再相关时，从界面上移除进度指示器。