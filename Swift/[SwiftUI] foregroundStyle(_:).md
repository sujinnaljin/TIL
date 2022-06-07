# [SwiftUI] foregroundStyle(_:)
지정된 스타일을 사용하도록 뷰의 foreground element를 설정

인자로는 ShapeStyle 을 준수하는 아무 style 이나 넣으면 됨. 
```swift
HStack {
    Image(systemName: "triangle.fill")
    Text("Hello, world!")
    RoundedRectangle(cornerRadius: 5)
        .frame(width: 40, height: 20)
}
.foregroundStyle(.teal)
```
![image](https://user-images.githubusercontent.com/20410193/172451602-f255fcd8-16de-4608-a8f0-76d50382cde3.png)


# 출처 
- https://developer.apple.com/documentation/swiftui/view/foregroundstyle(_:)
