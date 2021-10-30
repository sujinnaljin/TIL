# dynamic 키워드

- Swift와 Objective-C와의 **상호운용성(Interoperability)** 을 위해 변수 앞에 붙는 키워드로, dynamic 키워드를 붙히면 **objc 속성**이 암시적으로 표시됨.

  ```swift
  dynamic var name = "naljin"
  ```

- 이 키워드를 통해 **Objective-C 런타임을 사용**한 **동적 디스패치(dynamic dispatch)** 가 가능하며, **컴파일러에 의해 인라인되거나 가상화 되지 않음**

  > **동적 디스패치?** 
  >
  > Objective-C 런타임이 **호출해야하는 특정 메소드나 함수의 구현**을 **런타임에 결정**하는 것. 
  >
  > 예를들어, 하위 클래스가 슈퍼 클래스의 메소드를 재정의(override)하는 경우, **동적 디스패치**는 **메소드의 구현을 호출해야하는지, 하위 클래스의 메소드 또는 부모클래스의 메소드를 호출해야하는지를 파악**
  >
  > Objective-C는 **동적 디스패치**를 사용하기 때문에, 클래스의 메소드나 프로퍼티를 **호출**할 때 해당 **객체에 메세지**를 보냄 (message sending). ->  메세지를 받은 객체는 이제 **진짜 그 메소드가 구현되어있는 곳**으로 가서 비로소 그 메소드를 실행. 이러한 과정이 **런타임**에 발생.
  >
  > **코어데이터**나 **키-밸류 옵저빙**(KVO = Key-Value Observing)과 같은 것들은 동적인 Objective-C 런타임 덕분에 가능

- Swift는 가능할 때마다 **Swift 런타임을 사용** 하므로, 여러가지 **최적화 작업**을 수행할 수 있음. 만약 컴파일러가 **컴파일타임에 선택할 수 있는 메소드의 구현을 파악**할 수 있다면,  그 메소드가 어딨는지 찾을 필요도 없고, 바로 그 **주소로 점프해서 실행**하면 됨. 이게 바로 **정적 디스패치**로, 동적 디스패치보다 빠름.

- 하지만 `dynamic var` 또는 `dynamic func` 이 의미하는 바는 이러한 **Swift 런타임말고 Objective-C 런타임을 쓰겠다**는 의미. 즉 객체한테 메세지보내고 -> 객체가 메소드 위치 찾아서 실행한다는 것. 

- dynamic키워드는 **오직 Class의 멤버**에만 사용할 수 있음. 왜냐하면 **상속이 필요하지 않**은 **구조체**나 **열거형** 등은 **런타임에서 사용할 실제 구현을 파악할 필요가 없**기 때문.



# 출처

- [Swift) dynamic이란? / Realm의 dynamic var는?](https://zeddios.tistory.com/296)
- [Dongky's Programming](https://dongkyprogramming.tistory.com/)
- [Static Dispatch & Dynamic Dispatch](https://github.com/sujinnaljin/TIL/blob/master/Swift/Static%20Dispatch%20%26%20Dynamic%20Dispatch.md)
