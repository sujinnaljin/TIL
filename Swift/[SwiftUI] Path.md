# [SwiftUI] Path

Path 는 2D shape의 outline (위치 값을 가진 선, 곡선)

`Path`는 그 자체로 `View`. 

`Path` 는 절대 경로 안에서 좌표값에 맞춰 도형을 그림 (`Shape`는 `path(in:)`에서 주어진 `Rect`를 기반으로 상대 경로를 받아서 그림)

`Path`를 만들어서 Shape 안에 넣어줄 수 있음

```swift
struct MySquare: Shape {
    func path(in rect: CGRect) -> Path {
        var path = Path()
        
        path.move(to: CGPoint(x: rect.size.width, y: 0)) // 1. 오른쪽 모서리로 커서 이동
        path.addLine(to: CGPoint(x: rect.size.width, y: rect.size.width)) // 2.
        path.addLine(to: CGPoint(x: 0, y: rect.size.width)) // 3.
        path.addLine(to: CGPoint(x: 0, y: 0)) // 4. 왼쪽 모서리로 커서 이동
        path.closeSubpath() // 5. 자동으로 경로를 닫음
        
        return path
    }
}
```

# 출처 

- https://developer.apple.com/documentation/swiftui/path

- https://seons-dev.tistory.com/entry/SwiftUI-Path
