# [SwiftUI] LinearGradient

생성자에는 세가지가 있음

![image](https://user-images.githubusercontent.com/20410193/172455464-ee918529-e9b2-41d6-bdbd-f0b68cfdc6cf.png)

1. init(gradient:startPoint:endPoint:)

```swift
.fill(.linearGradient(.init(colors: [.orange, .red]), 
                      startPoint: .leading, 
                      endPoint: .trailing))
```

2. init(colors: [Color], startPoint: UnitPoint, endPoint: UnitPoint)

```swift
.fill(.linearGradient(colors: [.orange, .red], 
                      startPoint: .leading, 
                      endPoint: .trailing))
```

3. init(stops:startPoint:endPoint:)

```swift
.fill(.linearGradient(stops: [.init(color: .orange, location: 0),
                              .init(color: .red, location: 1)],
                      startPoint: .leading,
                      endPoint: .trailing))
```

# 출처

- https://developer.apple.com/documentation/swiftui/lineargradient
