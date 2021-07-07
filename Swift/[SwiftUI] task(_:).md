# [SwiftUI] task(_:)

- 해당 view가 appear될때 수행될 task
- SwiftUI ViewModifiers 의 instance method로 iOS 15.0 부터 사용 가능

```swift
//이 task는 해당 view가 disappear될때 취소됨. 
Text(displayValue)
    .task {
        var results = TextProcessResults()
        for try await line in textURL.lines() {
            results.accumulateResults(line: line)
        }
        displayValue = results.textSummary()
    }
```



# 출처

- [task(_:)](https://developer.apple.com/documentation/swiftui/emptyview/task(_:))

