---
title: SwiftUI中SecureField的用法
date: 2023-11-07 20:40:52
categories:
- SwiftUI
tags:
---
在 SwiftUI 中，`SecureField` 用于输入敏感信息，如密码，它会自动遮蔽输入的文字。`SecureField` 的 API 使用起来和 `TextField` 非常相似。

### 基础用法

下面是一个简单的 `SecureField` 示例：

```swift
struct ContentView: View {
    @State private var password: String = ""

    var body: some View {
        SecureField("Enter your password", text: $password)
            .textFieldStyle(RoundedBorderTextFieldStyle())
            .padding()
    }
}
```

在这个例子中，用户输入的文本会被遮蔽，不会直接显示在屏幕上。

### 与 `TextField` 结合

通常，在一个表单中，你可能会将 `TextField` 用于用户名输入，而将 `SecureField` 用于密码输入：

```swift
struct ContentView: View {
    @State private var username: String = ""
    @State private var password: String = ""

    var body: some View {
        VStack {
            TextField("Username", text: $username)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .autocapitalization(.none)
                .disableAutocorrection(true)
                .padding()

            SecureField("Password", text: $password)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .padding()

            Button("Login") {
                // Perform login action
            }
            .disabled(username.isEmpty || password.isEmpty)
        }
        .padding()
    }
}
```

在上面的代码中，登录按钮会在用户名或密码字段为空时被禁用。

### 自定义外观

和 `TextField` 一样，你可以使用修饰符来自定义 `SecureField` 的外观：

```swift
SecureField("Password", text: $password)
    .padding()
    .background(Color.gray.opacity(0.2))
    .cornerRadius(5)
    .padding(.horizontal)
```

### 响应提交事件

如果你想要在用户提交密码时做出响应（例如，按下键盘的 return 键），你可以使用 `onCommit` 闭包：

```swift
SecureField("Password", text: $password, onCommit: {
    // Perform an action when the user submits the password
    loginUser()
})
.textFieldStyle(RoundedBorderTextFieldStyle())
.padding()
```

在这个示例中，`loginUser()` 函数会在用户按下 return 键后被调用。

`SecureField` 是构建安全文本输入场景的基础组件，它可以与你的验证逻辑、用户认证服务以及其他 UI 组件结合使用，以创建安全的用户体验。