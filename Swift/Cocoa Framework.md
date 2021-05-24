# Cocoa Framework

- Apple에서 iOS, macOS 등의 Apple 운영 체제용 **어플리케이션을 제작**할 때 사용하는 **프레임워크**
- 초기에 Apple이 NeXT에서 이걸 샀을 때는 블루 박스라고 불리다가. 이제는 Cocoa라 불린다
- Java가 커피원산지에서 따온 이름이기 때문에, Apple 개발자는 어린아이도 할 수 있는 자바(Java for kids)라는 의미에서 Cocoa라고 이름지었다고 함
- Objective-C에는 C++같은 네임스페이스가 따로 없기 때문에, 충돌을 피하기 위해 보통 클래스의 이름 앞에 Prefix를 붙임. 덕분에 Foundation Kit 프레임워크 클래스들은 이름앞에 NS([NeXTSTEP](https://namu.wiki/w/NeXTSTEP))가 있음 (ex. NSString, NSArray) 
- 기본적인 **자료형과 메소드**가 정의되어 있는 **Foundation Kit**과 주로 **UI 개발**에 사용되는 **Application Kit**으로 이루어져 있음 (줄여서 **Foundation**, **AppKit**이라고도 한다)

![img](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/Art/osx_architecture.jpg)



- 특히 iOS 프로그래밍을 위한 아키텍처에서, Cocoa는 아래와 같이 구성됨
- 이때 Cocoa Touch 레이어와 Core Services 레이어에는 각각 iOS 용 애플리케이션 개발에 특히 중요한 Objective-C 프레임 워크가 있는데, 각각 Foundation과 AppKit
  - **Foundation**: 위에서 살펴본 것과 동일하게 기본적인 **자료형과 메소드**가 정의
  - **UIKit**: AppKit 대신 AppKit에 기반한 UIKit을 사용. **사용자 인터페이스**에 표시하는 객체를 제공하고 **이벤트 처리** 및 **그리기**를 포함한 응용 프로그램 동작의 구조를 정의.

![Cocoa in the architecture of Aspen](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/Art/architecture_stack.jpg)

- **Cocoa Touch 계층**
  - 하위 계층의 프레임워크를 사용하여 iOS 애플리케이션을 **구현을 직접 지원**
  - UIKit, GameKit, MapKit, iAd

- **Media 계층**
  - Core Services 계층에 의존하며, 상위 계층인 Cocoa Touch 계층에 **그래픽** 관련 서비스나 **멀티미디어** 관련 서비스를 제공
  - Core Graphics, Core Text, Core Audio, Core Animation, AVFoundation, Core Audi, 비디오 재생

- **Core Services 계층**

  - 문자열 처리, collection 관리, 네트워킹, 연락처 관리 관리, 환경 설정 등 **핵심적인 서비스**들을 제공. 또한 GPS, 나침반, 가속도 센서나, 자이로스코프 센서와 같이 **하드웨어** 특성에 기반한 서비스도 제공

  - Core Location, Core Motion, System Configuration, Core Data, Foundation, Core Foundation

- **Core OS 계층**
  - 커널, 파일 시스템, 네트워크, 보안, 전원 관리, 디바이스 드라이버 등이 포함. iOS가 운영체제로서 기능을 하기 위한 핵심적인 영역



# 출처

- [Cocoa API](https://namu.wiki/w/Cocoa%20API?from=Cocoa%28API%29)

- [What Is Cocoa?](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/WhatIsCocoa/WhatIsCocoa.html)

  
