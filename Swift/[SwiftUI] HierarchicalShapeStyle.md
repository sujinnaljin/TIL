# [SwiftUI] HierarchicalShapeStyle

번호가 매겨진 content style 중 하나에 매핑되는 ShapeStyle

아래 네가지 종류가 해당 됨

![image](https://user-images.githubusercontent.com/20410193/172452483-e3092946-fbaf-4071-859a-66b876f10207.png)

```swift

         VStack(alignment: .center, spacing: 5) {
            Text("San Jose")
                .foregroundStyle(.primary)
            Text("98°")
                .foregroundStyle(.secondary)
            Text("Cloudy")
                .foregroundStyle(.tertiary)
            Text("H:103°L:105°")
                .foregroundStyle(.quaternary)
        } 
```

![image](https://user-images.githubusercontent.com/20410193/172452877-b95dafa5-1817-4b7b-8630-a54ccfa8a19c.png)

SwiftUI는 `foregroundStyle(_:)` or the [`foregroundColor(_:)`](https://developer.apple.com/documentation/swiftui/view/foregroundcolor(_:)) modifier를 통해 적용하는 가장 중심 스타일을 찾음. 스타일을 지정하지 않은 경우 SwiftUI는 위와 같이 기본 foreground style 을 사용. (흰색 배경 - 검정색, 검정색 배경 - 흰색)

상단 VStack 바깥에 `.foregroundStyle(.blue)` 만 붙여주면 아래처럼 바뀜

![스크린샷 2022-06-08 오전 3 08 53](https://user-images.githubusercontent.com/20410193/172452642-bec695e8-c6c5-4895-bf33-eb3f7f4a28ac.png)

`foregroundStyle(_:_:_:)` 등을 이용해서  primary, secondary, tertiary 레벨의 foreground style 을 지정해줄 수도 있음

![스크린샷 2022-06-08 오전 3 09 01](https://user-images.githubusercontent.com/20410193/172452664-cc55def7-9967-4927-b2b9-7a0c6947a7a2.png)


 ```swift
         VStack(alignment: .center, spacing: 5) {
            Text("primary")
                .foregroundStyle(.primary)
            Text("secondary")
                .foregroundStyle(.secondary)
            Text("tertiary")
                .foregroundStyle(.tertiary)
            Text("quaternary)")
                .foregroundStyle(.quaternary)
        }
        .foregroundStyle(.red, .yellow, .green)      
```

![image](https://user-images.githubusercontent.com/20410193/172452904-66218e2b-a072-4142-8a7c-8f6bbe978abb.png)

# 출처

- https://developer.apple.com/documentation/swiftui/hierarchicalshapestyle

- https://developer.apple.com/documentation/swiftui/view/foregroundstyle(_:)
