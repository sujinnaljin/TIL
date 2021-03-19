# Property Wrapper
- 프로퍼티의 getter/setter에 들어갈 코드가 여러 프로퍼티에서 재사용되는 경우, 그것을 Property wrapper로 정의해놓으면 쉽게 그 동작을 끌어다 쓸수 있다. 
- 프로퍼티의 저장부-getter/setter 동작이 정의된 코드-와 정의부-실제로 프로퍼티를 사용하는 코드-를 분리하고 싶을때도 유용

## Property Wrapper를 정의하는 방법
- 구조체, ENUM, 클래스 등을 만들고 @propertyWrapper라고 명시해주기. 
- 실제 저장값은 wrappedValue로 정의
- 추가로 제공하고 싶은 정보가 있다면 projectedValue를 정의

## Property Wrapper를 밖에서 가져다쓰는 방법
- 나의 프로퍼티를 정의하고 @프로퍼티래퍼명으로 attribute를 붙인다. 그러면 이 프로퍼티는 get/set동작이 이루어질때마다 알아서 Property wapper의 getter/setter 코드를 통해 실행됨

## _ 와 $

property wrapper 안에 `foo()` 같이 추가 기능을 넣는 것은 흔한 일

```
@propertyWrapper
struct Wrapper<T> {
    var wrappedValue: T

    func foo() { print("Foo") }
}
```

우리는 variable 이름 앞에 **"_" 를 붙임으로써 wrapper type**에 접근 가능.  `_x`  는  `Wrapper<T>`,의 instance 이기 때문에 `foo()` 호출 가능.

```
struct HasWrapper {
    @Wrapper var x = 0

    func foo() { _x.foo() }
}
```

하지만 `HasWrapper` 와 같이 바깥에서 부르는건 컴파일 에러남. 왜냐면 syntesized wrapper는  `private` access control level을 갖고 있기 때문. 

```
let a = HasWrapper()
a._x.foo() // ❌ '_x' is inaccessible due to 'private' protection level
```

이러한 문제를 *projection* 을 통해 해결 가능.

```
@propertyWrapper
struct Wrapper<T> {
    var wrappedValue: T

    var projectedValue: Wrapper<T> { return self }

    func foo() { print("Foo") }
}
```

**달러 사인은 wrapper의 projection 에 접근**하는 syntactic sugar

```
let a = HasWrapper()
a.$x.foo() // Prints 'Foo'
```

아래와 같이 세가지 방법으로 wrapper에 접근할 수 있음

```
struct HasWrapper {
    @Wrapper var x = 0
    
    func foo() {
        print(x) // `wrappedValue`
        print(_x) // wrapper type itself
        print($x) // `projectedValue`
    }
}
```






# 출처
- [Property Wrapper](https://wlaxhrl.tistory.com/90)
- [The Complete Guide to Property Wrappers in Swift 5](https://www.vadimbulavin.com/swift-5-property-wrappers/)
