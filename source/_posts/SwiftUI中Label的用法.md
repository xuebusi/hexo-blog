---
title: SwiftUI中Label的用法
date: 2023-11-07 20:43:44
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`Label` 是用于展示一个文本和一个图标（icon）的用户界面控件。这对于清晰表达控件或动作的目的非常有用，尤其是在列表、菜单和导航栏中。

### 基础用法

最简单的 `Label` 用法是提供文本和系统图标的名称：

```swift
Label("Add Item", systemImage: "plus")
```

### 使用自定义图像

你也可以使用自定义图像来创建一个 `Label`：

```swift
Label("Edit", image: "customIconName")
```

这里 `"customIconName"` 是你的资产目录中的图像的名称。

### 使用 SF Symbols

SwiftUI 和 SF Symbols 集成，允许你方便地使用数千种符号图标：

```swift
Label("Message", systemImage: "message.fill")
```

### 自定义视图

如果你需要对 `Label` 的文本或图标部分进行更多的自定义，你可以提供自定义视图：

```swift
Label {
    Text("Settings")
        .foregroundColor(.primary)
        .font(.body)
        .padding()
        .background(Color.gray.opacity(0.2))
        .cornerRadius(5)
} icon: {
    Image(systemName: "gear")
        .foregroundColor(.blue)
        .padding()
        .background(Color.gray.opacity(0.2))
        .cornerRadius(5)
}
```

### 嵌套在其他视图中

`Label` 可以嵌套在列表、按钮和其他容器视图中。例如，在一个 `List` 中：

```swift
List {
    Label("Notifications", systemImage: "bell")
    Label("Privacy", systemImage: "hand.raised")
    Label("General", systemImage: "wrench")
}
```

### 样式

`Label` 还可以使用不同的样式，例如：`iconOnlyLabelStyle` 或 `titleOnlyLabelStyle`。

```swift
Label("Airplane Mode", systemImage: "airplane")
    .labelStyle(TitleOnlyLabelStyle())

Label("WiFi", systemImage: "wifi")
    .labelStyle(IconOnlyLabelStyle())
```

### 无障碍

不要忘了考虑无障碍需求，你可以添加描述来帮助辅助技术，如 VoiceOver，更好地解释 `Label` 的内容：

```swift
Label("Delete", systemImage: "trash")
    .accessibilityLabel(Text("Delete"))
    .accessibilityHint(Text("Double tap to remove the item"))
```

### 使用场景

- 在 `NavigationView` 和工具栏按钮中展示图标和文本。
- 在列表或菜单中作为条目显示，既要展示图标也要展示文本。
- 在用户界面的其他任何地方，当你需要在文本旁边显示图标以提高识别度时。

`Label` 由于其多功能性和易用性，在 SwiftUI 中是一个很受欢迎的组件。通过组合使用文本和图标，你可以创建既美观又直观的用户界面元素。