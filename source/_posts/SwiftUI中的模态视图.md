---
title: SwiftUI中的模态视图
date: 2023-11-07 21:09:46
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，通常我们提到“模态”时，指的是一个全屏或部分屏幕的视图，它会暂时中断用户的当前工作流程以获取用户的注意和交互。在 SwiftUI 中，没有直接命名为 `Modal` 的控件，但模态行为可以通过多种方式实现，如 `sheet`、`fullScreenCover` 以及 `popover`（在 iPad 上）。下面将介绍如何实现模态行为的几种方法。

### 使用 `sheet` 展示模态视图

`sheet` 修饰符可以用来展示一个模态视图，这个视图会覆盖在当前视图的上方，并带有一个背景遮罩。用户必须明确地关闭模态视图才能返回到原始视图。

```swift
struct ContentView: View {
    @State private var showingSheet = false

    var body: some View {
        Button("Show Sheet") {
            showingSheet = true
        }
        .sheet(isPresented: $showingSheet) {
            // Sheet的内容
            ModalContentView()
        }
    }
}

struct ModalContentView: View {
    var body: some View {
        Text("This is a modal view.")
    }
}
```

### 使用 `fullScreenCover` 展示模态视图

`fullScreenCover` 类似于 `sheet`，但是会覆盖整个屏幕，为模态视图提供了更多的空间。

```swift
struct ContentView: View {
    @State private var showingFullScreenModal = false

    var body: some View {
        Button("Show Full-Screen Modal") {
            showingFullScreenModal = true
        }
        .fullScreenCover(isPresented: $showingFullScreenModal) {
            FullScreenModalView()
        }
    }
}

struct FullScreenModalView: View {
    @Environment(\.presentationMode) var presentationMode

    var body: some View {
        Button("Dismiss") {
            presentationMode.wrappedValue.dismiss()
        }
    }
}
```

### 使用 `popover` 展示模态视图（仅限 iPad）

Popover 在 iPad 上提供了另一种模态体验，它显示一个浮动的视图，不会覆盖整个屏幕，允许用户在不完全离开当前上下文的情况下与 popover 互动。

```swift
struct ContentView: View {
    @State private var showingPopover = false

    var body: some View {
        Button("Show Popover") {
            showingPopover = true
        }
        .popover(isPresented: $showingPopover) {
            Text("Popover content here")
        }
    }
}
```

### 使用 `NavigationLink` 或 `@State` 和 `@Binding` 触发模态视图

在某些情况下，你可能需要通过更传统的方式来显示一个模态视图，例如使用 `NavigationLink` 或通过程序化地控制视图的显示。

```swift
struct ContentView: View {
    @State private var isModalPresented = false

    var body: some View {
        Button(action: {
            isModalPresented = true
        }) {
            Text("Show Modal")
        }
        .background(
            NavigationLink(destination: ModalView(), isActive: $isModalPresented) {
                EmptyView()
            }
            .hidden()
        )
    }
}

struct ModalView: View {
    var body: some View {
        Text("This is a modal view presented via NavigationLink.")
    }
}
```

在 SwiftUI 中，这些模态呈现方法都是通过声明式的 API 来控制，使状态变化与视图更新之间保持同步。你需要根据你的应用设计需求来选择使用哪种模态展示方式。