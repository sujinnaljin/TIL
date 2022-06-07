# [SwiftUI] background(_:in:fillStyle:)

```swift
func background<S, T>(_ style: S, in shape: T, fillStyle: FillStyle = FillStyle()) -> some View where S : ShapeStyle, T : Shape
```

뷰의 백그라운드를 style로 채워진 도형으로 설정

뷰 뒤에 단일 shape 를 배치하기 위한 편리한 방법.

```swift
.background(.ultraThinMaterial,
            in: CustomCorner(corners: .allCorners,
                                      radius: 12))
```

만약 다른 View 타입과 함께 백그라운드를 만들려면 [`background(alignment:content:)`](https://developer.apple.com/documentation/swiftui/view/background(alignment:content:)) 를 사용

`ShapeStyle` 을 배경으로 추가하려면,  [background(_:safeAreaEdges:)](https://developer.apple.com/documentation/swiftui/view/background(_:ignoressafeareaedges:)) 을 사용

출처 - https://developer.apple.com/documentation/swiftui/view/background(_:in:fillstyle:)-89n7j
