# [SwiftUI] GridLayout (feat. GridItem)

- `LazyVGrid` 는 **columns, item들의 정렬, item 간격, footer, header, conten**t의 제공과 함께 초기화됨
- `LazyHGrid` 도 비슷한데 columns 들을 rows 로 대체하면 됨

## GridItem

grid layout 을 초기화할때, 어떻게 보일지가 가장 중요함 (vertical로 보일건지 horizontal 로 보일건지..)

SwfitUI는 `GridItem` 이라는 컴포넌트가 있는데 이는 row가 될수도, column이 될수도 있음

GridItem을 초기화할 때 3가지의 파라미터가 필요 ([`init(GridItem.Size, spacing: CGFloat?, alignment: Alignment?)`](https://developer.apple.com/documentation/swiftui/griditem/init(_:spacing:alignment:)))

- `size` - item의 사이즈 (3가지 옵션 존재)

  - `adaptive(minimum: CGFloat, maximum: CGFloat)` - **minimum 값 이상의 사이즈**로 **열마다 가능한 많이** 아이템들을 배치하고자 할 때 사용되는 사이즈. (허용된 공간(available space) 안의 **다수의 item**)

    ```swift
    [
      GridItem(.adaptive(minimum: 50))
    ]
    ```

  - `flexible(minimum: CGFloat, maximum: CGFloat)` -  **minimum 값 이상의 사이즈로 column 수를 조절** 하고 싶을 때 사용되는 사이즈. 만약 min이 제공되지 않으면 허용된 공간을 item count 로 나눔으로써 item size 를 계산. adaptive와 유사하나 **열마다 배치되는 아이템 수를 조절**할 수 있다는 점에서 다름. (허용된 공간(available space) 안에서 **flexible** 한 **single item**.)

    ```swift
    [
      GridItem(.flexible()),
      GridItem(.flexible()),
      GridItem(.flexible()),
      GridItem(.flexible())
    ]
    ```

  - `fixed(CGFloat)` - **column 수와 크기를 직접 조절**하고 싶을 때 사용하는 사이즈. item 사이즈를 알고 있을 때 상수로 제공 (허용된 공간 안에서 **fixed** size 인 **single item**)

    ```swift
    [
      GridItem(.fixed(100)),
      GridItem(.fixed(100)),
      GridItem(.fixed(100))
    ]
    ```


    ![img](https://www.swiftcompiled.com/content/images/2020/06/vgrid.png)
- `spacing` - item 간 간격

- `alignment` - 각 grid item을 배치할때의 정렬

 # 출처

- [[SwiftUI] GridView 그리기](https://jaesung0o0.medium.com/swiftui-gridview-%EA%B7%B8%EB%A6%AC%EA%B8%B0-2f399c9d754c)
- [Grids in SwiftUI](https://www.swiftcompiled.com/swiftui-grids/)
- [GridItem](https://developer.apple.com/documentation/swiftui/griditem)
- [How to master grid layout in iOS with SwiftUI](https://levelup.gitconnected.com/how-to-master-grid-layout-in-ios-with-swiftui-8a9de16ec7ca)


