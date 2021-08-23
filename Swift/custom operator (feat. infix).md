

# custom operator (feat. infix)

## operator 종류

- **Infix** - **두 값 사이**에서 사용 됨 (ex: `<value>+<value>`)

- **Prefix** - **value 앞**에서 사용 됨 (ex: `!<value>`)

- **Postfix** - **value 뒤**에서 사용 됨 (ex: `<value>!`)

- **Ternary** - 세 값 사이에서 두개의 심볼이 사용 됨 (ex: `<value1> ?< value2> :<value3>`)

  현재 ternary operator 는 custom 하게 만들 수 없음.

## custom operator 만들기

- prefix operator

  - `√` 심볼을 prefix operator로 선언
  - 하나의 매개변수를 받아들이고 연산(제곱근)을 수행하는 함수 작성

  ```swift
  prefix operator √
  
  prefix func √(lhs: Double) -> Double {
  return sqrt(lhs)
  }
  ```

- infix operator

  - `◉` 심볼을 infix operator로 선언

  - 두개의 매개변수(`lhs`, `rhs`)를 받아들이고 연산을 수행하는 함수 작성

  - 만약 prefix operator 에서 작성한 함수처럼 func 앞에 infix 라는 키워드를 붙이면  `‘infix’ modifier is not required or allowed on func declarations prefix func ` 라는 에러가 뜨기 때문에 func 앞에 infix 는 뺀다

    ```swift
    infix operator ◉
    
    func ◉(lhs: Double, rhs: Double) -> Double {
    return lhs * lhs + rhs * rhs
    }
    ```

  - Infix operator를 만들때에는 **Operator precedence (연산자 우선 순위)를 고려**해야 함. 

    만약 새로운 연산자를 precedence group 지정 없이 만들었다면  `DefaultPrecedence` precedence group이 지정

  - 또한 **Operator associativity** 는 **우선 순위가 같은** 연산자를 **왼쪽에서 그룹화**하거나 **오른쪽에서 그룹화**하는 방법을 정의

    left associativity 가 있는 유형은 `v1 + v2 + v3 == (v1 + v2) + v3` 처럼 구문 분석이 됨

  - 아래와 같이 precedencegroup 을 만들고 사용할 수 있음

    ```swift
    // MultiplicationPrecedence 보다 우선 순위 낮고, AdditionPrecedence 보다 우선 순위 높음
    // left associativity 
    precedencegroup SquareSumOperatorPrecedence {
    lowerThan: MultiplicationPrecedence
    higherThan: AdditionPrecedence
    associativity: left
    assignment: false
    }
    
    infix operator ◉: SquareSumOperatorPrecedence
    ```

  - 물론 미리 associativity 와  precedence level 이 정의된 precedencegroup 도 있음. 참고 - [Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)

  - 그 중  `NilCoalescingPrecedence`  이라는 precedencegroup 은 right associative 를 가지고, 여기에 포함 되는 애는 `??` 가 있음

    이를 이용해서 아래와 같이 `nil` 일때 error 를 throw 하는 infix operator 를 정의할 수 있음 

    ```swift
    // 정의
    infix operator ?!: NilCoalescingPrecedence
    
    // Throws the right hand side error if the left hand side optional is `nil`.
    func ?!<T>(value: T?, error: @autoclosure () -> Error) throws -> T {
        guard let value = value else {
            throw error()
        }
        return value
    }
    
    // 사용
    init(operator token: JWT) throws {
        let decodedToken = try JWTDecode.decode(jwt: token) ?! UploadInputError.invalidJWT
        uploadIdentifier = try decodedToken.claim(name: "upload.id").string ?! UploadInputError.missingIdentifier
        uploadURL = try decodedToken.claim(name: "upload.url").url ?! UploadInputError.missingUploadURL
    }
    ```

# 출처

- [How to create a custom operator (like ~= operator) in swift ?? — 🤓🤓🤓🤓](https://abhimuralidharan.medium.com/how-to-create-a-custom-operator-like-operator-in-swift-55953c0c0bf2)

- [Unwrap or throw: Exploring solutions in Swift](https://www.avanderlee.com/swift/unwrap-or-throw/)

- [Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)

- [Swift Operators](https://nshipster.com/swift-operators/)

- [Swift 사용자 정의연산자(operator overloading)](https://medium.com/@miles3898/swift-%EC%82%AC%EC%9A%A9%EC%9E%90-%EC%A0%95%EC%9D%98%EC%97%B0%EC%82%B0%EC%9E%90-operator-overloading-e8cdf5f7dab0)

  

