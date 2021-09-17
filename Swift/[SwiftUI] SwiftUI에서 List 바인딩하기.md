# [SwiftUI] SwiftUI에서 List 바인딩하기

- 아래 코드는 하나의 요소만 변경하더라도 전체 list 를 다시 렌더링하게 함. 따라서 UI 업데이트가 느려지고 깜빡일 수 있음

```swift
struct TodoList: View {
  @Binding var todos: [TodoItem]
  
  var body: some View {
    List(0..<todos.count) { index in
      TextField("Todo", text: $todos[index].title)
    }
  }
}
```

- WWDC 2021에서 언급되었듯, iOS 15 부터 SwiftUI는 list 요소에 대한 바인딩을 지원함
-  collection에 대한 바인딩을 list로 전달하면, SwiftUI는 현재 요소에 대한 바인딩을 클로저에 전달

```swift
struct BetterTodoList: View {
  @Binding var todos: [TodoItem]
  
  var body: some View {
    List($todos) { $todo in
      TextField("Todo", text: $todo.title)
    }
  }
}
```

# 출처

- [Introducing SwiftUI List Bindings](https://betterprogramming.pub/introducing-swiftui-list-bindings-a150410b836b)

