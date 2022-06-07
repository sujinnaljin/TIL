# [SwiftUI] ViewModifier

모든 뷰에 적용할 수 있는 재사용 가능한 modifier를 생성하려면 ViewModifier 프로토콜을 사용.

해당 프로토콜을 채택하면

```
@ViewBuilder func body(content: Self.Content) -> Self.Body
```

를 무조건 구현해야함

```swift
struct BorderedCaption: ViewModifier {
    func body(content: Content) -> some View {
        content
            .font(.caption2)
            .padding(10)
            .overlay(
                RoundedRectangle(cornerRadius: 15)
                    .stroke(lineWidth: 1)
            )
            .foregroundColor(Color.blue)
    }
}
```

이렇게 작성한 ViewModifier 를 뷰에 직접  [`modifier(_:)`](https://developer.apple.com/documentation/swiftui/view/modifier(_:)) 를 통해 적용할 수 있지만, 보다 일반적이고, 관용적 접근 방식은 view 의 extension 에 [`modifier(_:)`](https://developer.apple.com/documentation/swiftui/view/modifier(_:)) 를 정의해두는것

```swift
extension View {    
  func borderedCaption() -> some View {   
    modifier(BorderedCaption())  
  }
}


Text("Downtown Bus") 
  .borderedCaption()
```

# 출처 

- https://developer.apple.com/documentation/swiftui/viewmodifier
