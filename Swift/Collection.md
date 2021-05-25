# Collection

- Collection은 Sequence를 상속받아 구현되었으며, Collection은 Sequence가 지닌 두 가지 문제점을 해결

  - 모든 Collection은 유한(finite) 함. 즉, 몇 개의 요소로 구성되어 있는지 항상 알 수 있음.
  - 원하는 만큼 이터레이트 가능 (Sequence에서는 오직 한 번)

  ```swift
  protocol Collection: Sequence {
    associatedtype Index: Comparable
    var startIndex: Index
    var endIndex: Index
    subscript(position: Index) -> Element { get }
    func index(after i: Index) -> Index
  }
  ```

- BidirectionalCollection

  Collection을 상속받았지만 정방향뿐만 아니라 역방향으로도 탐색 가능

- RandomAccessCollection

  빠르게 요소에 접근할 수 있게 도와 줌. 순서대로 하나씩 요소에 접근하는 대신, 해당 요소로 바로 접근 가능

- RangeReplaceableCollection

  Collection 중간의 요소를 다른 것으로 교체할 수 있는 기능 제공

- MutableCollection

  요소들의 값을 설정할 수 있는 기능을 제공

# 출처

- [Swift의 Sequence와 Collection에 대해 알아야 하는것들](https://academy.realm.io/kr/posts/try-swift-soroush-khanlou-sequence-collection/)

