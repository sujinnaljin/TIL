# [SwiftUI] ShapeStyle
ShapeStyle은 도형을 렌더링할 때 사용할 색상 또는 패턴으로 아래와 같은 Type 들이 ShapeStyle은 를 conform 함

그래서 해당 타입들을 다양하게 쓸 수 있음

```swift
.foregroundStyle(.red) //Color
.foregroundStyle(.thinMaterial) // Material
.foregroundStyle(.primary) // HierarchicalShapeStyle
.foregroundStyle(.selection) // SelectionShapleStyle
.foregroundStyle(.tint) // TintShpaeStyle
.foregroundStyle(.background) // BackgroundStyle
.foregroundStyle(.foreground) // ForegroundStyle
.foregroundStyle(.linearGradient(~~~~)) // LinearGradient
```
![스크린샷 2022-06-08 오전 3 04 36](https://user-images.githubusercontent.com/20410193/172451926-1abe0598-092a-41d3-89ef-7af6216a62be.png)


# 출처 
- https://developer.apple.com/documentation/swiftui/shapestyle
