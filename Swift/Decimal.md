# Decimal

- `Decimal` 은 base 10으로 숫자를 표기하기 위한 struct. 즉, Swift에서 **10진법으로 숫자를 표기**하기 위한 것
- 금액이나 환율 표기할 때 (오차 없어야할때) 유용
- 반면에 1/3 * 3 의 계산 값으로 정확히 1이 필요하면 유리수(rational) 타입이 필요
- 어떤 숫자 체계도 완벽하게 정확하지 않고, 항상 약간의 반올림이 존재함. 가장 큰 문제는 **반올림을 이진법으로 할지 십진법으로 할지 여부**임. 이진법으로 반올림하는 것이 더 효율적이지만 십진법으로 반올림하는 것이 특정 소수 중심 문제에 더 유용함.
- `Decimal` 타입은 내장된 Swift 유형이 아니라 Foundation 프레임워크의 일부

```swift
let doubleResult: Double = 0.1 + 0.2
print (doubleResult) //0.30000000000000004

let decimalResult: Decimal = 0.1 + 0.2
print (decimalResult) //0.3
```



# 출처

- [THIS is How to Store Money and Currency using Swift](https://stevenpcurtis.medium.com/this-is-how-to-store-money-and-currency-using-swift-89fbf9697dfa)
- [When should you use Decimal instead of Double?](https://www.jessesquires.com/blog/2022/02/01/decimal-vs-double/)

