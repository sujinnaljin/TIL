# StateObject vs ObservedObject

## @StateObject

- **Observable object**를 **인스턴스화** 하는 프로퍼티 wrapper 타입

  > observable object 란? 객체가 변하기 전에 퍼블리싱하는 퍼블리셔를 갖는 오브젝트 타입으로, **@Published** 속성으로 선언된 변수가 값이 변하려고 할 때(`willSet`) 외부에 퍼블리싱

- observable object 내에 `@Published ` 속성으로 선언된 프로퍼티의 값이 변할 때 뷰를 업데이트
- 뷰의 라이프 사이클에 의존하지 않음

## @ObservedObject

- **Observable object**를 **subscribe** 하고 해당 observable object에 **변화가 있을 때마다 뷰를 무효화**
- 상위 뷰에서 하위 뷰로 **인스턴스화 된 observable object를 전달**할 때 사용
  - `StateObject`와 달리  `ObservedObject`는 **이미 생성된 객체**를 바라보는데, 이때 이미 생성된 객체란 `@StateObject` 속성으로 인스턴스화 된 observable object
- 뷰의 라이플 사이클에 의존
- `StateObject` 는 SwiftUI2 부터 등장한 것으로, 그 전까지는  `ObservedObject`가 해당 역할 수행

## 예시

```swift
class TimeCounter: ObservableObject {
    @Published var count: Int = 0    
    func increase() {
        count += 1
    }
}
```

위 모델을 `CounterView` 뷰 (어느 부모 뷰에 속함) 에서 `counter` 라는 객체로 생성.

```swift
struct CountView: View {
  @ObservedObject var counter = TimeCounter()    
  var body: some View {
        VStack { 
            Text(counter.count)
            Button(action: counter.increase) {
                Text("Tap")
            }
        }
    }
}
```

이때 부모 뷰의 `body`가 **State** 속성의 프로퍼티에 의해 업데이트 되면 `CountView` 의 화면은 어떻게 될까?

###  `@ObservedObject` 속성 사용

- 무조건 숫자가 `0`으로 초기화
- 부모뷰의 `body`가 업데이트 되면서 그 안의 `CountView`도 `struct` 의 특징에 따라 다시 생성되게 되고, `CountView`안의 `observed object`도 초기화
- 모델이 **뷰의 라이프 사이클에 의존**

### `@StateObject` 속성 사용

```swift
struct CountView: View {
    @StateObject var counter = TimeCounter()
    ...
}
```

- 초기화 안됨. 즉, `CountView`의 숫자가 `2`였다면 부모뷰가 업데이트 되어도 여전히 `2`
- 부모뷰가 업데이트가 되면 `body`가 업데이트 되면서 `CountView` 를 재생성하지만, 모델을 “ **참조(source of truth)**” 하기 때문에 뷰가 사라져도 `StateObject` 프로퍼티는 **여전히 살아있음**
- 모델이  **뷰의 라이프 사이클에 의존하지 않음**



# 출처

- [[SwiftUI] Data Flow — StateObject vs ObservedObject](https://jaesung0o0.medium.com/swiftui-data-flow-stateobject-vs-observedobject-e32a37d80dd2)
- [SwiftUI: StateObject and ObservedObject](https://medium.com/geekculture/swiftui-stateobject-and-observedobject-c6640c1bd2fd)

