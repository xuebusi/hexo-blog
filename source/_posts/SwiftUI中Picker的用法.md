---
title: SwiftUI中Picker的用法
date: 2023-11-07 20:47:14
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`Picker` 是一个可以让用户从一组选项中选择一个选项的控件。它可以采用多种样式，如轮盘、菜单或段控制（segmented control）。

### 基础用法

最基本的 `Picker` 使用时需要配合 `@State` 属性来绑定选择的值。以下是一个简单的 `Picker` 示例：

```swift
struct ContentView: View {
    @State private var selectedFruit: String = "Apple"

    var fruits = ["Apple", "Banana", "Cherry", "Date"]

    var body: some View {
        Picker("Select a fruit", selection: $selectedFruit) {
            ForEach(fruits, id: \.self) { fruit in
                Text(fruit).tag(fruit)
            }
        }
    }
}
```

### 在表单中使用

`Picker` 常常用在 `Form` 中，这会自动采用最适合表单样式的方式显示：

```swift
struct ContentView: View {
    @State private var selectedFruit: String = "Apple"

    var fruits = ["Apple", "Banana", "Cherry", "Date"]

    var body: some View {
        Form {
            Picker("Select a fruit", selection: $selectedFruit) {
                ForEach(fruits, id: \.self) { fruit in
                    Text(fruit).tag(fruit)
                }
            }
        }
    }
}
```

### 使用 `PickerStyle`

可以使用 `PickerStyle` 来改变 `Picker` 的外观和感觉：

```swift
Picker("Select a fruit", selection: $selectedFruit) {
    ForEach(fruits, id: \.self) { fruit in
        Text(fruit).tag(fruit)
    }
}
.pickerStyle(SegmentedPickerStyle())
```

SwiftUI 提供的 `PickerStyle` 有：

- `DefaultPickerStyle`
- `SegmentedPickerStyle`
- `WheelPickerStyle`
- `InlinePickerStyle`
- `MenuPickerStyle`
- `AutomaticPickerStyle`

不同的 `PickerStyle` 在不同的平台和上下文中有不同的外观和行为。

### 在导航视图中使用

在导航视图中，`Picker` 通常会显示为可以导航到另一个视图选择项的列表：

```swift
struct ContentView: View {
    @State private var selectedFruit: String = "Apple"

    var fruits = ["Apple", "Banana", "Cherry", "Date"]

    var body: some View {
        NavigationView {
            Form {
                Picker("Select a fruit", selection: $selectedFruit) {
                    ForEach(fruits, id: \.self) { fruit in
                        Text(fruit).tag(fruit)
                    }
                }
                .navigationTitle("Fruits")
            }
        }
    }
}
```

在 iOS 上，`Picker` 会自动跳转到一个新的屏幕，用户可以选择一个选项后返回。

### 结合使用 `ForEach` 和 `Identifiable`

如果你的数据模型遵循 `Identifiable` 协议，你可以更简洁地使用 `ForEach`：

```swift
struct Fruit: Identifiable {
    let id = UUID()
    let name: String
}

struct ContentView: View {
    @State private var selectedFruit: Fruit?
    var fruits = [Fruit(name: "Apple"), Fruit(name: "Banana"), Fruit(name: "Cherry"), Fruit(name: "Date")]

    var body: some View {
        Picker("Select a fruit", selection: $selectedFruit) {
            ForEach(fruits) { fruit in
                Text(fruit.name).tag(fruit as Fruit?)
            }
        }
    }
}
```

注意，当使用自定义数据类型作为 `Picker` 的 `tag` 值时，需要将 `selectedFruit` 类型设置为可选的 (`Fruit?`)，因为 `Picker` 的 `tag` 需要能够匹配未选择任何值的情况。

SwiftUI 的 `Picker` 是一个强大的控件，提供了灵活的接口来适应各种选择需求，并且能够很好地与其他 UI 控件集成，创建直观且有效的用户界面。