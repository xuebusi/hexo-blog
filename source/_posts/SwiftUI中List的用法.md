---
title: SwiftUI中List的用法
date: 2023-11-07 22:05:28
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`List` 是一个用来展示数据集合的滚动列表视图。它类似于 UIKit 中的 `UITableView`，可以显示静态或动态的数据集合。`List` 可以和 `ForEach` 一起使用来显示动态数据，也可以直接放入静态视图。

以下是使用 `List` 的几种常见方法：

### 显示静态内容

```swift
import SwiftUI

struct StaticListView: View {
    var body: some View {
        List {
            Text("Item 1")
            Text("Item 2")
            Text("Item 3")
        }
    }
}

struct StaticListView_Previews: PreviewProvider {
    static var previews: some View {
        StaticListView()
    }
}
```

### 显示动态数据

当你有一组动态数据时，可以使用 `ForEach` 来创建列表的每一行。

```swift
import SwiftUI

struct DynamicListView: View {
    let items = ["Apple", "Banana", "Cherry"]

    var body: some View {
        List {
            ForEach(items, id: \.self) { item in
                Text(item)
            }
        }
    }
}

struct DynamicListView_Previews: PreviewProvider {
    static var previews: some View {
        DynamicListView()
    }
}
```

### 使用 `Identifiable` 数据模型

当你的数据模型遵守 `Identifiable` 协议时，你不需要指定 `id`。

```swift
import SwiftUI

struct Item: Identifiable {
    let id = UUID()
    let name: String
}

struct IdentifiableListView: View {
    let items = [Item(name: "Apple"), Item(name: "Banana"), Item(name: "Cherry")]

    var body: some View {
        List(items) { item in
            Text(item.name)
        }
    }
}

struct IdentifiableListView_Previews: PreviewProvider {
    static var previews: some View {
        IdentifiableListView()
    }
}
```

### 添加和删除操作

你可以通过在 `ForEach` 上调用 `.onDelete()` 和 `.onMove()` 方法来添加对列表的删除和移动操作的支持。

```swift
import SwiftUI

struct EditableListView: View {
    @State private var items = ["Apple", "Banana", "Cherry"]

    var body: some View {
        NavigationView {
            List {
                ForEach(items, id: \.self) { item in
                    Text(item)
                }
                .onDelete(perform: delete)
                .onMove(perform: move)
            }
            .navigationBarItems(trailing: EditButton())
        }
    }

    func delete(at offsets: IndexSet) {
        items.remove(atOffsets: offsets)
    }

    func move(from source: IndexSet, to destination: Int) {
        items.move(fromOffsets: source, toOffset: destination)
    }
}

struct EditableListView_Previews: PreviewProvider {
    static var previews: some View {
        EditableListView()
    }
}
```

### 自定义行

你可以在 `List` 中自定义行，比如添加图标和导航链接。

```swift
import SwiftUI

struct CustomRowListView: View {
    var body: some View {
        List {
            NavigationLink(destination: Text("Destination")) {
                HStack {
                    Image(systemName: "folder")
                    Text("Files")
                }
            }
        }
    }
}

struct CustomRowListView_Previews: PreviewProvider {
    static var previews: some View {
        CustomRowListView()
    }
}
```

### 使用 `List` 的选择模式

你还可以使用 `List` 的选择模式来创建可以选择单个或多个项的列表。

```swift
import SwiftUI

struct SelectableListView: View {
    let items = ["Apple", "Banana", "Cherry"]
    @State private var selectedItems = Set<String>()

    var body: some View {
        List(items, id: \.self, selection: $selectedItems) { item in
            Text(item)
        }
        .toolbar {
            EditButton()
        }
        .environment(\.editMode, .constant(.active)) // 使得列表始终处于编辑模式
    }
}

struct SelectableListView_Previews: PreviewProvider {
    static var previews: some View {
        SelectableListView()
    }
}
```

在 SwiftUI 中，`List` 是一个非常强大的构建用户界面的工具，可以通过上述的各种方式灵活地显示和管理集合数据。