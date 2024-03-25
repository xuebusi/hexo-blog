---
title: SwiftUI中ScrollView的用法
date: 2023-11-07 22:08:11
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`ScrollView` 是一个可以滚动显示其子视图的容器。使用 `ScrollView`，你可以创建一个可以纵向或横向滚动的区域，这对于构建超出屏幕限制的内容是非常有用的。

### 基本用法

默认情况下，`ScrollView` 滚动方向是垂直的。

```swift
import SwiftUI

struct BasicScrollView: View {
    var body: some View {
        ScrollView {
            VStack(spacing: 20) {
                ForEach(0..<50) { index in
                    Text("Row \(index)")
                }
            }
        }
    }
}

struct BasicScrollView_Previews: PreviewProvider {
    static var previews: some View {
        BasicScrollView()
    }
}
```

### 水平滚动

要创建一个水平滚动视图，需要设置 `ScrollView` 的 `axis` 参数。

```swift
import SwiftUI

struct HorizontalScrollView: View {
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            HStack(spacing: 20) {
                ForEach(0..<50) { index in
                    Text("Column \(index)")
                        .frame(width: 200, height: 200)
                        .background(Color.blue)
                        .foregroundColor(.white)
                        .cornerRadius(10)
                }
            }
        }
    }
}

struct HorizontalScrollView_Previews: PreviewProvider {
    static var previews: some View {
        HorizontalScrollView()
    }
}
```

### 同时水平和垂直滚动

可以通过在 `ScrollView` 中嵌套另一个 `ScrollView` 来创建同时支持水平和垂直滚动的视图。

```swift
import SwiftUI

struct BothDirectionsScrollView: View {
    var body: some View {
        ScrollView {
            VStack(spacing: 20) {
                ForEach(0..<10) { index in
                    ScrollView(.horizontal, showsIndicators: true) {
                        HStack(spacing: 20) {
                            ForEach(0..<10) { index in
                                Text("Item \(index)")
                                    .frame(width: 100, height: 100)
                                    .background(Color.green)
                                    .cornerRadius(10)
                            }
                        }
                    }
                    .frame(height: 100)
                }
            }
        }
    }
}

struct BothDirectionsScrollView_Previews: PreviewProvider {
    static var previews: some View {
        BothDirectionsScrollView()
    }
}
```

### 添加滚动条指示器

`ScrollView` 的 `showsIndicators` 参数可以控制是否显示滚动条指示器。

### 响应滚动事件

SwiftUI `ScrollView` 没有像 UIKit 的 `UIScrollView` 那样直接提供滚动事件的回调。但是，你可以通过在 `ScrollView` 内部放置一个 `GeometryReader` 来读取滚动偏移量。

### 使用 `ScrollViewReader`

`ScrollViewReader` 可以用来在 `ScrollView` 内部进行编程式导航（比如滚动到特定的子视图）。

```swift
import SwiftUI

struct ScrollViewReaderExample: View {
    var body: some View {
        ScrollViewReader { proxy in
            ScrollView {
                VStack(spacing: 10) {
                    ForEach(0..<100) { index in
                        Text("Item \(index)")
                            .id(index)
                    }
                }
                
                Button("Jump to Item 50") {
                    withAnimation {
                        proxy.scrollTo(50, anchor: .top)
                    }
                }
            }
        }
    }
}

struct ScrollViewReaderExample_Previews: PreviewProvider {
    static var previews: some View {
        ScrollViewReaderExample()
    }
}
```

在这个例子中，点击按钮会自动滚动到第 50 个元素。`ScrollViewReader` 提供了一个 `proxy` 对象，它有一个 `scrollTo(_:anchor:)` 方法可以使用，其中 `id` 参数对应子视图的 `id`。