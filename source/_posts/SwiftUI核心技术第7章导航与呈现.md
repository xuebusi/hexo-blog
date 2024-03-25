---
title: SwiftUI核心技术第7章导航与呈现
date: 2023-11-08 09:09:18
categories:
- SwiftUI
tags:
---
**第7章：导航与呈现**

**1. NavigationView和NavigationViewLink**

在SwiftUI中，`NavigationView`和`NavigationLink`组合起来使用，提供了一个强大的导航框架，让我们可以构建具有层次结构的页面间跳转。它们的设计遵循了SwiftUI的声明式语法，使得页面跳转和数据传递变得直观和易于管理。

**NavigationView**

`NavigationView`是一个容器视图，它承载着你的视图层次结构，并提供了展示这些视图的空间。你可以把它看作是页面导航的起点。通常，一个`NavigationView`包含一个或多个`View`，这些`View`可以通过`NavigationLink`来进行跳转。

```swift
NavigationView {
    List(selection: $selectedItem) {
        NavigationLink(destination: DetailView(item: item)) {
            Text(item.title)
        }
    }
    .navigationBarTitle(Text("Items"))
}
```

在这个例子中，`NavigationView`包含了一个列表，列表中的每一项都绑定了一个`NavigationLink`，点击时将展示`DetailView`。

**NavigationLink**

`NavigationLink`负责在`NavigationView`中触发页面跳转。它有多个初始化方法，可以根据需求选择使用。最基本的用法是提供一个目标视图和触发跳转的内容。

```swift
NavigationLink(destination: DetailView(item: item)) {
    Text(item.title)
}
```

这里`NavigationLink`的目的地是`DetailView`，用户点击列表项上的`Text`时会触发导航到`DetailView`。

**激活和反激活链接**

`NavigationLink`可以与SwiftUI的状态绑定，以编程方式激活或反激活导航。通过在`NavigationLink`初始化时传递一个布尔型的绑定，你可以控制导航的行为。

```swift
@State private var isLinkActive = false

var body: some View {
    NavigationView {
        NavigationLink(destination: DetailView(), isActive: $isLinkActive) {
            Button("Go to Details") {
                self.isLinkActive = true
            }
        }
    }
}
```

在这个例子中，一个按钮被用来激活`NavigationLink`。当用户点击按钮，`isLinkActive`状态变为`true`，触发导航到`DetailView`。

**传递数据**

在导航过程中，你可能需要向目的地视图传递数据。`NavigationLink`的`destination`参数让你可以轻松做到这一点。

```swift
NavigationLink(destination: DetailView(item: item)) {
    ItemRow(item: item)
}
```

在这里，每个`ItemRow`是一个列表中的行，它带有一个与之关联的`item`数据模型。点击`ItemRow`时，`item`将被传递到`DetailView`。

**小结**

`NavigationView`和`NavigationLink`构成了SwiftUI中页面导航的基石。它们的使用方式灵活而强大，支持各种复杂的导航模式。通过声明式语法，我们可以更专注于视图的内容和业务逻辑，而不是导航的细节。本章节的后续部分将探讨更高级的导航模式，如使用`TabView`和`modal`呈现，以及如何处理数据传递和状态管理。


**2. TabView**

在构建一个用户界面时，底部的标签栏是一种常见的设计，允许用户轻松切换不同的视图或功能模块。在SwiftUI中，这可以通过`TabView`来实现。`TabView`为应用提供了一个选项卡式的界面，每个选项卡都代表了一个视图。

**基本用法**

`TabView`的基本用法涉及到声明式地列出每一个选项卡，并使用`tabItem`修饰符来定义每个标签的外观，通常包括图标和文本。

```swift
TabView {
    Text("首页")
        .tabItem {
            Image(systemName: "house")
            Text("Home")
        }
    
    Text("设置")
        .tabItem {
            Image(systemName: "gear")
            Text("Settings")
        }
}
```

在这个简单的例子中，`TabView`包含了两个选项卡：“首页”和“设置”，每个选项卡都是用`Text`视图表示的，并且有对应的系统图标。

**选项卡状态管理**

`TabView`可以与`@State`变量绑定，从而允许你编程方式控制当前激活的选项卡。

```swift
@State private var selectedTab = 0

var body: some View {
    TabView(selection: $selectedTab) {
        HomeView()
            .tabItem {
                Image(systemName: "house")
                Text("Home")
            }
            .tag(0)
        
        SettingsView()
            .tabItem {
                Image(systemName: "gear")
                Text("Settings")
            }
            .tag(1)
    }
}
```

在上述代码中，`selectedTab`绑定到`TabView`的`selection`参数。通过改变`selectedTab`的值，你可以改变当前选中的标签页。

**自定义外观**

`TabView`的外观可以通过多种方式进行自定义。例如，可以使用`.accentColor`来改变选中标签的颜色。

```swift
TabView {
    // ... 你的选项卡
}
.accentColor(.green)
```

**结合NavigationView使用**

在`TabView`内部，你可能还会嵌入`NavigationView`，以在选项卡内部提供导航堆栈。

```swift
TabView {
    NavigationView {
        HomeView()
    }
    .tabItem {
        Image(systemName: "house")
        Text("Home")
    }
    // ... 其他选项卡
}
```

这样做可以确保每个选项卡都能拥有独立的导航历史，这对于用户体验非常重要。

**使用场景**

`TabView`在很多类型的应用中都非常有用。它是构建具有多个独立部分的应用的理想选择，例如社交网络应用、具有个人中心、消息、设置等的应用。

**小结**

通过本节的介绍，我们了解了如何使用`TabView`创建具有多个交互式选项卡的界面。`TabView`不仅提供了高度的可定制性，还能够很好地与其他视图和数据流模式协同工作。在接下来的小节中，我们将继续探索更多关于SwiftUI中视图呈现和数据流管理的先进概念。


**3. Sheets和Alerts**

在构建应用时，弹出视图和警告框（Alerts）是与用户交互的重要方式。在SwiftUI中，这通常通过使用`Sheet`和`Alert`视图完成。

**Sheets**

Sheet是一种覆盖在当前内容上的卡片样式视图，通常用于导航流程之外的辅助任务，比如表单填写、设置选项等。

**创建Sheet**

要创建一个Sheet，你需要使用`.sheet`修饰符，并为其提供一个绑定的布尔值，这个布尔值决定Sheet是否可见。

```swift
@State private var showingSheet = false

var body: some View {
    Button("Show Sheet") {
        showingSheet.toggle()
    }
    .sheet(isPresented: $showingSheet) {
        // Sheet的内容
        Text("Here's the Sheet")
    }
}
```

**自定义Sheet**

Sheet的内容可以是任意视图。例如，你可以创建一个包含表单的`NavigationView`。

```swift
.sheet(isPresented: $showingSheet) {
    NavigationView {
        Form {
            // 表单内容
        }
        .navigationBarTitle("Settings", displayMode: .inline)
        .navigationBarItems(trailing: Button("Done") {
            showingSheet = false
        })
    }
}
```

**使用Sheet实现细节**

在内部，Sheet会自动管理自己的显示和隐藏。你可以将`showingSheet`绑定到视图的某个状态或者对象的属性上，当这个属性变化时，对应的Sheet会自动显示或隐藏。

**Alerts**

Alerts用于显示重要信息，并可以提供一个或多个操作选项。

**创建Alert**

和Sheet类似，Alert也使用绑定的布尔值来控制显示状态。不过，创建Alert时，通常还会指定标题、消息和按钮。

```swift
@State private var showingAlert = false

var body: some View {
    Button("Show Alert") {
        showingAlert = true
    }
    .alert(isPresented: $showingAlert) {
        Alert(
            title: Text("Warning"),
            message: Text("Are you sure?"),
            primaryButton: .destructive(Text("Delete")) {
                // 删除操作
            },
            secondaryButton: .cancel()
        )
    }
}
```

**Alert的样式**

Alert可以有多种样式，从简单的带有单一按钮的Alert到复杂的带有多个按钮和自定义操作的Alert。

**小结**

Sheet和Alert都是在SwiftUI中管理临时视图和用户交互的重要工具。通过合理使用这两种视图，你可以创建出既直观又有效的用户体验。正如我们所见，SwiftUI提供的`.sheet`和`.alert`修饰符让这两种视图的展示和管理变得十分简单。在接下来的章节中，我们将深入探讨如何利用SwiftUI的高级特性来构建更为复杂的用户交互界面。


**4. Navigation的高级用法**

SwiftUI的导航系统提供了一种直观且声明式的方法来处理视图间的转换。除了基础的`NavigationView`和`NavigationLink`之外，我们还可以采用更高级的用法来增强用户体验和视图的灵活性。

**程序化导航**

在某些情况下，你可能需要从视图模型或响应某个事件时进行导航。这可以通过绑定到视图模型中的属性并使用`NavigationLink`的`isActive`参数实现。

```swift
class ViewModel: ObservableObject {
    @Published var isDetailViewActive = false
}

struct ContentView: View {
    @StateObject var viewModel = ViewModel()

    var body: some View {
        NavigationView {
            NavigationLink(destination: DetailView(), isActive: $viewModel.isDetailViewActive) { 
                EmptyView()
            }
            Button("Go to Details") {
                viewModel.isDetailViewActive = true
            }
        }
    }
}
```

**隐藏和显示导航栏**

在用户界面中，有时候我们需要隐藏导航栏来提供沉浸式体验，或者我们需要在特定的视图中改变导航栏的显示方式。

```swift
struct DetailView: View {
    var body: some View {
        // 隐藏导航栏
        .navigationBarHidden(true)
        // 在视图即将出现时设置导航栏样式
        .onAppear {
            // 设置导航栏样式
        }
    }
}
```

**自定义后退按钮行为**

有时默认的后退按钮行为并不符合我们的需要。在SwiftUI中，我们可以通过添加一个自定义的按钮并绑定其行为来覆盖默认的后退按钮。

```swift
struct CustomBackButtonView: View {
    @Environment(\.presentationMode) var presentationMode

    var body: some View {
        Button(action: {
            // 执行自定义后退操作
            presentationMode.wrappedValue.dismiss()
        }) {
            HStack {
                Image(systemName: "arrow.left")
                Text("Back")
            }
        }
    }
}
```

**深度链接**

深度链接是指直接导航到应用内部的某个特定页面的链接。在SwiftUI中，我们可以监听来自URL的深度链接，并根据链接的内容导航到相应的视图。

```swift
.onOpenURL { url in
    // 解析URL并进行导航
}
```

**嵌套导航**

在构建复杂的界面时，我们可能会需要嵌套多个`NavigationView`。虽然这在用户界面上并不常见，但在特定的设计中可能是必要的。嵌套导航需要细心处理，以确保导航栈的正确和流畅的用户体验。

```swift
NavigationView {
    // 主界面
    NavigationView {
        // 嵌套的子界面
    }
}
```

**小结**

通过上述高级导航功能，开发者可以在SwiftUI中创建复杂且具有出色用户体验的导航流。这些高级技巧的掌握可以帮助你更好地管理视图层级和导航逻辑，让应用的导航更加直观和响应用户操作。在后续章节中，我们将探索如何将这些高级导航技巧与应用的其它部分相结合，以构建完整且功能丰富的应用程序。