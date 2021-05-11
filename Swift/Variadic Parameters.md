# Variadic Parameters

- 특정 타입의 0개 이상의 value 를 받는다.
- 매개 변수 type 뒤에 마침표 (...) 세 개를 삽입하여 variadic parameter를 작성
- 받은 변수는 함수 내에서 array 로 사용할 수 있다.
- 함수는 여러 가변 매개 변수를 가질 수 있다. 이때 첫 번째 parameter에는 argument label이 있어야 한다

```swift
func arithmeticMean(_ numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8.25, 18.75)
// returns 10.0, which is the arithmetic mean of these three numbers
```

# 출처

- [Functions](https://docs.swift.org/swift-book/LanguageGuide/Functions.html)

