# recursive enumeration (feat. indirect)

- recursive enumeration은 associated value 로 또 다른 enum 인스턴스를 갖는 enum

- case 혹은 enum 앞에 `indirect` 키워드를 붙여서  recursive enum 임을 나타낼 수 있음

```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}

indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

아래와 같이  Linked list 의 node 를 정의할 수 있음

```swift
indirect enum LinkedListItem<T> {
    case endPoint(value: T)
    case linkNode(value: T, next: LinkedListItem)
}

let third = LinkedListItem.endPoint(value: "Third")
let second = LinkedListItem.linkNode(value: "Second", next: third)
let first = LinkedListItem.linkNode(value: "First", next: second)

var currentNode = first

listLoop: while true {
    switch currentNode {
    case .endPoint(let value):
        print(value)
        break listLoop
    case .linkNode(let value, let next):
        print(value)
        currentNode = next
    }
}
```



# 출처

- [Enumerations](https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html)
- [What are indirect enums?](https://www.hackingwithswift.com/example-code/language/what-are-indirect-enums)

