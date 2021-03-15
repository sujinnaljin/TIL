# @autoclosure

- @autoclosure는 **함수에 전달 된 argument에서 자동으로 클로저를 생성**
- argument를 closure로 바꾸면 **argument의 실제 요청을 지연**시킬 수 있다

## 예시

디버깅을 비활성화했지만 `Person`에 대한 description이 여전히 요청되는 코드. 

```swift
 struct Person: CustomStringConvertible {
     let name: String
     
     var description: String {
         print("Asking for Person description.")
         return "Person name is \(name)"
     }
 }

 let isDebuggingEnabled: Bool = false
 
 func debugLog(_ message: String) {
     /// You could replace this in projects with #if DEBUG
     if isDebuggingEnabled {
         print("[DEBUG] \(message)")
     }
 }

 let person = Person(name: "Bernie")
 debugLog(person.description)
 
 // Prints:
 // Asking for Person description. 
```

**클로저를 사용**하여 위의 문제 해결 가능. `message()` closure는 debugging이 활성화 될때만 호출되기 때문. 하지만 `debugLog` 함수 호출 시 별로 좋아보이지 않는 closure argument 를 넣어줘야함

```swift
let isDebuggingEnabled: Bool = false

 func debugLog(_ message: () -> String) {
     /// You could replace this in projects with #if DEBUG
     if isDebuggingEnabled {
         print("[DEBUG] \(message())")
     }
 }

 let person = Person(name: "Bernie")
 debugLog({ person.description })

 // Prints:
 // -
```

**@autoclosure 키워드를 이용**해 개선 가능. 

```swift
let isDebuggingEnabled: Bool = false
 
 func debugLog(_ message: @autoclosure () -> String) {
     /// You could replace this in projects with #if DEBUG
     if isDebuggingEnabled {
         print("[DEBUG] \(message())")
     }
 }

 let person = Person(name: "Bernie")
 debugLog(person.description)

 // Prints:
 // - 
```

## @autoclosure를 사용하는 표준 Swift API의 예

`assert(condition:message:file:line:)` 는 @autoclosure를 이용하는 api 중 하나로,  `#if DEBUG` 가 true 일때만 condition이 evaluated 되고, condition이 fail 될때만 message가 호출됨. 

```swift
@_transparent
public func assert(
  _ condition: @autoclosure () -> Bool,
  _ message: @autoclosure () -> String = String(),
  file: StaticString = #file, line: UInt = #line
) {
  // Only assert in debug mode.
  if _isDebugAssertConfiguration() {
    if !_fastPath(condition()) {
      _assertionFailure("Assertion failed", message(), file: file, line: line,
        flags: _fatalErrorFlags())
    }
  }
}
```



# 출처

- [How to use @autoclosure in Swift to improve performance](https://www.avanderlee.com/swift/autoclosure/)

  

