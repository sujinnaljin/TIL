# Access Control
- 아래와 같이 getter와 setter의 접근 제어자를 따로 지정할 수 있다
```swift
private(set) var numberOfEdits = 0 // internal getter & private setter
public private(set) var numberOfEdits = 0 //public getter, private setter
```
- 오직 setter만이 getter보다 더 낮은(더 제한적인) 접근 수준을 가질 수 있다.
```swift
private internal(set) var numberOfEdits = 0 //Error!
```
