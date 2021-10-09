# Property Observer

- Property Observers(프로퍼티 옵저버)를 정의해서 프로퍼티 **값의 변경을 모니터링** 가능
- 프로퍼티 옵저버는 새로운 값이 프로퍼티의 현재값과 **동일하더라도** 속성의 값이 설정(set)될 때 마다 호출 됨.

- 다음 위치에 Property Observer를 추가할 수 있음
  - **정의된 저장** 프로퍼티 (lazy 저장 프로퍼티 제외)
  - **상속된 저장** 프로퍼티 
  - **상속된 연산** 프로퍼티

- 참고로 **연산프로퍼티**에 대한 프로퍼티 옵저버는 연산프로퍼티 **setter**에서 해당 **값의 변경을 관찰하고 이에 응답**할 수 있으므로 **정의 할 필요 없**음 

- 하나의 프로퍼티에 대해 아래의 옵저버 정의 가능

  -  **willSet** - 값이 저장되기 **직전에 호출**.  **새로운 프로퍼티**의 값이 상수 매개변수(constant parameter)로 전달
  -  **didSet** - 새로운 값이 저장된 **직후에 호출**.  **이전 프로퍼티** 값을 포함하는 상수 매개변수(constant  parameter)가 전달

- 부모클래스 프로퍼티의 willSet, didSet 옵저버는 부모클래스 initializer가 호출된 후에 자식클래스의 initializer에서 set될때 호출됨. 즉, 부모클래스 initializer가 호출되기 전에, 클래스가 자체 프로퍼티들을 setting하는 동안은 옵저버들은 호출되지 않음 

  > 마지막은 뭔말인지 모르겠어서 원문 추가해둠 
  >
  > The willSet and didSet observers of superclass properties are called when a property is set in a subclass initializer, after the superclass initializer has been called. They are not called while a class is setting its own properties, before the superclass initializer has been called.
  >
  > For more information about initializer delegation, see [Initializer Delegation for Value Types](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html#ID215) and [Initializer Delegation for Class Types](https://docs.swift.org/swift-book/LanguageGuide/Initialization.html#ID219).





# 출처

- [Swift ) Properties - Property Observers(프로퍼티 옵저버)](https://zeddios.tistory.com/247)

- [Property Observers](https://docs.swift.org/swift-book/LanguageGuide/Properties.html#ID262)

