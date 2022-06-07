# [SwiftUI] ViewBuilder

Closure에서 View를 구성하는 custom parameter attribute

-> "Closure에서 (Child) View를 구성한다"만 알면 됨

```
    @inlinable public init(alignment: VerticalAlignment = .center,
                           spacing: CGFloat? = nil,
                           @ViewBuilder content: () -> Content)
```

HStack 의 생성자는 위과 같이 생겼는데,  @ViewBuilder content 파라미터(Closure타입)가 있는 것을 볼 수 있음

아래와 같이 사용. 이렇게 @ViewBuilder파라미터를 사용하여 해당 Closure가 여러 Child View를 제공할 수 있도록 할 수 있음

```
var body: some View {
    HStack {
        Text("Zedd")
        Text("Zedd")
    }
}
```

만약 Swift 5.4 이상을 사용하고, 딱히 생성자가 필요없다면 그냥 생성자 안만들고 content를 @ViewBuilder로 mark하여 사용해도 됨

```
@ViewBuilder let content: Content
```

참고로 body 프로퍼티는 암시적으로 @ViewBuilder로 선언되어 있지만 body외의 다른 프로퍼티나 메소드는 기본적으로 ViewBuilder로 유추(infer)하지 않기 때문에 @ViewBuilder를 명시적으로 넣어줘야함

# 출처 

- https://zeddios.tistory.com/1324 
