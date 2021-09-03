# protocol의 gettable & settable

- 프로퍼티 요구사항은 항상 var로 선언

- 프로토콜에서는 프로퍼티가 **저장 프로퍼티**인지, **연산 프로퍼티**인지 **명시하지 않음**
- 대신 **읽기만 가능**한지, **읽기/쓰기 모두가 가능**한지 명시 ( `{get}` or `{get set}`)

## 1. property가 `{get}` 으로 선언된다면

```swift
protocol FullyNamed {
    var fullName: String { get }
}
```

해당 프로토콜을 준수할 때는

- **저장 프로퍼티**로 선언하든, **연산프로퍼티**로 선언하든 상관없음

```swift
// 저장 프로퍼티 - 변수로 지정
struct Person: FullyNamed {
    var fullName: String
}

// 연산 프로퍼티
struct Person: FullyNamed {
    var name: String
    var fullName: String {
        return name
    }
}
```

- 저장 프로퍼티 중에서도 **상수** (`let`) 로 지정하든 **변수** (`var`) 로 지정하든 상관 없음

```swift
// 저장 프로퍼티 - 상수로 지정
struct Person: FullyNamed {
    let fullName: String
}

var john = Person(fullName: "John Appleseed")
john.fullName = "naljin"// ❌ 'fullName' is a 'let' constant

// 저장 프로퍼티 - 변수로 지정
struct Person: FullyNamed {
    var fullName: String
}

var john = Person(fullName: "John Appleseed")
john.fullName = "naljin"// ✅
```

- 필요하다면 연산 프로퍼티에 set 설정 할수도 있음

```swift
// 연산 프로퍼티 - only gettable
struct Person: FullyNamed {
    var name: String
    var fullName: String {
        return name
    }
}

var john = Person(name: "John Appleseed")
john.fullName = "naljin"// ❌ 'fullName' is a get-only property

// 연산 프로퍼티 - protocol 요구사항이 {get}으로 선언되었어도 정 필요하면 settable 가능 
struct Person: FullyNamed {
    var name: String
    var fullName: String {
        get { return name }
        set { name = newValue }
    }
}

var john = Person(name: "John Appleseed")
john.fullName = "naljin"// ✅
```

## 2. property `{get set}` 으로 선언된다면

해당 프로토콜을 준수할 때는

- **상수(`let`) 저장 프로퍼티** 또는 **읽기 전용 연산 프로퍼티**로 충족되서는 안됨 -> settable을 요구하는데, settable 연산을 할 수 없게 선언한거니까!

```swift
protocol FullyNamed {
    var fullName: String { get set }
}

// 저장 프로퍼티 - 상수로 지정
struct Person: FullyNamed {
    let fullName: String // ❌ error: type 'Person' does not conform to protocol 'FullyNamed'
}

var john = Person(fullName: "John Appleseed")

struct Person: FullyNamed {
    var name: String
    var fullName: String { // ❌ error! error: type 'Person' does not conform to protocol 'FullyNamed'
        return name 
    }
}

var john = Person(name: "John Appleseed")
```


## 출처

- [Swift ) Protocols (1)](https://zeddios.tistory.com/255)
