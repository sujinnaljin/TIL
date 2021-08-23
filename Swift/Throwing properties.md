# Throwing properties

- Throwing properties 는 실패시 **error 를 throw 하는 computed properties** 를 정의할 수 있게 함
- throwing computed properties를 정의함으로써,  **단순 접근자(accessor)에 대한 메서드를 정의하지 않아도** 됨

```swift
// throwing computed properties 사용
struct SampleFile {
    let url: URL
    var data: Data {
        get throws {
            try Data(contentsOf: url)
        }
    }
}

// throwing method 사용
struct SampleFile {
    let url: URL
    
    func data() throws {
        try Data(contentsOf: url)
    }
}
```

- 하지만 **setter와 같이 사용하진 못**함. (아래와 같은 에러 뜸)

  ```
  ‘set’ accessor is not allowed on property with ‘get’ accessor that is ‘async’ or ‘throws’
  ```

- throwing property 와 throwing method 중에 어떤걸 사용할지 결정하는 것은 케이스 바이 케이스. 출처의 필자는 경험상 계산이 더 복잡해지면 method을 선택

# 출처

- [How to use throwing properties to catch failures in Swift](https://www.avanderlee.com/swift/throwing-properties/)

