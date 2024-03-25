---
title: SwiftUI中Sheet控件的用法
date: 2023-11-07 21:02:27
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，sheet 是一种展示模态视图的方式。它通常用于显示一个临时的内容屏幕，例如表单、详情页或者是一个步骤的一部分。`sheet` 修饰符可以让你在当前视图上方展示一个新的视图，并提供内容和行为的自定义。

### 使用 `sheet` 修饰符展示模态视图

基本的 `sheet` 用法涉及一个绑定的布尔值，当这个布尔值为真时，sheet 会显示出来。这个布尔值通常用 `@State` 属性来管理。

```swift
struct ContentView: View {
    @State private var showingSheet = false

    var body: some View {
        Button("Show Sheet") {
            showingSheet = true
        }
        .sheet(isPresented: $showingSheet) {
            // Sheet 的内容
            DetailView()
        }
    }
}
```

在这个例子中，点击按钮会设置 `showingSheet` 为 true，从而触发 sheet 的显示。`DetailView()` 就是你想要作为 sheet 显示的视图。

### 使用 `sheet` 与 `item` 参数

当你要为每个不同的数据对象显示一个 sheet 时，你可以使用 `sheet(item:onDismiss:content:)` 修饰符。这个修饰符需要一个绑定到可选项的 `@State` 属性。当这个属性不是 `nil` 时，将会显示一个 sheet。

```swift
struct ContentView: View {
    @State private var selectedUser: User?

    var body: some View {
        // 假设你有一个用户列表
        ForEach(users) { user in
            Button(user.name) {
                selectedUser = user
            }
        }
        .sheet(item: $selectedUser) { user in
            // 当 selectedUser 不是 nil 时，显示的视图
            UserDetailView(user: user)
        }
    }
}

struct User: Identifiable {
    let id = UUID()
    let name: String
}
```

在这个例子中，当你点击一个用户的按钮时，`selectedUser` 被设置为那个用户的实例，触发对应用户详情的 sheet。

### 使用 `sheet` 的 `onDismiss` 参数

当你的 sheet 被关闭时，你可能想要执行一些代码。`onDismiss` 参数允许你添加一个闭包，当 sheet 消失时会被调用。

```swift
.sheet(isPresented: $showingSheet, onDismiss: {
    print("Sheet was dismissed.")
}) {
    DetailView()
}
```

### 动态内容的 sheet

你还可以在 sheet 的闭包中直接构造视图，而不是传递一个预先定义的视图。这样，你可以基于当前状态动态地改变 sheet 的内容。

```swift
.sheet(isPresented: $showingSheet) {
    if someCondition {
        SomeView()
    } else {
        AnotherView()
    }
}
```

### FullScreenCover

除了标准的 `sheet`，SwiftUI 还提供了 `fullScreenCover`，它以全屏模式显示内容，对于那些需要更多空间或需要从视觉上区分出来的情况非常有用。

```swift
.fullScreenCover(isPresented: $showingCover) {
    FullScreenModalView()
}
```

### 结论

Sheet 控件在 SwiftUI 中为模态内容的展示提供了一种简洁而强大的方式。通过简单的状态绑定和自定义视图传递，你可以创建丰富的用户体验，并管理视图的呈现和隐藏。记得，模态视图是一个打断用户当前流程的操作，所以应该在需要的时候谨慎使用。