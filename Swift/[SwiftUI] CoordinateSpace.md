# [SwiftUI] CoordinateSpace

GeometryReader를 통해 size뿐만 아니라


```swift
func frame(in coordinateSpace: CoordinateSpace) -> CGRect
```
위의 frame 메서드를 통해 CGRect에 접근해, 좌표값도 알 수 있다.

`GeometryProxy.frame(in:)` 처럼 사용할 수 있는데 이 좌표값은 지정한 좌표평면 공간에 따라 달라질 수 있다.

in 안에 들어가는 값의 타입은 `CoordinateSpace` 로 세가지가 있다


<img width="329" alt="image" src="https://user-images.githubusercontent.com/20410193/172456803-b0f1f1a6-f38c-4bb8-98f5-8746e83496c7.png">


1. `.local` - 자신이 속한 컨테이너 뷰 안에서의 좌표를 반환한다.

<img width="345" alt="image" src="https://user-images.githubusercontent.com/20410193/172456917-d1c3fe81-adff-4654-9380-378fbe75fe34.png">


2. `.global` - 전체 스크린에서의 좌표를 반환한다.

<img width="343" alt="image" src="https://user-images.githubusercontent.com/20410193/172456967-6556583e-48b9-4141-87db-6b6c60691ba2.png">

3. `.named(_:)` - 지정한 좌표평면에서의 좌표를 반환한다. `.coordinateSpace(name:)` 을 통해 사용자 정의 좌표평면을 지정할 수 있다.

```swift
struct ContentView: View {
    var body: some View {
        GeometryReader { proxy in
            HStack(spacing: 0.0) {
                    ZStack(alignment: .topLeading) {
                        GeometryReader { innerProxy in
                            Rectangle()
                                .foregroundColor(Color.pink)
                                .onTapGesture {
                                    let local = innerProxy.frame(in: .local)
                                    let global = innerProxy.frame(in: .global)
                                    let named = innerProxy.frame(in: .named("OuterGeometry"))
                                    
                                    print("[local] minX : \(local.origin.x), minY : \(local.origin.y)")
                                    print("[global] minX : \(global.origin.x), minY : \(global.origin.y)")
                                    print("[named] minX : \(named.origin.x), minY : \(named.origin.y)")
                                }
                        }.frame(width: 50, height: 50)
                    }
            }
        }.coordinateSpace(name: "OuterGeometry")
    }
}
```

# 출처

- https://protocorn93.github.io/2020/07/26/GeometryReader-in-SwiftUI/

- https://developer.apple.com/documentation/swiftui/geometryproxy/frame(in:)
