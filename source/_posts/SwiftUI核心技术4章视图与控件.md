---
title: SwiftUI核心技术4章视图与控件
date: 2023-11-08 00:35:47
categories:
- SwiftUI
tags:
---
**第二部分：构建用户界面**

**第4章：视图与控件**

**1. Text和Image**

在SwiftUI中，用户界面是由各种视图组成的，其中最基本的两种视图是`Text`和`Image`。本节将详细探讨如何使用这两种视图来显示文本和图像。

**Text**

`Text`视图用于在应用中显示一行或多行只读文本。它是最常见的视图之一，因为文本是用户交互的基本元素。

**基础用法**

创建一个`Text`视图非常简单，只需要传入一个字符串即可：

```swift
Text("Hello, SwiftUI!")
```

**样式定制**

`Text`视图提供了多种修饰符来定制文本的显示方式：

- `.font(_:)`：设置字体样式。
- `.fontWeight(_:)`：设置字体的粗细。
- `.foregroundColor(_:)`：设置文本颜色。
- `.lineLimit(_:)`：设置最多显示行数。
- `.multilineTextAlignment(_:)`：设置多行文本的对齐方式。
- `.padding(_:)`：为文本周围添加填充。

例如，要创建一个居中对齐、蓝色、加粗的文本，您可以这样写：

```swift
Text("Welcome to SwiftUI")
    .font(.headline)
    .fontWeight(.bold)
    .foregroundColor(.blue)
    .multilineTextAlignment(.center)
    .padding()
```

**国际化和本地化**

SwiftUI还支持文本的国际化和本地化。使用`LocalizedStringKey`初始化`Text`视图，可以确保文本根据用户的设备语言环境显示正确：

```swift
Text(LocalizedStringKey("hello_message"))
```

**Image**

`Image`视图用于在应用中显示图像。您可以从应用的资产目录、文件系统或网络加载图像。

**从资产目录加载**

最常用的加载图像方式是从Xcode项目的Assets.xcassets目录：

```swift
Image("myImage")
```

**图像修饰符**

`Image`视图也可以使用多种修饰符来调整显示的图像：

- `.resizable()`：允许图像根据视图的大小进行拉伸或压缩。
- `.aspectRatio(_:_:)`：设置图像的宽高比。
- `.clipShape(_:)`：剪切图像到特定的形状。
- `.shadow(_:)`：为图像添加阴影。

例如，要创建一个圆形、有阴影的图像，您可以这样写：

```swift
Image("profile_pic")
    .resizable()
    .aspectRatio(contentMode: .fill)
    .frame(width: 100, height: 100)
    .clipShape(Circle())
    .shadow(radius: 10)
```

**加载和显示网络图像**

要显示来自网络的图像，您通常需要使用SwiftUI的`AsyncImage`视图或结合URLSession自定义一个加载器。由于本节重点是`Image`视图，我们将在后续章节详细探讨如何加载网络图像。

**总结**

`Text`和`Image`是构建SwiftUI应用界面时最基础的元素。它们不仅用法简单，而且通过修饰符提供了强大的样式定制能力。通过合理使用这两种视图，您可以快速构建出丰富多彩且吸引人的界面。接下来的小节，我们将进一步探索SwiftUI中其他重要的视图和控件。


**2. Buttons和Toggle**

在SwiftUI中，`Button`和`Toggle`是两种用于用户交互的基本控件。它们使应用可以响应用户的操作，执行任务或更改状态。

**Button**

按钮是用户界面的基本组件，用于响应用户的点击或触摸操作。

**创建按钮**

在SwiftUI中，创建一个按钮需要提供一个动作和一个如何显示的内容：

```swift
Button(action: {
    // 在这里执行按钮的动作
    print("按钮被点击")
}) {
    // 提供按钮的内容
    Text("点击我")
}
```

**按钮样式**

您可以使用修饰符来定制按钮的样式。例如，给按钮添加边框、背景色等：

```swift
Button("点击我") {
    print("按钮被点击")
}
.frame(width: 200, height: 60)
.background(Color.blue)
.foregroundColor(.white)
.cornerRadius(10)
.padding()
```

SwiftUI还提供了`buttonStyle(_:)`修饰符来应用预定义的按钮样式。

**Toggle**

`Toggle`是一个开关控件，用于表示和改变一个布尔值的状态。

**创建Toggle**

创建一个`Toggle`同样需要一个绑定的状态和一个显示的标签：

```swift
@State private var isOn = false

var body: some View {
    Toggle(isOn: $isOn) {
        Text("切换状态")
    }
}
```

在上面的代码中，`isOn`是一个`@State`属性，这意味着它是一个可变状态，当`Toggle`被切换时，视图会自动更新。

**定制Toggle**

可以使用`.toggleStyle(_:)`修饰符来定制`Toggle`的外观。SwiftUI提供了一些内建的样式，如`SwitchToggleStyle`和`CheckboxToggleStyle`（后者在macOS上可用）。

**绑定和控制**

按钮和开关的强大之处在于它们与SwiftUI的数据绑定系统的整合。当您使用`$`前缀创建绑定时，UI 控件将能够直接修改数据，反之亦然。这是SwiftUI声明式编程范式的核心。

**响应用户输入**

通常，按钮和开关会更改应用的状态或触发某个操作。例如，您可能会根据开关的状态显示或隐藏文本视图：

```swift
@State private var isAccepted = false

var body: some View {
    VStack {
        Toggle(isOn: $isAccepted) {
            Text("接受条款和条件")
        }

        Button("继续") {
            // 可以在这里校验开关状态，例如是否接受了条款和条件
            proceedWithAction()
        }
        .disabled(!isAccepted) // 当不接受条款时禁用按钮
    }
}
```

**总结**

按钮和开关是任何交互式应用程序的基础，它们提供了一种简单有效的方式来收集用户输入并作出反应。SwiftUI的`Button`和`Toggle`视图配合数据绑定，使得创建动态和响应式的用户界面变得异常简单。它们的样式和行为可以通过一系列的修饰符进行定制，以适应您的设计需求。在接下来的小节中，我们将继续探讨如何使用SwiftUI构建更复杂的用户界面元素。


**3. TextField和Slider**

在SwiftUI中，创建交互式表单和控制元素是构建现代应用程序不可或缺的一部分。`TextField`和`Slider`是两种常用的控件，它们允许用户输入文本和选择值的范围。

**TextField**

`TextField`是一个用于用户输入文本的控件，它可以接受键盘输入，并且可以对输入的文本进行格式化和校验。

**创建TextField**

创建一个`TextField`通常需要两个参数：一个标签和一个绑定到文本值的变量。

```swift
@State private var username: String = ""

var body: some View {
    TextField("用户名", text: $username)
}
```

在这个例子中，每当用户在文本字段中输入时，`username`变量都会更新。`@State`属性包装器用于在本地视图状态中存储可变数据。

**定制TextField**

您可以使用修饰符来定制`TextField`的外观和行为，例如设置字体、颜色、对齐方式、键盘类型等。

```swift
TextField("用户名", text: $username)
    .textFieldStyle(RoundedBorderTextFieldStyle())
    .padding()
    .keyboardType(.default)
    .autocapitalization(.none)
    .disableAutocorrection(true)
```

在这个例子中，我们使用了`textFieldStyle(_:)`来为文本字段设置圆角边框样式，并进行了一些其他的配置。

**Slider**

`Slider`允许用户从一个范围内选择一个值。它可以用于设置音量、选择亮度或应用任何其他需要用户选择一个数值的场景。

**创建Slider**

创建`Slider`至少需要一个绑定到数值的变量和一个值的范围。

```swift
@State private var sliderValue: Double = 0.5

var body: some View {
    Slider(value: $sliderValue, in: 0...1)
}
```

这个`Slider`允许用户在0到1之间选择一个值。

**定制Slider**

`Slider`同样可以使用修饰符进行外观和功能的定制。您可以设置步长，决定滑块在变化时是否持续触发更新，以及添加标签。

```swift
Slider(
    value: $sliderValue,
    in: 0...1,
    step: 0.1,
    onEditingChanged: { editing in
        // editing 是一个布尔值，表示是否正在编辑
        print("当前滑块的值：\(sliderValue)")
    }
)
.accentColor(.green)
```

在这个例子中，滑块的步长设置为0.1，且我们还指定了一个闭包，在用户编辑滑块值时触发。

**总结**

`TextField`和`Slider`为SwiftUI应用提供了基本的用户输入功能。通过与`@State`或其他形式的状态管理相结合，它们为开发者提供了创建动态和响应式表单的能力。定制这些控件的外观和行为使它们能够匹配应用程序的设计语言，提供更好的用户体验。在接下来的章节中，我们将探索SwiftUI中的其他高级控件和视图，并学习如何将它们组合在一起以构建复杂和功能丰富的用户界面。


**4. 自定义视图和控件**

SwiftUI的真正魅力之一在于它为开发者提供了丰富的自定义视图和控件的能力。通过结合现有的视图和控件以及Swift语言的强大特性，我们可以创建完全定制的用户界面元素，以完美地适应我们的设计需求。

**理解视图的组合**

在SwiftUI中，最基本的自定义视图起始于现有的视图的组合。SwiftUI的视图是可组合的，意味着你可以将简单的视图组合成复杂的视图。比如，我们可以创建一个带有文本和图像的自定义按钮视图：

```swift
struct CustomButton: View {
    var body: some View {
        HStack {
            Image(systemName: "star.fill")
                .resizable()
                .frame(width: 20, height: 20)
            Text("收藏")
        }
        .padding()
        .background(Color.blue)
        .foregroundColor(.white)
        .cornerRadius(10)
    }
}
```

这个`CustomButton`就是一个自定义视图，它可以在任何其他的SwiftUI视图中使用。

**创建完全自定义的视图**

如果需要更高级的自定义，你可以从`View`协议开始，实现自己的`body`属性。这允许你控制视图的渲染方式，并响应用户的输入。

```swift
struct CircularProgressView: View {
    var progress: Double // 从0.0到1.0
    
    var body: some View {
        ZStack {
            Circle()
                .stroke(lineWidth: 20)
                .opacity(0.3)
                .foregroundColor(Color.blue)
            
            Circle()
                .trim(from: 0.0, to: CGFloat(min(self.progress, 1.0)))
                .stroke(style: StrokeStyle(lineWidth: 20, lineCap: .round, lineJoin: .round))
                .foregroundColor(Color.blue)
                .rotationEffect(Angle(degrees: 270.0))
                .animation(.linear)
        }
    }
}
```

在这个例子中，我们创建了一个显示进度的环形视图，它会根据`progress`属性显示不同的填充量。

**响应用户交互**

自定义视图可以通过各种手势识别器来响应用户的交互。例如，你可能会有一个自定义滑块控件，它通过拖动来改变值：

```swift
struct CustomSlider: View {
    @Binding var value: Double // 绑定到外部状态
    
    var body: some View {
        GeometryReader { geometry in
            Rectangle()
                .foregroundColor(.gray)
                .frame(height: 20)
                .gesture(
                    DragGesture(minimumDistance: 0)
                        .onChanged { gesture in
                            self.value = Double(gesture.location.x / geometry.size.width)
                        }
                )
        }
    }
}
```

在这个例子中，我们使用`GeometryReader`来获取视图的大小，并根据用户拖动的位置来更新`value`。

**保持性能**

在创建自定义视图和控件时，要记住保持它们的性能。这意味着：

- 避免不必要的视图重绘和状态更新。
- 合理使用`.animation()`和`.transition()`修饰符来为视图变化提供流畅的过渡效果。
- 当视图层次变得复杂时，考虑使用`drawingGroup()`或`cache`等优化技术。

**总结**

SwiftUI提供了强大的工具集，可以帮助开发者创建精美且功能丰富的自定义视图和控件。通过结合视图的组合、自定义绘制、响应用户输入等技术，你可以创建出完全符合你的设计和功能需求的用户界面元素。记住，自定义视图和控件不仅仅是关于创造外观，更重要的是它们提供了

与用户交互的新方式。随着你的SwiftUI技能的提高，你将能够更有效地利用这些工具，打造出既美观又高效的应用程序。
