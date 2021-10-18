



# Method Swizzling

- 메서드 스위즐링이란 **원래의 메소드를 런타임때 원하는 메소드로 바꾸어서 사용**할 수 있는 기법

- Objective-C 뿐만 아니라 **동적 메소드 디스패치**를 지원하는 언어들에서 메소드 스위즐링은 잘 알려진 기법

  > **Dynamic Dispatch**
  >
  > - 컴파일 타임이 아닌 **런타임에 어떤 메소드의 구현을 고를지** 결정
  > - 따라서 정적 디스패치에 비해 오버헤드가 더 든다
  > - 다형성을 위해 대부분의 OOP 언어들은 동적 디스패치를 지원

- 이러한 기능을 제공하는 런타임 라이브러리로 Swift는 **Objective-C 런타임**을 이용.

- Swift에서 메서드 스위즐링이 가능한 이유는 **Objective-C 런타임**이 **호출해야하는 특정 함수의 구현을 런타임에 결정** 하기 때문

- **동적 디스 패치** 중 **Message Dispatch** (**자기 자신이 오버라이드** 하거나 **새로 정의한 메소드**들만 테이블에 유지) 를 통해 실행 됨

## 동작 방식

- Objective-C는 **클래스의 메소드나 프로퍼티를 호출할 때, 해당 객체에 "메세지"를 보내는 방식**(message sending)으로 구현되어 있음
- 예를 들어 A라는 메서드를 실행하기 위해서는 대상 객체에게 “A메서드 실행” 하라는 **실행 메시지를 전송하**고, 그 메시지를 수시한 객체는 해당 **메서드를 찾아 실행**하는 것
- 메시지를 찾는 유일한 기준은 **selector의 이름**

- 각 클래스에는 **selector 이름과 메서드 간의 매핑**을 저장하는 메서드 목록이 있음. **IMP**는 **특정 메서드 구현**을 가리키는 함수 포인터와 비슷.
  ![image](https://user-images.githubusercontent.com/20410193/137716196-6931c2b8-4db1-412a-88bf-9cda33060823.png)
- 메소드 스위즐링을 구현하기 위해서는 아래 단계를 거침

  1. `class_getInstanceMethod(_:_:)` 를 통해 **selector의 구현(implementation)에 해당하는 메서드**를 가져옴. (Selector에 대응하는 메서드가 함수 테이블에 없을 수도 있으니 반환값은 Optional)
  2. `method_exchangeImplementations` 을 통해 두 method 의 **구현(implementation)을 교환**
  3. **AppDelegate** 등에서  메소드를 **스위즐해주는 메소드 최초 한번** 실행

  ```swift
  extension UIViewController {
      class func swizzleMethod() {
          let originalSelector = #selector(viewWillAppear)
          let swizzleSelector = #selector(swizzleViewWillAppear)
  
          guard
              let originMethod = class_getInstanceMethod(UIViewController.self, originalSelector),
              let swizzleMethod = class_getInstanceMethod(UIViewController.self, swizzleSelector)
              else { return }
        
          method_exchangeImplementations(originMethod, swizzleMethod)
      }
  
      @objc public func swizzleViewWillAppear(animated: Bool) {
          print("swizzleViewWillAppear")
      }
  }
  
  class AppDelegate: UIResponder, UIApplicationDelegate {
      func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        UIViewController.swizzleMethod()
      }
  }
  ```

  ![image](https://user-images.githubusercontent.com/20410193/137716155-9fd67856-3d8d-4a86-8f11-fb99daafaa03.png)

  

- **정적 디스패치**의 경우 **컴파일 타임에 이미 어떤 코드가 호출되어야 할지 결정**되어 버리지만, **동적 디스패치**의 경우 이러한 과정이 **런타임에서 발생**하기 때문에 런타임 과정에서 **임의의 메서드로 교체** 가능한 것.

### 재귀적으로 보이는 상황

```swift
extension NSString {
    
    class func swizzleReplacingCharacters() {
        let originalMethod = class_getInstanceMethod(
            NSString.self, #selector(NSString.replacingCharacters(in:with:)))
        
        let swizzledMethod = class_getInstanceMethod(
            NSString.self, #selector(NSString.swizzledReplacingCharacters(in:with:)))
        
        guard let original = originalMethod, let swizzled = swizzledMethod else {
            return
        }
        
        method_exchangeImplementations(original, swizzled)
    }
    
    func swizzledReplacingCharacters(in range: NSRange, with replacement: String) -> String {
        return self.swizzledReplacingCharacters(in: range, with: replacement)
    }
}

class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
      NSString.swizzleReplacingCharacters()
    }
}
```

- 해당 코드는 얼핏 보면 아래와 같은 의식의 흐름으로 재귀처럼 보일 수 있다
  1. 앱 딜리게이트에서 `swizzleReplacingCharacters()` 로 서로 메소드 스위즐링 해놓음
  2. 원래 Origin 함수 (`replacingCharacters`) 가 호출되면 스위즐링 된 메소드 (`swizzledReplacingCharacters`) 가 실행 
  3. 엥 안에서 또 스위즐링된 메소드 (`swizzledReplacingCharacters`) 실행하네?? 이거 재귀 아니냐?!

- 하지만 다시 잘 생각해보자

  1. 앱 딜리게이트에서 swizzleReplacingCharacters()로 서로 메소드 스위즐링 해놓음

     ```
     Origin 함수 겉 껍데기 - Swizzle 내부 구현
     Swizzle 함수 겉 껍데기 - Origin 내부 구현따라서 
     ```

  2. 원래 **Origin** 함수 (`replacingCharacters`) **겉 껍데기가 호출**되면 **스위즐링** 된 메소드의 **구현체** (`self.swizzledReplacingCharacters(in: range, with: replacement`) 가 실행됨.

  3. 스위즐링 된 **메소드의 구현체**에서 스위즐링 **메소드 겉껍데기**를 호출하는데, 메소드가 스위즐링 된 상태이므로 **Origin 내부 구현**이 실행 됨.

- **Origin 함수**가 호출될때 사실은 **swizzle 함수의 IMP**가 호출되고, 여기 안에서 재귀로 호출하는것 처럼 보이는 **swizzle 함수**는 사실 **Origin 함수의 IMP를 호출**하는 방식
- 즉 **Origin 함수 겉 껍데기 호출 -> Swizzle 의 내부 구현 호출 -> 내부 구현에서 swizzle 겉껍데기 호출 -> Origin 내부 구현 호출** 



# 출처

- [Static Dispatch & Dynamic Dispatch](https://github.com/sujinnaljin/TIL/blob/master/Swift/Static%20Dispatch%20%26%20Dynamic%20Dispatch.md)
- [Objective-C hook solution (1): Method Swizzling](https://www.programmersought.com/article/71063772553/)
- [class_getInstanceMethod(_:_:)](https://developer.apple.com/documentation/objectivec/1418530-class_getinstancemethod)
- [method_exchangeImplementations(_:_:)](https://developer.apple.com/documentation/objectivec/1418769-method_exchangeimplementations)
- [Swift) Method Swizzling을 알아보자](https://babbab2.tistory.com/m/76?category=828998)
- [[Swift] Foundation Framework 에서 NSString -> String 의 과정](https://darth-vader.tistory.com/2)

