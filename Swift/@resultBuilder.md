# @resultBuilder

- Swift 5.4에 도입된 Result builder를 사용하면 나란히 정렬 된 'build block'을 사용하여 결과를 빌드 할 수 있다
- Result builder는 최종 결과로 결합되는 부분들을 모으기 위해 사용 됨

## 예제

- 모든 result builder는 일종의 데이터를 가져와 변환하는 `buildBlock()` 이라는 정적 메서드를 하나 이상 제공 해야함
- 아래 예제의 최종 결과는 `SimpleStringBuilder` 구조체가 result builder가 되어, 문자열 결합이 필요한 곳에  `@SimpleStringBuilder` 를 어디에서나 사용할 수 있음

```swift
@resultBuilder
struct SimpleStringBuilder {
    static func buildBlock(_ parts: String...) -> String {
        parts.joined(separator: "\n")
    }
}

//각 문자열 끝에 쉼표가 필요하지 않음
@SimpleStringBuilder 
func makeSentence3() -> String {
    "Why settle for a Duke"
    "when you can have"
    "a Prince?"
}

print(makeSentence3())
```

물론 `SimpleStringBuilder.buildBlock()`과 같이 직접 사용할 수 도 있음

```swift
let joined = SimpleStringBuilder.buildBlock(
    "Why settle for a Duke",
    "when you can have",
    "a Prince?"
)

print(joined)
```

추가로 

```swift
static func buildEither(first component: String)
```

```swift
static func buildEither(second component: String) -> String
```

```swift
static func buildArray(_ components: [String]) -> String
```

등의 함수를 추가하여 더 많은 기능으로 확장 가능

# 출처

- [Result builders in Swift explained with code examples](https://www.avanderlee.com/swift/result-builders/)

- [What’s new in Swift 5.4?](https://www.hackingwithswift.com/articles/228/whats-new-in-swift-5-4)

