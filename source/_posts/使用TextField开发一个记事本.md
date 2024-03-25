---
title: 使用TextField开发一个记事本
date: 2023-11-07 20:30:16
tags:
---
要使用 SwiftUI 创建一个简单的记事本应用，你可以按照以下步骤操作：

1. 创建一个用于存储用户输入文本的状态变量。
2. 使用 `TextField` 为用户提供文本输入的地方。
3. 将输入的文本存储起来，并显示在界面上。
4. 提供一个方法来保存和加载这些文本。

下面是一个简单记事本应用的基本代码框架：

```swift
import SwiftUI

struct ContentView: View {
    @State private var noteText: String = ""
    @State private var savedNotes: [String] = []

    var body: some View {
        NavigationView {
            VStack {
                TextField("Write your note here...", text: $noteText)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                    .padding()

                Button("Save Note") {
                    saveNote()
                    noteText = "" // Clear the TextField after saving
                }
                .padding()
                .disabled(noteText.isEmpty) // Disable the button if there is no text

                List(savedNotes, id: \.self) { note in
                    Text(note)
                }
            }
            .navigationTitle("My Notes")
        }
    }

    func saveNote() {
        savedNotes.append(noteText) // Adds the current note text to the saved notes array
        // Here, you might also write the notes to persistent storage
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

上述代码创建了一个简单的记事本界面。用户可以在 `TextField` 中输入文本，点击 "Save Note" 按钮后，文本会被添加到 `savedNotes` 数组中，并且显示在下面的列表中。

### 进阶：

- 你可能希望将这些笔记保存到持久存储中，例如使用 `UserDefaults` 或文件系统，这样即使应用关闭，数据也不会丢失。
- 可以考虑使用 `ForEach` 而不是直接在 `List` 中使用数组，以便于处理删除操作等。
- 对 `List` 使用 `.onDelete(perform:)` 方法可以让用户删除条目。

这个代码示例仅是一个起点，根据你的需求，你可以添加更多的功能，比如编辑已保存的笔记、按日期排序笔记、将笔记分组等。记事本应用可以根据个人需求有很多复杂的变体，上面的示例为你提供了一个构建更复杂应用的基础。