# observe(_:options:changeHandler)

- `NSKeyValueObservation` 반환. 일종의 관찰자.
- 이렇게 반환된 관찰자는 **반드시 저장**하고 있어야 한다. 저장을 하지 않는다면 해당 코드는 호출되지 않는다.
- KeyPath를 `\.property.property` 형식으로 관찰하고자 하는 프로퍼티에 접근 가능
-  `options` 파라미터로 관찰할 변화의 상태들을 넣어줄 수 있음
- `NSKeyValueObservation`  담고 있는 객체가 사라지면 observing 자동 중단

```swift
@IBOutlet weak var myView: UIView!
@IBOutlet weak var myButton: UIButton!

var observation : NSKeyValueObservation?

observation = myButton.observe(\.isHighlighted, options: [.new], changeHandler: { _, change in
    if let isHighlighted = change.newValue {
        myView.backgroundColor = isHighlighted ? .red : .blue
    }
})
```

여러개 있을때는 이런식으로 저장

```swift 
var observations : [NSKeyValueObservation] = []

let observation = myButton.observe(\.isHighlighted, options: [.new], changeHandler: { [weak self] _, change in
    if let isHighlighted = change.newValue {
        self?.myView.backgroundColor = isHighlighted ? .red : .blue
    }
})

observations.append(observation)
```



## 참고

- [[ios\] Key-Value Observing in Swift4](https://baked-corn.tistory.com/126)

