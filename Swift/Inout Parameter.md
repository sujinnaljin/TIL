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

## inout의 원리

1. 함수가 호출되면, **매개변수로 넘겨진 변수가 복사**
2. 함수 몸체에서, **복사한 값을 수정**
3. 함수가 반환될 때, **변화된 값을 원본 변수에 재할당**

이 동작을 **copy-in copy-out** 혹은 **call by value result** 라고 부르며 **inout**은 실제로 **copy-in copy-out의 줄임말** 

## in-out 최적화

- SWIFT는 in-out 파라미터에 대한 **최적화**를 위해 **값을 복사하는 것이 아니라** 값이 저장된 **메모리 주소값을 함수 내,외부에서 사용**
- 이 최적화를 **call by reference**라고 함
- 이를 통해 복사로 인한 오버헤드를 줄일 수 있음
- 따라서 `inout`을 사용할 때 개발자가 따로 최적화를 고려할 필요는 없다.

## 메모리 접근

> 여기부터는 잘 이해가 안가긴 함 🤔

- `inout`을 사용할 때는 메모리 접근에 주의
- 아래와 같은 상황에서 `stepSize`와 `number`는 같은 메모리 주소를 참조하기 때문에 읽기, 쓰기가 동시에 이루어져 충돌이 발생할 수 있기 때문

```swift
var stepSize = 1

func increment(_ number: inout Int) {
    number += stepSize
}

increment(&stepSize)
// Error: conflicting accesses to stepSize
```

- inout 파라미터를 캡쳐할 수 있는 클로저, 중첩 함수는 반드시 nonescaping이여야 함. **inout 파라미터를 캡쳐**하길 원한다면, 반드시 **캡쳐리스트**를 사용해서 해당 파라미터를 **불변값**으로 명시.

```swift
func someFunction(a: inout Int) -> () -> Int {
    return { [a] in return a + 1 }
}
```

- 만약 값을 **캡쳐하고, 변경**시키길 원한다면 **local copy**를 사용하여 **copy-in copy-out을 직접 구현**해야 함. 멀티 스레드 코드에서 모든 변경이 함수 반환 전에 끝나야함을 보장하는 경우를 예로 들 수 있음

```swift
func multithreadedFunction(queue: DispatchQueue, x: inout Int) {
    // Make a local copy and manually copy it back.
    var localX = x
    defer { x = localX }

    // Operate on localX asynchronously, then wait before returning.
    queue.async { someMutatingOperation(&localX) }
    queue.sync {}
}
```



# 출처

- [Part 2 — iOS Interview Preparation Questions and Explanations](https://mdcode2021.medium.com/part-2-ios-interview-preparation-questions-and-explanations-2217792f3cea)
- [Swift의 함수 사용](https://hcn1519.github.io/articles/2017-05/swift_function)
- [Swift - Inout 파라미터](https://hyunsikwon.github.io/swift/Swift-Inout-01/)

