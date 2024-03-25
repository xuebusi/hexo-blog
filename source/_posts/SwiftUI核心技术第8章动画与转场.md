---
title: SwiftUI核心技术第8章动画与转场
date: 2023-11-08 09:13:02
categories:
- SwiftUI
tags:
---
**第8章：动画与转场**

**第1小节：基础动画**

在SwiftUI中，动画是用来增强用户体验的一种手段，它可以提供平滑的视觉过渡和引人注目的交互。通过使用SwiftUI强大的动画API，你可以轻松地为界面元素添加动画效果。

**动画的类型**

在SwiftUI中，动画主要可以分为隐式动画和显式动画两大类。

1. **隐式动画**：最简单的动画形式，仅需要使用`.animation()`修饰符，并为其提供一个动画样式。
2. **显式动画**：使用`withAnimation`函数来明确开始动画的时间点，并提供一个动画样式。

**创建一个简单的隐式动画**

你可以通过在视图的某个状态改变时附加`.animation()`修饰符，让这个状态改变带有动画效果。

```swift
struct SimpleAnimationView: View {
    @State private var scale: CGFloat = 1

    var body: some View {
        Circle()
            .scaleEffect(scale)
            .animation(.easeInOut, value: scale)
            .onTapGesture {
                scale += 1
            }
    }
}
```

在上面的例子中，每当用户点击圆形，`scale`状态变量就会增加，而圆形的尺寸变化将以渐进渐出的方式动画显示。

**使用`withAnimation`进行显式动画**

如果你想控制动画的触发时机，而不是依赖于状态的改变，你可以使用`withAnimation`来显式地执行动画。

```swift
Button("Tap Me") {
    withAnimation(.spring(response: 0.5, dampingFraction: 0.5, blendDuration: 1)) {
        scale += 1
    }
}
```

在这个例子中，当按钮被点击时，不管`scale`的值如何变化，动画都会被执行。

**动画的组合**

你可以将不同的动画效果组合在一起，创建复杂的动画序列。这通过在`.animation()`修饰符中使用`Animation`的静态方法来实现。

```swift
.animation(
    Animation.easeInOut(duration: 2).repeatForever(autoreverses: true)
)
```

此代码会创建一个无限重复并自动反向的渐进渐出动画。

**动画参数**

SwiftUI提供了许多可以调整动画行为的参数：

- `duration`：动画的时长。
- `delay`：动画开始前的等待时间。
- `repeatCount`：动画重复的次数。
- `autoreverses`：动画是否在完成后自动反向。

**小结**

基础动画是SwiftUI中最容易实现的动画类型，它们可以快速为你的应用添加视觉吸引力和反馈。通过使用隐式动画或显式动画，你可以控制动画的触发方式和行为。此外，SwiftUI的动画系统是高度可组合的，允许你通过组合和定制动画参数来创建复杂的动画效果。

在本书的后续章节中，我们将深入探讨如何使用SwiftUI的高级动画功能，比如路径动画、自定义时间曲线以及动画的联动效果，来创建更具表现力和创造力的用户界面。


**第2小节：自定义动画**

在SwiftUI中，除了内置的动画类型，开发者还可以通过自定义动画来进一步个性化UI交互。自定义动画允许开发者控制动画的具体行为和时间曲线，实现独特的动态效果。

**动画时间曲线**

时间曲线描述了动画状态值随时间的变化方式。SwiftUI提供了几种内置的时间曲线，例如`easeIn`、`easeOut`和`easeInOut`。要自定义这些曲线，你可以使用`timingCurve(_:_:_:_:)`方法。

```swift
.animation(.timingCurve(0.2, 0.8, 0.2, 1, duration: 0.5))
```

这个方法接受四个表示贝塞尔曲线控制点的参数，可以非常精细地控制动画的加速和减速过程。

**使用`interpolatingSpring`自定义弹簧动画**

如果你需要一个物理弹性效果，可以使用`interpolatingSpring(stiffness:damping:)`函数来自定义一个弹簧动画。

```swift
.animation(.interpolatingSpring(stiffness: 50, damping: 5))
```

调整`stiffness`（刚度）和`damping`（阻尼）参数可以模拟不同的弹簧物理特性。

**使用`Animation`的`delay(_: )`和`speed(_: )`自定义动画速度和延迟**

```swift
.animation(.easeInOut(duration: 2).delay(0.5).speed(2))
```

这里，动画会在半秒延迟后开始，并且以原始时长的两倍速度播放。

**自定义动画路径**

SwiftUI动画不仅限于简单的开始和结束状态之间的过渡。使用`GeometryEffect`，你可以创建完全自定义的动画路径。

```swift
struct CustomAnimationView: View {
    @State private var isAnimated = false

    var body: some View {
        Circle()
            .frame(width: 100, height: 100)
            .modifier(CustomPathModifier(isAnimated: $isAnimated))
            .onTapGesture {
                withAnimation {
                    isAnimated.toggle()
                }
            }
    }
}

struct CustomPathModifier: GeometryEffect {
    @Binding var isAnimated: Bool

    func effectValue(size: CGSize) -> ProjectionTransform {
        let path = UIBezierPath()
        path.move(to: CGPoint(x: 0, y: isAnimated ? size.height : 0))
        // Your custom path here
        return ProjectionTransform(CGAffineTransform(translationX: path.currentPoint.x, y: path.currentPoint.y))
    }
}
```

通过创建一个遵循`GeometryEffect`协议的自定义修饰符，你可以定义一个复杂的动画路径，这个路径由一个点沿着给定路径移动的动画组成。

**自定义动画的应用场景**

- 当内置动画不能满足你的设计需求时。
- 当你想要创建一个与众不同的动态交互体验。
- 在需要精确控制动画行为，如游戏或特定动画教程中。

**小结**

自定义动画是SwiftUI动画功能的深入使用。通过控制时间曲线、弹簧动画参数和自定义动画路径，你可以创造出个性化和专业级别的动态效果。自定义动画能提升应用的专业感和用户的交互体验，使你的应用在众多相似应用中脱颖而出。

接下来，我们将探讨转场动画，它们可以为视图的呈现和消失提供引人注目的视觉效果。


**第3小节：交互式和响应式动画**

在构建现代的用户界面时，我们不仅希望动画能够提供视觉上的引导和反馈，还希望它们能够与用户的交互紧密结合，创造流畅的体验。这就是交互式和响应式动画发挥作用的地方。在SwiftUI中，你可以根据用户的输入或其他事件，实时地调整动画，从而创建出高度交互的UI元素。

**交互式动画**

交互式动画是指随着用户的操作实时变化的动画。例如，拖动滑块时的动态变化或者通过手势控制的动画。

**实现交互式动画的方法**

你可以通过以下方式为你的应用添加交互式动画：

1. **使用Gesture**

   绑定手势到视图，然后在手势变化时更新视图的状态。

   ```swift
   struct InteractiveView: View {
       @GestureState private var dragState = CGSize.zero

       var body: some View {
           Rectangle()
               .fill(Color.blue)
               .frame(width: 100, height: 100)
               .offset(dragState)
               .gesture(
                   DragGesture()
                       .updating($dragState) { value, state, _ in
                           state = value.translation
                       }
               )
       }
   }
   ```

   在这个例子中，`DragGesture`跟踪手指的拖动，并实时更新`Rectangle`的偏移。

2. **与动画状态链接**

   将动画和状态变量结合起来，实现随状态改变而动画的效果。

   ```swift
   struct ResponsiveView: View {
       @State private var position = CGPoint.zero

       var body: some View {
           Circle()
               .position(position)
               .onTapGesture {
                   withAnimation {
                       position = CGPoint(x: position.x + 100, y: position.y + 100)
                   }
               }
       }
   }
   ```

   这里，每次点击都会使圆形移动，并伴随平滑的过渡动画。

**响应式动画**

响应式动画则是指根据外部事件或数据的变化而自动触发的动画。例如，当接收到新消息时，提示符以动画的形式出现。

**实现响应式动画的方法**

1. **数据绑定**

   通过观察对象中的`@Published`属性变化来驱动动画。

   ```swift
   class AnimationViewModel: ObservableObject {
       @Published var isLoading = false
   }

   struct LoadingView: View {
       @ObservedObject var viewModel = AnimationViewModel()

       var body: some View {
           Circle()
               .frame(width: 50, height: 50)
               .rotationEffect(Angle(degrees: viewModel.isLoading ? 360 : 0))
               .animation(Animation.linear(duration: 1).repeatForever(autoreverses: false), value: viewModel.isLoading)
               .onAppear {
                   viewModel.isLoading = true
               }
       }
   }
   ```

   当`isLoading`变为`true`时，圆形会开始无限旋转，模拟加载指示器的效果。

2. **环境变量**

   利用`@EnvironmentObject`或其他环境属性，在多个视图之间共享动画状态。

   ```swift
   struct ContentView: View {
       @EnvironmentObject var userSettings: UserSettings

       var body: some View {
           Text(userSettings.username)
               .scaleEffect(userSettings.isLoggedOut ? 0 : 1)
               .animation(.spring(), value: userSettings.isLoggedOut)
       }
   }
   ```

   当用户注销时，用户名的文本会通过缩放动画消失。

**小结**

交互式和响应式动画在提高应用的用户体验方面起着至关重要的作用。在SwiftUI中，你可以轻松地将动画与用户的交互以及应用的数据状态相结合。


**第4小节：转场动画**

转场动画是用户界面的一部分，它们在视图的呈现和消失时创造连贯的视觉效果。在SwiftUI中，转场是以声明的方式定义的，允许开发者指定添加或删除视图时的动画类型。利用转场，开发者可以提供一种流畅的视觉体验，减少用户界面变化对用户认知的干扰。

**基础转场**

SwiftUI提供了几种内置的转场类型，例如`.opacity`、`.slide`和`.scale`。

```swift
struct ContentView: View {
    @State private var isPresented = false

    var body: some View {
        VStack {
            if isPresented {
                Text("Hello, World!")
                    .transition(.slide)
            }
            Button("Toggle View") {
                withAnimation {
                    isPresented.toggle()
                }
            }
        }
    }
}
```

在这个示例中，当`isPresented`状态变化时，`Text`视图会滑入或滑出。

**组合转场**

你还可以组合多个转场来创建独特的效果：

```swift
.transition(.asymmetric(insertion: .scale, removal: .opacity))
```

在这里，视图出现时使用缩放效果，消失时使用渐隐效果。

**自定义转场**

SwiftUI还允许创建自定义转场。这通常是通过定义视图的两种状态并描述它们之间的动画过渡来实现的。

```swift
extension AnyTransition {
    static var pivot: AnyTransition {
        .modifier(
            active: CornerRotateModifier(amount: -90),
            identity: CornerRotateModifier(amount: 0)
        )
    }
}

struct CornerRotateModifier: ViewModifier {
    let amount: Double
    func body(content: Content) -> some View {
        content.rotationEffect(Angle(degrees: amount), anchor: .topLeading)
    }
}
```

在这个自定义转场中，`pivot`通过旋转视图的顶部锚点实现转动效果。

**使用转场动画**

转场可以与`withAnimation`闭包结合使用，来实现视图状态变化时的平滑过渡。

```swift
Button("Toggle View") {
    withAnimation(.spring()) {
        isPresented.toggle()
    }
}
```

这将创建一个在点击按钮时视图呈现和消失时有弹簧动效的交互。

**小结**

转场动画在SwiftUI中为开发者提供了一个高效的方法来处理视图的呈现和消失，使用户界面更加生动和有趣。内置的转场类型适用于多数情况，而自定义转场则为特殊需求提供了无限的可能性。通过恰当使用转场，可以大大提升应用的质感和用户满意度。