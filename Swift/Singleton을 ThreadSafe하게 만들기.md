# Singleton을 ThreadSafe하게 만들기

- Swift는  **전역 변수, 정적 프로퍼티(static property) 등에 대해 단 한번의 초기화 지연(lazily initialized)을 보장 **한다. 즉  **atomic한 초기화**를 보장한다.
- **상수** 는 한번 초기화되고나면 값을 바꿀 수 없다. 즉 **ImMutable** 하다. 
- 이 두 프로퍼티를 이용해서 아래와 같이 ThreadSafe한 싱글톤을 만들 수 있다.

```swift 
class Singleton {
    static let sharedInstance = Singleton()
}
```

