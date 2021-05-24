# NSObject

- 대부분의 Objective-C 클래스 계층 구조의 **루트 클래스**. 아래 계층 구조의 모든 객체에 공통적인 인터페이스와 동작을 정의

- 하위 클래스에서 **런타임** 시스템에 대한 기본 **인터페이스**와 **Objective-C 객체로 작동하는 기능**을 상속

  > **런타임 시스템**
  >
  > Objective-C용 운영체제 같은 것으로, **객체 생성, 해제에 따른 메모리 영역 관리**와 **송신된 메시지에 대응하는 메서드** 검색

- 기본 객체 인터페이스를 선언하고 내부 검사, 메모리 관리 및 메서드 호출을 포함한 기본 객체 동작을 구현

- Cocoa 및 Cocoa Touch 객체는 루트 클래스인 `NSObject` 의 상속을 통해, 런타임 시스템 기능을 고려하지 않아도 많은 관련 기능들 이용할 수 있음 



![2021-03-25 at 06.43.00 AM-rootclass](https://zdodev.github.io/assets/image/2021-03-25%20at%2006.43.00%20AM-rootclass.png)





# 출처

- [Cocoa Touch의 Root 클래스인 NSObject 클래스에 대해서](https://zdodev.github.io/swift/NSObject/)
- [NSObject](https://developer.apple.com/documentation/objectivec/nsobject)

