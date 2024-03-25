---
title: SwiftUI中的Image组件的用法
date: 2023-11-07 20:19:55
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`Image` 视图用于显示图片。你可以使用多种来源的图片，包括应用的资源包、系统图标库以及从网络获取的图片。下面是一些 `Image` 组件的基本和进阶用法。

### 基础用法

#### 从 Assets.xcassets 加载图片：

```swift
Image("myImage") // "myImage" 是你的图片在 Assets.xcassets 中的名称
```

#### 使用系统图标：

```swift
Image(systemName: "star.fill") // 使用 SF Symbols 的图标
```

### 常用修饰符

#### 重新着色（tinting）：

```swift
Image("myImage")
    .resizable() // 允许图片被拉伸
    .renderingMode(.template) // 将图片设置为模板模式
    .foregroundColor(.blue) // 改变图片颜色
```

#### 缩放以适应容器大小：

```swift
Image("myImage")
    .resizable() // 允许图片被拉伸
    .aspectRatio(contentMode: .fit) // 保持图片的宽高比
```

#### 填充整个容器：

```swift
Image("myImage")
    .resizable()
    .aspectRatio(contentMode: .fill) // 图片可能会超出容器大小
    .clipped() // 超出部分将被裁剪
```

#### 调整图片大小：

```swift
Image("myImage")
    .resizable()
    .frame(width: 100, height: 100) // 指定图片的宽和高
```

#### 圆形剪裁：

```swift
Image("myImage")
    .resizable()
    .scaledToFill()
    .frame(width: 100, height: 100)
    .clipShape(Circle()) // 将图片剪裁成圆形
    .overlay(Circle().stroke(Color.white, lineWidth: 4)) // 圆形边框
    .shadow(radius: 10) // 阴影效果
```

### 高级用法

#### 图片叠加（Overlay）：

```swift
Image("myImage")
    .resizable()
    .scaledToFit()
    .overlay(
        Text("Overlay") // 在图片上添加文本覆盖层
            .font(.caption)
            .padding(5)
            .background(Color.black.opacity(0.5))
            .cornerRadius(5)
            .foregroundColor(.white)
            .padding(10), // 文本内边距
        alignment: .bottomTrailing // 对齐到图片的右下角
    )
```

#### 图片遮罩（Masking）：

```swift
Image("myImage")
    .resizable()
    .scaledToFit()
    .mask(
        Text("SwiftUI") // 使用文本作为遮罩
            .font(.system(size: 72))
            .bold()
    )
```

#### 图片动画：

```swift
@State private var isRotated = false

var body: some View {
    Image("myImage")
        .resizable()
        .rotationEffect(.degrees(isRotated ? 360 : 0)) // 旋转动画
        .animation(.linear(duration: 2), value: isRotated)
        .onAppear {
            isRotated.toggle()
        }
}
```

### 使用网络图片

在 SwiftUI 中，没有内置从网络直接加载图片的方法。通常，你需要使用 `URLSession` 获取图片数据，然后将数据转换为 `UIImage`（在 iOS 上）或 `NSImage`（在 macOS 上），最后将其转换为 SwiftUI 可以使用的 `Image`。或者你可以使用第三方库，如 `SDWebImageSwiftUI` 来简化网络图片的加载过程。

在 SwiftUI 2.0 之后的版本，`AsyncImage` 视图提供了一个原生的异步加载网络图片的方法：

```swift
AsyncImage(url: URL(string: "https://example.com/myImage.png")) { image in
    image.resizable() // 如果成功加载，显示图片
}
placeholder: {
    ProgressView() // 在加载期间显示加载指示器
}
.frame(width: 100, height: 100)
```

`Image` 视图在 SwiftUI 中是核心组件之一，它配

合修饰符和其他视图，能够实现丰富多彩的用户界面元素。