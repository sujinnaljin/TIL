# KVO (Key Value Observing)

- **객체의 프로퍼티의 변경사항**을 다른 객체에 알리기 위해 사용하는 코코아 프로그래밍 패턴
- 프로퍼티 옵저버와 비슷하지만 KVO는 **타입 정의 외부에서 obsever 추가** 가능 (willSet, didSet 같은 propertyObserver는 타입 정의 내부에 위치)

## 사용 방법

- NSObjec를 상속한 **클래스**에서만 KVO를 사용할 수 있고, observe하려는 프로퍼티에 **@objc attribute와 dynamic modifier 추가** 필요

  ```swift
  class Address: NSObject {
      @objc dynamic var town: String
      
      init(town: String) {
          self.town = town
      }
  }
  ```

- 내가 observe하려는 프로퍼티가 변경되는지 보기 위해 **observer** 필요한데 **KeyPath를 사용**하여 추가 가능

  ```swift
  var address = Address(town: "A")
  
  address.observe(\.town, options: [.old, .new]) { (object, change) in
      // 2. handler내에서 oldValue와 newValue를 가져올 수 있음
      print(change.oldValue, change.newValue) // Optional("A") Optional("B")
  }
  
  address.town = "B" // 1. 프로퍼티에 변경사항이 생기면서 observer의 change handler가 호출
  ```

- 위에서는 observe 의 options 인자에  **`.old`, `.new`** 가 추가되어 있음. 이 외에도 `initial` 과 `prior` 추가 가능

  - **initial** - **초기 값**에 대해서도 이 observer handler를 호출

    ```swift
    address.observe(\.town, options: [.old, .new, .initial]) { (object, change) in
        print(change.oldValue, change.newValue) 
        // nil Optional("A") -> A로 값 초기화 될 때
        // Optional("A") Optional("B") -> A에서 B로 값 변경 될 때
    }
    ```

  - **prior** -이전의 상태와 지금의 상태를 다 주는 옵션

    ```swift
    address.observe(\.town, options: [.old, .new, .prior]) { (object, change) in
        print(change.oldValue, change.newValue)
        // Optional("A") nil
        // Optional("A") Optional("B")
    }
    ```

    > 아래와 같은 이유 때문에 print 가 이렇게 찍힌다는데,,, 잘 이해는 안감 🤔
    >
    > ```swift
    > Optional("A") nil
    > Optional("A") Optional("B")
    > ```
    >
    > - initial이 된 순간 A는 oldValue로 취급되고 newValue는 없는 상태
    >
    > - 프로터티가 변경된 순간 newValue는 B가 됨
    >
    > `address.town = "C"` 처럼 값 한번 더 바꾸면 아래 같이 프린트 됨
    >
    > ```swift
    > Optional("B") nil
    > Optional("B") Optional("C")
    > ```

  - 만약 변경사항이 필요하지 않으면 options을 비우면 됨

    ```swift
    address.observe(\.town) { (object, change) in
        print(change.oldValue, change.newValue) // nil nil
    }
    ```

- 만약 현재 보고 있는 값이 이전값인지, 현재값인지 알고싶다면 `isPrior` 프로퍼티를 통해 확인

  ```swift
  address.observe(\.town) { (object, change) in
      print(change.isPrior)
  } 
  ```

## 장점

- **두 객체간의 동기화**를 달성가능. 위에서 언급했듯이 Model과 View와 같이 논리적으로 분리된 파트간의 변경사항을 전달 ➞ 동기화 가능

- 관찰된 프로퍼티의 **이전값**(oldValue)와 **최신값**(newValue)를 제공

- **KeyPath**를 사용하여 프로퍼티를 관찰하므로 **nested 프로퍼티도 관찰** 가능. `.address.town.~~` 이런 식으로 쭉쭉 관찰도 가능하다는 뜻

- 따로 옵저버를 해제해주지 않아도 됨. **시스템이 알아서 removeObserver**해줌 

- 객체의 구현을 변경하지 않고 **내부 객체의 상태 변화에 대응**할 수 있음.

  예를 들어 **라이브러리에 들어있는 프로퍼티의 변경사항**을 알고싶다! ➞ 내가 내부 구현을 못바꾸니(프로퍼티 옵저버 이런걸 추가 못하니) KVO가 이럴때 좋음

 ## 단점

- NSObject 상속해야됨. 즉 **Objective-C 런타임에 의존**하게 됨. 



# 출처

- [Key-Value Observing(KVO) in Swift](https://zeddios.tistory.com/1220)

