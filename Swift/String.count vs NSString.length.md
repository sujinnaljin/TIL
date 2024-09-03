# String.count vs NSString.length

- **String.count** - 문자열의 실제 문자 수를 셈. 이모지 등 여러 유니코드 스칼라 값을 하나의 문자로 인식.
- **NSString.length** - 문자열을 **UTF-16 코드 유닛 단위**로 계산. 이모지 같은 특정 문자는 두 개의 코드 유닛으로 구성될 수 있기 때문에, 전체 길이가 다르게 나타날 수 있음.

```swift
let exampleString = "Hello, 🌍!" // "Hello, " + 지구 이모지 + "!"

// Swift에서 String.count 사용
let swiftCount = exampleString.count
print("Swift String.count: \(swiftCount)") // 출력: 9

// Objective-C에서 NSString.length 사용
let objcString: NSString = NSString(string: exampleString)
let objcLength = objcString.length
print("Objective-C NSString.length: \(objcLength)") // 출력: 10
```

