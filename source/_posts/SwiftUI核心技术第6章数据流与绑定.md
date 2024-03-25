---
title: SwiftUI核心技术第6章数据流与绑定
date: 2023-11-08 08:52:43
categories:
- SwiftUI
tags:
---
**第6章：数据流与绑定**

**1. State和Binding**

在SwiftUI中，数据流的管理是构建动态和响应式用户界面的核心。`State`和`Binding`是实现这种数据流动的基本工具，它们使得数据和视图能够保持同步。

**State：拥有数据的真相**

`@State` 是一个属性包装器，用于声明SwiftUI管理的状态。这个状态是私有的，仅在当前视图内部使用。当状态变化时，SwiftUI会自动重新绘制依赖于这个状态的视图部分。

```swift
struct ContentView: View {
    @State private var isToggled = false

    var body: some View {
        Toggle("开关", isOn: $isToggled)
    }
}
```

在上面的例子中，`isToggled` 是一个布尔状态，与一个开关绑定。当用户切换开关时，`isToggled` 的值会改变，触发视图的更新。

**Binding：连接状态和视图**

`Binding` 提供了对某个状态的读写权限，但不拥有这个状态本身。通过`$`符号，我们可以从一个`@State`变量创建一个`Binding`。

```swift
struct ToggleView: View {
    @Binding var isOn: Bool

    var body: some View {
        Toggle("开关", isOn: $isOn)
    }
}
```

在这个例子中，`ToggleView` 需要一个`Binding`来控制开关的状态，这个`Binding`可以从父视图的`@State`中派生而来。

**使用State和Binding**

在实际的应用中，`@State`适用于简单的局部状态管理，如界面的某个开关、文本输入框的内容等。当状态改变时，只有使用到该状态的视图会重新渲染，从而优化了性能。

`@Binding`则用于将状态的控制权从一个视图传递到另一个视图。例如，在一个父视图中定义了`@State`，那么就可以将这个状态以`Binding`的形式传递给子视图，让子视图能够读取并修改这个状态。

**State和视图的生命周期**

理解`@State`与视图的生命周期是紧密相关的也很重要。当一个视图被SwiftUI重新绘制时，`@State`所持有的状态会被保留下来。这意味着状态的变更是持久的，即使视图的某些其他部分可能因为不同的原因而重新渲染。

**总结**

`State`和`Binding`是SwiftUI中数据流的基础。`@State`用于创建可变的状态，当状态变化时，视图会响应这些变化。而`Binding`则用于在视图之间共享状态，允许多个视图共同拥有和修改状态。通过恰当地使用这两个工具，我们可以创建出既简洁又高效的响应式用户界面。在接下来的章节中，我们将深入探讨如何在更复杂的应用架构中管理状态和数据流。


**2. ObservedObject和EnvironmentObject**

在构建复杂的SwiftUI应用时，我们经常需要处理跨多个视图共享的数据。在这种情况下，仅使用`@State`和`@Binding`可能不够用，因为它们主要用于单个视图或其直接子视图的状态管理。这时，`@ObservedObject`和`@EnvironmentObject`就成为了重要的工具。

**ObservedObject：动态数据的监听者**

`@ObservedObject`用于绑定外部的可观察对象（通常是遵循`ObservableObject`协议的类实例），当可观察对象发出变化通知时，视图会重新渲染以反映新的数据。

这里有一个`ObservableObject`的示例：

```swift
class UserData: ObservableObject {
    @Published var username: String = "用户"
}
```

`@Published`属性包装器用于标记会发生变化的数据。一旦`username`的值发生变更，就会自动通知所有的观察者。

在视图中使用`@ObservedObject`：

```swift
struct UserView: View {
    @ObservedObject var userData: UserData

    var body: some View {
        Text("用户名: \(userData.username)")
    }
}
```

在`UserView`中，`userData`作为一个`@ObservedObject`提供了对`UserData`实例的引用。当`username`更新时，`UserView`也会更新其显示。

**EnvironmentObject：跨层级的数据共享**

`@EnvironmentObject`是一种特殊类型的数据流工具，它可以让数据在视图层级间传递而不需要显式地通过参数传递。它非常适合那些被多个视图访问的全局数据或设置。

首先，你需要在某个父视图中将数据对象添加到环境中：

```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
                .environmentObject(UserData())
        }
    }
}
```

然后，在子视图中，你可以直接通过`@EnvironmentObject`来访问这个数据对象：

```swift
struct ProfileView: View {
    @EnvironmentObject var userData: UserData

    var body: some View {
        Text("欢迎, \(userData.username)!")
    }
}
```

不需要显式地从父视图传递`UserData`到`ProfileView`，`ProfileView`可以直接从环境中获取这个对象。

**总结**

`@ObservedObject`和`@EnvironmentObject`为我们提供了强大的数据管理能力，使得数据在视图之间传递变得简单且高效。它们都依赖于`ObservableObject`协议来观察数据模型的变化并响应更新。`@ObservedObject`适用于需要直接引用的情况，而`@EnvironmentObject`更适合于全局或共享数据的情况，尤其是在视图层级较深时。

在接下来的内容中，我们将探讨这些数据流工具如何与SwiftUI的声明式UI框架协同工作，以及如何利用它们来构建响应式和可维护的应用架构。


**3. @Published和Combine**

在SwiftUI中，数据的流动和管理是构建应用的核心。为了实现响应式的数据流，SwiftUI密切结合了Combine框架。Combine是一个响应式编程框架，它可以处理所有类型的异步事件。`@Published`是Combine框架中的一个关键特性，它用于创建可观察的对象属性，当这些属性的值发生变化时，它会自动通知系统。

**使用@Published**

使用`@Published`可以很容易地将一个类属性变成响应式的属性。这意味着，当属性的值改变时，所有订阅了这个属性的订阅者都会接收到通知，并且可以响应这些变化。

下面是一个使用`@Published`的例子：

```swift
import Combine

class ProfileViewModel: ObservableObject {
    @Published var name: String = ""
    @Published var age: Int = 0
}
```

在这个例子中，`ProfileViewModel`是一个遵循`ObservableObject`协议的类，它有两个`@Published`属性：`name`和`age`。当这些属性中的任何一个的值改变时，所有的观察者都会得到通知。

**整合Combine**

Combine框架的强大之处在于它可以让你定义复杂的数据处理和变换流程。例如，你可以对输入进行校验、过滤、转换，然后将处理后的数据传递到UI或其他部分。

这里是如何使用Combine订阅`@Published`属性变化的例子：

```swift
var cancellables = Set<AnyCancellable>()

let profileVM = ProfileViewModel()
profileVM.$name
    .sink { name in
        print("Name is now \(name)")
    }
    .store(in: &cancellables)
```

在这段代码中，`$name`是对`name`属性的Publisher访问。`.sink`方法会接收一个闭包，这个闭包会在每次`name`属性更新时被调用。`.store(in:)`方法用于管理订阅生命周期，防止早期释放。

**结合SwiftUI视图**

在SwiftUI中，你通常不需要直接处理订阅，因为SwiftUI视图可以直接使用`@ObservedObject`或`@EnvironmentObject`来绑定到可观察的对象。

```swift
struct ProfileView: View {
    @ObservedObject var viewModel: ProfileViewModel

    var body: some View {
        TextField("Name", text: $viewModel.name)
        TextField("Age", value: $viewModel.age, formatter: NumberFormatter())
    }
}
```

在`ProfileView`中，每当`viewModel`的`name`或`age`属性变化时，视图会自动更新。

**小结**

`@Published`与Combine框架结合使用，为SwiftUI应用带来了强大的响应式编程能力。这种模式不仅使状态管理变得简洁，还能够创建可维护和可扩展的数据流处理逻辑。随着你深入本书，你将会看到更多关于Combine在实际SwiftUI应用中的强大用例和模式。


**4. 数据流的最佳实践**

在构建SwiftUI应用时，高效和可维护的数据流管理是非常重要的。为了达到这个目标，我们需要遵循一些最佳实践来确保我们的应用结构清晰，数据流动合理。以下是在使用SwiftUI时应考虑的数据流最佳实践。

**一、明确数据源的单一真相**

在任何给定的时刻，应用的每个数据点都应该有一个清晰的，可信的来源。这意味着对于任何可变的数据，都应该有一个单一的可信源，而所有视图的状态应反映这个来源。使用`@State`的私有属性用于视图的内部状态，而模型对象中的`@Published`属性用于应用范围的状态。

**二、使用单向数据流**

在SwiftUI中，数据应该从父视图流向子视图（单向数据流）。父视图传递数据到子视图，子视图通过事件传递回父视图，而不是直接修改父视图的状态。这样可以避免复杂的数据依赖和潜在的循环更新问题。

**三、合理使用@State，@Binding，@ObservedObject，和@EnvironmentObject**

- **@State** 应当用于视图的局部状态管理，不应跨越多个视图。
- **@Binding** 允许子视图与父视图的状态或模型中的数据进行通信。
- **@ObservedObject** 用于当视图需要响应外部模型对象变化时。
- **@EnvironmentObject** 适用于多个视图需要访问同一共享数据对象的情况。

**四、谨慎管理生命周期**

识别并管理数据对象的生命周期，尤其是当使用`@ObservedObject`或`@EnvironmentObject`时。避免不必要的重新创建对象，以减少内存使用和性能损耗。

**五、精细控制数据变更**

使用`objectWillChange`手动发送变更通知可以精细控制观察的对象何时更新UI。当需要优化性能或处理复杂的数据变更时这非常有用。

**六、利用Combine进行复杂的数据操作**

Combine框架提供了一套完整的工具，用于处理复杂的数据转换和异步操作。应充分利用这些工具来实现复杂的数据流和事件处理。

**七、避免内存泄漏**

当处理数据流和绑定时，确保正确管理订阅，使用`AnyCancellable`存储返回的订阅，并在不需要时取消订阅，以避免内存泄漏。

**八、编写可测试的代码**

将数据处理逻辑抽象到可单独测试的模型和服务中。避免将业务逻辑放入视图中，这样可以让代码更容易被测试和维护。

**小结**

遵循上述的数据流最佳实践将有助于您构建出高效、稳定且易于维护的SwiftUI应用。确保理解和正确应用每一种属性装饰器和Combine操作符是至关重要的。本书后续章节将会进一步深入这些概念，并结合实例演示如何在真实世界的应用中实践这些最佳实践。