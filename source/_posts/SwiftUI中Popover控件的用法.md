---
title: SwiftUI中Popover控件的用法
date: 2023-11-07 21:06:54
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，Popover 是一种用于临时显示浮动内容的组件。它通常被用来提供额外的信息或选项，而不必将用户带离他们当前的上下文。Popover 在 iPad 上特别有用，因为它可以利用更大的屏幕尺寸来展示额外内容而不覆盖整个屏幕。

### Popover 的基本用法

要在 SwiftUI 中创建一个 popover，你可以使用 `.popover` 修饰符。这需要一个绑定的布尔值，当该值为真时，会显示 popover。

```swift
struct ContentView: View {
    @State private var showingPopover = false

    var body: some View {
        Button("Show Popover") {
            showingPopover = true
        }
        .popover(isPresented: $showingPopover) {
            Text("Here's the content of the Popover.")
                .font(.headline)
                .padding()
        }
    }
}
```

在这个例子中，当用户点击按钮时，`showingPopover` 变为 `true`，这会触发 popover 的展示。

### Popover 的附加配置

Popover 支持使用 `attachmentAnchor` 和 `arrowEdge` 参数来进一步配置其位置和箭头的朝向。

```swift
.popover(isPresented: $showingPopover, attachmentAnchor: .point(.bottom), arrowEdge: .top) {
    // Popover 的内容
}
```

### Popover 用于显示动态内容

Popover 也可以用来显示动态内容，这意味着你可以根据某些状态或者条件来决定展示什么内容。

```swift
.popover(isPresented: $showingPopover) {
    if someCondition {
        SomeView()
    } else {
        AnotherView()
    }
}
```

### Popover 与 `item` 绑定

除了使用布尔值，popover 还可以通过绑定到一个可选项上来展示，这可以使你基于选择的特定对象显示不同的 popover。

```swift
struct ContentView: View {
    @State private var selectedPerson: Person?

    var body: some View {
        List(people) { person in
            Text(person.name)
                .onTapGesture {
                    self.selectedPerson = person
                }
        }
        .popover(item: $selectedPerson) { person in
            PersonDetailView(person: person)
        }
    }
}

struct Person: Identifiable {
    let id: UUID
    let name: String
}
```

在上面的例子中，每个人的名字是一个列表项。当点击名字时，`selectedPerson` 被设置为该 `Person` 实例，并且展示了一个 popover 来显示更多关于这个人的详情。

### 在不同设备上的表现

Popover 在 iPadOS 上以浮动视图的形式展现，在 iPhone 上则会占据整个屏幕，类似于 modal presentation。因此，设计你的 popover 内容时，要考虑它在不同设备上的表现。

Popover 通常用于临时任务或补充内容的场景，它的优点在于能够保持用户在当前的上下文中，同时提供额外的交互或信息。与其它模态视图一样，它们应该被谨慎使用，以免打断用户的工作流程。