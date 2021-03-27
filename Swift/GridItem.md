# GridItem

- row나 column 같은 하나의 gird item에 대한 설명

  [`init(GridItem.Size, spacing: CGFloat?, alignment: Alignment?)`](https://developer.apple.com/documentation/swiftui/griditem/init(_:spacing:alignment:))

- GridItem.Size 에는 3가지 타입 존재

  - adaptive: minimum 값 이상의 사이즈로 열마다 가능한 많이 아이템들을 배치하고자 할 때 사용되는 사이즈.
  - flexible: minimum 값 이상의 사이즈로 column 수를 조절 하고 싶을 때 사용되는 사이즈 입니다. adaptive와 유사하나 열마다 배치되는 아이템 수를 조절할 수 있다는 점에서 다름.
  - fixed: column 수와 크기를 직접 조절하고 싶을 때 사용하는 사이즈.

![img](https://www.swiftcompiled.com/content/images/2020/06/vgrid.png)

 # 출처

- [[SwiftUI] GridView 그리기](https://jaesung0o0.medium.com/swiftui-gridview-%EA%B7%B8%EB%A6%AC%EA%B8%B0-2f399c9d754c)
- [Grids in SwiftUI](https://www.swiftcompiled.com/swiftui-grids/)
- [GridItem](
