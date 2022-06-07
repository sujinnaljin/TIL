# [SwiftUI] .fill(HierarchicalShapeStyle)

`.foregroundStyle(.secondary)` /  `.fill(.tertiary)` 이런 식으로 하면 지정한 컬러의 .secondary, .tertiary 스타일이 적용됨

```swift
Capsule()
    .fill(.tertiary)
    .foregroundStyle(.white)

Text("\(Int(cast.farenheit - 8))")
    .font(.title3.bold())
    .foregroundStyle(.secondary)
    .foregroundStyle(.white)
```
