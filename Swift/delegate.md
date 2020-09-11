# delegate
- swift에서 delegate는 메모리 참조 문제때문에 `weak` 로 설정해야하는데, `weak` 로 설정하기 위해선 이 프로토콜은 클래스에서만 사용이 가능하다라는것이 명시가 되어있어야 한다

```swift
protocol MyProtocol: class {} 
weak var delegate: MyProtocol?
```
