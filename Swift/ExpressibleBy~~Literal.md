# ExpressibleBy~~Literal

- 우리는 지금까지 `10`은 `Int`, `"Hi"`는 `String`이라고 당연하게 인지하고 있었음.
- 하지만 **`Int`와 `String`** 모두 **생성자를 가지는 구조체**이기 때문에, `10`은 **원래** **`Int(10)`** 으로 선언되어야 하고, `"Hi"`는 **`String("Hi")`** 로 선언되어야 함. 
- 이렇게, **생성자를 사용하지 않고도 생성할 수 있게 만드는 것**을 **리터럴 (Literal)** 이라고 함. 직역하면 '문자 그대로'라는 뜻

```swift
let number = 10
let string = "Hi"
let array = ["a", "b", "c"]
let dictionary = [
  "key1": "value1",
  "key2": "value2",
]
```

- 이 리터럴을 가능하게 해주는 프로토콜이 있음 ->  **`ExpressibleBy~~Literal`**

  따라서 `Int`는 `ExpressibleByIntegerLiteral`을, `String`은 `ExpressibleByStringLiteral`을, `Array`는 `ExpressibleByArrayLiteral`을, `Dictionary`는 `ExpressibleByDictionaryLiteral` 프로토콜을 따르고 있음. 각 **프로토콜은 리터럴 값을 받는 생성자를 정의**.

- 예를 들어 String에서 URL 을 변환하기 위해서는 아래와 같은 URL 생성자를 사용해야함

```swift
func getURL() -> URL
    return URL(string: "https://swiftrocks.com")!
}
```

- 그러나 이 이니셜라이저를 매번 사용해야 하는 것을 방지하기 위해 `ExpressibleByStringLiteral` 을 사용하여 URL 문자열에서 직접 URL을 나타낼 수 있음

```swift
extension URL: ExpressibleByStringLiteral {
    public init(extendedGraphemeClusterLiteral value: String) {
        self = URL(string: value)!
    }

    public init(stringLiteral value: String) {
        self = URL(string: value)!
    }
}
```

- 이를 통해 문자열 토큰만 사용하여 URL을 생성 하도록 리팩토링 가능

```swift
func getURL() -> URL
    return "https://swiftrocks.com"
}
```

- 표준 라이브러리에는 아래와 같은 `ExpressibleBy` 프로토콜이 포함되어 있음
  - **Collection Literals**
    -  `ExpressibleByArrayLiteral`: `[1,2,3]`  같은 배열으로 표현 가능
    -  `ExpressibleByDictionaryLiteral`:  `["name": "SwiftRocks"]` 같은 dictionary 로 표현 가능
  - **Value Literals**
    -  `ExpressibleByNilLiteral`: `nil` 로 표현 가능
    -  `ExpressibleByIntegerLiteral`:  `10` 같은 숫자로 표현 가능
    -  `ExpressibleByFloatLiteral`:  `2.5` 같은 부동 소수점으로 표현 가능
    -  `ExpressibleByBooleanLiteral`:  `true/false` 같은 boolean 으로 표현 가능
  - **String Literals**
    - `ExpressibleByStringLiteral`:  `"Hello world"` 와 같은 문자열으로 표현 가능
    -  `ExpressibleByUnicodeScalarLiteral`: 단일 유니코드 스칼라 값으로 표현 가능. 이것의 사용 예는 `Character`및 `String`
    -  `ExpressibleByExtendedGraphemeClusterLiteral`: UnicodeScalar와 유사하지만 단일 스칼라 대신 스칼라 체인 (a grapheme cluster)으로 구성. 즉, 사용자에게는  단일 문자로 인식되는  하나 이상의 Unicode scalar 값의 그룹. "é", "김"  "🇮🇳" 와 같은 많은 개별 문자는 여러 유니코드 스칼라 값으로 구성될 수 있음. 이러한 코드 포인트는 유니코드의  boundary algorithms 에 의해 ExtendedGraphemeCluster로 결합됨
    - `ExpressibleByStringInterpolation` : `"One cookie: $\(price), \(number) cookies: $\(price * number)."` 와 같이 string interpolation 으로 표현 가능
      
      

# 출처

- [프로토콜(Protocol)](https://devxoul.gitbooks.io/ios-with-swift-in-40-hours/content/Chapter-3/protocols.html)
- [Swift ExpressibleBy protocols: What they are and how they work internally in the compiler](https://swiftrocks.com/swift-expressibleby-protocols-how-they-work-internally-in-the-compiler)
- [Initialization with Literals](https://developer.apple.com/documentation/swift/swift_standard_library/initialization_with_literals)
- [ExpressibleByExtendedGraphemeClusterLiteral](https://developer.apple.com/documentation/swift/expressiblebyextendedgraphemeclusterliteral)

