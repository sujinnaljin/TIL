# closure vs. block

- **익명함수**를 **스위프트**에선 **Closure**, **Objective-C**에선 **Block**이라고 함.
- 클로저와 블럭 모두 **내부 함수와 내부 함수에 영향을 미치는 주변 환경을 모두 포함한 객체**
- 단지 같은 Scope에 있는 **값(Value) 타입의 지역변수를 캡쳐하는 방식**이 다름
  - **Reference Capture** (**변수가 사용되는 시점의 값**을 Capture) vs. **Value Copy Capture** (**Closure & Block이 선언될 당시 값**을 Capture. 캡쳐된 값은 Const Value Type 으로 **변경 불가**)

|                         | **Value Type**                                               | **Reference Type** |
| ----------------------- | ------------------------------------------------------------ | ------------------ |
| **Closure (Swift)**     | Reference Capture. 하지만 Closure List를 사용하여 Value Copy Capture 가능. | Reference Capture  |
| **Block (Objective-C)** | Value Copy Capture. 하지만 __block 을 사용하여 Reference Capture 가능 | Reference Capture  |

## closure

- Value/Reference **타입에 관계 없이 Reference Capture** 

- 따라서 closure를 실행하기 전에 value type값을 바꾸면, closure에서 값도 바뀜. 즉 클로저의 변수가 사용되는 시점의 변수의 값을 평가

  ```swift
  func doSomething() {
      var num: Int = 0
      print("num check #1 = \(num)")
      
      let closure = {
          print("num check #3 = \(num)")
      }
      
      num = 20
      print("num check #2 = \(num)")
      closure()
  }
   
  // num check #1 = 0
  // num check #2 = 20
  // num check #3 = 20
  ```

- **Value 타입의 값을 복사해서 캡쳐**(Value Copy Capture)하기 위해선 **Closure List** 이용. 즉 Copy해서 캡쳐할 변수를 명시

  ```swift
  let closure = { [num1, num2] in
  ```

- 캡쳐 리스트를 작성한다고 해도 **Reference Type은 Reference Capture** 함. 하지만 **weak 나 unowned 로 캡쳐**함으로써 **강한 순환 참조 방지** 가능. 

## block

- **Value Type**일 경우 **값을 복사하여 Capture**하고, **Reference Type**일 경우 **Reference Captrue**

- block을 **선언할 당시의 Value Type 값**을 Const Value Type으로 캡쳐.

  ```objc
  - (void)doSomething {
      int num = 0;
      NSLog(@"num check #1 = %d", num);
      
      void (^block)(void) = ^{
          NSLog(@"num check #3 = %d", num);
      };
   
      num = 20;
      NSLog(@"num check #2 = %d", num);
      block();
  }
  
  // num check #1 = 0
  // num check #2 = 20
  // num check #3 = 0
  ```

- **Value type을 Reference Capture** 하고 싶다면 변수 선언부에 **__block** 를 접두사로 사용

  ```objc
  __block int num = 0;
  ```

  



# 출처

- [Swift & Objective-C) Closure와 Block의 차이점](https://babbab2.tistory.com/50)
