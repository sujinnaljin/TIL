
# OpaqueType & some
- Opaque 타입은 종종 **reverse generic 타입**이라고 불림
- 불분명한(opaque) 반환 타입으로된 함수는 **반환 값의 타입 정보를 숨김**.
- 함수의 반환 타입에 구체적인 타입을 제공하는 대신에, **해당 반환 값은 지원하는 프로토콜로 설명**
- 반환 값의 근본적인 타입은 비공개로 유지되기 때문에, 타입 정보를 숨기는 것은 모듈과 모듈을 호출하는 코드 사이의 경계에서 유용
- 프로토콜 타입인 값을 반환하는 것과는 다르게, Opaque 타입은 타입의 **독자성(identity)**을 지켜줌. 즉 컴파일러는 타입 정보를 사용할 수 있지만, 모듈의 클라이언트는 사용할 수 없음
- 위의 정보만 봐서는 이해가 잘 안갈 수 있기 때문에 reverse generic 타입이라고 불리는 만큼 Generic 타입을 먼저 알아보자

## Generic 

```swift
func max<T: Comparable>(_ x: T, _ y: T) -> T {
    return (x > y) ? x : y
}
```

- Generic 타입은 위처럼 여러 타입을 쓰고자 할 때 사용
- Generic 타입은 함수를 구현하는 내부에는 값을 숨김.
- 반면 함수 외부에서, 즉 우리가 이 함수를 쓸때는 언제나 어떤 타입이 들어가는지 알 수 있음 (반대로 값이 오픈되어 있다.)

## Opaque Type

- reverse generic 타입
-  Generic타입과 반대로 **함수 내부에서는 반환값을 정확히 알 수 있지만, 밖에서는 반환값의 유형을 정확히 알지 못하게 숨기는 것**

```swift 
func randomReturn() -> some Collection {
    return [1, 2, 3]
}
```

- 함수 내부를 구현할 때에는 리턴 값에 대해서 알 수 있음
- 반면 외부에서는 어떤 값이 리턴될지 알 수 없음
- Generic타입으로 구현한 함수와 정확히 반대기 때문에  reverse generic 타입이라고 불림
- 여기서 `some` 키워드를 통해 반환 값이 Opaque Type 이라는 걸 나타냄

## Some

```swift 
protocol Shape {
    func describe() -> String
}

struct Square: Shape {
    func describe() -> String {
        return "I'm a square. My four sides have the same lengths."
    }
}

struct Circle: Shape {
    func describe() -> String {
        return "I'm a circle. I look like a perfectly round apple pie."
    }
}

func makeShape() -> some Shape {
  return Circle()
}
```

- 위의 `makeShape()` 함수의 반환 타입에 `some` 키워드를 사용하지 않아도 작동함. (즉 Opaque Type이 아니라 protocol을 반환하는 것)

- 하지만 Opaque Type과 프로토콜 타입은 중요한면에서 다름.

- **Opaque Type은 type identitiy를 유지하고 프로토콜은 그렇지 않음** 

  - **Opaque Type**은 항상 **특정 타입**을 나타냄. 그리고 **함수 호출자는 어떤 타입인지 알 수 없**음.
  - **프로토콜 타입**은 **프로토콜을 준수하는 모든 타입 참조** 가능 
  - 일반적으로 말해서, 프로토콜 타입은 저장하는 값의 기본 타입에 대해 더 많은 유연성을 제공하고, Opaque Type은 기본 타입에 대해 더 강력하게 보장할 수 있음

- `some` 키워드를 통해 **Opaque Type을 반환**할때는 여러개의 타입 반환 안됨. 모든 **반환 값은 동일한 타입**이어야 함. 

```swift 
//1. OK
func flip<T: Shape>(_ shape: T) -> some Shape { 
  return FlippedShape(shape: shape) 
}

//2. Error
func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
   if shape is Square { 
    return shape // Error: return types don't match 
   } 
  return FlippedShape(shape: shape) // Error: return types don't match 
}
```

-  **프로토콜을 반환**하는 경우는 **항상 동일한 타입을 요구하지 않음**. Shape **프로토콜을 준수**하면 되기 때문에  flip(_:) 보다 훨씬 느슨한 API. **여러개의 타입의 값을 반환하는 유연함**을 가짐.

  ```swift 
func protoFlip<T: Shape>(_ shape: T) -> Shape { 
  if shape is Square { 
    return shape 
  } 
  return FlippedShape(shape: shape)
}
  ```

또 다른 상황을 보자

```swift 
// 1. 에러
func someList() -> Collection {
  return [1,2,3]
}

// 2. OK
func someList() -> some Collection {
  return [1,2,3]
}
```

- 1번: **Protocol Type 리턴**. 에러 발생.

  `"Protocol 'Collection' can only be used as a generic constraint because it has Self or associated type requirements"` 라며 **리턴하는 값도 관련된 타입을 명시하지 않으면 타입 자체를 알 수 없다**는 의미의 에러

- 2번: **Opaque Type 리턴**

  - `some` 을 사용하면 **특정 프로토콜을 만족하는 어떠한 인스턴스가 리턴될 것이다**라는 것을 컴파일러에게 알려주는 것.
  - `some` 은 일부가 가려진 채로 사용되는 타입이라는 의미를 가지고 있음

# 출처
- [SwiftUI에서 some이 뭘까?](https://usinuniverse.bitbucket.io/blog/some.html)
- [Swift - some 키워드란 무엇일까?](https://minosaekki.tistory.com/38)

- [불분명한 타입(Opaque Types)](https://kka7.tistory.com/353)
