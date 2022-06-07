# UIRectCorner

UIRectCorner 는 OptionSet 을 conform 함

```swift
public struct UIRectCorner : OptionSet, @unchecked Sendable 
```

`OptionSet`의 인스턴스는 생성할 수 있는 방법은 여러개임 

`OptionSet`의 static 멤버 중 **하나**를 할당할 수도 있고,  static 멤버가 여러개인 **Array Literal** 할당할 수도 있음

따라서 아래와 같이 프로퍼티를 정의해놨다면

```
struct CustomCorner: Shape {
    var corners: UIRectCorner
}
```

다양한 방식으로 값을 넘길 수 있음

```
CustomCorner(corners: .allCorners)
CustomCorner(corners: [.topLeft, .topRight, .bottomLeft, .bottomRight])
```

# 출처 
- https://sujinnaljin.medium.com/swift-uiaccessibilitytraits-%EC%B4%88%EA%B8%B0%ED%99%94%EA%B0%80-%EC%9D%B4%EC%83%81%ED%95%9C%EB%8D%B0%EC%9A%94-feat-optionset%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-50a39c032722
- https://developer.apple.com/documentation/uikit/uirectcorner
