# Static Dispatch & Dynamic Dispatch

## Static Dispatch (or Direct Dispatch)

- 메서드를 재정의할 수 없는 경우 정적 디스패치 수행 됨
- 컴파일러가 **명령(instructions)들이 어디에 있는지 알기** 때문에, 함수가 호출되면 컴파일러는 수행할 함수의 **메모리 주소로 바로(directly) jump**. 이는 성능을 크게 향상시키고 컴파일러가 inlining과 같은 최적화를 가능케 함
- 동적 디스패치보다 더 빠름
- `final`, ` static` 키워드를 사용하여 정적 디스패치 사용 가능
- value type에 정의된 func 은 override 할 수 없기 때문에 해당 value type func의 기본 디스패치 방법

## Dynamic Dispatch

- 컴파일 타임이 아닌 **런타임에 어떤 메소드의 구현을 고를지** 결정
- 따라서 정적 디스패치에 비해 오버헤드가 더 든다
- 다형성을 위해 대부분의 OOP 언어들은 동적 디스패치를 지원

### 1. V-Table Dispatch

- 이 디스패치 기법은 **테이블을 사용**
- 여기서 테이블이란 **함수 포인터로 이루어진 배열**을 의미하며 witness table (혹은 virtual table)이라고 불린다. 

- Table은 자기 **자신이 처리할 수 있는 모든 메소드에 대한 포인터를 유지**하기 때문에 **특정 메소드의 구현을 찾는(look up) 용도**로 사용된다. 즉, 메소드를 호출 시 테이블을 참조하여 실제 호출될 메소드를 찾아낸다.

- 부모 타입으로부터 **상속받은 메소드**의 경우에는 **같은 주소값**을 유지하고, **오버라이드**하게 되면 이를 **덮어쓴**다 (해당 메소드에 대한 함수포인터가 **배열 끝에 추가**됨). 

- 정적 디스패치와 달리 컴파일러가 먼저 테이블로부터 메모리 주소를 읽고(read) 해당 위치로 이동(jump)해야 하므로 두번의 연산이 요구된다. 이것이 정적 디스패치보다 느린 이유. (메시지 디스패치보다는 여전히 빠르다)

  ![image](https://user-images.githubusercontent.com/20410193/122256211-cd3e4e80-cf09-11eb-9ad0-9bcc7eebbd30.png)


### 2. Message Dispatch

- Message Dispatch는 **자기 자신이 오버라이드** 하거나 **새로 정의한 메소드**들만 테이블에 유지

- 대신 부모 타입으로의 포인터를 가지고 있어서, 부모 타입의 메소드들은 부모 타입에서 찾아서 실행

  ![image](https://user-images.githubusercontent.com/20410193/122256247-d4655c80-cf09-11eb-9d11-a4476885b47a.png)

- 이 동적 디스패치 기법은 **가장 동적**임. 사실 이 기법은 매우 좋아서 (최적화 측면을 제외하면) Cocoa 프레임워크에서도 KVO, 코어데이터와 같은 곳에서 자주 사용 됨

- 이러한 방식은 굉장히 **유연**해서, 런타임에 메소드의 기능(functionality)를 바꾸는 [**`method swizzling`**](https://github.com/sujinnaljin/TIL/blob/master/Swift/Method_Swizzling.md) 을 가능하게 한다. (원래의 메소드를 runtime 때 원하는 메소드로 바꾸어 사용할 수 있도록 하는 기법)

- 이러한 기능을 제공하는 런타임 라이브러리로 Swift는 Objective-C 런타임을 이용. 즉, Message Dispatch를 이용하기 위해서는 **Objective-C 런타임에 의존**

- 메시지 디스패치를 위해 Objective-C 런타임을 사용하기 때문에 메시지가 디스패치 될 때 런타임은 클래스 위계(hierarchy)를 모두 보고(crawl) 실행할 메소드를 결정하는데, 이 과정이 매우 느리기 때문에 **성능향상을 위해 캐시를 제공**하기도 함



# 출처

- [Message Dispatch](https://jcsoohwancho.github.io/2019-11-02-Message-Dispatch/)
- [[번역] Method Dispatch in Swift](https://sihyungyou.github.io/iOS-method-dispatch/)
- [A Deep Dive Into Method Dispatches in Swift](https://betterprogramming.pub/a-deep-dive-into-method-dispatches-in-swift-65a8e408a7d0)
- [Dynamic Dispatch와 성능 최적화](https://jcsoohwancho.github.io/2019-10-11-Dynamic-Dispatch%EC%99%80-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94/)
- [Swift의 Dispatch 규칙](https://jcsoohwancho.github.io/2019-11-01-Swift%EC%9D%98-Dispatch-%EA%B7%9C%EC%B9%99/)

