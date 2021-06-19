# ThreadSafe SingleTon

- Swift의  **전역 변수, 정적(static) 프로퍼티**는 **단 한번의 초기화 지연**(lazily initialized - **사용 시점에 초기화** 되는 것)이 보장 되고, **GCD의 dispatch_once**도 자동 적용 된다. 즉  **atomic한 초기화**를 보장한다.
`
(The lazy initializer for a global variable (also for static members of structs and enums) is run the first time that global is accessed, and is launched as dispatch_once to make sure that the initialization is atomic. This enables a cool way to use dispatch_once in your code: just declare a global variable with an initializer and mark it private.)`
- **상수(let)** 는 한번 초기화되고나면 값을 바꿀 수 없다. 즉 **ImMutable** 하다. 
- 이 두 프로퍼티(static, let)를 이용해서 아래와 같이 ThreadSafe한 싱글톤을 만들 수 있다.

```swift 
class Singleton {
    static let sharedInstance = Singleton()
}
```

# 출처
- [Files and Initialization](https://developer.apple.com/swift/blog/?id=7)
