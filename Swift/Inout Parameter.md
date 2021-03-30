# Inout Parameter

- **파라미터로 변수의 주소값**을 넘겨 **직접 파라미터 값에 접근**할 수 있도록 해주는 기능
- Swift에서는 기본적으로 함수의 모든 **인자가 상수로 호출**되어 넘어온 값을 **바꿀 수 없**지만(immutable),  **`inout`** 을 통해 **변경** 가능 (mutable).

```swift
func swap(_ a: inout Int, _ b: inout Int) {
    let temp = a
    a = b
    b = temp
}

var x = 15
var y = 47

swap(&x, &y)

print(x) // 47
print(y) // 15
```

# 출처

- [Part 2 — iOS Interview Preparation Questions and Explanations](https://mdcode2021.medium.com/part-2-ios-interview-preparation-questions-and-explanations-2217792f3cea)
- [Swift의 함수 사용](https://hcn1519.github.io/articles/2017-05/swift_function)

