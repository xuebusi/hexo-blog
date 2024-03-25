---
title: SwiftUI中Alert控件的用法
date: 2023-11-07 20:58:19
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`Alert` 控件用来向用户显示警告或者提示信息，并且可以提供一个或多个操作供用户选择。`Alert` 通常用于提示错误信息、确认重要操作或展示信息确认。

### 基础用法

使用 `Alert` 需要创建一个状态变量来控制它的显示与隐藏，并且在视图的某个事件中触发这个状态变量的变化。然后，你可以使用 `.alert` 修饰符来在视图层级中添加一个 `Alert`。

```swift
struct ContentView: View {
    @State private var showAlert = false

    var body: some View {
        Button("Show Alert") {
            self.showAlert = true
        }
        .alert("Important message", isPresented: $showAlert) {
            Button("OK", role: .cancel) { }
        }
    }
}
```

在这个例子中，当按钮被点击时，`showAlert` 状态变为 `true`，这会触发 `.alert` 修饰符显示一个带有 "OK" 按钮的警告框。

### 带有多个按钮的 Alert

```swift
.alert("Important message", isPresented: $showAlert) {
    Button("Cancel", role: .cancel) { }
    Button("Delete", role: .destructive) { }
    Button("Save") { }
}
```

这里，除了取消按钮外，我们还添加了一个具有破坏性的删除操作和一个普通的保存操作。

### Alert 的自定义内容

你还可以自定义 `Alert` 的标题、消息和按钮。例如，下面的 `Alert` 有一个标题、消息和两个按钮：

```swift
.alert(isPresented: $showAlert) {
    Alert(
        title: Text("Important message"),
        message: Text("Please read this."),
        primaryButton: .default(Text("Got it!")),
        secondaryButton: .cancel()
    )
}
```

### 使用 `Alert` 获取用户输入

在 SwiftUI 中，如果你想在 `Alert` 中获取用户的输入，你通常会使用其他视图，如 `sheet`，因为 `Alert` 本身并不支持文本输入。不过，你可以通过弹出一个带有文本字段的 `sheet` 或者使用自定义的对话框视图来模拟这个行为。

### 使用 `Alert` 与 `ObservableObject`

当你的视图模型遵循 `ObservableObject` 并使用 `@Published` 属性来管理状态时，你可以这样使用 `Alert`：

```swift
struct ContentView: View {
    @ObservedObject var viewModel = MyViewModel()

    var body: some View {
        Button("Show Alert") {
            viewModel.triggerAlert()
        }
        .alert("Important message", isPresented: $viewModel.showAlert) {
            Button("OK", role: .cancel) { }
        }
    }
}

class MyViewModel: ObservableObject {
    @Published var showAlert = false

    func triggerAlert() {
        showAlert = true
    }
}
```

在上面的例子中，`MyViewModel` 控制 `showAlert` 的状态，当 `triggerAlert` 方法被调用时，会显示 `Alert`。

`Alert` 在 SwiftUI 中是模态的，这意味着当它出现时，用户必须进行选择才能继续与应用交互。它的设计旨在用于必须要立即解决的情况，因此在设计用户界面时应慎用。